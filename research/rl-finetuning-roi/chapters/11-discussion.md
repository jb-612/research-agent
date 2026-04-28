# Chapter 11 — Discussion: Where the Sweet Spot Is, and Where It Isn't

*Date: 2026-04-28 | Synthesis*

## 11.1 What the six streams agree on

Across Streams S1–S6 plus the adversary's stress-test, six findings carry consensus weight:

1. **A workload-shaped envelope of positive ROI for proprietary RL fine-tuning exists today.** The strongest single instance is OpenPipe's ART-E: a 14B Qwen2.5 fine-tuned with GRPO + RULER beating o3 / o4-mini / Gemini 2.5 Pro / GPT-4.1 on email search at 96% accuracy, 1.1 s latency, $0.85 / 1,000 runs, on under $80 of training compute (S6; OpenPipe 2025). Stream S2 generalises the recipe — SFT-distillation at 32B + budget forcing for $25–$1,250, optional second-stage GRPO for $2.5–$12.5K — and Stream S3 closes the financial loop with a parametric break-even at ~500M tokens/month (vs mid-tier APIs) or ~50M tokens/month (vs premium APIs).

2. **The envelope is narrow, conditioned on at least six joint properties of the workload.** Synthesising S2, S3, S4, S6, and the adversary's added condition: the workload must (a) be narrow enough that a 7B–32B specialist can dominate, (b) have a verifiable or LLM-judge-rateable reward, (c) generate enough monthly tokens to amortise the inference engineering, (d) stay stable enough that re-fine-tunes per base-model generation are bearable, (e) permit self-hosted inference, and (f) have a reward function reliable enough to resist specification gaming — including, when LLM-as-judge is used, position-randomisation, multi-judge ensemble, and calibration audit.

3. **Inference engineering, not training, is the durable advantage layer.** A nine-layer compounding stack (compression / attention-architecture / decoding / KV-cache / batching-scheduling / parallelism-kernels / application-caching / I-O-shaping / routing-cost) yields 5–8× cost-efficiency over naive serving (S6, citing Kwon et al. 2023, Li et al. 2025, Zheng et al. 2024). The proprietary-kernel edge of platforms like Fireworks has compressed from ~4× to ~1.5–2× over 18 months as open-source vLLM and SGLang catch up (S3); the durable structural advantage now sits in hardware-software co-design (Groq LPU, Cerebras WSE), which is real but niche (S3, adversary §C3).

4. **Vendor lock-in is in prompt and harness assets, not in model weights.** Empirically: Claude 4 captured 45% of Anthropic's userbase in one month while Sonnet 3.5 dropped from 83% to 16% (S1a) — not the behaviour of a locked-in market. The Opus 4.6 → 4.7 migration documented by Chawla (2026g) names seven specific behaviour categories that break — adaptive thinking, effort tiers, response length, tool-call frequency, sub-agent delegation, instruction-literalness, code-review filtering — every one of them a *prompt-or-harness* asset, not a *weight* asset. Anthropic's own April 2026 Claude Code postmortem (Anthropic Engineering 2026) traces a quality regression to harness changes, not weight changes — independent corroboration.

5. **Multi-tenant RL economics are workable only under a narrow joint condition.** A vendor must (a) own the base model architecture, because no current OpenAI / Anthropic / Vertex contractual terms permit cross-client weight reuse by third parties (S4), and (b) price on outcomes (per-resolution, per-ticket-resolved), not on tokens, to capture the surplus from declining inference costs at renewal — Sierra's $150M ARR, Decagon's $35M ARR, and the a16z December 2024 outcome-based-pricing framework establish the pattern (S4). Federated RLHF remains research-stage with a 2–7 pp performance penalty under realistic non-IID data, rising to >15 pp at ε = 1 differential privacy (S4).

6. **The market has decisively priced foundation-model-training capability as talent, not as IP.** Inflection / Adept / Character.AI license fees were $9–41M per researcher (5–20× normal acqui-hire benchmarks); Adept's $414M training pipeline licensed for $25M; Stability's millions of weight downloads provided no revenue protection at 9:1 debt:cash (S5). Application-layer plays earn 35–100× ARR multiples (Sierra 100×, Harvey 41× at the time of measurement, Glean 36×) on zero proprietary training capability (S5). Aleph Alpha → Cohere confirms sovereignty / regulated-data residency as a third value category (S5).

## 11.2 Where the streams break with the user's prior thesis (A–G)

| Hypothesis | Verdict | Mechanism |
|---|---|---|
| **A** — No vendor lock-in; clients swap models every few weeks. | *Partially refuted, partially upheld.* Lock-in is real but lives in surrounding assets (prompts, harnesses, evals, agent workflows), not in model weights. Median enterprise does not swap weekly; the AI-native cohort does rotate that fast. The relevant cost is *prompt/harness re-engineering*, not model retraining. |
| **B** — RL gives ~5–8% gains, soon erased. | *Refuted in the verifiable-reward regime; partially upheld in the preference-only regime.* DeepSeek-R1-Zero's +55 pp on AIME at frontier scale and ART-E's +56 pp at 14B contradict the "ceiling at 5–8%" reading. But the Harvey BigLaw Bench evidence (adversary §C8; Harvey 2025b) shows that *durability* of gains is short — vanilla base models surpassed the original Harvey fine-tune within ~12–18 months on Harvey's own legal benchmark. So the "soon erased" half of B is closer to true than the "5–8% only" half. |
| **C** — Foundation-cost pass-through destabilises pricing. | *Confirmed.* Token prices fell ~300× over 36 months and continue to compress; pass-through margin is structurally compressed. The viable alternatives are **outcome-based pricing** (transfers token-cost risk from buyer to vendor) and **proprietary-fine-tune-plus-self-host** (captures the inference savings as margin). Both presuppose other conditions in C2 above. |
| **D** — Inference is the real ongoing engineering. | *Strongly confirmed.* The nine-layer optimisation discipline plus the Fireworks/Together/Groq revenue evidence — and crucially the practitioner-side framing that "we use vLLM" is no longer a moat — establish inference as the durable layer. |
| **E** — Multi-tenant RL collapses to cost+. | *Confirmed for third-party-base-fine-tuning; partially circumvented when the vendor owns the model architecture (Cohere) and prices on outcomes.* The Cohere wedge is real but narrow; an SI fine-tuning a third-party base on per-client data cannot legally amortise across clients under any current foundation-vendor terms. |
| **F** — Risk is real (DeepSeek wins co-exist with AI21Labs failures). | *Confirmed.* The risk distribution has a long left tail: Stability's collapse, Inflection's $650M acqui-hire, Aleph Alpha's pivot. The market signal is consistent — model-training-as-business sells at talent value; harness/workflow-as-business sells at product value. |
| **G** — There is an interior optimum (minimum RL spend × maximum gains × solved inference & scale). | *Located, but workload-shaped.* The optimum is not a global maximum; it is a region in (workload-narrowness × verifiability × token-volume × inference-engineering capability) space. Stream S3's break-even model is the closest publicly available formal characterisation of the region. The interior of the region is small; the user's hypothesis G is correct in *form* but more constrained in *extent* than the prose suggested. |

## 11.3 Where the adversary forced revisions

The adversary's stress-test produced four corrections the synthesis would have missed:

**Correction 1 — Harvey's BigLaw Bench is the closest empirical proxy for the missing three-arm durability study, and it lands pessimistic.** Within 12–18 months of Harvey establishing its original fine-tuned system as the BigLaw Bench reference, *seven vanilla base models* (including three non-OpenAI bases) surpassed it on the same benchmark. Base model legal scores rose from ~60% to ~90% on the benchmark. Harvey's own response was to abandon single-fine-tuned-model-as-moat and switch to multi-provider routing (Harvey 2025; Harvey 2025b; TechCrunch 2025). For the user's hypothesis G, this implies the durability window is shorter than Stream S1 implied even in the domain that should hold longest: high vocabulary specificity, high regulatory precision, hard proprietary eval. If durability fails in legal, the durable subset of domains is materially smaller than the streams' framing.

**Correction 2 — The reference open-source RL stack (OpenPipe ART) was acquired by CoreWeave in September 2025.** The user's "interior optimum" recipe — GRPO + RULER + ART — is now CoreWeave-controlled. ART is Apache-2.0 licensed (forkable), but active development priorities now sit with a GPU-infrastructure vendor whose revenue model rewards GPU consumption rather than RL-training-compute efficiency. Any enterprise betting the strategy on ART must treat it as a vendor-dependent toolkit, not a neutral open-source resource (TechCrunch 2025b; CoreWeave 2025).

**Correction 3 — The DP epsilon mapping for regulated industries is sharper than the streams claimed, and it is binding.** HIPAA best-practice guidance for medical fine-tuning recommends ε ≤ 0.5 (preprints.org 2026); the GDPR analogue lands at ε ≤ 5 for high-sensitivity personal data (ABA Jurimetrics 2023). Stream S4's "ε = 8 enterprise-tolerable" floor is therefore *not* an enterprise-tolerable floor for healthcare or banking customer data. The 6–12 pp performance penalty at ε ≤ 1 documented in S4 is the regime regulated buyers actually face, which materially compresses the multi-tenant economics in those industries.

**Correction 4 — The EU AI Act provider-reclassification threshold is now specific and enforceable.** A downstream modifier becomes the new GPAI provider when modification compute exceeds one-third of the original model's training compute (effective 2 August 2025; enforcement 2 August 2026; Arnold & Porter 2025; DLA Piper 2025). For frontier-base fine-tunes, this threshold is approximately 3,300 H100-days — well above the sub-$100 GRPO regime, but binding for any organisation pursuing heavy continued pretraining or large-budget RL on top of an EU-released base. The compliance cost (technical documentation, transparency, copyright compliance, post-market monitoring) is not in any of the streams' TCO models.

**Correction 5 — Application-layer ARR multiples (Sierra 100×, Harvey 41×, Glean 36×) rest on inferred renewal economics, not observed.** Stream S5's market-pricing reading is structurally supported but renewal data does not yet exist in public form (adversary §Q7). The first observable renewal cohort lands in late 2026 / 2027. The valuation theses for application-layer plays should carry a "renewal data not yet observable" qualifier.

## 11.4 The sweet spot, characterised

Synthesising all of the above, the sweet spot for proprietary RL fine-tuning of foundation models in 2026 is the joint region defined by all of the following simultaneously:

1. **Workload narrowness.** A 7B–32B parameter specialist must be capable of clearing the task. Multi-domain or general-capability workloads do not qualify.
2. **Reward verifiability or judge reliability.** Either deterministic verification (math/code/SQL/regex/JSON-schema) or a calibrated LLM-judge reward with position randomisation, multi-judge ensemble, and calibration audit. Naïve LLM-as-judge with self-preference bias is not safe.
3. **Token volume amortisation.** ≥ 50M tokens/month against premium APIs, or ≥ 500M tokens/month against mid-tier APIs, sustained over the 7–14 month payback window. Below these thresholds the inference-engineering headcount cost dominates.
4. **Workflow stability.** The workflow must remain in scope long enough to absorb at least one re-fine-tune per major base-model generation (12–18 months), with the eval suite and prompt assets re-engineered at the same cadence.
5. **Inference-stack capability.** The operator must be able to run a production-grade vLLM / SGLang stack with prefix caching, continuous batching, speculative decoding, multi-LoRA, and model routing. Without this layer, the cost arithmetic of self-hosting falls apart.
6. **Reward-function reliability controls.** Position-randomised judging, multi-judge ensemble, periodic calibration audits against held-out human labels. The Anthropic-documented "natural emergent misalignment from reward hacking in production RL" failure mode is real (Anthropic 2024).
7. **Regulatory regime fit.** ε ≥ 4 differential-privacy regime works for internal corporate workloads; ε ≤ 0.5 (HIPAA) and ε ≤ 5 (GDPR high-sensitivity) regimes do not yet have viable multi-tenant RL solutions; under-1/3-original-compute fine-tuning required to avoid EU AI Act provider reclassification for frontier bases.
8. **Acceptance of a 12–18 month durability window.** The Harvey BigLaw Bench evidence implies that even high-vocabulary-specificity domains do not reliably hold proprietary fine-tune leads beyond one major-version generation. The sweet-spot strategy must include a re-fine-tune pipeline at base-model release cadence, not a "train once, exploit forever" assumption.

A workload that satisfies *all eight* conditions today — the email-search, code-refactor-against-tests, customer-service-triage-with-deterministic-resolution, compliance-rule-application, structured-extraction families — is in the region. A workload that fails any one condition — broad capability requirements, non-verifiable rewards, low token volume, regulated DP, EU heavy fine-tuning — is not.

## 11.5 Where the sweet spot does *not* exist today

The streams identify several apparent candidates that fail one or more conditions:

- **Multi-tenant healthcare AI (under HIPAA ε ≤ 0.5).** The DP penalty exceeds the fine-tuning gain at the required privacy budget; until federated RLHF closes that gap, the economics do not work.
- **Heavy continued pretraining on EU GPAI bases.** EU AI Act reclassification threshold makes the compliance overhead disproportionate to the gain.
- **Per-customer dedicated fine-tuning at small token volumes.** Below ~50M tokens/month against premium APIs, the engineering cost dominates the inference savings, and the per-tenant amortisation pattern (Cohere) requires base-model ownership the operator does not have.
- **Generic "domain adaptation" plays** (e.g., generic legal AI, generic medical AI without verifiable sub-tasks). The Harvey BigLaw Bench data is a warning: the lead may not survive 12–18 months even in the theoretically durable case.
- **Pass-through-pricing wrappers over frontier APIs.** Token-cost decline compresses margin to zero on the renewal cycle; the only sustainable alternatives are outcome-based pricing or proprietary fine-tune + self-host.
- **Single-base "train once, exploit forever" theses.** No empirical base-rate supports them.

## 11.6 The shape of the recommendation that follows

The next chapter (12 — Recommendations and Decision Criteria) translates the eight conditions above into a go/no-go decision tree, an investment envelope ($-range and team size), and a list of the leading indicators that would shift the analysis. The conclusion (Chapter 13) summarises the position for the user's strategic question and names the things that would change it.

The synthesis does not say *fine-tune everything*. It does not say *fine-tune nothing*. It says: there is a workload-shaped, condition-bounded interior optimum, locatable now, but smaller than enthusiast framing suggests and shorter-lived than vendors imply. Hypothesis G is correct in form but more constrained in extent than its prose. The user's strategic position should be calibrated to that shape.

## References

See the consolidated bibliography in `99-bibliography.md` (annex). All citations in this chapter are resolved there.
