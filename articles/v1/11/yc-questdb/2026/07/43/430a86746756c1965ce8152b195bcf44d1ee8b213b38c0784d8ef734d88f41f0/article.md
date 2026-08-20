---
schema_version: "1.0.0"
document_id: "430a86746756c1965ce8152b195bcf44d1ee8b213b38c0784d8ef734d88f41f0"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/hdfc-bank-uses-questdb-for-mule-account-detection/"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-23T21:54:08.144992+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:3a383340c20712163464d653910423c35df61aa8bbd22092a0ca5da47b6fe2e8"
---

# HDFC Bank uses QuestDB for mule account detection across all major 25+ banking channels

[HDFC Bank](https://www.hdfcbank.com/) (HDFC Bank Limited) is one of India's leading private sector banks, headquartered in Mumbai, Maharashtra. It is the largest private bank in India by assets and market capitalization, and ranks among the top global banks by market capitalization.


HDFC Bank mainly operates through three main segments: Retail Banking – Individuals, small businesses; Wholesale Banking – Corporates, institutions; and Treasury – Investments and financial markets.


Every transaction HDFC Bank processes goes through its transaction risk monitoring system, and depending on the use case, suspicious patterns must be identified within milliseconds. To support these real-time processing requirements, HDFC Bank uses QuestDB as part of its in-house **Real-Time Streaming Platform (RTSP)** , built to process high-volume streaming transaction data with low latency. The current production implementation uses RTSP and QuestDB for real-time **mule account detection** .


INFO


A **money mule account** is a bank account used to receive and move funds obtained through fraud or other crime, layering the money to disguise its origin. The account holder may be a willing participant or an unwitting victim recruited under false pretenses. Detecting mule accounts means spotting the transaction patterns they leave behind, often spread across multiple channels.


Sub-100ms decisionsEvery transaction checked for fraud before approval


25+ channelsUPI, credit cards, online banking unified on one platform


Cross-channel detectionFraud patterns visible across channels for the first time


Sub-100ms decisionsEvery transaction checked for fraud before approval


25+ channelsUPI, credit cards, online banking unified on one platform


Cross-channel detectionFraud patterns visible across channels for the first time


## The challenge: mule detection at national scale


Banks need to monitor every transaction in real time for patterns that indicate fraudulent behaviour, or the suspicious account activity commonly associated with mule accounts. For a bank the size of HDFC, that means evaluating thousands of transactions per second, each one against multiple rule sets and models, while meeting strict latency requirements.


Examples of the rules evaluated include:


- Is this merchant blacklisted?
- Has the account performed more than **X transactions exceeding ₹Y during the last 15 minutes?**
- Have multiple transactions originated from geographically distant locations within five seconds?


Each transaction is also scored using ML and AI models. Based on the combined rule and model outcomes, the transaction is assigned a risk score together with the reasoning behind the decision.


Earlier, each channel was monitored in isolation, which meant cross-channel patterns were difficult to detect and correlate. HDFC Bank needed a single, high-performance data layer platform that could unify every channel and support both real-time rule evaluation and historical pattern analysis.


## Architecture: Kafka, Flink, and QuestDB


Money mule fraud detection architecture


The current deployment sustains **5,000 to 7,000 transactions per second (TPS)** on a single instance, processing that many transaction events per second on a **96-core machine with 700 GiB of RAM** . A primary-replica setup with object-storage-based replication provides resilience and fault tolerance.


QuestDB serves both sides of the fraud detection pipeline:


- **Real-time rule evaluation** : as each transaction arrives via Kafka and Flink, QuestDB is queried to check historical patterns for that account. For example, "is this transaction more than X times the total amount this account has transacted in the last 14 days?" Up to 300 such rules are evaluated per transaction on the card channel alone.
- **Historical aggregates** : precomputed aggregates are periodically loaded into QuestDB, where they are immediately available to both the rules engine and the machine learning models.


With full separation of compute and storage, HDFC Bank can scale capacity as demand grows without redesigning the architecture.


Metric Value


Sustained throughput 5,000 to 7,000 TPS on a single instance


Instance size 96 cores, 700 GiB RAM


Rules per transaction Up to 300 (card channel)


Volume growth since launch 6x


Target channels 25+


> Our objective was to build a platform that could support high-throughput transaction processing while maintaining sub-second analytical query performance. Working closely with the QuestDB engineering team allowed us to optimize the platform for our banking workloads and build a scalable foundation for future real-time analytics use cases.


Rana Sinha Ray


— Head Enterprise Competencies, HDFC Bank


## From money mule detection to Customer Level Monitoring and many more use cases


**Money mule account detection** is the first use case deployed in production. It identifies accounts being used to launder money through suspicious transaction patterns. This post-transaction analysis system flags anomalies and alerts the team for mule detection. Transaction volumes processed by the platform have **increased 6x** since deployment.


The next phase is **Customer Level Monitoring (CLM)** : end-to-end, real-time monitoring across multiple banking channels. Unlike mule account detection, CLM requires a fraud decision **before** the transaction is approved, moving from near real-time to true real-time.


By routing all channels through QuestDB, the bank can detect cross-channel patterns that were previously invisible when each channel was analysed in isolation.


CLM will cover transactions across major banking channels, including UPI, credit cards, internet banking, and others. The system is planned to roll out across **25+ channels** in a phased manner, reflecting both regulatory compliance requirements and the bank's long-term fraud prevention strategy.


The same platform will also enable other interesting use cases like upselling, cross-selling, customer engagement, real-time notifications, transaction alerts, and personalized recommendations. The possibilities of the platform are widespread.


Beyond fraud detection, the same streaming platform is being extended to support additional banking use cases without requiring a separate analytics infrastructure.


## Results


Before After


Each channel monitored
in isolation All channels unified
on one platform


Cross-channel patterns
hard to detect Cross-channel patterns
detected and correlated


Separate analytics stack
per use case One streaming platform
reused across use cases


Proprietary, siloed
systems Standard SQL for engineering,
risk and fraud teams


QuestDB's SQL interface enabled engineering, risk, and fraud teams to collaborate on the same platform using familiar tools, which accelerated adoption across the bank. The same platform is being reused across multiple use cases without lock-in to any proprietary system.


QuestDB has had a phenomenal role in shaping the overall architecture and reaching this milestone. The direct engagement of their CTO and Engineering teams went a long way in defining the product for the bank's specific use cases especially in areas where the QuestDB team understood the demands of the bank and created new or optimized existing features for the bank.


The collaboration has been truly exhilarating for both parties, and we look forward to continued engagement in fine-tuning RTSP wherein QuestDB is an integral part of the architecture for storage, compute, aggregation, time series retrievals of data within sub-second latency at extremely high throughput.


> QuestDB has been a key technology in the evolution of our Real-Time Streaming Platform. Its performance characteristics, SQL capabilities, and close engineering collaboration have helped us build a scalable architecture that supports both current fraud detection workloads and future real-time monitoring initiatives across the bank.


Zubin Kika


— Head Application Architect & Platform Engineering, HDFC Bank


## See also


- [Press release: QuestDB Supports HDFC Bank's Real-Time Transaction Monitoring and Risk Analytics](https://www.globenewswire.com/news-release/2026/02/16/3238706/0/en/QuestDB-Supports-HDFC-Bank-s-Real-Time-Transaction-Monitoring-and-Risk-Analytics.html)
- [QuestDB Enterprise](https://questdb.com/enterprise/)
- [More case studies](https://questdb.com/customers/)
