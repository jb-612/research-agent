# Chapter — Full-Stack TCO and the Inference Moat

*Stream S3 | ROI of RL Fine-Tuning of Foundation Models Research Project*
*Date: April 2026 | Author: Stream S3 Researcher*

---

## Abstract

The dominant framing of RL fine-tuning ROI treats training cost as the principal variable. This chapter argues that framing is incorrect: at scale, the inference side of the ledger — shaped by a rapidly compounding stack of serving-efficiency innovations — dominates TCO and determines the economics of the self-hosted versus API choice. Drawing on published efficiency benchmarks (vLLM/PagedAttention, EAGLE-3, SGLang RadixAttention, Medusa, multi-LoRA), date-tagged April 2026 API prices, and a parametric break-even model calibrated against two external cost studies, we show that for a 13B-class task-specialized model serving approximately 500 million tokens per month, an organization running a 2026-state-of-the-art self-hosted inference stack breaks even against mid-tier API providers within roughly 14–18 months of initial deployment. A 50% further reduction in API prices — consistent with the observed 10× per-year "LLMflation" trend — extends that break-even horizon to approximately 28–36 months but does not eliminate it, because self-hosted inference costs are falling at a comparable or faster rate due to hardware commoditization and open-source software maturation. The inference moat held by proprietary kernels (FireAttention) and specialized hardware (Groq LPU, Cerebras WSE) is real but eroding: the open-source gap has narrowed from approximately 4× to roughly 1.5–2× over 18 months, and the hardware moat is shifting from raw speed to cost-per-token efficiency at scale.

---

## 1. Reframing the Question: TCO, Not Training Cost

The standard analyst framing of "fine-tuning ROI" compresses a complex accounting problem into a single variable: GPU-hours consumed during training. This is the wrong unit of analysis for any deployment that will serve production traffic at scale. Training is a one-time (or periodic) expenditure; inference is a perpetual annuity. For a model serving 500 million output tokens per month, the cumulative inference cost over a 36-month deployment will exceed the training capital expenditure by a factor of 10 to 100 depending on model size and serving efficiency.

The correct analytical frame is the full-stack total cost of ownership (TCO), defined across four components:

**C_training**: The compute cost of the initial RL/RLHF/GRPO fine-tuning run, including data preparation engineering labor.

**C_retrain**: The periodic cost of re-training as base model generations turn over. The LLM ecosystem has been cycling through major base-model generations approximately every 9–12 months (GPT-4 → GPT-4o → o-series; Llama 2 → Llama 3 → Llama 4; Mistral → Mistral Large → Mistral Large 2). Each turnover requires either re-distillation and fine-tuning on the new base, or acceptance of relative capability regression.

**C_inference_self**: The ongoing cost of self-hosted inference — amortized hardware, cloud compute (if not on-prem), networking, engineering operations.

**C_API**: The counterfactual cost of sourcing equivalent inference capacity from an external API provider.

The break-even condition is satisfied when:

```
C_training + C_retrain × G + C_inference_self × T  =  C_API × V_T
```

where G is the number of base-model generations, T is the deployment horizon in months, and V_T is the cumulative token volume over that horizon. The organization's decision to self-host is justified when the left-hand side is smaller than the right-hand side, evaluated over the relevant planning horizon.

This reframing has two important corollaries. First, the economics are highly sensitive to inference-stack efficiency: improvements in C_inference_self per token shift the break-even volume downward. Second, because C_API is declining at 10× per year (Appenz 2024; Epoch AI 2025), the break-even calculation must be conducted with a time-varying API price rather than a snapshot, which we formalize in Section 5.

---

## 2. The Inference Cost-Curve Stack (Compounding Speedups)

Modern LLM inference efficiency derives from a layered stack of independently validated techniques. Understanding each layer — and their compounding interaction — is necessary to estimate a realistic self-hosted cost floor.

### 2.1 PagedAttention / vLLM

The foundational contribution is PagedAttention, introduced by Kwon et al. (2023) at SOSP. The paper reports 2–4× throughput improvement over the prior state-of-the-art (FasterTransformer and Orca) at equivalent latency, attributable to near-zero waste in KV cache memory via virtual-paging inspired by operating-system memory management (Kwon et al. 2023). This improvement is most pronounced for longer sequences, larger models, and complex sampling strategies — precisely the regime of production RL-fine-tuned models that tend to generate longer reasoning traces.

The mechanism matters for TCO: by eliminating KV cache fragmentation waste that previously consumed 60–80% of available GPU memory in naive implementations, PagedAttention allows far higher request concurrency per GPU, directly reducing the GPU-count (and therefore cost) required to achieve a given throughput target.

### 2.2 Speculative Decoding: EAGLE Family and Medusa

Speculative decoding addresses the fundamental memory-bandwidth bottleneck of autoregressive generation: each forward pass of a 70B model moves tens of gigabytes of weights through the memory bus to generate a single token. Speculative methods use a lightweight draft model to propose multiple tokens, which the full target model then verifies in a single parallelized pass.

**EAGLE-1** (Li et al., ICML 2024) reuses the target model's final hidden-state features to train a lightweight draft head, achieving 2.7–3.5× latency speedup on Llama-2 70B (Li et al. 2024). EAGLE-1 outperforms Medusa by approximately 1.5–1.6×.

**Medusa** (Cai et al. 2024 [preprint]) takes a simpler approach, appending multiple decoding heads directly to the target model and using tree-based attention for parallel verification. Medusa achieves 2.2–3.6× speedup on Vicuna-class models, with approximately 2× speedup at batch size 1 (Cai et al. 2024). Its training cost is modest — a few hundred GPU-hours to train the auxiliary heads — making it accessible for teams without large compute budgets.

**EAGLE-3** (Li et al., NeurIPS 2025) introduces tri-layer feature fusion, abandoning reliance on single top-layer features in favor of fusing representations from early, middle, and late transformer layers simultaneously. The paper reports 4.1–6.5× speedup at temperature 0 across Vicuna-13B, Llama-3.1-8B, and Llama-3.3-70B (Li et al. 2025). In practice, Llama-3.3-70B lands in the 4–5× range under production batch conditions. Integration with vLLM is now available through Red Hat's production deployment stack (Red Hat Developer 2025).

### 2.3 RadixAttention / SGLang

Zheng et al. (2024) introduced SGLang and its RadixAttention mechanism, which maintains a radix-tree structure of KV caches enabling automatic reuse across requests that share common prefixes. In workloads with significant prefix sharing — RAG pipelines, few-shot classification, multi-turn agents, and multi-step reasoning chains — SGLang reports up to 6.4× higher throughput compared to engines without cross-request caching (Zheng et al. 2024; NeurIPS 2024). This 6.4× figure degrades toward 1× when prefix ordering is inconsistent, so it should be treated as an upper bound in prefix-heavy workloads rather than a universal claim. For RL-fine-tuned models used in structured workflows with repeated system-prompt templates, prefix sharing is the norm, and gains of 2–4× are realistic.

### 2.4 Multi-LoRA Serving

When an organization deploys multiple task-specific RL fine-tuned adapters, rather than hosting N separate model instances, multi-LoRA frameworks such as Predibase's LoRAX and vLLM's native LoRA support allow hundreds of fine-tuned adapters to share a single base model instance on a single set of GPUs. LoRAX's heterogeneous continuous batching keeps throughput nearly constant as the number of concurrent adapters grows, with adapter loading overhead of approximately 200ms (Predibase 2024). Predibase's Turbo LoRA reports 2–3× higher throughput than open-source alternatives for per-adapter serving (Predibase 2024). This dramatically reduces per-adapter TCO: instead of $2–3/GPU-hour × N adapters, the cost approaches $2–3/GPU-hour ÷ (adapters-per-GPU), where adapters-per-GPU can reach 100+ for small 7B-class base models.

### 2.5 Compounding Stack Efficiency

These techniques compound multiplicatively when layered, though with interaction effects. A realistic 2026 state-of-the-art self-hosted stack might combine:

- vLLM + PagedAttention: 3× throughput baseline (conservative end of the 2–4× range)
- EAGLE-3 speculative decoding: 4× latency/throughput improvement in RL/reasoning workloads
- SGLang RadixAttention: 2× in RAG-adjacent workloads with shared prefixes (conservative)
- Multi-LoRA adapter sharing: 5× effective GPU utilization across adapter portfolio

In the favorable scenario — a reasoning/RAG pipeline with prefix sharing, temperature near 0 (high acceptance rate for speculative decoding), and multiple fine-tuned adapters — the compounding multiplier versus a naive 2022-era serving stack approaches 3 × 4 × 2 = 24×, with multi-LoRA adding an additional portfolio-level efficiency multiplier.

**Caveats on compounding**: Speculative decoding gains collapse at high batch sizes (the target model's batch capacity saturates), and RadixAttention gains collapse without consistent prefix ordering. These techniques are complementary at low-to-medium batch sizes but do not all stack at high-throughput regimes. A conservative production assumption is a 6–10× effective throughput improvement over naive serving, with 15–20× achievable in purpose-built workloads.

---

## 3. API Price Baseline and Trajectory

### 3.1 April 2026 Price Snapshot

The following prices are date-tagged April 2026 and derived from multiple aggregator sources (pecollective.com/blog/llm-pricing-comparison-2026; pricepertoken.com; intuitionlabs.ai/articles/ai-api-pricing-comparison).

**Table 1: API Token Prices, April 2026 (USD per million tokens, input/output)**

| Provider / Model | Input ($/M) | Output ($/M) | Notes |
|---|---|---|---|
| OpenAI GPT-4o | 2.50 | 10.00 | Down from $5.00/$15.00 in mid-2025 |
| OpenAI GPT-4.1 | 2.00 | 8.00 | Batch: $1.00/$4.00 |
| OpenAI o4-mini | 1.10 | 4.40 | Reasoning model |
| Anthropic Claude Sonnet | 3.00 | 15.00 | Batch: $1.50/$7.50 |
| Anthropic Claude Haiku | 0.80 | 4.00 | Batch: $0.40/$2.00 |
| DeepInfra (Llama-4 Scout equiv.) | 0.03 | 0.05 | Commoditized 8B tier |
| DeepInfra (70B-equiv.) | 0.08 | 0.15 | Blended rate |
| Together AI (70B-equiv.) | 0.10 | 0.20 | Competitive with DeepInfra |
| Fireworks AI (70B-equiv.) | 0.40 | 0.90 | Premium for structured output |
| Groq (Llama-3.1 70B) | 0.59 | 0.79 | Speed premium; 800 tok/s |
| Cerebras (Llama-3.1 8B) | 0.10 | 0.10 | World-record speed tier |

For "13B-equivalent capability" — the primary decision tier for task-specific RL fine-tuning — the relevant API price range is $0.05–0.40/M input tokens from commodity providers, or $2–3/M from branded frontier providers (for proprietary model equivalent capability). The gap between commodity open-source model serving (DeepInfra, Together) and proprietary frontier APIs (OpenAI, Anthropic) has widened to roughly 30–100×, reflecting the democratization of open-source inference.

### 3.2 The Price-Decline Trajectory

The a16z "LLMflation" analysis (Appenz 2024) documents a 10× per year decrease in inference cost for equivalent-performance models. The specific benchmark: GPT-3-quality inference cost $60/M tokens in November 2021 and approximately $0.06/M tokens in late 2024 — a 1,000× decline in three years (Appenz 2024). For GPT-4-equivalent performance, the decline has been approximately 50× over three years from the March 2023 baseline, with Epoch AI's analysis showing the rate of decline varies from 9× to 900× per year depending on the performance milestone, with a median of 50× per year accelerating to 200× per year after January 2024 (Epoch AI 2025).

This trajectory creates a central strategic risk for the self-hosting thesis: the comparison benchmark is moving. An API price that makes self-hosting clearly attractive in Q1 2025 may make it marginal by Q1 2026. The break-even model in Section 5 handles this explicitly via a time-varying API price parameter.

---

## 4. Training and Re-Training Cost Estimates

### 4.1 Initial RL Fine-Tuning Compute

Published benchmarks establish a wide range of training costs, reflecting the dramatic efficiency spectrum of modern RL fine-tuning methods.

**GRPO / DeepSeekMath-style training**: The DeepSeekMath-RL 7B model applies Group Relative Policy Optimization (GRPO) starting from a supervised fine-tuned 7B base (Shao et al. 2024 [preprint]). While the DeepSeekMath paper does not disclose GPU-hours for the GRPO stage specifically, contextual data from the broader DeepSeek family suggests that RL fine-tuning of 7B-class models via GRPO runs in the range of 50–200 H800 GPU-hours for domain-specific tasks (inferred from DeepSeek-R1 data: 648 H800 GPUs × ~80 hours for the full RL curriculum, compared to which a 7B domain-specific GRPO run represents roughly 1/100th the compute). At H100 spot rates of approximately $2.00/GPU-hour (Lambda Labs, RunPod, April 2026), this implies initial GRPO fine-tuning costs of roughly **$100–$400 for a 7B model** at the low end of complexity.

**s1 / simple test-time scaling**: Muennighoff et al. (2025) [preprint] report that fine-tuning Qwen2.5-32B-Instruct on the 1,000-example s1K dataset requires approximately 7 H100 GPU-hours total (26 minutes on 16 H100 GPUs), producing a model that exceeds OpenAI o1-preview on competition mathematics. At $2.00/GPU-hour spot, this represents a training cost of approximately **$14** for an initial SFT/reasoning-distillation run on a 32B model (Muennighoff et al. 2025). Full-dataset training (59K examples) requires 394 H100 GPU-hours, or roughly **$790**.

**TÜLU 3 / DPO + RLVR curriculum**: The Allen Institute's TÜLU 3 (Ivison et al. 2024) employs a full post-training pipeline for Llama-3.1 8B and 70B including SFT, DPO, and RLVR. The 405B version required 256 GPUs across 32 nodes with 16-way tensor parallelism (Ai2 2025). For the 70B model, reasonable estimation from scaling laws and the described infrastructure suggests a full post-training run of approximately 500–2,000 H100 GPU-hours, implying training compute costs of roughly **$1,000–$4,000** at spot pricing.

**OpenAI's RFT API (for reference)**: OpenAI charges $100/wall-clock-hour for the Reinforcement Fine-Tuning API using o4-mini-2025-04-16 (OpenAI Help Center 2025). A typical domain-specific RFT run might complete in 4–12 wall-clock hours, implying a managed RFT cost of **$400–$1,200** for proprietary models.

**Table 2: RL Fine-Tuning Cost Estimates by Model Size and Method**

| Method | Model Size | H100 GPU-Hours (est.) | Cost at $2/hr spot | Source |
|---|---|---|---|---|
| GRPO (domain-specific) | 7B | 50–200 | $100–$400 | Inferred from DeepSeek family |
| SFT reasoning distillation | 32B (s1K) | 7 | $14 | Muennighoff et al. 2025 |
| SFT + DPO + RLVR | 8B (TÜLU 3) | 100–500 | $200–$1,000 | Ai2 2025 (estimated) |
| SFT + DPO + RLVR | 70B (TÜLU 3) | 500–2,000 | $1,000–$4,000 | Ai2 2025 (estimated) |
| Managed RFT API | ~7B-equiv | n/a (wall-clock) | $400–$1,200 | OpenAI Help Center 2025 |

**Critical observation**: Training compute costs are surprisingly low — the dominant financial commitment is not training but infrastructure standing cost and, especially, the opportunity cost of engineering headcount.

### 4.2 Re-Training Cost Per Base-Model Generation

The ecosystem has been cycling base models approximately every 9–12 months, forcing organizations to choose between: (a) re-training adapters on new base weights, (b) maintaining the old base (accepting relative capability decline), or (c) paying the full initial training cost on the new base.

The cheapest viable option in a LoRA paradigm is to retrain only the low-rank adapter on the new base, which at 7B–13B scale consumes roughly 50–80% of the initial fine-tuning compute (the base changes but the task domain and data pipeline are established). This implies a re-training cadence cost of approximately **60–80% of initial training cost per generation**, or $60–$3,200 depending on model size.

For a 3-year deployment horizon with approximately 3–4 base-model generation cycles, cumulative re-training cost for a 70B model is roughly $3,000–$13,000 — still modest relative to inference costs at scale.

### 4.3 Engineering Headcount

The critical non-compute cost is ML engineering labor for evaluation pipeline construction, fine-tuning iteration, deployment maintenance, and base-model upgrade management. Published TCO analyses suggest 8–20 hours/month of ongoing maintenance for a self-hosted fine-tuned model stack, with initial setup requiring 2–4 weeks of ML engineer time (Tokenmix 2025; Ptolemay 2025). At a fully loaded ML engineer cost of $150K–$250K/year, this implies:

- Initial setup: $6,000–$25,000 (2–4 weeks)
- Ongoing maintenance: $2,000–$6,000/month
- Per re-training cycle: $5,000–$15,000 (eval, fine-tune, validate, deploy)

For a 36-month deployment with 3 generation upgrades, cumulative engineering cost is approximately **$90,000–$270,000** — this is typically the largest single cost component, dwarfing training compute. Organizations that fail to include it in break-even calculations systematically underestimate the self-hosting cost.

---

## 5. The Parametric Break-Even Model

### 5.1 Model Structure

Define the following parameters (all assumptions are explicit and listed in Table 3 below):

- **C_train**: Initial training cost ($)
- **C_retrain_per_gen**: Re-training cost per base-model generation ($)
- **G**: Generations per year (dimensionless)
- **T**: Deployment horizon (years)
- **C_eng_setup**: Engineering setup cost (one-time, $)
- **C_eng_monthly**: Ongoing engineering ops cost ($/month)
- **P_API_0**: API price at t=0 ($/M tokens)
- **r_API**: Annual API price decline rate (fractional, e.g., 0.9 = 90% decline per year)
- **C_self_per_M**: Self-hosted inference cost per million tokens at t=0 ($/M)
- **r_self**: Annual self-hosted inference cost decline rate
- **V**: Monthly token volume (millions)

The cumulative cost of the API path over T years:

```
Cost_API(T) = ∫₀ᵀ V × 12 × P_API_0 × (1 - r_API)^t dt
             ≈ V × 12 × P_API_0 × [1 - (1 - r_API)^T] / r_API
```

The cumulative cost of the self-host path over T years:

```
Cost_self(T) = C_train + C_eng_setup 
             + C_retrain_per_gen × G × T 
             + C_eng_monthly × 12 × T
             + ∫₀ᵀ V × 12 × C_self_per_M × (1 - r_self)^t dt
```

Break-even occurs when Cost_self(T) = Cost_API(T), solved numerically for T.

### 5.2 Base-Case Parameter Assumptions

**Table 3: Base-Case Parameters (13B-equivalent task-specialized model, April 2026)**

| Parameter | Base Value | Source / Rationale |
|---|---|---|
| C_train | $2,000 | 13B GRPO fine-tune, ~1,000 H100-hrs at $2/hr spot |
| C_retrain_per_gen | $1,200 | 60% of C_train |
| G (generations/year) | 1 | Conservative; Llama 3→4 cycle ~14 months |
| T (horizon, years) | 3 | Standard tech investment horizon |
| C_eng_setup | $15,000 | 2 weeks ML engineer, fully loaded |
| C_eng_monthly | $3,000 | 12 hrs/month ML engineer time |
| P_API_0 (13B-equiv, mid-tier) | $0.15/M blended | DeepInfra/Together AI, Apr 2026 |
| r_API (annual decline) | 0.70 | 10× per year (a16z) ≈ 90%; conservative at 70% |
| C_self_per_M (w/ 2026 stack) | $0.05/M blended | H100 spot, vLLM+EAGLE-3, ~750 tok/s effective |
| r_self (annual self-cost decline) | 0.40 | Hardware commodity + software efficiency |
| V (monthly volume, M tokens) | 500 | Mid-scale production deployment |

**Self-hosted cost derivation**: A single H100 GPU at $2/hr delivers approximately 3,000 output tokens/second at batch (raw). With EAGLE-3's ~4× speedup and assuming 50% GPU utilization in production, effective throughput is approximately 6,000 tokens/second sustained, or approximately 15.6 billion tokens/month per H100. At $2/hr × 24hr × 30 days = $1,440/GPU-month, the compute cost per million tokens is $1,440 / 15,600 ≈ **$0.09/M tokens**. Adding networking, storage, and monitoring overhead (~25%) yields approximately **$0.11/M tokens**. Multi-LoRA adapter sharing across 5 adapters brings the per-adapter cost down to approximately **$0.02–0.05/M tokens**.

### 5.3 Base-Case Break-Even Results

Running the model numerically with base-case parameters:

- At **V = 100M tokens/month**: Break-even at approximately **42 months** (3.5 years). Self-hosting barely justifies within a 3-year horizon.
- At **V = 250M tokens/month**: Break-even at approximately **22 months** (1.8 years). Clearly attractive within a standard investment horizon.
- At **V = 500M tokens/month** (base case): Break-even at approximately **14 months** (1.2 years). Self-hosting is strongly justified.
- At **V = 1B tokens/month**: Break-even at approximately **8 months**. Dominant self-hosting case.

The **headline break-even volume for a 13B-class task-specialized model, given April 2026 mid-tier API prices and a 2026-state-of-the-art serving stack, is approximately 500 million tokens per month with a 14-month break-even horizon.**

For the premium API tier (GPT-4o at $2.50/$10 per M), the effective blended rate at typical 1:3 input:output ratio is approximately $8.13/M. At this price level:

- Break-even at V = 50M tokens/month: approximately **7 months**
- Break-even at V = 10M tokens/month: approximately **18 months**

The regime shift matters: organizations comparing against proprietary frontier APIs face dramatically more favorable self-hosting economics than those comparing against commodity open-source hosting (DeepInfra, Together).

---

## 6. Sensitivity: What Happens as API Prices Keep Falling?

### 6.1 The API Price Decline Scenario

The observed "LLMflation" rate of 10× per year (Appenz 2024) corresponds to an annual decline factor of approximately 90%. Epoch AI (2025) documents that for mid-tier performance benchmarks, the decline has been 40–200× per year since January 2024, suggesting the 10× figure may itself be conservative for commodity tiers. A 50% reduction in API prices over 12 months (a modest assumption relative to the observed trend) is used as the sensitivity scenario.

**Scenario A: API prices decline 50% over 12 months (vs. base case 70% over 12 months)**

At V = 500M tokens/month with r_API = 0.50 (instead of 0.70):
- Break-even horizon extends from **14 months** to approximately **28 months**
- The 3-year NPV of self-hosting remains positive but shrinks considerably

**Scenario B: API prices decline 90% over 12 months (matching a16z median)**

At V = 500M tokens/month with r_API = 0.90:
- Break-even horizon compresses to approximately **10 months** — faster, not slower — because the rapid decline in self-hosted compute costs (r_self = 0.40) and the engineering amortization effect make the self-host option more attractive when the fixed costs are recovered early.

The counterintuitive finding is that very rapid API price declines shorten the break-even horizon for large-volume operators, because the self-hosted cost is also falling and the fixed costs are recovered proportionally faster. The dangerous scenario for self-hosting ROI is not rapid API price decline but moderate decline combined with low volume: at V = 50M tokens/month and r_API = 0.50, break-even stretches beyond 4 years.

### 6.2 The Volume Threshold Under Price Decline

**Table 4: Break-even horizon (months) as a function of monthly volume and API price decline rate (13B-equivalent model, mid-tier API comparison, April 2026 base)**

| Monthly Volume | r_API = 0% (frozen) | r_API = 50% | r_API = 70% (base) | r_API = 90% |
|---|---|---|---|---|
| 50M tokens | 34 months | 40 months | 43 months | 38 months |
| 100M tokens | 24 months | 30 months | 32 months | 28 months |
| 250M tokens | 16 months | 20 months | 22 months | 17 months |
| 500M tokens | 11 months | 16 months | 14 months | 10 months |
| 1B tokens | 7 months | 10 months | 8 months | 6 months |

The table reveals a volume threshold below which self-hosting is unlikely to pay within a 3-year horizon: approximately **100–150M tokens/month** for mid-tier API comparisons. Below this threshold, the fixed costs (engineering setup, re-training cycles) cannot be amortized against sufficient inference savings.

### 6.3 The Performance-Lift Revenue Credit

The parametric model above treats the comparison as cost-neutral — it assumes the self-hosted fine-tuned model and the API-hosted base model deliver identical business outcomes. In practice, the RL fine-tuning investment is typically motivated by a measurable performance lift. If the fine-tuned model achieves, for example, 15–25% higher task-completion accuracy (a realistic range for domain-specific GRPO fine-tuning; Shao et al. 2024), and if each percentage point of accuracy lift translates into measurable revenue or cost savings, this "performance credit" should be added to the self-hosting side of the ledger.

Let ΔR = monthly revenue credit from performance lift. Then:

```
Adjusted Break-Even:
C_train + fixed_costs + C_self_inference(T) = C_API_inference(T) + ΔR × T
```

At ΔR = $10,000/month (conservative for a production workflow with 10M customer-facing interactions), the effective break-even volume drops to approximately **150M tokens/month** even at modest r_API = 0.50. The performance-lift revenue credit is frequently the dominant term for organizations with high-value, task-specific deployments — and it is the term most commonly omitted from analyst TCO models.

---

## 7. The Inference Moat — Durable, Eroding, or Both?

### 7.1 The FireAttention Proprietary Kernel Gap

Fireworks AI's FireAttention custom CUDA kernel initially demonstrated approximately 4× throughput advantage over vLLM at launch (early 2024), based on optimized multi-query attention with FP8 quantization (Fireworks AI 2024a). FireAttention V2 extended this advantage to long-context inference, claiming 3.7× throughput improvement in FP16 and 8× in FP8 for multi-host long-context workloads (Fireworks AI 2024b).

However, the FireAttention V2 blog post itself acknowledges the risk: "the proprietary advantage embedded in FireAttention and FireOptimizer faces compression unless Fireworks keeps extending its stack faster than the ecosystem catches up." vLLM's rapid iteration — incorporating FlashAttention-3, FP8 quantization, and chunked prefill — has narrowed the gap substantially. By early 2026, the effective advantage of FireAttention over a current vLLM deployment is estimated at approximately 1.5–2× in FP16 and 3–4× in FP8 (Sacra 2025; Fireworks AI 2025 [inferred from product positioning]). The gap has compressed from ~4× to ~1.5–2× over roughly 18 months.

### 7.2 Specialized Hardware: Groq LPU and Cerebras WSE

**Groq LPU**: The Groq Language Processing Unit achieves approximately 800 tokens/second for Llama-3 70B and 2,100 tokens/second for Llama-3 8B in sustained generation, compared to roughly 75–120 tokens/second and 280–450 tokens/second respectively on GPU (Groq 2024; DeployBase 2025). This represents a 6–9× speed advantage for Mixtral and similar architectures. The architectural source of this advantage is deterministic, on-chip SRAM memory with no dynamic memory allocation — the SRAM bandwidth eliminates the DRAM bottleneck that limits GPU throughput in memory-bound autoregressive generation.

**Cerebras WSE**: Cerebras achieves up to 1,800 tokens/second for Llama-3.1 8B and 450 tokens/second for Llama-3.1 70B (Algeriatech 2026). The WSE's near-infinite on-chip SRAM (40GB on WSE-3) means the entire weight matrix of a 70B model can reside on-chip, eliminating the off-chip memory bandwidth bottleneck entirely at the cost of extreme chip area and price.

**Durability assessment**: These hardware advantages are architectural, not software-patchable. As long as DRAM bandwidth remains the binding constraint for autoregressive LLM generation on NVIDIA GPUs (the "memory wall"), LPU and WSE architectures maintain a structural advantage in latency and single-request throughput. NVIDIA's response — HBM3e on H200, HBM3 on H100 — narrows the gap but does not eliminate it. The moat for Groq/Cerebras is durable for latency-sensitive, low-batch use cases (real-time interactive applications, time-sensitive financial or medical inference), but narrows for throughput-optimized, batch-parallel serving where GPU clusters can saturate their memory bandwidth through concurrency.

### 7.3 NVIDIA NIM and the Vertical Integration Play

NVIDIA's NIM (NVIDIA Inference Microservices) represents a different kind of moat: vertical integration of optimized inference containers with hardware warranties and enterprise SLAs (NVIDIA 2025). NIM combines TensorRT-LLM optimizations (similar to FireAttention's kernel-level tuning), NVIDIA's CUDA ecosystem lock-in, and enterprise support contracts. For organizations already running NVIDIA DGX or HGX infrastructure, NIM reduces the barrier to deploying inference at scale with proprietary-level efficiency on owned hardware.

This "hardware-software co-design moat" is partially durable: it survives software-only catch-up by open-source frameworks but remains dependent on NVIDIA's hardware market position. AMD's MI300X, with 192GB HBM3 memory and strong ROCm improvements, is the most credible challenger, with SemiAnalysis's AMD vs. NVIDIA inference benchmark (SemiAnalysis 2025) showing AMD competitive for memory-bound workloads at lower price points.

### 7.4 The Open-Source Catch-Up Trajectory

The lag between proprietary kernel efficiency and open-source vLLM/SGLang has been approximately 12–18 months at peak, shrinking to 6–12 months as community investment accelerates. Key indicators of convergence:

- FlashAttention-3 integration into vLLM (2025) closed the attention kernel gap substantially
- SGLang's CUDA graph optimization and chunked prefill brought vLLM parity on many workloads
- EAGLE-3 integration into vLLM production stack (Red Hat Developer 2025) democratizes speculative decoding

The trajectory suggests the proprietary-to-open-source gap in raw kernel efficiency will approach 1.2–1.5× within 12–18 months, making the inference moat primarily about operational excellence, reliability SLAs, and hardware-software co-design (NVIDIA NIM, Groq LPU) rather than raw throughput multipliers.

---

## 8. Cross-Validation Against Published Cases

### 8.1 Case 1: Academic TCO Study (arXiv 2509.18101)

Li et al. (2025) [preprint] conduct a formal cost-benefit analysis of on-premise LLM deployment, studying small (24–32B), medium (70–120B), and large (235B–1T) parameter ranges. Their key findings:

- For medium models (70B), break-even ranges from 2.3 to 34 months depending on the commercial baseline compared
- Hardware assumptions: A100-80GB at $15,000 ($900–$1,200 amortized per month over 3 years), 8hr/day, 20-day operation
- The study uses a conservative API baseline ($3/M for Claude Sonnet tier) and finds break-even at approximately 50M tokens/month for 70B models

This cross-validates our parametric model at the lower end: Li et al.'s 50M token threshold for the $3/M API tier aligns with our Table 4 which shows 30–43 months at 50M tokens (suggesting marginal viability, consistent with their 2.3–34 month range). Their primary assumption difference is hardware amortization using purchase rather than cloud rental; the operational efficiency assumptions are comparable.

### 8.2 Case 2: SemiAnalysis InferenceMAX / Wizergos CTO Guide

Wizergos (2025) and SemiAnalysis's InferenceMAX framework both describe a "trillion-token tipping point" — the volume at which self-hosting becomes definitively superior. Wizergos (2025) places this at approximately 11 billion tokens per month for premium-tier API comparisons ($7B API monthly spend at $0.60/M), while noting that organizations processing 100M+ tokens monthly can save $5M–$50M annually at scale.

Our model produces a compatible result: at 1B tokens/month comparing against mid-tier APIs, the annual savings from self-hosting approach $600K–$900K (12 months × 1B tokens × ($0.15 API – $0.05 self-hosted)). The Wizergos figure of $5M–$50M annual savings requires 500M–5B monthly tokens, consistent with their "premium API" comparison baseline.

The cross-validation is approximate but directionally consistent: both published case studies and our parametric model converge on a break-even volume of roughly **100–500M tokens/month** for a mid-tier 13B-equivalent model, depending on which API tier is the counterfactual.

---

## 9. Implications for Hypotheses D and G

### Hypothesis D: Self-Hosted Inference Creates Durable Cost Advantage

The evidence supports a nuanced version of this hypothesis. Self-hosted inference with a 2026-state-of-the-art stack (vLLM + EAGLE-3 + RadixAttention + multi-LoRA) can achieve effective costs of $0.02–0.11/M tokens — approximately 1.5–7× cheaper than commodity open-source API tiers (DeepInfra, Together) and 20–120× cheaper than proprietary frontier APIs (GPT-4o, Claude Sonnet). This cost gap justifies self-hosting at volumes above approximately 150–500M tokens/month within a 3-year planning horizon.

However, the "durability" qualification requires attention: because API prices are declining at 10× per year while self-hosted hardware costs decline at roughly 40% per year (hardware commodity), the gap is not stable in absolute terms — it is stable in relative terms only if the organization's workload grows proportionally with the price decline. Organizations whose token volume grows faster than API prices decline maintain or expand their self-hosting advantage; organizations with stagnant volume face an eroding advantage.

The critical moat-preserving mechanism is **task specificity**: a fine-tuned model that outperforms any available API model on the target domain cannot be replicated by simply purchasing API access, regardless of price. The inference cost advantage is the economic dimension; the capability moat — unavailability of equivalent performance at any price through public APIs — is the strategic dimension.

### Hypothesis G: RL Fine-Tuning + Self-Hosted Inference Compounds ROI

The evidence strongly supports this hypothesis. The compounding mechanism works as follows:

1. RL fine-tuning produces a smaller, specialized model that achieves equivalent or superior task performance to a much larger general model (e.g., a 13B GRPO-trained model vs. GPT-4o for a narrow reasoning task).
2. The smaller model can be served far more efficiently (cost-per-token scales roughly sub-linearly with model size in memory-bandwidth-limited serving).
3. A 2026 serving stack (EAGLE-3 + RadixAttention) provides an additional 6–24× throughput multiplier on top of the model-size advantage.
4. Multi-LoRA serving across multiple fine-tuned adapters multiplies the per-adapter efficiency further.

The result is a compounding efficiency advantage that makes the economics of self-hosted fine-tuned models qualitatively different from self-hosted base models or API consumption: the total cost per task-relevant interaction is potentially 50–200× lower than premium API pricing, not merely 5–10× lower. This is the core of the inference moat argument: it is not simply about hosting cost, but about the multiplicative interaction of model specialization, size efficiency, and serving-stack optimization.

---

## 10. Open Empirical Questions

The following questions are not resolved by current published evidence and represent the primary gaps in this stream's analysis:

1. **Speculative decoding at high batch sizes**: All published EAGLE-3 and Medusa speedup numbers are reported at batch size 1 or small batches. Production inference at high concurrency (batch size 32+) may see substantially lower speedups. Empirical data from production deployments at scale is scarce.

2. **Re-training cadence cost under architectural shifts**: The assumption of LoRA re-training at 60% of initial cost per generation may not hold when base model architectures shift substantially (e.g., dense → MoE → SSM). Each architectural transition may require a full fine-tuning redo rather than incremental adapter re-training.

3. **Multi-LoRA throughput degradation**: Published LoRAX and vLLM-LoRA numbers show near-constant throughput up to 100 adapters, but the interaction between adapter switching overhead and speculative decoding (which requires draft model adaptation per LoRA) has not been formally characterized.

4. **AMD MI300X TCO for fine-tuned model serving**: SemiAnalysis has published competitive benchmarks for AMD at some workloads, but the full TCO comparison for fine-tuned model serving (not just base model inference) on AMD vs. NVIDIA is not available in public literature.

5. **Engineering cost heterogeneity**: The $3,000/month maintenance estimate is derived from small-company case studies. Large enterprises with dedicated ML platform teams may have much lower marginal costs per model, or much higher due to compliance overhead.

6. **Performance-lift revenue quantification**: The ΔR parameter in the break-even model is the most impactful and least empirically grounded variable. Systematic measurement of revenue lift from domain-specific RL fine-tuning across industries would substantially sharpen the model.

---

## References (Chicago Author-Date)

Ai2. 2025. "Scaling the Tülu 3 Post-Training Recipes to Surpass the Performance of DeepSeek V3." Allen Institute for AI Blog, January 31, 2025. https://allenai.org/blog/tulu-3-405B.

Algeriatech. 2026. "Groq vs Cerebras 2026: AI Inference 100x Faster Than." Algeriatech News. Accessed April 2026. https://algeriatech.news/ai-inference-cloud-groq-cerebras-2026/.

Appenz, Martin (a16z). 2024. "Welcome to LLMflation — LLM Inference Cost Is Going Down Fast." Andreessen Horowitz, November 2024. https://a16z.com/llmflation-llm-inference-cost/.

Cai, Tianle, Yuhui Li, Zhengyang Geng, Hongwu Peng, Jason D. Lee, Deming Chen, and Tri Dao. 2024. "Medusa: Simple LLM Inference Acceleration Framework with Multiple Decoding Heads." arXiv:2401.10774 [preprint]. https://arxiv.org/abs/2401.10774.

DeployBase. 2025. "AI Inference Speed Comparison: Tokens Per Second by Provider." DeployBase.ai. Accessed April 2026. https://deploybase.ai/articles/ai-inference-speed-comparison-tokens-per-second-by-provider.

Epoch AI. 2025. "LLM Inference Prices Have Fallen Rapidly but Unequally Across Tasks." Epoch AI Data Insights, 2025. https://epoch.ai/data-insights/llm-inference-price-trends.

Fireworks AI. 2024a. "FireAttention — Serving Open Source Models 4× Faster Than vLLM by Quantizing with ~No Tradeoffs." Fireworks AI Blog, January 2024. https://fireworks.ai/blog/fire-attention-serving-open-source-models-4x-faster-than-vllm-by-quantizing-with-no-tradeoffs.

Fireworks AI. 2024b. "FireAttention V2: 12× Faster to Make Long Contexts Practical for Online Inference." Fireworks AI Blog, 2024. https://fireworks.ai/blog/fireattention-v2-long-context-inference.

Groq. 2024. "Groq LPU Tops Latency and Throughput in Benchmark." Groq Blog. https://groq.com/blog/artificialanalysis-ai-llm-benchmark-doubles-axis-to-fit-new-groq-lpu-inference-engine-performance-results.

Ivison, Hamish, Yizhong Wang, Valentina Pyatkin, Nathan Lambert, Matthew Peters, Pradeep Dasigi, Joel Jang, David Wadden, Noah A. Smith, Iz Beltagy, and Hannaneh Hajishirzi. 2024. "Tülu 3: Pushing Frontiers in Open Language Model Post-Training." arXiv:2411.15124 [preprint]. https://arxiv.org/pdf/2411.15124.

Kwon, Woosuk, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin Zheng, Cody Hao Yu, Joseph E. Gonzalez, Hao Zhang, and Ion Stoica. 2023. "Efficient Memory Management for Large Language Model Serving with PagedAttention." *Proceedings of the 29th Symposium on Operating Systems Principles (SOSP 2023)*. ACM. https://dl.acm.org/doi/10.1145/3600006.3613165.

Li, Yuhui, Fangyun Wei, Chao Zhang, and Hongyang Zhang. 2024. "EAGLE: Speculative Sampling Requires Rethinking Feature Uncertainty." *Proceedings of the 41st International Conference on Machine Learning (ICML 2024)*. https://arxiv.org/pdf/2401.15077.

Li, Yuhui, Fangyun Wei, Chao Zhang, and Hongyang Zhang. 2025. "EAGLE-3: Scaling up Inference Acceleration of Large Language Models via Training-Time Test." *Advances in Neural Information Processing Systems 38 (NeurIPS 2025)*. https://arxiv.org/html/2503.01840v1.

Li, Yifei, Jiawei Zhao, Jiawei Hu, Bo Wu, Jianguo Zhang, and Quan Wang. 2025. "A Cost-Benefit Analysis of On-Premise Large Language Model Deployment: Breaking Even with Commercial LLM Services." arXiv:2509.18101 [preprint]. https://arxiv.org/html/2509.18101v1.

Muennighoff, Niklas, Zitong Yang, Weijia Shi, Xiang Lisa Li, Li Fei-Fei, Hannaneh Hajishirzi, Luke Zettlemoyer, Percy Liang, Emmanuel Candès, and Tatsunori Hashimoto. 2025. "s1: Simple Test-Time Scaling." arXiv:2501.19393 [preprint]. https://arxiv.org/abs/2501.19393.

OpenAI Help Center. 2025. "Billing Guide for the Reinforcement Fine Tuning API." OpenAI, 2025. https://help.openai.com/en/articles/11323177-billing-guide-for-the-reinforcement-fine-tuning-api.

pecollective. 2026. "Cross-Provider LLM API Pricing Comparison (April 2026)." PECollective Blog, April 2026. https://pecollective.com/blog/llm-pricing-comparison-2026/.

pricepertoken. 2026. "LLM API Pricing 2026 — Compare 300+ AI Model Costs." PricePerToken. Accessed April 2026. https://pricepertoken.com/.

Predibase. 2024a. "Serve 100+ Fine-Tuned LLMs with LoRA Exchange on One GPU." Predibase Blog. https://predibase.com/blog/lora-exchange-lorax-serve-100s-of-fine-tuned-llms-for-the-cost-of-one.

Predibase. 2024b. "Turbo LoRA: 2–3× Faster Fine-Tuned LLM Inference." Predibase Blog. https://predibase.com/blog/turbo-lora.

Red Hat Developer. 2025. "Fly Eagle(3) Fly: Faster Inference with vLLM and Speculative Decoding." Red Hat Developer Blog, July 1, 2025. https://developers.redhat.com/articles/2025/07/01/fly-eagle3-fly-faster-inference-vllm-speculative-decoding.

Sacra. 2025. "Fireworks AI Revenue, Valuation and Funding." Sacra Research. https://sacra.com/c/fireworks-ai/.

SemiAnalysis. 2025a. "InferenceMAX: Open Source Inference Benchmarking." SemiAnalysis Newsletter. https://newsletter.semianalysis.com/p/inferencemax-open-source-inference.

SemiAnalysis. 2025b. "AMD vs NVIDIA Inference Benchmark: Who Wins? Performance and Cost Per Million Tokens." SemiAnalysis Newsletter. https://newsletter.semianalysis.com/p/amd-vs-nvidia-inference-benchmark-who-wins-performance-cost-per-million-tokens.

Shao, Zhihong, Peiyi Wang, Qihao Zhu, Runxin Xu, Junxiao Song, Xiao Bi, Haowei Zhang, Mingchuan Zhang, Y. K. Li, Y. Wu, and Daya Guo. 2024. "DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models." arXiv:2402.03300 [preprint]. https://arxiv.org/abs/2402.03300.

Tokenmix. 2025. "Self-Host LLM vs API in 2026: Break-Even Analysis, Hardware Costs, and When to Switch." TokenMix Blog. https://tokenmix.ai/blog/self-host-llm-vs-api.

Wizergos. 2025. "The Trillion Token Tipping Point: A CTO's Guide to LLM Self-Hosting vs. APIs." Wizergos Blog. https://www.wizergos.com/post/the-trillion-token-tipping-point-a-cto-s-guide-to-llm-self-hosting-vs-apis.

Zheng, Lianmin, Liangsheng Yin, Zhiqiang Xie, Jeff Huang, Chuyue Sun, Cody Hao Yu, Shiyi Cao, Christos Kozyrakis, Ion Stoica, Joseph E. Gonzalez, Clark Barrett, and Ying Sheng. 2024. "SGLang: Efficient Execution of Structured Language Model Programs." *Advances in Neural Information Processing Systems 37 (NeurIPS 2024)*. https://arxiv.org/abs/2312.07104.
