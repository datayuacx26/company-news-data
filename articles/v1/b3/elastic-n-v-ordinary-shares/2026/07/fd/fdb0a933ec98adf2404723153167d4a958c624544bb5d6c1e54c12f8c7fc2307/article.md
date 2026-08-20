---
schema_version: "1.0.0"
document_id: "fdb0a933ec98adf2404723153167d4a958c624544bb5d6c1e54c12f8c7fc2307"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/observability-metrics-public-sector-it"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:eca098ebc29a425f83b368ac2da2f000ce5dd40dd12eecbec6660ba2368f7ca0"
---

# Elastic’s new metrics capabilities will dramatically improve uptime for public sector IT

# Elastic’s new metrics capabilities will dramatically improve uptime for public sector IT


The new columnar metrics engine in Elastic Observability enables public sector IT teams to combine logging, metrics, and traces in one platform. As a result, SREs can improve uptime while protecting taxpayer dollars in the process.


By


[Leanne Link](https://www.elastic.co/blog/author/leanne-link)[Abhi Pandey](https://www.elastic.co/blog/author/abhi-pandey)


July 23, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


Public sector site reliability engineers (SREs) operate under a distinct set of pressures, whether that’s supporting a federal agency, a health department, a public university, or a transit authority. You're expected to maintain mission-critical uptime across complex, hybrid infrastructure while facing strict audit and compliance requirements. And you must justify every dollar of IT spend to oversight bodies, legislators, and the public you serve.


The observability tools market has historically made that last part hard; the platforms with the best capabilities tend to be the most expensive, and the most expensive platforms tend to penalize you for the instrumentation richness that modern infrastructure demands. When costs spiral, the predictable response is to drop data, shorten retention, or skip high-cardinality metrics entirely. During a 2:00 a.m. incident affecting citizen-facing services, a student portal, or a hospital system, that missing context is the difference between a fast resolution and a prolonged outage.


Elastic has changed that equation with the recent launch of rebuilt metrics capabilities, including a fully


[columnar metrics engine](https://www.elastic.co/elasticsearch/metrics) , native


[Prometheus and PromQL](https://www.elastic.co/elasticsearch/prometheus-monitoring) support, out-of-the-box infrastructure content, and agentic investigation


[workflows](https://www.elastic.co/elasticsearch/workflows) . As a result, public sector IT teams can have complete operational visibility without the cost tradeoffs that have historically forced a choice between coverage and budget.


## Reduce observability debt with one platform for logs and metrics


Many public sector organizations already rely on Elastic for affordable log storage and retention, especially to comply with government mandates, such as


[OMB Memorandum M-26-14](https://www.elastic.co/blog/m-26-14-memorandum) in the US.


Now, that same trusted logging foundation extends to metrics. Most public sector organizations running modern infrastructure have accumulated observability debt in the form of siloed tools: one system for logs, another for metrics, and a third for traces. Every signal lives in a different back end, uses a different query language, and requires context-switching to correlate. When an incident occurs, engineers are assembling a picture by hand across disconnected systems under pressure.


Elastic Observability stores metrics, logs, and traces in a single unified back end. When an alert fires, the investigation context is already assembled in the form of correlated log events, preceding metric anomaly, or related trace. Engineers aren't opening three tabs before they can form a hypothesis. Machine learning (ML) anomaly detection runs automatically against infrastructure metrics, so alerts surface with context about what changed and how severe the deviation is, not just a raw threshold breach. For public sector SREs managing services that constituents depend on, this means faster mean time to resolution. For leadership, it means fewer prolonged outages and less personnel time spent on manual correlation.


## Deployment flexibility: Cloud, on-prem, air-gapped


Elastic Observability runs across three deployment modes: Elastic Cloud Serverless, Elastic Cloud Hosted, and fully self-managed on-premises or air-gapped environments. Unlike some vendors that limit on-premises deployment or reserve their highest-value features for hosted instances, Elastic's full capabilities are available across all three. Organizations with strict data residency requirements can run the complete platform in their own infrastructure without sacrificing capability.


## A pricing model built for budget accountability


Public sector IT budgeting depends on predictability. Multiyear contracts, budget cycles, and appropriations processes don't accommodate surprise invoices driven by opaque pricing mechanics.


Many commercial observability vendors use pricing models that compound unpredictably as modern infrastructure grows. Per-host fees are combined with custom metric charges and container overages, creating bills that climb as Kubernetes environments scale or OpenTelemetry instrumentation deepens. The costs are hard to forecast because they're tied to cardinality — the very property that makes observability valuable in complex environments.


Elastic's model is fundamentally different: You pay for data volume, not for how many hosts you have or how granularly you instrument your systems. On Elastic Observability Serverless, metrics stored in time series data streams (TSDS) index mode are priced at


[25% of the standard Observability per-GB rate](https://www.elastic.co/blog/metrics-pricing) for both ingest and retention, or roughly $0.023 per GB ingested and $0.005 per GB per month retained at volume tiers. There are no per-host licenses, no cardinality-based surcharges, and


**no custom metric classifications that penalize richer instrumentation**


,


****


landing at


[half the cost of Datadog](https://www.elastic.co/blog/metrics-pricing) and other vendors.


Long-term retention is also meaningfully affordable; at roughly $0.005 per GB per month, keeping a year or more of full-resolution metrics data is financially viable, supporting the kind of retrospective auditing that public programs often require.


**A note on pricing and deployment models**


**:**


****


Public sector customers often have non-negotiable constraints on where their data lives. For US organizations, Elastic is authorized at the


[FedRAMP Moderate and High](https://www.elastic.co/industries/public-sector/fedramp) levels on AWS GovCloud (US). For organizations running


**Elastic Cloud Hosted**


or


**self-managed**


deployments, including air-gapped environments common in classified or high-compliance contexts, TSDS metrics are available at


**no additional charge**


at this time. In both cases, the columnar storage engine means metrics simply require less infrastructure to run, delivering cost efficiency regardless of deployment model.


## The engineering behind the cost savings


The performance improvements in Elastic's new metrics capabilities reflect a fundamental re-engineering of how Elasticsearch stores and queries time series data.


Elastic rebuilt metrics functionality around a columnar storage engine purpose-built for TSDS workloads. The


[results are substantial](https://www.elastic.co/search-labs/blog/elasticsearch-metrics-columnar-engine) :


-


Metrics now land at 3.75 bytes per data point for OpenTelemetry metrics, which is down from 25 bytes a year earlier — a 6.6x reduction in storage footprint and up to 2.5x more storage efficient than Prometheus.


-


Time series queries run up to 160x faster than earlier TSDS versions and up to 30x faster than Prometheus with indexing throughput up to 50% higher.


For public sector IT teams, the implications are direct: Storage efficiency means maintaining full-resolution metrics for months or years at a fraction of the previous cost without sacrificing speed or performance, enabling trend analysis, capacity planning, and audit trails that span meaningful time horizons. And adding new Kubernetes labels, cloud tags, or application dimensions adds data volume without triggering pricing tier changes or architectural strain.


## Native Prometheus support: Protecting what you've already built


Public sector infrastructure evolves slowly and deliberately. Procurement cycles are long, integrations run deep, and the operational risk of disruption to constituent-facing services is high. Any platform transition has to protect what's already been built.


Most SRE teams running modern cloud and containerized infrastructure have invested significantly in Prometheus-based tooling: scrape configs, alert rules, PromQL queries, and Grafana dashboards that represent years of operational knowledge. Historically, migrating metrics back ends meant rewriting all of it.


Elastic has removed most of that friction. Prometheus metrics arrive via Prometheus Remote Write and land in the same columnar store without semantic changes. PromQL works natively in Kibana, so existing queries, dashboards, and alert rules migrate directly without modification. For organizations that want to keep Grafana as their visualization layer, Elasticsearch exposes a native Prometheus-compatible API that any compatible front end can query directly, meaning teams can replace the back end while keeping the interface their engineers already know.


## A streamlined migration process


Migration can be incremental and largely automated. The


[Observability Migration Platform](https://www.elastic.co/observability-labs/blog/migrate-datadog-grafana-dashboards-alerts-to-kibana) is a CLI-driven workflow that translates supported Grafana and Datadog assets into Kibana-native outputs and produces the evidence needed to review the result, changing migration from a manual rebuild into a translation-and-verification workflow. It works from exported assets or live APIs, covering dashboards and alerting content from both Datadog and Grafana paths. Crucially for public sector teams, the workflow also produces reviewer-facing evidence, such as a migration report, manifest, verification packets, and rollout plan, so teams can see what translated cleanly, what was downgraded or flagged for manual review, and what still needs human judgment.


## Elastic is a recognized observability leader


Elastic Observability’s credibility in the market continues to grow. In July 2026, Elastic was named


[a Leader in the 2026 Gartner® Magic Quadrant™ for Observability Platforms](https://www.elastic.co/blog/elastic-leader-gartner-magic-quadrant-observability-platforms-2026) for the third consecutive year. We believe this recognition reflects Elastic’s prioritization of staying ahead of market shifts while offering customers the efficiencies of standardization through Prometheus and OTel as well as the ability to use a single platform for logs, metrics, and traces.


## The strategic case for public sector IT leadership


For public sector IT leaders, the case is both operational and strategic. A


[unified observability platform](https://www.elastic.co/observability) combining best-in-class metrics with Elastic's established logging capabilities changes the conversation across several dimensions:


-


Observability coverage costs significantly less with a predictable model.


-


Automated investigation context means faster incident response and improved service availability.


-


Affordable long-term retention supports compliance and audit readiness.


-


Consolidating metrics, logs, and traces on one platform reduces procurement complexity and integration risk.


## Next steps for metrics in public sector


Learn more about how public sector leaders are approaching observability by downloading the


[2026 Landscape of Observability for Public Sector report](https://www.elastic.co/industries/public-sector/landscape-observability-report) , or


[schedule time with one of our experts](http://elastic.co/contact/public-sector) .


###### Related blogs


- [AI observability: The backbone of mission resilience in the public sector](https://www.elastic.co/blog/maintaining-public-trust-with-ai-observability)
- [Elasticsearch: Best-in-class for logs, now best-in-class for metrics](https://www.elastic.co/observability-labs/blog/prometheus-metrics-elasticsearch-faster-cheaper-datadog)
- [Updated metrics pricing for Elastic Observability: Best-in-class metrics — now cheaper, too!](https://www.elastic.co/blog/metrics-pricing)


Gartner, Inc. Magic Quadrant for Observability Platforms. Padraig Byrne, Martin Caren, etl. 13 July 2026.


GARTNER is a trademark of Gartner, Inc. and/or its affiliates. MAGIC QUADRANT is a registered trademark of Gartner, Inc. and/or its affiliates and is used herein with permission. All rights reserved.


Gartner does not endorse any company, vendor, product or service depicted in its publications, and does not advise technology users to select only those vendors with the highest ratings or other designation. Gartner publications consist of the opinions of Gartner’s business and technology insights organization and should not be construed as statements of fact. Gartner disclaims all warranties, expressed or implied, with respect to this publication, including any warranties of merchantability or fitness for a particular purpose.


This graphic was published by Gartner, Inc. as part of a larger research document and should be evaluated in the context of the entire document. The Gartner document is available upon request from Elastic.


**—**


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
