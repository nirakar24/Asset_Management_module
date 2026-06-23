# Asset Management Module — Feature Guide (with Examples)

**Product:** Asset Management (ITAM) module on our Endpoint Privilege Management (EPM) platform
**Date:** 2026-06-23
**Purpose:** Explain every feature in plain language — *what it does*, *a concrete example*, and *why it's practically useful*.

> Companion docs: roadmap & matrix in [product_pitch_6month.md](product_pitch_6month.md) · market context in [competitor_analysis.md](competitor_analysis.md) · pain points in [drawbacks.md](drawbacks.md).

---

## Phase 1 — Foundation (Months 1–2)

### Real-time, first-party agent data
- **What it does:** Pulls asset data directly from the EPM agent already running on every endpoint — live, not from a periodic network scan or a third-party feed.
- **Example:** A laptop gets a new 16GB→32GB RAM upgrade Monday morning. By the time IT opens the console, the asset record already shows 32GB — no manual edit, no nightly scan delay.
- **Why useful:** Eliminates the #1 ITAM pain — stale, inaccurate data. What you see is what's actually on the machine *right now*.

### Endpoint hardware inventory
- **What it does:** Auto-collects make/model, serial number, CPU, RAM, disk, OS, BIOS, and peripherals for every managed endpoint.
- **Example:** Finance asks "how many machines can't run the new app that needs 16GB RAM?" You filter `RAM < 16GB` and get an instant list of 42 laptops to upgrade or replace.
- **Why useful:** Turns "we think we have about 500 laptops" into an exact, queryable inventory — the basis for budgeting, refresh planning, and audits.

### Auto-discovery & classification
- **What it does:** Automatically detects new endpoints as they come online and tags them by type (laptop/desktop/server) and OS, with no manual data entry.
- **Example:** A new hire's laptop is imaged and joins the network. It appears in the inventory within minutes, auto-classified as "Windows Laptop," ready to assign an owner.
- **Why useful:** No spreadsheet upkeep; the inventory stays complete on its own as devices come and go.

### Basic software inventory (installed apps)
- **What it does:** Lists every application installed on each endpoint (name + presence), collected by the same agent.
- **Example:** A zero-day hits "Acme PDF Reader v9." You search the software inventory and instantly see the 87 machines that have it installed.
- **Why useful:** Answers "who has what software?" in seconds — critical for incident response, removals, and standardization. (Shipped early because we already collect this for EPM.)

### Core asset dashboard, search, custom fields, tagging
- **What it does:** A central screen to search/filter all assets, plus custom fields (e.g., "Department," "Cost Center") and tags for grouping.
- **Example:** Tag all devices in the Mumbai office "site:mumbai," then filter to that tag during an office-wide audit.
- **Why useful:** Makes the inventory *usable* — slice it however your org actually thinks about assets, not just how the tool ships by default.

### Asset detail view with ownership + status
- **What it does:** A single page per asset showing its full profile, assigned owner/user, and status (active / in-repair / retired).
- **Example:** Helpdesk gets a ticket "my laptop won't boot." They open the asset, see it's assigned to that user, under warranty until next year, and was last seen 2 days ago.
- **Why useful:** One source of truth per device — faster support, clear accountability, no "whose machine is this?" guesswork.

---

## Phase 2 — Depth (Months 3–4)

### Software versioning / publisher normalization
- **What it does:** Captures exact version numbers and cleans up messy publisher names (e.g., "Microsoft Corp.," "Microsoft Corporation," "MSFT" → **Microsoft**).
- **Example:** Instead of 5 different "Chrome" entries, you see one normalized "Google Chrome" with a version breakdown: 60% on v140, 40% on outdated v128.
- **Why useful:** Accurate counts and clean reporting. Without normalization, license counts and patch reports are garbage.

### Software usage / metering
- **What it does:** Tracks whether installed software is actually being *used*, and how often.
- **Example:** You find 120 installs of a $600/seat design tool, but 45 haven't been launched in 90 days. Reclaim those licenses and save ~$27,000/year.
- **Why useful:** Directly cuts software spend by exposing shelfware — one of the fastest ROI wins in ITAM.

### License tracking & compliance flags
- **What it does:** Records how many licenses you own per product vs. how many are installed, and flags over-deployment.
- **Example:** You own 100 licenses of a database tool but the agent finds 113 installs — a red compliance flag appears before the vendor's audit does.
- **Why useful:** Avoids surprise audit penalties and gives you early warning to true-up or remove installs.

### Asset lifecycle (procure → deploy → retire) + history
- **What it does:** Tracks each asset through its stages and keeps a full timeline of changes (owner, status, location).
- **Example:** A laptop's history shows: procured Jan → assigned to Alice Feb → reassigned to Bob Aug → retired next year. When Bob leaves, you know exactly what to reclaim.
- **Why useful:** Clean offboarding, accurate refresh forecasting, and an audit trail for compliance.

### Warranty / contract / EOL alerts
- **What it does:** Stores warranty end, contract, and end-of-life dates, and proactively alerts before they expire.
- **Example:** 30 days before 50 laptops fall out of warranty, you get an alert — time to extend coverage or schedule replacement instead of paying out-of-pocket repairs.
- **Why useful:** Turns reactive surprises ("this is out of warranty?") into planned, budgeted decisions.

### Lightweight CMDB: relationships & dependency view
- **What it does:** Maps how assets relate — e.g., which endpoints depend on a given server or application.
- **Example:** Before patching a file server, you see the 80 endpoints that mount shares from it, so you schedule the reboot for off-hours.
- **Why useful:** Prevents "I didn't know that would break X" outages by making dependencies visible before you act.

---

## Phase 3 — Convergence (Months 5–6) — *our differentiator*

### Asset-to-risk / vulnerability context
- **What it does:** Joins each asset to its security posture — known vulnerabilities, EPM policy state, missing patches.
- **Example:** The asset view of a CFO's laptop shows it has 3 critical CVEs and local admin still enabled — risk and identity in one place.
- **Why useful:** Bridges the ITAM↔security gap. You stop asking "what assets do we have?" and "which are risky?" in two different tools.

### Coverage / exposure gap detection
- **What it does:** Surfaces assets that are unmanaged, non-compliant, or missing protection — the blind spots.
- **Example:** A dashboard shows 12 endpoints with the EPM agent installed but no asset record reconciled, and 5 devices that haven't checked in for 30 days — your true exposure surface.
- **Why useful:** You can't secure what you can't see. This is the core CAASM value — closing the gaps attackers exploit.

### One-click remediation (EPM-native)
- **What it does:** Lets you act on an asset directly from its record using our existing EPM engine — revoke admin rights, block an app, push a policy.
- **Example:** You spot vulnerable "Acme PDF Reader v9" on 87 machines, click "block," and the EPM agent enforces it everywhere — from the same screen, no separate tool.
- **Why useful:** Closes the loop: *find the problem → fix it instantly*. Competitors show you the problem but hand you off to another product to fix it.

### CMDB sync / enrich (ServiceNow etc.)
- **What it does:** Pushes our clean, real-time asset data into existing CMDBs to fill gaps and correct stale records — complementing, not replacing them.
- **Example:** Your ServiceNow CMDB is missing 200 endpoints and has 50 duplicates; our sync adds the missing ones and flags the dupes automatically.
- **Why useful:** Customers keep their system of record but get it *trustworthy* — adoption without rip-and-replace.

### Reporting & exportable dashboards
- **What it does:** Build, schedule, and export reports/dashboards on any asset or software dimension.
- **Example:** Auto-email leadership a monthly "hardware refresh due + software compliance status" PDF, and export the audit list to CSV on demand.
- **Why useful:** Directly fixes the market's most common complaint (weak reporting) and makes the data actionable for non-technical stakeholders.

---

## At a Glance: Feature → Pain It Solves

| Feature | Pain point it kills |
|---|---|
| Real-time first-party data | Stale, inaccurate inventory |
| Software inventory + metering | Shelfware, unknown software spread |
| License tracking | Surprise audit penalties |
| Lifecycle + warranty alerts | Reactive surprises, messy offboarding |
| CMDB relationships | "I didn't know that would break X" outages |
| Asset-to-risk context + gap detection | ITAM↔security blind spots (CAASM) |
| One-click EPM remediation | Find-but-can't-fix tool fragmentation |
| CMDB sync | Stale system-of-record without rip-and-replace |
| Reporting | Data goes in, answers don't come out |

---

*See [product_pitch_6month.md](product_pitch_6month.md) for the phased roadmap and competitive matrix.*
