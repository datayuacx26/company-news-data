---
schema_version: "1.0.0"
document_id: "4409fab96c5a93f04fa54615d96ef3c4e0d78c02fb51b864ecaa26246fc352c2"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/thrive-dynamic-table-costs-custom-incremental"
published_at: "2026-07-28T23:09:00+00:00"
first_seen_at: "2026-07-28T16:56:15.209140+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:c57b23324e3b9b3c66d28904102feef7199b32d9fab429859e7df9397385d2bd"
---

# Custom Incrementalization for Dynamic Tables | Snowflake

*This post was written by the data engineering team at Thrive Learning. The views and experiences expressed are their own.*


Before Snowflake, Thrive's legacy analytics ran on a mix of scheduled batch jobs and MongoDB change streams triggering refreshes on a schedule. When the team moved to Snowflake, they built on Dynamic Tables from the start, driven by a core requirement to sync and transform platform data continuously as changes arrived, rather than in delayed batches.


That freshness isn't a back-office nicety; it's part of the product. Thrive's Analyse suite (the ability for customers to explore all their learning data) is a core part of what the Thrive platform offers. We've also noticed that better performance and data freshness translate directly into customer satisfaction and competitive strength in sales bids. Customers don't buy analytics as a separate line item, but its quality directly influences whether Thrive wins and retains business. More recently, that same data and Dynamic Table foundation underpins Thrive's first paid add-on product, Analyse with AI, which layers AI on top of the data pipelines at hand.


In this post, we'll share how we improved price-performance for a Dynamic Table using new custom incrementalization capabilities. Rob Howe, a senior data engineer at Thrive, led this work. He identified that custom incrementalization was a great fit for the challenges we were facing and came up with this solution, tested it and got it deployed to production within a couple of weeks.


#### Thrive Learning snapshot


Thrive is a UK-based enterprise learning platform (LMS/LXP) serving 500+ B2B tenants across manufacturing, aviation, retail and more. Thrive's data and AI engineering team runs its analytics and AI product stack on Snowflake, using Dynamic Tables extensively to turn raw operational data into the models that power customer-facing reporting and Thrive's AI assistant.


## Deduplication complexity increases costs


One of Thrive's core pipelines, internally called RecordStore, began life as exactly the kind of thing Dynamic Tables are made for: a simple, auto-refreshing, incremental Dynamic Table with no meaningful extra cost.


The trouble started when the team needed to enrich it with additional keys. Certain IDs weren't dependably placed upstream, and so resolving them meant joining across multiple sources (up to five, later optimized down to three). Those joins produced duplicates, which meant adding deduplication, and that combined complexity was enough that Snowflake could no longer refresh the table incrementally. It fell back to full refreshes.


To bring transformation cost back down, the team split the data into two lanes: a "fast lane" of recent data refreshed every six hours, and a "slow lane" of older data refreshed weekly. That did cut transformation cost, but the final` UNION` of the two lanes pushed auto-clustering cost sharply the other way. The team was now trading one cost for another. At its worst, this pipeline was running at hundreds of credits per day between auto-clustering and transformation costs.


Every mitigation was treating a symptom. The real root cause was the unreliable Content ID upstream, and until that was fixed, the team was stuck.


## Adding custom incrementalization to Dynamic Tables


The pattern the team was looking at was Snowflake streams and tasks: process only the rows that actually changed, and apply them with merge logic. A streams-and-tasks pipeline would have handled the complexity spike well, but as Thrive's engineers noted, it would also have made it harder to control overall refresh lag once they wanted to get more ambitious with freshness.


Custom incrementalization for Dynamic Tables (` REFRESH_MODE = CUSTOM_INCREMENTAL` ) removed that trade-off entirely. The team wrote explicit` REFRESH USING` logic with` MERGE INTO SELF` over` CHANGES()` , defining precisely what "incremental" means for this pipeline: process only the delta, merge it in, leave everything else untouched. They got the streams-and-tasks execution model they wanted, but with Snowflake still managing scheduling, retries, transactional guarantees and lag.


Two things followed:


1. **Transformation stopped reprocessing everything:** Each refresh now touches only the changed rows instead of full-refreshing around the join/dedup complexity.
2. **Auto-clustering stopped churning:** With no full rebuilds and no fast/slow UNION to maintain, clustering settled to near-nothing.


“Custom incrementalization gave us the execution model we wanted, with Snowflake still owning scheduling, retries and lag. And because the logic remained in the dbt DAG, the pipeline kept its place in the lineage graph with no separate orchestration layer to maintain.”


##### Alex Tasioulis


Head of Data & AI Engineering at Thrive


### Reduced both transformation and auto-clustering costs


Cost driver Before After Reduction


Auto-clustering ~150 credits/day ~2 credits/day ~99%


Transformation compute ~150 credits/day ~5 credits/day ~97%


*Note: Results based on Thrive’s internal production measurements.*


We saw refresh latency for the pipeline also dropped from hours to seconds, because each run only inspects the sources that actually changed. The pipeline now scales with the *rate of change* in the data rather than its total size.


Custom incrementalization let Thrive stop trading transformation cost against clustering cost and instead solve the refresh behavior directly, while keeping the lag control that a hand-rolled streams-and-tasks pipeline would have sacrificed. It's a pattern the team expects to reuse anywhere a transformation can't be expressed cleanly as an incremental SELECT.


## Lineage intact in dbt


There was a second reason custom incrementalization beat the stream-and-task alternative for Thrive: governance. Thrive manages its transformations in dbt. Knowing exactly which models feed which is central to how the team reasons about, tests and documents the platform.


Streams and tasks live outside dbt. Moving this logic into a hand-built streams-and-tasks pipeline would have pulled those objects out of the dbt DAG entirely, breaking lineage and forcing the team to manage and monitor them as a separate, off-to-the-side system.


Custom incremental Dynamic Tables are still Dynamic Tables. Even though native dbt support for custom incrementalization isn't there yet, the team wrapped them in a custom dbt materialization so they remain top-notch nodes in the dbt project. The pipeline keeps its place in the lineage graph and is managed with the same workflow, tests and docs as everything else, with no separate orchestration layer to maintain.


## Cheaper, fresher data for the agentic AI use cases ahead


The team is now applying the same pattern to other pipelines that hit similar join/dedup complexity, and expects comparable wins — lower cost and fresher data across Thrive's entire analytics estate. Beyond the direct savings, Thrive already has agentic AI consuming this data, and making it cheaper and near-live means that same data can support even more agentic AI use cases, putting the Thrive platform in a strong position to expand its AI capabilities further.
