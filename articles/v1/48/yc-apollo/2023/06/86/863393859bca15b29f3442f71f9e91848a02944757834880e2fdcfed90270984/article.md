---
schema_version: "1.0.0"
document_id: "863393859bca15b29f3442f71f9e91848a02944757834880e2fdcfed90270984"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/evaluating-apollo-router-understanding-free-and-open-vs-commercial-features"
published_at: "2023-06-13T11:01:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:124b5056788925af50d59a192a16cc41ffe5e8c96f4c7e4291be7859e44537a0"
---

# Evaluating Apollo Router: Understanding Free and Open vs Commercial Features

Apollo’s largest customers like Indeed and Expedia are embracing a platform engineering approach to GraphQL. This enables individual GraphQL API teams to publish changes to a schema in a self-service fashion without manual review. A build pipeline composes each team-level schema into a unified supergraph with policies that can be enforced by the Router at runtime. A platform approach enables organizations to efficiently scale their graph while providing enhanced security, performance, and governance.


Two of the key components in these platforms are Apollo Federation – an industry standard for API composability, and Apollo Router – the most performant runtime for federated graphs. While many Router features are available for free, some are only available with a GraphOS subscription. We want to make it clear to our community where we stand on our commercialization strategy to ensure you can make an informed decision on when to use each one. We also intend for this process to be a collaborative effort with our community. So we’d like to outline our approach to developing Apollo Router as well as how your organization can use it.


### The path ahead for the free and open Router


Apollo believes in developing in the open. Apollo Router is source available, so you are free to evaluate[Apollo Router’s codebase](https://github.com/apollographql/router) yourself and determine if it is a worthwhile part of your API stack. Once a Router feature has been released as part of our free kit, we are committed to keeping it free, forever. As with Apollo Federation, the core of the Router is also free and open should you want to experiment and contribute. We intend to keep both Federation and the core Router free and open, along with core GraphQL projects like[apollo-rs](https://github.com/apollographql/apollo-rs) , so you have the building blocks for modern APIs and can run supergraphs in production for free.


A lot of customers were under the impression that because the Router was built in Rust, they had to learn to write Rust to use it. To make it easier to deploy and maintain, the Router’s functionality can be[configured in YAML](https://www.apollographql.com/docs/router/configuration/overview#yaml-config-file) . The free Router also provides a well-defined extensibility model with lifecycle hooks that offer parity with Apollo Gateway middleware and plugin hooks beyond its built-in functionality. The Router supports[Rhai scripting](https://www.apollographql.com/docs/router/customizations/rhai) (like Lua scripts in NGINX) for in-memory manipulation of headers, cookies, and Router context. The free Router also supports[native Rust plugins](https://www.apollographql.com/docs/router/customizations/native) for advanced customizations. However, our goal is to provide built-in support for the free features needed to run the Router in production. If you have a common use case that you think should be supported, please[open an issue](https://github.com/apollographql/router/issues) .


### Why Elastic License v2?


The Apollo developer community is at the heart of everything we do. As stewards of our community, we want to continue serving you by funding the development of the Router. The ELv2 license enables us to do that – to remain commercially viable while facilitating open and collaborative development on the project.


### Extending the free and open kit with GraphOS


There is a difference between running Apollo Router in production and relying on it as a vital part of your business. Security, performance, and governance concerns become more complex – and a small platform team often can’t scale effectively as more subgraph teams join the graph. Enforcing policies becomes more complex as well. To remain agile, platform teams often distribute the responsibility for authoring policy to each subgraph team while having the Router enforce them at runtime.


Apollo GraphOS focuses on solving these problems. It’s a turnkey, self-service GraphQL API platform that will support additional defense-in-depth, faster performance at scale, scalable real-time data, faster onboarding of existing data and services, and build-time governance policies that ensure best practices.


- **Supergraph CI** provides an integrated build pipeline with composition, schema checks, and breaking change prevention. Platform teams define build-time policies (custom approval workflows, linting, etc) to ensure best practices are followed while supporting developer self-service.
- **Supergraph CD** safely deploys supergraph updates without downtime to the Router fleets serving your dev, UAT, and prod environments.
- **GraphOS runtime features of Apollo Router** provide improved defense-in-depth, faster performance at scale, scalable real-time data, and the like. Security and caching policies authored by subgraph teams are enforced by the Router at runtime.
- **Graph-native observability & developer decision support** help you understand graph performance, field usage by specific app versions, and the impact of proposed schema changes.


While you could build, maintain, and operate all of this yourself on top of the free building blocks we provide in Federation and the Router, it’s a lot of undifferentiated heavy lifting. We think GraphOS will provide a compelling value for most users, especially as we roll out these GraphOS features across our paid plans (Serverless, Dedicated, and Enterprise) to ensure there’s a suitable pricing model for all organizations — with Apollo standing behind the fully integrated stack.


### The Future of Apollo Router


Apollo Router serves billions of operations a day across applications used by millions of people around the world. We also intend to earn our customers’ business every day with compelling features in GraphOS, built on top of our free kit. Our goal is to be fully transparent about the differences between our free and paid kit, and we would like to involve our community as much as possible to shape the future of Apollo Router. Apollo aims to be open about this process. We encourage you to[ask any questions or surface any concerns directly with our team in our #apollo-router Discord channel](https://discord.gg/graphos) . We’ll be publishing more information in the coming months about graph routing, and look forward to going on this journey with you, together.


Written by


Phil Prasek


[Read more by Phil Prasek](https://www.apollographql.com/blog/author/philprasek)
