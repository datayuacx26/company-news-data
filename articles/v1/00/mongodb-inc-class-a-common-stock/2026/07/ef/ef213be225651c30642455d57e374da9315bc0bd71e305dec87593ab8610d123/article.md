---
schema_version: "1.0.0"
document_id: "ef213be225651c30642455d57e374da9315bc0bd71e305dec87593ab8610d123"
company_key: "mongodb-inc-class-a-common-stock"
company: "MongoDB Inc."
source_id: "mongodb-inc-class-a-common-stock-news-import-efe1743dc302"
canonical_url: "https://www.mongodb.com/company/blog/product-release-announcements/mongodb-search-vector-search-now-run-anywhere"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-24T04:07:24.248205+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:2d5ca5a39a62ac8078569c1efd3b5cba5905e4a7983b95c8e83505fc2b80d782"
---

# MongoDB Search and Vector Search Now Run Anywhere

MongoDB is excited to announce the general availability of[MongoDB Search](https://www.mongodb.com/products/platform/atlas-search) (full-text) and[Vector Search](https://www.mongodb.com/products/platform/atlas-vector-search) for[MongoDB Enterprise Advanced](https://www.mongodb.com/products/self-managed/enterprise-advanced) and[MongoDB Community Edition](https://www.mongodb.com/products/self-managed/community-edition) . Since announcing the[public preview at MongoDB.local New York](https://www.mongodb.com/company/blog/product-release-announcements/supercharge-self-managed-apps-search-vector-search-capabilities) last year, these milestones represent MongoDB’s commitment to ensuring developers can build intelligent, search-driven applications wherever their data lives without compromising on capability, data control, or infrastructure flexibility.


The way developers build applications has fundamentally changed: search has become a foundational capability, and context-aware experiences are no longer a differentiator, but the expectation. Whether developers are building an e-commerce storefront, a SaaS knowledge base, a customer support portal, a recommendation engine, or an AI agent, users want applications that understand their intent, anticipate their needs, and surface relevant information at a moment’s notice.


These agentic and user experiences range from chatbots that reason to recommendation engines that feel personal, to search experiences that go beyond keywords, and agents that act on a user's behalf, and all depend on the same thing: the ability to find and retrieve the right information—fast.


## Why MongoDB is the database for search workloads


Delivering those modern experiences requires more than a database. It requires full-text search for keyword and language-aware retrieval, vector search for semantic understanding, and the ability to combine both through[hybrid search](https://www.mongodb.com/docs/vector-search/hybrid-search/hybrid-search-overview/) within a single query pipeline. Until now, achieving this in a self-managed environment meant bolting on multiple external tools and managing the complexity they introduced: ingestion pipelines, synchronization overhead, inconsistent data across systems, and context-switching between different APIs.


What often gets overlooked in the database decision is the operational cost of retrofitting document workloads into systems designed for rigid tables. Most data stacks weren’t built for the fluid, unstructured data that AI workloads demand. When handling highly variable document workloads, relational databases can hit strict physical thresholds, ultimately causing storage systems to fragment data across disks and query performance to crater under load.


MongoDB addresses these challenges by centering its architecture around the document model itself. Because modern applications and[large language models](https://www.mongodb.com/resources/basics/artificial-intelligence/large-language-models) (LLMs) are fundamentally JSON-native, the document model isn’t adapted for AI; it’s the natural shape of AI data. MongoDB allows developers to store data exactly as it is queried, maintaining a core principle: data that is accessed together is stored together. Additionally, rather than forcing a choice between performance and features, MongoDB provides an integrated developer experience by bridging operational data and advanced search powered by Apache Lucene, offering workload isolation without data isolation. Developers can run search and vector search operations on dedicated, optimized processes without the operational burden of constantly moving or syncing data across separate systems.


This launch removes the friction that comes from building the latest modern applications. This includes the friction of managing and integrating multiple separate databases for OLTP, search, and vector. It also increases developer productivity by preventing context switching between different query languages and APIs and by reducing the need to maintain data consistency across multiple tools. These weren’t minor inconveniences for developers; they were barriers that slowed innovation, complicated the tech stack, and made it harder to build modern AI applications.


With the general availability of MongoDB Search (full-text) and Vector Search for[MongoDB Enterprise Advanced](https://www.mongodb.com/products/self-managed/enterprise-advanced) and[MongoDB Community Edition](https://www.mongodb.com/products/self-managed/community-edition) , developers can build search-driven, AI-powered applications directly in their self-managed environments, free of storage caps or imposed limits, with a fully featured implementation of search and vector search capabilities.


## One database, the freedom to run AI apps anywhere


The aggregation pipeline stages $search, $searchMeta, $vectorSearch, $rankFusion, and $scoreFusion are all supported with functional parity to[MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database) and, together, unlock the power to build[retrieval-augmented generation](https://www.mongodb.com/resources/basics/artificial-intelligence/retrieval-augmented-generation) (RAG) pipelines, AI agents, chatbots, agentic loops that combine full-text, vector, and match-based retrieval to surface the most relevant response, and other applications that depend on search at its core.


Beyond query execution, developers can also access one-click[automated embedding](https://www.mongodb.com/docs/vector-search/crud-embeddings/automated-embedding/) generation powered by Voyage AI’s state-of-the-art embedding models, eliminating the need to manage external models and sync data. Currently, automated embedding generation requires MongoDB-hosted Voyage AI. For applications that require the highest retrieval precision, Voyage AI[rerankers](https://www.mongodb.com/docs/voyageai/models/rerankers/?client=python) are also available, refining results beyond what embedding-based or lexical retrieval alone can achieve. Together, these capabilities form a complete AI stack from ingestion to retrieval to reranking, without leaving MongoDB.


Explore the full range of supported use cases in the[vector search](https://www.mongodb.com/docs/vector-search/) ,[full-text search](https://www.mongodb.com/docs/atlas/atlas-search/) , and[Voyage AI](https://www.mongodb.com/docs/voyageai/) documentation pages.


## Works with the tools developers love to use


MongoDB offers a rich ecosystem of native integrations with the frameworks developers are already using to build AI applications. Through a partnership with LangChain, MongoDB serves as a complete AI agent backend across[LangChain](https://www.mongodb.com/docs/atlas/ai-integrations/langchain/#std-label-langchain) ,[LangGraph](https://www.mongodb.com/docs/atlas/ai-integrations/langgraph/#std-label-langgraph) , and LangSmith, covering vector search and hybrid retrieval, persistent agent memory, and state to streamline developers' workflows.


These integrations keep agents reliable under real-world conditions and make it easier to embed RAG pipelines, agentic workflows, and semantic search directly into applications without wiring up custom retrieval logic from scratch. Developers can use MongoDB as a fully integrated operational database and vector store within these widely used frameworks to work faster and maintain a reliable tech stack that stays consistent from prototype to production.


To learn more about other AI frameworks supported by MongoDB, check out this[documentation](https://www.mongodb.com/docs/atlas/ai-integrations/) .


## Build confidently with MongoDB Community Edition


These capabilities will be available in Community at no additional cost under the[Server Side Public License (SSPL)](https://www.mongodb.com/legal/licensing/server-side-public-license) , and are part of a broader goal of making Community Edition the best place for developers to build locally, anywhere, for free.


With the recent release of mongot under the SSPL, developers can inspect the source for greater transparency, easier debugging of complex edge cases, and clarity into how search and vector operations work under the hood in MongoDB. MongoDB Community Edition now provides developers with a stronger foundation for building intelligent applications without any compromises. Run it locally anywhere—for free!—and start building today.


## Run mission-critical applications on MongoDB Enterprise Advanced


MongoDB has always provided the[freedom to run anywhere](https://www.mongodb.com/products/capabilities/run-anywhere) , and MongoDB Enterprise Advanced gives customers the most flexible way to deploy MongoDB on-premises and in private or hybrid clouds. Enterprise Advanced is perfect for customers with strict regulatory and governance requirements, as they retain full control of the database, even in air-gapped environments. That level of control is ideal when deploying advanced AI applications that need careful monitoring and refinement to deliver accurate results.


MongoDB Search and Vector Search for self-managed environments are deployed on Kubernetes through[MongoDB Controllers for Kubernetes (MCK)](https://www.mongodb.com/docs/kubernetes/current/) . That gives customers the operational benefits of a Kubernetes-native model: automation, resilience, and scalable operations, while preserving the flexibility to keep MongoDB databases where they already run.


Enterprise Advanced operates at massive scale, and now companies that depend on MongoDB for their mission-critical applications have access to an integrated search solution that uses the same query language their developers are already familiar with. Companies with large datasets and strict regulatory requirements, such as those in[financial services](https://www.mongodb.com/solutions/industries/financial-services) ,[healthcare](https://www.mongodb.com/solutions/industries/healthcare) , and[telecommunications](https://www.mongodb.com/solutions/industries/telecommunications) , are ideal for Enterprise Advanced, Search, and Vector Search.


## Getting Started with Search and Vector Search


**MongoDB Enterprise Advanced:** MongoDB Search and Vector Search are available as a paid add-on to MongoDB Enterprise Advanced. Enterprise Advanced comes with technical support and powerful tools for automation, operations, and security in self-managed environments. To learn more about deploying Search and Vector Search to an Enterprise Advanced cluster, check out the[documentation](https://www.mongodb.com/docs/kubernetes/current/fts-vs-deployment/) . Learn more about subscribing to Enterprise Advanced or adding Search and Vector to an existing Enterprise Advanced subscription by[contacting the MongoDB sales team](https://www.mongodb.com/company/contact#sales-support) .


**MongoDB Community Edition:** MongoDB Search and Vector Search for MongoDB Community Edition are available with MongoDB version 8.2 or later and operate on a separate binary, mongot, which runs alongside the standard mongod database binary. To learn more about deploying Search and Vector Search in Community Edition, check out the[documentation](https://dochub.mongodb.org/core/self-managed-search) .


###### Next Steps


Check out MongoDB's[documentation](https://dochub.mongodb.org/core/self-managed-search) to get started with Search and Vector Search in MongoDB Community Edition, or explore the[MongoDB MCP Server](https://www.mongodb.com/docs/mcp-server/get-started/) to integrate MongoDB directly into your agentic tools, assistants, and platforms. Subscribe to Enterprise Advanced or add Search and Vector Search to an existing Enterprise Advanced Subscription by[contacting the MongoDB sales team](https://www.mongodb.com/company/contact#sales-support) .
