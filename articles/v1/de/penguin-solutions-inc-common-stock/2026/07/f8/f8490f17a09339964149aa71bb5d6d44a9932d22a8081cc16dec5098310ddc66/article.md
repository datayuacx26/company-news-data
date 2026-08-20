---
schema_version: "1.0.0"
document_id: "f8490f17a09339964149aa71bb5d6d44a9932d22a8081cc16dec5098310ddc66"
company_key: "penguin-solutions-inc-common-stock"
company: "Penguin Solutions Inc."
source_id: "penguin-solutions-inc-common-stock-news-import-f9c47211aa5e"
canonical_url: "https://www.penguinsolutions.com/en-us/resources/blog/why-ai-needs-cxl"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-25T19:00:09.417744+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:5ee6c2ddd3d6b036f178ec9578635dfa97eb3bc8f5b925d9eecb4161fe9ad692"
---

# From GPUs to Memory Pools: Why AI Needs Compute Express Link (CXL)

Artificial intelligence (AI) is entering an era of unprecedented scale. From training trillion-parameter large language models (LLMs) to enabling real-time multimodal inference, AI workloads are reshaping the very foundations of data center infrastructure. While GPUs and accelerators have become the face of AI, a critical bottleneck lies behind the scenes: memory, bandwidth, latency, and scalability challenges often determine the success or limits of AI systems. This is where Compute Express Link (CXL) steps in, offering a transformative solution.


## The Memory Bottleneck in AI


These are some of the key items creating the memory bottleneck in AI:


- Training foundation models require enormous memory capacity, often exceeding what is available in a single GPU.
- Inference at scale demands rapid access to large datasets without duplicating memory across GPUs.
- Traditional architectures force CPUs, GPUs, and accelerators to operate in silos, creating inefficiencies.


***Figure 1.*** *A diagram comparing traditional siloed memory (CPU, GPU islands) vs. CXL-enabled pooled memory (Source: Rambus)*


As per figure 1, in siloed architectures, each CPU or GPU is tied to its own isolated memory, often leaving capacity underused. With CXL pooling, all processors can access a unified shared memory space. This shift enables flexible scaling, better utilization, and improved performance for AI and data-intensive workloads.


## How CXL Comes to the Rescue


Compute Express Link is an[open-industry standard interconnect](https://www.penguinsolutions.com/resources/blog/what-is-cxl-memory-expansion) created to address the growing performance and scalability demands of modern workloads, especially in AI and high-performance computing. Unlike PCIe (Peripheral Component Interconnect Express), which is optimized for general-purpose I/O, CXL is specifically designed to connect CPUs, GPUs, accelerators, and memory devices with low latency and full cache coherency, ensuring that data remains synchronized across heterogeneous processors. By extending beyond traditional device-to-host communication, CXL enables a unified fabric where computing and memory resources can be shared seamlessly across an entire system or even a cluster of servers.


One of the most impactful capabilities CXL introduces is[memory pooling](https://www.penguinsolutions.com/expertise/integrated-memory-expansion-pooling) , which allows memory to be dynamically allocated to different devices based on workload needs rather than being statically bound to a single CPU or GPU. This eliminates stranded capacity and enables higher utilization of memory resources. Another key feature is tiered memory, where high-performance local DRAM can be complemented by larger pools of CXL-attached memory—such as DDR5 or persistent memory—delivering a balance of speed, capacity, and cost efficiency. Finally, CXL is a cornerstone for composable infrastructure, where resources like compute, accelerators, and memory can be assembled and reassembled in real time to match shifting workload demands, from training massive, large language models to running latency-sensitive inference tasks.


In short, CXL represents a shift from static, siloed architectures towards flexible, fabric-based computing, paving the way for next-generation AI and data-intensive systems.


***Table 1.*** *Comparison table between PCIe vs. CXL in terms of latency, coherency and scalability for AI infrastructure:*


Features PCIe Technology CXL Technology


Primary Use Case High-speed I/O for peripherals (GPUs, NICs, SSDs) Memory and accelerator interconnect (CPU ↔ GPU ↔ Memory


Latency Higher latency (optimized for throughput, not cache coherency) Low latency with cache coherency for memory sharing


Memory Coherency No built-in coherency across devices Full cache coherency between CPU, GPU, accelerators, and memory


Scalability Point-to-point, limited memory sharing Memory pooling, disaggregation, and fabric-based scalability


Data Movement Requires copies between device memory spaces Direct memory access across heterogeneous devices


Flexibility Fixed per-device memory Dynamic, composable infrastructure (shared memory pools)


AI/ML Impact Memory duplication overhead, inefficient scaling Efficient training/inference, reduced duplication, scalable LLMs


## Why CXL Matters for AI Infrastructure


1. Large Language Models – CXL enables memory pooling across nodes, reducing the need for costly memory duplication.
2. Multi-GPU Inference – shared memory pools simplify deployment and reduce infrastructure overhead.
3. Composable AI Data Centers – instead of over-provisioning memory, CXL allows flexible scaling.


***Figure 2*** *. Flow diagram showing how memory pooling supports multi-GPU LLM training.*


As per figure 2, CXL memory pooling allows multiple GPUs to share a unified memory pool, enabling efficient scaling of large language models.


## The Road Ahead: CXL 3.0 and Beyond


With CXL 3.0, the industry is moving beyond incremental improvements to a fundamental shift in data center architecture. By introducing fabric topologies, multi-level switching, and coherent memory sharing across hosts,[CXL 3.0 allows entire racks of servers to function as a unified, flexible AI fabric](https://www.penguinsolutions.com/en-us/resources/blog/cxl-30-vs-cxl-20-key-differences-and-benefits) . This is especially significant for AI workloads such as large language models, where traditional GPU islands are constrained by memory limits and forced into complex model parallelism.


With shared, coherent memory pools accessible across GPUs, training becomes faster, duplication is reduced, and larger models can be supported more efficiently. Hyperscalers, cloud providers, and HPC facilities are already piloting CXL-enabled deployments, with vendors from Intel and AMD to Samsung and HPE building support into their roadmaps. Looking further ahead, CXL is expected to evolve toward even faster interconnects, finer-grained composability, and AI frameworks natively optimized for pooled memory.


In summary, CXL 3.0 is a pivot point—from server-centric computing to fabric-centric AI infrastructure—laying the foundation for the next generation of scalable AI and LLM systems.


***Table 2.*** *Timeline of CXL evolution (1.0 → 2.0 → 3.0) with AI-specific use cases:*


Timeline Point CXL Version Evolution Highlights AI-Relevant Use Cases


2019 CXL 1.0 First release with basic coherency and memory protocols. Enables unified memory access by CPU & device; foundational for heterogeneous compute.


2020–2021 CXL 2.0 Introduced memory pooling, switching, and persistent memory. Supports flexible memory pooling—critical for large model LLM training and inference.


2022–2023+ CXL 3.0 Advanced fabric architecture, coherent memory sharing, multi-host scaling. Designed for disaggregated AI clusters, multi-GPU sharing, and coherent memory access at rack scale.


## CXL Provides the Missing Link


AI’s future depends on more than GPUs—it requires rethinking how memory is connected, shared, and scaled. **CXL provides the missing link, transforming isolated resources into a coherent, flexible AI infrastructure.** For the AI infrastructure community, CXL represents not just technology, but a *foundation* for building the next generation of data centers.


---


Originally published: *EE|Times* ,[From GPUs to Memory Pools: Why AI Needs Compute Express Link (CXL)](https://www.eetimes.com/from-gpus-to-memory-pools-why-ai-needs-compute-express-link-cxl/) , October 27, 2025


‍


SMART Modular Technologies helps customers around the world enable AI and high-performance computing (HPC) through the design, development, and advanced packaging of integrated memory solutions. Our portfolio ranges from today’s[leading edge memory technologies like CXL](https://www.penguinsolutions.com/products/smart-modular-cxl-based-solutions) to standard and legacy DRAM and Flash storage products. For more than three decades, we’ve provided standard, ruggedized, and custom memory and storage solutions that meet the needs of diverse applications in high-growth markets.[Contact us today for more information](https://www.penguinsolutions.com/contact-us) .
