---
schema_version: "1.0.0"
document_id: "6a6cb20f0722c4a90d12bdc958a819ad40a366508160cecd7dd2f78434b7a88c"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/zedtokens"
published_at: "2023-10-18T13:35:59.596+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:394ee763b9509b2b5197dcff830ea63b0f9dc0d3f118b1cad08e395fb0cd2b0e"
---

# Zed Tokens, Zookies, Consistency for Authorization

## Secure authorization logic requires data consistency


If you've ever taken a course on databases or dove head-first into documentation, you may have bumped into the acronym "ACID". ACID describes the guarantees that are expected in order to effectively rely on a database system: atomicity, consistency, isolation, and durability. While it's rumored that the term "consistency" was fabricated in order to complete a catchy acronym, its colloquial usage by the software industry is no joke.


Taking a stroll across the Internet, one finds that many definitions of consistency are self-referential; they define consistency using the term "consistent". While one could attempt to define the term in a dense mathematical way, I'll leave that to Wikipedia. When most folks are referencing consistency, I find that they are trying to suss out the invariants that need to be preserved when their data is observed. This contract is largely driven by the problem and domain of the software being built and thus requires careful consideration.


The process of designing well-architected software often begins with determining the contracts and scope of the system. While the previous statement seems uncontroversial, I've seen time and time again that there is a critical component whose contract is often an afterthought: authorization systems.


Unless the domain is sensitive or regulated (e.g. healthcare), consideration of the design and requirements for access control is an exercise saved until a project has found product-market fit. By this time in the product's development, new functionality is assumed to be features that extend the existing foundation rather than something that entails its own end-to-end design.


## Google's journey building authorization


When faced with the challenge of building access control for their products, the team at Google recognized what many had not: they needed to do their due diligence and design a system that considered the consistency model used for authorization data.


> "Handling end-user access controls for distributed objects can be complex. The access controls must be handled **consistently** across all systems. This can be relatively straightforward if access is fixed at creation time (e.g. "this email may be read only by the recipient"). But if the ACL (access control list) can be updated, things get difficult quickly.
>
>
> \[..\]
>
>
> To solve this problem, we developed Zanzibar. Zanzibar has a central, highly-replicated, highly-available, and **consistent** ACL store."
>
>
> -
>
>
> Lea Kissner, co-author
>
>
> [Google's Zanzibar authorization system](https://authzed.com/blog/what-is-google-zanzibar)


Kissner and their co-authors have done the world a favor by[publishing](https://authzed.com/zanzibar) the observed results of their design. The paper focuses on various aspects of the design, but explicitly chooses to focus on[detailing the consistency model](https://authzed.com/zanzibar#2.2-consistency-model) . The paper even goes so far as to give a name to a scenario that commonly plagues inconsistent authorization systems:[The New Enemy Problem](https://authzed.com/blog/new-enemies) .


## SpiceDB is the successor to Google Zanzibar


Fast-forward to today: an open source community is flourishing around[SpiceDB](https://github.com/authzed/spicedb) , a re-imagining of Google's Zanzibar system that is not only faithful to the paper, but also inclusive in its design for those building software outside of a tech giant like Google.


SpiceDB is designed to start from a strongly consistent posture while providing users a way to relax overzealous consistency requirements in order to unlock higher performance.[Zed Tokens](https://authzed.com/docs/reference/zedtokens-and-zookies) , SpiceDB's analogue to the Zookie concept in Zanzibar, are a vital part of solving the New Enemy Problem. Zed Tokens are an encoded value that represents a particular point in time in SpiceDB. Requests to SpiceDB can optionally specify expected consistency when querying authorization data by providing Zed Tokens for when they need to query at or after a particular time (in addition to allowing the server to pick a time or specifying the most recent time in the system).


The most important benefit of designing for ad hoc consistency is that adopters can be confident that when they require strong consistency for a particular problem, they won't have to accept silent data corruption or re-architect their systems. SpiceDB is not the only system that takes this approach to consistency; the core databases powering Azure and AWS services, CosmosDB and DynamoDB respectively, also allow for this ad-hoc consistency model. And just as the cloud revolution moved the industry forward by democratizing elastic infrastructure, SpiceDB is now moving the industry toward a more secure way to build access control into our applications.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization) *﻿[Policy-Based Access Control (PBAC) vs Google Zanzibar: When You Should Use One or the Other](https://authzed.com/blog/policy-based-access-control)


On this page


- Secure authorization logic requires data consistency
- Google's journey building authorization
- SpiceDB is the successor to Google Zanzibar
- Additional Reading


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)
