---
schema_version: "1.0.0"
document_id: "bf1add6402075254684e91c2517e89e86a4e61e2382227bb3368eb10833d5907"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/influxdb-3-11/"
published_at: "2026-07-30T07:30:00+00:00"
first_seen_at: "2026-07-30T16:00:11.802254+00:00"
fetched_at: "2026-07-30T16:00:14.101665+00:00"
content_hash: "sha256:16fdfce8010b0072c6676ee1887c86ede516692e47ef44a79364aafb500147da"
---

# What’s New in InfluxDB 3.11: A Significant Performance Upgrade for Complex Time Series Workloads

Summary


InfluxDB 3.11 delivers major performance and data management updates for growing time series workloads, with significantly faster queries on recent data, expanded support for wide and ultra-sparse schemas, and a more predictable resource profile under load. InfluxDB 3 Enterprise also adds backup and restore, bulk Parquet import, row-level deletes, and a built-in Explorer UI.


Table of Contents


Time series workloads rarely stay predictable for long. A system might begin with a few devices reporting a small set of measurements. As the fleet grows, new sensors come online, tags multiply, and query patterns change. A table that looked simple at the start becomes wide and sparse. A workload built around recent data expands into long-range analysis. As complexity grows, maintaining fast query performance and a predictable resource profile becomes much harder.


InfluxDB 3.11 was built for that reality.


Our last two releases gave users an early look at a set of performance improvements designed for heavier, more complex time series workloads. Today’s release of InfluxDB 3.11 makes those performance improvements GA in InfluxDB 3 Enterprise, bringing significantly faster queries on live data, greater flexibility for wide and ultra-sparse schemas, and more predictable performance under heavy load. The release also adds new Enterprise capabilities for backup and recovery, bulk data import, row-level deletes, and cluster operations.


## Performance and flexibility for heavier, more complex workloads


#### Faster Reads for Single-Series Workloads


Time series applications rely on fast access to recent data, whether it’s the latest reading from a device, the current state of an asset, a narrow time range from a sensor, or a live view that tracks ingest. **For highly selective queries, like those for a single time series, this new release of InfluxDB 3 Enterprise is up to 4x faster than previous versions** .


Actual performance will vary by workload, schema, hardware, and query shape. But for industrial monitoring, fleet management, observability, energy systems, and other applications built around live operational data, these gains mean faster access to the current state of the system.


#### More Room for Wide, Sparse, and Changing Schemas


Time series data rarely arrives in one clean shape. As devices, sensors, tags, and operating environments change, schemas can become wide, sparse, and highly variable. Rigid schema limits can force teams to split data across more tables, drop useful context, or design around the database instead of the application.


InfluxDB 3.11 expands schema flexibility with support for thousands of tables and millions of columns, while efficiently handling ultra-sparse datasets where only a small fraction of fields may be populated at any given time. Teams can model complex, changing telemetry around the data itself, with predictable query performance even as schemas grow wider and sparser.


For customers moving from InfluxDB 1.x or 2.x to InfluxDB 3, this brings forward the schema flexibility they know from InfluxDB, now with the full SQL query engine, object-store-based durability, and unlimited cardinality.


#### More Predictable Performance Under Load


InfluxDB 3.11 reduces the resource spikes that can occur during heavy ingest and compaction. Memory usage is more predictable, and compaction runs with a more consistent resource profile, making it easier to understand what a workload actually requires.


Teams can size infrastructure with more confidence instead of holding extra capacity for occasional spikes. Query responsiveness stays steadier under load, while memory and infrastructure costs are easier to plan for as workloads grow.


## New data management capabilities for Enterprise


InfluxDB 3.11 also adds new Enterprise capabilities for backing up, importing, and deleting data:


- **End-to-End Backup & Restore** : You can now run full or incremental backups to capture compacted data. Restores run asynchronously, providing disaster recovery and allowing you to roll a live cluster back to an earlier point in time.
- **Bulk Import from Parquet** : Bringing existing or external data into InfluxDB 3 is now much simpler. A new bulk-import feature lets you upload an entire directory of Parquet files to ingest the data directly.
- **Row-Level Deletes** : Deleting individual rows is inherently more complex in columnar storage where data is optimized to be written and read in larger blocks. InfluxDB 3.11 now lets you target specific data by time range or tag predicate, without having to drop an entire table.


## InfluxDB 3 Explorer UI, now built in


InfluxDB 3.11 brings the Explorer UI directly into InfluxDB 3, giving users a visual interface for querying, exploring, and managing their data out of the box.


Explorer goes well beyond basic data visualization. You can query data using SQL, InfluxQL, or natural language, convert existing Flux queries to SQL with AI-assisted explanations, and manage Processing Engine plugins from the built-in plugin manager. Explorer also includes schema browsing, sample data generation, visualization, and live instance monitoring in one interface.


## Get started with InfluxDB 3.11


InfluxDB 3.11 brings the performance improvements introduced in our last two releases to GA, with faster single-series queries, support for wide and ultra-sparse schemas, and more predictable performance under load. New Enterprise data management capabilities and the built-in Explorer UI make these improvements easier to deploy and manage in production.


As time series workloads grow, so do demands on the database. InfluxDB 3.11 is built to maintain performance as complexity increases.


[Download the latest version](https://www.influxdata.com/downloads/?utm_source=website&utm_medium=direct&utm_campaign=influxdb-3-11&utm_content=blog) of InfluxDB 3 Core or Enterprise, pull the newest Docker image, or check out the docs ([Core](https://docs.influxdata.com/influxdb3/core/release-notes/?utm_source=website&utm_medium=direct&utm_campaign=influxdb-3-11&utm_content=blog) ,[Enterprise](https://docs.influxdata.com/influxdb3/enterprise/release-notes/?utm_source=website&utm_medium=direct&utm_campaign=influxdb-3-11&utm_content=blog) ) to get started.
