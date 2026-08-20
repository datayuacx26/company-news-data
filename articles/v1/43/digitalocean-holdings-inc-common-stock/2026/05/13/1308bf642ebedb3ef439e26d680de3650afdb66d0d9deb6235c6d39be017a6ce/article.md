---
schema_version: "1.0.0"
document_id: "1308bf642ebedb3ef439e26d680de3650afdb66d0d9deb6235c6d39be017a6ce"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/powering-the-inference-era"
published_at: "2026-05-04T20:43:38.164+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:12:49.473456+00:00"
content_hash: "sha256:084ca1c9d56cc85a6b82223d279fd12d02862d7229b0c42053bc3140a41133ac"
---

# Powering the Inference Era: Inside the DigitalOcean AI-Native Cloud

I’ve spent the last fifteen years building cloud services: early days of AWS building S3 and EBS, helping launch Oracle Cloud Infrastructure from inception, and now building the agentic cloud at DigitalOcean for AI-natives. Every cloud I’ve worked on was designed for the workloads of its era. Those clouds were built for human-centric SaaS applications: a few users, a handful of requests per session, predictable data flows.


AI workloads break every one of those assumptions.


AI runs in loops. Agents think, then act, then think again. A single user task can span hundreds of thousands of tokens, traverse half a dozen tools, hit a knowledge base, write code, execute it, and persist state, all before returning an answer. The clouds we have weren’t built for this. Hyperscalers give you hundreds of services built for yesterday’s applications, and leave the integration to you. Inference-only providers sit on someone else’s compute and stack their margin on top. GPU rental shops (frequently referred to as “Neoclouds”) give you silicon, but not a system.


This week at[Deploy 2026](https://www.digitalocean.com/deploy) , we launched the DigitalOcean AI-Native Cloud, a purpose-built platform for the inference and agentic era that integrates five layers from silicon to agents into a single open stack.


We shipped fifteen products on Tuesday. Here’s what’s inside.


## The shape of the stack


Our AI-Native Cloud is composed of five layers, each addressing a real workload pattern we’ve watched our customers wrestle with.


They’re independently useful and beautifully integrated:


- **Managed Agents:** production runtime for agents, with sandboxes, durable state, and a universal data plane
- **Data & Learning:** managed databases, vector stores, knowledge bases, and feedback loops
- **Inference Engine:** every open and frontier model on one endpoint, optimized at the kernel
- **Core Cloud:** compute, networking, and storage primitives, tuned for AI
- **Infrastructure:** DigitalOcean-owned silicon and facilities, co-engineered with the industry’s best


Open source isn’t an add-on at any of these layers. It’s the foundation: PostgreSQL, MySQL, MongoDB, Valkey, OpenSearch, Kafka, Weaviate, vLLM, SGLang, OpenCode, LangGraph, CrewAI. Open all the way down. You bring your weights, your harness, your tools. We provide the runtime.


Let me walk through it, from the ground up.


## Infrastructure: own the silicon, own the economics


Our global footprint now spans **19 data centers** and 200+ network points of presence, with future capacity coming online in **Kansas City and Memphis** . That includes our first **liquid-cooled racks** , purpose-built for next-generation high-density GPU workloads.


Our[Richmond data center](https://www.digitalocean.com/blog/building-ai-factory-for-agentic-era-nvidia-gtc) **is now generally available** , with **NVIDIA HGX™ B300** and **AMD Instinct™ MI350X** GPUs available alongside the H100, H200, and MI300/MI325 silicon already running across our fleet. We co-engineer at the kernel level with both NVIDIA and AMD. We don’t rent capacity. We own it. That’s why your unit economics improve as you scale on us, instead of getting worse.


## Core Cloud: the foundation under every agent


Hundreds of thousands of customers already run on our core cloud every day: Droplets, Kubernetes (DOKS), VPC networking, and object/block/network file storage. We’ve extended it for AI workloads with a non-blocking RDMA fabric, RDMA-enabled NFS, and VPC-native inference out of the box.


At Deploy we announced Burstable CPU and MicroVM Droplets, currently in Private Preview. These are Firecracker-based instances that start in roughly 200 milliseconds, ideal for agent sandboxes and lightweight, spiky workloads. Agents need GPUs for thinking and CPUs for doing. We have both, and now they’re sized for how agents actually behave.


## Inference Engine: every model, one endpoint


This is the layer we’ve rebuilt from the ground up. We co-developed it with design partners like Hippocratic AI, and the result is one of the highest-performing inference engines on the market today:[fastest inference for Qwen 3.5 and DeepSeek V3.2](https://www.digitalocean.com/blog/how-we-built-fastest-deepseek-minimax-qwen-on-blackwell-ultra) in independent Artificial Analysis benchmarks for token throughput.


Here’s what’s new:


- **Inference Router** (Public Preview): a preference-aware control plane that picks the right model for each request, balancing cost, latency, and quality with no code changes
- **Dedicated Inference** (General Availability): reserved capacity with predictable performance and economics for production workloads
- **Bring Your Own Model (BYOM)** (General Availability): a service for hosting your fine-tunes on our serving stack and inherit the kernel-level optimizations
- **Multi-modal model support** (General Availability): text, vision, audio, and video on a single API
- **Batch Inference** (General Availability): purpose-built for asynchronous workloads (document processing, eval runs, synthetic data generation) at roughly 50% of peak serverless pricing
- **Content Safety Guardrails** (General Availability): policy controls integrated at the inference layer
- **Serverless Inference with multi-modal support** (General Availability): single API, scale to zero, pay only for tokens consumed
- **Evaluations** (Public Preview): automated scoring against golden datasets or built-in judge models, so you can swap models without flying blind


The Router deserves a closer look. It’s a preference-aware control plane that picks the best model for each request, balancing cost, latency, and quality without touching application code. Unlike static routing rules, it runs on a purpose-built small language model that resolves intent in 200 milliseconds and ranks candidates against live cost and latency data, so the right model wins at 2am and at 2pm. Most AI builders start on a single frontier model. Then PMF happens, the bill scales linearly with usage, and the unit economics get painful fast. Most successful AI natives we work with run three or more models in production. The leading edge is running twenty or more. The Router makes that possible without a rewrite.


Take[Celiums.AI](http://celiums.ai/) , across 29.2M tokens processed through the Inference Router, 83% of their traffic now lands on open-source models, up from zero.


> “Our AI Ethics Engine was built with open-source AI, so running it on closed-source models felt backwards. DigitalOcean’s Inference Router closed the loop: we swapped frontier closed-source models for open alternatives and cut per-token cost by 61% while pulling p95 latency under 400ms. Same API. Zero code changes. The Router routes to the optimal model on every request. We just build.”
>
>
> — **Mario Gutiérrez** CTO at Unity Financial Network and Founder of[Celiums.AI](http://celiums.ai/)


We also expanded the Model Catalog with over 25 new models, including:


- **NVIDIA Nemotron 3 Nano Omni**
- **DeepSeek V3.2**
- **Llama 3.3 70B**
- **Qwen 3.5**
- **MiniMax-M2.5**


## Data & Learning: AI-ready data, no rebuild required


Stateful agents need context, memory, and the ability to learn from what happens in production. The Data & Learning layer is built on the managed services tens of thousands of customers already trust, extended for how AI systems actually run.


What’s new:


- **Knowledge Bases** (General Availability): managed retrieval with grounded, cited answers; every knowledge base is exposed as an MCP tool by default
- **Learning & Feedback Loops** (General Availability): capture production signals and route them back into model improvement, without a separate data pipeline
- **Managed Weaviate** (Private Preview): open-source vector store, fully managed
- **PostgreSQL Advanced Edition** and **MySQL Advanced Edition** (Public Preview): capacity to 50 TiB, 1 TiB scaling in minutes, proxy-based failover in seconds, and 100+ observability metrics


Transactional databases remain the foundation for AI. We made them production-grade for the agentic era.


## Managed Agents: a production runtime, not a monolith


This is the newest layer of the stack, and the one where we’ve spent the most time listening. We’ve watched customers deploy tens of thousands of agents on App Platform as containers. We’ve also watched them hit a wall when the agent loop, tool calls, state, observability, and code execution all live tangled together inside a single monolith.


So we asked a simple question: what would help you actually move faster? The answer became Managed Agents: five primitives that separate the plumbing from the business logic of your agent.


What’s new:


- **Managed Agents** (General Availability): the production runtime
- **Open Harness** (General Availability): bring your own agent framework, including OpenCode, LangGraph, CrewAI, or any other harness
- **Managed Sandboxes** (General Availability): E2B-compatible, Firecracker-based, sub-second cold start for safe execution of model-generated code
- **Durable State Management** (General Availability): checkpoints and memory primitives the harness can rely on
- **Plano** (General Availability): our orchestration framework and data plane for agents, released under Apache 2.0
- **Launchpad** (General Availability): go from prototype to deployed agent in clicks
- **Model Context Protocol (MCP)** (General Availability): expanded support across the platform
- **ToolBox** (Coming Soon): 3,000+ tool connectors so your agents can act on the systems your business actually runs on


## The compounding effect of the full stack


Any single layer of this stack is useful on its own. The reason to run them together is that the optimization compounds.


When your agents, your inference, your data, and your compute live in the same VPC, on the same silicon, billed on the same invoice, you eliminate the egress taxes, the margin stacking, and the integration debt that come from stitching across three vendors and three bills.


We’ve seen customers like Workato run a trillion automation tasks at 67% lower cost.[Character.AI](http://character.ai/) handle over a billion queries a day at 2x inference throughput. LawVo cut inference costs 42% with no code changes by routing through us. Hippocratic AI is powering 20M+ patient interactions with 40% lower latency. None of these are demos. They’re production workloads at scale.


## Start here. Scale here.


If you’re an AI builder, whether you’re writing your first line of code or accelerating past product-market fit, this stack is for you. You don’t need to wait in a hyperscaler queue behind a frontier lab. You don’t need to glue together a Neo Cloud, an inference wrapper, and a vector database vendor. You don’t need to compromise on openness, on economics, or on developer experience.


Welcome to the AI-Native Cloud. Let’s build.


→[Get started at digitalocean.com](https://www.digitalocean.com/)


---


Read the full[Deploy 2026 announcement and CEO Paddy Srinivasan’s perspective on why this moment requires a new cloud](https://www.digitalocean.com/blog/introducing-digitalocean-ai-native-cloud) .
