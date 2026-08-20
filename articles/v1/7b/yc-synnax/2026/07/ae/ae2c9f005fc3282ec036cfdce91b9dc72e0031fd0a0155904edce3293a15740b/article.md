---
schema_version: "1.0.0"
document_id: "ae2c9f005fc3282ec036cfdce91b9dc72e0031fd0a0155904edce3293a15740b"
company_key: "yc-synnax"
company: "Synnax"
source_id: "yc-synnax-news-import-49a52373f89d"
canonical_url: "https://docs.synnaxlabs.com/blog/one-billion-rows"
published_at: null
first_seen_at: "2026-07-22T15:26:28.054745+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:ddb1d526ed64e710e893cc690edc09c6b79f28925ae5efc52345071c92c90f5c"
---

# Performance Compared: Synnax vs. Timescale vs. Influx | Synnax

[Blog](https://docs.synnaxlabs.com/blog)[One Billion Rows](https://docs.synnaxlabs.com/blog/one-billion-rows) Oct 15, 2024


## Performance Compared: Synnax vs. Timescale vs. Influx


#### See how Synnax compares in performance to other time series databases.


In this article we’ll compare the performance of Synnax to two other leading time series databases:[TimescaleDB](https://www.tigerdata.com/timescaledb) and[InfluxDB](https://www.influxdata.com/) .


Performance is one of many factors to consider when evaluating the time series database for your use case: scalability, reliability, and query language are also important to consider. Synnax, Timescale, and Influx all have their strengths and weaknesses. This article is the first in a larger series comparing Synnax to other software tools.


We’ll start off with a high-level overview of the three databases, explaining their architecture and ideal use cases. We’ll briefly overview our test methodology and setup, and then dive into the results.


## Database Overviews


Timescale, Influx, and Synnax are all designed to store and query time series data, but the authors had different workloads and feature priorities in mind when building them.


### Synnax


[Synnax](https://www.synnaxlabs.com/) is specifically built for storing and streaming hardware sensor data. Unlike the other two databases, it is not designed for general purpose time series workloads, such as user analytics, log data, or financial tracking.


#### Data Model


It follows a[channel](https://docs.synnaxlabs.com/reference/concepts/overview) -based data model, where every sensor, actuator, or data source has its own named bucket of data called a *channel* . Synnax’s query model is much simpler than the other two databases, and focuses on quality integration with programming languages through client libraries. It’s well suited for scientific and industrial applications.


Synnax supports real-time streaming of data, much like you would find in a tool like[Apache Kafka](https://kafka.apache.org/) or a protocol like[MQTT](https://mqtt.org/) . This makes Synnax ideal for industrial applications requiring real-time control, monitoring, and data processing.


#### Performance Characteristics


The Synnax storage engine,[Cesium](https://github.com/synnaxlabs/synnax/tree/main/cesium) , is designed for high performance and extremely low latency for limited-cardinality data. As we’ll show in the results, Synnax dramatically outperforms Timescale and Influx when ingesting, reading, and streaming large volumes of data from small numbers of sensors (1 - 10,000 channels at kHz rates).


Synnax performs comparably worse than Timescale and Influx when dealing with large numbers of sensors (millions of channels) at very low data rates (1 Hz or lower).


Synnax is so fast and reliable that teams often use it in high-performance, mission critical environments such as firing rocket and jet engines.


### Timescale


[TimescaleDB](https://www.tigerdata.com/timescaledb) is a general-purpose time series database built on top of PostgreSQL. Its flexibility allows it to serve a number of workloads including IoT, user analytics, and financial data.


#### Data Model


Timescale uses a traditional,[relational](https://en.wikipedia.org/wiki/Relational_model) data model where each row in a table represents a single event or measurement.


Because it extends Postgres, Timescale integrates well with existing SQL applications and makes it easy to link metadata with real-time telemetry. The Postgres database engine is one of the most widely used in the world, and is known for its reliability across a variety of workloads.


The relational data model is not without its challenges. Timescale requires the user to understand the scheme of their data before ingestion, and modifying it can be challenging. Timescale also requires knowledge of SQL to query data, which can be a barrier for engineering teams that are familiar with scientific analysis tools like MATLAB and NumPy.


#### Performance Characteristics


Timescale uses[hypertables](https://www.tigerdata.com/docs/use-timescale/latest/hypertables) to increase query performance and reduce storage overhead when compared to Postgres. While a large step up from vanilla Postgres, Timescale follows a row-based storage model, which struggles when ingesting and querying data at very high rates.


### Influx


[InfluxDB](https://www.influxdata.com/) is a NoSQL time series database also built for general purpose time series workloads. Its primary focus is on ease of use, allowing users to quickly ingest and query data without needing to define a schema upfront. It also integrates well with popular analytics tools, such as[Grafana](https://grafana.com/) .


#### Data Model


InfluxDB uses a[tagset](https://docs.influxdata.com/influxdb3/cloud-serverless/write-data/best-practices/schema-design/) data model. Each measurement has a set of tags, which are key-value pairs that describe the data. This makes Influx ideal for storing and querying data with many dimensions, such as user analytics.


## Test Summary


We’ll be comparing the performance of Synnax, Timescale, and Influx across three separate tests that focus on ingestion performance. We’ll be running the tests on a single machine with the following specifications:


**Processor** 12 Core Apple M2 Max


**Memory** 64GB DDR4


**Storage** 2TB Apple SSD


We’ll pull the latest open source versions of each database, which, at the time of writing, are:


- [Synnax v0.32](https://docs.synnaxlabs.com/reference/core/installation?platform=linux)
- [InfluxDB v2.7.10](https://www.influxdata.com/downloads)
- [Timescale v2.17.1](https://github.com/timescale/timescaledb/releases)


### Disclaimer


Performance benchmarks are often controversial, as companies cherry pick workloads optimized for their database architecture. **We’re doing the same here** . Our goal is to illustrate the scenarios in which Synnax shines: high-speed, low-latency data ingestion from relatively small numbers of sensors.


That being said, we’ll also show where Synnax falls short, and where Timescale and Influx are better suited. As always, we recommend running your own benchmarks on workloads suitable to your use case - it’s the best way to evaluate the performance of a database.


##


## Test 1


Varying Batch Size


In the first test, we’ll keep the number of channels constant at 20, and vary the batch size of data we insert into each database. This table shows the configuration of each test:


**Configuration** **1** **2** **3** **4** **5** **6** **7** **8** **9** **10**


**Channel Count** 20 20 20 20 20 20 20 20 20 20


**Batch Size** 10 50 100 500 1k 5k 10k 50k 100k 1M


**Batch Count** 5k 2k 1k 500 250 100 50 20 20 20


**Total Samples** 1M 2M 2M 55M 5M 10M 10M 20M 40M 400M


We’ll also measure the average memory and CPU usage of each database during the test.


### Throughput


Throughput Comparison: Synnax dramatically outperforms alternatives at large batch sizes. Tabulated Data - Throughput in Samples/s **Configuration** 1 2 3 4 5 6 7 8 9 10


**Synnax** 271k 1.3M 2.5M 11.9M 22.9M 64.1M 73.7M 30.8M 30.9M 63.6M


**Timescale** 96k 490k 895k 2.2M 2.7M 7.1M 8.9M 4.0M 4.3M 8.6M


**Influx** 270k 776k 1M 1.4M 1.5M 3M 3.9M 1.4M 1.5M 2.9M


At 10 samples per batch, all three databases perform comparably. As the batch size increases, Synnax’s performance increases dramatically, outpacing the capabilities of the other two databases.


### Memory Usage


Memory Comparison: Synnax uses more memory than alternatives. Timescale balloons in memory usage at large batch sizes. Tabulated Data - Memory usage in GB


**Configuration** **1** **2** **3** **4** **5** **6** **7** **8** **9** **10**


**Synnax** 0.32 0.305 0.290 0.290 0.290 0.310 0.330 0.330 0.520 1.1


**Timescale** 0.25 0.25 0.25 0.38 0.38 0.36 0.36 0.7 1 3.5


**Influx** 0.076 0.086 0.084 0.088 0.086 0.089 0.25 0.23 0.23 0.5


### CPU Usage


CPU Usage: Synnax uses more CPU at small batch sizes, and comparable CPU at large batch sizes. Tabulated Data - CPU Usage **Configuration** **1** **2** **3** **4** **5** **6** **7** **8** **9** **10**


**Synnax** 63% 65% 67% 68% 70% 72% 74% 76% 78% 80%


**Timescale** 25% 23% 26% 25% 27% 35% 50% 65% 75% 83%


**Influx** 47% 37% 49% 48% 43% 42% 86% 78% 82% 83%


##


## Test 2


Varying Channel Count


In the second test, we’ll keep the batch size constant at 1,000, and vary the number of channels we insert data for.


**Configuration** **1** **2** **3** **4** **5** **6** **7** **8** **9** **10** **11**


**Channel Count** 2 5 10 20 50 100 200 500 1k 5k 10k


**Batch Size** 1k 1k 1k 1k 1k 1k 1k 1k 1k 1k 1k


**Batch Count** 3k 2.5k 2k 1.5k 500 250 200 150 100 50 20


### Throughput


Throughput: Synnax outperforms alternatives at low channel counts, but starts to lose ground as the channel count increases. Tabulated Data - Throughput in Samples/s


**Synnax** 12.8M 27.25M 28.9M 79.6M 65.3M 92.6M 95.1M 94.6M 94.3M 72.3M 47.5M


**Timescale** 610K 1.7M 2.3M 9.4M 7.9M 15.7M 18.8M 28.5M 44.8M 44.9M 27.1M


**Influx** 233K 680K 981K 3.85M 3.7M 7.7M 10.2M 15.8M 24.6M 28.6M 22.4M


At low channel counts, Synnax outperforms both Timescale and Influx by a wide margin. As the counts increase past 1,000, Synnax’s performance begins to degrade, while Timescale and Influx remain relatively stable.


### Memory Usage


Memory Usage: Synnax uses more memory than alternatives. Tabulated Data - Memory Usage in GB


**Synnax** 0.21 0.205 0.212 0.205 0.210 0.230 0.260 0.280 0.320 0.380 0.540


**Timescale** 0.280 0.220 0.180 0.170 0.170 0.170 0.160 0.210 0.240 0.260 0.230


**Influx** 0.050 0.060 0.070 0.045 0.048 0.047 0.039 0.050 0.060 0.090 0.120


### CPU Usage


CPU Usage: Synnax consistently uses more CPU than alternatives. Tabulated Data - CPU Usage **Configuration** 1 2 3 4 5 6 7 8 9 10


**Synnax** 63% 65% 67% 68% 70% 72% 74% 76% 78% 80%


**Timescale** 25% 23% 26% 25% 27% 35% 50% 65% 75% 83%


**Influx** 47% 37% 49% 48% 43% 42% 86% 78% 82% 83%


##


## Test 3


One Billion Row Ingestion


Inspired by the[One Billion Row Challenge](https://www.morling.dev/blog/one-billion-row-challenge/) , we’ll be inserting one billion rows of data into each database to see how they handle the load.


We’ll use 50 channels, each with a batch size of 50,000 rows per channel to insert the data.


Synnax ingests one billion rows in 44 seconds, 12x faster than Timescale and 17x faster than Influx.


## Synnax Architecture


> What about Synnax’s architecture leads it to these performance characteristics? And why did our team focus on high throughput, small cardinality ingestion?


Thoroughly answering these questions requires a collection of articles and supporting information on database architecture, disk I/O, and the sensor data use case. We’ll summarize two of the most important points here. If you’re interested in the details, take a look at our[technical RFCs](https://github.com/synnaxlabs/synnax/tree/main/docs/tech/rfc) .


###


### Reason 1


Individual Files for Each Channel


Synnax is much closer to an object storage system than a traditional database. Storage engines like Timescale (Postgres) rely on a strategy calling *paging* to organize writes on disk.


#### Background - Paging, OLTP, and Concurrency Control


Data inside these databases is stored chunks called *pages* that are between 4 kB and 16 kB in size. These pages contain a certain number of rows. When accepting writes, the database engine will fill up a page in-memory, and then flush that page to disk in a single I/O operation once the page has reached capacity.


Pages are very efficient for databases serving OLTP workloads that have a high degree of random inserts and/or updates. For high speed ingestion of ordered data (such as time-series values), pages struggle for a few reasons:


1. **Complex concurrency control** -> When there is high read/write contention over a particular page, the database engine must implement complex concurrency control mechanisms to ensure data stays consistent.
2. **Copy operations** -> Pages are maintained as in-memory structures, which means that incoming writes must be copied from the application to the page. Data within pages is frequently moved to maintain order within the page. This results in a large number of copy operations.
3. **WAL Files** -> Modern OLTP databases are ACID compliant. The most common way to ensure ACID compliance is to use a write-ahead log (WAL). The WAL is a log of all the changes to the database. This means that I/O operations are repeated for each write.


#### Individual Files for Each Channel - A Seemingly Terrible Idea


Synnax does not use traditional paging strategies. Instead, Synnax maintains an individual file for each channel in the database. At first glance, this seems like a terrible idea: For 10,000 channels, we’ll need to maintain at least 10,000 files on disk. When writing to 10,000 channels concurrently, we’ll need to perform a random I/O operation for each one. This is disastrous for performance, especially when using an HDD.


For the sake of argument, let’s assume that disk I/O is not an important factor to consider (ridiculous, but bear with us). In this case, maintaining 10,000 individual files has a number of benefits:


1.


**Simple concurrency control** -> There is zero contention for writes to different channels. This means fewer locks, context switching, copy operations, and other overhead associated with ordering data. What about contention for writes to the same channel? For sensors, this is almost always a non-issue. Unlike application metrics, sensor data almost always arrives from a *single* source, which means that there is limited contention for writes to the same channel.


2.


**No WAL Files** -> The file for each channel is already append-only, so there is no need for a WAL file.


3.


**Extremely fast reads** -> The most common accessed pattern for a sensor-focused time-series database is to read a large number of values from a small number of channels. Synnax only needs a single, sequential I/O operation to read a continuous time range of data for a channel. Since writes are almost always append-only, the vast majority of reads do *not* need to contend with a write operation.


#### Why it’s not such a bad idea


When I/O is not a bottleneck, the benefits of individual files are very clear. In our implementation process, we researched the design of modern object storage systems. S3 is a black box, so instead we read the source code of[MinIO](https://github.com/minio/minio) .


As it turns out, MinIO keeps every object in a single file despite the obvious consequences. Why?


1. **Linux file systems scale surprisingly well** -> File systems such as` EXT4` ,` XFS` , and` BTRFS` maintain surprisingly fast lookup times for a large number of files. MinIO runs nodes with millions of files on a single machine.
2. **Random I/O is not that bad on SSDs** -> As it turns out, the consequences of large numbers of random I/O operations are not as bad as one might think. SSDs are quite good at random access when occupancy is very high. While a series of sequential, random writes is much slower than a single, sequential write, the throughput differences drop dramatically with an increase in the number of parallel, random write operations.


###


### Reason 2


Zero Copy Hot Path


The write performance of a database is fundamentally limited by the speed of the drive it is connected to (technically this is not true, but it’s a fair approximation given the speed of modern hardware and memory).


This means that the fastest possible write-optimized database is a simple log that asks the question: *How fast can we slam bytes to disk?* Modern database engines are very far from being simple log files, and for good reason. Benefits like ACID compliance, faster random reads, and other features far outweigh the costs of extra overhead.


Synnax serves a more simple read and write pattern than an OLTP database like PostgreSQL. As a result, we made optimizations that bring our storage engine closer to the speed of a simple log file.


Most notably, the vast majority of writes in Synnax require zero in-memory copy operations from the time they are received over the network until they are written to disk. This simpler pattern sacrifices complex queries and transaction patterns like serializable snapshot isolation, although we’ve found those unnecessary for the sensor data use case.


## Conclusion


Timescale, Influx, and Synnax all have their strengths and weaknesses. Timescale and Influx are great for general purpose workloads, and support a larger number of query patterns.


Synnax, on the other hand, is built for fast reads and writes of time-series sensor data. If this is the use case you’re interested in, we’d love to hear from you.[Contact us](https://www.synnaxlabs.com/#contact) to learn more.
