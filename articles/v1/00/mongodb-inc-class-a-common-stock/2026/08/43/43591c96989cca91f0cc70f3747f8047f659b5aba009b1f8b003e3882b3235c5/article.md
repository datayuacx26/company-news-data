---
schema_version: "1.0.0"
document_id: "43591c96989cca91f0cc70f3747f8047f659b5aba009b1f8b003e3882b3235c5"
company_key: "mongodb-inc-class-a-common-stock"
company: "MongoDB Inc."
source_id: "mongodb-inc-class-a-common-stock-news-import-efe1743dc302"
canonical_url: "https://www.mongodb.com/company/blog/product-release-announcements/closing-gap-between-agentic-promise-impact-industry-leading-retrieval"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T20:14:35.483907+00:00"
fetched_at: "2026-08-13T20:14:38.318971+00:00"
content_hash: "sha256:98ec3191a496e89c858b2d0d43117cbd5c369bbfc7e4bbddaef1aba294b19dd4"
---

# Closing the Gap Between Agentic Promise and Impact with Industry Leading Retrieval

A recent McKinsey report* noted that AI agents and robots “could generate about $2.9 trillion in US economic value per year,” while another study from WRITER** quantified the speed at which organizations have moved toward agents: 97% of C-suite executives surveyed indicate their companies have deployed AI agents, and 75% expect agents to become *members* of the C-suite in the next five years.


However, there’s still a gap between the promise of agentic AI and its real-world impact. The same WRITER survey found that only 29% of respondents have seen “significant ROI from generative AI, despite individual productivity gains to date.” Why? Data accuracy and operations, to start. As my colleague Pete Johnson has put it,[the AI gap is a workflow problem](https://www.cio.com/article/4193951/the-ai-roi-gap-isnt-a-model-problem-its-a-workflow-problem.html) .


What often separates an agent that works in production from one that doesn't is the data they draw on: the memory agents carry between steps, and the accuracy of what they retrieve. When retrieval returns inaccurate information, the agent acts on the error, and the mistake compounds across the ensuing steps. To solve for this, teams stitch together databases, vector search solutions, and model providers—all of which add operational complexity to agentic workflows and slow development.


Today at MongoDB.local Build Fest, we’re announcing capabilities that bring benchmark-leading retrieval directly into the operational database, enabling teams to give their agents the best data available: **Automated Embedding in MongoDB Atlas powered by Voyage AI (GA)** , the **Atlas Embedding and Reranking API (GA)** , the **voyage-code-4** model, and **$vectorSearch in Atlas Stream Processing (GA)** . Together, these capabilities close the divide between agentic promise and impact.


## Accuracy without bolt-ons


First, with the general availability of **Automated Embedding in MongoDB Atlas powered by Voyage AI,** accurate retrieval comes built into the database; this is a meaningfully different starting point from the alternative, which is to bolt a vector extension onto the database and then stand up—and maintain—your own embedding pipeline alongside it. Getting accurate AI retrieval is now as easy as creating a vector search index inside of MongoDB.


Automated Embedding leverages the[Voyage 4 family of models](https://blog.voyageai.com/2026/01/15/voyage-4/) from[Voyage AI by MongoDB](https://www.mongodb.com/products/platform/ai-search-and-retrieval) . Voyage AI models consistently achieve top rankings in retrieval benchmarks, and the Voyage 4 series establishes a new retrieval accuracy frontier, delivering even more accurate retrieval for search, RAG, and agentic solutions.


[MedINT](https://www.mongodb.com/company/blog/product-release-announcements/unlocking-ai-search-introducing-automated-embedding-in-mongodb-vector-search) helps clinical teams find answers across a large and growing library of medical documents. It uses Automated Embedding in MongoDB Atlas, which builds accurate search into the same database that holds its data, so there is no separate embedding pipeline to run or keep in sync.


“Precise retrieval isn’t a nice‑to‑have, it’s mission‑critical. We deployed MongoDB’s auto‑embedding and vector search across our 40‑million‑document corpus, converting weeks of engineering into a low‑maintenance, production‑grade workflow that materially improved retrieval speed and relevance while reducing development overhead,” said Ohad Levinkron Fisch, VP R&D and AI, MedINT Medical Intelligence. "The right studies must surface quickly and reliably to avoid misleading signals, prevent dangerous extrapolations, and preserve clinician trust. That’s why we chose MongoDB."


**The Atlas Embedding and Reranking API** , meanwhile, brings Voyage AI's embedding and reranking models into MongoDB Atlas as a standalone, serverless API that works with any stack. Put another way, MongoDB's top-ranked retrieval models are now available to any application, even those outside MongoDB, through a single connection. Teams get one secure source of accurate retrieval across everything they build, instead of piecing it together from separate tools.


## Empowering agents (& agent-managing teams)


Today, we’re also announcing the new **voyage-code-4** model. voyage-code-4 has been specifically designed for agentic coding retrieval workloads, and the model supports flexible output dimensions (256d, 512d, 1024d, 2048d), 32K input context, and quantization-aware training.


voyage-code-4 brings retrieval accuracy tuned for coding agents, at a cost low enough that they can run constantly.


And today, MongoDB is bringing native semantic enrichment to[Atlas Stream Processing](https://www.mongodb.com/products/platform/atlas-stream-processing?utm_source=google&utm_campaign=search_gs_pl_evergreen_stream-processing_product_prosp-brand_gic-null_ww-multi_ps-all_desktop_eng_lead&utm_term=atlas%20stream%20processing&utm_medium=cpc_paid_search&utm_ad=e&utm_ad_campaign_id=20445624173&adgroup=155168612111&cq_cmp=20445624173&gad_source=1&gad_campaignid=20445624173&gbraid=0AAAAADQ1402GO84RGGAojYkqnVO71IeFb&gclid=CjwKCAjwpqHTBhAcEiwAj2AfuqMklB67bMMaj16ikzG70LZwaPFkW9fm_7HqvJbtOkyR6hgiFxz0dRoC2kUQAvD_BwE) with the general availability of **a dedicated $vectorSearch stage** , joined by $rerank. A stream processor can now consult an[Atlas Vector Search](https://www.mongodb.com/products/platform/atlas-vector-search) index directly, mid-pipeline, per document, without leaving the platform. Agents can now get accurate retrieval on data in motion (versus data already at rest), speeding up when data arrives and when an agent acts on it. That's a different starting point than relying on tables that are read-only for applications, where getting live retrieval means working around the platform rather than through it.


## A joint MongoDB and Temporal architecture for agents on live data


MongoDB and Temporal also introduced a[joint reference architecture](https://temporal.io/blog/mongodb-temporal-partnership-rag-agents) for teams building agents on live data, pairing Temporal's durable execution with MongoDB and Voyage AI for state, memory, and retrieval.


It keeps a vector store continuously in sync as source data changes, and after an interruption it resumes from the last completed step rather than re-embedding work already done. Critically, an agent's state and memory live in the same platform its retrieval reads from. Available as a live demo and an open, forkable repository, it gives teams a production-grade starting point instead of the brittle pipelines they often hand-build today.


“The best agentic apps we see share a clear pattern: Temporal runs the orchestration, and MongoDB holds the memory and the data agents retrieve. This reference architecture takes a pattern customers already run in production and hands it to any team as a starting point, so they can build on a proven design instead of assembling it from scratch,” said Cornelia Davis, Principal Developer Advocate, Temporal.


## Building agentic AI faster


The capabilities announced today at MongoDB.local Build Fest offers builders better ways to get agents the accurate, relevant data agents need to operate, while also helping builders consolidate their stacks. In short, builders no longer need to choose between accuracy and speed: the models, the retrieval, and the operational data run together, with no separate pipeline to build or keep in sync.


As promising as the agentic future is, it won’t simply happen. We have to build it. Agents are only reliable and impactful if they’re given accurate data to retrieve. By building that accuracy directly into an intelligent data platform, the capabilities announced today enable builders to move quickly from prototype to production, at the scale the agentic future demands.


###### Next Steps


For more on new capabilities to empower developers and agents alike,[read my colleague Apoorva Joshi's blog post](https://www.mongodb.com/company/blog/product-release-announcements/mongodb-for-agentic-era-built-for-developers-ai-agents) . And to learn about all the capabilities announced at MongoDB.local Build Fest, check out the[What's New page](https://www.mongodb.com/products/updates) .


*[https://www.mckinsey.com/mgi/our-research/agents-robots-and-us-skill-partnerships-in-the-age-of-ai](https://www.mckinsey.com/mgi/our-research/agents-robots-and-us-skill-partnerships-in-the-age-of-ai)


**[https://writer.com/blog/enterprise-ai-adoption-2026/](https://writer.com/blog/enterprise-ai-adoption-2026/)
