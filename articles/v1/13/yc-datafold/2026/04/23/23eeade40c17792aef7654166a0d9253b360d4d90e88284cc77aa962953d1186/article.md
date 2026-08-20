---
schema_version: "1.0.0"
document_id: "23eeade40c17792aef7654166a0d9253b360d4d90e88284cc77aa962953d1186"
company_key: "yc-datafold"
company: "Datafold"
source_id: "yc-datafold-news-import-e233b68116ce"
canonical_url: "https://www.datafold.com/blog/the-modern-data-stack-open-source-edition/"
published_at: "2026-04-03T00:00:00+00:00"
first_seen_at: "2026-07-24T03:54:32.907441+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:4aed28c5985ec0fd3679073db425e8824bdf2707328b0eb8916f0865b2f9cb2f"
---

# The Modern Data Stack: Open-source edition

*Note: Some technologies listed here are source-available rather than open-source per their licenses. We include them because certain licenses may still be permissive enough for internal use (the focus of this article) while restrictive for commercial use.*


*Originally published October 2024. Fully updated April 2026.*


## Changes since last year


A lot happened in the open-source data ecosystem since our October 2024 review. Here’s a quick rundown before we dive in.


### AI is taking over


It’s impossible to ignore the impact of agentic AI on data teams and their stacks. I offered[some predictions](https://www.datafold.com/blog/data-engineering-in-2026-predictions/) on where the field is going.


The cost of software development is plummeting. If anyone can vibecode anything in days, are open-source projects still relevant?


I’d argue they’re more relevant than ever, especially at the infrastructure layer:


1.


**AI increases demand for data.** We can do so much more with it now, which means we need more of it.


2.


**It’s much easier to build with AI,** so data infrastructure is used more.


3.


**Sometimes it’s better not to start from scratch.** Vibe coding gets you from 0 to 80% fast — but infrastructure needs the last 20%.


Agentic development is great for applications, but for infrastructure, reliability, and architecture matter too much to leave to a weekend prototype.


Open-source infrastructure gives agents better building blocks. Rather than replacing these tools, agents can extend them to fit your needs, which is how you close that last 20%.


This is huge: before AI, the vast majority of users of open-source tools never modified them as the cost was too high. Now anyone can deploy and extend an open-source project to fit the needs of their business.


Extensibility was always a major part of the open-source pitch, but now it’s actually a big deal and a huge advantage over vendor solutions.


There is a flip side.


The same dynamic increases pressure on open-source-first vendors that historically relied on team and enterprise features to convert free users to paid. If 80% of the value is already open-sourced and the remaining 20% is increasingly easy to vibecode, the commercial moat may shrink fast — which is why we may be seeing the changes below:


### License changes & proprietary pivots


-


**Snowplow** switched from Apache 2.0 to the Snowplow Limited Use License Agreement (SLULA) in January 2024. SLULA v1.1 (December 2024) further restricts usage to test and academic environments only — no production use with high availability allowed. We keep it in the comparison tables for reference but no longer recommend it.


-


**MinIO** went through a slow death spiral: stripped the admin console from the community edition (May 2025), halted binary and Docker distributions (October 2025), entered “maintenance mode” (December 2025), and finally archived the OSS repo entirely on February 13, 2026. The web UI now requires an enterprise license. A community fork exists at` pgsty/minio` , but for new deployments look at[SeaweedFS](https://github.com/seaweedfs/seaweedfs) .


### Lost momentum: stale projects


-


**Mage** (orchestration) — development collapsed from a healthy pace to 2.6 commits/week, last release January 2026. Effectively dormant.


-


**Amundsen** (data catalog, by Lyft) — 0 commits/week. Effectively abandoned.


-


**Hydra** (OLAP on top of PostgreSQL) — last release was April 2024, over two years ago. No meaningful commits since. The project appears abandoned though not formally archived.


-


**Evidence** (SQL+Markdown dashboards) — last commit February 18, 2026. At 6.1k stars there’s clearly interest, but the project may be pivoting or winding down.


-


**Observable Framework** (data apps) — 0.7 commits/week, last push 8 days ago. Observable’s focus has shifted to their hosted Notebooks/Canvases product; the open-source Framework appears to be winding down.


-


**SQLMesh** — after Fivetran acquired Tobiko Data in September 2025, development dropped 87% (from 22 to 2.5 commits/week). Fivetran donated SQLMesh to the Linux Foundation on March 25, 2026, but it remains to be seen whether the contributor base materializes. Effectively dormant for now.


-


**Meltano** — the parent company (Arch, fka Meltano) shut down in December 2025. The open-source project was transferred to Matatika for continued maintenance. It remains MIT-licensed but development has slowed significantly. Effectively dormant.


### New entrants and breakout stories


-


**Kestra** (orchestration) — exploded to 26.6k stars, raised a $25M Series A in March 2026 (led by RTP Global, total funding $36M), and is working on Kestra 2.0 with distributed execution. 2B+ workflows executed in 2025. Customers include Bloomberg, Toyota, and JPMorgan Chase.


-


**dlt** (data load tool) — continued its rise as the lightweight Python alternative to Airbyte. 5.2K stars, steady commits, and a loyal following among engineers who find full platforms overkill.


-


**Apache Paimon** — streaming-first lakehouse format at 3.2K stars with production use at Alibaba and TikTok. Fills a gap that Iceberg doesn’t fully address for streaming-heavy architectures.


-


**DuckLake** — the DuckDB team’s radically simpler table format (SQL database as catalog instead of manifest files). 2.5K stars already, experimental but promising.


-


**Apache Polaris** — Snowflake donated this Iceberg REST catalog to Apache in June 2024. Now incubating with Apache and adding Delta Lake support.


-


**Apache Gravitino** — graduated to Apache Top-Level Project in June 2025, hit v1.0.0, and joined the Agentic AI Foundation in early 2026. Production use at Pinterest and Bilibili.


-


**RisingWave** — streaming database with PostgreSQL-compatible SQL. 8.9k stars, 32 commits/week. Write SQL instead of learning Flink’s programming model.


### The Fivetran consolidation


The single biggest ecosystem shift: Fivetran acquired Census (May 2025), Tobiko Data/SQLMesh (September 2025), and dbt Labs (October 2025, all-stock). One company now controls the leading open-source SQL transformation tool plus an EL platform. dbt Core remains Apache 2.0 while the parent company makes a hard push to their new commercial-first offering Fusion; SQLMesh was donated to the Linux Foundation.


---


## Elements of the modern data stack


Since even a basic data stack has many moving parts and technologies to choose from, this post organizes the analysis according to the steps in the data value chain.


Organizations need data to make better decisions, either human (analysis) or machine (algorithms and machine learning). Before data can be used for that, it needs to go through a sophisticated multi-step process, which typically follows these steps:


1. **Collection & Integration:** Getting data into the data warehouse/lake. This includes event processing (collecting events) and data integration (adding data from APIs and databases).
2. **Data Lake:** Storing, processing, and serving data in a lake architecture, with its Storage, File, Table Format, Data Catalog, and Compute layers.
3. **Data Warehousing:** Similar to a data lake but an all-in-one solution with storage and compute more tightly integrated.
4. **Data Orchestration:** Managing how data pipelines are defined, structured, and executed.
5. **Streaming & Real-time:** Low-latency and operational data processing.
6. **Metadata Platforms & Data Discovery:** Finding, understanding, and trusting data assets.
7. **BI & Data Apps:** The consumption layer, including notebooks, self-serve analytics, dashboards, code-first data apps, and product/web analytics.


Some of the open-source products listed below are open-core: primarily maintained by teams that profit from consulting, hosting, and offering enterprise features. In this post, we only compare openly available functionality, not features locked behind SaaS/Enterprise tiers.


## Evaluation criteria


For each open-source project, we loosely use the following criteria:


**Feature completeness** : how closely the product matches best-in-class SaaS offering.


**Traction**


- # of stargazers
- # of contributors
- Velocity: commits per week. If it’s very low, you risk adopting a stale project, requiring you to fix bugs and build integrations on your own. We flag projects where momentum has shifted dramatically since our last review.


**Maturity** - beyond features, how easy and reliable it will be to use in production. This is a qualitative and subjective assessment.


- **Promising** : the technology demonstrates basic functionality, but using it in production will likely require some development and deployment work.
- **Mature** : the technology is production-ready out of the box, is used by multiple large organizations, and is considered one of the go-to solutions in its category.
- **Legacy** : the technology is well-adopted, but its popularity and momentum have already peaked, and new entrants are actively disrupting it.


## Collection and integration


**Best-in-class SaaS: Twilio/Segment (event processing), Fivetran (data integration + reverse ETL), Hightouch (reverse ETL)**


Collecting event data from client and server-side applications + third-party systems.


### Event collection and ingestion


[RudderStack](https://github.com/rudderlabs/rudder-server) remains the most well-rounded open-source Segment alternative, with steady development (8 commits/week) and a 4.4K-star community. Snowplow, once a strong contender, moved to a proprietary license (Snowplow Limited Use License) in early 2024 and is no longer truly open-source - a cautionary tale for teams betting on OSS longevity.[Jitsu](https://github.com/jitsucom/jitsu) is a lighter-weight option focused on event ingestion into warehouses, though its development has slowed (8 commits/week, down 42% year-over-year).


### Data integration + reverse-ETL


[Airbyte](https://github.com/airbytehq/airbyte) is the leading open-source data integration solution.


Airbyte focuses more on data integration - bringing data into the warehouse from multiple sources - but offers some reverse-ETL (pushing data from the warehouse into business apps) functionality.


Newer entrants worth watching are[dlt](https://github.com/dlt-hub/dlt) (data load tool), a Python library that takes a radically lightweight approach to data integration. Where Airbyte requires deploying a full platform, dlt is just a pip install. It has gained a loyal following among engineers who find Airbyte overkill for their needs, and its 5.2k stars and 12 commits/week suggest healthy momentum. Similarly,[ingestr](https://github.com/bruin-data/ingestr) offers a dead-simple CLI for copying data between databases and SaaS platforms - think of it as the “curl for data pipelines.”


Here are the contenders for open-source alternatives to Segment and Fivetran:


Feature Airbyte dlt ingestr RudderStack Jitsu


Repo[airbytehq/airbyte](https://github.com/airbytehq/airbyte)[dlt-hub/dlt](https://github.com/dlt-hub/dlt)[bruin-data/ingestr](https://github.com/bruin-data/ingestr)[rudderlabs/rudder-server](https://github.com/rudderlabs/rudder-server)[jitsucom/jitsu](https://github.com/jitsucom/jitsu)


Closest SaaS Fivetran Fivetran Fivetran Segment Segment


Approach Full platform Python library CLI tool Full platform Lightweight platform


API as a source Yes Yes Yes No No


Event collection No No No Yes Yes


Database replication Yes (via Debezium) No Yes No No


API as destination (reverse ETL) No Yes No No No


Database as destination Yes Yes Yes Yes Yes


Maturity Mature Mature Promising Mature Mature


Stargazers 21.0k 5.2k 3.4k 4.4k 4.7k


# of weekly commits 263 12 23 9 8


Primary language Python Python Go Go TypeScript


License MIT Apache 2.0 MIT Elastic License 2.0 MIT


## Data lake & warehousing


The heart of any data stack! Let’s get clear with the definitions first:


### Data lake vs. data warehouse


Data Lake Data Warehouse


Architecture Assembled from individual components, storage and compute are decoupled Storage and compute are more coupled


Query Engine Supports multiple query engines (e.g. Spark, DuckDB operating on a unified data lake) Typically has one query engine


Scalability Highly scalable, compute and storage can be scaled separately Depends on the architecture, usually less scalable than a data lake


Structure Supports both unstructured and structured (via metastore) data More structured


Performance Depends on the query engines and architecture Typically more performant due to tighter coupling of storage and compute


Management overhead Higher due to multiple components that need to work together Depends on the specific technology; usually lower due to fewer moving parts


Typically, a data lake approach is chosen when scalability and flexibility are more important than performance and low management overhead.


### Data lake components


Data lake architecture is organized into layers, each responsible for its part:


1. Storage Layer – operates the physical storage
2. File Layer – determines in what format the data is stored in the storage layer
3. Table Format Layer – defines structure (e.g. what files correspond to what tables and simplifies data ops operations and governance)
4. Compute Layer – performs data processing and querying


### Storage layer


**Best-in-class SaaS: AWS S3, GCP GCS, Azure Data Lake Storage**


Self-hosting the storage layer is increasingly rare, but if you need a self-hosted S3-compatible storage,[SeaweedFS](https://github.com/seaweedfs/seaweedfs) (Apache 2.0) is the best open-source option.


### File layer


The File Layer determines how the data is stored on the disk. Choosing the right file format can have a massive impact on the performance of data lake operations. Luckily, given the flexibility of the data lake architecture, you can mix and match different storage formats.[Apache Parquet](https://parquet.apache.org/) is the leading storage format for the file layer due to its balanced performance, flexibility, and maturity.[Avro](https://avro.apache.org/) is a more specialized option for specific applications.


Feature Lance Vortex Avro Parquet


Repo[lancedb/lance](https://github.com/lancedb/lance)[vortex-data/vortex](https://github.com/vortex-data/vortex)[apache/avro](https://github.com/apache/avro)[apache/parquet-format](https://github.com/apache/parquet-format)


Data orientation Columnar + vector Columnar (pluggable encoding) Row Columnar


Compression High Very high (pluggable) Medium High


Optimized for High-dimensional data, embeddings, ML features Analytical workloads (“faster Parquet”) Row-oriented workloads, data exchange, streaming Read-heavy analytical workloads, classic ELT architectures


Schema evolution Yes Yes Full Partial


Interoperability DuckDB, Pandas, Polars DuckDB, Spark, Pandas, Polars Good Best


Written in Rust Rust Java Java


Stargazers 6.3k 2.8k 3.3k 2.3k


# of weekly commits 34 74 6 0


Maturity Promising Promising Mature Mature


License Apache 2.0 Apache 2.0 Apache 2.0 Apache 2.0


Two newer file formats are worth watching:


[Vortex](https://github.com/vortex-data/vortex) is a next-generation columnar format now governed by the Linux Foundation (LFAI&Data). It separates logical schemas from physical layouts with pluggable encoding, claiming 100x faster random access and 10-20x faster scans versus Parquet. The project is genuinely active at 74 commits/week (more than many mature tools on this list) and already integrates with DuckDB, Spark, Pandas, and Polars. The file format spec is stable as of v0.64.0, and Iceberg support is on the roadmap. Still early, but the governance and momentum are real.


[Lance](https://github.com/lancedb/lance) is another columnar format, optimized for high-dimensional data (embeddings, images, multimodal). It’s at 6.3K stars with 34 commits/week and growing 40% year-over-year. Worth a look if your workloads involve vector search or ML feature storage.


And one more for the curious:[F3](https://github.com/future-file-format/F3) (Future-proof File Format), an academic project by Wes McKinney (creator of Pandas and Arrow) and Andrew Pavlo (CMU), embeds WebAssembly decoders directly in each file so any future reader can decode it without needing the original library. Published at SIGMOD 2026 with 422 GitHub stars, it’s explicitly not production-ready — but the idea is genuinely clever and worth tracking.


### Table formats


Since data lake architecture works with files directly, the table format layer is what makes it feel like a proper database. It links files to tables so users can query without caring about file locations, and it handles operations like concurrent writes, ACID transactions, versioning, and file compaction.


Hive Metastore was the standard for over a decade. It still exists and still works, but it only handles the “files to tables” part - none of the modern data operations. It’s legacy.


The table format war that played out from 2021-2024 is over.[Apache Iceberg](https://github.com/apache/iceberg) won. Format V3 shipped, adoption surveys show ~78% exclusive usage, and the inaugural Iceberg Summit is planned for April 2026. Every major engine and cloud vendor now treats Iceberg as the default. At 8.7k stars and 34 commits/week, development is steady and mature.


[Apache Hudi](https://github.com/apache/hudi) and[Delta Lake](https://github.com/delta-io/delta) are still maintained, but both have pivoted to adding Iceberg compatibility, which tells you everything about where the market landed. Hudi’s development has slowed (18 commits/week, trending down), while Delta Lake remains more active at 24 commits/week, largely driven by Databricks’ investment.


Two new table formats have appeared since our last review:


[DuckLake](https://github.com/duckdb/ducklake) , created by the DuckDB team in May 2025, takes a radically simpler approach: it combines the table format *and* the catalog into one system, using a plain SQL database (PostgreSQL, MySQL, SQLite, or DuckDB itself) as the catalog instead of Iceberg’s manifest files, with Parquet for data storage. You get time travel, schema evolution, ACID transactions, and partitioning — no separate catalog service needed. Where a typical lakehouse requires Iceberg + Polaris (or Unity Catalog), DuckLake replaces both with a single system. The tradeoff: it only works with DuckDB at the moment. For teams going all-in on DuckDB, that’s a feature. For multi-engine environments (Spark + Trino + Flink), stick with Iceberg + a catalog.


[Apache Paimon](https://github.com/apache/paimon) takes the opposite approach, optimizing for real-time streaming lakehouse scenarios. It uses LSM-tree-based storage and has native Flink and Spark integration. At 3.2K stars and 42 commits/week, it’s genuinely active and fills a gap that Iceberg doesn’t fully address for streaming-heavy architectures.


Feature DuckLake Iceberg Delta Lake Hudi Hive Metastore


Repo[duckdb/ducklake](https://github.com/duckdb/ducklake)[apache/iceberg](https://github.com/apache/iceberg)[delta-io/delta](https://github.com/delta-io/delta)[apache/hudi](https://github.com/apache/hudi)[apache/hive](https://github.com/apache/hive)


Originated at DuckDB Labs Netflix Databricks Uber Facebook


Catalog approach Plain SQL database Manifest files Delta log (JSON) Timeline metadata Metastore DB


File formats Parquet Parquet, Avro, etc. Parquet Parquet, Avro, etc. Parquet, Avro, etc.


Schema evolution Yes Yes Yes Limited Limited


Transactions ACID ACID ACID ACID No


Concurrency Limited High High High Limited


Table Versioning Yes Yes Yes Yes No


Interoperability DuckDB only (for now) High (Spark, Trino, Flink, BigQuery) Limited (Optimized for Apache Spark) Limited (Optimized for Apache Spark) High (Hive, Trino, Spark)


Stargazers 2.6k 8.7k 8.7k 6.1k 6.0k


# of weekly commits 55 34 24 21 7


Maturity Promising Mature Mature Legacy Legacy


License MIT Apache 2.0 Apache 2.0 Apache 2.0 Apache 2.0


### Data catalogs


Don’t confuse *data catalogs* with *metadata platforms* (covered later under Data cataloging). Data catalogs are the infrastructure layer that query engines talk to — they answer “what tables exist and where is the data stored.” Metadata platforms like DataHub and OpenMetadata sit on top and answer “who owns this table, what does this column mean, and what feeds it.”


[Apache Polaris](https://github.com/apache/polaris) , donated by Snowflake to Apache in June 2024, is an open-source Iceberg REST catalog. It implements the Iceberg REST Catalog spec, meaning any engine that speaks Iceberg REST (Spark, Trino, Flink, StarRocks) can use it out of the box. Delta Lake support is being added. At 1.9K stars and 40 commits/week, development is healthy.


[Unity Catalog OSS](https://github.com/unitycatalog/unitycatalog) (3.3K stars), donated by Databricks to the Linux Foundation, takes a broader scope: it catalogs tables, volumes, ML models, and functions in one place. Supports Iceberg, Delta, and Hive table formats. The LF governance and Databricks backing give it credibility.


[Apache Gravitino](https://github.com/apache/gravitino) (2.9K stars, 23 commits/week) — graduated to an Apache Top-Level Project in June 2025, hit v1.0.0, and has production deployments at Pinterest and Bilibili. Gravitino’s unique value is *federation* : it presents a unified catalog interface across multiple underlying catalogs (Hive Metastore, Iceberg REST, JDBC, etc.) and across regions and clouds, which may be useful if you’re running a multi-cloud lakehouse.


Feature Unity Catalog Polaris Gravitino


Repo[unitycatalog/unitycatalog](https://github.com/unitycatalog/unitycatalog)[apache/polaris](https://github.com/apache/polaris)[apache/gravitino](https://github.com/apache/gravitino)


Originally developed at Databricks (donated to LF) Snowflake (donated to Apache) Datastrato (Apache TLP)


Scope Universal catalog (tables, volumes, models, functions) Iceberg REST catalog Federated catalog across multiple backends


Table format support Iceberg, Delta, Hive Iceberg, Delta (emerging) 20+ (Iceberg, Hive, Kafka, etc.)


Multi-catalog federation No No Yes


Written in Java Java Java


Stargazers 3.3k 1.9k 2.9k


# of weekly commits 6 40 23


Maturity Mature Mature Mature


License Apache 2.0 Apache 2.0 Apache 2.0


### Compute layer


The Compute Layer is the most user-facing part of the data lake architecture since it’s what data engineers, analysts, and other data users leverage to run queries and workloads.


There are multiple ways compute engines can be compared:


1. **Interface:** SQL, Python, Scala, etc.
2. **Architecture:** in-process vs. distributed
3. **Primary processing:** in-memory vs. streaming
4. **Capabilities:** querying, transformations, ML We believe that today, the architecture – the in-process vs. distributed – is the most important distinction since other aspects, e.g. supported languages or capabilities, can be added over time.


### In-process compute engines


Over the past few years, in-process compute engines emerged as a novel way of running compute on data lakes. Unlike distributed compute engines like Trino, and Spark which run their own clusters of nodes that are queried over the network, in-process engines are invoked from and run within data science notebooks or orchestrator jobs. I.e., they function similarly to libraries that are called by the code that runs the flow. To illustrate, with[DuckDB](https://github.com/duckdb/duckdb) you can do the following from a Python application:


```text
import duckdb
duckdb.connect().execute("SELECT * FROM your_table")
```


Whereas working with a distributed system like Spark requires first spinning it up as a standalone cluster and then connecting to it over the network.


DuckDB has had a remarkable run since our last review. It went from 2.4K to 37.2k stars, ships 375 commits/week, introduced an LTS release cadence, and is now at v1.5.0. The community adores it - and for good reason. If your data fits on one machine (and more data fits on one machine than most people think), DuckDB is probably the right answer.


The power of the in-process pattern is simplicity and efficiency: distributed engines like Spark and Trino require a significant dedicated effort to operate, whereas in-process engines can run inside an Airflow task or a Dagster job and require no DevOps or additional infrastructure.


There are three main limitations of in-process engines:


1. **Size scalability:** data queried should generally fit in memory limited by the available memory to the process/machine; although disk spilling is increasingly supported as a fallback.
2. **Compute scalability:** parallelization is limited to the number of cores available to the machine and cannot be parallelized to multiple machines.
3. **Limited support for data apps:** while in-process engines can be used for various workloads, including ingestion, transformation, and machine learning, most BI tools and business applications require a distributed engine with an online server endpoint. One cannot connect to DuckDB from, say, Looker since DuckDB runs within a process and does not expose a server endpoint, unlike PostgreSQL. As a rule of thumb, the in-process engines are optimal and efficient for small-medium (up to hundreds of gigabytes) data operations.


The following three technologies are leading in-process data engines:


[DuckDB](https://duckdb.org/) – a column-oriented SQL in-process engine[Pandas](https://pandas.pydata.org/) – a popular Python library for processing data frames. Hot take: largely superseded by Polars.


[Polars](https://pola.rs/) – the modern default for DataFrame workloads in Python and Rust.


Polars raised EUR 18M in a Series A in September 2025, launched its Distributed Engine in Open Beta going after Spark.


It’s worth noting that DuckDB and Polars are rapidly growing their interoperability with other technologies with the adoption of highly efficient Apache Arrow (ADBC) as a data transfer protocol and Iceberg as a table format.


Feature Pandas Polars DuckDB


Repo[pandas-dev/pandas](https://github.com/pandas-dev/pandas)[pola-rs/polars](https://github.com/pola-rs/polars)[duckdb/duckdb](https://github.com/duckdb/duckdb)


Querying Interface Python Python, Rust, SQL SQL


File IO JSON, CSV, Parquet, Excel, etc. JSON, CSV, Parquet, Excel, etc. JSON, CSV, Parquet, Excel, etc.


Database integrations Any via ADBC or SQLAlchemy Any via ADBC or SQLAlchemy Native PostgreSQL, MySQL; Any via ADBC


Parallel (multi-core) execution No (through extensions) Yes Yes


Metastore Support None Iceberg (emerging) Iceberg


Written in Python Rust C++


Stargazers 48.3k 38.0k 37.2k


# of weekly commits 57 43.8 375


Maturity Mature Mature Mature


License BSD 3-Clause MIT MIT


## Distributed compute engines


**Best-in-class SaaS: Snowflake, Databricks, BigQuery**


We previously discussed[criteria for choosing a data warehouse](https://www.datafold.com/blog/choosing-data-warehouse-for-analytics/) and common misconceived advantages of open-source data processing technologies. We have highlighted Snowflake, BigQuery and Databricks SQL as optimal solutions for typical analytical needs.


Assuming the typical pattern for analytics:


1. Current or anticipated large (terabyte-petabyte) and growing data volume
2. Many (10-100+) data consumers (people and systems)
3. Load is distributed in bursts (e.g. more people query data in the morning but few at night)
4. Query latency is important but not critical; 5-30s is satisfactory
5. The primary interface for transforming and querying data is SQL To meet the assumptions above, teams frequently use a data lake architecture (decoupling storage from computing).


**Trino is the closest open-source alternative to Snowflake** for interactive SQL analytics at scale. It has the following advantages:


- Feature-rich SQL interface
- Works well for a wide range of use cases from ETL to serving BI tools
- Close to matching best-in-class SaaS offerings in terms of usability and performance
- Provides a great trade-off between latency and scalability


Trino has been so successful as an open-source cloud SQL warehouse alternative largely due to a substantial improvement in usability and abstracting the user (and the DevOps) from the underlying complexity: it just works.


But if your data fits in memory (say, up to hundreds of gigabytes), you may be better off just using[DuckDB](https://github.com/duckdb/duckdb) for its simplicity, no overhead and speed.


## ML & specialized jobs


[Spark](https://github.com/apache/spark) is a true workhorse of modern data computing with a polyglot interface (SQL, Python, Java & Scala) and unmatched interoperability with other systems. It is also extremely versatile and handles a wide range of workloads, from classic batch ETL to streaming to ML and graph analytics.


Do I need Trino if Spark can handle everything?


While Spark is a Swiss army knife for ETL and ML, it is not optimized for interactive query performance. Usually, it has significantly higher latency for average BI queries, as[shown](https://www.linkedin.com/pulse/aws-emr-fs-presto-vs-hive-spark-sql-rahul-natarajan/) by benchmarks.


A popular pattern is to have both Trino & Spark used for different types of workflows while sharing data and metadata:


Feature Spark Trino


Repo[apache/spark](https://github.com/apache/spark)[trinodb/trino](https://github.com/trinodb/trino)


Architecture Distributed Distributed


Querying Interface Scala, Java, Python, R, SQL SQL


Fault Tolerance Fault-tolerant, can restart from checkpoints None - partial failures require full restart


Latency Higher Lower


Streaming Yes No


Metastore Support Delta Lake (optimal), Hive Metastore, Iceberg Hive Metastore, Iceberg, Delta Lake (emerging)


Machine learning Yes No


Written in Scala Java


Stargazers 43.1k 12.7k


# of weekly commits 87 72.4


Maturity Mature Mature


License Apache 2.0 Apache 2.0


## Data warehouses


Two good reasons for choosing data warehouse (coupled storage and compute architecture) are:


1. **Performance** : use cases that require sub-second query latencies which are hard to achieve with a data lake architecture shown above (since data is sent over the network for every query). For such use cases, there are warehousing technologies that bundle storage and compute on the same nodes and, as a result, are optimized for performance (at the expense of scalability), such as[ClickHouse](https://github.com/ClickHouse/ClickHouse) .
2. **Simplicity** : Operating a data warehouse is usually significantly simpler than a data lake due to fewer components and moving parts. It may be an optimal choice when the volume of data and queries is not high enough to justify running a data lake, or when certain workloads require lower latency (e.g., BI).


ClickHouse takes advantage of the tighter coupled storage and compute standard for the data warehousing paradigm:


- Column-oriented data storage optimized for analytical workloads
- Vectorized data processing allowing the use of[SIMD](https://en.wikipedia.org/wiki/Single_instruction,_multiple_data) on modern CPUs
- Efficient data compression However, they have several important distinctions:


**ClickHouse** is one of the most mature and prolific open-source data warehousing technologies originating at Yandex. It powers Yandex Metrica (a competitor to Google Analytics) and[processes over 20 billion events](https://clickhouse.com/docs/en/about-us/history) daily in a multi-petabyte deployment.


Since our last review, ClickHouse raised a $400M Series D in January 2026 and acquired Langfuse (an LLM observability platform). At 46.7k stars and a staggering 1301 commits/week, it is by far the most actively developed project on this entire list. If you need sub-second OLAP at scale, ClickHouse is the answer.


Notable mentions:


[Pinot](https://github.com/apache/incubator-pinot) and[Druid](https://github.com/apache/druid) are distributed OLAP data engines similar to ClickHouse. Originally developed at LinkedIn and Metamarkets respectively, those systems are largely superseded by ClickHouse in terms of lower maintenance costs, faster performance, and a wider range of features (for example, Pinot and Druid have very limited support for JOIN operations that are key parts of modern data workflows).


## Data orchestration


The role of the data orchestrator is to help you develop and reliably execute code that extracts, loads, and transforms the data in the data lake or warehouse as well as perform tasks such as training machine learning models. Given the variety of available frameworks and tools, it is important to consider three major use cases:


### SQL-centric workflows


Premise: your transformations are expressed predominantly in SQL.


[dbt](https://www.getdbt.com/) remains the de-facto standard for SQL transformations with a massive community and ecosystem.


Following the acquisition by Fivetran, dbt Labs has shifted focus on their newer engine - Fusion - licensed under ELv2 but committed to maintaining dbt Core which remains under Apache 2.0, and development continues (12 commits/week), but the pace is notably slower than before the acquisition.


## Full-featured data orchestration


Premise: in addition to (or instead of) SQL, your workflows include jobs defined in Python, PySpark, and/or other frameworks. While dbt and SQLMesh provide basic support for Python models, their focus is on SQL and a full-featured orchestrator may be warranted for running multi-language workloads.


In this case, it may be beneficial to use a full-featured data orchestrator that:


1. Manages scheduling and dependencies of disparate tasks
2. Handles failures and retries gracefully
3. Provides visibility into the data pipeline status, logs, and performance


The most widely used data orchestrator remains[Apache Airflow](https://github.com/apache/airflow) . Despite the age, it still has good momentum: Airflow 3.0 shipped in April 2025 with a number of major upgrades: new task execution model, redesigned UI, better Kubernetes support. The Github numbers are strong: 168 commits/week (up 23% quarter-over-quarter), 44.9K stars, and 80K+ organizations using it.


If you’re starting fresh with no existing orchestration, Dagster may be a very strong choice for its excellent developer experience.


In the last few years, multiple teams have attempted to rethink and improve orchestration, and at the moment, we recommend taking a closer look at the following leading open-source orchestration frameworks.


[Prefect](https://github.com/PrefectHQ/prefect) is another popular open source player in the Airflow paradigm.


The biggest new entrant in orchestration is[Kestra](https://github.com/kestra-io/kestra) , which has exploded to 26.6k stars with 77 commits/week. Kestra raised a $25M Series A in March 2026, executed 2B+ workflows in 2025 (20x year-over-year).


Kestra’s perhaps most unique feature is event-driven workflows in addition to the classic Airflow-like scheduled workflows. In a classic Airflow paradigm, you execute tasks as batch operations on lots of data at once.


Event-driven workflows allow executing business logic based on individual events and pub/sub messages, bridging into the microservices world as well as automation platforms such as[n8n](https://github.com/n8n-io/n8n) and others.


Feature Airflow Kestra Prefect Dagster


Repo[apache/airflow](https://github.com/apache/airflow)[kestra-io/kestra](https://github.com/kestra-io/kestra)[PrefectHQ/prefect](https://github.com/PrefectHQ/prefect)[dagster-io/dagster](https://github.com/dagster-io/dagster)


Realtime per-event workflows No (batch only) Yes (Realtime Triggers) No (event-triggered batch) No (event-triggered batch)


Data contracts No Yes Yes Yes


Multiple Environments No Yes Yes Yes


SLA-driven Scheduling No No No Yes


dbt integration No/limited Yes (via plugin) No/limited Yes


Written in Python Java Python Python


Stargazers 44.9k 26.6k 22.0k 15.2k


# of weekly commits 168 76.8 68 58.8


Maturity Mature Mature Mature Mature


License Apache 2.0 Apache 2.0 Apache 2.0 Apache 2.0


## Streaming & real time


Streaming and real-time data processing space is vast with a lot of nuance. In this guide, we offer a high-level overview of concepts and mention a few foundational and breakthrough technologies.


Streaming paradigm is key in a few use cases:


1. Collecting data (e.g., events) and ingesting it into a data warehouse/lake
2. Processing data in-flight for enrichment before loading into the long-term storage
3. Real-time analytics and machine learning, e.g. fraud detection, operational analysis, etc. – where latency matters.


The streaming space breaks down into three layers: message brokers (the pipes), stream processors (the compute), and streaming databases (the query layer). You typically need at least a broker + one of the other two.


### Message brokers


The pipes that move events between systems. You produce messages, they store and deliver them, consumers read them.


[Apache Kafka](https://github.com/apache/kafka) is the cornerstone. Kafka 4.0.0 shipped with a landmark change: ZooKeeper is gone, replaced entirely by KRaft. This eliminates the single biggest operational headache of running Kafka yourself. At 32.3K stars and 37 commits/week, momentum is steady.


[Redpanda](https://github.com/redpanda-data/redpanda) is a Kafka wire-protocol compatible broker written in C++ (no JVM). It claims 10x lower latency and simpler operations. At 11.9k stars and 162 commits/week, it’s a formidable alternative. If you’re starting fresh and don’t need Kafka’s full ecosystem, Redpanda is worth evaluating.


[Hazelcast](https://github.com/hazelcast/hazelcast) (6.6k stars, 19 commits/week) straddles the broker and processing layers — it’s an in-memory data grid with built-in stream processing (Jet engine). Good for teams that want both message passing and lightweight stream processing without running separate systems.


Feature Kafka Redpanda Hazelcast


Repo[apache/kafka](https://github.com/apache/kafka)[redpanda-data/redpanda](https://github.com/redpanda-data/redpanda)[hazelcast/hazelcast](https://github.com/hazelcast/hazelcast)


Optimized for High-throughput event streaming Kafka-compatible, lower latency, no JVM In-memory data grid + stream processing


Kafka API compatible Yes (is Kafka) Yes (wire-compatible) No (own API, Kafka connector available)


Built-in stream processing Kafka Streams (library) Transforms (lightweight) Yes (Jet engine)


Latency Milliseconds Sub-millisecond Sub-millisecond


Written in Java C++ Java


Stargazers 32.3k 11.9k 6.6k


# of weekly commits 37 162 19


Maturity Mature Mature Mature


License Apache 2.0 BSL 1.1 Apache 2.0 + Hazelcast Community License


### Stream processors


The compute engines that transform data in-flight. These read from brokers (Kafka/Redpanda), apply transformations, and write results downstream.


[Apache Spark](https://github.com/apache/spark) Structured Streaming is the easiest on-ramp if you already use Spark for batch — same API, same infrastructure. Latency is seconds, not milliseconds.


[Apache Flink](https://github.com/apache/flink) is the streaming-first engine. Lower latency (milliseconds), native stateful processing, and better suited for complex event-driven workflows. Steeper learning curve, but the right tool for serious streaming workloads.


[Arroyo](https://github.com/ArroyoSystems/arroyo) (4.9K stars) is architecturally similar to Flink — a stream processor that reads, transforms, and writes — but with a SQL-only interface instead of Java/Scala. The team was acquired by Cloudflare in April 2025; OSS development has slowed to 3 commits/week.


Feature Spark Flink Arroyo


Repo[apache/spark](https://github.com/apache/spark)[apache/flink](https://github.com/apache/flink)[ArroyoSystems/arroyo](https://github.com/ArroyoSystems/arroyo)


Optimized for Versatility across ETL, ML, and streaming Low-latency high-throughput stream processing SQL-first stream processing


Batch support Yes Yes No


Latency Seconds Subsecond Subsecond


Interface Scala, Java, Python, SQL Scala, Java, SQL SQL


Written in Scala Java Rust


Stargazers 43.1k 25.9k 4.9k


# of weekly commits 87 25 3


Maturity Mature Mature Promising


License Apache 2.0 Apache 2.0 Apache 2.0


### Streaming databases


A different category entirely: databases that maintain continuously updated query results as new data arrives. Unlike stream processors (which transform and forward), streaming databases store queryable state — you can` SELECT` from them at any time and get fresh results. Think real-time materialized views.


[RisingWave](https://github.com/risingwavelabs/risingwave) (8.9k stars, 33 commits/week) is a streaming database with a PostgreSQL-compatible interface. Actively developed, Rust-based, and the simplest path to streaming for SQL-fluent teams.


[Materialize](https://github.com/MaterializeInc/materialize) (6.3k stars, 101 commits/week) is purpose-built for operational workloads where a warehouse would be too slow and a stream processor too complicated. Define SQL views over streaming data, and they update incrementally as events arrive.


Feature RisingWave Materialize


Repo[risingwavelabs/risingwave](https://github.com/risingwavelabs/risingwave)[materializeInc/materialize](https://github.com/materializeInc/materialize)


Optimized for SQL-first streaming, PostgreSQL-compatible Real-time materialized views


Queryable state Yes Yes


Interface SQL (PostgreSQL wire protocol) SQL (PostgreSQL wire protocol)


Written in Rust Rust


Stargazers 8.9k 6.3k


# of weekly commits 33 101


Maturity Mature Mature


License Apache 2.0 Business Source


Notable mention:[Apache Beam](https://github.com/apache/beam) is a framework (think dbt but for streaming) that lets you write batch or streaming transformations and execute them on a runner of your choice (Spark, Flink, Hazelcast). Powerful concept, but adoption outside of Google (Beam is the interface to Google Dataflow) remains limited.


## Metadata platforms & data discovery


Not to be confused with data catalogs (covered earlier under Data lake components), metadata platforms sit a layer above. Data catalogs answer “what tables exist and where is the data” — metadata platforms answer “who owns this table, what does this column mean, what upstream pipeline feeds it, and can I trust this data?”


[DataHub](https://github.com/datahub-project/datahub) by LinkedIn is the clear open-source leader with 11.8k stars and 106 commits/week (accelerating 41% quarter-over-quarter). It handles data discovery, metadata management, and column-level lineage, with a rich plugin architecture for integrating with your stack.


The strongest challenger is[OpenMetadata](https://github.com/open-metadata/OpenMetadata) , which has surged to 9.6k stars with 141 commits/week — actually outpacing DataHub in recent development velocity. It offers a unified metadata platform with 84+ connectors and a more opinionated, batteries-included approach versus DataHub’s flexibility.


Feature DataHub OpenMetadata


Repo[datahub-project/datahub](https://github.com/datahub-project/datahub)[open-metadata/OpenMetadata](https://github.com/open-metadata/OpenMetadata)


Originally developed at LinkedIn Collate/Open-source community


Optimized for Flexible metadata management and discovery Unified metadata platform with built-in quality


Lineage Column-level for many integrations Column-level with automated extraction


Connectors 40+ 84+


Written in Java, Python Java, Python


Stargazers 11.8k 9.6k


# of weekly commits 106 141


Maturity Mature Mature


License Apache 2.0 Apache 2.0


## BI & analytics


**Best-in-class SaaS: Hex, Omni**


The consumption layer covers everything from self-serve dashboards to SQL notebooks to product analytics. Here’s how the open-source options stack up.


**Is AI killing BI?:** BI is disrupted by AI faster than any other layer of the data stack. SaaS tools like Hex and Omni are shipping agentic workflows aggressively that allows users to ask a question in natural language, get a chart, iterate conversationally. None of the open-source BI tools offer anything meaningful here. AI features (where they exist) are locked behind commercial cloud offerings. As agentic analytics is quickly becoming the dominant interface, open-source BI centered around queries and charts may become obsolete.


### Self-serve dashboards


[Superset](https://github.com/apache/superset) (72.2k stars) is the most advanced open-source BI tool for visualization. Created by Maxime Beauchemin (also creator of Airflow) at Airbnb, it has a massive community and plugin ecosystem. The learning curve is steeper than Metabase, but the ceiling is higher.


**Metabase is the open-source Tableau** —[Metabase](https://github.com/metabase/metabase) (46.7k stars) has the best self-serve UX for non-technical users with point-and-click exploration without writing SQL.


[Lightdash](https://github.com/lightdash/lightdash) (5.7k stars, 181 commits/week) is the open-source Looker alternative with deep dbt integration and a LookML-like modeling layer. Development velocity is the highest of any BI tool on this list.


### Ad-hoc analysis & notebooks


[Jupyter](https://github.com/jupyter/notebook) (13.1k stars) remains the standard for Python/R/Julia data science work. Not much has changed — it’s stable, ubiquitous, and not going anywhere.


[Querybook](https://www.querybook.org/) (2.2k stars) by Pinterest is a SQL-first notebook for ad-hoc analysis with built-in data discovery. Low commit velocity (1/week) but stable.


Feature Superset Metabase Lightdash Jupyter Querybook


Repo[apache/superset](https://github.com/apache/superset)[metabase/metabase](https://github.com/metabase/metabase)[lightdash/lightdash](https://github.com/lightdash/lightdash)[jupyter/notebook](https://github.com/jupyter/notebook)[pinterest/querybook](https://github.com/pinterest/querybook)


Closest SaaS Tableau Tableau Looker Hex Hex


Primary interface SQL + visual builder Point-and-click + SQL dbt metrics + SQL Python/R/Julia SQL


Written in Typescript, Python Clojure Typescript Python Typescript, Python


Stargazers 72.2k 46.7k 5.7k 13.1k 2.2k


# of weekly commits 80 121 181 2 1


Maturity Mature Mature Mature Mature Mature


License Apache 2.0 AGPL + Commercial MIT BSD 3-Clause Apache 2.0


### Product analytics


For product teams optimizing UX, conversion funnels, and user behavior, end-to-end analytics tools handle event collection, storage, and visualization in one platform.


**PostHog is the open-source Mixpanel/Amplitude** —[PostHog](https://github.com/PostHog/posthog) (32.4k stars, 505 commits/week) has evolved into a full product engineering platform: analytics, session replay, feature flags, A/B testing, surveys, and a data warehouse. The second most actively developed project on this entire list.


[Plausible](https://github.com/plausible/analytics) (24.5k stars) is the privacy-first Google Analytics alternative. Lightweight, cookie-free, and GDPR-compliant out of the box.


Feature PostHog Plausible


Repo[PostHog/posthog](https://github.com/PostHog/posthog)[plausible/analytics](https://github.com/plausible/analytics)


Optimized for Full product analytics platform Privacy-first web analytics


Written in Python, Typescript Elixir, Javascript


Stargazers 32.4k 24.5k


# of weekly commits 505 11


Maturity Mature Mature


License MIT AGPLv3


## Code-first data apps


[Plotly Dash](https://github.com/plotly/dash)


Developed by Plotly, Dash is designed for creating analytical web applications. It’s Python-based and heavily integrates with Plotly for visualization, which makes it ideal for building highly interactive, data-driven applications. Dash is particularly popular among data scientists who are familiar with Python and want to create complex, customizable web apps without extensive web development experience.


[Streamlit](https://github.com/streamlit/streamlit)


Streamlit remains the default for Python data apps at 44.1k stars and 48 commits/week. The Snowflake acquisition has not killed the open-source project, which is a relief.


Feature Streamlit Plotly Dash


Repo[streamlit/streamlit](https://github.com/streamlit/streamlit)[plotly/dash](https://github.com/plotly/dash)


Primary interface Python Python


Optimized for Fast prototyping of data apps Building interactive analytical web apps


Written in Typescript, Python Python


Stargazers 44.1k 24.4k


# of weekly commits 48 16.8


Maturity Mature Mature


License Apache 2.0 MIT


Additional tools:


**Cube**


Unlike Dash and Streamlit,[Cube](https://cube.dev/) is not directly a tool for building applications but a semantic layer between your databases and applications. It is designed to provide a consistent way to access data for analytics and visualizations across different client-side frameworks (including Dash and Streamlit), making it a powerful backend for data applications.


## Putting it all together


A year and a half later, I’m more confident than before: you can build a production-grade data stack on open-source. The tools have matured, the ecosystem has stabilized around winners (Iceberg, DuckDB, Airflow 3.0 or Dagster, dbt).


Agentic AI can help you stitch together the stack quickly and extend it in the ways specific and impactful to your business.


The shift toward lighter-weight tools is real too. DuckDB over Spark for anything under a terabyte. dlt over Airbyte when you just need to move some data. Kestra over Airflow when your team doesn’t want to write Python. The community is gravitating toward tools that do less but do it well, and that’s probably the right instinct.


Choose open source for the right reasons - transparency, extensibility, control - and be realistic about the tradeoffs in support, hosting, and long-term maintenance.
