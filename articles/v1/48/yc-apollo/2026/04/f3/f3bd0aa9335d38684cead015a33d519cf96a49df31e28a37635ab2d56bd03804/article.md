---
schema_version: "1.0.0"
document_id: "f3bd0aa9335d38684cead015a33d519cf96a49df31e28a37635ab2d56bd03804"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/view-operational-insights-within-the-graphos-platform-api"
published_at: "2026-04-13T13:34:36+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:3a1d2fdebe8d38e60523b9724e8a30ddc06b017e09848bfba373c40112d09b57"
---

# View Operational Insights Within the GraphOS Platform API

Over the past few months, we’ve introduced several additions to the[GraphOS Platform API](https://www.apollographql.com/docs/graphos/platform/platform-api) that make it easier than ever to analyze your graph’s usage and performance trends. These new Insights APIs allow for detailed metric analysis of operations, fields, and subgraphs.


Let’s take a closer look at what’s new and how you can start using it today.


## Top Operation Report


The` topOperationsReport` field can be used when you’re looking for a point-in-time view of the operations run against your GraphQL server. This API can return operation details like the operation name, ID, and signature as well the total number of times the operation has been executed within the specified time range. The results can also be filtered to only show operations executed by specific client names and versions.


You could use this to generate a weekly report showing the top 10 operations executed by a client so that you can see how the operations and request counts are changing:


```text
query WeeklyTopOperations {
graph(id: "your-graph") {
variant(name: "current") {
topOperationsReport(
from: "2025-11-01T00:00:00Z"
to: "2025-11-07T00:00:00Z"
filter: { in: { clientName: [some-client"] } }
limit: 20
) {
operationId
name
type
requestCount
}
}
}
}
```


You could also use this to get a list of all operation signatures that were executed within a day (assuming this is less than the max limit of 10k operations):


```text
query AllDailyOperations {
graph(id: "your-graph") {
variant(name: "current") {
topOperationsReport(
from: "2025-11-01T00:00:00Z"
to: "2025-11-02T00:00:00Z"
limit: 10000
) {
operationId
name
signature
requestCount
}
}
}
}
```


## Insights Timeseries APIs


When you need to track how your graph’s performance and usage evolve over time, you can use the timeseries operations to provide structured, time-bucketed insights. These insights are available for operations (` operationInsightsTimeseriesReport` ), subgraphs (` subgraphInsightsTimeseriesReport` ), and fields/enums (` schemaCoordinateInsightsTimeseriesReport` ). These APIs allowing you to monitor changes in key metrics, automate data analysis workflows, or build dashboards.


They provide results in the form of a set of metrics, grouped by a set of dimensions, split into time chunks of a particular resolution across a specified time range, and include the option to filter in or out operations or fields that have specific dimensions.


### Operation Insights Timeseries API


The` operationInsightsTimeseriesReport` field provides metrics such as request counts, latency percentiles, and error rates, grouped by time and dimensions like graph variant, operation name and ID, or client name and version. The results can be returned in a CSV or GraphQL format depending on your needs.


This is ideal for investigating usage spikes, comparing latency between operations, or monitoring error rates after a deployment.


For example, if you notice that a particular operation named` GetUserProfile` is slow, you could query for the p95 latency for each hour across the last week:


```text
query OperationLatencyTrend {
graph(id: "your-graph") {
operationInsightsTimeseriesReport(
resolution: HOUR
from: "2025-11-01T00:00:00Z"
to: "2025-11-07T00:00:00Z"
dimensions: [OPERATION_ID]
metrics: [REQUEST_COUNT, REQUEST_LATENCY_P99_MS]
filters: {
include: {
variantName: "current"
operationName: ["GetUserProfile"]
}
}
) {
csv
records {
startTimestamp
endExclusiveTimestamp
dimensions { type value }
metrics { type value }
}
}
}
}
```


You could also query for the request count and error count per minute across an hour for each client and version except for an internal test client, so that you can see which clients contributed to a particularly heavy request period:


```text
query ClientRequestTrend {
graph(id: "your-graph") {
operationInsightsTimeseriesReport(
resolution: MINUTE
from: "2025-10-01T00:00:00Z"
to: "2025-10-01T01:00:00Z"
dimensions: [GRAPH_VARIANT, CLIENT_NAME, CLIENT_VERSION]
metrics: [REQUEST_WITH_ERROR_COUNT, REQUEST_COUNT]
filters: {
exclude: { clients: [{ clientName: "internal-test-client" }] }
}
) {
csv
records {
startTimestamp
endExclusiveTimestamp
dimensions { type value }
metrics { type value }
}
}
}
}
```


### Subgraph Insights Timeseries API


The` subgraphInsightsTimeseriesReport` field provides subgraph request count and latency percentiles grouped by time and dimensions like graph variant, fetch service name (the name of the subgraph or connector), client, or operation. Like all the timeseries reports, the results can be returned in a CSV or GraphQL format depending on your needs.


For example, to see a daily trend of fetches and fetches with errors to all your subgraphs and connectors across all variants of a graph, you could use the following query:


```text
query SubgraphInsights {
graph(id: "your-graph") {
subgraphInsightsTimeseriesReport(
resolution: DAY,
from: "2025-10-01T00:00:00Z"
to: "2025-11-01T00:00:00Z"
dimensions: [GRAPH_VARIANT, FETCH_SERVICE_NAME],
metrics: [FETCH_COUNT, FETCH_WITH_ERRORS_COUNT],
) {
records {
startTimestamp
dimensions {
type
value
}
metrics {
type
value
}
}
}
}
}
```


### Schema Coordinate Insights Timeseries API


The` schemaCoordinateInsightsTimeseriesReport` field focuses on schema coordinate metrics, helping you understand how specific object fields, input object fields, or enums are being used. Again, the results can be returned in a CSV or GraphQL format depending on your needs.


Use this to track adoption of new fields or monitor error rates tied to specific parts of your graph.


For example, to check the adoption and for any errors on a new` User.email` object field across all variants, you could use:


```text
query FieldUsageTrend {
graph(id: "your-graph") {
schemaCoordinateInsightsTimeseriesReport(
resolution: HOUR
from: "2025-11-01T00:00:00Z"
to: "2025-11-02T00:00:00Z"
dimensions: [VARIANT_NAME]
metrics: [REQUEST_COUNT, ERROR_COUNT]
filters: {
include: {
coordinates: [{
kind: OBJECT_FIELD,
namedType: "User",
namedAttribute: "email"
}]
}
}
) {
csv
records {
startTimestamp
endExclusiveTimestamp
dimensions { type value }
metrics { type value }
}
}
}
}
```


To see a monthly overview of the usage of the` UserRole` enum values or to check for any unused roles, you could use:


```text
query EnumUsageOverview {
graph(id: "your-graph") {
schemaCoordinateInsightsTimeseriesReport(
resolution: MONTH
from: "2025-01-01T00:00:00Z"
to: "2025-11-01T00:00:00Z"
dimensions: [NAMED_TYPE, NAMED_ATTRIBUTE]
metrics: [REQUEST_COUNT]
filters: {
include: {
coordinates: [{ kind: ENUM_VALUE, namedType: "UserRole" }]
}
}
) {
csv
records {
startTimestamp
endExclusiveTimestamp
dimensions { type value }
metrics { type value }
}
}
}
}
```


## Wrapping Up


For more information, check the[Platform API schema](https://studio.apollographql.com/public/apollo-platform/variant/main/schema/reference) and sample operations for[querying insights](https://studio.apollographql.com/public/apollo-platform/variant/main/explorer?collectionId=f210ae80-9302-4b70-8c06-492a22ae05df&focusCollectionId=f210ae80-9302-4b70-8c06-492a22ae05df) . We hope you try out these new features soon, and if you have any feedback, we’d love to hear it in our[Apollo Community discussion thread](https://community.apollographql.com/t/graphos-insights-via-the-apollo-platform-api/9803) .


Written by


Nick Marsh


[Read more by Nick Marsh](https://www.apollographql.com/blog/author/nick-marsh)
