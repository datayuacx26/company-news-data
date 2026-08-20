---
schema_version: "1.0.0"
document_id: "86fb8a4e55eb4cb268cf1e6649bb110eafcbd166714d0d2c911d674a83b02b7b"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-dashboards-on-databricks-data/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T14:57:54.915852+00:00"
fetched_at: "2026-08-12T14:58:00.418373+00:00"
content_hash: "sha256:49d0a299d5c3536b9e4e35df4f42600b6b461dd788bb8d290b3fabea232728cc"
---

# How to build dashboards on Databricks data

To build dashboards on Databricks, connect a BI tool to a Databricks SQL warehouse (not an all-purpose compute cluster) using the warehouse’s server hostname and HTTP path, authenticate with a personal access token or OAuth, and let Unity Catalog govern what the connection can read. Point tiles at your curated gold tables rather than raw ingested data, and lean on the built-in result cache so repeated dashboard loads do not recompute. The one choice that trips teams up: the warehouse type. A classic or pro SQL warehouse takes roughly four minutes to cold-start, so the first person to open a dashboard against a stopped warehouse stares at a spinner and assumes the dashboard is broken. A serverless SQL warehouse starts in about 2 to 6 seconds, which is why Databricks recommends it for BI.


This guide is for data engineers, analysts, and operators who already run Databricks as their lakehouse and want shareable dashboards on top of it. Databricks is not a plain SQL database. It is a lakehouse: Delta Lake tables in cloud storage, queried by separate compute you turn on and off. That separation of storage and compute is what makes dashboards on Databricks fast when set up well and slow or expensive when set up badly. Below: how to connect a BI tool safely, which warehouse type to use, which layer a tile should query, how to control cost, how Unity Catalog keeps dashboards governed, and when Databricks is the wrong place for a dashboard.


## TL;DR


- Connect BI to a **Databricks SQL warehouse** , not an all-purpose (interactive) cluster. Grab the Server hostname, Port, and HTTP path from the warehouse’s[Connection Details tab](https://docs.databricks.com/aws/en/integrations/compute-details) and authenticate with a token or OAuth.
- Use a **serverless SQL warehouse** for dashboards. It starts in[2 to 6 seconds versus roughly 4 minutes](https://docs.databricks.com/aws/en/compute/sql-warehouse/warehouse-types) for pro or classic, and its intelligent workload management autoscales when many viewers hit it at once.
- Let[Unity Catalog](https://docs.databricks.com/aws/en/data-governance/unity-catalog/) govern access. Create a dedicated service principal, grant it` SELECT` on only the reporting schemas, and it inherits row filters and column masks automatically.
- Point dashboard tiles at **curated gold tables** , not raw bronze ingestion, so each tile reads a clean, pre-modeled result.
- Push queries down to the warehouse rather than extracting Delta tables into a separate engine, or you lose freshness, duplicate storage, and drop out of Unity Catalog governance.
- Control cost with a tight **auto-stop** , serverless billing, and the built-in[result cache](https://docs.databricks.com/aws/en/sql/user/queries/query-caching) , which serves identical repeated queries without re-running them.
- For tool selection, see our[comparison of the best BI tools for Databricks](https://www.basedash.com/blog/best-bi-dashboarding-tools-for-databricks-2026) . Options include Databricks AI/BI, Tableau, Power BI, Sigma, and[Basedash](https://www.basedash.com/) .


## Why Databricks changes how you build dashboards


Most BI advice assumes a database that is always on, like PostgreSQL. Databricks separates storage from compute, and that changes three things about how you build dashboards.


**Storage and compute are decoupled.** Your data lives as Delta Lake tables in cloud object storage. Nothing queries it until you start a SQL warehouse, a compute resource that you size, start, and stop. This is why a dashboard can be instant or can hang for minutes: it depends entirely on whether the warehouse behind it is already running.


**Data usually arrives in layers.** Most Databricks teams follow a medallion pattern: raw data lands in bronze tables, gets cleaned into silver, and is aggregated into business-ready gold tables. Dashboards should read gold. Pointing a tile at raw bronze data means every load re-does joins and cleanup that a gold table already did once.


**Governance is centralized in Unity Catalog.** Unity Catalog sits beneath every query, enforcing access control, tracking lineage, and using a three-level` catalog.schema.table` namespace. It is[automatically enabled for workspaces created after November 8, 2023](https://docs.databricks.com/aws/en/data-governance/unity-catalog/) . The practical consequence: if your BI tool queries Databricks directly, one set of permissions covers the warehouse and the dashboard. If it extracts data out, those permissions no longer apply.


The takeaway: pick the right warehouse, read from gold tables, and keep queries inside Databricks so governance holds. The rest of this guide is how.


## How to connect a BI tool to Databricks


Connecting comes down to three things: the right compute target, the connection string, and access control.


### Point at a SQL warehouse, not an all-purpose cluster


Databricks has two kinds of compute you could technically connect to. All-purpose (interactive) clusters are built for notebooks and ad hoc data science; SQL warehouses are built for SQL and BI. Send dashboard traffic to a SQL warehouse. All-purpose compute is billed at a higher rate and is not tuned for the many small concurrent queries a dashboard generates, so using it for BI is both slower under load and more expensive.


### Get the connection details


Every SQL warehouse exposes a **Connection Details** tab with the values a BI tool needs: Server hostname, Port, and HTTP path,[per the Databricks connection docs](https://docs.databricks.com/aws/en/integrations/compute-details) . Most tools connect with the Databricks JDBC/ODBC driver or the native Databricks connector and ask for exactly those three fields plus credentials. For authentication, a personal access token works, but OAuth with a service principal is the better production choice because it is not tied to a single person’s account and can be rotated centrally.


### Create a dedicated, least-privilege identity


Do not connect BI as a workspace admin. Create a service principal for reporting and grant it read access to only the schemas dashboards need, using Unity Catalog’s three-level namespace:


```text
-- grant the reporting identity read access to a curated schema
GRANT   USE   CATALOG   ON   CATALOG   analytics   TO   `bi-reporting-sp`  ;
GRANT   USE   SCHEMA    ON   SCHEMA    analytics  .  gold   TO   `bi-reporting-sp`  ;
GRANT   SELECT        ON   SCHEMA    analytics  .  gold   TO   `bi-reporting-sp`  ;
```


Grant on the specific catalog and schema reporting needs, not on everything. Because Unity Catalog governs the warehouse itself, any row filters or column masks defined on those tables apply automatically to the dashboard, without you re-implementing them in the BI layer.


## Which SQL warehouse type should power your dashboards?


This is the decision that most often makes a Databricks dashboard feel broken. Databricks SQL offers three warehouse types (plus a beta real-time option), and they differ sharply in how fast they start and how they handle a crowd.


Warehouse type Cold start Autoscaling Best for dashboards


Serverless ~2 to 6 seconds Intelligent workload management, fast The default choice for BI and dashboards


Pro ~4 minutes Slower, no IWM Regions without serverless, or custom networking needs


Classic ~4 minutes Entry-level Basic interactive exploration, not busy dashboards


Lakehouse Real-Time (beta) Serverless High concurrency Sub-second reads for embedding to many end users


These figures come from the[Databricks SQL warehouse types docs](https://docs.databricks.com/aws/en/compute/sql-warehouse/warehouse-types) , which recommend serverless for business intelligence because of its rapid startup, efficient IO, and ability to autoscale when queries queue.


Why the cold start matters so much: a warehouse auto-restarts the moment[a JDBC/ODBC connection or a dashboard hits it while stopped](https://docs.databricks.com/aws/en/compute/sql-warehouse/) . On serverless, the viewer waits a few seconds. On classic or pro, they wait minutes, conclude the dashboard is down, and refresh repeatedly, which does nothing but extend the wait. For anything more than a handful of scheduled users, use serverless. Reserve pro or classic for regions where serverless is unavailable or when you specifically need the compute to live in your own cloud account for network federation. The beta Lakehouse Real-Time type is aimed at embedding dashboards for hundreds or thousands of concurrent external users, a different problem than internal BI.


## Which layer should a dashboard tile query?


On Databricks, the right thing to query is almost always a curated table, not raw data and not a giant view stacked on raw data.


Source What it is Best for Watch out for


Gold table A pre-aggregated, business-ready Delta table High-traffic tiles that always show the same rollup (daily revenue, active users) Needs a pipeline to keep it fresh


Silver table Cleaned, joined, but not yet aggregated Flexible exploration and drill-down where the question changes Heavier per query than gold


Bronze table Raw ingested data Debugging pipelines, not dashboards Every dashboard load repeats cleanup work


Result cache Databricks’ cache of prior query results Many people opening the same unchanged dashboard Only hits on identical query text; invalidated when tables change


The pattern for most dashboards: build gold tables that pre-compute the rollups your tiles show over and over, point high-traffic tiles at those, and reserve silver tables for exploratory views where the query shape varies. Let caching handle the repeat-load case. Databricks SQL keeps a[result cache](https://docs.databricks.com/aws/en/sql/user/queries/query-caching) with a 24-hour lifecycle that is available to JDBC/ODBC clients and invalidated automatically when underlying tables change, so ten people opening the same dashboard cost close to one query. A separate disk cache keeps recently read files on local SSD to speed subsequent scans.


If you are debating whether to model metrics in Delta tables, dbt, a semantic layer, or the BI tool, our guide on[where to define business metrics](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) walks through the tradeoffs.


## How to control Databricks cost from dashboards


Because compute is separate and metered, dashboards translate directly into spend. Databricks bills SQL warehouse compute in DBUs for the time a warehouse runs (serverless is billed by Databricks; pro and classic also incur the underlying cloud VM cost in your own account, per the[Databricks SQL pricing page](https://www.databricks.com/product/pricing/databricks-sql) ). A warehouse left running with tiles on aggressive auto-refresh is a real bill.


Keep it in check:


- **Set a tight auto-stop.** Configure the warehouse to stop after a short idle window. Serverless restarts in seconds, so a low auto-stop costs viewers almost nothing while saving idle compute. This is the single biggest lever.
- **Use serverless.** Beyond startup speed, its intelligent workload management scales compute to demand and back down quickly, so you are not paying for over-provisioned capacity between traffic spikes.
- **Let the result cache work.** Stable, identical queries return from cache without re-running. Avoid injecting volatile values like` now()` to the millisecond into tile queries, which breaks cache reuse.
- **Read gold, not bronze.** A pre-aggregated gold table turns an expensive multi-table scan into a cheap read on every load.
- **Cap refresh frequency.** A dashboard rarely needs sub-minute data. Match the refresh interval to how fast the data actually changes. Our[dashboard refresh strategies guide](https://www.basedash.com/blog/dashboard-refresh-strategies-live-queries-scheduled-refreshes-and-cached-extracts) covers live queries, scheduled refreshes, and caching in more depth.
- **Never point BI at an all-purpose cluster.** It costs more per DBU and keeps expensive compute alive for query patterns a SQL warehouse handles more cheaply.


If dashboards feel slow even after this, our[performance playbook for slow BI dashboards](https://www.basedash.com/blog/how-to-make-slow-bi-dashboards-fast-a-performance-playbook) covers the query-side fixes.


## How Unity Catalog keeps dashboards governed


The biggest governance advantage of building dashboards directly on Databricks is that you do not have to rebuild permissions in the BI tool.[Unity Catalog](https://docs.databricks.com/aws/en/data-governance/unity-catalog/) enforces access control, row and column filters, and lineage beneath every query the warehouse runs.


- **One permission model.** Grant the reporting service principal` SELECT` on the gold schema, and it sees exactly what those grants allow, including any row-level filters and column masks defined on the tables. The dashboard cannot show more than the connection is allowed to read.
- **Lineage covers dashboards.** Unity Catalog tracks how data flows from source tables through to dashboards, so you can trace a suspicious number on a tile back to the tables and transformations behind it.
- **Extraction breaks all of this.** The moment a tool copies Delta tables into its own engine, Unity Catalog no longer applies to that copy. You now maintain a second governance model and a staleness window. Pushing queries down to the warehouse keeps one copy and one set of rules.


For a deeper treatment of permissions in dashboards generally, see our guide on[row-level security in BI tools](https://www.basedash.com/blog/best-bi-tools-for-row-level-security-compared-2026) .


## When not to build a dashboard on Databricks


Databricks is excellent at large analytical queries and a poor fit for a few things dashboards commonly need. Use this as a filter before you build.


- **Single-record lookups.** “Show everything about order 88213” is a transactional query. If a dashboard mostly fetches individual rows by ID, that data belongs in your app’s Postgres or MySQL, not a lakehouse.
- **Tiny datasets.** If a table has thousands of rows, not millions or billions, a small always-on database is simpler and cheaper than spinning up warehouse compute.
- **A metric that lives in one operational system.** If everything a dashboard needs is already in Stripe or your product database, querying that source directly can be faster than piping it through the lakehouse first.
- **Sub-second embedding at massive scale before you are ready.** High-concurrency customer-facing embedding is its own problem; the beta Lakehouse Real-Time warehouse targets it, but for standard internal BI a regular serverless warehouse is the right call.


If you are still deciding whether you even need a lakehouse or warehouse yet, our guide on[when to add a data warehouse](https://www.basedash.com/blog/when-to-add-a-data-warehouse-signals-your-startup-has-outgrown-its-production-database) walks through the signals.


## Tool options for Databricks dashboards


Because Databricks SQL warehouses speak SQL over a standard endpoint, most BI tools can connect. The right one depends on who will use the dashboards.


- **Databricks AI/BI** is the native option, with zero connector setup and full Unity Catalog compliance, but it only reads Databricks data.
- **Tableau** and **Power BI** are the incumbents for rich visual analytics, strong when you have analysts to build and maintain workbooks.
- **Sigma** offers a spreadsheet interface on lakehouse data, which fits finance and planning teams.
- **Basedash** fits teams that want non-technical people to explore Databricks data and ask follow-up questions in plain English, pushing queries directly to the SQL warehouse without extracting data into a separate engine.


For an honest comparison across Unity Catalog depth, AI features, and pricing, see our[best BI tools for Databricks guide](https://www.basedash.com/blog/best-bi-dashboarding-tools-for-databricks-2026) .


## A setup checklist for Databricks dashboards


Use this as a practical sequence when you add a BI tool to Databricks.


1. **Create a serverless SQL warehouse** for BI and set a tight auto-stop so idle compute shuts down quickly.
2. **Create a dedicated service principal** for reporting and grant it` USE CATALOG` ,` USE SCHEMA` , and` SELECT` on only the gold schema dashboards read.
3. **Copy the Server hostname, Port, and HTTP path** from the warehouse’s Connection Details tab and connect the tool with OAuth or a token.
4. **Point tiles at gold tables** , not bronze ingestion or raw views, so each load reads a clean pre-modeled result.
5. **Build gold tables for the heaviest rollups** so repeated dashboard queries become cheap reads.
6. **Confirm Unity Catalog row filters and column masks** are set on sensitive tables; they apply to the dashboard automatically.
7. **Cap refresh intervals and rely on the result cache** so identical loads do not re-run and the warehouse can idle down.
8. **Define core metrics once.** Agree how “active user” or “revenue” is calculated and put it in a gold table or shared model so every dashboard matches. This is the foundation of[self-serve analytics](https://www.basedash.com/blog/self-serve-analytics-a-practical-guide-to-bi-adoption-across-your-organization) .


## FAQ


### How do I connect a BI tool to Databricks?


Connect to a Databricks SQL warehouse rather than an all-purpose cluster. Open the warehouse’s Connection Details tab to get the Server hostname, Port, and HTTP path, then enter those in your BI tool along with credentials, using the Databricks JDBC/ODBC driver or native connector. Authenticate with a personal access token or, for production, OAuth with a service principal. Grant that identity` SELECT` on only the schemas reporting needs so Unity Catalog governs what the dashboard can read.


### Should I use a serverless, pro, or classic SQL warehouse for dashboards?


Use serverless for almost all dashboards. It starts in about 2 to 6 seconds, autoscales when many viewers hit it, and Databricks recommends it for business intelligence. Pro and classic warehouses take roughly four minutes to cold-start, so the first viewer to open a dashboard against a stopped warehouse waits minutes and often assumes it is broken. Reserve pro or classic for regions without serverless or when you need compute in your own cloud account for network federation.


### Why is my Databricks dashboard slow to load the first time?


The most common cause is a stopped warehouse cold-starting. A SQL warehouse auto-restarts when a dashboard or JDBC connection hits it, and a classic or pro warehouse takes about four minutes to be ready. Switch to a serverless warehouse, which starts in seconds, and set a sensible auto-stop so it idles down without leaving viewers waiting. After that, pre-aggregate heavy tiles into gold tables and rely on the result cache.


### Do I need to copy Databricks data into my BI tool?


No, and you generally should not. Pushing queries directly to a SQL warehouse keeps one copy of the data, avoids a staleness window, and keeps Unity Catalog permissions in force. Extracting Delta tables into a separate engine duplicates storage, adds lag, and drops the dashboard out of Unity Catalog governance, so you end up maintaining a second set of permissions. Use a serverless warehouse and caching to make direct queries fast instead.


### How do I control the cost of dashboards on Databricks?


Set a tight auto-stop so the warehouse shuts down when idle, and use serverless so it scales to demand and restarts in seconds. Point tiles at pre-aggregated gold tables so each load is a cheap read, cap refresh intervals to match how fast data changes, and let the result cache serve identical repeated queries. Never route BI to an all-purpose cluster, which costs more per DBU and is not tuned for dashboard query patterns.
