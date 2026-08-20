---
schema_version: "1.0.0"
document_id: "5658aee3cd2e79dda40d2508deed3939f554d6d6bf97dcd5a2882eaa66e928bc"
company_key: "yc-hyperspell"
company: "Hyperspell"
source_id: "yc-hyperspell-news-import-13fe3511aa44"
canonical_url: "https://www.hyperspell.com/blog/enterprise-memory-at-scale"
published_at: null
first_seen_at: "2026-08-09T23:13:54.426753+00:00"
fetched_at: "2026-08-09T23:13:55.393599+00:00"
content_hash: "sha256:a7db00f6025f316ef0e433a0a2f079fd661bca5589858d86028131ec2928e8fd"
---

# Enterprise Context: What Breaks at 10,000 Users

Your memory system probably worked beautifully for the first 100 beta users. Then someone signed an enterprise contract. Now you are looking at 10,000 seats, an org chart with permission inheritance, a security questionnaire that asks for tamper-evident audit logs, and a procurement team that wants a SOC 2 Type II report by next quarter. None of that is "more of the same."


This is the section of the build-vs-adopt curve where most homegrown memory layers stall. The architectural assumptions that held at 100 users do not survive the jump to 10,000. Three things change at once: the tenant boundary becomes a first-class concern, query performance becomes sensitive to data distribution, and cost compounds across surfaces you forgot to budget for.


The point of this article is not "Hyperspell is enterprise-ready." It is to map the dimensions on which an enterprise rollout breaks, so you can make a clean-eyed call about which problems you are willing to operate yourself, and which you would rather hand to a managed memory layer.


## The scale shift is qualitative, not quantitative


Enterprise memory at 10,000 users is not a database with bigger numbers. It is a different system with new failure modes that arrive together.


The linear-scaling assumption fails for memory specifically because three breakages compound. The tenant boundary becomes a first-class architectural concern, with org-team-individual permission inheritance and the ever-present risk of cross-tenant similarity contamination. Query performance becomes sensitive to data distribution, because the long-tail tenants and skewed memory volumes that did not exist at 100 users are unavoidable at 10,000. And cost stops being a single line item, because storage, embeddings, indexes, governance logs, and connector calls all grow together.


**In summary:** At enterprise scale, a memory layer is not a database with bigger numbers. It is a different system with new failure modes.


Each of the next four sections takes one of those breakages and walks through the architectural choices it forces.


## Multi-tenancy: the boundary you cannot bolt on later


Multi-tenancy is the part most teams underestimate. A tenant boundary that is not designed in from day one is extremely expensive to retrofit, because it touches ingestion, indexing, deletion, and every read path.


The canonical patterns come from Amazon's[SaaS Tenant Isolation Strategies whitepaper](https://docs.aws.amazon.com/whitepapers/latest/saas-tenant-isolation-strategies/saas-tenant-isolation-strategies.html) . Three models keep showing up in production memory systems.


### Silo, pool, and bridge


Model


Isolation


Cost profile


Fit


Silo


Dedicated infrastructure per tenant


Highest operational cost, highest unit economics


Regulated industries, your largest enterprise customers


Pool


Shared infrastructure, tenant-ID metadata filtering


Lowest cost, exposes you to noisy-neighbor effects


Most B2B SaaS scenarios at the lower tiers


Bridge


Hybrid: dedicated knowledge bases per tenant, shared collections


Middle ground


Most SaaS memory layers once they have at least one regulated tenant


The AWS playbook for retrieval workloads is concrete. AWS's own[multi-tenant RAG architecture guide](https://aws.amazon.com/blogs/machine-learning/multi-tenant-rag-with-amazon-bedrock-knowledge-bases/) walks through how each model maps to Bedrock Knowledge Bases: silo gets per-tenant KMS keys end-to-end, pool relies entirely on tenant-ID metadata filtering, and bridge separates knowledge bases per tenant while pooling the underlying search collection.


Production vector databases land in roughly the same place.[Pinecone's multitenancy documentation](https://docs.pinecone.io/guides/index-data/implement-multitenancy) recommends namespace-per-tenant as the primary pattern, with metadata-only filtering as a fallback that scans the full index and costs more per query. The trade-off is real: namespaces give you data isolation, while metadata filtering gives you cheaper writes at the cost of slower, more expensive reads.


### Memory adds a model layer most isolation guides skip


Multi-tenant memory is not just data isolation.[Microsoft's multitenant AI architecture guidance](https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/approaches/ai-machine-learning) is explicit that models inherit the sensitivity of the data that trained them. Tenant-specific models, shared models, and tuned shared models each have different threat profiles. If you ever fine-tune on customer data, the model itself is now part of the isolation story.


The decision rule we use: pick isolation per tenant tier, not per system. Your Fortune 500 customer and your 5-person pilot do not need the same posture, and pretending they do leaves you either over-spending on the small accounts or under-securing the large ones.


## Query performance under realistic load


The benchmark trap at 100 users is uniformly distributed data. Real enterprise traffic almost never looks like that. Three things shift when you cross into 10,000-user territory.


Distribution skew is the first. In the workloads we see, a small fraction of tenants drives the large majority of queries. Naive partitioning leaves the hot tenants slow and the cold ones over-provisioned; the fix is to partition by tenant for query locality and then again by time for archival. Cardinality of metadata filters is the second. Permission-aware retrieval (org × team × project × user) explodes the filter space, and not every vector database handles high-cardinality filters gracefully. Noisy neighbors is the third, showing up in shared inference and shared index time. Microsoft's architecture guide names this as the dominant antipattern in multi-tenant AI workloads.


Caching helps, but cache invalidation across the org-team-individual hierarchy is its own problem. Read replicas help, but the eventual-consistency tradeoffs need to be on the product team's roadmap before they ship, not after a customer notices that yesterday's deletion is still showing up in retrieval today.


The honest framing: performance at 10,000 users is not "the same query, faster." It is a different query mix, on skewed data, with stricter permission filters.


## Cost: the line item that grows fastest is the one you forgot


There are four cost surfaces in an enterprise memory layer, and they get underestimated in roughly this order.


**Embedding generation** is first. At indicative[OpenAI pricing](https://openai.com/api/pricing/) of about $0.02 per million tokens for a small embedding model and $0.13 for a larger one, a continuously indexed enterprise corpus runs up real numbers fast. The bill arrives whether or not anyone runs a query.


**Storage and indexes** are second. Embeddings, vector indexes, governance logs, and audit retention all grow with users and with documents per user. At 10,000 users with even modest workspace footprints, you can find yourself managing hundreds of millions of vectors.


**Connector API calls** are third and easy to forget. Polling 50+ workspace sources for 10,000 users means rate limits, webhook delivery, sync retries, and exponential-backoff logic that has to actually work.


**LLM calls** for summarization, conflict resolution, and freshness checks are fourth. They compound on top of everything above.


Per-tenant metering is the ask that always lands a quarter too late. The first time finance asks "which customer drove the spike," you want to already have the answer.


## Compliance: the questions the security team will actually send


Compliance is where the enterprise rollout often gets gated. The questions that show up in practice are narrower and more specific than the marketing-page version of "enterprise-grade security" suggests.


SOC 2 is the audit framework procurement actually asks for. The[AICPA's Trust Services Criteria](https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2) define the audit scope, with Security as the mandatory category and Availability, Processing Integrity, Confidentiality, and Privacy added based on customer pressure. A Type II report covers operating effectiveness over a 6-12 month window, which means the controls have to be real, not aspirational.


Audit logging is not a feature. It is a regulatory obligation under[GDPR Article 30](https://gdpr-info.eu/art-30-gdpr/) , which requires controllers and processors to maintain records of processing activities. Logs need to be tamper-evident and retained for the periods your auditor signs off on.


Data residency follows from[GDPR Article 44](https://gdpr-info.eu/art-44-gdpr/) and the cross-border-transfer constraints in Chapter V. Enterprise buyers in regulated industries will require EU-hosted infrastructure, not just an EU-headquartered vendor.


Right to erasure under[GDPR Article 17](https://gdpr-info.eu/art-17-gdpr/) is the one that catches memory systems off guard. The right applies to derived memories and embeddings, not just the source documents. Deletion has to cascade through the index, the cache, the audit log, and any LLM-generated summaries. Getting this right is mostly a matter of designing the deletion path before you have 10,000 users to delete from.


External authorization systems become attractive once permissions outgrow what your application database can answer in a single query.[AuthZed's writeup on authorization for AI](https://authzed.com/blog/authzed-announces-support-for-ai-by-providing-permissions-aware-ai) is a useful reference point on Zanzibar-style permission infrastructure, even if you do not adopt SpiceDB itself.


In practical terms: the security questionnaire will ask for an audit report, an evidence package, a DPA, a sub-processor list, a deletion runbook, and an incident response SLA. None of these are bolt-ons. At Hyperspell, we ship with these compliance practices in place and document the architecture, including[security](https://docs.hyperspell.com/concepts/security) and[US/EU data residency](https://docs.hyperspell.com/concepts/data-residency) , at docs.hyperspell.com, which is what the procurement teams we deal with end up referring back to.


## Organizational hierarchies: org → team → individual


The realistic enterprise permission model is three-tier. Org-level memory is company-wide knowledge, written by few and readable by all. Team-level memory is shared within a team and private from other teams. Individual memory is the user's private context and preferences.


Inheritance and override is the part that bites. When org-level says X and team-level says not-X, which wins? Get this wrong and the agent confidently retrieves the contradicted fact. Most teams underspecify this in the data model and end up encoding it in retrieval-time logic, which works until it does not.


Hyperspell's approach is to keep this surface in the data plane: arbitrary custom metadata on memories, queryable with MongoDB-style filter operators ($eq, $gte, $in, and friends), so the org-team-individual hierarchy becomes a metadata filter rather than a separate permission engine. We covered the deeper enforcement architecture in our companion piece on permission-aware retrieval. This section sketches the model; that one covers the implementation.


## An enterprise readiness checklist


Before you take a memory layer, homegrown or vendor, into a 10,000-user rollout, the seven items below should all read "done."


1.


Tenant isolation model is documented per tier, with silo for regulated tenants and pool or bridge for the rest, following AWS's silo-pool-bridge patterns.


2.


Permission model is org → team → individual, with a clear inheritance and override rule.


3.


Query performance has been benchmarked at target distribution skew, not just uniform load.


4.


Per-tenant cost metering exists before the finance team asks.


5.


Audit reporting (SOC 2 Type II or equivalent) and audit log retention are operational, with Trust Services Criteria coverage decided.


6.


Data residency options exist for at least the EU, in line with GDPR Article 44.


7.


Right-to-erasure deletion cascades into derived memories, embeddings, and logs, satisfying GDPR Article 17.


Tenant-scoped retrieval with metadata filtering is the operational primitive most of these depend on. With Hyperspell, that looks like this:


` from hyperspell import Hyperspell client = Hyperspell() response = client.memories.search( query="Q3 revenue forecast", sources=\["vault", "google_drive", "slack"\], options={ "filter": { "tenant_id": {"$eq": "acme-corp"}, "team": {"$in": \["finance", "leadership"\]}, "confidential": False } } )`


The shape is mundane on purpose: a tenant filter, a team filter, a confidentiality filter, and the same multi-source search that answers the simpler queries. The full filter operator set is documented at[docs.hyperspell.com/usage/metadata](https://docs.hyperspell.com/usage/metadata) .


## Frequently asked questions


### When does silo isolation become required versus pool?


Silo isolation tends to be required for regulated industries (healthcare, financial services, government) and for your largest enterprise customers who want dedicated infrastructure as a contractual term. Pool works for most lower-tier B2B SaaS scenarios. Bridge models split the difference, and most production memory layers end up there once they have at least one regulated tenant. AWS's tenant isolation whitepaper covers the trade-offs in depth.


### How does memory deletion satisfy GDPR right to erasure?


Right to erasure under Article 17 applies to derived memories and embeddings, not just source documents. Compliant deletion has to cascade into the vector index, any cached representations, and downstream summaries, while preserving the records-of-processing trail required by Article 30. The hard part is designing the cascade before you have 10,000 users.


### Do I need SOC 2 if my customers are not asking for it yet?


Probably yes if you sell upmarket. The AICPA Trust Services Criteria define the audit scope; Security is mandatory and the others are added based on customer pressure. Type II covers operating effectiveness over 6-12 months, so starting the controls work months before the first enterprise customer asks is the realistic timeline.


### Is multi-tenant memory just multi-tenant RAG?


Related but broader. Multi-tenant RAG covers retrieval over tenant-segmented documents. Multi-tenant memory adds derived state (entities, conflict resolution, freshness, summarization) and longitudinal data on top, which makes the deletion, freshness, and permission problems harder. The vector-database patterns (namespace-per-tenant) carry over; the surrounding state machine does not.


## Key takeaways


-


Memory at 10,000 users is qualitatively different from memory at 100. Tenant boundary, query distribution, and cost compound at once.


-


Pick isolation per tenant tier, not per system. Silo your regulated customers, pool the rest, and use the bridge model where the math works out.


-


Namespace-per-tenant is the recommended pattern in production vector databases. Metadata-only filtering is a fallback with measurable cost penalties.


-


GDPR right to erasure applies to derived memories and embeddings, not just source documents. Designing the deletion cascade is a day-one decision.


-


SOC 2 Type II is the audit framework enterprise procurement actually asks for. Security is the mandatory category, and the audit window is 6-12 months of operating effectiveness.


-


Audit logging is a regulatory obligation under GDPR Article 30, not a feature. Logs need to be tamper-evident and retained for the period the auditor signs off on.


-


Build per-tenant cost metering before finance asks the first hard question, not after.


## Closing


Every dimension covered here is a system the buyer has to operate forever, not a feature they have to ship once. That is the build-vs-adopt question reduced to its honest form.


A memory layer that already operates these surfaces is the difference between a six-month enterprise rollout and a six-week one. At Hyperspell, we built the memory infrastructure so engineering teams could spend their time on agent logic instead of multi-tenant retrieval. If you are sizing up the same set of trade-offs, the architecture is documented, and the permission-aware retrieval companion piece goes one layer deeper into the enforcement model.


## Sources


-


AWS. "SaaS Tenant Isolation Strategies." https://docs.aws.amazon.com/whitepapers/latest/saas-tenant-isolation-strategies/saas-tenant-isolation-strategies.html


-


AWS Machine Learning Blog. "Multi-tenant RAG with Amazon Bedrock Knowledge Bases." https://aws.amazon.com/blogs/machine-learning/multi-tenant-rag-with-amazon-bedrock-knowledge-bases/


-


Microsoft Azure Architecture Center. "Architectural Approaches for AI and Machine Learning in Multitenant Solutions." https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/approaches/ai-machine-learning


-


Pinecone Documentation. "Implement multitenancy using namespaces." https://docs.pinecone.io/guides/index-data/implement-multitenancy


-


European Union. "GDPR Article 17: Right to erasure." https://gdpr-info.eu/art-17-gdpr/


-


European Union. "GDPR Article 30: Records of processing activities." https://gdpr-info.eu/art-30-gdpr/


-


European Union. "GDPR Article 44: General principle for transfers." https://gdpr-info.eu/art-44-gdpr/


-


AICPA. "SOC 2: SOC for Service Organizations, Trust Services Criteria." https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2


-


AuthZed. "Introducing Authorization Infrastructure for AI." https://authzed.com/blog/authzed-announces-support-for-ai-by-providing-permissions-aware-ai


-


OpenAI. "API Pricing." https://openai.com/api/pricing/


-


Hyperspell. "Custom Metadata." https://docs.hyperspell.com/usage/metadata


-


Hyperspell. "Security." https://docs.hyperspell.com/concepts/security
