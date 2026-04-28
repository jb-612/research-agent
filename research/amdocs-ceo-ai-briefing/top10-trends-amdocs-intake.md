---
title: "AI Market — What Actually Matters for the CEO"
subtitle: "Signal extracted from the noise — April 2026"
audience: CEO, Amdocs
created: 2026-04-28
sources: executive-brief.md · ai-market-update-april-2026.md · deep-dive-week-apr21-27.md · market-update-apr2026.md
pdf_options:
  format: A4
  landscape: true
  margin:
    top: 14mm
    right: 12mm
    bottom: 14mm
    left: 12mm
  printBackground: true
  preferCSSPageSize: true
  displayHeaderFooter: true
  headerTemplate: '<div style="font-size:8pt;color:#5f6c7b;width:100%;padding:0 12mm;display:flex;justify-content:space-between;"><span>AI Market — What Actually Matters for the CEO</span><span>Amdocs · April 2026</span></div>'
  footerTemplate: '<div style="font-size:8pt;color:#98a2b3;width:100%;text-align:center;">Page <span class="pageNumber"></span> of <span class="totalPages"></span></div>'
stylesheet_encoding: utf-8
body_class: brief
css: |
  @page { size: A4 landscape; margin: 0; }
  body { font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif; color: #1a1a1a; font-size: 9.5pt; line-height: 1.35; }
  h1 { font-size: 18pt; color: #0d2858; margin: 0 0 2mm 0; letter-spacing: -0.3px; }
  h2 { font-size: 13pt; color: #0d2858; margin: 6mm 0 2mm 0; border-bottom: 1px solid #c8d3e0; padding-bottom: 1mm; }
  h3 { font-size: 11pt; color: #0d2858; margin: 4mm 0 1.5mm 0; }
  p { margin: 0 0 2mm 0; }
  ul, ol { margin: 0 0 2mm 4mm; padding: 0; }
  li { margin-bottom: 0.8mm; }
  strong { color: #0d2858; }
  table { width: 100%; border-collapse: collapse; margin: 2mm 0 4mm 0; font-size: 8.5pt; line-height: 1.28; page-break-inside: auto; }
  thead { display: table-header-group; }
  tr { page-break-inside: avoid; }
  th { background: #0d2858; color: #fff; text-align: left; padding: 1.5mm 2mm; font-weight: 600; vertical-align: top; }
  td { padding: 1.5mm 2mm; border-bottom: 0.5px solid #d4dae3; vertical-align: top; }
  tr:nth-child(even) td { background: #f5f7fa; }
  a { color: #1565c0; text-decoration: none; }
  hr { border: none; border-top: 1px solid #c8d3e0; margin: 5mm 0; }
  blockquote { border-left: 3px solid #1565c0; padding: 1mm 3mm; margin: 2mm 0; color: #0d2858; font-style: italic; background: #f5f7fa; }
  code { background: #f0f3f7; padding: 0.3mm 1mm; border-radius: 1mm; font-size: 8.5pt; }
  h2 + table, h2 + p + table { margin-top: 1mm; }
---

# AI Market — What Actually Matters for the CEO

## The headline

The boardroom keeps hearing about "AI hype." Most of what reaches it is noise. The signal is three sentences:

1. **Customers are replacing operating models, not adding tools.** Allianz, JPM, Verizon, AT&T, EY, BBVA — these are platform-replacement programs, not features. Whoever owns the agent owns the customer process.
2. **The model is no longer the moat — customers swap on 30/60-day notice.** Every week the strategy assumes "we're aligned with vendor X" costs Amdocs optionality.
3. **The moat moved up the stack — to context, evaluations, harness, governance, observability, and cost control.** Every hyperscaler and SaaS-of-record vendor is now in the same land-grab. The question is which layers Amdocs defends.

---

## 1. New capabilities → what they actually mean commercially

| Capability that shipped | What's new | Commercial signal | Big change ahead? |
|---|---|---|---|
| **Moonshot Kimi K2.6** (Apr 2026): 300 sub-agents, 4,000 coordinated steps, **5 days fully autonomous** in Moonshot's own SRE [[1]](#ref-1) | Multi-day, multi-agent autonomous work is no longer a demo | A task that took a SI team a week is becoming an overnight job. Hourly-rate services revenue is the first to compress. | **Yes.** This is the proof point that BCG's "$200B opportunity *and* threat to tech services" thesis [[2]](#ref-2) is operational, not theoretical. |
| **DeepSeek V4 Preview** (Apr 2026): 1M-token context, MoE, dual-mode V4-Pro / V4-Flash [[3]](#ref-3) | Frontier-class capability at open-weight cost — Chinese cost disruption intact | Any RFP priced on 2025 model costs is **10× wrong by 2027**. Per-task price keeps falling 9–900× per year by capability tier [[4]](#ref-4). | **Yes for procurement.** Multi-year commercial commitments without a swap clause are now a financial risk, not a roadmap detail. |
| **Anthropic Claude Mythos** (Apr 2026): gated to ~40 critical-infrastructure orgs (Project Glasswing) at $25 / $125 per M tokens [[5]](#ref-5) | Selective frontier access — not all customers get the same model | Tiered-access becomes a procurement variable. "Can we get Mythos?" is a real RFP question. | **Yes for positioning.** Frontier model access is becoming a regulated good, not a self-serve API. |
| **Anthropic Opus 4.7** (Apr 22): new tokenizer uses **~35% more tokens** for fixed text — effective price up [[6]](#ref-6) | Sticker price is no longer the right metric | Token-bloat hits margin without anyone noticing. Cost-control is now a CFO topic, not an SRE one. | Quietly yes — surprises customers who priced 2025 deals on tokens-per-dollar. |
| **Apple Intelligence + Siri overhaul** (Mar 2026): on-screen awareness + cross-app actions [[7]](#ref-7); **Anthropic Claude Memory default-on** (Mar 3, 2026) [[8]](#ref-8) | The "agent shell" is a new platform tier — sits *outside* every application | A web-only enterprise app is invisible to the agent layer. Discoverability via MCP is the new SEO. | **Yes — long-fuse.** The "application" is being unbundled. Owning the agent-of-record for a customer process becomes more valuable than owning the UI. |

---

## 2. Major deals — where AI is changing the business model

The headline is no longer "Company X bought Y AI tool." It is **Company X is becoming the agentic system of record for process P** — and selling that as the new commercial frame.

| Vendor | Move | Business-model change | Amdocs read |
|---|---|---|---|
| **Salesforce** — Spring '26 (Apr 2026) | Dynamic Revenue Orchestrator + Decomposition Workspace + Agent Fabric + **Headless 360 (60+ MCP tools)** + Agentforce for Communications GA (named: One NZ, Lumen) [[9]](#ref-9)[[10]](#ref-10)[[11]](#ref-11) | Stops selling CRM. Sells **"agentic orchestration of revenue, order, catalogue, and engagement workflows — governed centrally, callable from any LLM client."** Direct invasion of BSS territory. | The CRM/CPQ/order slice **is gone or going.** Charging/OSS plane safe — for now. The gap closes every release; **the procurement question "you already buy Salesforce — why run a parallel orchestration stack?" is now in every Tier-1 conversation.** |
| **ServiceNow** | Now Assist ACV reportedly **>$600M**; Autonomous Network Operations co-build with Google Cloud (preview, GA late-2026) [[12]](#ref-12) | Positions explicitly as **"model-agnostic by design"** — that phrase is the new moat language. ServiceNow is the agentic system of record for IT and network ops. | Same playbook Amdocs needs to run. **"Model-neutral telco-AI control plane"** is the only positioning that survives the next 4–6 quarters. |
| **Allianz Project Nemo** (Nov 2025) — extending into motor/health 2026 | 7-agent claims pipeline on Anthropic; **80% claim-cycle reduction**, full workflow <5 min [[13]](#ref-13) | Anthropic is becoming the operational backbone of small-claims processing. Once a vendor is wired into a process at this depth, it's the system of record. | The pattern that beats Amdocs in BSS is the same — once an LLM vendor's agent runtime is wired into a billing dispute or service-activation flow at Allianz-Nemo depth, that vendor owns the process. |
| **JPMorgan Vault on Thought Machine** | **450+ AI use cases** on top of a core-banking replacement; ~$18B IT spend [[14]](#ref-14) | Agentic AI sits *across* the core ledger replacement, not bolted on. New core + agentic layer co-designed. | Verizon, AT&T, BBVA are doing exactly this in telco. Tier-1 operators **can and will build their own agentic platforms on hyperscaler infra** if Amdocs doesn't ship the productised version first. |
| **Mastercard Agent Pay · Visa Intelligent Commerce · Stripe ACS** | Stripe ACS live: **URBN, Etsy, Coach, Kate Spade, Halara** [[15]](#ref-15); **FIS + Visa + Mastercard "Know Your Agent" (KYA)** credentials Q1 2026 [[16]](#ref-16) | Payment networks become the agentic system of record for commerce. **Identity-of-the-agent is becoming the same procurement category as identity-of-the-employee.** | The KYA model is the template. Every B/OSS-side agent acting on a customer's account needs an identity, a policy boundary, and an audit trail — sellable as a platform feature. |
| **Workday Illuminate · BlackLine · Trintech · FloQast · Ledge · Nominal** | New "agentic close" category for financial close [[17]](#ref-17) | The general ledger is being abstracted behind an agent. The vendor that runs the agent owns the close. | Same shape will happen to telco BSS revenue close, dispute, and reconciliation processes. |
| **EY Canvas** (2026) | Multi-agent framework embedded across **160,000 audit engagements**, integrated with Microsoft [[18]](#ref-18) | Professional services are not advising on agents. They are **rebuilding their own delivery platforms around agents.** Same as Intel "One AI" internally [[19]](#ref-19). | Amdocs Services must do the same — convert threatened labour-hours into productised, governed, metered agent services *before* clients build their own. |
| **OpenAI × Deutsche Telekom** (Dec 2025) | ~200,000 DT employees onto ChatGPT Enterprise in phases; co-built network-ops, customer-service, finance, HR agents [[20]](#ref-20) | **Largest single telco-LLM-lab commitment to date.** A direct telco-tier procurement, not an SI-mediated one. | Hyperscaler/AI-vendor direct sale to operator. SI value-add must move up the stack — domain ontology + eval flywheel + regulatory packs + BSS-semantics expertise are what hyperscalers cannot ship. |
| **Microsoft Agent Framework v1.0 GA** (Apr 23) · **Google Gemini Enterprise Agent Platform** (Apr 22) · **Anthropic Claude Managed Agents** (Apr 8) [[21]](#ref-21)[[22]](#ref-22)[[23]](#ref-23) | Three production-ready hosted agent runtimes shipped in **two weeks** | The "agent runtime" is now a hyperscaler product line, not an SI build. | Choosing one is a 12-month bet. Choosing all three (model-neutral) is the only safe position. |
| **AWS × Amdocs MWC26** | Multi-year strategic collaboration for telco modernization on AgentCore [[24]](#ref-24) | Real co-engineering, real distribution. | **Cannot become exclusive packaging.** The Microsoft–OpenAI moat that anchored countless 2025 strategies got materially smaller in April 2026 (Anthropic–Pentagon spat) [[25]](#ref-25) — six-week half-life on hyperscaler exclusivity assumptions. |

---

## 3. The model is no longer the moat — and customers proved it

The single most under-appreciated finding of this quarter:

- **Anthropic donated MCP to the Linux Foundation** (Dec 2025) — protocol layer is now neutral [[26]](#ref-26).
- **Salesforce, SAP, ServiceNow, Microsoft, AWS** all run A2A in production. The protocol assumes portability [[27]](#ref-27).
- **ServiceNow's marketing line** is now literally *"model-agnostic by design, giving customers the flexibility to leverage their preferred provider"* [[12]](#ref-12).
- **The "LangChain Exit"** — production teams quietly migrating off framework lock-in to raw vendor SDKs [[28]](#ref-28).
- **"Model substitution clauses"** with 30/60-day swap rights are appearing in regulated-industry RFPs [[29]](#ref-29).

**Customers are switching despite enormous investment.** AT&T sits on ~27B tokens/day on Azure OpenAI Foundry [[30]](#ref-30) — and will move tomorrow if Anthropic or Google delivers more value. Verizon went all-in on Vertex/Gemini and got +40% sales lift through service teams [[31]](#ref-31) — and built it portable.

**Implication for Amdocs.** Any pitch that leans on "we co-engineer with Microsoft and OpenAI" needs a new pitch. The defensible commercial frame is *"we are the model-neutral orchestration and operations layer for your AI-native BSS/OSS"* — same playbook ServiceNow runs. **Model portability in CES26 / aOS / Agentic Services is now a customer-RFP requirement, not a roadmap aspiration.**

---

## 4. Where the moats are actually forming — the vendor land-grab

Since the model is not the moat, every hyperscaler, SaaS-of-record vendor, and SI is racing to plant flags on the **six layers above the model**. This table is the real competitive map for 2026.

| Moat layer | Why it's a moat | Who's planting flags (concrete moves) | Amdocs intake |
|---|---|---|---|
| **Context — knowledge graph + ontology** | Telco/finance/insurance domain semantics cannot be shipped by a hyperscaler | **Microsoft Fabric IQ Ontology** (preview, billing H1 2026) [[32]](#ref-32) · **Palantir Ontology** (12-layer agentic architecture) [[33]](#ref-33) · **Salesforce Headless 360** (everything-as-MCP-tool) [[10]](#ref-10) · **Salesforce Data Cloud** segment-driven eligibility · **TM Forum AAIF + Agentic Interactions Security** for telco [[34]](#ref-34) · GraphRAG ~86% vs ~32% baseline vector RAG [[35]](#ref-35) | The **SID-aligned telco knowledge graph** is Amdocs's single most defensible asset. Productize it as a queryable Context Manager — that is what compounds across model transitions. |
| **Evaluations** | Domain ground-truth + continuous online eval = the only safe model-swap mechanism | **AWS Bedrock AgentCore Policy + Evaluations Preview** (Apr 2026) [[36]](#ref-36) · **Microsoft Copilot Studio** evals · **Datadog AI Agent Monitoring + LLM Experiments** [[37]](#ref-37) · **Langfuse** · **LangSmith** | Pair telco ground-truth + tiered judge fleet (distilled @100% sampling, frontier @1–5%, humans <0.1%). The **Bain "23%" gap** [[38]](#ref-38) (only 23% of firms tie GenAI to measurable revenue/cost) is the procurement question — eval suite is the answer. |
| **Harness — runtime, replay, durable state** | Production-grade agent execution with audit and recovery | **Microsoft Agent Framework v1.0 GA** (Apr 23) [[21]](#ref-21) · **Google Gemini Enterprise Agent Platform** (Apr 22) [[22]](#ref-22) · **Anthropic Claude Managed Agents** (Apr 8) [[23]](#ref-23) · **AWS Bedrock AgentCore** modular runtime [[39]](#ref-39) · **Salesforce Agent Fabric** [[10]](#ref-10) · **LangGraph 1.0** (Oct 2025) [[40]](#ref-40) | aOS as the **model-neutral telco agentic control plane** — this is the ServiceNow positioning. Replay, durable state, sandboxing must be telco-grade. |
| **Compliance / governance** | EU AI Act + DORA + NYDFS + ISO 42001 = audit-ready or out | **Microsoft Agent Governance Toolkit** open-sourced (Apr 2) [[41]](#ref-41) · **Anthropic Project Glasswing** (Cisco joined Apr 7, $100M credits, AWS/Apple/Google/MS/Nvidia/Palo Alto/JPM/CrowdStrike on board) [[42]](#ref-42) · **"Know Your Agent"** credentials (FIS+Visa+Mastercard) [[16]](#ref-16) · **ISO/IEC 42001** — 100+ certified orgs in 18 months [[43]](#ref-43) | Per-tenant residency w/ cryptographic guarantees · agent-action-level audit logging · automated model/data card generation · HITL scaffolding as platform primitive. **Sovereign deployment will appear in tier-1 telco RFPs within 12 months** (Cohere acquired Aleph Alpha for ~$600M Schwarz Group anchor, Apr 24 [[44]](#ref-44); STC × Humain 1GW JV [[45]](#ref-45); S3NS SecNumCloud-qualified [[46]](#ref-46)). |
| **Observability** | Drift detection + cross-provider attestation = regulatory hook, not just SRE | **Datadog · W&B Weave · Langfuse · Phoenix** all ship eval-as-code on trace storage [[47]](#ref-47) · **OTel GenAI semantic conventions** [[48]](#ref-48) · gateway as observability cut-point [[49]](#ref-49) | aOS gateway = the joint **observability + policy + cost-attribution** cut-point. Drift on silent model updates is now a regulatory exposure. |
| **Cost control / FinOps** | 9–900× annual price compression + tokenizer-bloat (Opus 4.7 +35%) make cost a board-level item [[6]](#ref-6) | Per-tenant token attribution everywhere · gateway-level cost ceilings · **cascade routing** (cheap-model-first, frontier-fallback) becoming a platform feature | Token-attribution at agent-action level + cascade routing in the platform. Make customer cost **predictable** — that is now a buying criterion, not a feature. |

---

## 5. The signals worth ignoring — and the ones to watch

**Ignore:** model-leaderboard noise. New SOTA every other week. Public benchmarks saturate in weeks (MMLU >88%, GSM8K >99%) [[50]](#ref-50). Lab-vs-prod gap is **37%** [[51]](#ref-51) — leaderboard wins do not translate to enterprise wins.

**Watch:**
- **"Agent washing"** — Gartner conceded RPA and basic automation are being rebranded as "agentic" [[52]](#ref-52). The procurement counter is the Bain killer question: *"Can we prove business impact under controlled risk?"*
- **40% of agentic-AI projects will be cancelled by 2027** (Gartner) [[52]](#ref-52) — a board-level concern about pilot proliferation without business case.
- **Deloitte governance gap** — 75% plan agentic AI within 2 years; only 21% have mature governance [[53]](#ref-53). The buying criterion shifts from "does it work" to "can we prove business impact under controlled risk."
- **Geopolitical** — China reportedly blocked Meta acquisition of Manus (April 2026) [[54]](#ref-54); Cohere acquired Aleph Alpha (April 2026) [[44]](#ref-44). Cross-border agent platforms are becoming national-strategic assets. Sovereign deployment requirement is *now*, not future.

---

## Bottom line for the CEO

The strategy that wins is not "pick the right model." The strategy that wins is **"own the layers a hyperscaler cannot ship in your industry, behind a model-neutral abstraction."** For Amdocs, those layers are concrete and defensible:

- The **SID-aligned telco knowledge graph** (context).
- **Telco ground-truth + tiered evaluation** flywheel.
- **Telco-grade governance, audit, residency, HITL** packaged as platform primitives.
- **Model-neutral runtime** (CES26 / aOS) that lets a Tier-1 swap Claude / Gemini / open-weights without re-platforming.

These survive the model commoditization — but only if productised as governed, metered agent services *before* tier-1 customers build their own on hyperscaler infrastructure. Verizon, AT&T, BBVA, JPM are already doing exactly that. **Window: 4–6 quarters.**

---

## References

<a id="ref-1"></a>[1] Marktechpost on Moonshot Kimi K2.6 — 300 sub-agents / 4,000 steps / 5 days autonomous: <https://www.marktechpost.com/> (T2)
<a id="ref-2"></a>[2] BCG, "Agentic AI: $200B opportunity / threat to technology services": <https://www.bcg.com/publications> (T2)
<a id="ref-3"></a>[3] MIT Tech Review · Aurora Mobile / GPTBots on DeepSeek V4 Preview: <https://www.technologyreview.com/> (T1/T2)
<a id="ref-4"></a>[4] Epoch AI, "LLM inference price trends": <https://epoch.ai/data-insights/llm-inference-price-trends> (T1)
<a id="ref-5"></a>[5] Fortune · Anthropic on Claude Mythos / Project Glasswing: <https://fortune.com/> (T2)
<a id="ref-6"></a>[6] Anthropic · CloudZero on Opus 4.7 tokenizer change: <https://www.anthropic.com/> (T1)
<a id="ref-7"></a>[7] ACS, "Apple reveals the AI behind Siri's big 2026 upgrade": <https://ia.acs.org.au/article/2026/apple-reveals-the-ai-behind-siri-s-big-2026-upgrade.html> (T2)
<a id="ref-8"></a>[8] Bloomberg, "Anthropic memory feature" (Mar 3, 2026): <https://www.bloomberg.com/news/articles/2026-03-03/anthropic-tries-to-win-users-from-chatgpt-with-memory-feature> (T2)
<a id="ref-9"></a>[9] Salesforce Help, "Dynamic Revenue Orchestrator (Generally Available)": <https://help.salesforce.com/s/articleView?id=release-notes.rn_dro.htm> (T1)
<a id="ref-10"></a>[10] Salesforce Ben, "Salesforce Headless 360 and Agentforce Vibes 2.0 Revealed at TDX 2026": <https://www.salesforceben.com/salesforce-headless-360-and-agentforce-vibes-2-0-revealed-at-tdx-2026/> (T2)
<a id="ref-11"></a>[11] SiliconANGLE, "Salesforce Agentforce for Communications GA — One NZ, Lumen": <https://siliconangle.com/2026/03/02/analysis-salesforce-aims-help-telcos-grow-revenue-agentforce-communications/> (T2)
<a id="ref-12"></a>[12] CIO / Futurum Group, "ServiceNow Embeds AI Across Platform — model agnostic by design": <https://www.cio.com/article/4156549/servicenow-rolls-out-context-engine-to-embed-ai-governance-across-its-platform.html> (T2); BusinessWire on ServiceNow + Google Autonomous Network Operations (T2)
<a id="ref-13"></a>[13] Allianz · FStech on Project Nemo (Nov 2025) — 80% claim-cycle reduction: <https://www.allianz.com/> (T1)
<a id="ref-14"></a>[14] Computer Weekly, "JPMorgan Chase to replace core banking with Thought Machine": <https://www.computerweekly.com> (T2)
<a id="ref-15"></a>[15] Stripe, "Agentic Commerce Suite — URBN, Etsy, Coach, Kate Spade, Halara": <https://stripe.com/blog> (T1)
<a id="ref-16"></a>[16] American Banker · FIS on agentic-commerce platform with Visa + Mastercard "Know Your Agent" (Q1 2026): <https://www.americanbanker.com/> (T2)
<a id="ref-17"></a>[17] Coverage of Workday Illuminate Financial Close Agent · BlackLine · Trintech · FloQast · Ledge · Nominal "agentic close" category (industry coverage Q1 2026) (T2)
<a id="ref-18"></a>[18] EY, "EY Canvas multi-agent audit framework — 160,000 audit engagements": <https://www.ey.com/en_gl/news> (T2)
<a id="ref-19"></a>[19] Intel, "One AI internal agentic platform": <https://www.intel.com/content/www/us/en/newsroom> (T2)
<a id="ref-20"></a>[20] OpenAI press, "Deutsche Telekom multi-year — ~200,000 employees on ChatGPT Enterprise" (Dec 2025) (T1)
<a id="ref-21"></a>[21] Microsoft DevBlogs · Visual Studio Magazine, "Microsoft Agent Framework v1.0 GA" (Apr 23, 2026): <https://devblogs.microsoft.com/> (T1)
<a id="ref-22"></a>[22] Google Cloud blog · SiliconANGLE, "Gemini Enterprise Agent Platform — Vertex AI + Agentspace merger" (Apr 22, 2026): <https://cloud.google.com/blog> (T1)
<a id="ref-23"></a>[23] SiliconANGLE on Anthropic Claude Managed Agents (Apr 8, 2026): <https://siliconangle.com/> (T2)
<a id="ref-24"></a>[24] Access Newswire, "AWS × Amdocs multi-year strategic collaboration — MWC26 — AgentCore" (T2)
<a id="ref-25"></a>[25] Bloomberg · CNBC coverage of Anthropic-Pentagon spat / Trump CNBC interview Apr 21, 2026 (T2) — see deep-dive-week-apr21-27.md for full sourcing
<a id="ref-26"></a>[26] Linux Foundation, "AAIF formation press release" (Dec 9, 2025): <https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation> (T1)
<a id="ref-27"></a>[27] Linux Foundation, "A2A protocol surpasses 150 organizations" (Apr 2026): <https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year> (T1)
<a id="ref-28"></a>[28] Ravoid blog · Hacker News on the "LangChain Exit" trend (Q1 2026) (T3)
<a id="ref-29"></a>[29] Industry RFP language coverage — model substitution clauses (deep-dive-week-apr21-27.md §B) (T2)
<a id="ref-30"></a>[30] AT&T · Microsoft, "Ask AT&T / Workflows on Azure AI Foundry — 100k users / ~27B tokens/day / 71 GenAI solutions live" (T1)
<a id="ref-31"></a>[31] Google Cloud · Reuters on Verizon — 28k care reps + retail on Vertex/Gemini, ~40% sales lift since Jan 2025 (T1/T2)
<a id="ref-32"></a>[32] Microsoft Learn, "Fabric IQ Ontology overview": <https://learn.microsoft.com/en-us/fabric/fundamentals/ontology-overview> (T1)
<a id="ref-33"></a>[33] Palantir, "Connecting AI to Decisions with the Palantir Ontology": <https://blog.palantir.com/> (T1)
<a id="ref-34"></a>[34] TM Forum, "AI-Native Blueprint" / Agentic Interactions Security: <https://www.tmforum.org/> (T1)
<a id="ref-35"></a>[35] Microsoft Research, "Project GraphRAG": <https://www.microsoft.com/en-us/research/project/graphrag/> (T1)
<a id="ref-36"></a>[36] AWS, "Bedrock AgentCore Policy + Evaluations Preview" (Apr 2026): <https://aws.amazon.com/blogs/machine-learning/> (T1)
<a id="ref-37"></a>[37] Datadog press release, "AI Agent Monitoring, LLM Experiments" (Jun 2025): <https://www.datadoghq.com/about/latest-news/press-releases/datadog-expands-llm-observability-with-new-capabilities-to-monitor-agentic-ai-accelerate-development-and-improve-model-performance/> (T1)
<a id="ref-38"></a>[38] Bain & Company, "AI Survey 2025–2026 — 80% / 23% measurable-impact gap": <https://www.bain.com/insights/> (T2)
<a id="ref-39"></a>[39] AWS, "Bedrock AgentCore GA — modular Runtime/Memory/Gateway/Identity/Observability" (Oct 2025): <https://aws.amazon.com/> (T1)
<a id="ref-40"></a>[40] LangChain blog, "LangChain 1.0 / LangGraph 1.0 GA" (Oct 2025): <https://blog.langchain.dev/> (T1)
<a id="ref-41"></a>[41] Microsoft Open Source · Help Net Security, "Microsoft Agent Governance Toolkit open-sourced" (Apr 2, 2026): <https://opensource.microsoft.com/> (T1)
<a id="ref-42"></a>[42] Network World · Anthropic, "Cisco joins Project Glasswing — $100M credits; AWS/Apple/Google/MS/Nvidia/Palo Alto/JPM/CrowdStrike on board" (Apr 7, 2026) (T1/T2)
<a id="ref-43"></a>[43] ISO/IEC 42001:2023 catalogue: <https://www.iso.org/standard/42001> (T1)
<a id="ref-44"></a>[44] CNBC · TechCrunch, "Cohere to acquire Aleph Alpha — ~$600M Schwarz Group anchor" (Apr 24, 2026) (T2)
<a id="ref-45"></a>[45] Data Center Dynamics, "stc × Humain JV — 1GW Saudi data-centre capacity" (Dec 2025) (T2)
<a id="ref-46"></a>[46] Futurum, "S3NS sovereignty: SecNumCloud qualification" (Dec 17, 2025): <https://futurumgroup.com/insights/s3ns-sovereignty-can-thales-google-venture-make-ai-sovereignty-work-at-scale/> (T2)
<a id="ref-47"></a>[47] Datadog · W&B Weave · Langfuse · Phoenix product pages and release notes (T1)
<a id="ref-48"></a>[48] OpenTelemetry, "GenAI Semantic Conventions": <https://opentelemetry.io/docs/specs/semconv/gen-ai/> (T1)
<a id="ref-49"></a>[49] InfoQ, "AAIF MCP Dev Summit (April 2026) — gateway as observability cut-point consensus": <https://www.infoq.com/news/2026/04/aaif-mcp-summit/> (T2)
<a id="ref-50"></a>[50] TimeToAct, "LLM Benchmarks January 2025": <https://www.timetoact-group.at/en/insights/llm-benchmarks/llm-benchmarks-january-2025> (T3)
<a id="ref-51"></a>[51] Industry coverage of lab-vs-prod 37% performance gap (executive-brief.md §3) (T1)
<a id="ref-52"></a>[52] Gartner · xpander.ai on "agent washing" + 40%-cancellation by 2027: <https://www.gartner.com/en/research> (T2)
<a id="ref-53"></a>[53] Deloitte, "State of AI 2026 — 75% plan / 21% mature governance": <https://www2.deloitte.com/> (T2)
<a id="ref-54"></a>[54] Reuters · Bloomberg · FT coverage of China-Manus / Meta acquisition block (April 2026) — verify primary disclosure before quoting (T2)
