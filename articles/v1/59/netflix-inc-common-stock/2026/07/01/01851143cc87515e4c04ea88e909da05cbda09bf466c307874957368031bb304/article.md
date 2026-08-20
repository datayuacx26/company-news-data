---
schema_version: "1.0.0"
document_id: "01851143cc87515e4c04ea88e909da05cbda09bf466c307874957368031bb304"
company_key: "netflix-inc-common-stock"
company: "Netflix Inc."
source_id: "netflix-inc-common-stock-rss-c4c725f6f796"
canonical_url: "https://netflixtechblog.com/building-service-topology-at-scale-architecture-challenges-and-lessons-learned-f4b792f3f0d8"
published_at: "2026-07-13T22:44:11+00:00"
first_seen_at: "2026-07-20T03:30:10.154272+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:988e7e73ba41cbef3920ec8c7057f827f0076fa21fe32ccafb109b1a17bdbf17"
---

# Building Service Topology at Scale: Architecture, Challenges, and Lessons Learned

Featured


# Building Service Topology at Scale: Architecture, Challenges, and Lessons Learned


[Netflix Technology Blog](https://netflixtechblog.medium.com/?source=post_page---byline--f4b792f3f0d8---------------------------------------)


20 min read


·


Jul 13, 2026


--


*By*[Parth Jain](https://www.linkedin.com/in/parth-jain-8a09abb6/) ,[Rakesh Sukumar](https://www.linkedin.com/in/raskuma/) *,*[Yingwu Zhao](https://www.linkedin.com/in/yingwu-zhao-62037418/) *,*[Renzo Sanchez-Silva](https://www.linkedin.com/in/renzosanchezsilva/) *&*[Nathan Fisher](https://www.linkedin.com/in/nathfisher/) *A deep dive into the engineering challenges of building a real-time service dependency map at Netflix scale: from streaming architectures and distributed aggregation pipelines to time-travel queries and the methodology that made it work.*


## Introduction


In our[first post](https://netflixtechblog.com/from-silos-to-service-topology-why-netflix-built-a-real-time-service-map-0165ba13a7bc) , we introduced the problem: engineers at Netflix needed a unified, real-time view of service dependencies to troubleshoot faster, understand blast radius, and navigate our distributed architecture. We described our multi-source approach, combining eBPF network flows, IPC metrics, and distributed tracing into physically separate graph layers that can be queried independently or merged into a comprehensive view.


That post explained *what* we built and *why* . This post is about *how* , the engineering reality of building this system at Netflix scale.


Here’s the truth: the first version worked perfectly… in our local environment. Production was a different story. Kafka consumers fell behind. Instances ran out of memory. Some nodes received 100x the traffic of others. Garbage collection pauses consumed more CPU than actual business logic.


What you’ll learn in this post isn’t a success story, it’s a learning journey. We’ll walk through the architecture decisions that enabled scale, the production challenges that tested those decisions, the optimization methodology that guided us through, and the lessons that apply to any distributed system. Along the way, we’ll share the innovations that made it possible to process millions of flow records per second, reconstruct topology at any point in time, and provide sub-second query responses, all while maintaining near real-time freshness.


## Architecture Deep-Dive: Building for Streaming and Scale


### Streaming-First: Why Real-Time Matters


Traditional service topology systems use batch processing, aggregating data hourly or daily, then storing complete snapshots. This approach works at a modest scale but has a fundamental problem: by the time you see the data, it’s already old. During a production incident at 3am, an hour-old dependency map is archaeology, not observability.


Our key architectural decision was to build streaming-first. Instead of batch jobs that process historical data, we continuously ingest flow records from multi-region Kafka streams and IPC metrics as Server-Sent Events, process them through reactive pipelines with backpressure handling, and provide near real-time topology updates, typically within tens of minutes, compared to the hours-old or day-old data that batch processing approaches provide.


This wasn’t just about freshness, it was essential for our use cases. Live events can’t wait for the next hourly batch. Incident response needs current data. Change validation requires seeing immediate impact. The architecture had to support continuous updates while handling massive scale without falling behind.


**How Backpressure Enables Real-Time Processing** The streaming approach created new challenges, but also required solving a fundamental problem: how do you process millions of flow records per second in real-time without losing data when downstream systems slow down?


Traditional approaches fall short at our scale:


- **Unbounded queues** : Simple but dangerous. Keep buffering until you run out of memory, then the instance crashes.
- **Drop-based flow control** : Discard data when buffers fill. Fast, but now your topology is incomplete, you’ve lost connection information.
- **Batch processing** : Process everything, but hours later. By then, the incident is over (or worse, still happening with stale data).


We needed something different: the ability to slow down gracefully under load without losing data. This is where reactive streams with backpressure became essential.


Here’s how it works: when Stage 3 can’t write to the graph database fast enough, it signals Stage 2 to slow down. Stage 2 signals Stage 1. Stage 1 signals the Kafka consumer to pause. The data waits in Kafka until downstream capacity returns.


Press enter or click to view image in full size


When a downstream stage can’t keep up, it signals upstream to slow down — backpressure flows in the opposite direction of the data


Backpressure propagates naturally through the entire system. When any stage becomes overwhelmed from traffic spikes, GC pauses, or external slowdowns, the pipeline automatically slows to a sustainable rate. No data is lost in most cases, no instances crash, the system degrades gracefully.


This is what enables “real-time” at our scale. During normal operation, we process with minimal latency. During load spikes or temporary slowdowns, we slow down rather than fall over. The data still gets processed, just a few seconds or minutes later instead of immediately. For topology updates, this trade-off is acceptable: slightly delayed real-time updates are vastly better than hour-old batch data or incomplete topology from dropped records.


The cost of this approach is complexity. Reactive streams are harder to reason about compared to traditional synchronous blocking models (we’ll discuss this more in the challenges section). But at Netflix scale, backpressure isn’t optional, it’s the mechanism that keeps the system running reliably under production load.


### Multi-Layer Architecture: Physical Separation for Independent Optimization


As we covered in our[first post](https://netflixtechblog.com/from-silos-to-service-topology-why-netflix-built-a-real-time-service-map-0165ba13a7bc) , our multi-source approach uses three physically separate topology layers with different storage optimized for each:


- **Network Layer** : eBPF flow logs in graph database partition, comprehensive coverage but lacks application context
- **IPC Layer** : Application metrics in a different graph database isolated from the one for Network Layer, rich endpoint details but only instrumented services
- **Tracing Layer** : Distributed traces in columnar storage (Parquet), actual request paths but sampled.( *We cover the tracing layer and its integration in our next post* ***)*** .


Press enter or click to view image in full size


Flow logs and IPC metrics travel through two independently-optimized pipelines into separate graph stores, unified behind a single API


Physical storage isolation enables independent optimization, each layer has different throughput, query patterns, and evolution timelines. At query time, we execute parallel queries across relevant storage systems and merge results, providing unified views with sub-second latency while maintaining flexibility to evolve each layer independently.


### The Three-Stage Distributed Aggregation Pipeline


The heart of the network layer ingestion is a three-stage distributed pipeline. This architecture solves a fundamental challenge with network flow logs: **they only show individual network hops, not the true application-level connections we need to build a useful topology** .


**The Core Problem: Network Intermediaries**


In cloud environments, traffic between applications rarely flows directly, it traverses intermediate network components like load balancers, NAT gateways, API gateways, and proxies. Network flow logs show individual hops: App A → Load Balancer and Load Balancer → App B appear as separate flows. But what engineers need is the logical dependency: App A → App B. Without resolving these intermediaries, our topology would be cluttered with infrastructure components rather than showing the service-to-service relationships that matter for troubleshooting.


The three-stage pipeline solves this:


Press enter or click to view image in full size


The flow log pipeline in detail — three stages connected by SSE, with enrichment applied just before the final graph write


**Stage 1: Initial Aggregation (FlowLog Ingestion Service)**


```text
Multi-Region Kafka (4 regions)    → Filter invalid flow logs    → 5-minute time-window batching    → Create initial aggregators per window    → Distribute via consistent hashing    → Stream to Stage 2 via SSE
```


Stage 1 consumes flow logs from multi-region Kafka, filters invalid records, batches them into 5-minute time windows, and creates initial aggregator objects. At this stage, we’re still working with raw network hops, identifying which flows involve intermediaries but not yet resolving them. Aggregators stream to Stage 2 for resolution.


**Stage 2: Network Intermediary Resolution Layer (Intermediate GraphEntity Ingestion Service)**


```text
Stage 1 Aggregators (via SSE streams)    → Group flows by intermediary (load balancer, NAT gateway, proxy, etc.)    → Identify pairs: (Source → Intermediary) + (Intermediary → Destination)    → Resolve to direct edges: Source → Destination    → Track which intermediaries were traversed    → Aggregate metrics across both hops    → Re-distribute via consistent hashing    → Stream to Stage 3 via SSE
```


This is the key step **.** Stage 2 performs graph resolution:


1. **Collect flows by intermediary** : Group aggregators where an intermediary is either source or destination, creating maps of flows going TO intermediaries (Source → Intermediary) and FROM intermediaries (Intermediary → Destination)
2. **Resolve direct edges** : For each intermediary, join its incoming and outgoing flows to create direct application edges (App A → App B), combining metrics from both hops
3. **Result** : Clean application-level topology showing App A → App B instead of App A → Load Balancer → App B


This resolution happens at aggregation time, not query time, with resolved edges flowing to Stage 3.


**Why can’t we do this in a single stage?** The fundamental issue is **data locality** . To resolve App A → Load Balancer → App B into App A → App B, we need both flows on the same instance to perform the join. But in Stage 1, flows are scattered across instances based on Kafka’s partitioning. Stage 2’s critical function is to redistribute aggregators by intermediary identifier, all flows involving “Load Balancer X” route to the same instance for resolution. This is the classic map-reduce pattern: Stage 1 maps, Stage 2 shuffles and reduces by intermediary, Stage 3 performs final aggregation.


Press enter or click to view image in full size


A concrete example of why a single stage isn’t enough — Stage 1 scatters flows by partition, Stage 2 reshuffles by intermediary to resolve direct edges, and Stage 3 persists the final result.


**Stage 3: Final Aggregation and Enrichment (GraphEntity Ingestion Service)**


```text
Stage 2 Aggregators (via SSE streams)Flow    → Final aggregation across time windows    → Enrich with external data (query key-value stores)    → Convert to graph entities    → Persist to graph database (throttled writes)
```


Stage 3 performs final aggregation of resolved edges, enriches graph nodes with external data sources (application health, ownership, metadata), converts aggregators to concrete graph entities (nodes and edges with all properties populated), and persists them to the distributed graph database with controlled throttling to respect storage system limits.


**Why Three Stages, Not Two?**


We initially used two stages: aggregate in Stage 1, resolve and persist in Stage 2. This worked in testing but failed at production scale, Stage 2 became overwhelmed by data concentration.


The problem: intermediary resolution requires collecting ALL flows involving an intermediary on the same instance. As a result, the instances handling flow logs for popular applications and their intermediaries became ‘hot nodes’ due to significant data concentration. Compounding this, data enrichment (querying external stores for health and metadata) meant the busiest instances were also doing the most I/O.


The solution: split responsibilities into three stages. Stage 2 focuses purely on resolution and redistributes. Stage 3 handles enrichment and persistence. Rather than routing all flows for a hot key to one owner, we redistribute in stages. Each flow is distributed, resolved, distributed again, and then persisted, which spreads the work across multiple instances and isolates compute-heavy resolution from I/O-heavy enrichment. Even when intermediaries see 100x typical traffic, no single instance becomes a bottleneck.


**Why Server-Sent Events Instead of gRPC or Message Queues?**


We initially used gRPC but it became a performance bottleneck, serialization overhead, connection pool management, and memory pressure for streaming responses consumed more CPU than business logic. Message queues added infrastructure complexity without benefit for our use case.


SSE proved ideal: lightweight HTTP-based protocol with minimal serialization, natural backpressure integration with reactive streams, and simpler connection model. The lesson: industry best practices like “use gRPC for service communication” don’t apply universally. For streaming large volumes of pre-aggregated data, lighter-weight alternatives may be more appropriate. Measure, don’t assume.


**Why IPC Doesn’t Need Three Stages**


Press enter or click to view image in full size


The IPC pipeline mirrors the same pattern as the flow log pipeline, but needs only a single stage.


The IPC layer uses single-stage aggregation because: (1) IPC metrics are already at application level, no intermediaries to resolve, and (2) data is partitioned correctly from the start — each node receives all IPC metrics for its assigned applications via consistent hashing, eliminating the need for redistribution. This highlights a key principle: **data partitioning strategy determines processing architecture** . When data arrives with the right partitioning, you can aggregate directly; when it doesn’t (like network flows requiring intermediary resolution), you need shuffle/redistribution stages.


### Dynamic Load Distribution: How Hashing Works with Auto-Scaling


How do we decide which instance receives which aggregator when our Auto Scaling Groups dynamically add or remove instances? Traditional approaches assume static clusters requiring explicit rebalancing, coordination services, or manual data movement when cluster size changes.


**Our Approach: Dynamic Consistent Hashing**


We use consistent hashing with dynamic instance discovery from our service registry. Each instance queries the registry to get the current list of healthy ASG instances, maintains them in sorted order (ensuring all instances have the same view), and uses this list for the hash function` findOwnerInstance(aggregator.primaryKey)` . When ASG scales up or down, the hash function naturally redistributes aggregators based on the updated instance list, no explicit coordination needed.


The key insight: leverage existing infrastructure. Our service registry already tracks ASG membership for health checking. Using it as our source of truth gives us dynamic cluster membership for free. Consistent hashing provides stable partitioning (most aggregators stay on the same instance during membership changes), while the sorted list ensures consistency.


**The Result**


Load follows infrastructure automatically. During traffic spikes or live events, new instances immediately receive their share. During deployments, aggregators seamlessly shift to healthy instances. This pattern proved crucial for production stability, no manual intervention, no coordination protocol, just automatic rebalancing.


## The V1 Journey: Major Challenges at Production Scale


Getting the initial version (V1) to production taught us that scale changes everything. What works in development breaks in production. Every assumption gets tested. And fixing one bottleneck reveals the next.


### Challenge 1: Kafka Consumer Lag


**The Problem** : Our multi-region Kafka consumers started falling behind. Consumer lag grew from seconds to minutes, then hours. Flow logs were arriving faster than we could process them. If this continued, we’d never catch up, and our “real-time” topology would become increasingly stale.


**Investigation** : We instrumented Kafka consumer metrics heavily. Key findings:


- Kafka had fewer partitions than optimal for our consumer group size
- Each fetch operation retrieved relatively few records
- Network socket buffers weren’t right-sized for our throughput
- Cross-region read latency added overhead


**Solutions Applied** :


1. **Increased Kafka partitions** : More partitions enabled more parallel consumers in our consumer group, distributing load across more instances.
2. **Tuned fetch parameters** : Increased records per fetch operation, reducing the number of network round-trips. This trades off per-message latency (we fetch larger batches) for throughput (more records processed per second).
3. **Increased socket receive buffer size** : Ensured network buffers never limited fetch operations. At our scale, default buffer sizes were too small.


**Results** : Throughput improved significantly, and lag reduced to acceptable levels, typically under a minute even during peak traffic.


**Lesson** : At scale, you can’t optimize in isolation. Fixing Kafka lag revealed the next bottleneck: our instances themselves couldn’t keep up with the higher ingest rate. The pipeline moved faster, which exposed downstream capacity problems.


### Challenge 2: Hot Nodes and Data Amplification


**The Problem** : This was the most severe production issue we faced. Some instances in our Auto Scaling Group were receiving 100x more traffic than others. Memory usage spiked. Garbage collection pauses became frequent and long. More CPU time was spent in GC than in business logic. Eventually, hot instances would go DOWN, triggering cascading failures as their load redistributed to other instances.


**Root Cause Investigation** :
Flow logs for popular services dominate traffic volume. A service like our authentication layer or recommendation API is called by hundreds of other services, generating orders of magnitude more flow records than typical services.


Our initial architecture used consistent hashing to determine which instance owned aggregation for each destination service. All flow logs for a given destination are routed to the same instance, the “owner” for that destination. This design seemed reasonable: group related data for efficient aggregation.


But popular destinations created hot nodes. One instance might own authentication services, another might own a rarely-used backend service. The load distribution was wildly uneven, some instances handled 100x the flow records of others.


Worse, data amplification occurred during redistribution. Consider a service called by 100 upstream services across 10 instances. All 10 instances receive flow logs for that destination (because they all have local clients calling it). When they route aggregators to the owner instance, that instance receives 10 separate aggregators it must merge. The data volume multiplied during shuffling.


When many instances route data for the same key to one owner, the volume multiplies right where it lands — the root cause of hot nodes.


We profiled extensively using async-profiler and heap dump analysis. The results were clear: hot instances spent most of their CPU on garbage collection, trying to manage the rapid allocation and deallocation of aggregator objects as flow logs poured in faster than they could be processed. Memory pressure led to GC thrashing, which consumed CPU, which slowed processing, which increased memory pressure, a vicious cycle.


**Solution: The Three-Stage Pipeline’s Dual Benefits** The three-stage pipeline we described earlier, designed primarily for proxy resolution, turned out to be exactly what we needed to solve the hot nodes problem as well. Here’s why:


**Stage 1** performs initial aggregation locally before any distribution. Instead of sending every flow log to a remote instance immediately, each instance performs online aggregation of raw flow logs into time-windowed aggregators (over 5-minute periods) directly in memory; this allows the raw flow to be discarded and garbage collected quickly, significantly reducing memory pressure, and ensures only the aggregation results are transferred across the network to downstream stages.


## Get Netflix Technology Blog’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**Stage 2** focuses on proxy resolution but also provides intermediate redistribution. Aggregators from Stage 1 distribute via consistent hashing to Stage 2 instances. Now we’re moving compressed aggregators, not individual flow logs. After resolution, Stage 2 redistributes resolved edges again to Stage 3, providing a second hashing operation that further spreads load.


**Stage 3** receives resolved aggregators that have been compressed twice and distributed twice. Even for extremely popular services, load has been spread across enough distribution points that no single instance becomes overwhelmed.


The key insight: architectural decisions driven by one requirement (proxy resolution) often solve other problems (load distribution) as beneficial side effects. The three-stage pipeline with graduated redistribution achieves both goals, it resolves proxies to show clean application-level topology AND prevents hot nodes by spreading load across multiple distribution points.


**Switching from gRPC to SSE** As described earlier, this challenge also revealed that gRPC wasn’t the right protocol for inter-stage communication at our scale. We replaced gRPC with Server-Sent Events, dramatically reducing resource consumption on both sender and receiver sides.


**Results** :


- CPU usage became evenly distributed across instances, no more hot nodes with 10x the load of others
- Network bandwidth usage dropped significantly due to better aggregation and lighter-weight protocol
- Memory pressure decreased as we reduced the object allocation rate
- The system scaled gracefully with Auto Scaling Group changes


**Lesson** : Technology choices must match your specific use case. gRPC is excellent for request-response RPC patterns. For streaming large volumes of aggregated data in a pipeline, lighter-weight alternatives can be more appropriate. Let measurements guide the decision, not industry hype or existing team expertise.


### Challenge 3: Memory and Garbage Collection


**The Problem** : Even after fixing hot nodes, we still saw high heap usage, frequent garbage collection pauses, and instances occasionally going DOWN. GC logs showed pauses consuming significant CPU time, in some cases, more than our business logic.


**Root Cause** : Multiple factors contributed: objects accumulating in heap while waiting for 5-minute aggregation windows to complete, unnecessary conversions between different object types as data flowed through stages, and immutability overhead, following Scala best practices, we used immutable data structures for aggregators, but every update created new objects, overwhelming the garbage collector at millions of records per second.


**Investigation** : Heap dumps and GC logs revealed flow log objects retained beyond their useful lifetime, unnecessary intermediate conversion objects, and constant creation/disposal of immutable aggregator versions. Minor GCs occurred every few seconds, major GCs took hundreds of milliseconds, the JVM spent more time on garbage collection than business logic.


**Solutions Applied** :


1. **Faster processing** : Process flow logs immediately, aggregate quickly, release references. Optimized Pekko stream stages to minimize object lifetime.
2. **Eliminate unnecessary conversions** : Route aggregators directly between stages instead of converting to intermediate types.
3. **Mutable structures on hotpath** : This was controversial, Scala best practices emphasize immutability. But at our scale, immutability created too many objects. We pragmatically chose mutable aggregators on the hotpath (immutability elsewhere), prioritizing performance over convention. Switching to mutable aggregators reduced heap allocation by over 50% and cut GC pause time significantly, though it required more careful code review.
4. **Tuned time windows** : Balanced data freshness against memory pressure.


**Results** :


- Heap usage decreased substantially
- GC pauses reduced to acceptable levels (tens of milliseconds instead of hundreds)
- CPU freed up for business logic instead of garbage collection
- Instance stability improved, no more instances going DOWN due to memory issues


**Lesson** : “Best practices” are starting points, not absolute rules. At unique scale, you may need to diverge from conventions. But do it deliberately, with measurement justifying the decision, and with awareness of the trade-offs. Don’t abandon immutability everywhere, just where performance data proves it’s necessary.


## Challenge 4: Reactive Streams Complexity


**The Problem** : Our Pekko Streams pipelines would stall unexpectedly. Backpressure propagation didn’t work as expected. We struggled to debug why certain streams would stop processing without obvious errors. The reactive programming mental model, with its emphasis on async boundaries, backpressure, and demand-driven processing, proved harder to master than anticipated.


**What We Learned** :
Reactive streams with backpressure are powerful tools for building systems that handle load spikes gracefully. When downstream consumers slow down (due to temporary load, GC pauses, or external system slowdowns), backpressure allows upstream producers to slow down rather than overflow buffers or drop data.


But this power comes with complexity:


- **Non-intuitive behavior** : Traditional imperative code flows top-to-bottom. Reactive streams are demand-driven, downstream consumers pull from upstream producers. This inversion of control isn’t intuitive.
- **Async boundaries** : The .async operator in Pekko Streams creates a boundary where processing moves to a different thread. This can improve parallelism but also introduces complexity around buffer sizing, demand signaling, and error propagation. We initially misunderstood when to use .async and ended up with over-parallelized streams that created more overhead than benefit.
- **Debugging difficulty** : When a stream stalls, there’s no stack trace pointing to the problem. You must understand the internal mechanics, demand signals, buffer states, materializer state to diagnose issues.


**Our Approach** :


1. **Deep learning investment** : We invested significant time in understanding reactive streams concepts deeply. Reading documentation, experimenting with small examples, and building team expertise.
2. **Simplified patterns** : Where possible, we simplified our stream graphs. Complex branching and merging patterns are powerful but hard to debug. We preferred linear flows with clear stage boundaries.
3. **Better monitoring** : We added metrics at stream boundaries, tracking buffer sizes, element throughput, backpressure events. Visibility into stream internals helped diagnose issues.
4. **Team education** : We documented our learnings, shared patterns that worked, and built institutional knowledge about reactive streams.


**Lesson** : Powerful abstractions require investment. Don’t assume you understand a framework without validation. Build your mental model deliberately, test it with experiments, and be humble about your understanding. Reactive streams are worth mastering for systems that need to handle load gracefully, but expect a learning curve.


## V2 Evolution: Continuous Refinement


V1 got us to production. The major architectural challenges like Kafka lag, hot nodes, memory pressure, were solved. But production at full scale revealed new optimization opportunities. V2 represents the continuous refinement that turns a working system into a production-ready system.


### Challenge 5: Persistent Heap Pressure


**The Problem** : Despite V1 optimizations, we still observed higher-than-desired heap usage. GC metrics improved but weren’t optimal. Memory profiling showed room for improvement.


**Root Cause** : Deeper analysis revealed we were still doing unnecessary object conversions between stages. We’d convert aggregators to full graph entities (with all properties populated) before routing to the next stage, even though the next stage just needed the compressed aggregator state.


**Solution** : Architectural change to route aggregators directly through all stages, only converting to final graph entities at Stage 3 immediately before persistence. This eliminated two intermediate conversion steps and the associated object allocation.


**Result** : Heap usage dropped further, GC pauses became even less frequent, and memory headroom improved.


### Challenge 6: Serialization Complexity


**The Problem** : Custom serialization logic for SSE messages caused occasional erratic errors that were hard to reproduce and debug. Different parts of the codebase used inconsistent serialization approaches.


**Solution** : Standardized on JSON encoding throughout the pipeline. While slightly less efficient than binary serialization, JSON’s human readability made debugging far easier, and the overhead was negligible compared to other operations. Consistency eliminated an entire class of bugs.


**Result** : Serialization-related errors disappeared. Debugging became easier because we could read SSE message contents directly.


### Challenge 7: Stream Processing Inefficiencies


**The Problem** : Even after understanding reactive streams better, our Pekko configurations weren’t optimal. We had over-parallelized some stages and under-parallelized others. The .async boundaries weren’t placed optimally.


**Solution** : Through continued profiling and experimentation, we tuned parallelism parameters, adjusted buffer sizes, and refined async boundary placement. We added monitoring at stream boundaries to identify bottlenecks.


**Result** : Throughput improvements and more consistent processing latency.


### Challenge 8: Uneven Graph Database Throughput


**The Problem** : Write distribution to our graph database wasn’t even. Some partitions received heavy write traffic while others sat idle. This caused throttling to kick in unevenly and limited overall write throughput.


**Solution** : Implemented batching of aggregators before writing to the graph database and improved distribution logic across partitions. Rather than writing each aggregator immediately, we batch them and write multiple entities in coordinated operations.


**Result** : More consistent write throughput and better utilization of database capacity.


### Challenge 9: Data Enrichment at Aggregation Time


Beyond the core topology graph, we needed to enrich nodes with additional context. At Stage 3, before persisting graph entities, we integrate enrichment data from external sources, application health status, ownership information, and other metadata. Performing this enrichment at aggregation time rather than at query time avoids the performance overhead of post-query joins and ensures every topology node has full context when queried.


### Pattern Recognition


Each V2 challenge followed the same pattern: production revealed an assumption, profiling identified the root cause, targeted fixes improved specific metrics. Measure, hypothesize, validate, iterate. This is how you build at scale, not by getting everything right upfront, but by continuous learning and improvement.


## Time Travel: Continuous Topology Reconstruction


One of the most powerful capabilities we built enables querying historical topology: “What did the call graph look like when this incident happened?” This time-travel feature required solving an interesting architectural challenge, how to efficiently store and reconstruct topology across time.


### The Problem


Engineers need to answer temporal questions: What did the topology look like during an incident? How have dependencies evolved? Traditional approaches, full snapshots or event sourcing — either have exponential storage costs or require slow log replay.


### Our Approach: Time-Windowed Aggregators with Mutation Tracking


We combine three mechanisms:


**1. Time-Windowed Aggregator Snapshots** : Every aggregator stores startTs and endTs timestamps for its 5-minute window. These immutable aggregators persist in the graph database keyed by (entity_id, timestamp), providing checkpoint states every 5 minutes.


**2. Property-Level Mutation Tracking** : The graph database maintains mutation history at the property level, storing only changed properties with timestamps. This is much more efficient than full entity copies and provides sub-window precision beyond the 5-minute aggregation boundaries.


**3. Query-Time Reconstruction** : When querying historical topology, we query the mutation history API for the time range, retrieve all mutations, and reconstruct topology state by applying mutations in order.


This approach provides efficient storage (compressed aggregator states + sparse property mutations), fast retrieval (indexed mutation history, no log replay), and flexible analysis (arbitrary time ranges without pre-computing all possibilities).


**Query-Time Re-Aggregation** : We can further aggregate historical data at query time using the same aggregator classes from ingestion. This enables arbitrary groupby dimensions (availability tier, business domain, deployment cluster) that weren’t pre-computed, allowing exploratory analysis without exploding storage costs.


## Lessons for Distributed Systems


While these challenges were specific to service topology, the lessons apply broadly to distributed systems at scale.


### Scale Changes Everything


What works at 100 requests per second fails at 100,000 requests per second. The change isn’t linear, it’s qualitative. Approaches that are fine at modest scale hit fundamental walls at extreme scale.


Examples from our journey: immutable data structures create GC pressure at millions of allocations per second; single-stage aggregation fails catastrophically with power-law traffic distribution; standard gRPC becomes heavyweight for streaming aggregation at volume.


The lesson: be willing to break conventional wisdom when scale justifies it. But do it based on measurement, not speculation.


### Optimize One Bottleneck at a Time


Distributed systems have cascading bottlenecks. Fix Kafka lag, and you discover hot node issues. Fix hot nodes, and you discover GC problems. Fix GC, and you discover serialization inefficiencies.


This isn’t failure, it’s the nature of complex systems. Each optimization raises throughput, which stresses the next weakest point. The approach: prioritize based on impact, fix the current bottleneck thoroughly with measurement confirming resolution, then move to the next one. Optimization at scale is continuous, not one-time.


### Distribution Is Key to Scale


Single aggregation points are inevitable bottlenecks. Consistent hashing distributes load but doesn’t prevent concentration when data itself is unevenly distributed (power-law distributions like ours).


Our three-stage pipeline with graduated redistribution solved this. Load spreads across multiple distribution points at each stage. Even with highly skewed data, no single instance becomes overwhelmed. The general principle: use multi-stage processing with redistribution at each stage when dealing with skewed data at scale.


## Current State and Impact


Service Topology operates in production today, processing flow logs, IPC metrics and traces from multiple regions and serving queries with sub-second latency. Teams across Netflix use it daily for incident investigation, blast radius analysis, dependency understanding, and production change management. The system has become essential infrastructure for maintaining reliability at scale.


## Conclusion


Service Topology at Netflix represents a journey through building distributed systems at scale. We started with engineers struggling to understand dependencies across scattered tools. We built a multi-layer architecture using streaming aggregation, network intermediary resolution, and time-travel capabilities. And we learned that optimization at scale is continuous, measure, iterate, validate, repeat.


The challenges we faced, Kafka lag, hot nodes, memory pressure, required breaking conventional wisdom when data justified it. Each fix revealed the next bottleneck. But that iterative process, guided by constant measurement, is what makes systems work at extreme scale.


In our next post, we’ll explore the tracing layer integration, unified querying across heterogeneous storage, and how all three layers combine to provide comprehensive topology visibility.


## Acknowledgements


*Service Topology was built by*[Parth Jain](https://www.linkedin.com/in/parth-jain-8a09abb6/) *,*[Rakesh Sukumar](https://www.linkedin.com/in/raskuma/) *,*[Yingwu Zhao](https://www.linkedin.com/in/yingwu-zhao-62037418/) *,*[Renzo Sanchez-Silva](https://www.linkedin.com/in/renzosanchezsilva/) *, and*[Nathan Fisher](https://www.linkedin.com/in/nathfisher/) *.*


*Special thanks to the many engineers across Netflix who made this possible — the Observability team who built the broader system, the graph database platform team who provided the storage foundation, and the Platform Modernization Engineering and Live teams who provided invaluable feedback and use cases throughout development.*
