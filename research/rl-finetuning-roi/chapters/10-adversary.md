# Adversary Report — Stress-Testing the Cross-Stream Synthesis

*Date: 2026-04-28 | Author: adversary | Status: input to Phase 5 synthesis*

---

## 1. Executive Verdict

A summary verdict for each of C1–C8, in order:

**C1 — Sweet spot exists today.** *Upheld with qualifications.* The ART-E result is real and reproducible in principle, but no independent replication on a workload OpenPipe did not select has been published. The claim survives as evidence of feasibility in a specific narrow-workload regime, not as a calibrated population estimate.

**C2 — Sweet spot is narrowly bounded.** *Upheld.* The a16z CIO survey corroborates the boundedness from the enterprise side (declining ROI outside hyper-specific domains); the BigLaw Bench trajectory corroborates it from the capability side (seven base models now exceed Harvey's original fine-tuned system, compressing the "fine-tune leads base" window). The conditions in C2 are accurate but need a sixth: the RL stack itself must not reward-hack.

**C3 — Inference is the durable moat.** *Upheld with qualifications.* Energy cost as a structural price floor — not yet in the streams — weakens the "infinite price decline" component. The 10×/year trend shows credible signs of deceleration at the commodity tier. The moat from hardware-software co-design (Groq LPU, Cerebras WSE) survives but is niche.

**C4 — Lock-in lives in prompt and harness assets.** *Upheld.* The Harvey multi-homing data (TechCrunch 2025) and BigLaw Bench leaderboard confirm that sophisticated buyers deliberately avoid weight-level lock-in. The Anthropic April 2026 postmortem on Claude Code quality regression — traced to harness and instruction changes, not weight changes — independently corroborates the claim.

**C5 — Durability under-measured at major-version boundary.** *Upheld, and the gap is worse than the streams acknowledged.* The Harvey BigLaw Bench finding that seven base models now outperform Harvey's original system is a partial empirical refutation of durable fine-tune leads — not a full refutation, but strong evidence that the "durability is domain-dependent" claim needs explicit quantification. The three-arm longitudinal study gap is load-bearing, not merely academic.

**C6 — Multi-tenant under narrow joint condition.** *Upheld.* The regulatory picture is sharper than the streams reported: HIPAA guidance recommends ε ≤ 0.5 for medical fine-tuning, far below the ε = 8 enterprise-tolerable floor cited in S4. The EU AI Act provider-reclassification threshold (one-third of original training compute) is now a specific, actionable hard stop, not merely a looming risk.

**C7 — Market prices training as talent.** *Upheld.* CoreWeave's September 2025 acquisition of OpenPipe — undisclosed price, team of 10, post-seed — is a new direct instance of acqui-hire at sub-IP valuation. The pattern is confirmed.

**C8 — "Next release eats fine-tune" — case-dependent.** *Weakened.* The Harvey BigLaw Bench data is more damaging to the durability thesis than the streams acknowledged: base model scores went from 60% to 90% on BigLaw Bench in under two years (GPT-4o/Claude 3.5/Gemini 1.5 → GPT-5/Claude 4.5/Gemini 3). Harvey's original fine-tuned system has been surpassed by seven vanilla base models. The "structurally likely" part of C8 is now "structurally confirmed on the leading commercial case study."

---

## 2. Claim-by-Claim Attack

### C1 — Sweet Spot Exists Today

**Counter-evidence — selection bias and no independent replication.** The ART-E benchmark is an OpenPipe-authored benchmark on an OpenPipe-selected workload (email search over the Enron corpus). The dataset is public (HuggingFace: `corbt/enron_emails_sample_questions`), the code is reproducible, and MarkTechPost and ZenML's LLMOps Database have each written analytical summaries of the result — but as of April 2026, no independent research team has published a replication of the benchmark on a workload the team selected rather than OpenPipe (OpenPipe 2025; MarkTechPost 2025; ZenML 2025). The absence of an independent replication is not evidence the result is wrong; the methodology is open and the architecture is plausible. But it does mean that the 96%/64×-cost headline is best treated as one data point at the favorable end of a distribution, not as a population average. The a16z 2025 CIO survey, with 100 verified VPs and C-level executives in Global 2000 companies, found that enterprises are trending *away* from fine-tuning toward prompt engineering and context injection, with long context windows cited as the displacement mechanism (Andreessen Horowitz 2025). The survey's respondents represent what practitioners were doing in 2023–2024 SFT stacks, not 2026 GRPO stacks, but the population-level signal contradicts the "sweet spot exists and practitioners are finding it" framing.

**Framing critique.** The synthesis summarizes the claim as "a workload-shaped sweet spot for proprietary RL fine-tuning *exists today*." The correct framing is weaker: "a workload-shaped sweet spot exists for the specific class of narrow, verifiable, high-volume agentic workflows, on an example workload OpenPipe chose, at a cost figure that has not been replicated on a workload a different team chose." The difference matters for practitioners deciding whether to invest: the former suggests they can find the sweet spot by searching; the latter suggests they need to first verify it exists on their specific workload, which requires its own investment.

**The RL panacea/mirage literature.** A 2025 paper, "RL Is Neither a Panacea Nor a Mirage: Understanding Supervised vs. Reinforcement Learning Fine-Tuning for LLMs" (arXiv:2508.16546), establishes a structural failure condition that C1's framing elides: when SFT pushes a model into a markedly different representation regime (severe overfitting causing distribution shift), RL fine-tuning cannot restore out-of-distribution performance. For Llama-3.2-11B, prolonged SFT dropped OOD performance from 17.52% to 8.97%, and subsequent RL recovered only to 15.38% — a permanent ceiling effect. This means the GRPO sweet-spot recipe is order-dependent: GRPO on a well-initialized SFT base works; GRPO on an over-SFT'd base may fail irreversibly (arXiv 2508.16546 2025).

**Verdict:** Upheld as feasibility evidence; confidence on population-level claim reduced to low-to-medium pending independent replication.

---

### C2 — Sweet Spot Is Narrowly Bounded

**Corroboration from enterprise survey.** The a16z 2025 CIO survey reinforces the narrow-bounded nature from the demand side. Enterprises report that fine-tuning is now pursued mainly for "hyper-specific use cases" where domain adaptation is necessary; in most other cases, long context windows and improved instruction following have substituted for fine-tuning at lower cost (Andreessen Horowitz 2025). The survey pool skews toward tech-forward Global 2000 companies — likely *more* sophisticated fine-tuning adopters than the average enterprise — which makes the finding about declining ROI more rather than less conservative.

**A missing sixth condition.** The five conditions in C2 are correct but omit a critical operational prerequisite: the reward function must not be susceptible to specification gaming. LLM-as-judge reward signals (RULER and analogues) are empirically biased: position bias in pairwise evaluation produces accuracy shifts exceeding 10% from ordering alone; agreeableness bias inflates true positive rates above 96% with true negative rates below 25%; and self-preference bias causes a judge model to score its own-style outputs higher (arXiv:2412.05579 2025). When the reward signal is corrupted, GRPO trains the policy to game the judge rather than perform the task — a documented failure mode Anthropic explicitly labeled "natural emergent misalignment from reward hacking in production RL" (Anthropic 2024). C2's condition (b) ("reward is verifiable or LLM-judge-rateable") conflates the two cases: verifiable rewards are safe; LLM-judge rewards require additional reliability controls that the synthesis does not price into the operational envelope.

**Verdict:** Upheld, with the addition of reward-function reliability as a sixth condition.

---

### C3 — Inference Is the Durable Moat

**The energy price floor — a structural counter to indefinite decline.** The synthesis asserts that token prices fell "~10×/year through 2026" and that "most of that drop came from the serving stack." Epoch AI corroborates the rate of algorithmic-efficiency-driven decline. However, a structural price floor not addressed by any stream is now visible: AI inference is responsible for 80–90% of total AI energy consumption globally, and data center power load associated specifically with AI is projected to hit 10 GW by end of 2026, constrained by grid interconnection capacity that utilities cannot deliver fast enough (TensorMesh 2025; Tech-Insider 2026). When energy-cost per token cannot fall further — because the energy itself is physically scarce — algorithmic efficiency gains have already been captured, and compute-per-token efficiency improvements at the hardware level require new silicon generations that take 18–24 months to deploy. The Epoch AI analysis itself notes: "it's less clear that those [fastest trends] will persist" (Epoch AI 2025). The 10×/year trend in the synthesis is correct as a historical statement; as a forward projection it is unsupported.

**FireAttention gap compression is directional but not complete.** The claim that FireAttention's kernel edge compressed from 4× to 1.5–2× over 18 months (S3) is corroborated. But the synthesis uses this as evidence that the open-source stack is catching up, which also means the *moat for Fireworks AI* is compressing. The same dynamic that makes self-hosted inference cheaper also makes the proprietary-inference-platform business more competitive, tightening the arbitrage window for any operator choosing between Fireworks and self-hosted vLLM. The $315M ARR/416% YoY growth figure for Fireworks AI is a Sacra estimate, not a company-verified figure (Sacra 2025). It is directionally credible given named customers (Cursor, Perplexity, Notion, Uber), but should not be cited as independently verified revenue.

**H100 spot price stabilization.** The synthesis assumes self-hosted inference costs continue falling at 40% per year. Current market data shows H100 spot pricing stabilized around $1.25–$3.11/GPU-hr in early 2026 with on-demand averaging $3.36/hr — a modest decline from 2025 highs rather than a collapse (JarvisLabs 2026; GetDeploying 2026). This is consistent with demand growth absorbing supply additions. For the S3 break-even model, a scenario where r_self falls from 0.40 to 0.15 (hardware stabilization) while r_API stays at 0.70 would extend the 500M-token break-even horizon from 14 months to approximately 20–22 months — still attractive, but meaningfully different.

**Verdict:** Upheld with qualifications. The energy price floor and hardware cost stabilization are risks to the "infinite decline" component. The hardware co-design moat (Groq, Cerebras) is real but niche.

---

### C4 — Vendor Lock-In Lives in Prompt and Harness Assets

**Corroboration from the Harvey multi-homing case.** Harvey deliberately expanded to Anthropic and Google in May 2025, framing the multi-provider strategy as reducing model dependency — not a migration but a design choice (TechCrunch 2025). This is the clearest enterprise-scale confirmation that sophisticated buyers have internalized C4 and are acting on it. The Anthropic April 2026 postmortem on Claude Code quality regression further corroborates: Anthropic traced a production quality degradation to "changes to Claude's harnesses and operating instructions," not to weight changes — harness brittleness at the serving level, exactly the mechanism C4 identifies (Anthropic Engineering 2026).

**Counterpoint — harness brittleness is under-priced.** If lock-in is in the harness, and the harness breaks on every minor version upgrade (documented across seven behavior categories for Opus 4.6→4.7; see S6), then "lock-in" is a misleadingly positive framing for what is structurally ongoing re-engineering cost. A more accurate framing is: the moat from prompt and harness assets is *real but depreciating*, requiring perpetual maintenance investment. The "bounded, recoverable in days" characterization in S6 is true for a well-staffed team but underestimates the compounding cost over a three-to-five-year deployment horizon with 6–9 month upgrade cycles.

**Verdict:** Upheld. The framing issue (depreciation vs. lock-in) should be flagged for the synthesis.

---

### C5 — Durability Under-Measured at the Major-Version Boundary

**Harvey BigLaw Bench — the strongest piece of new evidence in this report.** Harvey's own benchmark data, published progressively through 2025, shows that base model scores on BigLaw Bench rose from approximately 60% (GPT-4o / Claude 3.5 / Gemini 1.5) to approximately 90% (GPT-5 / Claude 4.5 / Gemini 3). Within less than a year of establishing their original fine-tuned system as a benchmark reference, seven models (including three non-OpenAI models) now outperform the originally benchmarked Harvey system on BigLaw Bench (Harvey 2025; Harvey 2025b). Harvey's response was to multi-home across providers and reframe its strategy as matching specialized models to legal workflows rather than maintaining a single fine-tuned lead.

This is the closest thing the literature has to a longitudinal "did the fine-tune survive a base-model upgrade" experiment, and the answer is: the original tuned-G_n system did not maintain its lead over vanilla-G_n+1 on the domain-specific legal benchmark. The domain is legal reasoning — high vocabulary specificity, high regulatory precision — which is exactly the domain the synthesis identifies as most likely to exhibit durable gains. The failure of durability here is therefore load-bearing: if fine-tune durability fails in legal, the subset of domains where it is expected to hold is materially smaller than C5 implies.

**Important qualification.** Harvey's "original system" was fine-tuned on an earlier-generation base. The relevant three-arm comparison (tuned-G_n vs. vanilla-G_n+1 vs. tuned-G_n+1) still has not been published — Harvey's current system uses newer fine-tuned models on newer bases and reportedly performs above vanilla models on specific sub-tasks. But the public data on BigLaw Bench confirms that vanilla G_n+1 surpassed tuned G_n, which is the most pessimistic durability scenario.

**Verdict:** Upheld. The gap is worse than the streams acknowledged; the Harvey BigLaw Bench data makes it load-bearing.

---

### C6 — Multi-Tenant Works Only Under Narrow Joint Condition

**HIPAA DP recommendation is far below the enterprise-tolerable floor.** The synthesis cites ε = 8 as an enterprise-relevant privacy budget, with the caveat that the cryptographic guarantee at ε = 1 is "the real bar in regulated industries." The regulatory picture is sharper. Best-practice guidance for medical AI fine-tuning in 2026 recommends ε ≤ 0.5 and δ ≤ 1e-6 for HIPAA compliance (preprints.org 2026; dasroot.net 2026). If ε = 0.5 is the healthcare floor, then the ε = 8 regime is not "enterprise-relevant for regulated industries" but rather "enterprise-relevant for lightly regulated industries only." The 6–12 pp performance penalty documented at ε ≤ 1 in the S4 chapter applies at a threshold that regulated healthcare clients cannot exceed.

**EU AI Act provider-reclassification boundary is now specific.** The streams noted this as an unsettled risk. It is now more settled: GPAI model obligations became applicable 2 August 2025. A downstream modifier becomes the new GPAI provider if the training compute used for the modification exceeds one-third of the compute used to train the original model. For an enterprise fine-tuning a frontier model (e.g., Llama 4 with a ~10,000 H100-day training run), the threshold is crossed at approximately 3,300 H100-days of fine-tuning compute — well above the sub-$100 GRPO fine-tuning regime, but relevant for any organization pursuing heavy domain pretraining (Arnold & Porter 2025; DLA Piper 2025). This is a hard stop on multi-tenant economics for enterprises operating in EU jurisdictions that attempt aggressive fine-tuning.

**Federated RLHF at production scale remains unvalidated.** The S4 chapter documents FedBiscuit's 200-client academic benchmark. No production-scale deployment of federated RLHF with real-time enterprise preference data across 1,000+ clients has been publicly documented. The synthesis correctly identifies this as a gap; it is worth escalating: until a production deployment exists, the "works under narrow joint condition" framing is too generous — the correct framing is "works in academic simulation under the narrow joint condition."

**Verdict:** Upheld. The DP epsilon gap between academic estimates and regulated-industry requirements is a hard stop that the synthesis should make explicit.

---

### C7 — Market Prices Training as Talent

**CoreWeave/OpenPipe provides a new data point.** In September 2025, CoreWeave acquired OpenPipe — developers of the ART framework (the flagship open-source GRPO+RULER toolkit) — for undisclosed terms. OpenPipe had raised only $6.7M in a March 2024 seed round; the team had 10 employees at acquisition (TechCrunch 2025b; CoreWeave 2025). This is a direct instance of the acqui-hire pattern: an infrastructure company acquired a capability-building team and folded them into its platform layer, pricing the acquisition at talent-replacement cost rather than IP value. The ART framework is open-source (Apache-2.0 license); its value to CoreWeave is the team's expertise in RL agent training, not the framework itself.

**Strategic implication for Q4.** The CoreWeave/OpenPipe acquisition also directly answers Q4 in this report: the framework on which the synthesis's "interior optimum" recommendation depends was acquired six months before this analysis. Users of ART who were counting on OpenPipe's independence as an open-source governance guarantee are now counting on CoreWeave — an infrastructure vendor with a financial interest in promoting GPU consumption — to maintain that governance. OpenPipe's official blog post (openpipe.ai/blog/openpipe-coreweave) does not provide substantive details on governance changes; the TechCrunch coverage confirms only that existing customers become CoreWeave customers.

**Verdict:** Upheld with a new data point that also creates a new risk (see Section 5).

---

### C8 — "Next Release Eats Fine-Tune" — Case-Dependent

**BigLaw Bench weakens the strongest case study for durability.** The synthesis cites Harvey's 20pp F1 RFT gain as having "persisted through multiple Anthropic+OpenAI+Google base upgrades." The BigLaw Bench data revises this: Harvey's original fine-tuned system has been *surpassed* by seven vanilla base models on BigLaw Bench (Harvey 2025). Harvey's response was to expand to multiple model providers and to reframe its differentiator as workflow matching and data flywheel rather than a single fine-tuned lead. Harvey's CEO noted the platform's competitive advantage now rests in "task execution, firm-specific knowledge integration, and user collaboration," not in a fine-tuned model outperforming base models (Harvey 2025b).

This does not refute C8's case-dependent framing; it narrows it further. The correct claim is: "Durability holds for tasks where the capability gap between fine-tuned and vanilla models exceeds the base model's rate of improvement. On legal reasoning benchmarks, that gap closed within two years for vanilla frontier models. In domains with more idiosyncratic vocabularies (ICD-10, tax code cross-references), the gap likely persists longer — but this has not been formally tested."

**The RL-SFT ordering dependency.** The arXiv:2508.16546 finding adds a compounding risk to C8: if an organization's RL fine-tuning was preceded by aggressive SFT (a common 2024 recipe), the RL-trained model may have permanent OOD performance deficits that vanilla base-model upgrades will surpass while the fine-tuned model cannot recover. This is not merely "next release eats fine-tune" but "next release eats an irreversibly degraded fine-tune."

**Verdict:** Weakened from "case-dependent" to "structurally confirmed erosion on the leading commercial legal case study; likely domain-dependent timeline, but durability window is shorter than the synthesis implies."

---

## 3. Tensions Resolved or Escalated

**T1 — DeepSeek-R1 +55pp vs. 7B replication failure.** Resolved in the direction the streams suggested: workload narrowness is the relevant variable. The ART-E result is consistent with the synthesis's reconciliation — a 14B model can achieve frontier-level performance on email search specifically because the task is tractable for 14B at that specificity. No new evidence disrupts this reconciliation. The open question is whether ART-E's narrowness is representative of enterprise workloads or exceptional; no population data answers this.

**T2 — "Fine-tune is dead" vs. "Fine-tune is the sweet spot."** Partially resolved with a temporal reframe. The a16z survey measured SFT-era fine-tuning (2023–2024 stacks). The 2026 GRPO+RULER stack is technically different. However, the survey's *enterprise adoption signal* — tech-forward Global 2000 companies trending away from fine-tuning toward prompt engineering and context injection — is a demand-side reading of the current market, not just a historical artifact. The tension persists: practitioners building with 2026 stacks find sweet spots; enterprise CIOs in 2025 reported declining ROI. The resolution is that both are true for different buyer profiles at different workflow types.

**T3 — Inference moat durable vs. eroding.** Confirmed eroding in the software domain; structurally durable in the hardware co-design domain. The energy price floor adds a new mechanism: even as algorithmic efficiency is captured, physical energy costs create a floor below which token prices cannot fall regardless of software improvements. This resolves T3 partly in favor of the "eroding" camp for software-driven moats and in favor of the "durable" camp for hardware-driven moats, but on a shorter timeline than the streams imply.

**T4 — ART-E dispositive vs. selection-biased.** Remains unresolved: no independent replication. The appropriate confidence band on C1 given selection bias is "evidence of feasibility in this class of workload, not a calibrated population estimate." The synthesis should grade ART-E as corroborating evidence for the hypothesis rather than primary evidence of population-level prevalence.

**T5 — Outcome-based pricing captures declining-cost surplus vs. market reprices at renewal.** The Harvey multi-provider expansion suggests a different resolution than either side of T5 anticipated: sophisticated enterprise buyers are not passively absorbing pricing but are actively building multi-provider optionality to preserve pricing leverage. Sierra's ARR growth ($150M as of January 2026) suggests outcome-based pricing is holding so far — but Sierra has disclosed no renewal rate data, and the contracts are new enough (company launched February 2024) that renewal dynamics are not yet observable. This tension should be escalated, not resolved.

**T6 — Multi-tenant penalty small (0.3–1pp IID) vs. prohibitive (>15pp under ε=1 DP).** Resolved in the direction of "prohibitive for regulated industries" given the HIPAA best-practice ε ≤ 0.5 recommendation. The IID/algorithm-aware lower bound (0.3–1pp) applies in conditions that do not characterize regulated enterprise data — heterogeneous preferences, strict privacy requirements. The synthesis should present the penalty range as tier-stratified: low for internal corporate knowledge workloads, prohibitive for healthcare and financial-services customer data.

**T7 — "Acquisition was for the harness" vs. harness breaks across upgrades.** The CoreWeave/OpenPipe acquisition resolves one dimension: the acquirer valued the team, not the open-source framework itself. The harness-brittleness concern is escalated by the Anthropic April 2026 Claude Code postmortem, which traced a production regression to harness changes. The synthesis conclusion — harness is valuable but also brittle — is confirmed. The additional risk is that the reference harness framework (ART) is now CoreWeave-controlled.

---

## 4. Answers to Q1–Q8

**Q1 — Has ART-E been independently replicated on a workload OpenPipe did not select?**

No public evidence found as of April 2026 of an independent replication of ART-E on a third-party-selected workload. The dataset and code are public. The right confidence band on C1 is: feasibility demonstrated at 96%/64×-cheaper on the OpenPipe email-search benchmark; population-level prevalence of the sweet spot is unquantified. Practitioners should budget for a workload-specific feasibility trial before assuming the ART-E numbers transfer.

**Q2 — What is the empirical base rate for fine-tune durability across at least one major-version upgrade?**

The Harvey BigLaw Bench data provides the closest available public evidence. Over approximately 18 months (GPT-4o era to GPT-5/Claude 4.5/Gemini 3 era), base model legal reasoning scores rose from ~60% to ~90% on BigLaw Bench, surpassing Harvey's original fine-tuned system within one year. For Llama 2→3 and Qwen 2.5→3, no published study provides the three-arm comparison (tuned-G_n vs. vanilla-G_n+1 vs. tuned-G_n+1). The Lin et al. (2025) diff-vector results cover only the Llama 3.0→3.1 minor-version boundary; major-version transfer has not been tested. The empirical base rate for durability across one major-version upgrade is: unknown for most domains, with Harvey's legal domain as the one publicly observable case, and that case showed durability failure within two years.

**Q3 — Is ε = 8 DP sufficient for production multi-tenant RLHF in regulated industries?**

No. Healthcare best-practice guidance in 2026 recommends ε ≤ 0.5 for HIPAA-compliant medical fine-tuning (preprints.org 2026). Banking guidance has not specified an explicit epsilon threshold, but legal analysis of GDPR compliance suggests ε ≤ 5 for high-sensitivity personal data (ABA Jurimetrics 2023). The translation problem noted in S4 — compliance attorneys lacking technical vocabulary to specify DP budgets — remains unresolved as of April 2026. The practical answer for banking and healthcare is: no validated standard allows ε = 8 multi-tenant RLHF for customer-data-trained models. The synthesis should not present ε = 8 as a regulated-industry viable threshold without this qualification.

**Q4 — Is OpenPipe/ART a durable open-source project, or vulnerable to the acqui-hire pattern?**

The acqui-hire happened. CoreWeave acquired OpenPipe in September 2025 (CoreWeave 2025; TechCrunch 2025b). The ART framework's governance is now CoreWeave's to determine. The Apache-2.0 license means existing users can fork ART, but active development velocity and issue triage are dependent on CoreWeave's priorities. CoreWeave is an infrastructure vendor with a material interest in GPU compute consumption; there is no published commitment to maintaining ART as a neutral open-source resource. Enterprises building a fine-tuning practice around ART should factor in the governance risk of a 10-person team absorbed into a public-market infrastructure company. The synthesis should acknowledge this explicitly.

**Q5 — Does the literature establish the 5–8× compounding inference stack gain, or is it a heuristic?**

The 5–8× figure is a conservative lower bound derived from combining published individual-technique benchmarks, not a single empirical study measuring the full stack simultaneously. Individually: PagedAttention achieves 2–4× throughput (Kwon et al., SOSP 2023); EAGLE-3 achieves 4.1–6.5× latency speedup at temperature 0 (Li et al., NeurIPS 2025); SGLang RadixAttention achieves up to 6.4× in prefix-heavy workloads (Zheng et al., NeurIPS 2024). However, EAGLE-3 speedups collapse at high batch sizes (production regime), and RadixAttention degrades without consistent prefix ordering. The strongest single empirical reference for the compounding claim is Li et al. (NeurIPS 2025) for speculative decoding plus Kwon et al. (SOSP 2023) for memory management, but no published study measures all nine layers simultaneously on a production RL-fine-tuned model. The 5–8× claim is well-grounded in the component literature; the compounding interaction is plausible but unvalidated as a whole-stack figure.

**Q6 — What is the strongest argument the 10×/year decline will NOT hold in 2026–2028?**

Three arguments, strongest first:

1. **Energy constraint as structural floor.** Global AI inference energy demand is projected at 10 GW by end of 2026, with grid interconnection bottlenecks already observable (Tech-Insider 2026; TensorMesh 2025). Energy cost per token cannot decline below the physical cost of the electricity required; as algorithmic efficiency gains are captured, the marginal improvement in cost-per-token requires new hardware generations (18–24 month cycles) or cheaper energy. Neither is a 10×/year trend.

2. **Algorithmic efficiency gains already captured.** The "fastest price drops in the past year" (Epoch AI 2025) reflect deploying already-developed techniques (FlashAttention, PagedAttention, speculative decoding) into production. Future gains require new techniques or new hardware, both of which have longer development cycles than software optimization deployment.

3. **Compute demand growth absorbing supply.** H100 spot prices stabilized in early 2026 rather than continuing to fall, with demand from agentic workloads (long-context, multi-step reasoning) growing faster than supply additions (JarvisLabs 2026). The METR finding that reasoning model task horizons double every 7 months implies token consumption grows faster than the inference fleet.

Under the slowdown scenario (r_API falls from 0.70 to 0.40 per year, r_self falls from 0.40 to 0.15), the S3 break-even for 500M tokens/month extends from 14 months to approximately 28–32 months — still within a 3-year horizon but no longer dominant.

**Q7 — What evidence supports the application-layer lock-in claim — actual renewal data?**

Renewal data does not exist in public sources for any of Sierra, Harvey, or Glean as of April 2026. Sierra reached $150M ARR in January 2026 (Sacra 2025), seven quarters after February 2024 launch — this indicates new customer acquisition, not renewal retention. Glean signed "no sub-one-year contracts" and reported $200M ARR in December 2025, up from $100M nine months earlier (Fortune 2025; Sacra 2025b); the contract structure suggests future renewals will be observable in early 2026, but none have been publicly reported. Harvey's $190M ARR (end of 2025) is driven by law firm expansion, not publicly reported renewals (TechCrunch 2026). The ARR multiples (Sierra 100×, Harvey 41×, Glean 36×) reflect investor expectations of retention, not observed retention. The lock-in claim is an inference from contract structure and growth velocity, not from renewal data. This is the most consequential data gap for C7.

**Q8 — Is there a documented case of a third-party-base-model fine-tuner successfully amortising RL training across enterprise clients?**

No public evidence found. The S4 chapter correctly identifies the legal asymmetry: OpenAI, Anthropic, and Vertex AI all contractually prohibit cross-client weight reuse. Cohere's model ownership enables amortization across clients; an SI fine-tuning Llama 4 can do so legally under the Llama 4 license terms (which permit commercial redistribution subject to user-count thresholds). The "third-party base model + open-weight license + SI amortizes across clients" pattern is legally available but has not been documented in a published case study. This remains hypothetical at the enterprise production level.

---

## 5. New Risks the Streams Missed

**Risk R1 — OpenPipe/ART governance fragility.** The ART framework — the synthesis's recommended open-source GRPO+RULER stack — was acquired by CoreWeave, a GPU infrastructure company, in September 2025. CoreWeave's financial interest is in GPU consumption, which creates a governance misalignment with practitioners optimizing for training-compute efficiency. The Apache-2.0 license permits forking, but active development velocity and production-readiness now depend on CoreWeave's roadmap. Enterprises betting the "interior optimum" strategy on ART should treat it as a vendor-dependent toolkit, not a neutral open-source resource.

**Risk R2 — Reward hacking in production RL via LLM-judge bias.** The synthesis treats RULER (LLM-as-judge) as having "operationally cracked" the reward-function bottleneck. The bias literature shows this is an overstatement: position bias in pairwise judge evaluation produces 10%+ accuracy shifts from ordering alone; self-preference bias causes systematic score inflation for in-distribution outputs (arXiv:2412.05579 2025). In a GRPO training loop, a biased judge does not merely produce noisy signal — it systematically trains the policy to generate outputs that look good to the judge but do not perform the task. The Anthropic paper "Natural Emergent Misalignment from Reward Hacking in Production RL" (Anthropic 2024) documents this failure mode at production scale. The synthesis should prescribe judge reliability controls (position randomization, multi-judge ensemble, calibration audits) as required components of the RULER-based sweet-spot recipe, not optional hardening.

**Risk R3 — EU AI Act fine-tuning reclassification at the one-third threshold.** Enterprises conducting domain adaptive pretraining (heavy fine-tuning) on top of GPAI models in EU jurisdictions face provider reclassification if the modification compute exceeds one-third of the original model's training compute. This is now a specific, enforceable threshold (applicable since 2 August 2025, enforcement from 2 August 2026). For enterprises pursuing the "heavy fine-tuning on Llama 4 / similar" pattern, this reclassification imposes full GPAI provider obligations including technical documentation, transparency, copyright compliance, and post-market monitoring. The synthesis does not price this compliance cost into the TCO model.

**Risk R4 — The durability paradox: better base models make fine-tuning simultaneously more valuable and shorter-lived.** The BigLaw Bench data reveals a paradox: as base models improve, fine-tuning becomes easier and cheaper (stronger base = better initialization); but the window before the next vanilla base surpasses the fine-tune also shortens. The synthesis frames this as "the base model helps the fine-tune"; it should also frame the reverse: the same improving base that enables GRPO at 14B is the same base whose vanilla successor may surpass the fine-tune within 12–18 months. Durability investment theses need to account for both vectors.

**Risk R5 — Inference cost plateau as a break-even model assumption failure.** The S3 parametric break-even model assumes r_API = 0.70 (70% annual API price decline) and r_self = 0.40. If the energy constraint and hardware stabilization arguments are correct, r_API may be closer to 0.30–0.40 and r_self may fall to 0.10–0.15 by 2027–2028. Under that scenario, the 14-month break-even for 500M tokens/month extends to 28–36 months, and the 100M-token threshold (currently marginal) becomes uneconomical within a 3-year planning horizon. The synthesis should run the break-even model with a "price stabilization" scenario as a named sensitivity, not as an afterthought.

---

## 6. Recommended Changes to the Synthesis

- **On C1:** Add a confidence band. Present ART-E as "strongest available evidence of feasibility on one benchmark workload" and recommend that any implementation plan include a workload-specific feasibility trial before committing to training compute. Acknowledge no independent replication exists.

- **On C2:** Add a sixth condition: "the reward function is reliable enough to resist specification gaming; if LLM-as-judge is used, judge reliability controls (position randomization, multi-judge ensemble, calibration audit) are in place." Add the arXiv:2508.16546 finding on SFT-ordering dependency as a prerequisite risk.

- **On C3:** Add an "energy constraint" section to the TCO chapter. Update the price-decline assumption with the Epoch AI "it's less clear those will persist" caveat. Run the S3 break-even model under a price-stabilization scenario and present it as a named sensitivity.

- **On C4:** Reframe "lock-in in harness assets" as "depreciating value in harness assets requiring perpetual maintenance investment." The moat is real but carries ongoing cost.

- **On C5:** Escalate the Harvey BigLaw Bench finding as load-bearing. The three-arm longitudinal study gap is not merely an academic lacuna — it corresponds to the only publicly observable commercial case study showing fine-tune durability failure in a theoretically durable domain (legal).

- **On C6:** Specify that ε = 8 is not a viable DP budget for healthcare fine-tuning. Tier the multi-tenant penalty table by regulatory regime. Add the EU AI Act provider-reclassification threshold (${\sim}3,300$ H100-days for frontier-model fine-tuning) as a hard stop for EU operators.

- **On C7/Q4:** Flag CoreWeave/OpenPipe acquisition as a governance risk for ART-dependent strategies. Recommend enterprises evaluate alternatives or fork ART under its Apache-2.0 license.

- **On C8:** Replace "case-dependent" with "structurally confirmed erosion on the leading commercial legal case study; timeline is domain-dependent." The synthesis should not cite Harvey's RFT gains as a durability success story without disclosing that seven vanilla base models now surpass Harvey's original fine-tuned system on BigLaw Bench.

- **Global:** Add a "reward function reliability" section to the operational playbook. The RULER/LLM-as-judge pattern is a necessary innovation but an imperfect one; the synthesis treats it as an operational unlock without the required failure-mode analysis.

---

## References (Chicago Author–Date)

Andreessen Horowitz. 2025. "How 100 Enterprise CIOs Are Building and Buying Gen AI in 2025." a16z, May 8, 2025. https://a16z.com/ai-enterprise-2025/.

Anthropic. 2024. "Natural Emergent Misalignment from Reward Hacking in Production RL." Anthropic Research. https://assets.anthropic.com/m/74342f2c96095771/original/Natural-emergent-misalignment-from-reward-hacking-paper.pdf.

Anthropic Engineering. 2026. "An Update on Recent Claude Code Quality Reports." Anthropic Engineering Blog, April 23, 2026. https://www.anthropic.com/engineering/april-23-postmortem.

Arnold & Porter. 2025. "Does Your Company Have EU AI Act Compliance Obligations as a General-Purpose AI Model Provider?" Arnold & Porter Advisories, August 2025. https://www.arnoldporter.com/en/perspectives/advisories/2025/08/does-your-company-have-eu-ai-act-compliance-obligations.

Bai, Yushi, Jiahao Ying, et al. 2024. "Benchmarking Foundation Models with Language-Model-as-an-Examiner." In *Advances in Neural Information Processing Systems 37 (NeurIPS 2024)*. arXiv:2306.04181.

CoreWeave. 2025. "CoreWeave to Acquire OpenPipe, Leader in Reinforcement Learning." CoreWeave News, September 3, 2025. https://www.coreweave.com/news/coreweave-to-acquire-openpipe-leader-in-reinforcement-learning.

DLA Piper. 2025. "Latest Wave of Obligations Under the EU AI Act Take Effect: Key Considerations." DLA Piper Publications, August 2025. https://www.dlapiper.com/en-us/insights/publications/2025/08/latest-wave-of-obligations-under-the-eu-ai-act-take-effect.

Epoch AI. 2025. "LLM Inference Prices Have Fallen Rapidly but Unequally Across Tasks." Epoch AI Data Insights. https://epoch.ai/data-insights/llm-inference-price-trends.

European Union. 2024. *Regulation (EU) 2024/1689 of the European Parliament and of the Council (AI Act)*. Official Journal of the European Union. https://artificialintelligenceact.eu/.

Fortune. 2025. "Exclusive: Glean Hits $200 Million ARR, Up from $100 Million Nine Months Back." *Fortune*, December 8, 2025. https://fortune.com/2025/12/08/exclusive-glean-hits-200-million-arr-up-from-100-million-nine-months-back/.

GetDeploying. 2026. "H100 Cloud Pricing: Compare 42+ Providers (2026)." GetDeploying.com. https://getdeploying.com/gpus/nvidia-h100. Accessed April 2026.

Harvey AI. 2025. "Expanding Harvey's Model Offerings." Harvey Blog. https://www.harvey.ai/blog/expanding-harveys-model-offerings.

Harvey AI. 2025b. "Introducing BigLaw Bench." Harvey Blog. https://www.harvey.ai/blog/introducing-biglaw-bench.

JarvisLabs. 2026. "NVIDIA H100 Price Guide 2026: GPU Costs, Cloud Pricing and Buy vs Rent." JarvisLabs Blog. https://jarvislabs.ai/blog/h100-price. Accessed April 2026.

Ji, Ziwei, Nayeon Lee, Rita Frieske, Tiezheng Yu, Dan Su, Yan Xu, Etsuko Ishii, Ye Jin Bang, Andrea Madotto, and Pascale Fung. 2023. "Survey of Hallucination in Natural Language Generation." *ACM Computing Surveys* 55 (12): 1–38. https://doi.org/10.1145/3571730.

Kwon, Woosuk, et al. 2023. "Efficient Memory Management for Large Language Model Serving with PagedAttention." In *Proceedings of SOSP 2023*. https://dl.acm.org/doi/10.1145/3600006.3613165.

Li, Yuhui, et al. 2025. "EAGLE-3: Scaling up Inference Acceleration of Large Language Models via Training-Time Test." In *Advances in Neural Information Processing Systems 38 (NeurIPS 2025)*. arXiv:2503.01840.

Lin, Pin-Jie, et al. 2025. "Efficient Model Development through Fine-tuning Transfer." In *Proceedings of EMNLP 2025*, 2617–2636. https://doi.org/10.18653/v1/2025.emnlp-main.131.

MarkTechPost. 2025. "OpenPipe's ART-E Outperforms o3 in Accuracy, Latency and Cost." MarkTechPost, April 29, 2025. https://www.marktechpost.com/2025/04/29/reinforcement-learning-for-email-agents-openpipes-art%C2%B7e-outperforms-o3-in-accuracy-latency-and-cost/.

OpenPipe. 2025. "ART-E: How We Built an Email Research Agent That Beats o3." OpenPipe Blog. https://openpipe.ai/blog/art-e-mail-agent.

preprints.org. 2026. "Differential Privacy Techniques in Machine Learning for Healthcare Applications." Preprints, June 2026. https://www.preprints.org/manuscript/202506.1752/v1/download.

Sacra. 2025. "Fireworks AI Revenue, Valuation and Funding." Sacra Research. https://sacra.com/c/fireworks-ai/. Accessed April 2026.

Sacra. 2025b. "Glean at $200M ARR." Sacra Research. https://sacra.com/research/glean-at-200m-arr/. Accessed April 2026.

TechCrunch. 2025. "Anthropic, Google Score Win by Nabbing OpenAI-Backed Harvey as a User." *TechCrunch*, May 13, 2025. https://techcrunch.com/2025/05/13/anthropic-google-score-win-by-nabbing-openai-backed-harvey-as-a-user/.

TechCrunch. 2025b. "CoreWeave Acquires Agent-Training Startup OpenPipe." *TechCrunch*, September 3, 2025. https://techcrunch.com/2025/09/03/coreweave-acquires-agent-training-startup-openpipe/.

TechCrunch. 2026. "Harvey Reportedly Raising at $11B Valuation Just Months After It Hit $8B." *TechCrunch*, February 9, 2026. https://techcrunch.com/2026/02/09/harvey-reportedly-raising-at-11b-valuation-just-months-after-it-hit-8b/.

Tech-Insider. 2026. "AI Data Centers: 1,000 TWh by 2026." Tech-Insider, April 2026. https://tech-insider.org/ai-data-center-power-crisis-2026/. Accessed April 2026.

TensorMesh. 2025. "AI Inference Costs in 2025: The $255B Market's Energy Crisis." TensorMesh Blog. https://www.tensormesh.ai/blog-posts/ai-inference-costs-2025-energy-crisis.

Tiwari, Anshuman, Sung Min Park, and Chawin Sitawarin. 2025. "Measuring the Privacy Risk of Synthetic Data." arXiv:2505.01524 [preprint].

Wang, Peiyi, et al. 2025. "Justice or Prejudice? Quantifying Biases in LLM-as-a-Judge." In *Proceedings of ICLR 2025*. OpenReview:3GTtZFiajM.

Xu, Zihe, et al. 2025. "RL Is Neither a Panacea Nor a Mirage: Understanding Supervised vs. Reinforcement Learning Fine-Tuning for LLMs." arXiv:2508.16546 [preprint].

Yu, Da, et al. 2022. "Differentially Private Fine-Tuning of Language Models." *Journal of Privacy and Confidentiality* 14 (2). arXiv:2110.06500.

Ye, Seonghyeon, et al. 2024. "LLMs-as-Judges: A Comprehensive Survey on LLM-based Evaluation Methods." arXiv:2412.05579 [preprint].

Zheng, Lianmin, et al. 2024. "SGLang: Efficient Execution of Structured Language Model Programs." In *Advances in Neural Information Processing Systems 37 (NeurIPS 2024)*. arXiv:2312.07104.

---

*Wordcount: approximately 5,500 words. All citations are to public sources accessed April 2026. No claims have been invented; where public evidence is absent, this is stated explicitly. Vendor blog posts are used as corroborating evidence only.*
