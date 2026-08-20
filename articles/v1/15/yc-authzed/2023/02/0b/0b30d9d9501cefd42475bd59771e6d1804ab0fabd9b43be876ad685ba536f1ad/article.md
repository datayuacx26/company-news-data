---
schema_version: "1.0.0"
document_id: "0b30d9d9501cefd42475bd59771e6d1804ab0fabd9b43be876ad685ba536f1ad"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/crdb_v23"
published_at: "2023-02-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:f6eac7c408102aa5cda7747b0a67262a58d51990fd1f15b2f8be65fa5d2458ce"
---

# CockroachDB 23.1 new defaults impact SpiceDB

SpiceDB is a fairly unique database when it comes to consistency. Most databases implement a pattern called MVCC. Without going too deeply, when a query is made to an MVCC database, it runs that query against a snapshot of the data it manages. SpiceDB not only implements MVCC, but also supports the ability to specify the desired consistency on each request. By default the consistency level used is` minimize_latency` which delegates to the server to pick the fastest snapshot available. You can find all of the possible consistency values in the[SpiceDB Consistency API docs](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.Consistency) and read more about[ZedTokens](https://authzed.com/docs/reference/zedtokens-and-zookies#what-is-a-zedtoken) , but today we're going to focus on requests that are choosing their own specific snapshot by providing` at_exact_snapshot` consistency.


For most databases, including all of SpiceDB's available datastores at the time of this article, keeping around all snapshots for the entire history of a database isn't feasible. Because of this, SpiceDB provides the` --datastore-gc-window` to specify the maximum age of snapshots that are available in the datastore. If a request to SpiceDB contains a consistency` at_exact_snapshot` outside of this window, it will throw an error because it knows that datastore won't be able to provide that data it would need for a response.


### C﻿hange for CockroachDB v23.1


In CockroachDB v23.1, an important default configuration was changed for SpiceDB users:` gc.ttlseconds` . It seems pretty intuitive from the name what this value represents, but because databases can be subtle, we'll double check the[CockroachDB documentation](https://www.cockroachlabs.com/docs/stable/configure-replication-zones.html#replication-zone-variables) :


> The number of seconds overwritten values will be retained before garbage collection. Smaller values can save disk space if values are frequently overwritten; larger values increase the range allowed for AS OF SYSTEM TIME queries, also known as Time Travel Queries.
>
>
> It is not recommended to set this below 600 (10 minutes); doing so will cause problems for long-running queries. Also, since all versions of a row are stored in a single range that never splits, it is not recommended to set this so high that all the changes to a row in that time period could add up to more than 512 MiB; such oversized ranges could contribute to the server running out of memory or other problems.
>
>
> **Note:** Ensure that you set` gc.ttlseconds` long enough to accommodate your backup schedule, otherwise your incremental backups will fail with this error. For example, if you set up your backup schedule to recur daily, but you set` gc.ttlseconds` to less than one day, all your incremental backups will fail.
>
>
> Default: 90000 (25 hours)
>
>
> However, all CockroachDB Serverless clusters have a default gc.ttlseconds of 4500 seconds (1 hour and 15 minutes) that cannot be altered.


The CockroachDB datastore in SpiceDB is affected by this setting in particular because it relies on Time Travel Queries in order to implement querying at different snapshots. To properly configure SpiceDB using the CockroachDB datastore, SpiceDB's` --datastore-gc-window` cannot be older than CockroachDB's` gc.ttlseconds` . Thus, changes to the default value for` gc.ttlseconds` ultimately limit how far back in time you can query in SpiceDB.


**Users of Authzed's managed SpiceDB services are not affected by this change.** We will continue to configure our CockroachDB deployments to the previous default of 25 hours. However, open source users of SpiceDB have a slightly more complicated situation that varies based on how they are running CockroachDB.


SpiceDB versions up to and including v1.16.2 exit with a fatal error if the CockroachDB configuration for` gc.ttlseconds` is less than` --datastore-gc-window` . Future versions of SpiceDB will non-fatally warn and fallback to the CockroachDB value in this scenario to avoid any possible downtime while upgrading.


Below you can find a table of the changes to CockroachDB's` gc.ttlseconds` default value and configurability:


Serverless v22 Serverless v23 Dedicated v22 Dedicated v23


TTL 4 hrs **1.25 hrs** 25 hrs 4 hrs


Configurable N N Y Y


If you are spinning up a new CockroachDB Dedicated deployment, the default value has changed from 25 hours to 4 hours. By changing the configuration using the SQL` ALTER ZONE default CONFIGURE ZONE USING gc.ttlseconds = 90000;` , you can restore this value to 25 hours and the team at Cockroach Labs has assured us that any future upgrades to CockroachDB will not reset the value once you have configured it. Because of this policy, existing CockroachDB Dedicated deployments should keep their existing value of 25 hours.


If you are using CockroachDB Serverless, the situation is out of your hands. You are not able to configure this setting and the value has been reduced from 4 hours to 1.25 hours. For SpiceDB users with workloads that require a consistency level of` at_exact_snapshot` with times greater than 1.25 hours, we recommend running CockroachDB Dedicated or operating it yourself.


We'd like to specifically thank the folks over at Adobe for reporting this change in behavior. Additionally, we really appreciate the transparency from the team over CockroachDB about the changes and how they will impact their services and the product going forward. It's always a pleasure to see open source collaboration across businesses.


I'd also like to thank OpenAI's DALL-E for the hilarious image of "a cartoon of a cockroach under a magnifying glass".


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- C﻿hange for CockroachDB v23.1
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
