---
schema_version: "1.0.0"
document_id: "9489605fae89c9543784cf64cd5af8de8b78e984ce41f79a65d0caebce2d5f52"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations/"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:08:49.880649+00:00"
content_hash: "sha256:851b49730604abe2d6044d41ac5626031a1ea2ec815f536c61030d26f6b618ff"
---

# Where to define business metrics: SQL views, dbt, semantic layers, or BI tool calculations

There are four places business metrics typically get defined: as **SQL views** in the warehouse, as **dbt models** , in a **semantic layer** , or as **calculated fields inside a BI tool** . Most teams end up using two or three of these at once — often by accident — and then spend the next year wondering why three dashboards report three different numbers for active customers.


This guide is for analytics engineers, data leads, and founders who are deciding where business logic should live as their data stack grows beyond a single dashboard. It explains what each layer actually does, when to choose it, and how to combine them without ending up with metric drift. The short version is at the bottom in a decision matrix; the longer version below explains the reasoning so you can adapt it to your stack.


## The problem: the same metric, defined four times


Ask three people at most growing companies how revenue is calculated and you will get three subtly different answers. One person joined` invoices` to` subscriptions` , excluded refunds after 30 days, and called the result “MRR.” Another wrote a Looker explore that includes refunds but excludes trial conversions. A third built a Stripe-only number in a Notion doc that nobody updates anymore.


This is not a tooling problem. It is a placement problem. Business logic — what counts as a customer, when revenue is recognized, which users are “active” — has to live somewhere specific, and every other tool has to defer to that place. When it does not, you get[metric drift](https://www.basedash.com/blog/what-is-a-semantic-layer-the-complete-guide-for-modern-bi) : the same name attached to slightly different numbers in different systems.


The four places metrics commonly get defined each have different strengths. Picking the right one matters more than the specific technology.


## Option 1: SQL views in the warehouse


A SQL view is a saved query stored in the warehouse itself — PostgreSQL, Snowflake, BigQuery, Redshift, or ClickHouse. Anyone querying the warehouse sees the view as if it were a table.


This is the oldest pattern and still the most underrated. A view called` analytics.active_customers` is queryable from every BI tool, every notebook, every script, and every AI assistant that connects to the database. There is no extra service to run.


**When SQL views work well**


- You have one warehouse and most of your tools query it directly.
- Your transformations are mostly joins, filters, and simple aggregations.
- You want metric definitions to be visible to anyone with SQL access.
- You do not yet have a dedicated analytics engineering function.


**When SQL views start to hurt**


- You need dependency tracking. Views that depend on other views become hard to refactor without breaking dashboards.
- You need tests. Views do not have a native concept of “this column should never be null” or “this primary key should be unique.”
- You need versioning and code review. Editing a view in a database client leaves no audit trail.
- Performance matters and you need materialization, incrementality, or partition strategies.


A common pattern is to start with views and migrate to dbt when the view dependency graph becomes painful — usually somewhere between 30 and 100 models. The signal is when you find yourself afraid to change a view because you do not know what else depends on it.


## Option 2: dbt models


dbt is a transformation framework that compiles modular SQL into materialized tables and views in your warehouse, with tests, documentation, and a dependency graph (DAG). Models are versioned in git and run on a schedule.


dbt is where most modern data teams put core business logic today. Metrics like` mrr` ,` active_customer` ,` signup` , or` qualified_lead` end up as columns or rows in dbt-managed tables that the rest of the stack reads from.


**When dbt fits**


- You have multiple analysts or analytics engineers and want code review on metric changes.
- You need data quality tests on key columns (uniqueness, not-null, accepted values, referential integrity).
- You want documentation generated from the same place as the code.
- Your transformations have meaningful dependencies and you want a DAG to manage them.
- You are happy materializing tables on a schedule (every 15 minutes, hourly, daily) rather than computing metrics live.


**The dbt tradeoff**


dbt builds tables. Tables are great for performance and discoverability, but they are not “live.” A metric defined in a dbt model is as fresh as the last dbt run. For most reporting that is fine — daily and hourly freshness covers almost every business question. For real-time operational dashboards (alerting, anomaly detection, anything event-driven), you need either a faster dbt schedule or a different approach entirely.


dbt also does not, by itself, prevent two BI tools from defining the same metric differently. If your BI tool re-aggregates a dbt model with a slightly different filter, you are back to drift. dbt is necessary for managing transformations; it is not sufficient for guaranteeing one metric definition across consumers. That is what a semantic layer is for.


## Option 3: a semantic layer


A semantic layer sits between the warehouse (or dbt models) and the tools people query with. It defines metrics, dimensions, and joins in one place, then exposes them through APIs, SQL, or native integrations to dashboards, notebooks, embedded analytics, AI assistants, and spreadsheets.


The classic example is LookML in Looker. Newer options include the dbt Semantic Layer (formerly MetricFlow), Cube, AtScale, and the semantic layers built into modern BI tools like Omni, Lightdash, and[Basedash](https://www.basedash.com/features/semantic-layer) , where reusable SQL definitions are referenced by name and used to ground AI-generated queries. We’ve written a longer primer on[what a semantic layer is](https://www.basedash.com/blog/what-is-a-semantic-layer-the-complete-guide-for-modern-bi) for teams new to the concept.


**The unique thing a semantic layer does**


A semantic layer lets you write` revenue` once and have every consumer — dashboards, ad-hoc SQL, AI assistants — get the same number. Filters, slicers, and joins are applied consistently. If you change the definition of “active customer,” every downstream tool reflects the change without anyone editing twelve dashboards.


This matters more now than it used to, because AI assistants increasingly generate ad-hoc queries against your warehouse. Without a semantic layer, an AI tool answering “what was revenue last quarter?” is just writing a fresh SQL query against raw tables and hoping it gets the joins and filters right. With one, it asks for` revenue` and trusts the definition.


**When a semantic layer is worth it**


- You have more than one consumption tool (BI plus notebooks plus an AI assistant plus a customer-facing dashboard).
- Metric consistency is a recurring problem in board meetings or all-hands.
- You want non-technical users to slice metrics safely without writing SQL.
- You are deploying AI-powered analytics and want it constrained to approved definitions.


**When it is overkill**


- You have one BI tool and one warehouse and three people who all know the SQL.
- You move fast on metric definitions and the overhead of editing a semantic model slows you down.
- Your metrics are not that contested. Some businesses simply have unambiguous core metrics and rarely argue about them.


The honest tradeoff is that semantic layers add a real layer of indirection. Defining a metric becomes a multi-file change instead of a one-line SQL edit. For small teams, that overhead can outweigh the benefit of consistency.


## Option 4: calculated fields inside a BI tool


Every BI tool — Tableau, Power BI, Looker, Metabase, Mode, Hex, Omni, Sigma, Basedash — lets you define calculated fields, custom metrics, or formulas inside the tool. These are quick to create and live inside dashboards.


This is the layer most prone to drift, and also the layer most teams use for “just one more metric we needed for the board meeting.” Both things are true at once.


**When BI tool calculations are appropriate**


- The metric is genuinely dashboard-specific (a one-off ratio for one chart).
- You are prototyping a new metric before promoting it to a shared layer.
- You need to combine a warehouse metric with a slicer, filter, or threshold that only matters for one report.
- Speed matters more than canonicalization for that particular question.


**When BI tool calculations cause problems**


- The metric gets reused across dashboards by copy-pasting the formula. Now it lives in five places.
- The metric is foundational (revenue, customers, churn) and gets quietly redefined in a corner of a dashboard.
- Different BI tools or teams maintain their own version of the same calculation.


A good rule: if more than one dashboard needs the metric, it should not live only in a BI tool’s calculated fields. Promote it down the stack — to dbt, a semantic layer, or at least a warehouse view — and let the BI tool reference it.


## A decision framework


Use this matrix to choose where a given metric should live. Read each row as “if this is true of the metric, define it here.”


If the metric is… Define it here


One-off, dashboard-specific, exploratory BI tool calculation


Reused across 2+ dashboards but tied to one team dbt model or warehouse view


Used by multiple teams, multiple tools, and quoted externally Semantic layer (with dbt underneath)


Operational (real-time, event-driven, alerting) Streaming pipeline + warehouse view


Compliance- or finance-critical (revenue recognition, regulated metrics) dbt model with tests + semantic layer


Used by AI assistants or natural-language queries Semantic layer (so AI gets one answer)


Two practical principles sit underneath this matrix:


1. **Push canonical metrics as far down the stack as is practical.** The further down a metric lives, the more consumers automatically inherit it. A metric in a BI dashboard is visible to that BI tool. A metric in a dbt model is visible to anything that reads from the warehouse. A metric in a semantic layer is visible to anything that asks for it by name.
2. **Push exploratory metrics as far up the stack as is appropriate.** It is fine for new metrics to start as BI tool calculations or ad-hoc SQL. Promote them down only when they prove durable.


## A typical stack for a 10-100 person company


For most companies between 10 and 100 people, the practical answer is some version of this:


- **Warehouse** (PostgreSQL, Snowflake, BigQuery, or Redshift) holds raw data, replicated from production databases, Stripe, HubSpot, Segment, and so on.
- **dbt** owns transformations and core metric tables:` fct_revenue` ,` dim_customer` ,` dim_account` ,` fct_signups` . Tests run on each model. Documentation is generated automatically.
- **A semantic layer** (dbt Semantic Layer, Cube, or one built into the BI tool) exposes canonical metrics to consumers. This becomes valuable as soon as the second or third query interface appears — typically when AI-assisted analytics enters the picture.
- **BI tool calculations** are reserved for dashboard-specific tweaks: ratios, thresholds, what-if filters. Anything reused gets promoted to dbt.


Smaller teams can collapse this stack. A two-person startup might run PostgreSQL with a handful of views and a BI tool on top, no dbt, no semantic layer. That is fine. The point is not the number of layers — it is knowing which layer owns each metric.


## Common mistakes


A few patterns show up over and over.


- **Defining the same metric in dbt and in the BI tool.** Pick one. If dbt owns` mrr` , the BI tool should reference the dbt column, not recompute it. If the BI tool needs a variation (MRR excluding a specific plan), define it as` mrr_excluding_enterprise` in dbt rather than inventing a new formula in a dashboard.
- **Skipping the semantic layer “for now” and inventing it inside the BI tool.** Tools like Looker, Omni, and Tableau all let you define metrics that look like a semantic layer but are locked inside the tool. That works until you add a second consumer (an AI tool, a notebook, a customer-facing dashboard). Then you discover the metrics cannot be queried from anywhere else.
- **Treating warehouse views as a substitute for dbt without thinking about dependencies.** Views nest fine until they do not. A view depending on three other views depending on five other views is harder to refactor than a comparable dbt DAG.
- **Pushing real-time metrics into dbt and being disappointed by freshness.** dbt is built on batch runs. If you need sub-minute freshness, you need a streaming pipeline, not a faster dbt cron.
- **Not writing down who owns each metric.** This is a process problem, not a tooling problem, but it kills more metric trust than any of the above. Each canonical metric should have a named owner and a definition document.


## How AI changes the equation


AI-assisted analytics raises the stakes for metric placement. When a person writes SQL against your warehouse, they bring context — they know which tables to join, which filters to apply, and which edge cases to exclude. When an LLM writes SQL against the same warehouse, it does not know any of that unless you tell it.


There are two ways to give an AI tool that context. The first is to put canonical metrics in a semantic layer the AI is constrained to query. The second is to make the warehouse self-describing enough — through naming conventions, dbt documentation, and views with descriptive column names — that the AI rarely has to guess.


Most teams will need some of both. A semantic layer for the metrics that matter most (revenue, customers, churn, key product metrics) and well-documented warehouse tables for the long tail. Tools like Basedash, Hex, and Omni all read from dbt models and semantic layers and use them to constrain AI-generated queries, which is the pattern worth designing toward. Basedash also ships its own[built-in semantic layer](https://www.basedash.com/features/semantic-layer) — reusable SQL definitions referenced by name — so teams that don’t run dbt or a standalone layer can still give the AI a governed set of metrics to query against, all inside the same product that handles storage, syncing, dashboards, and reporting.


For more on how this shows up in practice, see our guides to[self-serve analytics](https://www.basedash.com/blog/self-serve-analytics-a-practical-guide-to-bi-adoption-across-your-organization) and[conversational BI tools](https://www.basedash.com/blog/what-are-conversational-bi-tools-the-complete-guide-for-saas-teams) .


## When to combine layers and when to keep it simple


The right answer for any given team depends on size, tool sprawl, and how contested metrics are inside the company. A useful test: if your CEO, head of finance, and head of marketing each pulled up their go-to dashboard right now, would the top-line revenue number match?


If yes, your current setup is probably fine, even if it is only views and BI tool calculations.


If no, the fix is structural, not cosmetic. Pick a place for canonical metrics — usually dbt for the underlying logic, with a semantic layer on top if you have more than one consumer — and migrate the contested metrics there one at a time. Most teams do not need to do this all at once. They need to do it for the five or ten metrics that matter most, and let the rest stay where they are.


Metric placement is one of those decisions that looks like a tooling choice and is actually an organizational one. The tools are mature. What matters is being explicit about where each metric lives, who owns it, and what every other system should defer to.


## FAQ


**Do I need a semantic layer if I am already using dbt?**


Not necessarily. dbt gives you consistent metric *tables* . A semantic layer gives you consistent metric *answers* across multiple consumers. If everyone queries the same dbt tables through the same BI tool, you may not need a semantic layer yet. If you have multiple BI tools, notebooks, embedded analytics, or AI assistants all asking the same business questions, a semantic layer becomes valuable.


**Is the dbt Semantic Layer the same as MetricFlow?**


The dbt Semantic Layer is the productized service. MetricFlow is the underlying engine that compiles semantic definitions into SQL. In practice, when people say “the dbt Semantic Layer” they mean both — you define metrics in YAML alongside your dbt models, and the semantic layer exposes them to consumers.


**Can I just use SQL views and skip dbt?**


For small projects, yes. The threshold where views start to hurt is around the point you have dependencies between views and start fearing refactors. If you are there or close to it, dbt is worth the migration.


**Where should I define real-time metrics?**


Not in dbt. dbt’s freshness is set by its run cadence — usually 15 minutes at the fastest in practice. Real-time metrics need to live either in a streaming pipeline (Materialize, RisingWave, Tinybird) feeding the warehouse, or in a tool that queries source systems directly. The semantic layer can still expose them to consumers — the storage and computation just happens elsewhere.


**What if I have an existing tangle of metric definitions across tools?**


Inventory first, migrate second. List every metric used in board reporting, finance close, customer success, and product analytics. For each, write down where it is currently defined and how it is computed. The list itself will surface the conflicts. Pick the top 10 and consolidate them into one place — usually dbt — before worrying about the rest.
