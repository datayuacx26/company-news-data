---
schema_version: "1.0.0"
document_id: "191e4d9822b1d0519a7bf7a88fa822a3c986fa194ea9467317c6223914b1b1e6"
company_key: "palantir-technologies-inc-class-a-common-stock"
company: "Palantir Technologies Inc."
source_id: "palantir-technologies-inc-class-a-common-stock-rss-f87a89a6619a"
canonical_url: "https://blog.palantir.com/managing-elasticsearch-reindex-at-scale-performance-reliability-and-observability-cf948d0efd47"
published_at: "2026-06-08T17:57:01+00:00"
first_seen_at: "2026-07-20T03:31:10.454667+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:8063663eab900d6151d3d0f5b73c1e095b1e7cd253d377e99d3a0ab9eb42bb11"
---

# Managing Elasticsearch Reindex at Scale: Performance, Reliability, and Observability

# **Managing Elasticsearch Reindex at Scale: Performance, Reliability, and Observability**


[Palantir](https://palantir.medium.com/?source=post_page---byline--cf948d0efd47---------------------------------------)


14 min read


·


Jun 8, 2026


--


Press enter or click to view image in full size


*Editor’s Note: This is the fourth post in a*[series](https://blog.palantir.com/all?topic=palantir-foundations) *exploring how Palantir customizes infrastructure software for reliable operation at scale.*


*The following is a guest contribution to the Foundations series from the Gotham Core Platform organization, which builds and maintains the bedrock for mission-critical applications within the Gotham ecosystem. This blog post by Kevin Liang, a backend developer based in CA, highlights the design considerations and improvements made to the Elasticsearch reindex machinery — which broadly aims to provide an easy-to-use, performant, reliable, and observable way to repair and rebuild search indices for backend applications. The goal of this post is two-fold: share our design decisions and learnings with the broader technical community, and shed light on what a typical project looks like for a systems-leaning backend engineer working in Gotham Core Platform at Palantir.*


Search is one of the most heavily used workflows across Palantir Gotham. Users are constantly querying, filtering, and drilling into large datasets to surface entities, trace connections, and assemble the analyses that drive mission-critical operational decisions. At any point in that workflow, they expect results to feel instantaneous, regardless of the scale or complexity of the underlying data graph.


Many of these search workflows leverage[Elasticsearch](https://www.elastic.co/docs) , an open-source distributed search and analytics engine that provides robust full-text search capabilities at scale. We have previously explained the role Elasticsearch plays within our broader ecosystem and architecture, which you can read more about in[Defensive Databases: Optimizing Index-Refresh Semantics](https://blog.palantir.com/defensive-databases-optimizing-index-refresh-semantics-554ef3852ff1) .


As mentioned in that blog post, most Gotham applications interact with Elasticsearch via an in-house *database client service* that acts as a layer between backend services and storage infrastructure. At a high level, the service resembles a document store and provides:


- Strongly consistent key-value CRUD backed by a transactional database, which remains the authoritative source of truth
- Full-text search on document fields via Elasticsearch, which functions as an “external”, secondary index
- First-class security policy enforcement built into the core read, search and write paths, not just bolted on as an afterthought


We will henceforth refer to this in-house database client service as “the document store.” The document store serves many distinct clients, each with its own group of **schemas** (analogous to database tables). Each **schema** declares field names, types, security, the structural layouts of documents and, critically, whether and how the field should be indexed into Elasticsearch via something called mappings. Some schemas are not indexed at all; others carry rich mappings that power faceted search, geo queries, and date-range filters. You can read more about Elasticsearch mappings in their[official documentation here](https://www.elastic.co/docs/manage-data/data-store/mapping) .


The flow of data from various application backends to the database, Elasticsearch, and the authentication service, is illustrated in the following diagram, expanding upon the diagram we used in the[Defensive Databases](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*QGaK8n6GOrxEXccLePvmYg.jpeg) blog post.


Press enter or click to view image in full size


While Palantir’s *Foundations* group is responsible for operating and delivering Elasticsearch clusters across our diverse fleet of deployments, the services that interact with this infrastructure also bear the responsibility of using the clusters efficiently and responsibly, and maintaining their own indices.


There are also times when a service needs to repair or rebuild Elasticsearch indices. When an index becomes outdated or falls into an inconsistent state, whether from mapping changes or operational failures, a process known as reindexing is often the remediation.


- For example, a logistics application that needs to add a new field for users to search supply chain disruption data, or that needs to support an additional security metadata field in the access policy, would need a reindex.
- Reindexing can also apply at the cluster level. For example, when migrating from Elasticsearch 8.x to 9.x, we prefer reindexing over an in-place upgrade because it allows us to roll back quickly if something goes wrong.


This post discusses the design and evolution of the Elasticsearch reindex machinery embedded in the document store, which has enabled us to carry out reindex reliably, performantly, and without taking the service offline or requiring excessive human oversight, across any number of client applications.


## What do we need


As previously mentioned, reindex can be necessary for multiple reasons. Schemas and their mappings evolve as our products do, index settings (including the number of shards) may change, and indices may become corrupted due to unexpected failures or bugs in the software. When they happen, a new index with the correct mappings and updated settings must be created, populated with data, and swapped in. Furthermore, when we need to migrate between clusters, such as during version upgrades (e.g. upgrading from Elasticsearch 8.x to 9.x) or to move to a standalone cluster rather than one shared with other services when the throughput justifies it, we would need to carry out a cross-cluster reindex.


Elasticsearch does offer a` _reindex` API, which copies documents from one index to another within the same cluster, and offers the ability to[source the reindex from a remote cluster](https://www.elastic.co/guide/en/elasticsearch/reference/8.19/docs-reindex.html#reindex-from-remote) in the API, too. However, since the database is the primary source of truth with strong consistency in our architecture, the reindex we are conducting will rehydrate the new index with documents loaded from the database instead, which means that our document store service will have to orchestrate and oversee the entire reindex process rather than simply dispatching a command and letting the Elasticsearch cluster handle the rest.


The requirements for our Elasticsearch reindex machine are:


1. **Schema-aware reconstruction** : The system must know how to construct each client schema’s indices — field types, analyzers, shard counts, etc.


2. **Low manual action** : Operators shouldn’t have to shepherd each schema through the process and move it through the stages. Managing reindex across hundreds of deployments, each with 100+ schemas, is simply too much work to do by hand.


3. **Automatically detect and repair inconsistency** : The system should be able to detect when reindex is needed for a specific schema and carry it out autonomously.


4. **Bounded resource usage** : Reindex must not bring down the server or impede API serving. It cannot monopolize memory, exhaust the thread pool, or saturate the database connection pool.


5. **Online, live reindex** : The service is serving live traffic the entire time, including search. In the context of our logistics-focused application, existing search functionalities must continue to work.


## Our first reindex machine


With the requirements in mind, we implemented the first Elasticsearch reindex machine within the document store. The following diagram shows the process to reindex a particular schema.


Press enter or click to view image in full size


## Shadow index


Our example application must continue to handle search requests during and after the reindex. The key to maintaining liveness during reindex is the **shadow index** , a separate, physical index on the same Elasticsearch cluster for the same schema with desired mappings and settings, and receiving all index / deindex requests from the document store’s client-facing API while getting backfilled by the reindex machine. Crucially, the shadow index does not yet serve reads. The reindex machine will create a shadow index, with suffix` _b` if the current index ends in` _a` (and vice-versa), at the inception with the intended mapping and settings, and backfill documents from the database. Upon conclusion, the machine will set the shadow index as the primary index and discard the previous index.


Searches are always served from the primary index, and write requests from the API dual-write to both the primary and shadow index to not lose the latest edits. We take advantage of the[optimistic concurrency control primitive provided by Elasticsearch](https://www.elastic.co/blog/elasticsearch-versioning-support) (` version_type.external_gte` ) to deconflict concurrent index writes from the API and the reindex machine.


As for the routing of read and write requests, instead of directly referencing names of physical indices, we use[Elasticsearch index aliases](https://www.elastic.co/docs/manage-data/data-store/aliases) in` read` ,` write` , and` reindex` code paths. Mappings from aliases to physical indices are only set or altered during index creation and after reindex operations.


The following table illustrates changes in aliases involving the shadow index,` _b` , and the existing primary index,` _a` , at key stages of a reindex. In this example,` _b` was created and tagged with the reindex alias at the onset of a reindex.` _a` kept the read alias while the write alias was dropped. When the backfilling phase of reindex is complete,` _b` becomes primary by assuming the read and write aliases, while` _a` gets deleted.


Press enter or click to view image in full size


## Orchestrating a reindex


The reindex machine maintains a schema-level index metadata table to track information related to reindex job runs, e.g. when a schema last started/ succeeded/ failed, whether and when the schema is queued, running a reindex, aborting one, or has no work. The schema-level index metadata is the single source of truth for coordination within the reindex machine.


Running as a background task on the server, the reindex machine periodically scans all schema definitions, compares the database-persisted metadata against the actual index state on the Elasticsearch cluster, and determines which schemas require attention: continue an ongoing reindex, start the next queued, or attempt to repair an inconsistent schema index before queueing it for a full reindex if a repair was insufficient.


In addition to proactively reindexing for schemas whose indices require repair, a separate background task, which picks up config-supplied requests, and several admin API endpoints allow operators and clients (including our logistics-focused application in the example) to request and abort a reindex.


## Cluster agnostic


A key design choice across the document store service is being cluster-agnostic. Elasticsearch clusters are discovered and mapped to logical components in code. In most cases there will be one cluster, but during a cross-cluster reindex, there will be an additional migration cluster attached and recognized as such. Via a dynamic layer, reads are served from the primary cluster only while writes — reindex included — fan out to all connected clusters, supporting both single-cluster and cross-cluster operations with minimal additional code.


## Get Palantir’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


With this basic design, we are able to perform reindex on any schemas within the document store. Our logistics-focused example application can include a reindex in their schema upgrade and gate the new search feature on reindex completion.


## Evolution of the reindex machine


As our products have expanded in both features and usage, and our storage infrastructure has evolved, the reindex machine has faced increasing demands. Running a full rebuild of an Elasticsearch index with millions of documents — while the document store service continues to handle live traffic — presents a constantly evolving challenge. As we add new features and improve stability, reindexing has become more frequent and critical. In response, we have continually enhanced the reindex machine to improve performance, stability, and observability. Here, we highlight a few notable improvements.


## Parallelization and asynchronous processing


Parallelization was built into the reindex machine early on, since both reading pages of documents from the database and indexing them into a search engine involve significant I/O wait. The solution is a parallel job runner backed by a configurable thread pool and a bounded staging queue. Worker threads dynamically assume one of two roles on each iteration: a thread that acquires the paging lock fetches the next batch of records into the queue, while threads that find the lock held instead claim a queued batch and perform the indexing work. A separate daemon thread periodically snapshots progress by recording the earliest unfinished batch’s position, enabling the job to resume from where it left off after a failure. This design creates a pipeline where multiple batches are in flight simultaneously — some being read from the database, others being indexed — without requiring explicit role assignment or blocking the reindex loop.


The diagram below illustrates the design of the parallel job runner.


Press enter or click to view image in full size


## Batch limiting


To prevent toppling the document store due to reindex, we have added a multitude of rate and batch limiting capabilities around the reindex machine. A single reindex run is bounded along three dimensions:


- Document count per batch: Caps the number of objects fetched from the database in a single transaction, keeping database connection hold times short.
- Memory per batch: A soft cap on the memory footprint of a single batch, preventing surprise memory surge caused by large documents due to a rigid batch size limit.
- Total batches per run: Limits how many batches a single invocation of the reindex loop will process before yielding. Combined with a configurable rest interval between runs, this creates natural breathing room for API-serving threads and garbage collection.


These three knobs interact to prevent any single dimension from becoming a bottleneck. A schema with many small documents hits the document-count limit; a schema with fewer but larger documents hits the memory limit; and the batches-per-run limit ensures the reindex machine periodically pauses regardless.


## Crash safety


Imagine if the document store server were to crash mid-way through an operation that modifies indices and aliases — say, while starting or finishing a reindex for our logistics application. The indices could be left in a broken state from which deterministic recovery is impossible without restarting the entire reindex from scratch. To guard against this, each index lifecycle operation is modeled as a fine-grained state machine: before dispatching any mutation to the search engine, the current state is persisted to the database, and every search engine operation is designed to be idempotent. If the server crashes at any point, it simply resumes from the last persisted state and safely re-executes the pending operation.


The states, illustrated below, are persisted in a separate per-cluster, per-schema metadata table, where we also persist key parameters such as the current primary index for crash-safety.


Press enter or click to view image in full size


## Logs? Metrics? or Diagnostics? Yes.


When a background process is silently rebuilding indices involving millions of documents, observability is non-negotiable. The diverse and often-times restricted environments in which Palantir products are deployed further means that viability and access to the different forms of observability vary greatly between deployments. You cannot backport the observability you wish you had when things go wrong. Not to mention that different forms of observability have their pros and cons, and we would like the best of all worlds.


### Logs:


- Pros: shows timestamped state snapshots with rich parameters, great for tracing exact steps and examining detailed states at each step.
- Cons: may be too spammy and occupy excessive disk space if parameters are too informative; while logs are helpful for investigation after the fact and we would want exhaustive logs during a root-cause analysis, most of the time logs and their parameters are simply not consumed.


### Metrics:


- Pros: an ideal, light-weight tool to track changes in states and progress over time, and can be turned into user-friendly dashboard visualizations to aid quick insight gathering and aggregation analyses.
- Cons: do not contain additional information beyond the time series and tags themselves, i.e. no additional parameters.


**Diagnostics** (on-demand, one-off reports generated upon request, often consumed on[Palantir Apollo](https://www.palantir.com/platforms/apollo/) ):


- Pros: can be as detailed as we need them to be (within reason, of course), without having to worry about spamming or disk space given that they are on-demand.
- Cons: they are often a snapshot (unless specifically engineered to recount historical events) when they are requested, making them less useful for an after-the-fact analysis


In our case, we have opted to implement all three forms of observability available to us, which has proven to be immensely valuable.


## Handling cross-cluster reindex


Recall from our earlier examples that a reindex can be triggered for reasons beyond application-level changes. Cross-cluster migrations — moving a service’s search traffic from one Elasticsearch cluster to another without downtime — are a fact of life in a large-scale platform. For example, if we want to migrate from Elasticsearch 8.x to 9.x while preserving the ability to rollback at any point, we would need to implement the migration with a reindex.


In the reindex machine design we have discussed thus far, which aims to repair the primary, request-serving Elasticsearch cluster, we create a new index as the shadow index on the same cluster that is then rebuilt from the primary store. When backfilling is complete, the shadow index is promoted for searching. While conceptually similar to a regular reindex, cross-cluster reindex does require slightly different handling. Specifically, we need to make sure that the new index is located on a different cluster than the cluster that hosts the old index, and skip the deletion of old indices upon reindex completion as there is nothing to delete on the new cluster, and we want to keep the indices on the old cluster around in case we need to roll back.


## A separate reindex machine for migration


We decided to keep the two reindex machines separate, each orchestrating reindex based on metadata tied to the appropriate Elasticsearch cluster in question. The **primary reindex machine** , which we have covered in this article thus far, works with the primary cluster and serves to repair active indices that handle API requests. The new **migration reindex machine** , on the other hand, backfills the migration cluster and prepares it to become the new primary cluster. Creating two separate reindex machines makes configuration, metadata management, code readability, and observability easier, compared to a single reindex machine filled with cluster-specific branching logic. Each reindex machine will have its own configuration: thread counts, batch sizes, memory limits, schedule interval, etc., and run as independent background tasks, working with only their respective cluster rather than all connected clusters.


Each reindex machine applies different index and alias evolutions tailored to their respective use case:


### Index and alias evolution on the primary reindex machine (same as before)


Press enter or click to view image in full size


### Index and alias evolution on the migration reindex machine


Press enter or click to view image in full size


The following diagram shows a comparison of active indices on relevant clusters when reindexing for the primary cluster, and when conducting a cross-cluster reindex for migration purposes.


Press enter or click to view image in full size


## Rome wasn’t built in a day


Over the years, many Palantir engineers have worked to support a first-class search experience by contributing a myriad of improvements across the reindex machinery in Gotham’s document store service. Ongoing learnings from both the engineering and operational sides continue to drive improvements across the Elasticsearch reindex apparatus. A lot of the same learnings, pertaining to observability and increasingly proactive support modalities, will inform the design of future pieces of software — across Foundry, AIP, Gotham, and Apollo development efforts — that interact with Palantir’s infrastructure layers. No product ever reaches perfection, but at Palantir, we strive to continuously learn from every challenge and operational outcome, using those insights as fuel to refine our products further. Our commitment is not just to deliver powerful capabilities, but also to ensure the highest levels of stability and reliability for our users.


If this sounds like the kind of project and impact you’re interested in, check out our open roles today:[https://www.palantir.com/careers/open-positions/](https://www.palantir.com/careers/open-positions/)
