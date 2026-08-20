---
schema_version: "1.0.0"
document_id: "66147d6e17b8053e9fb90b86bf76fddd5f142f6bd8a757bbc940aaae7a86a0a8"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/ad-data-freshness/"
published_at: "2026-07-20T13:44:26+00:00"
first_seen_at: "2026-07-24T13:42:01.434183+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:364942146efa29e08a815665ef6593523ccd68ced60ad0c40509e619aa9d6405"
---

# How fresh is ad data by the time anyone can query it?

# How fresh is ad data by the time anyone can query it?


Jul 20, 2026


•


5 min read


•


[Kevin Tran, Solutions Engineer](https://www.singlestore.com/blog/author/kevin-tran/)


While most advertising platforms label themselves as "real-time," the term is often applied loosely. It can mean the initial entry of events into a message queue or the data moving towards the dashboard. Once the scenario shifts, the performance follows, masking a significant gap between occurrence and accessibility. True real-time performance varies drastically, often mixing milliseconds of data ingestion with minutes of processing before a bidding model or campaign manager can actually interact with the data.


A more practical definition of **real-time ingestion** is the ability to query an event within seconds of its occurrence on the decision-making surface. However, typical production environments fall short: while events reach[Kafka](https://kafka.apache.org/documentation/) almost instantly, they then stall in consumer micro-batches, enrichment ETL tasks, and cache updates. Therefore, many organizations see total time from event to query in tens of minutes, with the 95th percentile often falling between fifteen and forty-five minutes. While the data stream itself moves in real time, the data available at the point of decision is anything but.


## **Where does the time actually go?**


The total latency of a data ingestion pipeline matters far more than the health of its individual stages in the advertising world. While a single event moves from producer to broker in mere milliseconds, the subsequent steps stretch the total delay. Because each data engineering team typically monitors only its segment against its own SLA, the end-to-end process lacks a unified owner. Therefore, a forty-minute delay composed of perfectly "healthy" individual hops remains invisible to standard monitoring, raising no alarms despite the foundational issue.


This cumulative latency is particularly damaging in AdTech due to the **rapid decay of data value** . The utility of an impression or conversion is highest in the immediate seconds following the event, providing the only window where budgets can be adjusted or bids repriced effectively. When a pipeline delivers information even minutes late, it is providing data for decisions that have either already been made using inferior signals or were missed entirely.


In an industry where millisecond auctions are vital, a forty-minute dashboard is not just slow; it is delivering data that has lost all its primary value.


The value decays before the data arrives


## **What breaks first while the data is in transit?**


Budget pacing breaks first in the way[the first post](https://www.singlestore.com/blog/adtech-serving-layer-sprawl) describes


****


: stale dashboards trigger corrections to problems that no longer exist and conceal the ones that do.


Launch verification breaks next. Where it breaks trust at the most expensive moment.


Take an advertiser launching a campaign at 9:00. They refresh the dashboard at 9:05 and see zeros. Spend is already flowing, but they don’t see the proof that the set will be delivered around 9:15. Frustrated with the inaccessibility to see budgeting, they open a ticket for the support team.


*First impressions of a reporting surface are formed on day one of a campaign, which is exactly when the lag is most visible.*


The sharpest version involves spend incidents. A campaign misconfigured at full spend burns budget at line speed for the entire length of the pipeline before any surface can show what is happening. Pipeline latency sets a hard floor under the size of every incident. With a thirty-minute path from event to queryable, thirty minutes of damage is the minimum, regardless of how fast the team reacts once the numbers appear.


## **What does closing the gap actually require?**


**Rather than optimizing individual hops, the structural solution is to eliminate the gap between data ingestion and querying entirely.**


When Kafka pipelines feed a low-latency engine where events are queryable the moment they arrive the vision of real-time accessibility becomes a reality. This shift transforms "seconds from occurrence to queryable" from a difficult tuning milestone into a fundamental property on


**the architecture**


.[The first post](https://www.singlestore.com/blog/adtech-serving-layer-sprawl) explores how an HTAP engine enables analytical reads on data written only moments prior


.


The path that remains untouched is the batch ingestion archival data and model training that still flow toward the lakehouse. Our focus is where the value curve of data is most concentrated achieving immediate business value for the customer-facing, serving path.


## **An Internal Diagnostic**


Most ad platform pipelines were architected for a different era, when the primary analytics consumer was an internal analyst reviewing historical data from the previous day. Today, advertisers who expect instance updates and automated model spending budgets at auction speed dominate the landscape. Despite this evolution, the data freshness contract remains the same.


A revealing internal exercise is to determine the end-to-end p95 latency across data pipelines, from the initial event to its availability on an advertiser-facing dashboard. Simply counting the number of teams required to provide that answer often reveals the extent of the underlying inefficiency.


The solution lies in eliminating the gap between ingestion and querying, rather than merely optimizing individual hops. By feeding Kafka pipelines into an engine that makes events queryable upon arrival, platforms can provide a single source of truth for pacing logic, models, and dashboards. This converts "seconds to queryable" from a tuning goal into a core architectural property.


While long-term storage and model training continue to leverage the lakehouse, the serving path requires a specialized approach. In AdTech, the value of data decays rapidly; therefore, the serving layer must be optimized for low latency and the immediate business value it generates.


This lag is particularly damaging when it affects identity data. We explore this topic further in the next post of the series.


****


## On this page


- Where does the time actually go?
- What breaks first while the data is in transit?
- What does closing the gap actually require?
- An Internal Diagnostic


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fad-data-freshness%2F)


Last modified - July 24th, 2026


[Engineering](https://www.singlestore.com/blog/category/engineering/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog The Moment a Lead Comes In: Building a Real-Time Lead Enrichment Agent with SingleStore Engineering](https://www.singlestore.com/blog/building-a-real-time-lead-enrichment-agent-with-singlestore/)


[Blog Unlocking Serverless MySQL: How Custom Handshake Interception Solved the Multi-Cluster Routing Problem Engineering](https://www.singlestore.com/blog/unlocking-serverless-mysql-how-custom-handshake-interception-solved-the-multi-cluster-routing-problem/)


[Blog Why SingleStore Customers Choose Zero ETL Over Reverse ETL Engineering](https://www.singlestore.com/blog/why-singlestore-customers-choose-zero-etl-over-reverse-etl/)


[Blog How Fast Data Ingestion Powers Real-Time AI Engineering](https://www.singlestore.com/blog/how-fast-data-ingestion-powers-real-time-ai/)


[Blog Protecto’s GPTGuard Uses SingleStore Vectors to Overcome RAG Limitations and Deliver Secure, Accurate Document Retrieval Engineering](https://www.singlestore.com/blog/protectos-gptguard-uses-singlestore-vectors-to-overcome-rag-limitations/)


[Blog Snowflake Too Slow? Unlock Real-Time Performance at Scale Product](https://www.singlestore.com/blog/snowflake-too-slow-unlock-real-time-performance-at-scale/)
