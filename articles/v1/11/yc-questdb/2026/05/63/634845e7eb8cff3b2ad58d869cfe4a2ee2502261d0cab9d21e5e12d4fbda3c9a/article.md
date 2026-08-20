---
schema_version: "1.0.0"
document_id: "634845e7eb8cff3b2ad58d869cfe4a2ee2502261d0cab9d21e5e12d4fbda3c9a"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/one-trading-runs-a-regulated-24-7-futures-exchange-on-questdb/"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-23T21:54:08.144992+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:70dba64f092e4a75114a78cda5b0ee10eb42f738a46c52e6f459c7b316436d50"
---

# One Trading runs a regulated 24/7 futures exchange on QuestDB

[One Trading](https://onetrading.com/) is a regulated European derivatives exchange operating under MiFID II and MiCAR. They offer derivatives including dated futures across crypto and equity markets, serving retail, professional, and institutional clients across the EEA with full cross margining and portfolio margining.


One Trading is also the first venue to bring 24/7 central limit order book (CLOB) trading to equity futures, a round-the-clock product that only works if the data platform behind it can keep pace without interruption.


Their matching engine sustains 1.8 million orders per second, with round-trip latency held under 200 microseconds through close work with AWS on[cloud-native colocation](https://aws.amazon.com/blogs/industries/one-trading-exchange-and-aws-cloud-native-colocation-for-crypto-trading/) . Everything downstream depends on the data platform keeping up.


1.8M orders/secSustained throughput from One Trading's matching engine


21B+ rowsOver 21 billion rows across the deployment, 5 billion in the orders table alone


<200µs round-tripMatching-engine round-trip latency, via cloud-native colocation on AWS


1.8M orders/secSustained throughput from One Trading's matching engine


## The journey to QuestDB


One Trading's initial stack used Amazon DynamoDB and RDS, and later raw NDJSON files on S3 to keep up with write volume. These worked early on, but as trading activity grew the exchange quickly needed a purpose-built, high-performance database: one that could ingest streaming market data at full speed, serve queries in real time, and scale under ever-growing volumes.


### Evaluating alternatives


One Trading evaluated several databases before choosing QuestDB:


System Issue Limitation


TimescaleDB Performance Could not sustain high-volume ingestion.
Lacks capital-markets SQL primitives


Amazon Timestream
for LiveAnalytics Cost and performance Now deprecated by AWS. Cost model
unsuitable, performance a concern


InfluxDB Performance Could not sustain high-volume ingestion.
Slow on high-cardinality data


## What worked: QuestDB


QuestDB met the requirements the previous architectures could not satisfy simultaneously:


5M+ rows/secConstant ingestion straight from the matching engine


99.9% uptimeSustained under extreme production load


Array supportExplore full order book snapshots with SQL


Distributed architectureMulti-AZ deployment with automatic failover


Horizontal read scalabilityBacked by object-storage-based replication


Direct line to the teamSupport from the engineers who built QuestDB


5M+ rows/secConstant ingestion straight from the matching engine


99.9% uptimeSustained under extreme production load


Array supportExplore full order book snapshots with SQL


Distributed architectureMulti-AZ deployment with automatic failover


Horizontal read scalabilityBacked by object-storage-based replication


Direct line to the teamSupport from the engineers who built QuestDB


> QuestDB is an essential part of our trading platform - giving us a high-speed, scalable store for billions of trades that we can query in real time to power both customer-facing features and internal systems.


Stefan Blackwood


— CTO, One Trading


One Trading's QuestDB Enterprise deployment: primary + replica across two AZs


## Results


Metric Value


Total rows 21 billion (5 billion in the orders table alone)


HA failover time < 1 second


Production queries served 600 million


Downtime Zero


All from a single data platform. The use cases running against this deployment include:


- **Market surveillance** - real-time detection of market abuse on the exchange
- **Compliance oversight** - regulatory audit trails
- **Regulatory reporting** - automated report generation
- **UI queries** - powering the exchange frontend
- **Real-time API workloads** - serving live data to clients
- **Historical order books** - full order book reconstruction
- **Historical trading requests** - trade history and audit


As Stefan Blackwood, One Trading's CTO, put it at Future Alpha 2026: "The data platform stopped being the problem."


## Business outcomes


Self-hosting QuestDB Enterprise, rather than a usage-priced managed service, reshaped both the economics of the platform and the control One Trading has over it:


- **Predictable, controllable cost** - easier to predict as they scale, not a per-query usage-priced bill that jumps with unexpected activity spikes
- **Stable under load** - absorbs 1.8M orders per second with room to spare
- **Headroom to scale** - more replicas and new products, without re-platforming
- **Data sovereignty** - runs in their own cloud, aligned with security, privacy and DORA
- **AI-ready** - AI agents interact with QuestDB directly via the REST API, with a Web Console MCP coming in v10
- **No vendor lock-in** - open-format Parquet files feed complementary systems


> QuestDB is a platform we can plan around. It stays predictable and stable under our full production load, and it scales with us as we grow, which gives us the headroom to bring new products to market.


Steven Harper


— CSO, One Trading


## Trade-offs accepted


One Trading was open about the trade-offs they accepted when adopting QuestDB:


- AWS deployment documentation was not 100% complete at the time
- Not ideal for complex relational queries
- Required good schema design upfront
- Indexing was still a work in progress


Working through these as a QuestDB Enterprise partner - not just a licensee - meant direct access to the engineering team to resolve gaps, shape the roadmap around real production needs, and get fixes deployed on their timeline. Most of these items have since been addressed in the product, including indexing: the recent[QuestDB 9.4.0 release](https://questdb.com/blog/questdb-9-4-0-release/) shipped a new posting index for SYMBOL columns.


## See also


- [One Trading](https://onetrading.com/)
- [One Trading chooses QuestDB for its low-latency trading platform](https://www.globenewswire.com/news-release/2025/04/10/3059029/0/en/One-Trading-Chooses-QuestDB-s-Time-Series-Database-for-Its-Low-Latency-Trading-Platform.html)
- [QuestDB Enterprise](https://questdb.com/enterprise/)
- [QuestDB Customers](https://questdb.com/customers/)
