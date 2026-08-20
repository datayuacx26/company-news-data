---
schema_version: "1.0.0"
document_id: "8919805439bd7ab7c00338ce5a7954b27feb075ad69d509952559b493ae0f6c3"
company_key: "yc-icepanel"
company: "IcePanel"
source_id: "yc-icepanel-news-import-9cf2a09ec197"
canonical_url: "https://icepanel.io/blog/2025-12-02-seezo-integration-with-icepanel"
published_at: "2025-12-02T00:00:00+00:00"
first_seen_at: "2026-07-21T23:29:04.459524+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:7f637a7d25add91e131d3a3dc7770b91c3e62ad23b0b1e3297e6f7890cb4fe0e"
---

# New — Seezo integration with IcePanel 🧊

Security teams have relied on static diagrams as the foundation for activities like threat modelling and audits. Teams would furiously spend hours and weeks consolidating product specs, architecture docs, and tasks in project management tools to produce artifacts like data flow diagrams.


This worked with the old way of building monolithic systems with slower deployment cycles. However, with the shift to modern practices like microservices, cloud, and CI/CD, diagrams now get out of date almost instantly after they’re done.


## What is Seezo?


[Seezo](https://seezo.io/) solves this problem with a context-first (LLM) approach. With a strong baseline of security context, Seezo extracts information from unstructured data (docs, JIRA epics, diagrams) to analyze systems instantly. This workflow enables AppSec teams to evaluate designs with faster feedback cycles continuously. Security becomes something teams can think about upfront, instead of as an afterthought.


## How does the IcePanel integration work?


Seezo now integrates with IcePanel, allowing you to add diagrams in an assessment. As your model remains continuously up to date in IcePanel, assessments will always pull the latest context.


Compared to traditional diagramming tools, IcePanel’s structured model allows teams to capture their architecture views comprehensively in a single place. Context is everything when it comes to LLMs, which means Seezo can produce more accurate results with the latest information.


---


Want to get started with Seezo using IcePanel? Follow these steps.


## Setup Instructions


To get started:


1. Sign up for a[Seezo](https://seezo.io/) account
2. In IcePanel, navigate to Settings > API keys
3. Create an API key (Read)
4. Copy the API token
5. Go to Seezo and navigate to Config > IcePanel
6. Paste the API token and save


Once you’re all setup:


1. Create a new assessment and click on the IcePanel button
2. Copy a share link in IcePanel of the diagram you want added.
3. Paste the share link in Seezo and start the assessment. Seezo will also include children diagrams in the assessment.


## Share your thoughts 💭


IcePanel works with[Seezo](https://seezo.io/) today. Give it a try and share your feedback —mail@icepanel.io 📩


Stay chill,


The IcePanel team 🧊
