---
schema_version: "1.0.0"
document_id: "515655b8765a8bb62cf80b8c8cfa333b7c691289d6d475de49519ffbb990a39b"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/announcing-trace-intelligence"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T16:27:30.844834+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:6378f421935893b94c4be1bce308fb0f93ca60c263520eb5e8d7e2719bac36ae"
---

# Announcing Trace Intelligence

The hardest part of building an agent is ensuring it’s accurate enough to ship into production. Teams can spend weeks or months reviewing user traces and turning them into agent fixes.


That’s why we’re excited to announce Trace Intelligence in beta today. Trace Intelligence is grouping traces into clusters so you can identify common goals, behaviors, sentiments, and outcomes across many runs. It’s built into observability on the Mastra platform.


Your browser does not support the video tag.


While building our own agents, Trace Intelligence has helped us prioritize fixes, select traces for evals, and verify that changes improve user experience.


## Embedding and clustering traces


Trace Intelligence takes completed traces, extract a compact trace representation along with metadata, generates signals per trace including` goal` ,` sentiment` ,` behavior` , and` outcome` , embed each signal and cluster similar signals together.


We use UMAP for dimensionality reduction of signal embeddings and HDBSCAN for density-based clustering in the reduced space. Then, we serve time-windowed views showing theme volume, trends, cross-signal flows, history, and representative traces.


## Completing the agent learning loop


Trace intelligence is our second step towards agent learning.


The full agent accuracy loop is something like:


- **review traces**
- **create datasets**
- **experiment** with potential fixes;
- **ship** fixes to prod


Back in March, we shipped Datasets and Experiments. Now, Trace Intelligence helps with the trace review and selection.


After this, we’ll be working on automating the experiment flow by generating proposed fixes. Soon, you’ll be able to automate the entire agent learning loop within Mastra.


## Get Started


Sign up for the beta here:[mastra.ai/trace-intelligence](https://mastra.ai/trace-intelligence) .
