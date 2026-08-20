---
schema_version: "1.0.0"
document_id: "416d07d660c4970d6581c11c112f5338274b76ed65a9311b6690cea0c414719c"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/how-mem0-uses-embeddings-and-why-we-are-evaluating-nvidia-nemotron-3-embed"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-23T17:29:03.310285+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:3281da09df9e3129bcf9e3578087c0418a15f7c8efbbb1042f7850d8dc828948"
---

# How Mem0 uses embeddings, and why we are evaluating NVIDIA Nemotron 3 Embed

Mem0 is building the memory layer for AI agents. It decides what to keep from an agent's conversations and returns the relevant pieces later. Every turn produces new memories, and each one is embedded and indexed as it arrives. Retrieval runs on those same embeddings. This post covers how the write and read paths use embeddings, and how NVIDIA Nemotron 3 Embed fits.


## **The write path**


When messages come in, a background worker extracts the facts worth keeping, each as one short statement.


Two embedding steps run on each add. Each fact is embedded and indexed for semantic search later. And the named entities in each fact are embedded to link them for entity (graph) search. Read more about Mem0 Add[here](https://docs.mem0.ai/core-concepts/memory-operations/add) .


## **The search path**


This is how Mem0 search runs today:


1.


Embed the inbound query. Also pull entities from the query.


2.


Hybrid search: vector similarity for meaning, keyword search against the query, and semantic boosts for each query entity. The hybrid search allows us to retrieve memories based on different retrieval signals.


3.


Additional optional signals like temporal boosts, decay, and a reranker on top (configured by the user).


4.


Adjust scores: memories tied to entities in the query get a boost (more details below), time-sensitive queries get a recency boost, and older memories can be down-weighted by decay.


5.


Optionally rerank the top candidates.


6.


Return the top results above a threshold, scoped to the user and project.


The embedding model is critical here: it is the backbone for the semantic searches used to recall the memories for each signal (vector search, entity search), acting as one of the fundamental building blocks for retrieval. Read more about Mem0 Search[here](https://docs.mem0.ai/core-concepts/memory-operations/search) .


## **Entity linking (Graph memory)**


Retrieval by memory similarity has a gap. If a query is about a person or project, the memory that answers it may not be worded like the query. It may just be one of several facts that mention that name.


Mem0 handles this with entity linking. When we store a memory, we pull the named things out of it and embed each name in the same space as the memories. When an entity already existing in the memories shows up in a new query, we match the new mention to the stored one by similarity and attach the memory to it, rather than duplicate it. Each entity ends up getting linked to the list of memories that mention it.


On a search, we embed the query's entities too, match them to stored entities, and boost the memories those entities link to. The boost is additive and can pull in a memory that shares an entity with the query even when its text is not a close match.


This boost helps us retrieve memories based on the entity-level signals from the query, and the embedding model is critical for this segment as well.


## **Role of embedding models**


Embedding runs on every fact, every entity, and every query, so the embedding model is on the hot path throughout:


-


**Quality** sets recall and entity-match accuracy, which nothing downstream can fix.


-


**Dimension** is fixed in the index. A different dimension means re-embedding and re-indexing everything, not a config change.


-


**Throughput** is real cost and latency, and because Mem0 is add-heavy, it shows up most on writes.


-


**We serve the model ourselves** , so a replacement has to be one we can host at our scale, not only call through an API.


## **Nemotron 3 Embed**


NVIDIA Nemotron 3 Embed is a family of open, commercially available embedding models for enterprise search and agentic retrieval, with an 8B model for top accuracy and a 1B model for high-throughput retrieval. What fits our needs:


-


**Open weights and self-hosting.** We run our embedding model on our own endpoints, and open weights let us fine-tune as required for the memory usecase.


-


**Two sizes for a real tradeoff.** Because embedding is on the hot path and adds dominate, the availability of choice between the 8B model's accuracy and the 1B model's throughput is very critical. For production scale, the 1B model is the interesting one.


-


**NVFP4 on Blackwell.** The 1B model's 4-bit path raises throughput while holding accuracy, which matters when you embed on every add and every search.


-


**Long context, multilingual, and code,** matching the range of content agents accumulate.


-


**Query and document prefixes,** which our embedding layer would tag on the query and the stored text.


## **What we saw**


We test embedding models inside our own stack rather than reading leaderboard tables, since a benchmark number does not show how a model behaves as the recall layer with entity linking and reranking on top. We ran Nemotron 3 Embed through it on common memory evaluations (like LongMemEval and BEAM) and saw retrieval quality improve. The open weights and fine-tuning recipes are what interest us most, since they let us adapt the model to memory text and serve it ourselves.


### LongMemEval retrieval comparison


**System**


**Retrieval@50**


**Retrieval@10**


Qwen-3-600M


96.45%


78.71%


Nemotron-3-Embed-1B


97.08%


80.38%


**Methodology** : Retrieval@50/10 is the percentage of LongMemEval questions where the top 50/10 retrieved memories included evidence from every conversation turn required to answer the question. Evaluated on the 479 of 500 questions that have actual answer evidence. Both systems used 768-dimensional embeddings.


##


## **Getting started**


Nemotron 3 Embed is available with open weights and training recipes on Hugging Face, with support for standard embedding workflows and vLLM, and NVIDIA NIM microservices on[build.nvidia.com](http://build.nvidia.com/) for production deployment.


*Mem0 is an intelligent, open-source memory layer designed for LLMs and AI agents to provide long-term, personalized, and context-aware interactions across sessions.*


Get your free API Key here:[app.mem0.ai](https://app.mem0.ai/?utm_source=mem0_blog&utm_medium=kw_blog&utm_campaign=understanding_memory_benchmark&utm_content=understanding_memory_benchmark)
Or self-host mem0 from our[open source github repository](https://github.com/mem0ai/mem0)
