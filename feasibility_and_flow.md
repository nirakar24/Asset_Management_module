# Asset Management Module — Feasibility & High-Level Flow

**Product:** Asset Management (ITAM) module on our Endpoint Privilege Management (EPM) platform
**Date:** 2026-06-23
**Purpose:** For each feature — *is it achievable?*, *what it depends on*, and a *high-level flow* of how we build it. Difficulty is relative to a team that already owns the EPM agent + backend.

> Companion docs: [feature_guide.md](feature_guide.md) · [product_pitch_6month.md](product_pitch_6month.md) · [competitor_analysis.md](competitor_analysis.md)

**Difficulty legend:** 🟢 Low (mostly surfacing what we have) · 🟡 Moderate (new collection/logic) · 🔴 Hard (deep domain or external data)

---

## Foundational Assumptions (the platform we build on)
1. **EPM agent already deployed** on every endpoint, running with elevated privilege → it can read hardware, OS, and installed-software data locally.
2. **Agent ↔ backend channel already exists** (heartbeat/check-in + command path used for EPM policy) → reuse it for asset telemetry and remediation actions.
3. **Backend + datastore + console** already exist for EPM → extend, don't rebuild.

These three are why most Phase 1–2 features are 🟢/🟡: we are *surfacing and structuring data the agent can already see*, not building new infrastructure.

---

## Phase 1 — Foundation

### Real-time, first-party agent data — 🟢 Achievable
- **Depends on:** existing agent check-in channel; an event/delta mechanism.
- **Flow:**
  1. Agent collects an inventory snapshot on startup + on a schedule.
  2. Agent watches for change events (hardware/OS/software install/uninstall) and sends **deltas** instead of full dumps.
  3. Backend ingestion service updates the asset record and stamps `last_seen`.
  4. Console reads the live record.
- **Risk:** event hooks differ per OS; start with scheduled snapshots + simple delta diff, add native change hooks incrementally.

### Endpoint hardware inventory — 🟢 Achievable
- **Depends on:** OS inventory APIs (WMI/CIM on Windows, system_profiler/IOKit on macOS, /proc + dmidecode on Linux).
- **Flow:**
  1. Agent queries OS APIs for CPU/RAM/disk/serial/BIOS/peripherals.
  2. Normalizes to a common schema.
  3. Ships to backend → stored against the asset.
- **Risk:** low — this is standard, well-documented OS data. Likely partially collected already.

### Auto-discovery & classification — 🟢 Achievable
- **Depends on:** agent enrollment (already how EPM onboards endpoints).
- **Flow:**
  1. New endpoint enrolls the EPM agent → backend auto-creates an asset record.
  2. Classification rules (OS type + chassis/form factor) tag it laptop/desktop/server.
  3. Record enters inventory automatically.
- **Risk:** only sees **agent-managed** endpoints (not network gear). Acceptable for v1; addressed by Phase 3 integrations.

### Basic software inventory (installed apps) — 🟢 Achievable
- **Depends on:** OS software registries (Windows registry/MSI, macOS /Applications + pkgutil, Linux package managers).
- **Flow:**
  1. Agent enumerates installed packages (name + version).
  2. Sends list/deltas to backend.
  3. Backend indexes software-per-asset for search.
- **Risk:** low — EPM already inspects software for policy. Mostly a surfacing exercise.

### Dashboard, search, custom fields, tagging — 🟢 Achievable
- **Depends on:** existing console framework + a searchable index.
- **Flow:**
  1. Define asset schema + custom-field/tag model in the datastore.
  2. Index assets (DB queries or a search engine for scale).
  3. Build console UI: list, filter, tag, custom-field editor.
- **Risk:** low — standard CRUD + search UI work.

### Asset detail view with ownership + status — 🟢 Achievable
- **Depends on:** user/owner data (from directory/EPM identity) + a status field.
- **Flow:**
  1. Join asset ↔ logged-in user / directory owner.
  2. Add status enum (active/repair/retired).
  3. Render a single asset profile page aggregating hardware + software + owner + status + last-seen.
- **Risk:** low; owner attribution accuracy depends on directory integration quality.

---

## Phase 2 — Depth

### Software versioning / publisher normalization — 🟡 Achievable
- **Depends on:** a normalization rules engine / reference catalog.
- **Flow:**
  1. Agent already sends raw name+version+publisher.
  2. Backend runs a normalization service: mapping rules + fuzzy matching to collapse publisher/title variants.
  3. Store both raw and normalized values.
- **Risk:** normalization quality scales with catalog richness. Start with a curated rules table; this is the *good-enough* 80% — full catalog parity with Flexera is **not** a 6-month goal.

### Software usage / metering — 🟡 Achievable
- **Depends on:** process/launch monitoring on the endpoint.
- **Flow:**
  1. Agent records process start events / foreground time per application.
  2. Aggregates locally (e.g., "last launched," "days used in 90d") and reports periodically.
  3. Backend stores usage metrics against software-per-asset.
- **Risk:** moderate — must keep telemetry lightweight (privacy + performance). Track app-level usage, not keystroke-level. EPM likely already hooks process execution.

### License tracking & compliance flags — 🟡 Achievable
- **Depends on:** a manual/imported entitlement record (licenses owned).
- **Flow:**
  1. Admin enters/imports licenses owned per product (CSV/UI).
  2. Backend counts normalized installs per product.
  3. Compare owned vs. installed → raise flag when installed > owned.
- **Risk:** "installs > owned" is straightforward; *true entitlement* (upgrade rights, bundles) is the hard part we defer. v1 = deployment-count compliance, clearly scoped.

### Asset lifecycle (procure → retire) + history — 🟡 Achievable
- **Depends on:** a state machine + append-only history log.
- **Flow:**
  1. Define lifecycle states + allowed transitions.
  2. Log every change (status/owner/location) as an immutable event with timestamp + actor.
  3. Render a timeline on the asset detail page.
- **Risk:** low-moderate; procurement-stage data may be manual unless integrated with purchasing.

### Warranty / contract / EOL alerts — 🟡 Achievable
- **Depends on:** date fields + a scheduled alerting job; optionally vendor warranty APIs.
- **Flow:**
  1. Capture warranty/contract/EOL dates (manual import, or auto-lookup via Dell/Lenovo/HP warranty APIs by serial).
  2. Scheduled job scans for dates within threshold (e.g., 30 days).
  3. Fire notifications (email/console).
- **Risk:** low for the alert engine; vendor warranty-API coverage varies, so support manual entry as fallback.

### Lightweight CMDB: relationships & dependency view — 🟡 Achievable
- **Depends on:** a relationship data model + a way to infer/declare links.
- **Flow:**
  1. Model assets as nodes, relationships as edges (depends-on, connects-to, runs).
  2. Seed edges from observable signals (network connections, mounted shares, app→server config) + manual declaration.
  3. Render a dependency graph in the console.
- **Risk:** **auto-discovery of dependencies is the genuinely hard part.** Scope v1 to endpoint-centric, observable relationships + manual links; full service mapping (ServiceNow-grade) is out of scope.

---

## Phase 3 — Convergence (our differentiator)

### Asset-to-risk / vulnerability context — 🟡 Achievable
- **Depends on:** EPM posture data (already ours) + a vulnerability source (CVE feed / scanner integration).
- **Flow:**
  1. Pull EPM posture per asset (admin rights, policy state) — already in our system.
  2. Match installed software+versions against a CVE feed (e.g., NVD) or ingest from a scanner.
  3. Attach risk indicators to the asset record; surface on detail + dashboard.
- **Risk:** software→CVE matching needs accurate version normalization (Phase 2 dependency) and a maintained feed. Start with CVE-feed matching; scanner integration later.

### Coverage / exposure gap detection — 🟢 Achievable
- **Depends on:** cross-referencing data we already hold.
- **Flow:**
  1. Reconcile asset records ↔ EPM-agent population ↔ directory/known devices.
  2. Flag mismatches: agent-but-no-record, stale `last_seen`, non-compliant policy, no recent check-in.
  3. Present an "exposure gaps" dashboard.
- **Risk:** low — this is primarily set-difference logic over existing data; high value for low effort.

### One-click remediation (EPM-native) — 🟢 Achievable
- **Depends on:** the **existing EPM command/policy channel** (our core strength).
- **Flow:**
  1. From an asset/software view, admin selects an action (revoke admin, block app, push policy).
  2. Backend translates to an EPM policy/command.
  3. Existing agent command path enforces it; result reported back to the asset record.
- **Risk:** low *technically* (we already do enforcement); add guardrails — confirmation, scoping, audit log — to prevent fat-finger mass actions.

### CMDB sync / enrich (ServiceNow etc.) — 🟡 Achievable
- **Depends on:** external CMDB APIs + field mapping.
- **Flow:**
  1. Build connectors (start with ServiceNow Table/Import API).
  2. Map our normalized fields → CMDB CI attributes.
  3. Scheduled/triggered push: upsert missing CIs, update changed ones, flag suspected duplicates.
- **Risk:** moderate — each CMDB is a separate connector + mapping effort. Ship ServiceNow first; others as demand dictates.

### Reporting & exportable dashboards — 🟡 Achievable
- **Depends on:** a query/reporting layer + export + scheduler.
- **Flow:**
  1. Reporting service queries the asset datastore by any dimension.
  2. Saved/scheduled reports; export to CSV/PDF.
  3. Optional emailed delivery to stakeholders.
- **Risk:** low-moderate; flexible ad-hoc reporting can balloon in scope — ship a fixed set of high-value reports + CSV export first, add a custom report builder later.

---

## Overall Feasibility Summary

| Phase | Features | Verdict |
|---|---|---|
| **Phase 1** | All 6 | 🟢 Highly achievable — mostly surfacing data the agent already sees |
| **Phase 2** | 6 | 🟡 Achievable with scoped depth — defer deep SAM & full dependency auto-mapping |
| **Phase 3** | 5 | 🟢/🟡 Achievable — remediation & gap detection are our easiest wins (reuse EPM); CMDB sync & vuln matching need external integrations |

**The pattern:** our existing EPM agent + command channel makes discovery, inventory, gap detection, and remediation *low-risk*. The hard parts we deliberately scope down — deep license true-up, full service-dependency mapping, and broad CMDB connectors — match exactly the "where we should be humble" boundaries in [drawbacks.md](drawbacks.md).

**Top cross-cutting risks to manage:**
1. **Agent performance/footprint** — asset telemetry must stay lightweight; reuse existing collection, prefer deltas.
2. **Normalization quality** — Phase 3 vuln matching depends on Phase 2 version normalization; sequence accordingly.
3. **Scope creep on "depth" features** — SAM, dependency mapping, and custom reporting each have a bottomless deep end; ship the 80% and defer.
4. **Non-endpoint blind spot** — agent sees endpoints only; set expectations that network/OT/cloud coverage comes via integrations, not the agent.
