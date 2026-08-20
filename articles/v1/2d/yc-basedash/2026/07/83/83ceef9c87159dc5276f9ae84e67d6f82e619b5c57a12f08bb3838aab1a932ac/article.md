---
schema_version: "1.0.0"
document_id: "83ceef9c87159dc5276f9ae84e67d6f82e619b5c57a12f08bb3838aab1a932ac"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-dashboards-on-clickhouse-data/"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T14:01:59.621446+00:00"
fetched_at: "2026-07-29T14:02:03.295268+00:00"
content_hash: "sha256:5bcb5cbe1d94b6de26f98b31d94dce81af8a32c480a751aa81958891d41f4f1a"
---

# How to build dashboards on ClickHouse data

To build dashboards on ClickHouse, connect a BI tool over ClickHouse’s HTTP interface (port 8123, or 8443 for TLS) using a dedicated read-only user, query directly against your tables so you keep ClickHouse’s speed instead of extracting data into a slower engine, and read from pre-aggregated materialized views for the heavy rollups. The one rule that trips up most teams: if a table uses` ReplacingMergeTree` or a` CollapsingMergeTree` engine, add the` FINAL` modifier (or query a view that does) or your dashboard will silently return duplicated, pre-merge rows.


This guide is for engineers, analysts, and operators who already run ClickHouse for events, logs, product telemetry, or real-time analytics and want shareable dashboards on top of it. ClickHouse is not a normal transactional database, and treating it like one is where dashboards go wrong. Below: why ClickHouse changes how you connect BI tools, how to set up the connection safely, whether to query raw tables or materialized views, the correctness gotchas that produce wrong numbers, how to keep real-time dashboards from hammering the cluster, and when ClickHouse is the wrong place to build a given dashboard.


## TL;DR


- ClickHouse is a columnar OLAP database built for fast aggregations over huge tables, not single-row lookups or frequent updates. Build dashboards that aggregate, not dashboards that fetch one record at a time.
- Connect over the HTTP interface (8123/8443) or the native protocol (9000/9440 for TLS). Most BI tools use HTTP. See the[ClickHouse HTTP interface docs](https://clickhouse.com/docs/interfaces/http) .
- Create a dedicated read-only user with` GRANT SELECT` on only the databases reporting needs. Do not reuse the` default` account.
- Query directly against ClickHouse rather than extracting into an in-memory engine, or you lose the speed you chose ClickHouse for.
- The big gotcha:` ReplacingMergeTree` and` CollapsingMergeTree` tables deduplicate only during background merges, so a plain` SELECT` can return duplicates. Use` FINAL` at query time for correct results ([ClickHouse docs](https://clickhouse.com/docs/guides/replacing-merge-tree) ).
- Use materialized views to pre-aggregate common dashboard queries at insert time, but remember they are insert triggers, not Postgres-style refreshable caches.
- For tool selection, see our[comparison of the best BI tools for ClickHouse](https://www.basedash.com/blog/best-bi-dashboarding-tools-for-clickhouse-2026) . Options include Grafana, Metabase, Superset, and[Basedash](https://www.basedash.com/) .


## Why ClickHouse changes how you build dashboards


Most BI advice assumes a row-oriented database like PostgreSQL or MySQL, or a cloud warehouse like Snowflake. ClickHouse is different in ways that matter for dashboards.


It stores data by column, not by row. A query that touches three columns of a 60-column table reads only those three columns off disk, which is why ClickHouse can aggregate billions of rows in under a second. The flip side:` SELECT *` on a wide table is wasteful, and single-row lookups by a non-key column are slow. Dashboards should aggregate and filter on the sort key, not fetch individual records.


It sorts data on disk by the table’s` ORDER BY` key and uses a sparse primary index. Filtering on the leading key columns lets ClickHouse skip most of the table. Filtering on a random column forces a wider scan.


It is append-optimized. ClickHouse handles high-volume inserts extremely well but does not do row-level updates and deletes the way a transactional database does. Instead, engines in the MergeTree family reconcile changes in the background. That reconciliation is the source of the correctness gotchas later in this guide.


The practical takeaway: a dashboard that works fine on Postgres can be slow or wrong on ClickHouse if you point the same query patterns at it. Build for aggregation, filter on the sort key, and know which table engine you are reading.


## How to connect a BI tool to ClickHouse


Connecting is straightforward once you know the two interfaces and set up access correctly.


### HTTP vs the native protocol


ClickHouse exposes two main interfaces. The HTTP interface listens on port 8123 (8443 for HTTPS), and the native TCP protocol listens on port 9000 (9440 for TLS), per the[ClickHouse network ports reference](https://clickhouse.com/docs/guides/sre/network-ports) . Most BI and dashboard tools connect over HTTP because it has the widest driver support; some, like Grafana’s official plugin, can use the native protocol for lower overhead.


For ClickHouse Cloud, you connect to the provided HTTPS endpoint on 8443 (or native TLS on 9440) with the credentials from the console. Either way, use an encrypted connection for anything beyond localhost.


### Create a dedicated read-only user


Do not connect BI with the` default` account. Create a user scoped to only what reporting needs. ClickHouse uses SQL-driven role-based access control ([ClickHouse access docs](https://clickhouse.com/docs/operations/access-rights) ):


```text
CREATE   USER   bi_readonly   IDENTIFIED   BY   'strong-password-here'
SETTINGS   readonly   =   1  ;


GRANT   SELECT   ON   analytics.  *   TO   bi_readonly;
```


The` readonly = 1` profile setting blocks writes and prevents the connection from changing server settings.` GRANT SELECT` on a single database (here` analytics` ) keeps the BI connection out of everything else. This makes analytics access auditable and revocable without touching ingestion. Grant on specific databases or tables rather than` *.*` . See the[GRANT statement docs](https://clickhouse.com/docs/sql-reference/statements/grant) for the full syntax.


### Point BI at a replica or a dedicated service, if you can


Heavy dashboard queries and real-time ingestion compete for the same CPU and memory. If you run a ClickHouse cluster with replicas, route reporting to a replica so a dashboard refresh does not slow ingestion. On ClickHouse Cloud, separate compute services can isolate analytics traffic from write traffic. On a single node, keep dashboard queries cheap and filtered so they do not starve inserts.


## Should a dashboard query raw tables, a materialized view, or a projection?


This is the decision that determines whether your dashboards are fast and correct. ClickHouse gives you three layers to query, and the right choice depends on the query pattern.


Layer What it is Best for Watch out for


Raw MergeTree table The base table your data lands in Ad hoc exploration, filtered detail views, anything you query occasionally Full-table aggregations get expensive as the table grows


Incremental materialized view An insert trigger that writes a pre-aggregated result into a target table High-traffic dashboard tiles that always run the same rollup (daily active users, revenue by day) Only reflects rows inserted after it was created; ignores updates and deletes to the source


Projection An alternate sort order or aggregation stored inside the same table, chosen automatically by the optimizer Speeding up queries that filter or group differently from the table’s` ORDER BY` key Adds storage and insert cost; less flexible than a separate view


A ClickHouse materialized view is not the Postgres concept of the same name. It is a trigger that runs your` SELECT` against each newly inserted block and appends the result to a target table, as the[ClickHouse materialized view docs](https://clickhouse.com/docs/materialized-view/incremental-materialized-view) explain. That is what makes real-time pre-aggregation possible at scale, but it also means the view only ever sees new inserts. It does not react to updates or deletes on the source table, and for a materialized view built on a` JOIN` , only inserts to the left-most table trigger it.


The practical pattern for most dashboards:


- Point tiles that always compute the same rollup at a materialized view, so the aggregation happens once at insert time instead of on every dashboard load.
- Query raw tables for the exploratory, filtered, or drill-down views where the query shape changes.
- Reach for projections when a single table needs to serve two different access patterns and you do not want to maintain a separate view.


If your source data is updated or deleted rather than only appended, prefer a refreshable materialized view or scheduled rollup over an incremental one, because incremental views will drift out of sync. The[materialized views versus projections guide](https://clickhouse.com/docs/managing-data/materialized-views-versus-projections) covers the tradeoffs.


## The FINAL gotcha that makes dashboards silently wrong


This is the most important section, because it produces numbers that look plausible but are wrong.


ClickHouse teams often use` ReplacingMergeTree` (to emulate upserts) or` CollapsingMergeTree` (to handle updates by canceling and re-inserting rows). Both engines only remove duplicate or canceled rows during background merges, which happen at an unpredictable time. Until a merge runs, a plain` SELECT` returns the pre-merge state, which means duplicated rows or rows that should have been deleted.


The ClickHouse documentation is explicit about this: deduplication “offers eventual correctness only,” and “queries can, therefore, produce incorrect answers.” To get correct results at query time you add the` FINAL` modifier ([ClickHouse docs](https://clickhouse.com/docs/guides/replacing-merge-tree) ):


```text
-- Can over-count: returns un-merged duplicate rows
SELECT   count  ()   FROM   orders;


-- Correct: FINAL completes the merge logic at query time
SELECT   count  ()   FROM   orders FINAL;
```


Why this bites dashboards specifically: most BI tools generate` SELECT ... FROM table` and have no idea the table is a` ReplacingMergeTree` . The dashboard renders a clean number that is quietly inflated by un-merged duplicates, and it changes over time as merges run in the background, so the same chart shows different totals on different days for no visible reason.


How to handle it:


- Check the engine of every table your dashboards read (` SHOW CREATE TABLE` ). If it is` ReplacingMergeTree` ,` CollapsingMergeTree` , or` VersionedCollapsingMergeTree` , you need deduplication at read time.
- If your BI tool can append` FINAL` automatically for these engines, turn that on. Some tools, including[Basedash](https://www.basedash.com/) , can detect the engine and add it; many cannot.
- If it cannot, create a plain view that bakes in` FINAL` (` CREATE VIEW orders_final AS SELECT * FROM orders FINAL` ) and point the dashboard at the view.
- ` FINAL` has a cost, especially when you are not filtering on primary key columns. Filter on the sort key where you can to keep it cheap, and lean on materialized views for the heaviest aggregations so you are not running` FINAL` over the whole table on every load.


The same care applies to counting distinct values. On high-cardinality columns, ClickHouse’s approximate functions like` uniq` are far faster than exact counts and are usually fine for a dashboard, but decide deliberately whether a tile needs an exact or approximate distinct count rather than letting the tool pick.


## How to keep real-time dashboards from overloading the cluster


ClickHouse is fast, but a dashboard that a hundred people leave open on auto-refresh can generate a surprising amount of load. A few habits keep it healthy.


- **Filter on the sort key.** Queries that constrain the leading` ORDER BY` columns (usually a time range plus a tenant or category) read a fraction of the table. Queries that filter on a random column scan far more.
- **Avoid` SELECT *` .** Select only the columns a chart needs. Because storage is columnar, narrow selects read dramatically less data.
- **Set a sane refresh interval.** A real-time tile does not need to re-query every second for every viewer. Match the refresh to how fast the underlying data actually changes, and cache identical queries so ten people viewing the same dashboard do not fire ten copies of the same scan. Our guide to[dashboard refresh strategies](https://www.basedash.com/blog/dashboard-refresh-strategies-live-queries-scheduled-refreshes-and-cached-extracts) covers live queries, scheduled refreshes, and caching in more depth.
- **Pre-aggregate the expensive tiles.** If a chart always shows the same daily or hourly rollup, compute it once with a materialized view instead of re-scanning raw events on every render.
- **Watch for full scans.** If a dashboard feels slow, run the query with` EXPLAIN` and check how many rows it reads. For a broader approach to diagnosing slow tiles, see our[BI performance playbook](https://www.basedash.com/blog/how-to-make-slow-bi-dashboards-fast-a-performance-playbook) .


## When not to build a dashboard on ClickHouse


ClickHouse is excellent for the analytics it was designed for, and a poor fit for some things dashboards commonly need. Use this as a filter before you build.


- **Single-record lookups.** “Show me everything about customer 4821” is a transactional query. If a dashboard is mostly fetching individual rows by a non-key identifier, that data probably belongs in Postgres, not ClickHouse.
- **Frequently updated small tables.** Dimension tables that change constantly (a live inventory count, an editable status field) do not fit ClickHouse’s append-and-merge model well. Keep them in a transactional store and join at query time only if the join is small.
- **Tiny datasets.** If a table has thousands of rows, not billions, ClickHouse’s advantages disappear and a simpler database is easier to operate.
- **Complex many-table joins.** ClickHouse handles joins, but it shines on wide, denormalized tables. If a dashboard needs to join five normalized tables on every query, either denormalize into ClickHouse at ingestion or build that view elsewhere.


A common healthy pattern is to run two systems: ClickHouse for high-volume event and time-series analytics, and a transactional database (often the app’s Postgres) for operational, record-level views. If you are weighing whether you even need ClickHouse yet, our guide on[when to add a data warehouse](https://www.basedash.com/blog/when-to-add-a-data-warehouse-signals-your-startup-has-outgrown-its-production-database) walks through the signals.


## Tool options for ClickHouse dashboards


Because ClickHouse speaks SQL over HTTP and the native protocol, most modern BI tools can connect. The right one depends on who will use the dashboards.


- **Grafana** is the standard for real-time operational and time-series monitoring, with an official ClickHouse plugin. It is strong for engineers watching live metrics, weaker for business self-service.
- **Metabase** and **Apache Superset** are open-source options with ClickHouse drivers, good for teams that want to self-host and have SQL skills on hand.
- **Basedash** fits teams that want non-technical people to explore ClickHouse data and ask follow-up questions in plain English without learning ClickHouse SQL, and it can handle the` FINAL` gotcha automatically when it detects the relevant engines.


For a detailed, honest comparison across integration depth, AI features, and pricing, see our[best BI tools for ClickHouse guide](https://www.basedash.com/blog/best-bi-dashboarding-tools-for-clickhouse-2026) . If you also have application data in Postgres, our companion guide on[building dashboards on Supabase data](https://www.basedash.com/blog/how-to-build-analytics-dashboards-on-your-supabase-data) covers the row-oriented side of the same problem.


## A setup checklist for ClickHouse dashboards


Use this as a practical sequence when you add a BI tool to a ClickHouse deployment.


1. **Create a dedicated read-only user** with` readonly = 1` and` GRANT SELECT` on only the databases reporting needs.
2. **Connect over HTTPS (8443) or native TLS (9440)** and never expose the` default` account to a BI tool.
3. **Inventory your table engines.** Note every` ReplacingMergeTree` or` CollapsingMergeTree` table so you know where` FINAL` is required.
4. **Decide the query layer per tile.** Raw table for exploration and drill-down, materialized view for always-on rollups, projection for a second access pattern on one table.
5. **Handle` FINAL` explicitly.** Turn on automatic` FINAL` if your tool supports it, or point dashboards at views that include it.
6. **Filter on the sort key and select only needed columns** so queries read the minimum data.
7. **Set refresh intervals and query caching** so real-time dashboards do not overload the cluster.
8. **Define core metrics once.** Agree how “active user” or “revenue” is calculated and put it in the BI tool’s model or a shared view so every dashboard matches. This is the foundation of[self-serve analytics](https://www.basedash.com/blog/self-serve-analytics-a-practical-guide-to-bi-adoption-across-your-organization) .


## FAQ


### How do I connect a BI tool to ClickHouse?


Point the tool at ClickHouse’s HTTP interface (port 8123, or 8443 for HTTPS) or the native protocol (9000, or 9440 for TLS), using a dedicated read-only user created with` CREATE USER ... SETTINGS readonly = 1` and` GRANT SELECT` on the databases reporting needs. Most BI tools connect over HTTP. For ClickHouse Cloud, use the HTTPS endpoint and credentials from the console. Always use an encrypted connection outside localhost.


### Why does my ClickHouse dashboard show duplicate or inflated numbers?


Almost always because the table uses` ReplacingMergeTree` or` CollapsingMergeTree` , which only remove duplicate rows during background merges. Until a merge runs, a plain` SELECT` returns un-merged rows, so counts and sums are inflated and change over time. Add the` FINAL` modifier at query time, enable your tool’s automatic` FINAL` for these engines, or query a view that includes` FINAL` .


### Should I query raw ClickHouse tables or materialized views for dashboards?


Query materialized views for tiles that always run the same aggregation, since the rollup is computed once at insert time. Query raw tables for exploratory and drill-down views where the query shape changes. Remember that ClickHouse incremental materialized views are insert triggers, so they only reflect newly inserted rows and ignore updates and deletes to the source table.


### Is ClickHouse good for real-time dashboards?


Yes. ClickHouse is built for real-time analytics on large, high-ingest datasets and can aggregate billions of rows in under a second. Keep dashboards efficient by filtering on the table’s sort key, selecting only needed columns, pre-aggregating heavy tiles with materialized views, and setting refresh intervals plus query caching so many concurrent viewers do not overload the cluster.


### When should I not use ClickHouse for a dashboard?


Avoid it for single-record lookups, frequently updated small dimension tables, tiny datasets, and dashboards that join many normalized tables on every query. Those fit a transactional database like PostgreSQL better. A common pattern is to run ClickHouse for event and time-series analytics and keep operational, record-level views on your application database.
