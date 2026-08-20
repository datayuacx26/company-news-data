---
schema_version: "1.0.0"
document_id: "477aa475f87b383182e6d1b58a604618cbed293927232d201cc59d05e4c17871"
company_key: "western-digital-corporation-common-stock"
company: "Western Digital Corporation"
source_id: "western-digital-corporation-common-stock-rss-a1b03adbb2f1"
canonical_url: "https://blog.westerndigital.com/why-hdd-is-essential-to-ai-economy/"
published_at: "2026-06-16T19:00:39+00:00"
first_seen_at: "2026-07-20T23:21:38.045244+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:b7741aff21287a44b4b4c9c301b9c7c246ae982b49cea8412428a7793514c55e"
---

# Why HDD Is Essential to the AI Economy

June 16, 2026


7 min read


[Technology](https://blog.westerndigital.com/category/technology/)


# Why HDD Is Essential to the AI Economy


[Xiaodong Che](https://blog.westerndigital.com/?guest_author=xiaodong-che)


*Reframing Jensen Huang’s Five-Layer Cake Through the Lens of the Token Economy*


## Key Takeaways


- AI is often framed as a compute problem. In reality, it is a data system.
- AI runs bottom-up in physics and top-down in economics. Power is the physical foundation of the stack, but monetized applications fund it.
- If the application layer monetizes useful tokens, then storage becomes the medium that preserves those tokens and their derivatives—checkpoints, logs, media, enterprise memory, and digital twins—over time.
- Compute generates intelligence—but data persists, accumulates, and compounds.
- Memory and SSD help AI think fast; HDD remembers AI creations at scale.


## Jensen Huang’s five-layer cake is missing a layer


Jensen Huang recently described AI infrastructure as a five-layer stack: energy, chips, infrastructure, models, and applications.1 NVIDIA’s AI factory materials use the same structure and say AI factories are built to optimize token generation—the fundamental unit of AI.2


This framing is useful because it treats AI not as a single model or a cluster of GPUs, but as a full industrial system that clearly explains how AI is generated. It is also missing a critical sixth layer—storage.


Energy supplies the physical base; chips convert electricity into computation; infrastructure operationalizes that computation at data-center scale; models turn compute into intelligence; and applications convert intelligence into user value.1,2


This missing layer acts as the experience repository for AI systems in preserving the data, outputs, context, and artifacts that allow intelligence to persist, compound, and improve over time—all thanks to the new storage layer.


## AI must be understood as full infrastructure


NVIDIA’s language around AI factories is revealing: the objective is not merely to deploy accelerators, but to manufacture intelligence at scale and optimize token generation.2,3 That means every layer in the stack matters. Power availability constrains deployment, chips constrain raw throughput, infrastructure constrains utilization, models constrain intelligence quality, and applications constrain monetizable value.


This systems view helps explain why AI is increasingly discussed in the same breath as electricity, telecom, and cloud infrastructure. It is a capital-intensive stack whose performance depends on coordination across multiple layers rather than on any single breakthrough in isolation.1,4


## Economics sustain the AI stack


Economically, the stack works in reverse: applications generate the economic value that funds the layers beneath them. Energy costs money. Chips cost money. Infrastructure costs money. Model development and operation cost money. The application layer is where customers actually pay for useful outcomes. Huang has explicitly pointed to the application layer as the place where economic benefit happens.4


This inversion leads to a more useful economic interpretation of the five-layer cake: power keeps AI running physically, but applications keep AI running economically.


## Token generation as economic output


NVIDIA’s recent AI-factory framing sharpens this idea by linking token throughput to economic performance. The company describes tokens as the fundamental unit of AI and has also tied token throughput per megawatt to AI-factory revenue potential.2,6


Not every business model charges directly per token, but token generation is still a useful economic abstraction. In code generation, tokens become software. Language-model tokens become reports or decisions, multimodal tokens become images, music, and video, and industrial AI outputs become predictions, actions, or digital twins.7


In that sense, token generation is not only a technical metric; it is the act through which AI becomes economically productive.


## Why storage matters to the token economy


If token generation is the productive act of the AI economy, storage is what makes that production durable. NVIDIA’s AI Data Platform materials stress that the data feeding enterprise AI is often unstructured, fragmented, and rapidly changing.8


AI needs storage for at least five reasons: large training corpora, model checkpoints, enterprise retrieval and agentic workflows, governance and retention requirements, and the preservation of AI-created artifacts.7,9,10


To support token generation and AI reasoning, a new layer within storage is also emerging: vector databases. While an object such as an image, video, or document may be stored in traditional filesystem or object store, the semantic representation of that content is increasingly stored as vector data that helps AI systems retrieve, reason over, and contextualize information without repeatedly processing the original object itself.


This shift is reshaping storage architecture for AI. Compute-intensive AI systems continue to drive memory growth, while AI data stored across objects and vectors is increasingly driving growth across both flash and HDD infrastructure tiers.


We believe IDC’s Global DataSphere program points toward continued expansion into the zettabyte era—reinforcing that storage is no longer simply a passive repository beneath the AI stack, but an active infrastructure layer that enables AI systems to retain context and scale economically over time.11


## Schematic: The five-layer cake in physics and economics


*Figure 1. The same five-layer stack can be read bottom-up in physics and top-down in economics.*


## Flash helps AI think fast—HDD ensures AI remembers at scale


AI absolutely requires fast tiers. Memory, NVMe, and SSD-based storage are essential for low-latency inference, active retrieval, and checkpoint performance in large-scale training.8,9


But the economics of AI are not determined only by the hottest data. They are determined by the full stock of data that must be retained, revisited, governed, or reused over time. Public cloud guidance still distinguishes SSD and HDD tiers by workload profile and cost profile; AWS describes Cold HDD as its lowest-cost block storage option for infrequently accessed data.12


As data creation, capture, and replication continue to expand, the critical question is no longer only “How do we compute fast enough?” It is also “How do we remember enough, cheaply enough, for long enough?”11


The token economy does not end when a token is generated. In many cases, its value begins there. Useful outputs often need to be stored, referenced, audited, transformed, retrieved later, or reused as future context and training material.


That is true for enterprise knowledge, software artifacts, checkpoints, logs, synthetic data, media assets, scientific outputs, industrial telemetry, and digital twins.8,9,10


> Memory and SSD enable AI systems to think and respond at speed. HDD enables AI-generated value to persist economically at scale.


HDD matters not because it replaces flash, but because it complements it. Flash accelerates active AI workloads, while HDD provides the durable and economically scalable foundation required to retrain the expanding stock of AI-generated and AI-relevant data over time.


## Conclusion


Jensen’s five-layer cake is a useful model because it captures AI as a full infrastructure system rather than a single technology wave.1 But as AI systems scale and deliver economical values, the model increasingly requires a key ingredient: storage.


AI infrastructure is no longer defined solely by compute performance. It is increasingly defined by how efficiently systems can retain, manage, and scale the growing body of AI data over time.


That shift is why storage is foundational to the future of AI infrastructure—and why HDD remains essential to the AI economy.


1. NVIDIA Blog. “AI Is a 5-Layer Cake.” March 10, 2026.[https://blogs.nvidia.com/blog/ai-5-layer-cake/](https://blogs.nvidia.com/blog/ai-5-layer-cake/)
2. NVIDIA. “AI Factories.” Accessed April 1, 2026.[https://www.nvidia.com/en-us/solutions/ai-factories/](https://www.nvidia.com/en-us/solutions/ai-factories/)
3. NVIDIA Glossary. “What Is an AI Factory?” Accessed April 1, 2026.[https://www.nvidia.com/en-us/glossary/ai-factory/](https://www.nvidia.com/en-us/glossary/ai-factory/)
4. NVIDIA Blog. “Largest Infrastructure Buildout in Human History: Jensen Huang on AI, Energy, and Industry at Davos.” January 2026.[https://blogs.nvidia.com/blog/davos-wef-blackrock-ceo-larry-fink-jensen-huang/](https://blogs.nvidia.com/blog/davos-wef-blackrock-ceo-larry-fink-jensen-huang/)
5. EPRI. “Powering Intelligence 2026: Executive Summary.” 2026.[https://powering-intelligence.epri.com/executive-summary.html](https://powering-intelligence.epri.com/executive-summary.html)
6. NVIDIA Developer Blog. “Scaling Token Factory Revenue and AI Efficiency by Maximizing Performance per Watt.” March 2026.[https://developer.nvidia.com/blog/scaling-token-factory-revenue-and-ai-efficiency-by-maximizing-performance-per-watt/](https://developer.nvidia.com/blog/scaling-token-factory-revenue-and-ai-efficiency-by-maximizing-performance-per-watt/)
7. U.S. Government Accountability Office. “Generative Artificial Intelligence: Uses, Benefits, and Risks.” 2024.[https://www.gao.gov/products/gao-24-106946](https://www.gao.gov/products/gao-24-106946)
8. NVIDIA. “AI Data Platform for Enterprise.” Accessed April 1, 2026.[https://www.nvidia.com/en-us/data-center/ai-data-platform/](https://www.nvidia.com/en-us/data-center/ai-data-platform/)
9. AWS Storage Blog. “Architecting Scalable Checkpoint Storage for Large-Scale ML Training on AWS.” June 16, 2025.[https://aws.amazon.com/blogs/storage/architecting-scalable-checkpoint-storage-for-large-scale-ml-training-on-aws/](https://aws.amazon.com/blogs/storage/architecting-scalable-checkpoint-storage-for-large-scale-ml-training-on-aws/)
10. Google Cloud. “Cloud Storage Controls for Generative AI Use Cases.” 2026.[https://docs.cloud.google.com/docs/security/security-best-practices-genai/storage-controls](https://docs.cloud.google.com/docs/security/security-best-practices-genai/storage-controls)
11. IDC. “Global DataSphere.” Accessed April 1, 2026.[https://my.idc.com/getdoc.jsp?containerId=IDC_P38353](https://my.idc.com/getdoc.jsp?containerId=IDC_P38353)
12. AWS. “Amazon EBS Cold HDD Volumes.” Accessed April 1, 2026.[https://aws.amazon.com/ebs/cold-hdd/](https://aws.amazon.com/ebs/cold-hdd/)


[Artificial Intelligence](https://blog.westerndigital.com/tag/ai/)[Data at Scale](https://blog.westerndigital.com/tag/data-at-scale/)[Data Center](https://blog.westerndigital.com/tag/data-center/)[HDD](https://blog.westerndigital.com/tag/hdd/)
