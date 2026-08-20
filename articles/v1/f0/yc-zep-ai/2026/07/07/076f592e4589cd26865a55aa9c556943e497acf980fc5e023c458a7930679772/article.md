---
schema_version: "1.0.0"
document_id: "076f592e4589cd26865a55aa9c556943e497acf980fc5e023c458a7930679772"
company_key: "yc-zep-ai"
company: "Zep AI"
source_id: "yc-zep-ai-rss-0db2565bfdf1"
canonical_url: "https://blog.getzep.com/how-zep-tracks-provenance-in-agent-memory/"
published_at: "2026-07-14T20:05:25+00:00"
first_seen_at: "2026-07-24T08:02:00.467869+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:0ec1f6b1a690108522b91e4fd12451d1ed4c938009f5537659f47ba008d2d403"
---

# How Zep tracks provenance in agent memory

I had fun presenting this material as a talk at the AI Engineer World's Fair 2026 several weeks ago. This post covers the same ground, with the diagrams from the deck.


Provenance is the record of where a piece of information came from and how it was produced. In agent memory, that means being able to take any fact an agent retrieves and name the raw sources behind it: which chat message, document, or record it was derived from, and what has happened to it since.


## Key takeaways


- **When building memory, LLM extraction breaks source tracking.** Several sources produce one fact whose wording matches none of them, entities merge, and new data invalidates old facts. A scalar` source_id` on the fact cannot represent any of this. Every[agent memory](https://www.getzep.com/?ref=blog.getzep.com) system built on LLM extraction has this problem.
- **Zep records provenance as graph structure.** Raw sources are kept verbatim as *episodes* . Every derived node and edge is associated with the episodes that produced it, written at construction time. Tracing a fact to its source is a graph traversal.
- **The associations are maintained through mutation.** When entities merge, the merged node keeps the episode associations from both sides. A fact accumulates every episode that extracted, reaffirmed, or invalidated it, with` valid_at` and` invalid_at` timestamps.
- **Provenance enables access control.** A metadata tag applied to an episode at ingestion is readable on every fact derived from it. Zep's attribute-based access control and search filters evaluate these projected attributes, so retrieval can be scoped by source without a separate access system.
- **Deletion propagates along the same associations.**[Graphiti](https://github.com/getzep/graphiti?ref=blog.getzep.com) 's` remove_episode` removes a fact or node only when no remaining episode supports it, which is the propagation behavior a right-to-be-forgotten implementation needs.


## The retrieval you can't trust


An agent asks its memory about a patient and gets back a single sentence: "Patient has a penicillin allergy." The graph that produced that sentence synthesized it from three sources: an EHR export, a PDF lab report, and something the patient typed into an intake chat.


The distinction matters. An allergy recorded by a clinician in the EHR and an allergy the patient mentioned in passing carry different weight in a treatment decision, but the retrieved sentence reads identically in both cases. If the pipeline discarded the source link, the agent presents both with the same confidence, and a clinician acting on the fact has no way to check which one they got.


This applies well beyond healthcare, and beyond graphs. Any pipeline where an LLM reads sources and writes something new produces output that appears in no source verbatim: summaries, extracted facts, structured records. When your agent retrieves a fact, can you name the exact source payload behind it? In many systems built this way, you can't.


**The retrieved fact reads cleanly; the sources that produced it are no longer attached.**


## Why a source pointer on the fact isn't enough


The obvious fix is to store a` source_id` on the fact. In a data warehouse this works. An ETL pipeline copies or transforms one value deterministically, lineage is one-to-one, and a single pointer records it exactly.


LLM extraction breaks the assumptions behind that pointer in three ways:


1. **Many sources, one fact.** The extraction prompt contains several sources, and a produced fact may draw on any subset of them. Its wording matches none, so you can't recover the mapping later by string matching.
2. **Entities merge.** "J. Smith" and "John Smith" resolve into one identity whose facts came from different sources. A pointer attached before the merge points at the wrong grain after it.
3. **Facts invalidate.** New data supersedes old facts, so the store keeps changing underneath the pointer. An append-only log of source IDs drifts away from the current state of the graph and becomes unwieldy to manage.


Lineage in this setting needs to be a set of links per fact, it needs to stay correct as the graph mutates, and it needs to be queryable, since the reason to keep it is to filter and trace by origin.


**A single source pointer holds in deterministic ETL and breaks under generative extraction.**


## How Zep models provenance


Set-valued links between facts and sources, maintained through mutation and exposed to queries, are relationships, and Zep models them on the graph itself. Three primitives carry the design.


- **Episodes** are the raw sources: a chat message, a document chunk, a JSON record. Zep keeps them byte-for-byte as ingested, and they remain retrievable.
- **Entities** are the nodes the LLM extracts across episodes.
- **Facts** are time-stamped relationships on the edges between entities. The penicillin sentence is the` fact` property of a` HAS_ALLERGY` edge between a Patient node and a Penicillin node.


Every artifact Zep derives is associated with the episodes it came from, and the associations are written during construction rather than reconstructed from logs afterward. A node is associated with every episode that mentions its entity. An edge carries an` episodes` list holding every episode that contributed to its fact. Tracing a fact back to its source payload is a traversal over these associations, using the same query machinery as any other traversal. Graphiti, Zep's open-source graph construction layer, implements this structure, and Zep runs it in production.


**Derived entities and facts sit above; verbatim episodes are the ground truth below.**


## Keeping the associations correct as the graph changes


Recording an episode link at first write is straightforward. The engineering work is in keeping the links correct while the graph mutates, which happens in three places.


**Entity resolution.** When "J. Smith" and "John Smith" resolve to one node, the merged node inherits the episode associations from both sides. Dropping either set silently orphans a source: facts remain in the graph with no path back to the episode that produced them.


**Multi-source facts.** An edge is associated with the episode that first extracted the relationship, plus any later episode that reaffirms the same fact or invalidates it. The association set grows over the life of the fact rather than being overwritten.


**Temporal invalidation.** When a new episode contradicts an existing fact, Zep records which episode did the invalidating and when. Facts are bi-temporal:` valid_at` and` invalid_at` track when something was true in the world, separately from when the system learned it. An invalidated fact stays in the graph with its full history rather than being deleted.


Doing this on every write costs inference. Over the past six months, optimizations across Graphiti and Zep cut LLM inference cost for graph construction by 40%, and Zep's fine-tuned graph-building models extract with higher fidelity than frontier models at lower latency and cost.


**Merges union lineage, facts keep every parent, and invalidation records which episode superseded a fact and when.**


## Metadata projection and access control


The episode associations also carry metadata. You tag an episode once at ingestion with its source system, verification status, department, or classification, and that metadata is readable on every artifact derived from the episode. Zep evaluates access policy against this projected metadata.[Attribute-based access control](https://help.getzep.com/attribute-based-access-control?ref=blog.getzep.com) matches a policy's required values against an object's effective metadata to decide which objects an API key may read.


Search queries may also be[filtered by episode-sourced metadata](https://help.getzep.com/searching-the-graph?ref=blog.getzep.com) . A query like "only facts from verified clinical sources" is a filter over episode ancestry that already exists in the graph.


**Tag the episode once at ingestion, and every artifact derived from it inherits the tag for filtering and access control.**


## Any parent, or all parents?


Mixed-trust parentage makes provenance filtering more than an equality check. Suppose a fact has three parent episodes, two tagged` verified=true` and one not. Its effective` verified` metadata is` \[true, false\]` . Whether that fact counts as verified depends on which failure mode is worse for the query at hand.


For an allergy flag, the dangerous failure is the false negative. An unverified self-report should still block a prescription, so the fact should surface if any parent is verified, or indeed regardless. For a fact like "consent is on file for the procedure," the dangerous failure is the false positive, and the agent should act only when every parent is verified.


The two cases have the same graph shape and opposite policies, which is why the predicate can't be hardcoded into the store. Zep exposes explicit AND and OR predicate groups over episode metadata in graph search, and ABAC policies evaluate the same effective metadata. Your application business logic chooses the predicate per use case.


**Mixed-trust parentage forces a deliberate choice: any verified parent, or all of them.**


## Deletion and the right to be forgotten


Retention policies and right-to-be-forgotten requests require[deleting a source](https://help.getzep.com/deleting-data-from-the-graph?ref=blog.getzep.com#delete-an-episode) and everything derived from it, and nothing else. If three conversations independently established the same relationship, deleting one of them must not remove the fact.


The episode associations make the cascade rule simple to state: when an episode is deleted, a fact or node is removed only when no remaining episode supports it. Delete the intake chat in the running example, and the allergy fact survives, because the EHR export and the lab report still support it. "Patient prefers contact by chat" is removed, because the deleted episode was its only support. This ships in Graphiti as` remove_episode` .


Two limits apply. Deleting an episode does not regenerate the summaries of nodes that other episodes still support, and a fact that was already invalidated stays invalidated.` remove_episode` is the propagation layer of an erasure program; the surrounding program of request handling, verification, and audit still has to exist.


**Shared facts survive an episode deletion; single-source facts are removed.**


## One structure, three uses


The design reduces to three decisions: keep sources verbatim, associate every derived node and edge with its source episodes at construction time, and maintain those associations through entity resolution, reaffirmation, and invalidation.


The same associations then serve three functions. Traversed backward, they take you from an agent's answer to a fact to the exact ingested payload, which is how you debug retrieval instead of guessing. Evaluated at query time, they scope what an agent retrieves by source attributes. Traversed on deletion, they propagate erasure to exactly the facts that depended on the deleted source. Graphiti implements the construction layer and is open source:[github.com/getzep/graphiti](https://github.com/getzep/graphiti?ref=blog.getzep.com) .


## Lineage: a scalar pointer versus episode associations


Requirement Scalar` source_id` Episode associations in Zep


Fact from many sources One value, loses the rest **Set of parent episodes**


Entities merge Pointer drops on one side **Union of both association sets**


Fact invalidation No record of what superseded it **Invalidating episode plus` valid_at` /` invalid_at`**


Query by origin Not queryable **AND / OR filters over episode metadata**


Access control Nothing to evaluate **ABAC over effective metadata**


Source deletion Cannot tell what to remove **` remove_episode` cascade**


*A scalar pointer fails once the graph mutates; episode associations support each requirement. Behavior per the Graphiti and Zep documentation.*


## FAQ


**Does tracking lineage slow down graph construction?**


Recording the associations is cheap relative to the LLM inference that builds the graph, and that inference cost has come down: optimizations across Graphiti and Zep cut construction inference cost by 40% over the last six months, with fine-tuned graph-building models extracting at higher fidelity than frontier models at lower latency and cost.


**How does Zep restrict retrieval to trusted sources?**


Metadata attached to an episode at ingestion is readable on every fact and entity derived from it. Graph search accepts AND/OR predicate groups over that episode metadata, and[attribute-based access control](https://help.getzep.com/attribute-based-access-control?ref=blog.getzep.com) evaluates the same effective metadata to decide what an agent (via an API key) can read.


**Who decides whether a fact needs any verified parent or all of them?**


Your application, per use case. Zep exposes the AND and OR predicates; the judgment about which failure mode is worse belongs to the code acting on the fact. An allergy flag and a consent record have the same graph shape and opposite policies.
