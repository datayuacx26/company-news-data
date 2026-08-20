---
schema_version: "1.0.0"
document_id: "591156597d82a4d5162c9d93897f9195ecd24fe65bd90f70319b9bec48b35bd0"
company_key: "yc-anjuna"
company: "Anjuna"
source_id: "yc-anjuna-news-import-d3424cd26fcb"
canonical_url: "https://www.anjuna.io/blog/anjuna-seaglass-bare-metal-confidential-computing"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-24T16:50:44.532118+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:02fada770a672652a1a3f7a2712ffe36cd19a85b72bc15ce430cbdef1c39da2f"
---

# Confidential Computing for the Data Center: Anjuna Seaglass Extends to Bare Metal

The threat to critical applications and AI has never been higher, and it no longer stops at the edge of the cloud. **Anjuna Seaglass** , the Universal Confidential Computing Platform, now extends to bare metal, bringing the same hardware-enforced trust that secures workloads on AWS, Azure, and Google Cloud directly into the data center.


Enterprises are increasingly turning to the strong isolation and trust capabilities of confidential computing to protect against software attacks that gain elevated privileges, defend against malicious or compromised insiders, meet data sovereignty requirements including **EU DORA** , run AI models in customer or partner infrastructure without loss of control, secure powerful AI agent operations, and protect critical assets from AI-driven attacks enabled by advanced models capable of automated exploit creation.


Confidential computing addresses these risks by shielding workloads from infrastructure attacks that punch holes through any software-only security solution. The hardware-based **Trusted Execution Environment (TEE)** prevents software or human-based attacks from accessing or modifying application memory, and *Remote Attestation* removes the need to store first secrets in cleartext, where they become available to any attacker with disk access.


Enterprises that understand the value of confidential computing but chose their own adventure in deploying it often see limited success, due to the deep skill requirements and complexity of the heavy lifting involved. This is where Anjuna Seaglass comes in: a proven, frictionless solution for securing sensitive workloads that protect data in-use, at-rest, and in-transit across multi-cloud environments, abstracting the heavy lifting away from deployment. With **Anjuna Seaglass on bare metal** , that same universal platform now extends seamlessly to AMD SEV hardware in your own data center, enabling fully secured and attested container workloads on-premises, all without code changes, kernel modifications, or a new operational model.


## Bare Metal Support for[Anjuna Seaglass](https://www.anjuna.io/product/seaglass)


The Anjuna Seaglass platform is built around a simple, two-stage model:


- Build applications into hardened Anjuna Confidential Containers without touching a line of code.
- Deploy and run the Confidential Containers on confidential computing-enabled hardware.


The **Anjuna Policy Manager (APM)** supports releasing secrets to Anjuna Confidential Containers based on the cryptographic proof coming from the workload running in the TEE. The APM verifies attestation reports from Anjuna Confidential Containers running in AMD SEV TEEs on bare-metal machines, and can also run in air-gapped environments, a common requirement for on-premises deployments.


The result: an enterprise running AMD SEV-capable servers on-premises can now deploy Anjuna Confidential Containers to bare metal exactly as they would to a cloud instance, with the same tooling, workflow, and attestation guarantees. The infrastructure changes, but the security model and usage experience do not.


## Security Risks for On-Prem Sensitive Workloads


The security risks enterprises manage in the cloud don't disappear when workloads move on-premises. A malicious or compromised administrator with privileged access is the same insider threat whether workloads run in the cloud or in your own data center. An on-prem environment can create just as many attack opportunities, especially with employees bringing their own mobile phones and laptops to access IT systems. Data-in-use and first-secrets like cryptographic keys remain exposed unless there is a hardware-enforced boundary around the workload itself.


### Sovereignty, Residency, and the Cloud-Off-Limits Problem


For regulated industry segments, including financial services institutions, defense contractors, healthcare organizations, and public sector agencies, running certain workloads in the cloud simply isn't an option. Data sovereignty laws, contractual obligations, and classification requirements mean critical apps and AI inferences involving regulated data must stay on-prem.


Until now, those organizations relied on software-based security to prevent unauthorized privileged access, detect when access occurred, and determine whether it was malicious. But once a malicious actor was inside, those solutions couldn't prevent the activity itself, only hope it would be caught quickly. **Anjuna Seaglass on bare metal solves that threat** : even if a malicious actor gains privileged access to the server, sensitive data running in the hardware-based TEE cannot be accessed.


### Closing the Compliance Gap for Security and AI Governance


Regulations including[DORA](https://www.eiopa.europa.eu/digital-operational-resilience-act-dora_en) **,**[HIPAA](https://www.hhs.gov/hipaa/index.html) **, and**[GDPR](https://gdpr-info.eu/) , plus internal enterprise data security policies, demand demonstrable proof that sensitive data is protected during processing, not just at-rest or in-transit.


Hardware-rooted TEEs with cryptographic attestation deliver both. When Anjuna Seaglass deploys on bare metal, every workload runs in a verified, measured environment. The APM evaluates attestation before releasing secrets, so security teams can prove, cryptographically, that the right code ran in the right environment. If a cybersecurity incident occurs downstream, that proof gives governance and audit teams evidence that sensitive systems were not impacted.


## The Technical Story


Anjuna Seaglass abstracts the application from the underlying confidential computing-enabled processor. Any existing workload, a database, proprietary AI model, AI agent runtime, or enterprise application, is packaged into an Anjuna Confidential Container that runs in a TEE, with no code changes required.


The **Anjuna Confidential Runtime** provides always-on encryption: workloads run inside AMD SEV TEEs with memory encrypted by hardware. Any attempt to access process memory, extract TLS keys, exfiltrate model weights, or replace application binaries with malware fails at the hardware boundary.


The **Anjuna Policy Manager (APM)** enforces attestation-based trust. Before any secret, an API key, database credential, or model decryption key, is released to a workload, the APM evaluates a cryptographic attestation report covering both the execution environment and the application being run. Only verified code, in a verified environment, gets access to the sensitive data it needs. With Anjuna Seaglass, this works the same whether the workload runs in AWS, Azure, GCP, or on a bare metal server in your own data center.


## The Benefits Everyone Can Agree Upon


### For security and compliance teams


- Hardware-rooted isolation for workloads and sensitive applications, with cryptographic attestation that satisfies regulatory demands for proof of data protection during processing.
- Defense against the highest-value threats to on-prem infrastructure, including zero-day vulnerabilities, insider access, and nation-state actors via local access attacks (including memory and disk access). Workload memory is encrypted by the CPU and inaccessible to the OS, root users, or anyone with physical access to the machine, with no secrets left in cleartext on disk.


### For AI governance and CAIO teams


- Model integrity verification from deployment through inference, providing the auditability and traceability required by the[NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) and the[EU AI Act .](https://artificialintelligenceact.eu/)
- A governed, hardware-isolated execution environment for agentic AI workloads that closes the exposure gap created when agents operate with broad system access on unprotected infrastructure.
- The ability to bring unsanctioned on-prem AI workloads under formal governance and hardware-level isolation without disrupting existing deployments.
- A foundation for on-prem AI use cases previously blocked by compliance constraints.


### For platform and operations teams


- No code changes, with minimal changes to the CI/CD process to simplify deployment. The same Anjuna Seaglass build-deploy-run-trust cycle that operates on public cloud now extends directly to bare metal, saving on operational overhead.


## The Next Step in Full Flexibility and Control


Bare metal support for confidential computing is a direct response to a reality the industry has been slow to address: the threat to critical apps and AI has increased exponentially, especially in the age of AI-driven attacks. LLMs, agents, and fine-tuned models run on infrastructure that was never designed to isolate execution, verify model integrity, or prove to a regulator that sensitive data was protected during processing. Threat actors know this, and the time it takes to inflict serious damage has dropped dramatically, making it hard for humans to keep up with patching. Regulators are starting to catch up with compliance requirements and audit questions.


Anjuna Seaglass on bare metal closes that gap for on-premises computation. Hardware-enforced trust is no longer a cloud privilege, it's available in your data center, on your hardware, under your control. Whether your infrastructure is in AWS, Azure, Google Cloud, or your own data center, the answer is the same: **one platform, zero code changes, hardware-enforced trust, full flexibility.**


Ready to see it for yourself? Contact us to set up a demo anytime at sales@anjuna.io. Or try Anjuna Seaglass free for 30 days at[anjuna.io/anjuna-free-trial](https://www.anjuna.io/anjuna-free-trial) .
