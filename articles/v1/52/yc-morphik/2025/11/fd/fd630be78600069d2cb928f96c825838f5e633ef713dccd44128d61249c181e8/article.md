---
schema_version: "1.0.0"
document_id: "fd630be78600069d2cb928f96c825838f5e633ef713dccd44128d61249c181e8"
company_key: "yc-morphik"
company: "Morphik"
source_id: "yc-morphik-news-import-d199aadc49bb"
canonical_url: "https://dev.morphik.ai/blog/why-we-use-mongodb-in-production"
published_at: "2025-11-20T00:00:00+00:00"
first_seen_at: "2026-07-22T04:54:38.041510+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:8afc1fd2956e07a63cb2267bd9fc791e9a47cb5c128dfeca02fd147756a70dfa"
---

# Why We Use MongoDB in Production at Morphik

Morphik builds accurate retrieval over complex documents—diagrams, CAD, schematics, dense PDFs—by leaning hard into vision and layout understanding. MongoDB is the storage and persistence layer that makes that feel like a simple, organized “file system” for our customers instead of a pile of vectors and blobs behind the scenes.


In practice, many teams use Morphik to power a **“knowledge base” or “documents” feature inside their own product** : their users log in, upload files, organize them, and expect fast, permission-aware search. Morphik provides the retrieval and AI layer; MongoDB gives us a clean way to represent that whole structure.


## Metadata wants to be documents


In Morphik, customers don’t just upload files into a bucket. They create structured knowledge spaces that look a lot like a filesystem:


- **Vaults / workspaces** to separate environments, clients, or projects
- **Folders** to group related documents
- **Documents** with versions, approvals, and change history
- **Pages and chunks** that our models actually retrieve over


Each of those layers carries metadata: tags, owners, versions, languages, permissions, and any customer-specific fields they care about.


All of that is naturally JSON. In MongoDB, that metadata fits directly into documents—` vault` ,` folder` ,` document` ,` page` ,` chunk` —with no awkward mapping, no complex joins, and no custom query DSL. A “document” in Morphik looks and behaves like a document in MongoDB, which makes it easy to evolve our schema as customers add new tags or change their workflows.


We also run Postgres for other workflows, but to get comparable filtering there we built a small operator “compiler” to translate product queries into JSONB expressions. MongoDB ships those operators out of the box, so we spend more time on retrieval quality and UX, and less time on query plumbing.


## Narrowing the search space


Retrieval quality comes from a mix of good modeling (chunking, embeddings, rerankers), good metadata, and sensible constraints on how much data you search over for each query.


For workloads with a lot of data and many concurrent users, **narrowing the search space using metadata and permissions before you touch embeddings** is one of the main levers for latency, cost, and system simplicity.


When someone queries Morphik—“Show me the latest signed revision,” or “Where did we approve this change?”—we usually know which vaults, folders, or document versions they’re allowed to see and which ones are relevant.


MongoDB’s query operators (` $and` ,` $or` ,` $not` ,` $in` , etc.) let us:


- Filter on metadata and permissions **before** vector search
- Enforce per-user and per-group access control at query time
- Route queries to the right subset of a tenant’s knowledge base


We lean on MongoDB’s query planner and indexes instead of re-implementing filtering logic in application code. That keeps the retrieval pipeline easier to reason about and reduces the amount of custom infrastructure we have to own. The engineering time goes into the parts that are specific to Morphik: visual-grounded retrieval, page-level understanding, and agent workflows over procedures, drawings, and spec sheets.


## Tenant isolation without heavy lifting


Morphik is multi-tenant, but most of our customers want **strong isolation** between tenants, and often between environments inside a tenant.


We run a separate database per enterprise tenant. In MongoDB Atlas, provisioning a new customer is essentially a` createDb` away. On top of that, we can subdivide into vaults and workspaces inside that database without giving up the isolation boundary.


That gives us:


- Strong data isolation for each tenant
- Cleaner compliance and audit stories
- The ability to tune performance per customer as they grow (indexes, hardware, backup policies, and more)


MongoDB handles operators, indexing, replication, and isolation, so we don’t have to build and maintain that stack ourselves. That’s the real reason we use it in production: it keeps the storage layer boring while we iterate quickly on retrieval and agent behavior.


## Read more from MongoDB


MongoDB wrote up a case study on how we’re using Morphik + MongoDB together. You can read it here:[Enterprise-Level, Scalable AI with Morphik and MongoDB](https://www.mongodb.com/company/blog/innovation/enterprise-level-scalable-ai-morphik-mongodb) .
