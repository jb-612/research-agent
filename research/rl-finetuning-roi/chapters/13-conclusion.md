# Chapter 13 — Conclusion

*Date: 2026-04-28*

## 13.1 The position the research supports

The user opened this research with seven prior beliefs (assumptions A–G) and one strategic question: is there an economically viable sweet spot for proprietary RL fine-tuning of foundation models, or is it a strategic dead-end? After six parallel deep-research streams, an adversarial review, and synthesis across four corroborating evidence channels (academic Q1 literature, market analyst output, practitioner curation, and live commercial benchmarks), the answer is:

> **A workload-shaped sweet spot exists in 2026. It is smaller than enthusiast framing suggests, narrower than the user's hypothesis G prose implied, and shorter-lived than vendor narratives admit. But it is real, locatable, and economically defensible — under eight specific joint conditions, on a recipe that is now technically mature, with an investment envelope a small team can carry.**

The position the user should adopt:

- **The thesis is not "fine-tune everything."** The bet is on a specific eight-condition envelope (Chapter 11 §11.4; Chapter 12 §12.1).
- **The thesis is not "fine-tune nothing."** The empirical evidence — DeepSeek-R1's emergent reasoning at frontier scale, OpenPipe's ART-E at 14B, the s1 distillation result — places the sweet spot inside the envelope unambiguously.
- **The thesis is "fine-tune within the envelope, on a renewable cadence, and capture the surplus on the inference and harness side, not on the weights."** Weights have repeatedly priced at near-zero standalone (Adept's $414M pipeline → $25M license; Stability's open-source weights → $0 revenue protection). The harness-and-inference layer is where the durable margin sits.

## 13.2 What the user got right in the prior thesis

Six of the seven A–G hypotheses survived in some form.

- **A** (vendor lock-in low) was *partially* correct: lock-in is real but in prompt and harness assets, not in model weights. The framing was right; the locus was off.
- **C** (pass-through pricing destabilises) was *confirmed*: 300× token-price decline in 36 months has compressed pass-through margins to zero on renewal cycles. The viable alternatives are outcome-based pricing or proprietary fine-tune + self-host.
- **D** (inference is the real ongoing engineering moat) was *strongly confirmed*: the nine-layer optimisation discipline yields 5–8× compounding gains; Fireworks ($315M ARR / 416% YoY) and the broader inference-platform layer carry more durable revenue than any pure post-training play.
- **E** (multi-tenant collapses to cost+) was *largely confirmed*, with the Cohere synthetic-data wedge as a narrow circumvention requiring base-model ownership.
- **F** (risk is real) was *confirmed* through the acqui-hire pattern (Inflection / Adept / Aleph Alpha at $9–41M per researcher) and the application-layer outperformance (Sierra / Harvey / Glean at 35–100× ARR).
- **G** (interior optimum exists) was *located* but more constrained in extent than the prose suggested.

## 13.3 What the user got wrong (or under-specified)

- **B** (5–8% gains, soon erased) was *refuted in the verifiable-reward regime*. DeepSeek-R1-Zero produced a +55 pp emergence at frontier scale; ART-E produced +56 pp at 14B. The "5–8% only" framing was correct for 2023-vintage SFT and preference-only RLHF on weak verifiers; it was wrong for 2026 GRPO + RULER + verifiable rewards. **However**, the "soon erased" half of B is closer to true than the streams' optimistic reading: Harvey's BigLaw Bench data shows seven vanilla base models surpassing the original Harvey fine-tune within 12–18 months in the legal domain — the domain that should hold durability longest.

- **G** was correctly intuited as an optimisation problem with an interior solution. What the prose missed:
  - The optimum is *workload-shaped*, not global.
  - The eight conditions are *joint*, not severable.
  - The durability window is *short* (12–18 months per fine-tune cycle), making the optimum a moving target rather than a fixed asset.
  - The reference open-source recipe (OpenPipe ART) was acquired by CoreWeave in September 2025 — the recipe is not vendor-neutral.

## 13.4 Timing — when the sweet spot opens, narrows, or closes

The synthesis identifies five timing-sensitive forces. The user's strategic position should be calibrated to each:

1. **Inference-cost decline rate.** Continued at 70%/yr → break-even at 14 months (S3 central case). Decelerated to 30–40%/yr (energy floor + hardware stabilisation) → break-even at 24–32 months. Watch H100 spot prices, GW-grid interconnection capacity, and the Epoch AI inference price index quarterly.

2. **Base-model release cadence.** Sub-quarterly through 2025–2026 (S1a). Each release shortens the durability window of the previous fine-tune by approximately one cycle. If cadence accelerates further, the re-fine-tune line item in §12.3 grows; if it decelerates (e.g., due to compute / capital constraints at the labs), the durability window expands.

3. **Open-source inference-stack catch-up vs proprietary kernels.** FireAttention's edge has compressed from 4× to 1.5–2× over 18 months. If vLLM / SGLang reach parity in 2026, the inference moat for managed-platform vendors disappears — but it *expands* for self-hosters with the engineering capability to run the stack. The asymmetry favours operators with their own inference team.

4. **EU AI Act enforcement.** Effective 2 August 2025; first enforcement landings expected from 2 August 2026. EU operators must monitor the 1/3 training-compute threshold; non-EU operators get a competitive window of relative compliance simplicity.

5. **First publicly observable Sierra / Harvey / Glean renewal cohort.** Late 2026 / 2027. If renewal rates hold at >100% net revenue retention, the application-layer ARR-multiple pricing is validated and the harness-as-moat thesis strengthens. If they collapse, the synthesis's "harness is depreciating, requires perpetual maintenance" framing tightens.

The next 12 months are the critical window: enough open-source maturity to execute the recipe, before energy constraints reshape the cost curve, and before the first regulatory enforcement landings clarify the EU compliance burden.

## 13.5 The single sharpest thing to remember

**The market repeatedly prices model-training capability as talent, not as IP.** Adept's $414M training pipeline licensed for $25M. Inflection's $650M deal split per-researcher across 70 people = $9M each. Reka's proposed $41M per-researcher valuation. Stability's millions of open-source weight downloads producing zero revenue protection at 9:1 debt:cash.

The implication for any strategic position: **do not bet on the weights**. Bet on the eval, the harness, the inference stack, the customer relationships, and the workflow-completion economics — the things that compound and the things that survive a re-fine-tune cycle. The fine-tune itself is a renewable input, not a fixed asset.

The user's hypothesis G framed the question as an optimisation problem. The synthesis confirms it, with the precision the question needs: the variables that compound (eval, harness, inference, customer-workflow lock-in) get the investment; the variable that depreciates (the fine-tuned model weights) gets the cycle budget — not the strategic bet.

## 13.6 What's next

This research enters HITL review at this point (Phase 6 of the original plan). The user should:

1. **Read Chapter 11 (Discussion) and Chapter 12 (Recommendations).** These two chapters are load-bearing for any decision.
2. **Decide which side of the eight-condition decision tree the user's specific workload sits on.** If all eight conditions are met → execute the recipe. If any is not → stop, and revisit when the failing indicator changes.
3. **Decide on the two leading indicators that matter most to the user's specific position** (likely: the renewal-cohort data for the application-layer thesis, and the EU AI Act enforcement landing for jurisdictional planning) and instrument quarterly monitoring of them.
4. **Edit the discussion / recommendations chapters** where the user's domain-specific knowledge would add weight — particularly on workload-narrowness assessment and on customer-renewal expectations.
5. **Approve the editorial pass and PDF export** once the chapters reflect the user's position.

The synthesis is positioned to be wrong in the conservative direction: every claim has been weighed against its strongest counter-evidence, and where the evidence is genuinely missing (no three-arm durability study; no ART-E independent replication; no Sierra/Harvey/Glean renewal cohort yet), the synthesis says so explicitly. If the user disagrees with any conclusion, the source chapters and the adversary report give the audit trail to the underlying primary evidence.

## 13.7 Closing

The opportunity the user sensed at the start of this research — an under-priced sweet spot for proprietary RL fine-tuning available to operators willing to do the engineering — is real. It is not the entire market. It is not even most of the market. But it is a defensible region for a small team with the right workload, the right eval, the right inference engineering, and the patience to renew the fine-tune at base-model cadence rather than treating it as a permanent moat. The window is open. The eight conditions are knowable in advance. And the indicators that would close the window are identifiable and trackable.

That is the answer. Hypothesis G is correct in form, narrower in extent, and time-sensitive in execution. Position accordingly.

## References

See the consolidated bibliography in `99-bibliography.md` (annex).
