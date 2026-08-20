---
schema_version: "1.0.0"
document_id: "96b3ec239c15ff14838b3c75bbbb15f6f5c80a4dd796f4c522691275ada6f8b2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-changelog-jan-30-2023"
published_at: "2023-01-30T17:15:07+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:7714807c1b6a497ab6ca554ff171286e341ded837fbe7c6f3afe6fd44a03c8b2"
---

# Apollo Changelog: Jan 30, 2023

Welcome to our second installment of the Apollo Changelog! We’ve got a ton of great news this week, so let’s dive straight in.


## Retail ❤️ GraphOS


Retail companies of all sorts face similar technical challenges around topics like checkout, product availability, and personalization. We’ve observed with many retail organizations (including some *very* large ones!) that GraphOS is perfectly suited to helping wrangle these different concerns in a consistent, integrated way.


To share everything we’ve learned over the years, we’ve created a dedicated[GraphOS for Retail page](https://www.apollographql.com/solutions/retail) . Our solutions architects even designed an example supergraph schema you can start with when building your next graph! Check it out, and let us know which types of businesses you’d like us to cover next.


## Making space for learning


Recently, the[SpaceX GraphQL API](https://github.com/SpaceXLand/api) was shut down due to the upstream data project being[retired](https://github.com/r-spacex/SpaceX-API/issues/1243) . This API was a fantastic learning tool used by countless tutorials, demonstrations, and blog posts. We didn’t want the community to lose access to such a helpful resource, so we recreated it using a different data source!


Check out a brand-new API implementation in[Apollo Studio](https://studio.apollographql.com/public/spacex-l4uc6p) and query it to your heart’s content! We’re working on getting the original API’s endpoint updated to point to this *new* API so that existing resources will continue to work. In the meantime, feel free to point any new content to this project.


## Help us build our next course


We’re building a new course in Odyssey called[“Getting started with GraphOS“,](https://www.apollographql.com/tutorials/getting-started-with-graphos) and we’d love your feedback. This course will become the definitive guide to learning about GraphOS, but it’s currently in beta while we finalize the content. Try it out today, and let us know what you think!


## Easier federation tests for library maintainers


Apollo maintains a list of[federation-compatible subgraph implementations](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs) and which federation features they support. This list is generated from an open-source testing project, and maintainers can add their projects to our testing matrix by opening a pull request. Until recently, this was the best way for maintainers to determine if their projects fully support federation. However, it didn’t give them feedback in their CI pipelines if they broke that compatibility, and it required multiple steps to re-test implementations when building out new features.


[Now, there is a CLI tool that can test libraries for federation compatibility](https://www.apollographql.com/blog/backend/testing/new-apollo-federation-spec-compliance-tools-for-graphql-server-maintainers/) , which should make implementing and maintaining federation capabilities much more straightforward!


## Manage contracts programmatically with Rover


Rover[0.11.0](https://github.com/apollographql/rover/releases/tag/v0.11.0) arrived this week with an exciting new feature: you can now view, create, and update[Apollo GraphOS contracts](https://www.apollographql.com/docs/graphos/delivery/contracts/) from the command line. This means it’s faster than ever to provide a limited subset of your schema to a group of consumers! This version also ships with some improvements to` rover dev` and output formats, so make sure to[check the full release notes](https://github.com/apollographql/rover/releases/tag/v0.11.0) .


By the way, did you know that you can[bootstrap new GraphQL services from Rover](https://www.apollographql.com/docs/rover/commands/template) ? It’s the easiest way to get started with a new subgraph!


## Router updates: Prometheus in plugins, persisted queries, and self-signed subgraph certificates


[Apollo Router 1.9.0](https://github.com/apollographql/router/releases/tag/v1.9.0) brings a host of new capabilities to your self-hosted deployments. If you need a self-signed TLS certificate or a custom certificate authority to communicate with your subgraphs, it’s now possible within[the Router’s config](https://www.apollographql.com/docs/router/configuration/overview/#tls) . While you’re there, you can also enable support for[automatic persisted queries](https://www.apollographql.com/docs/router/configuration/overview/#automatic-persisted-queries-apq) —an important feature for large queries on slow networks. If you’re building custom Rust plugins for your Router, you should also check out our[new guide on custom Prometheus metrics](https://www.apollographql.com/docs/router/customizations/native#add-custom-metrics) ! This new release of Router comes with many more improvements than we could fit here, so stop by[the release page](https://github.com/apollographql/router/releases/tag/v1.9.0) for all the details.


## Rapid roundup


- Michael Watson recently discussed query and mutation best practices in a livestream on our[Discord server](https://discord.gg/graphos) . Join the community to make sure you’re in the loop for future presentations!
- [Apollo Client 3.7.5](https://github.com/apollographql/apollo-client/releases/tag/v3.7.5) is out with a handful of improvements.
- [Apollo Server 4.3.2](https://github.com/apollographql/apollo-server/releases/tag/%40apollo%2Fserver%404.3.2) solves some dependency issues and improves TypeScript checking of` HeaderMap` .
- You can now deploy[every GraphOS template](https://www.apollographql.com/docs/graphos/graphs/creating-a-subgraph#starting-from-a-template) to[Railway](https://railway.app/) with no additional configuration.


## Upcoming events


On Wednesday, February 1st, we’ll present the first in a new series of livestreams in[Discord](https://discord.gg/graphos) . We’re investigating hosting GraphQL subgraphs in serverless environments—starting with Python and[Strawberry](https://strawberry.rocks/) ! Do you have a suggestion for a language or environment you’d like to see us implement?[Let us know!](https://discord.gg/graphos)


## Community spotlight


As part of these Changelog updates, we want to highlight some of the amazing contributions from the Apollo community! If you create or come across a helpful resource like a blog post, open-source library, or conference talk, drop a link in[our Discord](https://discord.gg/graphos) . We’ll help get the word out right here.


To kick things off, Yuleidy González wrote about[“GraphQL Pagination”](https://medium.com/prodigy-engineering/graphql-pagination-18568c6300e7) when your backing data source is a REST API. She describes the difference between “shallow” and “deep” pagination, and why understanding the difference is important.


For the front-end folks, there’s a blog post from Joosep Alviste titled[“Improving the user experience of page navigation with Apollo Client”](https://medium.com/scoro-engineering/improving-the-user-experience-of-page-navigation-with-apollo-client-ebfca0e83c08) . Joosep dives into the common problem of reusing cached values from different queries. It’s worth the read if you’re working with Apollo Client, and make sure to check out[our docs on caching](https://www.apollographql.com/docs/react/caching/advanced-topics/#cache-redirects) for more info!


That’s all for now—stay tuned for more updates!


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
