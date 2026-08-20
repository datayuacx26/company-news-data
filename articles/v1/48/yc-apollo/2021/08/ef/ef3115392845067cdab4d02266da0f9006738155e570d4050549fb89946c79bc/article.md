---
schema_version: "1.0.0"
document_id: "ef3115392845067cdab4d02266da0f9006738155e570d4050549fb89946c79bc"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-open-telemetry-for-apollo-federation"
published_at: "2021-08-03T15:01:42+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:1718b15e705d2f81ac072bc0cab0f52c75355ae0b073080d27e3af4d774dfea7"
---

# Introducing Open Telemetry for Apollo Federation

How do you debug or optimize a request as it travels through all the layers of your stack: from the client, through the Apollo Gateway, through your subgraph services, all the way to your databases?


First, you need visibility into what your code is doing during that request, regardless of how that work is distributed across machines. Fortunately, this is easier than ever with a Cloud-Native Computing Foundation project called[OpenTelemetry](https://opentelemetry.io/) !


As of version 0.31.1,[@apollo/gateway](https://www.apollographql.com/docs/federation/api/apollo-gateway) natively supports OpenTelemetry as part of a complete picture of the performance and reliability of a federated GraphQL request. You can find[the documentation here](https://www.apollographql.com/docs/federation/opentelemetry/) .


In this article, I’ll cover some OpenTelemetry basics, talk about the new support for OpenTelemetry in Apollo Gateway, and explain how Apollo Studio and OpenTelemetry work together to give you a complete picture of your graph.


## What is OpenTelemetry?


OpenTelemetry (or OTel) is “an observability framework for cloud-native software”. It is a collection of libraries, systems, and applications for instrumenting, collecting, and exporting information about workloads that span multiple applications.


A unit of work in OTel is a “span”. You can wrap a function call in a span, recording its duration and other metadata. You can also install instrumentation that automatically hooks into modules and functions, like the[HTTP instrumentation](https://www.npmjs.com/package/@opentelemetry/instrumentation-http) for Node.js.


A collection of related spans is called a “trace”. In a single system, a trace is very similar to a stack trace. But what makes OpenTelemetry (and its predecessors, OpenTracing and OpenCensus) powerful is the ability to correlate spans across systems. It does this by propagating trace identifiers in remote calls. When making HTTP requests, for example, OTel instrumentation adds trace IDs as HTTP headers.


Once you have a trace, you can aggregate them in a “collector” and efficiently send them to your observability tools with an “exporter”.


A diagram of a request flowing through a federated GraphQL architecture with OpenTelemetry and Apollo traces published to an OTel collector and Apollo Studio, respectively.


There’s obviously a lot more to OpenTelemetry ([see the docs!](https://opentelemetry.io/docs/concepts/what-is-opentelemetry/) ), but that covers it in the context of Apollo Federation.


## OpenTelemetry in Apollo Server and Apollo Gateway


Apollo Server has always supported OpenTelemetry via the[@opentelemetry/instrumentation-graphql](https://www.npmjs.com/package/@opentelemetry/instrumentation-graphql) library, which instruments the underlying[graphql.js](https://www.npmjs.com/package/graphql) library.


However, Apollo Gateway doesn’t use graphql.js to execute a GraphQL request; instead, it executes a “query plan”, coordinating smaller requests to your subgraph services and assembling them into a single response. The Gateway executor has a few phases:


- Validation
- Planning
- Execution
- Fetching
- Postprocessing


Apollo Gateway now instruments these phases as spans in an OTel trace. Combined with HTTP instrumentation, which propagates the trace ID as headers in requests to subgraphs, we can now connect the gateway query planning and execution to the actual GraphQL field resolution work in your subgraph services.


Check out our[documentation on setting up OpenTelemetry](https://www.apollographql.com/docs/federation/opentelemetry/#1-install-required-libraries) in your Apollo Gateway and Apollo Server apps.


Even if you’re using GraphQL framework other than Apollo Server or a language other than JavaScript, you can still use OTel to instrument your application. There are OTel instrumentation libraries for many other languages and frameworks, and[GraphQL-specific OTel support](https://opentelemetry.io/registry/?s=graphql&component=&language=) is small but growing.


## OpenTelemetry in your Infrastructure


Instrumenting a request is cool, but it’s not very useful on its own. You can export traces into many different systems to view, inspect, or search traces.


The open-source projects Zipkin and Prometheus both support OpenTelemetry. Here’s an example of a federated GraphQL request in Zipkin.


A screenshot of a trace in the Zipkin web UI, showing spans for the gateway and multiple subgraphs.


The OpenTelemetry project has a number of exporters for various Application Performance Monitoring (APM) tools, including SaaS products like Datadog and Honeycomb, or cloud provider solutions like AWS X-Ray. You can find a complete list in the[OpenTelemetry Registry](https://opentelemetry.io/registry/?s=&component=exporter&language=) .


## OpenTelemetry doesn’t replace Apollo Tracing


OpenTelemetry is great at recording what happened, but it doesn’t tell you much about what *could* happen. Apollo Studio uses the declarative nature of GraphQL to help you catch potential errors before they happen. Consider this GraphQL operation:


```text
query ProductsPage($categoryId: ID!) {
products(category: $categoryId) {
nodes {
sku
price
reviews { # could return null
nodes {
id
body
rating
author {
name
photoUrl
}
}
}
}
}
}
```


If the` Product.reviews` field returns null, then the GraphQL executor won’t run resolvers for any of the fields nested inside that selection like` Review.id` ,` Review.body` , or` Author.name` . OTel instrumentation won’t record any spans, so you’ll have no data about those fields.


However, if you remove or change the` Author.name` field, you could break all the clients that include that field in their operations.


We designed[Apollo Federated Tracing](https://www.apollographql.com/docs/federation/metrics/) to collect field usage regardless of what actually happens (or doesn’t happen!) in your services. Our tracing data powers the[Schema Checks](https://www.apollographql.com/docs/studio/schema-checks/) feature, alerting you of potentially breaking changes and giving you confidence as you evolve your API.


A screenshot of a failed check in Apollo Studio.


Apollo Studio also knows schema-related information like the` @deprecated` directive, so we can help you understand how clients are using deprecated fields.


A screenshot of deprecation information in Apollo Studio.


It’s possible to cross-reference OTel traces and Apollo Studio traces by customizing the[Usage Reporting plugin](https://www.apollographql.com/docs/apollo-server/api/plugin/usage-reporting/) in Apollo Server. By passing header values in trace sent to Studio that align with attributes on your OTel spans, you can jump between Studio and your OTel observability tooling to get a complete picture of a request.


A screenshot of a trace in Apollo Studio with metadata you can use to associate this request with an OpenTelemetry trace.


## Wrapping Up


OpenTelemetry is a fantastic open-source project and we’re excited to participate in the growing ecosystem. Combined with Apollo Studio, OpenTelemetry gives you a clear picture of your running systems, helping you debug, optimize, and evolve your federated graphs.


Check out the[OpenTelemetry documentation](https://opentelemetry.io/docs/) and our[documentation on instrumenting federated graphs](https://www.apollographql.com/docs/federation/opentelemetry/) .


Written by


Lenny Burdette


[Read more by Lenny Burdette](https://www.apollographql.com/blog/author/lenny)
