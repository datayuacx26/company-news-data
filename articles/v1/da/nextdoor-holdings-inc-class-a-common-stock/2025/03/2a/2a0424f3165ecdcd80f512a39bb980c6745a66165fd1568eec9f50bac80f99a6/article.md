---
schema_version: "1.0.0"
document_id: "2a0424f3165ecdcd80f512a39bb980c6745a66165fd1568eec9f50bac80f99a6"
company_key: "nextdoor-holdings-inc-class-a-common-stock"
company: "Nextdoor Holdings Inc."
source_id: "nextdoor-holdings-inc-class-a-common-stock-rss-10247875dc01"
canonical_url: "https://engblog.nextdoor.com/scaling-nextdoors-datastores-part-1-234d0cf67665"
published_at: "2025-03-19T15:08:13+00:00"
first_seen_at: "2026-07-20T23:17:36.074163+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:723618e210e7bdf81816324cd13f191c0eb92e926ce28f7870b8bfcbb70b0a4f"
---

# Scaling Nextdoor’s Datastores: Part 1

# Scaling Nextdoor’s Datastores: Part 1


[Slava Markeyev](https://medium.com/@stlava?source=post_page---byline--234d0cf67665---------------------------------------)


6 min read


·


Mar 19, 2025


--


At Nextdoor, the Core-Services team is responsible for the primary set of databases and caches that power the Nextdoor platform. This blog series explores our 2024 initiatives to enhance the scalability of this critical infrastructure. When we sat down at the whiteboard we sought to address two related problems:


1. How can we reduce load on our primary database(s) and better utilize database read replicas?
2. How can we improve our cache consistency?


In this post we’ll provide a primer on the common industry-wide solutions we’ve previously employed along with discussing their caveats and pitfalls. In subsequent posts we’ll dive into the technical details of the components of our solution and how they fit together.


### Table of Contents


1. Background primer (this post)
2. [Decreasing database load with dynamic routing](https://engblog.nextdoor.com/scaling-nextdoors-datastores-part-2-513922e4b4b1)
3. [Appropriately serializing data for caching](https://engblog.nextdoor.com/scaling-nextdoors-datastores-part-3-e9b4dd8a9393)
4. [Keeping the cache consistent](https://engblog.nextdoor.com/scaling-nextdoors-datastores-part-4-c9d3d3edcd34)
5. [A time-bounded, eventually-consistent cache](https://engblog.nextdoor.com/scaling-nextdoors-datastores-part-5-5221da60f374)


## Background


Nextdoor’s backend, built using the Python-based Django web framework, powers the core product experience for neighbors, government agencies, and local businesses. The power of Django and similar frameworks (Rails, Spring, etc) is that they allow development teams to focus on implementing business logic rather than getting caught up in the details like learning and writing SQL.


The Object Relational Mapping, ORMs, included in these frameworks provide a lever that allows developers to define data models and relationships between them in the application’s language without ever needing to worry about SQL.


As some readers are all too aware, relational data modeling comes at a cost. Without careful data modeling, performant access to relational data largely depends on that data residing on monolithic databases.


NoSQL or distributed SQL datastores are often advertised as solutions to the scalability challenges of relational databases like PostgreSQL. However, many companies face significant obstacles in transitioning to these modern datastores. Their relational data models are deeply entrenched, and much of their business logic relies heavily on the power of relational data.


## Get Slava Markeyev’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


This was the position the Core-Services team at Nextdoor found ourselves in at the end of 2023. We had explored changing our data model to one that could properly leverage distributed SQL but we ran into existential challenges supporting existing relational queries, specifically multi JOINs, that were scattered across our codebase. Our small team simply could not rewrite business logic to avoid these queries and we couldn’t stop all product development to have other teams do this either.


## Common solutions


Before diving into our most recent solution, it’s worthwhile to discuss how we got to this point. Many platform teams often begin with a basic architecture of the application, via an ORM, talking directly to a relational database like PostgreSQL.


Press enter or click to view image in full size


## Caching


A ubiquitous pattern that arises in the backend infrastructure lifecycle is adding a cache to take load off of the database. A popular solution, one that Nextdoor employed, was adding a look-aside cache powered by Redis.


When using a cache, a few common points of consideration arise:


- You likely want to maintain the flexibility the ORM provides with the benefits of caching. Due to the ORM’s flexibility in querying complex relations, not all queries will be cache-able. What then?
- If performing a read with the intention of doing a write, engineers probably don’t want stale data from the cache. An escape hatch must be built in.
- How do product engineers interact with the cache and database?
- What is actually stored in the cache and how is it formatted? Will the application still be able to read what’s in the cache if the data model / schema changes?
- If the data in the database changes, how does the cache get updated?


To be clear, not all of these points need to be addressed when initially deploying a cache but they do creep up over time. Like many companies, Nextdoor solved these problems as they started to appear on the horizon. Later parts of this blog series cover components that solve many of these problems, so stay tuned!


## Read Replicas


Another common infrastructure improvement is adding database read replicas to serve queries that can’t be answered by the cache.


Press enter or click to view image in full size


This approach helps address database load on the primary but does not entirely solve the problem. Common issues that arise:


- Engineers may not be able to reason about the various consistency pitfalls that come with this architecture, nor should they if their goal is to simply build product features. If an option is provided, they will choose the perceived safest one: sending their queries to the primary database.
- Business logic will often need to perform some reads followed by a write. This will typically be wrapped in a transaction which will mean this must run on the primary database.
- In order to avoid consistency problems, populating the cache on a cache-miss will likely need to be done by querying the primary database. However, even if the primary database is queried, it does not guarantee that what gets written into the cache is the most up to date version of the data.


## Data Partitioning


Another common solution which teams will employ is splitting up their data across multiple physical databases. This strategy has several different approaches such as splitting by tenancy or breaking up foreign-key relations. Due to our existing data model and access patterns we chose the latter option of severing foreign-key relations such that a set of tables could be moved into their own physical database. While our ORM handled routing to the appropriate database depending on what was being accessed, it did require us to carefully rewrite some business logic.


This comes along two key caveats:


- This strategy is valid and may be the only option but it only prolongs the runways and does not solve the fundamental problem that a physical database can only be so big.
- Transactions which exist in business logic may now be subtly broken in that they no longer provide atomicity when dealing with successive writes to different databases.


Teams like ours may eventually find themselves back at square one where the primary database(s) are still a bottleneck and single point of failures despite the efforts to mitigate these problems. Through thoughtful data modeling and incremental work, the problem of database scalability doesn’t have to be immediately existential to businesses. Despite all of the caveats, the above strategies will add years of runways before the problem is well and truly existential.


## Next Evolution


When reevaluating our architecture and system behaviors, we aimed to alleviate the database load issue while also addressing complementary problems discussed in the caveats listed above. Specifically we wanted to:


1. Automatically route database queries to read replicas whenever possible.
2. Perform cache filling from the database read replicas.
3. Provide guaranteed eventual consistency of the cache in a timely manner.
4. Allow the application to be able to use what was in the cache even if a developer added a new field/column to a data model.


Check out[the next post in this series](https://engblog.nextdoor.com/scaling-nextdoors-datastores-part-2-513922e4b4b1) on how we employed dynamic routing to shift load from our primary database to the read replicas.


Authors:


[Slava Markeyev](https://medium.com/u/d8db977cf129?source=post_page---user_mention--234d0cf67665---------------------------------------)


,[Tushar Singla](https://medium.com/u/e734698562ba?source=post_page---user_mention--234d0cf67665---------------------------------------)


, and[Ronak Shah](https://medium.com/u/3d01db79be59?source=post_page---user_mention--234d0cf67665---------------------------------------)
