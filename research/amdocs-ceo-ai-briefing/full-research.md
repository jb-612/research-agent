---
title: "AI/GenAI Landscape — A Depth Briefing for the Amdocs CEO"
subtitle: "Technology trends, instability, and architectural implications (2025–2026)"
version: 1.1
created: 2026-04-27
updated: 2026-04-27
audience: CEO, Amdocs (telco BSS/OSS software vendor)
revision_notes: "v1.1 — merged consulting-firm consensus (§12), Chinese challengers (§13), services-disruption thesis (§14), buyer evaluation framework (§16), and events/communities appendix from a complementary briefing scan."
companion_artifact: ./executive-brief.md
research_method: Multi-agent (5 parallel research agents — Frontier Models · Eval & Observability · Agent Standards & User Agency · Knowledge & Agentic Transformation · Compliance & Cross-cutting Strategy)
access_date: 2026-04-27
---

# AI/GenAI Landscape — A Depth Briefing for the Amdocs CEO

> **Working thesis.** The "AI hype" is not one phenomenon. It is eleven distinct technology trends, each on its own clock, each with its own instability profile, each with different architectural consequences. Treating them as a single wave produces shallow strategy. Treating them as a stack — model layer, eval layer, protocol layer, runtime layer, knowledge layer, agent layer, compliance layer — yields a defensible posture that survives the 6–12 month half-life of any single bet. The job is to identify what to bet on (the layers that compound advantage) and what to abstract away (the layers that commoditise).

---

## Executive summary

1. **The model layer is no longer one architecture.** Decoder-only transformers (GPT/Claude/Gemini family) now share the production frontier with sparse-activation MoE (DeepSeek-V3 at 5.5% active params [22], Qwen3-235B-A22B [16]), state-space hybrids (AI21 Jamba, IBM Granite 4.0, Mistral Codestral Mamba [25]), diffusion language models (Inception Mercury at 1109 tok/s [21]), latent-prediction encoders (V-JEPA 2 [9], LeJEPA [27]), and full-fidelity world models (DeepMind Genie 3 [6], NVIDIA Cosmos [11], Wayve GAIA-3 [13]). Architectural pluralism is real for the first time since 2017.

2. **Inference economics inverted.** Per-task inference price has fallen 9× to 900× per year by capability tier [3]; GPT-3.5-MMLU-equivalent dropped **280-fold** between Nov 2022 and Oct 2024 per Stanford AI Index [82]; DeepSeek-V3 trained at ~$5.6M against GPT-4's estimated $50–100M [22]. Any 2025-vintage cost model is 10× wrong by 2027.

3. **Evaluation is broken — and that is the strategic opening.** LLM-as-judge has documented position bias [33], self-preference / family bias [34], rating-roulette self-inconsistency [35], multilingual unreliability [37], and adversarial vulnerability. MAST (NeurIPS 2025) is the first empirical multi-agent failure taxonomy [38]. Public benchmarks (MMLU, GSM8K) are saturated; new ones turn over in weeks. The vendor that pairs domain ground truth with continuous online evaluation builds a flywheel hyperscalers cannot replicate.

4. **The protocol layer consolidated under the Linux Foundation in 9 months.** MCP donated to the **Agentic AI Foundation** (Dec 2025, founded by Anthropic + OpenAI + Block) [44][45]; A2A donated to LF (Jun 2025), absorbed IBM ACP in Aug 2025, hit v1.0 with 150+ orgs and 22k GitHub stars by April 2026 [46][47]; AG-UI is the third axis (agent↔user). MCP usage went from ~100k to 97M monthly SDK downloads in 18 months [48]. Stop building proprietary agent protocols.

5. **Observability and evaluation merged.** Datadog, Weave, Langfuse, Maxim, Phoenix all now ship eval-as-code on top of trace storage [40][41][42]. OpenTelemetry GenAI semconv is still in *Development* status but vendors emit ahead of stable [39]. Gateways are emerging as the de facto observability cut-point [43] — important for any platform vendor that sits between agents and systems of record.

6. **The semantic layer is the moat, not the model.** GraphRAG [55], LightRAG, HippoRAG [56], Neo4j Graphiti [57], Microsoft Fabric IQ Ontology [59], Palantir Ontology [60], FIBO [61], and TM Forum SID/AAIF are all converging on one insight: **domain-specific governed knowledge graphs are the durable enterprise asset**. Vector-only RAG is being explicitly downgraded in 2026 architectures.

7. **The "agentic OS" is a control plane over heterogeneous cores, not a replacement for them.** Across banking (JPMorgan/Vault [69], Mastercard Agent Pay [73], Block Goose [74]), GL (BlackLine, Trintech, FloQast, Workday Illuminate [70]), BSS (Amdocs aOS [71], Netcracker, Ericsson Telco Agentic AI Studio [72]), payments (Visa Intelligent Commerce [75], Stripe ACS [76]), and policy (Cedar/AgentCore [77], Duck Creek, Guidewire), the same five-component reference pattern is hardening: **policy gateway + MCP registry + Context Manager/KG + eval flywheel + HITL**.

8. **Computer use jumped from 14.9% to 79.6% on OSWorld in 18 months** [50] — the capability ceiling is no longer binding; reliability, latency, and prompt-injection are. Prompt injection is OWASP's #1 LLM risk in 2026, present in 73% of production AI deployments [54]. ShadowPrompt (Claude for Chrome, Dec 2025), Brave's Comet PoC, and "Claudy Day" all landed in the last quarter — security posture is now a product-level differentiator.

9. **Compliance is product, not paperwork.** EU AI Act high-risk obligations conditionally postponed to **Dec 2027 / Aug 2028** under the Digital Omnibus voted by the European Parliament in March 2026 [80]. ISO/IEC 42001 has 100+ certified organisations within 18 months of publication [81]; AWS, Anthropic, Microsoft, Google, IBM all certified. NYDFS Circular 7, DORA (live since Jan 17, 2025), Korea AI Basic Act (live Jan 2026), India IT Rules Amendment (Feb 20, 2026) are present obligations.

10. **The CEO call.** Bet on (a) the integration surface to systems of record, (b) the domain ontology / Context Manager, (c) the eval flywheel, (d) the compliance instrumentation, and (e) the policy gateway. Abstract (a) the specific model, (b) the framework, (c) the protocol implementation, and (d) the UX shell. The half-life of any specific model/framework/UX choice is 6–12 months; the half-life of a protocol commitment is 3–5 years; the half-life of a knowledge graph is the life of the business.

**Confidence**: High on commoditisation, protocol consolidation, eval-fragility, and regulatory timelines. Medium on agentic-core production maturity (vendor PR contamination is real). Medium on world-model commercial trajectory (capital signal is strong, replicable benchmarks are limited).

---

## 1. Edge models, SLMs, on-device AI

### 1.1 State of the art (2025–2026)

The on-device frontier is no longer a single model class but a tiered ecosystem of 0.3B–8B parameter models with hardware-specific compilation paths. Microsoft's **Phi-4-multimodal** (5.6B params, Feb 2025) unifies speech, vision, and text via mixture-of-LoRAs, supports 128K context, and topped HuggingFace OpenASR at 6.14% WER — beating Whisper V3 and SeamlessM4T-v2 specialists [1]. Microsoft followed with **Phi-4-reasoning-vision-15B**, a "knows-when-to-think" hybrid that gates chain-of-thought to suppress latency on easy tokens [2]. At platform level, Microsoft consolidated Windows Copilot Runtime into **Windows AI Foundry / Foundry Local** (May 2025), auto-detecting CPU/GPU/NPU and surfacing quantised variants through one SDK [3]; Phi Silica ships LoRA support in public preview on Snapdragon X NPUs.

**Apple's on-device Foundation Model** (AFM 2025 stack) is the most aggressive packaging exercise published. The shipped on-device model is ~3B params compressed to **2 bits-per-weight via QAT**, with 4-bit embedding tables, an 8-bit KV-cache, and low-rank adapters layered back on to recover quality. Apple split the transformer into Block 1 (62.5% of layers, full KV) and Block 2 (37.5%, no K/V projections) to slash Neural Engine memory traffic [4][5]. The companion Foundation Models framework (WWDC 2025) gives any iOS/macOS app guided generation against the same 3B model with no per-token cost. The server-side model in the same paper is a Parallel-Track MoE ("PT-MoE") with sliding-window plus global attention — Apple is **not running the same architecture on device and in cloud**, contradicting the "one model, two endpoints" model many architects still hold.

The open-weight tier has compressed the price/quality curve dramatically. By late 2025, **Qwen 3 4B** edges out Gemma 3 4B and Llama 3.2 3B on general benchmarks; **Qwen 3 0.6B** is the practical floor for sub-1B; Gemma 3 270M survives as the only realistic sub-300M for embedded silicon [6]. Qwen 3's MoE variant (235B total / 22B active, 128 experts / 8 active) is increasingly used in edge↔cloud cascades because the active-parameter footprint matches a single H100 or M3 Ultra [16]. **NVIDIA Nemotron 3 Nano 4B** (late 2025) is engineered around an explicit edge runtime — Jetson Thor, Jetson Orin Nano, DGX Spark — with a hybrid Mamba-attention backbone; the Rockwell Automation deployment (Nemotron Nano in FactoryTalk Design Studio) is the cleanest production-grade example of an edge SLM acting as a deterministic copilot [7][8].

The hybrid edge↔cloud routing layer has matured from research to architecture pattern. Surveys categorise collaboration into task-assignment, task-division, and mixture routing at request and token granularity; speculative decoding (edge drafts, cloud verifies) hits 60–80% draft acceptance for routine text and yields 2–3× cloud-side speedup with no quality loss. Reported field claims: 60–80% inference-cost reduction when 70–80% of requests are absorbed at the edge tier; hybrid pruning-then-quantization pipelines hit ~75% size and ~50% power reduction at ~97% accuracy retention [3][16].

### 1.2 Instability signals
- **Cadence**: Phi-3 → Phi-4 → Phi-4-mini → Phi-4-multimodal → Phi-4-reasoning-vision-15B inside 12 months. Qwen released 0.6B/1.7B/4B/8B/14B/32B dense plus 30B-A3B and 235B-A22B MoE in one drop, then iterated to Qwen3-Next and Qwen3.6-35B-A3B by April 2026 [16].
- **Capability shifts**: Models that dominated mid-2025 (Gemini 2.0 Flash, Gemini 2.5) have negligible market share by end-2025. GPT-4o, GPT-4, GPT-3.5 deprecated with the GPT-5.2 release.
- **Cost curves**: GPT-3-grade MMLU price fell from $60/M tokens (Nov 2021) to $0.06/M (Llama 3.2 3B) — three orders of magnitude in three years. Per-task price drops 9×–900× per year [3][82].
- **Standards gap**: No widely accepted SLM efficiency benchmark; vendor cherry-picking is the norm.

### 1.3 Architectural implications
- **Treat the on-device model as a substitutable cache, not a system component.** Compile-time hardware targeting (Phi Silica → NPU, AFM → ANE, Nemotron → Jetson) couples the artifact to silicon. Expose a thin capability contract (latency tier, modality, function-calling, structured output); let the runtime pick the model.
- **Build for cascades, not tiers.** Speculative decoding and confidence-cascading are the default cost-control primitive; hard-coded edge-vs-cloud paths fall behind the 60–80% cost gap that hybrid routers capture.
- **Quantization is a first-class deployment dimension.** 2-bit QAT with adapter recovery is now production-proven. Adapter management, quantization-aware fine-tuning, and per-tenant LoRA stores are core infrastructure.

---

## 2. World models

### 2.1 State of the art (2025–2026)

A "world model" in 2025–2026 is a learned simulator that predicts future observations conditioned on actions, with internal state coherent enough to support planning, counterfactual rollouts, or interactive control — distinct from text-conditioned video generators with no action interface. The frontier moved from clip-length prediction in 2024 to **interactive minute-scale rollouts** in 2025.

**DeepMind Genie 3** (announced Aug 2025; public release as Project Genie on 29 Jan 2026 to AI Ultra subscribers at $250/mo) generates 720p/24fps navigable 3D worlds in real time from a text prompt, retaining scene memory ~1 minute (up from Genie 2's ~10s) [9][10][11]. DeepMind framed it as a "stepping stone toward AGI" because the model self-consistently re-renders prior views without storing pixels — implying a learned latent world state.

**Meta V-JEPA 2** (Jun 2025, arXiv 2506.09985) is architecturally most distinctive: it pre-trains on >1M hours of internet video using **joint-embedding prediction in latent space — not pixel reconstruction** — then post-trains an action-conditioned variant (V-JEPA 2-AC) on <62 hours of unlabeled robot video from Droid. Deployed zero-shot on Franka arms in two unfamiliar labs, V-JEPA 2-AC achieves 65–80% pick-and-place success on novel objects with **~16 seconds per planning step versus ~4 minutes for Cosmos** — a 15× planning-latency improvement because prediction happens in embedding space [12][13]. This is the strongest published evidence that LeCun's bet on non-generative predictive architectures is paying off downstream.

**NVIDIA Cosmos** (CES Jan 2025; major release Dec 2025 with Cosmos Transfer 2.5) is the "physical AI" platform: 4–14B-param diffusion and AR WFMs, plus a 12B upsampler and 7B AR-optimised video decoder, under an open model license. Adopters: 1X, Agility Robotics, Figure AI, Skild AI, Waabi, XPENG, Uber [14][15]. Cosmos is more a data factory than a deployable inference target. **OpenAI Sora 2** (30 Sep 2025) advanced the "world simulator" narrative with measurably better physics — missed basketball shots now rebound off the backboard rather than teleporting — but documented limitations: object-count drift, gears/pulleys/linkages misbehave, physics degrades past 20–30s, and the model only generates near its training distribution [16][17].

**Wayve GAIA-2** (Mar 2025) is the most domain-specialised: multi-camera, ego-action-conditioned, surround-view generative WFM trained on UK/US/Germany driving data — used to synthesise rare safety-critical scenarios for AV stress-testing — followed by **GAIA-3** which extends from simulation to evaluation, scoring candidate driving policies against counterfactual rollouts [18][19][20]. **Decart MirageLSD** (Jul 2025; $100M raise at $3.1B valuation Aug 2025) achieves 20fps frame-by-frame live-stream diffusion at 768×432 with ~100ms per-frame latency — the closest thing to an "interactive video LLM" shipped in 2025 [21][22]. Runway shipped Gen-4 / GWM-1 with similar pivot from passive video gen to interactive worlds.

**The architectural fault line**: token-prediction LLMs model symbolic distributions; world models attempt to model latent dynamics. V-JEPA's embedding-space prediction (no decoder during planning) and Genie 3's persistent latent state are the clearest non-LLM bets. Sora and Cosmos still rely on diffusion in pixel/video-token space, which is why their planning loops are 1–2 orders of magnitude slower and long-horizon coherence degrades.

### 2.2 Instability signals
- **Cadence**: Genie 2 (Dec 2024) → Genie 3 (Aug 2025) → public release (Jan 2026); GAIA-1 → GAIA-2 (Mar 2025) → GAIA-3 (late 2025) — annual major version per programme.
- **Capability discontinuities**: V-JEPA 2-AC reduced robotic planning step time from ~4 min (Cosmos) to ~16 sec — 15× in one paper.
- **Vendor-claim vs replicable-evidence gap**: Sora's "world simulator" framing is the cleanest example of marketing outpacing benchmarks.
- **Capital signal**: Decart $3.1B valuation on a real-time world model thesis (Aug 2025) is the clearest market vote that this layer is now an investable category.

### 2.3 Architectural implications
- **World models are infrastructure for the data layer, not the inference layer.** For most enterprise platforms, the value is upstream: synthetic training data, counterfactual rollouts for testing autonomous decisions, simulation of network/system state.
- **Latent-space prediction (V-JEPA family) will dominate planning; pixel-space generation (diffusion WFMs) will dominate data synthesis.** Mix both as a hedge — but they are not interchangeable.
- **Action interfaces matter more than fidelity.** Sora's pixels are stunning and useless for planning; V-JEPA 2-AC's are rough and useful. Evaluate world models on action-conditioning and rollout latency, not visual quality.

---

## 3. Non-LLM GenAI architectures

### 3.1 State of the art (2025–2026)

The most consequential 2025 result in non-transformer language generation is **Inception Labs' Mercury** (Jun 2025), a commercial-scale **diffusion-based language model** trained to denoise multiple tokens in parallel rather than predict autoregressively. Mercury Coder Mini and Small report **1109 tok/s and 737 tok/s on a single H100 — up to 10× faster than speed-optimised AR LLMs** at comparable code-generation quality [23][24]. Diffusion's parallel refinement breaks the latency-per-token assumption that has defined LLM serving infrastructure. Whether Mercury generalises beyond code is the open question.

**State-space models (SSMs) — Mamba 2 family — moved from research to production** in 2025. AI21 scaled **Jamba 1.5/1.6 to 398B total / 94B active** params via a 1:7 attention-to-Mamba ratio, yielding 256K context on a single GPU with a ~9 GB KV cache (an order of magnitude smaller than pure-transformer equivalents) [25][26]. Mistral's **Codestral Mamba** beats CodeLlama 34B at code generation with **zero attention layers** — the first pure-SSM in production at competitive quality. **IBM Granite 4.0** (late 2025) is built on Mamba; Google ships RecurrentGemma; Falcon Mamba (TII) and **Zyphra Zamba 2** round out the open-weight tier. Zamba2-7B reports 25% faster TTFT, 20% higher tok/s, and lower memory than Llama 3-8B; Zamba2-mini fits in <700 MB at 4-bit [27]. SSMs win on **long context and constant-memory inference** — domains where the transformer's quadratic attention is the bottleneck.

**Mixture-of-Experts (MoE)** is now the default for any model claiming frontier performance per dollar. **DeepSeek-V3** (Dec 2024 / Jan 2025) is the canonical reference: 671B total params with 37B active per token, using Multi-head Latent Attention and an auxiliary-loss-free load-balancing strategy with multi-token prediction. Total reported training cost: **2.788M H800 GPU-hours, ~$5.6M** versus estimated $50–100M for GPT-4 [22][28]. **Qwen3-235B-A22B** matches the pattern (235B/22B, 128 experts, 8 active); **Qwen3.6-35B-A3B** (Apr 2026) extends MoE into the small-model tier with only 3B active params at vision-language quality competitive with much larger dense models [16]. **Active-parameter count is the new model-size primitive** — total params define memory footprint, active params define inference cost, and the two are now decoupled by 5–10×.

**Flow matching** has displaced classical DDPMs as the training objective of choice for SOTA image and video. SD 3.5 and FLUX both use DiT (Diffusion Transformer) backbones with flow-matching objectives. **Pyramidal Flow Matching** generates 5–10s videos at 768p/24fps within ~20.7K A100-hours; **Flowception** (late 2025) introduces temporally expansive flow matching that interleaves discrete frame insertion with continuous denoising to break the AR error-accumulation barrier in video [29][30]. Diffusion is also the preferred architecture for **protein structure (AlphaFold 3, RoseTTAFold All-Atom), molecule generation, and now code (Mercury)** — the unifying property is parallel multi-token refinement.

**JEPA / I-JEPA / V-JEPA** is the largest non-generative bet. The 2025 advance is **LeJEPA** (arXiv 2511.08544), the first provable theoretical foundation for JEPAs via Sketched Isotropic Gaussian Regularization (SIGReg), eliminating training heuristics that made earlier JEPAs fragile (single trade-off hyperparameter, linear time/memory, stable across architectures and domains) [31][32]. This converts a research bet into an engineering option. **Neuro-symbolic** systems re-entered the Gartner Hype Cycle as a Sample Vendor category in 2025 (Franz Inc. AllegroGraph cited), driven by hallucination control, regulatory traceability, and explainability requirements; reported deployments (Amazon Vulcan, Rufus) cite 15–23% reasoning-accuracy gains over neural-only baselines [62].

### 3.2 Instability signals
- **Architectural pluralism is real for the first time since 2017.** SSMs, diffusion LLMs, MoE transformers, JEPAs, and flow-matching DiTs are all shipping in production within the same 12-month window.
- **Mercury's 10× throughput** is the most disruptive single data point because it breaks the per-token serving cost model.
- **Active-parameter compression**: DeepSeek-V3 (5.5% active), Qwen3-235B-A22B (9.4%), Qwen3-Next 80B-A3B (3.75%) — converging on ~3–10% active for frontier MoE.
- **Hybrid is winning**: virtually every shipped non-LLM architecture is a hybrid — pure-architecture stories are research narratives, not production patterns.

### 3.3 Architectural implications
- **Stop assuming "the model" is decoder-only transformer with KV-cache.** Inference platforms need a model-shape abstraction that admits diffusion LLMs, SSMs, and MoE.
- **Active-parameter accounting is the right cost metric.** Budget by active params per token plus context-length memory, not total params.
- **Neuro-symbolic and JEPA are the likely substrates for governed enterprise reasoning.** When regulators ask "why did the model output this?" you cannot answer with a 671B-param softmax. Budget for neuro-symbolic and JEPA-style overlays as a control plane, not a model class.

---

## 4. LLM-as-a-Judge & autonomous evaluation

### 4.1 State of the art (2025–2026)

LLM-as-judge has moved from a 2023 curiosity (G-Eval, MT-Bench) to the de facto evaluation primitive for non-deterministic systems by 2026 — and in the same motion has revealed itself to be statistically fragile. The reference stack: **G-Eval** (CoT with weighted token probabilities), **MT-Bench** (multi-turn human-aligned judgments), **Arena-Hard / Arena-Hard-Auto** (LMSYS Berkeley pipeline; ~98.6% correlation with human preference rankings at ~$20/run, 3× MT-Bench's discriminative power), and the open evaluator family — **Prometheus 2** (7B and 8x7B Mixtral, Pearson 0.6–0.7 with GPT-4-1106) [33], **JudgeLM**, and Meta's **Self-Taught Evaluator** which lifted RewardBench from 75.4% to 88.7% over five iterations using *no* human-labeled preferences [34].

Two structural shifts dominate 2025–2026. First, **specialised, smaller judges** are displacing GPT-4-class generalists for ongoing evaluation traffic — Galileo's "Luna" line monitors 100% of production traffic at ~97% lower cost than frontier judges. Cost economics now drive architecture: a 1,000-sample weekly judge run on GPT-4o is ~$50/run / ~$2,600/yr; running it twice per ordering to control position bias **doubles** that floor. Second, the **"eval-as-code" stack consolidated**. **Inspect AI** from UK AISI is the most credible open framework: opinionated dataset → Task → Solver → Scorer primitives, sandboxed execution (Docker built-in, optional Kubernetes), Agent Bridge for OpenAI Agents SDK / LangChain / Pydantic AI, 200+ pre-built evals, and 2025 Bayesian GLMs for evaluator reliability [35][36]. HuggingFace's **lighteval** has effectively replaced lm-evaluation-harness inside HF and powers the public LLM Leaderboards; **OpenAI Evals** remains the reproducible code-first reference; vendor evals (Arize, Langfuse, LangSmith, Braintrust, Maxim, DeepEval) wrap these with traces and golden-set management.

The third shift is **benchmark obsolescence**. MMLU is saturated above 88% (GPT-5.3 Codex hits 93%); GSM8K is at 99% — both useless for top-tier discrimination [37]. The community response — Arena-Hard / BenchBuilder (continuously curated from Chatbot Arena and WildChat-1M), re-MMLU adversarial encoding, weighted-metric "revitalisation" — has not stabilised the leaderboard cycle. A new public benchmark now typically saturates within weeks of a frontier release. **There is no fixed yardstick.**

### 4.2 Known failure modes & open problems

- **Position bias** — empirically the largest single distortion. Shi et al. (IJCNLP 2025) studied 15 judges across ~150k instances: position bias varies sharply by judge and task, is not random noise, weakly modulated by component length, strongly amplified when quality gaps are small [38]. Mitigation requires double-ordering A/B and B/A — non-negotiable per practitioner consensus — at 2× cost.
- **Self-preference and family bias** — EMNLP 2025's "Measuring Self-Preference in LLM Judgments" formalises that the bias is rooted in *perplexity preference* — judges over-reward outputs familiar to themselves; "family bias" (same-vendor preference) is real and survives prompt-level controls [39].
- **Verbosity / length bias** — judges systematically reward longer answers as a quality proxy.
- **Self-inconsistency ("rating roulette")** — EMNLP 2025 Findings showed substantial variance when the same judge scores the same item repeatedly. A single-pass judge score is not a measurement; it is a sample. Production stacks now require k-pass sampling with confidence intervals [40].
- **Scoring-template brittleness** — rubric ordering, score-ID labels, and reference-score placement materially shift outputs.
- **Multilingual unreliability** — judges' inter-language agreement is materially worse than within-language [41].
- **Adversarial vulnerability** — stylistic injection (markdown, confident hedges) flips judgments.
- **Multi-agent failure invisibility — MAST (NeurIPS 2025)** — Cemri, Pan, Yang et al. built the first empirically grounded multi-agent failure taxonomy from 150+ MAS execution traces (avg. 15k lines) across 7 frameworks, with 1,600+ annotated traces. **14 failure modes in 3 buckets: system-design, inter-agent misalignment, task-verification.** The MAST team's own LLM-as-judge annotator achieves κ=0.88 with humans — but only after deliberate co-design with the taxonomy [42][43].

### 4.3 Self-improvement loops & flywheel dynamics

- **Constitutional AI / RLAIF** (Anthropic, 2022→) — principled basis for AI feedback alignment; 2025 critiques flag fragile in-the-loop judge calibration, value-automation limits, over-refusal as models scale [44].
- **Self-Rewarding LMs** (Meta, arXiv 2401.10020) and **Meta-Rewarding** (arXiv 2407.19594, EMNLP 2025): the model plays actor + judge + meta-judge; on Llama-3-8B-Instruct, win-rate rose 22.9% → 39.4% on AlpacaEval 2 and 20.6% → 29.1% on Arena-Hard *without human labels* [45].
- **Self-Taught Evaluator** (Meta): synthetic-data-only judge training; lifted RewardBench 75.4% → 88.7% in five iterations [34].
- **The eval flywheel in practice**: trace → tag → curate golden set → run code-evals (cheap, every commit) → run judge-evals (costly, scheduled) → human spot-audit (~100 per release) → feed disagreements back into the judge's rubric. The OpenAI cookbook's "resilient prompts via evaluation flywheel" formalises this loop [46]. Code-evals are nearly free per run and should gate every commit; judge-evals are sampled; human eval is a tiny calibration anchor — *not* the main signal.
- **Cost economics force tiered architecture**: a distilled small judge sampling 100% of production; a frontier judge sampling 1–5% on flagged spans; humans calibrating <0.1%. Single-judge architectures do not survive enterprise volumes.

### 4.4 Architectural implications

- **Judge as a versioned artifact, not a prompt.** Judges drift, models behind them deprecate quarterly, prompt rubrics distort scores. Treat the judge bundle (model + rubric + sampling-policy + bias-controls) as a first-class versioned dependency.
- **Tiered judge fleet, not a single judge.** Distilled small judge at 100% sampling; frontier judge at 1–5% sampling on flagged spans; human raters at <0.1% calibration.
- **Evaluation must be code-first AND reproducible.** Inspect AI's Task/Solver/Scorer is the right shape. Vendor lock-in into a closed eval UI is a strategic liability.

---

## 5. AI / Agent observability

### 5.1 State of the art (2025–2026)

Agent observability in 2025–2026 has crystallised around three axes: a wire-format standard (OpenTelemetry GenAI semantic conventions), a richer AI-specific overlay (OpenInference, by Arize), and a fast-fragmenting vendor layer. The fundamental shift from 2024: **traces — not logs, not metrics — are the primary signal** for non-deterministic systems. Spans now carry multi-turn message arrays, system prompts, tool definitions, multimodal payloads, prompt/completion/cached/reasoning token counts as first-class fields, tool-call arguments and results, and links across MCP and A2A boundaries.

OpenTelemetry's GenAI conventions define spans (model + agent), metrics, and events for both client and tech-specific systems (Anthropic, Azure AI Inference, AWS Bedrock, OpenAI). As of April 2026 the spec is still in **Development** status — *not yet stable* — but Datadog began native support in OTel v1.37 and Grafana is collecting LLM traces in Loki, so production adoption has run ahead of formal stability [47][48]. The `OTEL_SEMCONV_STABILITY_OPT_IN` env var enables dual-emission to bridge the transition.

The **agent-specific tracing problem is qualitatively harder than LLM tracing**. A single user request now fans out across (a) one or more model calls, (b) one or more MCP tool calls (each with its own JSON-RPC client/server hop), (c) sub-agent delegations over A2A, (d) RAG retrieval spans, and (e) eval-in-production scoring spans. Langfuse's MCP tracing implementation captures session init, tool-listing, and each call_tool span linked back to the LLM trace that selected it [49]. Datadog's MCP client monitoring does the same at the proxy boundary [50]. The OTel community has an open proposal to bake OTel tracing directly into MCP — without it, MCP is a "black box" inside agentic flows. **A2A observability is even less mature; the consensus from the AAIF MCP Dev Summit (April 2026) is that gateways are becoming the de facto observability cut-point** — Uber, Amazon, Docker, Kong and Solo.io all converged on "centralized gateway + registry" pattern [51][52].

**Eval-in-production has fused with observability** — the single biggest 2025→2026 change. Datadog's June 2025 release added AI Agent Monitoring, LLM Experiments, and the AI Agents Console — explicitly framing experiments as something you run *against datasets created from real production traces*, closing the trace→eval→improvement loop inside the observability tool itself [53]. W&B Weave's 2026 build added Trace Analytics with P50/P95/P99 latency, token-by-model usage, cost breakdowns, scoring across all operations [54]. Maxim AI markets sampled continuous evaluation of production traffic. **The category line between "observability platform" and "evaluation platform" no longer exists in 2026.**

**Drift detection** has matured from research idea to operational requirement. The November 2025 paper on cross-provider LLM output drift in financial workflows formalises the attestation problem — if a provider silently updates a model behind a stable name, your traces are the only evidence of behavioral change [55]. That is regulatory exposure for any vendor with telco or finance customers.

**Cost attribution** is now table stakes. Datadog auto-prices 800+ models from public price lists; Weave provides token-and-cost dashboards out of the box; Helicone's proxy model captures cost trivially but only sees HTTP traffic. For multi-tenant SaaS, *per-tenant* cost attribution across nested agent calls requires propagating tenancy context as baggage across MCP/A2A hops — not standardised in OTel-GenAI WG yet.

### 5.2 Standards landscape

- **OTel GenAI semantic conventions** [47]: spans (model + agent), metrics, events; tech-specific conventions for Anthropic, Azure AI Inference, AWS Bedrock, OpenAI. Status: **Development / experimental** as of April 2026. Vendors emit ahead of stable.
- **OpenInference (Arize)** [56]: semantic-convention overlay specifically for LLM/agent observability, fully OTLP-compatible. Defines span-kinds (LLM, CHAIN, RETRIEVER, RERANKER, EMBEDDING, TOOL, AGENT). As of Sept 2025 there is an open issue to add OTel GenAI compatibility — convergence is incomplete.
- **MCP + OTel proposal**: open proposal to add OTel tracing inside MCP itself, addressing the "black box" problem.
- **AAIF (Linux Foundation, December 2025)** [57][58]: Agentic AI Foundation, founded by Anthropic, OpenAI, Block; platinum members include AWS, Bloomberg, Cloudflare, Google, Microsoft. Hosts MCP, goose, AGENTS.md. Companion projects Agentgateway and AGNTCY explicitly cover security, guardrails, observability, governance.
- **OTel-GenAI WG**: actively defining agent-spans separately from model-spans — explicit recognition that the agent layer is not just LLM-with-tools.

### 5.3 Vendor landscape & differentiation

| Vendor | Posture | Strengths | Caveats (2026) |
|---|---|---|---|
| **Datadog LLM Obs** | APM + LLM, agent-aware | End-to-end agent traces; 800+ model auto-cost; experiments from real traces; MCP client monitoring; SDS PII redaction. Native OTel v1.37+ GenAI semconv. | Enterprise pricing; new pricing effective May 2026; vendor-PR contamination on agent-AI claims. |
| **OpenInference + Arize Phoenix** | Source-available, OTel-overlay | Defines AI-specific attribute schema; Phoenix self-hostable; UMAP/embedding-drift visualisation; Arize $70M Series C (Feb 2025). | OTel GenAI vs OpenInference convergence still open. |
| **Langfuse** | OSS, self-hostable, full-feature parity | MCP tracing; prompt management; transparent usage pricing. | Fewer enterprise integrations than Datadog. |
| **LangSmith** | LangChain-native | Best for LangChain/LangGraph; framework-internal debugging. | $39/seat/mo before traces — bad for large orgs; framework lock-in. |
| **W&B Weave** | OSS lib + W&B platform | `@weave.op()` decorator; trace analytics with P50/P95/P99, cost-by-model; scoring across all ops; AWS Marketplace AI Agents listing. | Best for teams already on W&B. |
| **Helicone** | Proxy-based | One-line install; fastest setup; auto cost. | Proxy-only — no agent span linkage. |
| **Honeycomb** | High-cardinality tracing | Tokens/latency/model-IDs/ratings as queryable spans; Honeycomb MCP server lets coding agents query observability data. | Generalist platform; less AI-specific UI. |
| **Maxim / Galileo / Confident / Braintrust** | Eval-first, observability-attached | Continuous prod-traffic eval; distilled-judge tiers. | Vendor-PR-heavy; comparative tables self-published. |

### 5.4 Architectural implications

- **Adopt OTel GenAI as the wire format and OpenInference as the AI overlay**, not the reverse. The GenAI spec will stabilise; OpenInference is the most credible AI-specific attribute layer to maintain until the WG closes the gap.
- **Gateway is the natural observability cut-point.** AAIF's gateway-and-registry consensus matches enterprise reality: PII redaction, tenancy context propagation, cost attribution, MCP/A2A span linking, and policy enforcement are easier at the gateway than in every agent.
- **Eval-in-production, drift detection, and tracing are one platform, not three.** Plan for trace volume to dominate observability cost — sample aggressively and tier sampling rates.

---

## 6. Agent-to-Agent (A2A) standards & agent interoperability

### 6.1 State of the art (2025–2026)

The agent-protocol layer crystallised around three primary standards in 2025–2026, and the consolidation moment arrived on **December 9, 2025**, when the Linux Foundation announced the **Agentic AI Foundation (AAIF)** as a dedicated, neutral home for agent interop standards — distinct from the broader LF AI & Data foundation. AAIF launched with three founding code contributions: **Anthropic's Model Context Protocol (MCP)**, **Block's goose** (open-source agent runtime, MCP reference implementation since early 2025), and **OpenAI's AGENTS.md** (markdown convention for project-level agent guidance). Platinum members at launch: AWS, Anthropic, Block, Bloomberg, Cloudflare, Google, Microsoft, OpenAI; by April 2026 AAIF had grown to **170+ member organisations in under four months** [57][58][59]. The strategic signal: the three companies that defined the agent product category in 2024–2025 — Anthropic, OpenAI, Block/Square — chose neutral governance over proprietary lock-in, a decisive contrast to the cloud-API wars of the prior decade.

In parallel, **Google's Agent2Agent (A2A) protocol**, originally announced April 2025 with 50+ partners, was donated to the Linux Foundation on **June 23, 2025**, with founding members AWS, Cisco, Google, Microsoft, Salesforce, SAP, ServiceNow [60]. At its one-year anniversary (April 2026), A2A reported **150+ supporting organisations**, **22,000+ GitHub stars**, **five production-ready SDKs** (Python, JavaScript, Java, Go, .NET), and version **1.0** with multi-protocol support, enterprise multi-tenancy, signed Agent Cards (cryptographic identity), and migration paths from preview [61]. **IBM's ACP** — initially launched March 2025 with the BeeAI platform and donated to LF mid-2025 — formally **merged into A2A in August 2025**; once-fragmented protocol space collapsed to two complementary primitives [62]. The mental model that has stabilised: **MCP for agent↔tool**, **A2A for agent↔agent**, **AG-UI for agent↔frontend**.

**MCP's adoption curve is the most aggressive of the three.** Independent census data (Nerq, Q1 2026) indexed **17,468 MCP servers**; PulseMCP tracking went from 425 servers in late August 2025 to 1,412 by February 2026 (232% jump in six months). Anthropic reported **97 million monthly SDK downloads by March 2026**, up from ~100,000 at launch — roughly **970× in 18 months** — and **67% of enterprise AI teams using or evaluating MCP**, with named Fortune 500 deployments at Block, Bloomberg, Amazon, Pinterest [48][63]. The **2025-06-18 spec revision** was a watershed: it replaced original SSE transport with **Streamable HTTP** (single endpoint, stateless-capable, load-balancer-friendly) and embedded a subset of **OAuth 2.1** for authorization with PKCE, dynamic client registration, and **mandatory Resource Indicators (RFC 8707)** to prevent token-confusion attacks. By 2026 the recommended posture for new builders is "Streamable HTTP + OAuth 2.1 + Token Exchange" for any cross-org tool surface [64].

The competing/adjacent protocols matter less than they did six months ago. **AGNTCY** — Cisco/Outshift's "Internet of Agents" stack — was donated to LF mid-2025 and has 75+ contributing organisations; it focuses on identity (DIDs), the **SLIM** messaging layer, and a layered semantic networking model. Production use exists at SoftServe/Webex (multi-agent voice) and at **Swisscom** for telco network-migration validation [65]. **Agent Network Protocol (ANP)** is a more academic, W3C-aligned proposal with a `did:wba` decentralised-identity scheme; adoption signals lag MCP/A2A by an order of magnitude [66]. **AG-UI**, stewarded by CopilotKit, is the agent-to-frontend event protocol now adopted by Google, LangChain, AWS, Microsoft, Mastra, PydanticAI [67].

The framework layer (LangGraph, CrewAI, AutoGen, Microsoft Agent Framework, OpenAI Agents SDK, Pydantic AI, Mastra) is fragmenting *upward* — every framework now consumes MCP and exposes A2A — meaning **the protocols have effectively pushed framework choice down to a "code-style" decision rather than an architectural lock-in**. LangGraph is consolidating the enterprise tier; CrewAI owns the rapid-prototype tier; AutoGen/AG2 is conversationally elegant but ~5–6× more expensive in tokens for equivalent reasoning [68].

### 6.2 Protocol landscape comparison

| Protocol | Owner / Governance | Scope | Adoption signal (2026) | Status |
|---|---|---|---|---|
| **MCP** | Anthropic → AAIF (LF, Dec 2025) | Agent ↔ tool/data | 17k+ servers, 97M monthly SDK downloads, 67% enterprise eval | Production; OAuth 2.1 + Streamable HTTP |
| **A2A** | Google → LF (Jun 2025) | Agent ↔ agent (cross-vendor) | 150+ orgs, 22k+ GitHub stars, 5 SDKs, v1.0 GA | Production in Azure AI Foundry, Bedrock AgentCore, Google Cloud |
| **ACP** (IBM) | IBM/BeeAI → LF, **merged into A2A Aug 2025** | Agent ↔ agent | Subsumed | Deprecated as standalone |
| **AGNTCY** | Cisco/Outshift → LF (mid-2025) | Internet-of-Agents (identity, messaging, semantic routing) | 75+ orgs; SLIM/identity in Webex, Swisscom production | Component-mature; identity still evolving |
| **ANP** | Independent / W3C-aligned | Decentralised agent network with DID-based trust | Spec-stage; limited production | Alpha |
| **AG-UI** | CopilotKit → open spec | Agent ↔ frontend (generative UI events) | Adopted by Google, LangChain, AWS, Microsoft, PydanticAI | Production |
| **AGENTS.md** | OpenAI → AAIF | Coding-agent project conventions | 60k+ open-source projects; Copilot, Cursor, VS Code, Gemini CLI support | De facto standard |
| **goose** | Block → AAIF | Open-source agent runtime, MCP reference impl | 29k+ GitHub stars; Claude/GPT/Gemini/Ollama | Production runtime |

### 6.3 Consolidation trajectory

- **Two-protocol equilibrium**: MCP (vertical, agent↔tool) + A2A (horizontal, agent↔agent), with AG-UI as the third axis. ACP folded into A2A; ANP remains niche.
- **Neutral-governance race won**: Within ~9 months (Apr 2025 → Dec 2025), every major frontier-lab protocol moved under the Linux Foundation. Proprietary agent protocols are now strategically untenable.
- **AAIF is the new center of gravity**: Anthropic + OpenAI + Block co-founding a single foundation distinct from LF AI & Data implies the agent layer is being treated as separate infrastructure from the model/training layer.
- **Identity is the unfinished problem**: All protocols still defer cross-org identity to OAuth/DID/Agent Cards in early form. This is where the next 18 months of standards work will concentrate.
- **Framework layer is being abstracted away**: LangGraph/CrewAI/AutoGen converging on common MCP-tool and A2A-skill semantics. **Framework choice is becoming reversible; protocol choice is becoming permanent.**

### 6.4 Architectural implications

- **Bet on protocols, not frameworks.** Treat MCP servers as the durable interface to data/capability surface; A2A endpoints as the durable interface to agentic services; AG-UI as the durable surface for any generative-UI shell. The framework that orchestrates these (LangGraph today, something else in 18 months) is a swappable dependency.
- **Authorization is the integration boundary.** With MCP standardising on OAuth 2.1 + RFC 8707 Resource Indicators and A2A v1.0 introducing signed Agent Cards, **the enterprise IAM/OAuth posture is now the agent-platform posture**. BSS/OSS vendors integrating with telco identity should expose every capability as an MCP server with a properly-scoped OAuth resource.
- **Plan for protocol churn at the edge, stability at the core.** MCP spec changed quarterly (2025-03 → 2025-06 transport replacement broke most servers); A2A v1.0 just stabilised in 2026. Treat the protocol adapter as a thin layer that can be re-implemented in a sprint, and keep business logic protocol-agnostic.

---

## 7. User agency extension — co-work, ambient agents, computer use

### 7.1 State of the art (2025–2026)

The "computer-use" frontier moved from research preview to general availability in 18 months, with a distinct bifurcation: **autonomous agents** (long-horizon, headless) vs **co-work agents** (human-in-loop-by-default, shoulder-to-shoulder).

On the autonomous side, **OpenAI Operator** was rolled into ChatGPT as **ChatGPT Agent Mode** in **July 2025** and now ships as a first-class mode inside **ChatGPT Atlas**, OpenAI's macOS-first browser launched late 2025 [69]. **Google's Project Mariner** (Gemini 2.0) achieves **83.5% on WebVoyager**, runs cloud-side, handles up to 10 simultaneous tasks, added "Teach & Repeat" macro-recording at I/O 2025; Mariner Studio, cross-device sync, marketplace roadmapped 2026 Q2–Q4 [70]. **Anthropic Computer Use** went from a 14.9% screenshot-only OSWorld score for Claude 3.5 Sonnet (Oct 2024) to **Claude Mythos Preview at 79.6% on OSWorld-Verified by April 2026** — the leaderboard top three (Claude Mythos, Holo3-122B-A10B, GPT-5.5) cluster within 0.9 points and the benchmark approaches saturation versus an estimated **72% human baseline** [71][72]. **The capability ceiling is no longer the binding constraint; latency, reliability, and prompt-injection surface area are.**

The **co-work paradigm** is where the product money is flowing. **Microsoft Copilot's "Cowork" / Work IQ** went GA in April 2026 across Word, Excel, and PowerPoint with multi-step app-native actions and visible progress steering [73]. **Anthropic Claude Cowork** launched January 2026 — a non-developer-friendly agent shell with shell, filesystem, and browser access, a community-skill ecosystem, and a "watch me work" UI built in ~10 days by a four-person team using Claude Code itself. **Claude for Chrome** shipped as a research preview (and was hardened post-ShadowPrompt — see §7.3). **Cursor agentic mode**, **Replit Agent 3** (200-minute autonomous trajectories with self-healing browser-test loops, late 2025) and **Replit Agent 4** (early 2026, "product development OS") ship long-running coding agents [74][75]. **Perplexity Comet**, launched July 2025 on Chromium, embeds Comet Agent powered by Claude Sonnet 4.6 (Pro) and Opus 4.6 (Max) [76]. **Genspark, Fellou, Brave Leo** complete the agentic-browser cohort.

**Memory and ambience are the second axis.** **Anthropic Memory** went default-on for all Claude users on **March 3, 2026**, and includes a memory **import** path from ChatGPT, Gemini, Perplexity, Grok — Anthropic is the first major vendor to make memory portable across platforms [77]. **Persistent memory in Claude Managed Agents** went public beta on **April 23, 2026**, storing memories as files exportable via API [78]. **Letta (formerly MemGPT)** ships an "LLM-as-OS" architecture with three memory tiers (core, archival, recall) where the agent self-edits memory through tool calls [79]. **LangChain LangMem** (early 2025) standardised episodic/semantic/procedural memory primitives and popularised the **"ambient agent"** label for always-on, event-triggered agents.

**The UX shift in 2026 is multimodal-and-screen-grounded by default.** **Apple Intelligence** is the most aggressive bet: the Siri overhaul targeted for **March 2026 alongside iOS 26.4** introduces **on-screen awareness** plus cross-app actions, with the next Apple Foundation Model leveraging Gemini and Google cloud [80]. Microsoft's **Copilot Vision/Actions** in Edge gives the agent a coloured hue around the active tab so the user retains visual situational awareness during automation. The accessibility implication is large and underappreciated: screen-grounded agents that can describe and act on visible UI are functionally a new generation of assistive technology.

### 7.2 Capability vs reliability gap

- **OSWorld scores cluster ~79–80% but step efficiency is bad**: top agents take **1.4–2.7× more steps than necessary**, and **each successive step takes ~3× longer than the first** as context grows. A demo task that "works" still costs 10–30× more compute than a human equivalent.
- **Long-horizon trajectories collapse**: Replit Agent 3 markets 200-minute autonomy, but reviewers explicitly say the sandbox is for prototyping — "browser-only architecture, compute limits, and vendor lock-in make it unsuitable for production."
- **Computer-use is launched as research preview, not GA, at most vendors.** Anthropic still labels Computer Use research preview in 2026 to limit liability while gathering safety data.
- **Tool errors dominate failure modes**: Opus 4.7 reported a 10–15% lift in task success vs 4.6 for Factory Droids, attributed primarily to fewer tool errors and better validation follow-through — the model is not the bottleneck; the tool-binding layer is.
- **Reliability for memory is unproven**: Letta and LangMem are architecturally elegant but have no public SLAs. Memory poisoning is now an OWASP-tracked risk class.
- **Demo vs production gap is a quarter-by-quarter moving target.** CEOs evaluating capability should discount demo videos by two model generations.

### 7.3 Prompt injection / security surface

- **Prompt injection is OWASP's #1 LLM risk in 2026**, present in **73% of production AI deployments** [81].
- **ShadowPrompt** (Claude for Chrome, disclosed Dec 27, 2025; patched in v1.0.41 Feb 2026): zero-click chained XSS + over-permissive origin allowlist (`*.claude.ai`) in an Arkose CAPTCHA on `a-cdn.claude.ai`. Any attacker-controlled subdomain could silently inject prompts as if the user typed them [82][83].
- **Brave's Comet indirect-prompt-injection PoC**: Comet feeds raw page content to its LLM without trust-tier separation. Researchers chained hidden white-text instructions → Perplexity OTP via domain-trailing-dot trick → Gmail OTP read → exfil to Reddit, demonstrating account takeover. Perplexity's first patch was incomplete [84].
- **"Claudy Day" (Oasis Security)**: three chained vulnerabilities in `claude.ai` / `claude.com` enabling targeted-victim delivery → invisible prompt manipulation → silent conversation-history exfiltration [85].
- **Comet→phishing in <4 minutes**: researchers used GAN-modulated traffic to compromise Comet on a real session.
- **OWASP Q1 2026 GenAI Exploit Round-up**: large-scale indirect-prompt-injection attacks documented in the wild (Unit 42); **Flowise CustomMCP** flaw enabled JavaScript injection → arbitrary code execution across thousands of MCP servers; **Trivy/LiteLLM** supply-chain compromise (Feb–Mar 2026) cascaded credential theft into the agent ecosystem [86].
- **Mitigation state**: Anthropic publicly cites a **~1% attack success rate** against Claude 4.5 in browser-extension config (down from preview baselines) using a triple defense — RL-trained refusal, classifier scanning of untrusted context, continuous red-teaming — but explicitly states "**No browser agent is immune to prompt injection**" [87]. 1% × every web page = a non-trivial enterprise threat surface.

### 7.4 Architectural implications

- **Untrusted-content boundary is now load-bearing.** Any agent that consumes web pages, emails, support tickets, or partner-supplied data needs a hard "trust tier" boundary in the context-assembly stage, plus output-side scrutiny (no autonomous credential use without explicit user re-auth on sensitive domains).
- **Memory portability changes data-residency posture.** With Anthropic shipping memory import/export, the locus of "user state" is moving out of the application database and into the model-vendor's memory layer. Enterprise platforms must decide whether to (a) own memory themselves (Letta/Mem0/LangMem in-house), (b) integrate with vendor memory and accept lock-in, or (c) provide a memory-broker layer.
- **The "agent shell" is a new platform tier.** Atlas, Comet, Copilot Vision, Claude for Chrome, Apple's Siri-with-on-screen-awareness, and ChatGPT Cowork are all competing to be the *default agentic surface*. Enterprise software that previously assumed "user opens our web app" must now assume "user delegates a task to an agent that may render our app, or may not." Generative-UI / AG-UI surfaces and MCP-exposed capabilities are how an enterprise app participates in a session it does not host.

---

## 8. Knowledge Graphs, Ontologies, Context Managers

### 8.1 State of the art (2025–2026)

The KG+LLM revival has moved from research to production. Three forces drove this. First, vector-only RAG hit a wall on relational, multi-hop, and compliance-critical queries — Microsoft Research's GraphRAG benchmarks reported ~86% accuracy versus ~32% for baseline vector RAG on enterprise tasks, with the January 2025 Dynamic Community Selection update reducing token usage 79% while preserving answer quality [88]. Second, Gartner's 2025 Hype Cycle elevated *neuro-symbolic AI* into the spotlight as generative AI slid into the Trough of Disillusionment [89]. Third, the major hyperscalers shipped first-party ontology/semantic-layer products in late 2025–2026, signalling that **the semantic layer — not the model — is the next platform battleground**.

**The graph-RAG family has fragmented into specialised variants**: (1) Microsoft GraphRAG and LazyGraphRAG (community-summary-based, expensive at index time, high accuracy, embedded in Microsoft Discovery); (2) LightRAG (dual-level retrieval with merged neighbour subgraphs, ~20–30 ms faster, ~10% accuracy lift on relational benchmarks); (3) **HippoRAG and HippoRAG 2** (NeurIPS'24, OSU-NLP) — neurobiologically inspired, using Personalized PageRank over a KG, **10–20× cheaper and 6–13× faster than iterative retrievers like IRCoT**, with up to 20 points improvement on multi-hop QA [90]; (4) PathRAG and OG-RAG, the latter reporting +55% fact recall via ontology-anchored hypergraph retrieval; (5) **Neo4j Graphiti** — temporally-aware KG engine for incremental, real-time agent memory, productised by Zep AI as a memory layer [91].

**Ontologies have come back from the dead.** Microsoft **Fabric IQ Ontology** (preview) shipped late 2025 with billing meters scheduled for H1 2026, explicitly positioning itself as "the operational context that powers your AI agents," exposed via public MCP endpoints [92][93]. **Palantir's AIP** doubled down on its Ontology as "the agent's persistent, queryable memory system" — not the LLM — at the centre of a 12-layer agentic architecture, formalising four memory types (working, episodic, semantic, procedural) [94]. **FIBO's January 2026 normative release** contained 3,173 entities, and FIBO terms are now part of schema.org core v3.0 [95]. In telco, **TM Forum's AI-Native Blueprint**, including the *Agentic Interactions Security* project and AAIF (LF, December 2025), is delivering a machine-readable ontology and policy language layered on top of SID/eTOM/ODA [96].

**Vendor lineup**: Neo4j (Graphiti, LLM Graph Builder, Microsoft Agent Framework integration, Walmart 1.6M-employee feedback graph, **Sanofi 50× compound-ID acceleration**), TigerGraph (TigerVector unified in v4.2), Stardog ("fusion of LLM + KG"), Ontotext (merged with Semantic Web Company), Microsoft Fabric IQ, Databricks (Neo4j connector, Mosaic Agent Framework with KG-RAG), Snowflake, AWS (Neptune + Bedrock GraphRAG patterns; **Deloitte cyber-intel** case), **Palantir** (Ontology + Agentic Hives), Oracle. The **"context engineering"** framing — coined publicly in June 2025 by Karpathy and Lütke — gave the discipline its name; Zep, Weaviate, ByteRover, Redis ship explicit "context manager" components.

### 8.2 When KG+LLM works vs when it doesn't

**Where it clearly wins (production-validated):** entity-rich, relationship-heavy, compliance-traced domains. Drug discovery and clinical decision support (Sanofi 50×; clinical-QA RDF/OWL hitting 98% accuracy via ontology grounding); financial risk and KYC (FIBO-grounded queries linking executives, subsidiaries, regulatory actions); supply chain (Walmart, Palantir Hives); telecom service assurance (TM Forum SID-grounded service-impact tracing); cybersecurity intel (Neptune + GraphRAG at Deloitte); IT service operations (**LinkedIn cut ticket resolution from 40h to 15h — 63% improvement**). Common thread: questions whose *correctness depends on traversal*.

**Where it under-delivers**: pure unstructured text Q&A on narrow corpora (long-context LLMs and well-tuned vector RAG often suffice), ad-hoc analytical questions where the schema isn't stable enough to amortise ontology engineering, and fast-moving research/news content. KG extraction costs ~3–5× baseline RAG and demands domain-specific tuning. The 2026 verdict: **GraphRAG is not a default — it is justified by relationship density, regulatory traceability, and reusability across many agents and questions.**

The "70–80% project failure" data point matters. Neo4j and multiple practitioner posts converge on the claim that teams who start with vector-only RAG hit a hallucination/accuracy wall, and adding a KG as the knowledge layer is often what separates the failing 70–80% from the successful production deployments [97].

### 8.3 Architectural implications

A 2026 "Context Manager" reference architecture has four parts:

1. **Semantic/ontology layer** (Fabric IQ Ontology, Palantir Ontology, Neo4j+Graphiti, FIBO/SID-anchored domain models) defining canonical entity types, properties, relationships, constraints.
2. **Hybrid retrieval pipeline** combining BM25, vector ANN, structured graph traversal (Cypher/GQL/SPARQL), and reranking — modern pipelines compress 140k tokens of raw retrieval to ~6k tokens of structured context.
3. **Explicit Context Manager (CAG / "context-augmented generation" pattern)** that enriches requests with user, session, and policy context before invoking the model — turning context engineering into an architectural component, not a prompt template.
4. **Agent-memory store** with temporal awareness (Graphiti/Zep/POLE+O schema) so agents don't relearn facts each turn.

The MCP boundary matters: Fabric IQ's commitment to expose its ontology via MCP, and Palantir's emphasis on Ontology-as-tool-registry, both signal that **the semantic layer becomes the canonical MCP target for enterprise agents in 2026**.

---

## 9. Core transformation with agentic tools

### 9.1 The thesis & the pattern shift

The "agentic core" thesis: instead of replacing legacy core systems with another monolithic platform, *wrap them with agents over standardized tool registries (MCP), drive workflow with policy-defined goals rather than code-defined process flows, and let the agent layer absorb the variation that previously lived in customisations and integration code.* This collapses the historic CTO binary — Big-Bang rewrite vs Strangler Fig — into the **"Agentic Strangler"** pattern: MCP-enabled agents act as context-aware proxies that wrap legacy calls, maintain state across archaic systems, and enable in-place modernisation [98][99]. McKinsey-cited figures: 40–50% faster modernisation timelines and ~40% reduction in technical-debt costs when AI-augmented.

**The pattern shift is from code-defined workflow to policy-defined goals.** Traditional core systems encode process as immutable code paths and configuration; an agentic core encodes *what good looks like* (goal, policy, KPIs, constraints) and lets an orchestrator compose tool calls against a curated MCP registry. The same shift is visible across **Salesforce Headless 360 (TDX 2026)**, **Microsoft Agent 365**, **ServiceNow AI Agents**, **Workday Illuminate**, **SAP Joule** (40+ specialised agents in 35 SAP solutions, **Joule Studio GA Q1 2026** [100]), **Pega Agentic / Pega Blueprint**.

### 9.2 Production evidence vs marketing

**Real production today.** **JPMorgan's 450+ AI use cases**, $18B annual tech budget, LLM Suite proprietary platform, parallel **Vault core-banking replacement on Thought Machine** [101][102]. Salesforce reports Engine deploying production agents in 12 days. SAP Joule embedded in 35 SAP modules and ships Joule Studio for custom agents (GA Q1 2026). Workday Illuminate ships Case Agent, Performance Review Agent, Financial Close Agent. Block's Goose has migrated from `block/goose` to AAIF. **Stripe's Agentic Commerce Suite** has named launch customers (URBN, Etsy, Ashley Furniture, Coach, Kate Spade, Revolve, Halara) onboarding in 2026 [103]. These are real.

**Mostly marketing today.** Vendor decks claiming "agentic core banking" while shipping conventional T24/Profile cores with chat skins. **Temenos and FIS are explicitly characterised in the 2026 Backbase analysis as "AI-bolted approaches with partial engagement focus"** — AI added to existing architecture rather than built AI-native [104]. Many telco "agentic BSS" announcements remain demo-ware; Amdocs' own outlook explicitly states aOS will not contribute "any significant revenue" in the current fiscal year [105]. Insurance "agentic AI" claims must be discounted by Duck Creek/Guidewire/Sapiens production data — only ~22% of the >90% of carriers testing AI in 2025 reached full production.

### 9.3 Risks

- **Reversibility.** A code-defined workflow can be rolled back via versioned artifacts; a goal-defined agent that has already executed a sequence of side-effecting tool calls cannot. The 2026 governance literature increasingly treats "reversibility budget" as a first-class architectural property — what fraction of agent actions are reversible without manual intervention, and what is the blast radius of irreversible ones.
- **Audit and examiner expectations.** EU AI Act first major enforcement cycle is underway in 2026; auditors will demand documented oversight pattern justifications. US OCC/FDIC/NCUA 2026 supervisory priorities focus on "data-driven and measurable risks." For a regulated core, "the model decided" is not a defensible audit trail.
- **Prompt-injection and tool-confusion risk.** The shift from code-defined to policy-defined workflow is exactly the shift from *the program decides what to do* to *the agent reasons about what to do under prompt influence*.
- **Skills and operating model.** Agentic-core programmes require eval flywheel ownership, prompt/policy version control, model-cost FinOps, MCP registry curation, and HITL escalation paths — *a new function*, not a rebadge of the old integration team.

### 9.4 Architectural implications

The emerging reference shape has six layers between the user and the legacy core: (1) **client/UX** (often headless — Salesforce Headless 360); (2) **agent orchestrator** (Agentforce, Agent 365, ServiceNow AI Agents, Pega Agentic, Joule, Illuminate, Amdocs aOS Cognitive Core); (3) **policy-enforcing gateway** (Bedrock AgentCore + Cedar; OPA/Rego; ARPaCCino-style policy-as-code; AAIF Agentic Tokens); (4) **MCP tool registry** (the 2026 MCP roadmap formalises Server Cards, gateway semantics, registry SLAs, scheduled for Q4 2026); (5) **Context Manager / KG / semantic layer** (Fabric IQ, Palantir Ontology, Graphiti, telco-SID-grounded KGs); (6) **legacy core APIs** (Vault, T24, Profile, BSS, ERP, GL, PAS) wrapped as MCP servers or Cedar-governed tools. The eval flywheel and HITL controls wrap the stack as cross-cutting governance.

---

## 10. Agentic OS / runtime for complex operations

### 10.1 Agentic core banking

JPMorgan is the headline. The **Vault replacement on Thought Machine**, layered with an LLM Suite platform driving 450+ AI use cases on an $18B annual tech budget, plus Dimon's explicit positioning of AI as the central competitive battleground (PYMNTS, March 2026), make it the most concrete agentic-core programme in tier-1 banking [101][102][106]. **Block's Goose**, originally codename-goose released January 2025, has been donated to the Linux Foundation's AAIF (founded December 2025); Block's own Square Managerbot (proactive agent for Square sellers) and Cash App Money Bot are real consumer-facing agents downstream [107]. **Mastercard Agent Pay** launched April 2025 with **"Agentic Tokens"** — Mastercard-issued credentials scoped specifically to AI agents, with programmable spend, counterparty allowlists, transaction-category controls; Australia's first authenticated agentic transactions on Agent Pay shipped in 2026 [108]. **Fiserv has integrated Agent Pay into its merchant platform** [109]. **Temenos and FIS remain in the AI-bolted tier**, not the AI-native tier [104]. What's actually agentic: tokenized agent-scoped credentials, agent-driven loan origination, document intelligence in commercial banking. What's still mostly marketing: end-to-end autonomous core-banking workflows.

### 10.2 Agentic GL / autonomous close

BlackLine has begun adding AI capabilities (Verity AI; the WiseLayer acquisition for accruals agents) but adoption is early — roughly 20% of customers using any AI features as of late 2025. Trintech Cadency holds the enterprise-mid-market middle ground with strong reconciliation and SAP/Oracle connectors. FloQast competes on close-management UX. A new category — *agentic close execution* — is explicitly emerging around **Ledge** and **Nominal**, automating the working-papers/schedules/reconciliations/journal-entry preparation layer rather than just *governing* the close [110]. **Workday Illuminate ships a Financial Close Agent**. NetSuite launched **Autonomous Close**. What's actually agentic in 2026: schedule and reconciliation prep, anomaly detection, suggested journal entries with HITL approval. What's not yet: fully autonomous quarter-close — auditors and SOX 404 controls still require human sign-off on materiality and judgement areas.

### 10.3 Agentic BSS

This is Amdocs' home turf. **aOS / Cognitive Core** (the rebrand of amAIz) is a telco-specific agentic AI platform offering libraries of telco agents/sub-agents that operate over existing BSS stacks **from any supplier — explicitly multi-vendor, not just Amdocs** [105][111]. **Microsoft and Amdocs jointly packaged Amdocs Agentic Services on Azure at MWC 2026** [112]. **Netcracker** has been showcasing autonomous-operations case studies at FutureNet World 2026. **NEC's $2.9B acquisition of CSG**, on top of its Netcracker ownership, marks major BSS consolidation; the Qvantel-Optiva combination creates an alternative monetisation foundation [113]. **Ericsson launched the Telco Agentic AI Studio** with intent-driven agentic capabilities and 20+ cloud-native Telco IT AI Apps [114]. **Symphonica** positions as agentic no-code OSS. **TM Forum's AAIF and the Agentic Interactions Security project** provide the standards substrate (machine-readable ontology + policy language) [96]. What's actually agentic: order-fallout resolution, service-assurance triage, billing-anomaly detection, intent-driven fulfilment. What's still demo-ware: end-to-end revenue-impact autonomous BSS.

### 10.4 Agentic payments / FX

**Mastercard Agent Pay** (Agentic Tokens, agent-aware identity and checkout, partnerships with major LLM platforms) and **Visa Intelligent Commerce** (scoped tokenized credentials, behavioural and issuer-side authentication for machine-initiated payments, integrations with Anthropic, OpenAI, Microsoft) are now the **network-side rails for agent-initiated commerce** [108][115]. **Stripe's Agentic Commerce Suite** (Shared Payment Tokens, expanded to work with Visa and Mastercard tokens, plus Affirm and Klarna) is the **merchant-side rails** — already onboarding URBN, Etsy, Ashley Furniture, Coach, Kate Spade, Nectar, Revolve, Halara, Abt Electronics [103]. The architectural pattern: ***agent-scoped tokens with programmable controls + verifiable intent + network-level authentication***. The risk pattern: agent impersonation, prompt-injected payment flows, and the regulatory question of *who is the consumer of record* when an agent transacts.

### 10.5 Agentic policy management

Two senses of "policy" matter here.

**Insurance policy administration**: Duck Creek is most explicit — embedding insurance-native Agentic AI into underwriting and rating; Guidewire has formalised a roadmap; Sapiens leans on its dual P&C/L&A footprint. **~25%+ insurance-AI spend growth in 2026, but only ~22% of carriers reached full production in 2025** [116].

**Policy-as-code / authorization for agents**: OPA/Rego, AWS Cedar, AWS Verified Permissions, and Amazon **Bedrock AgentCore Policy**. Cedar's design — formal-verification-friendly, ~42–60× faster than Rego per Styra/Permit.io benchmarks — has positioned it as the default policy language for agent governance inside AWS. **The AgentCore Gateway pattern** is architecturally important: Cedar is enforced at the *agent-tool boundary* (after agent reasoning, before tool execution), not at the resource/API level — "policy is enforced regardless of what the agent does, regardless of how the agent is prompted or manipulated" [117]. **ARPaCCino** (arXiv 2507.10584) demonstrates an agentic-RAG generating and validating Rego policies [118]. This is where compliance, audit, and reversibility get teeth.

### 10.6 Common architecture pattern across subdomains

Across banking, GL, BSS, payments, and policy admin, **the same five-component pattern is hardening into a de facto reference**:

1. **Policy-enforcing gateway** (Cedar/AgentCore, OPA, Mastercard Agentic Tokens, Visa Intelligent Commerce auth) intercepting every tool call before execution, with default-deny posture and identity/scope/temporal constraints.
2. **MCP tool registry** of legacy and new APIs, with discoverable Server Cards, audit, and SLA metadata (MCP 2026 Q4 roadmap).
3. **Context Manager / KG** grounding the agent in canonical, governed entities (FIBO for banking, SID/eTOM for telco, ACORD/insurance ontologies for PAS).
4. **Eval flywheel** — review of agent decisions becomes training data for prompts, routing, and eval sets, with continuous reinforcement-from-human-feedback.
5. **HITL controls** — explicit escalation thresholds tied to monetary impact, regulatory class, or reversibility, with documented oversight-pattern rationale required for EU AI Act audits.

### 10.7 Architectural implications

The "agentic OS" claim is real, with caveats. **What is genuinely converging is the *operating layer*** — the policy gateway + MCP + Context Manager + eval/HITL stack — across vertical operations. **What is *not* converging into a single OS is the underlying systems of record**; banking ledgers, telco BSS, GLs, PAS, and payment networks remain heterogeneous and will for years. **The agentic OS is therefore best understood as a control plane over heterogeneous cores — a 2026 analogue to what Kubernetes became for compute.** The vendors that win the control-plane fight (AWS Bedrock AgentCore, Microsoft Agent 365 + Fabric IQ, Salesforce Headless 360, ServiceNow AI Agents, Palantir AIP, Amdocs aOS in telco, Pega Agentic, Joule) are those investing in all five components — not just shipping an agent SDK.

---

## 11. Compliance & regulation

### 11.1 EU AI Act — timeline & enforcement state (2025–2026)

The EU AI Act (Regulation 2024/1689) entered into force on 1 August 2024 with a staggered application calendar that has now run into significant turbulence. **2 February 2025** brought the prohibited-practices regime into force — Article 5 bans on social scoring, untargeted facial-image scraping, real-time remote biometric identification in public spaces (with narrow law-enforcement carve-outs), emotion recognition in workplaces and education, manipulative or exploitative AI, predictive policing based solely on profiling — alongside the **Article 4 AI literacy obligation** that applies to all providers and deployers regardless of risk classification [119][120]. **2 August 2025** activated the governance architecture (the AI Office in DG CNECT, the European AI Board, the Scientific Panel) and obligations on providers of general-purpose AI (GPAI) under Articles 51–55, including the systemic-risk regime triggered at the **10²⁵ FLOPs training-compute threshold**.

The most consequential 2025 development was the **General-Purpose AI Code of Practice**, published 10 July 2025 after a multi-stakeholder drafting process led by 13 independent chairs. Three chapters — Transparency, Copyright, Safety & Security — operationalise GPAI obligations as a presumption-of-conformity instrument; signing is voluntary but signatories receive supervisory benefit-of-the-doubt. **Twenty-six organisations signed the full Code, including Anthropic, OpenAI, Google, Microsoft, Amazon, IBM, Mistral, Cohere, Aleph Alpha. Meta refused to sign**, citing legal uncertainty and scope-creep beyond the Act itself. **xAI signed only Safety & Security**, declining transparency and copyright [121].

**The headline 2026 development** is the **Digital Omnibus on AI**, proposed by the Commission on 19 November 2025 as part of the seventh simplification package and given fast-track support by the European Parliament in March 2026 (569 in favour, 45 against, 23 abstentions). The Omnibus was driven by two acknowledged failures: harmonised standards under CEN-CENELEC JTC 21 are not ready, and Member States have failed to designate national competent authorities and notified bodies on schedule. **The Omnibus introduces conditional postponement with backstops**: high-risk Annex III obligations can slip up to 16 months to **2 December 2027** if standards remain unavailable; high-risk Annex I obligations (AI in regulated products such as machinery, medical devices, toys) can slip up to 24 months to **2 August 2028**. The original 2 August 2026 date for "full applicability" remains nominally in force but is effectively hollowed out for the high-risk regime; **Article 50 transparency obligations** (deepfake labelling, AI-interaction disclosure, watermarking of synthetic content) and GPAI Commission enforcement powers still activate on that date [122][123].

**Enforcement architecture**: GPAI is enforced centrally by the AI Office, which on 2 August 2026 acquires the power to request information, conduct evaluations, demand corrective action, and ultimately fine GPAI providers. High-risk and other AI-system enforcement is delegated to national competent authorities. **Penalty tiers**: up to €35M or 7% of worldwide annual turnover for prohibited-practice breaches; up to €15M or 3% for high-risk and GPAI obligation breaches; up to €7.5M or 1% for incorrect/incomplete/misleading information. Fines are cumulative across Member States for cross-border deployments.

### 11.2 NIST AI Risk Management Framework

The **AI RMF 1.0** (January 2023) remains the de-facto reference architecture for U.S. enterprise AI governance — voluntary, structured around **Govern, Map, Measure, Manage** across the AI lifecycle. Strategic importance derives less from content than from incorporation by reference: FTC, CFPB, FDA, SEC, EEOC, DoD all cite the RMF in enforcement guidance, and federal procurement increasingly demands NIST-aligned governance documentation [124].

**NIST AI 600-1, the Generative AI Profile**, was finalised on 26 July 2024. It enumerates 12 GenAI-specific risks and maps **over 200 suggested actions** to the four RMF functions [125]. The profile is the document a U.S. enterprise procurement officer is most likely to cite when interrogating a vendor's GenAI controls. **2025–2026 trajectory**: NIST released a March 2025 update sharpening guidance on supply-chain risk and third-party model assessment. Forward roadmap items include **SP 800-53 Control Overlays for AI**, the **Cyber AI Profile**, agent-specific guidance under emerging NIST AI Agent Standards work, and continued evolution toward AI RMF 1.1. The Trump administration's January 2025 EO rescinded much of the Biden-era AI EO but explicitly preserved NIST's technical-standards role; the **U.S. AI Safety Institute (US AISI)** at NIST continues to operate with a narrower remit focused on evaluations and pre-deployment testing.

### 11.3 ISO/IEC 42001

**ISO/IEC 42001:2023** (December 2023) is the world's first AI management-system standard, following the harmonised structure familiar from ISO 27001 and ISO 9001, applying Plan-Do-Check-Act to an AI Management System (AIMS). The standard is **certifiable** by accredited third-party conformity-assessment bodies; ANAB and UKAS now accredit certification bodies including BSI, DNV, SGS, TÜV SÜD, Schellman, A-LIGN [126].

**Certified organisations** as of late 2025 / early 2026 include **AWS** (first major cloud provider, certified November 2024 covering Bedrock, Q Business, Textract, Transcribe; passed first surveillance audit August 2025 with no findings) [127]; **Anthropic** (January 2025, covering Claude on the API, Claude Enterprise, Bedrock, Vertex AI deployments); **Microsoft** (Azure AI services and Microsoft 365 Copilot scope); **Google Cloud**; **IBM** (Granite model family — first major open-source model developer certified, September 2025); **KPMG International** (December 2025 — first Big Four international entity). **Reported aggregate**: over 100 certified organisations within 18 months of standard publication, an unusually fast adoption curve.

**What certification gets you**: presumption of conformity in EU AI Act Article 17 quality-management-system obligations (subject to harmonised-standards alignment), procurement signal in U.S. federal and large-enterprise RFPs, and a defensible governance-of-record artefact in regulator inquiries. **What it does not get you**: technical product certification — 42001 audits the management system, not the model. **Cost**: practitioner reports place implementation effort at 6–12 months for an organisation with mature 27001, with audit fees mid-five to low-six figures USD for a mid-sized scope.

### 11.4 Sectoral rules

**NYDFS Insurance Circular Letter No. 7 (2024)**, finalised 11 July 2024, governs insurer use of AI systems and external consumer data in underwriting and pricing — four pillars: actuarial validity, unfair-discrimination testing (disparate-treatment + disparate-impact), governance with senior-management accountability, transparency to consumers. **Third-party vendor obligations** require insurers to embed audit rights and regulatory-cooperation clauses in vendor contracts — a flow-down obligation directly relevant to BSS/OSS vendors [128].

**EU DORA** (Regulation 2022/2554) entered application on **17 January 2025**. While not an AI regulation per se, DORA is the operational-resilience regime under which AI-as-ICT-service is governed when consumed by EU financial entities. The **Critical Third-Party Provider (CTPP) regime** activated 18 November 2025; designation triggers direct EU-level oversight including inspection rights. Penalties up to 2% of worldwide annual turnover for institutions; up to €5M for CTPPs; up to €1M for individuals. For a software vendor selling AI to financial-services customers, DORA flows down concretely as **mandatory contractual clauses (Article 30)**, incident-reporting integration, exit-strategy testability, and threat-led penetration-testing participation [129].

**U.S. interagency banking guidance**: the OCC issued **Bulletin 2026-13** (revised model risk management) jointly developed with the Federal Reserve and FDIC, rescinding Bulletin 2011-12 (SR 11-7) and adopting a risk-based, proportionate posture. The OCC, Fed, and FDIC have signalled an upcoming joint Request for Information specifically addressing AI, including generative and agentic AI, expected 2026 [130]. **UK FCA / PRA**: strategic approach to AI on 22 April 2024 — no new AI-specific regulation but explicit reliance on existing tools (Principles for Business, Consumer Duty, Operational Resilience, PRA SS1/23, SMCR). Most permissive of the major Western regimes [131].

**Asia-Pacific**: South Korea's **AI Basic Act** (passed Dec 2024, in force January 2026) is the world's second comprehensive AI law and adopts the EU's risk-based architecture [132]. Japan's **AI Bill** (passed mid-2025) is principles-based, no penalties. **China's** generative AI regime continues to layer: 2023 Interim Measures, 2024 Deep Synthesis Provisions, and the **1 September 2025 Synthetic Content Identification Rules**. **India's IT Rules Amendment 2026** (effective 20 February 2026) establishes the world's first explicit deepfake regulatory framework with mandatory synthetic-content identification [133].

### 11.5 Sovereign AI

The 2025–2026 sovereign-AI build-out is the fastest-moving compliance-adjacent vector. **National AI safety institutes** form an interconnected network: UK AISI (founded November 2023), US AISI (NIST-housed, narrower 2026 remit), EU AI Office, Singapore AI Verify Foundation, analogous bodies in Japan, Canada, Australia, France, Korea.

**Regional model providers** are consolidating. The headline 2026 transaction is **Cohere's $20B acquisition of Aleph Alpha** announced 24 April 2026, backed by Schwarz Group with €500M structured financing and explicit support from both Canadian and German governments — a transatlantic sovereign-AI champion play [134]. **Mistral** has cemented itself as Europe's flagship through November 2025 SAP partnership embedding Mistral models into the SAP Generative AI Hub on European-sovereign infrastructure. **G42** in the UAE is building **Stargate UAE**, a 1GW AI compute cluster on the UAE-US AI Campus. **IndiaAI Mission** (~$1.25B). **Saudi Arabia's STC group and HUMAIN** partnered with Cohere and Anthropic. **Korea** is pushing domestic models (Naver, LG AI Research, Upstage) under the AI Basic Act.

**Sovereign cloud platforms** have hardened. France's **Bleu** (Orange + Capgemini + Microsoft Azure) and **S3NS** (Thales + Google Cloud) are operational; **S3NS achieved SecNumCloud 3.2 qualification on 17 December 2025**, the first hyperscaler-derived stack to do so [135]. Germany's **Delos Cloud** (SAP + Arvato) is building the public-sector sovereign equivalent. The pattern: **dual-key architectures** where the foreign hyperscaler provides the technology stack but a domestic-controlled entity holds operational and cryptographic control. **Data-residency mandates** are now baked into procurement; the **"regional model" requirement** — that a model be trained or fine-tuned on regionally-curated data and hosted in-region — is now appearing in tenders from European public sector, GCC, and Indian PSUs.

### 11.6 Architectural implications (compliance-by-design)

For a software vendor selling AI to regulated enterprises, the regulatory surface drives concrete architectural commitments:

1. **Portable model-serving abstraction** so the same product can run on AWS Bedrock, Azure OpenAI, Vertex, Mistral-on-SAP, Bleu, S3NS, on-prem GPU, and air-gapped sovereign deployments without product rewrites.
2. **Per-tenant data-residency enforcement** with cryptographic guarantees, not policy.
3. **Comprehensive audit logging at the agent-action level**, not just the model-call level, for DORA, NYDFS, and AI-Act traceability.
4. **Content provenance and watermarking** via C2PA or equivalent for the GPAI transparency regime and Article 50.
5. **Model-card and data-card automation** producing the documentation Articles 11–13 of the AI Act and ISO 42001 demand.
6. **Red-team and evaluation harnesses** with versioned results retained for the 10-year retention horizon Article 18 imposes.
7. **Human-oversight scaffolding** as a first-class platform primitive for Article 14, NYDFS Circular 7, and FCA Consumer Duty.

---

## 12. Consulting-firm consensus (April 2026)

Consulting view triangulated across nine major firms (compare also with `research/agentic-ai-trends-si-perspective.md` for full per-firm rigor analysis):

| Firm | Headline 2026 thesis | Track tilt | Source quality |
|---|---|---|---|
| **Gartner** | "40% of enterprise apps will include task-specific AI agents by end-2026, up from <5% in 2025"; "by 2027, over 40% of agentic AI projects will be cancelled"; dedicated 2026 Agentic AI Hype Cycle [142] | Overlay/transitional bullish; **strongest skeptic on operational** | High |
| **Deloitte** | 2026 State of Generative AI: 74% expect moderate agent use by 2027; only ~21% have mature governance; widening pilot-to-production gap [143] | Transitional bullish; governance-gap framing | High; n=3,235 across 24 countries |
| **KPMG** | Agentic AI Pulse: deployment quadrupled then settled at 26%; **61% of boards "not actively exploring agentic AI"** — sharpest contrarian board-level data; AI Gateway emphasises non-human identity governance [144] | Transitional bullish | High; longitudinal Pulse |
| **EY** | 89% insist HITL remains crucial; banking 99% awareness vs 31% implementation; **EY Canvas embeds multi-agent framework across 160,000 audit engagements** [145] | Overlay/transitional bullish; operational bearish | High; multi-pulse |
| **PwC** | 2026 AI Predictions: agents move from experiments to mainstream; 79% adoption claim; 88% increasing budgets for agentic AI [146] | Operational bullish — **most aggressive** | Mixed; framing-heavy |
| **Bain** | "Rapid, fitful, hard-to-predict progress"; **80% of GenAI use cases met or exceeded expectations, but only 23% of companies can tie initiatives to measurable revenue or cost** [147] | Transitional bullish; closest to architecture-first posture | High; named-author research |
| **BCG** | **Agentic AI is both a $200B opportunity AND a structural threat to technology-services delivery models**; agents reduce low-value work 25–40%, accelerate processes 30–50% [148] | Operational bullish | Mixed; aspirational headline |
| **Accenture** | "Declaration of Autonomy" Tech Vision 2025; expanded Google Cloud / Gemini Enterprise partnership; Avanseus acquisition (Feb 2026) for autonomous-network telco capability [149] | Operational bullish; vertical-specific | Marketing-loaded but capital-deploying |
| **Thoughtworks** | "Vibe coding" displaced by "context engineering"; AI antipatterns flagged; **93% of IT leaders plan to deploy AI agents by 2026** [150] | Overlay/transitional bullish; operational skeptic | High practitioner rigor |

**Convergence**: every firm flags governance, observability, and data readiness as universal blockers. Every firm reports most enterprises are not ready to operate agents safely at scale.

**Divergence**: PwC + Accenture + BCG project rapid scaled value (79–88% adoption framings); Gartner + Bain + Thoughtworks expect "rapid but fitful" with high failure rates. **KPMG's 61% boards-not-exploring directly contradicts PwC's 79% adoption** — these are not the same definition of "adopted."

**Calibration for the CEO conversation**: weight **Bain, EY, Deloitte, Gartner, KPMG** for substantive operational claims. Use **Accenture and PwC** primarily as evidence of *executive-team narrative pressure* the vendor must respond to in client conversations — not as ground truth on adoption maturity.

---

## 13. Chinese challengers and global competition

The Chinese agentic-AI signal is no longer "cheap foundation models." It is moving toward **deployable intelligence, general agents, and cost-disruptive model stacks** — and toward national-strategic-asset framing.

- **Manus** launched as a general AI agent and went viral in China in 2025. In **April 2026, multiple reports said China blocked Meta's planned acquisition of Manus**, framing it as a national-strategic AI asset [151]. Implication: cross-border agent platforms are now subject to export, data, and ownership restrictions analogous to advanced semiconductors. Enterprise buyers should assume cross-border agent platforms may become subject to similar restrictions.

- **DeepSeek**: Bloomberg and WSJ reported 2025–2026 hiring and model work emphasising **agentic capability and workflow execution** beyond reasoning [152]. **DeepSeek-V3's $5.6M training cost** (against GPT-4's estimated $50–100M [28]) remains the canonical 2025 evidence that frontier-class models can be trained at order-of-magnitude lower cost — and DeepSeek is no longer just a model-cost disruptor; it is moving into agentic capability and workflow execution.

- **Qwen, Kimi, GLM, Doubao** ecosystem: Western practitioner uptake of Chinese open models is real but uneven. **Qwen 3.6-35B-A3B** (April 2026) extends MoE into the small-model tier with only 3B active parameters at vision-language quality competitive with much larger dense models [16]. **Kimi K2.6** demonstrated 300-sub-agent autonomy. The deeper trend: **price compression that forces Western vendors to justify premium pricing through reliability, governance, security, and integration** rather than raw capability.

**Competitive axes**: China's challengers compete on **cost, speed, and productisation**. The Western enterprise advantage remains **governance, legal trust, security certification, and integration with Microsoft / Google / AWS / enterprise SaaS**. If Chinese models become "good enough" for coding, research, and back-office agents, **margins compress across the whole stack** — particularly in markets without sovereignty constraints.

**Strategic implication for an enterprise software vendor**: assume the model layer commoditises faster in markets where Chinese-model adoption is permitted. The defensible posture is the same architectural answer as the EU AI Act / DORA / sovereign-AI question: **owner-of-record on the integration surface, the ontology, the eval flywheel, and the compliance instrumentation**. Models route. Trust persists. Sovereign and regional-model tender requirements (§11.5) work *for* Western incumbents in the regulated-industry segment and *against* them everywhere else.

---

## 14. The services-disruption question — directly relevant to Amdocs

The single most uncomfortable thread for a services-heavy enterprise software vendor (Amdocs included) is the **agentic-AI services-disruption thesis**. The argument, in BCG and Thoughtworks framings: if agents can autonomously plan, execute, test, document, migrate, and govern software or business processes, **traditional headcount-based services revenue compresses**.

### 14.1 Evidence supporting the thesis

- **Coding-agent maturity**: Claude Code, OpenAI Codex/Agents SDK, Cursor, GitHub Copilot Agent Mode, Replit Agent 4 — operating at repo-level on real artifacts: code diffs, tests, commits, PRs, build logs [69][74][75]. Anthropic describes Claude Code as reading the codebase, making changes across files, running tests, and delivering committed code.
- **OSWorld benchmark progression 14.9% → 79.6% in 18 months** [71][72] — capability ceiling no longer binding for general computer-use tasks.
- **Salesforce Engine** reportedly deploys production agents in 12 days; **SAP Joule Studio GA Q1 2026** with custom-agent authoring [100]; **Workday Illuminate** ships Case / Performance / Financial Close Agents.
- **Internal-platform signals**: **Intel "One AI"** unifies siloed enterprise chatbots into an agentic platform [153]; **EY Canvas embeds multi-agent framework across 160,000 audit engagements** [145]. Professional services firms are not only advising on agents — they are rebuilding their own delivery platforms around agents.
- **BCG's $200B agentic-AI opportunity framing is paired with explicit warning** that agentic delivery threatens labour-arbitrage services models [148].
- **Coding agents attack the unit economics of application maintenance, migration, test generation, modernisation, and documentation** — the high-margin layer of integrator services.

### 14.2 Evidence pushing back

- **Production maturity is narrow**. Only ~22% of insurance carriers reached full production in 2025 despite >90% testing AI [116]. Amdocs' aOS explicitly "no significant revenue this fiscal year" [105]. Temenos and FIS are "AI-bolted, not AI-native" [104].
- **OSWorld step efficiency is poor**: top agents take 1.4–2.7× more steps than necessary; each step ~3× slower than the first as context grows. Demo tasks cost 10–30× more compute than human equivalents.
- **Multi-agent failure taxonomy (MAST, NeurIPS 2025)** documents 14 failure modes across 3 categories — production-grade autonomy is a research problem, not a deployment problem [42].
- **Reversibility, audit, examiner-readiness** create a hard floor on autonomous core-banking, BSS, GL, payments, policy operations (§9.3, §11). "The model decided" is not a defensible audit trail.

### 14.3 Honest synthesis

Agentic AI compresses *unit economics on commoditised services* (code generation, test scaffolding, documentation, migration scripts, ticket triage) **faster than** it compresses *integrated services tied to systems of record, regulatory context, and domain ontologies*. The defensible business model is to **convert the threatened labour-hours into productised, governed, metered agent services with the eval flywheel and compliance instrumentation as the moat**. The undefensible model is to keep selling labour-hours on tasks an agent can do.

### 14.4 What this means for Amdocs specifically

The **SID-aligned telco knowledge graph, TM Forum AAIF Agentic Interactions Security positioning, and "control plane over heterogeneous BSS cores" framing** (§8, §10) are exactly the assets that survive the disruption — *if* they are productised as governed agent services rather than embedded in human-led integration projects. The strategic question is not "can our agents do the work?" but **"can we re-shape our services revenue from labour-hours into outcome-priced governed agent execution before our clients do it themselves with hyperscaler agents?"**

The window is short. JPMorgan ($18B internal AI spend, 450+ AI use cases [101][102]), Verizon (28k care reps on Vertex/Gemini), AT&T (Ask AT&T, 100k users, ~27B tokens/day), and BBVA (120k employees on ChatGPT Enterprise) demonstrate that **major customers can build their own agent platforms when the infrastructure is hyperscaler-provided**. The integrator value-add must move up the stack: **domain ontology, eval flywheel, regulatory packs, BSS-semantics expertise** — none of which a hyperscaler can ship off-the-shelf.

---

## 15. Cross-cutting strategic synthesis

### 15.1 The pace-of-change problem — quantified

The qualitative claim that "AI is moving fast" is not the right input for a CEO decision frame. The right input is the quantitative pace, expressed in metrics that bind to capital decisions and product roadmaps.

- **Model release cadence**: In Q1 2026 alone, LLM Stats logged 255 model releases from major organisations — roughly **2.8 model releases per business day** [136]. Anthropic shipped Claude 3 (Mar 2024), 3.5 Sonnet (Jun 2024), 3.5 Sonnet v2 (Oct 2024), 3.7 (early 2025), 4 (mid-2025), 4.5 (late 2025), 4.6 (early 2026), 4.7 (Q1 2026) — eight frontier-tier releases in roughly two years [137].
- **Cost-curve compression**: Stanford AI Index 2025 records a **280-fold reduction** in inference cost for GPT-3.5-MMLU-equivalent quality between Nov 2022 ($20/M tokens) and Oct 2024 ($0.07/M tokens via Gemini 1.5 Flash 8B) [138]. Epoch AI's data shows price-per-quality-unit falling **9× to 900× per year depending on task** [139]. Frontier reasoning-model pricing is the exception — relatively stable, suggesting bifurcation toward marginal-energy cost at commodity tier, premium at frontier.
- **Benchmark turnover**: MMLU, HumanEval, GSM8K — fully saturated. The 2023-introduced replacements (MMMU, GPQA, SWE-bench) saw single-year gains of 18.8, 48.9, and 67.3 percentage points per the AI Index. New harder-frontier benchmarks (Humanity's Last Exam at ~9% top-system score, FrontierMath at ~2%, BigCodeBench at ~36%) are designed for multi-year headroom but are themselves under pressure within months of release. **Eval suites have a useful life of 12–24 months** and are infrastructure, not artefacts.
- **Deprecation events**: OpenAI deprecated GPT-4.5-preview (14 Jul 2025), o1-preview (28 Jul 2025), o1-mini (27 Oct 2025); DALL-E 2 and DALL-E 3 retire 12 May 2026. Anthropic retired Claude 2, 2.1, Sonnet 3 on 6 Nov 2024; Claude 3.7 Sonnet deprecated 11 Nov 2025 with full shutdown 11 May 2026 [140][141]. **A model integration written in 2024 has roughly a one-year useful shelf life before forced migration.**

### 15.2 "Eyes on the ball" — bet vs abstract framework

| Layer | Posture | Why |
|---|---|---|
| **Model** | **Abstract** | Foundation models commoditise at frontier (~5% spread); aggressive at cost-equivalent tier. Use a model-agnostic gateway. Caveat: reasoning-model frontier may stay differentiated 18–24 months. |
| **Eval** | **BET** | Encodes domain knowledge, ground truth, business semantics. Only safe mechanism to swap models. **Treat like the test suite of your most valuable software.** |
| **Protocol** | **Abstract** | MCP, A2A, AG-UI, OAuth — converging to LF-governed open standards. Adopt, contribute upstream; assume commodity by 2027. |
| **Inference runtime** | **Abstract** | vLLM, TensorRT-LLM, SGLang commoditising rapidly. |
| **Agent runtime** | **BET** | Orchestration, durable state, tool execution, sandboxing, recovery, replay are differentiated and remain so through 2027. Memory systems pre-commoditisation; bet selectively. |
| **Vector DB** | **Abstract** | Commoditising fast. |
| **Knowledge graph / ontology / Context Manager** | **BET** | The durable enterprise asset. For a BSS/OSS vendor, the network/customer/service KG is irreplaceable. |
| **Generic agent** | **Abstract** | Will be commoditised by frontier-model providers (ChatGPT Agents, Claude Agents, Gemini Agents). |
| **Domain-specific agent + wrappers** | **BET** | Embedded process knowledge, regulatory context, system-of-record integration, guardrails, HITL UX, audit logging — the wrappers are more defensible than the reasoning loop. |
| **Compliance instrumentation** | **BET, AS PRODUCT** | Audit logs, lineage, watermarking, residency enforcement, eval evidence, model cards, incident reporting — moving from cost-centre to product feature. **Customers will pay for compliance-by-design.** |

### 15.3 Architectural shape that survives 2027–2028 churn

Seven principles for an enterprise GenAI platform built in 2026 that should still be standing in 2028:

1. **Model-portable by construction.** Every product feature specifies minimum capability rather than a specific model. The platform routes; it does not bind. Migration cost from one model to another should be hours, not quarters.
2. **Eval-first development.** Features ship with eval suites before they ship with prompts. No production deployment without a regression baseline. Evals are versioned, retained, and — for regulated workloads — disclosable to auditors.
3. **Deployment-target-pluggable.** The same product binary deploys to public hyperscaler, sovereign cloud, on-prem GPU, and air-gapped enclaves. Configuration, not code.
4. **Knowledge-graph-centric, not vector-centric.** RAG is a retrieval implementation detail; the knowledge graph is the durable artefact.
5. **Agent runtime with replay and durable state.** Agents fail; the platform survives. Every action is logged with sufficient fidelity to replay, branch, and audit. State is durable, not in-memory. This is also AI Act Article 12 logging operationalised.
6. **Policy-as-code at every boundary.** Authorisation, content filtering, residency enforcement, regulatory disclosure, human-oversight gating are externalised policy decisions executed by an engine (OPA, Cedar, custom). No business logic mingled with policy.
7. **Compliance evidence is generated, not assembled.** Model cards, data lineage, evaluation reports, incident logs emerge from the platform automatically. The audit binder is a query, not a project.

### 15.4 CEO posture / decision frame

Six postures for the CEO of an enterprise software vendor navigating 2026–2028:

- **Bet on the integration surface, not the model.** Models commoditise; the integration into the customer's system of record, the domain ontology, the workflow semantics, and the regulatory context do not. The defensible business owns the last mile from foundation model to enterprise outcome — and that last mile is mostly knowledge engineering, change management, and compliance instrumentation, not LLM science.
- **Treat compliance as a feature, not an overhead.** ISO 42001, EU AI Act readiness, NIST RMF alignment, NYDFS auditability, DORA contractual posture — increasingly the basis of vendor selection in regulated verticals. Build them into the product, charge for them, use them to shorten enterprise sales cycles.
- **Optimise for optionality, not for any single bet.** The Digital Omnibus, the Cohere–Aleph Alpha consolidation, the model-deprecation cadence, the cost-curve collapse all argue against committing to a single model, single hyperscaler, or single deployment topology. Architect for swap. Pay the abstraction tax willingly.
- **Invest in evaluations as the durable IP.** Five years from now, the model that powers your product will not be the one shipping today — possibly not even from the same vendor. The eval suite that proves your product works on the new model is what carries your enterprise value across the transition.
- **Run the regulatory clock as a product roadmap.** AI Act high-risk obligations land 2 December 2027 (or 2 August 2027 if the Omnibus stalls). Korea AI Basic Act is live now. China synthetic-content rules are live. India IT Rules Amendment is live. NYDFS Circular 7 is live. Vendors who treat regulatory dates as roadmap commitments will be the ones whose customers pass their own audits.
- **Hold strategic patience on the agent thesis.** Agentic AI is genuinely transformative but the runtime, protocol, and evaluation infrastructure for production-grade enterprise agents is 18–24 months from maturity. Build the platform foundations now (durable execution, policy-as-code, eval harnesses, MCP-compatible tool layer); deploy narrow agents in production for specific high-value use cases; avoid the trap of declaring an "agent strategy" that depends on protocols and runtimes that do not yet exist.

---

## 16. Buyer evaluation framework

A procurement-grade evaluation matrix for any "agent platform" purchase or build-vs-buy decision, synthesised from consulting consensus + technical findings:

| Evaluation area | What to ask | Red flags |
|---|---|---|
| **Context** | Can the system represent enterprise knowledge beyond vector search — entities, relationships, policies, process flows, source provenance, time, ownership? | Vector-DB-only architecture; no ontology; no temporal awareness |
| **Governance** | Can every agent action be permissioned, logged, replayed, revoked, and explained? | "Logs available on request"; no replay; no policy-as-code |
| **Identity** | Are agents treated as **non-human identities** with scoped credentials, lifecycle management, OAuth 2.1 + signed Agent Cards? | Service-account credential reuse; no MCP OAuth scopes |
| **Evaluation** | Tiered judge fleet (distilled at 100% sampling, frontier 1–5%, humans <0.1%)? Code-evals on every commit? Eval-as-code primitives (Inspect AI shape)? | Closed eval UI only; no regression baseline; benchmark-only metrics |
| **Integration** | MCP / A2A / AG-UI surfaces? OAuth on every tool? Trust-tier boundary on untrusted content? | HTTP webhook integration; no protocol abstraction; no untrusted-content boundary |
| **Observability** | OTel-GenAI + OpenInference traces? Span linking across MCP / A2A? Per-tenant cost attribution? Drift detection? | Vendor-proprietary trace format; no MCP span linkage |
| **Portability** | Same product binary on hyperscaler, sovereign cloud, on-prem GPU, air-gapped enclave? Model-routing layer? | Single-cloud lock-in; hard-coded model assumptions |
| **Compliance** | ISO 42001 management system? AI Act Article 11–13 documentation automation? DORA Article 30 contractual posture? NYDFS / regional rules covered? | "Compliance available in roadmap"; no certification pathway |
| **Economics** | Cost-attribution at agent-action level? Active-parameter accounting? Speculative-decoding / cascade routing? | Per-API-call billing only; no token attribution; flat per-seat pricing |
| **IP protection** | Does the vendor learn from, retain, or reuse enterprise domain context? Cross-client knowledge isolation guaranteed contractually, not just "opt-out"? | Data-for-training opt-out only; no contractual cross-tenant isolation |

**Killer question (Bain framing)**: ***"Can we prove business impact under controlled risk?"*** The 80%/23% gap (use cases meet expectations / firms can tie to measurable revenue or cost) is the single most quotable consulting data point [147].

**Bottom-line buyer thesis**: do not buy "agents." Buy **controlled execution capacity over a well-governed context layer**. The vendors who can productise that survive 2027–2028; the rest become labour-hour billing in a world that prices outcome-execution.

---

## References

*All sources accessed 2026-04-27. Tier indicators: T1 = primary source (paper, vendor docs, regulator publication); T2 = reputable secondary (press release, established trade publication); T3 = analyst commentary or vendor blog.*

### §1 Edge models / SLMs
1. Microsoft Tech Community, "Welcome to the new Phi-4 models — Microsoft Phi-4-mini & Phi-4-multimodal" — https://techcommunity.microsoft.com/blog/educatordeveloperblog/welcome-to-the-new-phi-4-models---microsoft-phi-4-mini--phi-4-multimodal/4386037 (T1)
2. Microsoft Research, "Phi-4 reasoning, vision, and the lessons of training a multimodal reasoning model" — https://www.microsoft.com/en-us/research/blog/phi-4-reasoning-vision-and-the-lessons-of-training-a-multimodal-reasoning-model/ (T1)
3. SiliconAngle, "Microsoft debuts Windows AI Foundry, Foundry Local for AI PCs" (May 2025) — https://siliconangle.com/2025/05/19/microsoft-debuts-windows-ai-foundry-local-model-development-ai-pcs/ (T2)
4. Apple ML Research, "Apple Intelligence Foundation Language Models Tech Report 2025" — https://machinelearning.apple.com/research/apple-foundation-models-tech-report-2025 (T1)
5. arXiv 2507.13575 — "Apple Intelligence Foundation Language Models" — https://arxiv.org/abs/2507.13575 (T1)
6. distil labs, "Best Small Language Models for Fine-Tuning 2025" — https://www.distillabs.ai/learn/best-small-language-model-for-fine-tuning-2025/ (T3)
7. NVIDIA / HuggingFace, "Nemotron 3 Nano 4B" — https://huggingface.co/blog/nvidia/nemotron-3-nano-4b (T1)
8. Rockwell Automation press release, FactoryTalk Design Studio + Nemotron Nano — https://www.rockwellautomation.com/en-ca/company/news/press-releases/rockwell-automation-to-advance-industrial-intelligence-through-e.html (T2)

### §2 World models
9. DeepMind, "Genie 3: A new frontier for world models" — https://deepmind.google/blog/genie-3-a-new-frontier-for-world-models/ (T1)
10. TechCrunch, "DeepMind thinks Genie 3 world model presents stepping stone towards AGI" — https://techcrunch.com/2025/08/05/deepmind-thinks-genie-3-world-model-presents-stepping-stone-towards-agi/ (T2)
11. Google Blog, "Project Genie" — https://blog.google/innovation-and-ai/models-and-research/google-deepmind/project-genie/ (T1)
12. Meta AI, "Introducing V-JEPA 2: world models for understanding, prediction and planning" — https://ai.meta.com/blog/v-jepa-2-world-model-benchmarks/ (T1)
13. arXiv 2506.09985 — "V-JEPA 2: Self-Supervised Video Models Enable Understanding, Prediction and Planning" — https://arxiv.org/abs/2506.09985 (T1)
14. NVIDIA Newsroom, "Cosmos World Foundation Model Platform" — https://nvidianews.nvidia.com/news/nvidia-launches-cosmos-world-foundation-model-platform-to-accelerate-physical-ai-development (T1)
15. NVIDIA Cosmos product page — https://www.nvidia.com/en-us/ai/cosmos/ (T1)
16. OpenAI, "Sora 2" — https://openai.com/index/sora-2/ (T1)
17. Skywork analysis, "OpenAI Sora 2 review 2025: strengths, limits, scenarios" — https://skywork.ai/blog/openai-sora-2-review-2025-strengths-limits-scenarios/ (T3)
18. arXiv 2503.20523 — "GAIA-2: A Controllable Multi-View Generative World Model for Autonomous Driving" — https://arxiv.org/abs/2503.20523 (T1)
19. Wayve press release, "GAIA-3" — https://wayve.ai/press/wayve-launches-gaia3/ (T1)
20. Wayve, "GAIA-2 Technical Report" PDF — https://wayve.ai/wp-content/uploads/2025/03/GAIA_2_Technical_Report.pdf (T1)
21. Decart AI, "MirageLSD publication" — https://decart.ai/publications/mirage (T1)
22. Fortune, "Decart raises $100M at $3.1B valuation" (Aug 2025) — https://fortune.com/2025/08/07/exclusive-decart-raises-100-million-at-a-3-1-billion-valuation-chasing-the-future-of-real-time-creative-ai/ (T2)

### §3 Non-LLM architectures
23. Inception Labs, "Introducing Mercury" — https://www.inceptionlabs.ai/blog/introducing-mercury (T1)
24. arXiv 2506.17298 — "Mercury: Ultra-Fast Language Models Based on Diffusion" — https://arxiv.org/abs/2506.17298 (T1)
25. AI21 Labs, "Announcing Jamba" — https://www.ai21.com/blog/announcing-jamba/ (T1)
26. AI21 Labs, "Attention was never enough: rise of hybrid LLMs" — https://www.ai21.com/blog/rise-of-hybrid-llms/ (T1)
27. Zyphra, "Zamba2-7B" — https://www.zyphra.com/post/zamba2-7b (T1)
28. arXiv 2412.19437 — "DeepSeek-V3 Technical Report" — https://arxiv.org/abs/2412.19437 (T1)
29. arXiv 2505.09388 — "Qwen3 Technical Report" — https://arxiv.org/abs/2505.09388 (T1)
30. arXiv 2410.05954 — Pyramidal Flow Matching — https://arxiv.org/abs/2410.05954 (T1)
31. arXiv 2511.08544 — "LeJEPA: Provable and Scalable Self-Supervised Learning Without the Heuristics" — https://arxiv.org/abs/2511.08544 (T1)
32. Turing Post, "LeJEPA" — https://www.turingpost.com/p/lejepa (T3)

### §4 LLM-as-Judge / Evaluation
33. arXiv 2405.01535 — Prometheus 2 — https://arxiv.org/abs/2405.01535 (T1)
34. arXiv 2408.02666 — "Self-Taught Evaluators" (Meta) — https://arxiv.org/abs/2408.02666 (T1)
35. UK AISI, "Inspect AI" — https://inspect.aisi.org.uk/ (T1)
36. UK AISI, "2025 Year in Review" — https://www.aisi.gov.uk/blog/our-2025-year-in-review (T1)
37. TimeToAct LLM Benchmarks Jan 2025 — https://www.timetoact-group.at/en/insights/llm-benchmarks/llm-benchmarks-january-2025 (T3)
38. ACL Anthology — "Judging the Judges: Position Bias in LLM-as-a-Judge" (IJCNLP 2025) — https://aclanthology.org/2025.ijcnlp-long.18/ (T1)
39. arXiv 2410.21819 / EMNLP 2025 — "Measuring Self-Preference in LLM Judgments" — https://arxiv.org/abs/2410.21819 (T1)
40. ACL Anthology — "Rating Roulette: Self-Inconsistency in LLM-as-a-Judge" (EMNLP Findings 2025) — https://aclanthology.org/2025.findings-emnlp.1361.pdf (T1)
41. ACL Anthology — multilingual LLM-judge unreliability (EMNLP Findings 2025) — https://aclanthology.org/2025.findings-emnlp.587.pdf (T1)
42. arXiv 2503.13657 — "MAST: Why Do Multi-Agent LLM Systems Fail?" (NeurIPS 2025) — https://arxiv.org/abs/2503.13657 (T1)
43. MAST GitHub repository — https://github.com/multi-agent-systems-failure-taxonomy/MAST (T1)
44. Anthropic, "Constitutional AI: Harmlessness from AI Feedback" — https://www.anthropic.com/research/constitutional-ai-harmlessness-from-ai-feedback (T1)
45. arXiv 2407.19594 / ACL 2025 — "Meta-Rewarding LMs" — https://arxiv.org/abs/2407.19594 (T1)
46. OpenAI cookbook, "Building resilient prompts using an evaluation flywheel" — https://developers.openai.com/cookbook/examples/evaluation/building_resilient_prompts_using_an_evaluation_flywheel (T1)

### §5 Observability
47. OpenTelemetry, "GenAI Semantic Conventions" — https://opentelemetry.io/docs/specs/semconv/gen-ai/ (T1)
48. OpenTelemetry blog, "Stability proposal announcement (2025)" — https://opentelemetry.io/blog/2025/stability-proposal-announcement/ (T1)
49. Langfuse, "MCP tracing" — https://langfuse.com/docs/observability/features/mcp-tracing (T1)
50. Datadog blog, "MCP client monitoring" — https://www.datadoghq.com/blog/mcp-client-monitoring/ (T1)
51. InfoQ, "AAIF MCP Dev Summit (April 2026)" — https://www.infoq.com/news/2026/04/aaif-mcp-summit/ (T2)
52. Solo.io, "AAIF announcement and Agentgateway" — https://www.solo.io/blog/aaif-announcement-agentgateway (T3)
53. Datadog press release, "AI Agent Monitoring, LLM Experiments" (June 2025) — https://www.datadoghq.com/about/latest-news/press-releases/datadog-expands-llm-observability-with-new-capabilities-to-monitor-agentic-ai-accelerate-development-and-improve-model-performance/ (T1)
54. W&B Weave, GitHub releases — https://github.com/wandb/weave/releases (T1)
55. arXiv 2511.07585 — Cross-provider LLM output drift in financial workflows — https://arxiv.org/html/2511.07585v1 (T1)
56. OpenInference (Arize) spec — https://arize-ai.github.io/openinference/spec/ (T1)

### §6 Agent protocols
57. Linux Foundation press release, "AAIF formation" (Dec 9, 2025) — https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation (T1)
58. Anthropic, "Donating MCP and establishing the Agentic AI Foundation" — https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation (T1)
59. TechCrunch, "OpenAI, Anthropic, Block join AAIF" — https://techcrunch.com/2025/12/09/openai-anthropic-and-block-join-new-linux-foundation-effort-to-standardize-the-ai-agent-era/ (T2)
60. Linux Foundation, "A2A donation press release" (Jun 2025) — https://www.linuxfoundation.org/press/linux-foundation-launches-the-agent2agent-protocol-project-to-enable-secure-intelligent-communication-between-ai-agents (T1)
61. Linux Foundation, "A2A protocol surpasses 150 organizations — first-year metrics" (April 2026) — https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year (T1)
62. LF AI & Data community blog, "ACP joins forces with A2A" (Aug 2025) — https://lfaidata.foundation/communityblog/2025/08/29/acp-joins-forces-with-a2a-under-the-linux-foundations-lf-ai-data/ (T1)
63. MCP Manager, "MCP Adoption Statistics 2026" — https://mcpmanager.ai/blog/mcp-adoption-statistics/ (T3)
64. Stack Overflow Blog, "Authentication and authorization in MCP" (Jan 2026) — https://stackoverflow.blog/2026/01/21/is-that-allowed-authentication-and-authorization-in-model-context-protocol/ (T2)
65. Next Platform, "Cisco Outshift sends agentic AI protocol to LF" — https://www.nextplatform.com/ai/2025/07/29/ciscos-outshift-incubator-sends-agentic-ai-protocol-to-the-linux-foundation/100917 (T2)
66. arXiv 2508.00007 — Agent Network Protocol white paper — https://arxiv.org/html/2508.00007v1 (T1)
67. CopilotKit, "AG-UI protocol" — https://www.copilotkit.ai/ag-ui (T1)
68. Medium / Data Science Collective, "LangGraph vs CrewAI vs AutoGen 2026" — https://medium.com/data-science-collective/langgraph-vs-crewai-vs-autogen-which-agent-framework-should-you-actually-use-in-2026-b8b2c84f1229 (T3)

### §7 User agency / co-work / security
69. OpenAI, "Introducing ChatGPT Atlas" — https://openai.com/index/introducing-chatgpt-atlas/ (T1)
70. DeepMind, "Project Mariner" — https://deepmind.google/models/project-mariner/ (T1)
71. XLANG, "OSWorld-Verified" — https://xlang.ai/blog/osworld-verified (T1)
72. BenchLM leaderboard, OSWorld-Verified — https://benchlm.ai/benchmarks/osWorldVerified (T2)
73. Microsoft 365 Blog, "Copilot agentic capabilities GA in Word, Excel, PowerPoint" (Apr 2026) — https://www.microsoft.com/en-us/microsoft-365/blog/2026/04/22/copilots-agentic-capabilities-in-word-excel-and-powerpoint-are-generally-available/ (T1)
74. LeaveIt2AI, "Replit Agent v3 deep-dive" — https://leaveit2ai.com/ai-tools/code-development/replit-agent-v3 (T3)
75. Product Blog, "Replit Agent 4: a product development OS" — https://www.product.blog/p/replit-agent-4-the-new-operating (T3)
76. CyberNews, "Perplexity Comet review" — https://cybernews.com/ai-tools/perplexity-comet-review/ (T2)
77. Bloomberg, "Anthropic Memory feature" (Mar 3, 2026) — https://www.bloomberg.com/news/articles/2026-03-03/anthropic-tries-to-win-users-from-chatgpt-with-memory-feature (T2)
78. Anthropic, "Memory tool documentation" — https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool (T1)
79. Letta, "Letta v1 agent loop" — https://www.letta.com/blog/letta-v1-agent (T1)
80. ACS / Apple Intelligence coverage — https://ia.acs.org.au/article/2026/apple-reveals-the-ai-behind-siri-s-big-2026-upgrade.html (T2)
81. OWASP GenAI, "LLM01 Prompt Injection" — https://genai.owasp.org/llmrisk/llm01-prompt-injection/ (T1)
82. The Hacker News, "ShadowPrompt — Claude extension flaw enabled zero-click attack" — https://thehackernews.com/2026/03/claude-extension-flaw-enabled-zero.html (T2)
83. Koi Security, "ShadowPrompt writeup" — https://www.koi.ai/blog/shadowprompt-how-any-website-could-have-hijacked-anthropic-claude-chrome-extension (T2)
84. Brave Browser blog, "Indirect prompt injection in Perplexity Comet" — https://brave.com/blog/comet-prompt-injection/ (T1)
85. Oasis Security, "Claudy Day — claude.ai data exfiltration" — https://www.oasis.security/blog/claude-ai-prompt-injection-data-exfiltration-vulnerability (T2)
86. OWASP GenAI, "Q1 2026 Exploit Round-up" — https://genai.owasp.org/2026/04/14/owasp-genai-exploit-round-up-report-q1-2026/ (T1)
87. Anthropic, "Prompt-injection defenses" — https://www.anthropic.com/news/prompt-injection-defenses (T1)

### §8 Knowledge graphs / Context Manager
88. Microsoft Research, "Project GraphRAG" — https://www.microsoft.com/en-us/research/project/graphrag/ + microsoft/graphrag GitHub (T1)
89. AllegroGraph, "Neuro-Symbolic AI in Gartner 2025 Hype Cycle" — https://allegrograph.com/the-rise-of-neuro-symbolic-ai-a-spotlight-in-gartners-2025-ai-hype-cycle/ (T3)
90. OpenReview, "HippoRAG (NeurIPS'24)" + OSU-NLP-Group/HippoRAG GitHub (T1)
91. Neo4j blog, "Graphiti: Knowledge Graph Memory for an Agentic World" — https://neo4j.com/blog/developer/graphiti-knowledge-graph-memory/ (T1)
92. Microsoft Learn, "What is ontology (preview)? — Fabric IQ" — https://learn.microsoft.com/en-us/fabric/fundamentals/ontology-overview (T1)
93. Microsoft Fabric blog, "What's next for Fabric IQ Ontology" (T1)
94. Palantir blog, "Connecting AI to Decisions with the Palantir Ontology" — https://blog.palantir.com/ (T1)
95. EDM Council, "FIBO January 2026 normative release" — https://edmcouncil.org/page/fibo (T1)
96. TM Forum, "AI-Native Blueprint" / Agentic Interactions Security — https://www.tmforum.org/ (T1)
97. arXiv 2502.11371 — "RAG vs GraphRAG: A Systematic Evaluation" — https://arxiv.org/abs/2502.11371 (T1)

### §9 Core transformation
98. DEV.to, "Rise of the Agentic Strangler Fig Strategy" — https://dev.to (T3)
99. webMethods, agentic strangler patterns (T3)
100. SAP, "SAP Joule Studio GA Q1 2026" — https://www.sap.com/products/artificial-intelligence/ai-assistant.html (T1)

### §10 Agentic OS subdomains
101. Computer Weekly, "JPMorgan Chase to replace core banking system with Thought Machine" — https://www.computerweekly.com (T2)
102. AIBM, "JPMorgan's $18B AI Blueprint" (April 2026) (T2)
103. Stripe, "Agentic Commerce Suite — adding payments to LLM agentic workflows" — https://stripe.com/blog (T1)
104. Backbase, "Top AI-native banking platform providers 2026" — https://www.backbase.com (T3)
105. Light Reading, "Amdocs goes all-in for agentic AI with telco OS platform" — https://www.lightreading.com (T2)
106. McKinsey, "JPMorgan Chase's Derek Waldron on building an AI-first bank culture" — https://www.mckinsey.com (T2)
107. Block, "codename Goose" announcement — https://block.xyz (T1)
108. Mastercard newsroom, "Agent Pay" press releases (April 2025; Australia 2026) — https://www.mastercard.com/news (T1)
109. PYMNTS, "Fiserv–Mastercard Agent Pay integration" — https://www.pymnts.com (T2)
110. ledge.co, agentic-close category analysis (T3)
111. The Mobile Network, Amdocs aOS coverage — https://www.themobilenetwork.com (T2)
112. Windows News, "Amdocs Agentic Services on Azure at MWC 2026" (T2)
113. TechLed, "NEC-CSG; Qvantel-Optiva BSS consolidation" (T3)
114. Ericsson, "Telco Agentic AI Studio" — https://www.ericsson.com (T1)
115. American Banker, "Visa Intelligent Commerce" — https://www.americanbanker.com (T2)
116. ment.tech, "AI Agents for Insurance 2026" (T3)
117. AWS ML Blog, "Secure AI agents with Policy in Amazon Bedrock AgentCore (Cedar)" — https://aws.amazon.com/blogs/machine-learning/ (T1)
118. arXiv 2507.10584 — "ARPaCCino: Agentic-RAG for Policy as Code Compliance" — https://arxiv.org/abs/2507.10584 (T1)

### §11 Compliance & regulation
119. EU AI Act Implementation Timeline — https://artificialintelligenceact.eu/implementation-timeline/ (T1)
120. European Commission, "AI Act overview" — https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai (T1)
121. GPAI Code of Practice — Final Version — https://code-of-practice.ai/ (T1)
122. European Parliament press release, "AI Act delayed application; ban on nudifier apps" (March 2026) — https://www.europarl.europa.eu/news/en/press-room/20260323IPR38829/artificial-intelligence-act-delayed-application-ban-on-nudifier-apps (T1)
123. Morrison Foerster, "EU Digital Omnibus on AI: What is in it and what is not" — https://www.mofo.com/resources/insights/251201-eu-digital-omnibus (T2)
124. NIST, "AI Risk Management Framework hub" — https://www.nist.gov/itl/ai-risk-management-framework (T1)
125. NIST AI 600-1, Generative AI Profile (PDF) — https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf (T1)
126. ISO, "ISO/IEC 42001:2023 catalogue entry" — https://www.iso.org/standard/42001 (T1)
127. AWS, "ISO/IEC 42001 surveillance audit completed (Aug 2025)" — https://aws.amazon.com/blogs/security/aws-successfully-completed-its-first-surveillance-audit-for-iso-420012023-with-no-findings/ (T1)
128. NYDFS Insurance Circular Letter No. 7 (2024) — https://www.dfs.ny.gov/industry-guidance/circular-letters/cl2024-07 (T1)
129. EIOPA, "DORA hub" — https://www.eiopa.europa.eu/digital-operational-resilience-act-dora_en (T1)
130. OCC Bulletin 2026-13, "Revised Model Risk Management Guidance" — https://www.occ.treas.gov/news-issuances/bulletins/2026/bulletin-2026-13.html (T1)
131. FCA AI Update 2024 (PDF) — https://www.fca.org.uk/publication/corporate/ai-update.pdf (T1)
132. MSIT (Korea), "AI Basic Act" announcement (English) — https://www.msit.go.kr/eng/bbs/view.do?sCode=eng&mId=4&mPid=2&pageIndex=&bbsSeqNo=42&nttSeqNo=1071 (T1)
133. MeitY AI Advisory, 1 March 2024 (PDF) — https://regmedia.co.uk/2024/03/04/meity_ai_advisory_1_march.pdf (T1)
134. Fortune, "Cohere–Aleph Alpha sovereign-AI deal" (24 Apr 2026) — https://fortune.com/2026/04/24/cohere-aleph-alpha-deal-signals-rise-of-ai-middle-powers-counterweight-to-u-s-china/ (T2)
135. Futurum, "S3NS sovereignty: SecNumCloud qualification" — https://futurumgroup.com/insights/s3ns-sovereignty-can-thales-google-venture-make-ai-sovereignty-work-at-scale/ (T2)

### §12 Cross-cutting strategy
136. LLM Stats, "Latest model releases" — https://llm-stats.com/llm-updates (T2)
137. Anthropic, "Claude model deprecation policy" — https://platform.claude.com/docs/en/about-claude/model-deprecations (T1)
138. Stanford HAI, "AI Index 2025 — Technical Performance" — https://hai.stanford.edu/ai-index/2025-ai-index-report/technical-performance (T1)
139. Epoch AI, "LLM inference price trends" — https://epoch.ai/data-insights/llm-inference-price-trends (T1)
140. Tom's Hardware, "AI costs drop 280-fold" — https://www.tomshardware.com/tech-industry/artificial-intelligence/ai-costs-drop-280-fold-but-harmful-incidents-rise-56-percent-in-last-year-stanford-2025-ai-report-highlights-china-us-competition (T2)
141. Subquery, "2025–2026 AI Model Deprecation Calendar" — https://subquery.ai/blog/2026-01-21-ai-model-deprecation-calendar (T3)

### §12 Consulting-firm consensus (added v1.1)
142. Gartner, "Hype Cycle for Agentic AI 2026" + supporting predictions on enterprise app integration — https://www.gartner.com/en/research (T2; analyst reports gated)
143. Deloitte, "2026 State of Generative AI in the Enterprise" — https://www2.deloitte.com/us/en/insights/research/genai-state-of-ai.html (T2)
144. KPMG, "Generative AI Pulse / AI Gateway" — https://kpmg.com/us/en/insights/2026/agentic-ai-pulse.html (T2)
145. EY, "EY Canvas multi-agent audit framework — 160,000 audit engagements" — https://www.ey.com/en_gl/news (T2)
146. PwC, "AI Predictions 2026" — https://www.pwc.com/us/en/tech-effect/ai-analytics/ai-predictions.html (T2)
147. Bain & Company, "AI Survey 2025–2026 — 80%/23% measurable-impact gap" — https://www.bain.com/insights/ (T2)
148. BCG, "Agentic AI: A $200B opportunity and a threat to technology services" — https://www.bcg.com/publications (T2)
149. Accenture, "Tech Vision 2025 / Declaration of Autonomy + Avanseus acquisition" — https://www.accenture.com/us-en/insights/technology/technology-trends-2025 (T2)
150. Thoughtworks, "Agentic AI Advantage 2026 — 93% of IT leaders plan agent deploy" — https://www.thoughtworks.com/insights (T2)

### §13 Chinese challengers (added v1.1)
151. Reuters / Bloomberg / FT coverage of China-Manus / Meta acquisition block (April 2026) — primary press coverage; verify exact source against Bloomberg or FT before quoting (T2)
152. Bloomberg, "DeepSeek hires for agentic AI roles" + WSJ DeepSeek agentic-capability coverage (2025–2026) — https://www.bloomberg.com / https://www.wsj.com (T2)
153. Intel, "One AI internal agentic platform" — https://www.intel.com/content/www/us/en/newsroom (T2)

### §14 Services-disruption (cross-references existing sources, no new)

### §16 Buyer evaluation framework (cross-references existing sources, no new)

### Appendix A — Events, communities, academic sources (added v1.1)
154. AI Engineer World's Fair 2026 — https://www.ai.engineer/ (T1)
155. Agentic AI Summit (virtual) — listing pages aggregated via https://aitechsuite.com/agentic-ai-events (T2)
156. AI Agent Conference 2026 — https://aiagentconference.com (T2)
157. ACM Computing Surveys, "LLM-Based Multi-Agent Systems for Software Engineering" (2025) — https://dl.acm.org/journal/csur (T1)
158. Information Fusion, "AI Agents vs. Agentic AI: Conceptual taxonomy" (2025) — https://www.sciencedirect.com/journal/information-fusion (T1)
159. ACM Computing Surveys, "Security and privacy of LLM agents" (2025) — https://dl.acm.org/journal/csur (T1)
160. AI Agent Index 2025 (MIT / Cambridge / Stanford-linked) — https://aiagentindex.mit.edu/ (T1)

---

## Appendix A. Events, communities, monitoring sources

For ongoing CEO-level signal scanning beyond this brief.

**Events (2026) worth tracking:**
- **AI Engineer World's Fair 2026** [154] — applied agent engineering, 6,000+ engineers, founders, AI leaders.
- **Agentic AI Summit** [155] — dedicated virtual summit on agent architectures, memory, planning, orchestration.
- **AI Agent Conference 2026** [156] — agentic enterprise focus.
- **Chatbot Summit 2026** — repositioned around "Mastering Agentic AI" including agentic commerce.
- **Hyperscaler events that are now agent-platform events**: Google Cloud Next, Microsoft Build / Ignite, AWS re:Invent, NVIDIA GTC.
- **TM Forum DTW** (telco-specific; AAIF Agentic Interactions Security work).

**Authoritative publishers (executive monitoring):** Gartner, Deloitte, BCG, Bain, PwC, EY, KPMG, Accenture, Thoughtworks, HFS Research.

**Authoritative publishers (technical):** OpenAI, Anthropic, Google Cloud, Microsoft Learn, AWS ML Blog, NVIDIA Developer, LangChain, LlamaIndex, Microsoft Semantic Kernel, CrewAI, Cursor / Anysphere updates, Linux Foundation AAIF.

**Weak-signal sources (treat as triangulation, not evidence):** AI Engineer, The Information, VentureBeat, The Batch, Latent Space, Interconnects, Ben Thompson (Stratechery), Simon Willison, swyx, Harrison Chase, Andrew Ng, Andrej Karpathy, Jack Clark, Nathan Lambert, Deedy Das, Bindu Reddy, Eugene Yan.

**Practitioner communities:** r/AI_Agents, r/aiagents, r/LangChain, r/cursor, r/LocalLLaMA. Useful for framework fatigue, tool reliability, pricing pain, Cursor / Claude Code / Codex comparisons. Treat as weak evidence unless triangulated.

**Academic streams (peer-reviewed, applied):**
- ACM Computing Surveys — "LLM-Based Multi-Agent Systems for Software Engineering" (2025) [157]
- Information Fusion — "AI Agents vs. Agentic AI: Conceptual taxonomy" (2025) [158]
- ACM Computing Surveys — "Security and privacy of LLM agents" (2025) [159]
- arXiv 2503.13657 — MAST: Why Do Multi-Agent LLM Systems Fail? (NeurIPS 2025) [42]
- AI Agent Index 2025 (MIT / Cambridge / Stanford-linked) [160]

**Academic conclusion (applied):**
1. **Agentic AI is architecturally distinct from chatbots** — memory, planning, tool-use, multi-agent coordination, and delegated action create new failure modes.
2. **Software engineering is the strongest near-term agentic domain** because tasks are artifact-rich and testable.
3. **Security and privacy risks are structurally amplified** when agents can act, not merely answer.
4. **Evaluation remains underdeveloped** — benchmark success does not reliably translate into production reliability, governance, or business value.

---

### Companion / prior research (in this repo)
- `research/agentic-ai-trends-si-perspective.md` — multi-agent SI strategic analyst report (Apr 27, 2026); contains full per-firm rigor analysis cited in §12
- `research/agentic-ai-trends-executive-slide.md` — three-track signal map slide
- `research/agentic-ai-trends-actor-matrix.md` — cross-actor matrix
- `research/telco-genai-control-layers.md` — telco baseline

---

*End of report. Companion executive brief: `./executive-brief.md`.*
