---
schema_version: "1.0.0"
document_id: "e2a097a43feeaa7f5256c48edb2eb8cae97b26e853db29af5c20e969ee9ffc47"
company_key: "yc-mindsdb"
company: "MindsDB"
source_id: "yc-mindsdb-news-import-209cdb193474"
canonical_url: "https://mindshub.ai/blog/mindsdb-product-updates-april-2026"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-22T04:26:30.664731+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:6d87fa5f7bcd2010ad04f90115e6130ff57c0e808303600e540cc59692aea78b"
---

# MindsDB Product Updates: April 2026

April was a busy month on both sides of the product. Anton picked up several upgrades that make analysis easier to share, audit, and continue across sessions. Underneath, MindsDB Query Engine v26.1 shipped with broader integration coverage, steadier SQL behavior, and Knowledge Base fixes.


Here’s what’s new and why you might care.


## What’s new in Anton


### ` /publish` - share a report with one command


Type` /publish` in Anton and your current report, analysis, or output goes live as an HTML page on` anton.mindsdb.com` . Run it again on the same work and Anton updates the existing page rather than creating a duplicate.` /unpublish` takes it back down.


The point: analysis shouldn’t stay trapped in a terminal. Now you can move from answer to shareable artifact without copy-pasting into a doc.


### Explainability - see how Anton got the answer


Anton now records the trail behind each answer: which data sources it touched, the SQL it ran, the scratchpad steps it executed, and a plain-language summary of its reasoning.


If a decision is going to ride on the result, you can inspect the work first.


### Stateful scratchpads - multi-step analysis that holds together


Scratchpads now keep state across cells in a session. Variables, imports, and intermediate results carry forward, so Anton can build on previous steps the way an analyst naturally would - without re-running setup or repeating itself.


### ` /memory` - full control over what Anton remembers


Anton’s memory is now inspectable and editable from the CLI:


Command What it does


/memory rules Behavioral rules Anton follows. View, edit, or delete entries.


/memory lessons Lessons Anton has picked up from past errors.


/memory identity Anton’s understanding of who you are and how you work.


/memory episodes Full session history for reviewing prior work.


/memory vacuum Deduplicate and compact memory.


/memory reset Wipe a specific memory scope cleanly.


Three memory modes ship alongside the commands:


-


**Autopilot** - Anton decides what to remember.


-


**Co-pilot** - Anton checks with you on ambiguous saves.


-


**Off** - memory is disabled.


The goal here is honest personalization. Anton can learn from past work, but you can always look at - and change - what it’s learned.


### Smarter` /connect` - paste a connection string and go


The data connection flow has been redesigned. Anton parses pasted connection strings, uses AI to fill in non-credential fields, saves credentials after a successful connection, and retries intelligently when something’s wrong.


Connecting a database now feels closer to a quick conversation than filling out a form.


## What’s new in Query Engine v26.1


` v26.1.0rc1` shipped April 17 and` v26.1.0` followed on April 23. The release was about reliability, integration coverage, Knowledge Base behavior, and the kind of stability work that doesn’t show up in screenshots but matters in production.


### SQL behavior, tightened


Several SQL paths got steadier this release: expanded MySQL API test coverage, better MSSQL query result conversion, improved schema scoping for MySQL column discovery, and more reliable parsing of BigQuery select responses.


These are small individually. They compound into more predictable agent behavior - fewer surprises when an LLM-generated query hits a real database.


### Broader integrations


New or improved this month: HubSpot (including Leads), Raindrop.io, Denodo, Freshdesk, additional GitLab configuration, MotherDuck support inside DuckDB, Azure provider fixes, and clearer boot error messages for handlers.


The throughline is reach. The more systems MindsDB connects to cleanly, the less custom glue code you write.


### Knowledge Base fixes


Two notable fixes: Knowledge Base creation now works reliably when provider configurations aren’t explicitly set in non-OpenAI scenarios, and integration lookup is now optimized by fetching directly from the database.


If you’re using Knowledge Bases to unify structured and unstructured data for retrieval, both of these should be felt as “things just work now.”


### Stability, streaming, and docs


Also in the release: improvements to error and crash handling, duplicate column handling, PID file creation, API handler filters, mixed search optimization, FAISS IVF behavior, and data-handler streaming. Documentation got refreshes across environment variables, Datastax, the GitLab handler, model-creation wording, and the home page.


The unglamorous work that makes a platform actually nice to run.


## Try the April updates


-


**Anton** - for shareable, explainable analysis workflows


-


**Query Engine v26.1** - for the data layer underneath


-


[Join the community](https://mindshub.ai/joincommunity) - for future updates and to send us feedback when something doesn’t feel right
