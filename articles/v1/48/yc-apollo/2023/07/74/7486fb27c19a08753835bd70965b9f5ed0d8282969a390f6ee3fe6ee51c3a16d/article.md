---
schema_version: "1.0.0"
document_id: "7486fb27c19a08753835bd70965b9f5ed0d8282969a390f6ee3fe6ee51c3a16d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/designing-the-graphql-schema-for-propel-an-analytics-api-platform"
published_at: "2023-07-25T10:26:29+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:36423e05929a32141a47798c31d7b98f3bad8be7e69784a3740d54a7eda24225"
---

# Designing the GraphQL schema for Propel, an analytics API platform

Are you looking to add features like dashboards, time series visualizations, metrics, and reports to your application? Are you wondering how to model these in your GraphQL schema?


At[Propel](https://www.propeldata.com/) , we have built an analytics platform powered by a GraphQL API. In this post, we want to share our lessons learned in designing the schema for analytics features. We will provide some background on what data applications are, discuss our design philosophy behind the schema, describe some of the key decisions we made, talk about the challenges we faced, and provide some examples along the way. We will close with recommendations on how to model analytics in your schema.


To provide context, Propel helps engineering teams deliver high-performance data applications. It is an analytics backend with a GraphQL API and a React component library that provides a unified platform for powering dashboards, reports, data APIs, and usage metering use cases.


## What are data applications?


Modern companies generate vast amounts of data. Every event that happens, whether it is a customer adding a product to a shopping cart, or a notification being opened, is a data point of interest. This data is typically used internally to gain insights into business operations and identify areas for improvement. However, data applications take this a step further by providing customers with data and insights about their own usage of a product or service.


This can include some of the following use cases:


- **Embedded analytics:** Customer dashboards and reports as part of your product experience.
- **Data APIs:** Self-service internal or external Data APIs.
- **Usage metering:** Metrics to measure, report, and alert on product usage.
- **Data sharing:** Sync data to your customers’ data warehouse.


To power a broad set of use cases with the same platform, you need an API-first approach.


## Our history with APIs and GraphQL


Propel’s founding team came from Twilio, where REST APIs are the norm. At Twilio, we debated the RESTfulness of APIs, which had an almost religious feeling to it. When we started Propel, we took the approach of “working backward” and started with what developers needed to build. A couple of things were very clear and especially important for an[analytics API](https://www.propeldata.com/docs) like Propel.


First, you must fetch many data pieces to render a page. Think of a dashboard. You need 10 or 15 different datasets to render all the visualizations. In REST, this would require 10 or 15 requests and round trips to the server. With GraphQL, it needs just 1 request for the 10 or 15 different queries. This is a big win as it simplifies the code you need to write to render a dashboard.


Second, when you take the perspective of the client, not the server, you realize that clients don’t always need all the data the API offers. Propel’s time series API offers labels & values for charting libraries in different formats. You’d want to get the format you need, not all of them. With REST, you get everything whether the client needs it or not. With GraphQL, the client only gets the data it requests.


Lastly, our customers wanted to use our analytics API from a bunch of different stacks, including front-end, mobile apps, and server-side code. They needed a robust toolchain to build their production-grade apps. The strongly typed nature of GraphQL has enabled the creation of tooling like client generators, SDKs, and libraries, whose quality, we feel, surpasses their OpenAPI peers. The fact that GraphQL has a tighter spec also helps a lot with this.


To summarize, despite having “grown up” with REST APIs at Twilio, we decided to take the perspective of the client and experiment with GraphQL. Very quickly, the benefits started adding tangible value to our customers, which led us to go all in with GraphQL as the backbone of Propel’s data application platform.


## Our API design philosophy


Propel’s philosophy for designing APIs centers around five principles:


### Principle #1: Start with what developers need to build and work backward


This principle is intended to challenge your perspective. With GraphQL it is very easy to fall into the trap of modeling your API after your data model, or to just model the objects as they exist in the relational database and expose those to developers. This, in our view, is a mistake. It does not start with the customer in mind, and so it makes it really hard to make decisions on what features the API should or not have. We also recommend against automatic schema generators for the same reason.


Let’s look at an example of this principle in practice with Propel’s Time Series API. This API is intended to make it super easy for developers to fetch data to render a time series chart. Instead of exposing the underlying data model, it just asks for the key pieces of information it needs: the metric name, the granularity, the time range, and any filters you need to apply. The response comes in a format that developers can just plug into their favorite charting library.


```text
query TimeSeriesExample1 (
$timeRange: RelativeTimeRange,
$timeRangeN: Int,
$granularity: TimeSeriesGranularity!,
$salesperson: String!
) {
sales: metricByName (uniqueName: "sales") {
timeSeries ({
timeRange: { relative: $timeRange n: $timeRangeN}
granularity: $granularity
filters: [{
column: "Salesperson"
operator: EQUALS
value: $salesperson
}]
}) {
values
labels
}
}
}
```


### Principle #2: Write the docs first


This principle was one we learned at Twilio. If you don’t have the clarity to write good docs, it is very unlikely that you will ship a good API. For GraphQL, this means we write the schema and its documentation first. At Propel, we agree on the schema and documentation before starting to build the API. We do this in a GitHub Pull Request. Below is an example of one of the iterations we did when designing one of our new APIs:


Similar to Principle #1, we want to start our design at the schema our developers need based on their use cases and operation shapes. We can iterate quickly and once we agree on the shape of the schema, begin implementing everything.


By also doing our schema design in a Pull Request, we can start to layer in various tools into the checks that automatically run. Linting is an important aspect of building any schema and we’re really excited to add[GraphOS’s Schema Linting feature](https://www.apollographql.com/docs/graphos/delivery/schema-linter) !


### Principle #3: Low barrier, high ceiling


The APIs should make the simple things simple and the complex things possible. A great example in the Propel API is the filters. As an API to query analytical data, the ability to filter data is crucial to uncover insights. For the simple case, you want to be able to filter your data by one dimension. In the example below, we filter for all the records where status equals “ordered”:


```text
{
"filters": [
{
"column": "status",
"operator": "EQUALS",
"value": "ordered"
}
]
}
```


Although it is nice and simple, these filters might not be enough to uncover insights. What if you needed to filter an expression like this:


```text
(value > 0 AND value <= 100) OR status = "confirmed"
```


You’d want your API to make the complex possible. It’s OK if it is a bit more complicated. What you don’t want is to complicate the simple case. With the filters in Propel’s API, we believe we achieved this by letting users opt-in to complexity when they need to. Below is an example of the filters above expressed in the GraphQL API.


```text
{
"filters": [{
"column": "value",
"operator": "GREATER_THAN",
"value": "0",
"and": [{
"column": "value",
"operator": "LESS_THAN_OR_EQUAL_TO",
"value": "100"
}],
"or": [{
"column": "status",
"operator": "EQUALS",
"value": "confirmed"
}]
}]
}
```


Remember when making the ** impossible possible, you should favor more specific arguments vs generic ones. The goal is to always have a graph that can be understood by the user without help!


### Principle #4: Specific over general names


We favor specific over general names to protect the API namespace. A good example of this is our` metricReport` query. We were very tempted to call this query just` report` . It seemed like a much more elegant and simple name. However, taking up the “report” name for an entire API is a big deal. It is impossible to foresee if there are other report-type queries that then would result in confusing the developer experience. Since this query returns a report based on metrics, we decided to call it` metricReport` .


### Principle #5: Meaningful mutations


Our last principle is around meaningful mutations. What do we mean by this? Mutations that describe the action they are doing. This is better explained with an example. At Propel, we sync data from your data warehouse to our high-speed storage, which we call Data Pools. Data Pools enable Propel to serve data with sub-second latency to your application.


Data Pools have a syncing status that can be “enabled” or “disabled”. It does what you’d expect. If it is enabled, it syncs data from your data warehouse. If it is disabled, it doesn’t. As a developer, you can control the syncing status on a given Data Pool. We explored two approaches to model this mutation.


First, we explored being able to change the sync status in the` modifyDataPool` mutation. This seemed like a good and easy choice. The problem is the user’s intent to pause syncing is not properly captured by the` modifyDataPool` mutation. There are a ton of things that you can modify on a Data Pool. Making matters worse, if you modify the wrong thing, it could potentially break your app.


The mutation should match the user’s intent as closely as possible. That’s why we decided to introduce` disableSyncing` and` enableSyncing` mutations. They perfectly match the user’s intent, make error handling very simple, and, most importantly, they don’t open the door for user errors that can break developers’ apps.


## Challenges


We also wanted to share the three main challenges we’ve faced building and operating a public GraphQL API that serves thousands of developers.


### Evolving the schema without breaking changes


At Propel, we move fast without breaking things. When we put out an API for our customers, we can’t change it. As we learn more about the customer problems and the product evolves, so does the API. It is critical for the business that we can move fast without introducing breaking changes for our customers or introducing long and complicated migrations.


To evolve the schema fast and safely, we opted for versioning our queries and mutations. It is not the most elegant solution, but it meets our two requirements of being able to move fast without breaking changes. Below is an example of our mutation to create a Data Pool, which we’ve evolved and now has a suffix of “V2”.


```text
"""
Fields for creating a Data Pool.
"""
input CreateDataPoolInputV2 {
" The Data Source that will be used to create the Data Pool. "
dataSource: ID!


" The table that the Data Pool will sync from. "
table: String!


# ...
}
```


We have also favored this approach over versioning the full schema because it allows different parts of the product to evolve at their own pace, rather than trying to get everything aligned on major version changes.


### Unions on inputs


We’ve faced this challenge a lot, as many others in the GraphQL community. In Propel, you can query any resource by its ID or unique name. Initially, we created two different queries. For example, to get a metric, the` metric` query gets a Metric by ID, and the` metricByName` gets a Metric by its name.


```text
type Query {
metric (id: ID!): Metric
metricByName (uniqueName: String!): Metric
}
```


We are now moving our APIs to leverage the oneOf to be able to support different types of inputs. Our future metricv2 query will look something like this:


```text
type Query {
metricV2(input: MetricByInput!): Metric
}


input MetricByInput @oneOf {
id: ID
uniqueName: String
}
```


## Modeling Analytic Metrics in Your Schema


In this section, we want to share some of the best practices we’ve learned building Propel’s GraphQL API and how to incorporate them into your own schema design. Specifically, we want to focus on how to design schemas to incorporate analytics into your application.


There are two primary ways to add analytics into your schema. First, adding a metrics property to a type, and second adding a high-level metric type. Below we discuss when to use each approach and give you examples of how to implement them


### Adding Metrics to a Type


When you have a type like “Customer” or “Blog Post” the data of interest for the given object might be analytical data. For the blog post example, you might want to see the page views for the last 30 days. For a customer, you might want to see the number of purchases made over the last year.


You would want to use this approach when the primary unit of analysis is the type, such as a blog post or a customer. For example, given a customer, you would want to know all the associated metrics to display them in the customer profile page.


How could you model something like this in your schema? Let’s look at an example for a customer that has three metrics: “purchases” (the number of purchases), “spend” (the amount spent), and “returns” (the number of returns). Imagine we want to query the purchases, returns, and spend for a customer over the last 90 days and visualize all three metrics both as a single number (counter) and as a time series.


This is what we’d want the query to look like:


```text
query {
customer (id: $customerId) {
id
name
metrics {
purchases (input: $metricInput) {
counter { value }
timeSeries { values labels }
}
returns (input: $metricInput) {
counter { value }
timeSeries { values labels }
}
spend (input: $metricInput) {
counter { value }
timeSeries { values labels }
}
}
}
```


This query is really neat. You can request the metrics for a customer in the format that you need them for your application. You can imagine how easy it is to build a customer profile page where they can see their purchase and return history when you get the values and time time series arrays ready to be plugged in to your charting library. To be able to execute the query above your schema has to look like this:


```text
type Customer {
id: ID!
name: String
email: String
metrics: CustomerMetrics
}


type CustomerMetrics {
spend(input: MetricInput!): MetricResponse
purchases(input: MetricInput!): MetricResponse
returns(input: MetricInput!): MetricResponse
}


type MetricResponse {
counter: CounterResponse
timeSeries: TimeSeriesResponse
}


type CounterResponse {
value: Float!
}


type TimeSeriesResponse {
values: [Float!]
labels: [String!]
}


input MetricInput {
timeRange: TimeRangeInput!
granularity: MetricGranularity
}


input TimeRangeInput {
relative: RelativeTimeRange!
n: Int!
}


enum MetricGranularity {
HOUR
DAY
MONTH
}


enum RelativeTimeRange {
LAST_N_HOURS
LAST_N_DAYS
LAST_N_MONTHS
}


type Query {
customer (id: ID!): Customer
}
```


### Creating Top-Level Metrics


Another way you might need to model analytical data is via top-level metrics. These are not necessarily attached to an object in your schema like a customer or blog post but they are their own high level concept.


A good example of a top-level metric is revenue. You’d want to know revenue over time. Sometimes you might need to know revenue by customer, other times by country, and other times by acquisition channel. In this case, we recommend you model revenue as a top-level metric and you include filters to have the maximum flexibility. For the example below imagine we wanted to query revenue in France for the last 90 days and we need the data both as a single value (a counter) and as a time series.


The query would look somethings like this:


```text
query {
revenue(
filters: $filters
timeRange: $timeRange
granularity: $granularity
) {
counter { value }
timeSeries { values labels }
}
```


And this would be the schema to support these kind of queries:


```text
type Query {
revenue(
filters: [Filter!]
timeRange: TimeRangeInput!
granularity: MetricGranularity!
): RevenueResponse!
}


type RevenueResponse {
counter: CounterResponse
timeSeries: TimeSeriesResponse
}


type CounterResponse {
value: Float
}


type TimeSeriesResponse {
values: [Float!]
labels: [String!]
}


input Filter {
column: String!
operator: FilterOperator!
value: String!
}


enum FilterOperator {
EQUALS
NOT_EQUALS
GREATER_THAN
GREATER_THAN_OR_EQUAL_TO
LESS_THAN
LESS_THAN_OR_EQUAL_TO
}


input TimeRangeInput {
relative: RelativeTimeRange!
n: Int!
}


enum MetricGranularity {
HOUR
DAY
MONTH
}


enum RelativeTimeRange {
LAST_N_HOURS
LAST_N_DAYS
LAST_N_MONTHS
}
```


### Propel as Your Analytics Backend


You are probably thinking in all the work and complexity that would need to go into the resolvers and the backend infrastructure to power these queries. This is where Propel comes in. You don’t need to stand up analytics infrastructure nor overload your databases with complex queries. With the schema design recommended above you can easily implement resolvers that consume Propel’s GraphQL API that uses similar concepts. This will make the implementation of the resolver extremely simple.


## Wrap up


In this post, we have shared our experience in designing the GraphQL schema for Propel, an analytics API platform. We have discussed our design philosophy, the challenges we faced, and some examples of how to model analytics in your schema. We hope this post has been helpful for experienced GraphQL developers who are looking to add analytics features to their products. By leveraging GraphQL, you can provide customers with data and insights about their own usage of a product or service, and you can do it with an API-first approach that makes it easy to build and maintain high-performance data applications.


If you want to learn more about Propel and how it can help you power dashboards, reports, data APIs, and usage metering use cases, you can try it for free[here](https://www.propeldata.com/) , check out the[docs](https://www.propeldata.com/docs) , or watch our upcoming live stream on August 8th at 10 AM PST in our[Discord server](https://discord.gg/graphos) .


Written by


Nico Acosta


[Read more by Nico Acosta](https://www.apollographql.com/blog/author/nico)
