---
schema_version: "1.0.0"
document_id: "9d0ab098d54c7e3a4deb4bc65e12ca8980cb68ac6d3e486f04373c8632962cac"
company_key: "yc-hyperspell"
company: "Hyperspell"
source_id: "yc-hyperspell-news-import-13fe3511aa44"
canonical_url: "https://www.hyperspell.com/blog/memory-graphs-vs-vector-stores"
published_at: null
first_seen_at: "2026-08-09T23:13:54.426753+00:00"
fetched_at: "2026-08-09T23:13:55.393599+00:00"
content_hash: "sha256:edd2cc9d5bdc3e2bd8d2f413084df862cbaaa37d0ccd5f739c1809c66c246e66"
---

# Memory Graphs vs. Vector Stores: When Relationships Matter

You ask your agent "which customers did the London team close deals with last quarter?" The vector store returns documents that mention London, customers, and deals. Individually relevant chunks. None of them answer the question.


Answering it requires traversing relationships: team members in London, their involvement in specific deals, the customer entities on the other side, filtered by time. Similarity search found similar text. It did not find connected facts.


This is not an argument that graphs are better. Recent research shows GraphRAG underperforms vanilla RAG on many common tasks. The right architecture depends on what your agent actually needs to retrieve. This article offers a decision framework grounded in benchmark data, not vendor preference.


## What vector stores actually do


Vector stores convert text into high-dimensional numerical representations (embeddings) and retrieve content by measuring mathematical distance between vectors. When an agent receives a query, it converts the question into an embedding and finds the stored vectors closest to it.


This works well for a specific class of retrieval problems. "What is our return policy?" lives in a chunk that talks about returns. The embedding captures that semantic similarity, and the top-k results surface the right content. Finding documents about a topic, answering factual lookups, surfacing semantically related content: these are vector search strengths.


Vector stores are the default for good reason. The mental model is simple, the tooling is mature (Pinecone, Weaviate, Qdrant, Milvus), iteration is fast, and operational overhead is low compared to maintaining a graph.


> In plain English: Vector search answers "what text looks like this question?" That is powerful for finding relevant documents, but blind to how those documents connect to each other.


## What memory graphs add


Memory graphs store information as entities (people, projects, documents, concepts) connected by typed relationships ("works on," "reports to," "mentioned in"). Retrieval becomes traversal: follow edges to find context that is structurally connected, not just textually similar.


Where vector search finds documents about Alice, a graph finds Alice's direct reports, the projects she manages, the clients those projects serve, and the Slack conversations where her team discussed those clients. The relationships are explicit, queryable, and traversable.


This structural awareness handles query patterns that vectors cannot: relationship queries ("who reports to Sarah?"), multi-hop reasoning ("what do Alice's teammates think about the new policy?"), temporal sequences ("what changed since Tuesday?"), and entity aggregation ("how many active projects does engineering have?").


The trade-off is build cost. Constructing a memory graph requires entity extraction, coreference resolution (recognizing that "Alice Chen" in Slack and "A. Chen" in the CRM are the same person), schema design, and ongoing maintenance as source data changes. Before Hyperspell, I built Airbnb's first knowledge graph, and the hardest part was never the graph model. It was entity resolution and the maintenance work of keeping the graph honest as the underlying data shifted.


> In plain English: Graph memory answers "what is connected to this through specific relationships?" It sees structure that embeddings flatten away.


## Four query types that break in vector-only systems


Not every query needs graph capabilities. But four patterns consistently fail when vectors are the only retrieval mechanism.


### Relationship queries


"Who reports to Sarah?" requires an organizational structure with explicit reporting relationships. Vector search returns documents that mention Sarah. It does not model the org chart. The highest-similarity chunks might be Sarah's project updates or meeting notes where her name appears frequently, none of which answer the question.


### Multi-hop reasoning


"What do Alice's teammates think about the new policy?" requires three hops: find Alice, find her teammates, find their opinions. No single chunk contains this chain.[Microsoft's GraphRAG research](https://arxiv.org/abs/2404.16130) demonstrated that graph-based retrieval substantially outperforms conventional RAG on these global, multi-hop sensemaking queries.


### Entity-dense queries


The[Diffbot KG-LM Accuracy Benchmark](https://www.falkordb.com/blog/graphrag-accuracy-diffbot-falkordb/) measured LLM accuracy on 43 enterprise-specific questions across categories like operational analytics, KPI tracking, and strategic planning. Without knowledge graph grounding, LLM accuracy was 16.7%. With graph-based retrieval, it reached 56.2%. The critical finding: vector-only accuracy degraded to 0% when queries involved more than five entities. Graph retrieval stayed stable even at ten or more entities per query.


### Temporal queries


"What changed on Project X since last Tuesday?" requires time-aware relationships between events, not semantic similarity. A vector store might surface recent documents mentioning Project X, but it cannot traverse a timeline of changes, identify what differs between states, or filter by temporal relationships.


Query type


Vector approach


Graph approach


Which works


"Find documents about topic X"


Embedding similarity, top-k


Unnecessary overhead


Vector


"Who reports to Sarah?"


Returns docs mentioning Sarah


Traverses org structure


Graph


"What do Alice's teammates think about the new policy?"


May find relevant chunks, cannot chain them


Three-hop traversal


Graph


"How many active projects does engineering have?"


No aggregation capability


Entity count with filters


Graph


"What changed since last Tuesday?"


No temporal awareness


Time-filtered edge traversal


Graph


## When vector stores are enough


Xiang et al. built[GraphRAG-Bench](https://arxiv.org/abs/2506.05690) to systematically evaluate when graph structures help retrieval. Their finding: GraphRAG frequently underperforms vanilla RAG on many real-world tasks, and works best when queries require hierarchical structure between specific concepts.


Vector stores are sufficient when:


**The answer lives in a single chunk.** Question-answering over documents where one passage contains the complete answer. "What is our refund window?" does not need relationship traversal.


**The retrieval task is topical, not relational.** Finding relevant content by subject, surfacing documents for a keyword cluster, or matching user queries to a knowledge base. These are similarity problems that vectors handle well.


**You have a single user with simple context.** Personal knowledge assistants where the agent serves one person. Graph overhead adds complexity without proportional retrieval gains.


**You are shipping an MVP.** Speed-to-market matters more than retrieval depth at the prototype stage. Vector stores let you iterate on the agent's behavior before committing to a more complex architecture.


The latency trade-off matters too.[Han et al. (2025)](https://arxiv.org/abs/2502.11371) measured retrieval efficiency across RAG and GraphRAG variants. KG-based GraphRAG, which relies on LLM-driven entity expansion and multi-step traversal, showed by far the highest retrieval latency. Community-based GraphRAG, by contrast, retrieved faster than vanilla RAG because it matches against precomputed community summaries.


The decision signal: if your users ask "find me X" more than "how does X connect to Y," vectors are probably sufficient.


## When you need a graph


Graph capabilities become necessary when the agent's value depends on connecting facts across sources through explicit relationships.


**Enterprise agents reasoning about organizational structure.** Teams, departments, reporting chains, project assignments, client relationships. This is inherently graph-shaped data. An agent that helps a sales team needs to know which account executive owns which relationship, what deals are in pipeline, and who has context on each account. This is also where connecting workspace data matters: the relationships between people, projects, and communication live across Slack, email, CRM, and calendar, not in a single source.


**Multi-user systems where relationships between users drive value.** Collaboration tools, CRM-connected agents, or any system where the agent needs to understand how people relate to each other and to shared projects.


**Agents that track changes over time.** Preferences evolve, project statuses shift, teams change. If the agent needs to reason about how things have changed (not just what they are now), temporal graph edges provide a mechanism that vectors lack. Attaching custom metadata like timestamps and source labels to each memory makes temporal filtering possible even in simpler architectures.


**Any domain with explicit entity relationships the agent must traverse.** Supply chains, legal contract networks, research citation graphs. If the domain has a natural graph structure, forcing it into embeddings loses information.


The decision signal: if your users ask questions that require connecting facts from different sources through a chain of relationships, you need graph capabilities.


## Hybrid architectures that actually ship


In practice, most production memory systems that go beyond simple document retrieval combine multiple retrieval modes. The question is not "vector or graph" but "which retrieval modes does each query need?"


### Vectors for breadth, graphs for depth


The most common production pattern. Use vector search for initial relevance, then graph traversal to expand with structurally connected context. A query about a customer might start with vector search to find relevant emails and documents, then traverse the graph to pull in the customer's account history, assigned team members, and recent project updates.


### Graph-enhanced retrieval


A lighter-weight approach: start with vector similarity, then rerank using graph proximity. Documents that are both semantically relevant and structurally connected to the query entities rank higher. This adds graph signal without a full traversal pipeline.


### Multi-graph memory


Jiang et al. (2026) introduced[MAGMA](https://arxiv.org/abs/2601.03236) , an architecture that represents each memory item across separate semantic, temporal, causal, and entity graphs. Retrieval becomes policy-guided traversal across these relational views. MAGMA outperformed single-store approaches on the LoCoMo and LongMemEval benchmarks by letting the system select which graph structure best serves each query.


### The operational reality


Hybrid means two (or more) systems to maintain. Consistency between stores requires careful engineering. When source data changes, updates must propagate to both the vector index and the graph. Failure modes multiply. Han et al. found that hybrid selection and integration strategies yield consistent performance improvements, but construction costs are substantially higher than either approach alone.


At Hyperspell, we've built multi-source retrieval across 50+ integrations. The system uses semantic, hybrid, and graph search methods depending on the query. In practice, this means you can weight different data sources based on query context:


` from hyperspell import Hyperspell client = Hyperspell(api_key="API_KEY", user_id="YOUR_USER_ID") response = client.memories.search( query="which customers did the London team close deals with last quarter?", sources=\["slack", "gmail", "hubspot"\], options={ "hubspot": {"weight": 1.5}, "slack": {"weight": 0.5} } )`


Source weighting lets you bias retrieval toward the sources most likely to contain structured relationship data (CRM, project management) while still pulling in conversational context from communication channels. The architecture question we've found most useful is not "which store" but "what retrieval modes does this query type require."


## Choosing your architecture


Start with the simplest architecture that serves your users' actual queries. Add complexity when retrieval failures show you need it, not before.


Your agent needs to...


Start with


Consider adding


Answer questions about documents


Vector store


Nothing yet


Track user preferences across sessions


Vector store with metadata


Graph for preference relationships


Reason about teams, projects, org structure


Graph store


Vectors for document content


Handle multi-hop queries across multiple sources


Hybrid architecture


Multi-graph if complexity warrants it


Serve enterprise users with structured business data


Graph-first with vector supplement


Temporal graphs for change tracking


The worst architecture is the one you built for queries your users never ask. Instrument your retrieval pipeline. Look at which queries fail, what those failures share, and whether they are similarity problems (vector) or relationship problems (graph). Let the data guide the decision.


Hyperspell's[evaluation API](https://docs.hyperspell.com/usage/evaluation) can help here: it measures retrieval quality across queries so you can identify where your current architecture falls short before committing to a more complex one.


## Frequently asked questions


### Can I add graph capabilities to an existing vector store?


Yes. The most common approach is to maintain a separate graph alongside your vector index, with entity references linking the two. You do not need to replace your vector store. Start by building a graph for the specific entity types and relationships where your retrieval is failing, then expand coverage as needed.


### How much does graph retrieval add to latency?


It depends on the implementation. KG-based GraphRAG that requires LLM-driven entity expansion can multiply retrieval time several times over. Community-based approaches that use precomputed summaries can be faster than vanilla vector search ([Han et al., 2025](https://arxiv.org/abs/2502.11371) ). The latency cost correlates with traversal depth and whether any LLM calls happen during retrieval.


### Do I need a dedicated graph database like Neo4j?


Not necessarily. Some memory platforms (including Hyperspell) handle graph-based retrieval as part of their search infrastructure. For custom implementations, Neo4j and Amazon Neptune are common choices, but lighter approaches using in-memory graph structures or property stores work for smaller-scale applications.


### What is the difference between GraphRAG and a knowledge graph?


A knowledge graph is a data structure: entities connected by typed relationships. GraphRAG is a retrieval strategy that uses a knowledge graph (constructed from your documents) as the retrieval substrate instead of, or alongside, vector embeddings. GraphRAG encompasses the full pipeline: graph construction, community detection, retrieval, and generation. A knowledge graph is one component of that pipeline.


## Key takeaways


-


Vector stores handle "find similar text" well. They break on relationship, multi-hop, and entity-dense queries.


-


Graph memory preserves structural connections that embeddings flatten. It excels when queries require traversing relationships between entities.


-


GraphRAG is not universally better. It frequently underperforms vanilla RAG on single-hop factual queries and adds significant build complexity ([Xiang et al.](https://arxiv.org/abs/2506.05690) ).


-


Vector-only accuracy degraded to 0% on queries with more than five entities in the[Diffbot KG-LM Benchmark](https://www.falkordb.com/blog/graphrag-accuracy-diffbot-falkordb/) . Graph retrieval stayed stable at ten or more.


-


Hybrid architectures combining vector breadth with graph depth are the production standard for serious agent memory systems.


-


The right question is not "vector or graph" but "which retrieval modes does each query type need."


-


Start with the simplest architecture that serves your users' actual queries. Add graph capabilities when retrieval failures show you need them.
