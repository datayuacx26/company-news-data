---
schema_version: "1.0.0"
document_id: "fa68e4d1a01047d4e4db4b6fbe96e5298e7f59d48f2db110bdc64cca1bbe4b6b"
company_key: "yc-verbiflow"
company: "Verbiflow"
source_id: "yc-verbiflow-news-import-392b5e1f9020"
canonical_url: "https://verbiflow.com/blog/1600-saas-companies-sorted-by-what-they-sell"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-22T18:39:30.762214+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:909aba4932a486890494e52584317ce3e0cf2329126049cf68b93e76a192cc65"
---

# We pulled 1,600 SaaS companies from a funding DB and sorted them by what they actually sell

Lists from funding databases are noisy. “B2B SaaS” covers everything from devtools to compliance to vertical CRMs. So when a customer asked us to find SaaS companies that fit their very specific ICP, we didn’t buy a list. We built a play that read the homepages.


1,600


SaaS companies pulled from a funding database


4 stages


seed → crawl → extract → enrich


1 SQLite DB


resumable, restartable, no orchestration layer


## The problem with buying the list


The customer was selling into B2B SaaS, but only a specific slice: companies whose product itself touches developer or infrastructure workflows. A standard funding database returned 1,600 “B2B SaaS” companies. Maybe 200 of them were a real fit. The other 1,400 would just drag down every sequence they got dropped into.


Manual qualification at that scale isn’t worth it. So we built a four-step pipeline that did the qualification automatically and let the customer sequence only the survivors.


## The pipeline


- **Seed.** Pull 1,600 companies from the funding DB into a SQLite table. Every row has a name, a domain, and a funding stage.
- **Crawl.** Visit each company’s homepage, customer page, and case studies with Playwright. Extract clean markdown.
- **Extract.** Hand the markdown to Claude with a structured-output prompt: who are this company’s customers, what does the product do, what category does it fit. Write the answer back to the DB.
- **Enrich.** For each extracted customer, look up funding stage via SERP. Roll that back up to rank the parent company by ICP fit.


Why SQLite


Every stage reads and writes the same DB. Kill any step mid-run, re-run it, and it picks up where it left off. No orchestration framework, no queue, no temporal worker. The pipeline is four Python scripts and a database.


## What the customer actually got


Three ranked CSVs, exported from the DB:


- **Top 100 by ICP fit.** Companies whose own customers were the customer’s exact buyer profile.
- **Series A targets.** Subset sized by funding stage, where the buying committee is small and the deal cycle is short.
- **Series B and below.** The broader long-tail to sequence at lower priority.


The customer loaded the top 100 into Verbiflow as the audience for a new sequence, and let the lower tiers feed into a slower-cadence track. Sequencing started the next day.


## How this differs from buying a list


The ranking is based on what the company actually does today, not what someone tagged it as five years ago. Reading the homepage gets you a fresher signal than any vendor’s industry taxonomy. And the pipeline is re-runnable. Six months later, when the customer wants the same view but with new entrants, you re-run the four scripts and the DB updates itself.


> A good list is something you build once and re-run, not something you buy and watch decay.


## The bigger pattern


This is one of the “plays” we build with customers. Other plays in the same family: every regional security firm in the US sorted by Google review sentiment; every YC company’s trust page read for compliance gaps; every B2B SaaS site with a careers page that mentions “Series A.”


They all work the same way. Public data, structured into a sequenceable audience, then run through Verbiflow’s outbound stack. The play handles the qualification and the platform handles the sending, with a CSV or an API call between them.
