---
schema_version: "1.0.0"
document_id: "d1fb20829cb6e769d1ce397f3b1819dd8111affc84b24f9116a53636257aa01c"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/infinite-cardinality-metrics/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:b5df628ef3a2d82ce6b7c04bd740715ecba3448a28afa02cc037da8ce93db57b"
---

# Infinite Cardinality Metrics: Custom metrics built for modern systems

Josh Mirchin


Jacob Simonov


Every technology shift adds new context you need to measure. Cloud computing added regions and services.[Kubernetes](https://www.datadoghq.com/blog/topic/kubernetes/) added containers and pods. Multi-tenant applications added users and tenants. AI systems add models, prompts, agents, and execution paths.


The result is that metrics are becoming dramatically more dimensional, faster than ever before. Over time, engineers are forced to make tradeoffs. They remove dimensions, sample data, or avoid instrumenting workflows altogether, not because the data isn’t valuable, but because the cost of capturing it becomes difficult to predict.


Today, we’re introducing **Infinite Cardinality Metrics** , a new way to capture, explore, and scale custom metrics built for modern workloads. It gives teams the freedom to capture every dimension that matters, aligns cost with data volume rather than cardinality, and enables agentic exploration of richly contextual data. Infinite Cardinality Metrics is built on three simple principles:


## 1. Freedom to capture every dimension


With Infinite Cardinality Metrics, teams can capture every attribute and dimension that matters without constantly evaluating the cost impact of each new tag. A metric such as request latency is counted once, regardless of whether it’s tagged by service, region, user, tenant, or device, giving teams the freedom to add the dimensions they actually need.


At[Clay](https://www.clay.com/) , an AI-powered go-to-market infrastructure platform, that freedom translated directly into how teams instrument their product.


> In one of the new products we are building, the team decided to instrument it so we can slice fully by customer, execution path, and LLM call. This would have been far too cost-prohibitive previously. But under Infinite Cardinality Metrics, our infrastructure team was able to support this decision. As a result, the team now has clear, real-time aggregate monitoring in Datadog that previously would have required a data warehouse query or manual log-digging, enabling us to focus on building a great product for our customers.


Instead of deciding what context to remove, engineers can focus on capturing the data that helps them understand their systems. A metric is now priced by its metric name, not by the number of unique time series created by tag combinations.


## 2. Scale with data volume, not cardinality


Systems are becoming more dynamic and dimensions are multiplying, making comprehensive visibility increasingly important as organizations scale. Modern systems scale through traffic, requests, usage, and workload growth, not cardinality alone. Infinite Cardinality Metrics aligns cost with those same drivers, helping teams continue adding context without worrying about sudden cost increases from cardinality.


For teams like[Figma](https://www.figma.com/) , a collaborative design and product development platform, this creates a much more intuitive relationship between system growth and observability costs.


> As a team that owns metrics at Figma, we no longer have to reason about cardinality when thinking about cost. Instead, cost scales with the same drivers as our systems—like requests and traffic—which is an intuition every engineer already understands.


The result is a different approach to observability. Instead of asking, “Can we afford to measure this?” teams can focus on capturing the data that helps them understand and operate their systems.


## 3. Built for agentic querying and exploration


Capturing more dimensions is only valuable if you can actually use them. Infinite Cardinality Metrics is built for agentic querying and exploration, enabling engineers—and increasingly, AI agents—to ask questions across highly dimensional datasets without first deciding which context to discard.


For[Modal](https://modal.com/) , an AI infrastructure provider that serves inference, training, and sandbox workloads across tens of thousands of compute nodes, this means they can instrument metrics with worker identifiers and user context that would previously have been difficult to justify. The result is richer visibility and faster debugging at the level of detail modern workloads require.


When teams preserve more context in their metrics, they create a stronger foundation not only for human investigation, but also for AI-assisted analysis and exploration.


## Metrics built for modern workloads


Infinite Cardinality Metrics gives teams the freedom to capture every dimension that matters, the ability to explore richly contextual telemetry with both humans and AI agents, and a pricing model that aligns with how modern systems actually scale.


By removing cardinality as a constraint, teams can instrument more freely, preserve valuable context, and gain deeper visibility into increasingly complex environments.


Infinite Cardinality Metrics is now generally available. To learn more, visit our[documentation](https://docs.datadoghq.com/account_management/billing/metric_name_pricing/) . If you’re new to Datadog,sign up for a 14-day free trial .


-
-
-
