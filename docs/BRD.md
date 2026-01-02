1. Business Requirements Document (BRD)

1.1 Business Objective
Build a small agentic system that:
	•	Monitors a curated set of AI-related sources (RSS, websites, YouTube).
	•	Weekly extracts relevant content.
	•	Uses an LLM (Gemini API) to summarize and synthesize “Last Week in AI”.
	•	Distributes a concise newsletter via email, with links for deep reading.
	•	Operates with low manual overhead and is testable via functional + BDD scenarios.

1.2 Scope
In-scope (Phase 1)
	•	Read source configuration from sources.json (RSS, websites, YouTube links).
	•	Fetch and normalize content once per week.
	•	Store weekly content snapshot.
	•	Summarize items and curate a newsletter using Gemini API, optionally orchestrated by DeepAgents.
	•	Read mailing list from subscribers.json.
	•	Render HTML + plaintext newsletter.
	•	Send emails via open-source SMTP server.
	•	Basic monitoring/logging of runs and failures.

Out-of-scope (Phase 1)
	•	Web UI for subscribers or admins.
	•	Self-service subscription/unsubscribe flow (handled manually via JSON edits).
	•	Full RAG implementation (Phase 2).
	•	Multi-language newsletters.
	•	Per-user personalization (tags exist but only for future use).

1.3 Stakeholders & Users
	•	Product Owner / Editor – defines sources, reviews newsletters (initially).
	•	Engineering / Ops – deploys and maintains the agent.
	•	Subscribers – receive and read the weekly digest.
	•	Compliance / Security (if relevant) – ensures use of sources and data complies with policies.

1.4 High-Level Functional Requirements
FR-1: System shall read RSS, websites, and YouTube links from sources.json.
FR-2: System shall fetch new content weekly and filter by date/time window (last 7 days).
FR-3: System shall normalize all content into a unified internal format (title, url, type, published_at, content).
FR-4: System shall persist the weekly content in a structured file (e.g. YYYY-MM-DD_raw_items.json).
FR-5: System shall summarize content and curate a newsletter using Gemini API (possibly via DeepAgents).
FR-6: System shall read subscribers from subscribers.json.
FR-7: System shall generate HTML and plaintext newsletter variants.
FR-8: System shall send the newsletter via SMTP to all subscribers and log send status.
FR-9: System shall provide basic run logs and error reporting.

(Phase 2 – FR-10+ for RAG, archive search, etc.)

1.5 Non-Functional Requirements (NFRs)
	•	NFR-1: Reliability – Weekly job must succeed > 95% of scheduled runs; failed run is logged and can be re-triggered.
	•	NFR-2: Performance – For up to 200 items, pipeline completes within 15 minutes under normal conditions.
	•	NFR-3: Security – API keys (Gemini, YouTube) not stored in code; use env variables or secrets manager.
	•	NFR-4: Observability – Logs for each major step (ingestion, summarization, email sending).
	•	NFR-5: Maintainability – Configuration via JSON or environment variables, not hardcoded constants.
	•	NFR-6: Testability – Core logic covered by unit tests; main flows covered by BDD scenarios.

1.6 Assumptions
	•	All sources allow programmatic access for personal/organizational use.
	•	SMTP server is reachable and correctly configured for outbound mail.
	•	Gemini API quota is sufficient for weekly summarization load.
	•	Number of subscribers is modest (e.g. < 10k) in Phase 1.

1.7 Risks
	•	Rate limiting or blocking of scraping for some websites.
	•	Changes in site structure breaking HTML extraction.
	•	LLM hallucination or misclassification of importance.
	•	SMTP deliverability issues (spam filters, domain reputation).

⸻

2. Feature Breakdown, User Stories & BDD-Oriented Success Criteria

Below: features with user stories and success criteria written in a BDD-friendly style (Given/When/Then).

⸻

Feature F1: Source Configuration & Management
User Story F1.1 – Maintain source list
	•	As a product owner
	•	I want to manage a list of curated AI sources in a single sources.json file
	•	So that the system can fetch content without code changes.

Success Criteria / BDD Examples
	•	Scenario F1.1.1 – Load valid sources.json
	•	Given a valid sources.json containing RSS, website, and YouTube entries
	•	When the weekly job starts
	•	Then the system loads all sources without errors
	•	And the total number of sources loaded is logged.
	•	Scenario F1.1.2 – Invalid sources.json
	•	Given an invalid sources.json (e.g. missing required field)
	•	When the weekly job starts
	•	Then the system fails fast with a clear error message
	•	And no network calls are made to external sources.

⸻

Feature F2: Weekly Ingestion Pipeline
User Story F2.1 – Fetch RSS content
	•	As the system
	•	I want to read RSS feeds and collect items from the last 7 days
	•	So that the newsletter reflects “last week in AI”.

Success Criteria
	•	Scenario F2.1.1 – Standard RSS feed
	•	Given a valid RSS feed URL in sources.json
	•	And the feed contains items with publication dates
	•	When the ingestion step runs for week W
	•	Then the system collects only items with published_at within W’s date range
	•	And each item includes title, URL, published_at, and content snippet.

User Story F2.2 – Fetch website articles
	•	As the system
	•	I want to fetch and clean articles from configured websites
	•	So that the LLM receives readable main text.

Success Criteria
	•	Scenario F2.2.1 – Extract article body
	•	Given a website URL in sources.json pointing to an article
	•	When the ingestion step runs
	•	Then the system downloads the HTML
	•	And extracts main article content (no navigation or ads)
	•	And stores it in the content field of the item.

User Story F2.3 – Fetch YouTube transcripts
	•	As the system
	•	I want to fetch transcripts for configured YouTube videos
	•	So that the LLM can summarize video content.

Success Criteria
	•	Scenario F2.3.1 – Transcript available
	•	Given a YouTube URL with accessible captions
	•	When the ingestion step runs
	•	Then the system retrieves the transcript text
	•	And stores it in the content field
	•	And marks the item type = "video".
	•	Scenario F2.3.2 – Transcript not available
	•	Given a YouTube URL without captions
	•	When the ingestion step runs
	•	Then the system marks the item as transcript_missing
	•	And logs a warning
	•	And excludes the item from summarization (or marks as partial).

User Story F2.4 – Persist weekly content snapshot
	•	As an engineer
	•	I want weekly raw content stored in a timestamped file
	•	So that I can debug or re-run summarization without refetching.

Success Criteria
	•	Scenario F2.4.1 – Save weekly raw file
	•	Given ingestion completes successfully
	•	When the pipeline finishes the ingestion phase
	•	Then the system saves YYYY-MM-DD_raw_items.json
	•	And the file contains all items with normalized structure.

⸻

Feature F3: Summarization & Curation (LLM / DeepAgents)
User Story F3.1 – Summarize individual items
	•	As a newsletter editor (via the system)
	•	I want each item summarized to a short, factual paragraph
	•	So that readers can scan the newsletter quickly.

Success Criteria
	•	Scenario F3.1.1 – Summarize article
	•	Given a raw item with non-empty content
	•	When the summarization agent is invoked for that item
	•	Then the system calls the Gemini API with the article text
	•	And receives a summary not exceeding a configured word limit (e.g. 80 words)
	•	And stores the summary alongside the item.

User Story F3.2 – Select and group key items into sections
	•	As the weekly digest editor (system)
	•	I want to group items into 3–5 sections and select the top N items
	•	So that the newsletter is structured and concise.

Success Criteria
	•	Scenario F3.2.1 – Create newsletter spec
	•	Given a list of summarized items for the current week
	•	When the newsletter editor agent runs
	•	Then it returns a JSON spec containing intro, sections, and closing_note
	•	And there are 3–5 sections
	•	And at most N (e.g. 12) total items included across all sections.
	•	Scenario F3.2.2 – No items available
	•	Given there are no summarized items for the week
	•	When the newsletter editor agent runs
	•	Then it returns a JSON spec with an intro explaining no major updates
	•	And zero sections or a single “No major news this week” section.

User Story F3.3 – Use DeepAgents for orchestration (optional)
	•	As an engineer
	•	I want the summarization and curation steps orchestrated by DeepAgents
	•	So that I can later extend the pipeline with additional tools (e.g. RAG) without major refactoring.

Success Criteria
	•	Scenario F3.3.1 – Orchestrated run
	•	Given DeepAgents is configured with a root agent and sub-agents
	•	When the weekly job is triggered
	•	Then the root agent logs task planning and execution steps
	•	And calls the sub-agent for summarization per item
	•	And calls the sub-agent for newsletter editing once per week.

⸻

Feature F4: Newsletter Rendering
User Story F4.1 – Render HTML and text variants
	•	As a subscriber
	•	I want a visually clean HTML email, and a text alternative
	•	So that I can read the digest from any client.

Success Criteria
	•	Scenario F4.1.1 – HTML rendering
	•	Given a valid newsletter spec (intro, sections, closing_note)
	•	When the rendering step runs
	•	Then the system produces an HTML string containing:
	•	Title (Last Week in AI – <week_label>)
	•	Intro paragraph
	•	One <h2> per section and <li> per item
	•	“Read more” links pointing to the original URLs.
	•	Scenario F4.1.2 – Text rendering
	•	Given the same newsletter spec
	•	When the rendering step runs
	•	Then the system produces a plaintext string
	•	And the plaintext preserves sections and includes URLs in readable form.

⸻

Feature F5: Subscriber Management
User Story F5.1 – Load subscribers
	•	As an operator
	•	I want subscribers stored in subscribers.json
	•	So that I can manage subscribers outside of code.

Success Criteria
	•	Scenario F5.1.1 – Valid subscribers file
	•	Given a subscribers.json file with valid emails
	•	When the email sending step starts
	•	Then the system loads all subscribers
	•	And logs the count of loaded subscribers.
	•	Scenario F5.1.2 – Invalid email entry
	•	Given subscribers.json contains an invalid email address
	•	When the email sending step starts
	•	Then the system skips the invalid entry
	•	And logs a warning with the problematic record.

⸻

Feature F6: Email Delivery via SMTP
User Story F6.1 – Send newsletter to all subscribers
	•	As a subscriber
	•	I want to receive the digest once a week
	•	So that I stay updated on AI news.

Success Criteria
	•	Scenario F6.1.1 – Successful send
	•	Given the SMTP server is reachable
	•	And the newsletter HTML and text are generated
	•	When the sending step runs
	•	Then an email is sent to each valid subscriber address
	•	And the system logs success for each email (or a summary).
	•	Scenario F6.1.2 – SMTP server failure
	•	Given the SMTP server is unreachable
	•	When the sending step runs
	•	Then the system logs an error and marks the run as failed
	•	And no further retries are performed unless explicitly configured.

User Story F6.2 – Idempotent re-send
	•	As an operator
	•	I want to be able to re-run the weekly send step without duplicating content
	•	So that I can recover from failures safely.

Success Criteria
	•	Scenario F6.2.1 – Re-run without duplication (optional if you track runs)
	•	Given the system keeps a record of sent newsletters by week
	•	When the sending step is triggered again for the same week
	•	Then the system either prevents the second send or requires a force flag.

⸻

Feature F7: Monitoring & Logging
User Story F7.1 – Log pipeline execution
	•	As an operator
	•	I want logs for each major step (ingestion, summarization, rendering, sending)
	•	So that I can debug failures and monitor health.

Success Criteria
	•	Scenario F7.1.1 – Structured logs
	•	Given the weekly job runs
	•	When the pipeline executes
	•	Then logs include start/end timestamps for each stage
	•	And counts for items ingested, summarized, included in newsletter, and emails sent.

User Story F7.2 – Run status
	•	As an operator
	•	I want a clear indication if a weekly run succeeded or failed
	•	So that I can take corrective action.

Success Criteria
	•	Scenario F7.2.1 – Run result flag
	•	Given all pipeline stages complete without unhandled exceptions
	•	When the job finishes
	•	Then the system marks the run as SUCCESS in logs (and optionally a status file).
	•	Given any stage fails
	•	Then the run is marked FAILED
	•	And the failure reason is logged.

⸻

Feature F8: RAG & Archive (Phase 2 – Optional)
(For planning, even if not implemented immediately.)

User Story F8.1 – Index content for semantic search
	•	As a power user
	•	I want to query past articles and newsletters
	•	So that I can research themes and trends.

Success Criteria (later)
	•	Scenario F8.1.1 – Index new content
	•	Given new items and summaries are produced
	•	When the RAG indexing job runs
	•	Then embeddings are stored in the vector DB with metadata (source, week, tags).
	•	Scenario F8.1.2 – RAG retrieval
	•	Given a user query “agentic AI frameworks last month”
	•	When the search endpoint is invoked
	•	Then relevant items from the last month are returned with titles and links.

⸻

3. High-level success metrics (beyond pass/fail tests)

Not for BDD directly, but useful for acceptance:
	•	At least 90% of weeks: newsletter generated and sent without manual intervention.
	•	Average number of items per newsletter within configured range (e.g. 8–12).
	•	Pipeline execution < 15 min for typical weekly volume.
	•	Error rate (ingestion or sending failures) < 5% of runs over 3 months.

This BRD + feature/user-story breakdown should be enough to:
	•	Define functional test cases for each feature.
	•	Write BDD scenarios in Gherkin (Feature: Weekly AI Digest, Scenario: Ingest RSS feed, etc.).
	•	Implement automated tests that validate behavior end-to-end and per component.