# Chapter 12 — Recommendations and Decision Criteria

*Date: 2026-04-28 | Synthesis*

This chapter translates the discussion of Chapter 11 into actionable form: a go / no-go decision tree, a defensible investment envelope, an explicit set of leading indicators that would flip the analysis, and a list of what *not* to invest in.

## 12.1 The go / no-go decision

Before committing to proprietary RL fine-tuning, the operator must answer eight questions in sequence. **Any "no" stops the process.**

| # | Question | If yes | If no |
|---|----------|--------|-------|
| **1** | Is the target workload narrow enough for a 7B–32B specialist to dominate? | Continue | Use frontier API. |
| **2** | Does the reward signal satisfy *either* (a) deterministic verifiability *or* (b) LLM-judge rateability with reliability controls (position randomisation, multi-judge ensemble, periodic calibration audit against held-out human labels)? | Continue | Use frontier API. *Naive LLM-judge without controls is a reward-hacking liability.* |
| **3** | Is sustained monthly token volume ≥ 50M against premium APIs, or ≥ 500M against mid-tier APIs? | Continue | Frontier API is cheaper. |
| **4** | Will the workflow stay in scope ≥ 12 months — long enough to amortise at least one base-model-generation re-fine-tune cycle? | Continue | Frontier API plus prompt engineering. |
| **5** | Can the operator run a production-grade self-hosted inference stack (vLLM or SGLang with PagedAttention, EAGLE-3 / Medusa speculative decoding, multi-LoRA, prefix caching, continuous batching, model routing)? | Continue | Use a managed inference platform (Fireworks, Together, Predibase) — but expect the proprietary-kernel edge to compress towards open-source over the next 12–18 months. |
| **6** | Does the regulatory regime permit the required DP epsilon? Specifically: ε ≥ 4 for internal corporate workloads; *ε ≤ 0.5 for HIPAA-covered medical data is not viably achievable today with a multi-tenant RL strategy*. ε ≤ 5 for GDPR high-sensitivity. | Continue | Use frontier API and accept the cost; or build a single-tenant on-premise NeMo-style deployment without cross-client amortisation. |
| **7** | If operating in EU jurisdictions: will modification compute remain below one-third of the base model's training compute (~3,300 H100-days for frontier bases)? | Continue | Heavy continued pretraining triggers EU AI Act provider reclassification. Stay below threshold or budget for full GPAI provider obligations. |
| **8** | Is the operator prepared to re-fine-tune at base-model-generation cadence (every 12–18 months) rather than treating training as a one-off? | Continue | Single-train assumption is unsupported by the empirical record. Don't proceed unless the workflow's economic margin survives the re-train cycle. |

Eight yeses → proceed with the recipe in §12.2. Any no → fall back to the highest-quality frontier API for that workload, plus aggressive prompt and harness engineering and prefix-cached batching.

## 12.2 The recipe (when go-criteria are satisfied)

The defensible 2026 recipe synthesises the practitioner channel (S6), the academic literature (S1b, S2), and the TCO model (S3):

1. **Pick a base model.** 7B–14B for narrow workloads (email search, ticket classification, structured extraction); 32B for reasoning-heavy workloads (legal research, code generation, math). The base must be open-weight or commercially licensable (Llama 4, Qwen 3, DeepSeek-R1-Distill, Mistral). It must be a *strong instruct* base — verifiable-reward RL amplifies latent capability, it does not instil it (S2).

2. **Begin with SFT-distillation, not pure RL.** Generate 1,000–10,000 high-quality reasoning traces from a strong open-weight teacher (DeepSeek-R1, Qwen-Math, R1-Distill-70B). The s1 result (Muennighoff et al. EMNLP 2025) shows 32 H100-hours on 1,000 well-curated traces beats o1-preview on competition math. **Do not over-SFT** — the arXiv:2508.16546 finding establishes that aggressive SFT can permanently damage out-of-distribution recovery, capping subsequent RL gains.

3. **Apply GRPO with verifiable rewards where possible.** Math (answer match), code (compilation + tests), SQL (query result match), structured output (schema validation), tool-use (final-state correctness) all admit deterministic verifiers. Use the OpenPipe ART framework (now CoreWeave-controlled — see §12.4 for governance risk) or fork the Apache-2.0 codebase and maintain internally.

4. **For non-verifiable workloads, use RULER (LLM-as-judge) with reliability controls.** Position randomisation across trajectory ordering, multi-judge ensemble (at least 3 distinct judge models), periodic calibration audit against held-out human labels (sample 50–100 trajectories per quarter). Without these controls, expect ~10pp accuracy degradation from position bias alone (arXiv:2412.05579 2025).

5. **Self-host inference with the nine-layer stack.** vLLM or SGLang as base; PagedAttention; EAGLE-3 speculative decoding; SGLang RadixAttention prefix caching; multi-LoRA adapter sharing; FP8 quantisation on H100 / B200; continuous batching with prefill–decode disaggregation if scale permits; prompt caching at the application layer; model routing for low-confidence queries to a fallback frontier API.

6. **Plan the re-fine-tune cycle from day one.** Maintain (a) the SFT trace dataset, (b) the verifier or judge specifications, (c) the eval suite, (d) the prompt and harness assets, all under version control. At each base-model generation release, re-run the SFT-distillation + GRPO pipeline; budget 1–2 engineers × 2–4 weeks for the re-train.

7. **Pricing model: outcome-based, not pass-through.** Per-resolution / per-ticket-resolved / per-execution-completed. This decouples vendor revenue from token cost decline and preserves margin through renewal cycles. Per-seat-without-outcome-anchoring is exposed.

## 12.3 Investment envelope

The following ranges are defensible point estimates from the TCO model (S3) and the practitioner data (S6). They assume a 13B base model on a moderately complex workload (e.g., email search with structured extraction or customer service with verifiable resolution).

### One-time setup (Year 1, months 1–3)

| Item | Range | Comment |
|---|---|---|
| Compute (initial GRPO + SFT-distillation) | $100 – $4,000 | At April 2026 H100 spot prices ($1.25–$3.11/GPU-hr) |
| Synthetic-data trace generation | $500 – $5,000 | API costs at frontier teacher prices for 1k–10k traces |
| Inference stack stand-up | $5,000 – $20,000 | Engineering time only; assumes existing GPU infra |
| Eval suite construction | $10,000 – $30,000 | Domain expert hours; varies by domain |
| **Total one-time** | **$15K – $60K** | Engineering dominates |

### Ongoing (steady state)

| Item | Annual range | Comment |
|---|---|---|
| Inference compute (self-hosted) | $0.02 – $0.11/M tokens × volume | At H100 spot; floor will rise as energy constraints bite |
| Re-fine-tune cycles (1–2 / year) | $5K – $30K each | Depends on workflow change rate |
| Eval maintenance & expansion | $20K – $80K | Domain-expert hours; this is the stealth cost of durability |
| Inference engineering FTE | $200K – $400K (0.5 – 1.0 FTE) | The compounding moat of §11.1 #3 |
| Reward-function calibration | $20K – $40K | Calibration audits, judge ensemble maintenance |
| **Annual ongoing total (excl. variable inference)** | **$240K – $550K** | At 500M tokens / month, add ~$120K – $660K of compute |

### Break-even horizons (from S3)

- **vs premium APIs (GPT-4o tier, ~$2.50/M input / $10/M output):** ~50M tokens/month, 7-month payback.
- **vs mid-tier APIs (DeepInfra/Together, ~$0.15/M blended):** ~500M tokens/month, 14-month payback.
- **Under price-stabilisation scenario** (API decline rate falls from 70%/yr to 30–40%/yr, plausible given energy floor): break-even extends to 20–28 months at 500M tokens/month. Still attractive but no longer dominant.

### Team

- 1.0 ML engineer (RL fine-tuning + eval design)
- 0.5 – 1.0 inference engineer (production stack)
- 0.5 ML platform / data engineer (eval pipelines, observability)
- 0.25 domain expert (per quarter for calibration audits)

A 2-FTE-equivalent steady state is the floor. Below this, the engineering compounding moat does not materialise and the organisation is paying for an under-utilised stack.

## 12.4 Risks to acknowledge (and price into the plan)

The five new risks the adversary surfaced (§5 of `10-adversary.md`) belong on the risk register:

1. **OpenPipe / ART governance fragility** — CoreWeave acquisition (Sept 2025) means the recommended open-source stack is now vendor-dependent. Mitigation: fork ART under Apache-2.0 and maintain internally, or evaluate alternatives (TRL, Unsloth's RL extensions, vLLM's emerging RL features).

2. **Reward hacking via LLM-judge bias** — the practitioner channel under-prices this. Mandatory mitigation: position randomisation, multi-judge ensemble, calibration audit. Cost is in §12.3 ongoing budget.

3. **EU AI Act provider reclassification** — if pursuing heavy fine-tuning on a frontier base in EU jurisdictions, monitor compute against the 1/3 threshold; budget for full GPAI provider obligations if exceeded.

4. **Durability paradox** — better base models make fine-tuning easier *and* shorten the lead window. Mitigation: budget the re-fine-tune cycle as a recurring line item (§12.3), not a one-off cost.

5. **Inference cost plateau** — if the 70%/yr API decline rate falls to 30–40%/yr, break-even extends materially. Mitigation: model the price-stabilisation scenario as a named sensitivity in any board-level approval; ensure go/no-go decision passes under the conservative scenario, not just the central case.

## 12.5 Leading indicators that would shift the analysis

These are the conditions under which the synthesis would change. Monitor each quarterly.

| Indicator | Direction of shift | Why it matters |
|---|---|---|
| **Independent ART-E replication** on a workload OpenPipe did not select, with comparable cost/quality | Strengthens the sweet-spot case; raises confidence on hypothesis G | The flagship single data point would graduate from "feasibility evidence" to "calibrated population estimate" |
| **Three-arm longitudinal study** (tuned-G_n vs vanilla-G_n+1 vs tuned-G_n+1) on any major-version base-model upgrade | Either direction could move the durability calculus | This is the load-bearing missing study |
| **Federated RLHF production deployment** at >100 enterprise clients with < 3pp performance penalty under non-IID, ε ≤ 1 | Opens the multi-tenant healthcare/banking wedge | Would refute hypothesis E in regulated industries |
| **Sierra / Harvey / Glean renewal cohort** with disclosed retention rates | Validates or refutes application-layer ARR multiples | Renewal data lands late 2026 / 2027 — watch for first disclosures |
| **Token-price decline rate** falling below 40%/yr at the commodity tier | Extends self-hosting break-even to 24+ months | Energy constraint, hardware stabilisation, demand-supply tightening could each trigger this |
| **EU AI Act enforcement actions** under the 1/3 compute threshold | Sharpens compliance cost; may push EU operators back to API-only | First enforcement landings expected in late 2026 after 2 Aug 2026 effective date |
| **DP-SGD performance recovery** to within 1pp of centralised at ε ≤ 1 with non-IID data | Opens healthcare/banking multi-tenant economics | Active research direction; not expected in 2026 |
| **Reward-hacking-resistant LLM-judge** patterns reaching production-grade reliability | Closes the C2/condition-(b) reliability gap | Multi-judge ensemble + verifiable-anchor approaches are emerging |
| **Hardware co-design moat erosion** (Groq / Cerebras catching up by software-only inference stacks) | Weakens the durable inference moat | Fundamental architecture limits suggest unlikely without new silicon |

## 12.6 What *not* to invest in

Equally important: the streams converge on a set of investments that the adversary's evidence specifically counter-recommends.

- **Pretraining a foundation model from scratch unless your distribution / sovereignty / regulated-data position guarantees a buyer at acquisition.** The market signal from Inflection / Adept / Aleph Alpha is that pretraining capability sells at talent-replacement cost, not at IP value. Pretraining is a hire-quality lever, not a moat.

- **Pass-through-pricing API wrappers.** Token cost decline has compressed margin to zero on renewal. Either move to outcome pricing or move to proprietary fine-tune + self-host; the middle is structurally untenable.

- **Generic "domain expert AI" without verifiable sub-tasks or a deep eval moat.** The Harvey BigLaw Bench evidence is the canonical caution: the legal domain is where durability should hold longest, and the original Harvey fine-tune was surpassed by seven vanilla base models within ~12–18 months. Generic domain plays without a verifiable inner loop are exposed.

- **Single-customer dedicated fine-tuning at sub-50M-token / month volumes.** The engineering envelope (§12.3) does not amortise.

- **Multi-tenant RL-as-a-service for healthcare or banking customer data, today.** The DP gap to HIPAA's ε ≤ 0.5 is unbridgeable with current federated RLHF performance penalties.

- **Heavy continued pretraining on EU GPAI bases.** The 1/3-compute provider-reclassification threshold imposes full GPAI obligations; budget the compliance load before committing.

- **"Train once, exploit forever" theses.** The empirical base rate is closer to 12–18 months of lead per fine-tune cycle in the durable domains.

- **A bet on OpenPipe / ART independence as an indefinite governance guarantee.** Apache-2.0 lets you fork; CoreWeave's roadmap may diverge from yours.

## 12.7 Decision summary in one paragraph

For the user's strategic question — "should an enterprise / SI / vertical-AI vendor invest in proprietary RL fine-tuning of foundation models, and where is the economic sweet spot?" — the answer is: **conditionally yes, in a workload-shaped envelope that satisfies eight specific joint conditions, with a $15–60K initial outlay and $240–550K + variable inference annual ongoing, against a 7–14 month break-even horizon vs frontier APIs but a 12–18 month durability window per fine-tune cycle.** The recipe is GRPO + SFT-distillation + reliable LLM-judge + nine-layer self-hosted inference stack, on a 7B–32B open-weight base, served by a 2-FTE-equivalent team, priced on outcomes, with a re-fine-tune cycle budgeted at every base-model generation. The bet is **not** on the model weights — it is on the eval, the harness, and the inference moat compounding over time, with the fine-tune as a renewable input rather than a fixed asset.

The next chapter (13 — Conclusion) names the leading indicators, the timing horizon, and the position the user should take given the analysis.

## References

See the consolidated bibliography in `99-bibliography.md` (annex).
