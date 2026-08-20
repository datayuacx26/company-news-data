---
schema_version: "1.0.0"
document_id: "4f598fb596cb64b0287c5af0add1b901e64090592f7916842e9dcd9bf255b8b6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/field-level-insights-in-studio"
published_at: "2022-09-13T08:30:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:797f44242792e5d546faf39b8150645d4f20f83c270d8c937032b3d3dec03e49"
---

# Continuously improve your graph with field-level usage and health metrics from Apollo Studio.

A healthy graph is never still — new features appear, old ones are pruned, and regretful decisions are repaired (“at last, my schema is perfect” – said no one ever).


But planning for change requires graph maintainers to tread carefully – the more you know about how your schema is being used, the more confidently and quickly you can move. With the new Field Insights page in Apollo Studio, safe and rapid innovation of your graph just got a whole lot easier.


## Why measure fields?


### Fields as *features*


Fields are the last stop for querying data from the graph. They represent the atoms of information that are exposed to the outside world. They are also functions – when used in a mutation, they are the entry points that trigger actions in the graph. Every field is literally a “feature” of the graph, and if we want to measure the usage or health of our graph’s features, the field is a great place to start. Maintaining a graph comes with a mindset of continuous curation, and knowing where to focus our improvements is key.


### Fields as *relationships*


Crucially, fields also model the relationships that bind objects together. This is what makes GraphQL uniquely powerful — queries can move freely about the graph, jumping from one entity to the next and fetching as much or as little data as they need along the way. But if one of these links breaks down, entire subtrees can go dark. This is of special concern in a federated context where fields provide the bridge between the business domains in your API.


## Stewardship through understanding


Viewing usage data through the lens of your fields unlocks some essential graph health workflows. With field insights, we are bringing these capabilities into focus for your graph:


1. **Understand usage and adoption patterns.** Client usage breakdowns make it easy to see how client applications and operations are using the field, and field usage over time tells you whether field usage is ramping up or down.
2. **Identify impact on user-facing apps.** Understand earlier in the schema lifecycle the changes that are likely to impact end-users and other team members based on observed usage.
3. **Guide change workflows** . Confidently deprecate and remove outdated or unused fields. See the first and last time a field was used, then verify changes with[schema checks](https://www.apollographql.com/docs/studio/schema-checks) at release time.
4. **Assess field-level health.** Per-field error rates and latency histograms help you pinpoint health issues in your graph to target optimization work and validate improvements.
5. **Understand federation dependencies.** Discover which subgraphs participate in resolving a field as teams work together to build the graph. (Hint: use the` @contact` directive in your subgraphs to let teams know how to get in touch with one another!)


New field-level insights for your graph.


The amazing flexibility of GraphQL has been game-changing for app developers, but it does present an observability challenge. With operations of all shapes and sizes, how can we best understand which parts of the graph are being used, how much, and by whom? What parts of the schema are unhealthy or causing performance bottlenecks for the graph? With the new Field Insights page, you’ll have a powerful new tool to answer these kinds of questions.


Field Insights are available to all organizations, with longer data retention offered on paid Apollo plans. As always, we’d[love your feedback](https://docs.google.com/forms/d/1lC-7aWNe4pnxcAPTB-W4-N8jZoyD_xQXzVByD6HOZpc/edit) on how we could further improve your graph workflows.


⚠️ *Note that field latency and execution stats require the use of field-level instrumentation in your server or subgraph – for details on how to configure your server, see*[our documentation.](https://www.apollographql.com/docs/studio/metrics/field-usage)


Written by


Tim Hingston


[Read more by Tim Hingston](https://www.apollographql.com/blog/author/timbotnik)
