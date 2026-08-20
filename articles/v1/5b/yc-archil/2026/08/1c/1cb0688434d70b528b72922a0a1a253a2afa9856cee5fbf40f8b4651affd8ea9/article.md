---
schema_version: "1.0.0"
document_id: "1cb0688434d70b528b72922a0a1a253a2afa9856cee5fbf40f8b4651affd8ea9"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/s3-files-deep-dive"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:258fc7cf7b892d2b73f818d52b59485a4ec0c8b0c6c2d0188dc0eb13b812cf60"
---

# S3 Files: the right product on the wrong foundation

In 2023, I was the product manager at AWS responsible for figuring out how to bring EFS file systems and S3 buckets together into one product. AWS launched my recommendation this week as S3 Files. I've since left to start[Archil](https://archil.dev/) , where we're building what I think cloud file storage should actually look like.


S3 Files is EFS sitting in front of an S3 bucket, which means it inherits the NFS protocol and all of its architectural constraints. Those constraints cap its throughput, inflate its latency, and force a pricing model that breaks the economics developers expect from cloud storage.


## How S3 Files actually works


Under the hood, S3 Files puts an EFS file system in front of an S3 bucket and runs replication between the two. EFS has been live since 2016, and it uses the NFS protocol -- the standard Linux network file system -- to talk to clients.


NFS was a reasonable choice for EFS when it launched. It's universally supported, it's in the Linux kernel, and developers know how to use it. But NFS was designed in an era when "shared file storage" meant a few workstations in a lab. It makes assumptions that create hard limits on what you can build on top of it.


To understand why, you need to understand how NFS differs from the two things it's often compared to: block storage (EBS) and parallel file systems (Lustre).


## The NFS problem


When you create a file on your laptop -- or on an EBS volume, or any local disk -- **the kernel buffers the operation in memory and returns immediately** . Data and metadata are flushed to disk asynchronously -- the application doesn't wait. This is why local file operations feel instant.


NFS can't do this. If two clients are both connected to the same file system and both try to create` hello.txt` , the only place that conflict can be resolved is the server. There is no client-side shortcut. As a result, every mutating metadata operation -- creating a file, renaming a file, changing permissions -- must round-trip to the server before the client can proceed.


NFS 4.1, as implemented by EFS, resolves to a single mount target IP and funnels all subsequent requests -- metadata, reads, writes -- through that one connection. NFS 4.1 does support session trunking in theory, but EFS doesn't use it in a way that changes this picture.


## Consequence 1: Latency


The round-trip-per-operation requirement is what made EFS infamous.


Most database products in AWS -- PlanetScale, Supabase, RDS, and many others -- are wrappers around EBS volumes. EBS is fast because it's a block device: it doesn't understand files, so it doesn't need to coordinate. But EBS is also limited -- it attaches to a single instance, so you can't use it for zero-downtime deployments or horizontal scaling.


This is exactly the use case file systems were built for. People would migrate from EBS to EFS when they needed multiple machines to share data. And then their application would fall over. A workload that created thousands of small files per second on EBS would slow to a crawl on EFS, because each of those creates now required a network round-trip instead of a memory operation. We're talking about an order of magnitude difference -- and that's the gap between "my application works" and "my application is unusable."


The EFS team employs a lot of talented engineers who have worked hard to shave microseconds off per-operation latency. But this isn't an implementation problem. It's a protocol problem. As long as NFS requires a server round-trip for every mutating operation, file systems built on NFS will always be an order of magnitude slower than block storage for metadata-heavy workloads.


File storage offers things block storage can't: usage-based pricing and multi-client access. There's no reason we should accept that it has to be *dramatically slower* than a raw disk.


## Consequence 2: Scalability


The flip side of latency is throughput -- how many operations you can do per unit time. When people say S3 is "fast," they mean throughput, not latency. Each individual S3 operation is slow, but you can fire off thousands of them in parallel without hitting a wall.


S3 achieves this because its operations are trivially parallelizable.` GetObject` and` PutObject` operate on individual keys with no cross-key coordination. Each request is a separate HTTP connection, so S3 can spread them across as many frontend and storage servers as it needs. The nominal per-prefix limit is 5,500 GETs/s and 3,500 PUTs/s, but S3 automatically partitions hot prefixes, so in practice the effective ceiling is much higher for well-distributed keyspaces.


File systems are harder. A single` rename` operation -- moving` foo/hello.txt` to` bar/other.txt` -- requires synchronization on the source directory, the destination directory, and the file itself. You can't just spray these across independent servers.


The industry solved this decades ago with parallel file systems. Lustre, the most widely adopted one, separates metadata (which needs coordination) from bulk data (which doesn't). Clients talk to a metadata server to find out where file data lives, then connect directly to storage servers for the actual bytes. You scale throughput by adding storage servers. You scale IOPS by clustering the metadata service.


The catch? Lustre's client never made it into the mainline Linux kernel -- it sat in the staging tree and was eventually removed for complexity. Using it means manually installing an out-of-tree kernel module that may or may not be compatible with your kernel version. Not great for a general-purpose cloud product.


So NFS was the pragmatic choice for EFS. But pragmatic comes with a ceiling.


Because NFS funnels everything through a single connection to a single server, there's a hard cap on per-client throughput: **3 GiB/s** .


That's fine for a lot of workloads. It is not fine for GPU instances doing model training, which need to saturate NICs that are much larger than 3 GiB/s. It's also not fine for any application where "just add more clients" doesn't help because the bottleneck is per-client, not aggregate.


pNFS (defined in the NFS 4.1 spec) could help with data throughput by letting the client talk directly to storage servers, Lustre-style. But pNFS does not help with metadata. Per the spec, all namespace operations -- creates, renames, lookups -- still funnel through a single metadata server. Lustre solved this with Distributed Namespace (DNE), which actually shards metadata across nodes. pNFS never did. And EFS doesn't use pNFS anyway.


## Consequence 3: Pricing


This is the one that might matter most to most people, and it's the least obvious consequence of the architecture.


When I joined EFS in 2015, I was shocked that storing a TiB on EFS cost $3,600 a year -- when the same TiB on S3 cost about $280. The conventional wisdom was that EFS was "fast storage" and S3 was "cheap storage" -- you paid a 13x premium for lower latency.


This made sense. There's a natural, almost linear relationship in cloud storage: as storage cost goes down, retrieval cost goes up. Look at the S3 storage classes -- Standard, Infrequent Access, Glacier Instant Retrieval -- and you'll see this curve clearly.


EFS originally fit this model. Retrieval was free, and throughput was proportional to stored data. But customers hated this because they'd get about 1 MiB/s by default, and deleting data would tank their performance.


So we launched Provisioned Throughput, where you paid for speed independently. Enterprise customers loved it (you can guess what they did). But it wasn't elastic, and customers didn't know how much to provision.


So we built Elastic Throughput -- you get maximum speed, and we bill per GiB transferred: **$0.03/GiB-read and $0.06/GiB-write** .


Glacier Instant Retrieval charges $0.01/GiB for retrieval. EFS Elastic Throughput charges $0.03/GiB for reads and $0.06/GiB for writes. You're paying 3-6x *more* to retrieve data from SSDs through a file system than to retrieve data from archival storage.


The storage-cost-to-retrieval-cost curve doesn't just bend -- it breaks.


You're paying a Glacier-tier access premium for SSD-backed data, because the NFS architecture is expensive to operate. Every request goes through a server that has to do coordination work, and that server is the bottleneck, so you can't amortize costs across more hardware the way S3 does.


The architecture makes cheap retrieval hard. When your protocol requires a server in the hot path for every operation, someone has to pay for that server.


At Archil, we didn't have a P&L to defend. We set pricing at $0.20/GiB stored with $0.00 for retrieval, because our protocol doesn't require a server in the hot path for most operations. We don't break the curve.


## What a better protocol looks like


The latency and scalability problems trace directly back to NFS putting a server in the critical path of every operation and funneling everything through a single connection. The pricing is a step removed -- NFS doesn't force you to charge $0.03/GiB, but it makes serving requests expensive enough that it's hard to charge less. The fix for all three isn't to optimize NFS. It's to replace it.


At Archil, we built our own protocol inspired by the Andrew File System (AFS). The core idea is simple: clients "check out" parts of the file system to get exclusive write access. While a client holds a checkout, it operates with the same semantics as a local disk -- mutations happen in memory, nothing hits the network until the client "checks in" or another client needs access. Under heavy write contention from multiple clients to the same path, the checkout/checkin overhead can exceed NFS. But most file system workloads are dominated by single-writer patterns, and for those, this closes the latency gap with EBS.


For scalability, we borrowed from Lustre's playbook. Clients talk directly to storage servers for bulk data, and we cluster the metadata service for IOPS scale-out. Because we own both the client and the server, we can do this without requiring users to install out-of-tree kernel modules -- and we can iterate on the protocol faster than waiting for a standards body.


Owning the protocol is what makes the pricing possible, too. When the server isn't in the hot path for every operation, the cost of serving a read drops dramatically.


## Where this leaves S3 Files


S3 Files launching validates that the industry is moving toward mounting object storage as file systems. AWS taking the competition between EFS and S3 off the table was the right call.


But the product inherits EFS's fundamental constraints. The NFS protocol forces a server round-trip on every mutation, caps per-client throughput at 3 GiB/s, and drives a pricing model that charges 3-6x more for retrieval than Glacier. These aren't bugs. They're architectural consequences of a protocol choice. AWS can't fix them without replacing NFS on the client side, which would mean building and supporting a new client -- something they've avoided for a decade.
