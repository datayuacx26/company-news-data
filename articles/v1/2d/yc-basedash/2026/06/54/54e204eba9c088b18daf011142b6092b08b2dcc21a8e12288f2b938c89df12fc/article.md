---
schema_version: "1.0.0"
document_id: "54e204eba9c088b18daf011142b6092b08b2dcc21a8e12288f2b938c89df12fc"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-bi-tools-combine-data-from-multiple-sources-federation-blending-and-warehouses/"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:063e180ab4b970f1b684ab8c2ec1951e6b13002f396c9f9371790a9871d81bd1"
---

# How BI tools combine data from multiple sources: federation, blending, and warehouses

When your data lives in more than one place, a Postgres production database, a Stripe export, a Snowflake warehouse, a marketing tool, there are three ways a BI tool can combine it: query each source live and federate the results at query time, blend results at the visualization layer after querying each source separately, or centralize everything in one warehouse first and query that. Most teams end up centralizing in a warehouse for anything recurring, and use federation or blending for one-off or low-volume cases. This guide explains how each approach works, the tradeoffs, and how to decide.


It is written for analysts, founders, and data leads who need a dashboard that joins data sitting in different databases or tools, and who want to understand what is actually happening under the hood before picking a tool or an architecture.


## TL;DR


- **Centralize in a warehouse (ELT)** is the default for anything you will run more than a few times. Pipe sources into Snowflake, BigQuery, or Redshift, model the joins once, and point the BI tool at one place.
- **Query federation** runs a single query across multiple live sources at query time using an engine like Trino, or a warehouse’s federated-query feature. Good for fresh data and avoiding a pipeline, weaker on performance and governance.
- **Data blending** queries each source separately, then joins the aggregated results at the visualization layer. This is how Tableau combines disparate sources. It is convenient but limited to aggregate-level joins.
- The right choice depends on how often you run it, how much data moves, how fresh it must be, and whether you need row-level joins or just aggregates side by side.
- The expensive mistake is wiring brittle live cross-source joins into a daily dashboard that should have been a modeled warehouse table.


## What “combining data from multiple sources” actually means


“Combine” hides two very different operations, and conflating them causes most of the confusion.


The first is a **true join** : matching rows from one source to rows in another on a key, like attaching the Stripe plan to each user row from your production database. This needs both datasets at row level and a shared key.


The second is **putting metrics side by side** : showing signups from Postgres next to ad spend from a marketing tool on the same dashboard, aligned by date, without joining at the row level. This only needs aggregates that share a common dimension such as day or campaign.


Different approaches are good at different ones. Centralizing and federation can do real row-level joins. Blending is mostly the second case: aligning aggregates on a shared dimension. Knowing which one you need narrows the options immediately.


## The three approaches


### Centralize in a warehouse first (ELT)


Move each source into one cloud data warehouse on a schedule, then do all joining and modeling there. Tools like Fivetran, Airbyte, or Stitch handle extract and load; dbt or SQL views handle the transforms. The BI tool then connects to a single source and never sees the original systems.


This is the modern default for a reason. The joins are modeled once and reused, queries are fast because everything is co-located in a columnar warehouse, and the BI layer stays simple. The cost is a pipeline to run and a sync delay: data is as fresh as your last load, typically minutes to hours old. If you are deciding whether you have outgrown a single database, see[when to add a data warehouse](https://www.basedash.com/blog/when-to-add-a-data-warehouse-signals-your-startup-has-outgrown-its-production-database) .


### Query federation


A federation engine runs one SQL query that reaches into multiple live sources, joins the results in memory, and returns a single answer, without copying data anywhere first.[Trino](https://trino.io/docs/current/overview/concepts.html) (formerly PrestoSQL) is the best-known standalone example: it exposes connectors to Postgres, MySQL, object storage, and dozens of other systems, and lets a single query join across them. Warehouses offer narrower versions: BigQuery’s[federated queries](https://cloud.google.com/bigquery/docs/federated-queries-intro) reach external sources like Cloud SQL through` EXTERNAL_QUERY` , and Snowflake can query across any database in the same account with fully qualified names, plus external tables over object storage.


Federation gives you fresh data and avoids building a pipeline. The tradeoffs are real: joining large tables across systems pulls data over the network and can be slow, governance spans multiple systems’ permission models at once, and a federated query is only as available as every source it touches.


### Data blending at the visualization layer


Some BI tools query each source separately and combine the results inside the tool after the fact.[Tableau’s data blending](https://help.tableau.com/current/pro/desktop/en-us/multiple_connections.htm) is the canonical example: you pick a primary data source and one or more secondary sources, and Tableau joins their aggregated results on shared dimensions (a left join from primary to secondary) at the sheet level.


Blending is convenient because no warehouse or federation engine is involved, and each source keeps its own connection. The limitation is that it operates on aggregates aligned by a common dimension, not arbitrary row-level joins. It is built for “signups next to ad spend by week,” not “attach every transaction to its user record.”


## Comparing the three approaches


Attribute Centralize in a warehouse Query federation Data blending


Where the join happens In the warehouse, pre-modeled At query time, across live sources In the BI tool, on aggregates


Data freshness As fresh as last sync (mins to hours) Live Live per source


Row-level joins Yes Yes No (aggregate-level only)


Performance at scale Strong (co-located columnar) Variable (network-bound) Fine for small aggregates


Setup effort Pipeline plus modeling Connector config Lowest


Governance One system to secure Spans every source Spans every source


Best for Recurring dashboards, large data Fresh ad hoc joins, no pipeline Quick side-by-side metrics


Typical tools Snowflake, BigQuery, Redshift + dbt Trino, BigQuery EXTERNAL_QUERY Tableau, similar BI tools


## When to use each: a decision framework


Run the question through four filters in order. The first one that gives a clear answer usually decides it.


1. **How often will you run this?** A one-off analysis or a weekly check tolerates federation or blending. Anything that powers a daily dashboard or a shared report should be modeled in a warehouse so the logic lives in one durable place.
2. **Do you need row-level joins or aggregates side by side?** If you must match individual rows across sources, rule out blending. If you only need metrics aligned on a shared dimension, blending or two separate warehouse queries are enough.
3. **How fresh must the data be?** If a few minutes of lag is fine, a warehouse sync is simplest. If the answer must reflect the source as of this second, federation against the live system is the only option that guarantees it.
4. **How much data moves in the join?** Joining millions of rows across two live systems over the network will be slow and fragile. Large joins belong in a warehouse where the data is co-located; keep federation for joins where at least one side is small.


A simple rule of thumb: **use this when** the analysis is recurring, the data is large, or you need governed, consistent definitions, centralize in a warehouse. **Use federation when** you need live data across systems for an ad hoc question and one side of the join is small. **Use blending when** you just want metrics from two tools on one dashboard, aligned by date or category. **Avoid** standing up federated cross-source joins for a high-traffic daily dashboard; that is a warehouse table waiting to happen.


## How cross-source joins actually work under the hood


The mechanics matter because they explain the tradeoffs.


In a **warehouse** , both datasets already live as tables in the same engine. A join is an ordinary SQL join the optimizer handles with full statistics, indexes or clustering, and co-located storage. This is why warehouse joins scale: nothing crosses a network mid-query.


In **federation** , the engine pushes down as much work as each source can do (filters, sometimes aggregations), pulls the intermediate results into the federation layer, and performs the join there. The performance ceiling is set by how much data has to cross the network and how much the engine can push down. Filter aggressively at the source and federation stays quick; join two large unfiltered tables and it crawls.


In **blending** , each source runs its own query and returns aggregated results, then the BI tool aligns them on the shared dimension in memory. There is no row-level join because the tool never has the raw rows together; it has two summaries it lines up.


This is also why governance differs. A warehouse gives you one place to manage permissions and row-level security. Federation and blending inherit the permission model of every source they touch, so a single dashboard might depend on three different access systems staying in sync.


## Permissions and governance across sources


Combining data multiplies the surface you have to secure. A dashboard that joins production Postgres, a warehouse, and a SaaS export is only as safe as the weakest connection behind it.


A few practices keep this sane:


- **Connect through read-only roles to every source** , especially production databases. A reporting connection should never be able to write. The patterns in[how to safely connect a BI tool to your production database](https://www.basedash.com/blog/how-to-safely-connect-a-bi-tool-to-your-production-database) apply to each source you federate.
- **Prefer the warehouse as the single governed layer** when row-level security and consistent permissions matter. It is far easier to enforce one access model than to reconcile three.
- **Define shared metrics once.** When data comes from multiple systems, the temptation to redefine “active customer” per source is strong. Pin the definition in one place; see[where to define business metrics](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) .


## Performance and cost tradeoffs


Each approach moves the cost somewhere different.


- **Warehouse** : you pay for storage and compute on every query, and for running the pipeline. Costs are predictable and query design controls them. Pre-modeled joins mean fast reads.
- **Federation** : you pay in latency and load on the source systems. A federated query against your production database competes with application traffic, which is why it should hit a replica and stay filtered.
- **Blending** : cheapest computationally because it works on small aggregates, but it caps what you can express. You will hit the aggregate-only ceiling on any real row-level need.


The general pattern: federation and blending are cheap to start and get expensive or limiting at scale; a warehouse costs more to set up and gets cheaper per question as usage grows.


## Common mistakes


- **Federating what should be modeled.** Wiring live cross-source joins into a daily dashboard creates a fragile dependency on every source being up and fast. Recurring joins belong in a warehouse.
- **Joining huge tables across the network.** Federation shines when one side is small and filtered. Two large unfiltered tables across systems will time out or crawl.
- **Treating blending as a full join.** Expecting row-level results from an aggregate-level blend produces silently wrong numbers. Know which operation you are doing.
- **Skipping read-only roles on federated sources.** Every live connection is a security and stability risk if it can do more than read.
- **Redefining metrics per source.** The same metric computed three different ways across three systems is how a multi-source dashboard loses trust.


## How this looks in practice with a modern BI tool


Most teams do not pick one approach forever; they grade into the warehouse. Early on, a single production database answers most questions, so you connect a BI tool directly and occasionally blend in a CSV or a second small source. As data spreads across tools and volume grows, you stand up a warehouse, sync the important sources into it, and model the joins once. Federation fills the gap for the live, ad hoc question that does not justify a pipeline. For a fuller picture of how these layers fit together, see[the modern BI stack for lean teams](https://www.basedash.com/blog/the-modern-bi-stack-for-lean-teams-what-to-build-and-what-to-skip) .


A modern BI tool should make all three comfortable.[Basedash](https://www.basedash.com/) , for example, connects directly to SQL databases and warehouses like PostgreSQL, MySQL, Snowflake, and BigQuery, so you can point it at a production database early and at a modeled warehouse later without changing how the team works. The dashboards, SQL, and natural-language queries all run against whichever source is the current source of truth, which keeps the combine-the-data decision an architecture choice rather than a tooling lock-in.


## FAQ


**Can a BI tool join data from two different databases?** Yes, in three ways. It can query a warehouse that already holds both datasets, federate a live query across both sources at query time, or blend their aggregated results at the visualization layer. Row-level joins require the first two; blending only aligns aggregates on a shared dimension.


**What is the difference between data federation and data blending?** Federation runs a single query across multiple live sources and joins the results at query time, supporting row-level joins. Blending queries each source separately and combines the aggregated results inside the BI tool, so it only aligns metrics on a shared dimension and cannot do arbitrary row-level joins.


**Do I need a data warehouse to combine data from multiple sources?** Not for one-off or low-volume cases, where federation or blending works. For recurring dashboards, large data, or governed shared definitions, a warehouse is the most reliable approach because the joins are modeled once and every query runs against one fast, co-located source.


**Is querying across live databases slow?** It can be. Federated joins pull intermediate results across the network, so performance depends on how much data moves and how much work each source can push down. Filtering aggressively and keeping one side of the join small keeps it fast; large unfiltered cross-source joins are slow and belong in a warehouse.


**Which is fresher, federation or a warehouse?** Federation, because it queries live sources at the moment you ask. A warehouse is only as fresh as its last sync, typically minutes to hours old. Choose based on whether the decision needs live data or can tolerate a short lag.
