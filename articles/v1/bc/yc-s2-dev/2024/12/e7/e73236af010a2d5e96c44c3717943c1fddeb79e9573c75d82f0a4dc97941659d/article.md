---
schema_version: "1.0.0"
document_id: "e73236af010a2d5e96c44c3717943c1fddeb79e9573c75d82f0a4dc97941659d"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/intro"
published_at: "2024-12-20T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:28ed68baf84a6283521f81d447534d7c1801de624352cd46820b0705a77b552a"
---

# Introducing S2

> Elevate the beating heart of data systems.


Our team has worked a lot on reliable real-time ingest, where[The Log](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying) is foundational. We loved the serverless experience of object storage, but it[simply did not exist](https://blog.schmizz.net/designing-serverless-stream-storage#heading-vision) for streaming data.


We believe the humble log – the ***stream*** – deserves to be a cloud storage primitive.


With S2, we are previewing just that: S2 is the Stream Store, our interpretation of streaming for the cloud era.


## What if streams had the primacy of objects?


Object storage has been nothing short of revolutionary. S3 broke ground in 2006 with simple storage operations on named objects – and 18 years later, S3 Express One Zone even allows appends. But ultimately, object storage is all about blobs and byte ranges. It is best for data at rest. Our vision of stream storage is predicated on the idea that the demands of *data in motion* need a fresh perspective.


With S2, you are elevated to the natural granularity of *records* . Writes to an S2 stream are appended at the tail, and even if multiple writers are acting at a time, S2 will durably sequence all records. S2 takes care of serving your reads efficiently, whether you need to start streaming from seconds ago or years. Streams can also be tailed in real-time, which is not possible with a blob in S3.


Object Storage Stream Storage


Blobs and byte ranges Records and sequence numbers


` PUT` /` GET` /` DELETE` value of a named` Object` in a` Bucket`` APPEND` /` READ` /` TRIM` records on a named` Stream` in a` Basin`


Cumbersome and expensive for granular appends Easy and cheap to append records


Just like buckets are a namespace for objects, *basins* play that role for streams in S2. Basins and streams lean into the scale of the cloud – there is no limit on how many you can have, or how long data can be retained.


Want to model streams per user? Do it, this isn't Kafka. There are no cluster limitations to wrangle, and no infrastructure to tune.


```text
$   s2   ls   s2://copilot-rag-ingest
user-foo/cool-project
user-foo/another-project
user-bar/fork-of-cool-project
# ... ∞
```


This stream interface brought us closer to our vision, but we also wanted to liberate the superpower of[offloading durability](https://blog.schmizz.net/disaggregated-wal) which databases like MemoryDB and Neon leverage. Decoupling compute and storage is safest when the storage service cooperates.


So we added a verb to[check the tail](https://maheshba.bitbucket.io/papers/osr2024.pdf) of the stream with strong consistency, and support for concurrency control when writing. You can be a pessimist wielding a[fencing token](https://brooker.co.za/blog/2024/04/25/memorydb.html) or optimistically supply the sequence number you expect assigned – no judgment which side of the fence you find yourself on.


## Serverless – at what cost?


S2 is architected around the infinite scale and unrelenting durability of object storage. That does not necessitate a slow or expensive offering – quite the opposite! We bridge the abstraction gap with a multi-tenant service so that you can have a truly serverless API for streaming data.


Durability is not negotiable for us in the undeniable[cloud storage triad](https://materializedview.io/p/cloud-storage-triad-latency-cost-durability) . We allow users to navigate their latency vs cost tradeoff on a per stream basis, with *storage classes* . We are starting out with two:


1.


**` Standard`** , backed by S3 Standard in AWS. S3 Standard has a counterpart in all public cloud providers, so we will be able to ship it in all cloud regions as we grow.


2.


**` Express`** , backed by a quorum of three S3 Express One Zone buckets in AWS. Azure has had a regional[counterpart](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-block-blob-premium) for years, and it is[in the cards](https://www.linkedin.com/feed/update/urn:li:activity:7170847492522061825) at GCP, so we are optimistic about wider availability.


Our` Standard` storage class provides end-to-end p99 latencies of **under 500 milliseconds** . With` Express` you can expect **under 50 milliseconds** – in the realm of disk-based cloud streaming systems! S2 on the other hand is completely diskless, and all writes will be safe in S3 with regional durability before being acknowledged.


These latencies are supported at throughputs of **hundreds of megabytes per second, per stream** . The overhead of reading recently written data is negligible in S2 because of in-memory caching. Lagging readers can be particularly thirsty for throughput, and S2 serves them directly from object storage without a cap. We are initially throttling writes at 125 MiBps and reads against recent writes at 500 MiBps, per stream.


The service is free during our preview period to optimize for feedback. We want to be transparent about our[intended pricing](https://s2.dev/pricing) , and you will find that S2 comes in meaningfully cheaper than the norms of cloud streaming systems. This will be particularly stark in comparison to "serverless" offerings, which attract tiny ceilings on the number of streams and the throughputs you can push through them – at a very high premium.


There are no fixed costs in S2 like instances or cluster units. When we say serverless, we mean it!


## What's next for S2


S2 stands on a foundation of battle-tested cloud infrastructure, and our own Rust codebase gets put through the wringer with deterministic simulation testing. That said, the system is young, and there will be kinks. We are working hard to mature towards general availability and an SLA you can count on in production.


We are now shipping a gRPC API,[Rust SDK](https://github.com/s2-streamstore/s2-sdk-rust) , and a[shiny CLI](https://s2.dev/docs/quickstart#get-started-with-the-cli) – and we are going to get cracking on a[REST API](https://s2.dev/docs/api) . Do[tell us](https://discord.gg/vTCs7kMkAf) which language SDKs you would be most interested in.


To give you a bigger picture sense of our direction:


-


**Kafka protocol compatibility** . This will be an open source layer, and we will integrate certain features like key-based compaction directly in S2.


-


**Multi-region basins** . Once we expand into more cloud regions, we see a path towards basins that can span regions and even clouds, for the highest standard of availability.


-


**Under 5 millisecond latencies** . We are just getting started with the architectural flexibility of storage classes, and another 10x improvement over` Express` is achievable.


Can you replace Kafka or Kinesis with S2 today? If you find yourself reaching for their “low-level” APIs, S2 is likely to be a fit, and even address your requirements more directly.


If you expect a lot more from the cloud than current norms for streaming data – like not being limited on how many streams you can have, 10-100x higher ordered throughput, and concurrency control – S2 is the missing piece.


We are beyond excited for all the innovative data systems S2 enables, and invite you to[build with us](https://s2.dev/docs) !
