---
name: rl-finetune-roi-research
description: Evaluate ROI of reinforcement learning and fine-tuning of foundation models via a structured 5-phase research project (parallel market+academic discovery → stream selection → parallel deep research → adversarial review → synthesis). Produces per-chapter Markdown files with verified academic citations, optional infographics, a mandatory human-in-the-loop gate, and a final PDF. Trigger ONLY when the user asks about the ROI, business case, "sweet spot," or decision criteria for fine-tuning / RL / RLHF / LoRA / domain-adapting foundation models, OR explicitly asks to run this exact 5-phase research workflow on a substituted topic. Do NOT trigger for general "research X" requests — the deep-research and research skills already cover those.
argument-hint: "[optional: topic override — substitutes the subject, preserves the workflow]"
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, Agent, WebSearch, WebFetch, TaskCreate, TaskUpdate, AskUserQuestion
---

# RL / Fine-Tuning ROI Research

This skill orchestrates a long, multi-phase research project. It does **not** do its own web search — it spawns the existing `deep-research`, `research`, and `adversary` skills as building blocks, and uses `diagram-builder` and `pdf-export` for outputs.

- If `$ARGUMENTS` is empty → run the embedded **Default Brief** below.
- If `$ARGUMENTS` contains a topic override (e.g. "RL ROI for vertical-AI healthcare startups") → substitute the **Objective** with the override, regenerate hypotheses appropriate to that topic, but keep the 5-phase process and all output requirements intact.

The point of this skill is the *workflow shape*: parallel discovery → focused stream selection → parallel deep dives → adversarial pressure-test → verified-citation synthesis → human review → PDF. Don't shortcut it.

---

## Default Brief — RL / Fine-Tuning ROI for Foundation Models

### Objective
Evaluate whether RL and fine-tuning of foundation models delivers positive ROI, and identify the decision criteria that govern when (if ever) it does. End-goal: find — or formally declare absent — a **sweet spot** of minimal RL/fine-tuning investment yielding maximal gains in model reasoning, while solving the inference-economics and scale problems. Purpose: recognize this opportunity ahead of the market.

### Working Hypotheses (test, do not assume)
- **A.** No vendor lock-in → no ROI. Clients swap foundry models every few weeks; investing in your own model is a stranded-asset risk.
- **B.** Gain ceiling is low. RL on open-source models yields ~5–8% domain-specific improvement at best; the next hyperscaler release typically erases it.
- **C.** Business model is harder. Wrapping foundry-model cost inside your product creates pricing instability — simplify and you lose margin, expose observability and you get replaced.
- **D.** Inference, not training, is the real challenge. Caching, session management, performance, hardware optimization compound into perceived quality but require scale; favors SaaS over bespoke.
- **E.** Enterprise economics fail at scale. Private fine-tuned models can solve domain problems foundry models can't, but data-isolation blocks cross-client reuse — forces cost-plus services, not a product business.
- **F.** Market signal is mixed. Chinese labs build credible models on ~1% of hyperscaler spend; AI21 fails; even OpenAI/Anthropic/Google/xAI burn cash. Risk is real and unevenly distributed.
- **G.** Framed as optimization. Investment vs. returns has an optimum — find it, or declare it doesn't yet exist and predict when maturing tooling will move it into reach.

These hypotheses are **prior framing, not conclusions.** Evidence may overturn any of them. State explicitly which hypotheses each chapter supports, qualifies, or refutes.

---

## Why the structure looks like this

A reader can fairly ask "why not just one big report?" Three reasons drive the chapter-per-file rule, the citation gate, and the HITL pause:

1. **One `.md` per chapter** keeps each write within a tight context budget. Long monolithic docs cause the writer to either skip evidence to stay coherent, or lose coherence trying to keep evidence. Per-file scoping forces honest depth.
2. **Citations are verified before synthesis**, not after, because synthesis built on hallucinated references *launders* hallucination into authority. By the time the bibliography is wrong, the conclusions feel solid — and the conclusions are the part that gets cited externally.
3. **HITL before PDF** because this output represents a strategic position with the user's name on it. Model-only sign-off is not appropriate for that.

If you find yourself wanting to skip one of these to save time, the answer is no — they are the things that make the output usable.

---

## Workflow

### Step 0 — Setup

1. Create the research folder: `research/<slug>-YYYY-MM-DD/`
   - Default brief → `<slug>` = `rl-finetune-roi`
   - Topic override → kebab-case slug derived from the override
2. Pre-create the file scaffold:
   ```
   research/<slug>-YYYY-MM-DD/
   ├── 00-brief.md           # frozen copy of the brief actually used
   ├── 01-discovery-market.md
   ├── 02-discovery-academic.md
   ├── 03-stream-selection.md
   ├── 04-stream-<a>.md      # one file per stream picked in Phase 2
   ├── 04-stream-<b>.md
   ├── 05-adversarial.md
   ├── 06-synthesis.md
   ├── 07-bibliography.md
   ├── diagrams/             # SVG / PNG outputs
   └── checks/               # citation-verification artifacts
   ```
3. Use `TaskCreate` to register one task per phase: P1-discovery, P2-stream-selection, P3-deep-research, P4-adversarial, P4.5-citation-audit, P5-synthesis, P6-diagrams, P7-HITL, P8-PDF. Move each through `pending → in_progress → completed` only when its artifact exists **and** its checklist passes.
4. Write `00-brief.md` containing the Objective + Hypotheses verbatim, so future chapters anchor against an immutable copy.

### Phase 1 — Parallel Discovery

Spawn **two agents in the same turn** (parallel — same message, two `Agent` tool calls):

- **Market agent** → writes `01-discovery-market.md`. Cover: vendor landscape, deal flow, observed pricing, public post-mortems (e.g. AI21), operator commentary, observability gaps. Pass it the brief and instruct it to invoke the `deep-research` skill with focus "industry / market signals."
- **Academic agent** → writes `02-discovery-academic.md`. Cover: Q1 peer-reviewed work on RLHF, RLAIF, parameter-efficient fine-tuning (LoRA, QLoRA, adapters, prefix tuning), inference economics, distillation ROI. Instruct it to invoke the `research` skill with `--perspectives explorer,architect` `--depth deep`.

Each chapter must end with a **"Surfaced themes"** section listing 5–10 candidate streams the writer thinks merit deeper investigation, each tagged with which working hypothesis (A–G) it pertains to.

**Phase 1 checklist (tick before marking task complete):**
- [ ] Both files exist
- [ ] Every claim has an inline citation (no bare assertions)
- [ ] Each file ends with "Surfaced themes" and tags hypotheses
- [ ] No claim silently contradicts the brief's hypotheses — contradictions are flagged

### Phase 2 — Stream Selection

Read both Phase-1 files. Write `03-stream-selection.md` containing:
1. A merged, deduplicated table of all surfaced themes.
2. A scoring rubric — three dimensions, 1–5 each:
   - **novelty** — would finding this change the answer?
   - **tractability** — can we get real evidence inside this project's scope?
   - **load-bearing-ness** — does the final ROI verdict hinge on it?
3. Rank by sum. Pick **2–4 streams** for Phase 3. Justify each pick in 2–3 sentences; one-line justification for each rejection.
4. For each selected stream, write a one-paragraph **research charter** — this becomes the Phase-3 deep-research agent's prompt verbatim.

**Stop and confirm with the user before launching Phase 3.** Phase 3 is the most expensive step. Use `AskUserQuestion` to confirm the picks before spawning agents.

### Phase 3 — Deep Research (Parallel)

Spawn **one agent per selected stream, all in the same turn** (parallel). Each agent:
1. Receives its charter from `03-stream-selection.md` as its task brief.
2. Invokes the `deep-research` skill scoped to its stream.
3. Writes exactly one file — `04-stream-<slug>.md`. **Never merge streams into a shared file.**
4. Each chapter includes:
   - An **Evidence table**: claim → source → strength (strong / medium / weak)
   - A **Hypothesis impact** subsection naming which of A–G this stream confirms, qualifies, or refutes, with the supporting evidence rows

**Phase 3 checklist (per chapter):**
- [ ] File exists at `04-stream-<slug>.md`
- [ ] Every citation includes author, year, title, venue, and URL or DOI
- [ ] Evidence table present and complete
- [ ] Hypothesis impact subsection present
- [ ] Substantive but not bloated (rough target: 800–2000 lines, but better short and tight than long and padded)

### Phase 4 — Adversarial Review

Run the `adversary` skill against the combined Phase-3 chapters. Goal: surface weakest links, load-bearing assumptions, missing counter-evidence, and any citation that doesn't actually support the claim it backs.

Write `05-adversarial.md` with sections:
- **Load-bearing assumptions** — if these break, what conclusions break with them?
- **Counter-evidence** — sourced challenges to specific claims
- **Citation challenges** — references that look wrong, weak, or misread
- **Synthesis risks** — premature pattern-matches to watch for in Phase 5

### Phase 4.5 — Citation Verification (HARD GATE)

**Do not start Phase 5 until this passes.** For every citation across files 01–05:

1. Re-fetch the source (URL/DOI). If unreachable after a reasonable retry, mark it `[UNVERIFIED]`.
2. Confirm the cited source actually contains the quoted/claimed material — not merely that it exists.
3. Write `checks/citation-audit.md` with one row per citation:

   | cite-id | source | exists? | supports claim? | evidence quoted | action taken |

4. **Fail the gate** if any citation is unverified or unsupported. Fix the chapters (replace the source or remove the claim) and re-run the audit before proceeding.

The reason for the gate: research that cites confidently but inaccurately is worse than research with no citations — it launders hallucination into authority. The bibliography is the part that gets independently checked first; if it doesn't survive scrutiny, neither does anything built on it.

### Phase 5 — Synthesis & Conclusion

Write `06-synthesis.md`:
1. **Reconciled narrative** — weave streams + adversarial findings into one argument, structured around the seven hypotheses.
2. **Sweet-spot finding** — state plainly whether the optimum exists, what it looks like, the conditions under which it holds, and the strongest objections to it. If it does not exist yet, say so and predict what would have to change for it to exist.
3. **Decision criteria** — a checklist a practitioner could apply to their own situation to make a go / no-go call on RL or fine-tuning.
4. **Confidence notes** — tag every major claim as **high / medium / low** confidence and explain the basis (number of independent sources, source quality, adversarial robustness).

Then write `07-bibliography.md` — full bibliography in academic style:
> Author, A. (Year). *Title.* Venue. DOI / URL.

Group by source type: peer-reviewed | preprint | industry report | press | primary data.

### Phase 6 — Diagrams & Infographics

Use the `diagram-builder` skill for structural diagrams (Mermaid → SVG):
- The 5-phase workflow itself
- Hypothesis × stream impact matrix
- Decision tree extracted from §06

For richer infographics where the analysis genuinely benefits from visual abstraction (e.g., cost-vs-quality frontier, vendor positioning quadrant), call GCP Vertex Nano Banana Pro.

**Only generate a diagram if it abstracts a concept or visualizes an analytical insight.** No decoration. If you can't articulate what insight a diagram conveys, don't make it.

Save all outputs to `diagrams/`, reference inline from the chapter that uses them.

### Phase 7 — HITL Gate (MANDATORY, NON-SKIPPABLE)

Before any final editorial pass or PDF export:

1. Summarize for the user: what was found, where the disagreements were, what the sweet-spot verdict is, what the strongest objections are.
2. Use `AskUserQuestion` to ask: *"Research complete and citation-verified. Ready for you to read the chapter files. Proceed to PDF, edit first, or continue investigating?"*
3. Wait for the user's response. Apply requested edits.
4. Only then proceed to Phase 8.

This gate exists because the output represents a strategic position cited under the user's name. Skipping it is not acceptable, even if the work feels complete.

### Phase 8 — PDF Export

Concatenate chapters in order (00 → 07) into a single Markdown file `final.md` with a title page, TOC, and the bibliography as the final annex. Then invoke the `pdf-export` skill on `final.md`.

---

## Topic Override

If `$ARGUMENTS` is non-empty, treat it as a topic override:
1. Re-write the **Objective** to fit the new topic. Show it to the user and confirm before proceeding.
2. Re-derive working hypotheses appropriate to the topic — typically 5–8, not always seven.
3. Keep folder layout, 5-phase process, citation gate, HITL gate, and PDF export identical.

The skill exists for the workflow shape, not the subject. A different topic doesn't earn a different process.

---

## What this skill is NOT for

Don't trigger on:
- "Research X" / "look up Y" / "summarize Z" — `deep-research` handles those.
- Quick competitive lookups, market sizing, due diligence — `research` and `deep-research` handle those.
- Anything that doesn't justify a multi-day, multi-phase, peer-review-grade output.

If the user wants a one-shot answer, recommend `deep-research` instead and stop.
