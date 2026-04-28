# Phase 2 — Stream Selection Brief
*Date: 2026-04-28 | Author: orchestrator | Status: awaiting HITL approval*

This brief synthesises the strongest signals from Phase 1a (market/industry) and Phase 1b (academic Q1) and proposes four deep-research streams for Phase 3. Each stream maps to multiple of the user's A–G hypotheses and to specific empirical gaps the orchestrator believes are *answerable* given Phase-3 budget.

## 1. Cross-cutting findings (where market and academic evidence agree)

| # | Finding | Phase 1a evidence | Phase 1b evidence | Hypotheses touched |
|---|---------|-------------------|-------------------|--------------------|
| F1 | Verifiable-reward RL (GRPO/RLVR) produces qualitatively larger reasoning gains than preference RLHF — **but the largest gains are tied to large base models.** | DeepSeek-V4 / R1 line forced industry-wide repricing; OpenAI RFT customers report 12–39% domain gains | DeepSeek-R1-Zero +55pp on AIME at ~600B; community 7B replications produce only 2–5pp | B, G |
| F2 | **Distillation can substitute for RL** at far lower compute when strong teacher traces are available. | s1-style approaches not yet quantified in market reports | s1 (Stanford, EMNLP 2025): Qwen2.5-32B + 1,000 traces in 32 H100-hours → beats o1-preview by ~27pp on competition math | B, G |
| F3 | **The inference layer is generating more durable revenue than the post-training layer.** | Fireworks $315M ARR (416% YoY) vs. fine-tuning vendors selling fine-tunes as cost-of-goods; multi-LoRA commoditises adapter deployment | PagedAttention 2–4×, EAGLE-3 3–6.5×, SGLang 6.4× — these compound multiplicatively for self-hosted workloads | D, G |
| F4 | **Token prices fell ~300× in 36 months and frontier release cadence is sub-quarterly** — pass-through margin and fine-tune lead-time both shrink. | tokencost.app price index; Menlo Ventures CIO survey | Not directly addressed in academic literature | A, B, C |
| F5 | **Durability has a partial mechanism (diff-vector transfer)** but no longitudinal study across major generations. | Harvey case shows durable RFT gain on hard proprietary eval; generic enterprise FT ROI declining (a16z survey) | Lin et al. (EMNLP 2025): diff vectors recover +46.9pp IFEval / +15.7pp LiveCodeBench when migrating Llama 3.0 → 3.1; no major-generation study published | B, G |
| F6 | **Multi-tenant RL is research-stage**; the only production-adjacent pattern (Cohere) requires owning the model architecture and uses synthetic data via design partners. | FedRLHF/FedBiscuit are research; Cohere RBC/Fujitsu pattern is production-adjacent | Federated SFT: 2–7pp degradation under non-IID; DP-SGD requires materially more data | C, E |
| F7 | **Failure modes have a fundamental ceiling** that verifiable rewards sidestep but preference RLHF cannot. | Acqui-hire pattern (Inflection/Adept/Aleph Alpha) prices model-training businesses at talent value | Goodhart overoptimisation (Gao et al. ICML 2023); sycophancy; verbosity bias; mode collapse (ICLR 2024) | F, G |

## 2. Convergent stream candidates

The two scouts independently proposed five streams each with strong overlap:

| Market stream | Academic stream | Convergent stream |
|---------------|-----------------|-------------------|
| 1 — Durability threshold by domain complexity | A — Durability across base-model generations | **S1 — Durability of fine-tuning gains** |
| 2 — Inference TCO at production scale | C — Training-vs-inference TCO model | **S3 — Full-stack TCO and the inference moat** |
| 3 — Synthetic data wedge / cross-client RL | D — Federated RLHF performance penalty | **S4 — Multi-tenant economics and the data-isolation penalty** |
| 5 — Outcome-based pricing survival | — | folded into S4 (business-model dimension) |
| 4 — Acqui-hire decomposition | — | deprioritised; folded into Phase 5 synthesis |
| — | B — Verifiable-RL at sub-13B scale | combined with E into S2 |
| — | E — Distillation vs RL substitution | **S2 — Where the sweet spot actually is: distillation, RL-at-scale, and the s1 line of attack** |

## 3. Proposed Phase-3 streams (4 chapters)

### S1 — Durability of fine-tuning gains across base-model generations

**Question:** Does a domain RL fine-tune retain its lead when the base model gets a major upgrade — and what fraction of the lead can be recovered cheaply via diff-vector recycling rather than full re-training?

**Why it's central:** Hypothesis B's core claim is that gains are erased. Phase 1 produced one strong piece of evidence on each side — Harvey's persistent gain (real, but on a hard proprietary eval) and Lin et al.'s diff-vector recovery (real, but only across minor versions 3.0 → 3.1). The longitudinal data across *major* generational gaps does not exist publicly. Constructing the best available estimate from public benchmarks + case studies is high-leverage.

**What the deep agent will produce:** A chapter quantifying the durability decay curve across (Llama 2 → 3 → 3.1 → 4) on three task families (math, code, instruction-following), citing every available empirical comparison; a separate analysis of which domain-eval properties (verifiability, vocabulary, regulatory specificity) predict durability; an integration with Lin et al.'s diff-vector method as a cost-saver.

**Maps to:** B (primary), G (durability is a sweet-spot input), F (failure-mode case studies).

### S2 — The actual sweet spot: distillation, RL-at-scale, and the s1 line of attack

**Question:** Across the (RL vs SFT-distillation × open-vs-proprietary teacher × small-vs-large base) design space, where is the cost-performance frontier today, and what does that imply for the practical "minimum RL spend × maximum gain" point hypothesis G calls the sweet spot?

**Why it's central:** Hypothesis G is the user's ROI question. The s1 result (1,000 traces, 32 H100-hours, beats o1-preview on competition math) is potentially decisive: it suggests the sweet spot is *not* RL at all for many tasks but rather curated distillation from a strong reasoning teacher. Conversely, the R1-Zero result shows pure RL can produce qualitative emergence — but only at large scale. Mapping when each strategy dominates is the most direct attempt to *locate the sweet spot*.

**What the deep agent will produce:** A chapter with a 2×2×2 cost-performance frontier across the design space; a synthesis of the GRPO replication record at sub-13B; an explicit cost-per-percentage-point comparison for SFT-on-traces vs DPO vs PPO vs GRPO; a frank assessment of whether the sweet spot is "small-RL + distillation" or "pure distillation + inference investment."

**Maps to:** G (primary), B (durability of small-budget gains), D (inference investment as the complement).

### S3 — Full-stack TCO and the inference moat

**Question:** Integrating training amortisation, ongoing re-training cost across base-model generations, and serving-stack savings (PagedAttention + speculative decoding + multi-LoRA), at what production token volume does proprietary RL fine-tuning + self-hosted inference reach break-even vs API consumption — and how does that break-even shift as token prices keep falling?

**Why it's central:** Hypothesis D states inference engineering is the real ongoing moat. Phase 1 confirmed this directionally (Fireworks/Together revenue), but no public source integrates training cost with serving savings end-to-end. This is the load-bearing financial calculation for any "build-your-own-RL-fine-tune" decision today.

**What the deep agent will produce:** A chapter with a parametric break-even model (training $ + re-training $ × N generations + inference $/M tokens at scale, vs API $/M tokens at scale, with sensitivity to token-price decline rate). Validation against 2–3 published case-study cost numbers. A clear chart of break-even token volume vs domain-gain magnitude.

**Maps to:** D (primary), C (pricing-model implications), G (the cost half of the optimisation problem).

### S4 — Multi-tenant economics and the data-isolation penalty

**Question:** What is the empirical penalty (in performance and economics) of doing RL post-training under enterprise data-isolation constraints — and is there a viable wedge (synthetic data, federated RLHF, secure aggregation) that turns dedicated RL training from cost+ into a SaaS-amortisable product?

**Why it's central:** Hypothesis E is the user's hardest economic constraint. Phase 1 confirmed: production federated RLHF doesn't exist publicly; Cohere's wedge depends on owning the model. The deep version of this question is whether a SaaS-amortised, multi-tenant RL fine-tune is actually achievable today, and what business-model architecture makes it pencil out (including outcome-based pricing as a hedge against token-cost decline).

**What the deep agent will produce:** A chapter quantifying the federated penalty (2–7pp from current literature, but stratified by client count, heterogeneity, and DP-ε); the synthetic-data-wedge mechanics (Cohere's RBC/Fujitsu pattern); a decision matrix for outcome-based-vs-token-pass-through pricing under declining token costs; a verdict on whether the multi-tenant wedge exists today.

**Maps to:** E (primary), C (pricing-model viability), G (scale half of the optimisation problem).

### S5 — Acqui-hire decomposition: what the market actually paid for

**Question:** In the 2024–2026 wave of foundation-model-company acquisitions (Inflection → Microsoft, Adept → Amazon, Aleph Alpha → Cohere, Stability AI's collapse, Reka rumours, AI21 rescue), what specifically was the acquired asset — talent, model weights, training pipelines, customer relationships, or contractual position — and what does the price decomposition imply about the standalone economic value of proprietary model-training capability?

**Why it's central:** This is hypothesis F made concrete. Every founder/operator decision on whether to invest in proprietary RL training has a counterfactual: would the resulting capability sell at a premium, or only at talent-replacement cost? The decomposition tells us whether the market believes fine-tuned model weights have standalone value.

**What the deep agent will produce:** A chapter with: a deal-by-deal table (price, headcount retained, pipeline transferred, contracts assumed); analyst commentary and PitchBook-style deal-term reconstruction where available; a normalised "$/researcher" and "$/customer-relationship" comparison; an explicit interpretation of what the market priced (and what it didn't) for proprietary training capability.

**Maps to:** F (primary), C (business-model viability), G (the risk side of the optimisation problem).

### S6 — The Daily Dose of Data Science perspective: practitioner-curated training & inference signals (2024–2026)

**Question:** What does ~18 months of curated practitioner-facing newsletters from `avi@dailydoseofds.com` (received in `jb@jishutech.io`) reveal about which fine-tuning techniques, RL methods, and inference optimisations are *actually* being adopted, recommended, and discarded — beyond the academic and analyst views captured in S1–S5? Which underlying primary sources (papers, repos, blog posts) does Avi's curation surface that the other streams missed?

**Why it's central:** Practitioner curation is a third, distinct evidence channel: it filters the academic torrent through "what real engineers find useful this month." Newsletter-cited links are also a high-signal route to find primary sources that don't surface via web search ranking. The user has read this archive over time and signals strong material density.

**What the orchestrator will produce:** A chapter built directly from the email archive — quote-and-link-cited — covering: which post-training techniques recur most often; which inference / serving optimisations Avi flags as production-ready; which methodological warnings he issues; the link tree behind the strongest items (papers, GitHub, vendor blogs) followed one level deep. This stream is mined by the orchestrator using the Gmail MCP, not delegated to a sub-agent, because email-archive access is local to the orchestrator session.

**Maps to:** B (practitioner-validated technique selection), D (inference-side practitioner signal), G (where practitioners think the sweet spot already is).

## 4. What is deliberately *not* a Phase-3 stream

- **Pure SFT scaling laws** — covered as comparator inside S2 only.
- **Pretraining economics / sovereign AI / regulatory landscape** — out of scope per charter.
- **Pure benchmark survey** — Phase 1b already covered the empirical magnitudes.

## 5. Resource estimate

Streams S1–S5 → one parallel `general-purpose / sonnet` agent each, ~10–15 web searches, ~6–10 deep fetches, output target 4,000–6,000 words to `phase2-deep/stream-{n}-{slug}.md`. S6 is mined by the orchestrator using the Gmail MCP and inline WebFetch on cited links. Total Phase-3 wall time roughly equal to Phase-1 (≈6–10 minutes per agent in parallel).

After Phase 3, Phase 4 (adversary) challenges the synthesised conclusions, then Phase 5 produces the actual chapter `.md` files for the final report.

---

**HITL gate (decision recorded 2026-04-28):** User approved all six streams (S1–S6). Phase-3 launches now.
