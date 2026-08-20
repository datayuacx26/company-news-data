---
schema_version: "1.0.0"
document_id: "3a132e7c25dfc3eb44c84fbef64e5fd9148bf4c47c88c5c916aeb43417f35302"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphos-router-apm-dashboard-templates-for-datadog"
published_at: "2025-10-07T07:00:47+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:2185be099304ac35a42f73335b12aa4d84f9c5bf2a9f99e19e348f6e71e4244b"
---

# GraphOS Router APM Dashboard Templates for Datadog

Today we’re launching **[APM dashboard templates for Datadog](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/apm-guides/datadog/connecting-to-datadog)** , so platform and SRE teams can get **best practices observability** into GraphOS Router performance in just minutes. Previously teams would need to determine the important information to monitor, what telemetry is available, and then how to configure the dashboards. This could take hours or days of an engineers time to initially set up and refine over time.


Whether you’re just turning on tracing or you’ve been running[Apollo GraphOS](https://www.apollographql.com/graphos) at scale for years, these dashboards provide a clear, shared view of the[GraphOS Router](https://www.apollographql.com/graphos-router) ,[supergraph](https://www.apollographql.com/docs/graphos/resources/glossary?docs%5Bquery%5D=supergraph) , and subgraph health in minutes, just import and go.


## **Turn-key observability for Datadog**


Operating a supergraph is a team sport. When latency spikes or error rates climb, you need to answer:


- Is it the **router** , a **specific subgraph** , or **client behavior** ?
- Is the issue limited to a region, version, or deployment?
- What changed right before the incident?


Our Datadog templates + GraphOS Router instrumentation answer those questions:


- **Clear, consistent operations:** Spans and metrics are mapped into Datadog with **stable operation and resource names** , so charts and traces line up.
- **First-class GraphQL error tracking:** GraphQL errors are promoted into APM error views and correlated with latency.
- **Supergraph ↔ subgraph drill-down:** See end-to-end behavior, then pivot to the slow or noisy subgraph instantly.


## **What you’ll see in Datadog**


Figure: Apollo Router latency overview


- [Golden signals](https://sre.google/sre-book/monitoring-distributed-systems/#xref_monitoring_golden-signals) **at the top:** Requests, p95/p99, error rate, saturation.
- **GraphQL errors:** Surfaced as first-class errors with example traces.
- **Subgraph focus views:** Rank subgraphs by latency contribution and error impact.
- **Operation insights:** Identify the slowest or flakiest operations and track regressions by version.
- **Deploy & version overlays:** Correlate performance shifts with version rollouts and activity.


## **Get started with the new dashboard templates**


### **Connect Apollo Router to Datadog**


Set up metric ingestion for[Datadog](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/apm-guides/datadog/connecting-to-datadog) based on your preferred method:


- [Datadog OpenTelemetry Collector (DDOT)](https://docs.datadoghq.com/opentelemetry/setup/) – straightforward if you’re already Datadog-centric.
- [OpenTelemetry Collector](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/apm-guides/datadog/connecting-to-datadog/otel-collector) – vendor-neutral with powerful processors/sampling.
- [Datadog Agent](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/apm-guides/datadog/connecting-to-datadog/datadog-agent/datadog-agent-traces) – direct OTLP ingestion alongside your existing Datadog infra agent.
- [Agentless](https://docs.datadoghq.com/opentelemetry/setup/otlp_ingest/) – when you want no agents at all.


### **Configure the** **Router to emit the needed instrumentation**


Add the[required opentelemetry settings](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/apm-guides/datadog/router-instrumentation) to the Router’s configuration file, tailoring resource.name and tags to your conventions (service, environment, version, region) if needed. Restart the router so that the new configuration can be applied.


### **Install the dashboard template**


Inside the Datadog console,[import](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/apm-guides/datadog/observing-and-monitoring/dashboard-template) the new dashboards: **Dashboards → New Dashboard → Import JSON** and then paste our[template](https://github.com/apollographql/apm-templates/blob/main/datadog/graphos-template.json) .


Set the template variables (service/env/version/region) to match your Datadog tags. You should see the dashboard graphs begin to populate within minutes.


That’s it! Your supergraph’s APM story is live in Datadog


## **Managing your observability costs**


In order to optimize observability costs, we recommend keeping` resource.name` low cardinality by using stable values such as` operation_kind` or` subgraph.name` , and avoid embedding IDs or other highly variable strings.


For high-throughput services, enable tail-based sampling in your OTLP collector to retain the most useful traces while controlling cost, and explicitly exempt error traces so failures always surface.


Finally, apply Datadog’s Unified Service Tagging (service, env, version) consistently across traces and metrics so dashboard filters and version overlays behave reliably across teams.


## Questions or feedback?


Check out our[documentation](https://www.apollographql.com/docs/graphos/routing/observability/router-telemetry-otel/apm-guides/datadog/connecting-to-datadog) to learn more about setting up and using these dashboards. We’d love to[hear](https://community.apollographql.com/t/announcing-apm-dashboard-templates-for-datadog) how these dashboards are working for your team and what you want to see next.


Written by


Matthew Ratzke


[Read more by Matthew Ratzke](https://www.apollographql.com/blog/author/mratzke)
