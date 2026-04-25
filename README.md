# research-agent

A weekly AI newsletter agent. Monitors a curated set of sources (RSS feeds, websites, YouTube), synthesises the week's most relevant content using an LLM, and distributes a concise HTML + plaintext digest to subscribers via SMTP.

## Overview

```
sources.json → Ingestion & Normalisation → Content Store → Agentic Summarisation → Critic Layer → Newsletter Render → SMTP
```

### Multi-agent pipeline

| Agent | Role |
|---|---|
| `WeeklyDigestOrchestrator` | Plans and sequences the pipeline |
| `ItemSummarizerAgent` | Summarises individual items |
| `TopicClassifierAgent` | Tags items by topic |
| `NewsletterEditorAgent` | Curates the final digest |

Critic agents (factual consistency, tone & safety, hype/commercial bias, privacy) validate every LLM output before it ships.

## Configuration

| File | Purpose |
|---|---|
| `sources.json` | RSS feeds, websites, and YouTube links to monitor |
| `subscribers.json` | Mailing list |
| `prompt_config.yaml` | LLM tone, safety rules, and volume per environment (`alpha` / `beta` / `prod`) |
| `prompts/` | Versioned prompt templates |

## Requirements

- Python 3.11+
- Gemini API key (LLM provider)
- SMTP server (outbound mail)
- Optional: YouTube Data API key for transcript fetching

## Setup

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp .env.template .env
# Fill in GEMINI_API_KEY, SMTP_*, and optionally YOUTUBE_API_KEY
```

## Running

```bash
# Dry run (no email sent)
python run.py --dry-run

# Full weekly run
python run.py
```

Artefacts are written to a `runs/YYYY-MM-DD/` folder: `raw_items.json`, `summaries.json`, `newsletter_spec.json`, `critics.json`.

## Phase 2 (planned)

RAG over the historical run corpus, per-subscriber personalisation, and archive search.

## Contact

[jb@jishutech.io](mailto:jb@jishutech.io) · [jishutech.io](https://jishutech.io)
