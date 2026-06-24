# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A **research-and-planning repository — markdown only, no code, no build/lint/test.** It contains the
market research, competitor analysis, and product/engineering plan for a new **Asset Management (ITAM)
module** to be built on top of an existing product, **EpmBackend** (an Endpoint Privilege Management
platform). The actual EPM and ITAM-module source code lives elsewhere; this repo is the reference that
informs that build.

Work here is editing/extending documents, keeping them mutually consistent, and (when researching)
adding sourced, verified claims. There is nothing to compile or run.

## The product context these docs assume

- **EpmBackend** = .NET / MySQL, self-hosted, multi-tenant (orgs/groups; RBAC super_admin / org_admin /
  group_admin), with a **Windows-only agent** doing real-time monitoring (SignalR heartbeat) and taking
  commands over RabbitMQ. EPM capabilities: privilege management, application control, watermarking/DLP.
- **The wedge / anchor differentiator** (locked with the user, repeated across docs): **EPM-enriched
  asset records + privilege-aware risk scoring** — "know not just what your endpoints are, but who has
  access at what privilege level and why." The strategic frame is **CAASM / ITAM-security convergence**:
  first-party agent ground truth made actionable for security.
- **Locked decisions** (see `AM_Module_Development_Plan.md`): separate AM agent process that reuses EPM
  enrollment/transport when co-installed and runs standalone otherwise; Windows-only in Phase 1
  (macOS/Linux later); separate AM microservice + own DB schema, integrated via APIs/events.

## Document map (how the files relate)

Research/evidence layer (feeds everything downstream):
- `ITAM_Market_Research.md` — the large (~940-line) landscape + per-vendor analysis; the master reference.
- `ITAM_Market_Comparison_Report.md` — market vs. EpmBackend; identifies EPM's structural gaps.
- `competitor_analysis.md` — vendor-by-vendor competitive read; source for `drawbacks.md`.
- `drawbacks.md` — the industry's recurring pain points, synthesized from `competitor_analysis.md`.

Plan/product layer (built on the research):
- `AM_Module_Development_Plan.md` — the 6-month phased engineering build plan; most authoritative on scope/decisions.
- `product_pitch_6month.md` — the 3-phase roadmap & pitch.
- `feature_guide.md` — every feature in plain language (what / example / why).
- `feasibility_and_flow.md` — per-feature: achievable?, dependencies, high-level build flow, difficulty rating.

The plan docs cross-link each other in their headers (companion-doc lines). When you change scope,
phasing, or the feature set in one, check the others link-referenced from its header and keep them
aligned — the same Phase 1/2/3 structure recurs across `AM_Module_Development_Plan.md`,
`product_pitch_6month.md`, `feature_guide.md`, and `feasibility_and_flow.md`.

## Conventions to preserve

- **The EPM codebase is NOT in this repo.** Any statement about EPM internals (exact .NET version, ORM/
  migration conventions, ID/GUID format, JWT/JWKS setup, whether a SAM design already exists) is an
  **assumption to verify against the real EPM code before coding** — `AM_Module_Development_Plan.md` calls
  this out explicitly. Do not invent EPM specifics; name components by role and flag them as to-confirm.
- **Dates are absolute** (the docs are dated 2026; today is 2026-06-24). Keep using explicit dates, not
  relative ones.
- **Research claims are adversarially verified.** The comparison/competitor docs were built with "3-vote
  adversarial verification" and distinguish Tier 1 (verified) from Tier 2 (sourced but not re-verified)
  claims, with caveats. When adding research, cite sources inline with attribution and preserve that
  Tier 1 / Tier 2 distinction rather than presenting unverified claims as consensus.
- Difficulty legend used in `feasibility_and_flow.md`: 🟢 Low / 🟡 Moderate / 🔴 Hard — reuse it there.
