---
schema_version: "1.0.0"
document_id: "5fb3dd7940d28f22e16d5355cbef2667c40b2ef60823e2de67d0d9dcc082d380"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-news-import-9b9c5ebd5b82"
canonical_url: "https://www.nutanix.com/blog/orchestrating-and-securing-ai-factory-infrastructure-with-nutanix-agentic-ai"
published_at: "2026-03-16T20:26:00+00:00"
first_seen_at: "2026-07-24T12:20:54.900145+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:1457c865ef410c8bfa60bcbdff850ea95fb8d5ba791a746a9c9758ac880f4ea7"
---

# Orchestrating and Securing your AI Factory Infrastructure with Nutanix Agentic AI

The promise of AI is massive, but the infrastructure required to power it often forces a difficult choice: do you prioritize the raw speed of bare-metal hardware or the security and management benefits of a virtualized environment? For too long, organizations have been forced to choose. Now, Nutanix is addressing this trade-off, delivering the near bare-metal speeds you need without sacrificing the control and security your enterprise demands.


How do we do this? We power GPU-dense platforms, such as the NVIDIA HGX systems, with the Nutanix AHV hypervisor and leveraging Topology-Aware Scheduling with AI optimized System-Defined VM profiles. Topology-aware scheduling improves AI workload performance by ensuring that virtualized resources (vCPUs, memory) are physically aligned with the hardware accelerators (GPUs, NICs) they utilize. AI-optimized VM profiles improve performance by automating complex, hardware-specific tuning that would otherwise require deep manual expertise.


The integration between Nutanix AHV, Nutanix Flow and NKP allows platform engineers to automate the deployment of AI workloads on complex, GPU-dense passthrough servers using these AI-optimized VM profile-based virtual machines as GPU-dense Kubernetes worker nodes for their AI workloads.


Capable of operating in fully air-gapped FIPS-compliant environments, this solution is designed to deliver the multitenancy and security of virtualization. It also provides the throughput and performance required for massive AI workloads, transforming the Nutanix Cloud Platform into a foundational engine for Enterprise Sovereign AI.


## High Performance AI Compute


We’ve optimized Nutanix AHV to help ensure the hypervisor enables highly performant placement of VMs for your AI workloads.


- **Topology-Aware Scheduling:** AHV aligns your workloads to optimize for performance by placing your VMs on physical NUMA nodes, aligning CPU, Memory and GPU resources. AHV also helps ensure the correct GPU and NIC mapping to avoid hops between processors. This topology-aware scheduling aligns all resources—NIC, CPU, Memory–to be as close as possible to the GPU to minimize latency for the workloads, even when other pathways exist to minimize latency.
- **8-GPU Passthrough:** We enable a single VM to consume all 8 x SXM GPUs on an HGX board. This is critical for massive LLM training and inferencing jobs that require the full power of the board without splitting resources.
- **AI-Optimized VM Profiles:** Manual tuning is the enemy of performance. By using pre-validated profiles (e.g., ai.large.h200.8gpu.cx7_ib), we automatically enforce NVIDIA-recommended configurations for memory, CPU, and networking to help ensure AI-optimized virtual machines.


## Orchestration: Treating Infrastructure as Code


The integration between Nutanix AHV and NKP allows platform engineers to automate the deployment of complex, GPU-dense environments with ease.


- **VM Profile-Based Node Pools:** Platform engineers can now treat complex hardware as code. By selecting an "AI Optimized Worker Pool" in NKP, the system automatically provisions the correct HGX-backed VMs, reducing the need for deep hardware knowledge at the Kubernetes layer.
- **Air-Gapped Lifecycle Management:** For highly regulated sectors, the Nutanix Agentic AI full stack solution allows for fully disconnected installations. You can automate driver updates and network optimization for NVIDIA GPU and Network Operators without ever exposing the cluster to the internet.
- **Sovereign Compliance (FIPS/STIG):** NKP enforces FIPS 140-3 and STIG compliance at both the OS and Kubernetes layers, meeting the strictest regulatory requirements for government and sovereign cloud clients.


## Predictable High-Performance via Advanced Networking


Agentic AI demands a high-performance, zero trust foundation that maximizes throughput and secures data flow across the AI Factory.
