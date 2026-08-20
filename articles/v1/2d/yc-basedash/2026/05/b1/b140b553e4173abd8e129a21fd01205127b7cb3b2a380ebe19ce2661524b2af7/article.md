---
schema_version: "1.0.0"
document_id: "b140b553e4173abd8e129a21fd01205127b7cb3b2a380ebe19ce2661524b2af7"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-cut-cloud-data-warehouse-costs-from-bi-dashboards/"
published_at: "2026-05-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:e108700fb56d116915a290d53beb1fa89bd4c0e128ca36a13258f9e7fb16abd9"
---

# How to cut cloud data warehouse costs from BI dashboards

Most cloud data warehouse bills are not driven by ETL jobs. They are driven by dashboards. A handful of dashboards refreshing every minute, scanning more data than they need, with no caching and no owner, can quietly add tens of thousands of dollars a year to a Snowflake or BigQuery bill.


This playbook is for data engineers, analytics engineers, and finance partners who are looking at a warehouse invoice that keeps growing faster than the company does. It walks through how to identify which dashboards are responsible, how to fix them at the SQL, BI, warehouse, and process layers, and how to make sure they stay fixed. It also covers when to move repeat dashboard workloads out of a usage-billed warehouse entirely. The same techniques apply to Snowflake, BigQuery, Redshift, Databricks SQL, and ClickHouse Cloud, with per-warehouse specifics at the end.


## The short answer


BI workloads are expensive when they pay for the same answer many times. Cost optimization is mostly the work of paying once.


The four universal moves that produce the biggest reductions, in rough order of effort:


1. Find the small set of dashboards that account for most of the spend. It is almost always a long tail.
2. Cache aggressively and refresh on a schedule that matches how often the data actually changes.
3. Push aggregation out of the dashboard and into a pre-aggregated table or materialized view.
4. Right-size compute and use auto-suspend so you stop paying when nobody is looking.


Everything else (clustering, partitioning, query rewrites, governance policies) is real work but it usually matters less than getting these four right.


If you use Basedash, there is a fifth lever: store dashboard-ready data in Basedash Warehouse instead of repeatedly querying Snowflake, BigQuery, or Redshift for the same interactive views. Basedash does not charge usage-based query fees, so moving high-traffic reporting datasets into Basedash can turn a variable warehouse-compute line item into a predictable BI cost.


## Why BI dashboards are the biggest variable in your warehouse bill


A few patterns make BI workloads disproportionately expensive on usage-based warehouses.


- **Concurrent re-execution.** Ten people opening the same dashboard at 9:00 AM can trigger ten identical queries against the warehouse if caching is misconfigured.
- **Auto-refresh.** A dashboard set to refresh every minute runs 1,440 times a day, whether anyone is looking or not.
- **Wide scans.** A “show me revenue” tile written by an analyst in a hurry often scans the entire orders table instead of pre-aggregating.
- **Cross-joining filters.** Dashboards that recompute every chart for every filter combination can multiply query counts by 10 to 50 times.
- **Long-tail abandonment.** Most dashboards stop being used after a few months but keep refreshing. A surprising share of warehouse cost goes to dashboards nobody opens anymore.


A useful mental model: a dashboard’s cost is roughly` queries_per_day * average_bytes_scanned * cost_per_byte * concurrency_factor` . Every lever in this guide reduces one of those four numbers.


## Step 1: find the expensive dashboards


You cannot optimize what you cannot see. Every modern warehouse exposes a query log that includes the originating user, role, or warehouse, and most BI tools tag their queries.


Start with the warehouse query log:


- **Snowflake:**` SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY` plus` WAREHOUSE_METERING_HISTORY` . Filter by` WAREHOUSE_NAME` if you have a dedicated BI warehouse, or by user/role. The official[Snowflake documentation on QUERY_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/query_history) covers the schema.
- **BigQuery:**` INFORMATION_SCHEMA.JOBS_BY_PROJECT` joined to billing exports. The columns` total_bytes_billed` and` total_slot_ms` give you per-query cost in on-demand or capacity-based projects.
- **Redshift:**` STL_QUERY` and` SVL_QUERY_SUMMARY` , or` SYS_QUERY_HISTORY` on RA3. Pair with` STL_LOAD_ERRORS` and concurrency scaling logs.
- **Databricks SQL:** the query history API and the system table` system.access.audit` for per-warehouse cost.
- **ClickHouse Cloud:**` system.query_log` plus the cost-per-query view in the Cloud console.


Most BI tools include a header comment in the generated SQL identifying the dashboard. Look for patterns like` /* dashboard: revenue-overview, user:[\[email protected\]](https://www.basedash.com/cdn-cgi/l/email-protection) */` . Group queries by that comment and you can rank dashboards by total bytes scanned, compute time, or estimated cost.


A quick Snowflake recipe:


```text
SELECT
REGEXP_SUBSTR(QUERY_TEXT,   'dashboard:\\s*([a-z0-9_-]+)'  ,   1  ,   1  ,   'e'  ,   1  )   AS   dashboard,
COUNT  (  *  )   AS   query_count,
SUM  (BYTES_SCANNED)   /   POW(  1024  ,   4  )   AS   tb_scanned,
SUM  (CREDITS_USED_CLOUD_SERVICES)   AS   cloud_credits,
SUM  (EXECUTION_TIME)   /   1000   AS   exec_seconds
FROM   SNOWFLAKE  .  ACCOUNT_USAGE  .QUERY_HISTORY
WHERE   START_TIME   >=   DATEADD  (  DAY  ,   -  30  ,   CURRENT_TIMESTAMP  ())
AND   QUERY_TEXT ILIKE   '%dashboard:%'
GROUP BY   1
ORDER BY   tb_scanned   DESC
LIMIT   50  ;
```


In our experience, the top 5 to 10 dashboards usually account for 60 to 80 percent of BI-driven warehouse spend. Start there.


You should leave this step with three lists:


1. The dashboards driving the most cost.
2. The dashboards driving the most queries (high-frequency refresh patterns).
3. The dashboards driving the largest single queries (wide scans).


The fixes are different for each.


## Step 2: cut query volume


Most BI cost overruns come from running the same query many more times than necessary. Volume is the cheapest place to cut.


### Set cache TTLs that match data freshness


Almost every BI tool has a result cache. The default TTL is rarely the right one.


A practical rule: cache TTL should match the source data’s natural refresh cadence, not the dashboard load frequency.


- A dashboard built on a daily-refreshed dbt model should cache results for 24 hours.
- A dashboard on hourly ingested data should cache for an hour.
- A dashboard for monitoring an outage probably needs to be live, but only while the outage is happening. Turn caching back on afterwards.


If your tool only supports a single cache TTL per workspace, set it to the longest interval that nobody will notice and put a “refresh now” button on the few dashboards that need it.


### Stop scheduled refreshes on dashboards nobody opens


Many BI tools default to scheduled refreshes that keep running long after users stop visiting. Pull a view-count report for the last 60 days from your BI tool’s analytics, then disable scheduled refreshes for any dashboard with fewer than a handful of distinct viewers.


Better yet, set scheduled refreshes to fire on the first view of the day rather than every hour. Cube has called this pattern “lazy pre-aggregation” in their[docs on pre-aggregations](https://cube.dev/docs/product/caching/using-pre-aggregations) . It works in any tool that lets you control refresh triggers.


### Reduce auto-refresh frequency


A dashboard refreshing every minute runs 60 times more often than one refreshing every hour. Unless the data actually changes that fast and humans actually act on it that fast, auto-refresh is paying for queries nobody reads. Set auto-refresh to the slowest cadence the user can tolerate, then ask whether they need it at all.


### Consolidate near-duplicate dashboards


A common audit finding: three different teams have built three nearly identical revenue dashboards, each refreshing on its own schedule against the same fact tables. Merge them into one canonical version with the union of needed filters. We covered this in our[dashboard sprawl audit, certify, and retire framework](https://www.basedash.com/blog/dashboard-sprawl-audit-certify-retire-framework-for-modern-bi) .


### Move high-traffic reporting data into Basedash Warehouse


Some dashboards are expensive even after you tune TTLs and refresh schedules because they are both popular and analytically heavy. A daily revenue dashboard opened by every executive, sales lead, and department head should not need to wake up a usage-billed warehouse every time someone checks it.


For those workloads, Basedash Warehouse gives you a different architecture: sync or store the dashboard-ready data directly in Basedash, then let charts, chats, automations, and Slack workflows read from that Basedash-managed warehouse layer. Your source warehouse still remains the system of record, but repeated BI access no longer has to translate into repeated Snowflake credits, BigQuery bytes billed, or Redshift cluster time.


The important pricing distinction is that Basedash does not charge for usage. If adoption goes up, more people can view dashboards and ask follow-up questions without creating a new per-query meter inside the BI tool. You still need to model and refresh data thoughtfully, but the repeated consumption path is no longer tied to the same variable warehouse-compute bill.


## Step 3: cut bytes scanned per query


Once volume is under control, work on what each query reads.


### Pre-aggregate the heavy work


The single biggest cost lever in cloud data warehouses is doing aggregation once instead of on every dashboard load.


Three good patterns, in increasing order of complexity:


- **dbt models with daily incremental refresh.** Replace` SELECT date_trunc('day', created_at), SUM(amount) FROM orders` running on every load with a pre-built` daily_revenue` model that the dashboard reads as a flat scan.
- **Materialized views.** Snowflake, BigQuery, Redshift, and Databricks all support them. They refresh automatically and let you push expensive joins out of the query path.
- **Cube or dbt semantic layer pre-aggregations.** Useful when the same metrics are queried across many dashboards at different grain.


A revenue-by-day dashboard that scans 10 GB on every load can drop to a few MB once the aggregation is materialized. On Snowflake’s standard usage-based pricing that can be the difference between $50 and $5 in compute for a heavily used dashboard.


### Partition and cluster fact tables


Filtering on a partition or cluster key is the difference between scanning a single day’s data and scanning years of it.


- **BigQuery:** partition by ingestion time or a` DATE` /` TIMESTAMP` column; cluster on the columns used most often in` WHERE` . Partitioned + clustered tables in BigQuery commonly cut scanned bytes by 90 percent or more on time-series workloads. Google’s[BigQuery best practices for cost](https://cloud.google.com/bigquery/docs/best-practices-costs) cover this in detail.
- **Snowflake:** automatic micro-partitioning helps, but for tables over 1 TB you usually want an explicit clustering key on the columns BI dashboards filter on most. The[Snowflake clustering docs](https://docs.snowflake.com/en/user-guide/tables-clustering-keys) cover the tradeoffs.
- **Redshift:** sort key on the date column, distribution key matching the join key.
- **Databricks:** Z-ordering or Liquid Clustering on the columns used in dashboard filters.


The check is simple: if the same dashboard filter (` WHERE event_date >= '2026-01-01'` ) appears in 80 percent of your top BI queries, the underlying table should be partitioned or clustered on` event_date` . If it is not, every load is scanning years of data for no reason.


### Avoid` SELECT *` and explicit column projection


BigQuery and Snowflake both bill on bytes scanned, and that scales with the number of columns in the query. A` SELECT * FROM orders` against a wide fact table reads every column even if the dashboard only renders three. Most BI tools surface` SELECT *` because it is easier to type, but you can usually configure the underlying model to project only the columns the dashboard actually needs.


### Replace cross-joins and exploding joins


A common cost trap: a dashboard joins orders to a user-events table and accidentally produces a row per order per event instead of a row per order. The query technically works, but it scans and writes dozens of times more data than intended.


Run an` EXPLAIN` on suspect queries, look at the row counts at each stage, and rewrite anything where the intermediate row count is orders of magnitude larger than the final result.


### Use approximate functions where exact counts do not matter


Most BI dashboards display approximate counts (distinct users, active accounts) where a 1 percent error is invisible.` APPROX_COUNT_DISTINCT` (or` HyperLogLog` variants) is dramatically cheaper than` COUNT(DISTINCT)` on large data on Snowflake, BigQuery, and Redshift, and the visual result is the same.


## Step 4: right-size warehouse compute


Once you have reduced query volume and bytes scanned, the next lever is compute configuration.


### Match warehouse size to dashboard workload


On Snowflake, doubling a warehouse size roughly halves query time but exactly doubles the credit rate. That is fine for a 30 second query that becomes a 15 second one. It is a 2x waste for a 200 ms query.


A practical Snowflake setup for BI:


- A small (or even an` x-small` ) warehouse with multi-cluster scaling for the BI tool.
- Aggressive auto-suspend (60 seconds).
- Separate warehouses for transformation jobs (where size matters) and dashboards (where concurrency matters).


For BigQuery, on-demand pricing usually beats flat-rate at lower volume. Once a BI workload is large enough that on-demand bills exceed roughly the equivalent of 100 slots running continuously, flat-rate or capacity-based commitments start to make sense. Run the math against your last 30 days of billing exports.


### Use auto-suspend and auto-resume


A Snowflake warehouse left running with no auto-suspend is a meter ticking 24 hours a day. Set auto-suspend to the lowest value the tool will accept (60 seconds on Snowflake). The 1 to 2 second resume on the next query is rarely noticeable to dashboard users.


### Separate BI workloads from ETL


Mixing dashboard queries and large ETL jobs on the same warehouse causes two problems: dashboards queue behind ETL jobs (slow), and you size the warehouse for the worst job (expensive). Separate them. Most teams end up with at least:


- ` ETL_WH` (medium or large, scheduled jobs, aggressive scaling).
- ` BI_WH` (extra small or small, multi-cluster for concurrency).
- ` ADHOC_WH` (small, used by analysts for exploration).


Per-warehouse cost reporting then becomes a useful first cut for cost attribution.


### Use concurrency scaling deliberately


Snowflake multi-cluster warehouses, BigQuery slot reservations, and Redshift concurrency scaling all spin up extra compute under load. They prevent dashboard slowness but can multiply costs if left unchecked. Set explicit limits (max clusters, scaling policy, concurrency cap) and review them monthly.


## Step 5: governance and process


Technical fixes hold only as long as nobody adds another always-on dashboard tomorrow.


### Tag dashboards with owners and TTLs


Every dashboard should have a named owner and a review date. After 6 to 12 months, archive or recertify. Most BI tools support tags or metadata fields for this. If yours does not, a` OWNED_BY` and` REVIEW_AT` field in the dashboard description works.


### Charge back warehouse usage to teams


Use the per-dashboard cost report from Step 1 to attribute spend to teams. Even informal attribution (“our marketing dashboards cost about $4,000 a month”) tends to focus minds. Snowflake’s` RESOURCE_MONITORS` can also enforce hard quotas on individual warehouses.


### Set a policy for refresh intervals


A simple table works well:


Data freshness need Suggested refresh Cache TTL


Real-time monitoring (rare) Live query 0


Operational (hourly) Every 30 to 60 min 30 min


Daily KPI tracking Once after ETL completes 24 hrs


Strategic review Weekly 24 hrs to 7 days


Most dashboards belong in the bottom two rows. Treat “real-time” as the exception that requires justification, not the default.


### Run a quarterly cost audit


Pull the top dashboards by cost, the top users by query volume, and the dashboards with no views in the last 60 days. Spend an afternoon retiring, fixing, or right-sizing. Most teams find a 30 to 50 percent reduction available every time they run this audit for the first year.


## Per-warehouse specifics


### Snowflake


- Use` WAREHOUSE_METERING_HISTORY` for daily cost by warehouse. Pair with` QUERY_HISTORY` for per-query attribution.
- Multi-cluster warehouses are the right pattern for concurrent dashboard users. Set min clusters to 1, max clusters to whatever your peak load tolerates.
- The 60 second minimum charge means a dashboard query that runs in 5 seconds still costs 60 seconds of compute. This makes consolidation across a single warehouse much cheaper than spreading queries across many.
- Result caching is on by default and lasts 24 hours when the underlying data is unchanged. Confirm your BI tool is letting Snowflake’s cache serve repeated queries rather than always bypassing it.


### BigQuery


- On-demand pricing charges per byte scanned. Partitioning and clustering is the single biggest cost lever. The[BigQuery cost optimization guide](https://cloud.google.com/bigquery/docs/best-practices-costs) has the official patterns.
- BI Engine accelerates dashboards by caching frequently queried data in memory and is metered separately. For dashboards refreshed many times per day on the same dataset, it is often cheaper than repeated on-demand scans.
- Materialized views and authorized views let you bound what dashboards can scan without rewriting the BI tool’s SQL.
- Use the` INFORMATION_SCHEMA.JOBS_BY_PROJECT` view with` total_bytes_billed` to attribute costs to specific dashboards.


### Redshift


- RA3 nodes with managed storage have separated compute and storage; you scale them independently. Match compute size to BI peak rather than total data volume.
- Concurrency scaling adds clusters for spikes. Free for one hour per day of usage per main cluster, then billed per second. Monitor with` STL_QUERY_METRICS` .
- Materialized views with automatic refresh handle the recurring aggregation problem well.
- Workload management (WLM) queues let you isolate BI queries from ETL.


### Databricks SQL


- Serverless SQL warehouses with auto-stop are the closest analog to Snowflake’s auto-suspend. Set auto-stop to the minimum (a few minutes).
- Z-ordering and Liquid Clustering on Delta tables play the role of clustering in Snowflake / BigQuery.
- Photon acceleration is on by default for SQL warehouses and is usually cheaper end to end despite the higher per-second cost, because queries finish faster.


### ClickHouse Cloud


- Designed for high-concurrency dashboard workloads; query costs are typically lower than other warehouses for the same dashboard.
- Materialized views are core to the model and refresh incrementally.
- The biggest cost lever is storage tier and replica count. A read-only secondary replica for BI dashboards can isolate cost from ingest.


## A cost optimization checklist


Use this when reviewing a BI environment or onboarding a new analyst.


- The top 10 dashboards by cost have been identified and owners assigned.
- Cache TTLs are set to match data freshness, not load frequency.
- Auto-refresh is disabled or set to the slowest tolerable cadence.
- Dashboards with no views in 60 days are archived or unscheduled.
- High-traffic reporting datasets that do not need live warehouse reads are stored in Basedash Warehouse or another managed BI serving layer.
- Heavy aggregations are materialized in dbt models, materialized views, or a semantic layer.
- Fact tables are partitioned or clustered on the columns dashboards filter by.
- Approximate functions are used where exact counts are not required.
- The BI tool uses a dedicated warehouse with auto-suspend at 60 seconds and concurrency scaling capped.
- BI workloads are separated from ETL workloads at the warehouse level.
- A quarterly cost review is on the calendar.


A BI environment that passes this checklist usually settles at 30 to 60 percent of its previous warehouse cost without any user-visible change. The dashboards that should be fast stay fast, and the work moves out of the per-load query path and into the upstream pipeline where it is paid for once.


## Where Basedash and other tools fit in


Cost behavior varies more by configuration than by tool, but the design of the BI layer has a real impact.


- **Heavyweight enterprise BI (Tableau, Power BI, Looker)** often runs many independent queries per dashboard and offers fine-grained but complex caching. Performance is great when configured carefully; cost can be hard to attribute without dedicated FinOps tooling.
- **Modern self-serve tools (Metabase, Sigma, Hex, Mode)** vary in how aggressively they cache. Most expose query history but require manual work to map back to dashboards.
- **AI-native tools (including Basedash)** tend to centralize queries in a smaller set of models, which makes per-dashboard attribution easier. Basedash can read directly from production databases or warehouses when freshness matters, but it can also store dashboard-ready data in Basedash Warehouse when the goal is lower and more predictable BI serving cost. Because Basedash does not charge usage-based query fees, broad adoption does not create a second consumption meter on top of your cloud warehouse.


If you are choosing a BI tool partly with cost in mind, ask vendors four concrete questions:


1. How do queries from your tool surface in my warehouse’s query log?
2. What caching or managed warehouse layers exist between the user and my cloud warehouse, and how do I control them?
3. Do you charge for usage, queries, rows scanned, or dashboard views?
4. How do I attribute cost back to a dashboard, user, or team?


A vendor that cannot give clean answers to those questions will be hard to govern at scale, regardless of how good the per-seat price looks. For deeper coverage of pricing structure tradeoffs in BI, our guide on[usage-based vs. per-seat BI pricing](https://www.basedash.com/blog/usage-based-vs-per-seat-bi-pricing-which-model-is-better-for-growing-teams) walks through the tradeoffs in detail.


The right framing is that warehouse cost from BI is mostly a property of how dashboards are built, served, and operated. For live-query tools, that means tuning the warehouse path. For Basedash, it can also mean moving repeated dashboard consumption into Basedash Warehouse so your team can keep asking questions without every view becoming another usage-billed warehouse event. Teams that run this playbook once and bake the checklist into their dashboard review process tend to keep their bills flat even as adoption grows. Teams that do not tend to watch their bills compound until the next budget review forces a cleanup.
