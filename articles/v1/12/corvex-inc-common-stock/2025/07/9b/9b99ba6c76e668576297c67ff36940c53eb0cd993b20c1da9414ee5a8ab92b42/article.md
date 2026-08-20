---
schema_version: "1.0.0"
document_id: "9b99ba6c76e668576297c67ff36940c53eb0cd993b20c1da9414ee5a8ab92b42"
company_key: "corvex-inc-common-stock"
company: "Corvex Inc."
source_id: "corvex-inc-common-stock-rss-5d3752083407"
canonical_url: "https://www.corvex.ai/blog/glossary-of-key-terms-h200-b200-edition"
published_at: "2025-07-14T00:00:00+00:00"
first_seen_at: "2026-07-26T12:16:19.295968+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:9d89e93f6983877b2947f6750a70af514346916ee15f7d5c516a46f39d6c14d9"
---

# GPU Leases 101: Glossary of Key Terms (H200 / B200 Edition)

Bare-Metal Server A physical machine delivered without a hypervisor. You get root control, predictable performance, and strong isolation because nothing is shared with other tenants. Burst Capacity Extra GPUs a provider can add to your cluster on short notice for temporary spikes in demand. CapEx vs OpEx Capital expenditure (CapEx) is money spent to buy hardware outright. Operating expenditure (OpEx) is the pay-as-you-go cost you record when renting resources. Cluster A group of servers connected by a high-speed fabric and managed as a single unit. Clusters let you train or serve large models across many GPUs. Confidential Compute Hardware and firmware features that keep data encrypted while it is processed. Common in finance, health, and R&D use cases where IP or personal data must stay private. Container Image A packaged runtime environment (code, libraries, dependencies) that ensures consistent behavior across nodes. Dedicated Host A server reserved entirely for one customer. No resources are shared, even if the machine is virtualized. Egress Outbound data transferred from the provider to the public internet or another cloud. Often metered separately from compute time. Elasticity The ability to scale GPU capacity up or down automatically to match workload demand. HBM High Bandwidth Memory located on the GPU package. Faster than system RAM and critical for large transformer models. HIPAA-Ready Indicates the provider supports an architecture that can meet HIPAA’s administrative, physical, and technical safeguards when you sign a Business Associate Agreement (BAA). Host Type The class of server you rent. Examples: single-GPU tower, dual-socket server with four B200s, or eight-GPU HGX H200 node. Ingress Data entering the provider’s network from outside sources. Usually free, but always verify. InfiniBand A low-latency, high-throughput network fabric commonly used to link GPUs inside a cluster. Enables fast parameter exchange during distributed training. Node A single server within a cluster. May contain one or many GPUs. On-Demand Lease A pay-by-the-hour or pay-by-day rental with no long-term commitment. Highest flexibility, typically higher rate than reserved terms. PCIe vs NVLink Two interconnect standards that link GPUs to CPUs or to each other. NVLink offers higher bandwidth and lower latency than PCIe. Provisioning Time The period between requesting a resource and having it ready. Fast provisioning means your workload can start sooner. Reserved Capacity A commitment to rent specific GPU nodes for weeks or months. Locks supply, often at a discount versus on-demand. RAG (Retrieval-Augmented Generation) An inference pattern that fetches external documents to ground language model responses in fresh or proprietary data. SLA (Service Level Agreement) The contractual uptime or performance guarantee. May include replacement time for failed nodes, network latency targets, or support response windows. System RAM Memory attached to the server’s CPU. Holds datasets, intermediary artifacts, or operating-system tasks separate from GPU HBM. Utilization The percentage of time a GPU is actively computing. High utilization means you are getting value for every rented hour. vGPU (Virtual GPU) A software layer that partitions a physical GPU into multiple virtual slices. Useful for light inference or development workloads that do not need an entire card.


## How to Use This Glossary


Keep it handy when reading marketing pages, architecture diagrams, or lease contracts. If a provider introduces a new acronym, map it back to these core ideas before making buying decisions.


**Need help choosing between H200, B200, or a mixed cluster?**
The[Corvex.ai solutions team](https://www.corvex.ai/talk-to-us) can translate your requirements into the right host type and commitment model—no jargon required.
