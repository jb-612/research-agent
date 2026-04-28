# Cross-Stream Synthesis — Claims and Tensions for Adversary Attack
*Date: 2026-04-28 | Author: orchestrator | Status: input to Phase 4 adversary*

This document distils the load-bearing claims emerging from Streams S1–S6 into a form the adversary can attack. Every claim below is intended as falsifiable. Each claim cites the stream(s) of origin so the adversary can drill down via the source chapter if needed.

## 1. Top-line claims (under attack)

### C1 — A workload-shaped sweet spot for proprietary RL fine-tuning *exists today*.

The strongest single piece of evidence is OpenPipe's ART-E benchmark: a 14B Qwen2.5 fine-tuned with GRPO + RULER beat o3, o4-mini, Gemini 2.5 Pro and GPT-4.1 on email-search at 96% accuracy, 1.1s latency, $0.85 / 1,000 runs, with **training compute under $80 on a single GPU**. This is +56pp over the base model. Source: S6.

Stream S2 generalises: SFT-distillation at 32B + budget forcing achieves 50–70% AIME24 for $25–$1,250, with a second-stage GRPO ($2.5–$12.5K) pushing to 70–82%. Pure RL without distillation only emerges at frontier scale (671B+ MoE).

Stream S3 closes the loop: at $0.02–$0.11/M tokens self-hosted floor and a $15K + $3K/mo engineering envelope, break-even against mid-tier APIs (~$0.15/M) lands at ~500M tokens/month with 14-month payback; against premium APIs ($2.50/$10) at ~50M tokens/month / 7-month payback.

### C2 — The sweet spot is *narrowly bounded* and assumes specific workload conditions.

The conditions are: (a) the workload is narrow enough that a 7B–32B specialist can dominate, (b) reward is verifiable or LLM-judge-rateable, (c) token volume is high enough to amortise the inference engineering, (d) workflow stays stable enough for re-fine-tunes per base-model generation to be bearable, (e) the operator can self-host inference. Outside this envelope, frontier APIs win on cost-per-task once you account for engineering. Sources: S2, S3, S6.

### C3 — Inference engineering, not training, is the durable moat.

A nine-layer inference optimisation stack compounds to a 5–8× cost-efficiency gap over naïve serving, replicable in principle but operationally hard. Token prices fell ~10×/year through 2026; "most of that drop came from the serving stack." Multi-LoRA collapses adapter inference cost towards zero per tenant; this is the fine-tuning enabler. Fireworks $315M ARR / 416% YoY at pure-inference-play scale corroborates the revenue side. Sources: S1a, S3, S6.

### C4 — Vendor lock-in is real but lives in *prompt and harness assets*, not in model weights.

Empirically: Claude 4 captured 45% of Anthropic users within one month of release; Sonnet 3.5 dropped from 83% to 16% (S1a). The Opus 4.6 → 4.7 migration documented by Chawla shows seven categories of behaviour change requiring prompt/harness re-engineering, but no weight-level lock-in (S6). Therefore: hypothesis A is partially true at the model layer, partially false at the surrounding-asset layer.

### C5 — Durability of fine-tuning gains is empirically under-measured at the major-version boundary.

The peer-reviewed literature lacks the canonical three-arm comparison (tuned-G_n vs vanilla-G_n+1 vs tuned-G_n+1) for any domain. Lin et al. EMNLP 2025 demonstrate diff-vector recycling transfers +46.9pp IFEval and +15.7pp LiveCodeBench across the *minor*-version Llama 3.0→3.1 boundary, but no published study extends to major generations. Catastrophic forgetting reaches 18–35% on 7B decoder models (Luo et al.). Sources: S1, S6.

### C6 — Multi-tenant economics work only under a narrow joint condition.

The data-isolation penalty under realistic federated RLHF + DP is 0.3–1pp (IID), 2–7pp (non-IID Dirichlet α=0.3), and >15pp at ε=1 DP. The wedge requires *both* (a) base-model ownership (no current OpenAI/Anthropic/Vertex terms permit cross-client weight reuse) *and* (b) outcome-based pricing (Sierra $150M ARR; Decagon $35M ARR). Synthetic-data approaches still carry residual membership-inference risk (AUC > 0.8 on outliers). Sources: S4.

### C7 — The market prices model-training capability as talent, not as IP.

In the Inflection/Adept/Character.AI/MosaicML/Reka cohort, license fees were $9–41M per researcher (5–20× normal acqui-hire). Adept's $414M training pipeline licensed for $25M. Application-layer plays (Sierra 100× ARR, Harvey 41×, Glean 35×) extract premium valuations with zero proprietary training. Aleph Alpha → Cohere confirms sovereignty as a third value category. Sources: S1a, S5, S6.

### C8 — The "next release eats your fine-tune" claim is partially confirmed, structurally likely, but case-dependent.

Confirmed structurally: a16z 2025 CIO survey shows declining fine-tuning ROI; Menlo Ventures shows open-source running 9–12 months behind frontier. Disconfirmed in specific cases: Harvey's 20pp F1 RFT gain has persisted through multiple Anthropic+OpenAI+Google base upgrades because the legal eval is hard and proprietary. Therefore: durability is a function of *eval verifiability, vocabulary specificity, and regulatory schema depth*, not a universal law. Sources: S1, S1a, S6.

## 2. Tensions and apparent contradictions for the adversary to probe

### T1 — DeepSeek-R1's +55pp at 600B vs the 7B replication failure
Stream S2 reports community 7B replications produce only 2–5pp gains; Stream S6 reports the 14B ART-E model beating frontier at 96%. **Reconciliation candidate**: ART-E is a narrow workload (email search) where 14B has enough capacity, while AIME-style competition math may genuinely require >32B latent capability. The adversary should test whether the workload-narrowness-vs-capability-floor relationship holds across domains, or whether ART-E's win is benchmark-specific.

### T2 — "Fine-tune is dead" (a16z CIO survey) vs "Fine-tune is the sweet spot" (S2/S3/S6)
The a16z 2025 CIO survey reports declining enterprise fine-tuning ROI; the practitioner channel and the academic literature both report the opposite is becoming true in 2026. **Reconciliation candidate**: the a16z respondents fine-tuned in 2023–2024 with SFT and weak verifiers; the 2026 stack (GRPO + RULER + multi-LoRA + self-host) is a different beast. The adversary should test whether the 2025 enterprise survey is even measuring the 2026 capability or is reporting on a stale stack.

### T3 — Inference moat is durable (S3, S6) vs eroding (S1a)
S3 reports FireAttention's proprietary kernel edge has compressed from 4× to 1.5–2× over 18 months. Open-source vLLM/SGLang catches up. S1a frames this as a 12–18 month durable lag. **The tension is durability period, not direction**. The adversary should pressure-test whether the durable structural advantages (Groq LPU SRAM, Cerebras WSE) are actually large enough to justify hardware-co-design investment, or whether they are niche.

### T4 — ART-E's 64× cost / 96% accuracy is dispositive (S6) vs vendor-published (and therefore selection-biased)
ART-E is OpenPipe's own benchmark. The methodology is open and the code is reproducible, but no independent replication on a workload OpenPipe didn't pick has been published. The adversary should grade this evidence appropriately and search for independent replications.

### T5 — Outcome-based pricing wedge (S4) vs inference-cost decline (S1a)
If token costs fall another 50% in 12 months, who captures the surplus? S4 says outcome-priced vendors (Sierra) capture as margin; S1a worries the market repricesces resolution-priced contracts on renewal. The adversary should test the renewal-cycle dynamic explicitly.

### T6 — Multi-tenant penalty is small (S4: 0.3–1pp IID) vs prohibitive (S4: >15pp under ε=1 DP)
Stream S4 itself contains the tension: under benign conditions multi-tenancy works; under regulated conditions it doesn't. The adversary should pressure-test whether enterprise buyers can actually live with ε=8 DP, or whether the cryptographic guarantee at ε=1 is the real bar in regulated industries.

### T7 — "The acquisition was for the harness, not the model" (Manus → Meta $2B) vs the evidence that harness assets break across model upgrades
Stream S6 documents the Manus framing; Stream S6 also documents the Opus 4.6 → 4.7 migration cost. **The harness is valuable but also brittle**. The adversary should test whether harness-as-moat survives the next two base-model generations or whether it gets re-engineered every 6–9 months.

## 3. The questions the adversary phase MUST answer

Q1 — *Has the ART-E result been independently replicated, on a workload OpenPipe did not select, with comparable cost/quality outcomes? If not, what's the right confidence band on C1?*

Q2 — *What's the empirical base rate for fine-tune durability across at least one major-version base-model upgrade boundary? Lin et al. covered Llama 3.0→3.1; what does Llama 2→3 or Qwen2.5→Qwen3 actually show?*

Q3 — *Is the ε=8 DP regime sufficient for production multi-tenant RLHF in regulated industries (banking, healthcare)? Cite specific regulatory rulings or industry guidance, not academic claims.*

Q4 — *Is OpenPipe / ART itself a durable open-source project, or vulnerable to the same acqui-hire pattern (Stream S5) that priced Adept's $414M pipeline at $25M? What happens to the 14B sweet-spot strategy if the framework is acquired?*

Q5 — *The 9-layer inference optimisation stack (S6) reports compounding 5–8× gains. Does the literature actually establish this number, or is it a heuristic? Show the strongest single empirical reference for the compounding number.*

Q6 — *The 10×/year token-price decline trend has held since 2022. What's the strongest argument it will NOT hold in 2026–2028, and what does S3's break-even calculation look like under that argument?*

Q7 — *Application-layer ARR multiples (Sierra 100×, Harvey 41×, Glean 35×) imply enormous embedded customer-lock-in value. What evidence supports the lock-in claim — actual renewal data, customer-by-customer? Or is this latest-round froth pricing that won't survive the next correction?*

Q8 — *The user's hypothesis E (multi-tenant collapses to cost+) was partially confirmed and partially refuted. The Cohere synthetic-data wedge requires owning the model architecture. Is there a documented case of *third-party-base-model fine-tuner* successfully amortising RL training across enterprise clients? Or is that pattern still hypothetical?*

## 4. Methodology for adversary

- Each numbered claim (C1–C8) gets a verdict: *upheld*, *upheld with qualifications*, *weakened*, or *refuted*. Cite the new evidence.
- Each tension (T1–T7) gets a reconciliation or a confirmed contradiction.
- Each question (Q1–Q8) gets an answer or an honest "no public evidence found, here's why this matters".
- Adversary may consult the source chapters (file paths in the next section) but should focus on **finding evidence the streams missed** — particularly counter-examples, post-mortems, regulatory rulings, and renewal-cycle data.

## 5. Source chapter file paths

- `research/rl-finetuning-roi/phase1-scan/market-scan.md` — Stream S1a output
- `research/rl-finetuning-roi/phase1-scan/academic-scan.md` — Stream S1b output
- `research/rl-finetuning-roi/phase2-deep/stream-1-durability.md` — Stream S1 chapter
- `research/rl-finetuning-roi/phase2-deep/stream-2-sweet-spot.md` — Stream S2 chapter
- `research/rl-finetuning-roi/phase2-deep/stream-3-tco.md` — Stream S3 chapter
- `research/rl-finetuning-roi/phase2-deep/stream-4-multitenant.md` — Stream S4 chapter
- `research/rl-finetuning-roi/phase2-deep/stream-5-acquihire.md` — Stream S5 chapter
- `research/rl-finetuning-roi/phase2-deep/stream-6-practitioner.md` — Stream S6 chapter

---

*End of synthesis-claims. Adversary phase fires next.*
