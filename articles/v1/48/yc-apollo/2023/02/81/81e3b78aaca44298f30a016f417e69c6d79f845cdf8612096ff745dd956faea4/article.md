---
schema_version: "1.0.0"
document_id: "81e3b78aaca44298f30a016f417e69c6d79f845cdf8612096ff745dd956faea4"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-changelog-feb-20-2023"
published_at: "2023-02-20T17:26:52+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:07296991863edc559bfb440d054d916a95e2465d2e850af019a0bd084ff28957"
---

# Apollo Changelog: Feb 20, 2023

## Federated interfaces


Last week, we shipped Federation 2.3, which adds the new` @interfaceObject` directive, enabling you to federate an interface *without* every subgraph knowing about every concrete implementation. Check out the[announcement post](https://www.apollographql.com/blog/announcement/platform/apollo-federation-and-graphos-add-support-for-entity-interfaces-to-streamline-collaboration/) for more details, and start upgrading your interfaces today!


## Subgraph and variant management in Studio


You can now manage even more of your supergraph in[Apollo Studio](https://studio.apollographql.com/) . The new Subgraphs page gives you a high-level view of all the subgraphs that make up your supergraph, along with a UI for adding and removing subgraphs!


You can now add or remove variants from Studio within graph settings.


## Time to update your subgraphs


Version[1.1.0](https://github.com/apollographql/apollo-federation-subgraph-compatibility/releases/tag/1.1.0) of the[subgraph compatibility tests](https://github.com/apollographql/apollo-federation-subgraph-compatibility) adds checks for` @composeDirective` and` @interfaceObject` . The[subgraph compatibility page](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs) has been updated to reflect the status of every subgraph implementation. If your favorite library doesn’t support these features yet, consider contributing them! If you need any help, reach out to us[in Discord](https://discord.gg/graphos) .


The first community library to support these features is` graphql-kotlin` with its[6.4.0 release](https://github.com/ExpediaGroup/graphql-kotlin/releases/tag/6.4.0) ! Strawberry (Python) followed closely behind with its[0.157.0 release](https://github.com/strawberry-graphql/strawberry/releases/tag/0.157.0) . Who will be next?


## Is GraphQL a trap?


Our series of condensed GraphQL Summit talks continues with “Is GraphQL a trap?” Sasha Solomon tackles this question head-on by showing us where GraphQL shines.


## Entity ownership


Federation 2.0 removed the requirement for one subgraph to “own” an entity and for others to “extend” it. Instead, each subgraph contributes to the entity, making them truly distributed. However, there are some situations where ownership is desirable, so our solutions architects created[a tech note](https://www.apollographql.com/docs/technotes/TN0036-owner-pattern/) to address this situation!


## Rapid roundup


- Apollo Client[3.7.8](https://github.com/apollographql/apollo-client/releases/tag/v3.7.8) and[3.7.9](https://github.com/apollographql/apollo-client/releases/tag/v3.7.9) are out with a variety of bug fixes.
- Apollo Gateway[2.3.2](https://github.com/apollographql/federation/releases/tag/%40apollo%2Fgateway%402.3.2) fixes a breaking change that was introduced in 2.3.0.
- Apollo Server[4.4.0](https://github.com/apollographql/apollo-server/releases/tag/%40apollo%2Fserver%404.4.0) adds the “Include Cookies” toggle in the embedded sandbox.
- Version 0.5.0 of both[apollo-parser](https://github.com/apollographql/apollo-rs/releases/tag/apollo-parser%400.5.0) and[apollo-encoder](https://github.com/apollographql/apollo-rs/releases/tag/apollo-encoder%400.5.0) were released with some improvements and fixes.
- Apollo Router[1.10.3](https://github.com/apollographql/router/releases/tag/v1.10.3) improves per-type metrics from subgraphs that support federated tracing.
- Our[homepage](https://www.apollographql.com/) was recently refreshed to better explain all that we do. What’s your favorite Apollo product or feature? Let us know[in Discord](https://discord.gg/graphos) .
- We released a[new video](https://www.youtube.com/watch?v=FdlAucR6ccs) to help introduce GraphOS. Learn the basics of connecting a graph in just 3 minutes!
- Rover[0.12.0](https://github.com/apollographql/rover/releases/tag/v0.12.0) was released with support for` subgraph check` without operation checks


## Community spotlight


Last week,[Graphiti 1.7.0](https://github.com/GraphQLSwift/Graphiti/releases/tag/1.7.0) was released, adding Apollo Federation support! This is the first server-side Swift library to support Federation, meaning trying out Swift on the server has never been easier. Adding a new subgraph to your supergraph is a low-risk way to try out a new language.


## Upcoming events


Join us this Wednesday, February 22, for a livestream in[our Discord](https://discord.gg/graphos) about implementing subgraphs in Rust. Make sure to bring your burning questions about Rust, as we’ll have dedicated time for Q&A at the end.


Our next Changelog post will be on March 6, we’ll see you then!


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
