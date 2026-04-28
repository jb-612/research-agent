# Chapter 2 — Introduction and Research Question

*Date: 2026-04-28*

## 2.1 The strategic question

This research evaluates a single strategic question:

> Is there an economically viable sweet spot for proprietary RL-based fine-tuning of foundation models that produces durable competitive advantage proportional to investment, or is RL fine-tuning a strategic dead-end given current market dynamics?

The question was posed by the project sponsor in the form of an *optimisation problem with a presumed interior solution*: a region in (training-spend × reasoning-gain × inference-cost × scale) space where the marginal return on proprietary RL training exceeds the marginal cost, and where that net positive holds against (a) the cost of running the resulting model in production and (b) the depreciation of any gain when the next-generation base model is released. The research mandate was either to *locate* this region or to *credibly argue it does not exist yet* and characterise the conditions under which it would.

The sponsor enters the analysis with seven prior beliefs (assumptions A–G), each of them framed to be falsifiable.

## 2.2 The seven prior hypotheses (A–G)

| # | Hypothesis | What would refute it |
|---|-----------|----------------------|
| **A** | Clients swap foundation models every few weeks; vendor lock-in is low. | Documented material switching costs (>2 weeks engineering, regression risk) in published case studies. |
| **B** | RL on open-source models gives ~5–8% domain gains, soon erased by next hyperscaler release. | Durable gains >12 months on a non-trivial benchmark fraction. |
| **C** | Encapsulating foundry-model cost destabilises pricing; observability invites disintermediation. | A stable, profitable pricing pattern surviving renewal at scale. |
| **D** | Fine-tuning is just a starting point; inference (caching, sessions, hardware) is the real ongoing engineering. | Empirical post-training accounting for the majority of TCO at scale. |
| **E** | Enterprise dedicated training collapses to cost+ because data isolation prevents cross-client amortisation. | A productionised federated / synthetic-data approach yielding cross-client gains without contractual risk. |
| **F** | Risk is real: cheap-model successes (DeepSeek) co-exist with capital-loss failures (AI21Labs). | Characterised base rate of ROI distribution. |
| **G** | An interior optimum exists: minimum RL spend × maximum reasoning gains × solved inference & scale. | Either locate the optimum or formally argue no interior optimum exists today. |

The synthesis (Chapter 11 §11.2) reports the verdict on each hypothesis after the six research streams and the adversary phase. Six of the seven survived in some form; one (B) was refuted in the verifiable-reward regime; one (G) was located as workload-shaped and time-bounded rather than as a single global maximum.

## 2.3 Scope

**In scope** — RL-based and adjacent post-training techniques applied to open-weight or licensed foundation models:

- Reinforcement Learning from Human Feedback (RLHF)
- Reinforcement Learning from AI Feedback (RLAIF) / Constitutional AI
- Direct Preference Optimization (DPO) and variants (IPO, KTO, ORPO)
- Group Relative Policy Optimization (GRPO) and reasoning-RL methods
- Reinforcement Fine-Tuning (RFT, OpenAI / Anthropic / Google APIs)
- Process Reward Models (PRM) and verifier-based RL
- Supervised Fine-Tuning (SFT) — only as comparator / baseline
- Inference optimisation (caching, KV-cache, speculative decoding, quantisation, hardware co-design)
- Business-model architecture (cost+, SaaS, fixed-price, observability tradeoffs, outcome-based pricing)
- Scaling economics across enterprise multi-tenant constraints

**Out of scope:**

- Pretraining from scratch
- Continued pretraining on domain corpora (only briefly compared)
- Pure prompt engineering / context engineering as a standalone strategy (treated as the null baseline)
- Safety / alignment as a standalone topic (only where it intersects with ROI)

## 2.4 Methodology

**Two-tier evidence model.** Tier 1 — Q1 peer-reviewed (NeurIPS / ICML / ICLR / ACL / EMNLP / NAACL / COLM / JMLR / TMLR / Nature MI / Science Advances / IEEE TPAMI). Tier 2 — high-quality grey (named-lab arXiv preprints, Stanford HAI, Epoch AI, SemiAnalysis, MLPerf, McKinsey / BCG / Bain genAI reports, a16z, Artificial Analysis, vendor engineering blogs with skepticism). Tier 3 — reputable trade press (The Information, MIT Tech Review, IEEE Spectrum, FT, WSJ). Tier 4 — vendor / advocacy (cited but flagged; never load-bearing for empirical claims).

**Citation rules.** Every empirical claim carries a citation. Every cited source has been verified to exist. Numbers trace to primary sources, not secondary citations. Disagreements between reputable sources are surfaced rather than smoothed over. A final citation-audit pass before the PDF export removes any unresolved reference and weakens the corresponding claim.

**Phase plan.** Six parallel research streams (S1–S6) reporting into a synthesis-and-adversary phase, then HITL review, then editorial and PDF export.

| Phase | Output | Status |
|-------|--------|--------|
| 1a — Market scan | `phase1-scan/market-scan.md` | complete |
| 1b — Academic scan | `phase1-scan/academic-scan.md` | complete |
| 2 — Stream selection | `phase2-deep/stream-brief.md` | complete |
| 3 — Six deep-research streams | `phase2-deep/stream-{1-6}-*.md` | complete |
| 4 — Adversary challenge | `phase3-adversary/adversary-report.md` | complete |
| 5 — Synthesis chapters | `chapters/11-discussion.md`, `12-recommendations.md`, `13-conclusion.md`, this chapter | complete |
| 6 — HITL review | sponsor edits | pending |
| 7 — Editorial + PDF | final assembly | pending |

## 2.5 The six streams (briefly)

- **Stream S1 — Durability of fine-tuning gains across base-model generations.** Lin et al. (EMNLP 2025) show diff-vector recycling transfers gains across the Llama 3.0→3.1 minor-version boundary; no published study covers a major-version boundary. Catastrophic forgetting reaches 18–35% on 7B decoder models. Domain expert evaluation overhead, not GPU compute, dominates re-train cost.
- **Stream S2 — The sweet spot recipe (distillation, RL-at-scale, the s1 line of attack).** SFT-distillation at 32B + budget forcing dominates the cost-performance frontier at $25–$1,250 ($1,250 with optional second-stage GRPO).
- **Stream S3 — Full-stack TCO and the inference moat.** Break-even ~500M tokens/month vs mid-tier APIs (14-month payback); ~50M tokens/month vs premium APIs (7-month payback). Engineering headcount is 10–50× larger than training compute. Hardware co-design (Groq / Cerebras) carries durable architectural advantage.
- **Stream S4 — Multi-tenant economics and the data-isolation penalty.** Federated RLHF: 0.3–1pp under IID, 2–7pp under non-IID Dirichlet, >15pp at ε = 1 DP. Wedge requires base-model ownership AND outcome-based pricing.
- **Stream S5 — Acqui-hire decomposition.** Market priced training capability at $9–41M per researcher (5–20× normal acqui-hire); model weights at near-zero standalone; application-layer plays at 35–100× ARR multiples.
- **Stream S6 — The Daily Dose of Data Science practitioner channel.** Eighteen months of practitioner-grade newsletters confirm GRPO + RULER + ART as the 2026 default RL stack and document the inference-stack discipline as a compounding moat.

## 2.6 The chapter map

The remainder of the report:

- **Chapter 3** — Landscape of RL fine-tuning, 2024–2026.
- **Chapters 4–9** — One chapter per research stream (S1 → S6).
- **Chapter 10** — Adversary report stress-testing the cross-stream synthesis.
- **Chapter 11** — Discussion: where the sweet spot is, where it isn't, and the eight joint conditions.
- **Chapter 12** — Recommendations: go/no-go decision tree, the recipe, investment envelope, leading indicators, what *not* to invest in.
- **Chapter 13** — Conclusion and the strategic position the user should take.
- **`99-bibliography.md`** — Consolidated Chicago author–date bibliography.

The Executive Summary (Chapter 1) compresses the discussion, recommendations, and conclusion into a 2-page brief for time-pressed readers. The deeper chapters (11, 12, 13) are load-bearing for any actual decision.
