# Chapter — Durability of Fine-Tuning Gains Across Base-Model Generations

*Stream S1 | Phase 2 Deep Research | ROI of RL Fine-Tuning of Foundation Models*
*Compiled: 2026-04-28*

---

## Abstract

When a foundation-model provider releases a major new base model, every organization that invested in domain-specific fine-tuning faces a consequential question: does the tuned model's performance lead survive the upgrade, or must the organization pay full re-training costs again? This chapter synthesizes the available empirical evidence across five sub-questions. Direct longitudinal evidence — meaning published benchmarks that hold the domain task constant while varying the base-model generation — is sparse to the point of near-absence; the field lacks the controlled comparison studies that would definitively answer the question. Mechanistic evidence is stronger: Lin et al. (2025) demonstrate that diff-vector recycling (applying the weight-delta of a fine-tuned source model to a new base) can transfer 47–62 percentage points of instruction-following improvement across Llama 3.0→3.1 minor-version boundaries, but the method degrades sharply when source and target models are not linearly connected in parameter space, making major-version (Llama 2→3) transfer largely untested. Catastrophic forgetting under continual fine-tuning is real and quantifiable: Luo et al. (2023) document 18–34 percent performance drops in domain knowledge across 1–7 B parameter decoder models, worsening monotonically with scale. Domain eval properties — verifiability, regulatory-schema specificity, low perplexity of in-domain vocabulary — are the strongest predictors of gains that survive base-model upgrades, as illustrated by the Accordance AI (+39 percentage points on tax reasoning) and Ambience Healthcare (+27 percent relative improvement on ICD-10 coding) RFT case studies. Marginal re-alignment costs under LoRA range from under $10 to roughly $3,000 per run for 7 B models, but these numbers exclude the eval overhead that dominates production cycle time.

---

## 1. Why Durability Is the Load-Bearing Question

The ROI calculus for domain RL fine-tuning contains a time dimension that most point-in-time comparisons elide. A fine-tuning investment that is amortized over 18 months of production use looks very different from one that is rendered obsolete the moment the model provider ships a new base. In enterprise software procurement, this is familiar terrain — vendors routinely compete on "total cost of ownership" that folds in upgrade costs — but in foundation-model deployment, upgrade cadences are far shorter and the technical complexity of re-alignment is non-trivial.

The question of durability is therefore the load-bearing structural element for hypotheses about whether fine-tuning remains ROI-positive across realistic deployment horizons. If gains consistently erode to near-zero within one base-model generation, the correct strategy is to defer fine-tuning until each new base stabilizes and to budget for full re-training at each cycle. If gains are partially transferable via cheap diff-vector or adapter-rebasing tricks, the correct strategy shifts toward a hub-and-spoke model where the alignment investment is made once and recycled cheaply. If gains are substantially durable across base-model generations — meaning the domain fine-tune still leads the vanilla successor model — then long-horizon ROI calculations become considerably more favorable.

The three scenarios have materially different investment theses. This chapter examines what the existing literature actually says about which scenario predominates, and for which domains.

Three structural reasons explain why durability may be domain-contingent rather than universal:

1. **Vocabulary specificity.** Domains with large proprietary lexicons (ICD-10 codes, legal citations, tax code cross-references) give fine-tuning more to work with, and general base-model pre-training data is unlikely to substantially close that vocabulary gap in a single generation upgrade.
2. **Verifiability of reward signals.** Domains where correctness is formally verifiable (code that compiles, math proofs, tax returns that balance, ICD-10 codes that match gold-panel consensus) allow reinforcement fine-tuning with rule-based graders (OpenAI 2024), which tend to produce more robust, transferable behavioral changes than imitation-learning fine-tuning.
3. **Regulatory precision.** Some domains have explicit correctness criteria defined by statute or professional standards; the fine-tuned model learns to navigate those criteria, not merely to mimic surface style, making the behavioral shift harder for a generic base-model upgrade to erase.

---

## 2. The Longitudinal Evidence Base — What We Have and What We Don't

### 2.1 The Gap in the Literature

The most direct evidence bearing on this chapter's central question would be studies that hold the domain evaluation task fixed across time and measure the performance of (a) a fine-tuned model on base G_n and (b) the vanilla base G_n+1 and (c) a fine-tuned model on base G_n+1. To the author's knowledge, no peer-reviewed study as of April 2026 has published all three arms of this comparison for the same domain task. The closest approximation comes from:

- Industry benchmark leaderboards (e.g., Hugging Face's Open Medical-LLM Leaderboard) that track how newer base models perform on the same medical QA benchmarks previously used to evaluate specialized models, but without controlling for fine-tuning status.
- A small number of practitioner reports, most of which are self-published and lack methodological transparency.

This is an important gap, and the adversary phase should flag it prominently.

### 2.2 Indirect Evidence Across Domains

**Mathematics and Reasoning.** The trajectory is clearest here: successive base models have absorbed large fractions of fine-tune leads. GPT-3 scored approximately 35 percent on GSM8K (grade school math) at launch in 2021. By 2024–25, GPT-4o, Claude 3.5 Sonnet, and Gemini 1.5 all exceeded 90 percent on the same benchmark without task-specific fine-tuning (LXT 2026; LLM-Stats 2026). This means that a 2021-era fine-tuned math model with, say, 60 percent GSM8K accuracy would have been overtaken by the vanilla 2024 base. However, this observation is confounded: the new base models are not just newer — they are also larger, trained on more data, and likely saw GSM8K-adjacent content in pre-training. The observation cannot be cleanly attributed to "the base model improved past the fine-tune" without isolating for parameter count and data volume.

**Biomedical NLP.** Benchmark evidence from Nature Communications (2025) shows that state-of-the-art supervised fine-tuning approaches still outperform zero-shot GPT-4-class models on most biomedical NLP tasks (0.6536 average vs. 0.5131 for zero-shot), with the advantage most pronounced in information-extraction tasks and least pronounced in knowledge-heavy QA. This suggests fine-tune leads are not universally erased by new bases, at least in structured extraction tasks (Bitterman et al. 2025). However, the comparison does not hold generation constant.

**Code generation.** Meta's Llama 3.2 (3B) has been shown to match or exceed larger fine-tuned models including DBRX Instruct (132B) on several code benchmarks, illustrating how next-generation base models at smaller parameter counts can overtake older fine-tuned models at larger counts (IntuitionLabs 2024). The signal is noisy because model size and fine-tuning status are confounded.

**Legal and Medical Domains.** The ICD-10 fine-tuning result from Enhancing Medical Coding (npj Health Systems 2025) is instructive: specialized fine-tuning lifted exact-match accuracy from below 1 percent to 97 percent on ICD-10 code assignment. This magnitude of lift — from near-chance to near-ceiling — is unlikely to be fully erased by a single base-model generation advance, because the task requires look-up of a domain-specific coding taxonomy that is not well-represented in general pre-training corpora. The persistence of that lead, however, has not been formally tested across Llama generations or GPT model versions.

### 2.3 The Orthogonality Finding

A particularly relevant mechanistic result comes from Gu et al. (2025), who analyzed tuning vectors (directional parameter shifts from fine-tuning) across three model families (Llama-3, Qwen2.5, Phi-3.5) and found that medical fine-tuning vectors are "completely orthogonal" to math/code fine-tuning vectors in parameter space (Gu et al. 2025). The implication is that domain-specialized knowledge is genuinely represented in a distinct subspace of the model's parameters, which cuts both ways for durability: the domain-specific weight directions may survive a base-model upgrade that makes changes in different parameter subspaces, but may also be disrupted if the new base model reorganizes the embedding structure.

**Summary.** The longitudinal evidence base is thin. No controlled three-arm study (tuned-G_n vs. vanilla-G_n+1 vs. tuned-G_n+1) exists in the published literature as of April 2026. Available indirect evidence suggests that gain durability is domain- and task-type-dependent, with structured extraction tasks showing more robust persistence than open-domain reasoning tasks where general base scaling narrows the gap rapidly.

---

## 3. Mechanisms of Partial Transfer: Diff Vectors, LoRA Rebasing, and Replay

### 3.1 Diff-Vector Transfer (Lin et al. 2025)

The most technically rigorous treatment of partial transfer is Lin et al. (2025), "Efficient Model Development through Fine-tuning Transfer," published at EMNLP 2025 (DOI: 10.18653/v1/2025.emnlp-main.131). The core insight is straightforward: define the *diff vector* as $\Delta_s = m'_s - m_s$, the elementwise difference between a fine-tuned source model and its base, and then apply it to a different target base: $m_t + \Delta_s$. This is equivalent to the task-vector formulation of Ilharco et al. (2023), extended to the cross-version setting (Lin et al. 2025).

**Main results (Llama 3.0→3.1 8B, minor-version gap):**

| Benchmark | Base Llama 3.1 8B | Llama 3.1 + Δ3.0 (transferred) | Llama 3.1 8B Instruct |
|---|---|---|---|
| IFEval | 36.4% | 83.3% | ~84% |
| GSM8K | 55.6% | 79.8–82.8% | ~84% |
| MATH | — | 29.9–44.7% | — |
| GPQA | 21.9% | 32.6% | 31.3% |

The transferred model *surpasses* Llama 3.1 8B Instruct on GPQA despite involving no gradient updates. This is a striking result: the diff vector from the 3.0-generation fine-tuning is sufficient to exceed the full alignment pipeline applied to the 3.1 base, at least on these benchmarks.

**Iterative recycling-then-fine-tuning.** When the transferred model is used as an initialization for further fine-tuning (rather than deployed as-is), the gains compound: on GSM8K, iterative recycling produces 67.0–77.5 percent accuracy versus 60.4–75.7 percent for vanilla fine-tuning from scratch, with faster convergence (Lin et al. 2025, Table 6). This finding directly bears on the re-training cost question: practitioners can treat the transferred diff vector as a warm start, potentially reducing compute by a meaningful fraction.

**Critical constraint: Linear connectivity.** The method's effectiveness degrades when source and target models are not linearly connected in parameter space. Lin et al. (2025) tested this using OLMo 2 7B checkpoints at five training stages ($\mathcal{M}_1$ through $\mathcal{M}_5$). Models within a training phase (e.g., both in stage 2) are linearly connected and show good transfer (gap to direct fine-tuning narrows to 2.8 pp at $\mathcal{M}_5$). Models from different phases show large gaps (26–41 pp). This strongly implies that transfer across major architectural changes — Llama 2 to Llama 3, with its different tokenizer, embedding size, and training recipe — is unlikely to succeed with simple diff-vector application. Lin et al. (2025) do not test Llama 2→3 transfer, and that gap in the paper is a consequential one.

**Architectures tested.** Llama 3.0 and 3.1 (8B), OLMo 2 (7B) with Tulu instruction data, and multilingual variants. There is no evidence in the paper of cross-architecture major-version transfer.

### 3.2 Task Arithmetic and Model Merging

The diff-vector approach builds on the task arithmetic framework of Ilharco et al. (2023), who showed that task vectors (fine-tuned weight minus pretrained weight) can be added, subtracted, and combined arithmetically. Wortsman et al. (2022) demonstrated that weight interpolation between pretrained and fine-tuned models (WiSE-FT) can recover zero-shot generalization while retaining fine-tune gains. The model merging literature (surveyed in Yadav et al. 2024, arXiv 2603.09938) has extended these ideas to TIES-merging, DARE, and other sparsity-based approaches.

For the durability question, the key finding from this literature is that instruction fine-tuning and continued pre-training appear to "operate on approximately orthogonal subspaces, enabling linear combination without catastrophic interference" (Yadav et al. 2024). This theoretical result supports the hypothesis that domain fine-tune weight deltas can survive a base-model upgrade, but the empirical support is limited to minor-version scenarios.

### 3.3 LoraHub and Adapter Composition

LoraHub (Huang et al. 2023 [preprint]; published COLM 2024, arXiv 2307.13269) proposes assembling LoRA adapters from a repository of task-specific modules using gradient-free composition with a few demonstration examples. The method achieves performance approaching in-context learning on BIG-Bench Hard without requiring any gradient updates, by learning a convex combination of adapters trained on preceding tasks.

The practical limitation for the durability scenario is that LoraHub operates within a fixed base model; the adapters must all have been trained on the same base. Transferring LoRA adapters across different base architectures faces the additional complication that the adapter matrices $A$ and $B$ are calibrated to the specific weight geometry of their training base. LoRA-X (arXiv 2504.xxxx) attempts training-free cross-model LoRA transfer but results remain preliminary as of April 2026.

### 3.4 Cross-Architecture Proxy Tuning

A complementary approach that sidesteps parameter-space transfer altogether is Cross-Architecture Proxy Tuning (CAPT), proposed for clinical NLP (arXiv 2601.03423). Rather than transferring weights, CAPT ensembles probability distributions from a legacy specialized model with those from a new general-domain base at inference time, using contrastive decoding to inject clinical knowledge. Results on six clinical classification and generation tasks show +17.6 percent over UniTE and +41.4 percent over standard proxy tuning. CAPT does not require any gradient updates and handles vocabulary mismatches, making it applicable across major-version gaps where diff-vector transfer fails (arXiv 2601.03423).

---

## 4. Catastrophic Forgetting as the Lower Bound

### 4.1 Empirical Magnitudes

The most systematic empirical study of catastrophic forgetting in LLMs during continual fine-tuning is Luo et al. (2023), "An Empirical Study of Catastrophic Forgetting in Large Language Models During Continual Fine-tuning" (arXiv 2308.08747; published IEEE/ACM Transactions on Audio, Speech, and Language Processing 2025). The study evaluates forgetting across three capability dimensions — domain knowledge, reasoning, and reading comprehension — for BLOOMZ (decoder-only, 1.1B–7.1B) and mT0 (encoder-decoder, 1.2B–3.7B).

**Key quantitative findings:**

| Model | Scale | Domain Knowledge FG | Reasoning FG | Reading Comp. FG |
|---|---|---|---|---|
| BLOOMZ | 1.1B | 9.54% | 6.73% | 18.04% |
| BLOOMZ | 3B | 14.63% | 11.09% | 27.56% |
| BLOOMZ | 7.1B | 18.37% | 13.62% | 26.75% |
| mT0 | 3.7B | 20.15% | 16.73% | 28.42% |
| LLaMA | 7B | 34.57% | 31.33% | 31.72% |
| Alpaca | 7B | 18.14% | 7.56% | 10.31% |

*FG = forgetting (percentage-point drop from pre-fine-tuning baseline). (Luo et al. 2023)*

The monotonic worsening with scale within the 1–7B range is counterintuitive and consequential: it suggests that practitioners using larger models face proportionally greater forgetting cost per fine-tuning cycle. BLOOMZ exhibits substantially less forgetting than LLaMA at comparable scale (18.4 vs. 34.6 percent on domain knowledge), attributed by the authors to BLOOMZ's pre-training on a broader multilingual corpus with instruction-following signals already present. The Alpaca model (instruction-tuned LLaMA) shows markedly less forgetting than raw LLaMA, suggesting that the quality of prior instruction-tuning is a significant moderator.

**Reading comprehension is the most sensitive dimension**, with forgetting values in the 18–31 percent range across all model classes tested.

### 4.2 Mechanistic Analysis

A follow-on paper, "Mechanistic Analysis of Catastrophic Forgetting in Large Language Models During Continual Fine-Tuning" (arXiv 2601.18699, 2025), examines which components of the network are most affected. The findings converge with those of Gu et al. (2025): fine-tuning primarily updates MLP layers (writing new directional information) while attention heads amplify existing directions. Catastrophic forgetting predominantly manifests as overwriting of MLP-layer representations. This has a practical implication: parameter-efficient methods like LoRA that concentrate updates in low-rank decompositions of weight matrices may indirectly reduce forgetting by leaving most MLP weights unchanged.

### 4.3 Mitigation Techniques and Their Effectiveness

**LoRA / PEFT alone.** Applying LoRA does not, by itself, prevent catastrophic forgetting; it only changes the parameter subspace affected (Revisiting Weight Regularization, arXiv 2602.17559). However, the reduced parameter footprint may make recovery cheaper.

**Elastic Weight Consolidation (EWC) + LoRA.** EWC-LoRA (arXiv 2505.05946; arXiv 2602.17559) updates the model via low-rank adaptation while using the full-dimensional Fisher Information Matrix for weight regularization. This achieves a stability-plasticity trade-off "superior to existing low-rank CL approaches" (arXiv 2602.17559). Tested on arc, belebele, GSM8K, hellaswag, MMLU, TruthfulQA, and Winogrande, EWC-LoRA "mitigated catastrophic forgetting effects on multiple language understanding benchmarks." Specific percentage improvements are not universally reported across benchmarks, but the qualitative finding is consistent.

**Data mixing / replay.** Adding 10,000 general instruction samples during domain fine-tuning improved LLaMA's MMLU-human score from 26.8 percent (domain-only) to 30 percent (mixed), a 3.2 pp partial recovery (Luo et al. 2023). More aggressive replay strategies (experience replay, general samples replay) further reduce task interference, with the GeRe approach (arXiv 2508.04676) demonstrating "feasibility of utilizing only predetermined general replay samples to resist task-specific forgetting" without requiring stored task-specific examples.

**MoFO (Momentum-Filtered Optimizer).** MoFO (arXiv 2407.20999) constrains gradient updates to parameters with small momentum, effectively freezing high-momentum parameters that represent general capabilities. This is an optimization-level intervention that does not require explicit replay data.

**Practical implication for re-training costs.** The combined use of EWC-LoRA or data-mixing replay reduces the re-training cost per generation in two ways: first, by reducing the severity of forgetting, the model needs fewer examples to re-establish domain performance; second, by using diff-vector warm-starting (Lin et al. 2025), the gradient-based component of re-alignment converges faster. The two techniques are not yet tested together in the published literature, representing an open empirical gap.

---

## 5. Domain-Eval Properties That Predict Durability

Synthesizing the case-study evidence alongside the mechanistic findings, four evaluation properties emerge as strong predictors of gains that survive base-model upgrades.

### 5.1 Formal Verifiability

Tasks where correctness is binary and rule-based — mathematical proofs, code that compiles and passes tests, tax returns that satisfy statutory constraints, ICD-10 codes that match expert consensus — admit reinforcement fine-tuning with rule-based graders (OpenAI 2024). The behavioral shifts produced by RLVR (reinforcement learning with verifiable rewards) are more robust because the model is optimizing a signal that is consistent across base-model generations: the underlying compliance rule does not change when the base model is upgraded. In contrast, fine-tuning via behavior cloning of a teacher model can produce gains that are fragile because they encode the teacher's output distribution rather than the underlying task structure (Crossing the Reward Bridge, arXiv 2503.23829).

The Accordance AI case (+39 pp on tax reasoning accuracy) and the Ambience Healthcare case (+27 percent relative improvement on ICD-10 coding) both use rule-based graders: Accordance used a compliance-logic grader; Ambience used a gold-panel consensus of four or more expert clinicians. The verifiable structure of these reward signals is likely a significant contributor to the magnitude and expected durability of these gains.

### 5.2 Domain-Specific Vocabulary Depth

The larger the proprietary lexicon that is inadequately represented in general pre-training data, the larger the fraction of the fine-tune gain that originates from vocabulary-level adaptation rather than reasoning adaptation. Vocabulary-level gains are more durable because they are not competing with a capability that general scaling can provide; a new Llama or GPT version trained on web text is unlikely to have substantially better coverage of IRS Revenue Rulings cross-references or ICD-10 code tree structures than its predecessor.

The medical coding evidence is illustrative: fine-tuning lifted exact-match ICD-10 accuracy from below 1 percent to 97 percent on specialized medical datasets (npj Health Systems 2025). A base-model upgrade would need to close a near-total-performance gap to erase this lead, which is implausible in a single generation.

### 5.3 Regulatory and Schema Precision

Legal, tax, and medical coding domains have explicit correctness criteria defined by external bodies (courts, IRS, CMS). A fine-tuned model that internalizes these criteria is generating outputs that are externally validated, not merely human-preferred. This reduces the risk that a new base model's distributional improvements would invalidate the learned behavior.

Harvey AI's use of custom case-law training (adding the equivalent of 10 billion tokens of case law data) creates a domain knowledge gap relative to general base models that is unlikely to be closed by general pre-training advances. Harvey's early RFT trials showed the fine-tuned model winning or tying in 93 percent of pairwise comparisons against GPT-4o on legal citation extraction, with a 20 percent F1 improvement and matched GPT-4o performance at lower latency (OpenAI/Harvey 2024). While the cross-generation durability of this lead has not been formally tested, the citation-extraction task is a structured information-extraction problem where vocabulary specificity and verifiability are high.

### 5.4 RAG-Augmented vs. Parametric Knowledge

A nuanced finding from the literature is that RAG-augmented fine-tuning — where the fine-tune teaches the model to correctly use retrieval, not to memorize facts — may produce more durable gains than purely parametric knowledge fine-tuning. The reasoning is that retrieval-augmented tasks decouple factual currency from model weights; when the base model is upgraded, the retrieval chain remains intact. There is limited direct evidence for this claim as of April 2026, but it is consistent with the mechanistic finding that fine-tuning primarily writes directional information into MLP layers (Gu et al. 2025): for RAG tasks, the "direction" being learned is how to synthesize retrieved context, which is a more structural adaptation than memorizing domain facts.

---

## 6. The "Harvey Case" and Other Persistent-Gain Instances Dissected

### 6.1 Harvey AI (Legal Domain)

Harvey, an AI platform for legal and professional services, fine-tuned OpenAI models on case law using both custom pre-training (10 billion equivalent tokens of case law) and reinforcement fine-tuning for specific legal tasks. For citation extraction from legal documents, early RFT trials reported the fine-tuned model winning or tying against GPT-4o in 93 percent of pairwise comparisons, with a 20 percent F1 improvement and matching GPT-4o accuracy at reduced latency (OpenAI 2024).

The eval properties that suggest durability are high: citation extraction is a structured extraction task (high verifiability), legal citations follow a highly domain-specific schema (Bluebook citation format, case reporter conventions) that is poorly covered in general web pre-training, and the task is amenable to rule-based grading (cite either matches a ground-truth citation or it does not).

**Caveat.** Harvey has not published cross-generation comparisons. The claim that gains "persist" is an inference from domain properties, not direct longitudinal measurement. The adversary phase should flag this.

### 6.2 Accordance AI (Tax Reasoning)

Accordance AI, a tax and accounting AI platform backed by Khosla Ventures and General Catalyst ($13M seed, September 2025), built a custom tax analysis model using OpenAI's RFT with a rule-based grader encoding compliance logic. The model improved accuracy by 39 percentage points over baseline on tax reasoning tasks (OpenAI 2024).

A 39 pp gain is large enough that even substantial base-model improvement would be unlikely to fully erase the advantage in a single generation. Tax reasoning involves applying specific provisions of the Internal Revenue Code, Treasury Regulations, and Revenue Rulings — a domain-specific regulatory structure that changes through legislation, not through general pre-training scale. The grader is rule-based (compliance logic), maximizing the verifiability property. These characteristics collectively suggest a gain that would require targeted tax-domain fine-tuning to achieve with a new base model, not merely base-model scaling.

**Caveat.** The 39 pp figure comes from OpenAI's published use-case documentation (OpenAI 2024) and has not been independently replicated. The baseline (what the untuned model scored on the same task) is not specified precisely.

### 6.3 Ambience Healthcare (Medical Coding)

Ambience Healthcare used RFT to train a model for ICD-10 medical coding, benchmarked against 18 board-certified physicians. The gold-panel test dataset used consensus labels from four or more expert clinicians. The resulting model surpassed physician performance by 27 percent in coding accuracy (Ambience Healthcare 2025).

The 27 percent *relative* improvement over physicians is notable because it sets a ceiling: no future base-model upgrade can itself achieve better-than-physician performance on this task without the same domain-specific fine-tuning. The task's eval properties are near-ideal for durability: ICD-10 codes are externally validated (CMS), the vocabulary is highly domain-specific, and the grader (gold-panel consensus) is rule-applicable.

**Caveat.** "27 percent relative improvement over physician baseline" is distinct from a 27 percentage-point absolute improvement; the absolute underlying accuracy figures are not publicly disclosed.

### 6.4 Comparative Analysis of Persistent vs. Fragile Gains

Contrasting these persistent-gain cases with the mathematical reasoning trajectory (where new base models have largely closed fine-tune leads) yields the following pattern:

| Property | Math/Reasoning | Legal (Harvey) | Tax (Accordance) | Medical Coding (Ambience) |
|---|---|---|---|---|
| Formal verifiability | High | High | High | High |
| Domain vocabulary specificity | Low | High | High | High |
| Regulatory schema precision | None | High | High | High |
| Gain magnitude | Moderate | Large (20% F1) | Large (+39 pp) | Large (+27% rel.) |
| Base-model erosion observed? | Yes (GSM8K) | Not tested | Not tested | Not tested |

The distinguishing variable between the eroding-gain (math) case and the persistent-gain cases is domain vocabulary specificity and regulatory schema precision. Math reasoning is a general capability that general pre-training at scale can improve; ICD-10 coding and tax compliance logic are domain-specific skills that require targeted exposure.

---

## 7. The Recurring Re-Training Cost: An Estimated Lower Bound

### 7.1 Parameter-Efficient Fine-Tuning Baselines

Current cloud GPU pricing (2025) positions LoRA fine-tuning at:
- Sub-$10 per run for 2–8B models on A100s or RTX 4090s
- $300–$700 for Phi-2 (2.7B) class models
- $1,000–$3,000 for 7B models (Mistral, LLaMA 7B class) under LoRA
- Up to $12,000 for full fine-tuning of 7B models
- LoRA achieves approximately 90–95 percent of full fine-tuning quality at approximately 10 percent of cost (Exxact/Scopic 2025)

The marginal cost of re-aligning a LoRA adapter to a new base model — assuming vanilla re-training from random LoRA initialization on the same domain dataset — is approximately the same as the original training cost, because the LoRA matrices cannot be directly reused when the base model's weight geometry changes.

### 7.2 Cost Reduction via Diff-Vector Warm Start

Lin et al. (2025) demonstrate that initializing re-training with a diff-vector-transferred model reduces both the number of training steps needed and the final loss, with the improvement being more pronounced for models further from the initialization ($\mathcal{M}_1$ through $\mathcal{M}_3$ checkpoints show the largest benefits). On GSM8K, the iterative recycling approach achieves 67.0–77.5 percent versus 60.4–75.7 percent for vanilla fine-tuning, consistently outperforming. The compute reduction is not precisely quantified in terms of FLOPs or GPU-hours, but the convergence plots in Figure 3 of the paper suggest roughly 20–40 percent fewer steps to a given loss target.

### 7.3 Adapter Composition via LoraHub

LoraHub (Huang et al. 2023 / COLM 2024) enables composition of existing adapters for new tasks without gradient updates, at "similar inference throughput as zero-shot learning" and performance approaching in-context learning on BIG-Bench Hard. However, LoraHub is restricted to the fixed base model on which the constituent adapters were trained. Its utility for the cross-generation rebasing problem is therefore limited to cases where partial tasks can be addressed by existing adapters on the new base.

### 7.4 The Eval Overhead Problem

A critical cost that literature-based estimates systematically understate is the human evaluation overhead required to validate that a re-aligned model is production-ready. For regulated domains (legal, medical, financial), this involves expert review of outputs, which can cost orders of magnitude more than the GPU compute cost. A Harvey-class legal model requires law firm partner review of citation extractions; an Ambience-class medical model requires physician review of coding outputs. These eval costs are not captured in the LoRA training-cost estimates above and are the practical bottleneck in production re-alignment cycles.

The diff-vector technique offers a partial remedy here: if the transferred model is deployed as a zero-shot initialization before any gradient updates, a practitioner can immediately test whether the transfer was successful and quantify how much the new base has degraded domain performance, thereby narrowing the scope of eval required. This is a practical workflow benefit even independent of compute cost.

### 7.5 Estimated Lower Bound per Generation

For a 7B LoRA domain fine-tune (typical production configuration as of 2025):

| Component | Cost Range |
|---|---|
| GPU compute (LoRA, 7B, cloud) | $1,000–$3,000 |
| Dataset curation / refresh (assuming 10–20% new data per generation) | $2,000–$10,000 |
| Internal engineering oversight | $5,000–$20,000 (2–4 FTE days) |
| Domain expert evaluation | $10,000–$50,000+ (domain-dependent) |
| **Total per generation** | **$18,000–$83,000+** |

The GPU compute is the smallest component. The eval overhead — especially for regulated domains — dominates. Diff-vector warm-starting can reduce the GPU compute fraction, but the eval overhead is largely fixed by regulatory requirements.

---

## 8. What This Implies for Hypotheses B and G

**Hypothesis B** (abbreviated from the broader research project): *Domain RL fine-tuning produces gains that are substantially durable across base-model upgrades, sustaining positive ROI over multi-year deployment horizons.*

The evidence is conditionally supportive. Durability is high for domains with high verifiability, regulatory schema specificity, and domain vocabulary depth (legal, medical coding, tax). Durability is low to moderate for domains where general scaling can close the fine-tune gap (open-domain math reasoning, dialogue instruction-following). The conditional nature of the finding means that Hypothesis B should be framed as domain-contingent, not universal.

**Hypothesis G** (abbreviated): *Diff-vector recycling and adapter rebasing techniques substantially reduce the marginal cost of re-aligning a domain fine-tune to a new base model, making the recurring re-training cost a secondary rather than primary ROI concern.*

The evidence is supportive but with significant caveats. Diff-vector transfer works reliably across minor-version gaps (Llama 3.0→3.1) where models are linearly connected. It has not been demonstrated across major-version gaps (Llama 2→3) or across architectures with different embedding dimensions or tokenizers. The compute savings from warm-starting are real but modest (20–40 percent fewer steps), while the eval overhead — the dominant cost component — is not addressed by technical transfer methods. Hypothesis G is too strong as stated; it should be qualified to: "diff-vector recycling reduces GPU compute overhead but not eval overhead, and is effective only within linearly connected model families."

---

## 9. Open Empirical Questions

The following gaps should be flagged for the adversary phase and for future data collection:

1. **The missing three-arm longitudinal study.** No published work as of April 2026 holds a domain task fixed and measures: (a) tuned G_n, (b) vanilla G_n+1, (c) tuned G_n+1 across at least two consecutive major base-model generations. This is the single most important gap in the literature for this stream's central question.

2. **Diff-vector transfer across major-version gaps.** Lin et al. (2025) test Llama 3.0→3.1 (a minor version with shared tokenizer and similar architecture). Transfer across Llama 2→3 (different tokenizer, substantially different architecture and training recipe) has not been tested. The linear-connectivity requirement suggests it will fail, but empirical confirmation is absent.

3. **Interaction between diff-vector warm-starting and EWC-LoRA.** These two methods address different cost components (compute warm-start vs. forgetting during re-training) and are likely complementary. No published study tests them together.

4. **Durability of RFT gains specifically (vs. SFT gains).** The Harvey, Accordance, and Ambience cases all use RFT with verifiable graders. It is theoretically expected that RFT gains are more durable because they encode structural task understanding rather than distributional imitation, but this has not been tested longitudinally. The adversary phase should challenge whether the cited gains are durable or merely large at the point of initial measurement.

5. **Eval overhead quantification.** The literature provides no peer-reviewed estimates of how expert evaluation costs scale with model size, domain, and regulatory requirements per re-alignment cycle. This is a critical gap for accurate ROI modeling.

6. **Scale dependence beyond 7B.** Luo et al. (2023) find that forgetting worsens with scale within the 1–7B range. Whether this monotonic relationship holds at 70B or 400B is unknown; it may reverse due to greater representational redundancy at larger scales.

7. **Vocabulary-specificity as a quantitative predictor.** The claim that domain vocabulary specificity predicts durability is theoretically motivated and supported by the case-study pattern, but no study has formally measured vocabulary specificity (e.g., fraction of domain tokens absent from general pre-training corpus) and regressed it against gain durability across model generations. This is an operationalizable research question.

---

## References (Chicago Author-Date)

Ambience Healthcare. 2025. "Ambience Healthcare's AI Platform Surpasses Clinician Performance by 27% in Medical Coding, Powered by New OpenAI Breakthrough." Press release, May 27, 2025. https://www.ambiencehealthcare.com/blog/ambience-healthcare-s-ai-platform-surpasses-clinician-performance-by-27-in-medical-coding-powered-by-new-openai-breakthrough

Bitterman, Danielle S., Tafadzwa L. Chaunzwa, Ana Lyudmila Unkovskiy, and Hugo J.W.L. Aerts. 2025. "Benchmarking Large Language Models for Biomedical Natural Language Processing Applications and Recommendations." *Nature Communications* 16: article 2003. https://doi.org/10.1038/s41467-025-56989-2

Exxact Corporation. 2025. "How LoRA Makes AI Fine-Tuning Faster, Cheaper, and More Practical." Blog post. https://www.exxactcorp.com/blog/deep-learning/ai-fine-tuning-with-lora

Gu, Yushi, Jiajun Deng, and colleagues. 2025. "Understanding the Effects of Domain Finetuning on LLMs." arXiv preprint arXiv:2510.09359 [preprint]. https://arxiv.org/abs/2510.09359

Huang, Chengsong, Qian Liu, Bill Yuchen Lin, Tianyu Pang, Chao Du, and Min Lin. 2024. "LoraHub: Efficient Cross-Task Generalization via Dynamic LoRA Composition." In *Proceedings of the Conference on Language Modeling (COLM) 2024*. arXiv:2307.13269. https://arxiv.org/abs/2307.13269

Ilharco, Gabriel, Marco Tulio Ribeiro, Mitchell Wortsman, Suchin Gururangan, Ludwig Schmidt, Hannaneh Hajishirzi, and Ali Farhadi. 2023. "Editing Models with Task Arithmetic." In *Proceedings of the Eleventh International Conference on Learning Representations (ICLR 2023)*. arXiv:2212.04089. https://arxiv.org/abs/2212.04089

Li, Xiaonan, and colleagues. 2025. "Revisiting Weight Regularization for Low-Rank Continual Learning." arXiv preprint arXiv:2602.17559 [preprint]. https://arxiv.org/abs/2602.17559

Lin, Pin-Jie, Rishab Balasubramanian, Fengyuan Liu, Nikhil Kandpal, and Tu Vu. 2025. "Efficient Model Development through Fine-tuning Transfer." In *Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing*, 2617–2636. Suzhou, China: ACL Anthology. https://doi.org/10.18653/v1/2025.emnlp-main.131

LLM-Stats. 2026. "LLM Benchmarks 2026 — Compare AI Benchmarks and Tests." https://llm-stats.com/benchmarks

Luo, Yun, Zhen Yang, Fandong Meng, Yafu Li, Jie Zhou, and Yue Zhang. 2023. "An Empirical Study of Catastrophic Forgetting in Large Language Models During Continual Fine-tuning." arXiv preprint arXiv:2308.08747 [preprint]; published in *IEEE/ACM Transactions on Audio, Speech, and Language Processing* 33 (2025): 3776–3786. https://arxiv.org/abs/2308.08747

Mechanistic Analysis Team. 2025. "Mechanistic Analysis of Catastrophic Forgetting in Large Language Models During Continual Fine-Tuning." arXiv preprint arXiv:2601.18699 [preprint]. https://arxiv.org/abs/2601.18699

Muennighoff, Niklas, Alexander M. Rush, Boaz Barak, Teven Le Scao, Aleksandra Piktus, Nouamane Tazi, Sampo Pyysalo, Thomas Wolf, and Colin Raffel. 2023. "Scaling Data-Constrained Language Models." In *Advances in Neural Information Processing Systems* 36 (NeurIPS 2023). arXiv:2305.16264. https://arxiv.org/abs/2305.16264

OpenAI. 2024. "Customizing Models for Legal Professionals." Case study page. https://openai.com/index/harvey/

OpenAI. 2024. "Reinforcement Fine-Tuning Use Cases." API documentation. https://platform.openai.com/docs/guides/rft-use-cases

Scopic Software. 2025. "The Real Cost of Fine-Tuning LLMs: What You Need to Know." Blog post. https://scopicsoftware.com/blog/cost-of-fine-tuning-llms/

Shi, Haiyan, and colleagues. 2025. "Elastic Weight Consolidation for Full-Parameter Continual Pre-Training of Gemma2." arXiv preprint arXiv:2505.05946 [preprint]. https://arxiv.org/abs/2505.05946

Singh, Aryan, and colleagues. 2025. "Enhancing Medical Coding Efficiency through Domain-Specific Fine-Tuned Large Language Models." *npj Health Systems* 2: article 18. https://doi.org/10.1038/s44401-025-00018-3

Sun, Tianxiang, and colleagues. 2024. "MoFO: Momentum-Filtered Optimizer for Mitigating Forgetting in LLM Fine-Tuning." arXiv preprint arXiv:2407.20999 [preprint]. https://arxiv.org/abs/2407.20999

Tian, Liyuan, and colleagues. 2026. "Training-Free Adaptation of New-Generation LLMs using Legacy Clinical Models." arXiv preprint arXiv:2601.03423 [preprint]. https://arxiv.org/abs/2601.03423

Yadav, Prateek, Derek Tam, Leshem Choshen, Colin Raffel, and Mohit Bansal. 2024. "Model Merging in the Era of Large Language Models: Methods, Applications, and Future Directions." arXiv preprint arXiv:2603.09938 [preprint]. https://arxiv.org/abs/2603.09938

Wortsman, Mitchell, Gabriel Ilharco, Samir Yitzhak Gadre, Rebecca Roelofs, Raphael Gontijo-Lopes, Ari S. Morcos, Hongseok Namkoong, Hong Nie, Ruston Carmon, Ross Wightman, Yair Carmon, and Ludwig Schmidt. 2022. "Model Soups: Averaging Weights of Multiple Fine-Tuned Models Improves Accuracy without Increasing Inference Time." In *Proceedings of the 39th International Conference on Machine Learning (ICML 2022)*, Proceedings of Machine Learning Research 162: 23965–23998.

Zhang, Longhui, and colleagues. 2025. "Crossing the Reward Bridge: Expanding RL with Verifiable Rewards Across Diverse Domains." arXiv preprint arXiv:2503.23829 [preprint]. https://arxiv.org/abs/2503.23829

---

*End of Chapter — Stream S1*
