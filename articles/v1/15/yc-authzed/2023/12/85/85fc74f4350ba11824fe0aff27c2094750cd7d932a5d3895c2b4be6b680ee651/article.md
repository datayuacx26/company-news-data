---
schema_version: "1.0.0"
document_id: "85fc74f4350ba11824fe0aff27c2094750cd7d932a5d3895c2b4be6b680ee651"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/zed-file-z-4902-spanner-spikes"
published_at: "2023-12-07T20:32:33.639+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:0cf69c40c0b8f8583e4887bdd2c23553dd0102dbef1a8371742fa4269c57ebd5"
---

# Zed File #Z-4902: Spanner Spikes

# Zed File # Z-4902: Spanner Spikes


💼 **Case facts**
Customer: \[redacted\] // Product: AuthZed Dedicated // Datastore: Spanner


A storm was brewing at AuthZed customer \[redacted\]- as their traffic grew, so did the number of permission requests, driving check latencies up to a full second at the 95th percentile. This spike in latency degraded the client system's performance - it was time to call in the experts.


AuthZed special agents were on the case.


*Read on to discover how our agents used AuthZed’s extensive extant observability toolkit and quickly rolled out additional tooling and optimizations to find the solution among a complex myriad of contributing factors.*


*Bonus - all improvements are now available in SpiceDB v1.28!*


**First aid**


As an emergency stopgap, AuthZed immediately scaled up resources and tuned Spanner’s configuration, improving overall performance. However, latencies were still spiking every few hours, indicating the system was not yet fully stable. With these initial measures in place, the team started investigating.


**Dispatch deduplication**


Agent V knew he had to first check the customer’s schema - it might reveal complex permissions that could lead to many subproblems being dispatched. With enough capacity, the system should be capable to handle it, but customer’s cluster was relatively small.


The venerable manual of Agent Operations, the Zanzibar paper, described a clever strategy to reduce fan out of complex permissions: request deduplication. Authzed Agent X introduced improvements in the SpiceDB dispatch with a deduplication middleware that yielded a 30% reduction in dispatched requests, which in turn, led to a proportional reduction in database usage. Despite this notable overall improvement in system performance, persistent latency spikes suggested there still were other underlying issues to address.


**Tracking consistency**


Initial investigation efforts were hindered due to insufficient observability, leaving key data shrouded in mystery. The situation improved when Agent J improved SpiceDB’s request consistency middleware to produce metrics describing the requested consistency level. This tool uncovered a correlation between the latency spikes and increases in '` fully_consistent` ' traffic, providing a new lead - one that would crack the case.


**Schema spotting**


Pursuing the lead, Agent V knew the team was onto something- they just needed more data. He set to improve the readability of OpenTelemetry traces across the dispatch graph and to expose additional metadata, particularly schema traversal details. Direct tracking of Spanner server-side query latency posed a challenge, as using` QueryWithStats` API method increased Spanner’s CPU usage significantly. Undeterred, Agent V successfully integrated Spanner Client metrics with OpenTelemetry and exposed the metrics through SpiceDB’s Prometheus endpoint. And to add the last bit of information missing, Agent V exposed request and response payloads via new CLI flags.


**The solution**


Newly equipped with observability data, the team utilized Google Cloud Platform’s log aggregator to locate the timing and source of the latency spikes. They discovered that \[redacted\] was intermittently issuing up to 800 CheckPermission calls within milliseconds, overwhelming the processing capacity of the provisioned Spanner units.


As it turned out, the database simply had insufficient capacity to handle these spikes.


A crucial log entry showed a single user making over 600 requests within 250 milliseconds for the same permission check, **indicating inefficient ACL filtering that fanned out individual checks too quickly for the allocated capacity, all of which is more efficiently computed with BulkCheck or LookupResources.**


AuthZed advised the customer to move from fanout CheckPermission calls to BulkCheck, which led to significant latency reduction and improved cache efficiency. As a result, the SpiceDB cluster has achieved stability, eliminating the problematic latency spikes and enhancing the performance of the Permissions System.


These improvements are now available to everyone in SpiceDB v1.28!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization) *﻿[Policy-Based Access Control (PBAC) vs Google Zanzibar: When You Should Use One or the Other](https://authzed.com/blog/policy-based-access-control)


On this page


- Zed File # Z-4902: Spanner Spikes
- Additional Reading


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)
