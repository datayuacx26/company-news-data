---
schema_version: "1.0.0"
document_id: "1a42c4f7041a23e73d020f9ec6f3b2a3875d58835b1b3916d52cb5169e4f3626"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/powerful-new-graphql-tools-for-apollo-federation"
published_at: "2021-04-06T08:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:c1b504dab7ae0142adb95f68cc47e775ef4787607dc25eb9bf78ced998b3b8bc"
---

# Powerful new GraphQL tools for Apollo Federation

It’s been two years since we launched Apollo Federation. What started as a spec has blossomed into a community of developers who share a desire to bring the joy of GraphQL to more teams at their company. Over 12 community projects in 9 languages—along with multiple database vendors—have implemented federation as the industry standard for composing graphs together.


Today, we’re excited to announce some exciting new (free!) Apollo tools to help you and your teammates use federation to its fullest:


- **Improvements in**[Apollo Studio](https://studio.apollographql.com/signup?referrer=blog) around navigation and management of federated graphs
- [Rover](https://www.apollographql.com/docs/rover/) , our new open-source Rust CLI for interacting with graphs and schemas
- **[Workbench](https://github.com/apollographql/apollo-workbench-vscode)** , an open-source VS Code extension for designing and growing your federated schemas


Tune in to the[GraphQL Summit](https://summit.graphql.com/) keynote tomorrow, **7 April at 9:00 AM PT** , to see demos of these tools, plus plenty more exciting news from Apollo!


## **First, what** ***is*** **federation?**


Federation is an architecture for **composing** two or more graphs (called **subgraphs** ) into a single data graph (called a **supergraph** ). A supergraph’s schema is made up of all the composed subgraph schemas, plus some metadata. Subgraph and supergraph schemas all adhere to the GraphQL spec, so they’re all valid GraphQL documents.


[Getting started with federation](https://www.apollographql.com/docs/federation/quickstart) doesn’t require any changes to your subgraphs. Once you’ve composed two subgraphs together, you can even extend your types across subgraphs.


Unlike schema stitching, which relies on imperative, manual code to connect graphs, federation is declarative. Federation’s declarative composition model produces a single, static artifact representing your supergraph. This artifact can power helpful workflows like validating breaking changes while you develop your subgraph.


## **Developing a federated graph**


Whether you’re working with 2 subgraphs or 50 subgraphs, you need a source of truth to coordinate changes to the supergraph. That’s where the Apollo registry comes in! By pushing each subgraph schema to the registry during CI/CD, you can ensure that your editor, graph browser, and terminal are always synced to the most up-to-date supergraph.


There are three stages in the federation development lifecycle: design, implementation, and consumption. We want each of these stages to be as delightful and fast as possible, so we’ve invested in tooling to help you every step of the way.


### **Implementing a federated graph with Rover**


[Rover](https://www.apollographql.com/docs/rover/) is our new CLI for interacting with graphs and the Apollo Studio registry. It’s also our[first open-source project written in Rust](https://github.com/apollographql/rover) 🦀, which we’re excited to build more with in the future.


What’s the difference between Rover and its predecessor, the Apollo CLI? For starters, Rover is built as a single static binary. Unlike the current Apollo CLI written in TypeScript, this means you can run Rover in any language server, CI/CD pipeline, or DevOps environment.


With Rover, you can[build your first federated graph](https://www.apollographql.com/docs/rover/supergraphs/) by running` rover supergraph compose` in your terminal, passing in a config file listing where to find your running subgraphs. Then, you can pipe the resulting schema into another command, print it to a local` .graphql` file, or[pass it to @apollo/gateway](https://www.apollographql.com/docs/federation/quickstart) (this feature is experimental). Running composition locally shortens the feedback loop between making subgraph changes and seeing how they impact the supergraph, which speeds up implementation considerably.


You can[publish your subgraph to the Apollo registry](https://www.apollographql.com/docs/rover/subgraphs/#publishing-a-subgraph-schema-to-apollo-studio) by running` rover subgraph publish` in your CI/CD pipeline. Before you[publish subgraph schema changes](https://www.apollographql.com/docs/rover/subgraphs/#publishing-a-subgraph-schema-to-apollo-studio) , you can even validate them with` rover subgraph check` to confirm that you aren’t introducing breaking changes to your application clients (this requires an Apollo Studio Team plan).


Rover is still in public preview, so these features are constantly evolving. We appreciate any help testing it out, as well as your[feedback in GitHub issues](https://github.com/apollographql/rover) !


### **Free Apollo Studio features for browsing and testing federated graphs**


Once your federated graph grows beyond 2 subgraphs, it’s difficult to know which subgraph is responsible for each type or field. Figuring out who to contact if there’s an issue with a subgraph is also challenging, especially when we’re all working remotely.


Knowing what’s in your graph and who built it makes your graph more discoverable and prevents duplication of work. We’re all about saving you time and reducing code, so we built federation awareness into[Apollo Studio](https://studio.apollographql.com/signup?referrer=blog) .


To take advantage of these new features, push your federated graph to the registry with Rover or the Apollo CLI. Then, you can[optionally integrate the @contact directive](https://www.apollographql.com/docs/studio/federated-graphs/#adding-the-contact-directive-to-your-subgraph) to assign an owner for each subgraph.


We’ve also improved the[Apollo Studio Explorer](https://www.apollographql.com/docs/studio/explorer/) , which is the only federation-aware GraphQL IDE for development and production graphs. In the Explorer, you can inspect query plans to learn how your supergraph fetches data. Turn on hints to see which subgraph each field comes from as you’re building your query.


### **Designing a federated graph with Workbench**


Once you’ve successfully composed two subgraphs, you can start thinking about extending their types and linking fields between them.[Our new VS Code extension Workbench](https://github.com/apollographql/apollo-workbench-vscode) can help you design a federated graph from the comfort of your editor without changing code or running servers.


Our Solutions team built Workbench after learning from our enterprise customers that they started designing their schemas on whiteboards, in shared documents, or even on blank sheets of paper. Those representations can quickly get out of date when a subgraph changes, so we decided to build an experience around the Apollo registry that syncs your editor to your graph. Workbench runs composition in your editor, so you can preview how subgraph changes will affect your supergraph without running any servers.


Workbench is still experimental, but[we’d love to hear your feedback](https://github.com/apollographql/apollo-workbench-vscode) . Let us know how we can improve how you design your schema.


## **Get started with federation! 🚀**


From design to implementation to consumption, we hope that Apollo Studio, Rover, and Workbench will make your entire GraphQL federation experience delightful. For demos of these tools,[tune into the GraphQL Summit keynote](https://summit.graphql.com/) tomorrow, 7 April at 9:00 AM PT. If you’d like to dive in yourself, we’ve put together a[new federation tutorial](https://www.apollographql.com/docs/federation/quickstart/) to help you along your journey.


Even if you’re not ready to start federating, we encourage you to[push your graph to the Apollo registry](https://www.apollographql.com/docs/rover/graphs/#publishing-a-schema-to-apollo-studio) (it’s free!) so you can take advantage of the Apollo Studio Explorer and Rover. Using the registry as your source of truth will allow you to turn off introspection in production, which is important for maintaining the security and privacy of your graph.


Happy federating!


Written by


Peggy Rayzis


[Read more by Peggy Rayzis](https://www.apollographql.com/blog/author/peggy)
