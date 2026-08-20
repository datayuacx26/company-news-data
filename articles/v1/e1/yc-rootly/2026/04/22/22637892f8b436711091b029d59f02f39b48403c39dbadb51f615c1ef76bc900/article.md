---
schema_version: "1.0.0"
document_id: "22637892f8b436711091b029d59f02f39b48403c39dbadb51f615c1ef76bc900"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/turning-your-incident-data-into-a-knowledge-graph"
published_at: "2026-04-10T18:40:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:762a32d8be4ad4b38419af680bd3b1a24de841cc32542280ebdced941ce25d67"
---

# Turning your incident data into a knowledge graph

[Graphify](https://github.com/raufer/graphify) is an open-source tool inspired by[Karpathy's LLM Wiki idea](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) : instead of rediscovering knowledge from scratch on every query, have an LLM build a persistent knowledge graph that accumulates structure over time. Point it at any folder, and it extracts entities, relationships, and communities into an interactive graph you can explore in a browser.


We built a Rootly plugin for Graphify so you can point it directly at your Rootly account and explore your incidents, alerts, teams, and service catalog as a knowledge graph.


` pip install "graphifyy\[rootly\]"`


` graphify rootly --days 30`


That's it. It pulls your data from the Rootly API, builds the graph, and outputs an interactive HTML visualization.


## Why a graph?


Questions like "which services always fail together?" or "who is stretched across too many on-call schedules?" require connecting data across lists, dashboards, and filters.


[Rootly's AI SRE](https://rootly.com/ai-sre) already does this; it maintains a complete graph of your Rootly data and beyond, and can answer these questions directly. This plugin gives you a lightweight, open-source way to explore that same kind of structural view on your own, as an interactive visualization.


No new data source needed. The data already lives in Rootly. The graph makes the structure visible.


- Service incident heatmap — which services are most incident-prone, and which ones tend to fail together
- Team on-call map — who covers what, and who is a single point of failure across multiple schedules
- Alert-to-incident funnel — which alert sources produce real incidents vs noise worth tuning
- Action item follow-through — whether retrospective action items are getting closed, by team and service
- Cross-service failure correlation — services that fail within the same time window, suggesting shared infrastructure dependencies


## What you can do with it


Onboarding. Hand a new SRE the graph, not a backlog of past incidents. They can explore the operational landscape – hot spots, team ownership, failure modes – in minutes.


During incidents. Query the graph: "what happened last time this service went down? Who responded? What fixed it?"


AI agents with memory. Feed the graph into an AI assistant or home made[AI SRE](https://rootly.com/ai-sre-guide) , and it accumulates context across hundreds of incidents instead of starting from scratch every time. This is Karpathy's original point.


Graph can easily help you find:


- "What happened last time this service went down?" (instant context during an incident)
- "What are the common threads across our SEV1s this quarter?" (cross-incident reasoning)
- "This alert just fired — what's the likely blast radius based on past incidents?" (predictive)
- "Write me a summary of our incident patterns for the quarterly review" (synthesis)


## Try it


Open source, works with any Rootly account. And if you are interested in a more sophisticated way, I'd be happy to show you Rootly AI SRE, just ask for a[demo](https://rootly.com/demo) .


[https://github.com/Rootly-AI-Labs/rootly-graphify-importer](https://github.com/Rootly-AI-Labs/rootly-graphify-importer)


‍
