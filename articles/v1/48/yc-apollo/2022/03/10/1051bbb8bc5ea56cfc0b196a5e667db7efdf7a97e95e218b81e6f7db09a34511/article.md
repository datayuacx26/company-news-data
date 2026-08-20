---
schema_version: "1.0.0"
document_id: "1051bbb8bc5ea56cfc0b196a5e667db7efdf7a97e95e218b81e6f7db09a34511"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-router-our-new-high-performance-federation-runtime-is-now-available-in-open-preview"
published_at: "2022-03-24T13:02:31+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:27ccef22f5d6e0be0663eaac4f064c7c40e9fd41e7cd17ee5a23abc87e5ea40c"
---

# Apollo Router: our new high-performance Federation runtime is now available in open preview

At last year’s GraphQL Summit we announced the alpha release of[Apollo Router](https://www.apollographql.com/blog/announcement/backend/apollo-router-our-graphql-federation-runtime-in-rust/) for the first time – our new, high-performance Federation runtime written in Rust. Today, we’re excited to bring Apollo Router out of Alpha and into an open preview!


## What does the Apollo Router preview mean for you?


If you’re currently running a federated supergraph with Apollo Gateway, Apollo Router gives you a leaner, high-performance runtime option for Federation (both Federation 1 and[Federation 2](https://www.apollographql.com/blog/apollo-federation-2-and-gateway-2-0-are-now-available-in-preview) !). Here are a few things to know about the preview release:


- Benchmarks show that Apollo Router is **10x faster** , has **10x higher throughput** , and has **12x less variance** compared to the latest release of Apollo Gateway.
- Apollo Router is ready for use with limited production workloads and proof-of-concepts.
- You can deploy Apollo Router as a standalone binary, making it easier to manage and scale.
- The preview release adds support for more advanced configuration and customization.


- Simple[YAML configuration](https://www.apollographql.com/docs/router/customizations/overview) for CORS, logging, header propagation, routing URLs, OpenTelemetry, and operation usage reporting to Apollo Studio
- [Native Rust plugins for the Apollo Router](https://www.apollographql.com/docs/router/customizations/native) for maximum flexibility & control.
- [Lightweight Rhai scripting](https://www.apollographql.com/docs/router/customizations/rhai) for string manipulation, custom header forwarding, and more to come as we fully develop this capability.


- Apollo Router offers key integrations with Apollo Studio:


- Automated supergraph rollout via[Managed Federation](https://www.apollographql.com/docs/router/managed-federation/overview) and Apollo Uplink
- Operation-usage & client-awareness reporting via[a new reporting agent called spaceport](https://www.apollographql.com/docs/router/configuration/spaceport)


- Automatic Persisted Queries (APQ) is enabled by default with zero config required and a local, in-memory cache.


- Existing clients that use APQ will work with Apollo Router which stores persisted queries in an in-memory cache.
- For an overview of the APQ protocol and Client configuration see:[https://www.apollographql.com/docs/apollo-server/performance/apq/](https://www.apollographql.com/docs/apollo-server/performance/apq/)


Apollo Router and Gateway response time


## Deploying Apollo Router


You may be wondering, “how do I get started with Apollo Router?” Good news! Unlike Apollo Gateway, Apollo Router is a standalone binary that’s purpose-built for running federated schemas, so it’s easy to deploy and scale. There are just three steps to get started:


1. Download Apollo Router
2. Make sure your project has a supergraph schema
3. Run Apollo Router 🚀


It’s that easy! For the full quickstart guide including downloads and detailed instructions, check out[our docs](https://www.apollographql.com/docs/router/) .


## Apollo Router extensibility


Using our example is a great way to get started, but if you’ve ever run a supergraph in production with Apollo Gateway, you may have encountered a common challenge: how do you extend your gateway to meet your specific needs. The Gateway has a well-documented extensibility model, but you need to know node.js to take full advantage of it.


With Apollo Router, we want to make extensibility more approachable, which is why plugins can be configured with just a \`router.yaml\` file. Apollo Router is written in Rust, but things like propagating headers to subgraphs and OpenTelemetry can be configured with the \`router.yaml\` file without writing any Rust code, with more plugins coming soon.


However, we know that not every use case can be covered with pre-built customizations, so Router also supports lightweight[Rhai scripting](https://www.apollographql.com/docs/router/customizations/rhai) . When YAML configuration is insufficient, Rhai scripting allows you to make programmatic customizations without requiring you to recompile the router with a custom Rust plugin.


## Where to next?


We’re so excited to have the GraphQL community get their hands on the Apollo Router preview release! It’s going to be a game-changer for building high-performance supergraphs that power the world’s most cutting-edge and creative applications.


When you’re using the preview, please don’t hesitate to drop us your questions and feedback in the Apollo Router[GitHub discussion](https://github.com/apollographql/router/discussions) – it will help us put the final touches on Apollo Router before our GA release 🚀


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
