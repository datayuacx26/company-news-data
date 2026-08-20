---
schema_version: "1.0.0"
document_id: "c13947bbb34df98e92107941081f9ab179d3990a1cbad6671c3db7ef6a063957"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/bi-as-code-how-to-version-control-dashboards-metrics-and-semantic-models/"
published_at: "2026-05-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:4db6df42be47348bbd72f5f91909d2365fbb4e7d3f7001a3c5a5c32ef9b0e914"
---

# BI as code: how to version control dashboards, metrics, and semantic models

“BI as code” is the practice of defining dashboards, metrics, and semantic models in version-controlled text files instead of through point-and-click editing inside a BI tool. The point is to apply the same workflow engineers use for software (Git history, pull request review, environment promotion, automated tests) to the analytics layer, so that a metric change goes through the same scrutiny as a code change.


This guide is for analytics engineers, founders, and data leads choosing a BI stack and deciding how much of it to manage in Git. It defines what “BI as code” actually means in 2026, breaks BI into the four layers that need version control, compares how the major tools handle each layer, and gives a four-level maturity model you can map your team to. The goal is to help you pick a level that matches your team’s size and risk tolerance, not to argue that everyone needs full CI/CD for their dashboards.


## TL;DR


- BI as code means representing dashboards, metrics, semantic models, and SQL as text files that live in Git, not as records inside a BI tool’s database.
- The four layers that benefit from version control are data models (warehouse), semantic layer, queries, and presentation (dashboards). Most teams version the first two long before the last.
- Looker (LookML), dbt (warehouse models), Cube, and Lightdash are designed code-first. Power BI added serious Git support through Power BI Projects (.pbip / TMDL). Tableau, Sigma, and most “spreadsheet-style” BI tools are still UI-first with limited Git workflows.
- A useful maturity model has four levels: live edits with no history, in-tool version history, code export with manual Git, and source-of-truth in Git with CI/CD. Most small teams should stop at level 2.
- AI-native BI tools like Basedash sit at level 2 by default: shared metric definitions live as text (Skills), but dashboards themselves are managed in the product, not as files. That tradeoff is intentional for teams that want speed over change-management ceremony.


## What “BI as code” actually means


The phrase gets used to mean three different things. Worth distinguishing them before going further.


**Definitions as code.** The metric definitions, semantic models, and SQL transformations that produce numbers live in text files. Changes to “what counts as MRR” go through a pull request. This is the most common interpretation and the one with the clearest engineering payoff.


**Dashboards as code.** The presentation layer (chart definitions, dashboard layouts, filters) is also defined in text. Changing a chart goes through a PR. This is much rarer and harder, because dashboards have visual state (positions, colors, sort order) that is annoying to express well in code.


**Full BI as code.** Definitions, dashboards, permissions, content folders, and lifecycle (dev to staging to prod promotion) all live in Git, and the BI tool is treated as a runtime that gets deployed to. Looker is the closest mainstream example. dbt + Cube + a thin viewer can get you there too.


Most teams who say “we do BI as code” mean the first one. Some teams have the second. Almost no one has the third end-to-end, and the ones who do have a dedicated platform team maintaining it.


## Why version control is hard in traditional BI


Software has a clean abstraction: the source code is the source of truth, and the running program is built from it. Most BI tools have inverted that. The dashboard you see in the tool is the source of truth, and any “export” is a flattened representation of it.


A few specific reasons this is hard:


- **Mixed concerns.** A typical dashboard mixes SQL, presentation, layout, and permissions in one object. Diffing a dashboard means diffing four things at once, three of which (layout, formatting, permissions) aren’t really code changes.
- **Binary or proprietary formats.** Tableau workbooks (.twb / .twbx) are XML but visual reorderings and small UI tweaks produce huge diffs. Power BI’s old .pbix format was effectively a binary blob.
- **Mutable shared state.** BI tools usually let anyone with edit access change a chart in place. There is no notion of a working copy, branch, or staged change.
- **Identity-tied permissions.** Many features (row-level filters, schedules, alerts) reference user IDs from the BI tool’s database. Those don’t round-trip cleanly through Git.
- **Generated artifacts.** Cached query results, materialized snapshots, and rendered images are all part of the running system but shouldn’t be in version control.


A modern BI tool’s Git story is largely about how cleanly it separates “definition” from “state.”


## The four layers of BI that need version control


Pulling the BI stack apart helps clarify which layers you should version-control first.


### 1. Warehouse / data models


The transformation layer that turns raw event tables and replicated SaaS data into clean, joinable models. This is the cleanest “as code” win in the entire stack. SQL files in Git, models built by a tool like dbt, SQLMesh, or Dataform, tests and lineage included. If you only do one thing in code, do this.


### 2. Semantic / metrics layer


The layer that defines what` revenue` ,` active_customer` , or` weekly_active_user` mean in business terms. Historically this lived inside the BI tool (LookML in Looker, datasets in Power BI, calculated fields in Tableau). The modern push is to move it to a standalone semantic layer (dbt’s MetricFlow, Cube, MetricQL) that any downstream tool can consume.


For a deeper take on where these definitions should live, see[where to define business metrics: SQL views, dbt, semantic layers, or BI calculations](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) .


### 3. Queries


The SQL (or generated equivalent) that powers a specific chart. Most BI tools store this inline with the chart. Some let you reference a saved query or a model from the semantic layer, which is what you want if you care about reuse. Increasingly, AI-native tools generate this on the fly, which adds a new wrinkle: do you version the prompt, the generated SQL, or both?


### 4. Dashboards and reports


The presentation layer: chart definitions, layout, filters, and the dashboard object itself. This is the layer that benefits least from Git, because most of what changes is visual. It’s also the layer that most tools handle worst.


A useful rule: every layer “above” data models has diminishing returns from full version control. Most teams should be ruthless about putting layer 1 and 2 in Git and pragmatic about layer 4.


## How major BI tools handle version control


Tool Format Git workflow Diffable Best for


Looker LookML (text files) Native Git integration Yes Teams that want a single source of truth across model and presentation


Power BI Power BI Project (.pbip), TMDL for the model Files designed for Git Yes (TMDL is text) Microsoft-shop teams with dedicated BI engineers


Tableau .twb (XML) / .twbx (zipped) Manual check-in Partial; diffs are noisy Visual reports where layout matters more than structure


dbt YAML + SQL Native Yes Warehouse modeling and metrics


Cube YAML / JS data model Native Yes Headless semantic layer feeding many consumers


Lightdash Uses dbt models directly Native (via dbt repo) Yes Teams already on dbt who want a code-first BI front end


Metabase Serialization to YAML (Pro/Enterprise) Export / import via CLI or API Mostly yes Open-source teams that want some Git workflow without rewriting their stack


Hex Project history, optional Git sync for SQL / Python Notebook-level versioning Partial Mixed SQL + notebook analytics


Sigma Workbook version history, no Git sync Tool-managed No Spreadsheet-style analysis where business users own the artifacts


Mode Notebook + report version history Tool-managed Partial Analyst-led reporting


ThoughtSpot Scriptable Document Object (SDX), Git Branch Native (per-object) Yes Search-driven BI with engineering ownership


Basedash Skills as text (lightweight semantic layer), in-tool history for charts and dashboards Tool-managed Partial AI-native teams who want metric definitions captured but don’t want to manage dashboards as code


A few notes on the table:


- “Diffable” means a sensible human can read a` git diff` and understand what changed. Tableau’s XML and Power BI’s older .pbix both technically check in but produce noisy diffs.
- Power BI Projects (` .pbip` with TMDL) are a significant improvement over` .pbix` and are now Microsoft’s recommended format for source-controlled Power BI development.
- Looker remains the most code-native of the mainstream BI tools. Its weakness is that the lockfile, in effect, is LookML, and migrating off it is hard.
- AI-native tools (Basedash, ThoughtSpot Sage, Power BI Copilot) introduce a new question: should you version control the AI’s context, the prompts, or the generated artifacts? Most teams version the context (Basedash Skills, ThoughtSpot Worksheets) and treat the artifacts as cheap.


## A maturity model for BI version control


Most BI version control discussions skip a step: not every team needs the same level. A useful frame is to think about four levels, each one adding more rigor and more overhead.


### Level 0: live edits, no history


Anyone with edit access changes anything, in place. No history beyond the most recent state. A dashboard that worked yesterday and is broken today is debugged through Slack archaeology.


This is the default in spreadsheets and the worst-case state for any BI tool. It is unfortunately common in early-stage startups using free-tier tools.


### Level 1: in-tool version history


The BI tool keeps automatic snapshots and lets you revert. There is no PR review, no branching, no environment promotion, but you can recover from accidental deletes.


Most modern BI tools (Sigma, Looker Studio, Tableau, Metabase, Power BI Service, Basedash) are at least level 1 out of the box. This is good enough for many teams under 20 people.


### Level 2: code export with manual Git


The BI tool can serialize objects to text. Someone occasionally exports the state and checks it into a repo, mostly for backup or audit purposes. Promotion between environments is manual.


This is where Metabase serialization, Power BI Project format used ad hoc, and most “we have a Git repo but it’s not really the source of truth” workflows sit. It gives you an audit trail without forcing change-management ceremony.


### Level 3: source-of-truth in Git, deployed to BI tool


The Git repo is the canonical version of metric definitions and (sometimes) dashboards. Changes go through pull requests. CI runs lint and tests. A deploy step pushes the validated state to the BI tool.


This is the classic Looker workflow and the recommended Power BI Projects workflow. It is also what dbt + Cube + Lightdash teams converge on for the metric layer specifically.


### Level 4: full CI/CD with environment promotion


Beyond level 3, the BI tool has multiple environments (dev, staging, prod). Branches deploy to dev workspaces. Merges to main deploy to staging. A separate promotion step pushes to prod. Lineage is tracked across the warehouse, semantic layer, and BI tool.


This is mostly an enterprise pattern. Looker + LookML projects, dbt + Cube + a strict deploy pipeline, and large internal data platforms all live here. Below roughly 30 analysts, level 4 is more overhead than it is worth.


The choice between levels is rarely a technical one. It is about who is allowed to break things and how loudly the team minds.


## A practical recommendation by team size


The right level depends mostly on team size and the cost of a wrong metric.


- **Under 10 people, pre-Series A:** Level 1. Pick a tool with reliable in-tool history, agree on a small number of “official” dashboards, and avoid metric-definition drift by writing metric meanings down in plain English somewhere. Skills, READMEs, or a shared Notion page are fine. Investing more is premature.
- **10 to 50 people, Series A to B:** Level 2. Get your warehouse models into dbt and into Git. Keep dashboards in the BI tool. Start tagging “certified” dashboards so people know which ones are trusted. See[dashboard sprawl: how to audit, certify, and retire dashboards](https://www.basedash.com/blog/dashboard-sprawl-how-to-audit-certify-and-retire-dashboards) for what this looks like.
- **50 to 200 people, Series B to C:** Level 3 for the data and semantic layers. Adopt a real semantic layer (dbt MetricFlow, Cube) so metric definitions live in code with PR review. Leave dashboards in the BI tool unless your team has a strong reason to move them.
- **200+ people, multi-team:** Level 4. You probably have a data platform team. They will build environment promotion and a CI/CD pipeline. The question is whether dashboards as well as definitions should be in Git, and the answer is “the ones that other systems depend on.”


The mistake is going to level 3 or 4 before you’ve felt the pain of level 1. Code review on dashboards is a real cost. If you do not have an actual problem of “people accidentally broke our metrics,” skipping ahead just slows the team down.


## Common patterns and best practices


A few patterns show up across teams that have made BI-as-code work.


- **Separate the model from the presentation.** Metrics live in dbt / Cube / the semantic layer; charts in the BI tool just reference them. This makes the high-stakes layer code-reviewed without forcing every chart through Git.
- **Adopt environment promotion for metrics, not dashboards.** Most teams want safe metric changes (staging → prod). Most teams do not want safe dashboard changes (most dashboards are low-stakes). Splitting these prevents a lot of pointless ceremony.
- **Use PRs for definitions, comments for dashboards.** When a metric definition changes, require a pull request with reviewers from analytics and the metric’s business owner. When a dashboard changes, let in-tool comments and history handle it.
- **Tag certified objects.** A small set of dashboards and metrics get a “certified” badge. Changes to them require review even if the BI tool itself doesn’t enforce it. Sprawl management and version control end up tied together.
- **Document non-obvious choices in the metric file.** A` revenue.yml` should have a comment explaining why trial revenue is excluded. Future-you will not remember.
- **Plan for AI context.** If you use an AI-native BI tool, treat the AI context (Skills, worksheets, instructions) as part of the semantic layer and version it. Drift between what the AI thinks “active customer” means and what the dbt model thinks it means is a new failure mode.


## Pitfalls to avoid


- **Treating dashboard YAML as a real source of truth too early.** Many teams export Tableau or Power BI files to Git, then never look at the diffs. The “source of truth in Git” claim is real only if changes flow through PRs.
- **Hand-rolling CI/CD before you’ve picked a semantic layer.** A pipeline that deploys a poorly modeled metric stack faster does not help anyone.
- **Forgetting permissions.** Permissions tied to user IDs in the BI tool’s database don’t round-trip through Git. Decide early whether permissions live in the BI tool, the warehouse, or both.
- **Letting dashboards drift from the semantic layer.** A chart with an inline custom SQL formula will not get the benefit of a definition change in dbt. Audit for inline SQL periodically.
- **Migrating everything at once.** Pick one metric, move its definition into a code-managed layer, and watch what breaks. Then do the next one.


## When you don’t need BI as code


There are real reasons to stay at level 1.


- **The team is small and the cost of a wrong metric is low.** A six-person seed-stage company doesn’t need a metric review board.
- **The BI tool is genuinely the working surface.** If business users are building and revising their own analyses (Sigma, spreadsheets, ad-hoc charts), forcing Git on them is a category error.
- **AI handles the long tail.** If most analytical questions are answered by an AI agent that writes queries on demand, the unit of reuse shifts from “dashboard” to “metric definition” and “skill.” You need to version the latter, but you can largely stop worrying about the former. This is the workflow[Basedash](https://www.basedash.com/) is built around: a few hand-authored Skills define the shared metrics, the AI writes the SQL for each new question, and dashboards are cheap.


The deeper version of this point is that BI as code is a means, not an end. The actual goal is: when a number changes, can you explain why. If you can answer that without Git, you don’t need Git for it.


## FAQ


### Is Looker the only “BI as code” tool?


No. Looker was the first mainstream BI tool built around a code-first workflow, but Power BI Projects (TMDL), dbt + Cube + Lightdash, and ThoughtSpot’s SDX all support comparable Git workflows in 2026. Looker still has the deepest LookML ecosystem.


### Can you version control Tableau or Power BI dashboards in Git?


You can check in Tableau workbooks (.twb XML) and Power BI Project files (.pbip with TMDL), but diff quality varies. Power BI Projects produce reasonably readable diffs. Tableau’s XML diffs are noisy enough that most teams use Git only for backup, not for review.


### Where should the semantic layer live: in the warehouse, the BI tool, or a standalone tool?


Most teams in 2026 are moving definitions out of the BI tool and into either the warehouse (dbt models with metrics) or a standalone semantic layer (dbt MetricFlow, Cube). The advantage is that the same definition feeds multiple consumers (BI tool, reverse ETL, internal APIs, AI agents). See[what is a semantic layer](https://www.basedash.com/blog/what-is-a-semantic-layer-the-complete-guide-for-modern-bi) for a fuller treatment.


### Do AI-native BI tools fit into a BI-as-code workflow?


Partially. AI-native tools generate SQL per question, so “versioning the query” is less useful. What does fit into code is the AI’s context: metric definitions, business rules, naming conventions. Tools like Basedash expose this as Skills; ThoughtSpot calls them Worksheets; Power BI uses semantic model annotations. Treat that context as part of the semantic layer and put it through the same review as a dbt model change.


### Is BI as code worth it for a startup?


Not yet, for most startups. Get the warehouse models into dbt and into Git as soon as you have a warehouse worth modeling. Leave dashboards alone until either (a) the team is large enough that uncontrolled dashboard changes cause real fights, or (b) the dashboards are read by external customers or regulators and need an audit trail. Before that, in-tool history plus a few documented “official” metrics is usually enough.
