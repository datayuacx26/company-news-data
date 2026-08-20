---
schema_version: "1.0.0"
document_id: "1df7557bcae3595969bf60cdbabd12eaa5a4eb32e37e995cc9db1a6ce9907746"
company_key: "yc-sf-tensor"
company: "SF Tensor"
source_id: "yc-sf-tensor-news-import-88ed66988698"
canonical_url: "https://sf-tensor.com/news/stfs"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T08:45:53.460276+00:00"
fetched_at: "2026-08-05T08:45:55.721043+00:00"
content_hash: "sha256:434d1fe78197e64641c02fdab0fd8215563fa6f4df76fa6d605270836965f1df"
---

# STFS: Stop Making Every Worker Hammer S3

**The short version** : on a large training cluster, hundreds of nodes read the same objects out of the same S3 bucket, over and over, epoch after epoch. Every one of those reads pays full origin latency and full origin cost and most of the bytes were already sitting on a disk one rack away. STFS is our distributed read-through cache that fixes this: it makes an S3 bucket look like a filesystem, lets the cluster share cached ranges peer-to-peer and pulls those bytes over the fabric that is otherwise sitting idle. In a 64-node production run, it cut origin traffic by 97% and turned reads into sub-200ms p95 operations.


## The problem


Training data, model checkpoints and container images live durably upstream, usually in an S3-style bucket. That is the right place for them: it is cheap, durable and it is the source of truth. But it is also the wrong place to read from a thousand times a second.


Here is the shape of a real workload. A training run runs on the same data every epoch. Epoch 0 reads n objects, epoch 1 reads the same n objects again just in a different order from different nodes, epoch 2 reads them again. Every worker on every node issues its own ranged GETs to origin, independently, as if none of its neighbors had ever seen the data. The bytes travel the full distance from object store to GPU, then the next worker asks for the exact same bytes and they travel the full distance again.


The result is a bottleneck that scales the wrong way: the more nodes you add, the more redundant load you put on the one thing you cannot scale by adding nodes. Origin egress becomes a tax you pay on every pass and it can get expensive fast.


That does not need to be the case.


## The bet


The SF Tensor File System (STFS) is a disposable performance layer. S3 stays the source of truth: if STFS evaporates, nothing is lost and the next read just goes to origin. What STFS adds is a fast path made of things the cluster already owns: local NVMe, the east-west network and, when it is free, the RDMA fabric.


The design commits to three things:


**It is a cache, not a filesystem.** STFS is not trying to be POSIX. The core abstraction is three operations:` lookup` ,` contents` and byte-range` read` . That is what object-store workloads actually use and refusing to fake rich write semantics is what keeps the routing layer simple enough to reason about.


**It's cluster-aware.** When a node caches an object or a byte range, it advertises that fact to its peers. A read that misses locally does not go straight to origin: it first asks whether any peer already holds that range and reads it from the neighbor if one does.


**It caches ranges, not just whole objects.** A worker asking for a 64 MiB slice of a 1 GiB object should not force a 1 GiB fill. STFS tracks available ranges, pending fills and inflight ranges per object, so that partial residency is a first-class state instead of something either not handled or neglected.


## The idle fabric


The reason we built the RDMA variant is visible in a single graph.


InfiniBand fabric utilization during a typical training run


TX and RX bandwidth from the raw node captures, with each panel scaled to its own capture window.


Loading InfiniBand samples...


TX


RX


Hundreds of Gb/s of fabric are idle between communication bursts.


Two dimensions of the same run. Even the busy sharding fabric sits well below its ceiling; the scale-out fabric is idle the majority of the time.


The bursts are the communication phases of training: gradient all-reduce, parameter sync. Between them the fabric does nothing. On a large cluster, that idle capacity is hundreds of terabits to petabits per second of bandwidth that we've provisioned and paid for and that is sitting idle for large fractions of every step.


With STFS we put that idle fabric to work moving data. Instead of pulling a file from origin over the general-purpose north-south network, a node can read the range directly from a neighbor's NVMe into GPU memory over RDMA, using bandwidth that would otherwise be wasted. This matters most in the workloads that are growing fastest: non-LLM training such as video, where a single dataset can be petabytes and keeping your silicon fed with data is a constraint.


We mix and match. When the fabric is free, we use it. When training is mid-all-reduce and the fabric is saturated, we fall back to the normal east-west network. The scheduler's job is to use the pipe that is idle right now.


We build this on` libfabric` (OpenFabrics Interfaces), which is the mechanism behind a core part of our bigger thesis: the details of the cluster should be invisible to the user. The same STFS read path runs over InfiniBand, RoCE and the hyperscaler-specific variants such as AWS EFA. You do not rewrite anything to move between them. Part of making heterogeneous compute become real is making the transport underneath interchangeable.


## What a read actually does


An application reads` /mount/bucket/key` like any other file. Under that:


1. FUSE maps the inode to a path.
2. STFS tries the local cache first. If the requested range is resident, the read completes locally, with FUSE passthrough and file references keeping cached local reads close to raw disk speed.
3. On a local miss, the service router asks: does another node own this object and range? If yes, the read is served from that peer, preferably over RDMA.
4. Only if no node holds the range does the local cache fetch it from S3 with a ranged GET, write it into cache and advertise the new availability so the next node to want it routes to us instead of origin.


For programs that just want a file, the FUSE mount means every existing tool works unchanged:` ls` ,` cat` , your data loader, all of it. For the hot path, a training driver can link` libstfs` directly and skip the VFS layer, the FUSE daemon and the round trip back into the kernel entirely, requesting a file and range straight into GPU memory. Observability is baked into the same surface: a` /stats/...` tree is generated by the mount machinery, so inspecting cache behavior is just reading files.


## The benchmark


First, we ran a simple production-style workload: 7 nodes, 2 ranks per node, 4 workers per rank, 4 epochs of 8 steps, 64 MiB range reads against a 256 GiB dataset. The point is to expose whether a system actually amortizes reuse. Total application volume was 112 GiB. We compared three configurations: reading directly from S3, STFS and` s3fs` (the common off-the-shelf FUSE-over-S3 mount) with local NVMe caching enabled.


112 GiB workload: STFS vs direct S3 vs s3fs


7 nodes, 4 epochs. Lower is better for both metrics.


STFS origin traffic: 0.29x origin bytes per application byte.


s3fs origin traffic: 1.13x, more than the naive direct-S3 baseline.


STFS is shortest on wall-clock time and origin traffic: 32.48 GiB from origin for 112 GiB of application reads.


The aggregate numbers:


run wall s throughput GiB/s origin GiB origin/app read p50/p95/p99 s


direct-s3 46.80 2.39 112.00 1.00x 0.735 / 1.687 / 2.343


stfs 41.65 2.69 32.48 0.29x 0.102 / 1.828 / 3.326


s3fs 81.01 1.38 126.74 1.13x 0.531 / 8.574 / 10.670


Two things stand out. STFS moved 32.48 GiB from origin to satisfy 112 GiB of reads: 0.29x origin bytes per application byte, a 71% cut in origin traffic. And` s3fs` , despite caching to the same local NVMe, actually pulled *more* from origin than the naive baseline (1.13x) and ran 73% slower: a cache that does not coordinate across nodes and does not handle range reuse well can lose to no cache at all.


But the aggregate hides the real story, which is in the epochs.


Read latency p95 by epoch


Seconds, lower is better.


Cache filled: STFS is about 10x faster than direct S3 across epochs 1-3 and holds there.


Epoch 0 fills the cache. From epoch 1 onward, STFS p95 latency drops to roughly 0.18s while direct S3 stays around 1.6-1.8s.


STFS is slower than direct S3 in epoch 0, which is the result of the design: epoch 0 is paying to fill the cache, reading from origin and writing to local NVMe while peers advertise ranges to each other. Its p95 read is 2.951s while direct S3 sits at 1.583s. (s3fs pays an even steeper fill cost, at 10.523s, because it fills the same NVMe cache without coordinating across nodes.)


Then the mechanism kicks in. From epoch 1 onward, STFS p95 read latency drops to roughly 0.18s and holds there: about 10x faster than direct S3, which stays flat at 1.6-1.8s every single epoch because it learns nothing between passes. This is the through-line: STFS is not built to make the first read magical. It is built to make the second, third and fourth reads nearly free, which is exactly the access pattern of every multi-epoch training run.


The movement counters confirm where the wins come from. Of 92,653 routed reads, 77,345 were served from local cache and 12,572 from peers, against only 2,736 true misses. Peer traffic in this run was modest, about 1.48 GiB, so in this configuration the dominant effect is local range caching and origin avoidance rather than peer sharing. STFS also did this on a memory budget: 0.75 GiB average and 1.08 GiB peak resident per node, against` s3fs` at 2.56 GiB average and 3.15 GiB peak.


Then, we ran a larger production workload: 64 nodes, 8 ranks per node, 4 workers per rank, 12 epochs, 64 MiB range reads against a 1 TiB dataset.


49 TiB workload: STFS vs direct S3 vs s3fs


64 nodes, 12 epochs. Bandwidth is linear; origin reads use a log-scale origin/app view.


Aggregate bandwidth, GiB/s (higher is better)


Origin read per application byte, log scale (lower is better)


Origin reads are shown as origin bytes per application byte on a log scale, with raw TiB labels on each bar.


STFS delivers the highest aggregate bandwidth while reducing S3 origin reads to 0.02x origin bytes per application byte.


The current node stores whole objects rather than blocks, which is easier to reason about but gives up the load-balancing and erasure-coding options that a block-based layout would open up, which we hope to explore in the future.


If you have a workload where the same data gets read many times and origin egress is your tax, we'd love to hear about it.
