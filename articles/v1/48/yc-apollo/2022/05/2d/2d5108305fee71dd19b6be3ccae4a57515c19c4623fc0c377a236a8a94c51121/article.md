---
schema_version: "1.0.0"
document_id: "2d5108305fee71dd19b6be3ccae4a57515c19c4623fc0c377a236a8a94c51121"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-router-is-now-generally-available"
published_at: "2022-05-18T08:55:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:56d255f6ce35c5f3cce5b530d7e466e6dc78c8318dbc4669a586cb1e029dc352"
---

# Apollo Router is now generally available

Apollo Router, the next-generation runtime for our[supergraph stack](https://apollographql.com/blog/the-supergraph-a-new-way-to-think-about-graphql) , is here, and it’s fully production-ready.


It’s been about six months since we first unveiled Apollo Router at GraphQL Summit. Since then, we’ve been improving it and testing it to ensure that it’s even more efficient, secure, reliable, and customizable. And today, we’re excited to make Apollo Router generally available.


## What is Apollo Router?


Apollo Router is a supergraph runtime written in Rust that supports[Apollo Federation](https://apollographql.com/docs/federation/v2) (v1 or v2) – a declarative GraphQL architecture that lets you build and collaborate on a unified GraphQL API (a supergraph) in a modular and declarative way. The router is what processes all of the incoming API requests from clients and *routes* them to the subgraphs that make up the overall supergraph.


You may be thinking, “doesn’t Apollo Gateway already do that?” It does! Apollo Gateway is our Node.js supergraph runtime, and we will continue to support it well into 2023 and likely beyond. But when it comes to routing API requests, speed is the name of the game, and Apollo Router is FAST ⚡️.


In our benchmarks Apollo Router processed requests **10x faster** than Apollo Gateway with **10x higher throughput** and **90% less variance in latency** . And that’s just the start. We have more improvements coming up on the roadmap that will make it even more efficient.


## The Apollo Router operational experience


While speed was one of our main goals for Apollo Router, it wasn’t our only goal.


### Easier to run


Apollo Router is easier to manage. Unlike Apollo Gateway, Apollo Router ships as a standalone binary. To use it, you simply download and run it with any valid supergraph schema. This makes containerization and scaling a breeze! We even supply[Docker images](https://www.apollographql.com/docs/router/containerization/overview) for every release.


### More native functionality


Apollo Router has a ton of built-in options like CORS controls, header propagation, telemetry,[and more](https://www.apollographql.com/docs/router/configuration/overview) that can all be configured with a declarative YAML file! What’s possible with Router today is just the beginning. In future releases, we will be adding even more native functionality like:


- ` @defer` support
- real-time data
- Studio operation trace reporting
- 1st-class caching
- Authentication and authorization
- Multi-language extensibility (write your own plugins in languages other than Rust)


### Tracing and monitoring


Apollo Router comes with Studio Reporting, Open Telemetry and Prometheus metrics out of the box.


Alerting on errors or high latencies is now possible via:


- Prometheus
- OpenTelemetry Collector (metrics)


In addition, you can diagnose where there are latency issues in your request pipeline by connecting to existing tracing solutions such as:


- DataDog
- Jaeger
- OpenTelemetry Collector (tracing)
- Zipkin


## The Apollo Router developer experience


Apollo Router has a rich extensibility model that enables you to add custom functionality when you need to.


### Rust plugins


All of our configurable functionality in Apollo Router is written as plugins, and the very same plugin API is available to you.


Plugins that you write yourself are automatically integrated with the main YAML config giving you a seamless configuration experience.


With the plugin API you could add things like:


- Custom authentication
- Custom logging
- Anything you want, as long as you can write the code!


Take a look at our[examples directory](https://github.com/apollographql/router/tree/main/examples) to see how it’s done.


### Rhai scripting


[We chose Rust](https://www.apollographql.com/blog/announcement/backend/apollo-router-our-graphql-federation-runtime-in-rust#why-rust) to build Apollo Router because of its memory-safety and performance potential, but we want everyone to be able to write custom plugins, not just Rust enthusiasts like us.


If you need to make small changes to your request pipeline then the experimental Rhai script plugin may save you from needing to compile your own binary.


What you can do:


- Manipulate request/response HTTP headers
- Manipulate request/response context
- Perform checkpoint-style short-circuiting of requests
- Modify the status codes of requests/responses
- Modify the body of requests (excluding variables)
- Modify the body of responses


## Getting started with Apollo Router


We can’t wait for you to start using Apollo Router! Depending on where you are in your supergraph journey, here are some next steps you can take to get started:


- **If you’re already using Apollo Gateway** , check out the[migration guide](https://www.apollographql.com/docs/router/migrating-from-gateway) . The migration guide will walk you through how to check if your use case is currently fully supported by Apollo Router.


- If you find yourself unable to migrate from the Gateway for some reason, we’d love to understand the obstacles you’re encountering. Please[open a discussion on GitHub](https://github.com/apollographql/router/discussions) if you need general help or[open an issue](https://github.com/apollographql/router/issues) if you have any troubles with our instructions.
- In future Router releases, we’ll be enhancing Router’s built-in Gateway migration support. Keep an eye on future[releases](https://github.com/apollographql/router/releases) .


- **If you’re starting fresh with the Router** , see our[Quickstart docs](https://www.apollographql.com/docs/router/quickstart) .
- **If you don’t have a supergraph schema yet and want to learn more about Apollo Federation** , head over to the[Federation docs](https://apollographql.com/docs/federation/v2) .


Written by


Phil Prasek


[Read more by Phil Prasek](https://www.apollographql.com/blog/author/philprasek)
