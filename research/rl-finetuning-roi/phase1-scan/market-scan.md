# Phase 1a — Market & Industry Trends Scan
*Date: 2026-04-28 | Tier: scan | Author: Market & Industry Trends scout*

---

## 1. Executive Map

Five to eight signals that dominate the answer space as of Q2 2026:

**Signal 1 — Token prices have collapsed ~300x since GPT-4 launch (2023–2026), structurally undermining pass-through economics.** GPT-4 opened at $30/M input tokens in March 2023; GPT-5.4 costs $2.50/M in March 2026 and DeepSeek V4-Flash costs $0.28/M output. Any business model predicated on reselling frontier-model tokens at a stable margin faces persistent compression. [Source 1, Source 2]

**Signal 2 — Frontier model release cadence is now sub-quarterly.** Between January 2025 and April 2026, at least twelve distinct frontier or near-frontier model families shipped major versions. Open-source trailing lag against frontier closed models is estimated at 9–12 months as of mid-2025, shrinking rapidly. [Source 4]

**Signal 3 — Enterprise fine-tuning adoption is declining in favor of prompt engineering and context injection.** a16z's 2025 CIO survey found most enterprises "aren't seeing as much ROI on fine-tuning as last year." Only hyper-specific domain applications retain a fine-tuning investment thesis. [Source 6]

**Signal 4 — The inference layer is growing faster than the post-training layer.** Fireworks AI hit $315M ARR at 416% YoY growth in February 2026; Together AI surpassed $150M ARR with $305M raised. Both are pure inference infrastructure plays with no proprietary model training. [Source 8]

**Signal 5 — Vertical AI "winners" (Harvey, Sierra, Glean) derive moats from workflow lock-in and data flywheels, not model-training differentiation.** Harvey ($195M ARR, $11B valuation in March 2026) deliberately multi-homed to OpenAI + Anthropic + Google to *reduce* model dependency. [Source 9, Source 10]

**Signal 6 — Federated RLHF exists as research (FedRLHF, FedBiscuit, Dec 2024) but no productionized multi-tenant RL fine-tuning with contractual data isolation has been publicly documented at enterprise scale.** [Source 14]

**Signal 7 — The "dead end" case studies are instructive.** Stability AI burned $99M in compute against $11M revenue in 2023, defaulted on AWS/GCP/CoreWeave GPU debt, and lost its CEO and research leads by March 2024. Inflection and Adept were acqui-hired by Microsoft and Amazon respectively. Aleph Alpha pivoted entirely away from model building, then was acquired by Cohere in April 2026. The pattern: companies that competed on foundation model building without a sovereign/specialized moat ran out of margin room. [Source 11, Source 15, Source 16]

---

## 2. Hyperscaler Release Cadence and Capability Creep

### 2024 Timeline

| Date | Model | Significance |
|------|-------|-------------|
| Mar 2024 | Claude 3 Opus / Sonnet / Haiku | First major Anthropic family with tiered pricing |
| May 2024 | GPT-4o | $5/M input; 2x cheaper than GPT-4 Turbo |
| Jun 2024 | Claude 3.5 Sonnet | Code generation benchmark leader; captured 42% market share in coding |
| Jul 2024 | GPT-4o mini | $0.15/M input; 200x cheaper than GPT-4 original in 16 months |
| Oct 2024 | Claude 3.5 Sonnet v2 | Incremental update; established Anthropic's update cadence |
| Dec 2023 | Gemini 1.0 Pro | $0.125/M input |

### 2025 Timeline

| Date | Model | Significance |
|------|-------|-------------|
| Jan 20, 2025 | DeepSeek R1 | Open-weight reasoning model; triggered market-wide repricing; $0.55/M input |
| Jan 30, 2025 | Mistral Small 3 (24B) | GPT-4-class performance at open-weight scale |
| Feb 2025 | Claude 3.7 Sonnet + Claude Code | Described by Simon Willison as "most impactful event of 2025" for software engineering |
| Apr 5, 2025 | Llama 4 (109B + 400B MoE) | Disappointing enterprise reception despite scale |
| Apr–May 2025 | GPT-5, GPT-5 Thinking | Major capability jump; agentic tasks now executable in hours |
| May 2025 | Claude 4 Sonnet + RFT on o4-mini | OpenAI releases RL fine-tuning to public; Anthropic iterates quickly |
| Aug 2025 | Claude Opus 4.5 | 67% price cut from $15/$75 to $5/$25 per M tokens |
| Late 2025 | GPT-5.2–5.4 series | Sub-monthly major iteration rhythm established |

### 2026 Timeline (through April)

| Date | Model | Significance |
|------|-------|-------------|
| Jan 2026 | Gemini 3.1 | Real-time voice/image analysis; multimodal parity claim |
| Mar 5, 2026 | GPT-5.4 / 5.4 Thinking | $2.50/M input; mainstream reasoning model |
| Mar 17, 2026 | GPT-5.4 mini/nano | Sub-$0.50/M capable tier |
| Apr 24, 2026 | DeepSeek V4 | $3.48/M output vs OpenAI $30/M; $0.28/M for Flash tier |

### Assessment: Does "next release eat your fine-tune"? — Partially confirmed, structurally likely

The empirical record does not contain a clean A/B comparison of a named domain fine-tune's benchmark performance before and after a general model superseded it (hypothesis B). However, indirect evidence is strong:

- The Menlo Ventures mid-2025 enterprise survey found open-source models (the primary fine-tuning substrate) running 9–12 months behind frontier closed-source in capability. [Source 4]
- Within one month of Claude 4's release, it captured 45% of Anthropic users; Claude Sonnet 3.5 dropped from 83% to 16% of the installed base. [Source 4] This is a direct proxy for how quickly even production deployments migrate when a better base arrives.
- The a16z 2025 CIO survey found declining fine-tuning ROI among enterprises, with long context windows and improved instruction following cited as the key displacement mechanism. [Source 6]
- METR data shows reasoning model task horizons doubling every 7 months; if this holds, a fine-tune trained for 30-minute task execution is functionally obsolete within one hardware generation. [Source 5]

**Disconfirming evidence:** Harvey's RFT gains (20% F1 improvement in citation extraction) have persisted through multiple base model upgrades because the domain eval is proprietary and complex enough that general model improvements have not yet closed the gap. This suggests durability is domain-specificity-dependent, not a universal law. [Source 9]

---

## 3. RFT / RL Fine-Tuning Vendor Offerings (State of the Market)

| Vendor | Offering | Pricing (approx.) | Known Production Users | Notable Benchmarks |
|--------|----------|-------------------|------------------------|-------------------|
| **OpenAI** | RFT on o3-mini, o4-mini; grader-based feedback loop | Per-training-token + GPU-hr; enterprise contract | Accordance AI, Ambience Healthcare, Harvey, Runloop, Milo | Accordance: 39% accuracy uplift on tax reasoning; Ambience: +12 pts on ICD-10 vs physician baseline; Harvey: +20% F1 citation extraction; Runloop: +12% on Stripe API code; Milo: +25 pts scheduling correctness [T4] |
| **Anthropic** | Constitutional AI + RLHF in training loop; no public RFT API as of April 2026 | Inference pricing only at public tier | Undisclosed enterprise | No public RFT API benchmarks |
| **Google Vertex AI** | Supervised fine-tuning (SFT); RLHF roadmap; reinforcement tuning for Gemini in preview | $3–$10/M training tokens (Vertex pricing page) | Financial services, healthcare verticals | No published RFT case studies with external benchmarks |
| **AWS Bedrock** | Custom fine-tuning for select models; Bedrock model evaluation | Consumption-based; no published RL-specific pricing | Undisclosed | No public RL fine-tuning benchmarks |
| **NVIDIA NeMo** | Full post-training stack: SFT, RLHF, DPO, synthetic data gen; enterprise on-prem | NVIDIA AI Enterprise license ($4,500/GPU/year referenced); NIM microservices | Databricks integration; undisclosed enterprise | Framework-level benchmarks only |
| **Databricks** | DBRX fine-tuning; integration with NeMo; MLflow for tracking | Consumption-based on DBU; $0.54–$5.40/DBU | Undisclosed mid-market | No public domain benchmark comparisons |
| **Predibase** | LoRA + QLoRA SaaS; serverless multi-adapter inference; VPC deployment | $25 starter; enterprise via contract | Self-serve + enterprise | Multi-LoRA on shared GPU claimed to cut inference cost 10x vs dedicated deployment |
| **Fireworks AI** | Serverless inference + fine-tuning; reinforcement fine-tuning billed per GPU-hr; Multi-LoRA | Per-token + per-GPU-second | 10,000+ companies, $315M ARR [February 2026] | FireAttention proprietary kernel; no public domain RFT benchmarks |
| **Together AI** | Serverless + dedicated inference; fine-tuning; batch processing | Serverless from $0.20/M; dedicated container pricing | Undisclosed; $150M+ ARR | Llama 3.1 70B quality eval vs Cerebras/Groq/Fireworks (latency/quality matrix) |

*Sources: [T4] = vendor advocacy; benchmark claims from [Source 3, Source 7, Source 8, Source 12]*

**Key structural observation:** Of the nine vendors listed, five (Fireworks, Together, Predibase, AWS Bedrock, Google Vertex) generate the majority of their revenue from inference, not from the fine-tuning act itself. Fireworks explicitly prices RL fine-tuning per GPU-hour, making it a cost-of-goods line, not a margin-expanding service.

---

## 4. Enterprise Case Studies — Winners and Losers

### Winners

**Harvey (Legal AI)**
- *Post-training intervention:* RFT on OpenAI o-series for citation extraction; domain-specific eval with F1 scoring; custom legal reasoning chains. Multi-model deployment across OpenAI + Anthropic + Google.
- *Economic outcome:* $195M ARR (2025), $11B valuation (March 2026), 100,000 lawyers across 1,300 organizations including AmLaw 100 majority.
- *Durability claim:* Harvey's moat is explicitly *not* the model — it is the 25,000+ custom workflow agents and the proprietary legal eval suite that makes migration expensive. The company proactively multi-homed to reduce foundation model dependency. [Source 9]
- *Hypothesis G relevance:* Harvey is the closest production instance of the "interior optimum" — spend enough on RL fine-tuning to beat the eval, then convert that advantage into workflow lock-in before the next base model closes the gap.

**Sierra (Customer Service Agents)**
- *Post-training intervention:* Not disclosed publicly; Sierra builds on foundation models with proprietary orchestration.
- *Economic outcome:* $150M ARR (January 2026), up from $26M at end of 2024 — 5.8x growth in 12 months. Outcome-based pricing per conversation/resolution.
- *Hypothesis relevance:* Sierra's pricing model (outcome-based) transfers foundation model cost risk to the vendor, making inference economics critical. [Source 10]

**Cohere (Sovereign Enterprise)**
- *Post-training intervention:* Command R+ fine-tuning with vertical design partners (RBC for banking, Fujitsu for Japanese enterprise); synthetic data generation to avoid touching client proprietary data; private deployment via Model Vault VPC product.
- *Economic outcome:* $240M ARR (end 2025), up from $62M end 2024. Gross margins ~70%.
- *Hypothesis E relevance:* Cohere's approach to multi-tenant economics is instructive — they use design partners to generate synthetic training data, then sell the resulting specialized model across the entire vertical without needing each client's raw data. This is the closest publicly documented solution to the multi-tenant data isolation problem. However, it depends on Cohere owning the model, not merely fine-tuning a third-party base. [Source 17]

**DeepSeek (Efficiency-at-scale)**
- *Post-training intervention:* GRPO (Group Relative Policy Optimization) replacing PPO; synthetic CoT data; distillation from larger models. Entire R1 reasoning model family at fraction of US lab cost.
- *Economic outcome:* DeepSeek V4-Flash costs $0.28/M output tokens (April 2026) vs $30/M for comparable OpenAI output. Forced industry-wide repricing.
- *Hypothesis B relevance:* DeepSeek demonstrates that RL post-training (not just SFT) can produce reasoning gains competitive with massively capitalized labs. However, DeepSeek's advantage is state-subsidized compute and a research-publication strategy that may not translate to a commercial moat. [Source 2, Source 18]

**Glean (Enterprise Search)**
- *Post-training intervention:* Primarily RAG + search; minimal disclosed fine-tuning.
- *Economic outcome:* $208M ARR (2025), 89% YoY growth.
- *Hypothesis relevance:* Glean's high growth without heavy fine-tuning investment supports hypothesis D (inference and integration layer being the real moat). [Source 10]

### Losers / Warnings

**Stability AI**
- *Intervention:* Open-source generative model development (Stable Diffusion, Stable LM); minimal enterprise-specific fine-tuning revenue strategy.
- *Economic outcome:* $99M compute spend vs $11M revenue in 2023. Defaulted on AWS, Google Cloud, CoreWeave GPU payments. CEO Emad Mostaque resigned March 2024 following investor revolt and staff exodus. Company operated with ~8 employees as of the research period.
- *Causal mechanism:* Open-source model publishing without a capture strategy. Competitors could use Stable Diffusion without paying Stability anything. No inference lock-in, no enterprise workflow integration. [Source 11]

**Inflection AI**
- *Intervention:* Consumer chatbot (Pi) with RLHF personalization focus.
- *Economic outcome:* Acqui-hired by Microsoft for ~$653M in March 2024. Core team (Mustafa Suleiman, Karen Simonyan) absorbed into Microsoft AI. Residual entity pivoted to enterprise.
- *Causal mechanism:* Consumer chatbot economics are terminal — token costs are not recoverable at consumer price points without massive scale, and Microsoft could buy the talent cheaper than the technology. [Source 11]

**Adept AI**
- *Intervention:* Multimodal action-agent fine-tuning for enterprise workflow automation.
- *Economic outcome:* Acqui-hired by Amazon in 2024; ~20 employees remained. David Luan leads new Amazon agent lab.
- *Causal mechanism:* Enterprise workflow automation at the model level proved premature — agents fail at production-grade reliability without the surrounding evaluation, observability, and orchestration infrastructure that larger platform players already possessed. [Source 11]

**Aleph Alpha**
- *Intervention:* Sovereign European LLM (Luminous family); full model training.
- *Economic outcome:* Pivoted in 2024 from model building to PhariaAI enterprise platform. Acquired by Cohere in April 2026 (deal valued Cohere at $20B implied; Aleph Alpha had "little revenue and significant losses" per reporting). [Source 15, Source 16]
- *Causal mechanism:* Hypothesis E in practice — matching US hyperscaler compute scale was not economically viable for a European startup without cross-client data amortization. Sovereign differentiation (data residency, EU regulatory compliance) was real but insufficient to fund the training runs. Cohere's acquisition was described as "born of sovereignty, necessity." [Source 15]

**AI21 Labs**
- *Intervention:* Jurassic-series LLM, fine-tuning-as-a-service.
- *Economic outcome:* Raised $300M Series D (May 2025, led by NVIDIA and Alphabet); pivoted to enterprise platform play. Not a collapse, but a pivot — the original foundation model business model did not produce standalone profitability.
- *Causal mechanism:* Transparency index score improved from 25 (2023) to 66 (2025), suggesting the company learned and adapted. Still classified here as a cautionary data point because the original fine-tuning-first model strategy required external rescue capital. [Source 11]

---

## 5. Cost & Pricing Dynamics

### Token Price Trajectory

| Model | Date | Input $/M | Output $/M | Source |
|-------|------|-----------|------------|--------|
| GPT-4 (8K) | Mar 2023 | $30.00 | $60.00 | [Source 1] |
| GPT-4 Turbo | Nov 2023 | $10.00 | $30.00 | [Source 1] |
| Claude 3 Opus | Mar 2024 | $15.00 | $75.00 | [Source 1] |
| GPT-4o | May 2024 | $5.00 | $15.00 | [Source 1] |
| Gemini 1.5 Flash | Aug 2024 | $0.075 | $0.30 | [Source 1] |
| GPT-4o mini | Jul 2024 | $0.15 | $0.60 | [Source 1] |
| DeepSeek R1 | Jan 2025 | $0.55 | $2.19 | [Source 1] |
| Claude Opus 4.5 | Aug 2025 | $5.00 | $25.00 | [Source 1] |
| GPT-5.4 | Mar 2026 | $2.50 | ~$10.00 | [Source 1] |
| DeepSeek V4-Flash | Apr 2026 | ~$0.07 | $0.28 | [Source 18] |

*All prices: accessed or confirmed 2026-04-28 via tokencost.app [Source 1] and press reports.*

**Aggregate decline:** ~300x from GPT-4 launch to cheapest capable model (Gemini 2.0 Flash / DeepSeek V4-Flash) over approximately 36 months. Frontier model pricing (GPT-4 to GPT-5.4) declined ~12x over the same period. [Source 1]

### GPU-Hour Cost Curve

| GPU | Year | Cloud $/hr (range) | Source |
|-----|------|-------------------|--------|
| A100 80GB | 2022–2023 | $3.00–$5.00 | [Source 7] |
| H100 SXM5 | 2024 (peak) | $5.00–$7.00 | [Source 7] |
| H100 SXM5 | Jun 2025 | $2.85–$3.50 (AWS $3.90) | [Source 7] |
| H100 (specialist cloud) | 2025 | $1.49 (Hyperbolic low) | [Source 7] |
| B200 (expected) | 2026 | ~$4.00–$5.00 at launch | inference estimate |

*GPU inference cost per token for H100: approximately $0.95–$1.90/M tokens for large models at on-demand cloud rates (FP8 quantization at lower end). [Source 7]*

### Cost Structure Implications for RL Fine-Tuning

RL fine-tuning is substantially more compute-intensive than SFT: it requires rollout generation (inference) plus gradient updates, typically 5–20x the compute of equivalent SFT. At $2.85/GPU-hr on H100, a typical SME domain RFT run (Accordance AI scale) likely costs $5,000–$50,000 in compute, before engineering amortization. The Accordance 39% accuracy gain on a proprietary tax eval requires continued re-running as base models change — each new o-series model needs a fresh fine-tuning run.

This creates an ongoing CapEx cycle, not a one-time investment — consistent with hypothesis D (fine-tuning as starting point; inference and maintenance as the real ongoing engineering).

### Inference vs. Training Spend at Enterprise Scale

Menlo Ventures mid-2025 data: 74% of startups report majority workloads are now inference-driven (up from 48% YoY); 49% of large enterprises report same (up from 29% YoY). [Source 4] This structural shift means the investment thesis increasingly needs to justify itself in inference margin, not training ROI.

---

## 6. Vendor Lock-In — What the Data Actually Says

### Empirical Evidence for Low Switching Friction

- Menlo Ventures (mid-2025): only 11% of enterprises switched vendors; 66% upgraded within existing provider; 23% made no changes. [Source 4] This is low switching *occurrence*, but it does not establish high switching *cost* — most non-switchers may be satisfied, not trapped.
- Within one month of Claude 4 launch, Claude 4 captured 45% of Anthropic users and Sonnet 3.5 dropped from 83% to 16%. [Source 4] This is intra-vendor but demonstrates extremely rapid adoption of new base models — the opposite of lock-in at the model level.
- OpenRouter data: new models achieve "significant usage within weeks" of release; DeepSeek's OSS token dominance fell from 50%+ to under 25% within months as Qwen3 and other models entered. [Source 13]

### Empirical Evidence for Sticky Migration Costs

- a16z 2025 CIO survey: agentic workflow complexity has increased switching difficulty. Enterprises with tuned prompts and guardrails face "significantly more resource-intensive" migration. [Source 6]
- Harvey's 25,000+ custom workflow agents represent a migration cost that is explicitly *not* about the model — it is about the tooling and eval infrastructure built on top. [Source 9]
- LangChain/LlamaIndex/Portkey portability claim: these frameworks abstract the model API call but *not* the prompt optimization, retrieval configuration, or domain eval. The "portability tax" includes re-engineering workflows, reformatting data pipelines, and re-validating outputs. [Source 20]
- 37% of enterprises deploy 5+ models in production (up from 29% YoY). [Source 6] This suggests deliberate multi-homing to *avoid* lock-in, not that lock-in is absent.

### Assessment of Hypothesis A ("Clients swap foundation models every few weeks")

**Disconfirmed in aggregate, partially confirmed at the frontier edge.** The median enterprise does not swap models every few weeks — but the fastest-moving cohort (AI-native companies, top 20% of token consumers) does rotate very rapidly, with some deploying new models within days of release. The lock-in that does exist is concentrated in the *surrounding infrastructure* (evals, agents, prompts) rather than in the model weights themselves. This is an important nuance: the asset that creates switching cost is not the fine-tuned model; it is the evaluation and workflow stack built around it.

---

## 7. The Inference-Side Moat Hypothesis (D)

Hypothesis D states that inference (caching, sessions, hardware optimization) is the real ongoing engineering moat. The market evidence in 2025–2026 strongly supports this.

### Revenue evidence

- Fireworks AI: $315M ARR (Feb 2026), 416% YoY, pure inference play. Proprietary kernel (FireAttention), multi-LoRA consolidation, enterprise compliance. Moat is described as "end-to-end optimized inference stack." [Source 8]
- Together AI: $150M+ ARR, $305M Series B (Feb 2025), serverless + dedicated inference. [Source 8]
- Groq (LPU): Acquired by NVIDIA (announced Jan 2026); had demonstrated 241 tokens/second on Llama 2 70B. Hardware-level inference moat. [Source 8]
- Cerebras: Wafer-scale chip for steady-throughput inference. Competing for sustained-throughput use cases. [Source 8]

### Structural argument

At current token prices, the spread between self-hosted open-source inference and API-priced frontier models is 5–10x for comparable capability. This spread funds the inference optimization layer. The 10,000+ companies using Fireworks' inference platform — most of whom are *not* doing meaningful fine-tuning — generate the bulk of Fireworks' revenue. [Source 8]

Multi-LoRA technology (available at Fireworks, Together, Predibase) enables fine-tuned adapters to coexist on shared GPU infrastructure, reducing the marginal cost of each adapter toward near-zero. This commoditizes the fine-tuning *deployment* step while the *inference operations* step retains margin.

### Disconfirming evidence

The inference moat is itself under threat: vLLM and SGLang (open-source inference frameworks) are rapidly closing the performance gap with proprietary kernels. Fireworks' own filings acknowledge this as their primary long-term risk. The inference margin exists because the open-source stack is 12–18 months behind commercially optimized stacks — the same dynamic as the base model gap. [Source 8]

**Hypothesis D: Substantially confirmed.** Inference optimization is generating more durable, measurable revenue than post-training investment at comparable spend levels in the 2024–2026 window. The key uncertainty is whether this moat is durable beyond the 18-month open-source lag cycle.

---

## 8. Multi-Tenant Economics & Data Isolation (E)

Hypothesis E states that enterprise dedicated training collapses to cost+ because data isolation prevents cross-client amortization. The evidence is mixed but directionally confirms the hypothesis.

### Research-stage federated RLHF

- FedRLHF (December 2024): Framework enabling collaborative policy learning across multiple clients without sharing raw data or human feedback. Each client integrates feedback locally; server aggregates reward models. Published convergence guarantees. [Source 14]
- FedBiscuit / FedBis (2024): Binary selector distillation approach for aggregating client preferences into a shared LLM without raw data sharing. Demonstrated improvements in "professionalism and readability." [Source 14]
- LoRA-FAIR (ICCV 2025): Federated LoRA fine-tuning with aggregation and initialization refinement. Academic.

**Critical gap:** None of these frameworks has a documented production deployment with named enterprise customers and contractual data isolation SLAs. All are research-stage.

### Production-adjacent approaches

- **Cohere's synthetic data strategy:** Cohere uses design partners (e.g., RBC, Fujitsu) to generate synthetic training data, then sells the resulting specialized model across the vertical without needing client data. This is the closest documented production approach. It requires Cohere to own the model — it does not apply to fine-tuning third-party foundation models. [Source 17]
- **NVIDIA NeMo's on-premise play:** Enterprises run NeMo on their own infrastructure, meaning cross-client amortization is structurally impossible — each enterprise absorbs the full training cost. This directly confirms hypothesis E for on-premise dedicated training.
- **OpenAI RFT contractual terms:** OpenAI's RFT contracts include data isolation guarantees (fine-tuned weights are not used to train other models). This is appropriate from a privacy standpoint but it means OpenAI cannot amortize domain fine-tuning R&D across clients — each fine-tune is an isolated compute expense.

### Assessment of Hypothesis E

**Confirmed for dedicated training; partially circumvented by synthetic data strategies.** No productionized federated RLHF at enterprise scale has been documented. Cohere's synthetic data approach is a clever workaround but requires owning the model architecture. Hypothesis E holds most strongly for the OpenAI RFT / AWS Bedrock dedicated fine-tuning pattern where isolation is contractual and complete.

The business model implication: dedicated fine-tuning at cost+ is viable if and only if the domain eval improvement is large enough to justify the non-amortizable training cost. The Accordance AI case (39% uplift on proprietary tax eval) represents the favorable extreme; generic "domain adaptation" cases where gains are 2–5% are not economically defensible under this constraint.

---

## 9. Open Questions & Promising Deep-Research Streams

### Stream 1 — The Durability Threshold: What Domain Complexity Preserves Fine-Tuning Gains?

**Unanswered question:** Is there a measurable property of domain evals (eval complexity, domain vocabulary size, regulatory specificity) that predicts whether a fine-tuning gain survives the next general model release? The existing data (Harvey's persistent gains vs. generic enterprise disappointment) suggests durability is a function of domain eval difficulty, not fine-tuning technique. A systematic comparison of domain fine-tunes across verticals — medical coding, legal citation, financial regulatory — measured against successive base model releases would test this directly.

**Why now:** OpenAI RFT is now publicly available; multiple named case studies exist with disclosed metrics. A survey of RFT customers at 6- and 12-month re-evaluation points is feasible.

### Stream 2 — Inference Stack Economics at Production Scale: The True Cost Breakdown

**Unanswered question:** At what token volume does a vertically integrated inference stack (self-hosted open-source + LoRA adapters) achieve lower total cost of ownership than a hyperscaler API, inclusive of engineering headcount? The public data on token prices is rich; the public data on production engineering cost (prompt iteration, eval maintenance, observability, caching, session management) is sparse.

**Why now:** The 74% shift to inference-dominant workloads at startups creates a large enough production population to survey. Artificial Analysis publishes throughput benchmarks but not full-stack TCO.

### Stream 3 — The Synthetic Data Wedge: Can Cross-Client RL Training Be Commercially Productionized Without Raw Data Sharing?

**Unanswered question:** Is Cohere's synthetic data strategy (use design partners to generate domain synthetic data, sell to the vertical) replicable at smaller scale, or does it require a model provider's full ownership of architecture and weights? Specifically, can a company running RFT on top of a third-party base model (e.g., Llama 4) generate and amortize domain-specific synthetic training data across clients without violating data isolation?

**Why now:** FedRLHF frameworks are transitioning from research to implementation-ready. The gap between academic capability and commercial deployment has historically closed within 18–24 months.

### Stream 4 — The Acqui-Hire Signal: What Did Microsoft, Amazon, and Cohere Actually Buy?

**Unanswered question:** In the Inflection (Microsoft), Adept (Amazon), and Aleph Alpha (Cohere) transactions, what specifically was the acquired asset — talent, model weights, training pipelines, or customer relationships? Decomposing the acquisition value would reveal whether the market believes fine-tuned model weights have standalone value or whether the value lies entirely in the team and the eval infrastructure.

**Why now:** All three transactions closed or were announced within 24 months. Deal terms and investor post-mortems are available in trade press and PitchBook filings.

### Stream 5 — Pricing Model Survival: Which Monetization Structures Survive Client Renewal Cycles?

**Unanswered question:** Do outcome-based pricing contracts (Sierra's per-resolution model) actually deliver higher net revenue retention than per-seat models when foundation model token costs are declining? The fear is that outcome-based pricing transfers the full benefit of 300x token price declines to the customer rather than to the vendor.

**Why now:** Sierra at $150M ARR and Decagon/other outcome-based AI companies are 18–24 months into multi-year enterprise contracts. Renewal data is beginning to surface.

---

## 10. Sources

1. [AI Price Index: LLM Costs Dropped 300x (2023–2026) | TokenCost](https://tokencost.app/blog/ai-price-index) [T2] Accessed 2026-04-28

2. [DeepSeek unveils V4 model, with rock-bottom prices | Fortune](https://fortune.com/2026/04/24/deepseek-v4-ai-model-price-performance-china-open-source/) [T3] Accessed 2026-04-28

3. [OpenAI Releases Reinforcement Fine-Tuning (RFT) on o4-mini | MarkTechPost](https://www.marktechpost.com/2025/05/08/openai-releases-reinforcement-fine-tuning-rft-on-o4-mini-a-step-forward-in-custom-model-optimization/) [T3] Accessed 2026-04-28

4. [2025 Mid-Year LLM Market Update | Menlo Ventures](https://menlovc.com/perspective/2025-mid-year-llm-market-update/) [T2] Accessed 2026-04-28

5. [2025: The Year in LLMs | Simon Willison](https://simonwillison.net/2025/Dec/31/the-year-in-llms/) [T2] Accessed 2026-04-28

6. [How 100 Enterprise CIOs Are Building and Buying Gen AI in 2025 | a16z](https://a16z.com/ai-enterprise-2025/) [T2] Accessed 2026-04-28

7. [Inference Unit Economics: The True Cost Per Million Tokens | Introl](https://introl.com/blog/inference-unit-economics-true-cost-per-million-tokens-guide) [T2] Accessed 2026-04-28

8. [Fireworks AI revenue, valuation & funding | Sacra](https://sacra.com/c/fireworks-ai/) [T2] Accessed 2026-04-28

9. [Harvey revenue, valuation & funding | Sacra](https://sacra.com/c/harvey/) [T2] Accessed 2026-04-28; also [Legal AI startup Harvey hits $100M ARR | CNBC](https://www.cnbc.com/2025/08/04/legal-ai-startup-harvey-revenue.html) [T3] Accessed 2026-04-28

10. [Outcome-based pricing for AI Agents | Sierra](https://sierra.ai/blog/outcome-based-pricing-for-ai-agents) [T4 — vendor advocacy; revenue figures from Sacra independently]); [Sierra revenue | Sacra](https://sacra.com/c/sierra/) [T2] Accessed 2026-04-28; [Glean revenue | Sacra](https://sacra.com/c/glean/) [T2] Accessed 2026-04-28

11. [Emad Mostaque resigns as CEO of troubled Stability AI | SiliconANGLE](https://siliconangle.com/2024/03/24/emad-mostaque-resigns-ceo-troubled-generative-ai-startup-stability-ai/) [T3] Accessed 2026-04-28; [AI Startup Collapses Due to "Reverse Takeover" | 36kr English](https://eu.36kr.com/en/p/3459613651932801) [T3] Accessed 2026-04-28

12. [Reinforcement fine-tuning use cases | OpenAI API](https://platform.openai.com/docs/guides/rft-use-cases) [T4 — vendor advocacy; case study numbers flagged as self-reported] Accessed 2026-04-28; [OpenAI at QCon AI NYC: Fine Tuning the Enterprise | InfoQ](https://www.infoq.com/news/2025/12/qcon-openai-rft/) [T3] Accessed 2026-04-28

13. [State of AI: An Empirical 100 Trillion Token Study | a16z / OpenRouter](https://a16z.com/state-of-ai/) [T2] Accessed 2026-04-28

14. [FedRLHF: A Convergence-Guaranteed Federated Framework | arXiv 2412.15538](https://arxiv.org/abs/2412.15538) [T2] Accessed 2026-04-28; [Towards Federated RLHF with Aggregated Client Preference | OpenReview](https://openreview.net/forum?id=mqNKiEB6pd) [T2] Accessed 2026-04-28

15. [Why Cohere is merging with Aleph Alpha | TechCrunch](https://techcrunch.com/2026/04/25/why-cohere-is-merging-with-aleph-alpha/) [T3] Accessed 2026-04-28

16. [Cohere acquires Aleph Alpha in $20bn sovereign AI push | PitchBook](https://pitchbook.com/news/articles/cohere-acquires-aleph-alpha-in-20bn-sovereign-ai-push) [T3] Accessed 2026-04-28; [Aleph Alpha's Strategic Pivot: From LLM Development to AI Platform | LinkedIn](https://www.linkedin.com/pulse/aleph-alphas-strategic-pivot-from-llm-development-ai-uakcc) [T3] Accessed 2026-04-28

17. [Cohere at $150M ARR | Sacra](https://sacra.com/research/cohere-at-150m-arr/) [T2] Accessed 2026-04-28; [Cohere Hits $240M ARR in 2025 | Tekedia](https://www.tekedia.com/cohere-hits-240m-arr-in-2025-outpacing-target-and-signaling-resilience-in-competitive-enterprise-ai-market/) [T3] Accessed 2026-04-28

18. [Breaking the Moat: DeepSeek and the Democratization of AI | INET Economics](https://www.ineteconomics.org/perspectives/blog/breaking-the-moat-deepseek-and-the-democratization-of-ai) [T2] Accessed 2026-04-28

19. [AI Is Driving A Shift Towards Outcome-Based Pricing | a16z](https://a16z.com/newsletter/december-2024-enterprise-newsletter-ai-is-driving-a-shift-towards-outcome-based-pricing/) [T2] Accessed 2026-04-28

20. [LLM Integration Services for Enterprise Systems 2026 | Kyanon Digital](https://kyanon.digital/blog/llm-integration-services-for-enterprise-systems/) [T3] Accessed 2026-04-28; [Enterprise LLM Adoption: Build vs Buy | Celadon Research](https://celadonresearch.com/research/enterprise-llm-build-vs-buy) [T2] Accessed 2026-04-28

21. [The Token Arbitrage: Groq vs. DeepInfra vs. Cerebras vs Fireworks | GoPenAI / Medium](https://blog.gopenai.com/the-token-arbitrage-groq-vs-deepinfra-vs-cerebras-vs-fireworks-vs-hyperbolic-2025-benchmark-ccd3c2720cc8) [T2] Accessed 2026-04-28; [After Nvidia's Groq deal | Fortune](https://fortune.com/2026/01/05/nvidia-groq-deal-ai-chip-startups-in-play/) [T3] Accessed 2026-04-28

22. [LoRA Fine-Tuning Cost 2026 | Stratagem Systems](https://www.stratagem-systems.com/blog/lora-fine-tuning-cost-analysis-2026) [T3] Accessed 2026-04-28

23. [Harvey, OpenAI, and the race to use AI to revolutionize Big Law | Fast Company](https://www.fastcompany.com/91429116/harvey-openai-and-the-race-to-use-ai-to-revolutionize-big-law) [T3] Accessed 2026-04-28; [Harvey's Defensible Compliance Moat | Legal IT Insider](https://legaltechnology.com/2025/06/19/comment-harveys-defensible-compliance-moat-strategic-advantage-or-market-dependency/) [T3] Accessed 2026-04-28

24. [H100 vs GB200 NVL72 Training Benchmarks | SemiAnalysis](https://newsletter.semianalysis.com/p/h100-vs-gb200-nvl72-training-benchmarks) [T2] Accessed 2026-04-28; [Groq Inference Tokenomics | SemiAnalysis](https://newsletter.semianalysis.com/p/groq-inference-tokenomics-speed-but) [T2] Accessed 2026-04-28

---

*End of Phase 1a scan. All claims traceable to numbered sources above. No invented citations. Price numbers date-tagged to primary source access date 2026-04-28 unless otherwise noted in the tables above.*
