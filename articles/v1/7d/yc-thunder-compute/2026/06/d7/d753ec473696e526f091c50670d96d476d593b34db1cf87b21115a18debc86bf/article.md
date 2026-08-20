---
schema_version: "1.0.0"
document_id: "d753ec473696e526f091c50670d96d476d593b34db1cf87b21115a18debc86bf"
company_key: "yc-thunder-compute"
company: "Thunder Compute"
source_id: "yc-thunder-compute-news-import-c1dcc13dd65c"
canonical_url: "https://www.thundercompute.com/blog/why-network-based-gpu-virtualization-is-the-future"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-22T16:40:14.192629+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:549ea078784091cf3b0b9eb249b6c7a8f4ed5e7a4ebdae5a49fa65b7b519e59b"
---

# GPU Virtualization: Approaches and Tradeoffs

## Introducing GPU virtualization


Virtualization is a concept in computer science for **creating virtual representations of physical hardware** . While virtualization is commonly associated with CPUs, such as Intel VT-x, it extends to other domains, including GPUs.


Virtualization is **essential for efficient hardware resource allocation** , and this is more relevant than ever with massive hardware buildouts for AI. However, the term is loaded and often misunderstood, especially when applied to GPUs, where the term can have multiple meanings.


## Existing types of GPU virtualization


GPU virtualization currently exists in three main forms:


- **Single-node GPU sharing**
- Dedicated **GPU passthrough**
- **Network-based GPU pooling** , which is Thunder Compute's approach


The first two operate within a single physical server and are widely used today. Thunder Compute is pioneering the third approach, which operates across a cluster of servers.


### Single-node GPU sharing, such as NVIDIA MIG


The current class-leading approach to sharing a single physical GPU is NVIDIA's MIG, or Multi-Instance GPU, which partitions a GPU into multiple smaller virtual GPUs. MIG allows several workloads to simultaneously use the same GPU, each getting a fixed partition of the compute cores and memory.


GPU partitioning generally falls into two categories: static and dynamic. Static partitioning splits a GPU into fixed slices, each guaranteed a set share of compute and memory, with the drawback that each workload can only use a fixed fraction of the GPU. Dynamic partitioning is more efficient as it lets workloads draw on the full pool of GPU resources as needed, adjusting allocations on the fly.


Modern AI workloads need more, not less, compute, so GPU partitioning is less common than other types of virtualization in production clusters.


### Dedicated GPU passthrough, such as NVIDIA vGPU


GPU passthrough assigns an entire physical GPU or MIG partition to a single workload. While this does not split the GPU, it is considered virtualization because it allows a VM to control the GPU through a virtual interface.


GPU passthrough is the industry standard for cloud environments, and allows providers to use orchestration software like Kubernetes, Slurm, and various hypervisors to manage workloads.


That said, GPU passthrough does not provide any kind of efficiency gain compared with bare metal allocation, and is used in conjunction with other types of virtualization.


### A new approach: network-based virtualization


Network-based GPU virtualization creates more flexibility within a data center by allowing any workload to use any GPU on the same network fabric. This flexibility enables efficiency: a scheduler is aware of all workloads and all GPUs, and can dynamically assign workloads to GPUs in a way that maximizes utilization across the fleet.


This enables dramatically more workloads to fit on a fleet of GPUs, filling in gaps that would otherwise have been underutilized. Conceptually, this type of GPU virtualization is very similar to storage virtualization, like Ceph or Storage Area Networks.


The key unlock to enable this virtualization is the ability to extend physical PCIe connections with virtual connections over a network. There is a latency impact to doing so, which varies by workload, but is often far outweighed by cluster-scale efficiency improvements.


## The future of GPU virtualization


In production systems you often see multiple types of virtualization used together. For example, MIG is used to slice up GPUs into smaller shapes, which are then passed to a network virtualization layer and allocated to workloads.


As network-based virtualization continues to improve and cluster utilization becomes increasingly important for cloud economics, we expect this will become standard, similarly to network-based storage virtualization.


> If you manage a fleet of GPUs and would like to learn more about network virtualization for your cluster,[contact us](https://www.thundercompute.com/contact) .
