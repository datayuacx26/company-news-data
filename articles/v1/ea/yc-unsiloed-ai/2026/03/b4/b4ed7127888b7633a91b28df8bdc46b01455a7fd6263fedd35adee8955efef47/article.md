---
schema_version: "1.0.0"
document_id: "b4ed7127888b7633a91b28df8bdc46b01455a7fd6263fedd35adee8955efef47"
company_key: "yc-unsiloed-ai"
company: "Unsiloed AI"
source_id: "yc-unsiloed-ai-news-import-f01c67e8267b"
canonical_url: "https://www.unsiloed.ai/blog/why-section-references-in-contracts-fail-after-chunking"
published_at: "2026-03-05T00:00:00+00:00"
first_seen_at: "2026-07-24T06:07:01.705483+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:bfd5a497b68ffb162a158019c7c0a6aaa2f02058bc0f06d5c7c0b36d3750c00d"
---

# Why Section References in Contracts Fail After Chunking

A credit agreement clause reads: *"The Borrower shall maintain a Debt Service Coverage Ratio of not less than 1.25:1.00, calculated in accordance with Section 1.01 and subject to the adjustments set forth in Schedule 2.05(b)."*


One sentence. Two cross-references. Both point to content dozens of pages away.


### Cross-references become dead pointers after chunking


When this agreement is chunked for retrieval, the clause lands in one chunk, the definitions in Section 1.01 in another, and Schedule 2.05(b) in a third. The words "Section 1.01" are preserved as plain text but function as a dead pointer - syntactically present, semantically broken.


A query for "what is the DSCR covenant" surfaces the covenant chunk. The model sees the references and either ignores them (incomplete answer) or hallucinates what Section 1.01 likely contains (plausible but potentially wrong).


This is not an edge case. A typical 80-page credit agreement contains hundreds of internal references - definitions referencing other definitions, covenants referencing schedules, representations referencing exhibits. The document is a directed graph of interconnected provisions. Chunking breaks the edges.


A single DSCR definition in a real credit agreement, referencing five terms defined elsewhere in the document.


### Why overlap windows don't help


The standard fix for boundary problems is increasing chunk overlap, typically 10-20%. This works when context is nearby. Section references in contracts are not proximity problems - they are architectural ones. Section 1.01 (definitions) sits at the beginning; a covenant in Article V references it from 40+ pages later. A 20% overlap on an 80-page document covers roughly 8 pages. The gap is 40+.


Overlap assumes context locality. In contracts, this is structurally false. A definitions section exists to centralize meaning and be referenced from everywhere else. The architecture is designed to separate definition from usage.


### Alias chains compound the problem


Cross-references interact with defined terms to create alias chains. Section 1.01 defines "Debt Service Coverage Ratio" using "Consolidated EBITDA" and "Consolidated Debt Service" - themselves defined terms referencing further terms.


Fully resolving the covenant means traversing a chain: the clause references Section 1.01, which defines DSCR using sub-terms, each defined using further sub-terms. The total context spans 3-4 pages of definitions, the covenant itself, and Schedule 2.05(b). No single chunk contains all of this.


Semantic similarity search cannot follow these chains. It retrieves chunks mentioning "DSCR" but has no way to know that "Consolidated EBITDA" - which never mentions DSCR by name - is also required.


### Can agentic RAG solve this?


Partially. An agent that retrieves the covenant, reads the cross-reference, and fetches Section 1.01 can resolve the first hop. With explicit cross-reference following and cycle detection, multi-hop chains are tractable.


But each hop costs a retrieval call and an LLM invocation. A single DSCR query might need 4-5 round trips: the covenant, the DSCR definition, the Consolidated EBITDA definition, the Consolidated Debt Service definition, and the schedule. Latency compounds. Token costs compound. And the agent must correctly identify every term that needs resolution - missing one produces a silently incomplete answer. Across hundreds of queries over dozens of agreements, agentic resolution becomes expensive and fragile in ways that pre-processing does not.


### Resolving references at ingestion


The more robust approach is resolving references during ingestion, not at query time. The system builds two indexes during parsing: a reference index mapping every section number, schedule, and exhibit to its location, and a defined-term index linking every capitalized term to its definition span.


When the parser encounters a cross-reference, it annotates the chunk with a link to the referenced content. Two strategies, each with tradeoffs:


**Inlining** expands referenced definitions directly into the chunk. Each chunk becomes self-contained, but size bloats. Some definitions run to hundreds of words; recursive inlining can push chunks well beyond context limits. Amendment maintenance is also a problem - change one definition and every chunk that inlined it must be re-processed.


**Linked retrieval** stores pointers to dependency chunks instead of inlining them. At query time, the retriever fetches the matching chunk and its linked dependencies as a group. Chunk sizes stay manageable, but the retrieval layer must know how to follow links. Standard vector similarity search won't do this. It requires either a graph-aware retrieval layer that traverses dependency edges, or a post-retrieval expansion step that pulls in linked chunks before passing context to the model.


Neither approach is free. But both shift cost from query time - where it compounds across every request - to ingestion time, where it is paid once per document. For credit agreements, where reference structure is dense but stable, this tradeoff is almost always worth making.


---
