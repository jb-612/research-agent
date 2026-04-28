# Research Charter — ROI of RL Fine-Tuning of Foundation Models

**Status:** Draft v0.1
**Date:** 2026-04-28
**Author:** research-agent (orchestrator) + sub-agents
**Decision target:** Should an enterprise / SI / vertical-AI vendor invest in proprietary RL fine-tuning of foundation models? If so, where is the economic sweet spot?

---

## 1. Central Research Question

> Is there an economically viable "sweet spot" for proprietary RL-based fine-tuning of foundation models that produces durable competitive advantage proportional to investment, or is RL fine-tuning a strategic dead-end given current market dynamics?

If a sweet spot exists, characterize it: what investment envelope, on which model classes, for which use-cases, with what inference-side complementary work, in what business model.

If it does not yet exist, articulate the conditions under which it would — i.e., which technological or market shifts would unlock it.

## 2. Scope

**In scope** — RL-based and adjacent post-training techniques applied to open-weight or licensed foundation models:

- Reinforcement Learning from Human Feedback (RLHF)
- Reinforcement Learning from AI Feedback (RLAIF) / Constitutional AI
- Direct Preference Optimization (DPO) and variants (IPO, KTO, ORPO)
- Group Relative Policy Optimization (GRPO) and reasoning-RL methods
- Reinforcement Fine-Tuning (RFT, OpenAI/Anthropic/Google APIs)
- Process Reward Models (PRM) and verifier-based RL
- Supervised Fine-Tuning (SFT) **only** as a comparator / baseline

**In scope — non-training** axes the user flagged as decisive:

- Inference optimization (caching, KV-cache management, speculative decoding, quantization, hardware co-design)
- Business model architecture (cost+, SaaS, fixed-price, observability tradeoffs)
- Scaling economics across enterprise multi-tenant constraints

**Out of scope** for this iteration:
- Pretraining from scratch
- Continued pretraining on domain corpora (only briefly compared)
- Pure prompt engineering / context engineering (treated as the null baseline)
- Safety/alignment research as a standalone topic (only where it intersects with ROI)

## 3. Working Hypotheses (User-Provided, A–G)

The user has articulated seven prior beliefs. Each is a falsifiable claim; the research must collect evidence pro/contra and rate confidence.

| # | Hypothesis | Falsifiable as |
|---|-----------|----------------|
| **A** | No vendor lock-in: clients swap foundation models every few weeks with no migration cost. | If empirical migration costs are documented as material (>2 weeks engineering, regression risk), A is weakened. |
| **B** | RL on open-source models yields ~5–8% domain gains, soon erased by the next hyperscaler release. | If durable gains >12 months exist on a non-trivial benchmark fraction, B is weakened. |
| **C** | Encapsulating foundry-model cost destabilizes pricing; transparent observability invites disintermediation. | If a stable, profitable pricing pattern exists at scale, C is weakened. |
| **D** | Fine-tuning is a starting point; the real ongoing engineering is inference (caching, sessions, hardware). | If empirical post-training accounts for most of TCO at scale, D is weakened. |
| **E** | Enterprise dedicated training collapses to cost+ because data isolation prevents cross-client amortization. | If federated / synthetic-data approaches yield production-grade cross-client gains without contractual risk, E is weakened. |
| **F** | Risk is real: cheap-model successes (DeepSeek) co-exist with capital-loss case studies (AI21Labs). | The base-rate of failure / ROI distribution to be characterized. |
| **G** | This is an optimization problem with an interior optimum: minimum RL spend × maximum reasoning gains × solved inference & scale. | The research must either locate this optimum or formally argue no interior optimum exists today. |

## 4. Methodology

**Two-tier evidence model** (per user instruction on academic rigor):

- **Tier 1 — Q1 peer-reviewed:** NeurIPS, ICML, ICLR, ACL, EMNLP, JMLR, TMLR, Nature MI, Science Advances. Used for empirical claims about training dynamics, gain magnitudes, durability.
- **Tier 2 — High-quality grey:** arXiv pre-prints from major labs (DeepMind, Anthropic, OpenAI, Meta FAIR, MSR, Google Brain), HuggingFace technical reports, EleutherAI, StanfordHAI annual reports, McKinsey/BCG/Bain genAI reports, Andreessen Horowitz, SemiAnalysis, Epoch AI, MLPerf benchmarks.
- **Tier 3 — Reputable trade press:** The Information, Wired, MIT Tech Review, IEEE Spectrum, FT, WSJ on AI economics.
- **Tier 4 — Vendor / advocacy:** flagged but not load-bearing.

**Citation rules:**

1. Every empirical claim must carry a citation.
2. Every cited source must be verified to exist (URL resolves, paper indexed, or quote retrievable).
3. Numbers (e.g., "X% gain", "Y× cost") must trace to a primary source, not a secondary citation.
4. When two reputable sources disagree, both are cited and the disagreement is surfaced.
5. Citation style: **Chicago author–date** (in-line `(Author Year)`) with full bibliography in `99-bibliography.md` annex.
6. Hallucination guard: a final citation-audit pass re-fetches every citation; broken/inventented refs are removed and the corresponding claim weakened or dropped.

**Phase plan:**

| Phase | Output | Owner |
|-------|--------|-------|
| 1a — Market scan | `phase1-scan/market-scan.md` | sub-agent (general-purpose, `model: sonnet`) |
| 1b — Academic scan | `phase1-scan/academic-scan.md` | sub-agent (general-purpose, `model: sonnet`) |
| 2 — Stream selection | `phase2-deep/stream-brief.md` | orchestrator |
| 3 — Deep research per stream | `phase2-deep/stream-{n}-{slug}.md` | one sub-agent per stream (parallel) |
| 4 — Adversary challenge | `phase3-adversary/adversary-report.md` | adversary sub-agent |
| 5 — Synthesis & conclusions | `chapters/01..NN-*.md` | orchestrator |
| HITL gate | user review | user |
| Final | merged PDF + annex | pdf-export skill |

## 5. Chapter Structure (target — final report)

```
chapters/
  01-executive-summary.md
  02-introduction-and-question.md
  03-landscape-rl-finetuning-2024-2026.md
  04-stream-1-durability-of-gains.md
  05-stream-2-inference-moat.md
  06-stream-3-business-model-viability.md
  07-stream-4-multi-tenant-scale-economics.md
  08-adversary-and-counterarguments.md
  09-discussion-the-sweet-spot-or-its-absence.md
  10-recommendations-and-decision-criteria.md
  11-conclusion.md
99-bibliography.md
```

Each chapter is a standalone `.md` to keep agent context windows tight. The PDF export concatenates them in order plus the bibliography annex.

## 6. Decision Criteria — what would constitute an answer

The final report must answer four crisp questions for the reader:

1. **Does a sweet spot exist today?** Yes / No / Conditional, with confidence rating.
2. **For whom?** Profile the buyer/builder where ROI is plausibly positive (segment, use-case, capability stack).
3. **At what investment envelope?** A defensible $-range and team-size range, broken down: data, compute, RL engineering, evaluation, ongoing inference engineering.
4. **What kills it?** The 3–5 conditions that destroy ROI (e.g., next-gen base model release, pricing-model collapse, data-isolation contract).

A "no sweet spot today" answer is acceptable — but must include the leading indicators that would flip the conclusion.

## 7. Risks to the Research

- **Recency bias.** The field shifts quarterly. Sources >12 months old may already be wrong on cost numbers.
- **Selection bias in case studies.** Successful proprietary fine-tuners publicize; failures are silent. Adversary phase must search for failures.
- **Vendor narrative.** Hyperscalers and OSS labs each have incentive narratives. Tier-1 sources weighted more heavily.
- **Cost-figure inflation/deflation.** GPU rental, power, and salary numbers have moved >2× in 24 months. Always cite the price-as-of date.

---

*End of charter. Phase 1 launches next.*
