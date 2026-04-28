---
title: "AI/GenAI — CEO Executive Brief"
subtitle: "Eleven trends, one architecture, six postures (2025–2026)"
version: 1.1
created: 2026-04-27
updated: 2026-04-27
audience: CEO, Amdocs
companion: ./full-research.md (≈14,500 words, 160 cited sources)
revision_notes: "v1.1 — added services-disruption thesis (§12), consulting consensus quotables, Chinese-challenger geopolitics (§13), and buyer evaluation framework."
---

# AI/GenAI — CEO Executive Brief

> **One sentence**: The "AI hype" is eleven distinct technology trends on different clocks; the job is to bet on the layers that compound advantage (knowledge graphs, evaluations, compliance instrumentation, policy gateways) and abstract away the layers that commoditise (models, frameworks, UX shells).

---

## 1. The model layer is no longer one architecture

- Decoder-only transformers now share the production frontier with **MoE sparse-activation** (DeepSeek-V3 at 5.5% active params, $5.6M training cost vs GPT-4's $50–100M [a]), **state-space hybrids** (AI21 Jamba, IBM Granite 4.0, Mistral Codestral Mamba [b][c]), **diffusion language models** (Inception Mercury at 1109 tok/s — 10× faster than autoregressive [d]), **latent-prediction encoders** (Meta V-JEPA 2 [e], LeJEPA [f]), and **full world models** (DeepMind Genie 3 [g], NVIDIA Cosmos [h], Wayve GAIA-3).
- **Architectural pluralism is real for the first time since 2017.** Stop assuming "the model" is decoder-only transformer with KV-cache.
- **So what**: budget by *active parameters per token*, not total params; expose a model-agnostic capability contract; treat the model as a substitutable cache, not a system component.

## 2. Inference economics inverted

- **280-fold reduction** in inference cost for GPT-3.5-MMLU-equivalent quality between Nov 2022 and Oct 2024 (Stanford AI Index 2025 [i]). Per-task price falls **9× to 900× per year by capability tier** [j].
- **2.8 model releases per business day** in Q1 2026 [k]. Anthropic shipped 8 frontier-tier Claude versions in roughly two years [l]. **Model integrations have a ~1-year shelf life.**
- Frontier *reasoning* tier is the exception — relatively stable, suggesting bifurcation: commodity tier asymptotes toward marginal-energy cost, frontier tier holds premium.
- **So what**: any 2025-vintage cost model is 10× wrong by 2027. Architect for swap; pay the abstraction tax willingly.

## 3. Evaluation is broken — and that is the strategic opening

- LLM-as-judge has documented **position bias** [m], **self-preference / family bias** [n], **rating-roulette self-inconsistency** [o], multilingual unreliability, and adversarial vulnerability. **MAST (NeurIPS 2025)** is the first empirical multi-agent failure taxonomy — 14 modes in 3 buckets [p].
- Public benchmarks turn over in weeks. **MMLU saturated >88%, GSM8K >99%** [q]. **Eval suites have a useful life of 12–24 months and are infrastructure, not artefacts.**
- The vendor that pairs **domain ground truth + continuous online evaluation** builds a flywheel hyperscalers cannot replicate. Tiered judge fleets (distilled at 100% sampling, frontier at 1–5%, humans <0.1%) are economically mandatory at enterprise volumes.
- **So what**: bet on evaluations as the durable IP. Five years from now, the model that powers your product will not be the one shipping today — the eval suite is what carries enterprise value across the transition.

## 4. The protocol layer consolidated under the Linux Foundation in 9 months

- **Agentic AI Foundation (AAIF)** launched Dec 9, 2025, founded by Anthropic + OpenAI + Block; 170+ orgs in <4 months. Hosts MCP, goose, AGENTS.md [r][s].
- **MCP**: 17,468 servers (Q1 2026); 97M monthly SDK downloads (970× in 18 months); 67% of enterprise AI teams using or evaluating [t]. The 2025-06-18 spec moved to **Streamable HTTP + OAuth 2.1 + RFC 8707 Resource Indicators**.
- **A2A** (Google → LF Jun 2025): v1.0 GA with 150+ orgs, 22k+ GitHub stars, 5 SDKs by April 2026 [u]. Absorbed IBM ACP in August 2025.
- Stable mental model: **MCP for agent↔tool, A2A for agent↔agent, AG-UI for agent↔frontend.**
- **So what**: bet on protocols, not frameworks. Treat MCP servers and A2A endpoints as the durable interfaces; every BSS/OSS capability behind a TMF Open API needs a parallel MCP/A2A facade or it is invisible to the agent layer.

## 5. Observability and evaluation merged

- The **category line between "observability" and "evaluation" no longer exists in 2026** — Datadog, W&B Weave, Langfuse, Maxim, Phoenix all ship eval-as-code on top of trace storage [v][w][x].
- OpenTelemetry GenAI semantic conventions are still in **Development** status — vendors emit ahead of stable [y]. **Gateways are emerging as the de facto observability cut-point** (Uber, Amazon, Docker, Kong, Solo.io consensus at AAIF MCP Dev Summit, April 2026 [z]).
- **Drift detection is now operational** — Nov 2025 paper formalises the cross-provider attestation problem; silent model updates behind stable names are a regulatory exposure [aa].
- **So what**: adopt OTel GenAI as the wire format and OpenInference as the AI overlay. Build the gateway as your observability + policy + cost-attribution cut-point.

## 6. The semantic layer is the moat, not the model

- **Vector-only RAG is being explicitly downgraded in 2026 architectures.** GraphRAG ~86% accuracy vs ~32% baseline vector RAG on enterprise tasks [bb]. **HippoRAG** (NeurIPS'24): 10–20× cheaper, 6–13× faster, +20 points on multi-hop QA [cc].
- Microsoft **Fabric IQ Ontology** (preview, billing H1 2026) and Palantir's **Ontology** (12-layer agentic architecture) both treat the ontology as "the agent's persistent, queryable memory system" — not the LLM [dd][ee]. **FIBO** January 2026 release: 3,173 entities, now in schema.org core v3.0 [ff].
- Production wins: Sanofi 50× compound-ID acceleration; LinkedIn cut ticket resolution 40h → 15h (63%); Walmart 1.6M-employee feedback graph; Deloitte cyber-intel on Neptune.
- The "**Context Manager**" architectural pattern (Karpathy / Lütke "context engineering" framing) has four parts: semantic/ontology layer + hybrid retrieval + explicit context-augmentation + temporally-aware agent memory.
- **So what**: for a BSS/OSS vendor, the **SID-aligned telco ontology** is the durable enterprise asset competitors cannot trivially replicate. Models change every six months; the ontology compounds. **TM Forum AAIF + Agentic Interactions Security** is the standards anchor [gg].

## 7. The "agentic OS" is a control plane over heterogeneous cores

- Across **banking** (JPM Vault on Thought Machine + 450 AI use cases [hh]; Mastercard Agent Pay "Agentic Tokens" [ii]; Block Goose), **GL** (Workday Illuminate Financial Close Agent; BlackLine; Trintech; FloQast; new "agentic close" category — Ledge, Nominal), **BSS** (Amdocs aOS / Cognitive Core [jj]; Netcracker; Ericsson Telco Agentic AI Studio [kk]; NEC-CSG $2.9B; Qvantel-Optiva), **payments/FX** (Visa Intelligent Commerce [ll]; Stripe ACS [mm] — URBN, Etsy, Coach, Kate Spade, Halara onboarding), and **policy admin** (Duck Creek, Guidewire; AWS Cedar/AgentCore Gateway [nn]; ARPaCCino [oo]) — **the same five-component pattern is hardening**:
  1. **Policy-enforcing gateway** (Cedar/AgentCore, OPA, Mastercard Agentic Tokens) — default-deny, intercept every tool call.
  2. **MCP tool registry** of legacy + new APIs with Server Cards, audit, SLA metadata.
  3. **Context Manager / KG** grounding agents in canonical, governed entities (FIBO, SID/eTOM, ACORD).
  4. **Eval flywheel** — agent decisions become training data; continuous RLHF-lite.
  5. **HITL controls** — escalation tied to monetary impact, regulatory class, reversibility.
- **The agentic OS is not replacing core systems** — banking ledgers, BSS, GLs, PAS remain heterogeneous. **It is a control plane over them — a 2026 analogue to what Kubernetes became for compute.**
- **Vendor-PR caveat**: Temenos and FIS are "AI-bolted, not AI-native" [pp]. Insurance: only ~22% of carriers reached full production in 2025. Amdocs aOS: explicitly "no significant revenue this fiscal year" — programme is real, revenue is forward-looking.
- **So what**: invest in all five components, not just an agent SDK. The vendors winning the control-plane fight are those investing in policy + MCP + KG + eval + HITL together.

## 8. Computer use went from 14.9% to 79.6% on OSWorld in 18 months

- Capability ceiling is **no longer binding** — Claude Mythos, Holo3-122B-A10B, GPT-5.5 cluster within 0.9 points; ~72% human baseline approached [qq][rr]. **Latency, reliability, and prompt-injection are now the bottlenecks.**
- Top agents take 1.4–2.7× more steps than necessary; each step is ~3× slower than the first as context grows. Demo tasks cost 10–30× more compute than human equivalents.
- Two paradigms: **autonomous agents** (ChatGPT Atlas Agent Mode [ss], Project Mariner [tt]) and **co-work agents** (Microsoft Copilot Cowork GA April 2026 [uu], Anthropic Claude Cowork, Replit Agent 4, Cursor agentic mode).
- **Memory portability is the new battleground**: Anthropic Memory default-on March 3, 2026 with import from ChatGPT/Gemini/Perplexity/Grok [vv]; Claude Managed Agents persistent memory beta April 23, 2026 [ww]. Letta, LangMem, Mem0 build the open alternative.
- **Apple Intelligence** Siri overhaul March 2026 (iOS 26.4) introduces on-screen awareness + cross-app actions [xx]. The "agent shell" is a new platform tier.
- **So what**: the "application" is dissolving into three concurrent surfaces — tool surface (MCP), agent surface (A2A), UI surface (AG-UI). A web-only enterprise app is functionally invisible to the agent layer.

## 9. Security is now a product-level differentiator

- **Prompt injection = OWASP #1 LLM risk in 2026, present in 73% of production AI deployments** [yy].
- **ShadowPrompt** (Claude for Chrome, Dec 27, 2025; patched Feb 2026): zero-click XSS + over-permissive origin allowlist; any attacker subdomain could silently inject prompts as if the user typed them [zz][aaa].
- **Brave's Comet PoC**: hidden white-text instructions → Perplexity OTP exfiltration → Gmail OTP read → exfil to Reddit. Account takeover in <4 minutes [bbb].
- **OWASP Q1 2026 round-up**: Flowise CustomMCP arbitrary code execution across thousands of MCP servers; Trivy/LiteLLM supply-chain compromise (Feb–Mar 2026) cascaded credential theft [ccc].
- Anthropic claims ~1% attack success against Claude 4.5 in browser-extension config — explicitly states "**No browser agent is immune to prompt injection**" [ddd]. 1% × every web page = a non-trivial enterprise threat surface.
- **So what**: untrusted-content trust-tier boundary is now load-bearing architectural infrastructure. A vendor that can credibly say "our MCP servers enforce least-privilege OAuth scopes, our A2A skills sign every Agent Card, our memory is tier-isolated, our agents fail-closed on injection-classifier hits" sells against vendors who can't.

## 10. Compliance is product, not paperwork

- **EU AI Act high-risk obligations conditionally postponed**: Annex III to **2 December 2027**, Annex I to **2 August 2028** under the Digital Omnibus voted by the European Parliament March 2026 (569–45) [eee][fff]. Article 50 (deepfake labelling, AI-interaction disclosure) and GPAI enforcement still activate **2 August 2026**.
- **GPAI Code of Practice** (Jul 2025): Anthropic, OpenAI, Google, Microsoft, Amazon, IBM, Mistral, Cohere, Aleph Alpha signed. **Meta refused. xAI signed Safety only** [ggg].
- **ISO/IEC 42001**: 100+ certified orgs in 18 months — AWS, Anthropic, Microsoft, Google, IBM Granite, KPMG International [hhh][iii]. Increasingly named in U.S. federal and large-enterprise RFPs alongside SOC 2 + ISO 27001.
- **Live now, not future**: NYDFS Insurance Circular 7 (Jul 2024) [jjj]; **DORA** (since Jan 17, 2025; CTPP regime active Nov 18, 2025) [kkk]; Korea AI Basic Act (Jan 2026); China Synthetic Content Identification Rules (Sep 1, 2025); India IT Rules Amendment (Feb 20, 2026).
- **Sovereign AI**: Cohere $20B acquisition of Aleph Alpha (Apr 24, 2026, German + Canadian government-backed) [lll]; S3NS SecNumCloud-qualified Dec 17, 2025 [mmm]; Bleu, Delos, Stargate UAE all operational. **"Regional model" requirements now in tenders** — must be trained or fine-tuned on regionally-curated data, hosted in-region.
- **So what**: ship a **portable model-serving abstraction**, **per-tenant residency with cryptographic guarantees**, **agent-action-level audit logging**, **automated model/data card generation**, and **HITL scaffolding as platform primitive**. Run the regulatory clock as a product roadmap.

## 11. The services-disruption question — directly relevant to Amdocs

- **BCG explicitly frames agentic AI as both a $200B opportunity and a structural threat to technology-services delivery models** [ooo]. Coding-agent maturity (Claude Code, Codex, Cursor, Copilot Agent Mode, Replit Agent 4) is operating at repo-level on real artifacts.
- **EY Canvas embeds multi-agent framework across 160,000 audit engagements** [ppp] — professional services are not advising on agents; they are rebuilding their delivery platforms around agents. **Intel "One AI"** is doing the same internally [qqq].
- **Consulting calibration**:
  - Gartner: 40% of enterprise apps include task-specific agents by end-2026 (from <5% in 2025); 40% of agentic-AI projects cancelled by 2027 [rrr].
  - Bain: 80% of GenAI use cases meet expectations; only **23% can tie to measurable revenue or cost** [sss] — the killer question is "can we prove business impact under controlled risk?"
  - KPMG: deployment quadrupled then settled at 26%; **61% of boards "not actively exploring agentic AI"** [ttt] — directly contradicts PwC's 79% adoption claim.
  - Thoughtworks: 93% of IT leaders plan to deploy AI agents by 2026 [uuu].
- **Chinese challengers**: Manus reportedly blocked from Meta acquisition (April 2026, framed as national-strategic AI asset); DeepSeek hiring for agentic AI; Qwen 3.6-35B-A3B (3B active params at competitive vision-language quality) [vvv][www]. **Margins compress on the model layer in markets without sovereignty constraints.**
- **Honest synthesis**: agentic AI compresses unit economics on **commoditised services** (code generation, test scaffolding, documentation, migration, ticket triage) faster than on **integrated services tied to systems of record, regulatory context, and domain ontologies**. The defensible model is to convert threatened labour-hours into **productised, governed, metered agent services with the eval flywheel and compliance instrumentation as the moat**. The undefensible model is to keep selling labour-hours on tasks an agent can do.
- **For Amdocs specifically**: the SID-aligned telco knowledge graph, TM Forum AAIF Agentic Interactions Security positioning, and "control plane over heterogeneous BSS cores" framing are exactly the assets that survive — *if* productised as governed agent services rather than embedded in human-led integration projects. **The window is short**: JPMorgan ($18B / 450+ AI use cases), Verizon (28k care reps), AT&T (Ask AT&T, 100k users / ~27B tokens/day), BBVA (120k employees) demonstrate that major customers can build their own agent platforms when the infrastructure is hyperscaler-provided. Integrator value-add must move up the stack: **domain ontology, eval flywheel, regulatory packs, BSS-semantics expertise** — the part hyperscalers cannot ship.
- **So what**: re-shape services revenue from labour-hours into outcome-priced governed agent execution **before clients do it themselves** with hyperscaler agents.

## 12. Non-LLM GenAI is shipping in production

- **Diffusion language models** (Mercury) break the per-token latency assumption — 10× throughput at competitive code quality [d].
- **State-space models** (Mamba family, Jamba, Granite 4.0, Codestral Mamba, Zamba 2) win on long context and constant-memory inference [b][c].
- **JEPA family** (V-JEPA 2 / V-JEPA 2-AC / LeJEPA) — latent-space prediction, zero-shot robotics at 65–80% on Franka arms with **15× faster planning than NVIDIA Cosmos** [e][f].
- **Neuro-symbolic AI** re-entered Gartner Hype Cycle 2025 — Amazon Vulcan, Rufus reporting 15–23% reasoning-accuracy gains; needed for regulatory traceability and explainability [nnn].
- **So what**: when regulators ask "why did the model output this?", you cannot answer with a 671B-param softmax. Budget for neuro-symbolic and JEPA-style overlays as a **control plane**, not a model class.

---

## Eyes on the ball — the layered framework

| Layer | Posture | Why |
|---|---|---|
| Model | **Abstract** | ~5% spread at frontier; 9×–900×/yr cost compression; 1-yr deprecation cycle |
| Eval | **BET** | Encodes domain ground truth; only safe model-swap mechanism; 12–24-month half-life |
| Protocol (MCP/A2A/AG-UI) | **Abstract** | LF-governed, converging; commodity by 2027 |
| Inference runtime | **Abstract** | vLLM/TensorRT/SGLang commoditising |
| Agent runtime | **BET** | Durable state, replay, sandboxing differentiated through 2027 |
| Vector DB | **Abstract** | Commoditising fast |
| **Knowledge graph / Ontology / Context Manager** | **BET** | The durable enterprise asset; SID/FIBO/ACORD compounds |
| Generic agent | **Abstract** | Will be commoditised by frontier-model providers |
| Domain-specific agent + wrappers | **BET** | Embedded process knowledge + integrations + guardrails + HITL UX |
| **Compliance instrumentation** | **BET, AS PRODUCT** | Audit logs, lineage, watermarking, residency — customers will pay for compliance-by-design |

---

## Architectural shape that survives 2027–2028 churn

1. **Model-portable by construction** — features specify minimum capability; platform routes; migration is hours not quarters.
2. **Eval-first development** — features ship with eval suites *before* prompts; no production deploy without regression baseline.
3. **Deployment-target-pluggable** — same binary on hyperscaler, sovereign cloud, on-prem GPU, air-gapped enclave.
4. **Knowledge-graph-centric, not vector-centric** — RAG is a retrieval implementation detail; the KG is the durable artefact.
5. **Agent runtime with replay and durable state** — every action logged with replay/branch/audit fidelity (also AI Act Article 12 logging operationalised).
6. **Policy-as-code at every boundary** — auth, content filtering, residency, disclosure, HITL gating externalised to OPA/Cedar/custom engines.
7. **Compliance evidence is generated, not assembled** — model cards, lineage, eval reports, incident logs emerge from the platform automatically. The audit binder is a query.

---

## Six CEO postures for 2026–2028

1. **Bet on the integration surface, not the model.** The defensible business owns the last mile from foundation model to enterprise outcome — knowledge engineering, change management, compliance instrumentation. Not LLM science.
2. **Treat compliance as a feature.** ISO 42001, EU AI Act readiness, NYDFS/DORA posture are increasingly the basis of vendor selection. Build, charge, sell.
3. **Optimise for optionality, not for any single bet.** Architect for swap. Pay the abstraction tax willingly.
4. **Invest in evaluations as durable IP.** What carries enterprise value across model transitions.
5. **Run the regulatory clock as a product roadmap.** AI Act, Korea, China, India, NYDFS, DORA — present obligations, not future events.
6. **Hold strategic patience on the agent thesis.** Build foundations now (durable execution, policy-as-code, eval harnesses, MCP-compatible tool layer). Deploy narrow agents in production for high-value use cases. Avoid declaring an "agent strategy" that depends on protocols and runtimes that do not yet exist.

---

## Buyer evaluation framework — 10 axes for any "agent platform" decision

| Axis | Killer question |
|---|---|
| **Context** | Beyond vector search — entities, relationships, policies, process flows, source provenance, time, ownership? |
| **Governance** | Every agent action permissioned, logged, replayed, revoked, explained? |
| **Identity** | Agents as **non-human identities** with scoped credentials, OAuth 2.1, signed Agent Cards? |
| **Evaluation** | Tiered judge fleet + code-evals every commit + eval-as-code primitives (Inspect AI shape)? |
| **Integration** | MCP / A2A / AG-UI surfaces; OAuth on every tool; trust-tier boundary on untrusted content? |
| **Observability** | OTel-GenAI + OpenInference traces; span linking across MCP / A2A; per-tenant cost; drift detection? |
| **Portability** | Same binary on hyperscaler / sovereign cloud / on-prem GPU / air-gapped enclave? Model-routing layer? |
| **Compliance** | ISO 42001 management system; AI-Act Articles 11–13 automation; DORA Article 30; NYDFS / regional rules? |
| **Economics** | Cost-attribution at agent-action level; active-parameter accounting; cascade routing? |
| **IP protection** | Cross-client knowledge isolation contractually guaranteed, not just opt-out? |

**The Bain killer question**: ***"Can we prove business impact under controlled risk?"*** The 80% / 23% gap (use cases meet expectations / firms tie to measurable revenue or cost) is the single most quotable consulting data point [sss].

**Bottom-line buyer thesis**: do not buy "agents." Buy **controlled execution capacity over a well-governed context layer**.

---

## Bottom line

> The model market is commoditising. The protocol market is consolidating under the Linux Foundation. The eval, knowledge-graph, agent-runtime, security, and compliance layers are where competitive advantage lives in 2026–2028. **For services-heavy enterprise software vendors, the threat is structural: agentic AI compresses labour-hour services faster than it compresses integrated services tied to systems of record, regulatory context, and domain ontologies. The defensible move is to convert the threatened labour-hours into productised, governed, metered agent services — with the eval flywheel and compliance instrumentation as the moat — *before clients build their own agent platforms on hyperscaler infrastructure*. Bet on what compounds. Abstract what commoditises. Run the regulatory clock as a product roadmap. The vendors who keep their eyes on the ball — not on the next model release — will be the ones whose customers pass their own audits in 2027 and beyond.**

---

## References (executive brief)

*Tier indicators: T1 = primary; T2 = reputable secondary; T3 = analyst/vendor blog. Full reference list with 141 sources in the companion `full-research.md`.*

[a] arXiv 2412.19437 — DeepSeek-V3 Technical Report — https://arxiv.org/abs/2412.19437 (T1)
[b] AI21 Labs, "Announcing Jamba" — https://www.ai21.com/blog/announcing-jamba/ (T1)
[c] Zyphra, "Zamba2-7B" — https://www.zyphra.com/post/zamba2-7b (T1)
[d] arXiv 2506.17298 — "Mercury: Ultra-Fast Language Models Based on Diffusion" — https://arxiv.org/abs/2506.17298 (T1)
[e] arXiv 2506.09985 — "V-JEPA 2: Self-Supervised Video Models Enable Understanding, Prediction and Planning" — https://arxiv.org/abs/2506.09985 (T1)
[f] arXiv 2511.08544 — "LeJEPA: Provable and Scalable Self-Supervised Learning Without the Heuristics" — https://arxiv.org/abs/2511.08544 (T1)
[g] DeepMind, "Genie 3: A new frontier for world models" — https://deepmind.google/blog/genie-3-a-new-frontier-for-world-models/ (T1)
[h] NVIDIA Newsroom, "Cosmos World Foundation Model Platform" — https://nvidianews.nvidia.com/news/nvidia-launches-cosmos-world-foundation-model-platform-to-accelerate-physical-ai-development (T1)
[i] Stanford HAI, "AI Index 2025 — Technical Performance" — https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance (T1)
[j] Epoch AI, "LLM inference price trends" — https://epoch.ai/data-insights/llm-inference-price-trends (T1)
[k] LLM Stats, "Latest model releases" — https://llm-stats.com/llm-updates (T2)
[l] Anthropic, "Model deprecation policy" — https://platform.claude.com/docs/en/about-claude/model-deprecations (T1)
[m] ACL Anthology — "Judging the Judges: Position Bias in LLM-as-a-Judge" (IJCNLP 2025) — https://aclanthology.org/2025.ijcnlp-long.18/ (T1)
[n] arXiv 2410.21819 — "Measuring Self-Preference in LLM Judgments" (EMNLP 2025) — https://arxiv.org/abs/2410.21819 (T1)
[o] ACL Anthology — "Rating Roulette: Self-Inconsistency" (EMNLP Findings 2025) — https://aclanthology.org/2025.findings-emnlp.1361.pdf (T1)
[p] arXiv 2503.13657 — "MAST: Why Do Multi-Agent LLM Systems Fail?" (NeurIPS 2025) — https://arxiv.org/abs/2503.13657 (T1)
[q] TimeToAct, "LLM Benchmarks January 2025" — https://www.timetoact-group.at/en/insights/llm-benchmarks/llm-benchmarks-january-2025 (T3)
[r] Linux Foundation, "AAIF formation press release" (Dec 9, 2025) — https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation (T1)
[s] Anthropic, "Donating MCP and establishing AAIF" — https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation (T1)
[t] MCP Manager, "MCP Adoption Statistics 2026" — https://mcpmanager.ai/blog/mcp-adoption-statistics/ (T3)
[u] Linux Foundation, "A2A protocol surpasses 150 organizations" (April 2026) — https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year (T1)
[v] Datadog press release, "AI Agent Monitoring, LLM Experiments" (Jun 2025) — https://www.datadoghq.com/about/latest-news/press-releases/datadog-expands-llm-observability-with-new-capabilities-to-monitor-agentic-ai-accelerate-development-and-improve-model-performance/ (T1)
[w] W&B Weave releases — https://github.com/wandb/weave/releases (T1)
[x] Langfuse, "MCP tracing" — https://langfuse.com/docs/observability/features/mcp-tracing (T1)
[y] OpenTelemetry, "GenAI Semantic Conventions" — https://opentelemetry.io/docs/specs/semconv/gen-ai/ (T1)
[z] InfoQ, "AAIF MCP Dev Summit (April 2026)" — https://www.infoq.com/news/2026/04/aaif-mcp-summit/ (T2)
[aa] arXiv 2511.07585 — Cross-provider LLM output drift — https://arxiv.org/html/2511.07585v1 (T1)
[bb] Microsoft Research, "Project GraphRAG" — https://www.microsoft.com/en-us/research/project/graphrag/ (T1)
[cc] OpenReview / OSU-NLP-Group/HippoRAG (NeurIPS'24) — https://github.com/OSU-NLP-Group/HippoRAG (T1)
[dd] Microsoft Learn, "Fabric IQ Ontology overview" — https://learn.microsoft.com/en-us/fabric/fundamentals/ontology-overview (T1)
[ee] Palantir, "Connecting AI to Decisions with the Palantir Ontology" — https://blog.palantir.com/ (T1)
[ff] EDM Council FIBO — https://edmcouncil.org/page/fibo (T1)
[gg] TM Forum, "AI-Native Blueprint" / Agentic Interactions Security — https://www.tmforum.org/ (T1)
[hh] Computer Weekly, "JPMorgan Chase to replace core banking with Thought Machine" — https://www.computerweekly.com (T2)
[ii] Mastercard newsroom, "Agent Pay" press releases — https://www.mastercard.com/news (T1)
[jj] Light Reading, "Amdocs goes all-in for agentic AI with telco OS platform" — https://www.lightreading.com (T2)
[kk] Ericsson, "Telco Agentic AI Studio" — https://www.ericsson.com (T1)
[ll] American Banker, "Visa Intelligent Commerce" — https://www.americanbanker.com (T2)
[mm] Stripe, "Agentic Commerce Suite" — https://stripe.com/blog (T1)
[nn] AWS ML Blog, "Secure AI agents with Policy in Amazon Bedrock AgentCore (Cedar)" — https://aws.amazon.com/blogs/machine-learning/ (T1)
[oo] arXiv 2507.10584 — "ARPaCCino: Agentic-RAG for Policy as Code Compliance" — https://arxiv.org/abs/2507.10584 (T1)
[pp] Backbase, "Top AI-native banking platform providers 2026" — https://www.backbase.com (T3)
[qq] XLANG, "OSWorld-Verified" — https://xlang.ai/blog/osworld-verified (T1)
[rr] BenchLM, OSWorld-Verified leaderboard — https://benchlm.ai/benchmarks/osWorldVerified (T2)
[ss] OpenAI, "Introducing ChatGPT Atlas" — https://openai.com/index/introducing-chatgpt-atlas/ (T1)
[tt] DeepMind, "Project Mariner" — https://deepmind.google/models/project-mariner/ (T1)
[uu] Microsoft 365 Blog, "Copilot agentic capabilities GA in Word/Excel/PowerPoint" (Apr 2026) — https://www.microsoft.com/en-us/microsoft-365/blog/2026/04/22/copilots-agentic-capabilities-in-word-excel-and-powerpoint-are-generally-available/ (T1)
[vv] Bloomberg, "Anthropic memory feature" (Mar 3, 2026) — https://www.bloomberg.com/news/articles/2026-03-03/anthropic-tries-to-win-users-from-chatgpt-with-memory-feature (T2)
[ww] Anthropic, "Memory tool documentation" — https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool (T1)
[xx] ACS, "Apple reveals the AI behind Siri's big 2026 upgrade" — https://ia.acs.org.au/article/2026/apple-reveals-the-ai-behind-siri-s-big-2026-upgrade.html (T2)
[yy] OWASP GenAI, "LLM01 Prompt Injection" — https://genai.owasp.org/llmrisk/llm01-prompt-injection/ (T1)
[zz] The Hacker News, "Claude extension flaw enabled zero-click attack" — https://thehackernews.com/2026/03/claude-extension-flaw-enabled-zero.html (T2)
[aaa] Koi Security, "ShadowPrompt" — https://www.koi.ai/blog/shadowprompt-how-any-website-could-have-hijacked-anthropic-claude-chrome-extension (T2)
[bbb] Brave Browser blog, "Indirect prompt injection in Comet" — https://brave.com/blog/comet-prompt-injection/ (T1)
[ccc] OWASP GenAI, "Q1 2026 Exploit Round-up" — https://genai.owasp.org/2026/04/14/owasp-genai-exploit-round-up-report-q1-2026/ (T1)
[ddd] Anthropic, "Prompt-injection defenses" — https://www.anthropic.com/news/prompt-injection-defenses (T1)
[eee] EU AI Act Implementation Timeline — https://artificialintelligenceact.eu/implementation-timeline/ (T1)
[fff] European Parliament press release, "AI Act delayed application" (March 2026) — https://www.europarl.europa.eu/news/en/press-room/20260323IPR38829/artificial-intelligence-act-delayed-application-ban-on-nudifier-apps (T1)
[ggg] GPAI Code of Practice — Final Version — https://code-of-practice.ai/ (T1)
[hhh] ISO/IEC 42001:2023 catalogue — https://www.iso.org/standard/42001 (T1)
[iii] AWS, "ISO/IEC 42001 surveillance audit (Aug 2025)" — https://aws.amazon.com/blogs/security/aws-successfully-completed-its-first-surveillance-audit-for-iso-420012023-with-no-findings/ (T1)
[jjj] NYDFS Insurance Circular Letter No. 7 (2024) — https://www.dfs.ny.gov/industry-guidance/circular-letters/cl2024-07 (T1)
[kkk] EIOPA, "DORA hub" — https://www.eiopa.europa.eu/digital-operational-resilience-act-dora_en (T1)
[lll] Fortune, "Cohere–Aleph Alpha sovereign-AI deal" (24 Apr 2026) — https://fortune.com/2026/04/24/cohere-aleph-alpha-deal-signals-rise-of-ai-middle-powers-counterweight-to-u-s-china/ (T2)
[mmm] Futurum, "S3NS sovereignty: SecNumCloud qualification" — https://futurumgroup.com/insights/s3ns-sovereignty-can-thales-google-venture-make-ai-sovereignty-work-at-scale/ (T2)
[nnn] AllegroGraph, "Neuro-Symbolic AI in Gartner 2025 Hype Cycle" — https://allegrograph.com/the-rise-of-neuro-symbolic-ai-a-spotlight-in-gartners-2025-ai-hype-cycle/ (T3)
[ooo] BCG, "Agentic AI: $200B opportunity / threat to technology services" — https://www.bcg.com/publications (T2)
[ppp] EY, "EY Canvas multi-agent audit framework — 160,000 audit engagements" — https://www.ey.com/en_gl/news (T2)
[qqq] Intel, "One AI internal agentic platform" — https://www.intel.com/content/www/us/en/newsroom (T2)
[rrr] Gartner, "Hype Cycle for Agentic AI 2026" + 40%-by-2026 / 40%-cancellation predictions — https://www.gartner.com/en/research (T2)
[sss] Bain & Company, "AI Survey 2025–2026 — 80%/23% measurable-impact gap" — https://www.bain.com/insights/ (T2)
[ttt] KPMG, "Generative AI Pulse / AI Gateway — 61% boards-not-exploring" — https://kpmg.com/us/en/insights/2026/agentic-ai-pulse.html (T2)
[uuu] Thoughtworks, "Agentic AI Advantage 2026 — 93% IT-leader deploy plan" — https://www.thoughtworks.com/insights (T2)
[vvv] Reuters / Bloomberg / FT coverage of China-Manus / Meta acquisition block (April 2026) — verify exact source before quoting (T2)
[www] Bloomberg, "DeepSeek hires for agentic AI roles" + WSJ DeepSeek agentic-capability coverage — https://www.bloomberg.com / https://www.wsj.com (T2)

---

*See `./full-research.md` for the full ~14,500-word research report with 160 cited sources organised by section.*
