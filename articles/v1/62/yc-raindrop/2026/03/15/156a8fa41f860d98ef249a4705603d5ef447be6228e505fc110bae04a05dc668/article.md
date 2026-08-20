---
schema_version: "1.0.0"
document_id: "156a8fa41f860d98ef249a4705603d5ef447be6228e505fc110bae04a05dc668"
company_key: "yc-raindrop"
company: "Raindrop"
source_id: "yc-raindrop-news-import-6a6050810bab"
canonical_url: "https://www.raindrop.ai/blog/trajectories/"
published_at: "2026-03-26T00:00:00+00:00"
first_seen_at: "2026-07-24T11:14:29.327591+00:00"
fetched_at: "2026-07-28T22:17:35.086279+00:00"
content_hash: "sha256:a4f0554b202386f3ad2fe845667e45abbfd595eb37bb9654dfa565870c43d312"
---

# Introducing Trajectories

Today we're launching Trajectories - a completely new way to visualize, search, and debug your agent traces.


## The problem with trace views


If you're building an agent, you've probably used a trace view to debug it. In the best case, it's a flat list with hundreds - or thousands - of spans. It doesn't really answer the simple question:


**what actually happened?**


Trace views were designed for distributed systems, not agents. An HTTP request fanning out to three microservices has a fundamentally different shape than an agent reasoning its way through a multi-step task, calling tools, recovering from errors, and changing strategies mid-run.


Trajectories solve this in two ways:


1.


**Visualizing agent traces in a sane way** - purpose-built for agents, not generic distributed systems


2.


**Making agent trajectories searchable** - find the trace you need in seconds using natural language


## Natural language search


You can search across all your agent traces using natural language. Just describe what you're looking for and Raindrop converts your query into structured filters automatically.


For example:


-


*"show me traces where the edit tool failed more than 5 times because it didn't read the file before"*


-


*"traces where bash was called after a glob error"*


-


*"long-running traces using claude-opus that had tool errors"*


The search bar also surfaces smart suggestions based on your actual tools, models, and signals - so you can quickly filter without typing a query.


## Visualization modes


There are two visualization modes, toggled at the top of the page.


### Output size


Output size mode scales each span by its output token count. This helps you instantly spot heavy tool calls - like an agent reading an enormous file or generating an unexpectedly large response.


### Duration


Duration mode is similar to a flame graph, but optimized for agents. Each span renders as a proportional horizontal bar on a shared time axis, making it easy to identify slow steps and performance bottlenecks.


Both modes make errors instantly visible (highlighted in red) and show you how the agent recovered.


## Explain trajectory


Click


**Explain** on any trajectory row to get an AI-generated summary of what happened. Raindrop analyzes the full sequence of tool calls - their inputs, outputs, errors, and timing - and streams a concise explanation directly into the UI.


This is especially useful for long, complex traces where scanning spans manually would take minutes.


## Filters


Beyond natural language search, you can build precise filters using the filter panel:


-


**Signals** : filter by any signal (e.g., task failure, user frustration, tool errors)


-


**Tools** : require specific tools to be present in the trace


-


**Models** : filter by AI model used


-


**Patterns** : define tool sequences (e.g., "edit must follow read")


-


**Attributes** : set minimum tool calls, minimum errors, per-tool count thresholds, and error message search


All active filters appear as removable pills and are persisted in the URL, so you can share filtered views with your team.


## Getting started


Trajectories require trace data. Check out the


[TypeScript SDK](https://www.raindrop.ai/docs/sdk/typescript) or


[Python SDK](https://www.raindrop.ai/docs/sdk/python) tracing docs to start sending traces, then head to


[Trajectories](https://www.raindrop.ai/docs/platform/trajectories) in your Raindrop dashboard.
