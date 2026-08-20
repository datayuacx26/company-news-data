---
schema_version: "1.0.0"
document_id: "0aa2cea81ccf2a13081d9031f1ec2c5f9aced8bb6fff9fbfe7b86a08a66ae441"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-news-import-9b9c5ebd5b82"
canonical_url: "https://www.nutanix.com/blog/data-foundation-of-ai-factory-enabling-agentic-ai-with-nus"
published_at: "2026-03-16T20:26:00+00:00"
first_seen_at: "2026-07-24T12:20:54.900145+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:bcc409999a29d3e7c752c884ceca1a78ccc79b2cd0e70663404819a7bea8ad84"
---

# The Data Foundation of the AI Factory: Enabling Agentic AI with Nutanix Unified Storage

The era of simple chatbots is over. Enterprise AI is rapidly evolving beyond model training and basic inferencing toward Agentic AI - autonomous systems capable of complex reasoning, long-running workflows, persistent memory, and real-time decision making.


But as AI agents evolve from short prompts to hours-long reasoning sessions operating on continuously changing "live” enterprise data, a major infrastructure challenge surfaces: traditional storage was never designed to be the "living memory" of AI.


## **The Two Bottlenecks of Agentic AI**


As enterprises scale from experimentation to production-grade AI factories, they hit two major walls:


1. **Inference Context Overload:** Agentic systems require persistent inference context - the AI's working memory. As sessions grow longer and models support larger context windows, this working memory rapidly exceeds limited GPU VRAM, system memory and local capacity. Inference context must be also shared across GPUs and nodes. When a session migrates to another GPU, it must retain access to prior context instantly. Without scalable, shared, low-latency storage for this long-lived memory, performance degrades, GPUs stall, and infrastructure costs can rise dramatically.
2. **Real-Time Data Inferencing:** Agents must reason over fresh enterprise data as it is created. This demands storage that doesn't just "sit there"—it must actively ingest, transform, and feed RAG (Retrieval-Augmented Generation) pipelines in near real-time. To achieve true real-time responsiveness, data processing pipelines must operate close to the data — ideally within the same storage cluster where the data resides. Without immediate access to current, trusted enterprise data, agentic AI systems become stale, less accurate, and ultimately less productive.


## **Nutanix Unified Storage: The Foundation of AI "Living Memory"**


To address these challenges, Nutanix is evolving[Nutanix Unified Storage](https://www.nutanix.com/solutions/unified-storage) into the data fabric of the Nutanix Agentic AI **** stack. Rather than acting as passive capacity, Nutanix Unified Storage becomes the high-speed data engine of the AI Factory.


### **1. Context-Memory Offload: The Shared Storage Tier**


Since Large Language Model (LLM) context-memory can become large, it is organized hierarchically in tiers for optimum performance and economics. Tiers 1-3 are local to the node and stored in GPU VRAM, system memory and local NVMe drives. Tier 4 is the foundational shared storage layer, representing the “living memory” of the AI Factory.


Nutanix is[operationalizing this fourth tier](https://next.nutanix.com/community-blog-154/context-overload-of-the-gpu-kind-how-lmcache-and-nutanix-files-storage-tame-llm-inference-costs-at-scale-part-1-45411) by providing an RDMA-enabled, high-performance, low-latency data layer capable of supporting thousands of GPUs. By integrating[LMCache – specialized cache-tiering orchestration software – with Nutanix Unified Storage](https://next.nutanix.com/community-blog-154/context-overload-of-the-gpu-kind-how-lmcache-and-nutanix-files-storage-tame-llm-inference-costs-at-scale-part-2-45412) , AI memory is seamlessly offloaded from expensive, capacity-constrained local nodes to resilient data-center shared storage.


This tiered approach to context memory helps ensure enterprises can:


- **Run massive context windows** without the risk of system crashes.
- **Support more concurrent users** on the same GPU fleet.
- **Maximize GPU utilization** , aims to reduce the "cost per token."


### **2. Real-Time Data Inferencing: Powering NVIDIA AI Data Platform**


Built with NVIDIA AI Data Platform reference design, Nutanix AI Data Platform provides capabilities that customers can use to enable AI agents to reason over enterprise data the moment it is created. By integrating NVIDIA AI Enterprise software and Milvus Vector Database directly with Nutanix Unified Storage, organizations can build continuous data pipelines that can ingest, transform, and vectorize raw data in real time. Uniquely, Nutanix allows mixing of GPU-enabled and CPU-only dense storage nodes within a single storage cluster. This “compute-adjacent” architecture brings AI to data, ensuring AI agents are always grounded in the freshest proprietary intelligence and significantly reducing the latency and friction of traditional data movement.


### **3. Maximum Velocity: NFS and S3 over RDMA**


To keep pace with high-speed AI compute, Nutanix Unified Storage aims to deliver a low-latency RDMA-enabled data path between GPUs and storage memory. As a validated NVIDIA Magnum IO GPUDirect Storage solution, Nutanix Unified Storage allows AI workloads to bypass the CPU entirely for I/O, reducing CPU overhead on both client and storage nodes while maximizing GPU utilization and lowering cost per token. Today, NFS over RDMA is supported for high-performance file access, and planned support is intended to extend this capability to S3 over RDMA for object storage. This breakthrough combines the massive scalability of object stores with ultra-low-latency direct GPU access—making Nutanix Unified Storage objects stores an ideal data foundation for large-scale AI workloads and modern AI Factories.


### **4. Enterprise Security and Governance with Nutanix Data Lens**


AI is only as trustworthy as the data it is grounded in. Nutanix Data Lens (NDL) provides the essential security and governance for the data being fed into the AI Factory, delivering proactive auditing, ransomware protection, and secure data isolation. Through a single SaaS-based portal—or running directly on a Nutanix storage cluster—NDL enables organizations to monitor, secure, and govern datasets across multiple Nutanix Unified Storage clusters, whether within a single data center or globally distributed environments. This helps ensure enterprise data remains protected as it moves through the AI lifecycle. With planned capabilities such as automated data classification and metadata tagging, sensitive information can be intelligently identified, protected, and governed end-to-end, helping organizations support their compliance efforts while safely powering agentic AI workloads.


## **Continuous Innovation**


### **NVIDIA STX and CMX Design Partner**


Nutanix is proud to be a design partner for NVIDIA STX, a modular storage reference architecture engineered for the AI fFactory. By co-designingdeveloping on the NVIDIA Vera Rubin NVIDIA Vera Rubin NVL72 architecture and leveraging NVIDIA BlueField-4BlueField-4 data processing units (DPUs), Nutanix will beis centralizing intelligent data handling directly into the storage layer. This helps ensure that GPUs, vector databases, and RAG pipelines operate as a cohesive, rack-scale system rather than disconnected components.


As a design partner for NVIDIA CMX, built on the NVIDIA STX reference architecture, Nutanix plans to build support for a new G3.5 pod-shared cache layer. This breakthrough delivers scalable capacity with ultra-high performance and seamless data sharing across GPU pods. This tiered approach to context memory helps ensure enterprises can run massive context windows, maximize GPU utilization and significantly reduce the “cost per token”.


## **Building AI Factories with Ease**


The Nutanix Agentic AI stack helps enterprises scale from experimentation to production-grade AI Factories by delivering:


- **Minimized Cost per Token:** Offloading AI context memory to the scalable, low-latency G4 storage tier dramatically improves the economics of long-context reasoning. By reducing pressure on expensive GPU memory, organizations can support larger context windows and more concurrent users while maximizing GPU utilization.
- **Improved AI Productivity:** Continuous data pipelines running directly on the storage cluster bring AI to the data. This minimizes costly data movement and helps ensure agentic AI systems always operate on the freshest enterprise data as it is created.
- **Linear Scalability:** Scale AI factory performance and capacity linearly with dense, high-performance GPU Direct Storage support for both file and object workloads, enabling thousands of AI agents to run with consistent performance.
- **Enterprise Security and Governance:** With NDL providing global visibility and control across datasets, organizations benefit from built-in security, auditing, and governance throughout the entire AI lifecycle.
- **Future-Proofed Architecture:** Deep alignment with the NVIDIA STX and CMX roadmaps help ensure AI infrastructure is optimized for next-generation platforms such as NVIDIA Vera Rubin NVL72, and BlueField-4.


## **The Bottom Line**


Nutanix Unified Storage is a core component of the Nutanix Agentic AI stack and the data foundation of the modern AI factory. By bringing AI closer to data and enabling scalable AI “living memory,” Nutanix is transforming storage from passive capacity into an intelligent, high-speed data engine built for the Agentic AI era.


In the race to operationalize agentic systems, the bottleneck is no longer just silicon—it’s the data path. The real question for modern enterprises is no longer how many GPUs they have, but whether their data foundation can keep pace with Agentic AI at scale.


With Nutanix Unified Storage, it can.
