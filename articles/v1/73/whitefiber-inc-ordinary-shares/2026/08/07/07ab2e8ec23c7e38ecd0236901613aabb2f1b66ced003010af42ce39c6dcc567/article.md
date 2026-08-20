---
schema_version: "1.0.0"
document_id: "07ab2e8ec23c7e38ecd0236901613aabb2f1b66ced003010af42ce39c6dcc567"
company_key: "whitefiber-inc-ordinary-shares"
company: "WhiteFiber Inc."
source_id: "whitefiber-inc-ordinary-shares-news-import-44ec15aad2d1"
canonical_url: "https://www.whitefiber.com/blog/re-engineered-network-ai-clusters-deterministic-ethernet"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-16T13:51:15.419840+00:00"
fetched_at: "2026-08-16T13:51:18.150276+00:00"
content_hash: "sha256:c912b9e01a4935a305400dac6dabf26aa61e9ec4cce552db9ac8852e6756131a"
---

# Why WhiteFiber Re-Engineered the Network for AI Clusters

When we design large AI clusters at WhiteFiber Cloud, we start with a simple question:


‍


Why are so many GPUs idle?


‍


Over the past year, we’ve worked with teams running state-of-the-art NVIDIA H200 clusters that were delivering 40–50% utilization. The hardware was not the problem. The models were not the problem. The network was.


‍


As models grow and parallelism scales, the network becomes the system. All-reduce, all-gather, checkpointing, storage reads, telemetry, every phase of training stresses east-west traffic. When congestion appears inside the fabric, GPUs stall. Tokens per second drop. Training time expands. Cost per model increases.


‍


We wanted to understand how close we could get to theoretical bandwidth while maintaining Ethernet’s flexibility and scale.


‍


‍


‍


## The Technical Challenge


‍


AI clusters are different from traditional data center workloads.


‍


They generate:


‍


Large volumes of small packets


Bursty, synchronized traffic patterns


Sustained all-to-all communication


‍


In standard Ethernet fabrics, even well-tuned ones, congestion control mechanisms are reactive. Techniques like PFC (Priority Flow Control) and ECN (Explicit Congestion Notification) help, but they introduce operational complexity and can struggle under synchronized load. Head-of-line blocking becomes visible at scale. Performance becomes variable.


‍


InfiniBand solves many of these issues through deterministic behavior and tight control of traffic flow. We deploy InfiniBand in environments where it is the right answer. But some customers with more diverse and complex environments require Ethernet interoperability, multi-tenancy flexibility, or broader vendor compatibility.


‍


We wanted an Ethernet architecture that behaved deterministically under AI load.


‍


‍


‍


## Why Scheduled Fabric


‍


We partnered with DriveNets to implement a scheduled fabric Ethernet design. The core idea is simple but powerful:


‍


- Instead of reacting to congestion, schedule traffic to avoid it.


‍


Packets are broken into fixed-size cells. These cells are scheduled across the fabric in a connection-oriented manner. The result is a lossless, deterministic system where the fabric behaves as a coordinated whole rather than as independent switches.


‍


From an architectural perspective, this shifts the network from probabilistic fairness to controlled allocation.


‍


The benefits are measurable:


‍


- **Congestion is controlled inside the fabric**
- **Links operate near peak efficiency**
- **No specialized NICs are required**
- **Standard Ethernet overlays remain supported**


‍


For large GPU clusters, this matters.


‍


‍


‍


## What We Built


‍


On top of this architecture, we deployed a 512-GPU NVIDIA H200 cluster with HPE, backed by a high-performance storage tier and a scheduled fabric Ethernet core capable of 400 GB/s theoretical bandwidth between buses.


‍


We intentionally kept tuning minimal. We wanted to measure real-world performance, not lab-only conditions.


‍


Using NCCL benchmarks, we measured an average of 390 GB/s, 97.5% of peak theoretical bandwidth.


‍


We then ran end-to-end TorchTitan benchmarks using a Llama 3.1 8B model across 32 and 64 GPU configurations.


‍


Two metrics mattered:


‍


Tokens per second (TPS)


Model FLOPS Utilization (MFU)


‍


TPS remained stable as we scaled. MFU reached 63%, exceeding prior published Hopper-class results.


‍


The result was not just high bandwidth. It was sustained, predictable performance under real training workloads.


‍


‍


‍


## What We Learned


‍


**Three lessons stand out:**


‍


**Network efficiency matters.**


When performance plateaus or diminishes, many teams scale compute. In many cases, the more effective lever is network determinism.


**Reactive congestion control has limits at AI scale.**


As synchronization increases, predictable scheduling outperforms probabilistic fairness.


**Architecture matters more than components.**


GPUs, storage, topology, orchestration, and networking must be designed together. Optimizing one layer in isolation yields diminishing returns.


‍


We do not view this as Ethernet versus InfiniBand. Both have strengths. Our responsibility is to understand the tradeoffs and design for the workload.


‍


‍


‍


## Why This Matters


‍


AI infrastructure investment is accelerating. As clusters scale from tens to hundreds to thousands of GPUs, inefficiencies compound quickly. A 10% drop in utilization at 512 GPUs is manageable. At 10,000 GPUs, it becomes material.


‍


Deterministic networking reduces variance. Reduced variance improves planning. Improved planning reduces cost.


‍


For enterprises building long-lived AI infrastructure, predictability is not optional.


‍


‍


‍


## Where We Go Next


‍


We will continue benchmarking across both scheduled fabric Ethernet and InfiniBand deployments. We will continue publishing data. And we will continue designing heterogeneous environments where customers can select the right interconnect for the job.


‍


Our goal is simple:


‍


- **Make GPUs do work.**


‍


When the network stops being the bottleneck, clusters behave the way they were meant to.


‍


That is the work.
