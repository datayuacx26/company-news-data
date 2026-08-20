---
schema_version: "1.0.0"
document_id: "e605ddd0e99270828e8396c303a8162b71cb55243397e239bda63cd8ab2d0c41"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/confidential-ai-for-gitlab-self-hosted/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T21:02:44.719684+00:00"
fetched_at: "2026-08-06T21:02:45.243733+00:00"
content_hash: "sha256:fecf2584984ac299b65940160a8a9bd85c586f7278fcb768faf55483d1b648b8"
---

# Confidential AI for GitLab Self-Hosted

Your developers want AI coding agents. Your source code is regulated IP that can't be sent to a third-party AI service, and your compliance team has said so in writing. The usual escape hatch, standing up your own GPU cluster and running the models yourself, means buying scarce hardware, hiring a team to operate it, and still trailing the frontier models by months. So you do nothing, and your teams fall behind the ones that adopted AI a year ago.


GitLab Duo Self-Hosted can now use[Privatemode AI](https://www.privatemode.ai/) as its model provider. Privatemode runs state-of-the-art models inside confidential-computing hardware, so prompts, source code, and completions stay encrypted end-to-end, including during inference. You point GitLab Duo's AI Gateway at the Privatemode proxy, and your developers get the same Duo features they'd get from any model, with one difference that matters to a regulated organization: The code never leaves an encrypted boundary, and no third party can read it.


## The productivity most regulated teams can't touch


AI coding assistance has moved well past autocomplete.[GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) runs multi-step work: It reviews merge requests, refactors across files, generates and runs tests, drives agentic flows as CI jobs, and much more. That is a real shift in how teams ship, and it compounds. The teams using it review faster and spend less time on boilerplate.


The catch is that every one of those features sends your prompts and your source code to a model provider. For a team working on proprietary code, regulated software, or sensitive IP, that single fact is disqualifying.


## Why regulated organizations self-host


For these organizations, source code is core intellectual property, and where it gets processed is a contractual, sectoral, and regulatory question, not a convenience one. A bank's trading algorithms, a medical-device vendor's firmware, a defense contractor's control software: Sending that intellectual property to an external AI service can breach IP protections, data-processing agreements, and sector rules in one step.


The pressure comes from several directions at once. NIS2 and Digital Operational Resilience Act (DORA) raise the bar on operational resilience and third-party risk across critical infrastructure and financial services. GDPR governs how personal data in code and logs is processed. Sector regimes such as BaFin supervision in German finance, BSI C5 as a procurement baseline, and healthcare data rules add their own constraints. And many European organizations treat data sovereignty as a first-order requirement regardless of any single regulation.


The segments feeling this most are financial services, healthcare and pharma, defense, the public sector, and critical infrastructure. GitLab Self-Hosted exists precisely so this code never has to leave a trusted perimeter. Adding AI to the workflow shouldn't quietly undo that.


## Why AI makes self-hosting hard


A self-hosted team that wants AI coding assistance has three conventional options, and each one has costs the regulated buyer can't easily pay.


Public AI SaaS VPC / private-cloud AI Run your own LLM stack


Source code leaves your perimeter Yes Yes (to the operator) No


Guarantee type Contractual (terms of service) Contractual (operator + cloud) Self-enforced


Sees your plaintext The provider The operator and cloud provider Only you


Infrastructure burden None Low High: GPUs, model ops, updates


Keeps pace with frontier models Yes Usually Rarely, you lag the frontier


The comparison table shows that public AI SaaS and VPC options expose code and rely on contractual guarantees, while running your own stack keeps code private but carries heavy GPU and model-operations burden.


The honest summary: Public SaaS is off the table because the code leaves the building. A VPC-isolated or private-cloud service reduces exposure but still hands plaintext to the operator and the underlying cloud provider. At the same time, a contract is a promise, but not a cryptographic guarantee. Running your own models keeps the code private but is expensive to buy, expensive to staff, and structurally behind the best available models. Faced with that, most regulated organizations adopt AI narrowly or not at all.


The question becomes: How do you give developers modern AI coding agents without source code leaving the perimeter, and without operating an LLM stack yourself? Answering it takes a different kind of guarantee than a contract can provide, one enforced by hardware.


## Confidential computing: The architectural answer


Confidential computing keeps data encrypted *in memory* , while it's being processed, using a hardware-based trusted execution environment (TEE), an isolated region of a CPU or GPU that even the operating system, the hypervisor, and the machine's operator cannot read into. The relevant hardware here is AMD SEV or Intel TDX on the CPU side, paired with NVIDIA Confidential Computing on the GPU. Data is encrypted in transit and at rest with AES-256, and it's only ever decrypted *inside* the TEE.


The piece that turns this from a promise into a guarantee is **remote attestation** . Before a client sends any data, it asks the TEE to prove what it is: The hardware produces signed, cryptographic evidence of exactly what code is running inside the enclave. The client verifies that evidence against known-good values, and only then establishes the encrypted channel and sends its request. Verify first, send second.


The data-flow diagram shows prompts encrypted at the client, transmitted as ciphertext, and decrypted only inside a hardware TEE that neither the service operator nor the cloud provider can read.


The security property that follows is the whole point: Neither the AI service operator nor the underlying cloud provider can see your prompts, your completions, or your context. Not because they promise not to look, but because the hardware won't let them.


## Privatemode AI


[Privatemode AI](https://docs.privatemode.ai/) is built by[Edgeless Systems](https://edgeless.systems/) , a Germany-based specialist in confidential-computing software.[According to NVIDIA](https://images.nvidia.com/aem-dam/Solutions/Data-Center/confidential-computing/hpc-solution-brief-edgeless-systems-3427281.pdf?ncid=no-ncid) , Privatemode is the "first gen AI framework that keeps prompts encrypted at all times."


For the developer, the mechanics are invisible. Privatemode exposes an OpenAI-compatible API, and a client-side proxy handles attestation and encryption transparently, so tools and SDKs that speak the standard` /v1` API work unchanged. The current flagship model for coding is Kimi K2.6 with 256K context, with others like Kimi K3 and GLM coming soon.


This isn't a lab experiment. Privatemode is already in production with organizations that have exactly the constraints described above. Capgemini uses it for regulated-industry clients, including confidential coding. The German Federal Employment Agency runs it in the public sector. And banks, insurers, and defense organizations use it where source code and data cannot leave a trusted boundary. Its cryptography is post-quantum-safe, which also defends against "harvest now, decrypt later" attacks. An adversary capturing today's ciphertext in the hope of decrypting it with tomorrow's hardware gets nothing usable.


## How the integration works


GitLab Duo Self-Hosted lets you serve Duo's AI features from a model you control instead of a GitLab-managed gateway. Privatemode slots in as that model.


The path is straightforward. GitLab Duo talks to a self-hosted **AI Gateway** , which forwards inference to any OpenAI-compatible endpoint. The Privatemode proxy *is* that endpoint: It exposes a standard` /v1` API and handles encryption and attestation against the Privatemode service. When a request comes through, the proxy encrypts it before it leaves your network, verifies the remote TEE through attestation, and only then forwards it. Decryption happens exclusively inside the confidential-computing environment. Inside Duo, across Code Suggestions, Chat, Code Review, and agentic flows, the developer experience is unchanged.


```text
sequenceDiagram
autonumber
actor Dev as Developer /<br>Duo Agent Platform
participant GW as Self-hosted GitLab<br>AI Gateway
participant Proxy as Privatemode<br>Proxy
participant TEE as Remote TEE<br>(CPU + GPU)


Dev->>GW: Inference request (Code<br>Suggestions, Chat,<br>Code Review, flows)
GW->>Proxy: Forward request<br>to OpenAI-compatible<br>endpoint


rect rgb(255, 244, 214)
Note over Proxy,TEE: Attestation before any code is sent
Proxy->>TEE: Request attestation evidence
TEE-->>Proxy: Signed evidence of enclave code
Proxy->>Proxy: Verify evidence against known-good values
Proxy->>TEE: Establish encrypted channel (AES-256, post-quantum-safe)
end


Proxy->>TEE: Forward encrypted request
Note over TEE: Decryption + inference<br>happen only inside the enclave
TEE-->>Proxy: Encrypted response
Proxy-->>GW: Encrypted response
GW-->>Dev: Completion (developer<br>experience unchanged)


```


In the sequence diagram, a GitLab Duo request flows through the self-hosted AI Gateway to the Privatemode proxy, which attests the remote enclave and establishes an encrypted channel before forwarding the request for inference inside the trusted execution environment. You run the AI Gateway and the proxy yourself, and the setup is deployment-agnostic (Linux package, Docker, or Kubernetes). The steps are documented and version-specific, so rather than reproduce them here, see[Privatemode's GitLab Duo integration guide](https://www.privatemode.ai/coding-tool/gitlab-duo) .


## The tradeoffs


No architecture is free, and a technical reader deserves the costs stated plainly.


**You operate the gateway and the proxy.** This is real infrastructure work: deploying the AI Gateway at a proper hostname, matching its image version to your GitLab release on every upgrade, configuring JWT keys and certificates, and keeping the proxy reachable. It is ordinary infrastructure operation, though, not GPU procurement and not model operations. That distinction is the point. The alternative that keeps code private, running your own LLM stack, means buying scarce GPUs and staffing a team to run and update models while lagging the frontier. Privatemode is a hosted SaaS that carries the model operations for you, and the burden you keep is a proxy and a gateway.


**Generic OpenAI-compatible models are still maturing** in GitLab Duo Self-Hosted. Today, they run as a beta feature, so support for model-specific issues isn't yet guaranteed. It's on the roadmap to harden, but if you need a fully supported configuration now, factor that timeline into your rollout.


**Attestation and encryption add some overhead.** The extra roundtrip and cryptographic work cost time per request. In our testing, this hasn't been a practical problem, but the integration guide does recommend raising the AI Gateway timeout for slower self-hosted backends, so it's worth measuring against your own latency budget.


**You still trust something, but something bounded.** The guarantee rests on the hardware TEE and the attestation chain being sound. That's a smaller, more inspectable trust assumption than "trust the operator not to look," and you can verify the attestation evidence yourself, but it isn't zero. Confidential computing narrows trust to the silicon. It doesn't eliminate it.


## What this means in practice


- **For regulated organizations:** a path to modern AI coding agents that fits existing IP- and data-protection policy, because the code stays inside a hardware-encrypted boundary, and without buying GPUs or standing up model operations.
- **For developers:** the same GitLab Duo experience, the same or better models, and no new tooling. Because model selection lives on the instance, in-editor Chat and Code Suggestions through the GitLab Workflow extension inherit the same confidential setup.
- **For compliance and security teams:** the architecture aligns with the direction of BSI C5:2026 toward confidential computing and remote attestation, and it gives you concrete technical evidence, namely data encrypted in use and attested execution, to bring to NIS2, DORA, and GDPR conversations. It doesn't make you compliant on its own, because compliance is organizational and ongoing. It removes the "our code would leave the perimeter" objection that blocks AI adoption in the first place.


## Get started with Privatemode


Privatemode as a GitLab Duo self-hosted model provider requires GitLab Self-Managed, Premium, or Ultimate, Version 17.9 or later, with the GitLab Duo Enterprise add-on. Agentic flows additionally need the GitLab Duo Agent Platform (generally available on Version 18.8+). Usage consumes GitLab credits under the bring-your-own-model rules and multipliers described in the[GitLab credits documentation](https://docs.gitlab.com/subscriptions/gitlab_credits/) .


## Read more


- [Privatemode documentation](https://docs.privatemode.ai/)
- [GitLab Duo integration guide](https://www.privatemode.ai/coding-tool/gitlab-duo)
- [Coding assistants guide](https://docs.privatemode.ai/guides/coding-assistants/)
- [Capgemini case study](https://www.privatemode.ai/case-studies/capgemini-case-study)
- [NVIDIA solution brief](https://images.nvidia.com/aem-dam/Solutions/Data-Center/confidential-computing/hpc-solution-brief-edgeless-systems-3427281.pdf?ncid=no-ncid)
