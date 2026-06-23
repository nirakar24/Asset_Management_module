# IT Asset Management (ITAM) Competitor Analysis

**Prepared for:** Product team building an Asset Management module on an Endpoint Privilege Management (EPM) product
**Date:** 2026-06-23
**Method:** Tier 1 core claims via multi-source research with 3-vote adversarial verification (21 confirmed; 4 refuted, excluded). Tier 2 vendors, Microsoft, and pricing via a follow-up sourced research pass (official pricing/feature pages + G2/Gartner/Capterra/news; not adversarially re-verified — see Caveats).

---

## Executive Summary

The ITAM market is mid-sized and growing steadily, with credible analyst sizings ranging from ~USD 2.09B (Mordor Intelligence, 2025) to ~USD 4.66B (TBRC/GII, 2025) depending on whether the definition is software-only or software-plus-services, with CAGRs clustering around 6-8%. The competitive frontier in 2025-2026 is no longer feature checklists for HAM/SAM/CMDB but rather (a) **agentic AI** (ServiceNow Yokohama AI Agents, Ivanti Neurons autonomous operations, Tanium Ask Agent) and (b) **IT/security operations convergence and CAASM** (Tanium's "Autonomous IT Platform," Axonius's aggregation/actionability model). Tier 1 leaders differentiate on data quality and scale — Flexera on its 98%+ normalization catalog (often *integrating* with rather than competing against BMC and ServiceNow CMDBs), and Tanium on real-time, agent-based endpoint telemetry at enterprise scale. For an EPM-based product, the strategic gap and opportunity is the **CAASM/security-convergence angle**: the market is rewarding products that unify asset visibility with security action, and an EPM foundation already owns real-time endpoint context that aggregation-based competitors (Axonius) must source from other tools.

---

## Market Size and Growth

| Source | 2025 Size | Forecast | CAGR | Confidence |
|---|---|---|---|---|
| Mordor Intelligence | USD 2.09B | USD 3.01B by 2031 | 6.28% (2026-2031) | High |
| TBRC / GII Research | USD 4.66B | USD 5.04B (2026) | 8.0% | Medium |

The wide spread reflects differing market definitions (software-only vs. software+services). Both figures should be cited *with attribution* rather than presented as consensus. (Sources: mordorintelligence.com; giiresearch.com/TBRC)

---

## Tier 1: Enterprise Leaders

### ServiceNow
- **Positioning:** Enterprise ITSM/ITAM platform with deep ITIL coverage, CMDB/Service Graph at the core.
- **2025-2026 developments:** The **Yokohama release (GA end of Q1 2025)** introduced "agentic AI at scale," including preconfigured **AI Agents** and an **AI Agent Orchestrator** ("AI agent control tower"). CMDB/Common Service Data Model positioned as core driver of the AI.
- **Differentiator:** Workflow automation at enterprise scale on top of an authoritative CMDB.
- **Confidence:** High (independently corroborated by ServiceNow newsroom, CIO.com, Constellation Research).

### Ivanti (Neurons for ITAM)
- **Positioning:** Hyperautomation platform using supervised/unsupervised ML to "discover and fix issues automatically before users even know about them." Self-heal / self-secure / self-service framing.
- **2025-2026 developments:** **April 21, 2026** — Neurons platform announced as a **unified agentic AI framework** with "data authority across devices, lifecycle status, entitlement, support context, and relationships," enabling endpoints to be "continuously discovered, secured, patched and supported with minimal human intervention." Strong shift toward an autonomous operating model.
- **Caveat:** Autonomy/AI claims are vendor positioning + early-adopter metrics (e.g., "up to 80% of endpoint issues resolved before users reported them"), not third-party benchmarked.
- **Confidence:** High (primary press releases corroborated by SiliconANGLE, Help Net Security, PR Newswire).

### Flexera (Flexera One)
- **Positioning:** Data-quality / enrichment layer for SAM/HAM. Notably **integrates** with — rather than competes against — other Tier 1 vendors, automatically building and maintaining the BMC Helix CMDB (and ServiceNow CMDB) with normalized user/software/hardware data. Framed as "true partnership, not channel conflict."
- **Differentiator:** Claims **98%+ software/hardware normalization rates** backed by "the world's largest enriched technology catalog" (Technopedia: 5M+ products, 100K+ vendors, 4,500+ daily curated updates).
- **Third-party standing:** Holds a **7.6 composite score** on Info-Tech SoftwareReviews' ITAM Enterprise category — higher than Ivanti Neurons for ITAM (6.6).
- **Caveat:** 98% is a vendor self-claim; no independent benchmark found.
- **Confidence:** High.

### BMC (Helix)
- **Positioning:** BMC Helix Discovery is positioned as a cloud-native (also available on-prem) discovery and dependency-modeling system providing "instant visibility into hardware, software, and service dependencies across multi-cloud, hybrid, and on-premises environments." Uses agentless discovery via Discovery Outpost + cloud engine, dynamic service modeling, and SAAM.
- **Ecosystem note:** Frequently paired with Flexera for CMDB data quality (see above).
- **Confidence:** High (BMC primary materials + InvGate, Faddom, Virima 2026 comparisons).

### Tanium
- **Positioning:** **"Autonomous IT Platform"** that converges IT operations and security operations (endpoint management + exposure management + security operations) rather than a standalone ITAM tool. Targets large enterprises and federal/regulated industries (Federal/State Gov, Healthcare, Financial Services, Energy).
- **Core differentiator:** Real-time, **agent-based** endpoint asset visibility at enterprise scale (e.g., JLL: real-time intelligence into ~100k endpoints; 36M+ endpoints worldwide). Includes a dedicated "Real-Time Asset Visibility" use case — a CAASM hallmark — explicitly differentiating from **agentless** competitors.
- **2025-2026 developments:**
  - **Oct 15, 2025:** Launched **Tanium Ask Agent**, its first agentic AI experience — intelligent Q&A, insights, and autonomous endpoint changes from a natural-language prompt, with **human-in-the-loop oversight**.
  - **Q2 2026:** Named a **Leader in The Forrester Wave: Endpoint Management Platforms** (highest possible score in 15 criteria including Vision, Innovation, Adoption). Forrester: well-suited to enterprises with "broad, complex IT/OT endpoint environments requiring IT and security operations convergence." Platform spans endpoint management, exposure management, security operations, and autonomous AI workflows. Also a Leader in the inaugural 2026 Gartner MQ for Endpoint Management Tools and IDC MarketScape 2025-2026.
- **Confidence:** High (multiple primary + independent analyst sources).

### Microsoft (Intune / Configuration Manager / Defender / Entra)
- **Positioning:** *Not a dedicated ITAM/SAM vendor.* Microsoft has no standalone ITAM product and **no native CMDB**. ITAM-relevant inventory/discovery is a by-product of its endpoint management (Intune, Configuration Manager/MECM), security (Defender), and identity (Entra) platforms. The common enterprise pattern is to pair Microsoft tooling with a third-party ITAM/SAM tool that ingests Intune/MECM/Defender data.
- **Target segment:** M365/Enterprise customers already standardized on Windows, Entra ID, and the M365 estate. Aimed at IT operations and security teams, not asset/SAM managers.
- **Core ITAM-relevant capabilities:** Hardware/software inventory for *enrolled/managed* devices (Intune) and deeper inventory via MECM agents; Defender Vulnerability Management maintains software inventory tied to vulnerabilities; Defender device inventory + Security Exposure Management discover/map assets and can ingest external sources (Tenable, ServiceNow CMDB). **No** procurement-to-disposal lifecycle, **no** contracts/warranty tracking, **no** true SAM/license reconciliation, **no** native CMDB, and **blind to non-enrolled assets** (monitors, network gear, peripherals).
- **Pricing/licensing:** Intune Plan 1 $8/user/mo (bundled into M365 E3/E5, EMS); Plan 2 +$4; Intune Suite +$10; EPM add-on $3. Effective **July 1, 2026**, M365 E3 rises $36→$42 and E5 $57→$65, with E3/E5 newly bundling Intune Plan 2 + Remote Help + Advanced Analytics (E5 also adds EPM, Enterprise App Management, Cloud PKI).
- **Deployment:** Intune (cloud SaaS), MECM (on-prem agent, co-managed/hybrid), Defender/Exposure Management (cloud SaaS).
- **Strengths:** Deep native integration across endpoint/identity/security; inventory comes "for free" with licenses enterprises already own; Defender adds risk/exposure/criticality scoring and attack-surface mapping that pure ITAM tools lack; can ingest third-party + CMDB data.
- **Weaknesses:** Not a system of record for assets; fragmented across multiple consoles (Intune, MECM, Defender, Entra); full value often requires E5-tier licensing and/or third-party ITAM integration.
- **2025-2026 developments:** Security Copilot in Intune (GA — natural-language queries, KQL generation, report building); Security Copilot **agents** (Ignite 2025) across Defender/Entra/Intune/Purview, bundled with M365 E5 at no extra cost; Security Exposure Management consolidation with new external data connectors (Tenable, ServiceNow CMDB) and source attribution per asset.
- **Confidence:** Medium-High (Microsoft Learn primary docs + InvGate/AssetLoom third-party; "not a dedicated ITAM tool" framing well-corroborated).

---

## Tier 2: Mid-Market Leaders

### Axonius
- **Positioning:** **CAASM-style aggregation / "actionability platform."** Rather than performing direct discovery, it aggregates data from existing tools via **1,200+ adapters** (including **27 CMDB platforms**), continuously collecting **40+ asset types** (devices, users, software, vulnerabilities, configurations). Targets security-led teams.
- **Differentiator:** The **Asset Intelligence pipeline** reconciles overlapping/noisy records into a single trusted model, then compares against the CMDB to delete redundant entries and add missing assets/metadata. Designed to **complement** CMDBs like ServiceNow (fill gaps) rather than replace them. Offers a query builder (Query Wizard) and an **Enforcement Center** (500+ automated actions).
- **Key limitation (strategic):** Visibility is bounded by what existing tools already see (aggregation, not first-party discovery) — a structural weakness vs. agent-based real-time telemetry.
- **Confidence:** High (Axonius primary docs + Virima, Cloudaware, Carahsoft third-party corroboration).

### ManageEngine (AssetExplorer & Endpoint Central)
- **Positioning:** Two relevant products. **AssetExplorer** is a dedicated standalone ITAM tool (the ITAM-only sibling of ServiceDesk Plus) for SMB/mid-market. **Endpoint Central** is a Unified Endpoint Management + security platform (formerly Desktop Central) that bundles ITAM as one module alongside patch, deployment, remote control, MDM, and endpoint security.
- **Core capabilities:** Agent + agentless discovery (Windows/macOS/Linux + SNMP); software inventory, license-compliance dashboards, under-licensing detection, software metering; procurement-to-disposal lifecycle, PO/vendor/contract management. AssetExplorer has a real CMDB (~50 CI types with relationship modeling); Endpoint Central's CMDB is inventory/lifecycle-focused (limited).
- **Pricing (transparent):** AssetExplorer asset-count tiers — 250 assets $955/yr, 500 $1,795, 1,000 $2,995, 5,000 $9,595, 10,000 $11,995; **free edition up to 25 assets**. Endpoint Central per-device, min 50 endpoints (~$795–$1,695/yr by tier; third-party figures).
- **Deployment:** On-prem and cloud (SaaS); Endpoint Central has a dedicated MSP edition.
- **Strengths:** Intuitive, reliable discovery, strong software reconciliation, cost-effective all-in-one (Endpoint Central), easy native integration across the ManageEngine suite.
- **Weaknesses:** Steep learning curve, cluttered setup, weak reporting/audit-readiness, many features as separately-licensed add-ons, inconsistent support.
- **2025-2026 developments:** Endpoint Central leads ManageEngine's **security/CAASM convergence** — integrated Vulnerability Management + built-in Attack Surface Management feeding CAASM/XDR/Zero Trust, plus AI/NGAV behavioral threat detection. AssetExplorer has essentially **no AI or CAASM story** (FIPS 140-2 mode, SAML, UEM remote-control integration only).
- **Confidence:** High (official pricing/feature pages + G2/Gartner/Capterra).

### Freshservice (Freshworks)
- **Positioning:** Cloud-native ITSM suite with integrated ITAM — "enterprise-grade without the complexity," a fast-deploy alternative to ServiceNow. Sweet spot is SMB-to-mid-market. Gartner placed Freshworks as a **Niche Player** in the 2025 MQ for AI Apps in ITSM.
- **Core capabilities:** Lightweight Discovery Agent + network probe + API integrations (Azure AD, SCCM, Intune, cloud); audit-ready inventory; software estate tracking; multi-source CMDB with service dependency visualization. **April 2026** added continuous infrastructure discovery, live dependency mapping, predictive impact analysis, configuration drift detection, and license harvesting.
- **Pricing:** Core ITSM per agent/mo (annual): Starter $19, Growth $49, Pro $99, Enterprise custom (includes Freddy AI). **ITAM licensed separately by Asset Units** (packs of 500; consumption varies by asset type) — official AU pack prices not published (third-party ~$75/mo per 500-pack, unverified).
- **Deployment:** SaaS / cloud-native only (manages on-prem/cloud/hybrid assets via agents + probe; no self-hosted option).
- **Strengths:** Clean UI, fast deployment, strong automation, good service catalog, growing Freddy AI; 4.5/5 Capterra; lower entry threshold than ServiceNow.
- **Weaknesses:** ITAM depth criticized as shallow by some reviewers; reporting/analytics inflexible; advanced features gated behind higher tiers; less suited to large/compliance-heavy enterprises.
- **2025-2026 developments:** April 2026 ITAM expansion (above); Freddy AI (Copilot/Agent/Insights); May 2026 launched AI Agent Studio + MCP Gateway. CAASM-adjacent only (asset governance + drift detection), no standalone CAASM product.
- **Confidence:** High.

### Lansweeper
- **Positioning:** "Technology/Cyber Asset Intelligence" platform — evolved from network inventory into unified IT/OT/IoT/cloud visibility. 25,000+ customers, ~100M devices monitored; increasingly straddles ITAM and security/CAASM teams.
- **Core capabilities:** Best-in-class **agentless deep-scan discovery** (+ optional agents) with Credential-free Device Recognition across IT/OT/IoT/cloud; detailed hardware inventory; software inventory, license tracking, compliance reporting; warranty/EOL lifecycle (Pro); functions as a single source of asset truth with ITSM integrations (ServiceNow, Jira).
- **Pricing (quote-driven, asset-based):** Free (up to 100 assets); third-party figures: Starter ~$239/mo, Pro ~$439/mo (adds vuln mgmt, lifecycle, API), Enterprise custom. (Official pricing paywalled; per-user framing is aggregator interpretation — real contracts are per-asset.)
- **Deployment:** Hybrid — on-prem (classic) and SaaS/cloud (Lansweeper Sites/Platform).
- **Strengths:** Best-in-class auto-discovery/inventory depth; easy low-maintenance agentless deployment; flexible reporting; Gartner-recognized CAASM positioning.
- **Weaknesses:** **Steep renewal price increases** (top complaint); weak software-deployment tooling; slow support; configuration complexity; some integration gaps.
- **2025-2026 developments:** Explicit **CAASM repositioning**; **Redjack acquisition (July 17, 2025)** adds passive network-traffic discovery + app dependency mapping; Fall 2025 "Connect" launched **Lansweeper Lens** (natural-language AI assistant, preview), BI dashboards, Flow Builder, and a Redjack-based Traffic Sensor; €130M investment from Insight Partners. *(Note: the earlier draft's "Cybersift acquisition" claim was erroneous — the real 2025 acquisition is Redjack.)*
- **Confidence:** High.

### SolarWinds Service Desk
- **Positioning:** Cloud ITSM with integrated asset management (formerly Samanage). Targets SMB to mid-enterprise wanting fast-to-deploy, ITIL-aligned tooling. Distinct from SolarWinds' separate on-prem Web Help Desk.
- **Core capabilities:** Agentless (Discovery Scanner, continuous IP scanning) + agent-based discovery (only the agent pulls software inventory); 200+ data points per asset; contracts/licenses/warranties; license-compliance monitoring (deeper license optimization is thin); integrated CMDB tied to discovery (**visual CMDB with dependency mapping reserved for the top Premier tier**).
- **Pricing (per-technician/mo, annual, unlimited end-users):** Essentials $39, Advanced ~$79–99, Premier ~$99–124 (sources conflict). **Asset/device pricing is separate and quote-based** (per device, packs of 500, or unlimited; non-networked items free).
- **Deployment:** SaaS / cloud-only (on-prem needs are steered to Web Help Desk).
- **Strengths:** Ease of use / fast implementation; unified asset-plus-ticket context; solid core ITSM with AI/ML automation (G2 4.3, Capterra 4.6).
- **Weaknesses:** Shallow reporting; advanced ITAM (license optimization, service mapping, vuln mgmt) thin or extra-cost; cost scaling; inconsistent support.
- **2025-2026 developments:** **Taken private by Turn/River Capital** (announced Feb 7, 2025 at $18.50/share ~$4.4B; closed April 16, 2025; delisted from NYSE) with a strategic shift toward AI-driven observability; **Squadcast acquisition (2025)** adds AI incident response; SWSD AI resolution summaries + virtual agent (Advanced) and full AI suite (Premier).
- **Confidence:** High.

### Snipe-IT (open source)
- **Positioning:** Free, open-source (AGPL-3.0) ITAM on PHP/Laravel, maintained by Grokability, Inc. A lightweight hardware asset register and checkout/check-in tracker for small/mid IT teams, schools, nonprofits, MSPs graduating from spreadsheets.
- **Core capabilities:** Strong **hardware** tracking (assets, peripherals, accessories, consumables, components; checkout/check-in, custom fields, depreciation, audit logs, barcode/QR). **Shallow SAM** (license seats/counts/expiry, but no metering or entitlement-vs-installed reconciliation). **No native network discovery** (manual/CSV/REST API; native JAMF and Kandji connectors). **No CMDB**, **no workflow automation**.
- **Pricing:** Self-hosted edition **fully free** (unlimited assets/users). Cloud: Basic $39.99/mo, Small Business $99.99/mo, Dedicated (Small) $249.99/mo, Medium/Large $5,000–$7,500/yr.
- **Deployment:** Self-hosted on-prem, Docker, or Snipe-IT Cloud (managed).
- **Strengths:** Very low cost, ease of use, open-source customizability, fast lightweight UI, very active project (4.6/5 G2, 4.4/5 Capterra).
- **Weaknesses (structural):** No native discovery, shallow SAM, no CMDB, no workflow automation, no native multi-tenancy; heavy manual entry raises accuracy/scalability risk; weak reporting and historically weak SSO/SAML.
- **2025-2026 developments:** Actively maintained (v8.0 Feb 2025 UX overhaul; v8.5 → Laravel 12/PHP 8.5; v8.6.3 June 15, 2026). **No shipped end-user AI features** — its "AI Contribution Policy" governs contributors, not the product.
- **Confidence:** High.

---

## Pricing Comparison

> Enterprise (Tier 1) vendors are almost universally **quote-only**; dollar figures shown for them are third-party estimates, not official list prices, and should be treated as indicative ranges. Mid-market (Tier 2) tools publish more transparent pricing.

| Vendor | Model | Figures / Notes | Disclosure |
|---|---|---|---|
| ServiceNow ITAM | Quote-only; per-user, modular (HAM+SAM) | Est. ~$100/user/mo + large implementation/consulting; heavily customized | Estimate |
| Ivanti Neurons for ITAM | Quote-only; modular per-user | Estimates vary widely; implementation $5K–$150K | Estimate |
| Flexera One | Quote-only; by module, asset count, cloud spend | Est. ~$50K–$100K/yr (ITAM) up to $200K–$500K+/yr (full platform) | Estimate |
| BMC Helix | Quote-only | Not publicly disclosed | None |
| Tanium | Quote-only; per-endpoint, enterprise | Not publicly disclosed | None |
| Axonius | Quote-only; per-device/asset | Not publicly disclosed; no free plan | None |
| Microsoft Intune | Published; per-user/mo | Plan 1 $8 (bundled in M365 E3/E5), Plan 2 +$4, Suite +$10 | **Published** |
| ManageEngine AssetExplorer | Published; per-asset/yr | 250 assets $955 → 10,000 $11,995; **free ≤25 assets** | **Published** |
| ManageEngine Endpoint Central | Published; per-device/yr | ~$795–$1,695/yr by tier, min 50 endpoints | Published (3rd-party tiers) |
| Freshservice | Published ITSM tiers + separate asset packs | Agent: Starter $19 / Growth $49 / Pro $99 /mo; ITAM by Asset Units (pack prices not public) | Partial |
| Lansweeper | Quote-driven; per-asset (banded) | Free ≤100 assets; ~$239/mo Starter, ~$439/mo Pro, Enterprise custom | Partial (paywalled) |
| SolarWinds Service Desk | Published; per-technician/mo + separate asset pricing | Essentials $39 / Advanced ~$79–99 / Premier ~$99–124; assets quote-based | Partial |
| Snipe-IT | Open-source free + flat-rate cloud | Self-hosted **free**; cloud $39.99–$249.99/mo (+ $5K–7.5K/yr dedicated) | **Published** |

---

## Key Cross-Market Themes (2025-2026)

1. **Agentic AI is the new battleground.** ServiceNow (AI Agents/Orchestrator), Ivanti (autonomous Neurons), and Tanium (Ask Agent) all shipped agentic capabilities in the 2025-2026 window, all with human-in-the-loop framing.
2. **IT/Security convergence and CAASM.** Tanium (autonomous IT), Axonius (CAASM aggregation), and Ivanti (IT + security ops) are all collapsing the ITAM/security boundary — the most relevant trend for an EPM-based entrant.
3. **Data quality as a moat.** Flexera (98%+ normalization catalog) and Axonius (Asset Intelligence reconciliation) compete on turning messy multi-source data into a trusted single model.
4. **Agent vs. agentless architecture** is a genuine differentiator: Tanium's agent-based real-time telemetry vs. Axonius's agentless aggregation.

---

## Strategic Implication for an EPM-Based Asset Management Module

An EPM product already owns **real-time, agent-based endpoint context** — the exact asset that Axonius (agentless) must source externally and that Tanium markets as its core moat. The market is rewarding **security/ITAM convergence** and **agentic AI with human-in-the-loop control**. The clearest differentiation path is to position the Asset Management module as a security-converged, first-party-telemetry CAASM/ITAM layer rather than another agentless CMDB aggregator — while planning for CMDB *complementarity* (the Axonius lesson) rather than CMDB replacement.

---

## Caveats and Time-Sensitivity

- **Market sizing diverges widely** across firms (USD 2.09B to 4.66B for 2025); always attribute to the specific analyst.
- **Most AI/autonomy capability claims are vendor marketing or early-adopter metrics**, not independently benchmarked (Ivanti 80%, Flexera 98%, Tanium "autonomous execution").
- **Info-Tech SoftwareReviews scores are point-in-time** and update as reviews accumulate (Flexera 7.6 vs. Ivanti 6.6 as of mid-2026).
- **Tier 1 pricing is quote-only.** The Tier 1 dollar figures in the Pricing Comparison are third-party estimates, not vendor list prices; the originally-claimed ServiceNow ~$90 / Ivanti ~$80 per-user figures were refuted during verification.
- **Tier 2 detail comes from a follow-up research pass** (official pricing/feature pages + G2/Gartner/Capterra/news) and was not run through the same 3-vote adversarial verification as the Tier 1 core claims; treat per-figure precision as indicative, especially paywalled/aggregator pricing (Lansweeper, SolarWinds asset pricing, Freshservice AU packs).
- **Refuted (excluded) claims:** an earlier Lansweeper "Cybersift" acquisition (real acquisition is **Redjack**, July 2025); "top five vendors = ~38% share"; "$6.78B by 2030 at 7.7%"; ServiceNow $90 / Ivanti $80 per user pricing.

---

## Open Questions

1. **Tier 1 pricing** remains quote-only — engage vendor sales (or a buyer-guide service like Vendr) for real per-seat/per-asset/per-node figures for ServiceNow, Ivanti, Flexera, BMC, Tanium, and Axonius before any TCO modeling.
2. **Freshservice Asset Unit pack pricing** and **Lansweeper / SolarWinds asset (vs. technician) pricing** are not officially published — confirm with sales.
3. **HAM/SAM/lifecycle feature depth** would benefit from hands-on trials (most Tier 2 tools offer free tiers/trials: ManageEngine, Freshservice, Lansweeper, Snipe-IT) rather than relying on marketing pages.
4. **CAASM positioning depth** for the EPM angle — a focused deep-dive on Axonius, Tanium, Lansweeper, and ManageEngine Endpoint Central's CAASM claims would sharpen the differentiation strategy.
