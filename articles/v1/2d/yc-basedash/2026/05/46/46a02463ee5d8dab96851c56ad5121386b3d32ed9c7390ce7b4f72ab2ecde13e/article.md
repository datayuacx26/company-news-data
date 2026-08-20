---
schema_version: "1.0.0"
document_id: "46a02463ee5d8dab96851c56ad5121386b3d32ed9c7390ce7b4f72ab2ecde13e"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/dashboard-refresh-strategies-live-queries-scheduled-refreshes-and-cached-extracts/"
published_at: "2026-05-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:13:06.313483+00:00"
content_hash: "sha256:fa8f5a5760e86b67fc81588b5d39d9593375d6d8d1dc7709957f3231ff32ee44"
---

# Dashboard refresh strategies: live queries, scheduled refreshes, and cached extracts

Most BI dashboards do not need to refresh in real time. They need to refresh at the right time. The three refresh strategies every BI tool implements are **live queries** (run the SQL on every page load), **scheduled refreshes** (run the SQL on a cron and cache the result), and **cached extracts** (materialize the result set into a separate engine and query that). Choosing among them is a tradeoff between data freshness, warehouse cost, dashboard latency, and how many concurrent viewers you need to support.


This guide is for analytics engineers, founders, and operators choosing between BI tools, or tuning the ones they already have. It explains how each refresh strategy actually works, when each one fits, and how to map a refresh cadence to the type of decision the dashboard supports.


## TL;DR


- Live queries are best when the underlying data changes constantly and you have a small number of viewers. They are the simplest mental model and the most expensive at scale.
- Scheduled refreshes are the default for most internal dashboards. Hourly is usually enough; daily is fine for board metrics.
- Cached extracts (Tableau Hyper, Power BI Import, Looker PDT, Domo Beast Mode tables) are for high-concurrency or large historical datasets where live querying the warehouse is too slow or too expensive.
- Match refresh cadence to decision latency, not to data latency. A dashboard that drives a daily decision does not need minute-level data.
- Real-time dashboards are a separate category. They require a streaming source, a different storage engine, and usually a different tool.


## The three refresh strategies, explained


Every BI tool implements some variation of these three. The names differ but the architecture is similar.


### 1. Live queries (also called direct query, live connection, query mode)


The BI tool issues a fresh SQL query to your warehouse or database every time a user loads or filters a dashboard. Nothing is cached on the BI side (or only a short-lived per-session cache is used).


How it shows up in different tools:


- **Tableau:** “Live” connection
- **Power BI:** “DirectQuery” mode
- **Looker:** the default behavior (Looker is live-query first)
- **Metabase:** the default behavior
- **Basedash:** live queries against your connected database or warehouse
- **Sigma:** live queries against the warehouse
- **Mode, Hex:** live queries inside notebooks; cached in saved reports


The advantage is that what you see is what the warehouse currently has. There is no staleness window. Permissions enforced by the database, like row-level security in PostgreSQL or Snowflake, are honored on every query.


The disadvantage is that every page load is a query. Ten people opening the same dashboard at 9:01 AM is ten queries. Add a filter and it is another query. On warehouses billed by compute time (Snowflake, BigQuery, Databricks), this can become the largest line item in your data bill if you are not careful.


### 2. Scheduled refreshes (also called extract refresh, snapshot, materialization)


The BI tool runs the SQL on a schedule — every 15 minutes, hourly, every six hours, daily — and stores the result. Dashboard viewers query the stored result, not the warehouse.


How it shows up:


- **Tableau:** “Extract” with a refresh schedule, stored in Hyper
- **Power BI:** “Import” mode, stored in the VertiPaq engine
- **Looker:** Persistent Derived Tables (PDTs), stored back in the warehouse
- **Metabase:** model caching with TTL
- **Mode, Hex:** scheduled runs that update saved query results
- **Sigma:** materialized views


The advantage is that one warehouse query serves many dashboard views. If a hundred people open the same dashboard between refreshes, it is still just one query. Load times are typically much faster because the data is pre-aggregated and stored in a fast columnar store.


The disadvantage is staleness. A dashboard refreshed at 6 AM does not reflect a transaction at 6:01 AM. For most internal reporting this is fine. For operational dashboards, customer-facing dashboards, or anomaly alerting, it is often not.


### 3. Cached extracts in a dedicated engine


This is a subset of scheduled refreshes worth calling out separately because of how aggressively it changes performance characteristics. Instead of caching a query result, you materialize an entire transformed dataset into a purpose-built engine optimized for analytics queries.


Examples:


- **Tableau Hyper:** an in-memory columnar engine. Extracts can be hundreds of millions of rows and still respond in seconds.
- **Power BI Import mode + VertiPaq:** compressed in-memory columnar storage.
- **Looker PDTs:** materialized tables in your warehouse, with incremental rebuild support.
- **ThoughtSpot Falcon (legacy) and the modern Liveboards cache:** in-memory aggregations.


These work because the engine is designed for slice-and-dice queries on a fixed schema. Filtering, grouping, and joining are very fast once the data is loaded. The tradeoff is that the dataset has to be rebuilt or incrementally updated on a schedule, and the engine is a separate piece of infrastructure to operate or pay for.


## How to choose: match refresh to decision latency


The most common mistake is choosing refresh rate based on how often the data changes, not how often a human acts on it.


A few examples:


- **Board metrics, ARR, churn rate.** Reviewed weekly or monthly. Daily refresh is fine; hourly is overkill.
- **Marketing campaign performance.** Reviewed daily. Hourly refresh is plenty.
- **Sales pipeline.** Reviewed daily, sometimes mid-day. Refresh every 1 to 4 hours.
- **Customer support queue depth.** Acted on within minutes. Refresh every 1 to 5 minutes, or use live queries.
- **Fraud or anomaly alerts.** Acted on within seconds. Not a dashboard problem; this needs streaming.
- **A customer-facing usage dashboard.** Viewers expect “now” but tolerate a few minutes. 5 to 15 minute refresh, or live queries with aggressive caching.


The rule of thumb: **refresh just often enough that no one ever sees the same number twice when they should see a new one** . Anything more frequent is wasted compute.


## A simple decision matrix


Question Live query Scheduled refresh Cached extract


Data freshness needed Seconds Minutes to hours Minutes to days


Typical concurrent viewers 1 to 20 10 to 1,000 100 to 10,000+


Warehouse cost per view High Low Very low


Setup complexity Low Medium High


Honors database row-level security Yes Sometimes Rarely


Good for ad-hoc exploration Yes Limited Limited


Good for embedded customer dashboards Only at low scale Yes Yes


Use it as a starting point. Most teams end up with a mix: live queries for analyst exploration, scheduled refreshes for the dashboards executives look at, and cached extracts for anything embedded in a customer-facing product.


## Use this when, avoid this when


### Live queries


**Use when:**


- You have fewer than ~50 daily dashboard viewers.
- The data changes often enough that staleness would be misleading.
- You want database-enforced permissions (row-level security, column masking) to apply on every query.
- You are doing ad-hoc exploration and need to follow up questions in real time.
- Your warehouse has result caching (Snowflake, BigQuery) that absorbs duplicate queries cheaply.


**Avoid when:**


- You are sharing the dashboard with thousands of viewers.
- Your warehouse bill is already a problem.
- The dashboard runs heavy joins that take more than a few seconds.
- The data only changes once a day anyway.


### Scheduled refreshes


**Use when:**


- The same dashboard is viewed many times between data changes.
- A 15 minute to 24 hour staleness window is acceptable.
- You want predictable, capped warehouse cost.
- You need consistent snapshots for things like daily KPI emails or week-over-week comparisons.


**Avoid when:**


- The data needs to be live to be trustworthy (operational ops, on-call dashboards).
- Refresh time is longer than the freshness budget (a 90-minute extract on an hourly schedule is fragile).
- You need per-viewer row-level security and the cached layer does not enforce it.


### Cached extracts in a dedicated engine


**Use when:**


- You are embedding analytics in a SaaS product and need predictable sub-second load times for many tenants.
- Your dataset is large enough that warehouse queries take seconds to minutes.
- You are running a tool with a native extract engine (Tableau, Power BI, ThoughtSpot) and the cost or latency of live querying is unacceptable.


**Avoid when:**


- You have a small dataset where a warehouse cache would do the same job.
- You cannot afford the operational overhead of monitoring extract rebuilds.
- The data freshness requirement is shorter than the rebuild time.


## Real-time dashboards are a different category


Many teams ask for “real-time dashboards” when they actually mean “fresh dashboards.” Real real-time analytics — the kind where each event lands on the dashboard within a second of happening — is a different problem than choosing a refresh rate.


True real-time analytics needs:


- A streaming source (Kafka, Kinesis, Pub/Sub, change data capture from the OLTP database).
- A storage engine that can ingest and query rows in seconds, like ClickHouse, Apache Pinot, Druid, Tinybird, or Materialize.
- A BI or visualization layer that supports either streaming subscriptions or short polling intervals.


Most BI tools — Tableau, Power BI, Looker, Metabase, Sigma, Basedash — are not designed for sub-second freshness on millions of events per minute. They will technically “refresh every minute,” but the underlying warehouse query is not architected for that workload.


If the use case is genuinely operational (live system health, ad bidding, fraud, trading), look at purpose-built real-time analytics stacks and consider[real-time dashboard tools](https://www.basedash.com/blog/best-real-time-dashboard-tools-compared-2026) separately from your general-purpose BI. For “fresh enough” reporting on a transactional database, hourly or 15-minute scheduled refreshes are almost always the right call.


## Cost: the part most teams underestimate


Refresh strategy is one of the biggest hidden drivers of warehouse cost. A few patterns to watch for.


**Live queries with no caching layer.** Every dashboard view triggers a warehouse query. If a 200-person team opens the same Snowflake-backed dashboard each morning, that is 200 warehouse executions of the same query. Most of them could be served from a 5 minute cached result with no one noticing.


**Scheduled refreshes that run every 5 minutes “just in case.”** A common Power BI or Tableau pattern. If the data only updates hourly upstream, 11 out of every 12 refreshes do nothing useful and still consume warehouse compute.


**Cached extracts that get re-built from scratch.** Many tools default to full rebuilds. If you have a hundred million row fact table and a daily extract, an incremental refresh pattern can cut warehouse usage by 90% or more.


**Filter combinations that bypass the cache.** Some BI tools cache by exact query hash. If users frequently apply unique filter combinations, every variant is a cache miss. Pre-aggregating common rollups (by day, by region, by product) often helps more than tuning the cache TTL.


A useful exercise: for each dashboard, multiply (refresh frequency in a day) × (average query runtime in seconds) × (warehouse credits per second) and compare it to (daily viewers) × (average load time in seconds) × (credits per second). The cheaper number tells you which strategy is right for that dashboard. Most teams have a handful of expensive outliers that would benefit from a different mode.


## How modern BI tools differ in refresh behavior


A short, honest read of where the major tools sit. This is not a recommendation; it is how their architecture pushes you toward certain choices.


- **Tableau.** Strongly biased toward extracts (Hyper). Live mode exists but most dashboards run faster as extracts. Scheduled refreshes are managed in Tableau Server / Cloud.
- **Power BI.** Three modes: Import (full extract), DirectQuery (live), and Composite. Import is the default and the most common in production. Real-time refresh on Premium can do incremental updates.
- **Looker.** Live-query first. Persistent Derived Tables (PDTs) are the answer when live queries are too slow. Caching is configurable per Explore.
- **Metabase.** Live-query first. Model caching with TTL is the main lever. No native dedicated extract engine.
- **Mode, Hex.** Notebook-style. Each query run is essentially a scheduled refresh; saved reports cache the most recent run.
- **Sigma.** Live queries against the warehouse with materialized views for heavy workloads.
- **Basedash.** Live queries against your database or warehouse, with per-query caching to absorb duplicate reads. Designed for teams that want the freshness of live queries without paying for every page load. A good fit when most viewers want “what is happening right now” against PostgreSQL, MySQL, Snowflake, BigQuery, Redshift, or ClickHouse.
- **Omni, Lightdash.** Hybrid models. Live queries by default, with semantic-layer-aware caching that can serve common rollups from the warehouse cache.


If you are picking a BI tool partly on refresh behavior, ask vendors: (1) what is cached and where, (2) whether row-level security applies to the cache as well as the source, (3) whether refresh failures alert someone, and (4) whether refresh time counts toward your warehouse bill or theirs.


## A short implementation playbook


If you are tuning refresh strategy on an existing BI deployment, the order of operations that usually works:


1. **Audit refresh frequency vs viewership.** Pull a list of every scheduled refresh and the number of dashboard views per day. Dashboards refreshing more than once per ten views are candidates for slowing down. Dashboards being viewed by hundreds of people and refreshing only daily are candidates for live or higher-frequency refresh.
2. **Set freshness budgets per dashboard.** Document the maximum allowable staleness for each one. Most internal dashboards survive at 1 to 24 hours.
3. **Push refresh upstream when possible.** If your dbt or transformation layer only runs every two hours, refreshing the BI dashboard every 15 minutes is theater. Align the BI schedule with the upstream pipeline.
4. **Use incremental refreshes for large extracts.** Most modern BI tools support them. The first rebuild after switching from full to incremental often cuts warehouse usage on that dashboard by 80% or more.
5. **Add a warehouse-level result cache where you can.** Snowflake has one on by default; BigQuery has 24-hour query result caching for free.
6. **Move embedded customer dashboards to a separate path.** Customer-facing dashboards have different concurrency, security, and freshness needs than internal reporting and almost always benefit from a cached extract or[embedded analytics tool](https://www.basedash.com/blog/best-embedded-analytics-platforms-compared-2026) tuned for multi-tenant load.


## Common mistakes


- **Confusing data freshness with refresh frequency.** A dashboard that refreshes every 5 minutes against a table updated daily is still 24 hours stale.
- **Running live queries on top of expensive joins.** Materialize the join in the warehouse or dbt; let the BI tool query a flat table.
- **Forgetting to refresh dimension tables.** Fact tables get attention; dimension tables (customers, products) get stale and cause join misses.
- **Caching past the user’s mental model.** If executives believe a number is live and it is 6 hours old, the next “why didn’t I see this?” conversation will be painful. Show the last-refreshed timestamp prominently.
- **Hourly schedules with 90-minute refresh runtimes.** Always size your schedule against the 95th percentile runtime, not the average.
- **Treating “real time” as a marketing claim.** If you advertise real-time and the dashboard actually refreshes every 15 minutes, users will notice the first time they need it to be live.


## Where Basedash fits


Basedash is built around live queries against your connected database or warehouse, with per-query caching to keep page loads fast even when many people view the same dashboard. This is a good fit for startups and lean teams who want one model — query the source — without operating a separate extract engine. For dashboards that need to scale to hundreds or thousands of concurrent viewers, or that pull from very large historical datasets, a tool with a dedicated cache like Tableau or Power BI may be a better match. The honest answer for most teams is to use one tool for internal live exploration and another for embedded or high-scale customer-facing dashboards. Refresh strategy is the lens that makes that choice obvious.


## FAQ


**How often should a BI dashboard refresh?**


Match the refresh interval to how often a human acts on the dashboard. Daily for board and finance reporting, hourly for marketing and sales, every few minutes (or live) for operational dashboards like support queues. Sub-minute freshness is a streaming problem, not a BI refresh problem.


**What is the difference between a live query and a scheduled refresh?**


A live query runs the SQL against the warehouse every time a user loads the dashboard. A scheduled refresh runs the SQL on a fixed cadence and stores the result; users query the stored result, not the warehouse.


**When should I use Power BI Import vs DirectQuery?**


Use Import (a scheduled extract into VertiPaq) for most dashboards. Use DirectQuery when the data needs to be live, you have row-level security enforced in the source, or the dataset is too large to import. Composite models let you mix the two on a per-table basis.


**Are Tableau extracts the same as caching?**


Not quite. Extracts are full materializations into Tableau’s Hyper engine, designed for fast slice-and-dice on a fixed schema. Caching usually refers to short-lived storage of recent query results. Extracts are what you set up; caching is what happens automatically on top.


**Do scheduled refreshes still cost warehouse credits?**


Yes. Each scheduled refresh runs a query against your warehouse and consumes compute. The savings come from amortizing that query across many dashboard views, not from avoiding warehouse usage entirely.


**How do I get real-time dashboards?**


Real-time analytics needs a streaming source (Kafka, CDC), a fast OLAP engine (ClickHouse, Pinot, Druid, Tinybird), and a tool designed for short polling or streaming subscriptions. General-purpose BI tools can simulate it with frequent refreshes but are not architected for sub-second freshness at scale.
