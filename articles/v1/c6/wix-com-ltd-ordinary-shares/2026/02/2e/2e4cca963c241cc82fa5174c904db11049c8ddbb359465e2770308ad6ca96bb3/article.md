---
schema_version: "1.0.0"
document_id: "2e4cca963c241cc82fa5174c904db11049c8ddbb359465e2770308ad6ca96bb3"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/data-to-production-bridging-the-gap-between-iceberg-and-live-microservices"
published_at: "2026-02-17T11:04:08+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:19:57.932105+00:00"
content_hash: "sha256:bc104d4526a7b4b777872beb806961f0ba3d532c96367992b57de8e81fa1cc68"
---

# Data-to-Production: Bridging the Gap Between Iceberg and Live Microservices

At Wix, our Data Warehouse (DWH) is a massive repository of insights. Built on Amazon S3 using Apache Iceberg table formats, and populated by Trino and Spark jobs, it houses petabytes of data—from user segmentation and logs to AI chat analytics.


However, storage is only half the battle.


The real challenge—and the "holy grail" for many data engineering teams—is


**Activation** : taking that petabyte-scale data and exposing it to backend microservices with millisecond latency, high availability, and strict type safety.


We call this initiative


**Data-to-Production** .


In the last few months, we’ve built a platform that has already ingested billions of rows from varying Iceberg schemas into ClickHouse, serving critical features like AI Chat Analytics and User Segmentation. This blog details how we architected the system, the custom tooling we built to make it robust, and the lessons learned along the way.


###


The Architecture: A Bird’s-Eye View


The system is designed to decouple the complexity of big data ingestion from the simplicity required by product engineers. It consists of three main pillars:


1.


**The MetaData Layer:** A registration and governance layer where users define what data they need.


2.


**The Ingestion Engine (Airflow + Python):** A dynamic, self-healing pipeline that moves data from S3/Iceberg to ClickHouse.


3.


**The Serving Layer (Scala):** A type-safe JSON API that allows microservices to query ClickHouse securely.


###


1. The Metadata Layer: Registration & Governance


We didn't want to become a bottleneck where data engineers had to write customized ETL scripts for every new request. Instead, we built a


**Self-Service Platform** .


####


The Internal Self-Service Console


Product teams onboard their data using a custom user interface. Instead of writing DAGs, they provide a declarative configuration for their table including:


-


**Source:** The Iceberg table location (S3 path/catalog info).


-


**Urgency:** Is this a critical 24/7 production feature or a business-hours-only dashboard?


-


**Performance Tuning:** Users must specify Sort Keys (crucial for ClickHouse performance), Partition Columns, and Primary Keys (for deduplication).


-


**Lifecycle:** An optional TTL column to automatically expire old data.


####


Human-in-the-Loop Governance


While ingestion is automated, performance remains highly sensitive. Since ClickHouse performance is highly dependent on indexing, a poorly defined sort key can impact the entire cluster's health.. To mitigate this, table registration is intentionally gated.


When a user registers a table, our team receives a Slack alert containing the proposed schema, partitioning, and indexing configuration. We review these choices before explicitly approving the table, ensuring that only performant, production-safe datasets are exposed to downstream services.


This human-in-the-loop model allows us to balance self-service velocity with cluster stability during the platform’s early and scaling phases. Looking ahead, we are actively working to automate these validations using rule-based checks and learned heuristics.


###


2. The Engine Room: Deep Dive into the Ingestion Pipeline


While the Serving Layer provides the polish, the Ingestion Engine is the heavy lifter. Moving petabytes of data reliably from a data lake (Iceberg on S3) to a real-time OLAP database (ClickHouse) is fraught with challenges: network timeouts, schema drift, data consistency, and idempotency.


####


Architecture: The "Configuration-as-Code" Approach


We moved away from writing custom DAGs for every table. Instead, we built a


**Dynamic DAG Generator** .


The process starts with the


**configuration service API** . This service holds the metadata for every table we want to sync. Our Airflow environment polls this configuration and spins up Task Factories. These factories automatically construct the dependency chain for a table based on its requested Loading Method. This means a simple config change (e.g., switching from "Overwrite" to "Upsert") instantly changes the underlying pipeline logic without a single line of Airflow code being touched.


####


The Three Loading Strategies


We support three distinct loading patterns, each utilizing different ClickHouse engines and SQL operations to optimize for consistency and performance.


**A. OVERWRITE (The Atomic Swap)**


-


**Used for:** Dimension tables, Lookup tables, and Full Refreshes.


-


**Staging:** We create a temporary MergeTree table (


table_staging


).


-


**Load:** We ingest the entire dataset from Iceberg/S3 into this staging table.


-


**Swap:** We execute


EXCHANGE TABLES target AND table_staging


. This operation is atomic—queries running against the target continue to work on the old data until the millisecond the swap completes.


-


**Cleanup:** The old data (now in


table_staging


) is dropped.


**B. UPSERT (Incremental Deduplication)**


-


**Used for:** Mutable Fact tables (e.g., User Events where properties might update) or dimension tables.


-


We utilize the


ReplacingMergeTree


engine, which deduplicates data in the background based on the Sorting Key.


-


**Watermark Detection:** The pipeline queries the target table for


MAX(watermark_column)


.


-


**Delta Load:** We construct a query to the Trino/Iceberg source:


SELECT * FROM source WHERE updated_at > {max_watermark}


.


-


**Insert:** Only these new/modified rows are inserted into ClickHouse.


-


**Schema Evolution:** If the pipeline detects a schema change (e.g., a new column), it automatically escalates this run to a Full Reload via


EXCHANGE TABLES


to ensure the new schema is applied historically.


**C. PARTITION_REPLACE**


**Used for:** Fact tables, Immutable Logs, Event Streams, and Time-Series data. This is our most complex but efficient strategy.


-


**Surgical Replacement:** Instead of a generic insert, we use


ALTER TABLE target REPLACE PARTITION {id} FROM staging


. This atomically swaps out specific blocks of time (e.g., specific hours or days), ensuring perfectly consistent historical data.


-


**The Catch-Up Logic:** If a DAG fails or is paused for several hours, the target table falls behind. The system automatically handles this by comparing the max partition date in ClickHouse against the current time. If it falls behind, it enters a CATCHUP mode; otherwise, it runs a NORMAL load.


####


Reliability: The Custom Verified Operator & Idempotency


Standard Airflow operators use synchronous HTTP calls, which often time out on long data loads. To fix this, we built a


**custom, verified Airflow operator** .


**Async Polling & Deterministic Query IDs:** Instead of holding a connection open, our operator submits the query and immediately detaches. It then polls


system.query_log


to verify completion.


Crucially, we generate a


**Deterministic Query ID** based on the DAG, Task, and Run context. If an Airflow worker crashes and restarts:


1.


The new worker generates the same ID.


2.


It checks


system.query_log


.


3.


If it sees the query already succeeded, it skips execution entirely. This makes our pipeline idempotent and crash-resilient.


####


Safety Valves


We implemented specific protections to prevent data corruption:


-


**Empty Staging Protection:** Before an


EXCHANGE TABLES


operation, we check if the staging table has rows. If it's empty (due to an upstream filter error), we abort the swap to prevent wiping production data.


-


**Row Count Validation:** After a load, we compare


count(*)


between Trino and ClickHouse. If a mismatch is detected, the pipeline fails.


###


3. The Serving Layer: Safe, Semantic, and Low-Latency Access


While the Ingestion Engine handles the "heavy lifting," the Serving Layer is the brain of the operation. It acts as the gateway through which all microservices access analytical data.


When designing this layer, we rejected two common approaches:


-


**Direct SQL Access:** Too dangerous. It creates tight coupling, invites SQL injection, and allows inefficient queries to degrade cluster performance.


-


**Rigid REST Endpoints:** Too slow to iterate. Creating a new endpoint for every business question creates a bottleneck.


Instead, we built a


**Type-Safe, Semantic Query DSL** running on our Scala backend framework.


###


**3.1. The Semantic DSL: Querying as Code**


Instead of sending raw SQL strings, developers construct a structured JSON object defined by a strict Protobuf contract. This treats a query as a logical tree of operations rather than a text blob.


**Example Request:**


*“Give me the daily message count and unique users for the 'PROJECT_ALPHA' namespace over the last 30 days, filling in gaps where no messages occurred.”*


```text
{
"table_id": "prod_ai_insights_daily",
"query": {
"measures": [
{
"aggregation": { "function_name": "SUM", "column": "total_message_count" },


"alias": "total_messages"
},
{
"aggregation": { "function_name": "COUNT", "column": "user_id", "distinct": true },


"alias": "unique_users"
}
],
"filters": {
"condition": {
"column": "namespace",
"operator": "EQUALS",
"values": ["PROJECT_ALPHA"]
}
},
"time_dimensions": [
{
"column": "message_ts",
"granularity": "day",
"date_range": { "relative": "last_30_days" },
"fill_gaps": true
}
]
}
}
```


###


**3.2. The Compilation Pipeline**


The Serving Layer is effectively a compiler. It transforms the semantic intent of the JSON into dialect-specific, highly optimized ClickHouse SQL.


-


**Validation & Type Checking:** The engine verifies the request against the Table Registry. It ensures columns exist and that operators are valid for the data type.


-


**Expression Tree Resolution:** We support complex, nested arithmetic and logic that doesn't exist in the raw table (e.g., calculating "Profit Margin" on the fly).


-


**Time-Series Intelligence (fill_gaps):** If you query for "daily visits" and a specific day has zero visits, standard SQL simply returns no row. Our engine detects


fill_gaps: true


and injects ClickHouse's specific


WITH FILL


syntax, ensuring the API returns a continuous time series with zero-filled gaps.


###


**3.3. Performance Optimizations & Consistency**


The Serving Layer is "engine-aware." It knows how the data was ingested and optimizes queries accordingly.


-


**Handling Upserts (FINAL):** For tables utilizing the UPSERT strategy (


ReplacingMergeTree


), duplicate rows may exist temporarily. To guarantee consistency, the Serving Layer automatically detects this table type and appends the


FINAL


modifier to the query.


-


**Partition Optimization:** Simply adding


FINAL


can act as a performance brake because it forces merging. To mitigate this, our compiler injects settings to instruct ClickHouse to merge data only within partitions—massive performance gain for time-series data—while still guaranteeing correctness.


###


**3.4. Security:**


Exposing analytical data requires strict guardrails.


-


**SQL Injection Immunity:** The DSL makes injection mathematically impossible. User input is never concatenated into the query string. Even if a user sends malicious strings, the engine binds them as strict String parameters.


-


**Resource Governance:** To prevent "Bad Neighbors" (queries that hog cluster resources), we enforce:


-


**Max Limit:** Strict caps on row counts.


-


**Timeouts:** Short default timeouts with hard execution limits.


-


**Complexity Limits:** Validation fails requests with excessive nested aggregations or filter depth.


###


**3.5. Developer Experience**


We recognized that while a JSON DSL is powerful for machines, humans think in SQL. To ease adoption, we built a


**SQL translation endpoint** .


A developer can paste standard SQL into this endpoint, and the service returns the perfectly formatted JSON object required to run it. This tool has been critical for adoption, allowing engineers to migrate legacy queries in minutes.


###


Conclusion


Data-to-Production has transformed how we build user-facing data products by turning data ingestion and serving into a first-class, managed platform. Instead of treating data pipelines as offline, analytical workflows, we enable Data Engineers to expose DWH data directly to backend production services—safely, consistently, and with production-grade guarantees.


At the core of the platform is an end-to-end flow:


1.


**Ingestion** from Iceberg tables into ClickHouse, with built-in schema evolution, validation, automatic catch-up, and operational safeguards.


2.


**Serving** through managed services, allowing production services to consume fresh, query-optimized data as part of user-facing request paths.


This closes the traditional gap between analytics and production. What once required weeks of custom glue code, manual reviews, and risky handoffs is now a self-service path. Crucially, this is not just faster—it’s safer. By treating data exposure as a platform capability, we’ve dramatically reduced the Activation Gap and unlocked a new class of data-powered features.


This post was written by


**Almog Gelber**


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/) |


[TikTok](https://www.tiktok.com/@wix_engineering)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) ,


[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS?si=PcOf7Rx_RUmGojFj5n7CEA) or


[Google](https://podcasts.google.com/?feed=aHR0cHM6Ly9yYW5sZXZpLmNvbS9mZWVkL3dpeF9wb2Qv&ved=0CAAQ4aUDahcKEwjY3bLcy7_oAhUAAAAAHQAAAAAQAQ)
