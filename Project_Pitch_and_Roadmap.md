# Asset Management Module — Pitch & Plan

Date: 2026-06-24

## The Idea

We already run a security agent on every customer laptop. We'll use it to build an Asset
Management product: one always-current list of every device, its software, and how risky it is.
Most tools tell you what you own. Ours also tells you who has access and whether that's a
security risk. No affordable competitor has that built in.

## Why It Wins

- Fast: results in minutes, because the agent is already installed. No months-long setup.
- Security built in: asset data and risk in one place.
- We're the source: data comes straight from the machine, not second-hand from other tools.
- Cheap and predictable pricing: no six-figure setup, no surprise renewal hikes.

## Competitors

| Competitor | Weakness |
|---|---|
| ServiceNow / Flexera | Expensive, slow, heavy to set up |
| Tanium | Costly; not built into security |
| Axonius | No agent of its own; only re-uses other tools' data |
| Lansweeper | Steep renewal price hikes; shallow security |
| Freshservice / ManageEngine | Good basics, but no built-in security view |

## Feature Comparison

✅ strong · ◐ partial · ❌ none. "When" = the phase we ship it.

| Capability | Us | When | ServiceNow | Tanium | Axonius | Lansweeper | Freshservice | ManageEngine |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Live, always-current device data | ✅ | P1 | ◐ | ✅ | ❌ | ◐ | ◐ | ◐ |
| Hardware & installed-software inventory | ✅ | P1 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Who has admin access, and is it risky | ✅ | P1 | ❌ | ◐ | ◐ | ❌ | ❌ | ❌ |
| Fast deploy + low, predictable cost | ✅ | P1 | ❌ | ❌ | ◐ | ◐ | ✅ | ✅ |
| License tracking + lifecycle + warranty alerts | ✅ | P2 | ✅ | ◐ | ❌ | ◐ | ✅ | ✅ |
| Reports, dependency maps, other-tool sync | ✅ | P3 | ✅ | ◐ | ◐ | ◐ | ◐ | ◐ |
| One-click security fix; cloud/SaaS coverage | ✅ | P3–P4 | ◐ | ✅ | ◐ | ❌ | ❌ | ◐ |

## The Plan

| Phase | When | What customers get |
|---|---|---|
| Phase 1 | By 2nd week of Aug 2026 | Live device & software inventory + security risk score |
| Phase 2 | Sept–Oct 2026 | Software licenses, lifecycle & warranty alerts, agentless discovery |
| Phase 3 | Nov–Dec 2026 | Reports, dependency maps, other-tool sync, standalone product |
| Phase 4 | Jan–Feb 2027 | Cloud/SaaS coverage, deeper security |

## Phase 1 — Target: 2nd week of August 2026

Goal: every Windows device a customer owns shows up on its own, with full hardware and software
details and a security view of who has admin access — all kept current automatically.

What we deliver:

1. **Live device inventory.** Each managed Windows laptop and desktop appears automatically in
   one list, with its make, model, CPU, memory, disk, and operating system. When something
   changes on a machine, the record updates on its own. No spreadsheets, no nightly scans.

2. **Software list per device.** For every machine we show the applications installed on it. This
   answers questions like "which machines have App X?" in seconds, which matters for security
   alerts, removals, and standardization.

3. **Security view.** For each device we show who has admin rights, whether admin access was used
   recently, and whether the machine is running an outdated operating system or unapproved
   software. This is information the affordable competitors don't collect, and it comes directly
   from the security agent we already run.

4. **Dashboard.** A single screen to search and filter all assets, a detail page per device
   showing its full profile and the security view, and one-click export to Excel for reports and
   audits.

Why it's on time: the agent already runs on every customer machine and already collects much of
this data. In Phase 1 we organize what we can already see and reuse our existing setup (how
machines connect, how users log in, how permissions work) rather than building from scratch. We
also tackle the hardest part — tying device data to security access — first, so the riskiest work
is proven early, not left to the end.

How we know it's done: install the asset agent on a Windows machine that already runs our security
agent, and within minutes it appears in the dashboard with its hardware, software list, and admin
access details — and exports cleanly to a spreadsheet.

Not in Phase 1: Windows only (Mac and Linux come later); deep software-license auditing and
non-laptop assets (network gear, cloud) come in Phases 2–3; the standalone version, sold to
customers who don't have our security product, arrives in Phase 3.
