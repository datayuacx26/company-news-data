---
schema_version: "1.0.0"
document_id: "767dd3bffc1f8c7cee55a1383355eb77a0698c6732192d6bd99469493650fac4"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/aquis-case-study/"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-23T21:54:08.144992+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:938eb85663db6fea1d4e4a0662d9e16f7abdc73b8bf37e975a7ba3750d6f4476"
---

# Aquis Exchange (SIX Group) runs exchange-wide surveillance on QuestDB

[Aquis Exchange (SIX Group)](https://www.aquis.eu/) uses QuestDB for exchange-wide surveillance, analyzing orders, trades, and infrastructure metrics in real time.


Lower TCOCompressed Parquet format reduces storage costs


Petabyte scaleObject storage tiering for unlimited historical data


Fast ingestionLow-latency and high throughput ingestion


Consistent latencyPredictably fast queries alongside high ingestion speed


Fast analyticsAnalyze orderbook data on the fly with SQL


Relational + time-seriesJoin time series with relational data via SQL


Lower TCOCompressed Parquet format reduces storage costs


Petabyte scaleObject storage tiering for unlimited historical data


Fast ingestionLow-latency and high throughput ingestion


Consistent latencyPredictably fast queries alongside high ingestion speed


Fast analyticsAnalyze orderbook data on the fly with SQL


Relational + time-seriesJoin time series with relational data via SQL


## About Aquis Exchange (SIX Group)


Aquis Exchange (SIX Group) is a pan-European financial exchange operating cash equities trading businesses and providing markets for equity and debt products. They also develop trading and exchange technology used by third parties.


Aquis is regulated by the UK Financial Conduct Authority and France's Autorité des marchés financiers, operating Multilateral Trading Facility venues across the UK and EU27. They are the only European trading venue operating on a subscription pricing model.


From conversations with Paul Roberts, Head of Infrastructure at Aquis Exchange, we learned firsthand the scale and latency requirements of their infrastructure, and how QuestDB now powers market surveillance, real-time monitoring of exchange activity, and operational metrics.


## Capturing every market event in real time


Aquis needed a system that could capture every order, trade, and infrastructure metric for real-time surveillance and monitoring.


Because they operate regulated markets, timing and latency are critical. Their engineering teams generate comprehensive latency reports per security, per market, and per client. These reports measure the exact time it takes for an order—whether submitted through FIX or native binary protocols—to reach the matching engine and complete as an executed trade.


Overall market metrics using Grafana dashboards


They also track all incoming orders and trades, including counts, quantities, and notional amounts, aggregating this data to construct a live order book representing the tradable volume at each price level.


Timeline of market events visualized on Grafana


## Why they chose QuestDB for monitoring their infrastructure


Aquis required a high-throughput database capable of ingesting every event entering their exchange with consistent performance. Queries needed to be fast enough to react to exchange activity in real time and trigger alerts when anomalies occur. SQL compatibility was essential for simplifying onboarding and reducing operational complexity.


During evaluation, it became clear that QuestDB's architecture aligned closely with Aquis' requirements. Our team's understanding of low-latency data pipelines and market-data workloads helped us guide them toward an architecture optimized for throughput and query speed.


QuestDB's performance enabled Aquis to restructure their domain model so that order-book aggregation and transaction analysis could be computed live, something that was not possible with the alternatives they tested.


Our PostgreSQL wire-protocol compatibility allowed Aquis to plug QuestDB directly into Grafana, achieving seamless real-time visualization of both metrics and market activity. By joining time-series data with relational information, their teams gained the ability to extract richer analytics from their trading systems.


As their dataset continues to grow, Aquis benefits from QuestDB's continual performance improvements and release cadence, ensuring ongoing efficiency for both operational monitoring and market-data processing.


> QuestDB is a time series database truly built by developers for developers. We found that QuestDB provides a unicorn solution to handle extreme transactions per second while also offering a simplified SQL programming interface.


Paul Roberts


— Head of Infrastructure, Aquis Exchange


## See also


- [Press release published by Aquis](https://www.globenewswire.com/news-release/2022/11/01/2545177/0/en/QuestDB-Selected-by-Aquis-Exchange-to-Power-Its-Financial-Exchange-Infrastructure.html)
