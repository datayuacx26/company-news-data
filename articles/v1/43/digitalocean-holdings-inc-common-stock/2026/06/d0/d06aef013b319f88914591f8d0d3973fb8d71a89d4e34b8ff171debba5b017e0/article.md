---
schema_version: "1.0.0"
document_id: "d06aef013b319f88914591f8d0d3973fb8d71a89d4e34b8ff171debba5b017e0"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/dataandlearning"
published_at: "2026-06-03T19:23:28.774+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:5fac56b6407eb6ee3c3aee375b1bf6e20fb6794524c3a727c47f2cc46d957f7c"
---

# Powering the Inference Era: Inside the DigitalOcean Data & Learning Layer

Building an AI-native application requires a data layer that can do two things at once: handle the structured, transactional queries your application runs on, and understand meaning well enough to power semantic search across unstructured content. An AI application needs both — precise SQL for account balances and transaction records, and vector search to surface conceptually related patterns, anomalies, or past cases that a keyword query would never find.


Most teams end up stitching these together across different environments, where every query crosses a boundary. Latency compounds and costs grow with the complexity of the glue, not the value of the data. What holds together in a prototype starts to fracture under production load.


The DigitalOcean[Data & Learning](https://www.digitalocean.com/data-learning) layer is designed to close that gap by giving you structured, vector, and retrieval layers that work together in the same ecosystem.


## Real-Time Inference and Learning


At the heart of any sophisticated AI application is the need for grounded, context-aware inference. DigitalOcean now supports a unified set of tools across the data layer:


-


[Managed PostgreSQL Advanced](https://docs.digitalocean.com/products/databases/postgresql/how-to/use-advanced-edition-clusters/) and[MySQL](https://docs.digitalocean.com/products/databases/mysql/how-to/use-advanced-edition-clusters/) Advanced Edition (Public Preview) for the structured, transactional data your application runs on


-


[Knowledge Bases](https://docs.digitalocean.com/products/knowledge-bases/) (General Availability) to handle the full retrieval pipeline from ingestion to answer


-


[Managed Weaviate](https://cloud.digitalocean.com/vectordatabases/new) (Public Preview) for vector search on unstructured data


Together, this unified platform allows developers to build real-time multimodal pipelines and manage enterprise knowledge bases with ease. Every retrieval your application or agent makes flows through this layer. When the data and retrieval layer is fully managed and scales with the application, your agent’s answers stay grounded and your service stays available.


These services run on the same platform as DigitalOcean’s Inference Engine and Managed Agent infrastructure. This means zero egress between the data layer and inference, one billing relationship instead of three, and no auth glue to write just to connect an agent to its knowledge. We don’t try to rebuild what the open-source ecosystem already does well. Knowledge Bases, Managed Weaviate, and our managed PostgreSQL and MySQL run on the same open standards developers already trust, integrated into one platform so the path to scale gets shorter.


### PostgreSQL and MySQL Advanced Edition: Scale the foundation, Faster


Every transactional application needs a database. It’s the layer everything else depends on, and when it goes down, the application goes with it. For AI-native companies, the stakes are even higher. If you are deploying autonomous agents that rely on structured data, or building an app with AI from scratch, your database is the single source of truth. If it goes down, your entire application goes down with it.


To support these critical workloads as they scale, we are launching **PostgreSQL and MySQL Advanced Edition** (now in[Public Preview](https://www.digitalocean.com/legal/managed-databases-advanced-edition) ). This new tier is purpose-built for larger customers and high-growth AI startups that require a more resilient, purpose-built platform. When your agent needs to pull a month of transaction logs before generating an analysis, Advanced Edition is the layer that has to stay up, stay fast, and stay consistent. That’s the foundation everything else in this stack sits on.


Advanced Edition retains the critical features you love from our Standard tier, like automatic disk scaling, but has been entirely re-engineered for maximum speed and minimal disruption:


-


**Scale in as little as minutes** : When you need to scale database capacity, the operation now takes mere minutes instead of hours.


-


**Reliable operations** : Benefit from highly resilient high-availability (HA) clusters with proxy-based failover which keeps your application alive even during node failures.


-


**Observe with confidence** : Get deeper insights into your query performance and system health to understand, debug, or dive deep into your database’s performance.


Even before the introduction of our Advanced Edition, customers were achieving massive scale on our Standard tier. For example, Picap,[a rideshare platform](https://www.digitalocean.com/customers/picap) supporting over 1 million rides per day, achieved 4x cost savings by moving Postgres and Kafka to DigitalOcean. With Advanced Edition, we are giving you a stronger foundation to support your app as it scales.


Enroll in the[MySQL](https://cloud.digitalocean.com/account/feature-preview?feature=advanced_mysql) and[PostgreSQL](https://cloud.digitalocean.com/account/feature-preview?feature=advanced_pg) Public Previews and get early access today.


### Knowledge Bases: Simplifying RAG


While robust databases handle your structural foundation, DigitalOcean Knowledge Bases (now in GA) takes the Data & Learning layer a step further by simplifying unstructured data management. Building a traditional Retrieval-Augmented Generation (RAG) stack typically requires stitching together multiple vendors for vector storage, embedding models, and retrieval logic; Knowledge Bases replaces that complexity by turning the entire subsystem into a built-in platform primitive.


Key benefits include:


-


**Zero-Config Lifecycle:** Simply upload your documents and pick an embedding model. DigitalOcean handles the entire pipeline—ingest, chunk, embed, and hybrid retrieval—automatically.


-


**The RAG Playground:** Test your strategies interactively in the console. You can refine chunking and retrieval quality without writing throwaway scripts—all in a single pane.


-


**Agent-Ready with MCP:** With one line of config, any MCP-compatible agent, including AI Platform Agents, can use your Knowledge Base as its retrieval tool.


-


**Affordable Pricing:** Production-ready infrastructure[starting at](https://docs.digitalocean.com/products/databases/opensearch/details/pricing/) $19.60, with embedding tokens[at just](https://docs.digitalocean.com/products/knowledge-bases/details/pricing/) $0.02/1M. No minimum commitment required.


Using DigitalOcean Knowledge Bases, LawVo was able to accelerate their path to production:


"Before DigitalOcean Knowledge Bases, we were looking at weeks of work to stand up a production RAG pipeline behind our LawvoAI offering—vector DB, embeddings, chunking, the whole stack. With DigitalOcean, we had a citation-backed knowledge base running in a day. That lets our team focus on the product, not the retrieval plumbing.”


- Hovsep Seraydarian, Co-founder & CTO LawVo


### Managed Weaviate: Relief from Vector Ops


While Knowledge Bases offer a seamless, turnkey experience, some teams need a more complete, hands-on control over their schema, chunking, and retrieval. However, self-hosting a vector database eventually leads to an operational wall and proprietary alternatives often result in unpredictable billing spikes. Managed Weaviate (now in[Private Preview](https://www.digitalocean.com/legal/managed-weaviate) ) offers a solution: the unmodified open-source engine you already[know](https://weaviate.io/) , delivered as a managed service.


How it works:


-


**Zero Lock-in:** Use the same Python, JavaScript, and Go clients with no proprietary SDKs or code changes required for migration.


-


**Platform Native:** One invoice, one API token. Your vector store now lives directly alongside your Droplets, Managed Databases and Serverless Inference—no separate vendor relationship to manage.


-


**Predictable Pricing:** Paired with DigitalOcean[Serverless Inference](https://www.digitalocean.com/blog/serverless-inference-deep-dive) , you can keep your data embeddings, retrieval, and generation co-located with zero egress fees. Plus, you can bring your own embedding provider if you prefer.[Be the first to try](https://try.digitalocean.com/managed-weaviate/) Managed Weaviate on DigitalOcean.


### DigitalOcean Data & Learning Layer: Built for Modern AI


The Data & Learning layer is built so the data and the inference don’t live in different houses. For AI-native companies building toward production, whether the workload is petabytes of transaction logs, an enterprise knowledge base, or real-time multimodal pipelines, the result is a faster path from prototype to scale on a stack the team doesn’t have to assemble.


Ready to dive deeper? Check out the[Data & Learning segment of the Deploy Keynote](https://www.youtube.com/watch?v=HYELtHc7QRo&list=PLseEp7p6EwiYlw9XdsasOPAKhq3CwirZ8&t=3816s) or watch the[Product Session on the Data Layer](https://www.youtube.com/watch?v=pE9y9EYGh7w&list=PLseEp7p6EwiYlw9XdsasOPAKhq3CwirZ8&index=7) *.*


Join the over 85,000 customer accounts already running on our Managed Databases, production-tested for the most demanding AI workloads.
