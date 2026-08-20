---
schema_version: "1.0.0"
document_id: "cf6cd3ff25f5346a5d491c9911e49c375855be7fc71f5e19a91605976ff136c8"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/spicedb-tracing-how-to-interpret-our-opentelemetry-traces"
published_at: "2023-11-21T14:03:54.265+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:b964be0d2ba17bf9ac9631b9abcdb9c0724bc5ec92f1c7433a15b388ecb7db3d"
---

# SpiceDB Tracing: How to interpret our OpenTelemetry traces

**T﻿ranscription**


This is Victor from AuthZed speaking and today we're going to look into some of the recent changes introducing the OTel instrumentation in SpiceDB.


Before these changes, folks needed a bit of knowledge on the internals of SpiceDB in order to interpret what was happening in requests, right?


It should be possible to visualize how SpiceDB walks the graph and that represents your schema. So you can better understand the sequence of steps taken to respond to an API request.


We're going to look into an example CheckPermission request using a simple schema with user and resource and the usual "view" permission, as you can see here which is granted if either user has a "reader" or "edit" relationship over a resource.


I have my SpiceDB instance running in the background with a Postgres datastore and I've spun up an instance of Jaeger, a popular distributed tracing service, and I've also just issued a check request with SpiceDB companion CLI tool, Zed. Let's have a look at the trace.


As you all probably know SpiceDB has a tunable consistency model which allows clients to decide the levels of consistency of their request.


By default, Zed uses minimize_latency which offers a good compromise between performance and security with a bounded staleness of up to five seconds maximum by default.


OptimizedRevision, ayou can see here at the top, is what indicates that minimized latency has been used as the server computes and a snapshot of the system and then SpiceDB validates the request parameters and making sure that it adheres to the schema that is currently in the system. As you can see in these two checks on the name spaces or the definitions on your schema.


Here's where the interesting bits start- this is at the first dispatch or we usually refer as sub problems being computed and we've issued a "view" over a specific resource for a user and so this is the first dispatch, this is the first sub-problem being computed.


This subproblem then SpiceDB determines that is composed of two parts or two sub problems: the "reader" side and the "edit" side. In turn, SpiceDB is going to turn these two guys into a new dispatch.


This is happening in one single instance so it's not actually dispatching to a different physical node, but for all intents and purposes, it should be the same. Here we can see then the subproblems of the first "view" permission check being computed.


We have "reader" on one side and then "edit" on the other side and the plus sign indicating this is a union. A "view" is a permission is granted to a user of resource over a resource if either they have "reader" or "edit" relationships.


When this is dispatched, then SpiceDB determines that this is a terminal or indicates that this is a relationship and the only thing it has to do is to actually query the database.


As you can see here it accesses the QueryRelationships datastore API and then issues a query "select." You can see details here of the actual SQL query issued to the Postgres database in case you're interested to know what's happening under the hood.


This is the new structure of the Otel traces that were introduced in SpiceDB 1.27.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)
- [Policy-Based Access Control (PBAC) vs Google Zanzibar: When You Should Use One or the Other](https://authzed.com/blog/policy-based-access-control)


On this page


- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
