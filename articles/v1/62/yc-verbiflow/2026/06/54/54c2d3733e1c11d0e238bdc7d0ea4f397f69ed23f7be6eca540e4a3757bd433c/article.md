---
schema_version: "1.0.0"
document_id: "54c2d3733e1c11d0e238bdc7d0ea4f397f69ed23f7be6eca540e4a3757bd433c"
company_key: "yc-verbiflow"
company: "Verbiflow"
source_id: "yc-verbiflow-news-import-392b5e1f9020"
canonical_url: "https://verbiflow.com/blog/gtm-as-code"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-22T18:39:30.762214+00:00"
fetched_at: "2026-07-28T21:44:22.050751+00:00"
content_hash: "sha256:c67c8cae02e1c88dfb584cef5229ed0401a7259933efbe99029c851080f5df3b"
---

# GTM as code: stop rebuilding outbound lists by hand

The old way to build an outbound list: open Apollo, build a filter, export CSV, dedupe in a spreadsheet, paste into a sequencer. The whole loop was clicks. The growth teams we work with in 2026 are doing it differently. They define the audience once, run it as a play, and let Claude Code adjust it the next time the market moves.


## What a play is


A play on Verbiflow is a short, repeatable pipeline that builds an audience and pushes it into a sequence. It has four parts:


- **A config.** One JSON file declaring the schema, the columns, and the views.
- **Sources you bring.** Local scripts that pull from wherever your data lives: Clay, Apollo, Apify actors, SERP, your own scraper, a Chrome session in your own browser. We don’t own the data layer; we make it easy to connect.
- **A pipeline.** A shell script that runs the steps in order. It can stop, restart, and run again, with all state in SQLite.
- **An output.** A clean audience, pushed into a Verbiflow sequence where the sending happens: connected mailboxes, deliverability, email and LinkedIn replies, CRM sync, and reporting.


The one-liner


Scaffold a new play from a template:` verbiflow-cli init` . Edit the sources. Run the pipeline.


## Why this matters at a growth-stage company


Most growth-stage companies don’t have a data team. The person running outbound is usually the only one who knows where the lists come from and how to refresh them. When that work lives in spreadsheets and exported CSVs, the list is stale in 30 days, and it’s impossible to hand off because the logic only exists in someone’s head.


With plays, the audience logic and run state live in code. Re-run the play next quarter and you get a fresh list with the same logic. A new GTM lead doesn’t need to read it by hand. They can ask Claude Code what the play does, change the filters, and rerun it from the saved workflow state.


## Plays we’ve shipped


- **SaaS by category.** Pulled 1,600 SaaS companies from a funding database and used Claude to analyze each homepage, then sorted by what each company actually sells. 4-step SQLite pipeline.
- **Regional security firms.** Used Google Maps and Census data to find every physical-security company in 1,935 US cities, classified them with Claude, fetched reviews, surfaced negative reviewers. 7-step pipeline, restartable.
- **YC trust atlas.** Analyzed every YC company’s trust and security page to find which ones were missing certifications. The customer sequenced the gaps first.
- **Company intelligence signals.** 16 dimensions of company research (funding, customers, hiring, public signals, compliance posture) on every account in your CSV. You source it once and query it however you want.


Each one is a pipeline you re-run when the market moves, not a static file you re-export.


## What changes when Claude Code can edit the play


The Verbiflow SDK gives Claude Code the scaffolding to work on a play instead of guessing from scratch. Ask it to add a new source. Ask it to filter by a new signal. Ask it to swap the LLM analysis prompt. It edits the workflow code, runs the play, and shows you the changed rows or artifacts before anything gets pushed into a sequence.


Customers extend plays this way every week. A change that used to be a half-day of Python is now a short conversation with Claude Code.


> The fastest GTM teams in 2026 are the ones whose lists are code, and whose plays can be adjusted through Claude Code.


## How this fits with sequencing


The play does the audience-building work, using sources you bring. Verbiflow’s core platform handles everything after launch: sequences, connected mailboxes, deliverability, email and LinkedIn replies, CRM sync, and reporting. The two connect with a push into a sequence, or a CSV when you want to review the list first.


We’re the place all your data lands and turns into sent outbound. We don’t compete with the tools you already use to build lists. We give them somewhere clean to land.


## If you’re the GTM lead at a growing company


Ask yourself: can the next person re-run your outbound work without asking you for the exact filter? If they have to ask you, you’re in spreadsheet territory. If they can ask Claude Code to explain and re-run the play, you’re in code territory.


Verbiflow is built around the second answer.
