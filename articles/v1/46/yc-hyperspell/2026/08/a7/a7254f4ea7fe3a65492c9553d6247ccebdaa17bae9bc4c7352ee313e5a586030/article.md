---
schema_version: "1.0.0"
document_id: "a7254f4ea7fe3a65492c9553d6247ccebdaa17bae9bc4c7352ee313e5a586030"
company_key: "yc-hyperspell"
company: "Hyperspell"
source_id: "yc-hyperspell-news-import-13fe3511aa44"
canonical_url: "https://www.hyperspell.com/blog/context-graph-lifecycle"
published_at: null
first_seen_at: "2026-08-09T23:13:54.426753+00:00"
fetched_at: "2026-08-09T23:13:55.393599+00:00"
content_hash: "sha256:305dac373626ec6e31960e30757ce4f852f3050637bd3065be0e04002145f46b"
---

# The Context Graph Lifecycle: From Raw Data to Structure

An agent that "knows" your workspace is doing four jobs in sequence. It pulls raw data from a dozen sources. It structures that data into entities and relationships. It keeps the structure honest as the world changes. And it serves a query in time for the user not to notice. Skip any one of those four and the agent breaks. Most blog posts about knowledge graphs explain the first job and stop.


I've been building this class of system for a while: before Hyperspell, I built Airbnb's first knowledge graph, and most of the lessons in this piece were learned the expensive way. When teams say "let's add a knowledge graph," they usually mean phase one. Extract entities once, ship a demo, declare victory. The production reality is the next three phases, and that is where projects stall. Microsoft's GraphRAG paper and Zep's Graphiti research both publish their lifecycles for a reason: the architecture decisions made in ingestion compound through every later phase. This piece walks through what each phase actually involves, with the latency and quality trade-offs each one imposes.


## What is a context graph?


A context graph is a set of entities, typed relationships, and metadata structured for an AI agent to consume rather than for a human to browse.


The distinction from a traditional knowledge graph matters. A knowledge graph aims for completeness over a domain, optimized for human queries and exploratory traversal. A context graph is query-driven, pruned, and provenance-rich. It carries connected facts ranked for relevance to a specific question, not an encyclopedia waiting to be browsed.


Why do agents need structure, not just similar text? Similarity surfaces topically related text. Structure surfaces who reports to whom, what changed since Tuesday, which documents the new policy supersedes. Vector search finds matches. A context graph traces connections.


> **In plain English:** A context graph is the smallest connected slice of your data that answers a query, not the full map.


## Phase 1: Ingestion


Ingestion turns documents and events into nodes and edges. It is latency tolerant and accuracy critical. What you extract here determines what every later phase has to work with.


### Data extraction


Workspace data is the hard mode of ingestion. Email threads, chat messages, shared docs, CRM records, calendar events, support tickets. Each source has a different format, a different permission model, and a different way of telling you that a record was deleted. Format normalization (HTML, PDFs, Markdown, structured rows, transcripts) loses fidelity differently in each direction. Teams building this from scratch usually underestimate the surface area by an order of magnitude.


### Entity extraction


Two approaches dominate: dedicated NER models and LLM-based extraction. Recent benchmarking finds fine-tuned LLMs and encoder baselines like BERT-large both reach F1 scores in the low 90s on CoNLL-2003 ([arXiv 2601.17898](https://arxiv.org/abs/2601.17898) ). Traditional models still win on cost and CPU footprint, which matters when you are running extraction over millions of records.


Rule-based extraction earns its keep for high-precision identifiers: emails, ticket numbers, SKUs, calendar event IDs. For relationship-heavy text,[Microsoft's GraphRAG](https://arxiv.org/abs/2404.16130) prompts an LLM to extract entities, relationships, and short descriptions per chunk. That choice trades cost for recall. The right answer depends on how dense your relationship structure needs to be.


### Relationship extraction


Explicit relationships from structured sources are cheap and high-confidence. The org chart, calendar invites, project memberships, GitHub PR reviewers. These come pre-typed.


Implicit relationships from co-occurrence are harder. Two people mentioned in the same doc, two projects addressed in the same email thread. Without confidence scores, these noisy signals pollute the graph. Edge typing is where naive systems flatten: "knows," "owns," "mentioned," and "discussed" all collapsing into a single generic relationship loses the structure that made the graph valuable.


### Chunking and embedding


Documents become retrievable units, with embeddings on each unit for semantic recall. Chunk granularity is a tuning parameter, not a default. Embeddings live alongside the graph, not instead of it. Hybrid retrieval is the production norm, and the chunking strategy needs to support both vector lookups and the entity references that tie chunks back to graph nodes.


### Latency characteristics


Ingestion is batch-tolerant. Bulk backfills run in minutes to hours; incremental updates run in seconds to minutes. Microsoft's GraphRAG runs full hierarchical indexing offline before queries are served. That is by design.


**In plain English:** Ingestion can be slow. Queries cannot. Most architectural pain comes from teams that did not design for that asymmetry.


## Phase 2: Enrichment


Ingestion gives you nodes and edges. Enrichment gives them meaning, weight, and connectivity.


### Metadata augmentation


Source, timestamp, document URL, author, content type. The audit trail is what makes the graph defensible later. Confidence scores at extraction time let downstream ranking deprioritize weak edges. Classification by content type (decision, status update, action item, request) lets the graph answer "what changed," not just "what exists." The metadata layer is unglamorous and disproportionately important.


### Relationship inference


Transitive linking is where the graph starts paying for itself. Alice is connected to Bob through Project X even when no document mentions both directly. The agent can follow the chain.


Microsoft's GraphRAG runs hierarchical community detection over the extracted graph using the[Leiden algorithm](https://www.nature.com/articles/s41598-019-41695-z) , then summarizes each community with an LLM. Those community summaries become the graph's pre-computed answer to global "tell me about this team" queries. It is the trick that lets graph-based retrieval handle questions that vector search cannot.


### Reference resolution


"The CEO," "John," "@john," and "John Smith (CC'd)" all need to resolve to a single entity. Coreference resolution is a primary lever for graph quality. LLM-driven coreference pipelines have measurably outperformed naive baselines:[LINK-KG](https://arxiv.org/abs/2510.26486) reduced average node duplication by 45.21% and noisy nodes by 32.22%.


The hard case is cross-document, cross-source resolution: the same person in Slack, email, the CRM, and a Google Doc. Get this wrong and the agent treats one person as four. Get it right and the graph starts looking like how the org actually works.


### Quality scoring


Confidence lives at three levels: the edge (this fact extracted with a 0.92 model score), the source (CRM record beats public webpage), and freshness (today's data beats data from 18 months ago). Quality scores feed query-time ranking. Without them, every edge looks equally true to the retriever, and ranking degrades to dice rolls.


## Phase 3: Maintenance


A graph that does not reflect the world stops being useful. Maintenance is continuous, mostly background, and often invisible until it is not.


### Updating nodes


Source data changes. The doc gets revised, the person changes title, the deal moves stage. Merge versus replace is a real architectural decision. Replacing wipes provenance; merging requires conflict logic.


Bi-temporal models are the design pattern that survives this.[Graphiti](https://arxiv.org/abs/2501.13956) tracks valid and invalid timestamps for facts and created and expired timestamps for storage, preserving history without confusing the present.


### Pruning stale data


Time-based pruning demotes anything older than N months unless reinforced by newer evidence. Access-based pruning removes edges that nothing has ever traversed. Aggressive pruning is risky because stale is not the same as wrong, and the compromise most production systems land on is demotion in ranking rather than deletion.


### Handling deletions


A source delete cascades through the graph. The Slack message is gone, but the entity it referenced may still be valid through other sources. Orphan cleanup removes nodes whose every supporting source has disappeared. Privacy-driven deletes (a GDPR right-to-erasure request, for instance) are not optional. The graph must support targeted removal across all related edges, end to end.


### Conflict resolution


New data contradicts old. Graphiti uses an LLM to compare new edges against semantically related existing edges; when temporally overlapping contradictions exist, it invalidates the affected edges by setting their invalid timestamp to the valid timestamp of the new edge. The contradicted fact is not deleted, just marked as no longer current.


> **In plain English:** The graph is not the truth. It is the audit trail of what was true and when. Treating it that way is what makes it survive contact with reality.


## Phase 4: Query


Query is latency-critical. Everything upstream optimizes for what happens here.


### Graph traversal


Path finding between entities, relationship-typed traversal, k-hop neighborhoods. Multi-hop traversal on dense subgraphs can eat hundreds of milliseconds and grows with graph size. Index-assisted traversal helps. Pre-computed community summaries help more. The query layer is where the enrichment investments pay off in measurable speed.


### Vector search


Embeddings on node content give semantic similarity at query time, and ANN indexes keep vector retrieval fast, which is part of why hybrid search is the production norm. Vectors give breadth, the graph gives depth. At Hyperspell, indexed search combines semantic, hybrid, and graph methods over indexed workspace data, with live search as the option for queries that need to hit source APIs at request time ([core concepts](https://docs.hyperspell.com/core/concepts) ).


### Relevance ranking


Ranking inputs include graph proximity, semantic similarity, edge confidence, source weight, and recency. Personalization signals (this user's projects, teammates, recent docs) sharpen ranking without retraining anything.


A useful caveat: graph retrieval is not free. Systematic comparisons find GraphRAG underperforms vanilla RAG on many real-world tasks; the win condition is hierarchical or structured queries, not blanket use ([Han et al.](https://arxiv.org/abs/2502.11371) ;[Xiang et al.](https://arxiv.org/abs/2506.05690) ). The decision about when to lean on graph traversal versus vector search is a per-query call, ideally made by the system rather than the user.


### Permission filtering


An edge that exists in the graph but should not exist for this user must not appear in results. Filtering after retrieval is wrong: the agent sees data it should not see, even briefly. Filtering during retrieval is the only correct pattern. This is the phase where build-from-scratch projects discover the integration cost they ignored at the beginning.


## Operational characteristics by phase


The asymmetry in latency and compute profiles is the design constraint. Architectures that ignore it fail in production.


Phase


Latency profile


Compute profile


Failure mode


Ingestion


Batch-tolerant (minutes to hours)


CPU/GPU heavy on extraction


Extraction errors, schema drift, source rate limits


Enrichment


Async, medium latency


LLM calls, expensive


Coreference errors, low-quality edges, hallucinated relations


Maintenance


Background, continuous


Lightweight per event, heavy on backfill


Stale data, unbounded graph growth, missed deletions


Query


Latency-critical (sub-second target)


Index lookups, traversal, ranking


Slow traversal, missing permissions, ranking misses


Read the table the way you would read a budget. The query column has the tightest constraints, so every other column has to absorb work. Pre-compute community summaries to keep traversal fast. Run coreference at enrichment time so ranking does not have to deduplicate at query time. Index permissions alongside content so filtering does not become a join.


## Building context graphs that last


A few patterns hold across the projects we have seen ship and the ones that have stalled.


Invest in enrichment quality before scaling ingestion volume. Coreference and confidence scoring compound; bad ingestion at scale becomes a liability faster than people expect. LINK-KG's reductions of 45% in node duplication and 32% in noisy nodes show how much there is to gain at this layer.


Plan maintenance from day one. Bi-temporal modeling, conflict resolution semantics, and deletion handling are easier to design in than to retrofit. Teams that bolt these on after launch tend to discover that retrofitting timestamps means rewriting every query that already shipped.


Monitor graph health. Coverage (what fraction of source data is represented), freshness (median age of edges per source), and accuracy (confidence distribution, contradiction rate). These three dashboards catch most production issues before users do.


Start narrow. The graph that wins is the one whose four phases all work end to end on a small domain, not the one whose phase one is impressive on a huge corpus. We have watched ambitious teams ship broad and brittle, and smaller teams ship narrow and durable. The narrow approach wins more often.


At Hyperspell, we run this lifecycle across 50+ connected sources, with the index updated continuously and queries served through hybrid semantic, vector, and graph methods. The lessons in this piece are what we have learned operating it, not what we would put in a whitepaper. As a concrete example, adding a record into the memory layer and querying across multiple sources looks like this with the Python SDK:


` from hyperspell import Hyperspell client = Hyperspell(api_key="API_KEY", user_id="user_123") # Ingest: add a record into the memory layer client.memories.add( text="Q2 planning notes: Alice owns the migration; ship date is June 15.", metadata={"collection": "planning"} ) # Query: search across multiple connected sources response = client.memories.search( query="who owns the Q2 migration?", sources=\["vault", "slack", "gmail"\] )`


The four phases are running underneath that interface. They have to, or the query does not return anything useful.


## Frequently asked questions


### How is a context graph different from a knowledge graph?


A knowledge graph aims for comprehensive coverage of a domain and optimizes for human exploration. A context graph is query-driven, pruned, and provenance-rich, optimized for an AI agent to consume in a single retrieval. Both use entities and relationships, but the design pressure points are different.


### How long does it take to build a context graph that's actually useful?


For a single source and a narrow domain, a working prototype takes days. A production system spanning multiple workspace sources, with maintenance and permission handling, is typically an effort measured in months, not weeks. The longer tail is in the enrichment and maintenance phases, not the initial extraction.


### Can I run a context graph on a vector database, or do I need a graph database?


You can run a small context graph on a vector database with metadata-based relationship encoding, but you lose efficient multi-hop traversal. Production systems usually pair a graph store (Neo4j, FalkorDB, or a managed equivalent) with a vector index. Hybrid retrieval is the production norm.


### What's the latency budget for graph-based retrieval at query time?


Aim for well under a second end to end, with most of that budget spent on retrieval and ranking. Multi-hop traversal on dense subgraphs is the expensive part, so pre-computed community summaries and aggressive caching are usually required to hit the target.


### How do I handle stale data without breaking the audit trail?


Use a bi-temporal model. Track when a fact was true (valid time) separately from when it was stored (system time). When new evidence contradicts old, mark the old edge invalid rather than deleting it. The audit trail stays intact, current queries see only valid facts, and historical queries can still reach back.


## Key takeaways


-


A context graph is four phases (ingestion, enrichment, maintenance, query), and skipping the back three is why most projects stall.


-


Coreference resolution and confidence scoring are the highest-leverage enrichment investments; LINK-KG showed 45% reductions in node duplication from improved coreference.


-


Bi-temporal modeling (when a fact was true versus when it was stored) is the design pattern that survives real-world updates.


-


Query-time latency is the architectural constraint everything upstream serves; dense multi-hop traversal needs community pre-summaries to stay fast.


-


GraphRAG is not universally better than vector retrieval. The win condition is hierarchical or structured queries.


-


Start narrow. A small graph whose four phases work end to end beats a large one whose maintenance has not been built.


## Sources


-


Edge, D., Trinh, H., Cheng, N., et al. "From Local to Global: A Graph RAG Approach to Query-Focused Summarization." Microsoft Research, 2024. https://arxiv.org/abs/2404.16130


-


Rasmussen, P., et al. "Zep: A Temporal Knowledge Graph Architecture for Agent Memory." 2025. https://arxiv.org/abs/2501.13956


-


Han, H., et al. "RAG vs. GraphRAG: A Systematic Evaluation and Key Insights." 2025. https://arxiv.org/abs/2502.11371


-


Xiang, J., et al. "When to use Graphs in RAG: A Comprehensive Analysis for Graph Retrieval-Augmented Generation." 2025. https://arxiv.org/abs/2506.05690


-


Nguyen, S., et al. "LINK-KG: LLM-Driven Coreference-Resolved Knowledge Graphs." 2025. https://arxiv.org/abs/2510.26486


-


"Assessment of Generative Named Entity Recognition in the Era of Large Language Models." 2026. https://arxiv.org/abs/2601.17898


-


Traag, V. A., Waltman, L., van Eck, N. J. "From Louvain to Leiden: guaranteeing well-connected communities." Scientific Reports, 2019. https://www.nature.com/articles/s41598-019-41695-z


-


Hyperspell Documentation. "Core concepts." https://docs.hyperspell.com/core/concepts
