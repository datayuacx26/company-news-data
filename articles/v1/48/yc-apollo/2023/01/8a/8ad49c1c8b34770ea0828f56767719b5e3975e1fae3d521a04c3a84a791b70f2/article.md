---
schema_version: "1.0.0"
document_id: "8ad49c1c8b34770ea0828f56767719b5e3975e1fae3d521a04c3a84a791b70f2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-changelog-jan-23-2023"
published_at: "2023-01-23T17:14:30+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:7677a023a8c3be6460effddd323feb90d4534324d3a75d2ad124eda17881d270"
---

# Apollo Changelog: Jan 23, 2023

We’re building so much great stuff at Apollo that it can be tricky to keep track of it all! To help with that, we’re kicking off our Changelog series to cover exciting releases for GraphOS and our open-source projects. We’ll also let you know about upcoming live events and new educational content to help you expand and sharpen your graph expertise. So without further ado, let’s see what’s new!


## Boost app responsiveness with @defer in GraphOS


If a query includes a field that takes a long time to resolve, it can delay your graph’s entire response, making your apps feel less responsive. With GraphOS support for the @defer directive (now generally available!), your graph can respond to the client with most of the response data almost immediately, then follow up with slower fields when they’re ready.


To learn more, check out[the announcement blog post](https://www.apollographql.com/blog/announcement/platform/rethinking-react-components-with-defer/) and[the docs](https://www.apollographql.com/docs/graphos/routing/defer) . Make sure to set your graph’s federation version to 2.1 or higher in Apollo Studio!


## Livestreams with library maintainers


This week we kicked off a new livestream series in the[Apollo GraphQL Discord](https://discord.gg/graphos) where maintainers and community experts demonstrate building subgraphs with different libraries and languages. To start, we built a subgraph live using Python and[Strawberry](https://strawberry.rocks/) . Next up, we’ll do the same with Rust and[async-graphql](https://async-graphql.github.io/async-graphql/en/index.html) on February 22. All of our livestreams are in[Discord](https://discord.gg/graphos) , so make sure to join so you don’t miss out!


## GraphOS introduction video


Your graph’s README page in[GraphOS Studio](https://studio.apollographql.com/) now features a friendly introductory video and some handy links to help you get the most out of GraphOS from day one! Look for this section to the right of your README:


## Keeping you in suspense


Can’t wait to use React 18’s Suspense feature when fetching data with your GraphQL queries? Try our new` useSuspenseQuery` hook today with[Apollo Client 3.8.0-alpha.5](https://www.npmjs.com/package/@apollo/client/v/3.8.0-alpha.5) . Install it with` npm i @apollo/client@alpha` , then check out[the RFC](https://github.com/apollographql/apollo-client/issues/10231) for more details. We can’t wait to hear your feedback!


## Rapid roundup


Here are a few more changes and version updates you might want to check out:


- [Apollo Server 4.3.1 was released with a few bug fixes](https://github.com/apollographql/apollo-server/releases/tag/%40apollo%2Fserver%404.3.1)
- [Apollo Kotlin 3.7.4 shipped with some minor improvements](https://github.com/apollographql/apollo-kotlin/releases/tag/v3.7.4)
- [Apollo Client 3.7.4 squashed some bugs](https://github.com/apollographql/apollo-client/releases/tag/v3.7.4)
- [Apollo iOS 1.0.6 addressed some community requests and bugs](https://github.com/apollographql/apollo-ios/releases/tag/1.0.6)
- [apollo-compiler 0.6.0 brings API enhancements to your Rust code](https://github.com/apollographql/apollo-rs/releases/tag/apollo-compiler%400.6.0)
- All GraphOS[cloud supergraphs](https://www.apollographql.com/docs/graphos/routing/cloud) have been updated to use version[1.6.0](https://github.com/apollographql/router/releases/tag/v1.6.0) of the Apollo Router
- [We’ve added an FAQ about Relay Connections](https://www.apollographql.com/docs/technotes/TN0029-relay-style-connections/) to our tech notes
- You can now[deploy our “Hack the Supergraph” example](https://github.com/apollographql/hack-the-supergraph#deploy-to-railway-and-graphos) to Railway with just a couple of clicks using a template


## Performance corner


Are you using data loaders in all of your graph’s resolvers? If you aren’t yet, learn all about[handling the N+1 problem](https://www.apollographql.com/docs/federation/entities-advanced/#handling-the-n1-problem) .


That’s all for now! We’ll be back soon with another update. In the meantime,[join our Discord server](https://discord.gg/graphos) to stay up to date with future programs and announcements. That’s also where you can give us feedback on this new format and request changes or additions. Happy querying!


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
