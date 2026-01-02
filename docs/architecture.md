

1. Revised architecture overview

1.1 Components (v2)
	1.	Configuration & Registry Layer
	•	sources.json – curated feeds/sites/YouTube.
	•	subscribers.json – mailing list.
	•	prompt_config.yaml – tone/safety/volume, per environment (alpha/beta/prod).
	•	prompts/ + schemas/ – versioned prompts and JSON schemas.
	2.	Ingestion & Normalization
	•	RSS fetcher.
	•	Web article fetcher + cleaner.
	•	YouTube transcript fetcher.
	•	De-duplication, language filter, length filter.
	•	Output: normalized RawItem[].
	3.	Content Store & Run Metadata
	•	Run store (per week):
	•	raw_items.json
	•	summaries.json
	•	newsletter_spec.json
	•	critics.json
	•	Option: SQLite/Postgres instead of flat files if you want RAG and analytics later.
	4.	Agentic Summarization & Editing Layer
	•	LLM: Gemini (google-genai).
	•	Framework: DeepAgents (or equivalent) for structure + tooling.
	•	Agents:
	•	WeeklyDigestOrchestrator
	•	ItemSummarizerAgent
	•	TopicClassifierAgent
	•	NewsletterEditorAgent
	•	Optional RAG tools (phase 2): search_news_corpus.
	5.	Governance / Safety / Evaluation Layer
	•	Critic agents:
	•	Factual consistency critic.
	•	Tone & safety critic.
	•	Hype & commercial bias critic.
	•	Privacy critic.
	•	Validation:
	•	JSON Schema validation for all machine outputs.
	•	HITL:
	•	Alpha/Beta workflow coordinators + editor preflight assistant.
	•	Quality metrics:
	•	Per-run quality scores, flagged segments, bias/diversity reports.
	6.	Rendering & Delivery
	•	Newsletter renderer:
	•	Jinja2 templates → HTML + text.
	•	Email delivery:
	•	SMTP client using open-source SMTP server.
	•	Run reporting:
	•	RunSummaryReporter → internal status mail / Slack / log.
	7.	Scheduler & Orchestration
	•	Cron / systemd / GitHub Actions → run_weekly_digest.py.
	•	Job orchestrator:
	•	Manages pipeline stages, retries, and status (SUCCESS/FAILED/PARTIAL).

This keeps responsibilities separated:
	•	Ingestion is deterministic.
	•	Agentic layer is “creative”, but behind schemas.
	•	Governance layer catches non-determinism and ethical issues.

⸻

2. Data model v2

You already have:
	•	sources.json
	•	subscribers.json
	•	data/<week>_raw_items.json

Extend to a run directory per week, e.g.:

data/
  2025-12-07/
    raw_items.json
    summaries.json
    newsletter_spec.json
    critics.json
    run_metrics.json

2.1 Raw items (unchanged conceptually)

raw_items.json stays as you defined:

{
  "week_start": "2025-11-30",
  "week_end": "2025-12-07",
  "items": [ /* normalized items */ ]
}

2.2 Summaries (new)

summaries.json:

{
  "week_label": "2025-11-30 → 2025-12-07",
  "items": [
    {
      "id": "openai_blog_2025_12_03_xyz",
      "title": "New agent framework for developers",
      "url": "https://openai.com/blog/...",
      "source_id": "openai_blog",
      "source_type": "vendor",
      "summary": "short factual summary...",
      "topics": ["agents", "tooling_and_infra"],
      "safety_flag": false,
      "safety_comment": ""
    }
  ]
}

2.3 Newsletter spec (already defined via prompts)

newsletter_spec.json:

{
  "week_label": "2025-11-30 → 2025-12-07",
  "intro": "...",
  "sections": [
    {
      "name": "Foundation Models & Agents",
      "items": [
        {
          "id": "openai_blog_2025_12_03_xyz",
          "title": "...",
          "summary": "...",
          "url": "...",
          "source_id": "openai_blog",
          "source_type": "vendor",
          "topics": ["agents", "tooling_and_infra"]
        }
      ]
    }
  ],
  "closing_note": "..."
}

2.4 Critics & run metrics (new)

critics.json:

{
  "factual": {
    "openai_blog_2025_12_03_xyz": {
      "has_unsupported_claims": false,
      "unsupported_claims": []
    }
  },
  "tone_safety": { /* per segment */ },
  "hype_bias": { /* per segment */ },
  "privacy": { /* per segment */ }
}

run_metrics.json:

{
  "week_label": "2025-11-30 → 2025-12-07",
  "items_ingested_total": 32,
  "items_after_filtering": 24,
  "items_summarized": 24,
  "items_in_newsletter": 11,
  "summarization_failures": 1,
  "email_send_success": 135,
  "email_send_failures": 0,
  "pipeline_duration_seconds": 427,
  "status": "SUCCESS"
}

This structure supports:
	•	Debugging.
	•	Trend analysis (coverage, failure patterns).
	•	RAG (later, embedding raw_items + summaries).

⸻

3. Execution flow v2 (alpha, beta, prod)

3.1 Common core pipeline (all environments)
	1.	Schedule
	•	Cron → run_weekly_digest.py.
	2.	Ingest
	•	Load sources.json.
	•	Fetch & normalize → raw_items.json.
	•	Hard filters (time window, language, min length, dedup).
	3.	Summarize + classify
	•	For each item:
	•	ItemSummarizerAgent → summary JSON (validate against schema).
	•	TopicClassifierAgent → topics JSON (validate).
	•	Persist → summaries.json.
	4.	Critics (item-level)
	•	Optionally run:
	•	Factual critic: (source_text, summary).
	•	Tone/safety critic: summary.
	•	Hype/bias critic: summary.
	•	Privacy critic: summary.
	•	Persist → critics.json.
	5.	Newsletter editing
	•	NewsletterEditorAgent receives summaries + topics (and optionally critics’ signals).
	•	Produces newsletter_spec.json (validate against schema).
	6.	Critics (newsletter-level)
	•	Run tone/safety, hype/bias, privacy critics on:
	•	intro
	•	Each section summary
	•	closing_note
	•	Update critics.json.
	7.	Governance/HITL gate
	•	Environment-dependent:
	•	Alpha: full editor review required.
	•	Beta: editor reviews flagged segments + top stories.
	•	Prod: automated thresholds; optional random audit.
	8.	Render
	•	Jinja → newsletter.html, newsletter.txt.
	9.	Send
	•	Load subscribers.json.
	•	Send via SMTP.
	•	Update run_metrics.json.
	10.	Report
	•	RunSummaryReporter → internal email / Slack with key metrics and issues.

3.2 Alpha specifics
	•	All items and sections go through editor review before send.
	•	AlphaWorkflowCoordinator:
	•	Compares auto vs final edited text.
	•	Logs patterns → feed back into prompts/config.
	•	Send only after explicit “approve” flag set.

3.3 Beta specifics
	•	Critics act as first filter.
	•	BetaWorkflowCoordinator:
	•	Build minimal “review queue”:
	•	All segments flagged by critics.
	•	Top N items.
	•	Intro (always).
	•	Editor reviews queue only.
	•	If unresolved critical issues remain → do not send.

3.4 Prod specifics
	•	Critics thresholds tuned (e.g. allow minor tone issues, no safety issues).
	•	No regular editorial review, but:
	•	Periodic quality audits with NewsletterQualityRater + BiasAndDiversityAuditor.
	•	FeedbackTriageAssistant processes subscriber feedback monthly.

⸻

4. Technology choices sanity check
	1.	File-based vs DB
	•	For v1, directory per week with JSON + HTML + metrics is fine.
	•	If you add RAG and more analytics:
	•	Introduce DB (Postgres) for items, summaries, newsletter issues.
	•	Vector store: Chroma/Qdrant/pgvector.
	2.	DeepAgents
	•	Using DeepAgents is justified if:
	•	You want explicit task planning and tooling (critics, RAG tools).
	•	You want filesystem middleware integration.
	•	For PoC:
	•	You can run simple “prompt + Gemini” with manual orchestration, then move to DeepAgents once behavior stabilizes.
	3.	Gemini
	•	Good for summarization, classification, critics, and RAG embeddings.
	•	Keep model names/config central in prompt_config.yaml:
	•	Summarization model.
	•	Critic model (can be cheaper/smaller).
	•	RAG embedding model.
	4.	SMTP
	•	Use Mailhog in dev.
	•	For prod:
	•	Harden Postfix/Exim/Mailcow with SPF/DKIM/DMARC to avoid spam issues.
	5.	Config & prompt mgmt
	•	Prompts under /prompts, schemas under /schemas, configs under /config.
	•	Version them in git and treat changes as product changes (code review, changelog).

Overall, the stack is simple but extensible.

⸻

5. Non-functional aspects

5.1 Reliability and failure modes
	•	Each pipeline stage returns explicit status:
	•	OK, PARTIAL, FAILED.
	•	On error:
	•	Fail fast for structural issues (bad JSON, schema violation).
	•	For external errors (RSS down, SMTP down) → configurable retry with backoff.
	•	Do not send email if:
	•	newsletter_spec missing or invalid.
	•	Any critic marks safety_ok=false for core segments.

5.2 Observability
	•	Structured logs:
	•	stage, item_id, elapsed_ms, status, error_code.
	•	Per-run summary:
	•	items ingested / summarized / included.
	•	critic flags count.
	•	email success/failures.

5.3 Security and privacy
	•	Secrets:
	•	Gemini key, YouTube API key, SMTP credentials in environment or secret manager, not in code.
	•	Content:
	•	No personal data beyond subscriber emails.
	•	No private OSINT about individuals; sources are public AI news only.

5.4 Testability
	•	Deterministic tests for:
	•	Ingestion normalization (no LLM).
	•	JSON schema validation.
	•	LLM tests:
	•	Use small synthetic corpora + deterministic prompts.
	•	Evaluate critic outputs with known “bad” summaries.

⸻

6. Phased implementation updated

Your existing phases are correct; just refine with governance:

Phase 0 – PoC
	•	Ingest → single LLM call → newsletter JSON → render → send.
	•	Minimal prompts, no critics, no DeepAgents.
	•	Use internal-only recipients.

Phase 1 – Structured agentic pipeline (DeepAgents, no RAG)
	•	Split into:
	•	Summarizer agent.
	•	Editor agent.
	•	Introduce:
	•	JSON schemas.
	•	Basic critics (tone/safety at least).
	•	Alpha HITL flow.

Phase 2 – Hardening and governance
	•	Add full critic suite and HITL coordinators.
	•	Add NewsletterQualityRater + BiasAndDiversityAuditor periodically.
	•	Move from alpha → beta → prod flows.

Phase 3 – RAG + archive UX
	•	Add vector store and RAG tools.
	•	Add “Ask the AI Digest” search/chat endpoint.
	•	Optionally add web UI for archive and search.

⸻
