---
schema_version: "1.0.0"
document_id: "2399cd63cdd594f19cbf372e0d37aa57af06339e75c94c53eda8845ede14f287"
company_key: "yc-ridecell"
company: "Ridecell"
source_id: "yc-ridecell-news-import-1074b151deb4"
canonical_url: "https://ridecell.com/blog/from-microservices-to-modular-monolith/"
published_at: null
first_seen_at: "2026-07-25T21:51:40.131543+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:fdc1554fadf2601e0bc173414da0a3e472990421b7fecab43c66180d763cf7bc"
---

# From Microservices to Modular Monolith: How We Cut Algorithm Runtime by 12× and Eliminated Peak-Hour Downtime

Real-time dispatch platforms have zero tolerance for lag. When a field worker comes online expecting a job assignment, every minute of delay is a minute of paid idle time, and at scale, that adds up fast.


That was the reality our engineering team was living with. Our field workforce dispatch platform matches workers to jobs in real time based on location, availability, ETA, and scheduling rules. At peak operation, large numbers of field workers depend on it to receive job assignments instantly.


What follows is the unvarnished account of how we diagnosed a set of compounding architectural problems, made the decision to consolidate, and came out the other side with a system that is faster, more reliable, and simpler to operate.


## Scale and context


Architecture discussions are most useful when grounded in operational scale. Although the specific numbers vary by customer deployment, the platform supports large field operations where thousands of workers depend on near real-time job assignment and lifecycle management. The architectural bottlenecks described here emerged under sustained production workloads and were observed repeatedly during peak operating periods.


The key lesson is not that the original architecture failed at small scale, but that its operational complexity grew faster than its business value. The consolidation effort focused on reducing that complexity while preserving the flexibility required for future growth.


## The original architecture


The platform was originally built as three independent microservices:


- **Job Management:** job lifecycle, status tracking, and worker assignment records.
- **Dispatch:** the matching algorithm, assigning workers to jobs based on ETA, scheduling windows, and business rules.
- **Rule Engine:** job creation based on alerts and transactions.


Each service had its own database and its own deployment pipeline, and communicated with the others through Kafka. A fourth legacy monolith also remained in the system.


The architecture reflected sound distributed systems thinking: independent deployability, isolated failure domains, and the ability to scale each service independently. In practice, it created problems that compounded until consolidation became unavoidable.


Figure 1: Original architecture, three microservices, four databases, Kafka sync, and a legacy monolith.


## What was going wrong


### Kafka as the backbone of a synchronous system


The three services were not loosely coupled, they were co-dependent. The dispatch service needed job data from job management to make routing decisions. Job management needed data from both to maintain lifecycle state. The dependencies were real, tight, and bidirectional.


Routing these dependencies through Kafka gave us the operational overhead of asynchronous messaging without the resilience benefits, because the downstream operations consuming those messages were themselves blocking. During peak hours, Kafka broker congestion cascaded across all three services simultaneously. Message backlogs built up, consumers fell behind, and the system became unresponsive precisely when field workers needed it most. This was not a one-time event. It was a recurring failure mode baked into the architecture.


### A 60-minute matching algorithm


The dispatch service ran the worker-to-job matching algorithm in batch cycles. The worst-case runtime was 60 minutes.


In a field workforce platform, this created a concrete business problem: a worker who came online immediately after an algorithm cycle completed could wait up to an hour before receiving a job assignment. That worker was on the clock, paid and available, with nothing to do. Not because there was no work, but because the system had not caught up yet. The root cause was structural: the algorithm ran as a separate service reading from replicated, Kafka-synced data that lagged behind the source of truth. Every cycle started from stale state.


### No single source of truth


Both job management and dispatch maintained their own job tables, kept in sync via Kafka consumers. When the sync worked, it was expensive: peak DB operations reached approximately 400,000 transactions per minute, driven largely by this synchronization overhead. When the sync lagged, the two services had different views of the same job, and dispatch decisions were made on stale data. There was no canonical source of truth, only an eventual approximation, with the inconsistencies that entails.


### DB row locks that froze the app


The dispatch algorithm assigned workers to jobs in a loop, saving each job record individually rather than in bulk. PostgreSQL’s row-level exclusive locks meant that while one write was in flight, every other write targeting the same row had to wait in queue. Under normal load, this was tolerable. At scale, the queue of waiting writes grew faster than the lock holder could release them.


The result was a cascading hold. Field workers trying to update job status in the mobile app, dispatchers triggering new assignments, and the algorithm itself were all contending for the same rows. From a worker’s perspective, the app appeared frozen: technically operational but practically unusable for periods of up to 30 minutes.


This was not a bug. It was an emergent property of the architecture: a per-row write pattern that was safe in isolation became a systemic lock contention problem when the algorithm processed hundreds of records in sequence across a service boundary.


### SDK coupling that defeated the point of microservices


Microservices are intended to reduce coupling. In our platform, the three services communicated through internal Python SDK packages: each service published a library that the others imported. This recreated tight coupling in a form that was harder to manage than co-located code. Upgrades required coordinated releases across multiple repositories, CI/CD pipelines, and deployment sequences.


The distributed architecture had not reduced coupling. It had moved coupling into the least manageable place possible.


### 30-minute deployment cycles


Any feature touching job management and dispatch, which was most features, required deploying multiple services in sequence with careful ordering to avoid contract mismatches. End-to-end deployment time was approximately 30 minutes. For a team of two to five engineers, that is a significant fraction of every release cycle, multiplied across every sprint.


## Why consolidation was the right call


The decision to consolidate was not about rejecting distributed systems as a concept. It was about recognizing that the preconditions that justify microservices were not present.


**The services were not independent, they were co-dependent.** Dispatch and job management changed together on nearly every feature. The theoretical benefit of independent deployment was never realized, because in practice you could not safely deploy one without the other.


**The team size did not justify the overhead.** Microservices solve an organizational scaling problem: enabling large, independent teams to deploy without coordination. A team of two to five engineers does not have this problem. What they have is a system complex enough to require distributed systems expertise to operate, with no corresponding benefit in deployment autonomy.


**The data model was wrong from the start.** Two services owning the same data is not a distribution strategy, it is a consistency problem waiting to manifest. The correct fix was a single schema with a single owner, not more sophisticated synchronization.


**The algorithm could not be fast while reading from stale replicated data.** No amount of tuning the rule engine would fix a 60-minute cycle when the input data had built-in lag. The speedup required co-location with the source of truth.


A note on context: this is not an argument against microservices. Large-scale dispatch platforms with dozens of independent teams, each owning a distinct business domain, benefit enormously from the model, because the organizational boundaries justify the operational cost. Our situation was different: a small team, co-dependent services, and shared data that had no business being split. The lesson is not that microservices are bad. The lesson is that architecture must match the actual constraints of your team and product, not the anticipated ones.


## The new architecture


The consolidated service merges job management and dispatch into a single Django service with a unified PostgreSQL schema. The rule engine was absorbed into a related legacy monolith. The result is a modular monolith: one deployable unit with explicit, enforced module boundaries.


Figure 2: Consolidated architecture, one service, one database, in-process communication, and measured outcomes.


### Key architectural decisions


- **Single source of truth.** One job schema, one owner, no synchronization required.
- **In-process communication.** Dispatch and the matching algorithm call job management directly, with no network hop, no broker, and no latency.
- **Unified internal library.** A shared common/ module replaces all legacy inter-service SDKs with a single versioned library. Cross-cutting concerns (authentication, notifications, external integrations, and configuration) are accessed through stable wrappers that can be upgraded independently of business logic.
- **Cache layer.** Redis caching on external service calls eliminates redundant network calls that previously added latency to every request.
- **Bulk writes.** The algorithm now processes jobs in bulk operations against the unified schema, eliminating the per-row lock contention that caused app freezes.
- **Live data migration.** Production data was migrated across service boundaries with schema changes using custom migration scripts with row-count and referential integrity validation, executed during a maintenance window with zero data loss.


## Results


Metric Before After Change


Deployments 4 1 4× reduction


Algorithm runtime 60 min (worst case) 5 min 12× faster


API latency ~600 ms ~200 ms 66% reduction


API throughput 60–90 req/s 300–400 req/s 3–5× increase


Error rate ~2% ~0% Near elimination


Peak DB operations ~400,000/min ~10,000/min 97% reduction


CPU / memory usage Baseline ~60–70% lower


Deployment time ~30 min ~7 min 77% faster


Peak-hour downtime Recurring Eliminated


Mobile crash-free sessions 100%


The algorithm improvement carried the largest direct business impact. The 12× speedup came from four compounding changes: the algorithm now runs in-process with direct database access (eliminating Kafka round-trips), it reads from a single authoritative source of truth with no sync lag, redundant synchronization queries were removed, and the matching and scheduling logic was redesigned as part of the consolidation.


The 97% reduction in peak DB operations came almost entirely from eliminating Kafka-driven synchronization between duplicate job tables. Fewer transactions meant fewer locks, and the elimination of the recurring app freeze incidents that had become a defined operational risk for field workers.


## Business impact


While the technical improvements were significant, the most important outcome was their effect on workforce operations. Reducing the dispatch cycle from 60 minutes to 5 minutes meant newly available workers could be assigned work within a single planning cycle instead of potentially waiting up to an hour. For field operations teams, that translates directly into higher workforce utilization, reduced paid idle time, faster response to customer requests, and more predictable service-level performance.


The elimination of peak-hour outages also improved operational confidence. Dispatchers no longer had to compensate for system lag with manual interventions, while field workers experienced a more responsive mobile application with fewer assignment delays. The result was not simply a faster system, it was a platform that enabled teams to operate more efficiently at scale.


## How the migration happened


The migration was executed by two to three engineers over approximately three to six months, culminating in a hard cutover during a planned maintenance window.


The most significant technical challenge was the live data migration: moving production data across service boundaries while applying schema changes, with zero tolerance for data loss. Custom migration scripts handled the transfer with pre- and post-migration validation (row counts, referential integrity checks, and spot-sampling of critical records), completing in approximately 7 minutes despite the underlying schema transformations.


Running the old and new architectures in parallel was deliberately avoided. A shadow mode would have required maintaining the Kafka synchronization infrastructure that was being removed, reintroducing the complexity we were eliminating. A clean cutover with a validated migration was the lower-risk path.


### Migration risk management


The migration plan emphasized reversibility, validation, and operational visibility. Before cutover, migration scripts were repeatedly exercised against production-like datasets to validate schema transformations, record counts, and referential integrity. Comprehensive monitoring dashboards tracked database performance, API latency, error rates, and background job execution throughout the migration window.


Rollback procedures were documented and rehearsed before execution. Although the migration completed successfully without data loss, treating rollback as a first-class design requirement significantly reduced operational risk and increased confidence in the cutover plan.


## Lessons that transfer


**Microservices solve organizational scale, not technical scale.** The canonical justification is Conway’s Law: large organizations build systems that mirror their communication structures. For a team of two to five engineers, there are no communication structures to encode. The overhead exists without the benefit.


**Data duplication is an architectural smell, not a distribution strategy.** If two services need the same data, one of them should own it, or they likely belong together. Duplicating and synchronizing is a complexity multiplier: it compounds in DB load, consistency bugs, lock contention, and debugging difficulty over time.


**Kafka is not a substitute for good data architecture.** We used Kafka to paper over the fact that two services needed shared data. The broker was doing work that a shared schema would have made unnecessary. Kafka is powerful for genuinely event-driven workloads; it is expensive overhead when used as a sync layer for relational data.


**SDK coupling across services is harder to manage than coupling within a service.** Shared code in a monolith can be refactored atomically, tested in one suite, and changed with full visibility into its consumers. Shared SDKs across services cannot. When inter-service contracts live in libraries, upgrades become coordinated deployments across repositories and pipelines, coupling in a form that actively resists change.


**Simplicity has compounding returns.** Every hour recovered from operational toil, deployment coordination, and distributed debugging compounds across every sprint. A simpler system is faster to change, easier to onboard, and more reliable by default. The return on consolidation is not a one-time improvement, it is a permanent change in what the team can ship.


## What we would do differently today


If we were designing the system today, we would begin with a modular monolith rather than a collection of tightly coupled microservices. A modular monolith provides clear domain boundaries, shared access to authoritative data, and significantly lower operational overhead while preserving the option to extract services later when organizational or scaling requirements justify it.


Our experience reinforced an important principle: architecture should evolve in response to demonstrated constraints rather than anticipated ones. Microservices remain a powerful pattern when independent teams own independent business domains. In our case, the services were highly co-dependent, shared data extensively, and changed together on most features. A modular monolith aligned more closely with those realities.


**Start with a modular monolith.** Enforce module boundaries from day one. Extract services only when you have a demonstrated organizational or scaling need, not in anticipation of one.
