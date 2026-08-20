---
schema_version: "1.0.0"
document_id: "6efb28e324d5a4426cebe06c8f852799f9aa120a70cc76787f7607905836387c"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/why-large-organizations-need-materialize"
published_at: "2026-07-20T07:00:00+00:00"
first_seen_at: "2026-07-22T14:45:41.190403+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:617fb6359449acbf93617a17fb9905cf3b603dc092de11c3e360701b6893e046"
---

# Why Large Organizations Need Materialize

Authorization systems are usually evaluated based on how efficiently they answer "can this user perform this action?" This access pattern is table stakes for any authorization service. But when the product evolves, different kinds of authorization questions emerge.


Some applications need to answer questions like "what resources can this user access, ordered by relevance?" or "which resources matching this search query can they view?"


Others face a different kind of challenge: deeply nested groups and tangled business graphs, where answering "can this user perform this action?" means traversing the authorization graph level by level.


Search, analytics, entitlement management, and AI retrieval increasingly need something different: not a one-time query, but continuous access to large, constantly updated sets of denormalized permissions.


That's why we built Materialize.


Materialize keeps computed permissions in sync with your SpiceDB permission graph. You configure the permissions you want it to track, and it incrementally updates them as relationships or schema definitions change.


Materialize is currently available to select AuthZed Dedicated customers through our Early Access program.


To get early access to Materialize,[schedule a call](https://authzed.com/call) with us.


Materialize enables organizations to:


- Apply authorization filtering in secondary indexes such as Elasticsearch and OpenSearch without repeatedly traversing large permission graphs.
- Build authorization-aware search, discovery, and navigation over billions of resources powering the search, analytics, and AI retrieval workloads that need to serve every resource a user can access.
- Accelerate SpiceDB permission checks and lookups to single-digit millisecond latency, even for the query patterns most expensive to traverse at request time.


Since announcing Materialize in early access, we've worked closely with customers operating large-scale authorization deployments. One Fortune 500 customer shared their experience:


> "We have an exceptionally complex authorization model, vast amounts of data to crunch, and a monolith that's in the process of being disaggregated. We evaluated the leading tools in the industry and determined that AuthZed's SpiceDB and Materialize products are the only solution that could handle our complexity. By leveraging Materialize's ability to co-locate computed permissions and keep them in sync, we've been able to fully support major product launches and critical reporting capabilities that weren't otherwise possible."


One challenge of denormalizing permissions is that SpiceDB permissions are derived from the relationship graph rather than stored as explicit access lists. A single relationship write can therefore affect permissions across large portions of the graph, particularly when nested groups are involved. Each time the graph changes, naively recomputing the affected permission sets consumes significant compute and delays the propagation of authorization changes.


Materialize addresses this by subscribing to SpiceDB's change stream and computing only the affected permission deltas as relationships change, rather than rebuilding entire datasets. Downstream systems consume a continuous stream of computed permissions, staying in sync with the source authorization graph as it evolves.


## Common Materialize Use Cases


### Accelerated Queries


In high-fanout permission models, checking whether a user can access a resource might require evaluating multiple permission paths: direct grants, nested groups, role assignments, and permissions granted further up the hierarchy.


Consider a system where access to a document is granted through a deep hierarchy of organizations, projects, and folders. A single permission check may require traversing multiple levels of the authorization graph before arriving at a decision, with more levels as the hierarchy deepens.


SpiceDB evaluates


document:keys.env#view@user:alice


Resolution trace


Run a check to trace how the decision is resolved.


Cost


Run a check to measure how much of the graph gets walked.


Subject


Organization


Project


Folder


Document


Resolution path


This cost is incurred on every request: SpiceDB's caching absorbs much of the repeated subcomputation, but across hundreds or thousands of distinct resource checks per request, each with its own paths and cache entries continually invalidated as relationships change, the aggregate still amounts to substantial datastore work.


Materialize evaluates these permission paths once and maintains the results incrementally as relationships change, instead of re-evaluating them on every request. Placing it in the path of expensive queries lowers latency, cuts compute, and makes performance far more predictable under demanding authorization workloads.


### **Search**


Search systems often need to return only the resources a user is allowed to see, but doing so presents a performance challenge at scale.


The pre-filter strategy queries the authorization service for all accessible resources, then adds those IDs as a search condition. This works for small permission sets, but becomes unmanageable when users can access tens of thousands or millions of resources: a search index can't efficiently evaluate a condition containing that many individual IDs.


The post-filter strategy overfetches search results and then checks each one for access. This is slow when many results match the query, and, more critically, it breaks pagination: you can't reliably show "page 2" until you know which results will be filtered out.


Either way, the search experience is capped by what the authorization system can serve efficiently, and at large scale, it tends to break down.


Materialize collocates computed permissions with the search index, so an access filter becomes an ordinary condition on the index itself - rather than a round-trip to a separate authorization service, or a query condition stuffed with thousands of resource IDs. Permission-aware search reduces to a single index query, with predictable query-time cost regardless of how many resources the user can access.


### **Authorization-Aware User Experiences**


Applications frequently need to present large collections of accessible resources directly in the UI - often filtered by attribute, sorted by a specific criterion, and split across pages. Unlike a search query that narrows to a subset, these views enumerate and order the entire set of resources a user can access.


This is the same pre-filter/post-filter problem as search, in a different place. To show a user their filtered, sorted, and paginated resources, you have two options, and both break at scale:


-


**Pre-filtering:** Ask the authorization service for all accessible resource IDs, then filter your database query to only those IDs. This collapses when users can access tens of thousands of resources because the query becomes too large to execute efficiently.


-


**Post-filtering:** Fetch a page of candidates from your database, then check each one for access. This forces multiple authorization checks per request and breaks pagination. You can't reliably show "page 2" when you don't know which results will be filtered out.


Materialize solves this by maintaining the accessible set as it changes and syncing it into your own database, alongside your business objects. Serving a sorted, paginated view becomes a simple JOIN against local data.


### **Analytics and Data Platforms**


Modern analytics and business intelligence platforms excel at aggregating and analyzing large datasets - scanning billions of rows, joining across tables, and computing complex aggregations in seconds. Teams rely on them to answer critical business questions about usage patterns, resource allocation, compliance posture, and operational efficiency.


Many of these questions need to respect authorization boundaries. A compliance report must accurately reflect who can view sensitive data; a usage dashboard shouldn't surface resources a team can't access. Answering them means analyzing the permissions of billions of resources alongside the data itself.


The problem is that permissions don't live in the warehouse. Checking them against SpiceDB mid-query would mean an external lookup for every resource in a scan over billions of rows - something no warehouse can do efficiently. Materialize solves this by continuously maintaining permission data that you can load directly into your data warehouse, turning authorization checks into standard table joins.


### **RAG Systems**


AI applications introduce a new authorization challenge. Retrieval-Augmented Generation (RAG) works by searching a knowledge base for relevant context, then feeding that context to an LLM to generate a response. But in enterprise environments, a user can't receive answers derived from documents they aren't authorized to see, and it isn't enough to filter the final answer. An unauthorized document that enters the model's context can leak through the synthesized response even if it's never quoted directly, so it must be excluded from retrieval entirely.


Materialize enables permission-aware RAG by maintaining each user's accessible document set, so the vector database can filter retrieval to authorized documents, keeping unauthorized content out of the context the LLM sees. Permission checks become just another condition in the query, applied at retrieval rather than after the model has already read something it shouldn't.


This is especially valuable when permissions change frequently. Each permission change can force an external index to re-tag and re-index documents, which becomes a bottleneck at scale. Materialize's permission-set model keeps permission changes separate from the index itself, so frequent permission changes don't translate into constant re-indexing, an advantage that applies to any authorization-aware external index, not just vector databases.


## Looking Ahead


Authorization has evolved beyond request-time permission checks. Modern applications increasingly depend on authorization-aware search, analytics, retrieval, and user experiences that require access to continuously maintained authorization data.


This trend is only accelerating. As authorization graphs grow and more systems are built on top of them, the questions that once brought databases down become everyday workloads that need infrastructure built to serve them.


Materialize is our answer to that shift - and we're just getting started.


On this page


- Common Materialize Use Cases
- Accelerated Queries
- Search
- Authorization-Aware User Experiences
- Analytics and Data Platforms
- RAG Systems
- Looking Ahead


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)
