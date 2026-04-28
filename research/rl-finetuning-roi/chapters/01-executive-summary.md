# Chapter 1 — Executive Summary

*Date: 2026-04-28 | Topic: ROI of RL Fine-Tuning of Foundation Models | Confidence: medium-high on directional claims; low-medium on population-level cost estimates pending independent replication of OpenPipe ART-E.*

## The question

Is there an economically viable sweet spot for proprietary RL fine-tuning of foundation models that produces durable competitive advantage proportional to investment, or is it a strategic dead-end given current market dynamics?

## The short answer

**Yes — but it is workload-shaped, narrowly bounded, and shorter-lived than most vendor narratives suggest.** A defensible sweet spot exists in 2026 for operators who satisfy *all eight* of the following joint conditions:

1. Workload is narrow enough for a 7B–32B specialist to dominate.
2. Reward signal is verifiable, or LLM-judge-rateable with reliability controls.
3. Sustained monthly token volume ≥ 50M (vs premium APIs) or ≥ 500M (vs mid-tier APIs).
4. Workflow stays in scope ≥ 12 months — long enough to amortise re-fine-tunes per base-model generation.
5. Operator can run a production-grade self-hosted inference stack (vLLM / SGLang / multi-LoRA / EAGLE-3 / prefix caching).
6. Regulatory regime permits the required differential-privacy budget (ε ≥ 4 for internal data; *ε ≤ 0.5 for HIPAA is not viable today*).
7. Modification compute stays below one-third of base-model training compute (EU AI Act provider-reclassification threshold, effective 2 Aug 2025, enforcement 2 Aug 2026).
8. Operator accepts a 12–18 month durability window per fine-tune cycle and budgets the re-train as a recurring line item.

Failure of any single condition kicks the workload out of the envelope. The *intersection* of conditions is the sweet spot.

## Headline evidence

- **Verifiable-reward RL produces large gains, not 5–8% gains.** DeepSeek-R1-Zero went from 15.6% to 71.0% on AIME 2024 via pure GRPO + verifiable rewards (DeepSeek-AI 2025). OpenPipe's ART-E (Qwen2.5-14B + GRPO + RULER) beat o3 / o4-mini / Gemini 2.5 Pro / GPT-4.1 on email search at 96% accuracy, 1.1 s latency, $0.85 / 1,000 runs, on under $80 of training compute (OpenPipe 2025).

- **But durability is short.** Harvey's own BigLaw Bench data shows seven vanilla base models surpassed Harvey's original fine-tuned legal-AI system within ~12–18 months as base scores rose from ~60% to ~90% (Harvey 2025b). Harvey responded by abandoning single-fine-tuned-model strategy and switching to multi-provider routing (TechCrunch 2025).

- **Inference engineering, not training, is the durable advantage.** A nine-layer optimisation stack (PagedAttention, EAGLE-3 speculative decoding, multi-LoRA, RadixAttention, etc.) compounds to a 5–8× cost-efficiency edge over naive serving (Chawla 2026f; Kwon et al. 2023; Li et al. 2025). Fireworks AI hit $315M ARR at 416% YoY on a pure-inference play (Sacra 2025).

- **The market prices model-training capability as talent, not as IP.** Inflection / Adept / Character.AI license fees were $9–41M per researcher (5–20× normal acqui-hire). Adept's $414M training pipeline licensed for $25M. Stability's millions of open-source downloads produced zero revenue protection at 9:1 debt:cash (TechCrunch coverage of all three; The Register 2024).

- **The reference open-source RL stack (OpenPipe ART) was acquired by CoreWeave in September 2025** (CoreWeave 2025; TechCrunch 2025b). Treat ART as a vendor-dependent toolkit, not a neutral resource; fork under Apache-2.0 if continuity matters.

## Investment envelope (13B specialist, mid-volume workload)

- **One-time setup:** $15K – $60K (engineering dominates; compute is $100 – $4K).
- **Annual ongoing:** $240K – $550K + variable inference compute at $0.02–$0.11/M tokens × volume.
- **Team:** 1.0 ML engineer + 0.5–1.0 inference engineer + 0.5 platform/data + 0.25 domain expert.
- **Break-even:** ~50M tokens/month vs premium APIs (7-month payback); ~500M tokens/month vs mid-tier APIs (14-month payback). Under the price-stabilisation scenario (energy floor + hardware stabilisation), payback extends to 24–32 months.

## What this is *not* a recommendation for

- Pretraining a foundation model from scratch unless distribution / sovereignty / regulated-data position guarantees an acquisition exit.
- Pass-through-pricing API wrappers (margin compresses to zero on renewal).
- Generic "domain expert AI" without verifiable sub-tasks (Harvey BigLaw Bench is the canonical caution).
- Single-customer dedicated fine-tuning at sub-50M-tokens/month volumes.
- Multi-tenant RL-as-a-service for HIPAA-covered data, today.
- Heavy continued pretraining on EU GPAI bases without budget for full provider obligations.
- "Train once, exploit forever" theses.

## What would change the analysis

- Independent ART-E replication on a workload OpenPipe did not select.
- A three-arm longitudinal study (tuned-G_n vs vanilla-G_n+1 vs tuned-G_n+1) on a major-version base-model upgrade.
- A federated-RLHF production deployment at >100 enterprise clients with <3 pp performance penalty under non-IID, ε ≤ 1.
- Sierra / Harvey / Glean disclosed renewal cohort (late 2026 / 2027).
- Token-price decline rate falling below 40%/yr at the commodity tier.
- First EU AI Act enforcement actions under the 1/3-compute threshold.

## Confidence levels

- **High** on the directional claims: inference is the durable moat (D); pass-through pricing destabilises (C); the market prices training as talent (F).
- **Medium-high** on the recipe: GRPO + SFT-distillation + reliable LLM-judge + nine-layer self-hosted inference stack is the 2026 default.
- **Medium** on the existence of the sweet spot: ART-E is strong feasibility evidence on one workload; population prevalence not yet calibrated.
- **Medium-low** on the population-level cost estimates: TCO model defensible but unvalidated against more than 2 published case studies.
- **Low** on durability across major-version generations: only Harvey BigLaw Bench provides empirical evidence, and that lands pessimistic.

## The position to take

**Bet on the eval, the harness, the inference stack, and the workflow lock-in — not on the weights.** The fine-tune is a renewable input, not a fixed asset. The variables that compound (engineering capability, customer-relationship depth, eval-suite IP) are the durable assets. The variable that depreciates (model weights at base-model release cadence) is the cycle line item. Hypothesis G is correct in form, narrower in extent than its prose, and time-sensitive in execution. Position accordingly.

## Where to read next

- **Chapter 11 — Discussion** for the eight-condition characterisation of the sweet spot.
- **Chapter 12 — Recommendations** for the go/no-go decision tree, the recipe, the investment envelope, and the leading indicators.
- **Chapter 13 — Conclusion** for the user's strategic-position summary.
- **Chapter 10 — Adversary** for the load-bearing counter-evidence (especially Harvey BigLaw Bench and the CoreWeave / OpenPipe acquisition).
- **Chapters 04–09** for the per-stream deep dives.
- **`99-bibliography.md`** for the consolidated reference list.
