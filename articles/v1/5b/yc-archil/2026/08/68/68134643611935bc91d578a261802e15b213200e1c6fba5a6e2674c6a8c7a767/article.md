---
schema_version: "1.0.0"
document_id: "68134643611935bc91d578a261802e15b213200e1c6fba5a6e2674c6a8c7a767"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-6ed493c8d31a"
canonical_url: "https://archil.com/article/scaling-s3"
published_at: null
first_seen_at: "2026-08-09T19:14:46.011593+00:00"
fetched_at: "2026-08-09T19:14:47.557372+00:00"
content_hash: "sha256:32f7c7a567b82d5d01ed9710a639c007fdf08939ea2c10cdc44a596802981dad"
---

# Scaling S3: Navigating request Rates, throughput, scaling, and S3TA

Whether you’re backing up petabytes of data, powering a global video-streaming service, or ingesting high‐velocity telemetry, you need predictable S3 performance at any scale. S3 is horizontally scalable but two limitations apply:


1. **Request rate limits:** determined by how many internal partitions your keys span
2. **Network Throughput:** dependent on client bandwidth, distance, and concurrency


Developers must understand these constraints and keep them in mind to ensure their products remain reliable. We’ll begin by reviewing request rates limits and how to scale them with prefixes. Then, we will explore S3 Transfer Acceleration with multi-part uploads for boosting throughput.


## Scaling Request Rates with Prefixes


First, let’s discuss how request rate throttling works.


S3 scales through partitions, where objects are stored in discrete slices of virtual memory. S3 object keys are partition-cognizant, where each key prefix maps to a partition. However, partitions have request limitations, with the baseline request rate of


~3,500 writes per second for` PUT` ,` COPY` ,` POST` and` DELETE` .


~5,500 reads per second for` GET and`` HEAD.`


### Prefixes determine partitions


AWS's partition system is reactive to keys. For example, by designating different key prefixes, a wildlife video workload might group objects under keys such as:


- ` birds/pigeon/…`
- ` birds/robin/…`
- ` birds/owl/…`


Here, each **prefix** maps to a separate partition. Since the rate limit applies per prefix, using three well-balanced prefixes effectively **triples the baseline capacity** to **~10,500 writes per second** and **~16,500 reads per second** across all of the data.


AWS automatically partitions data as well. If a single prefix suddenly receives more requests than its baseline capacity, S3 detects the traffic and automatically splits that prefix into additional internal partitions to raise its capacity. However, this is not immediate—while the split is fast, users may hit brief` 503 Slow Down` responses.


There’s no limits to the number of prefixes that you can create in a bucket, so using multiple balanced prefixes is a straightforward way to avoid hitting request rate limits.


## Increasing Throughput with S3 Transfer Acceleration


However, requests are only one side of the coin. Once you’ve architected your S3 buckets for high request rates, the next bottleneck to overcome is throughput.


AWS documentation doesn’t provide throughput numbers for S3 in the same manner as request rates. Throughput limits are more complex, subject to customized client bandwidth, regional latency, object size, concurrency, and other factors outside AWS’s control. However, to reduce transfer bottlenecks, AWS provides S3 Transfer Acceleration (S3TA).


### How S3TA Works


S3TA boosts upload and download speeds by routing each transfer through the nearest AWS edge location.


When a client requests an object, such as` https://birds.s3accelerate.amazonaws.com/pigeon/pigeon.mp4` , the request first hops to the AWS edge location closest to the user and then travels over AWS’s private backbone to the S3 bucket. Because this path is largely internal, it bypasses the public internet’s latency and instead uses network dedicated to AWS traffic.


The results are significant! AWS reports a 1.5x-10x reduction in latency. However, S3TA incurs an additional context, approximating to $0.04 per gigabyte (including data-in or data-out).


### Accelerating Large File Transfers with Multi-Part Uploads


For very large objects, you can combine S3TA with multi-part uploads to maximize throughput.


According to AWS benchmarks, uploading a 485 MB file with a single PUT over the public internet takes approximately 72 seconds. Enabling S3TA reduced this same request to 43 seconds (+40% boost). Even better, combining S3TA with multi-part uploads slashed the time to just 28 seconds, nearly 2.6x faster than the baseline.


Using multi-part uploads makes sense for files are larger than 100 MB. For files over 5 GB, it is required.


There are multiple additional benefits to this strategy. These include:


- **Parallelization** : S3 splits the object into independently uploaded parts, allowing multiple parts to upload concurrently.
- **Resilience** : If an individual part fails, only that part is retried (not the entire object). Minimizing lost work on large transfers.
- **Efficiency with S3TA** : When combined with S3TA, each part is routed to the nearest AWS edge location, ensuring consistent high throughput.


By leveraging multi-part uploads with S3TA, you can fully utilize concurrent network steams to maximize throughput


## Common use cases for S3TA


### Global Content Distribution


Imagine you’re opening up an app to watch a live stream of your favorite sports team on NBA, or trying to catch up on the latest series on Netflix. Without S3TA, video requests must travel across the public internet, leading to slow startups and potential playback issues. **Before S3TA:**


- Viewers need to wait 5–8 seconds for videos to start and experience frequent buffering, especially in areas with poor connectivity.


**But after S3TA r** equests connect to the nearest AWS edge location and travel through AWS's private backbone network Startup times reduce to just 1–2 seconds, and streams remain smooth and high-quality, even on unstable networks.


### Disaster Recovery Replication


For systems with tight recovery-point objectives (RPO), every minute of delay in backing up data represents potential loss. Financial service firms like Capital One, which processes millions of transactions every hour, cannot afford any gaps in its backup solutions. Before S3TA uploading a 1 TB snapshot over the public internet takes about 36 minutes, leaving that window of data unprotected. But after S3TA, the same snapshot completes in roughly 9 minutes, reducing exposure by 75% without changing backup schedules. With multi‑part uploads, the 1 TB snapshot is split into parts and uploaded in parallel, cutting transfer times to as little as 6 minutes and shrinking the unprotected window by over 80% compared to the original 36‑minute transfer.


## Key Takeaways


To ensure optimal performance for workloads, developers must thoughtfully address both request rates and throughput limits:


- Distribute traffic across multiple, balanced key prefixes to avoid` 503 Slow Down` errors and let S3 autoscale partitions smoothly.
- Enable S3TA to route data through the nearest AWS edge location and across AWS’s private backbone, slashing latency for global users.
- Use multi‑part uploads to efficiently transfer large objects by uploading parts concurrently and automatically retrying any that fail.


Together, these practices elevate S3 from a simple storage service into a scaleable, high performance backbone.
