---
schema_version: "1.0.0"
document_id: "73c9b9a02c12e6b8c21a9ac8894aa401d116a3e90338b5da784b85e9fb54334a"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-news-import-bfb1f574a3a6"
canonical_url: "https://azure.microsoft.com/en-us/blog/azure-iaas-defense-in-depth-built-on-secure-by-design-principles/"
published_at: "2026-05-04T16:00:00+00:00"
first_seen_at: "2026-07-25T02:59:23.665583+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:c5939225d9b54bebd3c449f20ece3a587e2395980d84a47298549c0ed22d0c95"
---

# Azure IaaS: Defense in depth built on secure-by-design principles

*This blog post is the third part of a blog series called[Azure IaaS](https://azure.microsoft.com/en-us/blog/tag/azure-iaas/) which will share best practices and guidance to help you build a trusted infrastructure platform—from performance, resiliency, and security to scalability and cost efficiency* .


---


​Security for cloud infrastructure is no longer defined by a single control, product, or boundary. Modern threats target identity, software supply chains, control planes, networks, and data simultaneously. Addressing this reality requires two things to work together: **a layered defense-in-depth architecture** and **security principles that are enforced consistently across the platform** .


In[Azure Infrastructure as a Service (IaaS)](https://azure.microsoft.com/en-gb/solutions/azure-iaas/) , security is built around these two reinforcing ideas. First, Azure implements **defense in depth** , applying multiple, independent layers of protection across compute, networking, storage, and operations so that no single control stands alone. Second, those protections are guided by[Microsoft’s Secure Future Initiative (SFI)](https://www.microsoft.com/en-us/trust-center/security/secure-future-initiative) principles: **secure by design, secure by default, and secure in operation** . Together, they define how[Azure IaaS](https://aka.ms/azureIaaS) is engineered, configured, and operated at scale.


[Explore Azure IaaS solutions](https://aka.ms/azureIaaS)


## Defense in depth as a system


Defense in depth is not a checklist of features—it is a **system-level security architecture** . Each layer is designed with the assumption that another layer may fail, and that compromise at one point should not lead to platform-wide impact.


In[Azure IaaS](https://aka.ms/azureIaaS) , defense in depth spans the full infrastructure stack:


- **Hardware and host integrity**
- **Virtualized compute isolation**
- **Network segmentation and traffic control**
- **Data protection for storage**
- **Continuous monitoring and response**


These layers are intentionally independent. Hardware root-of-trust mechanisms validate host integrity before workloads ever start. Virtual machines (VM) run with strong isolation boundaries enforced by the hypervisor. Network controls limit lateral movement and restrict exposure. Storage services encrypt and protect data even if credentials are compromised. And telemetry and monitoring systems operate continuously, detecting and responding to anomalous behavior across the platform.


This layered approach ensures that Azure IaaS security does not rely on perimeter assumptions or a single “control plane defense,” but instead applies **multiple mutually reinforcing controls** that work together.


[Build a stronger cloud infrastructure with Azure IaaS](https://azure.microsoft.com/en-us/blog/azure-iaas-series-explore-new-resources-for-building-a-stronger-more-efficient-infrastructure/)


## Secure by design: Engineering security into the platform


“Secure by design” means security is **architected into the platform from the beginning** , not added after deployment. In[Azure IaaS](https://aka.ms/azureIaaS) , this starts at the lowest layers of the stack.


## Hardware and host-level trust


Azure servers are built with **hardware roots of trust** , measured boot, and secure firmware validation. Technologies such as Trusted Platform Modules (TPMs) and secure boot validate that host firmware, boot loaders, and operating systems have not been tampered with before the system joins the[Azure](https://azure.microsoft.com/en-us) fleet. These mechanisms reduce exposure to firmware-level and boot-chain attacks that traditional software-only defenses cannot address.


Azure also offloads critical infrastructure functions—such as storage, networking, and management operations—into dedicated, hardened components like **[Azure Boost](https://azure.microsoft.com/en-us/products/virtual-machines/boost)** , reducing the attack surface of the host operating system and improving isolation between customer workloads and platform services.


## Virtual machine-layer trust


At the virtual machine layer, Azure enforces strong virtualization boundaries using a hardened hypervisor. Features like **Trusted Launch** for Azure VM combine secure boot, virtual TPMs, and integrity monitoring to protect VMs against low-level attacks such as bootkits and kernel rootkits.


For highly sensitive workloads,[Azure confidential computing](https://azure.microsoft.com/en-us/solutions/confidential-compute) extends defense in depth by using trusted execution environments (TEEs) backed by hardware-based memory encryption (such as AMD SEV‑SNP or Intel TDX). These technologies help ensure that data remains protected even while in use and inaccessible to the host or hypervisor.


Security here is not a bolt-on—it is a **design property** of how Azure compute infrastructure is built and operated.


## Secure by default: Protection enabled without friction


Secure-by-default controls reduce risk by **making the safest option the standard configuration** , without requiring customers to assemble security from scratch.


[Learn how to keep critical applications running with built-in resiliency at scale with Azure IaaS](https://azure.microsoft.com/en-us/blog/azure-iaas-keep-critical-applications-running-with-built-in-resiliency-at-scale/)


## Secure defaults across networking


In[Azure IaaS](https://aka.ms/azureIaaS) , networking defaults are aligned with least-privilege and Zero Trust principles. Virtual networks are isolated by default. Inbound traffic to VM is blocked unless explicitly allowed. Network security groups (NSGs) enforce stateful filtering, while[Azure Firewall](https://azure.microsoft.com/en-us/products/azure-firewall) provides centralized policy enforcement and traffic inspection when deployed.


Private connectivity options such as[Azure Private Link](https://azure.microsoft.com/en-us/products/private-link) and **private endpoints** allow services to be accessed without exposing them to the public internet. DDoS protection is automatically applied at the platform edge, helping protect workloads from volumetric attacks without additional configuration.


These defaults limit exposure by design, narrowing the attack surface before workload-specific rules are added.


## Encryption and data protection by default


Azure IaaS storage services encrypt data **at rest by default** , using platform-managed keys, with options to use customer-managed keys via[Azure Key Vault](https://azure.microsoft.com/en-us/products/key-vault) or **Managed HSM** . Disk encryption protects operating system and data disks for VM, and secure snapshots protect point-in-time copies of data.


Encryption in transit is enforced across Azure backbone networks, ensuring traffic between services within the platform is protected without requiring per-workload configuration.


Secure-by-default encryption ensures that data protections are always on, not optional.


## Compute protection defaults


Signed and measured Azure host boot, secure host operating system (OS) hardening, host‑level monitoring and patching by Microsoft, and hypervisor-enforced isolation between tenants are all enabled by default and cannot be disabled by Azure tenants.


Trusted Launch is enabled by default for newly created Azure Gen2 VMs and VM scale sets, when using supported OS images, VM sizes, and deployment methods. Supported deployments methods include deployment via the Azure Portal, ARM templates, Bicep, Terraform, and Azure SDKs.


## Secure in operation: Continuous protection at runtime


Security does not stop at deployment. The *secure in operation* principle focuses on maintaining protection continuously as threats evolve.


## Monitoring, detection, and signal correlation


Azure integrates telemetry from compute, network, and storage layers into centralized monitoring systems such as[Azure Monitor](https://azure.microsoft.com/en-us/products/monitor) and[Microsoft Defender for Cloud](https://www.microsoft.com/en-us/security/business/cloud-security/microsoft-defender-cloud) . These systems continuously analyze behavior to identify misconfigurations, detect threats, and surface actionable security recommendations.


For IaaS workloads, Defender for Cloud helps identify exposed management ports, missing disk encryption, and insecure network configurations, while also correlating threat signals across the environment.


## Identity-centric control and least privilege


Operational security depends heavily on identity.[Azure IaaS](https://aka.ms/azureIaaS) integrates with[Microsoft Entra ID](https://www.microsoft.com/en-ie/security/business/identity-access/microsoft-entra-id) to enforce identity-based access controls, reduce standing privileges, and apply conditional access policies. Features like **Just-In-Time (JIT) VM access** limit administrative exposure by only opening management ports when needed and only for approved identities.


By minimizing persistent access and rotating privileges dynamically, Azure reduces the impact of credential compromise.


## Bringing defense in depth and SFI together


Defense in depth provides the **technical structure** of Azure IaaS security. Secure by design, secure by default, and secure in operation provide the **engineering and operational discipline** that governs how those controls are built, deployed, and maintained.


Together, they ensure that Azure IaaS security is:


- **Layered** : No single control is assumed to be sufficient.
- **Intrinsic** : Security is part of the platform architecture, not an add-on.
- **Consistent** : Defaults and policies reduce configuration drift.
- **Adaptive** : Continuous monitoring and operational controls evolve with the threat landscape.


This combination allows Azure to protect IaaS workloads across compute, network, and storage while maintaining compatibility with diverse operating systems, workload types, and deployment models.


## Security as an ongoing platform commitment


Azure IaaS security is not defined by a static set of features. It is the result of **ongoing engineering investment** , guided by clear principles, and reinforced through layered technical controls.


Defense in depth ensures that failures are contained.[Secure-by-design architecture reduces attack surfaces](https://info.microsoft.com/ww-landing-top-11-factors-in-cloud-and-ai-security.html?lcid=en-us) from the start. Secure-by-default configurations minimize exposure without adding friction. And secure-in-operation practices ensure the platform continues to adapt as threats evolve.


Together, these principles define how[Azure IaaS](https://aka.ms/azureIaaS) delivers infrastructure security that is systematic, scalable, and aligned with modern threat realities.


To go deeper, explore the[Azure IaaS Resource Center](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Faka.ms%2FazureIaaS&data=05%7C02%7Cestela.virko%40wipro.com%7C8976f285132449ccf94a08dea4a9504b%7C258ac4e4146a411e9dc879a9e12fd6da%7C0%7C0%7C639129244599856580%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=HuTEBGrGMZAwSbxzDSdazucmpQyeVFfO7N3koO6A5OA%3D&reserved=0) for tutorials, best practices, and guidance across compute, storage, and networking to help you design and operate resilient infrastructure with greater confidence.


## Create a resilient infrastructure with Azure


Visit the Azure IaaS Resource Center to start building a stronger, more efficient infrastructure today.


[Get started with Azure](https://aka.ms/azureIaaS)


**Did you miss these posts in the**[Azure IaaS series](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fblog%2Ftag%2Fazure-iaas%2F&data=05%7C02%7Cestela.virko%40wipro.com%7C8976f285132449ccf94a08dea4a9504b%7C258ac4e4146a411e9dc879a9e12fd6da%7C0%7C0%7C639129244599871621%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=7z40Y9l8L2kOr%2BKYg9GW4Wg4pq3RCiLudZ%2BwUWVxehY%3D&reserved=0) **?**


- [Explore new resources for building a stronger, more efficient infrastructure](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fblog%2Fazure-iaas-series-explore-new-resources-for-building-a-stronger-more-efficient-infrastructure%2F&data=05%7C02%7Cestela.virko%40wipro.com%7C8976f285132449ccf94a08dea4a9504b%7C258ac4e4146a411e9dc879a9e12fd6da%7C0%7C0%7C639129244599886403%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=BZDQPpYZDcD5Vj%2FXqr%2FTQPuMYX8m0SXySkHrj5L8NYo%3D&reserved=0)
- [Keep critical applications running with built-in resiliency at scale](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fblog%2Fazure-iaas-keep-critical-applications-running-with-built-in-resiliency-at-scale%2F&data=05%7C02%7Cestela.virko%40wipro.com%7C8976f285132449ccf94a08dea4a9504b%7C258ac4e4146a411e9dc879a9e12fd6da%7C0%7C0%7C639129244599900207%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=FeptOA3GUJp8KJAyc3FNvPVM2SYlbBYHHahuvuvvv2s%3D&reserved=0)
- [Deploy high-performance workloads with a system-level approach](https://azure.microsoft.com/en-us/blog/azure-iaas-deploy-high-performance-workloads-with-a-system-level-approach/)
- [How to design, build, and optimize cloud infrastructure for long-term efficiency](https://azure.microsoft.com/en-us/blog/azure-iaas-how-to-design-build-and-optimize-cloud-infrastructure-for-long-term-cost-optimization-efficiency/)
