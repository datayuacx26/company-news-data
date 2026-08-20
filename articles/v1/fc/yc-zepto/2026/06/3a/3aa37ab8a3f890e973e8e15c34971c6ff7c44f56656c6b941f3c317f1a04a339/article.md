---
schema_version: "1.0.0"
document_id: "3aa37ab8a3f890e973e8e15c34971c6ff7c44f56656c6b941f3c317f1a04a339"
company_key: "yc-zepto"
company: "Zepto"
source_id: "yc-zepto-rss-dc680377f8f2"
canonical_url: "https://blog.zepto.com/clickhouse-ingestion-at-scale-an-open-source-zepto-engineering-story-7f57309e2175"
published_at: "2026-06-17T08:59:04+00:00"
first_seen_at: "2026-08-10T05:06:20.120332+00:00"
fetched_at: "2026-08-10T05:06:23.353340+00:00"
content_hash: "sha256:10d4700c84ebee9d48ea3b4ecb7a12e193e6650d3f6028652fb0e91928317c55"
---

# ClickHouse Ingestion at Scale: An Open-Source Zepto Engineering Story

# ClickHouse Ingestion at Scale: An Open-Source Zepto Engineering Story


[Zepto Tech](https://medium.com/@tech.culture?source=post_page---byline--7f57309e2175---------------------------------------)


8 min read


·


Jun 17, 2026


--


Press enter or click to view image in full size


Much like our journey described in[Debezium at Scale](https://blog.zeptonow.com/debezium-at-scale-an-open-source-cdc-story-from-zepto-aa4b12e32bf7) , our architecture relies heavily on real-time data flow. To understand user journeys, track operational metrics, and power our growth, we built **Lucid** — Zepto’s completely in-house product analytics engine designed to replace Mixpanel.


Lucid captures millions of events per minute, routes them through Kafka, and dumps them into ClickHouse to give us lightning-fast, high-precision insights without the third-party SaaS pricing trap. We use **Confluent Cloud** to manage our Kafka infrastructure and the **in-house** ClickHouse Sink Connector. It was seamless — until our scale broke the default physics.


Every hyper-growth engineering team eventually hits a wall where managed abstractions turn from a blessing into a bottleneck. For us, that wall appeared right at the intersection of Apache Kafka and ClickHouse.


To ingest billions of events into ClickHouse for **Lucid** at a sustained throughput of **10 MB/s (peaking up to 15–20 MB/s),** we hit a wall with Confluent Cloud’s managed infrastructure because its managed nature restricted our access to low-level broker and connector tuning. Instead of migrating our entire Kafka ecosystem, we proved our batching hypothesis on an In-house Kafka Proof-of-Concept, and then built that buffering logic directly into the open-source ClickHouse Kafka Connect framework. By rewriting core parts of the connector, we boosted ingestion by **45%** , eliminated crippling GC pauses, and drastically reduced ClickHouse insert pressure.


This is the story of how we overcame the **black box of managed cloud** , the hidden performance killers we found in the open-source connector, and the two major pull requests we merged to fix them and contribute back to the community.


## The Inciting Incident: The Confluent Cloud Black Box


Confluent Cloud is an incredible platform for getting off the ground quickly. But as our event volume skyrocketed, we noticed lag accumulating on our consumer groups.


When you operate at scale, tuning becomes everything. We initially suspected the bottleneck was tied to broker-side fetch limits. In open-source Kafka,[KIP-541](https://ossip.dev/kips/KIP-541.html) sets the default *fetch.max.bytes* to **55MB** to protect brokers from rogue consumers. We thought this was our culprit and wanted to tune these low-level broker settings to optimize for ClickHouse’s unique ingestion patterns.


But Confluent Cloud is a fully managed SaaS service, by design, it operates as a black box. Through our own deep dive into Confluent Cloud’s underlying architecture ( **the Kora engine** ), we realized it doesn’t have a **1–1 match** for these open-source Kafka configs. Because it is fully managed, CC relies on strict cluster capacity guardrails, partition-level ingress/egress limits, and connector throughput limits to protect the environment.


The broker-side batch size is limited only by cluster capacity. To get the ingestion speed we needed, our focus had to shift to connector poll configurations and scaling up tasks to bypass the CC connector throughput limits.


*We were driving a sports car, but we were locked out of the engine bay. We couldn’t tune the broker, so we had to adjust how we drove* .


## The In-house Kafka Proof of Concept


We suspected that if we could just control the batch sizes and flush intervals, we could stabilize the pipeline. To prove this hypothesis, we spun up a quick poc on In-house Kafka.


It gave us the configuration knobs we needed. We tuned the low-level polling and fetching behaviour, and instantly, the pipeline stabilized. The hypothesis was proven.


But migrating our massive production Kafka infrastructure off Confluent Cloud wasn’t a path we wanted to take. We started looking into whether we could force Kafka Connect itself to batch records. We weren’t alone in this struggle. In a[community discussion (#400)](https://github.com/ClickHouse/clickhouse-kafka-connect/discussions/400) , another engineer noticed that standard Kafka Connect consumer overrides like *fetch.max.wait.ms* and *fetch.min.bytes* were failing to accumulate large enough batches without tuning broker configs for ClickHouse before flushing.


The maintainers clarified that Kafka Connect’s out-of-the-box polling behaviour fundamentally lacks native connector-level batch size controls. When you pair this lack of connector-level batching with Confluent Cloud’s strict broker-level limits, you are stuck. As we pointed out in that very thread: *“In Confluent Cloud, how can this problem be solved without batching?”*


The answer was simple: it couldn’t.


**The Decision:** Since we couldn’t tune the cloud environment, and Kafka Connect lacked the native capabilities, we had to shift our focus and build custom buffering intelligence directly into the open-source ClickHouse connector itself.


## Trial 1: The “Too Many Parts” Problem


Before our buffering implementation, the ClickHouse cluster was throwing alarms constantly. The error logs were flooded with:


```text
DB::Exception: Too many parts in order by…
```


## The 5 Whys: Uncovering the Polling Flaw


To get to the root of the issue, we ran a quick “5 Whys” analysis:


1. **Why is the ClickHouse cluster choking?** Because it’s throwing constant “ *Too many parts in order by* ” exceptions.
2. **Why are there too many parts?** Because ClickHouse (a columnar DB that thrives on large, infrequent inserts) is receiving thousands of tiny inserts per minute.
3. **Why are we doing tiny inserts?** Because the ClickHouse Kafka sink connector is flushing data too frequently.
4. **Why is it flushing so frequently?** Because when we looked at the[ClickHouseSinkTask.java](https://github.com/ClickHouse/clickhouse-kafka-connect/blob/main/src/main/java/com/clickhouse/kafka/connect/sink/ClickHouseSinkTask.java) , we found that the flush behaviour was strictly coupled to the Kafka consumer’s *poll()* loop. If a poll returned 500 records, it immediately flushed 500 records.
5. **Why can’t we just configure the poll to return larger batches?** We tried. We maxed out every consumer override we could throw at the connector


```text
consumer.override.fetch.min.bytes  consumer.override.fetch.max.wait.ms  consumer.override.request.timeout.ms  consumer.override.max.poll.records  consumer.override.max.poll.interval.ms  consumer.override.session.timeout.ms  consumer.override.heartbeat.interval.ms
```


But math always wins. Our broker’s` fetch.max.bytes` were rigidly capped around 20–55 MB per fetch. With an average record size of **~3 KB** , the theoretical maximum ranged from roughly **7,000 to 18,000** records per poll. In practice, due to partition-level fetch limits and consumer behaviour, we typically observed **7,000–9,000** records per poll. It was mathematically impossible to build a batch large enough to satisfy ClickHouse within a single *poll()* cycle. Confluent Cloud’s managed guardrails only made this worse, restricting our ability to dictate the broker-side fetch sizes needed to close the gap.


## Get Zepto Tech’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Our motivation for the fix drew philosophical inspiration from Confluent’s well-known article on[Kafka consumer multi-threaded messaging](https://www.confluent.io/blog/kafka-consumer-multi-threaded-messaging/) . While that post focuses on spawning multiple worker threads to process messages(eg: IO-bound processing) truly independently of the main polling thread, the underlying principle is identical: **you must decouple the strict, synchronous cadence of the poll() loop from your downstream processing and I/O strategy.**


## The Fix: Decoupling Polling from Flushing ([PR #658](https://github.com/ClickHouse/clickhouse-kafka-connect/pull/658) )


We needed a buffer that transcended the *poll()* lifecycle. We dove into the codebase and introduced an internal buffering implementation.


We added two new configuration options:


- *bufferCount* : The number of records to accumulate across *multiple* poll calls before flushing.
- *bufferFlushTime* : The maximum time in milliseconds to wait before flushing, regardless of the count.


By accumulating records in memory and flushing them as a single large batch, we reduced the number of *INSERT* queries hitting ClickHouse by over an order of magnitude. The *Too many parts errors* vanished, and cluster health stabilized.


**Community Validation:** We weren’t the only ones suffering from this. In the[PR comment](https://github.com/ClickHouse/clickhouse-kafka-connect/pull/658#issuecomment-4013479232) for our buffering fix, another GitHub user, highlighted exactly how this lack of buffering destroyed throughput. They noted that running **v1.3.5** in production, the connector was making one ClickHouse INSERT *per partition per poll cycle* . With ~45 partitions per task, this resulted in severe degradation during low traffic, dropping to ~750 records/sec overnight when they needed ~50K records/sec just to recover from the peak-hour backlog. They urged the maintainers to prioritise our PR, validating that we had directly addressed the root cause.


## Trial 2: The Silent CPU Killer


We resolved the database pressure, but our throughput remained inexplicably capped. Our Kafka Connect worker CPU usage was pegged at 80%, and Garbage Collection (GC) pauses were out of control.


We attached a profiler, expecting to see network I/O or compression as the culprit. Instead, the flame graph pointed directly to JSON serialisation.


## The Culprit: Gson and UTF-16 String Allocations


The open-source connector was using Google’s Gson library for JSON serialisation (doInsertJsonV1/V2). For every single record, the connector instantiated per-call Gson instances and executed a fundamentally expensive operation:


1. It serialized the Java Struct into a UTF-16 Java String.
2. It then called *.getBytes(StandardCharsets.UTF_8)* to convert that String into a byte array for the network payload.


At million of events per minute, allocating and destroying gigabytes of intermediate UTF-16 String objects was completely overwhelming the JVM Garbage Collector. It was a massive GC churn on the hottest path in the application.


## The Fix: Jackson writeValueAsBytes ([PR #676](https://github.com/ClickHouse/clickhouse-kafka-connect/pull/676) )


We ripped out the per-call Gson instances and replaced them with a shared, static Jackson *ObjectMapper* .


Jackson has a massive advantage here: writeValueAsBytes(). This method serialises Java objects *directly* into UTF-8 byte arrays, completely bypassing the intermediate UTF-16 String allocation.


```text
// Before (The Memory Hog):  String gsonString = gson.toJson(cleanupExtraFields(data, table), gsonType);  byte[] bytes = gsonString.getBytes(StandardCharsets.UTF_8);
```


```text
// After (The Jackson Speed Demon):  Map<String, Object> cleaned = cleanupExtraFields(data, table);  byte[] bytes = OBJECT_MAPPER.writeValueAsBytes(cleaned);
```


We also registered a custom *JsonSerializer<Struct>* so Jackson could cleanly serialize Kafka Connect Struct objects as \` *{field: value}\`* JSON (replacing Gson’s reflection-based internals, which exposed schema data).


## The Impact


The performance gains were staggering. Production measurements showed:


Press enter or click to view image in full size


Press enter or click to view image in full size


## The Return: Contributing Back


We didn’t just patch this on our internal fork. The beauty of open-source is the feedback loop. We packaged our buffering implementation into[PR #658](https://github.com/ClickHouse/clickhouse-kafka-connect/pull/658) , and our *Jackson ObjectMapper* rewrite into[PR #676](https://github.com/ClickHouse/clickhouse-kafka-connect/pull/676) .


Working with the ClickHouse maintainers (shoutout to[@mzitnik](https://github.com/mzitnik) , @[chernser](https://github.com/chernser) , and the team), both PRs went through rigorous review, integration testing, and were successfully merged into the main branch.


## Conclusion


Managed services like Confluent Cloud are fantastic multipliers for small teams. But as your data gravity grows, you will inevitably hit constraints.


When you hit a wall with a managed platform, you don’t necessarily have to tear out your infrastructure and migrate elsewhere. Sometimes, the most effective solution is to understand your systems at a microscopic level, fire up a profiler, and push the optimization down into the open-source world.


**The solution is usually just a PR away.**


**What’s Next?**


Now that our ClickHouse ingestion is stable and lightning-fast at billions of events, we are ready to talk about the system it powers. Stay tuned for our next deep dive, where we will unpack the architecture of **Lucid** — how we built Zepto’s in-house product analytics engine from scratch and successfully ditched Mixpanel for good.
