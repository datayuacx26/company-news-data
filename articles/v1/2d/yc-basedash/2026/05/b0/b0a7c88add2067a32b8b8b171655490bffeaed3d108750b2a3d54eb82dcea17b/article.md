---
schema_version: "1.0.0"
document_id: "b0a7c88add2067a32b8b8b171655490bffeaed3d108750b2a3d54eb82dcea17b"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-migrate-from-looker-to-a-modern-bi-tool-a-practical-playbook/"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:9377aec3bf6ced0470743def4c8784f6ad36a13f361f59719fd6598b13d7388c"
---

# How to migrate from Looker to a modern BI tool: a practical playbook

A Looker migration is a five-phase project: audit what is actually used, decide what to do with LookML, choose a replacement that fits the work your team does in 2026, rebuild explores and dashboards against the same warehouse, then run both tools in parallel before turning Looker off. Most teams can move the critical 20% of content in three to four weeks. The hard part is rarely connecting databases. It is deciding how much of your LookML estate is worth porting and how much is sunk cost.


This guide is for analytics leads, data platform managers, and finance owners at companies that have decided Looker is no longer the right BI tool and want a defensible plan to leave. It assumes you are running Looker on Google Cloud against a warehouse like Snowflake, BigQuery, Redshift, Databricks, or PostgreSQL, and that you have a shortlist of candidate replacements.


## TL;DR


- Treat the LookML estate as a content audit, not a translation problem. Most teams find that 50 to 70 percent of Explores and Looks have not been opened in 90 days.
- Decide early whether your semantic layer moves to dbt, Cube, MetricFlow, or the new BI tool. This single decision drives everything else.
- Pick a replacement based on how your team actually works in 2026, not the LookML feature you remember loving in 2019.
- Run Looker and the new tool in parallel for at least three weeks. Most Looker users have built up muscle memory; cold cutovers create instant pushback.
- Decommission deliberately. Export LookML to git, archive scheduled reports, and turn the instance off only after the last consumer has migrated.


## Why teams leave Looker


Looker is excellent at the job it was originally built for: a small data team encodes business logic in LookML once, and analysts ask questions through Explores without writing SQL each time. That model still works. Teams typically leave for one of a few specific reasons rather than a vague “we need something better.”


- **Cost relative to value.** Google Cloud’s Looker pricing is per-platform plus per-user at the upper tiers, and the all-in spend (instance, viewer add-ons, embed licensing, professional services) often runs into six figures a year. Teams ask whether the modeling discipline justifies that line item against newer alternatives.
- **LookML maintenance overhead.** Every new metric, dimension, or filter is a code change. A small data team can spend half its capacity maintaining LookML for stakeholders who would rather ask a question in plain English and get a chart.
- **Slow self-serve for non-technical users.** Explores are powerful for analysts but still abstract for ops, finance, and CS teammates. They want a search bar, not a field picker. Looker has added natural-language features, but the default experience is still LookML-first.
- **Performance and PDT complexity.** Persistent derived tables can become a hidden source of warehouse cost. Triggered rebuilds, dependency graphs, and schedule conflicts get expensive to manage as the project grows.
- **Fragmented AI experience.** Buyers in 2026 expect to ask follow-up questions, generate ad hoc charts, and have a system that understands schema and intent. Looker’s roadmap on this is real but uneven, and competitors built around AI from the start tend to feel more native.
- **Embedded use cases that have outgrown Looker.** Customer-facing dashboards have different requirements (multi-tenant theming, per-customer filtering, fast load times) than internal BI. Many teams keep Looker for internal use and move embedded analytics to a dedicated platform, or vice versa.


If none of these match your situation, do not migrate. Invest in your LookML project, fix the specific pain, and reassess in six months.


## Phase 1: Audit your Looker project


Before you talk to a single vendor, get an honest picture of what is in the project and who actually uses it. Most migrations stall because teams try to recreate every Look, including the ones nobody has opened in a year.


### Pull the usage data


Looker exposes content usage through System Activity (the` i__looker` model). A useful starting query against the` history` Explore:


```text
select
content_id,
content_type,
title,
count  (  *  )   as   views_last_90d
from   history
where   created_time   >   current_date   -   interval   '90 days'
and   is_user_dashboard   =   'No'
group by   1  ,   2  ,   3
order by   views_last_90d   desc  ;
```


Pull the same data for Looks, dashboards, and scheduled deliveries. You are looking for three categories:


1. **Critical content.** Top 10 to 20 percent of dashboards and scheduled reports by recent usage. These define migration success.
2. **Long tail.** Used occasionally, valuable to someone. Worth rebuilding only if cheap.
3. **Abandoned content.** No views in 90 days. Default action: do not migrate.


Most teams discover that more than half their Looker content is in category three. Cutting that work up front is the highest-leverage decision in the entire project.


### Inventory your LookML estate


Pull the LookML project from git and classify what is in it:


- Models and Explores in active use
- Views and derived tables (regular vs PDT)
- Persistent derived tables, with rebuild schedules and downstream dependencies
- Access grants, user attributes, and access filters (your real permissions model)
- Liquid templated logic (often the hardest to migrate cleanly)
- Native dashboards, LookML dashboards, and Looks
- Schedules, alerts, and merge results


Count the Explores in active use, not the ones in the project. A typical mature Looker install has tens of Explores, of which fewer than a dozen are queried each week.


### Capture the business logic that lives only in Looker


Some logic exists nowhere else. Extract it before you touch anything:


- LookML measure definitions (especially complex aggregations and ratios)
- Joins and filters defined in views
- Liquid templates that change SQL based on user attributes
- Access filters that scope data per user
- Custom drill paths and link templates
- Scheduled deliveries with subject lines, recipient lists, and delivery formats


Save the relevant LookML, plus a written description of each business definition, into a separate git repository or doc. This single step prevents the worst Looker failure mode: turning the instance off and discovering that the definition of “qualified opportunity” lives only inside one view file.


## Phase 2: Decide what happens to LookML


This is the decision that determines the next two months of work. There are three realistic paths. Do not try to defer this choice; it constrains your tool shortlist.


### Option A: Move the semantic layer to dbt


Translate LookML measures, dimensions, and joins into dbt models, and use dbt’s metric definitions or a metrics layer like Cube or MetricFlow on top. The new BI tool consumes those definitions instead of redefining them in its own format.


This is the right path when:


- You already have a dbt project.
- Multiple tools consume the same metrics (BI, reverse ETL, embedded).
- You want metrics tested and version-controlled alongside transformations.
- You expect to re-platform BI again in the future and want to avoid lock-in.


It is wrong when you do not have analytics engineering capacity to maintain a separate metrics layer. The work is real and ongoing.


### Option B: Define metrics inside the new BI tool


Most modern BI tools have a lighter semantic layer of their own. Re-implement LookML logic inside the new tool’s metric or model layer, then deprecate LookML.


This is the right path when:


- You want a single owner for the metrics layer.
- Your data team is small and prefers fewer moving parts.
- The new tool has a credible semantic layer (not all do; verify during the POC).
- LookML was causing more friction than the discipline saved.


Verify the new tool’s semantic layer offers the governance LookML gave you — centralized admin ownership, documented definitions, and version history. Basedash’s[semantic layer](https://www.basedash.com/features/semantic-layer#governance) reproduces that parity in plain SQL instead of a separate modeling language.


### Option C: Replace LookML with disciplined SQL and a thinner metric store


Let metrics live in dbt models or warehouse views as plain SQL, then reference them in BI without a heavy semantic layer. AI-assisted BI tools tend to work well in this mode because they can read schema and inferred metrics directly.


This is the right path when:


- Most queries are operational and ad hoc.
- The team prefers SQL fluency over modeling artifacts.
- The pain that pushed you out of Looker was modeling overhead itself.


For a deeper look at this tradeoff, see[where to define business metrics](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) .


## Phase 3: Choose a replacement


Resist the urge to skip this phase. Most teams that “just pick the obvious next tool” end up doing a second migration later.


### Define what you actually need


Map your top three Looker pains to capabilities you must verify in any candidate:


- **Self-serve for non-technical users.** Verify natural-language querying against your real schema, AI-assisted chart generation, and a visual builder that handles joins without writing LookML.
- **Total cost of ownership.** All-in cost including seats, viewer access, embed licensing, and professional services. Many teams cut spend by 50 to 70 percent with the right replacement.
- **Performance and warehouse cost.** Look for query caching, materialized result caches, semantic layer support, and the ability to push aggregates into the warehouse. The wrong tool will reproduce your PDT bills against the same warehouse with worse visibility.
- **Permissions and governance.** Verify the tool models your real groups and customer-scoped data. “Has RLS” is not enough; test it on your real schema during a trial. Map LookML access grants to whatever the new tool calls them. For a deeper checklist on preserving access grants, access filters, and metric governance, see[governance continuity for Looker migrations](https://www.basedash.com/blog/data-governance-for-ai-powered-bi-row-level-security-access-controls-and-compliance#what-looker-teams-migrating-to-a-modern-bi-tool-need-to-know-about-governance-continuity) .
- **Embedded analytics.** If you embed Looker in a customer-facing product, the embedded use case is a separate evaluation. Multi-tenant filtering, theming, SSO patterns, and load times matter more than feature checklists.


### Shortlist candidates


The realistic shortlist in 2026 looks like this. None is the right answer for every team:


- **[Basedash](https://www.basedash.com/)** is a natural fit for teams that want an AI-first BI experience, a visual editor non-technical users can drive, governed access to production databases and warehouses, and embedded analytics for customer-facing dashboards. It works well when the migration goal is “give business teams a way to answer their own questions without filing tickets.” See[Basedash vs Looker](https://www.basedash.com/vs/basedash-vs-looker) .
- **Omni** pairs a Looker-style semantic layer with a Metabase-style usability layer. Worth testing if your team is split between modelers and consumers and you do not want to abandon the modeling discipline entirely.
- **Hex** is strong for analyst-heavy teams that want notebook-style exploration alongside dashboards. It is not a Looker replacement for non-technical end users.
- **Sigma** is strong for spreadsheet-native users on Snowflake. It fits ops and finance teams that want pivot tables more than they want Explores.
- **Power BI** is the right answer if you are deep in Microsoft Fabric or Azure. Outside that ecosystem the integration cost is real.
- **Tableau** remains strong for advanced visualization and pixel-perfect reports. Not the cheapest substitute for Looker, but valid where visualization sophistication is the point.
- **ThoughtSpot** is the closest match for “search-based BI.” Worth testing if natural-language querying and AI-driven analytics are the top priority.
- **Looker Studio** is sometimes pitched internally as the cheap replacement. It is not. Looker Studio is a separate product with no LookML support, weaker permissions, and a different positioning. Treat it as a free dashboarding tool for adjacent use cases, not a migration target.


Use the[Looker alternatives comparison](https://www.basedash.com/vs/looker-alternatives) to narrow further, then run a structured proof of concept. The[BI tool POC framework](https://www.basedash.com/blog/how-to-run-a-bi-tool-proof-of-concept-a-30-day-evaluation-framework) covers a 30-day evaluation in detail.


### Use a scoring rubric, not a feature checklist


Score each candidate on six criteria, weighted by your situation:


Criterion What you are measuring


Time to rebuild a top-5 dashboard How long it takes one person to recreate a real Looker dashboard end to end


Non-technical user success Whether a finance, ops, or CS teammate can answer a new question without an analyst


Permission fit Whether you can model your real groups and customer-scoped data, including embed scenarios


Warehouse load and cost Whether the tool pushes more or less work to your warehouse under realistic concurrent use


Total cost of ownership All-in cost including seats, viewer add-ons, SSO upcharges, and migration services


Migration cost Estimated person-weeks to move the critical 20% of content and the chosen LookML path


Whoever owns the decision should write this matrix before vendor demos start. Vendors will reorient toward whatever you score on. That is good; you want them solving your real problems, not selling you generic features.


## Phase 4: Rebuild and validate


Once you have picked a tool and a LookML path, the work is straightforward but easy to underestimate.


### Connect data and rebuild the model layer first


Start by connecting the same warehouses Looker uses, in roughly the same configuration. If Looker had a dedicated service account, mirror its permissions exactly so you do not discover halfway through that one Explore relied on a privileged role.


For each connection:


- Verify the new tool can see the same datasets, schemas, and views Looker exposed.
- Confirm row-level security or column masking continues to apply.
- Check that warehouse cost limits and statement timeouts are in place. Migration is when new tools tend to issue accidental full-table scans.
- Decide what happens to PDTs. If you move semantic logic into dbt, dbt models replace most PDTs. If you move it into the BI tool, you may need a few materialized views or scheduled tables to replicate behavior.


Rebuild the metrics layer before any dashboards. Every downstream chart depends on it.


### Rebuild content in dependency order


Move content in this order:


1. **Shared metrics and joined models.** Whether they live in dbt, the new BI tool, or both, they come first.
2. **Source-of-truth dashboards.** Executive and customer-facing dashboards used weekly. Rebuild these in the first sprint and validate with the people who use them.
3. **Operational dashboards and Looks.** Used daily by smaller teams. Rebuild in week two or three.
4. **Long tail Looks and Explores.** Either rebuild on demand as requests come in, or archive and require teammates to recreate them in the new tool when needed.


Aim to rebuild rather than mechanically translate. A dashboard built in Looker three years ago probably has Explores nobody opens, filters nobody uses, and metrics that no longer match how the business measures itself. Migration is the cheapest moment in the next three years to clean that up.


### Validate before you celebrate


For each rebuilt dashboard, run a side-by-side check:


- Pull the same date range in both tools.
- Compare top-line numbers to the integer.
- Spot-check at least one segment and one drill-down.
- Have the dashboard’s primary user open both tools and confirm the answer matches their mental model.


Differences will surface. Some are good (a LookML join that excluded recent rows, a measure with the wrong null handling). Document each one and resolve before sign-off.


### Migrate schedules and alerts deliberately


Scheduled deliveries are easy to miss in the audit and embarrassing to discover after Looker is off. For each scheduled report:


- Capture recipient list, cadence, format, and any dynamic subject lines.
- Recreate the schedule in the new tool with the same cadence and recipients.
- Run both schedules in parallel for one full cycle so recipients can compare.
- Disable the Looker schedule only after the new tool has delivered without issue for that cycle.


Alerts (Looker’s metric alerting) follow the same pattern. Translate thresholds and recipients, then run in parallel for at least one trigger window.


## Phase 5: Cut over and decommission


The most common mistake at this stage is going too fast. The second most common is going too slow.


### Run in parallel for three to four weeks


Keep Looker running and read-only for at least three weeks after the new tool has the critical content rebuilt. During this time:


- Redirect new development to the new tool. No new Looks, dashboards, or LookML changes in Looker.
- Send announcements with links to the rebuilt dashboards, not to “the new tool’s homepage.”
- Track who is still opening Looker. Each day of usage tells you which dashboards are not yet ported.


If a dashboard has zero Looker opens for two consecutive weeks, it is safe to retire on the Looker side. If a dashboard still has heavy Looker usage after week three, find out why; either it is missing in the new tool or it does not work the same way and someone is quietly suffering.


### Communicate the deadline once


Pick a single decommission date, communicate it once, and stick to it. The pattern that drags out migrations is moving the date in response to a stakeholder who has not bothered to migrate their pet dashboard.


A reasonable cadence:


- Week 1: announcement, deadline, rebuilt critical dashboards live.
- Week 2 to 3: side-by-side validation; office hours for help.
- Week 4: targeted nudges to stragglers.
- Week 5: Looker set to read-only.
- Week 6: Looker subscription cancelled and instance shut down.


### Archive, do not delete


Before shutting Looker down:


- Snapshot the LookML project from git. You already have it; confirm the head commit is the production version.
- Export Looks and dashboards via the API to JSON for archival.
- Save scheduled delivery configurations as a record of what was running.
- Cancel the Looker subscription and document the date for finance and security.
- Note any lingering integrations (Slack, Sheets, embed) so they can be cleanly switched off.


## A migration checklist


Use this in order. Anything skipped becomes a source of post-migration confusion.


- Query Looker System Activity for usage stats by content and schedule.
- Classify content into critical, long tail, and abandoned.
- Inventory the LookML project: models, Explores, views, PDTs, access grants.
- Decide the LookML path: dbt, new BI semantic layer, or thinner SQL approach.
- Define your top three replacement criteria in writing.
- Shortlist two to four candidates.
- Run a 30-day POC against your real warehouse with at least one non-technical user and one analyst.
- Pick the tool using a written rubric.
- Connect warehouses with the same permissions Looker used.
- Rebuild metrics and joined models first.
- Rebuild critical dashboards in the first sprint.
- Side-by-side validate each rebuilt dashboard.
- Recreate scheduled deliveries and alerts; run in parallel for at least one cycle.
- Run parallel for three to four weeks.
- Set a single decommission date and communicate it once.
- Archive LookML, content exports, and schedule configurations before shutting down.
- Cancel the Looker subscription and update internal documentation.


## Common mistakes


- **Trying to translate LookML 1:1.** A migration is also a cleanup. Aggressively cut Explores nobody uses.
- **Skipping the LookML decision.** Without it, the tool shortlist is incoherent.
- **Picking a tool before defining the pain.** You will end up with a different version of the same problem.
- **Underestimating embedded use cases.** Customer-facing analytics has different requirements; migrate it as a separate workstream.
- **Skipping parallel run.** A cold cutover guarantees a credibility hit the first time the new tool returns a number that looks unfamiliar.
- **Forgetting schedules and alerts.** They are easy to miss in the audit and embarrassing to discover after Looker is off.
- **Treating the project as IT-only.** The hardest part is socializing a new tool inside the teams that consume dashboards. Pair the migration with a real enablement effort.


## When not to migrate


A Looker migration is real work. It is the right call when Looker is the bottleneck and a replacement actually fixes the pain. It is the wrong call when:


- The real issue is data quality, not the BI tool. Migrating will not fix dirty joins or undefined metrics.
- You have a strong, current LookML project that the team uses well. The cost of context-switching is higher than the value of the upgrade.
- You have a large LookML estate but only one analytics engineer. The migration will stall on capacity.
- You are in the middle of a warehouse migration. Move one piece at a time. Doing both at once multiplies risk and makes it impossible to attribute problems.


If any of these describe you, fix the upstream problem first. The migration will be easier and cheaper later.


## Where Basedash fits in the migration


If the reason you are leaving Looker is that business users still cannot answer follow-up questions without filing a request, put[Basedash](https://www.basedash.com/) in the proof of concept early. Connect Basedash to the same warehouse Looker already uses, rebuild a handful of critical dashboards, and ask the non-technical teams who rely on those dashboards to answer new questions from the same data.


That test is different from asking whether a vendor can reproduce a chart. A useful Looker replacement should help someone move from “what happened?” to “why did it happen?” without starting a new ticket. In Basedash, that means using the AI-native query flow, visual editor, governed warehouse access, and shareable dashboards together rather than treating AI as a small feature beside the old Explore workflow.


Basedash is especially worth testing when migration goals include:


- Giving operators, customer success, finance, and product teams a self-serve way to explore warehouse data without LookML.
- Replacing a pile of stale Looks with fewer reusable dashboards and more ad hoc question answering.
- Keeping direct, governed access to Snowflake, BigQuery, Redshift, Databricks, or PostgreSQL while moving the day-to-day experience toward AI-assisted BI.
- Serving both internal BI and customer-facing embedded analytics from the same modern analytics layer.


It is not automatically the right choice if your company has a mature, well-loved LookML project, a Microsoft-first analytics stack, or a team that primarily wants notebook-based analysis. But if Looker was attractive because it gave non-technical teams structured access to data, and the problem is that the modeling overhead and cost grew faster than the value, Basedash is one of the few replacements that keeps that structure while moving the experience toward AI-assisted BI.


## FAQ


### How long does a Looker migration usually take?


Critical dashboards take three to four weeks with a dedicated owner. The long tail and decommission run another three to five weeks in parallel. Total elapsed time is typically six to ten weeks for a team of moderate Looker usage. Teams with hundreds of Explores or heavy embedded use should plan for longer.


### Can I migrate LookML automatically?


Not reliably. A few open-source tools attempt to translate LookML to dbt or other formats by parsing the project, and they help with view-level boilerplate. They cannot reliably migrate Liquid templates, access grants, custom drill paths, or PDT dependencies. Expect to rebuild the semantic layer manually; this is also the highest-leverage point in the project to decide what is worth keeping.


### Should I keep Looker as a read-only archive?


Usually not. Looker’s per-platform pricing makes a read-only archive expensive. Snapshot the LookML project, export Looks and dashboards via the API, and shut the instance down. Spin it back up only if you need to investigate a historical metric.


### What about embedded Looker in our product?


Treat embedded analytics as a separate migration with its own validation. The replacement should support the same iframe or SDK pattern, the same per-tenant filtering, and the same SLA expectations. Some teams choose a different tool for embedded use cases than for internal BI; that is fine if the cost is justified. The[embedded analytics guide](https://www.basedash.com/blog/embedded-analytics-for-saas-the-complete-guide-to-adding-dashboards-to-your-product) covers the tradeoffs.


### Do we need to move our warehouse too?


No. The point of the migration is to change the BI tool, not the data foundation. If you are also unhappy with your warehouse, separate that decision and sequence it after the BI migration. Doing both at once multiplies risk and makes it impossible to attribute problems.


### Is Looker Studio a viable replacement?


Not in most cases. Looker Studio is a free dashboarding tool with no LookML support, weaker permissions, and a different positioning. It is fine for ad hoc Google Marketing or GA-style dashboards. It is not a credible replacement for a Looker project that drives operational reporting, embedded analytics, or governed self-serve.


The hard work of a Looker migration is not connecting databases or rebuilding charts. It is taking the LookML decision seriously, choosing a tool that solves the specific pain that pushed you out, and being patient enough to let the new tool earn trust before you switch the old one off.
