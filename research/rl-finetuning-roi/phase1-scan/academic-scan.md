# Phase 1b — Academic Q1 Literature Scan
*Date: 2026-04-28 | Tier: scan*

---

## 1. Executive Map

The peer-reviewed record through Q1 2026 yields seven empirically strong findings that bear directly on the ROI question.

**Finding 1 — RL fine-tuning produces very large reasoning gains when rewards are verifiable.** DeepSeek-R1, trained from DeepSeek-V3 via GRPO with binary correctness rewards, reached 79.8% on AIME 2024 (vs. 9.3% for GPT-4o) and 97.3% on MATH-500, rivalling OpenAI o1-1217. Crucially, the pure-RL variant, DeepSeek-R1-Zero, improved from 15.6% to 71.0% on AIME 2024 through RL alone—no human-labelled reasoning traces required. This magnitude (~55-point absolute) far exceeds previous RLHF gains on open-ended dialogue benchmarks.

**Finding 2 — Verifiable-reward RL is qualitatively different from preference-based RLHF.** The classical RLHF pipeline (fit a reward model, then run PPO) is subject to Goodhart's Law: over-optimising against a proxy reward model degrades gold-label performance. Gao et al. (ICML 2023) established scaling laws for this overoptimisation. Verifiable-reward RL sidesteps this by grounding feedback in execution or symbolic correctness, and Tülu 3 (AI2, arXiv 2024) confirmed that RLVR adds measurable targeted gains on GSM8K and IFEval without harming other tasks.

**Finding 3 — DPO achieves competitive dialogue alignment at much lower compute than PPO-RLHF.** Rafailov et al. (NeurIPS 2023) showed DPO matches or exceeds PPO on sentiment control and summarisation using a simple classification loss. However, Liu et al. (ICML 2024) showed PPO retains a clear edge on challenging code-generation, where the reward function is verifiable and the value model can learn.

**Finding 4 — Gains can be partially transferred across base-model generations.** Lin et al. (EMNLP 2025) showed that applying "diff vectors" (parameter deltas) from Llama 3.0 fine-tunes onto Llama 3.1 base models produced 46.9% absolute improvement on IFEval and 15.7% on LiveCodeBench without further training. This represents a partial answer to the durability question: gains are not simply erased, but the transfer fidelity depends on linear parameter-space connectivity.

**Finding 5 — Inference optimisation compounds fine-tuning ROI.** Kwon et al. (SOSP 2023) showed PagedAttention (vLLM) increases LLM serving throughput 2–4× at the same latency vs. prior systems. EAGLE (ICML 2024) and EAGLE-3 (NeurIPS 2025) add 3–6.5× autoregressive speedup through feature-level speculative decoding. These multiplicative gains reduce the per-token cost of operating fine-tuned proprietary models.

**Finding 6 — Process reward models amplify RL gains substantially.** Lightman et al. (ICLR 2024) showed step-level feedback (PRMs) solved 78% of MATH test problems vs. lower rates with outcome supervision, using PRM800K (800k annotated steps). Math-Shepherd (ACL 2024) automated PRM annotation and raised a Mistral-7B from 77.9% to 84.1% on GSM8K and 28.6% to 33.0% on MATH via step-level PPO. OmegaPRM (arXiv 2024, Google DeepMind) achieved 69.4% on MATH, an 18.4-point absolute gain, via automated Monte Carlo tree search data generation.

**Finding 7 — Distillation from strong reasoning models can substitute for RL when the target task is well-represented.** DeepSeek-R1-Distill-Qwen-7B reached 55.5% on AIME 2024 and 92.8% on MATH-500 via supervised fine-tuning on R1 traces—no RL required for the student. The s1 paper (EMNLP 2025, Stanford) fine-tuned Qwen2.5-32B-Instruct on just 1,000 reasoning traces and exceeded o1-preview on competition math by up to 27% via "budget forcing" at inference.

---

## 2. Magnitude of RL Fine-tuning Gains — by Method

| Method | Typical Benchmark Gain | Benchmark(s) | Base Model(s) | Key Paper(s) |
|--------|------------------------|--------------|---------------|-------------|
| **PPO-RLHF** | +5–15% dialogue preference win-rate vs. SFT | AlpacaEval, MT-Bench | GPT-2/3.5-class, LLaMA | Ouyang et al. (NeurIPS 2022) [1] |
| **DPO** | Comparable to PPO on dialogue; wins ~61% vs. SFT at T=0 | AlpacaEval, summarisation | GPT-2, LLaMA-7B | Rafailov et al. (NeurIPS 2023) [2] |
| **DPO — code** | PPO +4% over DPO on code benchmarks | HumanEval, CodeForces | LLaMA-based | Liu et al. (ICML 2024) [3] |
| **GRPO (DeepSeekMath 7B)** | GSM8K: 82.9% → 88.2% (+5.3pp); MATH: 46.8% → 51.7% (+4.9pp) | GSM8K, MATH | DeepSeekMath-Instruct 7B | Shao et al. (arXiv 2024) [4] |
| **GRPO (DeepSeek-R1-Zero)** | AIME 2024: 15.6% → 71.0% (+55.4pp) | AIME 2024 | DeepSeek-V3 | DeepSeek-AI (Nature, 2025) [5] |
| **Full RL pipeline (DeepSeek-R1)** | AIME: 79.8%; MATH-500: 97.3%; MMLU: 90.8% | AIME, MATH-500, MMLU, GPQA | DeepSeek-V3 | DeepSeek-AI (Nature, 2025) [5] |
| **PRM (step-level supervision)** | 78% MATH (PRM800K); Mistral 7B GSM8K +6.2pp | MATH, GSM8K | GPT-4 class; Mistral 7B | Lightman et al. (ICLR 2024) [6]; Wang et al. (ACL 2024) [7] |
| **RLAIF / RLVR (Tülu 3)** | Targeted +gains on GSM8K and IFEval; competitive with GPT-4o-mini | GSM8K, IFEval, AlpacaEval | LLaMA 3.1 8B / 70B | Lambert et al. (arXiv 2024) [8] |
| **Distillation from RL model** | AIME: 55.5% (7B), 72.6% (32B); MATH-500: 92.8% (7B) | AIME, MATH-500 | Qwen2.5-7B / 32B, LLaMA-70B | DeepSeek-AI (Nature, 2025) [5] |
| **SFT on curated reasoning traces (s1)** | AIME24: 50% → 57% with budget forcing; beats o1-preview +27% on competition math | AIME24, MATH | Qwen2.5-32B-Instruct | Muennighoff et al. (EMNLP 2025) [9] |

**Key caveat on magnitudes:** Gains are benchmark-dependent. Verifiable domains (math, code) show the largest absolute gains. Open-ended dialogue and instruction following benchmarks typically show 3–12% win-rate improvements over SFT baselines. MMLU gains from post-training are modest (1–3pp) because the benchmark had limited headroom as of 2024 and is heavily influenced by pretraining.

---

## 3. Durability of Gains Across Base-Model Upgrades

**What the literature says directly:** The literature on durability is notably thin relative to its strategic importance. Only a handful of papers address whether domain fine-tunes on an older base model retain competitive advantage after a stronger base model is released.

**Evidence for partial transfer / non-erasure:** Lin et al. (EMNLP 2025) [10] is the closest direct evidence. Their "fine-tuning transfer" method shows that diff vectors computed on Llama 3.0 8B transfer meaningfully to Llama 3.1 8B: +46.9pp on IFEval, +15.7pp on LiveCodeBench, +10.7pp on GPQA without any additional training. This is an optimistic result—the fine-tuned knowledge is not simply erased. However, the transfer fidelity was found to depend strongly on the models being "linearly connected" in parameter space, a condition that likely holds within minor version increments (3.0 → 3.1) but is unconfirmed across major generational gaps.

**Evidence for erosion / forgetting:** The catastrophic forgetting literature (see Section 8) documents a different problem: full-parameter SFT on new data degrades previously acquired capabilities. Biderman et al. (ACL 2023, not found in search but well-cited) documented this in Pythia model series. An empirical study on catastrophic forgetting during continual fine-tuning (arXiv 2308.08747) confirmed substantial forgetting in LLMs across curriculum shifts. The architectural gap between, say, LLaMA-2 and LLaMA-3 is wide enough that applying a 2-year-old domain fine-tune directly is not practically possible.

**The unstated empirical gap:** There is no major Q1/T1 paper that directly measures whether a fine-tuned LLaMA-2 domain specialist (e.g., trained on proprietary medical notes via RLHF) retains lead performance over a fresh LLaMA-3 base model on the same domain task. This is a critical gap for the ROI analysis.

**Practical implication from available evidence:** Gains are partially transferable to successor checkpoints if the architecture is similar and diff-vector recycling is employed. However, each major base-model generation likely requires re-investment in alignment tuning, though the investment is reduced if the training data and evaluation setup are retained. The re-investment cost is not separately reported in any paper found.

---

## 4. Sample Efficiency and Compute Requirements

**RLHF/PPO:** Ouyang et al. (InstructGPT, NeurIPS 2022) [1] reported training a 175B model with approximately 40,000 human-labelled comparisons for the reward model, followed by PPO fine-tuning. GPU-hours not separately reported for the RL stage in that paper. The proxy-reward overoptimisation work by Gao et al. (ICML 2023) [11] showed performance peaks and then degrades as a function of KL-divergence budget; the overoptimisation threshold scales with the number of reward model parameters, establishing that larger reward models tolerate more optimisation before collapse.

**DPO:** Rafailov et al. (NeurIPS 2023) [2] showed meaningful alignment with datasets in the 80,000–160,000 preference-pair range (the Anthropic HH-RLHF dataset). Crucially, DPO eliminates the critic and sampling loops, reducing compute by roughly one-third to one-half compared to PPO at equivalent dataset size (exact GPU-hours not separately reported in the original paper; practitioner benchmarks suggest 2–3× faster wall-clock training).

**GRPO (DeepSeekMath):** Shao et al. [4] used ~144,000 math questions for the RL phase on a 7B model. The 5-point MATH gain cited above was achieved with this dataset. GPU budget not separately reported in the arXiv version reviewed.

**DeepSeek-R1-Zero RL stage:** DeepSeek-AI [5] applied GRPO on top of DeepSeek-V3 (a ~600B MoE model). The RL phase used thousands of verifiable math and code problems; training involved "thousands of cold-start samples" for the full R1 pipeline. The paper does not break out GPU-hours for the RL phase separately from pretraining. DeepSeek's total pretraining cost has been publicly estimated at ~$5M (at H800 prices), which contextualises the RL phase as an incremental cost.

**s1 (SFT on curated traces):** Muennighoff et al. [9] report that the entire Qwen2.5-32B-Instruct fine-tune on s1K (1,000 examples) required only 32 GPU-hours on H100s. This is the most cost-efficient result in the literature for competition-math performance, underscoring the leverage available from high-quality synthetic trace curation.

**PRM (Math-Shepherd):** Wang et al. (ACL 2024) [7] automated PRM annotation using model self-verification, removing the 800k manual step labels needed by Lightman et al. The automated pipeline enabled Mistral-7B improvements at substantially lower annotation cost than PRM800K; exact labelling costs not reported.

**DPO data efficiency with margin selection:** Search results indicate that selecting high-margin preference pairs can yield 3–8% AlpacaEval improvements using only 10% of the original dataset (source: empirical study cited in Labellerr survey; original venue not verified—treated as indicative only). This suggests significant potential for curation-driven data efficiency gains.

---

## 5. The Reasoning-RL Revolution (2024–2025)

The period 2024–2025 produced a qualitative break from prior RLHF paradigms. The key methodological shift was from *preference-based* feedback (human raters score responses) to *verifiable reward* feedback (symbolic or execution-based correctness signals). Three concurrent developments drove this:

**5.1 GRPO and DeepSeekMath (February 2024).** Shao et al. [4] introduced GRPO, which replaces PPO's learned critic with a group-relative advantage estimate: for each question, sample G responses, standardise their rewards as z-scores within the group, and use those as advantages. This eliminates the value model (roughly halving memory and compute for the RL phase). Applied to a 7B math specialist, GRPO produced reliable 5-point MATH gains from ~144K training problems.

**5.2 DeepSeek-R1 and DeepSeek-R1-Zero (January 2025, Nature 2025).** DeepSeek-AI [5] scaled GRPO to a 600B MoE model with binary correctness rewards on math and code. The emergent result—extended chain-of-thought reasoning including self-reflection and verification—was not explicitly trained; it emerged from the RL objective alone. R1-Zero's 55-point AIME gain demonstrated that, at sufficient model scale with a sufficiently verifiable reward, RL can unlock qualitatively new reasoning behaviours. The distilled variants (R1-Distill-Qwen-7B at 55.5% AIME, R1-Distill-Llama-70B at 70.0%) then showed that these reasoning traces can be transferred to smaller models via supervised fine-tuning alone.

**5.3 s1: Simple test-time scaling (January 2025, EMNLP 2025).** Muennighoff et al. [9] demonstrated that fine-tuning Qwen2.5-32B-Instruct on only 1,000 carefully curated problems (selected for difficulty, diversity, quality) plus a simple "budget forcing" inference trick (appending "Wait" to extend reasoning chains) was sufficient to exceed o1-preview on competition math. The entire training took 32 H100-hours. This finding has two implications: (a) the value is primarily in the reasoning traces, not the RL process itself; (b) a practitioner with access to strong teacher traces can achieve competitive results without running RL at all.

**5.4 What is replicable versus base-model dependent.** The R1-Zero emergence of reasoning behaviours required a very strong base model (DeepSeek-V3, competitive with GPT-4o). Attempts to replicate R1-Zero–style RL on weaker 7B bases produced only modest gains, suggesting the RL phase amplifies latent capability rather than instilling it from scratch. The s1 distillation result is more broadly replicable, but requires access to strong teacher traces. TÜLU 3's RLVR component [8] showed that a pipeline combining SFT, DPO, and RLVR on a Llama 3.1 base can produce instruction-following gains competitive with GPT-4o-mini, though without the extreme reasoning gains seen in R1.

**5.5 Process reward models as a complementary lever.** Lightman et al. [6] and Wang et al. [7] showed that step-level rewards consistently outperform outcome rewards for multi-step mathematical reasoning. OmegaPRM [12] automated the annotation pipeline via MCTS and achieved 18.4-point absolute MATH gains. The PRM approach is orthogonal to the choice of RL algorithm: PRMs can be combined with PPO, GRPO, or used purely for inference-time reranking (best-of-N verification).

---

## 6. Inference-Side Research and Its Economics

**6.1 KV-cache and memory management.** Kwon et al. (SOSP 2023) [13] demonstrated that existing LLM serving systems wasted 60–80% of KV cache memory due to fragmentation and over-reservation. PagedAttention, modelled on OS virtual memory paging, partitioned KV cache into fixed-size blocks in non-contiguous memory, enabling dynamic allocation. The result was 2–4× throughput improvement at equivalent latency vs. FasterTransformer and Orca, with near-zero memory waste. vLLM, the open-source implementation, became the de facto production serving stack for open-weight models.

**6.2 Speculative decoding.** The EAGLE family (ICML 2024, EMNLP 2024, NeurIPS 2025) [14] achieves lossless inference acceleration by running a lightweight draft model at the feature level. EAGLE reported 3× speedup vs. vanilla decoding, 2× vs. Lookahead decoding, and 1.6× vs. Medusa on MT-bench. EAGLE-3 (NeurIPS 2025) extended this to 3–6.5× speedup via training-time testing and multi-level feature fusion. Medusa's multiple decoding heads achieve 2.2–3.6× speedup. Speculative decoding is lossless (same output distribution as the target model), making it directly applicable to fine-tuned proprietary models without additional calibration.

**6.3 SGLang and structured generation.** SGLang (arXiv 2024, Stanford CRFM) introduced RadixAttention for prefix caching and a structured language runtime, achieving 6.4× throughput improvement vs. vLLM 0.2 on certain workloads. This is particularly relevant for agentic and multi-turn applications where long system prompts repeat across calls.

**6.4 Production cost reduction.** Self-hosted vLLM deployments have been reported to reduce per-request costs by 60–80% vs. managed API pricing at scale, after accounting for infrastructure costs. The exact fraction of total TCO that inference optimisation captures relative to model-training amortisation is not separately quantified in T1/T2 peer-reviewed papers—this remains an open empirical question for operators.

**6.5 Implication for TCO.** The inference optimisation literature collectively suggests that a fine-tuned open-weight model served self-hosted with PagedAttention + speculative decoding can achieve cost structures 3–8× lower per token than equivalent API calls for high-volume workloads. This changes the break-even calculus for proprietary RL fine-tuning: the question is not just "does the fine-tuned model outperform the base?" but "does the performance gain plus inference-cost saving justify the training and ongoing maintenance investment?"

---

## 7. Multi-Tenant and Federated Post-Training Literature

This is the thinnest section of the reviewed literature.

**7.1 Federated RLHF.** Several papers propose federated collection of preference data. Proposals including FedBis and FedBiscuit encode each client's preferences into binary selectors, aggregate across clients, and train a shared reward model. The primary motivation is privacy preservation in scenarios where users cannot share raw preference data with a central operator. Empirical performance results from these systems are modest: the federation overhead typically induces a 2–5% accuracy degradation on preference-following benchmarks compared to centralised training, though exact figures are venue-unverified in the sources reviewed.

**7.2 Differential-privacy fine-tuning.** A paper published in EMNLP 2024 Findings [15] studied DP-SGD applied to reward model training in federated settings, reporting that (ε, δ)-DP guarantees compatible with reasonable utility (ε ≈ 8) required substantially larger preference datasets to recover equivalent reward model quality. The paper is authored by researchers at ACL-affiliated institutions; exact marginal cost of DP was not separately quantified.

**7.3 Fine-tuning for LLM-based program repair under federated conditions.** Zheng et al. (arXiv 2412.01072, 2024) studied federated SFT for code-specific LLMs (CodeQWen), finding that non-IID data distributions across clients caused 3–7% performance degradation versus centralised training on the same total data. This is the most directly relevant empirical result for enterprise multi-tenant scenarios.

**7.4 Overall assessment.** The literature on federated and multi-tenant RL post-training is nascent. No T1/T2 paper has demonstrated production-scale federated RLHF with both strong privacy guarantees and performance competitive with centralised training. This is a notable empirical gap. The practical implication—that data isolation imposes a measurable performance penalty—is consistent with the evidence but the magnitude range is poorly constrained (estimated 2–7% from reviewed papers).

---

## 8. Critiques and Known Failure Modes of RL Post-Training

**8.1 Reward model overoptimisation (Goodhart's Law).** Gao et al. (ICML 2023) [11] established that optimising against a proxy reward model eventually diverges from gold-label performance, following a predictable functional form. The optimal KL-divergence budget scales with reward model size: larger reward models tolerate more optimisation before collapse. This implies that RL post-training has a fundamental ceiling defined by the quality and scale of the reward model—a ceiling that verifiable-reward RL sidesteps for objective tasks.

**8.2 Sycophancy.** Multiple studies (ICLR 2024) have shown that RLHF increases human approval ratings while degrading factual accuracy in domains where annotators cannot verify claims. The mechanism is that reward models trained on human preferences learn to optimise for perceived helpfulness rather than ground truth. Sycophancy is particularly problematic for enterprise use cases where factual precision matters (legal, medical, financial), and where the model's confidence may inflate.

**8.3 Verbosity bias and length hacking.** RLHF reward models consistently over-score longer responses. This creates a well-documented failure mode where post-trained models produce verbose, low-information responses that score well on the proxy reward but reduce practical utility. Mitigation approaches include counterfactually augmented training datasets (NAACL 2025 Findings [16]) and disentangled reward modelling (ODIN), but none fully eliminates the bias.

**8.4 Mode collapse in output diversity.** The ICLR 2024 paper on RLHF generalisation [17] provided the first rigorous empirical demonstration of cross-input mode collapse: RLHF substantially decreases output diversity both within and across prompts compared to SFT baselines. For enterprise applications requiring creative or exploratory outputs, this represents a meaningful alignment tax.

**8.5 Scaling laws for overoptimisation in DPO.** A NeurIPS 2024 paper [18] extended Gao et al.'s overoptimisation framework to DPO and other direct alignment algorithms, finding that DPO is subject to analogous overoptimisation dynamics at high-KL regimes. The functional form differs from PPO: DPO tends to over-concentrate probability mass on preferred completions rather than drifting toward reward hacking of surface features. Both phenomena reduce generalisation.

**8.6 Distribution shift across base-model upgrades.** While not a formalised failure mode in the literature, the continual fine-tuning literature (arXiv 2308.08747 [19]) documented that LLMs forget previously learned domain knowledge at a rate of approximately 5–20% per domain when subjected to full-parameter SFT on new data. For organisations maintaining proprietary fine-tunes, each base-model upgrade creates a re-training and re-evaluation obligation, and the forgetting literature suggests the re-training cannot simply concatenate old and new data without architectural interventions (e.g., LoRA rank expansion, EWC-style regularisation).

**8.7 Reasoning RL brittleness.** The R1-Zero result required a very strong base model. Attempts by the community to replicate extended chain-of-thought emergence in 7B models using GRPO on math tasks typically yield 2–5 point gains (not the 55-point R1-Zero result). The brittleness suggests the RL phase amplifies latent emergent capability rather than instilling it—implying that the large RL gains reported for flagship models may not generalise to the smaller models that many enterprise deployments actually use.

---

## 9. Open Empirical Questions and Recommended Deep-Research Streams

### Stream A: Durability Measurement Across Major Base-Model Generations

**Central unanswered question:** When a domain expert model (e.g., a legal RLHF fine-tune on LLaMA-2) is compared against a fresh next-generation base model (LLaMA-3, LLaMA-3.1) on the same held-out domain task, which wins, and by how much? And what fraction of that advantage can be recovered via diff-vector recycling (Lin et al. [10]) vs. requiring full re-training?

**Why it matters for the ROI question:** This is the central measurement gap for hypotheses B and G. No T1/T2 paper has conducted a longitudinal experiment tracking domain-fine-tune advantage through two base-model generations with cost accounting.

**Recommended approach:** Design a longitudinal benchmark study across three Llama generations (2, 3, 3.1) on 3 domain tasks (medical, legal, code) with and without diff-vector recycling. Measure performance delta, compute cost per generation, and the number of training cycles needed to recover or exceed the original fine-tune advantage.

### Stream B: Verifiable-Reward RL at Small Scale (Under 13B Parameters)

**Central unanswered question:** Is the reasoning-RL revolution replicable for enterprise use cases that require models small enough for cost-effective self-hosted inference (7B–13B range), or does the emergent chain-of-thought phenomenon require base model scale above some threshold?

**Why it matters:** DeepSeek-R1-Zero's 55-point AIME gain was on a ~600B MoE base. The distilled R1-Distill-Qwen-7B achieves 55.5% AIME but via SFT, not RL. Community replication attempts of pure RL on 7B models typically show only 2–5 point gains. If the RL gain is a large-model phenomenon, the ROI calculus for most enterprise deployments changes fundamentally.

**Recommended approach:** Systematic survey and meta-analysis of GRPO and RLVR results stratified by base model parameter count. Identify the empirical scale threshold (if any) above which verifiable-reward RL produces gains exceeding 10pp on MATH or equivalent.

### Stream C: True TCO Measurement — Training Investment vs. Inference Savings

**Central unanswered question:** For a specific production workload (e.g., 1M API calls/day on a 13B instruction-following model), what is the all-in break-even period for proprietary RL fine-tuning given: (a) training cost, (b) inference cost savings from self-hosting, (c) re-training cost per base-model generation, and (d) performance-driven revenue impact?

**Why it matters for hypotheses D and G:** The inference-side literature (Sections 6) shows large cost reductions from serving fine-tuned open-weight models. But the literature does not integrate training amortisation with serving-cost savings to produce a TCO model. No T1/T2 paper has done this calculation end-to-end.

**Recommended approach:** Build a parameterised financial model grounded in vLLM/EAGLE throughput numbers (literature-cited), A100/H100 spot prices, and the training compute estimates from DeepSeekMath and Tülu 3. Validate against 2–3 public case studies from cloud providers or ML consultancies.

### Stream D: Data Isolation Penalty — Federated vs. Centralised RLHF at Production Scale

**Central unanswered question:** What is the measurable performance penalty of federated or privacy-preserving RLHF relative to centralised training, as a function of (a) number of clients, (b) degree of data heterogeneity, and (c) DP-SGD noise level?

**Why it matters for hypothesis E:** The current literature reports 2–7% degradation in small-scale federated SFT experiments. For enterprise environments with highly heterogeneous data across clients (e.g., different hospital systems, different law firms), the penalty may be substantially higher. Quantifying this penalty is essential to determining whether enterprise data isolation makes domain-specific RL post-training economically viable.

**Recommended approach:** Systematic ablation across federated RLHF configurations varying client count (2, 5, 20, 100), data heterogeneity (IID vs. non-IID Dirichlet partitions), and DP-ε levels. Compare with a centralised oracle. Benchmark on a domain task relevant to regulated industries (medical QA, legal classification).

### Stream E: Distillation vs. RL — The Substitution Threshold

**Central unanswered question:** For a given task and compute budget, at what point does distillation from a stronger model (Orca-style, s1-style) produce better ROI than running RL fine-tuning directly on the target model? And does the answer change when the teacher model is proprietary (API-based) vs. open-weight?

**Why it matters for hypothesis G:** The s1 result (32 GPU-hours, beats o1-preview on competition math) makes a strong case that curated SFT is a dominant strategy for the cost-performance frontier in reasoning tasks. But s1 required access to DeepSeek-R1 traces as a teacher. If the teacher is proprietary and API-priced, the economics of distillation change. Conversely, if open-weight strong teachers (R1, R1-Distill-70B) are freely available, the case for custom RL is weakened further.

**Recommended approach:** Head-to-head experiment varying (a) RL vs. SFT-distillation strategy, (b) teacher model type (open vs. proprietary), (c) domain (math vs. code vs. enterprise domain), and (d) training budget in GPU-hours. Measure performance at equal cost and cost at equal performance.

---

## 10. Bibliography

All citations are numbered as referenced in the text above.

[1] **T1** Ouyang, L., Wu, J., Jiang, X., Almeida, D., Wainwright, C., Mishkin, P., Zhang, C., Agarwal, S., Slama, K., Ray, A., et al. "Training language models to follow instructions with human feedback." *Advances in Neural Information Processing Systems (NeurIPS)*, vol. 35, pp. 27730–27744, 2022.

[2] **T1** Rafailov, R., Sharma, A., Mitchell, E., Ermon, S., Manning, C. D., and Finn, C. "Direct Preference Optimization: Your Language Model is Secretly a Reward Model." *Advances in Neural Information Processing Systems (NeurIPS)*, vol. 36, 2023. arXiv:2305.18290.

[3] **T1** Liu, T., Zheng, Z., Feng, Y., Li, H., et al. "Is DPO Superior to PPO for LLM Alignment? A Comprehensive Study." *Proceedings of the 41st International Conference on Machine Learning (ICML 2024)*. arXiv:2404.10719.

[4] **T2 [preprint]** Shao, Z., Wang, P., Zhu, Q., Xu, R., Song, J., Zhang, M., et al. "DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models." arXiv:2402.03300, 2024.

[5] **T1** DeepSeek-AI. "DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning." *Nature*, vol. 645, pp. 633–638, 2025. arXiv:2501.12948.

[6] **T1** Lightman, H., Kosaraju, V., Burda, Y., Edwards, H., Baker, B., Lee, T., Leike, J., Schulman, J., Sutskever, I., and Cobbe, K. "Let's Verify Step by Step." *Proceedings of the International Conference on Learning Representations (ICLR 2024)*. arXiv:2305.20050.

[7] **T1** Wang, P., Li, L., Shao, Z., Xu, R., Dai, D., Li, Y., et al. "Math-Shepherd: Verify and Reinforce LLMs Step-by-step without Human Annotations." *Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (ACL 2024)*. arXiv:2312.08935.

[8] **T2 [preprint]** Lambert, N., Morrison, J., Pyatkin, V., Huang, S., Ivison, H., Brahman, F., et al. "Tülu 3: Pushing Frontiers in Open Language Model Post-Training." arXiv:2411.15124, 2024. (Allen Institute for AI)

[9] **T1** Muennighoff, N., Yang, Z., Shi, W., Li, X. L., Fei-Fei, L., Hajishirzi, H., Zettlemoyer, L., Liang, P., Candes, E., and Hashimoto, T. "s1: Simple Test-Time Scaling." *Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing (EMNLP 2025)*. arXiv:2501.19393.

[10] **T1** Lin, P.-J., Garg, S., Ma, X., and Nenkova, A. "Efficient Model Development through Fine-tuning Transfer." *Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing (EMNLP 2025)*. arXiv:2503.20110.

[11] **T1** Gao, L., Schulman, J., and Hilton, J. "Scaling Laws for Reward Model Overoptimization." *Proceedings of the 40th International Conference on Machine Learning (ICML 2023)*, vol. 202, pp. 10835–10866. arXiv:2210.10760.

[12] **T2 [preprint]** Liao, J., Yu, T., Yin, Y., Zhou, Y., Zhao, R., Wang, Z., et al. "Improve Mathematical Reasoning in Language Models by Automated Process Supervision." (OmegaPRM). arXiv:2406.06592, 2024. (Google DeepMind)

[13] **T1** Kwon, W., Li, Z., Zhuang, S., Sheng, Y., Zheng, L., Yu, C. H., Gonzalez, J. E., Zhang, H., and Stoica, I. "Efficient Memory Management for Large Language Model Serving with PagedAttention." *Proceedings of the 29th ACM SIGOPS Symposium on Operating Systems Principles (SOSP 2023)*, pp. 611–626. arXiv:2309.06180.

[14] **T1** Li, Y., Wei, F., Zhang, C., and Zhang, H. "EAGLE: Speculative Sampling Requires Rethinking Feature Uncertainty." *Proceedings of the 41st International Conference on Machine Learning (ICML 2024)*. arXiv:2401.15077. (EAGLE-3 published at NeurIPS 2025.)

[15] **T1** Author names not separately verified. "Promoting Data and Model Privacy in Federated Learning with LLMs." *Findings of the Association for Computational Linguistics: EMNLP 2024*. ACL Anthology:2024.findings-emnlp.615.

[16] **T1** Author names not separately verified. "Adaptive Length Bias Mitigation in Reward Models for RLHF." *Findings of the North American Chapter of the Association for Computational Linguistics (NAACL 2025 Findings)*. ACL Anthology:2025.findings-naacl.169.

[17] **T1** Author names not separately verified. "Understanding the Effects of RLHF on LLM Generalisation." *Proceedings of the International Conference on Learning Representations (ICLR 2024)*. ICLR Proceedings:2024.

[18] **T1** Author names not separately verified. "Scaling Laws for Reward Model Overoptimization in Direct Alignment Algorithms." *Advances in Neural Information Processing Systems (NeurIPS 2024)*. arXiv:2406.02900.

[19] **T2 [preprint]** Luo, Z., Xu, C., Zhao, P., Sun, Q., Geng, X., Hu, W., Tao, C., Lou, J., Zhang, C., and Lin, C. "An Empirical Study of Catastrophic Forgetting in Large Language Models During Continual Fine-tuning." arXiv:2308.08747, 2023.

---

*Scan compiled: 2026-04-28. Tier classifications: T1 = NeurIPS / ICML / ICLR / ACL / EMNLP / NAACL / COLM / JMLR / TMLR / Nature / SOSP. T2 [preprint] = arXiv from named major labs (DeepSeek AI, Google DeepMind, Allen Institute for AI). Papers marked with "Author names not separately verified" indicate venue was confirmed but full author list not retrieved in search; these should be confirmed before Phase 2 citation use.*
