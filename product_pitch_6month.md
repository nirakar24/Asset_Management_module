# Asset Management Module — 6-Month Delivery Pitch

**Product:** Asset Management (ITAM) module on our Endpoint Privilege Management (EPM) platform
**Date:** 2026-06-23
**Thesis:** We already run a real-time agent on every endpoint. We turn that first-party telemetry into a **security-converged ITAM/CAASM layer** — clean asset data and security action in one place, without the cost, complexity, or blind spots of incumbents.

---

## Why Us (the wedge)

- **First-party ground truth** — real-time agent data, not third-party aggregation (Axonius) or stale CMDB syncs.
- **Born security-converged** — asset + risk + action in one console, not bolted together (the CAASM trend everyone is chasing).
- **Fast time-to-value** — no new agent rollout, no $50K–$500K implementation. It's already deployed.

---

## The 6-Month Roadmap (3 Phases)

### Phase 1 — Foundation (Months 1–2): "See everything we manage"
- Hardware asset inventory from the existing EPM agent (real-time, auto-updating)
- Auto-discovery + auto-classification of endpoints (Windows/macOS/Linux)
- **Basic software inventory** — installed-application list per endpoint (we already collect this for EPM; surfacing it is low-cost, high-value)
- Core asset dashboard, search, custom fields, tagging
- Asset detail view with ownership + status

### Phase 2 — Depth (Months 3–4): "Track software, lifecycle, relationships"
- Software inventory **deepened**: versioning, publisher normalization, usage/metering, unauthorized-software flags
- Software license tracking & compliance flags
- Asset lifecycle (procure → deploy → retire) with state + history
- Lightweight CMDB: asset relationships & dependency view
- Warranty / contract / EOL tracking with expiry alerts

### Phase 3 — Convergence (Months 5–6): "Make assets actionable for security"
- Asset risk context: tie each asset to its EPM posture & vulnerabilities (CAASM core)
- Exposure/coverage gaps view (unmanaged, non-compliant, risky assets)
- One-click action: trigger EPM policy/remediation directly from an asset
- CMDB complementarity: enrich/sync to ServiceNow & other CMDBs (don't replace)
- Reporting & exportable dashboards (a deliberate fix for the market's #1 complaint)

---

## Feature Matrix vs. Market

Our column shows **when we ship it** (P1/P2/P3) and the end-of-6-month strength. Competitor ratings reflect today's market.

### Discovery & Inventory
| Capability | **Us** | When | ServiceNow | Tanium | Axonius | Lansweeper | Freshservice | ManageEngine |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Real-time, first-party agent data | ✅ | P1 | ◐ | ✅ | ❌ agentless | ◐ | ◐ | ◐ |
| Endpoint hardware inventory | ✅ | P1 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Auto-discovery & classification | ✅ | P1 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Basic software inventory (installed apps) | ✅ | **P1** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Software versioning / publisher normalization | ✅ | P2 | ✅ | ◐ | ◐ | ◐ | ◐ | ✅ |
| Software usage / metering | ◐ | P2 | ✅ | ◐ | ❌ | ◐ | ◐ | ✅ |
| Non-endpoint assets (network/OT/cloud) | ◐ via integrations | P3 | ✅ | ✅ | ✅ | ✅ | ◐ | ◐ |

### Management & Governance
| Capability | **Us** | When | ServiceNow | Tanium | Axonius | Lansweeper | Freshservice | ManageEngine |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| License tracking & compliance flags | ◐ | P2 | ✅ | ◐ | ◐ | ◐ | ◐ | ✅ |
| Deep SAM / license true-up | ❌ post-6mo | — | ✅ | ◐ | ◐ | ◐ | ◐ | ◐ |
| Lifecycle (procure → retire) + history | ✅ | P2 | ✅ | ◐ | ❌ | ◐ | ✅ | ✅ |
| Warranty / contract / EOL alerts | ✅ | P2 | ✅ | ❌ | ❌ | ◐ | ✅ | ✅ |
| CMDB / relationship & dependency mapping | ◐ lightweight | P2 | ✅ | ◐ | ◐ complements | ◐ | ✅ | ◐ |
| CMDB sync/enrich (ServiceNow etc.) | ✅ | P3 | n/a | ◐ | ✅ | ✅ | ◐ | ◐ |
| Reporting & exportable dashboards | ✅ | P3 | ✅ | ◐ | ◐ | ◐ | ◐ weak | ◐ weak |

### Security Convergence (CAASM) — our differentiator
| Capability | **Us** | When | ServiceNow | Tanium | Axonius | Lansweeper | Freshservice | ManageEngine |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Asset-to-risk/vulnerability context | ✅ | P3 | ◐ | ✅ | ✅ | ◐ | ❌ | ◐ |
| Coverage/exposure gap detection | ✅ | P3 | ◐ | ✅ | ✅ | ◐ | ❌ | ◐ |
| One-click remediation (EPM-native) | ✅ | P3 | ◐ | ✅ | ◐ via others | ❌ | ❌ | ◐ |

### Commercial
| Capability | **Us** | When | ServiceNow | Tanium | Axonius | Lansweeper | Freshservice | ManageEngine |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Fast deploy (agent already present) | ✅ | P1 | ❌ | ❌ | ◐ | ✅ | ✅ | ◐ |
| Low / predictable TCO | ✅ | P1 | ❌ | ❌ | ◐ | ◐ renewal hikes | ✅ | ✅ |

**Legend:** ✅ strong · ◐ partial / limited / planned-lightweight · ❌ absent · **When** = phase we ship it (P1 = M1–2, P2 = M3–4, P3 = M5–6)

---

## How We Compete

- **vs. ServiceNow / Flexera (enterprise):** we don't out-feature them — we win on **time-to-value and price**. No heavy implementation, security action built in.
- **vs. Axonius (CAASM):** they aggregate what other tools see; **we are the source**. First-party telemetry is cleaner and real-time.
- **vs. Tanium:** comparable first-party agent strength, but we arrive **already inside the security/EPM workflow** at a fraction of the cost.
- **vs. Freshservice / ManageEngine (mid-market):** we match core ITAM and add **native security convergence** they don't have.

**Honest boundaries:** deep SAM/license compliance and full non-endpoint (network/OT/cloud) coverage are post-6-month. We *complement* the CMDB, we don't replace it.

---

*Supporting research: [competitor_analysis.md](competitor_analysis.md) · [drawbacks.md](drawbacks.md)*
