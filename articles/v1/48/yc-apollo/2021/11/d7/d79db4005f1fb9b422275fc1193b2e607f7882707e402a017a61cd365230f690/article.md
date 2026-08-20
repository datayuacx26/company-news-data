---
schema_version: "1.0.0"
document_id: "d79db4005f1fb9b422275fc1193b2e607f7882707e402a017a61cd365230f690"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-federation-2"
published_at: "2021-11-03T09:15:01+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:35.155530+00:00"
content_hash: "sha256:715187a1c5cbf3d7f3b7dbaeef213f892a6ce61e76a5982b2a044c1e244414a0"
---

# Announcing Apollo Federation 2

*🚀 🎉 Heads up – Federation 2 and Gateway 2.x are now GA! Check out the[GA announcement blog](https://www.apollographql.com/blog/apollo-federation-2-is-now-generally-available/) or head over to the[Federation docs](https://www.apollographql.com/docs/federation/) to get started.*


---


It’s been amazing to see the momentum grow around Apollo Federation with over 1M weekly downloads and 12K+ projects using Apollo Federation. Thanks to community contributions, the thriving ecosystem around Apollo Federation means you can federate subgraphs written in[over 12 languages](https://www.apollographql.com/docs/federation/other-servers/) and frameworks.


Originally[released in 2019](https://www.apollographql.com/blog/announcement/apollo-federation-f260cf525d21/) , Federation powers some of the largest graphs in the world, including[Netflix](https://netflixtechblog.com/how-netflix-scales-its-api-with-graphql-federation-part-2-bbe71aaec44a) ,[Walmart](https://medium.com/walmartglobaltech/federated-graphql-walmart-bfc85c2553de) ,[RetailMeNot](https://engineering.ziffmedia.com/retailmenot-graphql-federation-3bb36dac5aaa) , and[PayPal](https://medium.com/paypal-tech/graphql-at-paypal-an-adoption-story-b7e01175f2b7) . Developers are choosing Apollo Federation because it’s a proven approach for rapidly scaling a graph across multiple teams.


Today, we’re excited to celebrate the next major milestone: Apollo Federation 2. The new version builds on the success of the original with an improved shared ownership model, enhanced type merging, and cleaner syntax for a smoother developer experience. It’s backwards compatible, requiring no major changes to your subgraphs.[Try the alpha today!](https://www.apollographql.com/docs/federation/v2/quickstart/) 🚀


## Apollo Federation 2


Apollo Federation is an architecture for declaratively composing subgraphs into a unified graph. Each team can own their slice of the graph independently, empowering them to deliver autonomously and incrementally.


Designed in collaboration with the GraphQL community, Federation 2 is a clean-sheet implementation of the core composition and query-planning engine at the heart of Apollo Federation. We’ve improved the developer experience by adding deeper static analysis, cleaner error messages, and new composition hints that help you catch errors sooner and understand how your schema impacts performance. With the new version, we’ve streamlined common tasks like extending a type and simplified advanced workflows like migrating a field across subgraphs with no downtime.


Following in the footsteps of modern infrastructure projects like Elasticsearch and MongoDB,[we’re moving Federation 2 to the Elastic License v2](https://www.apollographql.com/blog/announcement/moving-apollo-federation-2-to-the-elastic-license-v2) — an open and permissive license with few restrictions. This change shouldn’t affect the vast majority of our community, and the new code is still free to use and modify. Subgraph packages will remain MIT, as well as the 0.x Federation packages, so there are no changes to what you’re doing today.


### Build with any natural GraphQL schema


Federation 2 adds first-class support for shared interfaces, enums, and other value types. Common tasks like extending a federated type or denormalizing a field for better performance are now possible without special directives and keywords.


### Deliver smaller increments with better shared types


Apollo Federation already supports a shared ownership model that lets you spread a federated type across subgraphs to distribute its implementation. You can reuse common value types like interfaces and enums. However, there are restrictions on how shared types can be defined and evolved. As a result, teams may find themselves stuck when making changes to shared types until all subgraphs are able to deliver a coordinated change in lockstep.


Federation 2 removes these restrictions with an improved shared ownership model and enhanced type merging that enables teams to deliver smaller incremental changes with a cleaner separation of concerns.


**Flexible value type merging** is now supported, so value types don’t need to be identical across subgraphs. Value type definitions are now merged into a single unified type, much like type merging support for federated types today. Smaller incremental changes, like adding a field, can often be rolled out one subgraph at a time.


**Federated types have improved shared ownership,** which means fields can now exist in multiple subgraphs simultaneously. This paves the way for natively supported field migrations with an asynchronous transfer of ownership from one subgraph to another with no downtime or tight release coordination.


### Catch errors sooner with improved static analysis


Federation 2 has deeper static analysis, better error messages and a new generalized composition model that helps you catch more errors at build-time instead of at runtime. The rewritten composition engine now validates all theoretically possible queries and provides more descriptive error messages when a query can’t be satisfied.


New composition hints help you understand how schema definitions influence query planning and performance. We’ve integrated them into the[powerful tools for Apollo Federation](https://www.apollographql.com/blog/announcement/tooling/powerful-new-graphql-tools-for-apollo-federation/) :


- [Apollo Workbench](https://marketplace.visualstudio.com/items?itemName=apollographql.apollo-workbench) shows composition hints in the problems tray with new hover tips.
- [Rover](https://www.apollographql.com/docs/rover/) includes composition hints in both standard and structured JSON output so you can integrate them with other tools in your pipeline.
- [Apollo Studio](https://www.apollographql.com/docs/studio/) uses composition hints to help you ensure design guidelines and best practices.


While Federation 2 is[backwards compatible](https://www.apollographql.com/docs/federation/v2/federation-2/backward-compatibility/) , its enhanced validation might catch a few things that the original Federation didn’t. Things you’ll be happy to know about!


## Quick tour of Federation 2


We’ve prepared this video to highlight some key improvements in Federation 2, including Apollo Workbench support for composition hints. You’ll also learn how to compose and run a graph with Federation 2 using the v2 gateway and Apollo Studio.


## Getting started


We’d love your early feedback on the initial alpha release!


- [Read the docs](https://www.apollographql.com/docs/federation/v2)
- [Try it out](https://www.apollographql.com/docs/federation/v2/quickstart/)
- [Let us know what you think!](https://community.apollographql.com/t/announcing-apollo-federation-2/1821)


## What’s next?


There’s a lot we’re adding in the coming months:


- New directives that support a stronger source of truth, field migration across subgraphs, and type merging that can be relaxed even further.
- Expanded use of[core schemas](https://specs.apollo.dev/core/v0.2) to compose your own directives in subgraphs and process supergraph schemas with a new[core-schema-js](https://github.com/apollographql/core-schema-js) library.
- Harmonizing shared value types across subgraphs to steward core common types to a desired state.
- Importing shared types into subgraph schemas, to keep things more DRY.
- Advanced caching, auth, demand control, rate limiting, governance, and more!


## Learn more at GraphQL Summit


See Federation 2 in action at GraphQL Summit on November 10th and 11th, 2021.


[Register for free](https://summit.graphql.com/#register) and join us for the keynote and a deep dive session on “The Future of Apollo Federation.” We’ll discuss and demo Federation 2, designed in collaboration with Netflix, to make it even easier to build, use and scale your graph!


## Get involved!


- [GitHub](https://github.com/apollographql/federation)
- [Apollo Community Forum](https://community.apollographql.com/)
- [Subgraph Library Maintainer Support](https://community.apollographql.com/t/apollo-federation-subgraph-library-maintainer-support/1112)


Written by


Phil Prasek


[Read more by Phil Prasek](https://www.apollographql.com/blog/author/philprasek)
