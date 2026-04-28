# Chapter 3 — Landscape: RL Fine-Tuning of Foundation Models, 2024–2026

*Date: 2026-04-28 | Compressed from Phase-1 market scan (S1a) and academic scan (S1b)*

This chapter synthesises the Phase-1 landscape view — what shipped, what won, what failed, what the academic literature established — to set context for the deep-research chapters that follow. The full Phase-1 scans are at `phase1-scan/market-scan.md` and `phase1-scan/academic-scan.md`.

## 3.1 The market shape — eight signals dominating Q2 2026

1. **Token prices have collapsed ~300× over 36 months.** GPT-4 launched at $30/M input tokens (March 2023); GPT-5.4 sits at $2.50/M (March 2026); DeepSeek V4-Flash at $0.28/M output (April 2026). The decline is structural, driven primarily by the inference serving stack (Epoch AI 2025; tokencost.app 2026).

2. **Frontier model release cadence is sub-quarterly.** Between January 2025 and April 2026, at least twelve frontier or near-frontier model families shipped major versions: Claude 3.7 / 4 / 4.5 / Opus 4.6 / Opus 4.7; GPT-5 / 5.4; Gemini 1.5 / 3 / 3.1; Llama 4; DeepSeek R1 / V4; Mistral Small 3; Kimi K2.6 (Menlo Ventures 2025; Chawla 2026g; Moonshot AI 2026).

3. **Enterprise fine-tuning adoption declined in the SFT regime.** The a16z 2025 CIO survey of 100 verified Global 2000 executives found enterprises trending *away* from fine-tuning toward prompt engineering and context injection, with long context windows cited as the displacement mechanism (Andreessen Horowitz 2025). The survey reflects 2023–2024 SFT-stack experience; it predates the 2026 GRPO + RULER stack.

4. **The inference layer outpaced the post-training layer commercially.** Fireworks AI: $315M ARR at 416% YoY (Feb 2026); Together AI: $150M+ ARR with $305M Series B; both pure-inference plays (Sacra 2025). Groq's LPU acquired by NVIDIA (Jan 2026). Cerebras WSE for steady-throughput inference. The platform layer has out-grown the training-platform layer.

5. **Vertical AI winners derive moats from workflow lock-in, not from model training.** Harvey ($195M ARR / $11B valuation, March 2026) deliberately multi-homed across OpenAI / Anthropic / Google to *reduce* model dependency, framing the moat as 25,000+ workflow agents and the proprietary legal eval (Harvey 2025; Sacra). Sierra ($150M ARR Jan 2026, 5.8× growth in 12 months) and Glean ($208M ARR, 89% YoY) followed parallel patterns.

6. **Federated RLHF exists as research; no productionised multi-tenant RL with contractual data isolation has been publicly documented at enterprise scale.** FedRLHF (Dec 2024 arXiv), FedBiscuit (ICLR 2025), LoRA-FAIR (ICCV 2025) are academic. Cohere's RBC / Fujitsu synthetic-data pattern is the closest production-adjacent approach; it requires Cohere to own the model.

7. **The model-training "dead end" case studies are instructive.** Stability AI: $99M compute / $11M revenue in 2023; defaulted on AWS / GCP / CoreWeave GPU debt; CEO resigned March 2024. Inflection: acqui-hired by Microsoft for ~$650M (March 2024). Adept: licensed to Amazon for ~$25M plus investor recoupment. Aleph Alpha: pivoted away from model building, then acquired by Cohere (April 2026). AI21 Labs: rescue Series D ($300M, May 2025).

8. **Reasoning RL (DeepSeek R1, GRPO, RLVR) is the methodological frontier from 2024–2025.** DeepSeek-R1-Zero achieved +55 pp on AIME 2024 (15.6% → 71.0%) via pure GRPO + verifiable rewards on a 600B MoE base, reaching o1-level performance with no human-labelled reasoning traces (DeepSeek-AI 2025). The reasoning-RL line has dominated capability-frontier work ever since.

## 3.2 The academic shape — seven empirical anchors

1. **Verifiable-reward RL produces qualitatively larger gains than preference RLHF.** Classical RLHF is bounded by reward-model overoptimisation (Goodhart's law; Gao et al. ICML 2023). RLVR sidesteps this by grounding feedback in deterministic verifiability. TÜLU 3 (AI2, Nov 2024) confirms RLVR adds targeted gains on GSM8K and IFEval without harming other tasks.

2. **DPO matches or exceeds PPO on dialogue alignment at lower compute.** Rafailov et al. (NeurIPS 2023). However, PPO retains the edge on verifiable code-generation tasks where the value model can learn (Liu et al. ICML 2024).

3. **GRPO collapses RLHF's four-model setup to two.** Shao et al. (DeepSeekMath, arXiv 2402.03300, 2024) introduced the group-relative-advantage estimator that eliminates the critic. DeepSeek-R1 (DeepSeek-AI Nature 2025) scaled the technique to 600B MoE with binary rewards.

4. **Process Reward Models substantially amplify RL gains.** Lightman et al. (ICLR 2024) "Let's Verify Step by Step" — 78% on MATH via step-level supervision. Math-Shepherd (Wang et al., ACL 2024) automated PRM annotation; Mistral 7B from 77.9% to 84.1% on GSM8K.

5. **Distillation can substitute for RL when the target task is well-represented.** DeepSeek-R1-Distill-Qwen-7B reaches 55.5% on AIME 2024 via SFT alone. The s1 paper (Muennighoff et al. EMNLP 2025) fine-tuned Qwen2.5-32B-Instruct on 1,000 traces in 32 H100-hours and exceeded o1-preview on competition math by up to 27%.

6. **Inference optimisation compounds.** PagedAttention (Kwon et al. SOSP 2023): 2–4× throughput. EAGLE-3 (Li et al. NeurIPS 2025): 4.1–6.5× speedup. SGLang RadixAttention (Zheng et al. NeurIPS 2024): up to 6.4× in prefix-heavy workloads. Multi-LoRA collapses adapter inference cost towards zero per tenant.

7. **Diff-vector recycling preserves gains across minor-version base-model upgrades.** Lin et al. (EMNLP 2025) demonstrate +46.9 pp IFEval, +15.7 pp LiveCodeBench, +10.7 pp GPQA when transferring Llama 3.0 fine-tune weights to Llama 3.1 base without re-training. *Major-version transfer is untested in the published literature.*

## 3.3 The methodological shift, in one sentence

Between January 2025 and April 2026, the operative RL fine-tuning recipe shifted from *human-preference RLHF + custom rewards + manual data curation* to *verifiable-reward GRPO + automated LLM-as-judge + curated synthetic traces*, collapsing the soft-cost barrier to RL fine-tuning by roughly an order of magnitude and shifting the open empirical question from "can RL produce gains" to "do those gains survive base-model upgrades and amortise across customers."

The remaining chapters address that shifted question.
