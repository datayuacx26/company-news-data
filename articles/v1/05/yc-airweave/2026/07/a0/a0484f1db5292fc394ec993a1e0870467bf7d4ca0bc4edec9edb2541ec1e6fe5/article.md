---
schema_version: "1.0.0"
document_id: "a0484f1db5292fc394ec993a1e0870467bf7d4ca0bc4edec9edb2541ec1e6fe5"
company_key: "yc-airweave"
company: "Airweave"
source_id: "yc-airweave-news-import-4b0eb08abbcd"
canonical_url: "https://airweave.ai/blog/how-we-use-airweave-to-improve-airweave"
published_at: null
first_seen_at: "2026-07-24T14:56:51.264588+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:39fcd659b1bddc0f42ef43741ea36245fda674506b26785be7ca7b3eef6a7455"
---

# How we use Airweave to improve Airweave

We're big believers in dogfooding your own software. As our usage grew, so did the volume of error logs to monitor. Our small team of 5 struggled to keep up while maintaining the same quality of support and speed of issue resolution.


In the spirit of pre-PMF frugality, we didn't want to hire this problem away. We needed a way to efficiently scale the number of issues we could track and resolve. That's when we realized: **we were building the exact tool we needed.**


#### Enter Donke: Our Intelligent Error Monitoring Agent


We built Donke, an LLM-powered agent that runs every 30 minutes and uses Airweave to transform raw error logs into actionable, enriched alerts. Here's how it works:


**1. Error Detection & Clustering**


Donke fetches errors from Azure Log Analytics and uses Claude to semantically cluster similar errors together. A flood of 500 raw logs might reduce to just 10 distinct issues worth investigating.


**2. Code Context via Airweave**


For each error, Donke queries Airweave to search our synced GitHub codebase. It finds the exact modules and files where the error originated, complete with line numbers, so we can dive straight into the relevant code.


**3. Linear Search via Airweave**


Before creating a new ticket, Donke uses Airweave to semantically search our synced Linear workspace. If a related issue already exists, it links to it instead of creating duplicates.


**4. Enriched Slack Alerts**


Everything comes together in a single Slack message: affected organizations, severity assessment, code context with clickable file links, Linear ticket status, and mute controls.


#### The Results


This has enabled us to **10x the volume and quality of improvements we push** . We often resolve problems before customers even realize they occurred, then proactively reach out to let them know an issue they never experienced has already been fixed.


But most importantly, building Donke forced us to test the limits of our own software. We stepped into our users' shoes and battle-tested our solution at scale. **Donke now handles ~40,000 Airweave queries per month.**
