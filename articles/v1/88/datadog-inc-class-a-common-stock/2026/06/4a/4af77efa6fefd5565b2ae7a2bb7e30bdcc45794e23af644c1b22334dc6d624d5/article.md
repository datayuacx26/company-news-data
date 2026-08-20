---
schema_version: "1.0.0"
document_id: "4af77efa6fefd5565b2ae7a2bb7e30bdcc45794e23af644c1b22334dc6d624d5"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/engineering/ai-assisted-storage-migration/"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:83e45f8cd88f6f24c8839f4f5b76fca263a74c22073fcf8f60860db8fd70db96"
---

# How we migrated a live routing system using AI-assisted refactoring

Arnold Wakim


When the storage backend for Stream Router hit hard limits, we needed to redesign its data model and migrate it to a new storage architecture without disrupting live production traffic. We would not have completed the implementation in the time frame we had without AI tools.


We used Claude and Cursor to accelerate a systematic, test-driven refactoring process. They weren’t generating code autonomously: For each method, we provided the old implementation, the new schema, and a failing test. The models would generate a first pass, and the tests told us whether it was correct.


We were curious whether AI could help us safely evolve a critical production system. This post is about what worked, what didn’t, and what we learned along the way. We’ll walk through the migration itself, the workflow we used, what gave us confidence in the migration, and where the models were useful versus where they still required human expertise.


Before we get into the migration, it’s worth understanding the system we were changing.


At Datadog, we ingest massive volumes of metrics data every second as part of a platform that processes over a hundred trillion events per day. Routing that data correctly is just as important as ingesting it. Every datapoint then needs to be routed to the right Kafka cluster, topic, and set of partitions so it can be stored and queried correctly, and those routing decisions are constantly changing as our infrastructure evolves. (For a deeper look at the full metrics pipeline, see our[overview of the metrics platform](https://www.datadoghq.com/blog/engineering/timeseries-indexing-at-scale/#metrics-platform-overview) .)


Once collected, datapoints are processed and written to Kafka, a durable message broker, before being routed to various storage and query systems. At this scale, Datadog’s internal services need to know exactly which Kafka cluster, topic, and sharding and load-balancing strategy to use for each datapoint. Those answers change constantly as infrastructure evolves.


Stream Router, an internal control plane service at Datadog, provides those answers. It tells producers where to write and queriers where to read, and maintains a history of where data has been at any given point in time. Stream Router does not produce or consume Kafka messages at runtime. Instead, it manages the routing decisions that other services use to configure their own Kafka producers and consumers. At this scale, routing decisions are critical to the health of the metrics pipeline. A bad routing change can have consequences far beyond Stream Router itself.


## From config file to control plane


In 2016, routing for the metrics pipeline was managed through a three-line configuration file distributed to every service. As Datadog grew, so did the file—from a couple of lines to thousands. It was edited by hand and rolled out manually. And despite existing safeguards, managing changes became increasingly cumbersome and operationally heavy at scale.


Stream Router replaced that workflow with a centralized gRPC service backed by automated orchestration. Routes were managed through APIs instead of file edits, and rollouts became gradual and automated.


To support high availability and fault tolerance, Stream Router was designed with an eventually consistent architecture that decouples writes from reads. The write path was backed by FoundationDB using a key-value (KV) model, which fit well with Datadog’s infrastructure and operational model at the time. But as the system grew, that model began to show its limits. The read path relied on RocksDB, serving static snapshots of the write path to high-throughput query traffic.


The following diagram shows this architecture. Administrative changes flow through the write path into the database, which periodically exports snapshots to object storage. On the read side, distributors restore those snapshots into an in-memory database and serve routing queries from there. Producers and queriers never talk to the write path directly.


Stream Router’s architecture separates write and read paths. Routing decisions are written to a database, exported as snapshots to object storage, and restored into in-memory distributors that serve routing queries to producers and queriers. Stream Router’s architecture separates write and read paths. Routing decisions are written to a database, exported as snapshots to object storage, and restored into in-memory distributors that serve routing queries to producers and queriers.


This design worked for a while, but the storage layer carried a trade-off that the team didn’t fully anticipate. As the routing table grew and operators needed to handle more routes at once, critical operations began hitting FoundationDB’s transaction size limits. The system that had replaced manual configuration was now becoming a bottleneck itself.


## When the KV model stopped scaling


To understand why the KV storage hit limits, it helps to look at what Stream Router actually stores.


A **route** is the core unit of work: It maps a customer’s payload to a specific Kafka stream. But a route doesn’t exist in isolation. It references a **sharding strategy** , which tells producers how to partition data across that topic. A route is activated by a **rule** , which controls when and how it takes effect for a given customer.


These entities are inherently relational. Routes reference streams and sharding strategies. Rules reference routes. Business logic validation needs to reason across all of these relationships at once to help ensure consistency before any change takes effect.


In the KV model, the code had to reconstruct these relationships. It would pull tens of thousands of entries into pod processes and effectively act as a relational database in-process—handling logic that foreign keys would normally enforce in a relational system.


The cost showed up in concrete ways. Some operations hit FoundationDB’s transaction size limits. We first explored the obvious shortcut: swapping FoundationDB for PostgreSQL while keeping the same KV access patterns. That didn’t work either. The most demanding operations were estimated to take 45 minutes, since they still required thousands of sequential round trips to the database.


The bottleneck came from the data model and the application logic built around it, not the database itself.


We needed a full redesign: a new relational schema and new storage engines. Most importantly, we needed to do it without breaking a system that serves live production traffic to every metrics customer.


## Designing a schema that matches the data


Before writing any code or involving AI tools, we redesigned the schema by hand to properly reflect the relationships between domain entities.


The simplified schema below captures the core structure: Streams and sharding strategies are referenced by routes, which are in turn referenced by rules. Each entity maps directly to the domain concepts introduced earlier, and the foreign key relationships replace what previously had to be reconstructed in application code. (Only primary and foreign keys are shown; each table includes additional columns omitted for clarity.)


Simplified relational schema for Stream Router, where routes reference streams and sharding strategies, and rules reference routes, allowing relationships and constraints to be enforced directly in the database. Simplified relational schema for Stream Router, where routes reference streams and sharding strategies, and rules reference routes, allowing relationships and constraints to be enforced directly in the database.


For the write path, the choice was straightforward. Datadog had been building a[self-managed PostgreSQL platform](https://www.datadoghq.com/blog/engineering/unwinding-shared-database/#platform-investments-that-change-the-equation) that provided the relational semantics and transaction model we needed, making PostgreSQL a natural fit.


The read path required more thought. The read-serving tier boots from static snapshots and serves queries from memory, so we needed an embeddable database. SQLite was the first candidate, but our schema relies on array columns, which SQLite does not natively support.[DuckDB](https://duckdb.org/library/duckdb-the-sqlite-for-analytics/) solved both problems: It handles arrays natively and has a SQL dialect that is[closely compatible with PostgreSQL](https://duckdb.org/docs/lts/sql/dialect/postgresql_compatibility) . This enabled us to share query logic across both engines rather than maintaining two separate implementations.


This was the human-driven part of the project. Everything that followed—the method-by-method refactoring—was where AI entered the picture.


## What made the migration safe


Before we dive into how AI fit into this project, it’s worth stepping back to look at the bigger picture. The migration worked as well as it did because of three pieces that were already in place. These elements enabled us to move quickly without starting from scratch and gave us real trust in the generated code.


**First, modular code.** At Datadog we strive for modular software, and Stream Router is no exception. Its storage layer sat behind an internal interface we call the Controller. The existing implementation used FoundationDB; building the new one meant writing a second implementation of the same interface against PostgreSQL. The rest of the system didn’t need to change.


**Second, a thorough test suite.** Given the criticality of what we do, we invest heavily in testable code. Every Controller method was covered by end-to-end tests with clear expectations for what the storage state should look like after each operation. This suite became the binary success criterion for every AI-generated change.


**Third, parallel infrastructure.** Rémi Calixte, one of our tenured engineers, had built a blue/green deployment architecture—think A/B testing, but for infrastructure. Two fully independent instances of Stream Router run side by side, serving the same requests, and clients target one or the other based on feature flags. A dedicated validator service runs in every cluster, periodically comparing routing responses between the two and alerting the team immediately if anything diverges.


These three pieces—modular interfaces, comprehensive tests, and parallel infrastructure—are what made it safer to hand most of the implementation work to AI.


## Mapping the codebase, then implementing method by method


To make AI-assisted refactoring tractable, we needed a way to give the models a clear, human-readable understanding of what the existing system was doing and why.


### Phase 1: Building a map


Before any refactoring began, we used Claude to build a structural map of the old codebase. We fed Claude the existing KV controller—a large and deeply nested call stack tightly coupled to the storage layer—and asked it to produce markdown documents describing the intent of each key function. Not what the code did line by line, but **why** it existed and what behavior it protected.


This turned out to be one of the most valuable steps in the project. In later phases, when we fed these documents alongside the new schema and failing test output, the models had much better context about intent versus implementation, and the back-and-forth per iteration dropped noticeably.


We saw the same effect with code comments. Adding comments that explained **why** a piece of logic existed—not just what it did—consistently improved the quality of AI-generated implementations. For anyone attempting a similar migration, it’s worth noting that investing in documentation and comments before handing code to AI pays for itself quickly.


### Phase 2: Stub, test, iterate


The core refactoring workflow followed a repeatable loop:


1. Pick a method from the old KV controller.


2. Create a stub in the new PostgreSQL-backed controller that returns an error.


3. Run the end-to-end test suite.


4. Feed the failing test output to Claude or Cursor with context: the old implementation, the new schema, and the markdown documentation from Phase 1.


5. Iterate—sometimes with human input, sometimes not—until the tests pass.


A typical prompt included the failing test output, a description of the expected behavior, and the relevant context from Phase 1. The model would produce a skeleton implementation, which we reviewed and adjusted before running tests again and feeding back any remaining failures.


This worked because of the end-to-end test suite described above. Every test that passed on the KV backend had to pass identically on the relational backend. “Translate this method so these tests pass” is a task AI handles well, and we think the pattern generalizes: Any codebase with a strong test suite can turn a migration into a convergence problem where AI does the iteration and tests do the judging.


What did not work was prompting at a higher level. When we gave the models broader tasks with more context, the results were consistently worse: context overload, hallucinated interfaces, code that compiled but didn’t match the expected behavior. Keeping each prompt scoped to a single method or a specific failing test produced far better results.


We started sessions frequently as the context window filled up. This is where documenting the purpose of existing code shines: Even with a fresh session, the models could pick up where we left off because the intent was captured outside the conversation.


### Phase 3: Blue/green validation in production


The end-to-end test suite gave us confidence in correctness against test fixtures. But Stream Router routes real production traffic, so we needed confidence against real production data. That’s where the blue/green infrastructure came in.


We deployed the PostgreSQL-backed system as “blue” alongside the existing FoundationDB-backed “green.” A validator service compared their responses continuously, every 30 seconds.


Test suites verify behavior against fixtures. The validator service verified behavior against live production data for weeks before we cut over. For a system this critical, that distinction matters.


## Where AI fell short


We want to be honest about the limits.


Claude and Cursor did much of the heavy lifting on repetitive work: extracting validation logic, translating method after method from one data model to another, and wiring up the new schema. But when it came to SQL performance, they consistently produced correct queries that were not optimal.


Niche optimizations such as batching,` UNNEST` tricks, and common table expressions required human input. The AI-generated queries returned the right results but issued far more round trips than necessary. We wrote the optimized versions ourselves, and once the models had seen the pattern, they could replicate it in subsequent methods. But they did not discover these patterns independently, even though they exist in the literature.


The takeaway is straightforward: **Stay critical of generated code** . AI does not automatically recognize all optimization opportunities. Nudging it in the right direction, showing it a well-optimized query, and letting it generalize produced far better results than expecting it to find the optimal path on its own. You can even run` EXPLAIN ANALYZE` and feed the planner output to the model to spot potentially missing performance optimization opportunities.


Token consumption was also significant, especially early on. We were feeding full test output dumps rather than trimmed excerpts, and the iterative loop of test output, code context, and schema information burned through tokens quickly. We became more disciplined about this over time, but our test suite is verbose by nature and was—at the time—not optimized for AI-assisted workflows.


## What changed after the migration


The project went from initial design to production in roughly 3 months (December 2025 through February 2026), with the core proof of concept reached in 4 weeks.


The schema redesign expanded what was possible operationally. Validation that previously required thousands of sequential reads could now be expressed as a single SQL query with joins. Large operations that exceeded transaction size limits on the KV store now complete in one transaction on PostgreSQL with no inherent size ceiling. Business logic that was tightly coupled to the KV abstraction is now decoupled from the storage layer entirely: The controller issues SQL queries, and the storage engine is an implementation detail.


The results were significant:


- **Operations that were estimated to run in 45 minutes now complete in ~1 second** (nearly 3,000x). API calls that previously timed out now complete in about a second.


- **The routing dataset shrank 40x in PostgreSQL** , with DuckDB static snapshots even smaller. The KV model required maintaining relationships in application code; PostgreSQL and DuckDB handle that natively.


- **Overall latencies decreased by one or more orders of magnitude** , from hundreds of milliseconds to a few. PostgreSQL generates efficient query plans and pushes predicates down to the server via` WHERE` clauses rather than reconstructing relationships in application code only to filter them.


- **CPU and memory consumption decreased across pods** , both on the read and write paths.


- **Database costs decreased by 90%** after retiring the managed FoundationDB clusters.


- **Deployment to production proceeded without incident.**


## What we learned from migrating a live system with AI


Stream Router’s migration is a case study in what it takes to deliberately evolve a critical production system. The architecture was human-designed: a relational schema that fits the domain, PostgreSQL and DuckDB chosen for concrete technical reasons, and a blue/green validation strategy that ran for weeks against live data. AI accelerated the implementation: the method-by-method grunt work of translating well-specified behavior from one data model to another. It was not in charge of making design decisions but could be critical when prompted to spot discrepancies. Most importantly, when fed narrow prompts and a set of failing tests, it excelled.


For teams considering AI-assisted refactoring on high-stakes systems, the lesson we offer is this: **The quality of your test suite is the ceiling for how much you can trust AI-generated code.** Ours made this possible.


The system is faster, cheaper to run, and structurally easier to extend as the metrics pipeline continues to grow. We are already building on top of the new relational foundation, and the ability to iterate on Stream Router’s behavior with confidence is perhaps the most valuable outcome.


If you’re interested in building and evolving large-scale distributed systems, working on data infrastructure, or tackling problems like safe migrations in production,[we’re hiring](https://careers.datadoghq.com/all-jobs/?parent_department_Engineering%5B0%5D=Engineering&utm_source=engblog&utm_medium=corpsite&utm_campaign=engcommunity-2025-ai-kafka&gh_src=hm4uekgj1us) .


-
-
-
