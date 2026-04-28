# Chapter — The Sweet Spot: Distillation, RL-at-Scale, and the s1 Line of Attack

## Abstract

The 2024–2025 period produced two structurally different routes to capable reasoning models: supervised fine-tuning on high-quality reasoning traces (distillation) and large-scale reinforcement learning against verifiable rewards (RL). This chapter maps the cost-performance frontier across both routes by examining the design space defined by (RL vs SFT-distillation) × (open vs proprietary teacher) × (small vs large base model). The central empirical finding is that effective reasoning-RL is strongly scale-gated: on 7B-class bases trained from scratch with GRPO, reported absolute AIME gains cluster in the 10–25 pp range, well below the 55 pp gain obtained by DeepSeek-R1-Zero from a ~671B mixture-of-experts base trained for millions of steps. Meanwhile, distillation from a sufficiently strong open-weight teacher (the R1 family, QwQ) converges faster, costs less, and currently dominates pure RL at the 7B–32B tier. The s1 result by Muennighoff et al., accepted at EMNLP 2025, sits at a striking extreme: seven H100-hours of SFT on 1,000 curated traces yielded a 32B model that beat o1-preview on AIME24. That result does not prove RL is unnecessary; it proves that a well-chosen, already-reasoning base model requires very little additional supervised compute to unlock competitive performance. The practical "minimum spend / maximum gain" point today is SFT-distillation at the 7B–32B scale using open-weight R1-family teachers, with narrow-task commercial RL (OpenAI o4-mini RFT) occupying a high-cost niche for organisations that lack inference control.

---

## 1. Reframing the Optimisation Problem

The standard framing in the post-training literature asks: given a pre-trained base model, how much additional supervised or reinforcement signal is needed to reach a target capability level? That framing, however, conflates three distinct sub-problems: (a) acquiring step-by-step reasoning behaviour at all, (b) specialising an already-reasoning model to a domain, and (c) aligning a model to produce correct final answers reliably under distribution shift.

These sub-problems respond differently to different training signals. Sub-problem (a) — acquiring reasoning from scratch — appears to require either a sufficiently large base model under pure RL or a strong teacher for distillation. Sub-problem (b) — domain specialisation — responds well to small SFT datasets and, in the commercial RFT setting, to verifiable reward graders. Sub-problem (c) — reliable correctness at inference time — can be partially addressed by test-time compute scaling (budget forcing, best-of-N, process reward models) without any additional training.

The cost-performance frontier therefore has multiple Pareto-efficient points depending on which sub-problem is primary. This chapter works through each region of the space in turn.

---

## 2. The Reasoning-RL Revolution and Its Scale Threshold

### 2.1 DeepSeek-R1-Zero: The Existence Proof

The most consequential empirical demonstration of 2025 was DeepSeek-R1-Zero (DeepSeek-AI 2025a), which showed that extended chain-of-thought reasoning could emerge from pure RL without any supervised fine-tuning. The base model was DeepSeek-V3, a 671B-parameter mixture-of-experts architecture with 37B active parameters per token. The RL algorithm was Group Relative Policy Optimization (GRPO), introduced in DeepSeekMath (Shao et al. 2024). The reward signal was binary: correctness of the final answer verified against ground truth, plus a structural reward penalising malformed output.

The AIME 2024 pass@1 trajectory is the canonical evidence: the model progressed from 15.6% at initialisation to 71.0% after RL training (with majority voting lifting this to 86.7%). The final DeepSeek-R1, which added a cold-start SFT phase to the RL pipeline, achieved 79.8% on AIME 2024, matching OpenAI o1 at the time of its release (DeepSeek-AI 2025a). The total RL training cost has been estimated at approximately 2.788 million H800 GPU-hours, not including the cost of the V3 base model itself, making this a frontier lab-scale experiment rather than a practitioner recipe.

The paper also directly compared distillation and RL at the 32B scale: DeepSeek-R1-Distill-Qwen-32B (SFT on R1 outputs, 800K traces) outperformed DeepSeek-R1-Zero-Qwen-32B (pure RL from the same 32B Qwen2.5 base), strongly suggesting that for sub-frontier base models, distillation from a stronger teacher dominates self-discovered RL reasoning. The 7B distilled model reached 55.5% on AIME 2024, surpassing QwQ-32B-Preview despite having 4.5x fewer parameters — a result that anchors the distillation efficiency argument.

### 2.2 The Scale Threshold in Open Replication

The open-source community rapidly attempted to replicate R1-Zero's RL emergence on more accessible models. The results are instructive precisely because they reveal where the recipe fails.

The simpleRL-reason project from HKUST (Zeng et al. 2025) [preprint] applied GRPO with rule-based rewards to ten diverse base models including Llama3 8B, Mistral 7B, and Qwen2.5 0.5B–32B, using only 8K training examples. The headline result is that all tested models showed absolute accuracy gains of 10–20+ points on math benchmarks; the Qwen2.5-Math-7B model reached 33.3% on AIME, 62.5% on AMC, and 77.2% on MATH500. Training used 2×8 H100-80G GPUs for approximately 15 hours (~240 H100-hours). However, the authors explicitly noted that increased response length — a proxy for extended reasoning — did not consistently correlate with emergence of higher-order cognitive behaviours such as self-verification. This is a key observation: small-model RL improves accuracy via better search over existing capabilities, not via the qualitative reasoning mode change seen in R1-Zero.

Open-Reasoner-Zero (Hu et al. 2025) [preprint] demonstrated that vanilla PPO with a learned critic, rather than GRPO, could replicate R1-Zero's scaling phenomenon on Qwen2.5-32B base using only 1/10 of DeepSeek's training steps. Results on AIME 2024 and MATH500 were competitive. Crucially, the paper showed consistent improvement with scale across the 0.5B–32B model family, but the 7B and smaller models showed substantially smaller absolute gains than the 32B. The authors recommended PPO over GRPO for the 32B scale, citing more robust advantage estimation from the learned critic.

Skywork-OR1 (Skywork 2025) [preprint] applied a modified GRPO variant called MAGIC (Multi-stage Adaptive entropy scheduling for GRPO In Convergence) to the DeepSeek-R1-Distill model series, treating RL as a second post-distillation stage. Starting from already-distilled 32B checkpoints scoring 57.8% on average across AIME24, AIME25, and LiveCodeBench, Skywork-OR1-32B reached 72.8% — a 15 pp gain. The 7B equivalent improved from 43.6% to 57.5% (+13.9 pp). This hybrid distillation-then-RL pipeline currently holds some of the strongest published open-model results.

TÜLU 3 (Ivison et al. 2024) introduced Reinforcement Learning from Verifiable Rewards (RLVR) as the final stage of a comprehensive post-training recipe for Llama 3.1 models (8B, 70B, 405B). The RLVR stage contributed measurably to MATH performance, with gains scaling more sharply at the 405B tier than at 8B — consistent with a general scale threshold for RL-induced reasoning. At the 8B scale, RLVR added only modest gains on top of SFT and DPO stages.

**Empirical synthesis.** The replication record converges on a soft scale threshold near 32B dense-equivalent parameters for robust RL-induced reasoning emergence. Below this threshold, RL does produce measurable accuracy gains (10–25 pp on math benchmarks), but those gains are achieved through existing-capability exploitation rather than new reasoning mode emergence. Above 32B — and emphatically at frontier 100B+ MoE scales — RL training reliably elicits extended chain-of-thought that generalises to out-of-distribution problems. The community has not yet established the exact critical scale, and it is model-family-dependent: a heavily math-pretrained 7B model (DeepSeek-Math-7B) appears more responsive to RL than a general-purpose 7B.

---

## 3. The s1 Result and What It Actually Proved

### 3.1 What the Experiment Was

Muennighoff et al. (2025), in a paper subsequently accepted at EMNLP 2025, pursued the minimal sufficient conditions for test-time scaling. The recipe had two components: a curated 1,000-example dataset (s1K) and a test-time intervention called budget forcing.

**The s1K dataset.** Fifty-nine thousand initial questions were drawn from sixteen sources — NuminaMath (30,660 problems), OlympicArena (4,250 questions spanning mathematics, physics, chemistry, biology, and computer science), OmniMath (4,238 competition mathematics problems), AGIEval (2,385 standardised test questions), historical AIME problems (1983–2021), 182 Stanford PhD probability exam questions, and 23 quantitative-trading brain teasers, among others. Filtering applied three criteria: quality (format and coherence validation), difficulty (based on model error rate and reasoning-trace length as a proxy for complexity), and diversity (50 domains via Mathematics Subject Classification). The resulting 1,000 questions were paired with reasoning traces generated by Gemini Thinking and QwQ-32B-Preview, giving each trace a long chain-of-thought structure already present in the teacher's output.

**Training.** Qwen2.5-32B-Instruct was fine-tuned on s1K using supervised learning on these reasoning traces. Training ran for 26 minutes on 16 NVIDIA H100 GPUs via PyTorch FSDP — approximately 6.9 H100-hours total. No reinforcement learning was used.

**Budget forcing.** At inference, the model's thinking process was controlled by two operations: (I) forcefully terminating the thinking chain by appending an end-of-thinking delimiter when time budget was exhausted, or (II) suppressing the end-of-thinking delimiter and appending "Wait" to extend reasoning when additional compute was available. This intervention enables test-time compute scaling without any additional training — the model re-examines its answer when prompted to "wait."

### 3.2 The Results

The s1-32B model achieved:

| Benchmark | s1-32B | o1-preview | o1 |
|---|---|---|---|
| AIME24 (pass@1) | 56.7% | 44.6% | 74.4% |
| MATH500 | 93.0% | 85.5% | 94.8% |
| GPQA Diamond | 59.6% | 73.3% | 77.3% |

With budget forcing enabled, AIME24 performance scaled from 50% to 57% — a demonstration of test-time compute scaling from a model trained on fewer than seven H100-hours of compute.

### 3.3 What the s1 Result Actually Proves — and What It Does Not

The s1 result is frequently misread as evidence that RL is unnecessary. The accurate reading is more specific. Qwen2.5-32B-Instruct was already a heavily post-trained model with strong instruction-following capabilities; the reasoning traces came from teachers (Gemini Thinking, QwQ-32B-Preview) that are themselves the product of large-scale reasoning RL. The s1 experiment shows that *amortised* reasoning capability — distilled from already-capable models — can be activated in a new model with minimal additional compute. It does not show that the underlying capability can be *created* cheaply from a raw base model.

Put differently: s1 is a distillation experiment at extreme efficiency, not an existence proof that RL is dispensable. The cheap SFT step only works because both the Qwen2.5-32B base and the trace-generating teachers embodied enormous prior compute investment.

Furthermore, the GPQA Diamond result (59.6% vs o1-preview's 73.3%) indicates that the 1,000-trace, seven-H100-hour approach does not close all capability gaps against frontier reasoning models. The result is impressive for its cost-efficiency; it is not a full substitute for the reasoning depth of o1-class systems.

**Budget forcing as an inference trick.** The "Wait" token extension has been validated across multiple independent replications — Sky-T1 from NovaSky-AI (NovaSky 2025) [preprint] and several community reproductions. It is not a benchmark artifact: the mechanism genuinely elicits continued reasoning and self-correction. However, its effectiveness is bounded by the model's pre-existing reasoning capability; on models with shallow chains of thought, appending "Wait" produces repetition rather than substantive reconsideration. It is better characterised as a compute-allocation interface for models already predisposed to extended reasoning.

---

## 4. Method-by-Method Efficiency: Data and Compute

The table below synthesises the primary-source evidence across five fine-tuning paradigms at representative base sizes, using training GPU-hours as the cost metric and AIME24 pass@1 as the primary capability metric where available.

| Method | Base Model (Size) | Training Data | Approx. GPU-Hours | Benchmark Gain | Primary Benchmark | Source |
|---|---|---|---|---|---|---|
| SFT-distillation (s1) | Qwen2.5-32B-Instruct | 1,000 traces | ~7 H100-hrs | +27% vs o1-preview (AIME24); 56.7% abs | AIME24, MATH500 | Muennighoff et al. 2025 |
| SFT-distillation (R1-Distill) | Qwen2.5-7B-Base | 800K R1 traces | ~200–400 H100-hrs (est.) | 55.5% AIME24 abs | AIME24 | DeepSeek-AI 2025a |
| SFT-distillation (R1-Distill) | Qwen2.5-32B-Base | 800K R1 traces | ~500–1,000 H100-hrs (est.) | 72.6% AIME24 abs | AIME24 | DeepSeek-AI 2025a |
| GRPO-RL (DeepSeekMath) | DeepSeek-7B-Math | 8K MATH examples | ~240 H100-hrs | MATH: 46.8%→51.7% (+4.9 pp) | MATH, GSM8K | Shao et al. 2024 |
| GRPO-RL (simpleRL-reason) | Qwen2.5-Math-7B | 8K MATH examples | ~240 H100-hrs | AIME: ~33% abs; MATH: 77.2% abs | AIME, MATH | Zeng et al. 2025 |
| GRPO-RL (Skywork-OR1) | DS-R1-Distill-32B | NuminaMath-1.5 | ~1,000–2,000 H100-hrs (est.) | +15.0 pp AIME/LiveCodeBench avg | AIME24, LiveCodeBench | Skywork 2025 |
| GRPO-RL (Skywork-OR1-7B) | DS-R1-Distill-7B | NuminaMath-1.5 | ~200–500 H100-hrs (est.) | +13.9 pp avg | AIME24, LiveCodeBench | Skywork 2025 |
| PPO-RL (Open-Reasoner-Zero-32B) | Qwen2.5-32B-Base | ~1/10 of R1-Zero steps | ~100K H800-hrs (est.) | Competitive with R1-Zero | AIME24, MATH500, GPQA | Hu et al. 2025 |
| GRPO-RL (DeepSeek-R1-Zero) | DeepSeek-V3 (671B MoE) | RL only, millions of steps | ~2.8M H800-hrs | AIME24: 15.6%→71.0% (+55.4 pp) | AIME24 | DeepSeek-AI 2025a |
| RLVR (TÜLU 3 405B) | Llama-3.1-405B | Verifiable math+code tasks | ~50K H100-hrs (est.) | Surpasses GPT-4o on MATH | MATH, GSM8K, safety | Ivison et al. 2024 |
| Commercial RFT (o4-mini) | OpenAI o4-mini | Customer-specific, ~10–1K examples | ~$65–$5,000 per job | +12 to +39 pp task-specific | Domain-specific | OpenAI 2025 |

*Note: GPU-hour estimates for distillation runs are calculated from typical per-GPU throughput for the model size and batch configuration reported; they should be treated as order-of-magnitude estimates, not exact figures.*

**GRPO vs PPO trade-offs.** GRPO eliminates the critic (value model) required by PPO, substantially reducing memory overhead: for a 7B policy, training with GRPO requires roughly half the VRAM of PPO because no second network of equivalent size is maintained (Shao et al. 2024). However, the GRPO advantage estimate — based on relative performance within a group of sampled completions — is noisier than PPO's learned value estimate, which can cause training instability at large batch sizes. Open-Reasoner-Zero found PPO superior at 32B scale; simpleRL-reason used GRPO successfully at 7B–32B. Skywork-OR1 introduced MAGIC to address entropy collapse during GRPO, demonstrating that GRPO's stability weaknesses are surmountable with careful scheduling.

**DPO** offers the lowest training cost of any alignment method (classification over preference pairs, no rollouts required) but does not naturally produce extended chain-of-thought because it trains on fixed-length outputs. Recent theoretical work has shown that GRPO with two rollouts per prompt is mathematically equivalent to a form of DPO, suggesting the methods share inductive biases (Xu et al. 2025) [preprint]. For instruction-following and preference-based tasks, DPO remains cheaper and often competitive with RL; for tasks requiring reasoning exploration, GRPO and PPO dominate.

---

## 5. Distillation vs RL: When Each Dominates

### 5.1 The Case for Distillation

The Orca programme from Microsoft established the principal that small models can acquire reasoning behaviour almost entirely from trace-level supervision from stronger models. Orca 2 (Mitra et al. 2023), fine-tuned on GPT-4 explanation traces at 7B and 13B scales, matched or exceeded LLaMA-2-Chat-70B across a wide range of reasoning benchmarks — a 5–10× parameter efficiency gain attributable entirely to trace quality, not to RL. The 7B Orca 2 used approximately 5 million SFT examples (a mix of ChatGPT and GPT-4 traces); the 13B achieved a 33% relative improvement over LLaMA-2-Chat-13B on HellaSwag and 62% over WizardLM-13B at comparable scale.

Phi-3-mini (Abdin et al. 2024) pushed further: a 3.8B model trained on 3.3 trillion tokens of heavily filtered web data and synthetic LLM-generated data achieved 69% on MMLU and 59.1% on HumanEval, rivalling Mixtral-8x7B. The Phi recipe, like Orca, demonstrates that data curation and synthetic trace quality can substitute for model scale to a considerable degree in sub-frontier capability regimes.

The DeepSeek-R1 distill family represents the current state of the art in distillation efficiency: DeepSeek-R1-Distill-Qwen-7B (800K R1 output traces, SFT only) achieves 55.5% on AIME 2024, surpassing QwQ-32B-Preview, which is itself a large reasoning model trained with extensive RL. The DeepSeek team's own ablation showed that distillation from R1 outperformed pure RL at 32B scale using the same amount of training compute — the definitive internal comparison between the two paradigms at sub-frontier scale.

Phi-4-Reasoning (Microsoft 2025) extended the Phi line to a 14B model specifically fine-tuned for reasoning via SFT on filtered chain-of-thought data, demonstrating that the distillation approach scales gracefully to reasoning tasks without requiring RL at all for the 10–20B tier.

### 5.2 The Case for RL

Distillation inherits the teacher's distribution of reasoning strategies and errors. It is a compression operation: the student model cannot exceed the teacher's capability on the training task distribution (though it can generalise differently). RL, by contrast, allows the model to discover reasoning strategies not present in any demonstration — in principle enabling the trained model to surpass its teacher on certain tasks. This is the "going beyond distillation" argument for RL, and it is empirically supported by DeepSeek-R1-Zero's performance on AIME, where the model achieved results matching or exceeding contemporaneous proprietary systems despite having access to no expert reasoning traces.

RL also excels when verifiable reward signals are available but labelled traces are not. In the commercial RFT setting (Section 7), this is precisely the typical situation: a legal firm knows whether a citation is correct; it does not have labelled step-by-step reasoning traces for every contract review. RL from correctness signals fills this gap. The simpleRL-reason finding that RL improves accuracy even on small models (10–20 pp gains on Llama 7B with 8K examples and ~240 H100-hours) suggests that RL has a role at the practitioner scale even when full reasoning emergence does not occur.

### 5.3 The Teacher-Quality Requirement

Distillation quality is monotonically dependent on teacher quality, introducing a structural cost barrier. The original Orca used GPT-4 traces; at the time of publication (2023), GPT-4 API access cost approximately $0.03–0.06 per 1K output tokens, making large-scale trace generation (millions of tokens) expensive but feasible for well-funded labs. The s1 traces came from Gemini Thinking and QwQ-32B-Preview; QwQ is open-weight and self-hostable, effectively zeroing the teacher API cost.

The shift from proprietary-teacher (GPT-4, Claude 3 Opus) to open-weight-teacher (DeepSeek-R1, QwQ-32B, Qwen3-72B) has substantially altered the economics of distillation since early 2025. An organisation can now generate 800K reasoning traces from DeepSeek-R1 self-hosted on a cluster of H100s at a marginal inference cost of approximately $0.0005–$0.002 per trace (depending on length), compared to GPT-4 API costs of $0.06–$0.12 per trace at comparable length. For an 800K-trace dataset, this represents a cost difference of roughly $400–$1,600 (open-weight inference) vs $48,000–$96,000 (GPT-4 API) — a 30–60× cost reduction that fundamentally changes the distillation calculus.

This shift does not eliminate the teacher-quality ceiling; it moves the ceiling. Open-weight teachers as of mid-2025 are approximately on par with proprietary frontier models on mathematics and coding (the domains where they have been most intensively trained), but trail on general reasoning and instruction-following. The practical implication is that open-weight distillation is now economically viable for math, code, and structured reasoning, but may still require proprietary teacher access for softer tasks requiring nuanced human-preference alignment.

---

## 6. The Cost-Per-Percentage-Point Frontier

### 6.1 Methodology

Constructing a "cost-per-percentage-point" frontier requires normalising heterogeneous benchmarks, compute environments, and base models. The analysis below uses AIME24 as the primary reasoning metric (because it has the widest coverage across papers), H100-hours as the compute unit (converting H800 to H100 at approximate parity), and estimates published GPU costs at ~$2.50/H100-hour (mid-market cloud spot pricing in 2025) to express cost in dollars.

### 6.2 Observed Gains at Representative Compute Budgets

**~10 H100-hours (~$25):** The s1 regime. SFT-distillation on 1,000 curated traces at 32B scale. Achievable: 56.7% AIME24 absolute (from a strong instruct base). Gain over base Qwen2.5-32B-Instruct: approximately +35 pp (the base achieves ~22% on AIME24). Not-yet-achieved at this budget: reasoning emergence from a raw base model; substantial GPQA Diamond improvement.

**~100–250 H100-hours (~$250–$625):** The simpleRL-reason / DeepSeekMath RL regime. GRPO on 7B–14B models with 8K examples for ~100 steps. Achievable: 10–25 pp absolute gains on MATH and AMC; ~33% AIME24 from Qwen2.5-Math-7B. Ambitious at this budget: exceeding 40% AIME24 from a 7B model without distillation. Not-yet-achieved: reliable extended chain-of-thought emergence; >50% AIME24 from a 7B base.

**~500–2,000 H100-hours (~$1,250–$5,000):** The 32B RL or distillation regime. Achievable via distillation: 70–80% AIME24 (Skywork-OR1-32B: 82.2% AIME24). Achievable via RL alone: competitive results with distillation on 32B models (Open-Reasoner-Zero). Ambitious: surpassing the 32B distillation ceiling via pure RL.

**~10,000 H100-hours (~$25,000):** Multi-stage training (distillation + RL). Achievable: near-frontier reasoning performance at 32B scale. Not-yet-achieved: performance matching 671B-class frontier systems.

**>1,000,000 H100-hours (>$2.5M):** Frontier RL regime (DeepSeek-R1, OpenAI o-series). Achievable: SOTA reasoning, emergent extended chain-of-thought on novel problem classes. The practitioner-accessible "sweet spot" does not extend to this regime.

### 6.3 Interpretation

The cost-per-percentage-point analysis reveals a concave gains curve: the first 20–30 AIME points on a 32B base are cheapest (pure distillation SFT), the next 20–30 points require either RL augmentation or a substantially larger base, and the final frontier gains require either massive RL investment or distillation from frontier-class proprietary teachers. For organisations operating below $5K compute budget, distillation at 7B–32B scale is strictly dominant over RL.

One notable finding that the literature does not yet settle cleanly is whether the gains from Skywork-OR1 (RL applied after distillation) are additive over distillation alone or whether a comparably-scaled pure-distillation experiment with more data would match them. The Skywork paper does not include this ablation. This is a key open empirical question.

---

## 7. Commercial RFT vs Open-Weight RL: The Price Comparison

### 7.1 OpenAI Reinforcement Fine-Tuning (o4-mini)

OpenAI's production RFT API, generally available as of mid-2025 for the o4-mini-2025-04-16 model, prices training at $100 per wall-clock hour, prorated to the second, with a per-job cap of $5,000 (OpenAI 2025). Token costs for model-graded rewards are additional, billed at standard API rates. This makes an RFT job roughly equivalent in per-hour cost to a cloud H100 instance at standard on-demand pricing ($3–4/hour), but the managed service includes training infrastructure, reward model invocation, and hyperparameter management.

### 7.2 Documented Commercial Gains

The publicly documented RFT case studies as of mid-2025 (OpenAI 2025) are:

| Organisation | Domain | Task | Reported Gain | Grading Signal |
|---|---|---|---|---|
| Accordance | Tax | Complex tax analysis | +39 pp accuracy | Correctness vs tax code |
| Ambience Healthcare | Medical coding | ICD-10 assignment | +12 pp over physician baseline | Label match |
| Harvey | Legal | Citation extraction F1 | +20% F1 | Citation correctness |
| Runloop | Engineering | Stripe API code generation | +12 pp | Syntax/AST validation |
| Milo | Productivity | High-complexity scheduling | +25 pp correctness | Schedule validity |

These are vendor-reported figures disclosed in the context of product announcements and should be interpreted accordingly. Independent replication of these benchmarks is not available in the peer-reviewed literature as of this writing. The gain magnitudes (12–39 pp) are large but not implausible given the narrow task scope: RFT excels precisely when the task has a crisp, automatable verifier, which all five cases possess.

### 7.3 The Dollar-Per-Point Comparison

At $100/hour with a typical job running 2–8 hours for a narrow task, the training cost of a single RFT experiment is $200–$800. The $5,000 cap suggests that production-quality runs can cost up to $5,000 per trained checkpoint. For the Accordance +39 pp gain, if achieved in an average-length job (~$500–$2,000 estimated), the cost-per-point is approximately $13–$51 per percentage point. For Harvey's +20% F1 at similar costs, the figure is similar.

Comparable open-weight RL runs on 7B models using GRPO with H100 instances ($2.50–$3.50/hour spot) for ~240 GPU-hours cost roughly $600–$840 total. The simpleRL-reason gains of 10–20 pp on math benchmarks imply ~$30–$84 per percentage point — comparable to the commercial RFT figure. However, the open-weight approach requires significant MLOps infrastructure, reward grader development, and hyperparameter tuning that the commercial API abstracts away. When researcher/engineer time is included (2–5 engineer-days for setup), the true total cost of open-weight RL is likely 3–5× the raw compute cost, eroding the apparent cost advantage.

The meaningful advantage of open-weight RL is inference cost and data control: the trained model can be deployed at marginal inference cost, whereas the RFT-trained o4-mini checkpoint must be served through OpenAI's API at ongoing per-token rates. For organisations with high inference volumes, the open-weight path becomes economically superior once inference amortises the training overhead.

TensorZero (2025) conducted an independent evaluation of OpenAI RFT vs SFT across three tasks, finding that RFT costs 100–700× more than SFT on equivalent datasets, and that SFT on a larger dataset outperformed RFT in two of three tasks. The one task where RFT meaningfully outperformed SFT was agentic coding with sparse rewards — precisely the use case for which RL's exploration advantage is theoretically expected.

---

## 8. Locating the Sweet Spot — or Arguing None Exists Yet

### 8.1 The Case for a Sweet Spot

The empirical record supports a provisional claim that the current cost-performance sweet spot is **SFT-distillation at 7B–32B scale using open-weight R1-family teachers, with budget-forcing at inference, targeting verifiable reasoning tasks**. The key evidence is:

1. s1-32B achieves 56.7% AIME24 from ~7 H100-hours of SFT and no RL (Muennighoff et al. 2025).
2. DeepSeek-R1-Distill-Qwen-7B achieves 55.5% AIME24 from SFT on 800K traces, outperforming QwQ-32B-Preview (DeepSeek-AI 2025a).
3. Skywork-OR1-32B (RL post-distillation) achieves 82.2% AIME24 at estimated costs well under $10,000 (Skywork 2025).
4. Open-weight teachers (DeepSeek-R1, QwQ-32B) are now free to self-host, eliminating the historical API cost barrier to distillation.
5. Commercial RFT is approximately cost-comparable to open-weight RL per percentage point gained but locks the operator into proprietary inference costs.

This sweet spot is domain-specific: it applies most cleanly to structured, verifiable tasks (mathematics, code, logic) where the teacher's reasoning traces are reliable and the evaluation signal is automatable. For open-ended generation, instruction-following, or tasks requiring world-model quality that exceeds current open-weight teachers, the sweet spot is less clearly defined.

### 8.2 The Case Against a Universal Sweet Spot

Several considerations complicate the "sweet spot" framing:

**The base model problem.** The s1 result depends on Qwen2.5-32B-Instruct already being a strong instruct model (which itself required substantial post-training investment by Alibaba). The 7 H100-hours of SFT do not include the cost of the base model or the teacher traces. When full pipeline costs are amortised, the s1 approach may be less anomalously cheap than it appears.

**Task conditionality.** The methods with the clearest sweet spots (s1-style distillation, GRPO on math) show their best results on competition mathematics and STEM reasoning benchmarks. Transfer to non-math domains (medical, legal, code in natural-language-intensive forms) is systematically weaker and has been demonstrated primarily in the commercial RFT setting, not the open-weight RL literature.

**The moving frontier.** Between the submission of DeepSeek-R1 in January 2025 and mid-2025, the open-weight frontier moved dramatically (Qwen3-72B, Llama-4 Scout/Maverick, DeepSeek-V3-0324, Phi-4-Reasoning). The "sweet spot" at any given moment is relative to the current open-weight base model quality, which is itself a rapidly improving target.

**Exploration vs exploitation.** Distillation optimises exploitation of a teacher's existing capabilities. For organisations pushing into genuinely novel task territory — where no existing model performs well — RL's exploration advantage may be irreplaceable, but this comes at frontier-scale costs.

### 8.3 Provisional Answer

The minimum-spend / maximum-gain point as of mid-2025 is **SFT on ~1,000–50,000 curated reasoning traces from an open-weight R1-class teacher, applied to a 32B-class instruct base, with budget-forcing enabled at inference**. Total compute: 10–500 H100-hours (~$25–$1,250). Expected AIME24 range: 50–70%. This regime is accessible to well-resourced individual researchers or small teams, requires no reinforcement infrastructure, and produces models that meaningfully outperform frontier proprietary systems from 12 months prior.

RL adds value in two complementary niches: (a) as a post-distillation refinement step at 32B scale (the Skywork-OR1 pattern), which is accessible at ~1,000–2,000 H100-hours; and (b) as the only viable method for acquiring new reasoning patterns not present in any available teacher, which currently requires frontier-scale infrastructure.

---

## 9. Open Empirical Questions

The research record as of mid-2025 leaves the following empirically open:

1. **Exact scale threshold for reasoning emergence.** Is the threshold near 32B dense-equivalent parameters, or is it model-family-specific? A controlled experiment varying base model size while holding training data and algorithm constant across the 7B–70B range (analogous to the Chinchilla scaling law methodology) has not been published.

2. **Additive gains from distillation + RL.** Does Skywork-OR1's +15 pp post-distillation RL gain represent a capability genuinely beyond what additional distillation data would achieve? The ablation (more SFT data vs same compute on RL) is absent from the literature.

3. **Budget-forcing generalisation bounds.** Budget forcing has been demonstrated on mathematics; its effectiveness on medical, legal, and agentic tasks remains poorly characterised. There is no published study varying the base model's reasoning depth and measuring the incremental gain from budget forcing as a function of that depth.

4. **Teacher contamination.** The s1K dataset draws from the same competition mathematics benchmarks used for evaluation (AIME, AMC). A careful contamination audit isolating evaluation instances from training-source instances has not been published for the full s1 family.

5. **The entropy-collapse mechanism.** Skywork-OR1 and other GRPO studies identify premature entropy collapse as the primary training failure mode. The precise conditions under which entropy collapse occurs (batch size, KL coefficient, off-policy ratio) are not yet characterised in a theoretically grounded way.

6. **Commercial RFT generalisation.** The five published commercial RFT case studies all involve narrow, verifiable tasks. Whether RFT at comparable cost produces useful gains on broader instruction-following or knowledge-intensive tasks remains undocumented in any accessible source.

7. **Cost of teacher trace generation at scale.** The field lacks a rigorous cost accounting for open-weight teacher distillation that includes inference cost, quality filtering compute, and human review. Such an accounting would allow direct comparison of the full pipeline cost between open-weight and proprietary teacher routes.

---

## References

Abdin, Marah, et al. 2024. "Phi-3 Technical Report: A Highly Capable Language Model Locally on Your Phone." arXiv:2404.14219 [preprint].

DeepSeek-AI. 2025a. "DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning." *Nature* 645: 633–638. [Also arXiv:2501.12948 [preprint].]

Hu, Jian, et al. 2025. "Open-Reasoner-Zero: An Open Source Approach to Scaling Up Reinforcement Learning on the Base Model." arXiv:2503.24290 [preprint].

Ivison, Hamish, et al. 2024. "Tülu 3: Pushing Frontiers in Open Language Model Post-Training." arXiv:2411.15124 [preprint].

Microsoft. 2025. "Phi-4-Reasoning Technical Report." Microsoft Research. https://www.microsoft.com/en-us/research/wp-content/uploads/2025/04/phi_4_reasoning.pdf.

Mitra, Arindam, et al. 2023. "Orca 2: Teaching Small Language Models How to Reason." arXiv:2311.11045 [preprint].

Muennighoff, Niklas, Zitong Yang, Weijia Shi, Xiang Lisa Li, Li Fei-Fei, Hannaneh Hajishirzi, Luke Zettlemoyer, Percy Liang, Emmanuel Candès, and Tatsunori Hashimoto. 2025. "s1: Simple Test-Time Scaling." In *Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing (EMNLP 2025)*, Suzhou, China. [Also arXiv:2501.19393 [preprint].]

NovaSky-AI. 2025. "Sky-T1: Train Your Own O1 Preview Model Within $450." GitHub and Hugging Face, January 2025. https://github.com/NovaSky-AI/SkyThought.

OpenAI. 2025. "Reinforcement Fine-Tuning Use Cases." OpenAI Platform Documentation. https://platform.openai.com/docs/guides/rft-use-cases.

Shao, Zhihong, et al. 2024. "DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models." arXiv:2402.03300 [preprint].

Skywork. 2025. "Skywork Open Reasoner 1 Technical Report." arXiv:2505.22312 [preprint].

TensorZero. 2025. "Is OpenAI's Reinforcement Fine-Tuning (RFT) Worth It?" TensorZero Blog. https://www.tensorzero.com/blog/is-openai-reinforcement-fine-tuning-rft-worth-it/.

Xu, Jiecao, et al. 2025. "It Takes Two: Your GRPO Is Secretly DPO." arXiv:2510.00977 [preprint].

Zeng, Weihao, et al. 2025. "SimpleRL-Zoo: Investigating and Taming Zero Reinforcement Learning for Open Base Models in the Wild." arXiv:2503.18892 [preprint].
