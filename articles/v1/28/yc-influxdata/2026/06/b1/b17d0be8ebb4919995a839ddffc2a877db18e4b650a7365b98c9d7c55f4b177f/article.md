---
schema_version: "1.0.0"
document_id: "b17d0be8ebb4919995a839ddffc2a877db18e4b650a7365b98c9d7c55f4b177f"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/why-relational-databases-fail/"
published_at: "2026-06-19T08:00:00+00:00"
first_seen_at: "2026-07-20T23:23:58.693982+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:065cf65615b6e8b11dba30662e55804cdd184561b3acf00c64d7f00dfeefc4a0"
---

# Why Relational Databases Fail Satellite Telemetry

Table of Contents


Satellite operations depend on telemetry as the primary interface to systems that teams cannot directly inspect. Once a spacecraft reaches orbit, signals such as battery levels, temperature, signal strength, and fault codes become the foundation for understanding system health and maintaining control.


Telemetry streams continuously, so the underlying data system becomes a critical control point that needs to handle a constant, heavy flow of data. When that system cannot ingest, query, and manage data efficiently, dashboards lag, investigations slow, and the clarity teams rely on begins to erode.


## Relational data vs. satellite telemetry


Relational data organizes information into structured records with consistent fields and defined relationships. A business application might represent customers, orders, and products as related datasets, making it easy to answer questions such as which customer placed an order or which products were included in a purchase. This relational model works well when information is relatively stable and relationships between records are clearly defined.


Satellites are dynamic systems. Operators are not tracking static records or one-time transactions, but monitoring systems that change continuously. Telemetry arrives as a stream of time-stamped measurements, or[time series data](https://www.influxdata.com/time-series-database/#what-is-time-series) . Each value depends on when the system recorded it and its change relative to earlier readings.


A single battery reading of 80% may appear normal. In isolation, it provides limited insight. If that value declined from 95% over the past hour, the same reading indicates a different condition. Operators need the sequence, rate of change, and surrounding context to determine whether a change reflects normal behavior, an emerging anomaly, or a sign of impending failure.


As telemetry volume grows, individual readings lose meaning without historical continuity. Teams need to understand how values change over time, not just what they are at one moment. As that requirement grows, relational systems show their limits.


## Where relational databases start to strain


PostgreSQL- and MySQL-style databases can support early telemetry workloads, especially when teams already use them for operational data. The strain begins when a transactional, row-oriented database becomes the primary store for high-volume telemetry.


For these systems, telemetry challenges typically appear in three areas: query speed, data lifecycle management, and storage efficiency.


#### Speed and Query Performance


Satellite operations rely on dashboards, alerts, and analytics to maintain visibility into system health. When a subsystem begins overheating or signal strength drops during a contact window, teams need recent data immediately.


Relational databases such as PostgreSQL and MySQL can store telemetry, but they don’t optimize for high-volume time series workloads. As measurements accumulate, queries must process larger datasets, indexes expand, and the database spends more resources balancing continuous writes with analytical reads.


Slow queries affect more than latency. Delayed dashboards and alerts reduce the time teams have to detect anomalies, investigate issues, and make operational decisions. Teams may need additional infrastructure and ongoing tuning to keep systems responsive, increasing the cost of managing mission data.


#### Time Awareness and Data Lifecycle


Telemetry does not retain the same value over time. Recent measurements are critical for real-time monitoring,[anomaly detection](https://www.influxdata.com/glossary/anomaly-detection/) , and fault investigation, where operators need full-resolution data. Older data remains useful, but teams more often use it for trend analysis, reporting, and long-term performance evaluation.


PostgreSQL- and MySQL-style transactional databases do not manage this telemetry lifecycle by default. Teams can use partitions, scheduled jobs, archive tables, or custom pipelines to retain, move, or summarize older records, but those processes require separate design, maintenance and governance. Other relational systems handle historical analysis differently. Analytical databases and data warehouses can support large-scale scans and reporting, but continuous telemetry creates separate tradeoffs around ingest speed, data freshness, cost, and operational responsiveness.


For telemetry stores, lifecycle management becomes ongoing operational work. Without a clear strategy, raw measurements continue to accumulate, historical datasets grow harder to manage and queries may process more full-resolution data than operators need for long-term analysis.


#### Storage and Scale


Satellite telemetry generates large volumes of measurements, many of which share the same contextual information. Spacecraft identifiers, subsystem names, and sensor metadata often repeat across millions of records while only the timestamp and value change.


Telemetry schemas can also become large and sparse as missions add more sensors, subsystems, and measurement types. Not every field applies to every reading, but the database still has to store, index, and manage the growing structure around those measurements. As the number of sources and dimensions increases, data volume grows rapidly, and[cardinality](https://www.influxdata.com/glossary/cardinality/) expands, multiplying the number of unique series the system must store.


PostgreSQL- and MySQL-style transactional databases are not optimized for this pattern. Row-oriented storage works well when each row represents a distinct transaction or operational record. Telemetry behaves differently. It repeats similar context across continuous streams of time-ordered measurements, which can make compression less efficient and increase storage overhead.


Over time, organizations may allocate more infrastructure simply to retain and manage telemetry. What begins as a manageable archive can become increasingly expensive to maintain, especially when long-term historical data remains important for analysis and mission planning.


These limitations stem from storage models designed for transactional records rather than continuous, time-ordered measurements.


## Time series databases: A better fit for time-series data


Time series databases are purpose-built for data that changes over time. Instead of organizing information around static records and relationships, they structure data around timestamps, time ranges, and continuous streams of measurements.


This design matches how satellite telemetry behaves. Operators need to monitor recent readings, compare values across time windows, identify trends, and investigate anomalies using historical context. The database must support both high-ingest workloads and fast access to time-based data.


InfluxDB is a time series database built for these requirements. It provides a data layer optimized for telemetry, helping satellite teams power real-time dashboards, alerts, anomaly detection, and long-term analysis while avoiding many of the performance and scalability challenges that emerge when telemetry is stored in a relational database.


#### Maintaining Query Speed at Scale


InfluxDB 3 maintains query performance as telemetry volumes grow. Its architecture combines a real-time columnar engine with technologies designed for analytical workloads, helping teams retrieve and analyze large datasets efficiently.


Telemetry is stored in a columnar format, allowing queries to read only the fields they need instead of scanning entire records. Because data is organized around time, queries can focus on relevant time ranges rather than searching across the full dataset.


InfluxDB 3 also uses[Apache DataFusion](https://www.influxdata.com/glossary/apache-datafusion/) to power SQL queries. DataFusion applies filters early and processes data efficiently through[batches](https://www.influxdata.com/glossary/batch-processing-explained/) , reducing the amount of information that must be scanned and moved during query execution.


For satellite operations, these optimizations help keep dashboards, alerts, and investigations responsive even as telemetry volumes increase. Teams can access recent measurements and historical trends without the growing query overhead that often affects relational systems handling large-scale time series data.


#### Managing Data Over Time


Telemetry does not retain the same value over time. Recent measurements are critical for real-time monitoring, anomaly detection, and fault investigation, where operators need full-resolution data. Older telemetry remains useful, but teams more often use it for trend analysis, reporting, and long-term performance evaluation.


InfluxDB supports this lifecycle through retention and downsampling. Retention policies define how long data remains available, while downsampling converts older high-frequency telemetry into lower-resolution aggregates that preserve long-term trends.


This approach helps teams manage telemetry according to how it’s actually used. Recent data can remain detailed and readily accessible for operational workflows, while historical data can be summarized to reduce storage requirements and query costs.


By automating retention and downsampling, InfluxDB reduces the need for custom cleanup scripts, archive processes, and manual data management. Teams can spend less time maintaining telemetry pipelines while keeping storage growth and query overhead under control.


#### Reducing Storage Overhead at Scale


Telemetry adds both fresh and repeated data. **As missions add sensors, measurement types, and metadata, telemetry schemas can become large, sparse, and expensive to store** . InfluxDB 3 organizes telemetry in a columnar format built on[Apache Arrow](https://www.influxdata.com/glossary/apache-arrow/) and[Apache Parquet](https://www.influxdata.com/glossary/apache-parquet/) , which helps store similar values together and improve compression.


This matters for satellite workloads. Spacecraft, subsystem, and sensor labels may repeat across large volumes of readings, while different measurements may rely on different fields. A storage model designed for time series data can compress repeated values and sparse data more efficiently than a transactional model that treats each reading like a separate record.


Stronger compression helps reduce storage overhead as telemetry volume and cardinality grow, allowing teams to retain historical context without carrying the full storage cost of every raw measurement, repeated label, and unused field. InfluxDB 3 also supports architectures that separate compute from storage, giving organizations more flexibility to scale storage and query resources independently as data grows.


It’s a model that helps slow the growth of storage overhead: satellite teams can keep more telemetry available for analysis while reducing the infrastructure cost of storing large, repetitive telemetry datasets.


## Processing telemetry beyond storage


The value of satellite telemetry lies in understanding and responding to real-time spacecraft behavior. Operational impact comes from turning continuous streams of measurements into timely insights that help teams maintain visibility and make informed decisions with confidence.


InfluxDB is the foundation that makes that possible. By combining scalable telemetry storage with built-in processing capabilities, it helps satellite teams support real-time monitoring, accelerate analysis, and automate workflows that keep mission operations running smoothly.


Whether the goal is improving spacecraft health monitoring, reducing investigation time, scaling telemetry infrastructure, or preserving long-term mission insight, InfluxDB helps transform telemetry data into a continuous source of operational value.


2Get started with[InfluxDB 3 Core OSS](https://www.influxdata.com/products/influxdb/?utm_source=website&utm_medium=direct&utm_campaign=why_relational_databases_fail&utm_content=blog) or[InfluxDB 3 Enterprise](https://www.influxdata.com/products/influxdb3-enterprise/?utm_source=website&utm_medium=direct&utm_campaign=why_relational_databases_fail&utm_content=blog) to build a telemetry platform designed for time series workloads.
