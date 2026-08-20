---
schema_version: "1.0.0"
document_id: "34b0c61f3fafdd019f35e08cece316e18b0b819e84410a9d5d19d046d329fe20"
company_key: "western-digital-corporation-common-stock"
company: "Western Digital Corporation"
source_id: "western-digital-corporation-common-stock-rss-a1b03adbb2f1"
canonical_url: "https://blog.westerndigital.com/ai-storage-vs-memory-infrastructure/"
published_at: "2026-05-14T22:00:25+00:00"
first_seen_at: "2026-07-22T05:42:54.619060+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:199ed8ecd5cb109a82112e40c054d745ce72d0c9b4dd32d195d5cd3a796b39be"
---

# The Difference Between Storage and Memory in AI Workflows

May 14, 2026


8 min read


[Technology](https://blog.westerndigital.com/category/technology/)


# The Difference Between Storage and Memory in AI Workflows


[Brad Warbiany](https://blog.westerndigital.com/?guest_author=brad-warbiany)


When I was a teen, which to those who know me will be no surprise, I was the only computer-literate person, and surely the only computer geek, in the household. I recall telling my father at the time that we needed to upgrade the RAM in our PC from 4MB to 8MB. I recall his response:


“I just bought more megabytes! How do we need them again?”


Well, he was right. He’d just bought a 340MB hard drive for our PC. So he’d, indeed, just purchased more megabytes. But he didn’t understand that memory and storage, despite being denominated in the same metric, were two completely different things. He thought megabytes were megabytes. And yes… we were talking about MB then, not GB or TB… *I am that old* .


Fast-forward 30 years, and the same confusion persists, now with real implications for how AI infrastructure is designed. Memory and storage are still routinely conflated, often leading to flawed assumptions in system design. In investment circles, WD is oft-considered alongside “memory stocks” despite HDDs being overwhelmingly used as storage. This is also consistent with how we think: we think of our “memories” as our stored experiences, whereas we think of “short-term memory” as a sort of mental scratch pad where we’re holding things we need in the next few seconds/minutes.


So perhaps we need to demystify this, to recognize how storage and memory exist in the modern data center and today’s AI workflows.


## Ephemeral memory vs durable storage


The first breakdown is volatility. And although I mentioned stocks, I’m not talking about the VIX. I’m talking about whether your memory is volatile or non-volatile. This is the first breakdown between memory and storage:


- Memory is by definition ephemeral—upon power loss, the contents of memory disappear forever. It is what we call “volatile,” meaning it does not persist in a system long term under all conditions.
- Storage is by definition durable—upon power loss, the data is retained. We often refer to flash as non-volatile memory (NVM), but hard disk drives or tape are also NVM. The expectation is that what is stored will remain stored and accessible regardless of conditions.


Non-volatile memory or storage can be used, in many cases, as memory. But it’s very rare that volatile memory can be used as storage. At a fundamental level, non-volatility is the prerequisite for “storage.”


## Storage and memory in the AI data center


In a modern data center, using AI as an example, there is a simple heuristic to distinguish between memory and storage. This distinction becomes especially important in AI systems, where data movement and access patterns define performance at scale.


Compute architectures are connected by extensive networking, enabling compute to communicate both with other compute resources and with data. A useful heuristic is that resources on the “compute” side of the network function as memory or cache, while resources beyond the network function as storage.


As often happens with heuristics, this breaks down under close examination. In many cases, with modern fabrics like NVMe-oF™ and NVIDIA’s GPUDirect® architecture, “storage” resources that are used as memory or cache are attached via ethernet fabrics. The rise of Compute eXpress Link (CXL) is a way to attach disaggregated compute and memory across an ethernet fabric. These may be on the opposite sides of the network, but are still dealt with as volatile memory resources.


On the opposite end, in a storage hierarchy there are many uses of DRAM or flash-based caches that are “working memory” to accelerate storage system performance. But from the standpoint of the compute architecture, these resources do not function as “memory.” They’re not directly accessible from the GPU/CPU.


So, from the standpoint of an AI data center, we can separate the two: memory is on the compute side of the network, and storage is on the opposite side of the network.


## Performance vs data durability


This distinction directly drives architectural decisions. If memory is treated as volatile, and backed up by storage, it allows for a systems architect to prioritize things in each tier very differently.


In the storage tier, durability and data integrity are paramount. This means that the emphasis is placed on making sure that data is protected across multiple failure domains. Often, this means a combination of erasure coding with geographic replication, to ensure that data loss is nearly inconceivable.


At scale, this is usually termed software-defined storage (SDS), which typically requires compute resources to manage data placement. For erasure coding, data is split into “shards” with overhead for parity. For example, in a 10+3 erasure code, a 1MB object or file would be split into 13 shards totaling 1.3MB. These would be distributed across multiple storage devices, with only 10 of the 13 being necessary to recreate the original data. Thus, the erasure code allows for as many as 3 of the 13 devices to fail without data loss. This is fundamentally similar to RAID, but is more efficient at large scale as it does not require a fixed “stripe” across a static set of identical HDDs. In a large deployment, a set of AI training data measured in petabytes (PB) would be distributed across hundreds or even thousands of individual HDDs.


In a memory or caching tier, performance is paramount. When caching, the cached data is already secured in the storage tier, where data integrity is assured. Data is moved to the caching tier based on expected access patterns. In AI, this can be a temporary buffer for AI training data, or can be the critical user context data needed in KV cache for inference.


In the caching layer, the system architect can deliberately forego data protection schemes because they’re unnecessary; the data is already protected elsewhere. Thus, the cost, overhead, and performance implications of protecting data integrity are unnecessary. And forgoing these protections reduces latency and increases input-output operations per second (IOPS).


From an architectural standpoint, this means that an AI storage architect is basing their decisions on data durability, failure domain, and TCO, whereas an AI memory/cache architect is basing their decisions on performance and latency, and total QoS of their AI software stack.


## Throughput vs IOPS/latency


In most AI workloads, there is one cardinal sin: do not starve the GPU! The GPUs are high-value components of every AI data center, and they only pay for themselves when they’re generating tokens. However, depending on which side of the network you reside, how you ensure you serve the GPU is very different.


For AI training, the GPU-adjacent tier of DRAM and flash exists as a buffer for the training data being served by the storage layer. Throughput is the goal, but these memories used as a buffer ensure that any upstream bottlenecks don’t starve the GPU by pulling data forward before the GPU requires it. In inference workloads, the situation flips. Because prompts are random and unpredictable, the demands upon the model and the KV cache do not lend themselves to sequential throughput. Latency and IOPS dominate. Thus, DRAM and flash dominate. And the flat storage architecture in this cache layer both increases performance while reducing overhead and cost.


However, on the storage side of the network, there are multiple layers of networking and data center infrastructure between storage and GPU. Each of these layers can add latency and bottlenecks to getting data from the storage system to the compute system. But the way storage is architected for resiliency becomes a benefit. The very same architectures that ensure data resilience—erasure coding and replication—increase the available throughput of data.


For AI training data stored in a distributed manner for resiliency purposes, the training set may be spread across hundreds or thousands of HDDs. While each individual HDD may not have latency or performance on par with SSDs, the aggregate throughput of the entire hardware pool can keep the caching layer full so the GPU never starves. For inference, the storage side of the network is accessed to pull relevant user context into the KV cache as soon as a new session is opened. During inference, data is mostly served by DRAM and flash, while the storage layer remains ready in case data either doesn’t exist, or fails to be read, in the caching layer.


## The demands of the layer determine the architecture


At AI scale, the distinction between storage and memory becomes critical. Systems are no longer constrained by compute alone, but by how efficiently data can be stored, protected, and accessed when and where it is needed.


Understanding the difference between memory and storage is not just a technical detail—it’s foundational to building AI infrastructure that can scale sustainably.


[Artificial Intelligence](https://blog.westerndigital.com/tag/ai/)[Data Center](https://blog.westerndigital.com/tag/data-center/)[Flash Memory](https://blog.westerndigital.com/tag/flash-memory/)[HDD](https://blog.westerndigital.com/tag/hdd/)
