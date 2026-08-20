---
schema_version: "1.0.0"
document_id: "7c1e8c8e97c350bbb6e1eb893329c894d8df489294749012dde8c0cfbf9414b5"
company_key: "uipath-inc-class-a-common-stock"
company: "UiPath Inc."
source_id: "uipath-inc-class-a-common-stock-rss-2f83a748bf9d"
canonical_url: "https://engineering.uipath.com/scaling-observability-with-opentelemetry-adx-how-we-improve-the-monitoring-with-cost-reduced-42100a99b89a"
published_at: "2025-07-10T10:27:33+00:00"
first_seen_at: "2026-07-20T23:16:59.255384+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:d05a754a6d52b990a86a0684f7d5cccc6708363cf0dcb692d7c47611802e97d4"
---

# Scaling Observability with OpenTelemetry + ADX: How We improve the monitoring with cost reduced

# Scaling Observability with OpenTelemetry + ADX: How we improved system monitoring while reducing costs


[Junda Yin](https://medium.com/@yinjunda?source=post_page---byline--42100a99b89a---------------------------------------)


5 min read


·


Jul 10, 2025


--


Press enter or click to view image in full size


## Introduction


Cost of goods sold (COGS), COGS, COGS. Everyone is talking about their growing cloud bills these days.


You might even hear extreme proposals — like quitting the cloud entirely and running a self-hosted infrastructure. ‌We don’t subscribe to such radical moves, but budgeting is still a core priority. Earlier, my colleague Florin shared how we reduced computing costs in his story ([you can read that here](https://engineering.uipath.com/throwing-sand-in-compute-how-project-sandman-reduces-costs-without-compromise-542b243f4c96) ). In this blog, we continue our cost-optimization journey by focusing on telemetry, and will talk you through how migration to OTel and Azure Data Explorer (ADX) saved us missions on telemetry costs, and hopefully this will help you save on your telemetry bills.


## Background


We started with Azure Application Insights for monitoring because most of our applications are deployed in Azure. It’s a great tool out of the box and has a solid feature set. While this stack worked reliably, the pricing model became problematic. Application Insights charges per GB ingested, and as the company scaled, telemetry grew to account for roughly 25–30% of our cloud billing.


This was clearly unsustainable.


## Our existing efforts to optimize telemetry costs


Before committing to a platform overhaul, we took several practical steps to curb rising telemetry costs within the constraints of Application Insights.


1. **Reducing log verbosity** : teams were encouraged to demote non-essential log levels from Information to Debug. We also configured the telemetry pipeline to ingest only logs at Information level or higher.
2. **Dynamic sampling and filtering** : we built internal tools that allowed teams to control telemetry ingestion dynamically using configuration or feature flags. This enabled real-time tuning of what data got ingested, without requiring code changes.


These approaches worked for a while. But as service traffic increased and our codebase complexity grew, we hit diminishing returns. Developers needed more logs to debug live-site issues, and the sampling controls couldn’t keep up.


Ultimately, these stopgap measures couldn’t address the core problem: Azure Monitor charges per GB ingested, regardless of how useful that data‌ is.


## Rethinking our telemetry stack


When we began exploring alternatives to Application Insights, we outlined several critical criteria for a replacement telemetry backend:


1. **Cost-effectiveness** : the solution ‌should significantly reduce our telemetry-related expenses
2. **Flexibility** : it needed to work well with a modern observability stack and offer freedom to route, process, and visualize data
3. **Cloud alignment** : since we run on Azure, a solution that fit naturally into that ecosystem was ideal


After evaluating multiple options, we selected **Azure Data Explorer (ADX)** . ADX offered strong performance, native Kusto Query Language(KQL) support, and a much cheaper billing model, which was especially appealing as our data volumes continue to grow.


## Understanding Azure Data Explorer (ADX)


ADX is a fully managed, high-performance analytics platform designed for large-scale data exploration. It supports real-time analysis of structured, semi-structured, and unstructured data, making it ideal for telemetry and observability use cases.


Key strengths of ADX include:


- **Speed and scalability** : ADX ingests large volumes of data quickly and supports fast queries using Kusto Query Language (KQL)
- **Cost efficiency** : it charges primarily for compressed storage and computing, enabling predictable cost scaling
- **Tiered storage** : ADX separates hot-cache and long-term storage, allowing fine-tuned control over performance vs. cost
- **Full Kusto capabilities** : developers retain access to Kusto features for queries, joins, and visualizations — just as they do in Application Insights


Though ADX doesn’t have a native SDK for telemetry ingestion, we solved this by integrating it with the OpenTelemetry Collector to handle export and schema transformation.


## Cost comparison


Press enter or click to view image in full size


The savings potential was clear — so we made the move


## Enter OpenTelemetry


ADX looked promising, but one blocker remained: the Application Insights client is tightly coupled with Azure Monitor. There’s no supported way to send telemetry elsewhere. On the other hand, ADX has its ingestion SDK, but clearly it is not suitable for telemetry instrumentation.


To move forward, we needed a clean break. That led us to **OpenTelemetry** .


OpenTelemetry (OTel) is an open-source observability framework that lets teams generate, process, and export telemetry in a consistent format. It supports logs, metrics, and traces, and is backed by a strong community.


## Get Junda Yin’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Key benefits:


- **Vendor-neutral instrumentation**
- **Support for all major signal types**
- **Large and active ecosystem**
- **Decoupled architecture** — instrument once, export anywhere


We used the **OpenTelemetry Collector** to centralize telemetry processing. It receives OpenTelemetry Protocol (OTLP) signals from the SDK and routes them to ADX.


> Fun fact: many contributors to the OTel .NET SDK originally worked on Application Insights .NET


## Architecture overview


We landed on the following stack:


Press enter or click to view image in full size


## OpenTelemetry SDKs


We instrumented our services with OpenTelemetry SDKs to emit logs, traces, and metrics. These SDKs are vendor agnostic and widely adopted, including robust support for .NET (as well as many popular languages). Using OTLP, we decoupled telemetry generation from backend specifics.


## OpenTelemetry Collector


The Collector serves as a gateway. It ingests OTLP signals, applies filtering or enrichment as needed, and exports to ADX. This abstraction layer makes the backend swappable and reduces coupling across the system.


## Azure Data Explorer (ADX)


ADX is our telemetry store and query engine. We defined update policies and used Kusto functions to convert incoming telemetry into Application Insights-compatible tables like` requests` ,` dependencies` ,` traces` , and` exceptions` . This allowed us to keep existing dashboards and alerts intact while improving cost efficiency.


## Grafana for visualization


We integrated Grafana with ADX to offer flexible, real-time dashboards. This filled gaps in trace visualization that ADX doesn’t natively support. A good example: end-to-end transaction traces, which were heavily used in Application Insights, are now fully replicated in Grafana.


## Results


After onboarding several core services into ADX, we saw 50–70% reductions in monthly telemetry costs.


## Why so much cheaper?


Azure Monitor charges based on GB ingested. ADX costs break down into:


- **Compute** : fixed monthly cost based on provisioned resources
- **Storage** : based on data volume after compression
- **Network** : negligible in our case


This pricing structure means marginal costs decrease as volume grows.


## Gaps and next steps


We’re happy with the progress, but a few gaps remain:


1. Today, only .NET services are onboarded. SDK support for other languages (like JavaScript) is not mature enough for a full rollout.
2. We’ve instrumented traces and logs. Metrics are still pending and will be addressed in our next phase.


## Conclusion


By adopting OpenTelemetry and ADX, we:


- **Reduced telemetry costs by up to 70%**
- **Maintained developer experience and query compatibility**
- **Removed vendor lock-in**
- **Built a modern, scalable observability foundation**


If you’re wrestling with rising telemetry costs or feeling boxed in by your current tooling, the OpenTelemetry and ADX pairing is worth a serious look. It’s not just a protocol — it’s a strategic enabler for scale.
