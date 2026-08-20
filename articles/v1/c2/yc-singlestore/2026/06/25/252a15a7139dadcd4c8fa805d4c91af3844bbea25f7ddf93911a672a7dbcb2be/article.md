---
schema_version: "1.0.0"
document_id: "252a15a7139dadcd4c8fa805d4c91af3844bbea25f7ddf93911a672a7dbcb2be"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/welcome-to-the-party-databricks/"
published_at: "2026-06-17T15:35:54+00:00"
first_seen_at: "2026-07-22T13:37:02.744795+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:b10af4a3e81836aaf547a99ab72dece76a9f2e69c2b829b15db2002e157f3c38"
---

# Welcome to the Party, Databricks

# Welcome to the Party, Databricks


Jun 17, 2026


•


4 min read


•


[Micah Bhakti, Director of Product Management](https://www.singlestore.com/blog/author/micah-bhakti/)


Yesterday, Databricks CEO Ali Ghodsi


[asked a question](https://www.forbes.com/sites/victordey/2026/06/16/databricks-ceo-says-hes-cracked-a-40-year-old-database-problem-with-ltap/) during the Data + AI Summit:


*"Show me which company on the planet today does not have two copies — one for transactional databases like Oracle or Postgres, and one for analytical processing. Who has merged these two?"*


The answer is SingleStore. It has been since 2019.


This is not our roadmap. It is our product, running in production for enterprise customers today.


Databricks' LTAP and Lakehouse//RT announcements are worth taking seriously — not because they invented a new category, but because one of the largest lakehouse vendors in the world just held a keynote arguing for our architecture. When Ghodsi says customers want one data copy, fewer pipelines, lower latency, and analytics on fresh operational data, he is describing exactly why SingleStore exists.


That is meaningful validation. It is also not the same architecture.


## LTAP is two engines. SingleStore is one.


Databricks' own description makes this clear. LTAP is "Lakebase plus Lakehouse." In Ghodsi's words in that same article: "an OLAP engine that accesses one copy and an OLTP engine that updates the records."


That is better than the alternative. The alternative is a brittle mess of CDC jobs and downstream replicas held together with Terraform, Slack alerts, and the institutional memory of one exhausted data engineer. We have nothing against that data engineer. They deserved a vacation they never got.


But sharing a storage layer is not the same as a unified query engine.


Customers who run two systems eventually produce a document called something like "Query Routing Guidelines v4." That document is always out of date. Somebody is always on call because something chose the wrong engine.


SingleStore runs transactions, analytics, ingestion, search, and application-serving workloads in one distributed SQL engine. No separate Postgres-compatible layer feeding a separate lakehouse runtime. No routing logic. No format translation between systems. No v4 of the guidelines document.


In production, that difference shows up in latency, in operational complexity, and in your on-call schedule.


## Postgres compatibility does not make Postgres horizontally scalable.


Lakebase is Databricks' serverless Postgres product. Postgres is excellent. Developers like it for good reasons, and there are a lot of good reasons.


Handling 10,000 concurrent agent sessions while simultaneously running analytical joins on live operational data was not on the original design brief.


That matters now because agents change the shape of database demand in ways dashboards never did. Agents fan out, ask follow-up questions, retrieve operational context, combine current state with historical data, and generate bursty concurrent load — while the business is still writing new records underneath them. Databricks' own data reflects this shift: they report that roughly 80% of databases on their platform are now created by agents rather than humans.


Reducing data copies is helpful. But agents need more than a fresher lake. They need broad operational context at low latency, under high concurrency, from a system that scales horizontally when load spikes. When the transactional layer is built on Postgres-compatible instances and the analytical layer lives somewhere else, the routing problem does not disappear. It just becomes someone else's 2am problem.


SingleStore was built for that load. Distributed-first, horizontally scalable by design.


## The benchmark is useful. It is not proof of LTAP.


Lakehouse//RT's headline latency results — the published millisecond charts — are measured on TPC-H Q6: a single-table scan with filters and a simple aggregation. Q6 is a legitimate query. It tests scan speed, predicate pushdown, vectorized execution, and columnar efficiency. It does not test joins.


Databricks separately cites 16x and 5x performance improvements over unnamed competitors, and those references do mention multi-join query patterns. That is relevant context. But a benchmark against an unnamed competitor, at an unspecified scale, with no absolute latency numbers and no published configuration, is not something a customer can replicate or verify. You cannot tell whether the 16x claim applies to your workload, to a narrow test case, or to a competitor running on a potato.


The millisecond headline is Q6. The join performance claim is relative and unverifiable. Neither, together, proves that Lakehouse//RT handles concurrent writes, operational reads, analytical joins, and agent/application load in one engine the way a production distributed database does.


The benchmarks are not false. They are doing different jobs. It is fair to ask which job each one is doing before treating the combination as proof.


## Welcome to the party.


Databricks is right about the destination. Enterprises do not want stale analytics bolted onto operational systems. They do not want complete data pipelines. They do not want AI agents reasoning over a copy of the business that is already an hour behind.


They want one platform that can ingest, transact, analyze, search, and serve applications in real time.


That is the category SingleStore has been building.


So, welcome. We have been here a while. The drinks are warm.


But let's not confuse a tighter lakehouse integration story with a production-grade distributed database for real-time AI applications. Shared storage is useful. Open table formats are useful. Postgres compatibility is useful.


None of those, by themselves, merge OLTP and OLAP execution into one horizontally scalable engine.


Databricks validated the problem. SingleStore built the database that solves it.


## On this page


- LTAP is two engines. SingleStore is one.
- Postgres compatibility does not make Postgres horizontally scalable.
- The benchmark is useful. It is not proof of LTAP.
- Welcome to the party.


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fwelcome-to-the-party-databricks%2F)


[Product](https://www.singlestore.com/blog/category/product/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog Why Shared Responsibility Isn't a Risk Transfer Product](https://www.singlestore.com/blog/cloud-database-shared-responsibility-model/)


[Blog Float16 Vector Type Support in SingleStore: Cheaper, Faster, Better Product](https://www.singlestore.com/blog/float16-vector-type-support-in-singlestore-cheaper-faster-better/)


[Blog Agentic AI: Why Enterprise AI Is Not Delivering on Its Promise Product](https://www.singlestore.com/blog/agentic-ai-why-enterprise-ai-is-not-delivering-on-its-promise/)


[Blog Why AI Fails Without Real-Time Data in Civilian Government & Public Services Product](https://www.singlestore.com/blog/why-ai-fails-without-real-time-data-in-civilian-government-public-services/)


[Blog The Lakebase Vision Is Right. Who Will Build It First? Product](https://www.singlestore.com/blog/lakebase-vision-is-right/)


[Blog Stop Waiting for the Future: Real-Time Data in One Platform Is Already Here Product](https://www.singlestore.com/blog/stop-waiting-for-the-future-real-time-data-in-one-platform-is-already-here/)
