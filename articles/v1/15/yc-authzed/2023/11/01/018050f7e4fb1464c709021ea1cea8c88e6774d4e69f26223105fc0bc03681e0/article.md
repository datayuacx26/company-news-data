---
schema_version: "1.0.0"
document_id: "018050f7e4fb1464c709021ea1cea8c88e6774d4e69f26223105fc0bc03681e0"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/fall-2023-product-update"
published_at: "2023-11-03T21:53:01.661+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:0400aeb387bd8b5c70a4de5b789a1db8c849bef3fcb08b9fee808fa1d5022f0a"
---

# Fall 2023 Product Update

## We're back


Just like the seasonal favorite, PumpkinSpiceDB is back! We experienced a time distortion and suddenly realized we've missed sharing updates for far too long.


As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community.


A few highlights:


- [Caveats](https://authzed.com/blog/caveats) are now generally available!
- SpiceDB now features a Bulk Check API for batching checks.
- For AuthZed customers,[FGAM](https://authzed.com/blog/introducing-fine-grained-access-management) and audit logging provide additional administrative tooling for SpiceDB.


### Check out our latest presentation


Learn to manage your permissions system like Google and follow along with the journey of building SpiceDB! **[W﻿atch Recording](https://www.youtube.com/watch?v=p8xh_z6PUqE)**


## Reading List


**AuthZed Blogs**
We've been writing like the wind! Check out some of our new posts:


- [Getting to 1 Million QPS on SpiceDB Dedicated with CockroachDB](https://authzed.com/blog/google-scale-authorization)
- [Authorization Must Scale](https://authzed.com/blog/authz-must-scale)
- [Zed Tokens, Zookies, Consistency for Authorization](https://authzed.com/blog/zedtokens)
- [A Primer on Modern Enterprise Authorization Systems](https://authzed.com/blog/authz-primer)
- [Hotspot Caching in Google Zanzibar and SpiceDB](https://authzed.com/blog/hotspot-caching-in-google-zanzibar-and-spicedb)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


**Guest Posts**
Learn more about our collabs with other organizations:


- [ABAC on SpiceDB: Enabling Netflix’s Complex Identity Types](https://netflixtechblog.com/abac-on-spicedb-enabling-netflixs-complex-identity-types-c118f374fa89) at Netflix Tech Blog
- [Flexible and correct Identity Access Management](https://www.cockroachlabs.com/blog/authzed-and-cockroachdb/) at Cockroach Labs


## SpiceDB


**Releases**
SpiceDB v1.17.0 -[v1.26.0](https://github.com/authzed/spicedb/releases/tag/v1.26.0) (latest)


**New Features**


- **[Caveats](https://authzed.com/blog/caveats)** : We were taken ABAC when we found a way to effectively model policy-based authorization in SpiceDB! Caveats allow you to conditionally evaluate permissions based on user-defined access management policies.[Top-3 Most Used SpiceDB Caveat Patterns](https://authzed.com/blog/top-three-caveat-use-cases)
- **Bulk Check API for batching checks** : Now you can send multiple concurrent check permission requests with a single API call!
- **Datastore pagination** : Datastore accesses are now paginated to efficiently handle large Reads and LookUps


**Community Updates**
We now have 1,581 members in the SpiceDB[Discord](https://authzed.com/discord) !


**New: Github Discussions** We've recently enabled[GitHub discussions](https://github.com/orgs/authzed/discussions) to help facilitate asynchronous communication in our community.


**Huge thank you to new contributors:**
@AmbientLighter, @posya, @rxu-plaid, @trrrrrys, @suttod, @yordis, @drew-richardson, @bradengroom, @thewunder


## SpiceDB Office Hours


Every Friday at 13:00 ET, join SpiceDB experts in the office-hours Discord channel to ask questions, discuss project topics, and get to know your fellow community members![Join here](https://discord.gg/ta3HF2yT?event=1166148762960400405)


## AuthZed


**Product Updates**


- **[Fine Grained Access Management (FGAM)](https://authzed.com/blog/introducing-fine-grained-access-management)** : Manage access to your access management system! Now SpiceDB Dedicated and Enterprise users can apply the principle of least privilege a step further by allowing a client to access only the minimum set of APIs required to perform its operations.
- **[Audit Logging - early access](https://authzed.com/docs/spicedb-dedicated/audit-logging)** : Asynchronously log every API call and stream to your preferred log sink. SpiceDB Dedicated and Enterprise users can access logs containing full details related to a request, including API token hashes, RPCs, payload request IPs, responses, and any possible errors.
- **Offsite + New Friends** : It's been a banner year for growing the AuthZed team! We were delighted to gather together in the Poconos last month to get acquainted, share knowledge, and make big plans for next year.


## Entropy


According to the poll in[#off-topic](https://discord.gg/spicedb) , the Chalamet film adaptation of Dune is by far everyone's favorite. We'll see if it holds strong after the second movie debuts next year...


@thewunder pushed a new release of their php client for SpiceDB,[chiphotle-rest](https://github.com/alsbury/chiphpotle-rest) , which never fails to make us hungry


@bison and the team learned that most rental car agencies do not accept virtual credit cards #themoreyouknow


@joey lamented the loss of[Product 19](https://en.wikipedia.org/wiki/Product_19) and @ecordell informed us that there is, indeed,[a podcast for everything](http://bowl.rest/) .


On this page


- We're back
- Check out our latest presentation
- Reading List
- SpiceDB
- SpiceDB Office Hours
- AuthZed
- Entropy


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Engineering Introducing: Fine-Grained Access Management The systems we build at AuthZed are the direct result of feedback from our community and customers. Because security is the core of our flagship product, SpiceDB, we take feedback on this topic very seriously. We’ve heard you, and today we’re proud to introduce a better way to secure AuthZed customers’ client applications accessing the SpiceDB API: **Fine-Grained Access Management** (FGAM). Apr 27, 2023 · 4 min](https://authzed.com/blog/introducing-fine-grained-access-management)[Engineering Introducing: Fine-Grained Access Management The systems we build at AuthZed are the direct result of feedback from our community and customers. Because security is the core of our flagship product, SpiceDB, we take feedback on this topic very seriously. We’ve heard you, and today we’re proud to introduce a better way to secure AuthZed customers’ client applications accessing the SpiceDB API: **Fine-Grained Access Management** (FGAM). Victor Roldan Betancort · Apr 27, 2023 · 4 min](https://authzed.com/blog/introducing-fine-grained-access-management)
