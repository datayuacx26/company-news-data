---
schema_version: "1.0.0"
document_id: "82508ada0cac4ee1e7d123f4ca12590b7fc764b1bff51371848c3ac708657bf8"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/data-pipeline-completeness/"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:f12f5357a9b372e26cfaa37070f477bb7df3a4a636bf47bbc685a18f952728f4"
---

# How we measure data completeness at scale

Valentin Touffet


Alexandre Olivier


When a customer gets paged at 3 a.m., they expect the graphs in Datadog to show the full picture of the data they sent. When an AI agent, such as one making autoscaling or remediation decisions, acts, it relies on the same assumption.


At Datadog’s scale, billions of payloads per second flow through hundreds of distributed pipelines. Even a small hiccup raises hard questions: Did the data stall? Where? And did it impact customers?


Those payloads power everything from ad hoc queries to push-based products like[Monitors](https://docs.datadoghq.com/monitors/) . As more workflows are automated using AI agents, the stakes get higher: Downstream systems act on whatever data they receive. Incomplete data doesn’t just affect a dashboard. It affects real decisions. But those decisions are only as good as the data behind them. To trust the data, we need a way to verify its completeness in real time for every customer.


The Data Completeness team at Datadog is responsible for measuring completeness across all ingestion pipelines. This system has to answer that question for every product and every customer. In this post, we explain how we built our data completeness system to track payloads end to end across our ingestion pipelines.


## Define what complete data means at Datadog scale


What does **complete** mean in Datadog’s ingestion pipelines?


For us, **completeness** means that every payload entering Datadog is available to our customers, whether they’re using our AI agents, looking at dashboards, or receiving an alert at 3 a.m. In this context, a payload can be a metric datapoint, a log, a span, or any other type of telemetry data ingested into Datadog.


Answering “Is the data complete right now?” is harder than it sounds. Our ingestion stack spans hundreds of services and many possible paths between them. More importantly, we need to answer this question per customer, not globally. Each customer’s data can follow a different path depending on partitioning, isolation strategies, or traffic patterns. At scale, this creates a combinatorial explosion in the number of possible paths we need to reason about.


Metrics pipelines span hundreds of distinct paths in a single region. Products like[Application Performance Monitoring (APM)](https://www.datadoghq.com/product/apm/) , which also feed data into the metrics pipeline, add their own tens of paths.


We needed a system that could track data end to end across Datadog’s ingestion pipelines without depending on the intake and processing services it observes. This system had to answer “Is the data complete?” and explain where and why it is not when systems degrade.


Just as importantly, the system needed to support the engineers operating these ingestion pipelines by providing accurate diagnostics to identify which parts of the system are unhealthy so that both humans and automated systems can react in seconds.


## Track data completeness one segment at a time


Before landing on this approach, we considered watermark-based tracking, similar to what streaming frameworks like[Flink](https://flink.apache.org/) use. The idea is appealing: Advance a watermark as data arrives and declare completeness once it passes a threshold.


But watermarks rely on data arriving within a predictable window. At Datadog, customers can send arbitrarily delayed data, making it impossible to advance a watermark. Our ingestion pipelines also involve loops, traffic replay, and other edge cases that make any watermark-based approach too unreliable for the guarantees we needed. So we went back to first principles.


To track payloads across distributed ingestion pipelines, we started with a simple idea: Split pipelines into segments and track each payload as it moves through them. We can then combine those segment-level measurements to compute the overall completeness of a pipeline.


In practice, a pipeline is composed of multiple services connected by queues (for instance, intake to Kafka to processing to Kafka to router). We define a **segment** for each step in that flow, both within a service and between services. For example, intake-in to intake-out is one segment, and intake-out to processing-in is another. This lets us reason about completeness locally at each step, rather than trying to observe the entire pipeline at once.


We model the ingestion pipeline as a sequence of segments inside and between services (for example, intake-in to intake-out or processing-in to processing-out), allowing us to measure completeness at each step and combine those measurements into an end-to-end view. We model the ingestion pipeline as a sequence of segments inside and between services (for example, intake-in to intake-out or processing-in to processing-out), allowing us to measure completeness at each step and combine those measurements into an end-to-end view.


The segment model lets us localize where completeness degrades (for example, between intake and processing, or within a specific service) while still aggregating those signals into an end-to-end view. It also handles pipeline evolution well, as branching paths can appear and disappear over time without requiring a global redefinition of the system.


### Count payloads by using creates and acknowledgments


Once we defined pipeline segments, tracking payloads became possible: How many payloads entered a segment, and how many exited it? If both counts are equal, we haven’t lost any. To make this reliable—even when payload submissions are retried—we assign a unique identifier to each payload.


When a payload enters a segment, we record a **create** operation in our completeness system. When the payload exits, we record an **acknowledgment** for that same identifier. By comparing these two counts, we can determine the completeness of each segment.


Because distributed systems can retry or reorder events, these counts need to be idempotent. We achieve this with a time-bucket model. Completeness is always scoped to a bucket indexed by when the payload first entered Datadog, using a timestamp we control rather than relying on customer-side clocks.


Within each bucket, every unique identifier is tracked with its current state per segment: created, acknowledged, or acknowledged before the create identifier was received, which is a valid edge case in distributed systems. When a duplicate create or acknowledgment arrives for the same identifier in the same bucket, the system recognizes it from the existing state and ignores it. The time-bucket model makes the count inherently idempotent without coordination overhead.


The following diagram shows how create and acknowledgment events flow through the system and are aggregated per segment.


Each segment reports create and acknowledgment counts to the completeness system, which computes completeness as the ratio of acknowledgments to creates within a time bucket (for example, 9/10 = 90%). Each segment reports create and acknowledgment counts to the completeness system, which computes completeness as the ratio of acknowledgments to creates within a time bucket (for example, 9/10 = 90%).


### Compute end-to-end pipeline completeness from segment ratios


Each segment is measured independently by comparing how many payloads enter it with how many exit it. We call this ratio **segment completeness** .


To compute the completeness of an entire pipeline, we combine segment-level measurements by using mathematical operations. For sequential pipelines, completeness is the product of segment ratios.


For parallel pipelines, it turned out to be more complex. Some pipelines have internal branches. APM trace data, for example, is processed by two independent downstream services: one computing error rates and request counts, another computing latency distributions. If we treat the two branches as a single pipeline, a completeness bucket stays incomplete until the slowest branch has processed all relevant data. This makes already available data appear incomplete.


This forced us to rethink how we aggregate results. We used a weighted average model for parallel branches, where each origin contributes to the overall completeness in proportion to the volume it carries.


We combine segment-level completeness values to compute end-to-end pipeline completeness across sequential and parallel paths. We combine segment-level completeness values to compute end-to-end pipeline completeness across sequential and parallel paths.


For example, in the diagram above, we show a pipeline that processes 10k payloads with two parallel branches:


-


One branch contains two services that process 6k payloads (98% complete) and 5.88k payloads (96% complete), respectively. To compute the overall completeness of this branch, we multiply their completeness ratios: 98% × 96% = 94% complete.


-


The other branch processes all of its payloads, resulting in a 100% completeness ratio.


We compute the combined completeness by weighting each branch by its volume:


This result is then combined with the rest of the pipeline by multiplying sequential segments. In this example, the surrounding segments are all 100%, so the final end-to-end completeness remains 96%.


## Design for reliability and cost efficiency at scale


Measuring completeness is only useful if the measurement is trustworthy, especially during an incident, when the stakes are at their highest and the data is most likely to be wrong. This requirement shaped every architectural decision we made.


Designing this system meant balancing competing constraints.


We needed per-customer accuracy without turning the system into a bottleneck. The measurement also had to remain reliable even when the ingestion pipeline itself was degraded. At the same time, we avoided external dependencies, since failures in those systems would undermine the very signal we rely on during incidents.


## Understand the completeness system’s core architecture


Let’s take a closer look at how the system works. It is built around two main services: an **intake** layer and a **storage** layer.


The intake receives tracking events and forwards them to storage. The storage layer maintains pipeline completeness state and tracks the unique identifiers of in-flight payloads.


Each ingestion service runs a lightweight client library that sends create and acknowledgment events to the intake. A query service retrieves this data from storage and serves completeness queries.


The completeness system collects create and acknowledgment events from ingestion services via an intake layer, forwards them to storage, and serves completeness queries through a query service. The completeness system collects create and acknowledgment events from ingestion services via an intake layer, forwards them to storage, and serves completeness queries through a query service.


### Achieve per-customer accuracy without drowning in cost


Given that each customer’s data can follow a different path depending on partitioning, isolation strategies, or traffic patterns, we needed accurate per-customer measurements without overwhelming the system.


You might wonder how we can track every payload entering Datadog for every customer without prohibitive cloud costs. The answer is that we don’t.


Instead, we track a subset of payloads per customer and adjust this subset dynamically based on customer traffic. This allows us to maintain accurate measurements while keeping overhead under control.


In each ingestion service, the completeness client makes a load-shedding decision to sample or not whenever a payload enters a segment. If the payload is sampled, a create or acknowledgment event is sent to the completeness system. If not, the client increments a counter representing the weight of unsampled payloads, so they are still accounted for without being tracked individually. The client sends this accumulated weight alongside the next sampled create operation.


To decide which payloads to sample, the client relies on a **load-shedding bulletin** . This bulletin specifies which segment and customer combinations should be sampled, along with a sampling ratio.


The completeness system computes the bulletin centrally and distributes it to ingestion services, allowing each client to make consistent decisions based on current traffic and system load.


The completeness system generates the bulletin by using a fixed threshold per segment and customer, which bounds overhead while preserving accuracy. In practice, we tune this threshold so that the completeness signal remains actionable without driving up tracking costs.


The system computes load-shedding bulletins in two stages, as shown below.


First, each storage node computes a partial bulletin for the subset of customers and segments it is responsible for. This computation runs over a longer time window, which keeps it stable but prevents it from reacting quickly to sudden traffic spikes. In particular, it cannot account for traffic that never reaches storage because the system is already overloaded.


To address this, the intake layer computes the final bulletin. It combines the storage-generated bulletin with a view of in-flight traffic, allowing it to detect sudden surges and adjust sampling more aggressively. This two-stage design ensures the system can react in real time while still benefiting from the stability of the storage-side computation.


The completeness system computes a load-shedding bulletin based on traffic observed across storage nodes and distributes it to ingestion services via the intake layer. Clients use this bulletin to decide which payloads to sample, while unsampled payloads are accounted for through aggregated weights. The completeness system computes a load-shedding bulletin based on traffic observed across storage nodes and distributes it to ingestion services via the intake layer. Clients use this bulletin to decide which payloads to sample, while unsampled payloads are accounted for through aggregated weights.


In addition to load-shedding, we focused on optimizing our storage layer. Even with aggressive load-shedding, we still store hundreds of millions of unique payload identifiers per second. This data is retained only for a short time window (a few hours), but still amounts to dozens of terabytes.


Given the volume of data processed, queried, and indexed, as well as our access pattern, we built a custom in-memory storage layer rather than relying on existing solutions.


The workload is highly specialized. Create and acknowledgment events arrive as a continuous stream, scoped to short time windows, and must be counted per segment and per customer without cross-payload coordination. Off-the-shelf solutions like Redis would introduce an external dependency, which we avoid, and their general-purpose data models add overhead we do not need.


Instead, our storage is organized around 60-second time buckets and a lock-free, worker-per-shard model. Each worker owns a slice of the keyspace and processes it in a single-threaded event loop, eliminating lock contention. Payload identifiers are encoded compactly, using only a few bytes per entry to store their idempotency state. This keeps the memory footprint significantly lower than a general-purpose solution, at a fraction of the cost.


### Build a system that outlives the outage


Building a system that outlives the thing it monitors sets an unusually high bar. If our completeness system depends on Kafka, it fails exactly when Kafka fails, which is when we need it most. This led to one foundational decision: **minimal external dependencies** .


This meant no Kafka, no external database layer, and avoiding dependencies on systems whose failure modes are outside our control. This approach proved effective in practice. During a major Kafka cluster incident, our completeness system continued running while dependent systems degraded around it.


Our deployment model reflects this focus on reliability and blast radius reduction. We implemented what we call **tracks** , where we partition the products we cover into separate deployments. This ensures that issues in one product’s ingestion pipeline do not affect completeness measurements for others.


We also rely on availability zones. We deploy our intake and storage layers independently across two availability zones, running two copies of the system in parallel so each zone can continue operating if the other is degraded. This allows us to roll out changes safely, with one zone running the new version while the other continues serving traffic.


This design also improves resilience to cloud provider incidents and traffic surges. Each availability zone uses a different sharding scheme, which reduces the blast radius when a single customer generates a traffic spike and impacts its neighboring workloads.


The completeness system is deployed as independent tracks, with each track running in multiple availability zones. This isolates products from one another and allows completeness data to remain available even when part of the system is degraded. The completeness system is deployed as independent tracks, with each track running in multiple availability zones. This isolates products from one another and allows completeness data to remain available even when part of the system is degraded.


Each availability zone uses a different partitioning scheme. If customer 1 experiences a traffic surge that overloads storage A and storage C, data for customers 2 and 3 remains available in storage D and storage B. Each availability zone uses a different partitioning scheme. If customer 1 experiences a traffic surge that overloads storage A and storage C, data for customers 2 and 3 remains available in storage D and storage B.


We also built protection mechanisms to prevent the system from collapsing under traffic surges. These are frequent because we depend on the traffic of upstream systems we monitor, which are themselves driven by customer traffic.


One example is a last-resort rate limiter in the intake layer. It tracks in-flight requests and drops a subset of new ones if the downstream storage layer becomes unavailable. To avoid distorting completeness measurements, the rate limiter is aware of storage partitioning and only rejects requests targeting slow or unavailable partitions. Dropping requests indiscriminately would degrade completeness data even when the underlying pipelines are healthy.


Lastly, we invested in tooling for incident response. We built an internal interface to manage the completeness system, including an invalidation timeframe tool that allows us to override completeness measurements for a given time window. The invalidation timeframe tool is useful when the completeness system itself is misbehaving while the underlying ingestion pipelines remain healthy.


## Act on completeness signals during incidents and automated decisions


Measuring completeness is only half the problem. The other half is making the signal useful.


To do this, we store an additional layer of information alongside payload tracking: a **metadata layer** , kept in a separate custom in-memory storage system that resembles a graph database. This metadata lets us discover system topology and run analytics on completeness data.


The metadata layer is structured as a time-bucketed graph. Nodes represent services, and edges represent the segments between them. Like the storage layer, it is entirely custom-built and in memory.


The completeness system collects both payload tracking events and topology metadata. The intake layer forwards create and acknowledgment events to storage, and topology metadata to a separate metadata store, which is used to describe pipeline structure. The completeness system collects both payload tracking events and topology metadata. The intake layer forwards create and acknowledgment events to storage, and topology metadata to a separate metadata store, which is used to describe pipeline structure.


This structure allows us to answer questions such as:


-


What is the full path between two services?


-


What does a pipeline look like for a given track?


-


Which segments does a service participate in?


It gives both engineers and automated systems a live view of the ingestion topology without relying on static configuration.


Once completeness data is collected, we need a way to query it. The query layer takes a topology description, for example a path between two services, and returns the completeness of that path.


In a distributed system like Datadog, where services are owned by many teams, it is rare for any single engineer to have a complete view of system interactions. The metadata layer fills this gap by showing how services connect, how to query completeness data for automatic decision-making, and where payloads are delayed during an incident.


The metadata also lets us understand which system is owned by which team and how they depend on each other, so our system can automatically page the right team when an issue is detected.


Our completeness information is mostly consumed by internal systems to make automatic decisions and reduce the customer impact of pipeline delays. One example is Datadog[Kubernetes Autoscaling](https://www.datadoghq.com/product/kubernetes-autoscaling/) . This system relies on[Metrics](https://docs.datadoghq.com/metrics/) data submitted to Datadog, but if the metrics pipeline is delayed, we don’t want to make scaling decisions based on stale or incomplete data. Instead, Kubernetes Autoscaling uses the completeness information bundled with every query to the Metrics platform to decide whether to act on that data.


## Extend coverage and improve automation


Completeness data has become our primary signal for detecting customer-impacting incidents. Most pipeline degradations we’ve seen in recent years surfaced through it, either as a direct page or as a contributing signal. Detection typically happens in under a minute, though our monitors are tuned to absorb transient lag and avoid false positives from short-lived hiccups.


Beyond detection, the system actively contains damage. When a pipeline starts lagging, completeness-aware systems stop acting on stale data. Scaling decisions on incomplete metrics are avoided, and alerts are less likely to fire on data that has not fully arrived. Issues that would otherwise trigger a 3 a.m. page or silently distort customer workloads are less likely to escalate.


Completeness measurement is useful only if it drives action fast enough to matter. Looking ahead, we’re working to bring completeness information closer to the engineers who need it. This includes building an AI assistant that can help troubleshoot completeness issues and evaluate customer impact during incidents. Our goal is to integrate this directly into incident response workflows so that Datadog engineers can access it where they already work, such as Slack or in[Bits AI](https://docs.datadoghq.com/bits_ai/) .


We also continue to face challenges such as making measurements granular enough to cover increasingly complex partitioning or extending completeness measurements to systems we don’t support today, like databases. Those challenges continue to reinforce a lesson we learned early on: Measuring completeness is often less difficult than understanding what the result actually means.


Data completeness becomes difficult when pipelines are large, branching, and constantly evolving. The hardest part is not computing a number, but confidently explaining where the data is and why it isn’t complete. By modeling the ingestion graph as segments and tracking payload progress through each one, we turned a fuzzy, end-to-end problem into something observable, debuggable, and actionable, enabling both faster operational understanding and automated responses.


Along the way, we learned that correctness at scale is less about perfect precision and more about reliable signals with bounded cost: fast in-memory state, adaptive controls, and designs that keep working even when dependencies fail.


We are continuing to extend coverage, improve accuracy, and invest in tooling and automation. We hope the patterns we shared here help other engineers build completeness systems that hold up under real-world distributed failures.


If you’re excited about building systems that operate reliably at scale,[we’re hiring](https://careers.datadoghq.com/all-jobs/?s=distributed&parent_department_Engineering%5B0%5D=Engineering&child_department_Engineering%5B0%5D=Backend&utm_source=engblog&utm_medium=corpsite&utm_campaign=engcommunity-2025-dataengineering-architecture&gh_src=hm4uekgj1us) **!**


-
-
-
