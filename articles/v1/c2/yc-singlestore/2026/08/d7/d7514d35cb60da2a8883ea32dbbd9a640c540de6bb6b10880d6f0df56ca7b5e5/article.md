---
schema_version: "1.0.0"
document_id: "d7514d35cb60da2a8883ea32dbbd9a640c540de6bb6b10880d6f0df56ca7b5e5"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/advertiser-dashboard-concurrency/"
published_at: "2026-08-14T10:15:12+00:00"
first_seen_at: "2026-08-14T20:18:11.586561+00:00"
fetched_at: "2026-08-14T20:18:14.367350+00:00"
content_hash: "sha256:bd10eda97a32195a1746d8ddad896bbc8a8529e472873772f299621adf91ecc0"
---

# Advertiser dashboards at thousands of concurrent users

# Serving advertiser dashboards to thousands of partners at once


Aug 14, 2026


•


5 min read


•


[Kevin Tran, Solutions Engineer](https://www.singlestore.com/blog/author/kevin-tran/)


A reporting dashboard is the central real-time analytics product for advertisers across the digital advertising ecosystem. It acts as the main control hub they interact with daily. In most stacks, the underlying architecture supporting it is designed for a different audience entirely: a dozen internal analysts running heavy queries occasionally, rather than thousands of customers running small ones at the same moment.


99% of the time, this architecture is built using a data warehouse. Warehouses are optimized for a small number of large, scan-heavy queries supporting those dozen of internal analysts.


The thousands of customers running concurrent, small, filtered aggregations on the other hand, don’t have the same foundational support. Instead, the queries they expect to respond in a matter of seconds lead to queuing and throttling warehouses.


AdTech platforms then look to alternative ways like scaling compute, but the architecture doesn’t remove the underlying root problem of costs. It only chooses who pays for it.


Let’s explore this further and see how to address it.


## **The Monday morning concurrency spike**


Advertiser activity across agencies, platforms, and ad networks tends to concentrate around predictable, high-traffic moments. . What this looks like is budget resets, campaign reviews, and quarterly reporting all happening simultaneously across agencies resulting in login volume and query concurrency spiking to several times the weekday average in under an hour.


In a warehouse, this surge drives utilization to a tipping point where queues form and query latency jump from two seconds to thirty. In other words, this performance degradation affects the largest spenders at the exact moment they are preparing high-stakes commitment decks!


Concurrency knee


## **The limitations of the caching mitigation stack**


The standard response to dashboard latency is building a familiar stack: BI extracts, cache layers, and pre-aggregated rollup tables. Engineers often favor this path because the natural instinct is to maximize existing tools rather than introduce a new system. This cycle continues until every combination of caches and rollups is exhausted.


Only then is an alternative sought.


Each of these additional layers swaps the latency issue for a new one, data staleness. That’s another problem we discuss further


[in the initial post.](https://www.singlestore.com/blog/adtech-serving-layer-sprawl)


In a real-time analytics environment of continuous streaming, cache invalidation failures create the most damaging artifact in client-facing analytics: the metric that moves backward upon refresh. While a slow response is interpreted as high load, a retreating number signals to the customer that the dashboard is unreliable, leading to aloss of trust that is both permanent and can resurface during billing disputes.


Pre-aggregation also imposes a significant cost on the product roadmap. By freezing the set of queries that can be answered efficiently, rollups turn simple requests for new breakdowns into pipeline tickets requiring sprint estimates. The platforms most dependent on pre-aggregation inevitably experience the slowest product evolution (though this cost is rarely acknowledged because it is paid in features that are never developed).


## **The True Cost of Concurrency in Consumption Models**


While the cost of a single query might seem negligible, the cumulative effect of concurrency under consumption pricing is anything but. When multiplied across an entire client base, these small costs skyrocket where the expense of delivering analytics scales exponentially in user growth.


Because AdTech loads are inherently spiky, the required capacity often scales even more aggressively than the customer count. Every new partner adds concurrent strain to the shared infrastructure, further steepening the cost curve.


If a reporting product's gross margin deteriorates as its adoption increases, the issue isn't all about pricing. Instead, it’s a structural failure cascading as a financial one. Attempting to solve this by renegotiating warehouse contracts merely addresses the symptom while leaving the underlying cause untouched.


## **Architectural Transformation via a Unified Serving Layer**


The solution lies in augmenting the existing data platform with a real-time analytics engine purpose-built for these types of workloads: one such as SingleStore. SingleStore handles massive volumes of filtered, aggregate queries against data that arrived only seconds ago, all while maintaining flat latency under heavy concurrency.


By caching common query plans using its


[patented Universal Storage](https://www.singlestore.com/blog/memsql-singlestore-then-there-was-one/) , the engine can manage multiple point lookups and aggregations from one place. Serving dashboards directly from the ingestion point eliminates the staleness window


[identified in the freshness analysis](https://www.singlestore.com/blog/ad-data-freshness) and removes the need for the external caching and additional layers that cause data drift.


It’s important to note that the serving layer is an augmentation of the existing warehouse, not a replacement for it. Deep, historical exploration and heavy back-end analytics remain the specialty of the warehouse. The boundary is centered on the distinction between two workload profiles: (1) concurrent, latency-sensitive, customer-facing tasks, and (2) occasional, scan-intensive, exploratory research on the other. A modern architecture assigns each to the engine best equipped for the job.


## **Evaluating the margin impact**


While a warehouse can theoretically serve advertiser dashboards if provided with sufficient compute, the real concern is the impact on gross margins due to the ongoing engineering cost of maintenance. Most platforms only confront the limitations of this architecture at renewal time. At that point, the suboptimal infrastructure has been overloaded for years.


At the same time, dashboards are no longer the sole consumers of the serving path. The rise of models and agents introduces a second concurrent audience with unpredictable query patterns and an absolute requirement for speed.[The next post in this series](https://www.singlestore.com/blog/graph-vs-distributed-identity) explores what these AI-driven clients require from the data layer.


## On this page


- The Monday morning concurrency spike
- The limitations of the caching mitigation stack
- The True Cost of Concurrency in Consumption Models
- Architectural Transformation via a Unified Serving Layer
- Evaluating the margin impact


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fadvertiser-dashboard-concurrency%2F)


[Engineering](https://www.singlestore.com/blog/category/engineering/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog Why Real-Time EdTech Needs Security Built In, Not Bolted On Engineering](https://www.singlestore.com/blog/edtech-data-security-student-privacy-rbac-ferpa-compliance/)


[Blog Real-Time Data Convergence: The Architecture Hi-Tech Teams Are Building for Agentic AI Engineering](https://www.singlestore.com/blog/real-time-data-convergence-architecture-agentic-ai-hitech/)


[Blog SingleStore vs. ClickHouse: Why Consistent Vector Search Latency Matters Engineering](https://www.singlestore.com/blog/singlestore-vs-clickhouse-why-consistent-vector-search-latency-matters/)


[Blog SingleStore vs. the Classic Data Stack: Why Real-Time and AI Break Patchwork Architectures Engineering](https://www.singlestore.com/blog/singlestore-vs-the-classic-data-stack-why-real-time-and-ai-break-patchwork-architectures/)


[Blog AI Database Examples With SingleStore Engineering](https://www.singlestore.com/blog/ai-database-examples-with-singlestore/)


[Blog Retrieval-Augmented Generation (RAG) for Real-World Machine Learning Engineering](https://www.singlestore.com/blog/retrieval-augmented-generation-rag-for-real-world-machine-learning/)
