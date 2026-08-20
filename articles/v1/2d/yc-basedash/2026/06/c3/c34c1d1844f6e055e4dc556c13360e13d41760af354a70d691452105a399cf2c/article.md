---
schema_version: "1.0.0"
document_id: "c34c1d1844f6e055e4dc556c13360e13d41760af354a70d691452105a399cf2c"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/bi-tools-for-dbt-how-to-choose-without-duplicating-your-metrics/"
published_at: "2026-06-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:c81c2c30991e238da7c2e72e7255b7bcb216488ad726a8bf3e35020030d0fdf1"
---

# BI tools for dbt: how to choose without duplicating your metrics

The best BI tool for a dbt stack is the one that reads your existing dbt models instead of making you rebuild them. If your analytics engineer already defines metrics in dbt, you want a BI layer that consumes those definitions, not one that quietly becomes a second source of truth where` active_customers` means something slightly different.


This guide is for data teams and analytics engineers who run dbt (Core or Cloud) and are choosing what sits on top of it. It explains the three ways a BI tool can connect to dbt, the real tradeoff between the dbt Semantic Layer and a BI tool’s own semantic layer, how specific tools approach the integration, and a framework for picking one without ending up with duplicated metric logic.


## TL;DR


- “dbt-native” is ambiguous. Some tools read your dbt models and metrics directly, some consume the dbt Semantic Layer over an API, some only sync descriptions, and most just query the tables dbt built and ask you to redefine metrics in their own layer.
- The dbt Semantic Layer (powered by MetricFlow) lets you define metrics once in dbt and query them from tools like Tableau, Hex, Mode, Power BI, and Google Sheets. It requires dbt Cloud. dbt Core users can query MetricFlow locally on the command line but cannot use the APIs or downstream integrations. ([dbt docs](https://docs.getdbt.com/docs/use-dbt-semantic-layer/sl-faqs) )
- [Lightdash](https://docs.lightdash.com/get-started/develop-in-lightdash/adding-tables-to-lightdash) is the most directly dbt-native option: metrics and dimensions live in your dbt YAML and the BI layer generates itself from the project. It assumes a well-maintained dbt repo.
- Looker, Power BI, and most traditional BI tools have their own modeling layer (LookML, DAX). Used on top of dbt, they create a second place where metrics are defined.
- If your canonical metrics are already materialized as dbt models (wide mart tables), a tool that queries those tables directly, like[Basedash](https://www.basedash.com/) , avoids a separate semantic layer to keep in sync.


## What “dbt-native” actually means


Vendors use “dbt integration” to describe very different things, and the difference decides whether you maintain one metric definition or two.


dbt does two jobs that matter here. It **transforms** data into models (tables and views in your warehouse), and it **documents** those models with descriptions, tests, column metadata, and lineage stored in artifacts like` manifest.json` . On top of dbt Cloud, the **dbt Semantic Layer** adds a third job: defining metrics in YAML with MetricFlow so they can be queried consistently from downstream tools.


A BI tool can plug into any of these layers, or none of them. “Reads dbt natively” might mean it parses your full project and uses your metric definitions, or it might just mean it imports column descriptions while you rebuild every metric by hand. Both get marketed the same way.


## The four ways a BI tool can work with dbt


When you evaluate a tool, figure out which of these it actually does. This is the single most useful question to ask a vendor.


### 1. Read the dbt project directly


The BI tool points at your dbt repo, parses the compiled project, and turns your models into queryable tables. Metrics and dimensions are defined in your dbt YAML, and the tool generates SQL against the warehouse from those definitions. Lightdash is the clearest example: every column declared in a model’s YAML becomes a dimension, descriptions flow through as hover text, and metrics live in` meta` blocks alongside the models. Your dbt project is the semantic layer.


This is the tightest form of integration. The cost is that the whole experience assumes a maintained dbt project, and every metric change is a dbt pull request rather than a click in the UI.


### 2. Consume the dbt Semantic Layer over an API


The BI tool connects to the dbt Semantic Layer through its JDBC or GraphQL API and queries metrics defined with MetricFlow. The metric logic stays in dbt; the BI tool sends a request like “revenue by month for the last year” and MetricFlow generates and runs the SQL. Tableau, Hex, Mode, Power BI, Google Sheets, and Excel all have integrations of this kind. ([dbt docs](https://docs.getdbt.com/docs/platform-integrations/avail-sl-integrations) )


This keeps a single definition of each metric while letting you use a mainstream BI front end. The constraint is that the Semantic Layer and its integrations require dbt Cloud, and you have to actually adopt MetricFlow to define your metrics.


### 3. Sync dbt metadata, but define metrics elsewhere


The BI tool imports descriptions, primary and foreign keys, and semantic types from dbt so business users see good documentation, but the metrics themselves are still built inside the BI tool. Metabase is the common case: it has no native dbt metadata ingestion, but the open-source[dbt-metabase](https://github.com/gouline/dbt-metabase) tool propagates model and column descriptions, relationships, and semantic types from` manifest.json` into Metabase, and can extract Metabase questions back into dbt as exposures.


You get consistent documentation and lineage, but metric definitions can still drift because they live in two places.


### 4. Ignore dbt and query the tables it built


Most traditional BI tools fall here. They connect to the warehouse, see the tables and views dbt produced, and ask you to define measures in their own modeling layer. Power BI uses DAX, Looker uses LookML, and others use calculated fields. dbt is just the thing that built the tables. This works, and plenty of teams run it happily, but it means` gross_margin` is defined in dbt for the data team and again in the BI tool for everyone else.


There is a variant of pattern 4 worth calling out: if your team already builds wide, denormalized “mart” tables in dbt that contain the metrics pre-computed, then querying those tables directly is not duplication. The metric still lives in dbt. The BI tool is just reading the column.


## dbt Semantic Layer vs a BI tool’s built-in semantic layer


This is the question most dbt-first teams are really asking: should metrics live in the dbt Semantic Layer, or in the BI tool’s own layer?


A[semantic layer](https://www.basedash.com/blog/what-is-a-semantic-layer-the-complete-guide-for-modern-bi) is where metric definitions, dimensions, and relationships are centralized so every dashboard computes the same number the same way. The dbt Semantic Layer puts that layer in dbt. A BI tool’s built-in layer (LookML in Looker, models in many modern tools) puts it in the BI tool.


Consideration dbt Semantic Layer (MetricFlow) BI tool’s built-in layer


Where metrics are defined dbt project, in YAML The BI tool, in its own modeling language


Reuse across tools Any tool that connects to the API Locked to that BI tool


Version control Git, alongside your models Depends on the tool; varies


Requires dbt Cloud for APIs and integrations A license for that BI tool


Who edits metrics Analytics engineers via pull requests Often analysts in the UI


Risk of a second definition Low, if every tool reads it High, if dbt also defines the same metric


The honest answer depends on how committed you are to dbt as the center of your stack. If you want metrics defined once and consumable from a notebook, a spreadsheet, and a dashboard, the dbt Semantic Layer wins because the definition is not tied to one front end. If you only use one BI tool and your analysts need to create and edit metrics quickly without filing pull requests, a built-in layer can be faster day to day, at the cost of a second place where logic lives.


What you want to avoid is the accidental middle: defining metrics in dbt and then redefining the same metrics in the BI tool because the integration was metadata-only. That is the[metric drift](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) problem, and it is the source of most “why do these two dashboards disagree” tickets.


## How specific BI tools approach dbt


The table below maps common tools to the patterns above. “Where metric logic lives” is the column that matters most for avoiding duplication.


Tool How it connects to dbt Where metric logic lives Best for


Lightdash Reads the dbt project directly; metrics in dbt YAML dbt repo Teams that want BI defined as code in the same repo as dbt


Tableau dbt Semantic Layer connector (Tableau Cloud) dbt (MetricFlow) Existing Tableau shops that want governed dbt metrics


Hex / Mode dbt Semantic Layer via SQL cells / API dbt (MetricFlow) Analysts who work in notebooks and SQL


Power BI dbt Semantic Layer integration, or DAX on warehouse tables dbt or Power BI Microsoft-centric orgs


Metabase Community` dbt-metabase` syncs descriptions and keys Metabase (metrics rebuilt) Self-serve dashboards where dbt is documentation, not the metric source


Looker Queries warehouse tables; modeling in LookML LookML Teams committed to LookML as the semantic layer


Cube Headless layer that can import dbt models, serves metrics to many tools Cube (can mirror dbt) Serving one metric definition to several BI front ends


Basedash Connects to the warehouse and queries the models dbt builds, with AI dbt models (marts) dbt teams whose canonical metrics are materialized as tables, and non-technical users who ask questions in plain English


A few clarifications so the table is not read too literally:


- **The dbt Semantic Layer requires dbt Cloud.** If you run dbt Core, the API-based integrations (Tableau, Hex, Mode, Power BI through MetricFlow) are not available to you. You can still query MetricFlow locally, and you can still use pattern 1 (Lightdash) or pattern 4 (query the tables).
- **Looker’s LookML predates dbt and overlaps with it.** Many teams run both and treat dbt as the transformation layer and LookML as the metrics layer. That is a deliberate choice, not a bug, but it is two layers.
- **Basedash does not consume the dbt Semantic Layer API today.** It connects to your database or warehouse and lets people query the tables and views dbt produced, including with natural-language questions that turn into SQL. It fits teams that keep their source-of-truth metrics in dbt models rather than in MetricFlow, and want non-technical teammates to explore that data without re-modeling it.


## How to choose: a short decision framework


Pick based on where you want metrics to live and who needs to use the result.


**Choose a tool that reads the dbt project directly (e.g. Lightdash) when:**


- Your team is comfortable defining metrics as code and reviewing them in pull requests.
- You want BI logic versioned in the same repo as your dbt models.
- You are happy to self-host or run a tool whose value depends on a healthy dbt project.


**Choose a tool that consumes the dbt Semantic Layer (Tableau, Hex, Mode, Power BI) when:**


- You are on dbt Cloud and willing to adopt MetricFlow.
- You already use one of these front ends and want it to read governed dbt metrics.
- You need the same metric available in dashboards, notebooks, and spreadsheets.


**Choose a tool that queries dbt-built tables directly (e.g. Basedash) when:**


- Your canonical metrics are already materialized as dbt models or mart tables.
- You want non-technical teammates to ask questions and build views without writing SQL or learning a modeling language.
- You do not want to run or pay for a separate semantic layer service.


**Be cautious about a tool with its own heavy modeling layer (Looker, Power BI in DAX mode) when:**


- You are dbt-first and want one definition per metric. You can make it work, but you are accepting a second layer and the governance to keep them aligned.


If you are still mapping out where each metric should live before you even pick a front end, start with[where to define business metrics](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) , then come back to tool selection.


## Common mistakes


- **Treating “dbt integration” as one feature.** Ask whether the tool reads your metrics, reads the semantic layer, or only reads descriptions. The marketing word is the same for all three.
- **Adopting the dbt Semantic Layer on dbt Core.** The API integrations need dbt Cloud. If you are on Core and want pattern 2, budget for the upgrade or pick a different pattern.
- **Rebuilding metrics in the BI tool “just for now.”** Temporary BI-side metrics become permanent and drift from dbt. Decide where each metric lives and enforce it.
- **Ignoring who edits metrics.** If analysts need to ship metrics in minutes, an all-pull-request workflow will frustrate them. If governance matters most, a UI-editable layer will scare your data team. Match the tool to the team.
- **Skipping exposures.** Whatever tool you pick, declare dashboards as dbt exposures (or sync them) so you can see which reports break when a model changes.


## FAQ


**What is the best BI tool for dbt users?**


There is no single best one. If you want metrics defined as code in your dbt repo, Lightdash is the most dbt-native. If you are on dbt Cloud and want a mainstream front end, Tableau, Hex, Mode, or Power BI can consume the dbt Semantic Layer. If your metrics are already materialized as dbt mart tables and you want non-technical self-serve, a tool that queries those tables directly, like Basedash, avoids a second semantic layer.


**Can I use the dbt Semantic Layer with dbt Core?**


You can run MetricFlow and query metrics locally from the command line, but the APIs and downstream BI integrations require dbt Cloud. ([dbt docs](https://docs.getdbt.com/docs/use-dbt-semantic-layer/sl-faqs) )


**Which BI tools connect to the dbt Semantic Layer?**


dbt lists integrations including Tableau, Hex, Mode, Power BI, Google Sheets, and Excel, plus any tool that can connect over the JDBC or GraphQL APIs. ([dbt docs](https://docs.getdbt.com/docs/platform-integrations/avail-sl-integrations) )


**How do I avoid defining metrics twice, once in dbt and once in my BI tool?**


Pick one of two paths. Either define metrics in dbt (as models or in MetricFlow) and use a tool that reads them, or define metrics in the BI tool and keep dbt to transformations only. The trap is doing both, which happens when the integration is metadata-only and you redefine metrics in the BI layer out of habit.


**Does Metabase read dbt models?**


Not natively for metrics. The open-source` dbt-metabase` tool can sync model and column descriptions, keys, and semantic types from` manifest.json` into Metabase, and export Metabase questions back to dbt as exposures. Metrics themselves are usually rebuilt inside Metabase. ([dbt-metabase](https://github.com/gouline/dbt-metabase) )


**Do I need a semantic layer at all if I build wide mart tables in dbt?**


Often not. If your dbt models already pre-compute metrics into denormalized tables, a BI tool can read those columns directly and the metric still lives in dbt. A separate semantic layer earns its keep when you need to combine dimensions and metrics flexibly across many tables without materializing every combination.


## The short version


Decide where metrics live before you pick a tool. If they live in dbt, choose something that reads dbt (the project or the Semantic Layer) so you keep one definition. If you are fine defining metrics in the BI tool, treat dbt as your transformation layer and move on. The expensive mistake is picking a tool whose dbt “integration” only syncs descriptions, then rebuilding every metric by hand and spending the next year reconciling two numbers that should have been one.
