---
schema_version: "1.0.0"
document_id: "ecb199d9eba4396352d9b972b5e9e7b057384d7a0bca9671e276092adb581769"
company_key: "yc-confident-ai"
company: "Confident AI"
source_id: "yc-confident-ai-news-import-3974c9e901d3"
canonical_url: "https://www.confident-ai.com/blog/launch-week-q2-2026-day-2-workflows"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-21T14:44:45.986766+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:74cf95ad086bbce36e97e4e9ff302b993e415d84f5fd2fd80463010267abe478"
---

# Introducing AI Observability Workflows: Custom automations for every trace on the platform

Blog


# Introducing AI Observability Workflows: Custom automations for every trace on the platform


Jun 23, 2026


·


5 min read


Jeffrey Ip


Co-founder @ Confident AI. Creator of DeepEval & DeepTeam. Building an unhealthy LLM evals addiction. Ex-Googler (YouTube), Microsoft AI (Office365).


Today we're launching **AI Observability Workflows** on Confident AI — a graph based interface for managing everything that happens after your traces, spans, and threads hit the platform.


Dataset ingestion, queue ingestion, evaluation rules, and classifiers aren't new; you've been able to auto-ingest data and run online evals for a while. What's new is that they now live in **one place** — a graph-based editor where you stitch these tasks together into a pipeline and define the order they run in.


## What is Observability Workflows?


Workflows gives you a single view of your entire post-ingestion pipeline. Incoming traces, spans, and threads sit at the top of a graph, and everything you've configured to run on them — ingestion tasks, evaluation rules, and classifiers — branches off below.


Use the **Traces** , **Spans** , and **Threads** buttons at the top to scope the page to a specific entity type. The graph and every tab update to show only the workflows relevant to that type, so you're always looking at one coherent pipeline rather than four disconnected settings screens.


Below the graph are four tabs — **Dataset Ingestion** , **Queue Ingestion** , **Evaluation Rules** , and **Classifiers** — each managing one kind of downstream task.


## The graph editor: stitch tasks into an order of operations


The graph isn't just a picture of what you've configured — it's how you wire tasks together. Each task is a node, and the order you connect them in is the order they execute when data arrives.


That ordering matters, because tasks build on each other. A few examples:


- **Evaluate, then ingest** — run an evaluation rule first, then ingest only the traces that scored below your threshold into a dataset, so your golden set fills with real failures instead of noise.
- **Classify, then route** — classify incoming traces first, then route everything labelled a security risk or negative sentiment straight into an annotation queue for human review.
- **Classify, then evaluate** — label by use case first, then run the metric collection that actually fits that use case.


You decide the sequence. Drag tasks into the order you want, and Workflows runs them as a pipeline — each step operating on the results of the one before it — rather than as four independent jobs that happen to fire on the same data.


## Dataset ingestion


Dataset ingestion tasks continuously ingest matching traces, spans, or threads into a dataset as goldens. Each task runs automatically against incoming data and adds qualifying items without manual curation.


To create one:


1. Select **Traces** , **Spans** , or **Threads** at the top of the page
2. Open the **Dataset Ingestion** tab and click **New ingestion task**
3. In the side drawer, pick the target dataset, set filters, and name the task
4. Save


Each row shows the task's name, target dataset, data model, and golden count. Use the toggle to pause a task without deleting it, or the edit and delete icons to manage it.


## Queue ingestion


Queue ingestion tasks route matching traces, spans, or threads into an[annotation queue](https://www.confident-ai.com/docs/llm-tracing/workflows) for human review — so the examples worth a closer look reach reviewers automatically.


To create one:


1. Select **Traces** , **Spans** , or **Threads**
2. Open the **Queue Ingestion** tab and click **New ingestion task**
3. Choose the target annotation queue and configure the task in the side drawer
4. Save


Rows show the task name, target queue, data model, and how many items have been ingested so far. Toggle, edit, and delete work exactly as they do for dataset ingestion.


## Evaluation rules


Evaluation rules run a metric collection on incoming data at ingest time — no code changes required. They're a no-code complement to inline evaluation: a rule only fires when the SDK call that produced the data **didn't** already pass a` metric_collection` . If the SDK supplies one, that value wins and the rule is skipped.


To create a rule, open the **Evaluation Rules** tab, click **New rule** , and configure it in the side drawer. The key fields:


- **Data Model** —` Trace` ,` Span` , or` Thread` . Trace and span rules run at ingest on each item and require a single-turn collection; thread rules require a multi-turn collection and run once a thread has been idle for a configurable **Time Limit** (default` 300s` ).
- **Metric Collection** — the collection to run on matching items.
- **Filters** — scope the rule by environment, tags, metadata, latency, and more. Leave empty to match everything.
- **Sample Rate** — the fraction of matching items the rule fires on (` 0.0` –` 1.0` ). Sampling is deterministic, so the same item always makes the same decision.


For threads, evaluation rules are the primary way to evaluate automatically — there's no inline SDK parameter that triggers a thread-level eval. Note that only one enabled thread rule can target a given metric collection at a time.


## Classifiers


Classifiers assign labels to traces and threads as they're ingested, based on a description and a set of labels you define. Those labels surface as[Signals](https://www.confident-ai.com/docs/llm-tracing/workflows) and as filterable dimensions across the Observatory and Dashboards. (Classifiers aren't available for spans — switch to **Traces** or **Threads** to manage them.)


There's no rule engine or regex under the hood. When a classifier runs, the model sees the classifier's description, each label's description, and the trace or thread payload, then picks one label or returns "no match." Specificity is everything — concrete label descriptions ("label as` Negative` if the user expresses frustration or restates the same question after a wrong answer") drive accuracy far more than anything else.


A few things worth knowing:


- **Generate Labels** proposes a starter set from your recent data via a summarize → cluster → label pipeline, so you don't have to define a taxonomy from scratch. Recommended labels show **Accept** / **Decline** actions.
- **Auto Classify** lets a classifier propose new labels when none of your existing ones fit — leave it on to keep discovering edge cases, off for a fixed taxonomy.
- **Sample Rate** is a project-wide setting controlling what fraction of incoming items get classified, and each classification logs a usage event you can track under **Project Settings → Data Usage** .


## Get started


Workflows is live on Confident AI now.


- Read the[Workflows documentation](https://www.confident-ai.com/docs/llm-tracing/workflows) for the full reference on every tab, field, and option
- Open the **Workflows** page in your project to see the graph for your own traces, spans, and threads


And keep an eye on the blog this week — we've got more shipping.


---


Do you want to brainstorm how to evaluate your LLM (application)? Ask us anything in our[discord](https://discord.com/invite/a3K9c8GRGt) . I might give you an "aha!" moment, who knows?


## Standardize AI Quality for the entire org, not just individual teams


Give all AI use cases the same quality bar with all-in-one evals, observability, and red teaming, and enforce them at scale.


AI evals for product teams, not just engineers.


Observability for production traffic.


Red teaming for security and safety.


AI governance for multiple projects at once.


[Book a Demo](https://www.confident-ai.com/book-a-demo?utm_source=blog&utm_medium=content&utm_campaign=q2_2026_launch_week&utm_content=product)[Or sign up](https://app.confident-ai.com/auth/signup?utm_source=blog&utm_medium=content&utm_campaign=q2_2026_launch_week&utm_content=product)


In this story


- What is Observability Workflows?
- The graph editor: stitch tasks into an order of operations
- Dataset ingestion
- Queue ingestion
- Evaluation rules
- Classifiers
- Get started
