---
schema_version: "1.0.0"
document_id: "d072170915320f1a61aa2f9f577a995515bed3506423a9d483324d6c2bd674d6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/faster-federated-graphs-and-better-usage-data-with-apollo-server-3-6"
published_at: "2022-02-22T14:36:18+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:1db6955b60734cb243f70ffe37563c8812faf2e3342004413719600ac4b9dbac"
---

# Faster federated graphs and better usage data with Apollo Server 3.6

With the release of Apollo Server 3.6, we introduced new features and configuration to improve usage reporting to Apollo Studio. They are companion features to Inline Tracing for federated GraphQL APIs and OpenTelemetry support. With operation shape-based usage reporting, you get a clearer picture of how your graph is used with a much smaller impact on overall performance.


## A brief history of tracing and usage reporting


One of my favorite benefits of GraphQL is the fine-grained usage information you get when clients must specify the fields they need in the query language. When I started using Apollo Studio, one of the features I was most excited about was access to all this data:


Screenshots of the Fields page showing field usage and the Operations page showing resolver traces


At the time, both of these views were powered by *tracing* .


Tracing is a means of collecting detailed, low-level information for investigating and diagnosing issues and performance. In a GraphQL service, we usually refer to “resolver tracing”, which tracks the time spent and the errors that occur with in field resolver functions.


In a monolithic GraphQL API, resolver tracing is pretty simple: instrument each resolver function call, then package it up and ship it to Studio.


It’s harder in a federated graph. Apollo Gateway doesn’t execute resolvers. Instead, that happens within subgraph services on other machines. So Apollo invented “Inline Tracing”, where subgraphs trace their requests and send the data back to the gateway *inline* — as extensions on the response. The gateway then combines traces from all subgraph responses into a complete trace and sends it to Studio.


```text
{
"data": {
"topProducts": [
{
"title": "Dune",
"author": "Frank Herbert"
}
]
},
"extensions": {
"ftv1": "676f6f64206a6f6220696e7665737469676174696e67207468697320737472696e672120636f6d6520776f726b20666f72207573212068747470733a2f2f7777772e61706f6c6c6f6772617068716c2e636f6d2f63617265657273"
}
}
```


*The trace data is a base64-encoded protocol buffer message.*


I was so excited to get this data that I ended up contributing the first ports of *` ftv1`* (”federated tracing version one”) to both[graphql-ruby](https://github.com/Gusto/apollo-federation-ruby#tracing) and[federation-jvm](https://github.com/apollographql/federation-jvm#federated-tracing) so I could see federated traces in the graphs I worked on at Square.


## The Challenges of Inline Tracing


Apollo Federation allows you to build subgraphs in[many languages and platforms](https://www.apollographql.com/docs/federation/other-servers/) , not just Ruby, Java, and Node. However, not all of the subgraph implementations support` ftv1` tracing yet, meaning that your choice of language could affect the availability of traces and field usage data.


On top of that, inline trace data adds overhead to subgraph requests. You may see a performance impact on the individual subgraph responses to the gateway.


Other systems like OpenTelemetry don’t use inline tracing. Instead, they send data out-of-band to external systems, which collate tracing data from multiple services.


Why doesn’t Inline Tracing work like OpenTelemetry? Because it’s a lot more work to set up a system of collector sidecars and exporter services! Inline Tracing is so easy to set up that it’s on by default—you can’t say that about most tracing systems!


However, the performance impact of inline tracing is unacceptable for some. This is especially true for GraphQL developers supporting very large requests. The size of the trace scales linearly with the size of the request (making the total response size grow super-linearly!)


Prior to Apollo Server 3.6, you had only two options to mitigate the performance impact.


- Disabling inline tracing entirely. This avoids extra work during execution and avoids bloating the response payload. Unfortunately, it also removed both the trace data in Operations tab and the field usage data in the Fields page.
- Sampling using` includeRequest` . This would affect *all* statistics reported to Studio, so you’ll lose accurate request counts, client usage data, and more.


Given these downsides, we wanted to enhance Apollo Server’s performance while collecting as much useful data as we could. To achieve this goal, we built a new feature that uses static analysis for usage reporting.


## Usage reporting with static operation analysis


With GraphQL, it’s critical to know which fields are referenced in operations, even if they’re never resolved. This data powers the breaking change detection in Apollo Studio’s[Checks](https://www.apollographql.com/docs/studio/schema-checks/) feature and is the inspiration for this new usage reporting based on the static analysis of “operation shapes”.


Operation shape-based reporting doesn’t rely on tracing. Instead, it statically analyzes the operation document to count the types and fields. It’s much cheaper to run this algorithm over the document than it is to trace every single field resolver. And most importantly, we run this algorithm only in the gateway, avoiding the need to send tracing data between servers. This means we support detailed field usage information for all subgraph servers regardless of language or framework.


Usage reporting based on operation shape analysis is available in Apollo Server as of version 3.6,[released in December 2021](https://github.com/apollographql/apollo-server/blob/main/CHANGELOG.md#v360) . The new field usage metrics improvements apply to monolithic graphs as well as federated graphs that use Apollo Gateway.


## The nuances of operation shape-based analysis


Consider this operation:


```text
query TopProducts {
topProducts {
title
... on Book {
author
}
... on Album {
artist
}
}
}
```


When you statically analyze this operation, you know that these fields are *referenced* :


- ` Query.topProducts`
- ` Product.title` (the interface field!)
- ` Book.author`
- ` Album.artist`


Notably, it’s not possible to know if` Book.title` or` Album.title` are used!


However, by tracing all the resolvers, you know exactly how many times a field is resolved. When` topProducts` returns the possible types of the` Product` interface, you’ll know which items are Books and which ones are Albums. This is how you can know the number of times the` Book.title` and` Album.title` fields are *executed* .


The difference between “field executions” (from tracing) and “referencing operations” (from operation shape-based usage reporting) is important. We decided to make this crystal clear by showing two different metrics for each field.


Screenshot of the Fields page with the new “Referencing Operations” column.


The Apollo Server[documentation](https://www.apollographql.com/docs/studio/metrics/field-usage/) goes into even more detail about the differences between “Field Execution” and “Referencing Operations”.


If you disable Inline Tracing, you won’t get the data that powers the “Field executions” metric in the Fields page, but you’ll still see the “Referencing operations” metric.


## Trace Sampling


Instead of disabling Inline Tracing, Apollo Server 3.6 also includes a new option,` fieldLevelInstrumentation` , that makes it super easy to enable Inline Tracing for a small percentage of requests:


```text
plugins: [
ApolloServerPluginUsageReporting({
fieldLevelInstrumentation: 0.01
})
]
```


With the above configuration, the gateway will enable` ftv1` tracing on subgraphs for 1% of requests. This means you’ll pay a performance penalty for a small subset of requests, but with enough traffic that 1% of requests will give you plenty of data to power the Operations tabs and help you find and diagnose issues in your resolvers.


Crucially, you will still get accurate metrics on field usage, overall operation latency, and error rates for 100% of requests.


The` includeRequest` option is still available but is no longer recommended for configuring tracing. It could still be useful if you want to exclude bot traffic from data in Studio with a configuration similar to:


```text
ApolloServerPluginUsageReporting({
includeRequest({ request }) {
// set by a security scanner at the ingress
if (request.headers["X-PROBABLY-A-BOT"]) {
return false;
}
return true;
}
})
```


## Wrapping Up


Apollo Server 3.6 is a big win for GraphQL and Federation. It directly addresses the limitations of Inline Tracing while still allowing you to capture a crystal-clear picture of graph usage and performance in Apollo Studio. And when combined with OpenTelemetry ([as discussed previously](https://www.apollographql.com/blog/backend/architecture/introducing-open-telemetry-for-apollo-federation/) ), you get an even more complete picture of your API’s performance across your entire stack.


Personally, I’ve enjoyed learning about the differences between field references and executions from[the great documentation we’ve put together on Field Usage](https://www.apollographql.com/docs/studio/metrics/field-usage/) . It’s given me a whole new appreciation for GraphQL and graph usage data!


### Resources


- [Field Usage in Apollo Studio](https://www.apollographql.com/docs/studio/metrics/field-usage/)
- [Usage Reporting Plugin documentation](https://www.apollographql.com/docs/apollo-server/api/plugin/usage-reporting/)
- [Federated Tracing documentation](https://www.apollographql.com/docs/federation/metrics/)
- [OpenTelemetry support in Apollo Gateway documentation](https://www.apollographql.com/docs/federation/opentelemetry/)


Written by


Lenny Burdette


[Read more by Lenny Burdette](https://www.apollographql.com/blog/author/lenny)
