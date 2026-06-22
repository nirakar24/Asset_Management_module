# Asset Management (ITAM) Module — 6-Month Phased Build Plan

## Context

We sell **EpmBackend**, an Endpoint Privilege Management product: a .NET/MySQL,
self-hosted, multi-tenant backend (orgs/groups, RBAC: super_admin/org_admin/group_admin)
with a Windows-only agent that does real-time monitoring (SignalR heartbeat: sessions,
lock status, Windows services) and takes commands over a RabbitMQ queue (freeze/unfreeze,
lock/terminate, refresh; priority + status + DLQ). The agent can be remotely deployed via
PsExec, WMI, PowerShell, SMB, Group Policy, and LDAP. EPM capabilities include privilege
management, application control, and watermarking/DLP.

Market research (`ITAM_Market_Research.md`, `ITAM_Market_Comparison_Report.md`) shows a
clear gap: EpmBackend has no SAM/license compliance, no CMDB/relationship mapping, no
procurement/contract/warranty/cost lifecycle, no real patch management, no SaaS/cloud
discovery, no ITSM integration, and is Windows-only. The ITAM market is crowded
(ServiceNow, Flexera, Ivanti, Freshservice, ManageEngine, Lansweeper, Tanium, Axonius),
but **no mid-market rival natively fuses privilege/access context into asset records.**

**Goal:** build an Asset Management module that (a) enhances EpmBackend as an add-on and
(b) ships as a standalone ITAM product, within 6 months, in phases. The wedge — locked
with the user — is **EPM-enriched asset records**: every asset carries privilege/admin/
elevation context and a privilege-aware risk score. *"Know not just what your endpoints
are, but who has access at what privilege level and why."*

### Decisions locked with the user
1. **Agent:** a *separate* AM agent process with its own lifecycle and update channel,
   that *reuses* EPM enrollment + transport (SignalR/RabbitMQ) when co-installed, and runs
   fully standalone otherwise.
2. **Platform:** Windows only in Phase 1; macOS/Linux later.
3. **Backend:** a *separate* AM microservice + its own DB schema, integrated with EpmBackend
   via APIs/events; independently deployable.
4. **Anchor differentiator:** EPM-enriched asset records + privilege-aware risk scoring.

> **Assumption to verify against the real EPM codebase (not in this research-only repo):**
> exact .NET version, ORM/migration conventions, GUID/ID format, JWT signing/JWKS setup,
> and whether a prior SAM design exists. The plan below names EpmBackend components by role;
> confirm concrete names/conventions before coding and mirror them exactly.

---

## Target Architecture

### AM Agent (new process, sibling of the existing EPM agent)
- **Collector framework:** pluggable per-collector modules behind a platform-provider
  abstraction (mirror the existing EPM agent's inventory/device-info provider pattern).
  Phase-1 Windows collectors: **Hardware** (CPU/RAM/disk/BIOS/NIC via WMI/CIM),
  **Installed software** (registry uninstall keys + MSI + per-user hives), **OS/patch state**
  (KBs, build, pending reboot), and the differentiator **EPM-context collector** (local
  admin membership, who holds admin, UAC/elevation posture, EPM agent presence + policy id).
- **Scheduling:** config-driven cadence per collector; startup full scan + delta scans +
  on-demand push. **Continuous mode** via WMI event watchers for software install/uninstall
  and admin-group changes (near-real-time privilege/software change visibility).
- **Offline buffering:** durable local spool (SQLite or append-only file) with dedup-on-flush;
  survives restart; flushes on reconnect.
- **Comms (one transport abstraction, mode chosen at startup):**
  - *Co-installed:* detect EPM agent, reuse its enrollment identity (DeviceId) and
    SignalR/RabbitMQ transport; AM payloads ride as a distinct message type/queue (never
    block the EPM heartbeat).
  - *Standalone:* self-enroll (REST/gRPC register → DeviceId), own persistent channel,
    POST inventory to the AM service over HTTP.
- **Update channel:** decoupled from the EPM updater — signed manifest + package, staged
  install with rollback, per-org version pinning.
- **Security:** signed binaries + signed update manifests (verify signature + hash + version
  before apply), mutual auth, least-privilege service account, tamper-evident spool.

### AM Microservice (new .NET service, own schema, independently deployable)
- **Ingestion:** two entry points → one handler: (a) HTTP `POST /api/am/inventory/report`
  (standalone/direct), (b) RabbitMQ consumer for messages forwarded by EpmBackend when
  co-installed. Ack-fast / process-async with DLQ. Idempotent by (DeviceId, collectorType,
  reportHash).
- **Normalization / dedup (first-class on every ingest path):** software-title normalization
  (canonical product catalog + ordered match rules; unmatched → review queue) and **asset
  identity resolution** via a match-key ladder: DeviceId > serial/UUID > MAC set >
  hostname+domain. Deterministic merge; conflicts flagged.
- **CMDB / relationships:** `Asset` (CI) + typed `CiRelationship` edges (runs-on,
  installed-on, connected-to, depends-on, owned-by, located-in); auto-derived + manual;
  impact/dependency via bounded graph traversal.
- **EPM-enrichment layer (the anchor):** pull privilege/admin/elevation data from EpmBackend
  API + subscribe to elevation events (RabbitMQ/webhook); join to `Asset` by DeviceId →
  `AssetEpmContext`; feed the **privilege-aware risk scorer**. Standalone mode is fed by the
  agent's local-only EPM collector (degraded, clearly labeled).
- **Multi-tenancy/auth:** reuse EPM org/group model + role names; `OrganizationId` (+ optional
  `GroupId`) on every row. Add-on validates EPM-issued JWT (shared signing) for SSO;
  standalone runs the same auth stack with its own user store.

### Data model highlights (new schema)
- **Asset** — canonical CI: Id, OrganizationId, GroupId?, AssetType, DeviceId? (EPM link),
  Hostname, Serial, Vendor, Model, identity keys, LifecycleState, Source
  (Agent/Agentless/Cloud/Manual/Import), FirstSeen/LastSeen.
- **AssetHardware / AssetSoftware** — normalized current inventory (replace-per-report idiom;
  AssetSoftware joins the product catalog).
- **SoftwareProduct / SoftwareLicense / ComplianceSnapshot / UnmatchedSoftware** — SAM.
- **CiRelationship** — Source/Target/RelType/Direction/Confidence/Source, valid-from/to.
- **AssetEpmContext** (1:1 to Asset) — HasStandingAdmin, AdminPrincipals, LastElevationAt,
  ElevationCount30d, EpmPolicyId, AppControlMode, **PrivilegeRiskScore (0-100)**, RiskFactors.
- **AssetContract / AssetWarranty / AssetCost** — procurement, depreciation, warranty/contract
  expiry. **CloudResource / SaaSApplication / VulnerabilityFinding** — later phases.

### Standalone vs co-installed
| Concern | Co-installed (add-on) | Standalone |
|---|---|---|
| Enrollment | Reuse EPM DeviceId + token | AM self-enrolls |
| Transport | Ride EPM SignalR/RabbitMQ | AM own channel + HTTP |
| EPM enrichment | Full (authoritative via API/events) | Degraded (agent local-only, labeled) |
| Auth/RBAC | EPM JWT (SSO), shared signing | AM auth store, same role names |
| Deploy tooling | Reuse EPM remote deploy | Same tooling, AM installer |
| Update channel | Separate AM channel | Separate AM channel |

---

## Phased Plan (6 months, 4 shippable phases)

Assumed ~6 eng: 2 backend, 1.5 Windows agent, 1 frontend, 1 platform/DevOps, 0.5 QA.
Each phase ends deployable and demo-able.

### Phase 1 — MVP: EPM-Enriched Endpoint Inventory + Risk Score (Weeks 1-6)
**Goal:** prove the wedge early — every endpoint asset carries privilege context + a score.
- AM microservice skeleton (own schema, JWT validation reusing EPM signing, org/group scoping).
- Ingestion v1: HTTP report + RabbitMQ consumer; idempotent persist of HW + installed software.
- AM agent v1 (Windows, co-installed): HW + software + EPM-context collectors; reuse EPM
  transport; offline spool; on-demand collect.
- EPM-enrichment layer v1: pull privilege/admin + subscribe to elevation events → `AssetEpmContext`.
- **Privilege-aware risk scorer v1:** deterministic, explainable rules (standing admin, recent
  elevations, stale OS/patch, unsanctioned software) → 0-100 + factor breakdown.
- Dashboard v1: asset list, detail w/ EPM-enrichment panel + score; CSV/XLSX export.
- *Deliverables:* deployable service + agent, demo script, DDL, API docs, scoring spec.
  Highest-risk integration front-loaded.

### Phase 2 — SAM + Hardware Lifecycle + Agentless Discovery (Weeks 7-13)
**Goal:** clear core ITAM table-stakes; cover devices with no agent.
- SAM: product catalog + entitlements + reconciliation (install-count vs seats, expiry,
  unmatched review queue).
- Hardware lifecycle + finance: procurement→retirement states, contract/warranty/cost,
  depreciation, expiry alerts.
- **Agentless discovery:** scheduled credentialed SNMP/WMI/SSH sweeps through the same
  ingestion + identity-resolution + dedup pipeline (reuse EPM WMI/SMB credential tooling).
- Dedup hardening across agent + agentless; catalog seeding.
- Reporting v2: compliance + lifecycle + expiry dashboards and PDF export.

### Phase 3 — CMDB/Relationships + ITSM Integration + Standalone GA (Weeks 14-20)
**Goal:** enterprise-RFP completeness + ship the standalone SKU.
- CMDB: relationship model, auto-derived edges, relationship graph view, impact/dependency queries.
- **Standalone GA:** AM self-enrollment + own transport, AM auth store (same roles),
  AM-packaged deployment, labeled degraded enrichment, licensing split.
- ITSM integration: ServiceNow + Jira connectors (push assets/CIs + privilege-risk context to
  tickets; pull CI refs) via a reusable outbound REST/webhook connector framework.
- Continuous discovery mode GA (event-driven reporting hardened).

### Phase 4 — Cloud/SaaS + Vuln-to-Asset + AI Risk + Hardening (Weeks 21-26)
**Goal:** close the cloud/SaaS blind spot; deepen security convergence.
- Cloud connectors: AWS + Azure + GCP API inventory → CloudResource → Asset (continuous).
- SaaS / shadow-IT discovery from endpoint signals (+ optional IdP/OAuth-app pull), correlated
  with privilege.
- Vulnerability-to-asset linkage + flagship cross-ref: *"vulnerable AND running with standing admin."*
- Risk scorer v2: ML/AI anomaly signals layered on the deterministic base.
- Scale + GA hardening: large-inventory load tests, partitioning/retention; SBOM groundwork (stretch).

**Sequencing rationale:** P1 proves the wedge + de-risks integration; P2 makes it a credible
ITAM tool; P3 unlocks enterprise sales + the standalone SKU; P4 chases highest-growth trends.

---

## Cross-Cutting Concerns
- **Scalability:** ack-fast/process-async + DLQ; idempotent upserts keyed by (AssetId,
  collectorType, hash); replace-per-report current state + append-only history; batch
  reconciliation in background jobs off the hot path; partitioning/retention; per-org batched
  jobs. Target ~100k+ assets/tenant by P4.
- **Agentless:** credentialed scheduled SNMP/WMI/WinRM/SSH; cloud via API (P4); all results
  pass through the same ingestion + identity-resolution + dedup pipeline → one canonical asset.
- **Normalization/dedup:** product catalog + priority-ordered match rules (regex with timeout
  to avoid ReDoS), unmatched → review queue; identity ladder DeviceId > serial/UUID > MAC >
  hostname+domain.
- **Licensing/packaging:** add-on gated by EPM entitlement + SSO; standalone has independent
  key + asset-count-tiered metering; same codebase, feature-flag mode select; separate
  service + DB so standalone never needs EPM.
- **Agent security:** signed binaries + manifests, staged rollout + rollback + per-org pinning,
  mutual auth, tamper-evident spool, AM updater decoupled from EPM's.

## Build-vs-Reuse
- **Reuse from EpmBackend:** enrollment (DeviceId + token co-installed); transport (SignalR +
  RabbitMQ w/ priority/status/DLQ); RBAC + multi-tenancy + JWT signing; remote deployment
  (PsExec/WMI/PowerShell/SMB/GPO/LDAP) to push the AM agent; backend conventions (controllers,
  error handling, export, background-service template, license-check); Windows HW/software
  collector logic from the existing agent.
- **Build new (independently deployable):** AM microservice + schema; AM agent process + own
  update channel; EPM-enrichment layer + privilege-aware risk scorer; CMDB/relationship model;
  identity-resolution/dedup engine; agentless scanner; cloud/SaaS + vuln connectors; standalone
  auth + licensing.

## Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Enrichment coupling defeats standalone | Enrichment is one pluggable layer; standalone degrades to agent-local data (labeled); mode-matrix tests enforce both. |
| Shared transport interferes with EPM agent | Distinct message types/queues; never block EPM heartbeat; fallback to standalone transport if shared channel unavailable. |
| Duplicate/conflicting assets across sources | Identity-resolution + dedup as first-class service on every ingest path; conflict review queue. |
| Scope creep toward ServiceNow parity | Lock the wedge; table-stakes in priority order; defer SBOM/OT/ESG/FinOps. |
| Risk score not credible | Deterministic, explainable factor breakdown before any ML; publish spec; AI only in P4. |
| Two agents = endpoint bloat | Shared transport + enrollment co-installed; lightweight AM agent; continuous + privilege value justifies separate process. |
| Multi-tenant data leak | OrganizationId on every row + enforced scoping; isolation tests gate release. |
| ORM-vs-DDL drift / convention mismatch | Confirm real EPM conventions first; verify schema mapping in CI. |

---

## Verification
- **Phase gates:** each phase ends with a deployable service + agent build and a recorded
  demo proving that phase's headline (P1: an endpoint asset showing privilege context + score;
  P2: a license-compliance + agentless-discovered asset; P3: a CMDB graph + a ServiceNow ticket
  enriched with privilege-risk + a standalone install; P4: a cloud asset + a "vulnerable AND
  standing-admin" finding).
- **Mode matrix:** every feature is tested both co-installed and standalone.
- **Test layers:** unit (risk scorer, software matching, identity/dedup edge cases);
  integration (idempotency, reconciliation, enrichment join, DLQ, tenant isolation); contract
  (agent↔service payload pinned; EPM API/event); E2E for both modes; scale/burst (P4); security
  (signing/update/tamper/authz).
- **First-week smoke:** stand up the AM service against a test DB, enroll one co-installed AM
  agent on a Windows VM that already runs the EPM agent, confirm a HW+software+EPM-context
  report lands, `AssetEpmContext` is populated from EPM data, and a risk score renders in the
  dashboard.

## Open items to confirm before coding
- Real EpmBackend tech conventions (versions, ORM/migrations, ID format, JWT/JWKS) and whether
  any prior SAM/inventory work exists to reuse.
- EpmBackend API/event surface for privilege/admin/elevation data (the enrichment contract).
- Licensing/metering model for the standalone SKU (asset-count tiers).
