---
schema_version: "1.0.0"
document_id: "cc2719541e7276c739277520ec5bad214225bd486e71c0f8104d428e34b1d9c9"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-dashboards-on-amazon-redshift-data/"
published_at: "2026-08-01T00:00:00+00:00"
first_seen_at: "2026-08-01T14:24:04.385160+00:00"
fetched_at: "2026-08-01T14:24:08.038357+00:00"
content_hash: "sha256:243383736d8edf3d1c6e53c3319717412b7838c13d1c64d337e4143bc23c8a9c"
---

# How to build dashboards on Amazon Redshift data

To build dashboards on Amazon Redshift, connect a BI tool over Redshift’s SQL endpoint (the default port is 5439) using a dedicated read-only user, push queries directly to Redshift instead of extracting data into a slower engine, and lean on materialized views plus result caching so a hundred people refreshing dashboards do not queue behind each other. The one rule that trips teams up: Redshift does not enforce primary key, unique, or foreign key constraints, but its query planner trusts them. If you declare a key that is not actually unique, a` SELECT DISTINCT` or an aggregation can return the wrong numbers, and the dashboard will look perfectly plausible while doing it.


This guide is for engineers, analysts, and operators who already run Amazon Redshift as their warehouse and want shareable dashboards on top of it. Redshift is a columnar, massively parallel (MPP) data warehouse, not a transactional database, and the things that make it fast (columnar storage, distribution and sort keys, result caching) also change how you should build dashboards. Below: how to connect a BI tool safely, which layer a dashboard tile should query, the constraint gotcha that produces wrong results, how to keep concurrent dashboards from bottlenecking, how to control Redshift compute cost, and when Redshift is the wrong place for a given dashboard.


## TL;DR


- Redshift speaks a PostgreSQL-compatible dialect, so most BI tools connect with a Redshift or PostgreSQL JDBC/ODBC driver on port 5439. It is not Postgres, though; see the[Amazon Redshift and PostgreSQL differences](https://docs.aws.amazon.com/redshift/latest/dg/c_redshift-and-postgres-sql.html) .
- Create a dedicated read-only user and group with` GRANT SELECT` on only the schemas reporting needs. Do not reuse an admin login for BI.
- Push queries directly to Redshift rather than extracting into an in-memory engine, or you lose the warehouse you paid for and add staleness.
- The big gotcha: Redshift constraints are informational only and not enforced. If a declared key is invalid, “some queries could return incorrect results,” per the[Redshift table constraints docs](https://docs.aws.amazon.com/redshift/latest/dg/t_Defining_constraints.html) .
- Use[materialized views](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-overview.html) with autorefresh to pre-compute the heavy rollups your dashboards run over and over. Redshift can even auto-rewrite queries to use them.
- Turn on[concurrency scaling](https://docs.aws.amazon.com/redshift/latest/dg/concurrency-scaling.html) for the WLM queue your dashboards use so bursts of viewers do not wait in line.
- For tool selection, see our[comparison of the best BI tools for Redshift](https://www.basedash.com/blog/best-bi-dashboarding-tools-for-redshift-2026) . Options include Amazon QuickSight, Tableau, Metabase, and[Basedash](https://www.basedash.com/) .


## Why Redshift changes how you build dashboards


Most BI advice assumes a row-oriented database like PostgreSQL or MySQL. Amazon Redshift is a different animal, and a query pattern that is fine on Postgres can be slow or expensive on Redshift.


It stores data by column, not by row, and spreads each table across many compute nodes. A query that touches three columns of a 40-column table reads only those three columns, which is why Redshift can aggregate huge tables quickly. The flip side:` SELECT *` on a wide table is wasteful, and fetching one row by a non-key column makes every node do work for a single record.


It is shaped by distribution and sort keys. A table’s distribution style controls how rows are spread across nodes, and its sort key controls the order data is stored in. Dashboards that filter and join along those keys stay fast. Dashboards that ignore them force data to be redistributed across the network on every query, which is the most common reason a Redshift dashboard feels slow.


It is built for a modest number of large analytical queries, not thousands of tiny concurrent ones. That matters the moment a dashboard has many simultaneous viewers, which is what the concurrency section below is about.


The practical takeaway: build dashboards that aggregate and filter along the table’s keys, read only the columns a chart needs, and treat every dashboard load as a query that costs compute. The rest of this guide is how to do that.


## How to connect a BI tool to Redshift


Connecting is straightforward once you handle two things: the driver and access control.


### Drivers and the endpoint


Redshift is based on PostgreSQL, so it speaks a familiar SQL dialect and works with PostgreSQL JDBC/ODBC drivers. AWS also publishes Redshift-specific JDBC, ODBC, and Python drivers that handle Redshift features (IAM authentication, better type handling) more cleanly, and those are the recommended choice for a production BI connection. Either way, you point the tool at the cluster or Redshift Serverless endpoint on the default port 5439 with a database name and credentials.


Redshift is close to Postgres but not identical. Some SQL features behave differently, and a few functions run only on the leader node. If a query that works in Postgres fails on Redshift, the[Amazon Redshift and PostgreSQL differences](https://docs.aws.amazon.com/redshift/latest/dg/c_redshift-and-postgres-sql.html) page is the first place to look.


### Create a dedicated read-only user


Do not connect BI with an admin account. Create a user and a group scoped to only what reporting needs, so analytics access is auditable and revocable without touching anything else:


```text
CREATE   USER   bi_readonly   PASSWORD   'strong-password-here'  ;
CREATE   GROUP   bi_readers   WITH   USER bi_readonly;


GRANT   USAGE   ON   SCHEMA   analytics   TO   GROUP bi_readers;
GRANT   SELECT   ON   ALL TABLES   IN   SCHEMA   analytics   TO   GROUP bi_readers;


-- so new tables are readable automatically
ALTER   DEFAULT   PRIVILEGES   IN   SCHEMA   analytics
GRANT   SELECT   ON   TABLES   TO   GROUP bi_readers;
```


Grant on the specific schemas reporting needs, not on everything. For authentication, prefer IAM-based credentials or federated single sign-on over static passwords where your tool supports it, and require SSL so the connection is encrypted. Keep the cluster in a private subnet and reach it through a VPC or bastion rather than exposing 5439 to the open internet.


## Which layer should a dashboard tile query?


This is the decision that determines whether your dashboards are fast, correct, and affordable. Redshift gives you a few places to read from, and the right one depends on the query pattern.


Layer What it is Best for Watch out for


Base table The table your data lands in Ad hoc exploration, drill-down, detail views, anything you query occasionally Full-table aggregations get slower and pricier as the table grows


Materialized view A stored, precomputed result set over one or more base tables High-traffic tiles that always run the same rollup (daily revenue, active users by day) Goes stale until refreshed; needs autorefresh or a schedule


Result cache Redshift’s automatic cache of a query’s results Identical repeated queries, like ten people opening the same dashboard Only helps when the query text matches and underlying data has not changed


Spectrum external table Data queried in place in Amazon S3 Cold, high-volume history you rarely aggregate Slower and priced per data scanned; not for hot dashboard tiles


A Redshift materialized view stores a precomputed result set so a dashboard can read the answer instead of recomputing an expensive join or aggregation every load. The[materialized view docs](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-overview.html) call out dashboards as the ideal use case precisely because dashboard queries are “predictable and repeated over and over again.” You can configure a materialized view to autorefresh when its base tables change, and Redshift can automatically rewrite a query to use a materialized view even when the query does not mention it.


The practical pattern for most dashboards:


- Point tiles that always compute the same rollup at a materialized view, so the aggregation runs once instead of on every dashboard open.
- Query base tables for the exploratory, filtered, drill-down views where the query shape changes.
- Let result caching handle the case where many people load the same unchanged dashboard, and design tiles so their query text is stable enough to hit the cache.


## The constraint gotcha that makes dashboards silently wrong


This is the most important section, because it produces numbers that look plausible but are wrong, and no error is ever raised.


Redshift does not enforce uniqueness, primary key, or foreign key constraints when you load data. They are informational only: you can declare a primary key and then happily insert duplicate rows, and the insert succeeds. Redshift enforces` NOT NULL` , and nothing else.


That would be harmless if the constraints were ignored, but they are not. The query planner treats declared keys as facts and optimizes around them. The[Redshift table constraints docs](https://docs.aws.amazon.com/redshift/latest/dg/t_Defining_constraints.html) are blunt about the consequence:


> The planner leverages these key relationships, but it assumes that all keys in Amazon Redshift tables are valid as loaded. If your application allows invalid foreign keys or primary keys, some queries could return incorrect results. For example, a SELECT DISTINCT query might return duplicate rows if the primary key is not unique.


Why this bites dashboards specifically: a dedupe or` COUNT(DISTINCT ...)` tile relies on the planner’s uniqueness assumption. If your ETL ever double-loads a batch, or a late-arriving update inserts a second version of a row, the declared primary key is now a lie. The planner keeps trusting it, so a distinct count can quietly over- or under-count, and the chart still renders a clean number. It changes only when someone notices the total does not tie out to another report.


How to handle it:


- Declare primary and foreign keys only when your pipeline actually guarantees them. They are worth declaring for query-planning speed, but only if they are true.
- If you cannot guarantee uniqueness, do not rely on` SELECT DISTINCT` to fix it. Deduplicate explicitly in a materialized view or model, for example with` ROW_NUMBER() OVER (PARTITION BY id ORDER BY loaded_at DESC)` and a filter to the latest row.
- Add a data-quality check that counts duplicate keys on the tables your dashboards read, and alert when it is non-zero, so a bad load surfaces before it reaches a chart.
- When a dashboard number looks off, check for duplicate keys before you suspect the BI tool.


## How to keep concurrent dashboards from bottlenecking


Redshift is fast per query, but it is tuned for a limited number of heavy queries at once. A popular dashboard on auto-refresh can send a burst of queries that queue behind each other, and viewers see spinning tiles.


The mechanism for handling this is workload management (WLM) plus concurrency scaling. Concurrency scaling adds transient cluster capacity when a queue’s queries would otherwise wait, and, per the[concurrency scaling docs](https://docs.aws.amazon.com/redshift/latest/dg/concurrency-scaling.html) , “users see the most current data, whether the queries run on the main cluster or a concurrency-scaling cluster.” You turn it on for a WLM queue, and eligible read queries spill onto extra capacity instead of waiting.


Practical steps:


- Put BI traffic in its own WLM queue routed by the` bi_readers` user group, so dashboard queries do not fight ETL for slots.
- Turn on concurrency scaling (set the queue’s mode to` auto` ) for that queue so bursts of dashboard viewers get extra capacity instead of a queue.
- Pre-aggregate the heaviest tiles into materialized views so each viewer runs a cheap read, not a full re-scan.
- Set sane refresh intervals. A tile does not need to re-query every few seconds for every viewer; match the refresh to how fast the data actually changes, and let result caching serve identical queries. Our guide to[dashboard refresh strategies](https://www.basedash.com/blog/dashboard-refresh-strategies-live-queries-scheduled-refreshes-and-cached-extracts) covers live queries, scheduled refreshes, and caching in more depth.


One caveat: concurrency scaling has limits. It does not apply to queries on tables that use interleaved sort keys or to temporary tables, among other exclusions, so check the docs before you assume every dashboard query is eligible.


## How to control Redshift cost from dashboards


On both Redshift Serverless (billed by compute usage) and provisioned clusters (where concurrency scaling adds metered capacity), dashboards translate directly into compute spend. A single expensive tile left on a five-second auto-refresh, multiplied by every open browser tab, is a real bill.


Keep it in check:


- **Pre-aggregate.** Materialized views turn a repeated expensive query into a cheap read. This is the single biggest lever.
- **Let the result cache work.** Stable, identical queries return from cache without re-running, so ten people on the same dashboard cost close to one query. Avoid injecting volatile values (like` now()` to the millisecond) into tile queries that break cache reuse.
- **Filter on the sort key and select only needed columns** so each query scans the minimum data.
- **Cap refresh frequency.** Real-time-looking dashboards rarely need sub-minute data. Set intervals to match the decision the dashboard supports.
- **Push down, do not extract.** A tool that extracts your Redshift data into a separate engine doubles storage and adds a staleness window; a tool that pushes SQL to Redshift keeps one copy and one governance model.


## When not to build a dashboard on Redshift


Redshift is excellent for what it was built for and a poor fit for some things dashboards commonly need. Use this as a filter before you build.


- **Single-record lookups.** “Show everything about customer 4821” is a transactional query. If a dashboard mostly fetches individual rows by a non-key identifier, that data belongs in Postgres or your app database, not Redshift.
- **Tiny datasets.** If a table has thousands of rows, not millions or billions, Redshift’s MPP advantages disappear and a simpler database is easier and cheaper.
- **High-concurrency, low-latency embedding.** Embedding a chart in an app that thousands of end users hit simultaneously with sub-second expectations is a different problem; a purpose-built serving layer or an OLAP engine like ClickHouse often fits better. See our guide on[building dashboards on ClickHouse](https://www.basedash.com/blog/how-to-build-dashboards-on-clickhouse-data) for that side of the tradeoff.
- **Operational data that changes every second.** Redshift handles updates, but a constantly mutating small table is not where it shines. Keep it transactional and join only if the join is small.


If you are still deciding whether you even need a warehouse yet, our guide on[when to add a data warehouse](https://www.basedash.com/blog/when-to-add-a-data-warehouse-signals-your-startup-has-outgrown-its-production-database) walks through the signals.


## Tool options for Redshift dashboards


Because Redshift speaks SQL over a standard endpoint, most BI tools can connect. The right one depends on who will use the dashboards.


- **Amazon QuickSight** is the native AWS option, with tight IAM integration and its SPICE in-memory cache. It fits teams that live entirely in AWS and want low per-reader cost.
- **Tableau** and **Power BI** are the incumbents for rich visual analytics, strong when you have analysts to build and maintain workbooks.
- **Metabase** is a solid open-source choice for teams that want to self-host and have some SQL on hand.
- **Basedash** fits teams that want non-technical people to explore Redshift data and ask follow-up questions in plain English, pushing queries directly to the warehouse without extracting data into a separate engine.


For a detailed, honest comparison across integration depth, AI features, concurrency handling, and pricing, see our[best BI tools for Redshift guide](https://www.basedash.com/blog/best-bi-dashboarding-tools-for-redshift-2026) .


## A setup checklist for Redshift dashboards


Use this as a practical sequence when you add a BI tool to Redshift.


1. **Create a dedicated read-only user and group** with` GRANT SELECT` on only the schemas reporting needs, plus` ALTER DEFAULT PRIVILEGES` so new tables are covered.
2. **Connect on port 5439 over SSL** using the AWS Redshift driver, ideally with IAM or SSO authentication rather than a static password.
3. **Audit your declared constraints.** Confirm every primary key and unique constraint your dashboards depend on is actually enforced by your pipeline, or deduplicate explicitly.
4. **Decide the query layer per tile.** Base table for exploration and drill-down, materialized view for always-on rollups, result cache for identical repeated loads, Spectrum for cold history.
5. **Give BI its own WLM queue with concurrency scaling on** , routed by the read-only group, so viewers do not queue behind ETL.
6. **Pre-aggregate heavy tiles into autorefreshing materialized views** and let Redshift auto-rewrite queries onto them.
7. **Filter on the sort key, select only needed columns, and cap refresh intervals** so each load scans and costs the minimum.
8. **Define core metrics once.** Agree how “active user” or “revenue” is calculated and put it in the BI model or a shared view so every dashboard matches. This is the foundation of[self-serve analytics](https://www.basedash.com/blog/self-serve-analytics-a-practical-guide-to-bi-adoption-across-your-organization) .


## FAQ


### How do I connect a BI tool to Amazon Redshift?


Point the tool at your cluster or Redshift Serverless endpoint on the default port 5439 using the AWS Redshift JDBC/ODBC driver (a PostgreSQL driver also works, since Redshift is Postgres-compatible). Connect as a dedicated read-only user created with` GRANT SELECT` on only the reporting schemas, use SSL, and prefer IAM or single sign-on authentication where the tool supports it. Keep the endpoint in a private subnet rather than exposing 5439 publicly.


### Why does my Redshift dashboard show wrong or duplicated numbers?


The most common warehouse-side cause is Redshift’s constraints being informational only. Redshift does not enforce primary key, unique, or foreign key constraints, but the query planner trusts them, so if a declared key is not actually unique (a double-loaded batch, a duplicate row), a` SELECT DISTINCT` or distinct count can return incorrect results. Check for duplicate keys on the tables your dashboards read, and deduplicate explicitly in a view or model rather than relying on` DISTINCT` .


### Should I query base tables or materialized views for Redshift dashboards?


Query materialized views for tiles that always run the same aggregation, since Redshift computes the result once and can auto-rewrite queries to use it. Query base tables for exploratory and drill-down views where the query shape changes. Configure materialized views to autorefresh when base tables change so dashboards do not read stale rollups, and lean on Redshift’s result cache for identical repeated dashboard loads.


### How do I stop a popular Redshift dashboard from slowing down for everyone?


Put BI traffic in its own workload management (WLM) queue routed by the read-only user group, and turn on concurrency scaling for that queue so bursts of viewers get transient extra capacity instead of waiting. Pre-aggregate heavy tiles into materialized views, filter on the sort key, select only the columns each chart needs, and cap refresh intervals so identical queries hit the result cache.


### Is Redshift good for customer-facing embedded dashboards?


It can work for moderate concurrency, but Redshift is tuned for a limited number of heavy analytical queries, not thousands of simultaneous low-latency requests. For high-concurrency embedded analytics, pre-aggregate aggressively, use concurrency scaling, and consider whether a purpose-built serving layer or an OLAP engine like ClickHouse fits the workload better. For internal dashboards and self-service analytics, Redshift is a strong fit.
