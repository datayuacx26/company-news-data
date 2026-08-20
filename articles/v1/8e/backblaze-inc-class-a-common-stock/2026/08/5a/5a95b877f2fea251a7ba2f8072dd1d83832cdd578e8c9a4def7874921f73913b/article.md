---
schema_version: "1.0.0"
document_id: "5a95b877f2fea251a7ba2f8072dd1d83832cdd578e8c9a4def7874921f73913b"
company_key: "backblaze-inc-class-a-common-stock"
company: "Backblaze Inc."
source_id: "backblaze-inc-class-a-common-stock-rss-a06767c1ff83"
canonical_url: "https://www.backblaze.com/blog/90-of-enterprise-data-is-unstructured-and-genai-cant-use-it-without-better-storage/"
published_at: "2026-08-06T13:41:24+00:00"
first_seen_at: "2026-08-06T16:30:01.914583+00:00"
fetched_at: "2026-08-06T16:30:03.333678+00:00"
content_hash: "sha256:9c138d1983c88572e73bd9f10e56a93300fe96e17a379d8fbab741c5cffab27e"
---

# 90% of Enterprise Data Is Unstructured, and GenAI Can’t Use It Without Better Storage

At[GTC](https://www.nvidia.com/gtc/keynote/) this year, Jensen Huang put a number on a problem that most enterprise AI teams already feel but struggle to articulate: about 90% of what gets generated every year is unstructured data. This includes PDFs, emails, video, contracts, support tickets, and voice recordings.


Until recently, that data was, in his words, almost entirely useless. You could store it or search it by keyword, but machines couldn’t read it, understand what it meant, or draw any connections between it and other content.


GenAI has changed that. RAG pipelines, vector databases, and embedding workflows give machines a way to process unstructured content, comprehend its meaning, and synthesize information across the entire enterprise.


The problem is that these tools put a completely different set of demands on storage than anything that came before them. And most enterprise storage wasn’t built for it.


**The access pattern no one planned for**


Traditional enterprise storage assumes that data gets written and then occasionally retrieved. The architecture for it was built with hierarchical file systems and tiered storage (hot archives for data that gets accessed regularly, cold archives for anything that hasn’t been touched in a while). It worked well for decades, until RAG pipelines broke it.


A RAG pipeline works by reading your documents and converting them into a format the AI can search. But that conversion process is not a one-time event. It has to be run repeatedly as you test, adjust, and update. Every adjustment means re-reading the same content, re-processing it, and re-writing the results back to storage.


For example, the pipeline breaks documents into chunks because the AI can only search effectively when content is divided into manageable pieces. The size of those chunks directly affects how well the AI answers questions. Too large, and the AI retrieves too much irrelevant content. Too small, and it loses context.


Whenever you have to update your underlying documents, the pipeline has to re-read and re-process everything affected by the change. The same is true when upgrading to a newer AI model, because the new model produces different representations of the same content and requires everything to be re-converted.


While the pipeline is reading, it’s also saving converted content, updated search indices, and checkpoints. And what it saves is tiny. Embeddings, vector chunks, and metadata objects are often just a few kilobytes each, but a production RAG pipeline generates millions of them. Legacy storage treats every save operation the same regardless of file size, so saving a million tiny files costs nearly as much as saving a million large ones.


Tiered storage exacerbates this even more. These systems judge which data is still needed and which can be moved to cheaper, slower storage based on how recently something was accessed. But a RAG pipeline might go quiet for weeks between runs and then suddenly need everything at once. Or it might re-index documents on a schedule that the storage system doesn’t recognize as regular access. Either way, the tiered system sees cold data and moves it to a slower tier. When the pipeline needs that data back, it waits and pays a retrieval fee before it can proceed. That cost and delay compounds across millions of objects and dozens of pipeline runs.


**The governance problem hiding inside your index**


When a RAG pipeline indexes documents into a vector database, it takes a snapshot of your data at that moment in time. The embeddings it generates reflect the content of those documents as they existed when the pipeline ran. When the original documents are updated or changed, the snapshot doesn’t change with them. The vector index keeps pointing to the old version, and the AI keeps retrieving it and presenting it as current fact.


To a user, that looks like hallucination. The model is confidently citing a document that no longer says what the model thinks it says. But the failure isn’t happening inside the model; it’s happening at the storage layer. Specifically, in the gap between when source documents change and when the index gets updated.


Most enterprises running RAG pipelines today have no systematic way to close that gap. Document management systems aren’t built to notify downstream AI pipelines when content changes. Vector databases aren’t built to automatically invalidate and re-index affected embeddings. The result is an AI system that’s architecturally incapable of knowing when its knowledge has gone stale, so it presents outdated information with the same confidence as current information.


**What GenAI storage actually requires**


Closing the gap between what RAG pipelines demand and what legacy storage delivers requires three things:


1. **Sustained high throughput.** RAG pipelines generate continuous, mixed traffic at every stage of the process—ingesting documents, processing them, writing the results to an index, and querying that index—often all running simultaneously. Storage must be able to move data in and out fast enough to match the speed at which the pipeline generates requests, and sustain that speed across the entire duration of a run, not just handle short bursts.


1. **Always-hot architecture with no tiering** . Every document needs to be immediately accessible regardless of when it was last touched. This is not just for pipeline performance, but also because slow re-indexing is what produces stale outputs. When source documents change, the pipeline needs to re-index immediately. Storage that puts documents behind retrieval delays or tiering penalties widens the window in which the AI is working from outdated information and presenting it as fact.


1. **Free data movement.** RAG workflows span multiple GPU providers, MLOps platforms, and inference environments across multiple regions. Storage that charges egress fees for every transfer turns architecture decisions into cost calculations and discourages the experimentation that gets AI systems from prototype to production.


**The storage layer that doesn’t get in the way**


Backblaze B2 Overdrive is built around exactly these requirements. It is always-hot object storage with no tiering, no retrieval delays, and no waiting for data to be promoted before a pipeline can run. Every document is always available, regardless of how recently it was written, which means when the pipeline needs to re-index, storage never slows it down.


At up to 1Tbps sustained throughput, it keeps high-volume RAG pipelines fed continuously through the constant mixed traffic of reads and writes they generate. Because egress is free, teams can move data between storage and compute across regions and providers without a cost penalty. And S3 compatibility lets B2 Overdrive drop into existing LangChain, LlamaIndex, and vector database pipelines without rewrites.


Ninety percent of enterprise data is sitting in formats that, until recently, machines couldn’t do anything useful with. The tools to unlock it now exist. How well they perform and what they cost to run depends largely on the storage underneath them.


*Ready to build a RAG pipeline that doesn’t stall on storage?*[Learn more about Backblaze B2 Overdrive.](https://www.backblaze.com/cloud-storage/b2-overdrive)
