---
schema_version: "1.0.0"
document_id: "7cbe806e78ee9e4f47a995972391cff8769a4975e115d976dd0b955cc097b0b1"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-32a3c94cba04"
canonical_url: "https://building.nubank.com/how-we-reduced-critical-path-latency-by-76-at-nubank/"
published_at: "2026-01-12T10:00:00+00:00"
first_seen_at: "2026-07-20T04:35:52.604812+00:00"
fetched_at: "2026-07-28T20:54:51.009577+00:00"
content_hash: "sha256:6840aab8da7031117dfae04c78fd3caeb1d31cbf0dea37a235b1e8eeb7a2da15"
---

# How we reduced critical path latency by 76% at Nubank

*Author:[Jonatan Michael](https://www.linkedin.com/in/jonatanmichael/)*


At Nubank, latency is not just a performance metric. It directly affects customer experience and business outcomes. When building financial products at scale, consistent sub-second responses influence conversion rates, operational stability, and how confidently we integrate with external partners.


In one of our core payment systems, a specific integration required responses within 360 milliseconds at the 90th percentile (P90). During initial benchmarking, the P90 latency of our Payment Conditions API was approximately 1200ms, with P99 often exceeding 1600ms. Reducing this gap required more than tuning infrastructure or optimizing isolated components, as it required understanding how architectural choices translated into runtime behavior.


Our approach was intentional and staged. We began by addressing local inefficiencies in the existing design (Phase 1). Once the limits of those improvements became clear, we moved toward a structural change that decoupled the read path from synchronous downstream calls (Phase 2). Each phase was driven by a clear problem statement followed by a targeted engineering response, balancing correctness, performance, and operational safety.


## Phase 1: Optimizing the existing architecture


In large distributed systems, there are inefficiencies that accumulate over time and add latency without delivering proportional business value. In this phase, our objective was to remove those costs and isolate which latency contributors were inherent to the architecture itself.


**Problem 1: Deep call chains across domains**


A single request to compute payment conditions synchronously traversed a long chain of services:


*The synchronous call chain problem*


###### Image created by AI (Gemini)


Each hop added network overhead, serialization cost, and exposure to tail latency. While no individual service was particularly slow, the cumulative effect of strict synchronous composition across domains dominated end-to-end response time.


**Fix 1: Context propagation**


Tracing revealed that multiple services independently fetched and validated the same data, such as credit limits and account metadata. Instead of allowing each service to recompute this information, we refactored the orchestration layer to propagate validated context forward.


This change reduced redundant calls and lowered downstream load. It also made data ownership more explicit, improving both runtime efficiency and system clarity.


**Problem 2: Sequential dependencies without domain semantics**


Some services in the call graph existed primarily as routing layers. They forwarded requests downstream without contributing meaningful domain logic, yet still imposed latency costs.


**Fix 2: Collapsing non-semantical layers**


Where domain boundaries allowed, we reorganized the flow so the orchestrator could invoke destination services directly. Removing intermediary layers reduced call depth and simplified reasoning about the critical path.


##### *Removing intermediate dependencies*


###### Image created by AI (Gemini)


**Limits of phase 1**


These optimizations were effective and low risk, but they exposed a structural limit. Even after removing redundancies and collapsing layers, the request still depended on synchronous calls across many independent services.


Operational characteristics amplified this issue. Many services run on the JVM and are deployed on AWS Spot Instances. Instance recycling, deployments, and JVM warm-up effects directly influenced tail latency. As long as the read path depended on these services, latency variability was unavoidable.


At this point, further improvements required a change in architectural strategy rather than incremental optimization.


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## Phase 2: Decoupling read-time composition from back-end dependencies


Despite the gains from Phase 1, payment conditions were still composed synchronously at read time. This meant that customer-facing latency was bounded by the slowest downstream dependency chain in the system.


To meet strict SLOs consistently, we reframed the problem. Instead of assembling complex domain views on every request, we asked whether those views could be continuously precomputed and made available for fast reads using caching strategy.


**Problem 3: Passive caching does not eliminate tail latency**


Adding a passive cache in front of the existing flow would not fundamentally solve the issue. In high-latency distributed systems, cache misses still invoke the original synchronous path. Under load, misses become indistinguishable from timeouts and amplify tail latency.


What we needed was a model where cache hits were the default behavior and cache misses were exceptional.


**Fix 3: Active and event-driven persisted cache**


We formalized a decision framework to choose between passive and active caching. The criteria included data complexity, update frequency, read volume, and consistency requirements.


Payment conditions involve non-trivial business logic, are read at very high volume, and change frequently through observable domain events. Eventual consistency was acceptable within defined bounds, but unpredictable latency was not. These characteristics made an active caching strategy the right choice.


**Architectural design: Write-time aggregation**


In the new architecture, domain services emit events whenever relevant state changes occur, such as updates to credit limits or account balances. A dedicated aggregation service consumes these events, retrieves any required canonical data, applies aggregation logic, and persists a fully materialized representation into a low-latency datastore.


This persisted representation becomes the primary read surface for the Payments Conditions API. Most requests are served through a single millisecond-scale lookup, completely removing downstream services from the critical path.


By shifting complexity to the write path, we insulated customer-facing latency from infrastructure churn, JVM warm-up effects, and unrelated deployments in other domains.


**Safety, correctness, and fallbacks**


In a financial platform, performance improvements cannot compromise correctness. The persisted cache is therefore not treated as the sole source of truth. Clients implement a mandatory fallback path. If the cache is unavailable or fails to meet freshness guarantees, requests transparently fall back to the original synchronous flow.


Although slower, this fallback preserves correctness and ensures that temporary issues in the caching layer do not block customer actions.


**Observability in an asynchronous architecture**


With complexity moved to the write side, operational focus shifted accordingly. We monitor event consumption lag, cache hit and miss ratios, success rate and percentile latencies for persisted reads. These metrics allow us to detect degradation early and maintain confidence in both correctness and performance.


## Results and impact


After deploying the new architecture, P90 latency dropped from approximately 1200ms to 280ms, representing a 76% reduction. Tail latency stabilized significantly, and success rates reached five nines, comfortably meeting partner SLAs.


More importantly, the system became predictable. Latency was no longer coupled to the slowest downstream chain dependency or to infrastructure-level variability. This predictability reduced operational risk and enabled new integrations that were previously infeasible.


This effort reinforced an important lesson at Nubank. The path for performance improvements often starts from small changes (like context propagation and collapsing non-semantical layers). The next step requires structural changes (like Event-Driven Persisted Cache architecture). By moving complexity to the write path, we accepted the cost of asynchronous aggregation, state management, and monitoring in exchange for fast and reliable reads.


The phased approach was important. By exhausting tactical optimizations before introducing structural changes, we avoided unnecessary architectural complexity and gained confidence that each step addressed a real constraint.


Engineering at scale is not about maximizing optimization. It is about choosing the right abstractions to meet customer needs reliably, sustainably, and at the right level of complexity.
