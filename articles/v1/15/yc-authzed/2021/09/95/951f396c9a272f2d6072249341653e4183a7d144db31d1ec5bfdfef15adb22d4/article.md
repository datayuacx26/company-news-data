---
schema_version: "1.0.0"
document_id: "951f396c9a272f2d6072249341653e4183a7d144db31d1ec5bfdfef15adb22d4"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/spicedb-is-open-source-zanzibar"
published_at: "2021-09-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:ed2933cc6c771a82e051b864e3a7974e9bb7a010f60fbf63847e811eaed336a3"
---

# SpiceDB is Open Source Zanzibar!

Today we’re releasing our production-ready implementation of Google Zanzibar:[SpiceDB, an open source, Zanibar-inspired permissions database](https://github.com/authzed/spicedb) .


## Introduction


[Zanzibar](https://research.google/pubs/pub48190/) , is a distributed relationship-based authorization system that Google uses to manage permissions for most of their core cloud products (Docs, YouTube, Calendar, Maps, etc). It is an incredibly flexible, robust, and performant service, with 99.999% uptime and 20ms 99th %ile latency for permissions checks.


As second-time startup founders and long-time product builders, we’ve run into application permissions challenges numerous times in the past. While building the[Quay container registry](https://www.projectquay.io/) , which was eventually acquired by CoreOS, we took the standard path and delegated permissions to our application framework. This model brought us quickly to production, but would later constrain us in terms of flexibility and performance. There are some[product](https://issues.redhat.com/browse/PROJQUAY-615)[features](https://issues.redhat.com/browse/PROJQUAY-2294?focusedCommentId=19014067&page=com.atlassian.jira.plugin.system.issuetabpanels%3Acomment-tabpanel#comment-19014067) still in the icebox because the permissions system couldn’t model recursive relationships (think groups that belong to groups) out of the box. When our service became popular, computing and re-computing permissions became the single largest consumer of database CPU in our production[Quay.io](https://quay.io/) service.


In the summer of 2019, Google published the Zanzibar paper, which lays out a blueprint for both an API and service architecture which can flexibly solve these kinds of permissions problems, even at the largest of scales. Just over one year ago, we set out to build a company to bring this type of permissions system to everyone else. With[Authzed.com](https://authzed.com/) , our hosted permissions platform, and[SpiceDB](https://github.com/authzed/spicedb) , the powerful engine behind it, we’ve done just that.


## Why Should I Use a Separate Service for Permissions?


If you’re using any kind of service based architecture, I’m sure you have felt the need to just peek at the permissions governed by another service. If you’ve got one centralized database but code written in multiple languages, I’m sure you’ve felt the pain of having the same permissions code re-written in multiple languages, each with their own backlog and bugs. You may have also noticed that permissions don't lend themselves to being federated or sharded. In fact, as of today,[AWS’s IAM](https://aws.amazon.com/iam/) is still the only global service that AWS offers.


## Why Should I Choose SpiceDB?


In short: SpiceDB is scalable, has support for globally replicated backends, and is available under the permissive Apache License 2.0.


We achieve scalability by implementing the best scaling concepts from the Zanzibar paper, such as distributed caching and parallel execution. With support for[CockroachDB](https://github.com/cockroachdb/cockroach) , your storage backend has a[Spanner](https://research.google/pubs/pub39966/) equivalent which gives you globally distributed data with ACID semantics. And since SpiceDB is available under[Apache 2.0](https://tldrlegal.com/license/apache-license-2.0-(apache-2.0)) , it should be usable in most personal and corporate situations.


As the backing software behind Authzed.com, you can also be confident that the service is production-ready. We’re on the hook for keeping the service alive and running, so we’ve built monitoring in from the ground up. With industry standards like[Prometheus](https://prometheus.io/) metrics,[OpenTracing](https://opentracing.io/) , and structured logging, the service should fit nicely into most application hosting and monitoring stacks.


## Everybody is “Doing Zanzibar”, How is SpiceDB Different?


Aside from being both production-ready and open source, SpiceDB has several additions to and changes from the Zanzibar paper. Instead of relying solely on Spanner, SpiceDB has multiple data backends including Spanner, Postgres, and CockroachDB for durable storage, and an in-memory datastore for testing and development.


SpiceDB also eliminates the special case for users: instead users are defined as just another type of object. This critical deviation allows SpiceDB to stay identity vendor neutral and model subtle scenarios such as simultaneously using multiple identity providers. As far as we know, this is something that no other Zanzibar implementation offers, even the one at Google.


Unlike Zanzibar and other implementations, which are optimized around checking individual permissions, SpiceDB also lets you[compute inverse permissions](https://buf.build/authzed/api/docs/main/authzed.api.v1#LookupResources) . In Zanzibar you can efficiently ask the question: "does` user` have permission to access` resource` ?" With SpiceDB you can additionally ask: "which` resources` can` user` access?" This powerful paradigm, called "ACL filtered list", is commonly used for building personalized landing pages and search.


Finally, to make it easier to develop against SpiceDB, we offer a number of developer conveniences. A powerful Schema DSL, the[SpiceDB Schema Language](https://authzed.com/docs/reference/schema-lang) , makes it easier to write and reason about a permissions system's configuration. Our playground, and its backing developer API, makes it easy to iterate on schema and relationships. And finally, there is an in-memory test server command built into the binary which makes it easy to integrate with CI and tests.


All of these improvements come directly from our experience creating, running, and integrating with Authzed.com, our hosted service which is powered by SpiceDB. Probably the most notable way SpiceDB is different from most other permissions systems solutions, is that you can pay someone to run it on your behalf!


## How Can I Contribute?


Early, often, and in a variety of ways! We’re looking for feedback about how the service and its APIs meet your application’s requirements. While it has been battle tested on a number of use cases already, there are always interesting new uses that crop up! If you feel like working on the service itself, we’re open to patches and contributions using standard GitHub development practices.


Get started today by checking out the[source code](https://github.com/authzed/spicedb) ,[protecting your first app](https://docs.authzed.com/guides/first-app/) , or just[join us on Discord](https://authzed.com/discord) to chat about SpiceDB, or Zanzibar in general.


On this page


- Introduction
- Why Should I Use a Separate Service for Permissions?
- Why Should I Choose SpiceDB?
- Everybody is “Doing Zanzibar”, How is SpiceDB Different?
- How Can I Contribute?


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)


[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Jimmy Zelinskie · Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)
