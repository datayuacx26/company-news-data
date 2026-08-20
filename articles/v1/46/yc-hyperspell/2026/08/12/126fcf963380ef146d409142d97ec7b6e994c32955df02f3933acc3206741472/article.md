---
schema_version: "1.0.0"
document_id: "126fcf963380ef146d409142d97ec7b6e994c32955df02f3933acc3206741472"
company_key: "yc-hyperspell"
company: "Hyperspell"
source_id: "yc-hyperspell-news-import-13fe3511aa44"
canonical_url: "https://www.hyperspell.com/blog/memory-schema-design"
published_at: null
first_seen_at: "2026-08-09T23:13:54.426753+00:00"
fetched_at: "2026-08-09T23:13:55.393599+00:00"
content_hash: "sha256:1745ae24129725d597cb1966ab5d28392ec5e6c553fe6c0413171151dbaadab2"
---

# Agent Memory Schema Design: Key Data Modeling Decisions Explained

The first memory system everyone ships looks the same. A memories table. An id, a content column, an embedding column for the vector, maybe a user_id if you're being thorough. You wire up similarity search, demo it to the team, and ship.


It works. Until the third real question hits.


"What did the agent know last Tuesday?" wants timestamps you didn't store. "Show me only the memories from Slack" wants source attribution that lives nowhere. "Two users in the same workspace, but one of them shouldn't see this memory" wants a permission model you don't have. None of those are vector problems. All of them are schema problems.


We've shipped Hyperspell across more than 50 sources of workspace data, and nearly every interesting failure we've watched teams hit has been a schema failure rather than a retrieval one. This article is the field-by-field guide we wish someone had handed us: a working mental model for every column, index, and isolation boundary a production memory layer needs, plus a sense of which decisions you can defer and which you cannot.


## Why is memory a schema problem, not a vector problem?


The seductive logic of v0 is that memory equals "text plus embedding plus similarity search." Vector quality is real, and it's the easy part. The actual production workload is filtering, sorting, aggregation, tenant isolation, permission checks, point-in-time queries, and retention. Similarity alone serves none of those.


What makes the schema layer hard is that the field hasn't agreed on its own vocabulary. A recent large-scale survey of agent memory finds substantial fragmentation across implementations and evaluation protocols, with loosely defined terminology obscuring shared concepts ([Hu et al., "Memory in the Age of AI Agents"](https://arxiv.org/abs/2512.13564) ). Each team rediscovers the same fields. The schema is where convention has to live, because the words don't yet.


Vector similarity is one query path. A production memory system answers four or five other questions in the same request, and the schema is what makes that possible.


## The four memory types and the schema each one demands


The closest thing to an industry-default taxonomy comes from the CoALA framework: working, semantic, episodic, and procedural memory ([Sumers et al., "Cognitive Architectures for Language Agents"](https://arxiv.org/abs/2309.02427) ). Most production systems we see implement at least three of the four, and each has different schema requirements.


Working memory is ephemeral and session-scoped. It often never hits the database, just an in-memory cache or a context buffer. Keep it out of the long-term memory table; mixing the two creates noise on every query.


Semantic memory holds facts and preferences. It needs entity references, deduplication, and conflict resolution. Put entities in their own table and let memories foreign-key into them.


Episodic memory is a log of timestamped events. Store both the source timestamp (when it happened) and the ingestion timestamp (when you saw it). They are not the same.


Procedural memory holds workflows, configs, and learned behaviors. It is usually a JSON document, not a flat row. Treat it that way from the start.


Memory type


Primary access pattern


Indexing priority


Typical storage


Working


Read-heavy within a session


None (in-memory)


Cache or context buffer


Semantic


Similarity + entity filter


Vector + entity FK


Vector store + relational


Episodic


Range over time + filter


Time + tenant composite


Time-partitioned table


Procedural


Lookup by agent or user


Lookup key


JSON document column


When you're confused about where a piece of state belongs, naming the memory type usually answers it.


## Which fields does every memory need?


Below are the columns we've seen earn their place in every production schema, regardless of vendor. Think of them as table stakes.


### Content and embedding


You need the raw text and the vector together. The trap is making embedding non-nullable too early, before you've decided which model produces it. Embeddings are tightly coupled to a model version, and you will change models.


Add an embedding_model column from row one. Re-embedding is the most common migration nobody plans for, and without that column you cannot tell which rows are stale. pgvector supports HNSW and IVFFlat indexes; HNSW is the common default for read-heavy workloads, with IVFFlat as a faster-to-build alternative that uses less memory ([pgvector README](https://github.com/pgvector/pgvector) ).


### Timestamps (you need at least three)


A single created_at column will burn you. Production memory systems need three distinct time concepts, and once you conflate them, you cannot recover the distinctions in code.


The source_timestamp records when the underlying event happened in the world. The email was sent at 2:14pm. The Slack message was posted yesterday. This is the time the user thinks about.


The ingested_at timestamp records when your pipeline saw it. This is the time your operations team thinks about, and it's how you debug freshness.


The valid_at and invalid_at pair tracks when the fact was actually true. Zep's Graphiti research formalizes this bi-temporal pattern: every edge in the graph carries both transaction-time (when the system learned it) and validity-time (when it held in the world) intervals ([Rasmussen et al., "Zep: A Temporal Knowledge Graph Architecture"](https://arxiv.org/abs/2501.13956) ).


Why all three: queries like "what did the agent believe last Tuesday?" need transaction-time, not validity-time. Mixing them gives confidently wrong answers, and confidently wrong is worse than no answer at all.


### Source attribution


Add source_type, source_id, and source_url as first-class columns. Slack, Gmail, Notion, Vault, internal documents: these are values your queries filter on constantly, not metadata you stash in a JSON blob.


Source attribution is also how you debug retrieval, attribute claims in agent answers, and let users selectively prune their memory. Putting it in metadata makes every one of those queries slow.


### Tenant and user identifiers


Add tenant_id and user_id as first-class columns from row one, even if you're single-tenant today. Adding tenant scoping after the fact is one of the most expensive migrations in this space, because every existing query has to be re-audited for leakage.


Index tenant_id first in every composite index. The[AWS Database Blog's pattern](https://aws.amazon.com/blogs/database/multi-tenant-data-isolation-with-postgresql-row-level-security/) for PostgreSQL row-level security is a useful default for shared-database isolation.


### Permission metadata


Pick one model (ACL list, role list, or visibility scope) and enforce it at the storage layer, not the application layer. Application-layer permission checks are one missed import away from a leak.


The pattern we use is to inherit permissions from the underlying source. A Slack message indexed for one user does not become readable by another user, even inside the same tenant, because the source's permissions travel with the memory ([Hyperspell docs](https://docs.hyperspell.com/usage/permissions) ).


## Optional fields that earn their place fast


Beyond the core, four optional fields appear often enough in mature systems that we recommend designing around them from day one, even if you don't populate them yet.


Entity references, a mentions table joining memories to people, projects, and concepts, unlock relationship queries that pure vector search cannot answer.


Memory type or classification: tag rows as fact, event, preference, or summary. This lets you apply different retention and retrieval rules per type, instead of one blunt policy for everything.


Confidence score: extraction confidence on a 0-to-1 scale. Cheap to add, expensive to backfill, and reliably wished-for once you start tuning retrieval.


Version pointer: when a fact is corrected, do not delete the old row. Mark it invalidated and link the new row to it. This is how bi-temporal systems support point-in-time queries (Rasmussen et al., 2025).


## Schema evolution: the migrations you will run


Every memory schema we've watched in production has run at least three migrations within the first year. Plan for them.


Adding a field looks easy and often isn't. The honest pattern is "tombstone": leave the new column nullable, write a backfill in the background, and ship a query layer that handles both states from day one. Plan the backfill window before the field ships, not after.


Changing a field type is harder, especially the embedding dimension when you upgrade models. The dual-write pattern is the safest path: write to old and new columns for a window, switch reads when the new column is fully populated, then drop the old.


Removing a field requires a deprecation period with logging on read. The fastest way to break a production agent is to drop a column the retrieval code still references. Logging unexpected accesses for a week before the drop catches almost every reference.


Treat your memory schema like a public API. Every column you add becomes a query someone depends on within a week, and removing it is harder than adding it ever was.


## Multi-tenancy and isolation: the decision that doesn't bend


Three patterns are viable for multi-tenant memory, and the choice is structural, not cosmetic.


Namespace per tenant is the default in most vector-only databases. It's the weakest of the three and the one that fails regulated audits when stress-tested. Namespace separation is a logical convention, not a security boundary.


Schema per tenant is Postgres-native. Each tenant gets its own schema with its own tables, indexes, and permissions. It scales operationally to dozens of tenants but starts to hurt at hundreds.


Row-level security in a shared schema is the strongest pattern, at the cost of a more complex query path. It's database-enforced isolation that survives bugs in your application code ([AWS Database Blog](https://aws.amazon.com/blogs/database/multi-tenant-data-isolation-with-postgresql-row-level-security/) ).


For agent memory specifically, regulatory pressure pushes toward row-level isolation paired with an audit table. Record-keeping obligations for high-risk systems under the EU AI Act make the audit path a structural requirement rather than a feature ([Regulation (EU) 2024/1689](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) ).


## Which indexes are worth the write tax?


Indexes are how queries you run thousands of times a day stay fast, and they are also a write tax you pay on every insert. Pick the four or five queries that dominate your workload and index for those.


A vector index for similarity is non-negotiable. HNSW is the common default for read-heavy workloads; IVFFlat is faster to build and uses less memory but offers lower recall at the same probe count ([pgvector README](https://github.com/pgvector/pgvector) ).


B-tree indexes go on tenant_id, user_id, source_type, and your primary time fields. Composite indexes are where the real wins live. (tenant_id, source_timestamp DESC) covers the most common access pattern in agent memory: "recent memories for this tenant."


` CREATE INDEX memories_tenant_time_idx ON memories (tenant_id, source_timestamp DESC); CREATE INDEX memories_embedding_hnsw_idx ON memories USING hnsw (embedding vector_cosine_ops); CREATE INDEX memories_source_idx ON memories (tenant_id, source_type, ingested_at DESC);`


A short audit before adding any new index: which query does it serve, how often does it run, and what does it cost on writes. If you can't answer all three, you don't need the index yet.


## Normalization vs denormalization


The right answer is almost always partial denormalization. Pure normalization buys consistency at the cost of joins on every read; pure denormalization buys read speed at the cost of update fan-out.


Approach


Pros


Cons


When to use


Normalized


Consistent updates, smaller rows


Joins on every read, slower at scale


Slow-changing reference data (sources, entities, users)


Denormalized


Fast reads, simpler queries


Update fan-out, drift risk


Read-hot fields on the memory row


Hybrid


Best of both


More schema discipline required


Production memory systems


The pattern that survives contact with reality: normalize the slow-changing reference data, denormalize the read-hot fields onto the memory row. When a source's display name changes, a single update to the source table propagates. When a query filters on source_type, no join is required.


## Compliance fields you will regret skipping


Three columns and one extra table will save you a quarter of pain when compliance shows up.


A soft-delete column (deleted_at) with a tombstone pattern, not hard deletes. Soft-deletes preserve audit history and let you recover from buggy delete code. Pair them with a hard-delete schedule that runs on demand for erasure requests, since[GDPR Article 17](https://gdpr-info.eu/art-17-gdpr/) requires deletion of personal data without undue delay.


An audit table that records who accessed which memory, when, and from which agent. Record-keeping is a structural obligation under the EU AI Act for high-risk systems, and structuring it on day one is far cheaper than retrofitting.


A key version column on encrypted columns. Cheap to add now, vital during a key rotation later. Without it, rotating keys means re-encrypting every row blind.


Hyperspell keeps these concerns at the storage layer rather than the application layer. Soft deletes, source-permission inheritance, and agent traces are all addressable through the documented APIs ([traces docs](https://docs.hyperspell.com/api-reference/traces/add-an-agent-trace) ).


## A working schema you can copy


The Postgres + pgvector schema below collects every recommendation in this article into one runnable example. It assumes a single shared database with row-level security and pgvector installed.


` CREATE EXTENSION IF NOT EXISTS vector; CREATE TABLE sources ( id BIGSERIAL PRIMARY KEY, source_type TEXT NOT NULL, -- slack, gmail, notion, vault external_id TEXT NOT NULL, display_name TEXT, UNIQUE (source_type, external_id) ); CREATE TABLE memories ( id BIGSERIAL PRIMARY KEY, tenant_id UUID NOT NULL, user_id UUID NOT NULL, source_id BIGINT REFERENCES sources(id), source_type TEXT NOT NULL, -- denormalized for fast filter source_url TEXT, content TEXT NOT NULL, embedding VECTOR(1536), embedding_model TEXT NOT NULL, -- never null, never assume memory_type TEXT NOT NULL, -- fact, event, preference, summary confidence REAL, source_timestamp TIMESTAMPTZ NOT NULL, -- when it happened ingested_at TIMESTAMPTZ NOT NULL DEFAULT NOW(), valid_at TIMESTAMPTZ, invalid_at TIMESTAMPTZ, supersedes_id BIGINT REFERENCES memories(id), permission_scope TEXT NOT NULL, -- inherited from source deleted_at TIMESTAMPTZ, key_version INT NOT NULL DEFAULT 1 ); CREATE INDEX memories_tenant_time_idx ON memories (tenant_id, source_timestamp DESC) WHERE deleted_at IS NULL; CREATE INDEX memories_embedding_hnsw_idx ON memories USING hnsw (embedding vector_cosine_ops) WHERE deleted_at IS NULL; ALTER TABLE memories ENABLE ROW LEVEL SECURITY;`


Tenant first in every index. Embedding model never null. Three time concepts kept separate. Soft-delete with index predicates that exclude deleted rows. Row-level security enabled at the table level. None of these are exotic. All of them are easier to add now than later.


## When should you stop building your own?


The buy-vs-build cliff is rarely the v0 schema. It's the third migration. The day you re-embed every row because the model changed. The day you wire permission inheritance across 50+ different sources, each with its own auth model. The day a customer asks for an audit trail that interleaves correctly with deletion.


A managed memory layer is the right answer when memory is not your differentiation. If it is, build, but build with this schema. If it isn't, the leverage is enormous.


What Hyperspell handles in this space is the multi-source schema underneath: typed memories, source attribution, custom metadata you can filter on, permission inheritance from the underlying source, and conflict resolution across overlapping facts ([metadata docs](https://docs.hyperspell.com/usage/metadata) ,[core concepts](https://docs.hyperspell.com/core/concepts) ). The schema work is the same. The difference is whether your team owns the migrations.


## Frequently asked questions


### Do I really need bi-temporal timestamps?


Yes, if your data ever gets corrected, deleted, or superseded, which is every real production system. Bi-temporal models track when a fact was true in the world and when your system learned it, separately. The Zep / Graphiti paper is the canonical reference (Rasmussen et al., 2025).


### Should I use a vector database or Postgres + pgvector?


For most workloads below the tens of millions of memories, Postgres + pgvector is usually the right answer for schema flexibility and operational simplicity. At larger scales, dedicated vector engines start to win on query performance. Pick based on your other queries, not just similarity.


### How do I handle GDPR deletes if my embeddings are derived from the original text?


Treat embeddings as personal data and delete them when you delete the source. GDPR Article 17 requires erasure without undue delay, and embeddings derived from personal data are still personal data. Most teams forget this until an audit.


### Can I add multi-tenancy later?


Technically yes; in practice it is one of the most painful migrations in this space. Every query has to be re-audited for tenant leakage, and a missed query means a security incident. Adding tenant_id as a first-class column from day one is essentially free. Adding it later is not.


## Key takeaways


-


Memory is a schema problem before it is a vector problem. Similarity is one query path among five or six.


-


Every memory needs at least three timestamps: when it happened, when you ingested it, and when it was true in the world.


-


Add tenant_id and embedding_model from row one. Both are expensive migrations later.


-


Bi-temporal models, tracking transaction-time and validity-time separately, are the canonical pattern for memory that survives correction (Rasmussen et al., 2025).


-


Namespace isolation is not row-level security. Regulated workloads need the latter.


-


Treat your memory schema like a public API.


## Sources


-


European Union. "Regulation (EU) 2024/1689 (AI Act)." 2024. https://eur-lex.europa.eu/eli/reg/2024/1689/oj


-


European Union. "GDPR Article 17: Right to erasure." https://gdpr-info.eu/art-17-gdpr/


-


Sumers et al. "Cognitive Architectures for Language Agents (CoALA)." 2023. https://arxiv.org/abs/2309.02427


-


Rasmussen et al. "Zep: A Temporal Knowledge Graph Architecture for Agent Memory." 2025. https://arxiv.org/abs/2501.13956


-


Hu et al. "Memory in the Age of AI Agents." 2025. https://arxiv.org/abs/2512.13564


-


pgvector. "Open-source vector similarity search for Postgres." https://github.com/pgvector/pgvector


-


AWS Database Blog. "Multi-tenant data isolation with PostgreSQL Row Level Security." https://aws.amazon.com/blogs/database/multi-tenant-data-isolation-with-postgresql-row-level-security/


-


Hyperspell Documentation. https://docs.hyperspell.com
