---
schema_version: "1.0.0"
document_id: "97750024d2d03514240ea465c127f8e51a2b5bf164c9456d338b64640fe83eea"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/turn-databricks-telemetry-into-actionable-decisions-with-dynatrace/"
published_at: "2026-08-11T12:44:37+00:00"
first_seen_at: "2026-08-11T13:21:27.589369+00:00"
fetched_at: "2026-08-11T13:21:29.169075+00:00"
content_hash: "sha256:13c27cbfcca1af55bbb541513ba170b15fd1d305ebc7a7639d83cfef5c0c7c74"
---

# Turn Databricks telemetry into actionable decisions with Dynatrace

Dynatrace simplifies Databricks observability by unifying cost, performance, job health, and security insights in one place—enabling faster, data‑driven decisions for FinOps, DevOps, and platform teams.


Databricks has become a critical part of modern cloud operations. It runs scheduled jobs, interactive workloads, SQL warehouses, and increasingly, AI services. But when telemetry is split across logs, system tables, dashboards, and product views, even basic questions can take too long to answer. Why did this job fail? What caused this cost spike? Is a serving endpoint under strain, or just busy?


The[Databricks Workspace extension](https://www.dynatrace.com/hub/detail/databricks-workspace/) for Dynatrace can bring those signals together, helping teams troubleshoot faster, make better capacity and cost decisions, and understand how their Databricks environment is behaving as it grows more complex. It pulls telemetry from across the Databricks platform into a single, correlated view—enriched with context and queryable. This helps give teams the context to investigate issues more efficiently, make more confident resource decisions, and strengthen governance with real data.


In this post, we’ll look at how the extension helps with job and Spark reliability, cost and capacity optimization, AI-serving observability, and audit telemetry for governance and security.


## Operational reliability: a faster path to root cause


For operations teams, one of the most common questions is simple: which of my runs are failing, and why? Without correlated telemetry, the answer requires checking job run logs, pivoting to Spark UI for stage output, and cross-referencing cluster metrics separately — all before any real analysis begins.


The Databricks Workspace extension for Dynatrace helps answer that by combining:


- Job success rate, run count, queue/setup/execution/cleanup durations
- Active and completed run visibility
- Task-level distributed traces for each job run
- Spark-level metrics: failed tasks, stage throughput, shuffle pressure, and executor health


Job duration and success rate trends over time, broken down by individual job — useful for spotting regressions against a baseline.


The duration breakdown is particularly useful: when a job slows down, you can see immediately whether the delay is in the queue before execution starts, or in a specific stage mid-run. The distributed traces then let you drill into the exact task that regressed, rather than re-running the whole job to reproduce.


Example use cases:


1. Pinpoint whether a workflow slowdown starts in the queue, during setup, or in execution.
2. Isolate problematic tasks in long-running workflows and focus remediation on the exact stage that regressed.
3. Detect Spark inefficiencies early by tracking failed tasks, shuffle pressure, and executor behavior trends.


With this context, teams can move beyond broad retries and multi-tool investigation towards more evidence-based root cause analysis.


## Connect cost spikes to the workloads driving them


Databricks billing data alone tells you how much you spent but it doesn’t tell you what you spent it on. The extension ingests billing system table data and enriches it with workload metadata, so when a cost spike appears, you can trace it to the specific job, cluster, or SQL warehouse responsible.


Spend can be broken down by:


- Workspace, product category and SKU, job and job run, cluster and node type, SQL warehouse, notebook, model serving endpoint, instance pool, app, and pipeline.


Total spend broken down by SKU, product category, and individual job — showing exactly where your Databricks budget is going.


On the capacity side, compute node timeline metrics (CPU, memory, swap, network) support right-sizing decisions with real usage data rather than assumptions.


Automated right-sizing signals based on p50/p90 CPU and memory utilization, with explicit recommendations per cluster role.


The right-sizing table in the Cluster Resource Utilization dashboard makes this concrete: Clusters flagged as over-provisioned come with a specific recommendation (reduce CPU, downsize both CPU and memory) derived from actual utilization percentiles.


Example use cases:


1. Identify exactly which workloads drive daily or weekly cost spikes.
2. Distinguish growth in productive workloads from inefficient or unexpected usage.
3. Right-size clusters using real utilization data rather than assumptions.


This gives teams the workload context they can use to pursue more targeted cost optimizations instead of relying on blanket cuts.


## Govern AI-serving costs and reliability as usage grows


As model-serving endpoints move into production, teams face questions that go beyond uptime: who is consuming capacity, how much, and at what cost? Without usage visibility, token consumption and serving costs accumulate without accountability.


The extension captures model serving endpoint usage and health signals, including:


- Request count and status codes
- Request latency (average, p95, p99)
- CPU, memory, and GPU utilization
- Token usage (input, output, and total)
- Request trends by endpoint, model, and requester


AI gateway and model serving usage at a glance: total and failed request counts, token consumption split by input/output, and request trends broken down by endpoint, model, and requester.


The requester breakdown is especially useful for governance: you can see which users or services are driving the most traffic and token consumption, giving you the data you need to set quotas or support internal chargebacks before costs become a conversation.


Example use cases:


1. Identify which endpoints and requesters consume the most tokens to inform quota-setting and chargeback models.
2. Tune endpoint configuration based on real concurrency and utilization data rather than estimates.
3. Catch reliability issues early using request success rate and error trends by endpoint.


These signals can help teams make more informed decisions as they scale AI services, with visibility into reliability, performance, and cost.


## Turn audit logs into an active investigation tool


Audit data answers the questions that come after something goes wrong: who changed what, when it happened, and what the platform did in response. The challenge is usually speed — getting from “something looks off” to a concrete answer before the incident grows.


The extension surfaces Databricks audit events with enough context to investigate quickly: the service and action involved, the user or identity behind it, and the resulting status or error. Events can be filtered and sliced by IP, kind (accounts, clusters, Unity Catalog, etc.), status code, and more — turning raw audit data into a navigable investigation surface.


Audit events broken down by IP, event kind, and more — with a filterable event table showing individual actions, their outcomes, and the identities behind them.


Example use cases:


- Governance and compliance reviews: Track critical administrative actions and validate change history.
- Security investigations: Find repeated failed actions, suspicious patterns, or unexpected access behaviors.
- Operational troubleshooting: Correlate failed platform operations with affected services and user workflows.
- Incident reviews: Provide shared, timestamped evidence for platform, security, and data teams.


This can help turn audit logs into a more usable investigation surface, helping teams explore events in context instead of treating logs as a static archive.


## Ready-made value with bundled dashboards


The extension includes six bundled dashboards to accelerate time to value:


- Workspace overview
- Job runs
- Cluster resource usage
- Cost management
- AI gateway and model serving insights
- Audit logs


These dashboards provide immediate visibility so you can drill down into the questions that matter most for your environment.


## Putting it all together


Dynatrace helps teams operationalize Databricks telemetry with unified context across logs, metrics, and traces so they can move from insight to action faster. It helps transform Databricks from being yet another silo to monitor into part of a more proactive, resilient cloud operations model.


If your goals include higher reliability, better cost efficiency, stronger governance, and scalable AI operations, the Databricks Workspace extension can help you get there.


[Try the Databricks Workspace extension](https://www.dynatrace.com/hub/detail/databricks-workspace/) on Dynatrace Hub and share your feedback with us.
