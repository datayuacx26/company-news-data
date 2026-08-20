---
schema_version: "1.0.0"
document_id: "5621ff030f0095bb8d26451d9cabda36e2f679d517fca71bc4d563ff5b660171"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-6ed493c8d31a"
canonical_url: "https://archil.com/article/deepseek-3fs-distributed-file-system-ai"
published_at: null
first_seen_at: "2026-08-09T19:14:46.011593+00:00"
fetched_at: "2026-08-09T19:14:47.557372+00:00"
content_hash: "sha256:bb3aae3d327c5a2110e08a6c3ad990e56d34f1c2bedadd5e49b38fc260bbc7b6"
---

# When should I be using DeepSeek 3FS for AI?

[DeepSeek 3FS](https://github.com/deepseek-ai/3FS) (Fire-Flyer File System) is a high-performance distributed file system designed for AI training and inference workloads. In early 2025, DeepSeek AI open-sourced it during their Open Source Week event, but the product was battle-tested internally since the company's inception.


Now that 3FS is open-sourced under the permissive MIT License, if and when should you use it?


## What is a Distributed Filesystem?


Before we get into what 3FS is, let’s establish what a distributed filesystem is.


A local filesystem is something that everyone is familiar with. It’s the mechanism that stores your files virtually on your phone, laptop, or any device. In a local file system, the files live on the device and can be accessed locally. Meanwhile, distributed filesystems offer a similar mechanism as a local filesystem, but the actual files are spread out across multiple devices or servers and are accessible via network.


Distributed filesystems are everywhere and essential for applications that store more data than a single device can hold. There are several advantages to the approach:


- **Scalability and Storage Capacity:** Distributed file systems can grow horizontally by adding more nodes to a network allowing you to scale to massive datasets
- **Redundancy:** Since data is typically replicated across multiple nodes if one node fails, a different node can pick up the slack. Redundancy also prevents data loss; if a device is corrupted, the data stored on it is stored on another device
- **Performance:** Multiple parts of the file system can be accessed in parallel without bottlenecks since files are spread across multiple machines. This overall speeds up reads and writes significantly and does not limit throughput and IOPS to the limits of a single device


However, there are downsides to distributed filesystems:


- **Complexity:** Setting up and maintaining a distributed file system requires significant technical expertise. The architecture is much more complicated than managing a single server, involving coordination and synchronization between nodes
- **Consistency Challenges:** Keeping data synchronized across multiple locations is difficult. You often face trade-offs between consistency, availability, and partition tolerance (the CAP theorem). 3FS chooses CP (consistency and partition tolerance).
- **Network Dependency and Increased Latency:** While distributed systems can have higher throughput than local filesystems, time to first byte and overall latency can be higher especially for smaller operations since retrievals go across a network subjecting them to latency.


Regardless, for devices with storage constraints, these disadvantages are overwhelmed by the sheer increase in capacity that distributed file systems provide.


## How is 3FS unique?


There are many distributed file systems, including some built by massive technology companies. Meta built[Tectonic](https://engineering.fb.com/2021/06/21/data-infrastructure/tectonic-file-system/) , Google created[Colossus](https://cloud.google.com/blog/products/storage-data-transfer/a-peek-behind-colossus-googles-file-system) , and Microsoft developed[ADLSv2](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction) . There are also ample open source alternatives, such as[Ceph](https://ceph.io/en/) and[Lustre](https://www.lustre.org/) .


DeepSeek built their own filesystem to prioritize what they do most: run and train AI agents. Designing a bespoke filesystem for AI was a worthy challenge: minor efficiency boosts add up at scale. For example, DeepSeek optimized 3FS for random reads since that is common for AI workflows. You can interact with 3FS through a native client or FUSE client, but the performance gains for random reads are achieved with the native client.


### What is CRAQ?


One of the most critical design tenets of 3FS is the CRAQ protocol.


CRAQ (Chain Replication with Apportioned Queries) is a replication protocol that allows 3FS to achieve strong consistency and high read performance. In traditional chain replication, all reads go through a single node (the tail), creating a bottleneck. CRAQ improves on this by allowing reads from any node in the replication chain.


Here's how it works:


1. Writes still propagate sequentially through the chain to ensure consistency, but reads can be served by any replica.
2. If a replica has pending writes that haven't fully propagated, it queries the tail node to ensure it returns the most recent committed version.


This system allows for 3FS to distribute read load across all replicas while guaranteeing that clients always see the latest data. There is a trade-off, however: writes remain slower since they must traverse the entire chain. For AI workloads where datasets are read frequently but updated infrequently, this is an acceptable compromise that enables both strong consistency and high read throughput.


CRAQ is a major part of what separates 3FS from other distributed systems and has some major tradeoffs in its design.


### What are the advantages of 3FS?


- **Scale-Out Metadata Service:** The metadata service for 3FS can be spread across multiple machines, preventing metadata service from being overwhelmed
- **Native FUSE Client:** Allows you to interact with 3FS as if it was your local file system. Makes reading and writing data far simpler than learning a new interface and paradigm
- **Native Client for Resource Intensive Workflows:** If you want to squeeze as much efficiency as possible from 3FS the native client allows more optimizations than the FUSE client with its zero-copy API
- **Strong Consistency:** Using CRAQ, 3FS is able to offer strong consistency. The cost of this is slower writes, but for read heavy workloads this is an acceptable trade-off


### What are the disadvantages of 3FS?


- **Narrow Workload Optimization:** 3FS is optimized for AI inference and training workloads. It performs well with lots of large files and read heavy workloads. If your workload is anything else, another distributed file system is likely better for you
- **Slow Write Performance:** Since 3FS offers strong consistency with CRAQ, the write performance suffers. For write intensive workloads 3FS is likely not the best choice
- **Operational Complexity:** 3FS for now is completely self managed which means you need to manage the metadata service, storage nodes, and networking yourself. Even for someone with a distributed systems background this can be operationally burdensome
- **Tooling Immaturity:** Even though DeepSeek has been using 3FS for years, it has a significantly smaller community and third party tools than more popular options like Ceph or Lustre.


3FS has an extremely narrow use case and comes with heavy operational complexity. It is designed to squeeze a bit extra efficiency out of a distributed file system that more general purpose solutions do not optimize for.


## When Should You Use 3FS?


The use cases for 3FS are limited since it is optimized for read-heavy low-write workloads. It works incredibly well for shared dataset storage across ML pipelines, AI inference workloads, and large scale AI Model training. However, unless you are running extremely large AI workloads and can stomach the operational burden of 3FS, a more general purpose solution could work better for you.


For a more resilient option with S3 synchronization, Archil offers a middle-ground solution. Archil stores data in S3 and caches it while in use, offering the same random read optimizations as 3FS once the data is hot in the cache. However, Archil is *also* strongly consistent. For AI training and inference workloads where data is often idle for long periods, object storage can store data at rest more cheaply and reliably than a self hosted system.


To test Archil, you can sign up[here](https://accounts.archil.com/sign-in?redirect_url=https%3A%2F%2Fconsole.archil.com%2Fdisks) .
