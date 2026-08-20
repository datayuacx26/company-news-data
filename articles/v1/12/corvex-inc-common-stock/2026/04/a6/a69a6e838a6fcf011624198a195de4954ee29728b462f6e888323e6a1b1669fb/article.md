---
schema_version: "1.0.0"
document_id: "a69a6e838a6fcf011624198a195de4954ee29728b462f6e888323e6a1b1669fb"
company_key: "corvex-inc-common-stock"
company: "Corvex Inc."
source_id: "corvex-inc-common-stock-rss-5d3752083407"
canonical_url: "https://www.corvex.ai/blog/secure-model-weights-from-corvex-protecting-ai-model-weights-in-the-cloud-with-confidential-containers-and-nvidia-gpus"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-26T12:16:19.295968+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:0e5b4e2740f5a6301431d1e2205843cd06f4f08866f579e410b9ab8da3a30365"
---

# Secure AI Model Weights in the Cloud with Corvex

Organizations that build frontier AI models face a structural challenge. They need third-party infrastructure to run inference at scale. But deploying to an external host means handing over their most valuable IP: weights that represent years of research and hundreds of millions of dollars in compute.


Today, the only thing preventing a host from reading those weights out of memory is policy. For models worth nine figures or more, policy is not enough.


Solving this requires hardware-enforced protection that spans the full execution environment: CPU, GPU, and the bus between them. It requires attestation that proves the environment is genuine before any keys are released. And it requires that the model owner, not the host, controls when and whether decryption happens.


That is what Corvex Secure Model Weights delivers, built on NVIDIA GPU confidential computing, Intel TDX, and the Confidential Containers (CoCo) open source project.


**Deploying with Confidence on Third-Party Infrastructure**


For organizations that own proprietary models, Secure Model Weights changes what external deployment means. They can run inference on any third-party infrastructure built on NVIDIA Hopper or Blackwell GPUs and Intel TDX CPUs, without trusting the operator. They retain exclusive control over key management at all times. Even with root access to every machine in the cluster, the host cannot read the weights. The guarantee is hardware-enforced, not policy-enforced.


That protection also unlocks entirely new markets. Regulated industries, e.g. healthcare, government, financial services, that have historically been out of reach for model builders deploying on shared infrastructure, because the security guarantees required simply did not exist. With hardware-enforced weight protection and independent key custody, model builders can now serve these customers directly, on properly built third-party infrastructure like Corvex GPU Clusters and AI Factories, without compromising control over their IP.


‍


**How It Works: Three Layers, One System**


This solution combines three layers of technology. Each one addresses a different part of the problem. Together, they form a complete chain of protection from attestation through key exchange through model loading, creating a fully scalable secure cluster for live inference.


‍


***NVIDIA GPU Confidential Computing***


The NVIDIA H100 was the world's first GPU with confidential computing capabilities. In CC mode, the host and hypervisor cannot access GPU memory. A hardware Compute Protected Region firewalls it off. Data crossing the PCIe bus is encrypted by the GPU's DMA engine. And the GPU can produce a cryptographic attestation report proving its identity and firmware integrity, so a model owner can verify the environment is genuine before releasing any sensitive data to it. For large model inference, this adds less than 7% overhead, approaching zero at the 70B parameter scale.


The NVIDIA Blackwell architecture (starting with B200 GPUs) extends this to multi-GPU workloads. Hopper required encrypted bounce buffers through unprotected memory for GPU-to-GPU communication, limiting throughput. NVIDIA Blackwell adds dedicated hardware for inline NVLink encryption, so organizations can run confidential multi-GPU inference at near-native speeds.


Together, these capabilities create a hardware-enforced boundary around the GPU at runtime. What remains is controlling who holds the keys and how the model gets from encrypted at rest to decrypted inside that boundary, without the key ever being exposed along the way.


‍


***Confidential Containers (CoCo)***


Hardware protection is only useful if it can scale to meet the massive throughput demands of enterprise AI. Confidential Containers is a sandbox project under the Cloud Native Computing Foundation (CNCF) that makes scaling that protection possible, moving workloads from isolated secure nodes into deployable containers.


CoCo brings hardware TEEs to containers through a standardized stack: VM-isolated pods via Kata Containers, attestation with native Intel TDX support, a key broker service (Trustee) for secure secret delivery, and support for GPU passthrough into confidential VMs. Instead of every organization building custom confidential computing infrastructure, teams deploy on upstream components capable of orchestrating highly scaled inference clusters that are collectively maintained and audited by the open source community.


CoCo securely delivers secrets into the TEE at the CPU level across the entire cluster. For GPU workloads, the key needs to go one step further: directly into GPU memory without ever existing in plaintext on the CPU.


‍


***Corvex Secure Model Weights: The Ultimate Guarantee***


NVIDIA CC mode and CoCo solve the hardware and scaling challenges, but one challenge remains: the ability to control who can unlock model weights. Corvex closes that gap, giving builders sole custody of their encryption keys and the ability to set their own attestation policy, so they can deploy on any qualifying infrastructure without ever having to trust the operator.


NVIDIA CC mode keeps the host out of GPU memory. CoCo makes it seamlessly scalable. Corvex adds the layer on top that puts the model owner in control: independent key custody.


The model builder encrypts their weights and retains sole control of the encryption key. That key is only released after remote attestation confirms the CPU, GPU, and software stack across the cluster are genuine. And when it is released, it is delivered directly into GPU memory. The model owner decides the attestation policy. The model owner holds the keys. The host never does.


‍


**What Comes Next**


Every layer in this system exists today, and each one is advancing. NVIDIA Hopper introduced GPU-level hardware protection. NVIDIA Blackwell extends it to multi-GPU with inline NVLink encryption. CoCo provides the ability to scale. Intel TDX provides the CPU-side attestation foundation. And Corvex adds independent key management that guarantees model owners have verifiable, hardware-enforced control over their most valuable assets.


Together, these layers enable organizations to deploy proprietary models on third-party infrastructure — from a single NVIDIA Hopper GPU to multi-GPU NVIDIA Blackwell clusters — with protection that is hardware-enforced, attestation-verified, and owner-controlled. It is the first secure cluster on the market serving inference, and it changes the baseline for how frontier models are deployed.


Corvex and NVIDIA are collaborating to advance this work and contributing back to the ecosystem. Corvex is working with the CoCo Trustee project to contribute a reference implementation for GPU-memory-only key delivery using ML-KEM, so the broader community can build on it.


‍


**Security for the AI Era**


Secure Model Weights from Corvex, built on NVIDIA Confidential Computing and Intel TDX, delivers something the industry has never had before: a complete, hardware-enforced chain of protection from attestation through key exchange through inference at enterprise scale. For model builders, that means proprietary weights stay under their exclusive control even when running on infrastructure they don't own, unlocking access to the most cost-effective GPU capacity available without trading away their most valuable IP, and opening doors to regulated markets that were previously out of reach.


These capabilities don't just reduce risk. They expand what's commercially possible for innovators deploying frontier AI.


#### ‍


#### [Talk to us](https://www.corvex.ai/talk-to-us) to learn more about how Corvex Secure Model Weights and confidential computing can improve your AI security.
