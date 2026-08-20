---
schema_version: "1.0.0"
document_id: "ecb12b78aaa6e7244168cc3156ce52a92226c1b1071f2269290080a957c12737"
company_key: "bitdeer-technologies-group-class-a-ordinary-shares"
company: "Bitdeer Technologies Group"
source_id: "bitdeer-technologies-group-class-a-ordinary-shares-rss-2127a671e698"
canonical_url: "https://www.bitdeer.ai/en/blog/nvidia-nemotron-3-embed/"
published_at: "2026-07-16T16:03:01+00:00"
first_seen_at: "2026-07-24T08:23:43.073205+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:9181507dc515ca4dde026da14efb46d68b5674d2e321c916d8632a2f9cf4c5b0"
---

# Day 0 Availability: Build Smarter Retrieval for Agents with NVIDIA Nemotron 3 Embed on Bitdeer AI Model Studio

When an AI agent gives a wrong answer, it’s easy to blame the model's reasoning. But in production agentic systems, the failure often happens one step earlier—with retrieval. If the retrieval layer misses the right passage, code file, policy, or customer-specific record, the downstream agent starts from the wrong context and no amount of reasoning power can recover from a bad starting point.


And agents retrieve constantly. They decompose tasks into multiple queries, rewrite queries until the right data surfaces, search memory, and inspect code often dozens of times within a single task. Every weak retrieval adds turns, tokens, latency, and hallucination risk. As enterprises move from simple semantic search to retrieval-driven AI systems, retrieval quality has become the control point for accuracy, cost, and trust.


Today,[NVIDIA Nemotron 3 Embed](http://huggingface.co/blog/nvidia/nemotron-3-embed-wins-rteb?ref=bitdeer.ai) **8B** and **NVIDIA Nemotron 3 Embed 1B** are available on[Bitdeer AI Model Studio](https://www.bitdeer.ai/en/home?ref=bitdeer.ai) , bringing production-ready open embedding models for RAG, enterprise search, code retrieval, and agentic workflows. You can immediately deploy the models through Bitdeer AI Model Studio’s serverless inference platform, built for secure, scalable enterprise AI.


## **What is NVIDIA Nemotron 3 Embed?**


[Nemotron 3 Embed](http://huggingface.co/blog/nvidia/nemotron-3-embed-wins-rteb?ref=bitdeer.ai) is a collection of open embedding models designed for enterprise and ISV teams building retrieval systems for enterprise search and emerging agentic workflows. The launch pairs two models on a single, practical accuracy-efficiency curve:


- **Nemotron 3 Embed 8B** — a frontier-quality embedding model designed to establish the accuracy ceiling across major retrieval benchmarks. The model topped the RTEB leaderboard overall across both open and closed embedding models.
- **Nemotron 3 Embed 1B** — an efficient embedding model designed to retain more than 95% of the 8B model's accuracy through pruning, distillation, and quantization-aware training (QAT), built for high-volume production workloads that need strong accuracy with substantially better throughput and serving efficiency.


The design philosophy: use the 8B model where maximum retrieval quality matters, and the 1B model where high-volume workloads demand efficiency. Together, they let teams improve retrieval quality without impractical compute tradeoffs.


## **Key Specifications**


Property


Details


**Models** ****


Nemotron 3 Embed 8B / Nemotron 3 Embed 1B (nemotron-3-embed-8b / nemotron-3-embed-1b)


**Architecture** ****


Transformer (Ministral-3-3B-Instruct-2512 based pruned model)


**Modalities** ****


Text-only input and output, including code (multimodal planned for a future release)


**Context Length** ****


32K tokens


**Quantization** ****


8B: BF16 · 1B: BF16 & NVFP4


**Openness** ****


Open model weights, datasets, training recipes, and fine-tuning guidance


**NVIDIA Technology** ****


NeMo Retriever Library, NVIDIA NIM, NeMo AutoModel, NVIDIA Model Opt


**Supported GPUs** ****


H100, RTX Pro 6000 Blackwell, GB200


## **Why Retrieval Quality Now Decides Agent Quality**


Most enterprise retrieval stacks face a set of hard tradeoffs:


- **Accuracy vs. cost.** Better retrieval usually means paying more per query.
- **Accuracy vs. model size.** Larger embedding models can improve retrieval quality, but they are harder to serve at scale.
- **Latency vs. recall.** Production systems need fast query response, but high-quality retrieval often means searching large volumes of content.
- **Openness vs. production readiness.** Open models can be customized, but teams still need tested deployment paths, quantization support, and enterprise-grade serving.


This tradeoff is most visible in agentic systems. Multi-turn agents retrieve repeatedly for planning, memory, code, and tool-use context. Weak retrieval increases turn count, token usage, hallucination risk, and user frustration. Strong retrieval reduces irrelevant context and keeps agents grounded. For retrieval to become a default agent capability, embedding models need to feel almost as easy to use as file search: fast, inexpensive, accurate, and available without major compute tradeoffs.


Nemotron 3 Embed addresses these tradeoffs head-on:


**Frontier accuracy for retrieval agents.** Nemotron 3 Embed models deliver frontier retrieval accuracy across enterprise search, RAG, code retrieval, and agentic retrieval workflows. Nemotron 3 Embed 8B tops the RTEB leaderboard over other open and closed embedding models, and is designed to achieve the highest agentic retrieval accuracy with the fewest tokens used among open embedding models.


**Efficiency without impractical compute tradeoffs.** Nemotron 3 Embed 1B retains more than 95% of the 8B model's accuracy through pruning, distillation, and quantization-aware training, and NVFP4 support on NVIDIA Blackwell doubles throughput. This makes semantic search fast and inexpensive enough for agents to retrieve constantly, almost like file search. On NVIDIA Blackwell GPUs, the NVFP4 version delivers 2x higher throughput while retaining 99% of BF16 accuracy.


**Open and transparent, from weights to recipes.** Nemotron 3 Embed ships with open model weights, datasets, training recipes, optimization techniques, and fine-tuning guidance. Teams can inspect how the model was built, fine-tune it for domain-specific retrieval, and deploy it with full control over their data and infrastructure: no black box, no lock-in.


## **Enterprise Use Cases**


**Agentic retrieval.** Give agents a stronger retrieval layer for multi-turn planning, tool use, memory, and repeated lookup loops. Nemotron 3 Embed supports query decomposition where an agent breaks one user request into multiple retrieval queries and query rewriting, where an agent reformulates a query multiple times until the right data is retrieved.


**RAG and enterprise search.** Ground copilots and RAG pipelines in the right enterprise context documents, knowledge bases, and policies with frontier retrieval accuracy that reduces irrelevant context and wasted tokens downstream.


**Code retrieval.** Retrieve relevant source files, functions, and implementation examples for developer copilots and software engineering agents. With text and code embedding in one model, engineering teams can power code search and coding-agent context from a single retrieval layer.


## **Run Nemotron 3 Embed via API on Bitdeer AI Model Studio**


You can run Nemotron 3 Embed on[Bitdeer AI Model Studio](https://account.bitdeer.com/en/sign_in?method=3&service=https://www.bitdeer.ai/auth&ref=bitdeer.ai) , our serverless inference platform designed to make access to advanced foundation models simple and scalable. With a unified API, Model Studio allows developers and enterprises to start using models quickly without managing underlying infrastructure, reducing deployment complexity and time to value.


Bitdeer AI is a preferred[NVIDIA Cloud Partner](https://www.nvidia.com/en-us/data-center/gpu-cloud-computing/partners/?ref=bitdeer.ai) , certified to ISO/IEC 27001:2022 and SOC2 Type I & Type II, providing the secure, compliant, high-performance, and enterprise-grade infrastructure that production retrieval and agentic AI deployments require. Run your embedding workloads at the precision and scale your business requires, on Bitdeer AI's purpose-built GPU fleet.


## **Get Started**


1. Log in to[Bitdeer AI Model Studio](https://www.bitdeer.ai/en/model/explore?ref=bitdeer.ai)
2. Locate **NVIDIA Nemotron 3 Embed 8B** or **NVIDIA Nemotron 3 Embed 1B** in the model list


1. Generate an API key and start making embedding API calls


> **NVIDIA Nemotron 3 Embed 1B:**


> curl -v --location '[https://api-inference.bitdeer.ai/v1/embeddings](https://api-inference.bitdeer.ai/v1/embeddings?ref=bitdeer.ai) ' --data '{"model":"nvidia/Nemotron-3-Embed-1B-BF16","input":"The cat danced gracefully under the moonlight, its shadow twirling like a silent partner."}' --header 'Authorization: Bearer <API_KEY>'


> **NVIDIA Nemotron 3 Embed 8B**


> curl -v --location '[https://api-inference.bitdeer.ai/v1/embeddings](https://api-inference.bitdeer.ai/v1/embeddings?ref=bitdeer.ai) ' --data '{"model":"nvidia/Nemotron-3-Embed-8B-BF16","input":"The cat danced gracefully under the moonlight, its shadow twirling like a silent partner."}' --header 'Authorization: Bearer <API_KEY>'


## **Conclusion**


Retrieval is becoming the control point of enterprise AI, the layer that decides whether agents stay grounded or go off course, and whether every reasoning cycle is spent on the right context or wasted on the wrong one. NVIDIA Nemotron 3 Embed brings frontier retrieval accuracy to practical enterprise deployment: an 8B model that sets the accuracy ceiling, a 1B model that carries that quality into high-volume production, and full openness from weights to recipes. With Day-0 availability on Bitdeer AI Model Studio, you can start building higher-quality RAG, search, and agentic retrieval systems today with better context, fewer wasted tokens, and more grounded AI systems.
