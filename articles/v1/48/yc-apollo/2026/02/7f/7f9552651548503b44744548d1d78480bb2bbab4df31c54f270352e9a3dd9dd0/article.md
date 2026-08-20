---
schema_version: "1.0.0"
document_id: "7f9552651548503b44744548d1d78480bb2bbab4df31c54f270352e9a3dd9dd0"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/telemetry-cardinality-in-the-apollo-graphos-router"
published_at: "2026-02-10T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:21:02.085623+00:00"
content_hash: "sha256:e06b0e850b307e3ae5e637f4a66fe1784f36c57783f651f861f14cf7c5d7cb1e"
---

# Deep Dive: Telemetry cardinality in the Apollo GraphOS Router

The[GraphOS Router](https://www.apollographql.com/graphos-router) has been built with comprehensive observability in mind, using OpenTelemetry as its telemetry backbone. While we strive to provide defaults that work well for most use-cases, understanding how telemetry works within the router can be crucial for high-scale production graphs. This post provides a deep technical exploration of the router’s telemetry system, from initial instrumentation through to the export to various observability backends.


## Router Instrumentation


Let’s start where the metrics and traces are generated in the router. At its core, the GraphOS Router uses OpenTelemetry (OTel) for all telemetry instrumentation. The router uses the OpenTelemetry Rust SDK, which provides the foundational primitives for metrics and traces. Metrics and traces flow through OpenTelemetry’s standardized APIs before being exported to various backends.


Measurement tools like counters, histograms, and gauges that record telemetry data throughout the router codebase are known in OTel terminology as Instruments, which are created by Meters, which are in turn created by Meter Providers. A Metric Reader periodically collects the measurements recorded by the Instruments. Finally, the reader passes the collected data to a Metric Exporter that serializes and transmits it to a telemetry backend.


A critical limitation of the version of the OpenTelemetry SDK currently in use by GraphOS Router (SDK version 0.24.0 in Router version v2.10.0) is that it has a hard-coded metric cardinality limit of 2000. This limit exists at the SDK level, not in the router’s code itself.


In the context of metrics, cardinality refers to the number of unique metrics created by combining a metric name with all possible values of its attributes. For example, if you have a cache metric (which tracks cache usage by schema type) that includes attributes for one of 10 subgraphs, 1000 operation names, and 100 type names, you would end up with a maximum cardinality of 1,000,000 unique combinations of attributes for that metric.


This limit of 2000 unique attribute combinations applies to each batch of metrics that is exported by the router. If this limit is reached, the OpenTelemetry library emits a warning such as:


```text
Metrics error: Warning: Maximum data points for metric stream exceeded.
Entry added to overflow. Subsequent overflows to same metric until next
collect will not be logged.
```


The router detects this error condition and increments a special counter metric:` apollo.router.telemetry.metrics.cardinality_overflow` . If you see this counter incrementing, you know you’ve hit the limit. Once you pass the limit, metrics will still be recorded, but any attributes used after the limit is reached will have been be removed and can no longer be used for grouping or filtering.


More recent versions of the OpenTelemetry Rust SDK remove this hard-coded limit, allowing for configurable cardinality constraints. We are working to upgrade the OTel SDK in future versions of the Router to unlock this configurability. Doing so is complex, and the newer SDK versions introduce breaking changes. It is valuable to understand how to mitigate this cardinality limit, and we will share a few strategies you can do that soon, but first let’s take a look how these metrics are sent to Apollo or to your metric or trace service of choice.


## Metrics Exporters


Once metrics are being generated, they are sent by the router to various endpoints using one or more exporters, each serving different purposes. The behavior of these exporters can be configured using a few different settings (note that this is only a subset of all config options, see[Router YAML Configuration Reference](https://www.apollographql.com/docs/graphos/routing/configuration/yaml) for a complete reference):


```text
telemetry:
apollo:
metrics:
usage_reports:
batch_processor:
max_export_timeout: 30s
scheduled_delay: 5s
max_queue_size: 2048
otlp:
batch_processor:
scheduled_delay: 5s
max_export_timeout: 30s
exporters:
metrics:
otlp:
batch_processor:
max_export_timeout: 30s
scheduled_delay: 5s
enabled: true
endpoint: example_endpoint
prometheus:
enabled: true
listen: 0.0.0.0:9080
path: /metrics
```


These exporters share common options in their` batch_processor` config:


- ` scheduled_delay` is the delay between metric exports (default 5 seconds).
- ` max_export_timeout` is the amount of time that the exporter will wait before cancelling the export (default: 30s).
- ` max_queue_size` is the maximum number of unique metrics that can be held in the buffer (default: 2048). If the queue fills up completely, metrics will be dropped.


Since router v2.7.0, the configured values for these batch processors are logged, e.g.:


```text
configuring Apollo usage report metrics: ApolloUsageReportsBatchProcessorConfiguration { scheduled_delay=12s, max_queue_size=2042, max_export_timeout=32s }
```


Let’s explore each of these exporters in detail.


### Apollo Usage Reports


Apollo usage reports are the router’s specialized metrics format designed for GraphOS Studio. They have been in use since early versions of Apollo Server and pre-date the OpenTelemetry standard. While all new metrics are being sent using OpenTelemetry, we still have some important metrics that use these usage reports. These reports aggregate detailed operation and field-level statistics including:


- Request latency histograms with custom bucketing optimized for GraphQL operations
- Fields referenced by each operation
- Field-level execution statistics (when field-level instrumentation is enabled)
- Error rates and error paths within GraphQL responses
- Operation type and subtype classification
- Client identification (name and version)


### Apollo OTel Metrics


While baseline metrics are sent via usage reports, newer metrics are also sent to Apollo via OpenTelemetry. This includes metrics such as feature usage, enhanced error details, and subgraph insights.


### Standard OTel Metrics


The router can export standard OpenTelemetry metrics to any OTLP-compatible endpoint (OpenTelemetry Collector, Datadog, New Relic, Grafana Cloud, etc.). This exporter uses the standard OTLP protocol over gRPC or HTTP.


### Prometheus Metrics


The Prometheus exporter exposes metrics via a pull-based HTTP endpoint, following Prometheus conventions. This exporter creates a registry that accumulates metrics, which are then scraped by Prometheus servers. Since Prometheus is pull-based, there are no` batch_processor` configuration options for this exporter.


## Trace Exporters


Tracing in the router captures the complete request lifecycle as a distributed trace, with spans representing each stage of query processing. As with the metrics exporters, the behavior of these trace exporters can be configured using a few different settings (again note that this is only a subset of all config options, see[Router YAML Configuration Reference](https://www.apollographql.com/docs/graphos/routing/configuration/yaml) for a complete reference):


```text
telemetry:
apollo:
tracing:
batch_processor:
max_export_timeout: 30s
scheduled_delay: 5s
max_export_batch_size: 512
max_concurrent_exports: 1
max_queue_size: 2048
exporters:
tracing:
common:
sampler: always_on
datadog:
batch_processor:
max_concurrent_exports: 1
max_export_batch_size: 512
max_export_timeout: 30s
max_queue_size: 2048
scheduled_delay: 5s
enabled: true
endpoint: example_endpoint
otlp:
batch_processor:
max_concurrent_exports: 1
max_export_batch_size: 512
max_export_timeout: 30s
max_queue_size: 2048
scheduled_delay: 5s
enabled: true
endpoint: example_endpoint
protocol: grpc
propagation:
zipkin: true
zipkin:
batch_processor:
max_concurrent_exports: 1
max_export_batch_size: 512
max_export_timeout: 30s
max_queue_size: 2048
scheduled_delay: 5s
enabled: true
endpoint: example_endpoint
```


These exporters share the same options in their` batch_processor` config as the metrics exporters, but the cardinality limits apply to trace spans rather than distinct metrics. The tracing exporters also include some new configurations:


- ` max_concurrent_exports` (default 1), which is the maximum number of concurrent export operations that can run simultaneously. This limits the number of spawned tasks for exports and thus memory consumed by an exporter. A value of 1 causes exports to be performed synchronously on the batch processor task.
- ` max_export_batch_size` : the maximum number of spans to include in a single export batch (default: 512). Once the queue reaches this size, the batch will be exported immediately even if the` scheduled_delay` period has not been reached. This must be set to less than or equal to` max_queue_size` .


An excessive amount of traces are usually not useful, so you may want to modify the` telemetry.exporters.tracing.common.sampler` configuration so that fewer traces are sampled. This defaults to` always_on` which is equivalent to a value of` 1` or 100%. Sampling fewer traces (1-10 percent based on requirements) will reduce the cardinality as well as have a beneficial impact on CPU and memory usage.


Let’s explore each of these exporters in detail as well.


### Apollo Usage Report Traces


Similar to metrics, Apollo usage reports include trace data for sampled operations. These traces are transformed into Apollo’s proprietary trace format, which includes:


- Query plan tree structure (parallel, sequence, fetch, flatten nodes)
- Subgraph request/response details
- Field-level execution timing (when instrumented)
- HTTP request/response metadata
- Error details with redaction based on configuration


The exporter implements an LRU cache to store recent spans, keyed by parent span ID. This allows field-level statistics to be aggregated before export.


### Apollo OTel Traces


The router can send traces in OpenTelemetry format to Apollo’s OTLP endpoint, enabling trace visualization in Apollo Studio with full OpenTelemetry semantics. These traces include all of the details that exist in the Apollo Usage Report Traces, but also contain additional spans that capture internal router behavior.


The percentage of traces sent via OTel vs those sent via Apollo Usage Reports can be configured using the` telemetry.apollo.otlp_tracing_sampler` configuration, which has been defaulted to` 1` (100% OTel) since router v2.0.0.


### Standard OTel Traces


This exporter exports traces to any OpenTelemetry collector using standard OTLP protocol. This can be used to send traces to services like Datadog, Jaeger, or your own OpenTelemetry Collector.


### Zipkin Traces


The router also supports exporting traces in Zipkin format for compatibility with Zipkin-based tracing systems.


## Managing Cardinality


As mentioned previously, the version of the OTel library currently used by the router has a hard-coded cardinality limit of 2000. If you notice that some metrics are missing metrics, the likely reason is that you have hit this limit.


The first step to fixing this is confirming that you have hit the limit. You can do this in a number of ways:


- The router will be emitting the` apollo.router.telemetry.metrics.cardinality_overflow` metric. If the value of this metric is greater than 0, it means you have hit the limit.
- The router will output log messages that look like:


- ` Metrics error: Warning: Maximum data points for metric stream exceeded.`` Entry added to overflow. Subsequent overflows to same metric until next`` collect will not be logged.`


- Your metrics will include data points with the attribute` otel.metric.overflow=true` . These represent measurements that exceeded the cardinality limit. The metric values are preserved but their original attribute labels are lost.


One way to be able to see the` apollo.router.telemetry.metrics.cardinality_overflow` metric and the` otel_metric_overflow` attribute is to enable Prometheus logging in your router config:


```text
telemetry:
exporters:
metrics:
prometheus:
enabled: true
listen: 0.0.0.0:9080
path: /metrics
```


When this exporter is enabled, metrics are be visible at` http://<your router IP address>:9080/metrics` . The overflow metric will look like:


```text
apollo_router_telemetry_metrics_cardinality_overflow_total{otel_scope_name="apollo/router"} 3
```


Metrics with the overflow attribute will look like:


```text
apollo_router_operations_entity_cache_total{otel_metric_overflow="true",otel_scope_name="apollo/router"} 1992
```


If you have high cardinality metrics, you may need to update your metric exporter batch config by decreasing` scheduled_delay` . This will mean that metric batches will be sent more often, and the time window containing the batch of metrics is less likely to hit the cardinality limit. However, reducing` scheduled_delay` to values lower than 1 second can result in dropped metrics as you may be sending metrics faster than they can be received.


For very high cardinality attributes, you may reach the cardinality limit extremely quickly. In this case, the best way to work around the OTel cardinality limit is to audit the attributes that you are adding to your metrics using your router config. By default, the router does not emit high-cardinality attributes, so you are unlikely to see an overflow unless you have customized the attributes in some way.


A detailed description of the possible configuration for metrics and attributes can be found at[Instruments](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/enabling-telemetry/instruments) , but an example of a possibly problematic configuration is:


```text
telemetry:
instrumentation:
instruments:
default_requirement_level: required
router:
http.server.request.body.size:
attributes:
client_name:
request_header: "apollographql-client-name"
client_version:
request_header: "apollographql-client-version"
```


This configuration will add the client name and version as attributes to all` http.server.request.body.size` metrics. If you use highly unique client names or versions, or you’re using some other request header that has a large number of distinct values, you will end up with lots of separate metrics for each combination of header values. The fact that the attributes are on a histogram metric exacerbates the issue even more, since a metric is emitted for each histogram bucket:


```text
http_server_request_body_size_bytes_bucket{client_name="some-client",client_version="some-version",http_request_method="POST",http_response_status_code="200",server_address="1.2.3.4",server_port="4000",otel_scope_name="apollo/router",le="0.001"} 0
http_server_request_body_size_bytes_bucket{client_name="some-client",client_version="some-version",http_request_method="POST",http_response_status_code="200",server_address="1.2.3.4",server_port="4000",otel_scope_name="apollo/router",le="0.005"} 0
http_server_request_body_size_bytes_bucket{client_name="some-client",client_version="some-version",http_request_method="POST",http_response_status_code="200",server_address="1.2.3.4",server_port="4000",otel_scope_name="apollo/router",le="0.015"} 0
etc
```


The general recommendation is that any attributes you add to metrics should have a well-constrained list of values and should definitely not come directly from a value that a user of your graph specifies.


## Apollo’s Cardinality Protection


After metrics are sent to Apollo GraphOS, we also implement own cardinality protection. When certain cardinality thresholds are exceeded, Apollo may replace high-cardinality attribute values with the value # CardinalityLimitExceeded.


This protection can affect:


- Client names
- Client versions
- Operation names


If you observe` # CardinalityLimitExceeded` appearing in your GraphOS Studio metrics, it indicates that your graph has exceeded Apollo’s cardinality limits for that dimension. This is a protective measure to ensure the stability and performance of Apollo’s aggregation infrastructure.


If you see this, here are some things you can check:


1. Review your client identification strategy – are you inadvertently creating unique client names/versions per request? Ensure your client instrumentation is correctly identifying client name and version (not using request IDs, timestamps, or GUIDs).
2. Consider consolidating operation names – dynamically generated operation names create unbounded cardinality.
3. Send all query variables using parameters rather than hard-coded strings or numbers. The entire operation body is hashed to generate a unique ID, so the following queries will create two distinct query IDs:


```text
query AuthorQuery {
author(id: "1") {
name
}
}
```


```text
query AuthorQuery {
author(id: "2") {
name
}
}
```


If you pass this variable as a parameter, the query ID will be the same for both executions:


```text
query AuthorQuery($authorId: ID!) {
author(id: $authorId) {
name
}
}
```


1. When using parameters, you should ensure that the parameter names are static and not unique like` $authorId_<some GUID>` , as the parameter names form part of the query body and will contribute to the hash.


## Conclusion


By understanding the OpenTelemetry foundation, the various exporters, and the configuration options available, you can build a robust observability strategy that scales with your GraphQL infrastructure. The key is balancing telemetry completeness with cardinality management, using the batch processor and router configuration to tune performance for your specific workload.


To learn more about GraphOS Router telemetry capabilities check out our[documentation](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel) .


Written by


Nick Marsh


[Read more by Nick Marsh](https://www.apollographql.com/blog/author/nick-marsh)
