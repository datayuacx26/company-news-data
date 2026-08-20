---
schema_version: "1.0.0"
document_id: "a4758cb434ce32cbe26a6552cdb24636e38e2820450bf5bfb18dcbd20b35518b"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/streaming-platform-kafka-custom-abstractions/"
published_at: "2025-02-19T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:feb23f258498352c75804151b33a5032848115683885a926d1205c564026d8f4"
---

# Achieving relentless Kafka reliability at scale with the Streaming Platform

Guillaume Bort


At Datadog, we use Apache Kafka as a durable, flexible buffer in our Data Platform. Ingesting hundreds of trillions of observability events each day requires massive Kafka deployments: hundreds of clusters, thousands of topics, and millions of partitions spanning multiple Kubernetes and cloud environments. This translates to terabytes of traffic per second, and issues that might be rare in smaller setups—like disks filling up or brokers failing—occur daily at this scale.


Traditional Kafka configurations are very static: a producer writes to a specific topic on a specific cluster and consumers are tied to that same, often immovable configuration. But we can’t pause to debug an unhealthy cluster or wait for a problematic broker to recover. We must be able to instantly redirect traffic to a healthier cluster and reroute consumers accordingly—without reconfiguring and without waiting on redeployments.


To achieve this, we built the **Streaming Platform** , a control layer that abstracts Kafka’s complexity and enables real-time reliability at scale. In this post, we’ll explore its key components: how **Streams** allow us to build resilient pipelines decoupled from specific clusters, how we handle failovers and rebalancing with the **Assigner** , how we eliminate head-of-line blocking with a **smarter commit log** , and why we built **libstreaming** , a custom Kafka client to optimize performance and observability across all our applications.


## A fluid control plane for multi-cluster orchestration


Our approach with the Streaming Platform is to treat Kafka infrastructure like commodity hardware—modular, interchangeable, and dynamically managed rather than static and tightly coupled. Instead of binding producers and consumers to fixed Kafka resources, our system continuously adapts by shifting workloads across clusters, automatically replacing unhealthy components and ensuring uninterrupted data flow. This design allows Kafka to operate as a self-healing system, eliminating the need for manual intervention.


Achieving real-time reliability across Datadog’s massive, multi-cluster infrastructure required reimagining Kafka’s architecture—designing resilient pipelines, redefining consumer semantics, and building a dynamic control plane tailored to our unique scale and needs.


### Resilient pipelines with Streams


Streams form the backbone of our resilient infrastructure. Unlike traditional Kafka topics, Streams are designed to span multiple Kafka clusters, availability zones (AZs), and Kubernetes clusters, making them highly resilient to any single point of failure. Streams also provide stable identifiers, allowing producers and consumers to interact with the Streaming Platform without worrying about the underlying Kafka topology.


By decoupling applications from Kafka’s resources, Streams allow us to reconfigure infrastructure in real time and seamlessly reroute traffic to new topics or clusters when necessary. This eliminates the need for application reconfigurations or redeployments, ensuring our platform can adapt instantly to changing conditions.


### Live traffic failovers


The Stream abstraction is critical for enabling live failovers. When a Kafka cluster experiences issues—whether due to maintenance, scaling needs, or unexpected failures—we don’t wait for it to recover. Instead, the control plane creates a new topic, seamlessly redirects traffic to it, and begins draining the old one. Producers automatically shift to sending new data to the replacement topic, while consumers process the remaining backlog in the original topic before transitioning fully. This process happens transparently for uninterrupted data processing.


Failovers aren’t limited to emergencies. We leverage this capability proactively to redistribute traffic, balance loads, decommission clusters, and adjust the number of partitions dynamically. These operations, which might take hours in a traditional Kafka setup, are completed in seconds with the Streaming Platform.


### Consumer semantics focused on scale, durability, and reliability


Operating at this level of dynamism required rethinking Kafka’s core consumer model. Kafka’s native reliance on strict ordering guarantees and single-pointer offsets can create bottlenecks. We addressed this by shifting to an at-least-once delivery model and relaxing ordering guarantees at the processing stage.


This decision allowed us to unlock massive parallelism: Events can be processed independently and at scale, with ordering[re-established later downstream in our event store](https://www.datadoghq.com/blog/engineering/husky-deep-dive/) . This trade-off allows us to fully leverage the distributed nature of Kafka, ensuring our pipelines can process petabytes of data efficiently.


### A real-time multi-cluster coordinator: the Assigner


Kafka’s default group coordinator wasn’t designed for multi-cluster environments and relies on session timeouts to detect changes—a process that can take tens of seconds or even minutes. To meet our real-time requirements, we built the **Assigner** , a custom control plane coordinator.


The Assigner continuously monitors cluster health, resource usage, and workload distribution, reacting to changes in seconds rather than waiting for timeouts. This real-time capability ensures seamless failovers and rapid rebalancing during incidents or traffic spikes.


Additionally, the Assigner introduces intelligent load balancing that distributes workloads based on actual metrics like CPU load and resource availability. This allows us to efficiently utilize heterogeneous environments and adapt to resource shortages or capacity changes without disrupting data processing. By giving us fine-grained control over workload distribution, the Assigner pushes the limits of what’s possible with Kafka coordination, ensuring reliability and performance at a massive scale.


## Overcoming the main Kafka issue: head-of-line blocking


Kafka’s model guarantees strict ordering within partitions, but this introduces a major reliability challenge: **head-of-line blocking** . If even one event—like a poison pill that fails to process—causes a delay, it blocks the entire partition. This is unacceptable for a platform like ours, where data integrity and real-time performance are critical.


We solved this challenge through two key innovations: **Stream lanes** for better quality of service (QoS) and an **advanced commit log** to bypass Kafka’s single-pointer limitation.


### Enabling quality of service with Stream lanes


To prevent traffic with different requirements from interfering with one another, we introduced independent lanes within a Stream. Each lane represents a separate priority level, allowing us to isolate and manage traffic based on its quality of service (QoS) needs. For example, real-time traffic flows through a high-priority lane to ensure it is processed without delay, while less critical bursts or late-arriving data is routed to slower lanes. This separation prevents spikes or delays in one type of traffic from impacting others, maintaining smooth and predictable data flows.


In addition, Streams include a special dead-letter queue (DLQ) lane for handling poison pills. When an event causes an unrecoverable error, it is copied to the DLQ so the rest of the partition can continue processing. This allows us to commit progress without losing any data and ensure that no single problematic event can block an entire partition.


### Advanced Kafka commit log


Beyond QoS, we needed a way to overcome Kafka’s traditional commit log model, which tracks a single pointer per partition. This limitation prevents consumers from making progress on new events while still processing older ones, creating a bottleneck when dealing with backlogs or transient failures.


To address this, we enhanced the Kafka commit log by leveraging its commit metadata space. This allows us to track multiple offsets or ranges of offsets within a partition simultaneously. With this smarter commit log, consumers can process live traffic while continuing to reprocess older backlogs or retry failed events in the background. For example, if a downstream dependency becomes temporarily unavailable, we can skip over the affected events and return to them later without impacting real-time traffic.


A smarter commit log is essential for advancing reliability and processing flexibility. Features like tracking retry counts or marking offsets as “rejected” without the overhead of copying them to another queue could significantly improve performance and resilience. While hijacking commit metadata has been a valuable starting point, its limitations are clear. We are already exploring more powerful approaches, including building a totally ad hoc commit log that operates independently of Kafka’s built-in mechanisms, just as we did with our custom coordinator.


## Real-time lag monitoring


Lag is a critical metric for any streaming platform, as it measures the delay between when data is ingested and when it is processed by consumers. However, Kafka’s default lag metric, expressed in offsets, provides little actionable value. Offsets only indicate how many messages are unprocessed without showing how delayed the data is in real-world terms.


Calculating **time lag** —the delay in wall-clock time—is notoriously difficult in a standard Kafka setup. Kafka does not provide metadata to directly associate offsets with timestamps, and determining lag would require fetching records from partitions to extract their timestamps. At Datadog’s scale, with hundreds of consumer groups, millions of partitions, and the need to compute lag in real time, this approach is computationally impossible.


Our solution leverages the enriched commit log we built as part of our Streaming Platform. By enhancing the commit log with ingestion timestamps and other metadata, our consumer applications can commit additional context with each offset. This allows our control plane to compute time lag by consuming` __consumer_offsets` topics across all Kafka clusters. These topics, which contain all consumer group commit updates, now provide instant visibility into the current state of consumer applications.


These insights are exposed as metrics in Datadog, enabling fine-grained alerts and automated responses. If a consumer group begins to fall behind, our system detects the delay immediately and rebalances workloads to prioritize real-time data while addressing backlogs in the background. This ensures uninterrupted data flow for real-time use cases, even during high-traffic spikes or processing disruptions.


## A custom client library built for scale


To integrate seamlessly with the Streaming Platform, we initially wrapped existing Kafka clients in custom logic to support Streams and the Assigner. However, as Datadog’s scale and complexity grew, this approach proved insufficient, leading us to develop a client library tailored specifically to our needs.


### Challenges of scale: the Zstandard compression bug


The need for a custom client became clear when we encountered a particularly nasty issue while trying to enable[Zstandard (zstd) compression](https://github.com/facebook/zstd) for our Kafka data plane. Zstd offers excellent compression, but the kind of data we stream (such as logs and spans) compresses extremely well—sometimes too well. In edge cases, the default Kafka producer would create compressed batches that exploded to massive sizes once decompressed, sometimes several gigabytes. These “zstd bombs” caused catastrophic failures in consumers, which were unable to handle such oversized batches.


The failure modes varied across Kafka clients. For example, rdkafka would double its buffer size repeatedly in an attempt to decompress the batch, eventually crashing the entire process. This made the problem worse due to Kafka’s head-of-line blocking model, as a single “zip bomb” could stall the entire partition. We spent significant effort patching default Kafka clients, tweaking them to limit batch sizes based on decompressed size rather than compressed size. This required deep monkey-patching, such as overriding the compression factor estimation in the Java client. While these patches mitigated the issue temporarily, it became clear that relying on off-the-shelf clients could no longer meet our needs at Datadog’s scale.


### A unified client: libstreaming


From this realization, we developed **libstreaming** , a unified client implemented in Rust with lightweight bindings for Java, Go, Python, and other languages used at Datadog. Rust offered the perfect combination of performance, memory safety, and compatibility with multiple ecosystems. By consolidating our business logic into a single client, we simplified development, ensured feature parity across languages, and aligned observability metrics—eliminating the divergence caused by using different Kafka clients.


Initially, libstreaming relied on rdkafka as its underlying Kafka client, but challenges like zstd bombs and other performance bottlenecks revealed its limitations. To overcome these constraints, we built our own Kafka client in Rust, focusing on the subset of Kafka features essential to our platform. This approach allowed us to optimize every aspect of data handling—from fetching and decompression to committing—resulting in a client built for our massive throughput and real-time requirements.


### Why not a gRPC proxy?


Another approach we considered was building a gRPC service to abstract Kafka access, similar to the Kafka Consumer Proxy described by[this blog post](https://www.uber.com/en-FR/blog/kafka-async-queuing-with-consumer-proxy/) . This would have centralized the complexity of our platform into a single service, allowing for thin, lightweight clients in each language. While this approach works well at smaller scales, it wasn’t feasible for Datadog’s data plane. Our consumers process terabytes of data per second, and proxying this volume through a centralized service would have introduced significant latency and bottlenecks. By deploying libstreaming directly within each application, we maintain low-latency access to Kafka while fully leveraging the capabilities of our custom control plane.


With libstreaming, we’ve built a client library that scales seamlessly with Datadog’s infrastructure. It integrates deeply with our Streaming Platform, enables consistent observability across all applications, and provides the reliability needed to handle the extreme demands of our data pipelines.


## Next steps


Our Streaming Platform is constantly evolving as we push the limits of real-time reliability at Datadog’s scale. Today, we are focused on strengthening our Kafka storage layer to further improve resilience against availability zone failures while leveraging Kafka tiered storage to enhance durability and cost efficiency.


At the same time, we’re continuing to refine our commit log design—exploring smarter approaches to predicate-based filtering in the data plane, more flexible offset tracking, and better handling of reprocessing scenarios. These improvements will allow us to further decouple consumers from partition constraints and make our platform even more reliable, scalable, and efficient.


And we’re not stopping there. The challenges of operating Kafka at Datadog’s scale continue to push us to rethink and innovate beyond its traditional limits.


If solving large-scale, real-time challenges excites you,[we’re hiring](https://careers.datadoghq.com/all-jobs/?s=kafka) .


-
-
-
