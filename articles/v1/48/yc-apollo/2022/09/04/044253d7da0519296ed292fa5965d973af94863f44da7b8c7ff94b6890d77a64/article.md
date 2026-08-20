---
schema_version: "1.0.0"
document_id: "044253d7da0519296ed292fa5965d973af94863f44da7b8c7ff94b6890d77a64"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-apollo-router-v1-0"
published_at: "2022-09-22T10:48:18+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:524a79288f555b785858d790ae917f8a4fea0c34cc92d717c5b77d288b6381b7"
---

# Announcing Apollo Router v1.0

Today, we’re excited to announce that[Apollo Router](https://www.apollographql.com/docs/router/) has officially reached version 1.0! It’s been a few months since the first GA release of Apollo Router, our lighter and faster supergraph runtime written in Rust. Since then we’ve been focused on expanding its capabilities to support even more use cases.


The release of v1.0 takes Apollo Router beyond raw speed with optimizations for client app performance, hardened security, and robust extensibility:


- **Entity-based` @defer` support** in the Router mitigates slow services and works with existing subgraphs written in over[20 languages and frameworks](https://www.apollographql.com/docs/federation/supported-subgraphs/) , even if the server itself doesn’t support` @defer` .
- **Apollo Router is now secure-by-default** , has passed an independent security audit, and offers new security features like cross-site forgery protection
- **Enhanced YAML config and full Rhai scripting support** make it simple to adapt the Router to your environment. Customize the Router even further with the stabilized native extension API.


## **A quick refresh: what is Apollo Router?**


Apollo Router is our next-generation supergraph runtime that supports[Apollo Federation](https://www.apollographql.com/docs/federation/) (v1 or v2) — a declarative GraphQL architecture that lets you build and operate a modular GraphQL API. The supergraph runtime is what processes all of the incoming API requests from clients and *routes* them to the subgraphs that make up the overall supergraph.


## **Faster app performance with entity-based` @defer`**


Apps need to access essential data with as little latency as possible to improve performance benchmarks like first-content-paint (FCP), time-to-interactive (TTI), and total-blocking-time (TBT). Other less critical data can be returned slower and rendered incrementally while still keeping the user engaged.


However, since multiple REST APIs are often used to fetch data for a single GraphQL query, a single slow REST endpoint can bring an entire query to a halt. Hand tuning individual REST APIs may be needed for critical situations but are often hard to justify for typical roadmap features — leaving you with a difficult choice: slow app performance or slow feature delivery.


With Apollo Router v1.0, apps querying the supergraph can` @defer` the slow, non-essential parts of a request and have the most critical data returned immediately, with no backend changes to underlying services. The upgraded query planner in v1.0 uses[entities](https://www.apollographql.com/docs/federation/entities/) to fetch deferred fields from subgraphs and return them incrementally in a new multipart response format.


The best part? Because entity-based` @defer` is powered by the Router rather than subgraph servers, teams can continue using the same[subgraph languages and frameworks](https://www.apollographql.com/docs/federation/supported-subgraphs) they’re comfortable with and` @defer` just works, even if the subgraph server itself doesn’t offer` @defer` support.


### Apollo Client and Sandbox support


App developers can add` @defer` to queries issued from Apollo Client ([React](https://www.apollographql.com/docs/react/data/defer/) and[Kotlin](https://www.apollographql.com/docs/kotlin/fetching/defer/) , iOS support coming soon) and[Apollo Sandbox](https://www.apollographql.com/docs/studio/explorer/sandbox/) and both will now automatically handle the deferred responses. Additionally, when using Sandbox with Apollo Router running locally in` --dev` mode, it will now show query plans to visualize how deferred queries are fetched.


Entity-based` @defer` is classified as[preview functionality](https://www.apollographql.com/docs/resources/product-launch-stages/#preview) today. We’d love your feedback to polish it up before officially marking it as GA and to help inform the[@defer spec](https://github.com/graphql/graphql-spec/pull/742) which is currently a work in progress.


## Hardened security


A major focus for v1.0 was enhancing security across the board:


**Secure by default –** Apollo Router v1.0 is locked down to be secure by default. This includes cross-site request forgery prevention, subgraph error redaction, and disabling introspection — among other production best-practice settings. For a more frictionless local development experience, you can relax the secure by default stance using the new` rover dev` command and` router --dev` mode flag.


**Independent security audit** – Apollo Router passed a rigorous independent security audit by Doyensec, a leading security research firm. They did a great job finding a few areas in the Router codebase that could be hardened and all issues were addressed in the v1.0 release. Check out[the full report](https://doyensec.com/resources/Doyensec_Apollo_Report_Q22022_v4_AfterRetest.pdf) published by Doyensec for Apollo Router.


**The security control point for your graph** – security and governance controls like[Contracts](https://www.apollographql.com/docs/studio/contracts) are enforced by the Router today. The Router hides fields not included in a Contract from its public API, but those fields can still be used inside the supergraph for API-side joins and computed fields. As such, subgraphs must serve these hidden fields within a supergraph and the Router must keep them secret from the outside world.


The Router can establish trust with public-facing subgraphs like[MongoDB Atlas](https://www.mongodb.com/blog/post/building-modern-app-stack-apollo-graph-ql-atlas) by passing API tokens in[subgraph headers](https://www.apollographql.com/docs/router/configuration/header-propagation#example) with secrets from key stores like Vault using[environment variable](https://www.apollographql.com/docs/router/configuration/overview#variable-expansion) support. This ensures all supergraph traffic flows through the Router creating a security control point where practices like JWT validation, key rotation, OAuth token conversion, schema-driven authorization, and demand control can be implemented.


## Enhanced extensibility


As Apollo Router has been rolled out in more environments, we’ve learned about what integration points and customizations are required to make it work across a wide variety of conditions. With v1.0 we’ve further improved the extensibility model of Apollo Router to address these concerns:


**No code required** – Apollo Router ships a[standalone binary](https://www.apollographql.com/docs/router/quickstart) that can be configured using a[YAML config file](https://www.apollographql.com/docs/router/configuration/overview#yaml-config-file) . v1.0 has a new stable v1 configuration schema for things like header forwarding and CORS configuration. You can also configure many new features like[traffic shaping](https://www.apollographql.com/docs/router/configuration/traffic-shaping/) with support for rate limiting, query deduplication, configurable timeouts, and compression options. Apollo Router can very often be deployed with the stock Router binary and a minimal YAML config file.


**Lightweight Rhai scripting** – v1.0 officially has full support for[Rhai scripting](https://www.apollographql.com/docs/router/customizations/rhai) with a[stable v1 API](https://www.apollographql.com/docs/router/customizations/rhai-api/) that offers a safe, sandboxed way to customize the Router’s request flow. Rhai is ideal for common scripting tasks like manipulating strings, processing headers, and mutating request context. Check out the growing cookbook of[example scripts](https://github.com/apollographql/router/tree/main/examples) that can be used with the stock Router binary as a simple way of programmatically customizing the Router for your environment.


**Native extensions** – Many Router features are built as native extensions that can be used via standard YAML config and Rhai scripting. Native extensions are a good choice for advanced use cases when Rhai scripting is not enough. With v1.0 we have stabilized all key extension points in the[native extension API](https://www.apollographql.com/docs/router/customizations/native) and enabled more powerful schema-driven extensions to be built using Apollo’s new[Rust tooling for GraphQL](https://www.apollographql.com/blog/announcement/tooling/apollo-rs-graphql-tools-in-rust/) and new` @composeDirective` support for subgraph authors.


Our goal is to identify common customizations and elevate them into standard Router features over time,[so let us know](https://github.com/apollographql/router/discussions/categories/ideas) if you have a Router customization or idea that others in the community could benefit from!


## … and more!


**Stock router binaries and docker images** – stock Router binaries are available for Linux, Mac, and Windows. We also ship[pre-built docker images](https://www.apollographql.com/docs/router/containerization/overview) and an updated[Helm chart](https://www.apollographql.com/docs/router/containerization/kubernetes) for[Kubernetes deployments](https://www.apollographql.com/docs/router/containerization/kubernetes) . Kubernetes configuration examples are also provided for use of` kustomize` and other Kubernetes tooling.


**Scales consistently with low variance** – Apollo Router has 90% less variance in latency than the Apollo Gateway at much higher CPU utilization, so auto-scaling thresholds can be set much higher. The Router is multi-core aware so there are benefits to running fewer Router instances with more CPU and memory, resulting in fewer instances to manage and hotter caches.


**Open Telemetry support for distributed tracing & metrics –** Enhanced[Open Telemetry metrics](https://www.apollographql.com/docs/router/configuration/metrics) and support for custom resources, labels, and attributes make the Router easier to operate at scale. Built-in exporters for Prometheus, Jaeger, Zipkin, and Datadog are provided in addition to native support for the OpenTelemetry Protocol (OTLP) that enables[a wide variety of observability tooling](https://opentelemetry.io/vendors/) to work with Apollo Router.


**Graph-native observability in Apollo Studio** – new support for GraphQL operation trace reporting to Apollo Studio goes beyond previous operation-usage & client-awareness reporting. Provide full operation trace data with subgraph fetch times and field-level execution latency statistics that help app developers see where directives like` @defer` should be placed in client queries for the desired effect.


Placing Apollo Router in front of an existing GraphQL monolith gives you immediate access to operation traces,[field-usage insights](https://www.apollographql.com/docs/studio/metrics/field-usage) , and[schema checks](https://www.apollographql.com/docs/studio/metrics/field-usage) for your graph in Apollo Studio.


## Getting started with Apollo Router


We can’t wait for you to start using Apollo Router! Depending on where you are in your supergraph journey, here are some next steps you can take to get started:


- **If you’re starting fresh with Apollo Router** , see our[Quickstart docs](https://www.apollographql.com/docs/router/quickstart) .
- **If you don’t have a supergraph schema yet and want to learn more about Apollo Federation** , head over to the[Federation docs](https://apollographql.com/docs/federation/v2) .
- **If you’re already using Apollo Gateway** , check out the[migration guide](https://www.apollographql.com/docs/router/migrating-from-gateway) . The migration guide will walk you through how to check if your use case is currently fully supported by Apollo Router.


- If you find yourself unable to migrate from the Gateway for some reason, we’d love to understand the obstacles you’re encountering. Please[open a discussion on GitHub](https://github.com/apollographql/router/discussions) if you need general help or[open an issue](https://github.com/apollographql/router/issues) if you have any troubles with our instructions.
- In future Router releases, we’ll be enhancing Router’s built-in Gateway migration support. Keep an eye on future[releases](https://github.com/apollographql/router/releases) .


Written by


Phil Prasek


[Read more by Phil Prasek](https://www.apollographql.com/blog/author/philprasek)
