---
schema_version: "1.0.0"
document_id: "1d02be5d582ab5563abac74f234a25ec47af88a04ae397739bca66b76f3d99f5"
company_key: "yc-convictional"
company: "Convictional"
source_id: "yc-convictional-atom-2bd8494c6cc9"
canonical_url: "https://get.convictional.com/posts/context-entropy/"
published_at: "2024-12-12T00:00:00+00:00"
first_seen_at: "2026-08-09T21:02:07.552290+00:00"
fetched_at: "2026-08-09T21:02:10.293363+00:00"
content_hash: "sha256:89ba8e6788c896e2d3886413949419c2cd4c55b3357ebeaa0eae57acdcab2157"
---

# Context Entropy

#


At Convictional, we're building infrastructure that helps businesses make better decisions by bridging the gap between AI capabilities and human judgment. One of our core challenges has been developing systems that can effectively capture and utilize an organization's knowledge across all its documents, activities, and people. This piece shares insights from our research into how business knowledge naturally distributes itself across documents, and what that means for building effective knowledge systems.


Large Language Models are incredibly capable, but in business applications they can be limited by their knowledge and understanding of said business' operating model. Context and information of topics within the business sprawls across documentation, activity and people. Not all topics are documented equally however, and so we observe an entropic-like behavior in how the information of a topic is distributed. While some topics are structured and targeted (low entropy), others are highly fragmented, almost homogenous across all documents (high entropy).


To solve for this entropy in 2024, you've probably implemented or at least considered implementing RAG (Retrieval Augmented Generation). The pattern is elegant in its simplicity: when a user asks a question, find relevant documents, stuff them into the context window of an LLM, and let the model work its magic. But if you've deployed RAG in production, you've likely encountered its limitations in addressing core business context challenges:


- **Information Silos** : While RAG can search across repositories, it struggles to meaningfully connect information that lives in disconnected islands across customer, operations, and financial systems. As a topic's entropy increases, this siloed nature is exacerbated.
- **Incomplete Context** : Executive status often creates barriers to authentic information flow. RAG can retrieve documents but can't account for these organizational dynamics that affect how information should be interpreted.
- **Historical Amnesia** : Even when past decisions and their rationale exist in documents, RAG might miss crucial connections or fail to surface them at the right moment, leading to missed learning opportunities.


This leads to a crucial architectural decision: Should you stick with simple document retrieval and RAG, or invest in building a more sophisticated pre-processed knowledge store that can better handle these organizational complexities?


After analyzing over 3,200 entities retrieved from over 2,500 documents spanning decisions, meeting summaries, GitHub issues, and Google Drive docs, we have some concrete answers -- and they might surprise you.


##### The Evidence: What We Found


We analyzed 3,281 distinct entities across 16 categories, tracking over 100,000 individual facts about these entities. What emerged was a fascinating pattern in how knowledge is distributed across documents.


##### The Power Law of Knowledge Distribution


The most striking finding was just how concentrated knowledge tends to be. For most entities:


- 50% of everything we know about them comes from just 1-2 documents
- 95% of their knowledge is contained in 5-6 documents
- Only 4.7% of entities need more than 10 documents to capture 95% of their knowledge


**Fig 1:** The top 500 entities, ranked by total documents their respective facts span, show the super-exponential drop off in the number of documents needed to span 50% - 95% of an entity's facts. We sort entities, descending, by the number of associated documents and plot how many documents are required to reach the varying levels of fact coverage ('x' marks). The y-axis has been logged, meaning small integer values are 'spread' out leading to the plateaus - as we see at the start of the long tail, many documents reach less than 5 documents needed to cover 50% of facts. The vertical spread between the blue (50% fact coverage) and green (95% fact coverage) represents the number of additional documents (log-scale) needed to bridge the fact coverage gap.


This has profound implications for RAG implementation. With typical LLM context windows now handling 8-10 documents comfortably (although LLM attention can still be a limitation), pure RAG is theoretically sufficient for over 95% of entities. But the devil, as always, is in the details.


##### The Impact of Entity Size on Knowledge Distribution


Our analysis revealed a clear pattern: the size of an entity - measured by the number of facts about it - strongly influences how its knowledge is distributed across documents. We identified three distinct categories:


The Long Tail: Small Entities
These small entities are highly efficient from a knowledge management perspective. They rarely cross document boundaries, making them ideal candidates for pure RAG approaches.


The Middle Ground: Medium Entities
Medium entities strike a good balance - while their knowledge spans multiple documents, they remain manageable. Only a small fraction (4.3%) require more than 10 documents for complete coverage.


The Complex Core: Large Entities
These entities present the greatest challenge for knowledge management. Despite being few in number, they are often your organization's most important entities, and their knowledge is spread across many documents.


Size (# of Assoc. Facts) 1 - 10 11 - 100 100+


Proportion of all Entities 67.8% 28.8% 3.4%


Typical Entity Types Individual features, minor projects, peripheral team members Active projects, core products, team leads Mission-critical systems, key personnel, major initiatives


Coverage Pattern Remarkably contained - typically all facts about these entities are found in a single document Moderately spread - typically needs 3-4 documents for comprehensive coverage Widely distributed - requires an average of 94 documents for comprehensive coverage


This size distribution has important implications for system design. While pure RAG works well for over 95% of entities (the small and medium categories), that crucial 3.4% of large entities may benefit from more sophisticated handling through entity pre-processing.


##### Implementation Approaches: Finding the Right Balance


While our data suggests RAG is sufficient for most cases, implementing any approach requires careful consideration of practical tradeoffs. Our production experience has revealed key insights about different implementation strategies:


Pure RAG: The Simple Foundation Entity Pre-processing: When Complexity Adds Value


Advantages Pure RAG offers compelling advantages through its simplicity:


- Straightforward pipeline: index documents, store vectors, retrieve, and pass to LLM
- Always fresh data without maintaining separate knowledge stores
- No schema maintenance overhead
- Lower operating costs Entity pre-processing provides important benefits for specific use cases:


- Improved token efficiency through pre-extracted facts
- Consistent, deterministic entity views
- Ability to synthesize information across many documents
- Type-aware optimization for different entity categories


Challenges However, production deployments reveal important limitations:


- Inconsistent responses due to varying document combinations
- Token inefficiency from including potentially irrelevant content
- Challenges handling that critical 3.4% of large, complex entities But these benefits come with significant costs:


- Complex processing pipelines to maintain including temporal and knowledge decay aspects
- Risk of stale data as information about Entities evolves
- Schema lock-in and migration complexity
- Higher processing and storage overhead even in a lightweight approach


##### A Practical Implementation Strategy


Based on our research and production experience, we recommend a pragmatic approach that starts simple and adds complexity only where it demonstrably adds value:


1.


**Start with Document Processing**


- Implement basic document indexing and RAG
- Add named entity detection during indexing
- Track entity mentions and their distribution across documents
- Monitor which entities are frequently queried


2.


**Measure Entity Patterns**


- Track how knowledge about entities spreads across documents
- Identify entities that consistently require synthesizing information from many sources
- Monitor query patterns to understand which entities are most important to your users


3.


**Selective Pre-processing**


- Begin pre-processing only for entities that:


- Span many documents (typically more than 10 if using full documents)
- Are frequently queried
- Require consistent responses
- Have high business impact


- Continue using simple RAG for the majority of entities


4.


**Continuous Monitoring**


- Track system performance and user satisfaction
- Measure response quality for both RAG and pre-processed entities
- Use this data to adjust which entities deserve pre-processing investment


**Fig 2:** Based on our research, entities fall into distinct categories based on their document distribution. Low entropy entities are concentrated, while high entropy entities spread across multiple documents, requiring different retrieval strategies.


##### Key Takeaways


Our research reveals a power law in how organizational knowledge distributes itself across documents:


1.


**Most knowledge is more concentrated than you think** : For 95% of entities, all their relevant information exists in just a handful of documents, making pure RAG surprisingly effective.


2.


**Size matters** : Entity complexity strongly correlates with knowledge distribution. Small entities (67.8% of all entities) typically need just one document, while large entities (3.4%) may need dozens.


3.


**Start simple, evolve strategically** : Begin with pure RAG and add entity pre-processing only for high-value, frequently-accessed entities that span many documents.


4.


**One size doesn't fit all** : Different entity types benefit from different approaches. Design your system to handle both concentrated and distributed knowledge patterns.


The key insight? Context entropy - the degree to which knowledge about an entity spreads across documents - follows predictable patterns. By understanding these patterns, you can build knowledge systems that are both effective and maintainable, avoiding the trap of over-engineering for edge cases while still handling your most important entities well.


**Fig 3:** The distribution of entities across document counts shows a clear power law pattern, with the vast majority of entities concentrated in fewer than 10 documents.


As we continue building Convictional's decision-making infrastructure, these insights guide our technical choices. We've learned that the best knowledge systems aren't necessarily the most sophisticated - they're the ones that match their complexity to the actual patterns in your data.


**Fig 4:** A practical implementation flowchart showing how to evolve from pure RAG to selective entity pre-processing based on measured entity patterns and business value.


*This research was conducted on Convictional's internal knowledge base spanning 2,500+ documents and 3,200+ entities. Your organization's patterns may vary, but the underlying principles of context entropy likely apply.*
