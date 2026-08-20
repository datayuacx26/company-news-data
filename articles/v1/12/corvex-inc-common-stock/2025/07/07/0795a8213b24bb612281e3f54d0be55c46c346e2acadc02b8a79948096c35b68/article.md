---
schema_version: "1.0.0"
document_id: "0795a8213b24bb612281e3f54d0be55c46c346e2acadc02b8a79948096c35b68"
company_key: "corvex-inc-common-stock"
company: "Corvex Inc."
source_id: "corvex-inc-common-stock-rss-5d3752083407"
canonical_url: "https://www.corvex.ai/blog/unlocking-healthcare-ai"
published_at: "2025-07-18T00:00:00+00:00"
first_seen_at: "2026-07-26T12:16:19.295968+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:e1b7120da4cf7c198617d9229baab59e0febe6f5ab96344efd124cd8a86fed86"
---

# Unlocking Healthcare AI: Solve IT Security with Architecture

Artificial intelligence is poised to revolutionize healthcare, but its progress is tethered to a critical challenge: how to train, fine-tune and inference models on **Protected Health Information (PHI)** without violating **HIPAA** and exposing sensitive patient data. For CTOs and CISOs in the AI and healthcare sectors, this isn't just a compliance hurdle—it's a fundamental security problem that can stall innovation and introduce significant risk.


The solution requires a new paradigm that addresses the challenges of **Data in Use** and **Multi-Tenancy** .


## **The Core Challenges: "Data in Use" and Multi-Tenancy**


For decades, cybersecurity has focused on two primary states of data:


1. **Data at Rest:** Encrypting data stored on disks or in databases.
2. **Data in Transit:** Encrypting data moving across a network.


However, there's a third state that represents a significant security gap: **data in use** . To be processed by a CPU or GPU, data must be decrypted and loaded into memory (RAM). In that state, it's potentially visible to the cloud provider, system administrators, the operating system, the hypervisor, or attackers who compromise the host system. For AI workloads that process PHI, this is an unacceptable risk.


This vulnerability is magnified in standard **multi-tenant** cloud environments. While logically separated, tenants share the same underlying physical hardware. This architecture presents several problems:


- **Failures in Tenant Isolation:** The primary risk in a multi-tenant environment is the breakdown ofl boundaries between tenants.


- **Compute Isolation Failure:** Vulnerabilities in the hypervisor or container runtime can allow a malicious actor in one tenant environment to "break out" and gain access to the host system or other tenants' environments. Additionally, sophisticated **side-channel attacks** (like Spectre, Meltdown, and Downfall) can exploit shared physical CPUs to infer sensitive data from other tenants processing on the same hardware.
- **Access Control Misconfiguration:** At a logical level, improperly scoped Identity and Access Management (IAM) or Role-Based Access Control (RBAC) can inadvertently grant one tenant permissions to see or modify another tenant's data, leading to direct data leakage.


- **Perception and Compliance Friction:** Even if the technical risk is managed, the *perception* of shared infrastructure remains a major red flag for compliance officers and IT security reviewers, which can significantly slow down sales cycles with the customers of AI companies pursuing healthcare clients as well as review cycles for enterprises processing PHI.


## **The Corvex Solution: Defense in Depth via Single-Tenancy + Confidential Computing**


At Corvex, we engineered our AI cloud to directly address these challenges with a two-layer solution designed for zero-trust execution of sensitive workloads.


### **Foundation: HIPAA-Compliant, Single-Tenant VPCs**


First, we can eliminate multi-tenancy risk entirely. Side-channel attacks like Spectre/Meltdown showed how shared hardware can leak data between tenants. Corvex customers can operate within a **single-tenant Virtual Private Cloud (VPC)** . This provides a dedicated, physically isolated environment for your compute and storage resources. For CISOs, this immediately simplifies the compliance conversation. The attack surface is dramatically reduced, streamlining security reviews and satisfying auditors who demand strong infrastructure isolation.


### **The Game-Changer: Confidential Computing**


While single-tenancy solves the infrastructure problem, **Confidential Computing** solves the "data in use" problem, which is the 'third pillar' of data security - protecting data at rest, in transit, and, now, in use. It uses hardware-based **Trusted Execution Environments (TEEs)** —often called secure enclaves—to create an encrypted black box for data processing.


Think of it like this: your encrypted PHI and your AI model go into the TEE. The GPU performs the training or inference *inside* this protected environment. The raw data is never decrypted outside the enclave where it could be seen by the OS, hypervisor, or even by us as the cloud provider. Only the authorized code running inside the enclave can see the plaintext data.


These TEEs, built into modern CPUs (like Intel SGX and TDX) and GPUs (like the NVIDIA H200 and B200), provide four key security properties:


- **Isolation:** The enclave is cryptographically isolated from all other software on the system, including the kernel and hypervisor. This isolation is enforced at the hardware level, making it more trustworthy than software sandboxing.
- **Runtime Memory Encryption:** All data and code inside the enclave remain encrypted in memory. The CPU/GPU decrypts it on the fly inside the processor chip itself, processes it, and immediately re-encrypts it before sending it back to RAM. Encryption keys are generated and managed by the hardware itself, never exposed to software.
- **Sealing:** The enclave can securely save its state (e.g., a partially trained model) to untrusted storage, as the data is encrypted with a key only the enclave itself can access. This enables resuming training after interruptions without security loss.
- **Remote Attestation:** This is perhaps the most powerful feature for compliance. The TEE can cryptographically prove to a remote party (like a data provider or auditor) that it is a genuine, secure piece of hardware running the exact, unmodified code you authorized. This provides a verifiable, auditable record of a secure processing environment, like a tamper-evident seal that can be verified remotely and cryptographically. Now, for example, a hospital can verify their patient data is processed in a genuine H200 GPU running unmodified PyTorch code.


## **How Confidential Computing Directly Supports HIPAA**


Corvex's approach provides the technical safeguards required to meet some of the most stringent components of the HIPAA Security Rule.


- **Access Control (§ 164.312(a)(1)):** The Rule requires you to "limit electronic PHI to authorized persons." A TEE is the ultimate form of technical access control. By design, only the specific AI model code you approve can ever access the decrypted PHI.
- **Data Integrity (§ 164.312(c)(1)):** This requires protecting PHI from "improper alteration or destruction." Because data and computation occur within a sealed, tamper-proof environment, its integrity is cryptographically guaranteed during processing.
- **Transmission Security** ( **§ 164.312(e)(2)(ii)):** This implementation specification calls for a mechanism to "encrypt electronic protected health information" during transmission. Confidential computing provides an exceptionally strong mechanism for this. Data is not only sent to the TEE over an encrypted channel, but the TEE first provides a cryptographic proof (attestation) of its identity and integrity. Only after this verification is the encrypted PHI transmitted, ensuring it is unreadable in transit and can only be decrypted within the secure TEE itself.
- **Minimum Necessary Principle (§ 164.502(b)):** This principle mandates that you use or disclose only the minimum amount of PHI necessary for a task. Confidential computing aligns perfectly. For example, in a federated learning scenario, multiple hospitals can collaborate to train a single diagnostic model. The TEE processes their respective datasets, but only the resulting model updates—not the underlying PHI—are ever exposed or shared.


## **How Single-Tenant VPCs Directly Support HIPAA**


While confidential computing secures the data during processing, the choice of infrastructure is a foundational pillar of compliance. A single-tenant Virtual Private Cloud (VPC) provides a level of isolation that directly addresses core requirements of the HIPAA Security Rule in a way that multi-tenant environments inherently cannot.


- **Risk Analysis and Management (§ 164.308(a)(1)):** The Rule requires a "thorough assessment of the potential risks" to PHI. In a multi-tenant cloud, your risk assessment must account for the "noisy neighbor" problem—where a vulnerability or attack on another tenant could potentially compromise the shared hypervisor. This creates a complex and ambiguous attack surface. A **single-tenant VPC** drastically simplifies risk analysis by providing a clear, defensible boundary. The risks are confined to your dedicated environment, making them easier to identify, manage, and mitigate, satisfying the need for a thorough and accurate assessment.
- **Security Incident Procedures (§ 164.308(a)(6)):** This requires procedures to identify and respond to security incidents. In a shared environment, the "blast radius" of an incident is uncertain. An attack on the shared infrastructure makes it difficult to determine the impact on your specific data, and forensic investigation can be slowed by the provider's need to protect other tenants' privacy. With a **single-tenant VPC** , the blast radius is naturally contained. Incident response is faster and more precise because the logs, network traffic, and hardware are exclusively yours, enabling a swift and decisive investigation.
- **Audit Controls & Terminal Access (§ 164.312(b))** : The Rule requires mechanisms to record and examine activity in systems containing PHI. Proving sufficient isolation to an auditor is a common challenge in multi-tenant setups. It relies on trusting the provider's logical separation, which can be a point of contention. A **single-tenant VPC** provides unambiguous proof of isolation. It's a simple, powerful narrative for auditors: your PHI workloads run on dedicated infrastructure, physically and logically firewalled from any other entity. This satisfies audit requirements more directly and reduces the friction of compliance reviews.


## **Performance Without Compromise**


Security is non-negotiable, but it can't come at the cost of performance, especially in AI. That’s why Corvex runs on the latest NVIDIA **B200** and **H200** GPUs, designed to accelerate massive AI workloads. And because security shouldn't be a performance tax, we're launching **Corvex Ignite** in Q3 2025—a software accelerator that dramatically speeds up AI models in confidential environments, lowering the total cost of ownership for secure AI compute.


By combining the isolation of single-tenant VPCs with the data-in-use protection of confidential computing, Corvex removes the security and compliance roadblocks that have held back healthcare AI. Now, leaders in biopharma, health systems, and insurance can finally use AI to its full potential, with auditable proof that their most sensitive data is always secure.
