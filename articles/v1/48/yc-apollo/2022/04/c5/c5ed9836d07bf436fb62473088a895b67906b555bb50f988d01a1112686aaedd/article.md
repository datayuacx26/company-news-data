---
schema_version: "1.0.0"
document_id: "c5ed9836d07bf436fb62473088a895b67906b555bb50f988d01a1112686aaedd"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-federation-2-is-now-generally-available"
published_at: "2022-04-13T09:30:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:34fc72f2a15c69a7c36c96df95587df3472018d34ede9f05212bd0c2b5c48677"
---

# Apollo Federation 2 is now generally available

The momentum continues to build around Apollo Federation with over 1M weekly downloads and 18K+ projects now using Apollo Federation, up 50% in the last 5 months since[Federation 2 was first announced](https://www.apollographql.com/blog/announcement/backend/announcing-federation-2/) .


We’d like to extend a big thank you to our community! Because of your contributions, Federation developers can compose subgraphs written in[over 18 languages and frameworks](https://www.apollographql.com/docs/federation/v2/other-servers) !


**Today, we’re excited to announce the general availability of Apollo Federation 2, with improvements across the entire Apollo platform including Gateway 2.0, the Rover CLI, and Apollo Studio.**


Keep reading to learn more about Federation and what you can expect from the new improvements in Federation 2 or head to our docs to[get started today](https://www.apollographql.com/docs/federation/v2/) ! 🚀


## What’s new in Apollo Federation 2


Apollo Federation is a[modular GraphQL architecture](https://www.apollographql.com/docs/federation/v2) that powers the world’s most important graphs, including[Netflix](https://netflixtechblog.com/how-netflix-scales-its-api-with-graphql-federation-part-2-bbe71aaec44a) ,[Walmart](https://medium.com/walmartglobaltech/federated-graphql-walmart-bfc85c2553de) ,[Adobe](https://medium.com/adobetech/graphql-making-sense-of-enterprise-microservices-for-the-ui-46fc8f5a5301) , and[more](https://www.apollographql.com/docs/federation/enterprise-guide/federation-case-studies/) .


Designed in collaboration with the GraphQL community, Federation 2 is a clean-sheet implementation of the core composition and query-planning engine at the heart of Apollo Federation.


The new version builds on the success of the original with an improved shared ownership model, enhanced type merging, and cleaner syntax for a smoother developer experience. It’s backward compatible and designed for incremental adoption one subgraph at a time.


### Build with a smoother developer experience


Federation 2 adds first-class support for shared interfaces, enums, and other value types. Common tasks like extending a federated type are now possible without special directives and keywords, resulting in cleaner syntax.


### Deliver smaller increments with better shared types


Apollo Federation’s shared ownership model lets you spread a federated type across subgraphs so that you can reuse common value types like interfaces and enums. Federation 2 improves upon the shared ownership model and makes shared types more flexible:


**Value type definitions are now merged into a single unified type** . Value types don’t need to be identical across subgraphs, so smaller incremental changes, like adding a field, can often be rolled out one subgraph at a time by using the` @inaccessible` directive to hide it from the client-facing schema.


**All types are shared equally across subgraphs** . This means you don’t have to use the` extend` keyword.


**All fields have a single source of truth by default.** You can optionally mark a field or object with the` @shareable` directive so other subgraphs can resolve them, which may increase performance.


### Migrate fields across subgraphs without downtime or coordinated delivery


As the shape of your subgraphs change, sometimes fields need to be migrated from one subgraph to another. This previously required a coordinated release across subgraphs to avoid downtime.


With Federation 2, field migration is painless. Simply add the field to the destination subgraph, mark it with` @override` to accept production traffic without downtime, and remove the migrated field from the old subgraph.


### Catch errors sooner with improved static analysis


Federation 2 has deeper static analysis, better error messages, and a new generalized composition model that helps you catch more errors at build-time instead of at runtime. The rewritten composition engine now validates all theoretically possible queries and provides more descriptive error messages when a query can’t be satisfied. New composition hints help you understand how schema definitions influence query planning and performance.


While Federation 2 is backwards compatible, its enhanced validation might[catch a few things](https://www.apollographql.com/docs/federation/v2/federation-2/backward-compatibility#breaking-changes) that the original Federation didn’t. Things you’ll be happy to know about!


## Getting started


⚠️ **Note: The Apollo Contracts preview does not yet support Federation 2. If you are using Contracts in production you should not upgrade to Federation 2 yet. The generally available release of Contracts will include support for Federation 2.**


Moving to Federation 2 is a simple and incremental process:


1. **Upgrade your Gateways to version 2.0.0 or later.** Gateway 2 is backward compatible with Federation 1 and any existing customizations you have, so this won’t break anything.
2. **Turn on Federation 2 composition** for the Rover CLI and Apollo Studio. Federation 2 composition will automatically convert Federation 1 subgraphs, so you can get some great benefits (like improved error messages) without upgrading any subgraphs! 🎉
3. **Upgrade individual subgraphs to Federation 2 syntax at your own pace** . Because Federation 2 composition is backward compatible with Federation 1 subgraphs, you can add Federation 2 syntax to your subgraphs at your own pace.


💡 You can find detailed instructions for the steps above in our[migration guide](https://www.apollographql.com/docs/federation/v2/federation-2/moving-to-federation-2) .


## What’s next


If you’re already using Apollo Federation 1.x, check out[our migration guide docs](https://www.apollographql.com/docs/federation/v2/federation-2/moving-to-federation-2) to start taking advantage of the benefits from Federation 2. If you’re interested in trying things out, check out our[quickstart docs](https://www.apollographql.com/docs/federation/v2/quickstart/setup) or our full[supergraph demo for Federation 2](https://github.com/apollographql/supergraph-demo-fed2) that includes a complete enterprise-grade example.


Check out our public[Federation Roadmap](https://github.com/apollographql/federation/blob/main/ROADMAP.md) to learn about upcoming releases. We’d love to hear your thoughts![Open a GitHub issue](https://github.com/apollographql/federation/issues) to share your feedback.


## Get involved!


If you’re interested in getting involved or becoming a contributor, checkout our GitHub repos and community forums:


- [GitHub](https://github.com/apollographql/federation)
- [Apollo Community Forum](https://community.apollographql.com/)
- [Subgraph Library Maintainer Support](https://community.apollographql.com/t/apollo-federation-2-compatibility-showcasing-community-libraries/3272)


Written by


Phil Prasek


[Read more by Phil Prasek](https://www.apollographql.com/blog/author/philprasek)
