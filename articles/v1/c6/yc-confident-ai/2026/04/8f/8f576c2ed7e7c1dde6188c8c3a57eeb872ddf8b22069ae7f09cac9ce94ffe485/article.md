---
schema_version: "1.0.0"
document_id: "8f576c2ed7e7c1dde6188c8c3a57eeb872ddf8b22069ae7f09cac9ce94ffe485"
company_key: "yc-confident-ai"
company: "Confident AI"
source_id: "yc-confident-ai-news-import-3974c9e901d3"
canonical_url: "https://www.confident-ai.com/blog/launch-week-q1-2026-day-5-dataset-generation"
published_at: "2026-04-04T00:00:00+00:00"
first_seen_at: "2026-07-21T14:44:45.986766+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:d6fb2b7bc7702653d98f74164126f3b863774ccb30d08947efc74cce8dc74a22"
---

# Launch Week Day 5 (5/5): Generate Datasets from Your Data Sources

Blog


# Launch Week Day 5 (5/5): Generate Datasets from Your Data Sources


Apr 4, 2026


·


4 min read


Jeffrey Ip


Co-founder @ Confident AI. Creator of DeepEval & DeepTeam. Building an unhealthy LLM evals addiction. Ex-Googler (YouTube), Microsoft AI (Office365).


Welcome to the final day of Confident AI's Launch Week. We've spent the week shipping[error analysis](https://www.confident-ai.com/blog/launch-week-q1-2026-day-1-error-analysis) ,[scheduled evals](https://www.confident-ai.com/blog/launch-week-q1-2026-day-2-scheduled-evals) ,[auto-ingest](https://www.confident-ai.com/blog/launch-week-q1-2026-day-3-auto-ingest-traces) , and[auto-categorization](https://www.confident-ai.com/blog/launch-week-q1-2026-day-4-trace-categorization) — and today we're closing out with the feature teams have been asking us for more than anything else.


**Launch Week Day 5 (5/5): Generate evaluation datasets directly from your data sources — Google Drive, SharePoint, Notion, S3, and more.**


## The Dataset Problem Nobody Wants to Admit


Here's the dirty secret of LLM evaluation: most teams are evaluating on datasets they made up.


Not "made up" in the malicious sense — but hand-crafted. Someone on the team sat down, wrote 30–50 question-answer pairs based on what they *thought* users would ask, and called it a golden dataset. Maybe they got a PM to review it. Maybe they didn't. Either way, the dataset reflects the team's imagination, not reality.


And the problem compounds. Your RAG pipeline retrieves from a knowledge base that has *thousands* of documents — product specs, HR policies, legal contracts, support playbooks, API docs — but your eval dataset covers maybe 2% of that surface area. You're testing the tip of the iceberg and assuming the rest is fine.


The result? Your evals pass. Your agent ships. And then a user asks about a niche policy buried in page 47 of a SharePoint doc, your model hallucinates confidently, and you're scrambling to figure out what went wrong.


## Why Building Good Datasets is So Hard


Teams that *do* try to build comprehensive eval datasets hit the same wall:


1. **The knowledge base is massive.** You've got hundreds or thousands of documents across multiple sources. No one person understands all of it. Manually writing questions that cover the full breadth is unrealistic.
2. **The sources are scattered.** Some docs live in Google Drive. Others in SharePoint. Some in Notion. A few in S3 buckets. Just *gathering* the source material into one place is a project in itself.
3. **Writing good QA pairs is slow.** Even when you have the documents in front of you, crafting high-quality questions — the kind that actually stress-test your retrieval and generation pipeline — takes real expertise and real time. A skilled engineer might produce 20–30 good pairs in a day.
4. **Datasets go stale.** Your knowledge base updates constantly. New policies, new product features, new documentation. But the eval dataset that was built three months ago? Still frozen in time. You're evaluating against a version of reality that no longer exists.
5. **No one owns it.** Dataset creation falls into the gap between engineering, product, and domain experts. Everyone agrees it should be better. No one has the bandwidth to make it happen.


The net effect is that most teams are flying blind — evaluating their AI agents against datasets that are too small, too narrow, and too outdated to catch real failures.


## Dataset Generation on Confident AI


Dataset generation solves this by going straight to the source. Instead of asking humans to write eval data from scratch, you connect your actual data sources and let Confident AI generate evaluation-ready datasets from them.


Here's how it works:


1. **Connect your data sources.** Google Drive, SharePoint, Notion, S3, Azure Blob Storage, Confluence — connect wherever your documents live. We handle auth, pagination, and format parsing.
2. **Select what to generate from.** Pick specific folders, files, or collections. Filter by file type, last modified date, or tags. You control exactly which documents feed into your dataset.
3. **Configure generation settings.** Choose the types of QA pairs you want — factual recall, multi-hop reasoning, edge cases, adversarial queries. Set the volume and complexity level.
4. **Generate.** Confident AI reads your documents, understands the content, and synthesizes evaluation-ready question-answer pairs with proper context references. Every generated row traces back to the source document it came from.
5. **Review and refine.** The generated dataset lands in Confident AI ready for review. Edit, filter, or regenerate specific rows. Then use it immediately for evals — or feed it into Scheduled Evals from Day 2.


That's the full loop. Your documents become your eval dataset — automatically, continuously, and with full traceability.


## A Concrete Example


Let's say you're building an internal HR assistant. Your knowledge base spans:


- **Google Drive:** 120+ policy documents (leave policies, benefits guides, onboarding checklists)
- **SharePoint:** compliance docs, org charts, role descriptions
- **Confluence:** engineering onboarding wiki, internal tooling guides


You connect all three sources on Confident AI and generate a dataset. Here's what you get back:


Source


Generated QA Pairs


Category


Google Drive — Leave Policies


85


Benefits & leave


Google Drive — Benefits Guide


62


Benefits & leave


SharePoint — Compliance Docs


71


Compliance & legal


SharePoint — Role Descriptions


48


Org & roles


Confluence — Eng Onboarding


93


Engineering


That's 359 QA pairs covering the full breadth of your knowledge base — generated in minutes, not weeks. Each pair includes the source document reference, the expected answer, and the retrieval context your RAG pipeline should find.


Now run evals with that dataset. Suddenly you're testing whether your HR assistant can handle questions about *parental leave eligibility for part-time employees in the UK office* — not just the five generic HR questions someone on your team wrote last quarter.


## Why This Matters


**Coverage you can't get manually.** No team has the bandwidth to write evaluation data that covers every document in their knowledge base. Dataset generation gives you breadth that manual curation never will.


**Datasets that stay fresh.** When your knowledge base updates, regenerate. New policy doc added to SharePoint? New product spec pushed to Confluence? Your eval dataset catches up in minutes — not months.


**Traceability from eval to source.** Every generated QA pair links back to the document it came from. When an eval fails, you don't just know *what* failed — you know *which document* the failure traces to, and you can go fix the retrieval, the chunking, or the prompt.


**Connect the full launch week loop.** Generate a dataset from your data sources (Day 5). Schedule evals to run on it weekly (Day 2). Auto-ingest traces from production to compare real traffic against your generated expectations (Day 3). Categorize the failures (Day 4). Run error analysis on the worst categories (Day 1). That's the entire evaluation lifecycle — connected end to end.


## Wrapping Up Launch Week


That's all five days. Here's the full recap:


- **Day 1:**[Automated Error Analysis](https://www.confident-ai.com/blog/launch-week-q1-2026-day-1-error-analysis) — annotate traces, get metric recommendations with alignment rates.
- **Day 2:**[Scheduled Evals](https://www.confident-ai.com/blog/launch-week-q1-2026-day-2-scheduled-evals) — run evals on a cadence so you never forget.
- **Day 3:**[Auto-Ingest Traces](https://www.confident-ai.com/blog/launch-week-q1-2026-day-3-auto-ingest-traces) — turn production traffic into datasets and annotation queues, continuously.
- **Day 4:**[Auto-Categorize Traces & Threads](https://www.confident-ai.com/blog/launch-week-q1-2026-day-4-trace-categorization) — understand what your users are asking and where your agent struggles.
- **Day 5:** Generate Datasets from Data Sources — turn your existing documents into evaluation-ready datasets, automatically.


Every feature this week was built around the same principle: the workflows that teams need to run reliable AI agents in production shouldn't require duct-taped scripts and manual processes. They should be infrastructure.


If you want to turn your knowledge base into an evaluation dataset today,[sign up for Confident AI](https://app.confident-ai.com/) and connect your first data source.


---


Do you want to brainstorm how to evaluate your LLM (application)? Ask us anything in our[discord](https://discord.com/invite/a3K9c8GRGt) . I might give you an "aha!" moment, who knows?


## Standardize AI Quality for the entire org, not just individual teams


Give all AI use cases the same quality bar with all-in-one evals, observability, and red teaming, and enforce them at scale.


AI evals for product teams, not just engineers.


Observability for production traffic.


Red teaming for security and safety.


AI governance for multiple projects at once.


[Book a Demo](https://www.confident-ai.com/book-a-demo?utm_source=blog&utm_medium=content&utm_campaign=q1_2026_launch_week&utm_content=product)[Or sign up](https://app.confident-ai.com/auth/signup?utm_source=blog&utm_medium=content&utm_campaign=q1_2026_launch_week&utm_content=product)


In this story


- The Dataset Problem Nobody Wants to Admit
- Why Building Good Datasets is So Hard
- Dataset Generation on Confident AI
- A Concrete Example
- Why This Matters
- Wrapping Up Launch Week
