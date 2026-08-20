---
schema_version: "1.0.0"
document_id: "d3da59e55290e6e4992d7fe9c6347043c2926227bbfefa21e82c4de859207327"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/ai-dashboards-kibana-vega-lite"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T19:47:14.307175+00:00"
fetched_at: "2026-08-04T20:28:10.240209+00:00"
content_hash: "sha256:061304b238080c8bb28e4fb5bd51ebca8808bdbde68179a8ce648ea50092dfb2"
---

# Prompt to dashboard in under a minute, 5x cheaper: AI dashboards and custom Vega-Lite charts in Kibana

Kibana's[AI chat](https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/chat) now builds full[Elasticsearch Query Language–backed (ES|QL-backed)](https://www.elastic.co/docs/explore-analyze/visualize/esorql) dashboards from a natural-language prompt in under a minute. In Elastic 9.5, this moves to general availability (GA) ([technical preview in 9.4](https://www.elastic.co/search-labs/blog/ai-dashboard-generation-elastic-agent-kibana) ) with error recovery that retries failed queries, 5x lower ES|QL generation costs through tiered model routing, and interactive filter controls. This release also adds[Vega-Lite](https://www.elastic.co/docs/explore-analyze/visualize/custom-visualizations-with-vega) chart creation through natural language, including scatter plots, box plots, conditional formatting, and custom tooltips that you'd normally have to hand-code in JSON.


## What's new in Kibana's AI dashboard creation


**Capability**


**Technical preview (9.4)**


**GA (9.5)**


Error handling


No retry on failed ES|QL queries


Automatic retry up to three times with query inspection and adjustment


ES|QL generation cost


All queries routed through the primary model


Tiered model routing, up to 5x cheaper


Time range


Fixed default window


Automatic selection based on data time distribution


Filter controls


Not supported


Automatically added for the most relevant fields


Vega-Lite charts


Not supported


Natural-language creation, including scatter plots, box plots, conditional formatting, custom tooltips


Chart editing


Not supported


Edit existing Vega-Lite panels through natural language


### Automatic error recovery for AI dashboard generation


In the technical preview, the agent didn’t retry failed ES|QL queries. In 9.5, it detects query errors and retries up to three times, inspecting each error and adjusting the query before giving up. In practice, this eliminates the majority of empty-panel issues and produces dashboards that render correctly on the first try.


### Why is AI dashboard creation cheaper in Elastic 9.5?


Not every step in dashboard generation needs the same level of reasoning. In 9.5, ES|QL generation routes through a lighter model by default and falls back to the primary model only when needed. If your[connector](https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/connectors) uses Anthropic's Claude Opus 4.8, that means 5x cheaper for ES|QL generation across all panels.


### Automatic time range selection based on your data


Dashboards are only useful when they show the right window of data. The agent now applies improved logic to pick a time range that makes sense for the data it's querying, unless the user asks for a specific time range. It considers the data's time distribution and adjusts accordingly, whether that means the last hour for a live incident or the last 90 days for a trend analysis, rather than defaulting to a fixed window.


### Automatic filter controls on AI-generated dashboards


Dashboard creation now supports controls; that is, interactive filters that let viewers narrow a dashboard by field values without editing the underlying queries. When generating a dashboard, the agent automatically adds controls at the top for the fields most relevant to filter by.


## Vega-Lite charts from natural language: Chart types and formatting beyond the defaults


[Vega](https://vega.github.io/vega/) and[Vega-Lite](https://vega.github.io/vega-lite/examples/) support a wide range of chart types and customizations in Kibana. With 9.5, you can build them from plain language instead of writing the code yourself.


### Scatter plots, box plots, and more Vega-Lite chart types


Scatter plots, box plot charts, faceted small multiples, bubble charts, and composition charts (like combining histograms with heatmaps), among many others, are supported by[Vega-Lite](https://vega.github.io/vega-lite/examples/) . A prompt like *Show me a scatter plot of response time vs. request size, colored by service name* produces a Vega-Lite panel with the right data mappings. They use Kibana's default color palettes to blend with the rest of the dashboard.


Dashboard showing four different Vega-Lite charts: Box plot, scatterplot, small multiples, and heatmap with marginal histograms.


### Conditional formatting, custom tooltips, and labels on standard charts


Even for chart types that are already native to dashboards, like bar, line, or area, sometimes you need more control than the default capabilities offer. Vega-Lite through the chat fills that gap. Some examples include:


-


**Conditional color formatting:** Color data points above a threshold differently; for example, turning data points in a line or bars red when some metric spikes beyond your Service Level Objective (SLO). Ask the agent something like *Turn any points above 500ms red for my line chart.*


-


**Custom marks and labels:** Add emojis, symbols, or inline text labels to data points for at-a-glance status indicators.


-


**Custom tooltips:** Enrich hover states with additional metrics, context, or computed values that aren't part of the chart's axes. Ask something like *Add a tooltip that shows total record counts and the % per bar.*


This also works for editing existing Vega charts. If you have a Vega-Lite panel that needs a tweak like changing a color scale, adjusting an axis, or switching the mark type, describe the change in chat instead of digging into the JSON code.


## How we built natural-language Vega-Lite generation in Kibana


Generating a Vega-Lite chart from a sentence is not a one-shot *ask the model for JSON* prompt. We built a small agentic pipeline that turns natural-language intent into a validated, data-backed chart.


When a request comes in, the agent first determines whether Vega-Lite is the right fit. For Vega-Lite requests, it grounds the visualization in a real ES|QL query against Elasticsearch and then uses a model to generate the Vega-Lite code. Before rendering, the result goes through a normalization layer that corrects the schema and binds the canonical query. It also applies render-safety transformations.


A few design choices make this workflow reliable:


-


**Typed tool calling** : Chart creation is a structured tool invocation rather than free-form Vega-Lite pasted into the conversation.


-


**Constrained generation** : The model generates Vega-Lite code within a defined schema, making the output more predictable and easier to validate.


-


**Curated examples:** Structural patterns, such as faceting, layered marks, and heatmaps, provide guidance without copying the underlying data.


-


**Execute-and-verify loops** : Queries are executed before chart authoring, and validation failures trigger corrective retries for ES|QL generation.


## Try AI dashboard creation and Vega-Lite charts in Kibana


To try natural-language dashboard creation and Vega-Lite charts, upgrade to **Elastic 9.5** (or[start a free trial](https://cloud.elastic.co/registration) ), and open the **chat** in Kibana. Then ask it to build a dashboard from your data. For Vega-Lite, try asking for a chart type you've wanted but never built, like a scatter plot or a bubble chart. If the result isn't quite right, tell the agent what to change. It iterates with you.


This requires an Enterprise license.[Get started](https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/chat#get-started) .


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*
