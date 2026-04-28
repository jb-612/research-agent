---
title: "AI Market Update — Q1 / April 2026"
subtitle: "News digest: model releases, vendor moves, deals, telco-customer announcements"
version: 1.0
created: 2026-04-27
audience: CEO, Amdocs
format: dated news bullets with sources — no strategy framing
window: January–April 2026 (heaviest weight on March–April 2026)
---

# AI Market Update — Q1 / April 2026

> Dated news only. Models, capabilities, deals, vendor releases, telco-customer moves, standards. Strategy and architectural analysis are in the companion `full-research.md` and `executive-brief.md`.

---

## Top stories — last 30 days

- **2026-04-27** — Microsoft and OpenAI **amend their partnership**: OpenAI now free to multi-cloud (incl. AWS/Google); Microsoft loses revenue share; revenue caps run through 2030 [1][2].
- **2026-04-24** — **Cohere acquires Aleph Alpha**, ~$20B combined valuation; Schwarz Group anchors with €500M; backed by both Canadian and German governments [3][4].
- **2026-04-23** — **OpenAI GPT-5.5 / GPT-5.5 Pro** rolls out across ChatGPT Plus/Pro/Business/Enterprise [5][6].
- **2026-04-23** — **Anthropic Claude Managed Agents persistent memory** in public beta (Netflix, Rakuten, Wisedocs early adopters) [7].
- **2026-04-22** — **Microsoft Copilot Agent Mode goes GA in Word, Excel, PowerPoint** [8].
- **2026-04-22** — **AWS Bedrock AgentCore** ships Managed Harness preview, AgentCore CLI, AgentCore Skills [9].
- **2026-04-16** — **Anthropic Claude Opus 4.7 GA**; Anthropic publicly concedes 4.7 still trails unreleased Mythos [10].
- **2026-04-15** — **Salesforce TDX 2026**: Headless 360, Agent Fabric, Agentforce Vibes 2.0, AgentExchange [11][12].
- **2026-04-09** — **A2A Protocol v1.0 GA** under Linux Foundation AAIF; 150+ orgs in production; signed Agent Cards [13].
- **2026-04-08** — **Anthropic Claude Mythos Preview** released to limited cybersecurity / coding partners; available on Vertex AI and Bedrock [14].
- **2026-04-02/03** — **AAIF MCP Dev Summit NA 2026** (NYC); ~1,200 attendees; Anthropic + AWS + Microsoft + OpenAI maintainers laid out enterprise security roadmap [15].

---

## 1. Models & capabilities

### Frontier models
- **2026-04-23** — **OpenAI GPT-5.5 + GPT-5.5 Pro** rolled out to all ChatGPT tiers, API live April 24 [5][6].
- **2026-04-16** — **Anthropic Claude Opus 4.7 GA** at same price as 4.6; better SWE / instruction-following / self-checking [10].
- **2026-04-08** — **Anthropic Claude Mythos Preview** available on Bedrock and Vertex AI for selected partners [14].
- **2026-03-11** — OpenAI deprecates GPT-5.1 line; **GPT-5.2 Instant** becomes the everyday default in ChatGPT [16].
- **Early 2026** — **Google Gemini 3** family (3 Flash default; 3 Pro and 3.1 Pro flagship); Project Mariner Computer Use available in Gemini 3 Pro / Flash [17][18].

### Chinese frontier
- **2026-04-24** — **DeepSeek V4** (V4-Pro and V4-Flash preview): 1.6T MoE / 49B active params, native 1M-token context [19][20].
- **2026-04-20** — **Moonshot Kimi K2.6**: first open-weight model to beat GPT-5.4 (xhigh) on SWE-Bench Pro (58.6 vs 57.7); 1T MoE / 32B active, 256K context, weights on HuggingFace [21].
- **2026-04-07** — **Z.ai GLM-5.1 open weights** (MIT licence); first open-source model to top SWE-Bench Pro at 58.4 [21].
- **2026-03-30** — **Alibaba Qwen 3.6 Plus** free on OpenRouter, 1M-token context, no API key [22].

### World models / specialised
- **2026-01-29** — **Google rolls out Project Genie**: Genie 3 + Nano Banana Pro powering AI Ultra prototype that generates playable worlds [23].
- **2026 (Apr)** — **Meta Superintelligence Labs releases Muse Spark** as a Llama replacement [24].

### Apple / on-device
- **2026-03-25** — **Apple iOS 26.4** with Apple Intelligence Playlist Playground in Music; **2026-04-22** iOS 26.4.2 follow-up [25][26].
- **Note**: major Siri / next-gen AFM upgrade was **deferred to iOS 27** — a notable miss against Q1 2026 expectations.

### Open / enterprise
- **Q1 2026 ongoing** — **IBM Granite 4.0** (Mamba-Transformer hybrid) in enterprise rollout; **AI21 Jamba Reasoning 3B** released; **AI21 Jamba Large 1.7** across enterprise platforms [27].

---

## 2. Vendor releases & moves

### Salesforce — and competitor reactions

**Salesforce moves**
- **2026-04-15 (TDX 2026)** — **Headless 360** (60+ new MCP tools, 30 preconfigured coding skills usable from Claude Code / Cursor / Codex / Windsurf), **Agent Fabric** (cross-platform agent governance control plane), **Agentforce Vibes 2.0** IDE (Sonnet + GPT-5; ~40% cycle-time cut claim), **Agentforce Experience Layer (AXL)**, **open-sourced Agent Script**, **AgentExchange** (10,000 Salesforce apps + 2,600 Slack apps + 1,000+ agents/MCP servers) [11][12].
- **2026-03-02** — **Agentforce for Communications** (pre-MWC launch): five prebuilt telco-tuned agents pulling live CRM/OSS/BSS data; **Lumen Technologies** as flagship reference (saved $5.6M year one, 300+ hours/week productivity) [28][29].
- **2026 Q1** — **Salesforce enters ITSM with Agentforce IT Service** — direct attack on ServiceNow's core market [30].
- **2025-10-13 (Dreamforce)** — **Agentforce 360**: GPT-5 inside Salesforce + Anthropic models embedded; Reddit (-46% cases, -84% resolution time) and PepsiCo (25–30% efficiency) cited [31].

**Competitor reactions**
- **2026 Q1** — **ServiceNow AI Control Tower**: ~$9B in acquisitions in six weeks; Zurich release positions back-office as automated-execution engine [32][33].
- **2026 Q1** — **SAP Joule platform play**: Joule Studio GA + 40+ pre-built agents + 2,400+ Joule Skills across 35 SAP solutions; framed as strategic challenge to Salesforce, Oracle, ServiceNow [34][35].
- **2026** — **Microsoft positions Copilot Studio + Dynamics 365 agents** as Agentforce alternative across the 2026 release wave [36].

### Microsoft
- **2026-04-22** — **Copilot Agent Mode GA in Word, Excel, PowerPoint** [8].
- **2026-03-30** — Copilot Cowork available to **Frontier** customers; full **E7 Cowork** suite GA targeted **2026-05-01** at $99/user/month [37].
- **2026-03-09** — **Microsoft Copilot Cowork** announced [38].
- **2026** — **Microsoft Agent Framework v1.0** released (AutoGen + Semantic Kernel merger); **AutoGen moves to maintenance mode** [39].

### Anthropic
- **2026-04-23** — **Claude Managed Agents persistent memory** public beta; Netflix, Rakuten, Wisedocs, Ando reporting -97% first-pass errors and +30% speed in document workflows [7][40].
- **2026-03-02** — **Memory + import tool free for all Claude users**: previously paid-only; import pulls from ChatGPT/Gemini/Perplexity/Grok [41][42].

### OpenAI
- **2026** — **ChatGPT Atlas Agent Mode** in preview for Plus/Pro/Business; **AgentKit** released (Agent Builder beta, ChatKit GA, Connector Registry beta) [43][44].

### AWS
- **2026-04-22** — **Bedrock AgentCore**: Managed Harness preview, AgentCore CLI, AgentCore Skills (filesystem persistence, microVM per session, IaC deploy) [9].
- **2026-04-13** — **Claude Mythos Preview lands on Bedrock**; **AWS Agent Registry** announced [45].
- **2026-04-27** — **AWS + Anthropic + Meta partnership** announced, plus AgentCore CLI updates [46].

### Google Cloud
- **2026 (Cloud Next 26)** — **Gemini Enterprise Agent Platform** (evolution of Vertex AI), C4N/M4N network-optimised compute, AlloyDB Vector upgrades, Workspace Studio, **$750M Partner Fund** for agentic AI startups [47][48].

### NVIDIA
- **2026 (GTC, March)** — **Nemotron Coalition** with leading global AI labs; **NemoClaw** stack for OpenClaw agents on DGX Station/Spark; Open Agent Development Platform [49][50][51].

### Standards & protocols
- **2026-04-09** — **A2A Protocol v1.0 GA** under Linux Foundation AAIF: multi-protocol bindings, multi-tenancy, signed Agent Cards; 150+ orgs in production [13][52].
- **2026-04-02/03** — **AAIF MCP Dev Summit NA 2026 (NYC)**: ~1,200 attendees; MCP Apps spec (released 2026-01-26) featured; enterprise security roadmap announced [15][53].

---

## 3. Deals & M&A (Jan–Apr 2026)

- **2026-04-27** — **Microsoft–OpenAI partnership amended**: OpenAI free to multi-cloud (AWS / Google); Microsoft loses revenue share; revenue caps through 2030 [1][2].
- **2026-04-24** — **Cohere acquires Aleph Alpha** — ~$20B combined valuation; Schwarz Group €500M anchor + lead Cohere Series E; pending German/EU approval [3][4].
- **2026-04** — **xAI agreement to acquire Anysphere/Cursor**: right to acquire for $60B later in 2026 (or $10B work-payment) [54].
- **2026-02** — **Anthropic Series G $30B** led by GIC and Coatue at ~$380B post-money [55].
- **2026-02** — **OpenAI extends megaround**: added $10B on top of February's $110B; total ~$120B [55].
- **2026-02** — **KKR + Singtel acquire remaining 82% of STT GDC** for SGD 6.6B (~US$5.1B); Singtel keeps 25% [56].
- **2026-01-02** — **Qvantel completes Optiva acquisition**: combined entity serves 70+ telcos in 40+ countries with 1,000+ staff; **four new operator wins in first 3 months** [57][58].
- **2025-10-29** — **NEC to acquire CSG Systems for ~$2.9B** at $80.70/share; closing targeted 2026; positions Netcracker + CSG to compete head-on with Amdocs [59][60].
- **2026** — **AT&T closes ~$5.75B acquisition of Lumen's fibre-to-the-home business** [61].

---

## 4. Telco industry — customer AI announcements

### MWC Barcelona 2026 (late Feb / early Mar 2026) — biggest single source

- **2026 (MWC) — Verizon agentic + GenAI overhaul**: dual goal of CX and employee empowerment via autonomous agents; joint Amdocs session on AI-driven network slicing [62][63].
- **2026 (MWC) — AT&T launches first standardised 5G network APIs in the US** via Aduna (Ericsson/Google JV); also rolls out new consumer app to consolidate services [64][65].
- **2026 (MWC) — T-Mobile**: AI-based self-optimised network (SON) tools handled Winter Storm Fern; T-Life app connected to ~34M households/businesses [66].
- **2026 (MWC) — Pan-European Federated Edge Continuum**: **DT + Orange + Telefónica + TIM + Vodafone** demo first live federated edge across the five largest European operators [67][68].
- **2026 (MWC) — MTN + Huawei "Agentic Networks"**: closed-loop optimisation, autonomous operations across MTN footprint via Huawei-MTN Tech Innovation Lab in South Africa [69].
- **2026 (MWC) — Vodafone + Amazon Project Kuiper**: LEO-satellite augmentation of Vodafone 4G/5G coverage in Europe and Africa; Germany first [70].
- **2026 (MWC) — GSMA Open Telco AI launched** (with AT&T, AMD): industry-wide telecom-AI benchmarking + AI Telco Troubleshooting Challenge [71].
- **2026 (MWC) — Ericsson + Intel pact for AI-native 6G** [70].
- **2026 (MWC) — Ericsson + Mistral AI partnership** on AI-native 6G-era networks [72].
- **2026 (MWC) — Qualcomm 6G Coalition with KDDI, NTT DOCOMO, Reliance Jio**: roadmap toward 2029 commercial 6G; NTT DOCOMO doubled 6G throughput in AI-enabled trial [73].
- **2026 (MWC) — Lumen Technologies on Salesforce Agentforce**: $5.6M saved year one, 300+ hours/week productivity [29].
- **2026 (MWC) — SoftBank "Natural AI Phone"** (with Brain Technologies): intent-based mobile OS [74].
- **2026 (MWC) — Telefónica positions as European federated sovereign cloud / AI / edge provider** [68].
- **2026 (MWC) — Google Cloud telco lineup**: working with **Mas Orange, Vodafone, DT, DigitalRoute, One New Zealand**, and Nokia network-as-code platform; AI agents to build/maintain network digital twins [75].

### Specific operator announcements (post-MWC)

- **2026-04-23/24 — BT + Nscale + NVIDIA UK sovereign AI partnership**: up to 14 MW AI data-centre capacity across three BT sites; **BT also launches "UK's first complete sovereign telecom services"** [76][77].
- **2026-02 — Reliance Jio commits $110B to AI infrastructure**: 120+ MW AI-ready datacentres at Jamnagar in H2 2026; **LLM-agnostic AI telecom network platform with agentic AI**; partnerships with Google, Meta, others [78][79].
- **2026 (Cloud Next) — Vodafone deploys Gemini Enterprise agents** for proactive outage resolution and infrastructure optimisation [47].
- **2026 (early) — Bell Canada rolls out Cohere "North"**: agentic AI platform across the management team; **100+ identified use cases**; data-sovereignty focus; Bell targets **$2B annual AI revenue by 2028** [80].
- **2026 — Optus deploys YesGPT internally on Anthropic Claude**; **Optus + Perplexity partnership**: 12-month free Perplexity Pro to 6M consumer + SMB mobile customers [81].
- **2026 — Rogers deploys Agent Assist AI** for call-centre note generation and troubleshooting [82].
- **2026 — Telstra International** advancing zero-touch / autonomous network plans for AI-era demand [83].
- **2026 — Vodafone–Microsoft 10-year, $1.5B cloud + customer AI deal** continues to anchor Vodafone's AI overhaul [84].

---

## 5. Telco vendor moves (BSS/OSS/network-AI)

- **2026-02-27 — Amdocs unveils CES26**: agent-driven BSS-OSS-Network suite, powered by aOS Cognitive Core, MWC 2026 [85].
- **2026-02-27 — Amdocs + Microsoft MWC 2026 packaging**: aOS Cognitive Core Agentic Services stitched into **Microsoft Foundry, Azure OpenAI, GitHub Copilot, Fabric IQ, Microsoft Migration Agents** — branded "Service-as-Software" [86].
- **2026-02-17/20 — Ericsson + AWS launch Agentic rApp as a Service on AWS Marketplace**: built on **Telco Agentic AI Studio**; uses SageMaker, Bedrock AgentCore, Kiro, Amazon Q [87][88].
- **2026-04-14/21 — Netcracker showcases autonomous-operations cases at FutureNet World 2026** with embedded AI agents and CSP success stories [89].
- **2026-03-02 — Salesforce Agentforce for Communications** (pre-MWC) — five prebuilt telco-tuned agents; Lumen reference customer [28][29].
- **2026 (MWC) — Telefónica/DT/Orange/TIM/Vodafone Federated Edge Continuum** running portable apps across the five-operator footprint [67][68].

---

## 6. Standards & regulation (most cited in the last quarter)

- **2026-04-09 — A2A Protocol v1.0 GA** (Linux Foundation AAIF), 150+ orgs in production [13].
- **2026-04-02/03 — AAIF MCP Dev Summit NA**: SEP-1442 (stateless transport), MCP Apps spec (Jan 26, 2026), SEP-1686 (durable tasks primitive) [15][53].
- **2026-03 (early March) — European Parliament fast-tracks Digital Omnibus on AI** (569–45–23): conditional postponement of high-risk obligations to **2 December 2027 (Annex III)** and **2 August 2028 (Annex I)**; ban on "nudifier" apps added [90].
- **2025-12-09 — Linux Foundation AAIF formed**: founding contributors Anthropic + OpenAI + Block; platinum members AWS, Bloomberg, Cloudflare, Google, Microsoft; hosts MCP, goose, AGENTS.md [91][92].
- **2025-12-17 — S3NS achieves SecNumCloud 3.2 qualification** (first hyperscaler-derived stack) [93].
- **2025-11-18 — DORA CTPP regime activated**: first ESA-designated Critical Third-Party Providers triggers direct EU oversight of cloud/AI infrastructure to financial entities [94].
- **2026-02-20 — India IT Rules Amendment** in force: world's first explicit deepfake regulatory framework with mandatory synthetic-content identification [95].
- **2026 (Jan) — Korea AI Basic Act in force** (passed Dec 2024): risk-based architecture, generative-AI labelling mandatory [96].

---

## Watchlist (next 60–90 days)

- **2026-05-01** — Microsoft full **E7 Cowork** suite GA at $99/user/month [37].
- **2026-Q2** — DeepSeek V4 wider rollout; Kimi K2.6 vs GPT-5.5 SWE-Bench Pro saturation race.
- **2026-Q2** — EU AI Act 2 August 2026 deadline: GPAI Commission enforcement powers + Article 50 transparency obligations activate (deepfake labelling, AI-interaction disclosure).
- **2026-H2** — NEC–CSG closing; Cohere–Aleph Alpha regulatory clearance.
- **2026-H2** — xAI–Anysphere/Cursor potential exercise of acquisition right.
- **2026-Q3** — TM Forum DTW (telco; AAIF Agentic Interactions Security work).
- **Apple iOS 27** — Siri / next-gen Apple Foundation Model upgrade (deferred from iOS 26.4).
- **Gemini 3.x cadence** — Google rolling release cadence on Gemini 3 sub-versions.

---

## Key sources

[1] Microsoft Blogs, "The next phase of the Microsoft–OpenAI partnership" (2026-04-27) — https://blogs.microsoft.com/blog/2026/04/27/the-next-phase-of-the-microsoft-openai-partnership/
[2] CNBC, "OpenAI–Microsoft partnership amended; revenue cap through 2030" (2026-04-27) — https://www.cnbc.com/2026/04/27/openai-microsoft-partnership-revenue-cap.html
[3] TechCrunch, "Cohere acquires/merges with Aleph Alpha — transatlantic AI powerhouse" (2026-04-24) — https://techcrunch.com/2026/04/24/cohere-acquires-merges-with-german-based-startup-to-create-a-transatlantic-ai-powerhouse/
[4] Axios, "Cohere $20B Aleph Alpha Europe" (2026-04-24) — https://www.axios.com/2026/04/24/cohere-20-billion-aleph-alpha-europe
[5] OpenAI, "Introducing GPT-5.5" — https://openai.com/index/introducing-gpt-5-5/
[6] TechCrunch, "OpenAI ChatGPT GPT-5.5 superapp" (2026-04-23) — https://techcrunch.com/2026/04/23/openai-chatgpt-gpt-5-5-ai-model-superapp/
[7] Help Net Security, "Claude Managed Agents bring execution and control" (2026-04-09) — https://www.helpnetsecurity.com/2026/04/09/claude-managed-agents-bring-execution-and-control-to-ai-agent-workflows/
[8] Windows News, "Microsoft Copilot Agent Mode GA in Word, Excel, PowerPoint" (2026-04-22) — https://windowsnews.ai/article/microsoft-copilot-agent-mode-now-ga-in-word-excel-and-powerpoint.415032
[9] AWS What's New, "AgentCore: new features to build agents faster" (2026-04-22) — https://aws.amazon.com/about-aws/whats-new/2026/04/agentcore-new-features-to-build-agents-faster/
[10] Anthropic, "Claude Opus 4.7" — https://www.anthropic.com/news/claude-opus-4-7
[11] Salesforce DevOps, "TDX 2026 Reporter's Notebook — Headless 360" — https://salesforcedevops.net/index.php/2026/04/15/tdx-2026-reporters-notebook-salesforce-goes-headless-and-widens-the-builder-gap/
[12] Salesforce Ben, "Headless 360 and Agentforce Vibes 2.0 at TDX 2026" — https://www.salesforceben.com/salesforce-headless-360-and-agentforce-vibes-2-0-revealed-at-tdx-2026/
[13] A2A Protocol, "Announcing 1.0" — https://a2a-protocol.org/latest/announcing-1.0/
[14] Anthropic Red, "Mythos Preview" — https://red.anthropic.com/2026/mythos-preview/
[15] InfoQ, "AAIF MCP Dev Summit (April 2026)" — https://www.infoq.com/news/2026/04/aaif-mcp-summit/
[16] OpenAI, "GPT-5 system card update — GPT-5.2" — https://openai.com/index/gpt-5-system-card-update-gpt-5-2/
[17] Google Blog, "Gemini 3" — https://blog.google/products/gemini/gemini-3/
[18] Google Blog, "Gemini 3 in the Gemini app" — https://blog.google/products-and-platforms/products/gemini/gemini-3-gemini-app/
[19] Simon Willison, "DeepSeek V4" (2026-04-24) — https://simonwillison.net/2026/apr/24/deepseek-v4/
[20] BuildFastWithAI, "DeepSeek V4-Pro Review 2026" — https://www.buildfastwithai.com/blogs/deepseek-v4-pro-review-2026
[21] BenchLM, "Best Chinese LLM" / Akita on Rails benchmarks part 3 — https://benchlm.ai/blog/posts/best-chinese-llm
[22] Renovate QR, "Chinese AI models April 2026" — https://renovateqr.com/blog/chinese-ai-models-april-2026
[23] 9to5Google, "Google Project Genie" (2026-01-29) — https://9to5google.com/2026/01/29/google-project-genie/
[24] LLM Stats — https://llm-stats.com/llm-updates
[25] 9to5Mac, "iOS 26.4 new iPhone features" — https://9to5mac.com/2026/03/25/ios-26-4-new-iphone-features/
[26] MacRumors, "iOS 26.4.2 coming soon" — https://www.macrumors.com/2026/04/20/ios-26-4-2-coming-soon/
[27] AI21 / X update on Jamba Reasoning 3B — https://x.com/AI21Labs/status/1975206913403961431
[28] SiliconAngle, "Salesforce aims to help telcos grow revenue with Agentforce for Communications" (2026-03-02) — https://siliconangle.com/2026/03/02/analysis-salesforce-aims-help-telcos-grow-revenue-agentforce-communications/
[29] CXToday, "Salesforce Agentforce for Communications — telecom AI" — https://www.cxtoday.com/crm/salesforce-agentforce-for-communications-telecom-ai/
[30] Techzine, "Salesforce enters ITSM market — will ServiceNow regret its CRM move?" — https://www.techzine.eu/blogs/applications/135486/salesforce-enters-itsm-market-will-servicenow-regret-its-crm-move/
[31] Salesforce Press, "Agentic enterprise announcement" (2025-10-13) — https://www.salesforce.com/news/press-releases/2025/10/13/agentic-enterprise-announcement/
[32] Token Ring / Financial Content, "The great agent war: Salesforce and ServiceNow clash" — https://markets.financialcontent.com/wral/article/tokenring-2026-1-1-the-great-agent-war-salesforce-and-servicenow-clash-over-the-future-of-the-enterprise-ai-operating-system
[33] Forrester, "ServiceNow and Salesforce cross battle lines in escalating CRM war" — https://www.forrester.com/blogs/servicenow-and-salesforce-cross-battle-lines-in-an-escalating-crm-war/
[34] Cloud Wars, "SAP leaps into agent wars" — https://cloudwars.com/ai/sap-leaps-into-agent-wars-with-strategic-challenge-to-salesforce-oracle-servicenow/
[35] SAP News, "Business AI release highlights Q1 2026" — https://news.sap.com/2026/04/sap-business-ai-release-highlights-q1-2026/
[36] Digital Iceberg, "ServiceNow's AI-native bet" — https://www.thedigitaliceberg.com/post/servicenow-s-ai-native-bet-and-what-it-says-about-the-future-of-enterprise-saas
[37] Microsoft 365 Blog, "Copilot Cowork now available in Frontier" (2026-03-30) — https://www.microsoft.com/en-us/microsoft-365/blog/2026/03/30/copilot-cowork-now-available-in-frontier/
[38] Microsoft 365 Blog, "Copilot Cowork — a new way of getting work done" (2026-03-09) — https://www.microsoft.com/en-us/microsoft-365/blog/2026/03/09/copilot-cowork-a-new-way-of-getting-work-done/
[39] Microsoft Dev Blogs, "Microsoft Agent Framework v1.0" — https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-version-1-0/
[40] SD Times, "Anthropic adds memory to Claude Managed Agents" — https://sdtimes.com/anthropic/anthropic-adds-memory-to-claude-managed-agents/
[41] 9to5Mac, "Free Claude users can now use memory and import context from rivals" (2026-03-02) — https://9to5mac.com/2026/03/02/free-claude-users-can-now-use-memory-and-import-context-from-rivals/
[42] MacRumors, "Anthropic memory import tool" (2026-03-02) — https://www.macrumors.com/2026/03/02/anthropic-memory-import-tool/
[43] OpenAI Help, "ChatGPT Atlas release notes" — https://help.openai.com/en/articles/12591856-chatgpt-atlas-release-notes
[44] OpenAI, "Introducing AgentKit" — https://openai.com/index/introducing-agentkit/
[45] AWS Blogs, "AWS weekly roundup — Claude Mythos Preview in Bedrock, AWS Agent Registry" (2026-04-13) — https://aws.amazon.com/blogs/aws/aws-weekly-roundup-claude-mythos-preview-in-amazon-bedrock-aws-agent-registry-and-more-april-13-2026/
[46] AWS Blogs, "AWS weekly roundup — Anthropic-Meta partnership, AgentCore CLI" (2026-04-27) — https://aws.amazon.com/blogs/aws/aws-weekly-roundup-anthropic-meta-partnership-aws-lambda-s3-files-amazon-bedrock-agentcore-cli-and-more-april-27-2026/
[47] The Next Web, "Google Cloud Next — AI agents agentic era" — https://thenextweb.com/news/google-cloud-next-ai-agents-agentic-era
[48] Bitcoin World, "Google Cloud Next 2026 AI startups" — https://bitcoinworld.co.in/google-cloud-next-2026-ai-startups/
[49] NVIDIA Newsroom, "Nemotron Coalition" — https://nvidianews.nvidia.com/news/nvidia-launches-nemotron-coalition-of-leading-global-ai-labs-to-advance-open-frontier-models
[50] NVIDIA Newsroom, "NemoClaw" — https://nvidianews.nvidia.com/news/nvidia-announces-nemoclaw
[51] NVIDIA Newsroom, "AI Agents" — https://nvidianews.nvidia.com/news/ai-agents
[52] Google Open Source Blog, "A year of open collaboration — A2A anniversary" — https://opensource.googleblog.com/2026/04/a-year-of-open-collaboration-celebrating-the-anniversary-of-a2a.html
[53] AAIF Blog, "MCP is now enterprise infrastructure — MCP Dev Summit NA 2026" — https://aaif.io/blog/mcp-is-now-enterprise-infrastructure-everything-that-happened-at-mcp-dev-summit-north-america-2026/
[54] Tech Insider, "Cursor $60B valuation Anysphere AI coding 2026" — https://tech-insider.org/cursor-60-billion-valuation-anysphere-ai-coding-2026/
[55] Crunchbase News, "Foundational AI startup funding doubled — OpenAI, Anthropic, xAI Q1 2026" — https://news.crunchbase.com/venture/foundational-ai-startup-funding-doubled-openai-anthropic-xai-q1-2026/
[56] Minichart, "Singtel stock analysis 2025 — STT GDC" — https://www.minichart.com.sg/2025/09/03/singtel-stock-analysis-2025-growth-drivers-ai-data-centers-dividend-outlook-maybank-research/
[57] Qvantel, "Qvantel completes acquisition of Optiva" — https://blog.qvantel.com/blog/qvantel-completes-acquisition-of-optiva-to-drive-the-next-evolution-of-ai-powered-bss
[58] Investing.com, "Qvantel completes Optiva acquisition; four operator wins" — https://www.investing.com/news/company-news/qvantel-completes-acquisition-of-optiva-secures-four-new-operator-wins-93CH-4426667
[59] NEC, "NEC to acquire CSG Systems" (2025-10-29) — https://www.nec.com/en/press/202510/global_20251029_03.html
[60] TeckNexus, "NEC buys CSG for $2.9B" — https://tecknexus.com/nec-buys-csg-for-2-9b-to-scale-saas-bss-monetization/
[61] Fierce Network, "AT&T's $5.75B Lumen deal now complete" — https://www.fierce-network.com/broadband/atts-575b-lumen-deal-now-complete
[62] Fierce Network, "Verizon builds trust in AI through data and governance" — https://www.fierce-network.com/sponsored/verizon-builds-trust-ai-through-data-and-governance
[63] Telecoms.com, "MWC 2026 — Verizon and Amdocs on AI to network slicing" — https://www.telecoms.com/automation/mwc-2026-verizon-and-amdocs-on-applying-ai-to-network-slicing-and-the-connected-ecosystem
[64] SDxCentral, "AT&T launches first 5G network APIs for US market" — https://www.sdxcentral.com/news/att-launches-first-5g-network-apis-for-us-market/
[65] Fierce Network, "AT&T debuts new app — T-Life" — https://www.fierce-network.com/wireless/att-debuts-new-app-it-annoying-t-life
[66] Fierce Network, "MWC 2026 — T-Mobile, 6G, 5G Advanced, AI to combat storms" — https://www.fierce-network.com/broadband/mwc-2026-t-mobile-6g-5g-advanced-and-using-ai-combat-storms
[67] Vodafone, "Pan-European Federated Edge Continuum" — https://www.vodafone.com/news/newsroom/technology/pan-european-federated-edge-continuum
[68] Telefónica, "Federated Edge Continuum at MWC 2026" — https://www.telefonica.com/en/communication-room/press-room/deutsche-telekom-orange-telefonica-tim-vodafone-logran-el-primer-edge-continuum-federado-paneuropeo-en-el-mwc-2026/
[69] MTN Group, "MTN Group at MWC Barcelona 2026" — https://www.mtn.com/mtn-group-at-mwc-barcelona-2026/
[70] Fierce Network, "MWC 2026 — 5 takeaways for the telecom industry" — https://www.fierce-network.com/wireless/mwc-2026-5-takeaways-telecom-industry
[71] Nucleo Visual, "GSMA launches Open Telco AI" — https://nucleovisual.com/en/GSMA-launches-Open-Telco-AI-to-boost-AI-telco-grade-from-Barcelona/
[72] Telecom Infra, "Ericsson teams up with Mistral AI — 6G era" (2026-02-22) — https://telecominfra.wordpress.com/2026/02/22/ericsson-teams-up-with-mistral-ai-building-smarter-ai-native-telecom-networks-for-the-6g-era/
[73] StockTitan, "Qualcomm 6G coalition with KDDI, NTT DOCOMO, Reliance Jio" — https://www.stocktitan.net/news/QCOM/qualcomm-and-other-industry-leaders-commit-to-6g-trajectory-towards-gyxbviv8z9zj.html
[74] Intralink, "MWC 2026 fast-growing tech trends" — https://www.intralinkgroup.com/latest/intralink-insights/mwc-2026-fast-growing-tech-trends-and-lunch-prices
[75] Fierce Network, "MWC Google Cloud targets telco networks operations and more AI" — https://www.fierce-network.com/cloud/mwc-google-cloud-targets-telco-networks-operations-and-more-ai
[76] ResultSense, "BT + Nscale 14MW AI data centre UK" (2026-04-23) — https://www.resultsense.com/news/2026-04-23-bt-nscale-14mw-ai-data-centre-uk
[77] VoIP Review, "BT launches UK's first complete sovereign telecom services" (2026-04-24) — https://voip.review/2026/04/24/bt-launches-uks-first-complete-sovereign-telecom-services/
[78] The Register, "Jio AI plans India summit" (2026-02-20) — https://www.theregister.com/2026/02/20/jio_ai_plans_india_summit
[79] Telecoms Tech News, "India's Reliance to pump $110B into AI infrastructure" — https://www.telecomstechnews.com/content/ai/india-s-reliance-to-pump-110bn-into-ai-infrastructure-54899/
[80] Complete AI Training, "Bell Canada begins Cohere North rollout — data sovereignty focus" — https://completeaitraining.com/news/bell-canada-begins-cohere-north-rollout-focusing-on-data/
[81] Telecompaper, "Australian operators expand AI deployment across operations" — https://www.telecompaper.com/news/australian-operators-expand-ai-deployment-across-operations-acma--1568633
[82] CBC, "Rogers customer service issues — Agent Assist AI" — https://www.cbc.ca/news/gopublic/go-public-rogers-customer-service-issues-9.6977461
[83] Fierce Network, "Telstra International bets on autonomous networks" — https://www.fierce-network.com/broadband/telstra-international-bets-autonomous-networks-keep-pace-ai-era-demand
[84] TelecomTV, "Vodafone CTO on the sovereign opportunity for telcos" — https://www.telecomtv.com/content/ai/vodafone-s-cto-on-the-sovereign-opportunity-for-telcos-55350/
[85] AccessNewswire, "MWC 2026 — Amdocs unveils CES26 agent-driven BSS-OSS-Network suite" — https://www.accessnewswire.com/newsroom/en/computers-technology-and-internet/mwc-2026-amdocs-unveils-ces26-an-agent-driven-bss-oss-network-sui-1142366
[86] AccessNewswire, "MWC 2026 — Amdocs collaborates with Microsoft" — https://www.accessnewswire.com/newsroom/en/computers-technology-and-internet/mwc-2026-amdocs-collaborates-with-microsoft-to-bring-ai-accelerat-1142341
[87] Ericsson Press, "Ericsson launches Agentic rApp as a Service on AWS" (2026-02) — https://www.ericsson.com/en/press-releases/2026/2/ericsson-launches-agentic-rapp-as-a-service-on-aws-to-accelerate-autonomous-networks-transformation
[88] AWS Industries Blog, "Shaping the future of telco operations with agentic AI collaboration approach" — https://aws.amazon.com/blogs/industries/shaping-the-future-of-telco-operations-with-an-agentic-ai-collaboration-approach/
[89] Netcracker, "Netcracker showcases autonomous operations with agentic AI at FutureNet World 2026" — https://www.netcracker.com/news/press-releases/netcracker-showcases-autonomous-operations-with-agentic-ai-at-futurenet-world-2026
[90] European Parliament, "AI Act delayed application; ban on nudifier apps" (March 2026) — https://www.europarl.europa.eu/news/en/press-room/20260323IPR38829/artificial-intelligence-act-delayed-application-ban-on-nudifier-apps
[91] Linux Foundation, "AAIF formation press release" (2025-12-09) — https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation
[92] Anthropic, "Donating MCP and establishing the Agentic AI Foundation" — https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation
[93] Futurum, "S3NS sovereignty SecNumCloud qualification" — https://futurumgroup.com/insights/s3ns-sovereignty-can-thales-google-venture-make-ai-sovereignty-work-at-scale/
[94] EIOPA, "DORA hub" — https://www.eiopa.europa.eu/digital-operational-resilience-act-dora_en
[95] India MeitY IT Rules Amendment, references in OWASP / regulatory tracking pages
[96] MSIT (Korea), "AI Basic Act" — https://www.msit.go.kr/eng/bbs/view.do?sCode=eng&mId=4&mPid=2&pageIndex=&bbsSeqNo=42&nttSeqNo=1071

---

*All sources accessed 2026-04-27. For prior reporting and strategic context: see `./full-research.md` and `./executive-brief.md` in this folder.*
