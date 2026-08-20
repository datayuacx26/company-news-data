---
schema_version: "1.0.0"
document_id: "6030b63ab559ebcc17a4d075e1122e620b570a86f25e0b3c12cc85ba9099d048"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/accelerating-graph-development-with-subgraph-mirroring-in-the-rover-cli"
published_at: "2024-07-29T09:45:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:acfcd121d60e9bf8e39002c1657ecb5207b6412b8cb048ea85000b87ee3094c2"
---

# Accelerating graph development with subgraph mirroring in the Rover CLI

Today, we’re excited to announce improvements to the GraphOS local development experience. These improvements make it faster for developers to contribute to the supergraph using the[Rover CLI](https://www.apollographql.com/docs/rover/) . Rover provides all the functionality you need to compose schemas, test changes locally, and publish schemas to the GraphOS schema registry. And with today’s release, testing changes locally gets even faster.


### **Get started locally in seconds with subgraph mirroring**


We heard from customers that they wanted to make it faster for new contributors to get up and running with a copy of their supergraph locally.[Rover dev](https://www.apollographql.com/docs/rover/commands/dev/) is the CLI command that enables developers to spin up a local router instance and test changes to their graph; and now it supports[subgraph mirroring](https://www.apollographql.com/docs/rover/commands/dev/#starting-a-session-from-a-graphos-studio-variant) . Subgraph mirroring makes it easy to bootstrap a locally running supergraph in seconds. Just pull down subgraph routing URLs and schemas from a GraphOS Studio variant to your rover dev session, and get started.


Previously, you had to manually[generate](https://github.com/apollosolutions/supergraph-yaml-generator) and maintain a supergraph config that contained all your routing URLs and subgraph schemas. For teams maintaining supergraphs with dozens or hundreds of subgraphs, rover dev with graph ref support drastically reduces time spent getting up and running locally. As an additional perk, subgraph mirroring saves subgraph contributors who aren’t Federation experts from wasting time writing or debugging supergraph configs.


To start a local router instance using a GraphOS Studio variant, include the variant’s graph ref with the –graph-ref option like so:


```text
rover dev --graph-ref platform@staging
```


### **Quickly test subgraph changes before publishing**


Subgraph mirroring is particularly useful when testing subgraph changes or adding new subgraphs altogether. The –graph-ref option also supports passing a supergraph config so you can override specific subgraphs or test new Federation versions. For example, if you’re moving a type or modifying an entity, you may want to test these schema changes locally before publishing them. Now, you can maintain a simpler supergraph config with just the subgraphs you want to override. In the example below, we can override the movies subgraph for your theater booking service:


```text
subgraphs:
movies:
routing_url: http://localhost:4000
schema:
file: ./movies.graphql
```


And then with the command:


```text
rover dev --graph-ref platform@staging --supergraph-config subgraphs.yaml
```


### **Wait, there’s more!**


Alongside subgraph mirroring, we’ve been hard at work increasing the release cadence for Rover and have tackled some long standing customer requests. In the previous v0.24 release, we shipped a host of quality-of-life improvements like more helpful linting messages, better support for router customizations, native Apple Silicon binaries, a bug fix for managing your Rover installation with pnpm/npm, and[several other fixes](https://github.com/apollographql/rover/pull/1973) .


Subgraph mirroring brings Studio and Rover closer together, and we’re working on other major improvements to the Federation contributor experience across all of Apollo’s local development tools including Sandbox, Explorer, and Rover. We’d love tohear from you if you have any feedback on your experience developing a federated supergraph.


Happy coding!


Written by


Jack Sellwood


[Read more by Jack Sellwood](https://www.apollographql.com/blog/author/jacksellwood)
