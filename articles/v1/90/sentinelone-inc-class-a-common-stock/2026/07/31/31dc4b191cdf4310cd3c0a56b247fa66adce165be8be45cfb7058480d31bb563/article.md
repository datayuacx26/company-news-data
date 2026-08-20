---
schema_version: "1.0.0"
document_id: "31dc4b191cdf4310cd3c0a56b247fa66adce165be8be45cfb7058480d31bb563"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/hpc-ai-workloads-need-runtime-security-the-architecture-already-exists/"
published_at: "2026-07-08T16:24:38+00:00"
first_seen_at: "2026-07-20T23:17:13.754958+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:a2b86c729d0f92a8e0ce6bf58d9b94ef0ae3c5a1ff6e5cdbe191466a75e2b75d"
---

# HPC AI Workloads Need Runtime Security. The Architecture Already Exists.

The US Federal Government is committing $600 million to build one of the world’s most advanced AI infrastructure systems. Executive Order[14363](https://www.federalregister.gov/documents/2025/11/28/2025-21665/launching-the-genesis-mission) , the Genesis Mission, connects national laboratory supercomputers across nuclear simulation, biodefense, energy grid modeling, and every major scientific domain. Fifty-one organizations signed on, including NVIDIA, OpenAI, IBM, Microsoft, AWS, Google, and Oracle.


The security framework governing these workloads was not written for this scale of use.


[NIST SP 800-234](https://csrc.nist.gov/pubs/sp/800/234/final) , the High-Performance Computing Security Overlay, is well-constructed, tailoring 60 controls across four security zones, building on the[SP 800-53B](https://csrc.nist.gov/pubs/sp/800/53/b/upd1/final) moderate baseline. It was designed for deterministic HPC workloads such as climate simulations, finite element analysis, and computational fluid dynamics. These workloads share a common attribute: code that runs the same way, every time, and behaves predictably under well-understood inputs. The security controls governing those workloads assume you can scan at the perimeter, clear memory between jobs, and attest to integrity at load time.


AI workloads break every one of those assumptions.


SentinelOne has submitted a formal proposal to the[NIST HPC Security Working Group](https://csrc.nist.gov/projects/high-performance-computing-security) regarding this gap, and NIST has acknowledged it. We have a post on LinkedIn to share our proposal, and welcome commentary from across the industry.


## The supply chain problem just got a lot more dangerous


This spring, in just three weeks,[three AI-driven supply chain attacks](https://www.sentinelone.com/blog/hypersonic-supply-chain-attacks-one-solution-that-didnt-need-to-know-the-payload/) targeted widely deployed software: LiteLLM, the most-used AI infrastructure package in Python development environments, Axios, the most-downloaded HTTP client in the JavaScript ecosystem, and CPU-Z, a trusted system diagnostic tool with a legitimate signed binary from the official vendor domain.


SentinelOne stopped all three on the same day each attack launched, with no prior knowledge of any payload.


The most important aspect of this outcome is *how* these attacks were stopped, and why signature-based detection couldn’t work. Each attack arrived through a trusted delivery channel. LiteLLM was compromised after credentials were stolen via Trivy, a security scanner. The attacker published two malicious versions to the[PyPI](https://pypi.org/) repository. In at least one confirmed case, an AI coding agent with unrestricted permissions auto-updated to the infected version, meaning there was no human review or approval step before the payload ran. The Axios attacker exploited a legacy access token that the project maintainers had forgotten to revoke, bypassing every npm security control. CPU-Z attackers targeted the vendor’s distribution infrastructure directly; anyone who downloaded from the official website received a properly signed binary containing a payload. In all three cases, while the authorization chain was legitimate, the *intent* was not.


This is the defining characteristic of modern supply chain attacks: the workflow is verified, but the intent has been subverted. Every perimeter control, signature library, and reputation lookup checks authorization and passes. These attacks were designed to exploit that gap, and they ran at machine speed through automated pipelines with no human checkpoint.


To put this into the context of HPC and AI workloads running at scale, a compromised Python package in a developer’s environment is a serious incident; a poisoned training pipeline on classified biodefense data on a national laboratory supercomputer is on a different order of magnitude. The model it produces may be correct 99.9 percent of the time and adversarially wrong under precisely targeted conditions. No perimeter scan, signature check, or load-time integrity verification will catch it after training completes.


## Where the current framework falls short


Of the 60 controls[SP 800-234](https://csrc.nist.gov/pubs/sp/800/234/final) tailors, three bear directly on AI workload protection, and each carries a documented gap. In a fourth area, supply chain, the overlay does not tailor at all.


- SI-3 (Malware scanning):


The control acknowledges that real-time scanning is most effective but explicitly permits tailoring for performance on HPC systems, deferring to perimeter scanning before data reaches the compute zone. For traditional HPC workloads, that tradeoff may be defensible, but for AI workloads, it leaves behavioral analysis of the execution process completely unaddressed. A poisoned training run that executes within the expected statistical range of a training job looks like legitimate compute to a perimeter scanner.
- SI-4 (System monitoring):


The control notes that high-speed data flows in HPC environments can overwhelm standard monitoring tools, and lacks AI-specific monitoring requirements or telemetry collection requirements from execution pipelines. The practical interpretation of this is: monitor what you can, accept the gap for what you can’t. On infrastructure running AI at scale, that gap creates a primary attack surface.
- SC-4 (Information in shared resources):


Requires GPU memory clearing between user reassignments. It addresses data residency at the transition but does not address runtime behavioral monitoring of workloads during execution, side-channel attack detection, or anomalous compute-pattern identification while training is active.
- SR family (Supply chain risk management):


The overlay carries all 12 moderate-baseline SR controls forward from SP 800-53B, with no HPC or AI-specific guidance, and supply chain is not among the 14 categories it tailors to. The SR controls still address only the conventional software and hardware supply chain; they say nothing about training-data provenance, model-weight integrity, or pre-trained-model validation, and the framework defines no AI equivalent of a software bill of materials. LiteLLM, Axios, and CPU-Z all arrived through legitimate software supply chain channels. AI workloads carry that same exposure one layer deeper, in the data and model artifacts that software trains on, which is exactly where the overlay is silent


## AI Runtime Threats


The attacks against AI workloads on HPC are not theoretical, and they are not detectable at the perimeter.


- Training data poisoning


scales at rates most security teams are not equipped to respond to. Research1 across 41 studies documents attack success rates exceeding 60 percent from manipulation of 100 to 500 training samples, a fraction of a percent of a typical dataset. Poisoning as little as 3 percent2 of training data achieved 41 percent attack success rates in code-generating models. OWASP’s LLM Top 103 documents the consequence. Backdoors leave model behavior intact until a specific trigger activates adversarial outputs. The model ships, it gets deployed, and operates correctly, until it doesn’t. No post-training audit reliably catches a well-designed poisoning attack.
- GPU side-channel attacks


are executed remotely by a co-tenant workload on shared GPU infrastructure; no physical access is required. The[NVBleed](https://arxiv.org/abs/2503.17847) research demonstrated covert channel attacks on NVIDIA NVLink, achieving over 91 percent accuracy in recovering data-dependent information from co-tenant GPU workloads on a shared fabric. The[BarraCUDA](https://arxiv.org/abs/2312.07783) research demonstrated the extraction of neural network weights via electromagnetic side channels from NVIDIA hardware. Both attack classes execute during active training, not at job transition. If your HPC environment runs multiple projects or security classifications on shared accelerators, the co-tenancy model is an active attack surface today.
- Inference pipeline compromise


survives load-time integrity checks. A model with clean weights at deployment faces attacks through three vectors: hot-swap modification of serving configurations while inference runs; preprocessing and postprocessing layer injection that alters inputs before they reach the model or modifies outputs before delivery; and adversarial input manipulation that triggers targeted misbehavior in a model that appears fully operational. For AI serving safety-critical inference, each is a security risk, not just a research concern.


The characteristic that makes AI workloads uniquely difficult is persistence. A compromised simulation may produce visibly wrong results, but a compromised *model* can produce correct results the overwhelming majority of the time and adversarially wrong results under precisely targeted conditions. By the time anyone has reason to investigate, the window for recovery has often closed.


## Securing HPC AI Workloads


We know the technology required to address these gaps exists and has been proven at scale in environments with performance constraints far tighter than those in HPC. What is needed is a well-defined architecture that enables the secure execution of large-scale AI workloads.


Dedicated security compute.


Runtime security that shares CPU resources with the workload it monitors can be starved of CPU time under heavy load and interfered with by a workload that achieves kernel-level access. The[SPiCa](https://www.reddit.com/r/cybersecurity/comments/1rv0yeb/bypassing_ebpf_evasion_in_state_of_the_art_linux/) research demonstrated that eBPF monitoring pipelines can be manipulated from within the kernel by rootkits filtering events before they reach the analysis engine, meaning that a co-scheduled monitor is not a reliable monitor.


Every other infrastructure function on an HPC node has dedicated resources. The job scheduler, the filesystem client, and the out-of-band management plane. Security monitoring is infrastructure and should be afforded the same dedicated resources.


Modern HPC nodes have 128 to 256 CPU cores. One reserved for security monitoring is less than one percent of the available compute. Linux kernel CPU isolation via` isolcpus` ,` nohz_full` , and` rcu_nocbs` is production-proven in high-frequency trading and real-time systems, with bounded, predictable overhead.


eBPF-based behavioral telemetry at the training layer.


Effective monitoring of an AI training pipeline means continuous observation of compute behavior profiles, memory access patterns, GPU utilization, and inter-node communication, with behavioral baselines established for approved training configurations. A poisoning attack that executes within expected statistical ranges is not visible to a perimeter scanner, but it is visible to a behavioral baseline that knows what the training job should look like.


This is the same principle that SentinelOne’s on-device Behavioral AI detected for LiteLLM, Axios, and CPU-Z. The LiteLLM detection flagged a Python interpreter executing Base64-decoded code in a spawned subprocess. The CPU-Z detection flagged an anomalous process chain:` cpuz_x64.exe` spawning PowerShell, which spawned` csc.exe` , which spawned` cvtres.exe` . CPU-Z doesn’t do that. The behavioral baseline knew what legitimate execution looked like, and in these cases, that behavior was the decisive signal.


[Cloudflare](https://blog.cloudflare.com/defending-the-internet-how-cloudflare-blocked-a-monumental-7-3-tbps-ddos/) uses an eBPF-based architecture to mitigate DDoS attacks exceeding 7 Tbps. SentinelOne uses it to detect and stop threats in under one second across enterprise fleets. A training job that begins writing to unexpected locations, establishing anomalous inter-node communication, or deviating from its expected compute profile is detectable at runtime, before the model completes training. The performance argument against runtime monitoring on HPC was never about the technology; it requires a shift in architecture.


Inference-time output monitoring.


Deployed models require continuous observation of output distributions, latency patterns, confidence score distributions, and input-output statistical properties. A model under adversarial input attack, or serving modified weights, exhibits detectable output patterns before any human analyst notices the outputs are wrong. Circuit-breaker logic needs to be designed into the serving architecture, not added after the first incident.


Model integrity verification that runs during inference.


Load-time attestation is a necessary and important requirement; it is not sufficient. Long-running inference deployments are vulnerable to hot-swap attacks that replace weights after the initial integrity check passes. Continuous cryptographic hash verification of loaded model weights, running on the dedicated security core with automated circuit-breaker logic on failure, closes that vector. For a model serving safety-critical calculations, the re-verification frequency should match the workload’s risk profile with predictable overhead.


An SR-family extension for the AI supply chain.


The existing SR controls address software supply chain risk. They do not address training data provenance, model weight integrity at ingestion, or pre-trained model validation. An AI bill of materials, including cryptographic documentation from the training data source through intermediate checkpoints to the deployed model, is the model-layer equivalent of software supply chain controls. Without it, every pre-trained model loaded into an HPC environment is an unverified artifact from an unverified chain.


## Defending AI at Every Layer


The supply chain attacks this spring demonstrated what happens when defense architecture falls behind the delivery mechanisms attackers use. LiteLLM, Axios, and CPU-Z all arrived through trusted channels, carrying payloads no signature database contained. They were stopped because behavioral detection does not require prior knowledge of the payload. It requires knowing what legitimate execution looks like and acting when execution deviates.


Defenders protecting AI workloads face that same problem across every layer they own. HPC is the hardest version of it. But identities, endpoints, applications, and infrastructure all carry the same exposure at different scales. SentinelOne gives defenders coverage across all four, with behavioral AI running at each layer to catch what signatures miss. The specifics of how that works across your AI environment are in our[AI security overview](https://sentinelone.com/solutions/secure-ai-apps-and-data) .


### Citations


1 “Data Poisoning 2018–2025: A Systematic Review. IACIS (2025)”, and “Data Poisoning Vulnerabilities Across Health Care AI Architectures. JMIR (2026)


2 “Poisoning Attacks on LLMs Require a Near-Constant Number of Poison Samples” (2025). arXiv:2510.07192 and Huang et al., 2020.


3 OWASP (2025) LLM04:2025 Data and Model Poisoning. OWASP Gen AI Security Project.
