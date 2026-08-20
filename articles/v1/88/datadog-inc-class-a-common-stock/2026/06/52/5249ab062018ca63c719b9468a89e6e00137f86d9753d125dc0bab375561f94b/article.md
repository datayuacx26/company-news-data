---
schema_version: "1.0.0"
document_id: "5249ab062018ca63c719b9468a89e6e00137f86d9753d125dc0bab375561f94b"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/datadog-clickhouse-log-management/"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:cfb3514fd3292f9918c714347117a8425630e8df61dfc32c930ca601c42baf5d"
---

# Store and search high-volume logs with ClickHouse and Datadog

Andy Lihani


As teams scale AI and agentic workloads, log volumes can grow fast. That growth can force teams into a difficult trade-off: Keep logs searchable in their existing workflows, or store them cost-effectively for longer periods. For teams that rely on logs during incident response, compliance reviews, and long-running investigations, losing either affordability or searchability can slow down troubleshooting.


Datadog and ClickHouse are partnering to help remove that trade-off. Two new capabilities, now in Preview, let you route high-volume logs to ClickHouse through Datadog Observability Pipelines and search those logs directly from the Datadog Log Explorer without re-ingesting them into Datadog.


First, we’llexplain what ClickHouse is and why it is useful for high-volume observability data. Then we’ll describe how the integration enables you to:


-


Route logs to ClickHouse with Observability Pipelines


-


Search ClickHouse logs from the Log Explorer


## What is ClickHouse?


[ClickHouse](https://clickhouse.com/) is a high-performance, open source columnar database originally built for real-time analytics at massive scale. For observability use cases, ClickHouse supports sub-second analysis across petabytes of logs, metrics, traces, and events. It also helps reduce storage costs through high compression and separation of storage and compute.


ClickHouse is well suited for high-cardinality telemetry data, which makes it a strong fit for organizations managing large and fast-growing observability datasets. Organizations including OpenAI, DoorDash, Anthropic, and Shopify use ClickHouse as an observability database for large-scale analytics.


## Route logs to ClickHouse with Observability Pipelines


With a native ClickHouse destination for[Datadog Observability Pipelines](https://www.datadoghq.com/product/observability-pipelines/) , you can send application and infrastructure logs to ClickHouse with in-stream parsing, enrichment, and redaction. This helps high-volume data land in a cost-efficient store already shaped for querying.


You can decide which logs should go where from a single pipeline. For example, you might route high-value logs to Datadog for real-time monitoring and send high-volume logs, or logs that require longer retention for compliance reasons, to ClickHouse. Observability Pipelines gives you a vendor-agnostic way to control these routing decisions before logs reach their destinations.


## Search ClickHouse logs from the Log Explorer


With[Federated Logs](https://www.datadoghq.com/blog/federated-logs-databricks-clickhouse-snowflake/) , you can query logs stored in ClickHouse directly from the[Datadog Log Explorer](https://docs.datadoghq.com/logs/explorer/) without re-ingesting the data into Datadog. This gives teams one search experience across Datadog-managed and ClickHouse-stored logs.


This approach helps teams retain higher volumes of logs for longer periods without sampling, while still investigating them alongside the rest of their observability data. Because the data stays in ClickHouse, teams can query it in place instead of duplicating logs across systems. And during an incident, engineers can move between Datadog and ClickHouse data without switching tools or rebuilding queries in a separate UI.


## Get started with Datadog and ClickHouse


The native ClickHouse destination for Observability Pipelines and federated search for ClickHouse logs are now available in Preview. Together, these capabilities help teams store high-volume logs cost-effectively in ClickHouse while keeping those logs searchable from the Datadog Log Explorer.


To get started, read the[ClickHouse integration documentation](https://docs.datadoghq.com/integrations/clickhouse/?tab=host) or[request access to the Federated Logs Preview](https://www.datadoghq.com/product-preview/federated-search/) . To learn more about routing telemetry data with Observability Pipelines, read the[Observability Pipelines documentation](https://docs.datadoghq.com/observability_pipelines/) . If you’re new to Datadog, you cansign up for a 14-day free trial .


-
-
-
