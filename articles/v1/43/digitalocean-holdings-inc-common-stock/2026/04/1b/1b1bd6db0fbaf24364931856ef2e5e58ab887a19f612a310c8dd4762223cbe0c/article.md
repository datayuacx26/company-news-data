---
schema_version: "1.0.0"
document_id: "1b1bd6db0fbaf24364931856ef2e5e58ab887a19f612a310c8dd4762223cbe0c"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-news-import-6460f63f60af"
canonical_url: "https://investors.digitalocean.com/news/news-details/2026/DigitalOcean-Unveils-AI-Native-Cloud-Built-for-the-Inference-Era/default.aspx"
published_at: "2026-04-28T15:30:00+00:00"
first_seen_at: "2026-07-21T16:29:05.431888+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:10dae0d49ed6b91a229bc87a4cc6d968f6541c721e858e9ded97f1bbf85aaac0"
---

# DigitalOcean Unveils AI-Native Cloud Built for the Inference Era

[View All News](https://investors.digitalocean.com/news/default.aspx)


### DigitalOcean Unveils AI-Native Cloud Built for the Inference Era


April 28, 2026


*AI workloads have outgrown the clouds built for the last era. DigitalOcean’s AI-Native Cloud brings agents, data, inference, cloud primitives, and cloud infrastructure together in one integrated stack, helping builders start fast, scale production workloads, and improve unit economics without stitching together fragmented services.*


SAN FRANCISCO--(BUSINESS WIRE)--


[DigitalOcean](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fwww.digitalocean.com%2F&esheet=54524745&newsitemid=20260428061753&lan=en-US&anchor=DigitalOcean&index=1&md5=c71d61a61b5ab132dde58a0029728319) (NYSE: DOCN) today introduced the DigitalOcean AI-Native Cloud, the first cloud built end-to-end for the inference and agentic era. The integrated platform spans five layers: infrastructure, core cloud, inference, data, and managed agents, and is already running production workloads at Higgsfield AI, Hippocratic AI, ISMG, Bright Data, and LawVo. The platform debuts at[Deploy 2026](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fwww.digitalocean.com%2Fdeploy&esheet=54524745&newsitemid=20260428061753&lan=en-US&anchor=Deploy+2026&index=2&md5=eb744e5a14818eac82c2db6f5fdff8ac) , the company's conference for builders, and is available to customers today.


AI-native builders are caught between imperfect options: hyperscalers built for the enterprise cloud era, with complex services and unpredictable costs, and newer GPU clouds that rent bare metal and tokens, but leave teams to assemble the surrounding platform themselves. Both approaches add complexity when AI companies need to move faster, control costs, and scale production AI efficiently. DigitalOcean’s AI-Native Cloud is purpose-built for production AI, bringing the full AI application stack together with the best of the AI ecosystem into one, developer-first platform.


**AI workloads have outgrown the last era's cloud**


The DigitalOcean AI-Native Cloud is engineered for the four shifts redefining production AI: the rise of inference over training, reasoning models as the default, autonomous agents at scale, and open-source models reaching quality parity at a fraction of the cost.


These shifts change what infrastructure has to do. A typical agentic task can consume hundreds of model calls, hundreds of database queries, and over a million tokens. 50 to 90% of that workload runs on CPUs, not GPUs, requiring orchestration, sandboxes, state, and tool calls. Agentic systems consume approximately 4x more CPU capacity than equivalent traditional workloads, and consume 15x more tokens than human users.


**DigitalOcean's answer: a five-layer stack, from infrastructure to agents**


Five layers, one integrated platform, enabling builders to spend their time on AI, not on stitching disparate services and infrastructure together:


- **Managed Agents:** Open agent harness support, secure sandboxes, durable state management, and agent orchestration.
- **Data and Learning:** PostgreSQL with pgvector, Valkey, Knowledge Bases, and real-time data capabilities.
- **Inference Engine:** Serverless and dedicated endpoints, batch processing, an intelligent model router, a growing model catalog, and bring-your-own-model support, with custom vLLM forks, tuned KV-cache, speculative decoding, and GPU-aware scheduling under the hood.
- **Core Cloud:** Kubernetes (DOKS), CPU and GPU Droplets, VPC networking, and S3-compatible object, block, and file storage.
- **Infrastructure:** 20 global data centers of CPU and GPU capacity purpose built for AI, including owned NVIDIA H100, H200, and HGX™ B300 and AMD Instinct™ MI300X, MI350X, and MI355X GPUs on a 400G RoCE RDMA fabric, backed by 15 years of operating cloud at scale for more than 640,000 customers.


Across the platform, DigitalOcean's analysis of a representative 1M-bookings/month corporate-travel agent workload prices the AI-Native Cloud at $67,727 per month, compared to $84,827 on Baseten + AWS and $110,337 on AWS AgentCore. That 20-40% savings comes with no egress fees between layers and transparent, consumption-based pricing.


**Open source throughout the stack, frontier models when you need them, bringing builders the best of the AI ecosystem**


DigitalOcean’s AI-Native Cloud supports open standards and open-source technologies at every layer, because lock-in is the single biggest tax on AI builders today: OpenCode and LangGraph for agent harnesses; PostgreSQL, MySQL, pgvector, and Qdrant for data; DeepSeek, Llama, Qwen, and NVIDIA Nemotron 3 Nano Omni alongside frontier closed models like Claude and GPT for inference; and Kubernetes, Cilium, and S3-compatible storage at the cloud primitive layer. Customers can mix open and closed models in a single application, route between them dynamically, and switch when something better ships — without rewriting their stack.


"Open models are giving builders more choice in how they build AI applications," said Kari Briski, Vice President of Generative AI Software at NVIDIA. "AI companies need agents that can run continuously and improve over time. Our work with DigitalOcean brings NVIDIA Nemotron models to an open, full-stack platform that gives developers the infrastructure to build, deploy, and scale real-world AI applications more easily."


**Customers are improving performance and unit economics on DigitalOcean’s AI-Native Cloud**


AI teams see these platform gains translate into production outcomes. Information Security Media Group (ISMG) cut infrastructure costs over 5x by consolidating on DigitalOcean. Different workloads, different stakes, same platform. Bright Data scaled from 4,000 Droplets to 75,000 vCPUs in eight months while moving 765 petabytes of egress in a single month. And Higgsfield AI runs the multi-model creative workflows powering its consumer product on DigitalOcean's integrated stack:


“At Higgsfield, we are building for a world where AI-generated content becomes part of everyday creative work. That requires more than access to GPUs or models; we need an AI-native platform that can support fast iteration, multi-model workflows, and production scale,” explained Alex Mashrabov, CEO & Co-founder, Higgsfield AI. “DigitalOcean's integrated cloud provides the infrastructure, inference, and simplicity we need to move quickly while staying focused on the creative experience for our users.”


**Delivering the AI-Native Cloud with notable launches**


The AI-Native Cloud arrives with 15+ new general availability and preview launches across the stack,[detailed here](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fwww.digitalocean.com%2Fblog%2Fintroducing-digitalocean-ai-native-cloud&esheet=54524745&newsitemid=20260428061753&lan=en-US&anchor=detailed+here&index=3&md5=0d58fb456006d6d6a305f496a89c8078) . Highlights include:


- **Inference Router:** Developers define a model pool, describe tasks and priorities in natural language mapped to a model, and optimize each request for cost and latency. Powered by DigitalOcean’s purpose-built MoE (Mixture of Expert) router model, Early customers like LawVo, a legal-tech platform, runs 130+ AI agents against 500M+ tokens per week with a 42% inference cost reduction after switching with zero code changes.
- **Bring Your Own Model with Dedicated & Batch Inference:** Run custom or fine-tuned models across Serverless, Dedicated, or Batch Inference on the same OpenAI-compatible API. Dedicated Inference offers reserved per-GPU-hour pricing; Batch Inference cuts costs up to 50% with a 24-hour completion window.
- **Expanded Models and Services:** 70+ open-source and frontier models with day-zero access, discoverable through a centralized Model Catalog with clear pricing, performance, and hardware insights. New additions include NVIDIA Nemotron 3 Nano Omni (first on DigitalOcean), DeepSeek V3.2, Llama 3.3 70B, Qwen 3.5, and MiniMax M2. New Evaluations and Guardrails services round out production safety and quality monitoring.
- **Knowledge Bases:** A complete RAG pipeline exposed as an MCP tool. A RAG-native SaaS customer moved from prototype to production in nine days, with answer accuracy jumping from 71% to 94%.
- **Managed Weaviate:** A fully managed vector database for production AI workloads, with native integration to Knowledge Bases and the Inference Engine, eliminating the operational overhead of self-hosting Weaviate at scale.


**A market measured in trillions of tokens**


By 2030, the world is projected to process more than 500 trillion inference tokens per day, up from ~50 trillion today, a 10x increase in under five years. DigitalOcean is targeting three workload patterns with the AI-Native Cloud: Cloud-Native SaaS adding AI features; AI-Native products where every interaction burns tokens; and Agent-Native systems running autonomously in long loops.


“AI has moved from thinking to doing, and that changes what builders need from the cloud. AI-native companies are no longer building simple applications that make a single model call; they are building distributed, stateful, multi-agent systems that need infrastructure, inference, data, orchestration, and agents working together,” said Paddy Srinivasan, CEO, DigitalOcean. “DigitalOcean’s AI-Native Cloud brings those layers together on one integrated platform so teams can move faster, scale production AI, and focus on their products instead of stitching infrastructure together.”


Build, deploy, and scale AI-native applications on the cloud built for the Inference Era. Builders can[get started today](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fwww.digitalocean.com%2Fblog%2Fintroducing-digitalocean-ai-native-cloud&esheet=54524745&newsitemid=20260428061753&lan=en-US&anchor=get+started+today&index=4&md5=771e71118c54fb2ddab57919997a4bb4) with new GA and public preview products, or request access to private previews on the DigitalOcean AI-Native Cloud.


**About DigitalOcean**


DigitalOcean (NYSE: DOCN) is the AI-Native Cloud — a fully integrated platform spanning infrastructure, core cloud, inference, data, and managed agents, purpose-built for the workloads defining modern software. Serving more than 640,000 customers across 20 data centers in 5 global regions, DigitalOcean gives builders everything they need to ship production AI on one open platform with no lock-in. Learn more at[digitalocean.com](https://cts.businesswire.com/ct/CT?id=smartlink&url=http%3A%2F%2Fdigitalocean.com&esheet=54524745&newsitemid=20260428061753&lan=en-US&anchor=digitalocean.com&index=5&md5=ad83e79c25edd1bb06f4b35d1650ae3c) .


**Media Relations**
Meghan Grady
press@digitalocean.com


**Investor Relations**
Radu Patrichi, CFA
investors@digitalocean.com


Source: DigitalOcean


[View All News](https://investors.digitalocean.com/news/default.aspx)
