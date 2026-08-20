---
schema_version: "1.0.0"
document_id: "7a37d0b42f45abe2c86e33b46469e29bcca0eb5fa7b4172fd1b3bc3968c0e14d"
company_key: "airbnb-inc-class-a-common-stock"
company: "Airbnb Inc."
source_id: "airbnb-inc-class-a-common-stock-rss-d1a7958eece3"
canonical_url: "https://medium.com/airbnb-engineering/scaling-airbnbs-identity-graph-with-a-unified-knowledge-graph-infrastructure-ebac467b7836"
published_at: "2026-05-19T17:01:01+00:00"
first_seen_at: "2026-07-20T04:35:41.377728+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:3d1427613576bb283c980ca066134c579ffe4d54238cb3e047237b6048f30b90"
---

# Scaling Airbnb’s identity graph with a unified knowledge graph infrastructure

# Scaling Airbnb’s identity graph with a unified knowledge graph infrastructure


[Lucen Zhao](https://medium.com/@lucen.zhao?source=post_page---byline--ebac467b7836---------------------------------------)


8 min read


·


May 19, 2026


--


How Airbnb shifts from PaaS to an internal knowledge graph infrastructure at scale.


Press enter or click to view image in full size


**By:**[Lucen Zhao](https://www.linkedin.com/in/lucen-zhao-56772bb8/) ,[Shukun Yang](https://www.linkedin.com/in/shukunyang/) ,[Ashish Jain](https://www.linkedin.com/in/ashish-jain-55127425/)


Knowledge graphs offer a natural and powerful way to represent relationships between entities. Many real-world systems are fundamentally about connections.


Airbnb’s identity graph captures relationships between users in a graph database. The identity graph serves aggregated insights that enable user identity resolution and relationship understanding. These capabilities support a wide range of Trust and Safety use cases, from detecting suspicious activities to identifying linked accounts. Over time, the identity graph has grown into one of the largest and most complex graph data products at Airbnb, both in terms of scale and the complexity of queries it supports.


In 2024, Airbnb began investing in a new, internally managed, paved-path graph data platform to build a unified knowledge graph infrastructure. Airbnb’s identity graph became one of the first systems to adopt this platform. In this post, we’ll walk through the foundations and challenges of the identity graph, introduce the architecture behind the graph infrastructure, and highlight several key optimizations that emerged during the onboarding process.


## Airbnb’s identity graph


Airbnb’s identity graph is a critical foundation layer, playing an important role in Trust and Safety applications. It contains two major components:


- Graph data storage: a storage layer composed of a graph database and a key–value (KV) caching layer. It models users and relationships as vertices and edges. Most data is ingested in near real-time through asynchronous events and served through low-latency, real-time service calls.
- Graph service: this service provides a unified interface for accessing graph data. It retrieves data from underlying sources, including the graph database, applies aggregation logic or models as needed, and serves the results to downstream customer services.


### Evolution of the identity graph architecture


The identity graph architecture progressed through three major iterations. It started with a relational database for user and entity data, paired with a KV store holding JSON‑encoded edge lists, a pattern that became difficult and expensive to scale as graph density increased. A third‑party SaaS graph database replaced the KV store in 2021, improving horizontal scalability but introducing long‑tail latency, operational instability, and limited ability to tune performance or enforce fine‑grained access controls. As part of ongoing system enhancements, the system was migrated to a graph infrastructure: an internally managed, high‑performance graph platform built to support low‑latency, large‑scale graph workloads.


A number of challenges persisted during the evolution of the identity graph:


- **Scalability** : The identity graph consists of 7 billion nodes and 11 billion edges. The data is fast-growing, at a speed of roughly 5 million new edges per day, which imposes a huge scalability challenge on the write side.
- **Query complexity** : The graph read queries typically contain 4–8 hops for the majority of identity graph use cases, posing a huge challenge in meeting the latency requirements of critical data flows.
- **Long-tail latency** : Naturally, the density of the graph data structure varies within each subgraph, and hitting high fanout nodes during the graph traversal can significantly increase the amount of data accessed, which would result in huge performance degradation. The P95 and P99 of graph queries can grow disproportionally long compared to P50.
- **Stability** : Slow queries can take up a large amount of resources during their executions, which can result in stability issues in graph DB performance.


## Airbnb’s graph infrastructure


Before the centrally managed graph infrastructure, graph adoption at Airbnb was fragmented. Teams typically fell into one of four anti-patterns, each with significant operational overhead:


- **Relational “graphs”:** Modeling nodes and edges in SQL tables, resulting in expensive joins during traversal.
- **Offline graphs:** Building graphs in the data warehouse, which limited data freshness to daily snapshots.
- **DIY open source:** Self-managing community versions of graph DBs, leading to high operational toil.
- **Managed PaaS:** Using third-party vendors, which introduced vendor lock-in and performance bottlenecks.


To solve this, we built an internal graph infrastructure, a paved-path, multi-tenant platform designed to bring our use cases together under a single, supported infrastructure.


### The tech stack: JanusGraph + DynamoDB


Graph databases vary widely in storage design, schema models, and query languages. We evaluated options based on four requirements:


1. **Scalability for online queries**
2. **Expressive schema and query capabilities**
3. **Fit with Airbnb’s infrastructure and operational model**
4. **A visible, extensible codebase**


We chose **JanusGraph** (a distributed, open-source graph database built on Apache TinkerPop), with **DynamoDB** as the storage backend and **OpenSearch** for indexing. JanusGraph’s labeled property graph model provides strong schema support, and **Gremlin** enables expressive traversal queries.


This combination offers a unique advantage: **Storage separation.** Because JanusGraph supports pluggable storage backends, we were able to leverage the scalability and reliability of AWS DynamoDB for the persistence layer while maintaining full control over the graph logic layer. This allowed us to iterate quickly on graph features without reinventing the wheel on distributed storage operations. Moreover, we have the ability to evolve the storage layer over time as our internal persistence platform matures.


### Architecture & optimizations


Airbnb’s knowledge graph infrastructure provides a managed experience where each tenant (such as the identity graph) operates in an isolated namespace. We built a management service on top of JanusGraph to handle schema enforcement, index management, and schematized Thrift APIs.


To meet Airbnb’s latency requirements, we made several key optimizations to the core JanusGraph engine:


- **Optimized transactions:** JanusGraph’s default locking can be heavy. We implemented a custom transaction strategy leveraging DynamoDB’s conditional writes and transaction APIs to ensure data integrity with lower overhead.
- **Parallel query execution:** We improved the **getMultiSlices** interface to fetch data in parallel, significantly reducing latency for high-fanout queries.
- **Observability:** We integrated Airbnb’s distributed tracing into our internal fork, closing the observability gap present in the open source version.


Press enter or click to view image in full size


This architecture now supports critical use cases across the company, including fraud detection, inventory knowledge graphs, and data lineage.


## The migration


We moved from a vendor-provided solution to an internally-built solution based on the open source Apache Tinkerpop graph computing framework, which includes the Gremlin graph query language. The change has delivered considerable improvements in performance and reliability.


### Airbnb’s knowledge graph infrastructure architecture


The identity graph service consists of four applications: two for event-based data ingestion and bulk loading, another two for data serving and pre-computation of complex graph queries. For both the internal solution and the previous third-party vendor solution, read and write traffic are isolated in the graph computation engine layer.


Press enter or click to view image in full size


Both graph engines also support Gremlin, enabling us to benchmark Gremlin queries side-by-side when serving shadow traffic. After benchmarking, internal graph infrastructure was used to start serving production traffic before the vendor solution was deprecated.


### Client-side query optimization


Even though both graph engines support the Gremlin query language, they applied very different optimizations over TinkerPop query steps during the query planning phase. Thus, during the migration, identical Gremlin queries produced significantly different performance between Airbnb’s graph infrastructure and the third party vendor. To address this and enhance the performance of graph queries, optimizations were made on both the JanusGraph side and the client side.


Client-side optimization included a series of query rewriting improvements, including:


- Removal of Path steps: Path steps in Gremlin, such as Path or SimplePath, are not optimized as batched queries within JanusGraph, and many of the queries would fall back to slow, non-batched backend queries, which can occupy large connections in the backend storage thread pool. Thus, the path steps are removed wherever possible, and replaced with a series of conditional queries to ensure the returned results are acyclic.
- Side-effect step optimization: Aggregation within side-effect steps may not be fully optimized within JanusGraph query planning strategy, resulting in non-batched substeps. Thus, side-effect steps were modified to minimize the amount of computation.


### Gains from the migration


Integrating the identity graph with the internal infrastructure has delivered meaningful improvements in performance, stability, and scalability:


- **Performance** : The new, internal solution outperforms the previous third-party vendor in all graph query patterns. This led to huge improvement in the latency of the end-to-end read API, which typically involves multiple graph queries. Significant P99 latency reduction also demonstrates our success in reducing long-tail latency in complex graph queries.
- **System stability** : The previous vendor’s solution required periodic manual instance reboots to maintain optimal performance.These reboots are no longer needed with the internally hosted solution. Plus, our ability to manage the new solution internally yields shorter response times for incidents and more transparent incident investigations.
- **Scalability** : The new, internal service supports auto-scaling. During load tests, the write queries per second (QPS) was successfully scaled to ten times the previous solution’s write QPS.


Press enter or click to view image in full size


Press enter or click to view image in full size


## Conclusion and future plans


Airbnb’s knowledge graph has demonstrated significant improvement in both query performance and system stability compared to the third-party vendor identity graph we had been relying on. Enabling multi-step queries in a broader range of queries in JanusGraph has improved overall query performance, while client-side query optimizations helped greatly in shrinking long-tail query latency. Our ability to manage the new system internally enables faster and more transparent incident investigations.


If this type of work interests you, check out some of our[open roles](https://careers.airbnb.com/) .


## Acknowledgements


Special thanks to Pawan Rathi, Zach Fein, Cong Zhao, Peter Li, Haiyang Han, Jisheng Liang, Abhishek Ravi, Yi Li, Adam Kocoloski, Kaushik Srinivasan, and everyone else who supported this project. The infrastructure upgrade would not be such a huge success without contributions from these people.


We also want to thank Primus Lam and Rajan Jon for their support in authoring this post during their time at Airbnb.


*All product names, logos, and brands are property of their respective owners. All company, product, and service names used in this website are for identification purposes only. Use of these names, logos, and brands does not imply endorsement.*
