---
schema_version: "1.0.0"
document_id: "274fddd2e424696073d647f43a6aacc4066c77faa82b35a88ed8f53194af5e94"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-changelog-may-5-2023"
published_at: "2023-05-05T17:34:27+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:198095dc284fd44f360595c5bd0f69e30442cc751619f94b6d287a293f785585"
---

# Apollo Changelog: May 5, 2023

## Rapid router releases


Since the last changelog, three new versions of Apollo Router have been released! We can’t cover all the changes here, so check out[the release notes](https://github.com/apollographql/router/releases) for more details.


One exciting addition is a new[preview](https://www.apollographql.com/docs/resources/product-launch-stages/#preview) feature for Enterprise users:[operation](https://www.apollographql.com/docs/router/configuration/operation-limits) limits. You can now configure your router to reject queries exceeding certain complexity metrics. Individual subgraphs have always been able to enforce limits, but now the router can protect an entire supergraph, making defense against denial-of-service attacks easier than ever! If you want to know more, there’s also a[dedicated blog post](https://www.apollographql.com/blog/announcement/backend/prevent-graph-misuse-with-operation-size-and-complexity-limits/) about this feature.


## How GraphQL & Apollo help drive Reddit’s mission


After 4 years of GraphQL successfully powering the iOS home feed, the gateway eventually became unwieldy. So Reddit turned to federation. Once a federated graph was in place, Reddit could build services in individual subgraphs, allowing engineers to work in unison but independently. And inevitably opening the doors to some exciting projects…


## Office hours


We kicked off a new series called “office hours”, where community questions are answered directly by Apollonauts. Our first stream focused on Apollo iOS, featuring both the engineering manager (Jeff Auriemma) and one of the maintainers (Anthony Miller), and you can now[watch the recording](https://www.apollographql.com/blog/mobile/ios/office-hours-apollo-ios/) !


The next stream will be all about Apollo Kotlin on May 10, 13:30 UTC—[join the Discord server](https://discord.gg/NEPmWUdKn7) now so you don’t miss it! Which topics or products would *you* like us to cover in office hours? Let us know in Discord!


## Rapid roundup


- [Apollo Client 3.7.13](https://github.com/apollographql/apollo-client/releases/tag/v3.7.13) and[3.7.14](https://github.com/apollographql/apollo-client/releases/tag/v3.7.14) are available with some bug fixes and improvements
- Apollo Server[4.7.1](https://github.com/apollographql/apollo-server/releases/tag/%40apollo%2Fserver%404.7.1) fixes the landing page for Safari
- Apollo iOS[1.1.3](https://github.com/apollographql/apollo-ios/releases/tag/1.1.3) is out with several bug fixes
- ` @apollo/datasource-rest`[6.0.0](https://github.com/apollographql/datasource-rest/releases/tag/v6.0.0) drops support for Node 14
- [Announcing the end-of-life schedule for Fed 1 in Studio](https://www.apollographql.com/blog/announcement/backend/announcing-the-end-of-life-schedule-for-fed-1-in-studio/)
- There is a new “unnamed operations” filter in the “operations” section of Studio, for quickly identifying anonymous operations
- Serverless cloud routers are now running version 1.15.1, incorporating some small improvements and fixes


## Community spotlight


In honor of May the Fourth, Michael Solati[wrote about creating a new Star Wars-themed GraphQL API](https://dev.to/amplication/may-the-fourth-be-with-you-1d90) leveraging all sorts of interesting tools (like ChatGPT). I’m a big fan of generating boilerplate code (see[our subgraph templates](https://www.apollographql.com/docs/graphos/graphs/creating-a-subgraph/#starting-from-a-template) ), so I’m excited to see where Amplification goes next!


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
