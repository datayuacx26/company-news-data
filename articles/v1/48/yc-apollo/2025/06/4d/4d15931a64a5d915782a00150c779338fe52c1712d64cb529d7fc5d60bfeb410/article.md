---
schema_version: "1.0.0"
document_id: "4d15931a64a5d915782a00150c779338fe52c1712d64cb529d7fc5d60bfeb410"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/getting-started-with-error-diagnostics-in-graphos-studio"
published_at: "2025-06-24T08:52:53+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:40293131467754febbf0d30fe6971cc26bfe96cc3b9f6c1232b5780837171bad"
---

# Getting Started with Error Diagnostics in GraphOS Studio

Apollo Federation makes it easy to orchestrate your backend services into a single unified API. With the recently announced availability of Apollo Connectors, even existing REST APIs can now be easily federated together. Today, we’re proud to announce[Extended Error Diagnostics](https://www.apollographql.com/docs/graphos/platform/insights/errors#extended-error-reporting) , a set of tools designed to help developers quickly identify, classify, and address errors across their graph.


## HTTP vs. GraphQL


With previous API technologies such as REST, developers have been accustomed to relying on HTTP status codes to track errors in their services (e.g. 404 not found, 500 internal server error, etc.). Frustratingly,[GraphQL errors](https://spec.graphql.org/draft/#sec-Errors) have been missing this same notion of error taxonomy. In addition,[GraphQL Federation](https://www.apollographql.com/docs/graphos/schema-design/federated-schemas/federation) introduces the possibility for one request to raise multiple errors from subgraphs and connected services – HTTP status codes fall short here as well.


Being able to identify the kind of errors that are happening and find the source errors quickly is crucial to maintaining API health. Apollo’s new error categorization framework eliminates guesswork by standardizing error classification, enabling clear insights into the root causes of issues. Teams can now better understand whether errors originate from a client query, a misconfigured schema, or an underlying service.


The feature integrates seamlessly into Apollo’s existing tooling, offering:


- **Actionable Insights in Apollo Studio:** Quick identification of error type and origin, enabling faster resolution and proactive mitigation strategies.
- **Categorization by Error Code and Service:** Errors reported to GraphOS will now contain the error code and service (subgraph or connector name) associated with the error.


## Observing GraphQL Errors


Errors can happen at the Router layer, the subgraph layer, or the transport layers in between. While many have strived to follow the[“errors as data”](https://www.apollographql.com/docs/graphos/schema-design/guides/errors-as-data-explained) pattern, in practice it’s not always possible. GraphQL errors are commonly used to represent service outages, enforcement of rate limits, rejections due to security policies, and many other runtime scenarios. When errors are raised at the Router or transport layers, it’s not currently feasible to use the errors as data patterns, and that’s where the GraphQL errors array becomes key.


A sampling of error codes emitted by Router.


We’ve witnessed growing adoption of the` errors.extensions.code` field in GraphQL responses, as mentioned in[this example](https://spec.graphql.org/draft/#example-8b658) in the GraphQL Spec:


To allow for categorization, Apollo Router provides the` code` and` service` extensions along with GraphQL responses wherever possible:


- Codes are useful to begin root-cause analysis, and also can be used to inform client logic with respect to retries, error messaging, or other complex error handling scenarios.
- The service dimension is incredibly powerful in a federated API, making it possible to tell if an error trend is specific to a single subgraph / connector service or due to a wider API issue.


An example of a GraphQL error response containing code & service extensions.


## New Error Diagnostics in Studio


Once you’ve enabled extended errors (see Setup below), new error diagnostic tools will be available in Studio:


- See error rates over time broken down by service or code, along with sample error messages when available.
- Use filters to drill down into specific services and codes of interest.
- Configure grouping options to aggregate error counts according to service, code, or path.
- Dig into specific operations and traces where the errors have been recorded.


These tools provide several ways to slice the error data, enabling you to diagnose error trends across the federated graph.


Error diagnostics in Studio, grouped by service and broken down by code


## Codes vs. Error Messages


While it’s important to use a traditional logger to record the detailed error message for every error that happens, relying on the error message alone presents some observability challenges:


- It’s hard to group errors together with different error messages. Error messages can come from anywhere and everywhere in the graph, following different conventions or no conventions at all. Without a standard method of categorization, it’s hard to diagnose trends in error behavior.
- Observability tooling has its limits. Error messages are often generated dynamically and contain information that can vary from one error to the next. For example, it’s common to include a specific dynamic value (user ID, call site, stack trace) related to an error. This is no problem for a logging system, but when trying to count the number of errors of each kind it quickly explodes with all possible combinations of errors.
- Error messages have an added security issue in that if they are not carefully reviewed, they can contain potentially vulnerable PII data which should not be made available to observability tooling.


These problems are alleviated by using a pre-defined set of error codes, making it possible to count errors according to their type, while allowing you to focus in on the groups that are the most problematic and filter out others that are purely noise.


In Studio, we’ve taken an approach of combining the low-cardinality dimensions of` code` and` service` along with a sampling of traces that contain the full request / response snapshot as well as the specific error message. Note that to see error messages, you will need to modify Router’s redaction policies, which are on by default. Our aim is to provide the best of both worlds – grouped and filterable errors along with specific examples of those errors. With traces providing additional context and complete error messages, you can tune the level of depth depending on your needs.


## **Setup**


### Error Metrics


Error categorization features are available in Router 2.1.2 and higher and currently offered as an opt-in[preview feature](https://www.apollographql.com/docs/graphos/resources/feature-launch-stages) . Additionally, for users of Router’s redaction and error inclusion policies, you’ll need to modify those settings to obtain further error information. Enable this via the[Router config yaml](https://www.apollographql.com/docs/graphos/routing/graphos-reporting#enabling-extended-error-reporting) :


```text
telemetry:
apollo:
errors:
preview_extended_error_metrics: enabled # (default: disabled)
subgraph:
all:
# By default, subgraphs should report errors to GraphOS
send: true # (default: true)
# Allows error messages from subgraphs to be included (default: true)
redact: false
# Allows code & service dimensions (default: strict)
# Only required if redact is true
redaction_policy: extended
subgraphs:
account: # Optional: override the default behavior for a single subgraph
send: false
```


### Traces, Including Error Messages from Subgraphs


Trace sampling is[configurable by the user](https://www.apollographql.com/docs/graphos/routing/graphos-reporting#subgraph-trace-sampling) , and depends on a number of factors including performance. For production systems we generally recommend between 1% – 10% to strike a good balance of breadth, depth, and performance. In testing and staging environments where performance is less of a concern, a higher sampling rate (50%-100%) is helpful for surfacing issues before they hit production. For further reading,[the OTel documentation on sampling](https://opentelemetry.io/docs/concepts/sampling/) describes these concepts in greater detail.


```text
telemetry:
# In this example, the trace sampler is configured
# with a 10% probability of sampling a request.
exporters:
tracing:
common:
sampler: 0.1
apollo:
# Further configuration for field-level tracing and subgraph error details.
# In this example it's set to 5%, meaning half of all traces sampled above.
# This value can't exceed the value of tracing.common.sampler.
field_level_instrumentation_sampler: 0.05
```


### Known Limitations


At scale, error metrics may encounter cardinality limitations in the OTel reporting agent. When this happens, extended metrics attributes may no longer be visible in Studio and will appear as` #AgentCardinalityLimitedExceeded` values. Adjusting the Apollo batch processing configuration to send reports more frequently can help to alleviate this.[Visit the docs to learn more.](https://www.apollographql.com/docs/graphos/routing/graphos-reporting#cardinality-limitations)


## **How Does It Work?**


When an error is raised inside of Router or simply passed through from an upstream (subgraph or connector) service, the error codes included in the GraphQL response will be monitored and measured by Router’s internal telemetry. Metrics emitted from Router will be tagged with` code` and` service` attributes allowing them to be visualized in Apollo Studio. From there, developers can analyze trends and identify root causes more easily. Additionally, traces with errors support the same categorization, making it trivial to find and debug specific errored requests.


Finding recent traces with error diagnostics in Studio.


Viewing a sampled trace related to the error.


The metrics themselves are emitted using the[OpenTelemetry (OTel) protocol](https://opentelemetry.io/docs/concepts/signals/metrics/) , which provides a mechanism for tagging errors with attributes for the` code` corresponding to the error and` service` (e.g. subgraph or connector name) corresponding to where the error originated. We’ll be collaborating with the newly minted working group for GraphQL + OTel to drive further standardization of these metrics.


## Customizing Error Codes


Apollo Router’s error reporting capabilities now include both predefined and customizable error codes. To include subgraph-specific error codes, simply include the error extension for code and it will automatically get included in Router’s error telemetry. For connectors, you can now define[custom mappings of REST errors to the GraphQL response](https://www.apollographql.com/docs/graphos/connectors/responses/error-handling#customizing-errorsextensions) , and include the error code in the mapping as follows:


```text
type Query {
users: [User]
@connect(
http: { GET: "http://my-api.com/users" }
selection: "id name"
errors: {
# sets the value of code to the REST response body at path error.code
# sets the value of status to the REST response HTTP status code
extensions: """
code: error.code
status: $status
"""
}
)
}
```


### Integration with other APM tools


Router also supports using a[standard OTel instrument](https://www.apollographql.com/docs/graphos/routing/observability/telemetry/instrumentation/standard-instruments#graphql) to measure the frequency of GraphQL errors and use an aggregation by` code` in the APM tool of your choice.


## Level up Your GraphQL Error Handling


Router includes support for a number of error scenarios out of the box, but the power of customizability will let graph practitioners take this even further. Managing errors is a crucial component of API health, and these measurement tools are there to help guide your efforts. To get the most of these new features, explore the documentation and links below:


- Learn more about the[error codes supported in Apollo Router](https://www.apollographql.com/docs/graphos/routing/errors#graphql-error-codes) .
- Start using error codes in your graph! Most GraphQL server libraries make it easy to add extensions to your errors:


- [Apollo Connectors](https://deploy-preview-652--apollo-docs-rewrite.netlify.app/docs/graphos/connectors/responses/error-handling#customizing-errorsextensions)
- [Apollo Server](https://www.apollographql.com/docs/apollo-server/data/errors#custom-errors)
- [GraphQL Java](https://javadoc.io/static/com.graphql-java/graphql-java/18.0/graphql/GraphQLError.html)
- [C# / HotChocolate](https://chillicream.com/docs/hotchocolate/v15/api-reference/errors)


- Visit the[GraphOS documentation](https://www.apollographql.com/docs/graphos/platform/insights/errors) to wire up error reporting and Studio.


Written by


Tim Hingston


[Read more by Tim Hingston](https://www.apollographql.com/blog/author/timbotnik)
