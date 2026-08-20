---
schema_version: "1.0.0"
document_id: "b20e6a178b234eb87a544122d1375c7fa5865d7c97497d1dd10467355f5a77b1"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-changelog-april-17-2023"
published_at: "2023-04-17T16:13:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:6a1e197dec689abe7945354f52f3f7145d97685b23295e15de49787915d923c6"
---

# Apollo Changelog: April 17, 2023

## A new way to navigate


A major update to[our docs site](https://apollographql.com/docs) has made it easier than ever to find what you’re looking for. Having trouble finding your favorite article? Have a question our docs don’t answer? Want to compliment the beautiful new design? You can do all of that and more in[our Discord server](https://discord.gg/NEPmWUdKn7) .


## Apollo’s GraphQL tooling in Rust


If you keep hearing about` apollo-rs` (like the` apollo-compiler` release later in this post) and you’re wondering what it’s all about, we’ve got just the thing! Iryna Shestak gave a talk at GraphQL Summit walking through the motivation and goals of` apollo-rs` and there’s now a condensed version of that talk available on YouTube:


## Fine-tuning Apollo Client’s normalized cache


The standard Apollo Client cache works well with minimal config for most cases, but sometimes you need to really fine-tune your performance. To get you started, here’s a shortened talk from GraphQL Summit by Raman Lally breaking down the internals of the cache:


## Upcoming events


- [Live Coding: Router Deployment—Bring your questions about Router, Rover, and templates. Tuesday, April 18 at 1:00 pm PDT](https://discord.gg/H5FQRuVn?event=1096545688885661746)
- [Office Hours: Apollo iOS—Ask questions and chat about Apollo iOS with a maintainer. Thursday, April 20 at 1:00 pm PDT](https://discord.com/invite/RyuSebwd?event=1095109881020813413)
- [Live Coding: Investigating unusually high usage in your GraphQL API. Wednesday, April 26 at 2:30 pm PDT](https://discord.gg/NEPmWUdKn7)
- [Live Coding: Optimizing a GraphQL API with caching. Thursday, April 27 at 11:00 AM PDT](https://discord.gg/NEPmWUdKn7)


## Rapid roundup


- GraphOS no longer performs[operation checks](https://www.apollographql.com/docs/graphos/delivery/schema-checks/#operation-checks) as part of its[launch](https://www.apollographql.com/docs/graphos/delivery/launches/) process. Instead, you should always run schema checks (with` rover subgraph check` ) *before* deploying updates to a subgraph. Note that launches still *always* perform supergraph schema composition, and your router retains its current valid schema if this composition fails.
- [Apollo Client 3.7.12](https://github.com/apollographql/apollo-client/releases/tag/v3.7.12) fixes a bug with HTTP multipart chunks.
- [Apollo iOS 1.1.2](https://github.com/apollographql/apollo-ios/releases) fixes bugs in` Cancellable` and deprecation messages and enables error interceptors for` Upload` operations.
- [apollo-compiler 0.8.0](https://github.com/apollographql/apollo-rs/releases/tag/apollo-compiler%400.8.0) introduces new ways to limit complexity and several other features and fixes. There are also improvements in[apollo-parser 0.5.1](https://github.com/apollographql/apollo-rs/releases/tag/apollo-parser%400.5.1) and[apollo-encoder 0.5.1](https://github.com/apollographql/apollo-rs/releases/tag/apollo-encoder%400.5.1) .
- There’s a new short-and-sweet tech note:[“Keeping schemas up-to-date in client apps”](https://www.apollographql.com/docs/technotes/TN0038-updating-client-schema/)


## Community spotlight


“One Stop Shop” is a new, open-source way to visualize your GraphQL schema and queries! Check out the[announcement blog post](https://medium.com/@cnbryan129/managing-a-complex-graphql-schema-head-on-down-to-the-one-stop-shop-6b27e1c0c82b) for more details, or go to[https://www.graphql-oss.io](https://www.graphql-oss.io/) right now to try it out.


Also under the` oslabs-beta` umbrella is an open-source, server-side caching solution for GraphQL APIs called TroveQL. The team recently released[a blog post describing how it works](https://medium.com/@shendo87/graphql-has-a-new-best-friend-80831fe412ac) —exciting stuff!


Have you tried out these or any other open-source GraphQL projects recently? We’d love to hear about it over in[our Discord server](https://discord.gg/NEPmWUdKn7) . We’re always looking to try out new technologies!


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
