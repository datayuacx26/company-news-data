---
schema_version: "1.0.0"
document_id: "ce10f2bb4cf082841c5010d69a3262dc3d30331f30201ab68210083fd603101e"
company_key: "docusign-inc-common-stock"
company: "DocuSign Inc."
source_id: "docusign-inc-common-stock-news-import-9953cf346b2b"
canonical_url: "https://www.docusign.com/blog/architecting-for-millions-how-docusign-processes-agreements-at-scale"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-25T01:48:27.982719+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:5d1619344a3fcda7ae555db91b25744038ea98f92b401bceac0a0ebb74fd6bd8"
---

# Architecting for Millions: How Docusign Processes Agreements at Scale

*The Docusign AI team includes: Prabhakar Marnadi, senior principal engineer; Ramachandra Kota, senior director of applied science; Aaditya Srivathsan, machine learning engineer; and Abhinav Sharma, lead machine learning engineer.*


*This is the first post in a 4-part series around how Docusign handles extractions at scale.*


The modern global enterprise runs on agreements — documents that encode renewal dates, pricing structures, and compliance obligations. When that intelligence stays trapped in static documents, the result is nearly[US$2 trillion in lost](https://www.docusign.com/en-sg/blog/the-agreement-trap-how-inefficiencies-add-to-2-trillion-lost-opportunities) productivity and missed opportunities. Historically, unlocking it required laborious manual review: an approach that scales poorly, drives up cost, and is prone to human error.


Enabling our customers to drive strategic growth, mitigate compliance risks, and unlock hidden value from every agreement requires a highly accurate, automated extraction mechanism capable of turning static documents into dynamic, queryable data. The engineering requirements at this scale are immense. Docusign’s platform must process over 50 specific types of extractions — such as Contract Value, Governing Law, and Renewal Type — across millions of documents daily while balancing synchronous, live user interactions with massive asynchronous bulk-processing jobs. This scale is driven by our global reach:


To support this, the system must maintain sub-minute latencies for live interactions while handling historical backlogs that can generate millions of requests from large enterprise customers with agreements scattered across 20+ systems.


In this series of posts, we'll lay out how we built the extraction pipeline that powers our AI engine,[Docusign Iris](https://www.docusign.com/products/platform/ai) , which handles more than a million documents per day at high accuracy, with per-document inference costs measured in fractions of a cent.


Across four posts, we'll cover the runtime architecture (this post); the teacher-student distillation approach behind our fine-tuned extraction models; how we extended the platform to let enterprises define their own custom extractions; and finally how we manage feedback at scale so the system continuously improves against each customer's specific document patterns.


## **The path to automated extraction**


Manual review couldn't keep pace: it was an architectural mismatch between the size of enterprise agreement portfolios (Docusign’s platform processes 1M+ documents daily) and the throughput of human workflows, and no amount of hiring closes that gap. Automated extraction does. But high accuracy comes from the pipeline, not the model alone. The architecture has to absorb the hard problems — scale, latency, cost, reliability — so the model can focus on what it does best.


## **Docusign’s answer: A modular, orchestrated pipeline**


Effectively addressing these challenges required moving away from monolithic document processing toward a modular, decoupled orchestration pipeline. Early pipeline iterations coupled I/O-bound tasks — fetching document blobs from storage, network transfers, disk writes — with CPU and GPU-bound tasks on the same compute nodes. The result was expensive GPU clusters sitting idle while waiting for network payloads.


We divided the infrastructure into three strictly decoupled layers that each scale independently: an application layer that handles user-facing surfaces and API contracts; a storage layer that persists documents, pre-processing artifacts, and extraction outputs; and an AI platform layer that owns all document processing and inference. The rest of this post focuses on the AI platform layer, where the bulk of the engineering complexity lives.


## **The AI platform layer: AI Orchestrator**


At the heart of the AI platform layer is the AI Orchestrator, the central routing and coordination component for all inference requests. Every document Docusign processes passes through three stages in sequence, and the engineering decisions within each stage are what make the pipeline work at this scale:


-


**Stage 1: Pre-processing** — getting the document into a form the model can use.


-


**Stage 2: Model inference** — selecting the right model, building the right context, and running inference at scale.


-


**Stage 3: Post-processing** — validating, normalizing, and grounding the output before it touches a system of record.


The rest of this post walks through each stage, the engineering challenges within it, and the architectural decisions that resolve those challenges.


Iris extraction pipeline architecture


## **Stage 1: Pre-processing**


Before any model sees the document, the pipeline must solve a problem that is easy to underestimate: getting the text out of a stagnant document (like a PDF) in a form the model can use. Rasterization, OCR, layout analysis — these are unglamorous but load-bearing. Get them wrong, and the most capable model in the world is working with corrupted input.


### ***Decoupling compute from I/O***


To eliminate GPU idle time, we isolated document ingestion and pre-processing into dedicated CPU-optimized microservices. Rasterization is both CPU-intensive and I/O-bound — a natural candidate for dedicated nodes rather than shared compute, freeing GPUs for the work that actually needs them.


Pre-processing handles three distinct workloads in this layer:


-


We parse the text layer directly rather than routing to an external service, which is substantially more efficient for documents with extractable text.


-


Scanned and image-based content is handled by a production-grade OCR service, chosen for accuracy across the wide variety of scan quality and document formats our enterprise customers ingest.


-


Rasterization runs internally producing the high-resolution page images that downstream document object detection and Vision Language Models consume.


The resulting artifacts are cached in ephemeral blob storage with a strict TTL expiration. Only then does the orchestrator trigger downstream GPU work, which pulls directly from that cache and stays reserved exclusively for computation.


### ***Document layout detection with YOLOv9***


Beyond raw text extraction, the pipeline needs to understand document structure such as tables, signature blocks, embedded images, and multi-column layouts. We run a fine-tuned, lightweight object-detection model for document structure. Because its memory footprint is small, we serve it on smaller, cheaper GPUs and scale the fleet horizontally, meeting our latency budget through parallelism rather than per-instance horsepower.


For documents with complex visual structure — particularly tables, where general-purpose OCR fails to recover the semantic relationships between cells — we route to[specialized vision-language models opens in a new tab](https://huggingface.co/nvidia/NVIDIA-Nemotron-Parse-v1.2) served on higher-memory GPUs. These models are bound by memory bandwidth rather than raw compute, so the serving setup is tuned to manage memory under load and sustain throughput without failing under burst traffic..


We serve them via[vLLM opens in a new tab](https://github.com/vllm-project/vllm) , tuned with bfloat16 precision and chunked-prefill to manage KV cache under load, with concurrent batching and concurrent requests to produce sustained per-page throughput suitable for production document parsing without OOM cascades under burst traffic.


### ***Caching pre-processing artifacts across extraction tiers***


Pre-processing artifacts are not consumed only once. A single agreement typically passes through multiple extraction tiers — standard fields first, then obligations, then clause analysis, often with custom extractions layered on top. Each tier needs the same underlying inputs: extracted text, OCR output, layout structure, rasterized pages, and embeddings. Re-deriving these for every tier would multiply pre-processing cost and latency by the number of downstream consumers.


The orchestrator serves every downstream tier from the same cached artifacts, tracking freshness via TTL and re-deriving only when the source document or pipeline changes. The result is bounded end-to-end latency regardless of how many tiers an agreement triggers, with a high cache-hit rate and prompt caching eliminating most re-derivation work.


## **Stage 2: Model inference**


With artifacts in usable form, the orchestrator routes them through inference. The naive approach — feed the whole document into a model's context window — is expensive, slow, and error-prone at hundred-page scale: the clauses that matter get buried in boilerplate, and the model's attention struggles to surface them. This is the Needle in a Haystack problem at enterprise volume, and three decisions address it: routing, context engineering, and hardware matching.


### ***Routing: matching the model to the task***


Not every extraction needs the same class of model. The AI Orchestrator routes different extraction classes to different model tiers based on the work they actually require.


Smaller models — fine-tuned through teacher-student distillation, which we’ll get into more in our next post — perform the high-volume structured extraction work: pulling fields like effective date, parties, governing law, and contract value across millions of documents a day. These models are fast, cheap, and specialized for tasks where the extraction pattern is well-defined and the cost-per-document constraint is binding.


Larger frontier models are reserved for judgment-intensive work — complex clause analysis, multi-document reasoning, summarization — where general reasoning capability is worth the higher per-call cost. Matching model capability to task complexity is what lets one platform serve both the high-volume structured workload and the lower-volume, higher-value insight workload.


### ***Context engineering: optimizing context to the model***


At enterprise scale, every unnecessary token in a prompt costs money, latency, and accuracy. Context engineering is what addresses all three at once. The AI Orchestrator runs context engineering as an explicit step before inference.


A few patterns we apply: pre-filtering documents that lack legal context (handbooks, exhibits, and other supporting documents outside the extraction scope), and semantic compression via a fine-tuned, lightweight encoder model that scores each extraction target against the document's text and surfaces the spans most likely to contain the answer, meaningfully improving retrieval recall over an unfiltered baseline. This typically reduces input from 100,000+ tokens to a few thousand.. This is not just for cost optimization. Removing legal boilerplate and irrelevant clauses materially improves extraction accuracy by sharpening the model's attention on the signal that actually carries the answer. On several extraction types, the compressed input produces higher accuracy than sending the full document to a more capable model.


### ***SLA-based queuing: separating live from bulk***


Enterprise traffic is highly unpredictable. A live user uploading a contract expects a response in under a minute while a customer onboarding a backlog of 10,000 legacy contracts can wait. Treating these as the same workload was unsustainable.


We migrated from a legacy event-streaming architecture to a dedicated message-queue layer that routes inference traffic into distinct processing lanes: live queues with a strict 15-minute P95 SLA, bulk queues running batched background workers during off-peak hours, and workflow-specific queues that prevent heavy summarization tasks from blocking lightweight extractions.


Without workflow-specific isolation, computationally dense tasks caused head-of-line blocking that degraded the entire queue. The result is predictable latency on live traffic regardless of bulk-job pressure, and isolation of expensive operations from blocking lightweight ones.


### ***Inference serving across providers and tiers***


For the LLM extraction tier, both frontier models and fine-tuned models – including the distilled extraction models we’ll cover in the next post – run on managed inference endpoints from multiple model providers. Specialized vision and embedding models are served on internal GPU clusters with continuous batching enabled at the inference server level to maximize throughput across concurrent documents rather than within a single request.


To handle unpredictable traffic spikes and avoid single-provider capacity limits, the platform runs a hybrid multi-cloud strategy: reserved capacity carries the predictable baseline traffic with guaranteed latency, while on-demand capacity absorbs bursts. The AI Orchestrator routes inference requests across these tiers based on workload priority and current capacity headroom.


The principle we kept returning to: match the silicon to the workload's actual bottleneck — memory bandwidth, compute, or memory footprint — rather than to whichever accelerator is newest or most powerful.


## **Stage 3: Post-processing**


Detection and extraction without validation is an expensive alarm system. The post-processing layer is the difference between a demo and a production system. Without it, a confident but wrong extraction propagates silently downstream.


At a minimum, post-processing handles normalization — converting extracted date formats to a canonical representation, resolving party-name variations, standardizing currency and units — and hallucination checks that verify each extracted value is actually grounded in the source document rather than confidently invented by the model.


Business-logic validation then enforces domain constraints: an expiration date that precedes its effective date is rejected, a contract value with no currency is flagged. Only after these checks pass does the extraction commit to the storage layer.


## **The architecture in production**


The architectural decisions above translate directly into production performance across three dimensions:


**Daily throughput.** During recent monthly review periods, the infrastructure successfully processed an average daily throughput in the millions of documents, with peak days running materially higher.


**System availability.** Despite this immense computational load, the platform sustains high availability under this load, meeting the reliability bar enterprise customers expect..


**Cost savings.** With context engineering, model-class routing to efficient models and optimized GPU cluster utilization combined to drive 50X reduction in per-document cost of more than an order of magnitude relative to our earlier pipeline


**P95 processing latency.** The decoupled queueing architecture and optimized inference servers consistently meet stringent SLAs. The P95 processing latency for unified extractions stays comfortably within our asynchronous processing SLA.


One thread runs through every decision above: separate what can be separated, route what can be routed, cache what can be cached, and match the hardware, the model, and the context to the work each actually requires. The result is a pipeline that absorbs the unpredictability of enterprise traffic at the volume of Docusign's global business, while keeping per-document economics in the range that makes the platform viable.


## **What’s next**


Infrastructure gets documents to the right model with the right context, but it doesn't determine how accurate that model is, how well it fits a customer's specific portfolio, or how it improves as corrections accumulate. Those are the harder problems.


Post 2 covers the first: the teacher-student distillation behind our fine-tuned extraction models, and how a smaller model learns to do the work of a frontier one at sub-cent cost.
