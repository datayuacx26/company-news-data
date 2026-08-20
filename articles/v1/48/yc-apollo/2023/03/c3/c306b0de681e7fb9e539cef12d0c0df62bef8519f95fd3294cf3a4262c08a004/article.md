---
schema_version: "1.0.0"
document_id: "c306b0de681e7fb9e539cef12d0c0df62bef8519f95fd3294cf3a4262c08a004"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-changelog-march-20-2023"
published_at: "2023-03-20T16:42:32+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:d4d03d6e9868998fb24028735b39edcb4b8c632591e8ff8b525ef121b9291838"
---

# Apollo Changelog: March 20, 2023

## Coprocessing, distributed caching, and JWT auth in the router


[Apollo Router 1.12](https://github.com/apollographql/router/releases/tag/v1.12.0) comes with several exciting new features for GraphOS Enterprise. You can now extend the router with any programming language via the new “coprocessors” feature. Your router can also cache automatic persisted queries in Redis *and* validate JSON web tokens before calling your subgraphs. For the full details, check out[the announcement blog post](https://www.apollographql.com/blog/announcement/backend/apollo-router-v1-12-improved-router-security-performance-and-extensibility/) .


## Last call for proposals


Have you submitted your talk for GraphQL Summit 2023 yet? If not, there’s only one week left to[send in your proposal](https://summit.graphql.com/event/c51538f6-4b76-44e3-871e-54180c77cad8/websitePage:ee8a1bc8-1286-4106-b34b-f28798e56f93) . Everyone is welcome to speak, regardless of background, even if this will be your first conference talk! Conference tickets, lodging, and airfare are all covered for speakers, so all you have to think about is what you want to share with the GraphQL community.


## Introducing entity interfaces


Lenny Burdette, a Principal Solutions Architect at Apollo, has created a short video introducing the newest federation feature: entity interfaces.


## Rapid roundup


- Blog post:[“How to use Apollo Client with Next.js 13”](https://www.apollographql.com/blog/apollo-client/how-to-use-apollo-client-with-next-js-13/)
- Find entities in Studio faster with a new icon in the schema reference tab
- [Apollo Kotlin 3.7.5](https://github.com/apollographql/apollo-kotlin/releases/tag/v3.7.5) comes with a bunch of bug fixes as well as some hints at version 4.0
- [@apollo/gateway](https://github.com/apollographql/federation/releases/tag/%40apollo%2Fgateway%402.4.0) and` <a href="https://github.com/apollographql/federation/releases/tag/%40apollo%2Fsubgraph%402.4.0" target="_blank" rel="noreferrer noopener">@apollo/subgraph</a>` 2.4.0 include performance improvements to query planning
- The[Apollo Kotlin tutorial](https://www.apollographql.com/docs/kotlin/tutorial/00-introduction/) has been updated to use Jetpack Compose
- There is now a[changelog for the Federation spec](https://www.apollographql.com/docs/federation/federation-versions/) , making it easier than ever to find out about new features


## Community spotlight


There have been several recent releases to` <a rel="noreferrer noopener" href="https://github.com/Gusto/apollo-federation-ruby" target="_blank">apollo-federation-ruby</a>` adding support for more federation features. If you’ve been waiting on` @tag` ,` @inaccessible` , or` @interfaceObject` in your Ruby subgraph, it’s time to update!


Last week, we missed a piece of the Volvo Car Mobility story, but no worries, this knowledge is evergreen! The N+1 problem requires care to avoid in every type of API. In GraphQL, this is typically solved by adding data loaders to your resolvers—but it can be easy to forget that even resolvers returning a single object should use data loaders.[“Using Data Loaders with Apollo Federation”](https://medium.com/volvo-car-mobility-tech/using-data-loaders-with-apollo-federation-f21260e3abd1) discusses why data loaders are important in entity resolvers and how you can implement them in Kotlin.


Have you seen a cool blog post, video, or open-source project related to GraphQL? Let us know[in our Discord server](https://discord.gg/graphos) so we can include it in an upcoming community spotlight!


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
