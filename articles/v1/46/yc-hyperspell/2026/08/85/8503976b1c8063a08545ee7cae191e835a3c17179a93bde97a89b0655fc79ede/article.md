---
schema_version: "1.0.0"
document_id: "8503976b1c8063a08545ee7cae191e835a3c17179a93bde97a89b0655fc79ede"
company_key: "yc-hyperspell"
company: "Hyperspell"
source_id: "yc-hyperspell-news-import-13fe3511aa44"
canonical_url: "https://www.hyperspell.com/blog/permission-aware-retrieval"
published_at: null
first_seen_at: "2026-08-09T23:13:54.426753+00:00"
fetched_at: "2026-08-09T23:13:55.393599+00:00"
content_hash: "sha256:fb202d6a964cdd1c39ae8b95334b2677706bb377be90e22bb2ce20b1b248e3c1"
---

# Permission-Aware Retrieval: The Architecture Explained

An AI agent retrieved salary negotiation data from an HR document and surfaced it to a junior employee who was asking about team headcount. The data was relevant to the query. The retrieval was correct. The access was not.


This is not a hypothetical edge case. It is the default behavior of any memory system that indexes workspace data without understanding who is allowed to see what. Enterprise pilots pass every technical benchmark, demonstrate strong retrieval quality, and then get blocked in security review because no one can explain how the system respects org-chart boundaries.


The problem is widely misunderstood. "Who can see what" is not an access control bolt-on to add before launch. It is a retrieval architecture problem that shapes how a system indexes, stores, queries, and returns memories. The permission model determines the indexing strategy, metadata schema, query pipeline, and latency profile.


This guide covers the permission models available for memory systems, the enforcement patterns that implement them, the performance tradeoffs each pattern introduces, and the mistakes that lead to data leakage in production.


## What is permission-aware retrieval?


Permission-aware retrieval ensures every memory or document fragment returned by a retrieval system respects the requesting user's access rights at query time.


Access control asks whether a user can authenticate or reach a service. Permission-aware retrieval asks a different question: given an authenticated user with a valid query, which results are they allowed to see?


Traditional applications solve this with WHERE clauses. A SQL query filters rows by user ID, team membership, or role. Memory systems use similarity search, which has no native concept of permissions. A cosine similarity score measures relevance. It says nothing about access rights.


This gap is why[Glean](https://www.glean.com/perspectives/security-permissions-aware-ai) frames permissions-aware frameworks as essential for enterprise AI deployment. Security reviews require it. Compliance frameworks demand it. Organizations have information hierarchies that predate any AI system by decades.


> **In plain English:** Permission-aware retrieval is the difference between "this document is relevant" and "this document is relevant, and this user is allowed to see it." Without it, similarity search will surface restricted content to anyone who asks the right question.


## Why are permissions harder for memory than for apps?


Memory systems face three permission challenges that traditional applications avoid: cross-source aggregation, temporal permission drift, and similarity-based retrieval with no native access control.


### Memory aggregates across sources


A single memory might synthesize data from a Slack message, a Google Doc, and a Jira ticket. Source A was visible to the requesting user. Source B was not. Is the derived memory visible?


A row in a relational database has one access policy. A memory node may inherit permissions from multiple sources, each with its own access model.[Research on access control in LLM agent systems](https://arxiv.org/abs/2510.11108) (Li et al., 2025) argues that traditional access control mechanisms were never designed for the dynamic information flows of agentic systems.


### Permissions change over time


A user gets promoted and gains access to the executive Slack channel. Do old memories, ingested before the promotion, become visible? A user leaves a team. Should memories ingested while they had access disappear from results?


Source permissions change, too. A Google Doc moves from team-only to org-wide sharing. Memories derived from it need to reflect the updated state.[Auth0's analysis of access control for AI agents](https://auth0.com/blog/access-control-in-the-era-of-ai-agents/) observes that an agent's effective permissions can shift from moment to moment. The gap between a permission change and its effect on retrieval (call it the staleness window) is a design parameter with direct security implications.


### Retrieval is similarity-based


SQL databases support WHERE clauses natively. Vector similarity search does not. There is no way to append "AND user_has_access = true" to a cosine similarity query. This forces an architectural decision: filter before search, after search, or partition the index entirely. Each choice carries different trade-offs in latency, accuracy, and complexity.


## Permission models for memory systems


Five permission models apply to memory systems, ranging from simple tenant boundaries to full attribute-based policies. Each adds flexibility at the cost of complexity.


### Tenant isolation


Hard boundaries between organizations. Each tenant gets a separate data store or namespace. No query from Tenant A can ever return data from Tenant B.


The simplest model prevents cross-organization leakage entirely. The limitation: tenant isolation does not address access control within the organization. Engineering should not have access to HR compensation data, and executives hold board materials that managers cannot access.


### User-level permissions


Each memory is tagged with its owner. Queries retrieve only that user's memories. This works for personal agents with private memory spaces, but breaks down for shared-workspace agents, where memories are derived from team documents or shared channels.


### Role-based access control (RBAC)


Memories are tagged with required roles. At query time, the system checks the requesting user's roles against the memory requirements. RBAC is well understood, and[authorization tooling for RAG pipelines](https://www.cerbos.dev/features-benefits-and-use-cases/access-control-for-rag) supports it. The catch: roles describe organizational structure, while memory boundaries follow information flow, which often cuts across those lines.


### Document-level ACLs


Inherit permissions directly from source documents. If the user can access the Google Doc, they can access memories derived from it. Lose access, lose the memories. Most accurate and most complex. Every memory must map to its source documents and their current permission state.[LlamaIndex demonstrates this for SharePoint](https://www.llamaindex.ai/blog/permissions-aware-content-retrieval-with-sharepoint-and-llamacloud) , but extending it across dozens of integrations significantly increases complexity.


### Attribute-based access control (ABAC)


Policies evaluate user attributes (department, clearance level), resource attributes (sensitivity, creation date), and contextual attributes (time of day, device).[NIST SP 800-162](https://csrc.nist.gov/pubs/sp/800/162/upd2/final) provides the formal framework. ABAC offers maximum flexibility but introduces maximum policy complexity. Practical for large enterprises with existing ABAC infrastructure; overkill for most AI agent deployments.


### Permission models comparison


Model


Complexity


Best For


Key Limitation


Tenant Isolation


Low


Multi-tenant SaaS, cross-org boundaries


No within-org access control


User-Level


Low


Personal agents, private memory spaces


Cannot handle shared workspace data


RBAC


Medium


Teams with clear role hierarchies


Roles rarely align with information flow


Document-Level ACLs


High


Workspace agents with many integrations


Requires live permission sync per source


ABAC


Very High


Large enterprises with existing ABAC infra


Policy complexity, evaluation overhead


Start with tenant isolation and user-level permissions. Layer RBAC or document-level ACLs only as organizational complexity demands.


## Ingestion-time vs. query-time permission checks


Once a permission model is chosen, the next decision is when to enforce it.


### Ingestion-time approach


Tag each memory with permission metadata (allowed roles, ACL lists, tenant ID) at storage time. At query time, use those tags to filter results without external calls.


Fast and simple. Permission data is co-located with memory embeddings, so latency is predictable. The downside is staleness: stored metadata stays wrong until the memory is re-ingested after a permission change. Works best for static or slowly changing permission environments.


### Query-time approach


Check live permissions against source systems on every query, verifying each candidate memory against the current permission state of its source documents.


Results always reflect the current state, with no staleness. But every external permission call adds latency, and the pipeline now depends on the availability of every source system.[AuthZed](https://authzed.com/blog/authzed-announces-support-for-ai-by-providing-permissions-aware-ai) addresses this by centralizing permission checks in a dedicated authorization service, but the per-query cost never fully disappears.


### Hybrid approach


Cache permissions with a TTL. Subscribe to permission change events via webhooks. Invalidate cache entries on change. Fall back to a live check on a miss.


Most production systems converge on this pattern. It accepts a bounded staleness window (minutes, not hours) in exchange for predictable performance. The staleness window is a design parameter, not a bug.


At Hyperspell, we implement this hybrid pattern across our[integrations](https://docs.hyperspell.com/usage/connect) . Hyperspell Connect gives users granular control over which data sources are connected, and our[authentication layer](https://docs.hyperspell.com/usage/permissions) scopes each user's queries to their authorized data.


## How this looks in practice


A product manager named Sarah asks her AI agent: "What did the team decide about the Q3 pricing change?"


The memory system holds relevant information from three sources: a public Slack channel discussion, a private executive strategy document, and an HR compensation analysis. Here is what happens at each layer of the retrieval pipeline:


1.


**Tenant isolation** confirms that Sarah's query runs against her organization's partition only. Other tenants' data is architecturally unreachable.


2.


**Vector search** identifies all three source memories as semantically relevant to "Q3 pricing change."


3.


**Permission filtering** evaluates Sarah's access. She is a member of the Slack channel, so that memory passes. She does not have access to the executive strategy document or the HR compensation analysis. Both are excluded.


4.


**The agent responds** using only the Slack channel discussion, with no indication that restricted materials exist.


The Hyperspell SDK makes this straightforward to implement. The` user_id` parameter scopes the search to only what Sarah is authorized to see:


` from hyperspell import Hyperspell # Initialize with user-scoped access client = Hyperspell(api_key="YOUR_API_KEY", user_id="sarah@company.com") # Sarah's query returns only documents she has access to results = client.memories.search(query="Q3 pricing change") for doc in results.documents: print(f"{doc.title} (score: {doc.score})") # Returns: Slack channel discussion (accessible) # Filtered: executive strategy doc, HR analysis (not accessible)`


Filtering happens server-side. The calling application never receives documents that Sarah cannot access. The` user_id` parameter (or the` X-As-User` header in REST calls) is the only change needed. See our[permissions documentation](https://docs.hyperspell.com/usage/permissions) for the full API reference.


If Sarah is later promoted to VP, the permission cache updates via webhook. Her next query returns both the Slack discussion and the strategy document. The HR analysis remains filtered because her new role still does not include HR access.


Same query. Same corpus. Different results based on who is asking. For more, see our[core concepts documentation](https://docs.hyperspell.com/core/concepts) .


## Architecture patterns


Three patterns dominate production deployments.


### Pre-filter pattern


Apply permission filters before vector search. Only search the subset of vectors the user is allowed to see, using metadata filters on the vector store, such as[Pinecone's metadata filtering](https://docs.pinecone.io/guides/search/filter-by-metadata) or Weaviate's where filters.


Efficient and predictable. No computation is wasted on inaccessible results. The tradeoff is reduced recall: shrinking the search space may miss relevant results just outside the permission boundary. For users with narrow access, the effective search space can become too small for meaningful similarity matching.


### Post-filter pattern


Retrieve K times N results from vector search, then filter down to N permitted results.


Simpler indexing: no permission metadata needs to be co-located with vectors. The cost is waste and unpredictability. If the user has access to a small fraction of the corpus, large over-retrieval factors are required, or result sets may come back empty. Latency scales with the over-retrieval factor.


### Partition pattern


Maintain separate vector indexes per tenant or permission group. Access control is enforced at the infrastructure level, with no metadata filtering overhead. Fast within a partition; absolute isolation between partitions. Infrastructure cost scales linearly: N tenants means N indexes.[Tencent Cloud documents these isolation strategies in detail](https://www.tencentcloud.com/techpedia/126617) .


At Hyperspell, we enforce tenant data boundaries at the platform level through our multi-tenant architecture and user-scoped authentication.


### Architecture patterns comparison


Pattern


Recall Impact


Latency Profile


Infrastructure Cost


Best For


Pre-Filter


Reduced (smaller search space)


Low, predictable


Moderate (metadata storage)


Most deployments (default choice)


Post-Filter


Full recall preserved


Variable, scales with over-retrieval


Low (no extra indexing)


Large corpus, broad user access


Partition


Full within the partition


Low within the partition


High (linear scaling)


Strict multi-tenant isolation


> **In plain English:** Pre-filter is the right default. Fast, predictable, and safe. Use the partition pattern when organizational boundaries must be enforced at the infrastructure level. Reach for post-filter only when users have broad access to a large corpus and recall matters more than latency predictability.


## Performance implications at scale


Pre-filtering reduces the search space, improving median (p50) latency but potentially hurting recall. Post-filtering inflates retrieval volume, degrading tail (p99) latency. The partition pattern trades query latency for infrastructure cost.


Index size grows with permission metadata. Storing ACL lists, role tags, and tenant identifiers alongside embeddings adds real storage overhead, and it grows with the granularity of the permission model.


Caching requires careful design. A stale permission cache is not a performance problem. It is a data leak. Short TTLs (minutes, not hours) are appropriate in sensitive environments. Longer TTLs are acceptable only when permission changes are infrequent and brief staleness carries low risk.


At Hyperspell, we maintain fast retrieval across our multi-tenant architecture with permission filtering active on every query, using the hybrid pattern described above.


## Common mistakes


**Bolting permissions on after launch.** Retrofitting permission-aware retrieval is an architectural rewrite. The indexing strategy, metadata schema, and query pipeline all change. Design for permissions from day one.


**Trusting source permissions without verifying.** Source APIs can return stale data. A Google Drive API might report cached sharing settings that were updated minutes ago. Design the staleness window with this in mind.


**Caching too aggressively.** A 24-hour permission cache means a terminated employee retains access for up to 24 hours after deactivation. Minutes are the right TTL unit, not hours.


**Omitting permission context from audit logs.** Logs must capture which permission model authorized each retrieval and the conditions under which it was authorized. Without this, compliance and incident investigation are impossible.


**Ignoring derived memories.** If Memory C was synthesized from Documents A and B, and the user loses access to Document B, Memory C should no longer be retrievable. Most systems miss this by tracking permissions at the document level but not at the derivation level. Maintaining provenance links solves this, but only if permission checks traverse them.


## Getting permission-aware retrieval right


Permissions are foundational architecture. The indexing strategy, metadata schema, and query pipeline should be designed around them from day one.


Start simple. Tenant isolation and user-level permissions handle the most critical boundaries. Layer on RBAC or document-level ACLs as organizational complexity demands. The hybrid enforcement approach (ingestion-time metadata with query-time validation and caching) handles most enterprise requirements without sacrificing performance.


Pre-filter is the right default. Switch to the partition pattern for strict multi-tenant isolation. Use post-filter only when the search space is large relative to the user's accessible subset.


At Hyperspell, we build permission-aware retrieval into the memory layer from the ground up: multi-tenant architecture with user-scoped authentication, enterprise-grade security and compliance practices, user-controlled data connections via Hyperspell Connect, and dozens of integrations with per-user query scoping. These are not features added to a retrieval system. They are the retrieval system.


## Key takeaways


-


Permission-aware retrieval is a retrieval architecture problem, not an access control bolt-on. It affects how a system indexes, stores, queries, and returns memories.


-


Memory systems face unique permission challenges: cross-source aggregation, temporal permission changes, and similarity-based retrieval with no native access control.


-


Five permission models (tenant isolation, user-level, RBAC, document-level ACLs, ABAC) offer increasing flexibility at increasing complexity. Start with the simplest model that meets the requirements.


-


The hybrid enforcement approach (ingestion-time metadata plus query-time validation with caching) balances accuracy and performance for most enterprise deployments.


-


Pre-filter is the right default architecture pattern. Partition for strict multi-tenant isolation. Post-filter only when the search space is large relative to the user access scope.


-


Design for permissions from day one. Retrofitting is an architectural rewrite, not a feature sprint.


-


Audit logs must capture permission context alongside retrieval results. Without it, compliance and incident investigation are impossible.


## FAQ


### Does permission-aware retrieval add latency to every query?


Pre-filtering with co-located metadata adds minimal overhead. Query-time checks against external authorization services cost more, since each check is a network round trip. The hybrid approach with cached permissions keeps latency predictable within a bounded staleness window.


### How do you handle permissions for memories derived from multiple sources?


The conservative approach is intersectional: a derived memory is accessible only if the user has access to all source documents. The permissive approach is union: accessible if the user has access to any source. Most enterprise deployments default to intersection. The union model risks exposing restricted information through the context of permitted sources.


### What happens when source document permissions change after memories are ingested?


Ingestion-time tagging serves stale permissions until re-ingestion. Query-time checks always reflect the current state but add latency. The practical solution: subscribe to permission change events, invalidate affected cache entries, and re-tag affected memories in background jobs.


### Can standard vector database features handle permission filtering?


Partially. Most vector databases support metadata filtering (Pinecone, Weaviate, Qdrant), enabling pre-filtering by pattern. Metadata filters operate on exact matches or simple conditions. Complex models like ABAC require external policy evaluation. The vector database handles similarity; the permission layer handles authorization. They are separate concerns.
