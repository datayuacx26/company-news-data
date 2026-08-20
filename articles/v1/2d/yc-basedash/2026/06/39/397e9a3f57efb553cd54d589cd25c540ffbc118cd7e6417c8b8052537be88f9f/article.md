---
schema_version: "1.0.0"
document_id: "397e9a3f57efb553cd54d589cd25c540ffbc118cd7e6417c8b8052537be88f9f"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/introducing-basedash-semantic-layer/"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:e26db45fc6552fdee2033a9f61faa22eacdaa1c8cfe699a101ea0f97670eb28e"
---

# Introducing the Basedash semantic layer

Today we’re launching the **Basedash semantic layer** — a way to define trusted business metrics in reusable SQL and make them available everywhere AI works in Basedash.


The core feature is called definitions. A definition is a reusable SQL query attached to a data source, with a name, reference name, description, and version history.


Write the SQL for a metric like MRR, activation rate, cohort retention, or qualified pipeline once. Basedash can then reference that exact definition when it creates charts, answers chat questions, builds dashboards, generates insights, or runs automations.


Define your metrics once. Use them everywhere.


## Why we built a semantic layer


AI is excellent at exploring data, but teams still need deterministic calculations for the metrics they run the business on. If “activation rate” means completed onboarding within seven days, every chart, dashboard, and automation should use that same calculation.


Before the Basedash semantic layer, the usual options were copy-pasting SQL, writing prose instructions for the AI, or relying on an external semantic layer that lived outside the product where the analysis happened. Those all work until the metric changes, the SQL drifts, or someone asks the AI a question from a different workflow.


The Basedash semantic layer brings that modeling layer into the same product where your team asks questions, builds dashboards, and creates automations. Your warehouse, synced data, reusable SQL models, AI analyst, BI dashboards, and automations can now live in one place.


## How definitions power the semantic layer


A definition is simple:


1. Go to the **Data** page
2. Select a data source
3. Open **Definitions** above the table list
4. Create a definition with a name, reference name, description, and SQL query


Once saved, reference it in SQL with Liquid syntax:


```text
WITH   mrr   AS   (
{{   definition  (  "mrr"  ) }}
)


SELECT   month  ,   SUM  (amount)   AS   total_mrr
FROM   mrr
GROUP BY   month
ORDER BY   month  ;
```


Basedash expands the definition inline when the query runs. They are scoped to a data source, so the SQL always runs against the same warehouse or database it was designed for.


## AI can create and use definitions


The definitions feature is also part of the AI context. When Basedash works with a data source, it sees the catalog of definitions available for that source. It can inspect a definition, reference it inside generated SQL, and use it when building charts or answering questions.


You can also ask the AI to create a definition:


> Create a definition for activation rate using users who completed onboarding within 7 days.


From there, future prompts like “show activation rate by signup month” can reuse the same SQL. The AI still helps you explore, but the core calculation is now deterministic and auditable.


## What to define first


Start with the numbers that appear in multiple places:


- **Revenue** : MRR, ARR, net revenue retention, gross margin
- **Growth** : Activation rate, signup conversion, trial-to-paid conversion
- **Product** : Cohort retention, feature adoption, active accounts
- **Sales** : Qualified pipeline, win rate, sales cycle length
- **Support** : SLA risk, ticket backlog, escalation rate


If a metric appears in dashboards, board reports, automations, and chat answers, it probably deserves a definition.


## Semantic layer, skills, and AI context


The semantic layer works alongside the rest of Basedash’s AI context system.


Use definitions when the calculation itself should be exact SQL. Use skills or global AI context when the AI needs prose guidance, team preferences, business terminology, or analytical playbooks.


For example, a definition can encode how MRR is calculated. A skill can explain that finance prefers month-over-month comparisons, GAAP terminology, and conservative summary language. Together, they give the AI both the deterministic SQL and the business context around it.


## Getting started


The Basedash semantic layer is available today.


1. [Sign up for Basedash](https://charts.basedash.com/signup) or log in
2. Go to **Data** and select a data source
3. Create a definition for a metric your team uses often
4. Ask chat or the chart builder to use that definition in a new analysis


For more detail, see the[semantic layer feature page](https://www.basedash.com/features/semantic-layer) or the[definitions docs](https://www.basedash.com/docs/features/definitions) .


## What’s next


The Basedash semantic layer is another step toward making Basedash the full data stack for AI-native BI: storage, syncing, semantic modeling, dashboards, reporting, chat, insights, and automations in one product.


**Create your first definition today and make every answer use the same source of truth.**
