---
schema_version: "1.0.0"
document_id: "b32ae89defd918b8a7a5b7f60c1a2b691d0b8658cd0859124cd463d7e1f298f9"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/okx-case-study/"
published_at: "2026-01-22T00:00:00+00:00"
first_seen_at: "2026-07-30T08:49:27.848388+00:00"
fetched_at: "2026-07-30T08:49:28.915959+00:00"
content_hash: "sha256:de077c7068ee4db9722ab8b3140e556385a080a9661a1cda9180718665578fe4"
---

# OKX relies on QuestDB for exchange-wide analytics

OKX is one of the world's largest cryptocurrency exchanges, handling billions of dollars in daily trading volume and serving millions of users worldwide. OKX's quantitative trading division requires real-time analytics, low-latency ingestion, and continuous performance at scale.


To meet these requirements, OKX selected **QuestDB** as its core time series database for market data analytics and trade execution analysis. They replaced legacy systems that could no longer keep up with ingestion rates and operational complexity, standardizing on QuestDB for its mix of high performance, simplicity, and open architecture.


Reliability at scale


Fault tolerance with distributed deployment


Operational simplicity


Stable ingestion, predictable query latency, and reduced maintenance load


Extreme performance


Handle volatile crypto markets with high-frequency data streams


## The Challenge


OKX handles streams of **high frequency market data** , capturing every quote, order fill, and event happening on the exchange. Each system must process large volumes of messages per second while ensuring data integrity and enabling fast query access for analytics.


Before adopting QuestDB, OKX used **InfluxDB** to store internal market data. Over time, ingestion performance degraded as data volumes increased, which led to operational friction and frequent re-tuning of clusters. Data retention had to be limited, query latency grew, and debugging slowdowns became costly.


OKX needed a system that could:


- Ingest high-frequency market events without bottlenecks
- Offer SQL capabilities for rapid analytics and monitoring
- Integrate easily with Apache Kafka and Grafana
- Deploy in their own cloud environments for data locality
- Support ~1 week hot retention while archiving raw data to object storage


## Key Components


**Apache Kafka integration:** OKX publishes internal market data and order fill streams to Kafka. QuestDB ingests those streams and runs behind a load balancer, allowing multiple instances to process data in parallel for scalability and resilience.


**Fast market data ingestion:** QuestDB's high-throughput endpoint provides ultra-fast ingestion from internal services while keeping writes schema-aware and simple to operate.


**Grafana dashboards:** Real-time metrics and candlestick visualizations are powered by Grafana dashboards connected directly to QuestDB.


**Hot + cold storage:** Around one week of hot trading data stays in QuestDB for instant analytics. Older data is archived to S3-compatible object storage. OKX is evaluating QuestDB Enterprise cold storage to automate this while keeping data queryable via the same SQL.


**Cloud deployment:** QuestDB Enterprise runs inside OKX's own cloud accounts for flexibility, cost control, and compliance. The initial setup ran on dozens of QuestDB instances across multiple AZs, with sub-second data for real-time dashboards.


## Architecture


## Results


- **Higher ingestion capacity.** QuestDB keeps up with high-frequency internal market data that previously caused bottlenecks.
- **Lower and more predictable latency.** Recent data is queried in milliseconds.
- **Simpler operations.** Stability improved after aligning configuration with the QuestDB team.
- **Better hybrid-cloud fit.** QuestDB runs directly inside OKX's infrastructure.


Metric Before QuestDB After QuestDB


Data ingestion throughput Limited by Influx bottlenecks Sustained millions of records/sec


Query latency Variable, often seconds Milliseconds


Operational overhead Frequent tuning and restarts Stable with minimal maintenance


Architecture Static, single cluster Distributed across 3 AZs


Data lifecycle Manual cleanup scripts Hot/cold tiering with S3-compatible storage


## What is next


- **High availability with write failover.** Moving toward QuestDB Enterprise Multi-Primary ingestion and automatic failover to read-replicas.
- **Cold storage extension.** Query archived data natively without manual exports.
- **Compression.** In-place conversion of partitions to Parquet for reduced storage.
- **Better integrations.** Shipping QuestDB logs and metrics to internal monitoring.


As one of the world's leading exchanges, OKX demonstrates how QuestDB can power mission-critical market data infrastructure at scale.
