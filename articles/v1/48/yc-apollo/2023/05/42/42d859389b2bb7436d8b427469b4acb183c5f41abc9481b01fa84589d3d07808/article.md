---
schema_version: "1.0.0"
document_id: "42d859389b2bb7436d8b427469b4acb183c5f41abc9481b01fa84589d3d07808"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-changelog-may-26-2023"
published_at: "2023-05-26T15:45:31+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:5e131a2120426b8622be9406374ea0117167a85500d71b888b574736251ab683"
---

# Apollo Changelog: May 26, 2023

## Growing your supergraph


There’s a[fresh new course](https://www.apollographql.com/tutorials/graphos-growing-supergraph) in Odyssey all about expanding an existing supergraph. Add a new subgraph and walk through the composition and query-planning processes. Check it out and let us know what you think!


## Upcoming events


- [Ask the Apollo Client (web) maintainers anything during the next office hours on Tuesday, May 30, at 14:00 UTC.](https://discord.gg/NEPmWUdKn7?event=1109171270769201252) Can’t make it? Drop your question in the` #frontend` channel!


## Frontend news


- Jeff Auriemma published a[community update](https://www.apollographql.com/blog/apollo-client/community-update-apollo-client-apollo-ios-and-apollo-kotlin/) for all Apollo Client users (web, iOS, and Kotlin).
- [Apollo Client (web) 3.7.15 was released](https://github.com/apollographql/apollo-client/releases/tag/v3.7.15) with a couple of bug fixes.
- [Apollo Client (web) 3.8.0 is now in beta!](https://github.com/apollographql/apollo-client/blob/release-3.8/CHANGELOG.md#380-beta0) If you haven’t tried out` useSuspenseQuery` yet, now’s a great time to test it and send us feedback.
- [Apollo Kotlin 3.8.2 brings several fixes](https://github.com/apollographql/apollo-kotlin/releases/tag/v3.8.2) , mainly around websockets and subscriptions.
- [Apollo Kotlin 4.0 is now in alpha!](https://github.com/apollographql/apollo-kotlin/releases/tag/v4.0.0-alpha.1) Test it early to send us your feedback.


## The Router and the Rust


- [Apollo Router 1.19.1 is available](https://github.com/apollographql/router/releases) with improvements to coprocessors, query plan caches, federated fragments, and many more!
- [Apollo Compiler 0.9.2](https://github.com/apollographql/apollo-rs/releases/tag/apollo-compiler%400.9.2) brings quality-of-life improvements for tool-makers and a few bug fixes.


## Community spotlight


There’s been a lot of talk recently about the pros and cons of introspection, and one of the biggest reasons to disable it is to hide features you don’t want users to know about (e.g., before a new product launches). In[a recent blog post](https://engineering.hashnode.com/keep-parts-of-your-graphql-introspection-query-hidden) , Florian Fuchs at HashNode shared how they hide only certain schema pieces from introspection using a custom directive and plugin with Apollo Server. If you’re interested in` @hidden` , be sure to also check out[the @inaccessible](https://www.apollographql.com/docs/federation/federated-types/federated-directives#inaccessible)[Federation directive](https://www.apollographql.com/docs/federation/federated-types/federated-directives#inaccessible) !


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
