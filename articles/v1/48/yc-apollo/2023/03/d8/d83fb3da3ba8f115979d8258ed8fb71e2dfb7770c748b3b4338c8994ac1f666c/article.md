---
schema_version: "1.0.0"
document_id: "d83fb3da3ba8f115979d8258ed8fb71e2dfb7770c748b3b4338c8994ac1f666c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-changelog-march-6-2023"
published_at: "2023-03-06T19:38:51+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:18dd0848041693a04dc69d4bd2e90124acf942bbe5df2fe086e6dc1a226c2a6e"
---

# Apollo Changelog: March 6, 2023

Welcome back to the Apollo Changelog. After taking a week off, we’re back with a fresh batch of news for you!


## A glimpse into Apollo’s monorepo


Ever wonder how GraphOS works under the hood? A big chunk of it lives in a monorepo that Apollo engineer Ryan Wilcox has shared details about in a recent blog post. Check out[“How we deploy the Apollo GraphOS monorepo”](https://www.apollographql.com/blog/engineering/how-we-deploy-the-apollo-graphos-monorepo/) to learn how we use CircleCI, Kotlin, Terraform, and more!


## More subgraphs in more languages


If you missed our recent “Building a Subgraph in Rust” stream, don’t worry, the series continues later this month with “Building a Subgraph in TypeScript with Apollo Server”. Join[our Discord server](https://discord.gg/graphos) so that you don’t miss any upcoming streams. Not only that, but you can request which libraries and languages *you’d* like to see in future streams!


## Updating your graph safely


A supergraph includes many moving parts, and it can be tricky to figure out how best to update those parts. That’s why we added[a new page to our docs](https://www.apollographql.com/docs/graphos/graphs/updating/) specifically to help you decide where and when to make changes. This is recommended reading for anyone who operates a router, even if it’s running via GraphOS Serverless!


## A mountain of new videos


We’ve been regularly releasing GraphQL Summit talk summaries on our YouTube channel, which you should subscribe to if you haven’t yet! Since our last changelog, we’ve added three more—each under 10 minutes:


### Streamlining how you shop: Walmart and GraphQL – Aditya Bakle & Ben Wilson


### Write better queries & mutations – Erik Bylund


### GraphQL: everything you want in one query – Matt Debergalis


## Handling null in Kotlin clients


If you’re consuming GraphQL APIs from Kotlin, take a look at a new blog post titled “[Nullability in GraphQL and Apollo Kotlin](https://www.apollographql.com/blog/mobile/kotlin/nullability-in-graphql-and-apollo-kotlin/) ”. Learn all of the best tricks for handling optional fields from one of the maintainers!


## Rapid roundup


- In the Apollo Studio Explorer, you can now more easily identify entities and create` @defer` fragments. Click the megaphone icon in the top right of Studio for more details on what’s new.
- Apollo Client[3.7.10](https://github.com/apollographql/apollo-client/releases/tag/v3.7.10) has a couple of improvements to` useMutation` ,` writeQuery` , and` writeFragment` .
- Rover[0.12.2](https://github.com/apollographql/rover/releases/tag/v0.12.2) now recommends that all users of` rover supergraph compose` pin their federation version.
- Apollo Router[1.11.0](https://github.com/apollographql/router/releases/tag/v1.11.0) brings a variety of improvements around Rhai scripting and deployments, mostly targeted at self-hosted users.
- Subgraph compatibility tests version[1.2.0](https://github.com/apollographql/apollo-federation-subgraph-compatibility/releases/tag/1.2.0) updates to the latest Router and Rover, as well as bringing some extra diagnostics to maintainers when tests fail.


## Community spotlight


Volvo has started a blog series about their adventures with Apollo Federation! Iman Radjavi kicked it off with[“Why Volvo Car Mobility use Apollo Federation”](https://medium.com/volvo-car-mobility-tech/why-volvo-car-mobility-use-apollo-federation-59c647a2ab60) and Christopher Gustafson quickly followed up with[“Working with entities in Apollo Federation”](https://medium.com/volvo-car-mobility-tech/working-with-entities-in-graphql-federation-a88e9f9865b2) . Check out both posts to learn more about what Volvo is working on.


Several more subgraph-compatible GraphQL servers are now being validated to support` @composeDirective` and` @interfaceObject` —the latest Federation features. Our latest[subgraph compatibility results](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs) show that GraphQL Yoga, GraphQL Helix, and NestJS all support these new directives!


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
