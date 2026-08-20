---
schema_version: "1.0.0"
document_id: "49932a047a6b80bcca6b855fc5d8ca8e3e2edde1814d0aaf8f86adcbb101e801"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-12b6d71fe86e"
canonical_url: "https://www.windmill.dev/blog/launch-week-data-tables-ducklake"
published_at: "2026-03-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:07.070377+00:00"
fetched_at: "2026-07-28T22:17:33.975741+00:00"
content_hash: "sha256:36f505c3da68c2db4e67b97ca9d2b36fa868a18aa37e86cbbbf42d0f5fa74387"
---

# Data Tables & Ducklake: managed storage for workflows

**Day 2 of[Windmill launch week](https://www.windmill.dev/launch-week-march-2026) .** We are shipping two new storage primitives: **Data Tables** for relational data with managed SQL, and **Ducklake** for massive datasets backed by S3.


## The problem​


Workflow engines typically punt on data storage. You end up managing separate databases, connection strings, credential rotation, and permission models. Your orchestration layer knows how to run code but has no opinion about where results go.


For analytics workloads, the gap is wider. Teams default to managed data warehouses (Snowflake, BigQuery) that charge per query and live entirely outside the orchestration layer. The result: two systems, two permission models, and a lot of glue code to move data between them.


We wanted Windmill users to go from "I have a script" to "I have a script that reads and writes data" without leaving the platform.


## Data Tables: managed SQL with one line​


Data Tables give you a workspace-scoped PostgreSQL layer where credentials are managed by Windmill. Users write SQL; they never see connection strings.


- TypeScript
- Python
- DuckDB


```text
import     *     as   wmill   from     'windmill-client'  ;         export     async     function     main  (  user_id  :     string  )     {     	  let   sql   =   wmill  .  datatable  (  )  ;        	  // String interpolation is safe: auto-converted to parameterized queries     	  let   friend   =     await   sql  `  SELECT     *     FROM   friend   WHERE   id   =     ${  user_id  }  `  .  fetchOne  (  )  ;        	  return   friend  ;      }
```


```text
import   wmill        def     main  (  user_id  :     str  )  :         db   =   wmill  .  datatable  (  )              # Positional arguments for safe parameterized queries         friend   =   db  .  query  (  'SELECT * FROM friend WHERE id = $1'  ,   user_id  )  .  fetch_one  (  )              return   friend
```


```text
-- $user_id (bigint)        ATTACH   'datatable'     AS   dt  ;      USE   dt  ;         SELECT     *     FROM   friend   WHERE   id   =   $user_id  ;
```


That's it. One import, one function call, standard SQL. TypeScript uses tagged template literals that are automatically converted to parameterized queries, so string interpolation is safe by default. Python uses positional arguments (` $1` ,` $2` ). DuckDB uses the native` ATTACH` syntax.


### Why PostgreSQL​


We chose PostgreSQL because:


- It is the most widely understood SQL dialect. No new query language to learn.
- Battle-tested ACID guarantees out of the box.
- DuckDB can attach to Postgres natively, so Data Tables and Ducklake share the same query surface.


### Organizing with schemas​


We recommend using one or a few Data Tables per workspace and organizing data with schemas:


```text
let   sql   =   wmill  .  datatable  (  ':analytics'  )  ;      await   sql  `  SELECT     *     FROM   events  `  ;     // refers to analytics.events
```


This keeps things clean without spinning up separate databases for every project.


## Why we built it this way​


Three design choices drove the architecture:


**Workspace scoping.** Data Tables are scoped to a workspace. All members can read and write. This removes the need for database-level user management while keeping workspaces isolated from each other.


**Credential opacity.** Users never see or manage database connection strings. Windmill handles credentials internally. This eliminates a whole class of credential-rotation bugs and accidental leaks.


**Bring your own Postgres.** You attach a workspace Postgres resource to the data table. Windmill manages credentials internally so users never see connection strings. This gives you full control over database hosting while keeping the API simple.


## Asset tracking and data lineage​


When you reference a Data Table in a script, Windmill automatically parses your code and detects which tables you read from and write to.


Assets appear as nodes in flows, giving you a visual data dependency graph. Click any asset node to open it in the Database Studio and inspect the data directly.


## Ducklake: S3-backed data lakehouse​


Data Tables are great for transactional data. But some workloads produce millions of rows that do not belong in a relational database. For those, we built Ducklake support directly into Windmill.


[Ducklake](https://ducklake.select/) stores data as Parquet files in S3 and keeps a metadata catalog in Postgres. You query it with standard SQL through DuckDB.


The API follows the same pattern as Data Tables:


- TypeScript
- Python
- DuckDB


```text
import     *     as   wmill   from     'windmill-client'  ;         export     async     function     main  (  user_id  :     string  )     {     	  let   sql   =   wmill  .  ducklake  (  )  ;        	  let   friend   =     await   sql  `  SELECT     *     FROM   friend   WHERE   id   =     ${  user_id  }  `  .  fetchOne  (  )  ;        	  return   friend  ;      }
```


```text
import   wmill        def     main  (  user_id  :     str  )  :         dl   =   wmill  .  ducklake  (  )            friend   =   dl  .  query  (  'SELECT * FROM friend WHERE id = $id'  ,     id  =  user_id  )  .  fetch_one  (  )              return   friend
```


```text
-- $user_id (bigint)        ATTACH   'ducklake'     AS   dl  ;      USE   dl  ;         SELECT     *     FROM   friend   WHERE   id   =   $user_id  ;
```


### Real-world example: sentiment analysis pipeline​


Here is a DuckDB script that receives analyzed messages (e.g., from an LLM sentiment analysis step in a flow) and inserts them into a Ducklake table. Each insert creates a new Parquet file in S3 and updates the catalog metadata.


```text
-- $messages (json[])        ATTACH   'ducklake://main'     AS   dl  ;      USE   dl  ;         CREATE     TABLE     IF     NOT     EXISTS   messages   (       content STRING   NOT     NULL  ,       author STRING   NOT     NULL  ,         date   STRING   NOT     NULL  ,       sentiment STRING     )  ;         CREATE     TEMP     TABLE   new_messages   AS         SELECT           value  -  >>  'content'     AS   content  ,           value  -  >>  'author'     AS   author  ,           value  -  >>  'date'     AS     date  ,           value  -  >>  'sentiment'     AS   sentiment        FROM   json_each  (  $messages  )  ;         INSERT     INTO   messages        SELECT     *     FROM   new_messages  ;
```


Under the hood, TypeScript and Python integrations run DuckDB inline within the same worker. No separate job is spawned, so the overhead is minimal.


## Why Ducklake over a data warehouse​


Ducklake Managed warehouse


**Storage format** Parquet on your S3 Proprietary


**Query engine** DuckDB (single-node, in-process) Managed cluster


**Catalog** Postgres (already in your stack) Proprietary


**Cost model** S3 storage pricing Per-query compute pricing


**Lock-in** None: standard Parquet files High


Ducklake gives you a data lakehouse without new infrastructure. Your data stays in an open format on storage you control. The catalog metadata lives in the same Postgres that Windmill already uses. And DuckDB handles analytical queries on a single node without cluster management.


## Database Studio​


Both Data Tables and Ducklake are browsable through the Database Studio, a visual interface for inspecting schemas, editing rows, and running SQL.


## Building data pipelines​


Data Tables and Ducklake are designed to work with Windmill[flows](https://www.windmill.dev/docs/flows/flow_editor) . A typical data pipeline looks like this:


1. **Extract** : a script pulls data from an external source (API, webhook, database).
2. **Transform** : one or more steps clean, enrich, or aggregate the data using Python, TypeScript, or SQL.
3. **Load** : the result is written to a Data Table for operational use or to Ducklake for analytical queries.


Because each step is a standalone script, you can mix languages freely. For example, fetch data with TypeScript, run a sentiment analysis in Python, and insert the results with a DuckDB query into Ducklake. Windmill handles the orchestration, retries, error handling, anddata lineage tracking.


You can also schedule pipelines with[cron triggers](https://www.windmill.dev/docs/core_concepts/scheduling) , react to events with[webhooks](https://www.windmill.dev/docs/core_concepts/webhooks) , or chain them with other flows.


[Data pipelines docs Build ETL and data pipelines on Windmill.](https://www.windmill.dev/docs/core_concepts/data_pipelines)[Data pipelines use case See how teams use Windmill for data pipelines.](https://www.windmill.dev/use-cases/data-pipelines)


## Getting started​


**Data Tables:**


1. Go to workspace settings, then Data Tables.
2. Add your own Postgres resource.
3. Use` wmill.datatable()` in any script.


**Ducklake:**


1. Configure a workspace S3 storage.
2. Go to workspace settings, then Object storage and set up a Ducklake.
3. Use` wmill.ducklake()` in any script.


[Data Tables Store and query relational data with near-zero setup.](https://www.windmill.dev/docs/core_concepts/persistent_storage/data_tables)[Ducklake Store massive datasets in S3 and query with SQL.](https://www.windmill.dev/docs/core_concepts/persistent_storage/ducklake)


## Benchmark: Windmill + Ducklake vs Airflow + Snowflake​


Most startups don't need a terabyte-scale data warehouse. If your data fits in 10 GB to 1 TB, you can run analytical workloads on Ducklake at a fraction of the cost of Airflow + Snowflake while keeping full control over your data.


We ran the[TPC-DS](https://www.tpc.org/tpcds/) benchmark at three scale factors (10 GB, 100 GB, 1 TB) to compare Windmill + Ducklake against Airflow + Snowflake, the most common open-source orchestrator paired with a managed data warehouse.


### Why TPC-DS?​


TPC-DS simulates a retail company's data warehouse, which mirrors a real ETL pipeline. The benchmark runs 52 tasks organized in 5 stages:


Stage Tasks What it does


Ingest 24 Load raw data from 24 source tables (sales, returns, inventory, customers, products...)


Validate 8 Check referential integrity and data quality across fact and dimension tables


Denormalize 3 Join fact tables with dimensions to create analytics-ready tables


Aggregate 6 Compute business metrics: daily sales, customer LTV, return rates, channel comparison...


Query 10 Run 10 analytical queries (TPC-DS queries 3, 7, 19, 27, 34, 43, 46, 53, 67, 79)


### Pricing assumptions​


- Airflow + Snowflake: Snowflake Standard tier at $2/credit. Enterprise is $3-4/credit, Business Critical is $4-5/credit. Airflow orchestration cost excluded (adds $100-500/month for managed Airflow).
- AWS EC2: On-demand pricing in us-east-1


Snowflake Warehouse Credits/hour $/hour


Small 2 $4


Large 8 $16


AWS Instance vCPUs Memory $/hour


m6i.4xlarge 16 64 GB $0.77


m6a.8xlarge 32 128 GB $1.38


m6a.16xlarge 64 256 GB $2.76


r6a.8xlarge 32 256 GB $1.81


### SF10: 10 GB dataset​


At 10 GB, Windmill matches Snowflake's speed while costing 2-2.5x less.


#### TPC-DS SF10 (10 GB)


Configuration Time Cost $/hour


Airflow + Snowflake Small 1m 25s $0.09 $4.00


Windmill 2× m6i.4xlarge 1m 26s $0.04 $1.54


Windmill 2× m6a.8xlarge 1m 7s $0.05 $2.76


With 2x m6a.8xlarge nodes, Windmill completes the benchmark in 67 seconds, 21% faster than Snowflake Small (85s). At comparable speed (2x m6i.4xlarge), Windmill costs $0.04 vs Snowflake's $0.10.


### SF100: 100 GB dataset​


At 100 GB, the cost advantage becomes more significant. Windmill runs at 3x lower cost while delivering competitive performance.


#### TPC-DS SF100 (100 GB)


Configuration Time Cost $/hour


Airflow + Snowflake Small 11m 51s $0.79 $4.00


Airflow + Snowflake Large 3m 14s $0.86 $16.00


Windmill 1× m6a.8xlarge 10m 52s $0.25 $1.38


Windmill 1× m6a.16xlarge 6m 31s $0.30 $2.76


Windmill 3× m6a.8xlarge 4m 21s $0.30 $4.15


Windmill 3× m6a.16xlarge 2m 37s $0.36 $8.29


Key findings:


- Windmill 3x m6a.16xlarge (157s, $0.36) is 19% faster than Snowflake Large (194s, $0.86) while costing 2.4x less
- Windmill 3x m6a.8xlarge (261s, $0.30) is 35% slower than Snowflake Large but costs 2.9x less
- Windmill 1x m6a.16xlarge (391s, $0.30) runs 1.8x faster than Snowflake Small (711s, $0.79)
- Per dollar spent, Windmill delivers 2.7x more throughput than Snowflake Small


### SF1000: 1 TB dataset​


At terabyte scale, Snowflake's distributed architecture shows its strength. However, Windmill remains cost-competitive.


#### TPC-DS SF1000 (1 TB)


Configuration Time Cost $/hour


Airflow + Snowflake Large 27m 51s $7.43 $16.00


Windmill 3× r6a.8xlarge 1h 10m $6.35 $5.44


Snowflake Large completes in 28 minutes vs Windmill's 70 minutes, but Windmill still costs 15% less ($6.35 vs $7.43). If you're processing terabyte-scale data daily, Snowflake's performance advantage matters. For occasional large queries, Windmill's cost savings may be worth the extra time.


note


Benchmarks ran on AWS us-east-1. Snowflake pricing uses Standard tier ($2/credit). Enterprise and Business Critical tiers cost 50-150% more, making Windmill's cost advantage even greater. Airflow orchestration costs (managed Airflow or self-hosted) add to the total cost of ownership.


### Why not Airflow + Pandas?​


Some teams try to avoid data warehouse costs by running Pandas locally. This approach breaks down quickly. We ran the same TPC-DS benchmark with Airflow + Pandas on a powerful local machine (AMD Ryzen 9 9955HX, 64 GB RAM).


#### TPC-DS SF10 (10 GB): Pandas vs Ducklake


At just 10 GB, Airflow + Pandas takes 47 minutes compared to Windmill's 67 seconds, a 42x slowdown. The Pandas run required limiting concurrency to 1 task at a time to avoid out-of-memory crashes. Even with 64 GB of RAM, Pandas loaded the store_sales DataFrame at ~25 GB due to decimal type overhead, forcing heavy swap usage.


Beyond 10 GB, Pandas becomes unusable. Memory consumption grows linearly with data size, but performance degrades exponentially as the system swaps to disk. At 100 GB, you would need 500+ GB of RAM to avoid swapping, and even then you are still limited to single-threaded execution on one machine.


Ducklake avoids these problems entirely. DuckDB processes data in streaming chunks, uses columnar Parquet compression, and parallelizes across all available cores. The data stays on S3, not in RAM.


### The real comparison: data sovereignty​


Beyond raw performance, consider what you give up with Airflow + Snowflake:


Windmill + Ducklake Airflow + Snowflake


**Data location** Your S3 bucket Snowflake-managed storage


**Compute location** Your infrastructure Snowflake-managed clusters


**Data format** Open Parquet files Proprietary


**Query visibility** Full control Runs on third-party infra


**Exit cost** None (standard Parquet) Data export fees


**Orchestration** Built-in, no extra cost Separate service ($100–500/mo)


With Ducklake, your data never leaves your environment. Queries execute on your nodes, against Parquet files in your S3 bucket. No vendor lock-in, no data egress fees, no loss of control.


## What's next​


Tomorrow is Day 3: **AI sandboxes** . Run Claude Code, Codex, or custom agents in isolated environments with persistent volumes.[Follow along](https://www.windmill.dev/launch-week-march-2026) .


[Windmill](https://www.windmill.dev/) is an[open-source](https://github.com/windmill-labs/windmill) and[self-hostable](https://www.windmill.dev/docs/advanced/self_host/) developer platform to build, orchestrate, and monitor internal tools and data pipelines, combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.


You can[self-host](https://www.windmill.dev/docs/advanced/self_host/) Windmill using a` docker compose up` , or go with the[cloud app](https://app.windmill.dev/user/login) .
