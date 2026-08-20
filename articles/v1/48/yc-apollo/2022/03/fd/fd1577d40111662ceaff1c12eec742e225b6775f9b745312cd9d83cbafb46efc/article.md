---
schema_version: "1.0.0"
document_id: "fd1577d40111662ceaff1c12eec742e225b6775f9b745312cd9d83cbafb46efc"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/new-operation-insights-with-client-usage-and-federation-query-plans"
published_at: "2022-03-15T06:24:54+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:796da8e520b1cc50c776f0e86237fa3083ba0160104d8164d6a5ecdaace55726"
---

# New Operation Insights with Client Usage and Federation Query Plans

Today we’re excited to show off some recent upgrades to Operation Insights in Apollo Studio. This update features three significant enhancements which make it easier to understand the usage and data flow of operations in your graph.


## 1. What does an operation look like?


First, we brought the operation signature front and center so that you can quickly understand the shape of fields being requested by the operation. The signature now appears as a side-panel that you can resize instead of hidden behind another tab.


Easier to find operation signatures.


## 2. Which subgraphs are used by an operation?


In the same panel, we added the ability to view the federation query plan for an operation. The query plan lets you see how Apollo Gateway or Router is working under the hood, describing how data from your subgraphs is woven together to build a full query response.


See query plans and subgraph dependencies for federated operations.


Similar to features found in Apollo Explorer, you can view the query plan as either a text document or visualized as a tree of subgraphs. ( Only available to federated graphs. Learn more about[Apollo Federation Query Plans](https://www.apollographql.com/docs/federation/query-plans/)


).


## 3. Which clients are using this operation?


Last but not least, we introduced a way to understand which clients are using an operation in a new Client Usage display. The most active clients are sorted to the top of the list based on the number of requests reported to Apollo Studio via[usage reporting](https://www.apollographql.com/docs/studio/metrics/usage-reporting/) .


See client usage breakdowns for operations.


### Now Available in Apollo Studio!


As a reminder, you can find all of these features by visiting the Operations tab in Apollo Studio and then choosing a single operation from the operations drop-down. Note that you can also get to insights for individual operations from field usage details, failed operation checks, client breakdowns, and Slack daily reports.


## Explore Operation Insights today


When utilizing[Apollo usage reporting](https://www.apollographql.com/docs/studio/metrics/usage-reporting/) for your graph, you’ll have access to these insights and more.


[Visit Apollo Studio](https://studio.apollographql.com/) to start using these tools and more today. And as always, we love hearing from you, so if you have any feedback, please fill out this[feedback form](https://forms.gle/Ep9cRWq9j3hwdmTL6) or drop a post on the[Apollo Studio GitHub Community](https://github.com/apollographql/apollo-studio-community/issues) !


Written by


Tim Hingston


[Read more by Tim Hingston](https://www.apollographql.com/blog/author/timbotnik)
