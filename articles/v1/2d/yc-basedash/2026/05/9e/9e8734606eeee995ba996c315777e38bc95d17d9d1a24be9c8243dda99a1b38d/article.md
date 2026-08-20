---
schema_version: "1.0.0"
document_id: "9e8734606eeee995ba996c315777e38bc95d17d9d1a24be9c8243dda99a1b38d"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-make-slow-bi-dashboards-fast-a-performance-playbook/"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:13:03.870884+00:00"
content_hash: "sha256:1629fb2be043852029b15f8f5a85cfba44b0a48864027b5691332ced52124c7e"
---

# How to make slow BI dashboards fast: a practical performance playbook

A BI dashboard that takes thirty seconds to load is not a dashboard. It is a status page that occasionally shows numbers. People stop opening it, stop trusting it, and quietly go back to asking the data team for screenshots in Slack.


This playbook is for analytics engineers, founders, and operators with at least one slow dashboard that nobody has gotten around to fixing. It walks through how to figure out where the time is going, then how to fix it at four layers: the warehouse, the SQL, the BI tool, and the dashboard design itself. Most slow dashboards are slow for one of about ten reasons, and fixing them is usually cheaper than buying a new tool.


## The short answer


Before changing anything, figure out *which* layer is slow. A dashboard that takes 30 seconds to render can be slow because:


1. The query takes 25 seconds in the warehouse.
2. The query takes 2 seconds but runs 20 times (once per chart, once per filter combination).
3. The BI tool is rendering 50,000 rows in the browser.
4. The cache is configured incorrectly and rebuilding on every page load.


The fix is completely different in each case. Optimizing SQL when the bottleneck is rendering does nothing. Adding a cache when the bottleneck is the warehouse changing partition keys does nothing.


The rest of this guide assumes you have run the dashboard once with the browser dev tools open and the warehouse query log visible, and you know roughly where the seconds are going.


## Step 1: diagnose where the time is going


Every BI tool has some version of a query log or performance recorder. Use it before tuning anything.


- **Tableau:** the[performance recorder](https://help.tableau.com/current/pro/desktop/en-us/perf_record_create_desktop.htm) shows query, render, and compile times per visualization.
- **Power BI:** the[Performance Analyzer](https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-performance-analyzer) breaks each visual into DAX query, visual display, and “other” time.
- **Looker:** the SQL Runner shows the generated SQL; Looker Studio has the “explore” timing pane.
- **Metabase:** the question detail page shows query runtime; the admin tools include query history.
- **Basedash:** the query log and dashboard detail view show per-block runtime against your connected database.
- **Sigma, Mode, Hex:** all expose per-cell or per-element runtimes.


On the warehouse side, every modern warehouse has a query history view:


- Snowflake:` SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY` (or` INFORMATION_SCHEMA.QUERY_HISTORY` for the last 7 days).
- BigQuery:` INFORMATION_SCHEMA.JOBS_BY_PROJECT` .
- Redshift:` STL_QUERY` /` SVL_QUERY_SUMMARY` .
- PostgreSQL:` pg_stat_statements` .
- ClickHouse:` system.query_log` .


Pull the slow dashboard’s queries from the log. You usually find one of three patterns:


1. **One query dominates.** A single chart is running an unindexed scan over a fact table or a join that explodes row counts. Fix that one query and the dashboard becomes fast.
2. **Many small queries.** Twenty queries are running, each taking a second, often because each tile is independent and runs serially. Fix the parallelism or consolidate.
3. **Everything is slow.** The warehouse is undersized, every query is competing for compute, or the dashboard is hitting a small OLTP database that is already loaded with production traffic.


Write down which pattern applies before doing anything else.


## Step 2: fix the SQL


For most internal dashboards, query time is the single biggest contributor. The wins come from a small set of techniques.


### Reduce row counts before joins


The slowest queries usually join two large tables, then filter. Filter first, then join. This is especially important on warehouses where the optimizer is good but not perfect on complex joins, and on PostgreSQL where the planner can mis-estimate.


```text
-- Slow: join, then filter
SELECT   u  .  email  ,   COUNT  (  o  .  id  )
FROM   users u
JOIN   orders o   ON   o  .  user_id   =   u  .  id
WHERE   o  .  created_at   >=   CURRENT_DATE   -   INTERVAL   '30 days'
GROUP BY   u  .  email  ;


-- Faster: filter in a CTE, then join
WITH   recent_orders   AS   (
SELECT   user_id, id
FROM   orders
WHERE   created_at   >=   CURRENT_DATE   -   INTERVAL   '30 days'
)
SELECT   u  .  email  ,   COUNT  (  o  .  id  )
FROM   recent_orders o
JOIN   users u   ON   u  .  id   =   o  .  user_id
GROUP BY   u  .  email  ;
```


On Snowflake, BigQuery, and Redshift the difference is usually small because the optimizer pushes the filter down. On PostgreSQL it can be 10x.


### Aggregate in the warehouse, not in the BI tool


If a chart shows daily revenue for the last 90 days, do not pull 90 days of raw transactions and aggregate in the BI tool. Aggregate in SQL and return 90 rows.


This sounds obvious, but BI tools quietly encourage the opposite. A “drag a date field onto a chart” workflow in Looker or Tableau often produces a query that returns far more rows than the chart actually needs. Inspect the generated SQL before you trust the abstraction.


### Replace correlated subqueries with window functions


Anything of the form “for each row, look up something in another table” is usually slow. Window functions are almost always faster on a modern warehouse:


```text
-- Slow
SELECT
user_id,
amount,
(  SELECT   MAX  (amount)   FROM   orders o2   WHERE   o2  .  user_id   =   o1  .  user_id  )   AS   user_max
FROM   orders o1;


-- Faster
SELECT
user_id,
amount,
MAX  (amount)   OVER   (  PARTITION   BY   user_id)   AS   user_max
FROM   orders;
```


### Pre-aggregate with materialized views or dbt models


If a dashboard runs the same aggregation every page load, materialize the result. Three good options, in roughly increasing complexity:


- **A daily-refreshed table** in dbt or as a scheduled SQL job. Simple, debuggable, fine for most KPI dashboards.
- **A materialized view** in PostgreSQL, Snowflake, or BigQuery. Maintained automatically by the warehouse; refreshes can be incremental.
- **An incremental dbt model** with a merge or insert strategy. Best when the source data is append-only and large.


For a fact table with hundreds of millions of rows, a pre-aggregated daily snapshot can take a 25 second query to under a second.


## Step 3: tune the warehouse


If SQL changes alone do not get the dashboard fast enough, the warehouse may be the bottleneck.


### Partitioning and clustering


Partition fact tables on the column you filter by, usually a date or event timestamp. A query that filters` WHERE event_date >= '2026-01-01'` on a partitioned table scans only matching partitions; on an unpartitioned table it scans the whole table.


- **BigQuery:** partition by ingestion time or a` DATE` /` TIMESTAMP` column; cluster on up to four columns used in` WHERE` and` JOIN` clauses.
- **Snowflake:** use clustering keys on large tables (` >1 TB` ); micro-partitioning is automatic but clustering helps prune.
- **Redshift:** set a sort key on the filter column and a distribution key matching the join column.
- **ClickHouse:** the` ORDER BY` clause on the table definition is your partition key; choose it to match dashboard filters.
- **PostgreSQL:** table partitioning by range on a date column for fact tables; otherwise add covering indexes.


### Right-size the compute


On Snowflake, doubling the warehouse size roughly halves query time, but also roughly doubles cost. For a dashboard hit by 5 people a few times per day, a small warehouse with a short auto-suspend is usually better than a medium that runs all day.


For high-concurrency dashboards (many viewers at once), the issue is often queuing rather than per-query speed. Snowflake’s multi-cluster warehouses, BigQuery’s slot reservations, and Redshift’s concurrency scaling are all ways to handle this.


### Don’t query production OLTP from a BI tool


If your dashboard hits the same PostgreSQL or MySQL database that serves your application, large analytical queries will compete with production traffic. The fix is not a query optimization. The fix is a read replica or a warehouse. We have a[longer guide on when to add a data warehouse](https://www.basedash.com/blog/when-to-add-a-data-warehouse-signals-your-startup-has-outgrown-its-production-database) for teams in this exact spot.


## Step 4: use the BI tool’s caching wisely


Every BI tool caches. The defaults rarely match the dashboard’s actual usage pattern.


### Pick the right refresh strategy


Live queries, scheduled refreshes, and cached extracts each fit different dashboards. A dashboard tracking yesterday’s sales does not need a live query. A dashboard supporting an on-call investigation does. We cover this tradeoff in detail in[dashboard refresh strategies](https://www.basedash.com/blog/dashboard-refresh-strategies-live-queries-scheduled-refreshes-and-cached-extracts) .


A practical default: cache aggressively, then add explicit “refresh” buttons for the few dashboards where freshness genuinely matters.


### Watch for filter cache misses


The biggest cache footgun is filters. A dashboard cached for “last 7 days” is a cache miss when a user changes the filter to “last 14 days,” even though the underlying data is identical. Some tools cache per filter combination, some per query, some per dashboard. Read the docs for yours.


If users tend to apply the same handful of filter combinations, pre-warm the cache by scheduling those combinations to refresh during off-hours.


### Use extracts for large, slow-moving data


A “cached extract” is a materialized snapshot stored in the BI tool’s own engine: Tableau Hyper, Power BI Import, Looker PDTs, Qlik QVD. Extracts shine when:


- The underlying dataset is large enough that warehouse queries are slow.
- The data is updated infrequently (daily, weekly).
- The dashboard is read by many concurrent users.


They are the wrong choice for sub-hour freshness or for tables that change shape frequently.


## Step 5: redesign the dashboard


Sometimes the dashboard is slow because it is doing too much. The cheapest performance optimization is removing things.


### Cut the number of charts


A dashboard with 25 charts is usually running 25 queries. Even if each one takes 0.5 seconds and they run in parallel, you are limited by the slowest one and by the warehouse’s concurrency limits. Cut to the 5-8 charts that actually drive decisions and move the rest to linked detail dashboards.


This is also good design, not just good performance. We have a guide on[building dashboards that drive decisions](https://www.basedash.com/blog/how-to-build-dashboards-that-drive-decisions-a-practical-guide) that argues for the same trim.


### Replace high-cardinality breakdowns with top-N


A bar chart showing revenue by customer for 5,000 customers is unreadable and slow. Top 20 customers plus an “all other” bucket is readable and fast. Apply the same rule to product breakdowns, geography, channel, and any other dimension with a long tail.


### Avoid cross-filter chains


A dashboard where every filter triggers a re-query for every chart can produce dozens of queries per interaction. If your BI tool supports it, batch queries or restrict cross-filtering to the charts where it actually matters.


### Reduce row-level rendering


Tables with 50,000 rows are a renderer problem, not a query problem. Either paginate, virtualize, or aggregate before rendering. Some tools handle this automatically; others ship the whole result set to the browser. Check the network tab before assuming.


## Tool-specific tips


A few patterns are common to most tools but show up under different names. If you are tuning a specific tool, start here.


Tool Most common quick wins


Tableau Switch from Live to Extract; reduce custom SQL; avoid blends in favor of joins; use context filters


Power BI Move from DirectQuery to Import where possible; reduce DAX measures with iterators (` SUMX` ,` FILTER` ); use aggregations; star schema, not flat


Looker Use PDTs for expensive joins; merge results sparingly; check` derived_tables` for unintended re-runs


Metabase Cache questions; turn on query caching with a sane TTL; use models for shared logic; avoid X-rays on large fact tables


Sigma Use materializations; restrict ad-hoc workbook expansion on large tables


Mode / Hex Run heavy queries in scheduled jobs; cache results; reduce notebook cell count on shared reports


Basedash Use AI-generated views for repeated queries; rely on built-in query caching; connect to a read replica when querying production data


Cube / dbt Semantic Layer Pre-aggregate roll-ups for common dashboard granularities; alias to materialized tables for repeated cuts


The general pattern across all of them: move work earlier in the pipeline (warehouse, extract, model) and ask the BI tool to do less at query time.


## A 30-minute performance audit


If you have a slow dashboard and 30 minutes, run this:


1. **5 minutes.** Open the dashboard and the warehouse query log side by side. Reload. Identify the slowest query.
2. **5 minutes.** Run that query in isolation in the warehouse. Note the runtime. If it is fast there but slow in the dashboard, the bottleneck is rendering or caching. If it is slow there, the bottleneck is the query.
3. **10 minutes.** If it is a slow query: check for missing filters, exploding joins, unnecessary` DISTINCT` , or aggregates on raw rows. Try a CTE-first rewrite.
4. **5 minutes.** Check whether a pre-aggregated table or materialized view would solve the problem. If yes, add it to your dbt project or as a scheduled job.
5. **5 minutes.** Configure the BI tool’s cache TTL. If it is set to “live” or 0 seconds, change it to something reasonable (15 minutes, 1 hour, daily depending on freshness needs).


Most slow dashboards drop from “annoyingly slow” to “fine” with one or two of these moves. If they do not, the underlying problem is usually that you are trying to do real-time analytics on the wrong stack, and a deeper rethink is in order.


## When the answer is “this is the wrong tool”


A small number of dashboards are slow because the architecture is wrong for the job. Signs:


- You need sub-second response on a dataset that changes thousands of times per minute. That is real-time analytics, not BI. Look at[real-time dashboard tools](https://www.basedash.com/blog/best-real-time-dashboard-tools-compared-2026) or column stores like ClickHouse.
- The dashboard is embedded in your product and serves thousands of concurrent customers. That is embedded analytics, with its own performance considerations. We have a[guide to embedded analytics for SaaS](https://www.basedash.com/blog/embedded-analytics-for-saas-the-complete-guide-to-adding-dashboards-to-your-product) .
- The dashboard tries to scan billions of rows on every load with no aggregation. That needs a pre-aggregation layer (Cube, dbt rollups) or a different storage engine, not a faster BI tool.


Treat performance work as a way to find these mismatches early. A dashboard that cannot be made fast within the current stack is telling you something about the stack.


## A performance checklist


Use this when you ship a new dashboard or audit an existing one. None of these are absolute rules, but each one is worth a deliberate decision.


- The slowest query in the dashboard runs in under 2 seconds in the warehouse.
- Every filter the user actually changes has a covered index, partition, or cluster key.
- The dashboard returns fewer than ~5,000 rows total across all charts.
- Aggregations happen in SQL, not in the BI tool.
- Repeated heavy queries are pre-aggregated in dbt, a materialized view, or a scheduled snapshot.
- Cache TTLs match how often the data actually changes, not how often the page loads.
- The dashboard has fewer than 10 visualizations on the default view.
- High-cardinality dimensions use top-N plus “other,” not the full list.
- No dashboard queries production OLTP directly during business hours.


A dashboard that passes this checklist loads in a couple of seconds on most stacks. One that fails three or more of them is almost certainly going to feel slow no matter which BI tool it lives in.


The right mental model is that BI dashboard performance is a property of the whole pipeline, not the BI tool. Tools like Basedash, Metabase, Looker, Power BI, and Tableau all surface the same underlying queries against the same warehouse. The fast ones are the ones where the team has done the work upstream: clean models, sensible aggregations, the right caching strategy, and dashboards that ask reasonable questions. Most slow dashboards become fast dashboards once that work is done.
