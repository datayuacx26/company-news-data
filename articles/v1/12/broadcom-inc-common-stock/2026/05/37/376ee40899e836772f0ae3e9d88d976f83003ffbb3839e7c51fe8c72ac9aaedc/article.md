---
schema_version: "1.0.0"
document_id: "376ee40899e836772f0ae3e9d88d976f83003ffbb3839e7c51fe8c72ac9aaedc"
company_key: "broadcom-inc-common-stock"
company: "Broadcom Inc."
source_id: "broadcom-inc-common-stock-rss-6823269edf1e"
canonical_url: "https://news.broadcom.com/cloud/private-ai-infrastructure-vmware-cloud-foundation"
published_at: "2026-05-12T19:27:23+00:00"
first_seen_at: "2026-07-20T23:21:13.318217+00:00"
fetched_at: "2026-07-28T21:14:05.751806+00:00"
content_hash: "sha256:7beaf7947485e14eb7d3a52b1f3fbdc02491e55fb84edded389d11e7f61659fe"
---

# AI Is Demanding More From Your Private Infrastructure. Are You Ready To Deliver It?

-
-
-


AI is demanding more from private infrastructure than most organizations anticipated.


More compute, governance, security and cost discipline. And it is all happening faster than IT planning cycles were designed to handle.


Public cloud, while often used for fast experimentation and learning, does not satisfy all of those requirements at once. Not when your IP needs to stay under your control. And not when data sovereignty, data security, model performance and cost predictability are non-negotiable. That’s why enterprises are moving production AI to their private infrastructure.[VMware Cloud Foundation 9.1](https://news.broadcom.com/releases/broadcom-announces-vmware-cloud-foundation-9-1) is built to support that shift. It is the most secure, cost-effective platform for Private AI that runs on your infrastructure, under your governance.


## The Infrastructure Approach Enterprises Need


Every major technology shift follows the same infrastructure pattern. New capability, new platform, new team, and new tools. All of it added alongside everything that already exists, creating disparate silos.


AI is no different. Clusters are proliferating across teams. Tooling sprawl with no consistent operational model, with models running on infrastructure that was never designed for production operations. Every new initiative adds another layer of overhead, risk, and management cost that displaces spending on actual value.


Agentic AI amplifies every one of these challenges. Hundreds of agents running across silo’d environments. Different infrastructure, security models, and storage - with no common operational layer. Every agent that operates outside a unified governance framework is a gap in compliance and a potential entry point for attackers.


## What VCF 9.1 Does


[VCF 9.1 is a single unified cloud platform](https://www.vmware.com/video/6394989928112) that delivers the most advanced enterprise grade capabilities to run and manage AI workloads and agents. One operational model and one security posture, irrespective of how your teams and applications are structured.


It is[the industry's leading private cloud platform](https://www.vmware.com/docs/vmware-cloud-foundation-9-1-solution-brief) that combines the agility of public cloud with the security, resilience, and performance of private cloud. All while running a variety of container and VM based AI workloads on a kubernetes-native foundation, with developer self-service and enterprise governance maintained end to end.


This release particularly advances innovations in three areas where enterprises are running into real operational problems with AI.


## 1. Governance and Control for Production AI


The barrier to enterprise AI is not the technology. CIOs and compliance leaders need to answer three questions before AI goes into production: Who controls the data and IP? Who governs the agents? Who owns the audit trail?


VCF 9.1 is built to address all.


**Data privacy and compliance** .[VCF](https://www.vmware.com/docs/vmware-cloud-foundation-9-1-datasheet) now includes VCF Private AI Services, giving organizations a choice of deploying open-source and commercial AI models while maintaining privacy, compliance, and control over models and training data.


**Agentic AI with governance.** Enterprises are rightfully concerned about the potential for AI agent sprawl and lack of central control over model context protocol (MCP) servers, which can lead to unauthorized access, data leakage, and compliance risks. VCF 9.1 enables IT operations to centrally manage and control access to MCP tools and associated servers across their environment, ensuring that user groups can only access approved tools.


**AI metrics and visibility.** Customizable dashboards track AI model and agent performance in real time. Metrics include token throughput and infrastructure utilization, giving operations and platform leadership the visibility needed to optimize AI workloads and account for infrastructure ROI.


**Accelerator choice** . WIth VCF, Inference runs on your infrastructure and under your policies. Through partnerships with AMD, NVIDIA, and Intel, VCF 9.1 supports a range of AI accelerators. Organizations have more control over their infrastructure decisions, and can match compute to the workload needs.


## 2. Infrastructure Economics for AI at Scale


AI infrastructure can be expensive. The question is whether that cost is being managed or just absorbed.


Dedicated AI hardware running below capacity is a planning problem that compounds at scale. Every idle GPU draws power. Every additional server expands software licensing exposure, security surface area, energy and support costs. These factors erode projected returns quietly, often before a model reaches production.


[VCF 9.1](https://www.vmware.com/docs/vmware-cloud-foundation-9-1-solution-brief) addresses this through improvements across storage, compute, and lifecycle operations.


- **Up to 40% lower server TCO.** Intelligent NVMe Memory Tiering automates data placement across memory tiers, reducing the need to purchase additional DRAM to meet workload demands. Built-in software mirroring removes the hardware RAID dependency.
- **Up to 39% lower storage costs** . Next-generation global deduplication and compression reduce vSAN capacity requirements for compressible workloads. This is a material cost impact for AI environments running relational databases and model storage at scale.
- **80% reduction in maintenance downtime.** Live patching moves lifecycle operations out of weekend maintenance windows and into normal business hours. That returns engineering time to productive work and reduces the risk of running AI environments on outdated software.
- **Improved operational scale.** VCF 9.1 doubles host capacity per deployment from 2,500 to 5,000, increases parallel cluster upgrades from 64 to 256, and cuts Kubernetes cluster provisioning time by 70%. Platform teams can manage a significantly larger AI estate while controlling the operating costs.


## 3. Security Designed for AI Workloads


AI introduces asset categories that most existing security architectures were not designed to protect.


Proprietary models represent significant investment and competitive value. Training data carries legal obligations around protection and privacy. Agentic AI workflows interact with production systems in ways that traditional applications do not while creating access paths that require explicit governance. A security incident in an AI environment carries both competitive and regulatory consequences, not just operational ones.


VCF 9.1 integrates security at every layer, from the hypervisor to the AI agent, without requiring additional tooling on top.


- **Zero-trust architecture across workload types** . Segmentation, access controls, and policy enforcement span VMs, containers, and AI agents on the same platform. Six layers of isolation from the datacenter to the application, with no gaps between workload types.
- **Sovereign cyber recovery architecture.** Recovery environments stay within the jurisdictional boundaries that compliance teams require. VCF 9.1 enables compliant, sovereign recovery that protects critical workloads without sacrificing regulatory control.
- **CrowdStrike integration** . Organizations can identify breaches faster, validate that environments are clean before restoring, and prevent reinfection. These are capabilities that are directly relevant when high-value AI models and training data are involved.
- **AI-accelerated threat response** . The threat landscape has changed. Frontier AI security models are now discovering vulnerabilities at a speed and volume that is much higher than historical norms, including the ability to chain low-severity vulnerabilities into critical exploits . Broadcom is using these same models to analyze VCF code and[strengthen the platform against the threats it will face](https://blogs.vmware.com/cloud-foundation/2026/05/11/ai-has-changed-the-threat-landscape-is-your-infrastructure-ready/) .


## The Decision


Private AI is not a future planning item. Enterprises running production AI today are making infrastructure decisions now that will determine their cost structure, security posture, and operational capacity for years.


Organizations that consolidate traditional and AI workloads onto a unified platform will run AI at lower cost and with fewer governance gaps than those continuing to build fragmented stacks. VCF 9.1 is that platform. It runs container based and VM based AI workloads on a single integrated infrastructure layer in a seamless enterprise-grade operating model, with zero-trust security from the hypervisor to the agent, and economics that hold up at production scale.


Explore VMware Cloud Foundation 9.1[vmware.com/products/cloud-foundation](https://www.vmware.com/products/cloud-foundation.html)
