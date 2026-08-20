---
schema_version: "1.0.0"
document_id: "6f905ef94c75f39d07d84ee3066a0e7f07319fc0ca864cb97734858103b0f3e6"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-migrate-from-metabase-to-a-modern-bi-tool-a-practical-playbook/"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:13:17.137376+00:00"
content_hash: "sha256:62bffccd370f9f257be2b79290a783cfc783d40c8afe62cf2c9efe58893d10b1"
---

# How to migrate from Metabase to a modern BI tool: a practical playbook

A Metabase migration is a four-phase project: audit what you actually use, choose a replacement, rebuild questions and dashboards in the new tool against the same data sources, then run both tools in parallel before decommissioning Metabase. Most teams can get the critical 20% of dashboards moved in two to three weeks, and the long tail in another month. The work that breaks migrations is not technical; it is deciding which Metabase content is worth rebuilding at all.


This guide is for product, data, and operations leads who have outgrown Metabase and need a defensible plan to move off it without losing the dashboards their teams rely on. It assumes you are running Metabase Open Source or Metabase Cloud against a production warehouse like PostgreSQL, MySQL, Snowflake, BigQuery, Redshift, or ClickHouse, and that you have a shortlist of candidate replacements.


## TL;DR


- Treat the migration as a content audit, not a software install. The biggest cost is rebuilding dashboards, not connecting databases.
- Inventory your Metabase content first. Most teams discover that 60 to 80 percent of their saved questions and dashboards are stale and should not be rebuilt.
- Pick a replacement based on the actual jobs your team does, not a feature checklist. Self-serve question building, governed permissions, and AI-assisted querying are the usual gaps.
- Run Metabase and the new tool in parallel for at least two weeks. Switching cold breaks trust and creates an instant rollback narrative.
- Decommission deliberately. Export a snapshot of the Metabase database, archive critical SQL queries to a git repo, and turn off the instance only after the last consumer has migrated.


## Why teams leave Metabase


Metabase is a useful starting point because it is free, self-hostable, and easy to point at a database. Teams typically outgrow it for one of a few specific reasons rather than a vague “we need something better.”


- **Performance at scale.** Metabase runs queries directly against the source database. When dashboards multiply, concurrent users grow, or someone builds a question with a few unindexed joins, the warehouse takes the hit. Caching helps, but it does not fix architectural patterns like “every dashboard re-runs the same five-minute query.”
- **Limited self-serve for non-technical users.** The question builder works well for simple aggregations. As soon as a teammate needs a window function, a CTE, or a metric that requires three joined tables, they file a ticket with whoever owns SQL. That is the same bottleneck Metabase was supposed to remove.
- **Permissions that do not match how the business actually works.** Metabase has data sandboxes and row-level controls, but configuring them across many teams and customer segments tends to become a part-time job. Teams with regulated data or customer-scoped views often hit the ceiling first.
- **Weak collaboration and review.** Comments, version history, and shareable links are all there, but reviewing a chart change or tracking who modified a question is more painful than it should be. There is no real audit trail for dashboard edits.
- **Missing AI and natural-language features.** Buyers in 2026 expect to ask a dashboard a follow-up question in plain English and get a chart back. Metabase has added some AI features, but they are not the default experience and competitors have moved further ahead.
- **Cost growth on Cloud or Enterprise.** Once you need SSO, audit logs, advanced permissions, and a managed instance, the price is no longer “free.” That is fine if the value is there. It is not fine when the value gap with paid alternatives narrows.


If none of these describe your situation, do not migrate. The right move is to invest in Metabase, fix the specific pain, and reassess in six months.


## Phase 1: Audit your Metabase install


Before you talk to a single vendor, get an honest picture of what is actually being used. Most migrations fail because teams try to recreate every dashboard, including the ones nobody has opened in a year.


### Pull the usage data


Metabase tracks view counts, question runs, and last-edited timestamps in its application database (Postgres or H2). You can query it directly. The fields you care about live in tables like` report_card` (questions),` report_dashboard` ,` view_log` , and` query_execution` .


A useful starting query for the application database:


```text
select
c  .  id  ,
c  .  name   as   question_name,
c  .  created_at  ,
c  .  updated_at  ,
count  (  v  .  id  )   as   views_last_90d
from   report_card c
left join   view_log v
on   v  .  model_id   =   c  .  id
and   v  .  model   =   'card'
and   v  .  timestamp   >   now  ()   -   interval   '90 days'
group by   c  .  id  ,   c  .  name  ,   c  .  created_at  ,   c  .  updated_at
order by   views_last_90d   desc  ;
```


Do the same for dashboards. You are looking for three categories:


1. **Critical content.** Top 10 to 20 percent of questions and dashboards by recent usage. These define the success of the migration.
2. **Long tail.** Used occasionally, valuable to someone. Worth rebuilding only if cheap.
3. **Abandoned content.** No views in 90 days. Default action: do not migrate.


Most teams discover that more than half their Metabase content is in category three. Cutting that work up front is the single biggest lever you have.


### Classify what is left


For each surviving question or dashboard, capture five fields in a spreadsheet:


- Owner (who maintains it)
- Consumer team (who looks at it)
- Underlying data source
- SQL complexity (simple aggregate, joined query, native SQL, requires variables)
- Replacement priority (week 1, week 2 to 4, long tail)


This list becomes your project plan. It also makes the migration negotiable: when a stakeholder demands you preserve their pet dashboard, you can point at the usage data and ask whether it is worth a week of work to a tool nobody opens.


### Snapshot business logic


Before you touch the new tool, extract any logic that lives only in Metabase. The common offenders:


- Native SQL questions with custom CASE statements
- Metabase “Models” with field mappings, joins, or filters
- Custom expressions in the visual question builder
- Dashboard-level filters and parameter mappings
- Alerts and pulses (scheduled reports)


Save the SQL into a git repo. This single step prevents the most painful Metabase failure mode: a critical query lives only inside a Metabase question, the instance dies, and nobody can reconstruct the logic.


## Phase 2: Choose a replacement


Resist the urge to skip this phase. Most teams that “just pick the obvious next tool” end up doing a second migration eighteen months later.


### Define what you actually need


The right replacement depends on which Metabase pain points pushed you out. Map your top three pains to capabilities you must verify in any candidate:


- **Self-serve for non-technical users:** check whether the tool offers natural-language querying, a visual builder that handles joins without writing SQL, and AI-assisted chart generation.
- **Performance:** look for query caching, materialized result caches, semantic layer support, and the ability to pre-compute aggregates. A direct-query tool with no caching layer will reproduce your existing problem against your existing warehouse.
- **Permissions:** verify that the tool supports group-based access, column masking, and row-level security against your specific warehouse. “Has RLS” is not enough; test it on your real schema during a trial.
- **Collaboration and governance:** look at version history, comment threads, ownership metadata, and audit logging. If you are in a regulated environment, audit logs are usually a hard requirement.
- **Cost model:** decide whether per-seat pricing or usage-based pricing fits how your team will use the tool. A 20-person ops team using a per-seat tool can cost more than a 100-person team using a usage-based one.


### Shortlist candidates


The realistic shortlist in 2026 looks like this. None is the right answer for every team:


- **[Basedash](https://www.basedash.com/)** is a natural fit for teams that want an AI-first BI experience, a visual editor that non-technical users can drive, governed access to production databases, and embedded analytics for customer-facing dashboards. It is positioned as a modern alternative to Metabase and is closer in spirit to “the thing you wanted Metabase to be” than to a legacy BI platform. See the side-by-side at[Basedash vs Metabase](https://www.basedash.com/vs/basedash-vs-metabase) .
- **Looker (Google Cloud)** is the right answer when you need a hardened semantic layer (LookML) and you are willing to invest in modeling work. It is the heaviest of the alternatives and typically overkill for early-stage teams.
- **Sigma** is strong for spreadsheet-native users and Snowflake-first analytics. It is less ergonomic against Postgres or MySQL.
- **Hex** is best for analyst-heavy teams that want notebook-style exploration alongside dashboards. It is not really a Metabase replacement for non-technical end users.
- **Power BI** is the right choice if you are already deep in Microsoft Fabric or Azure. Outside that ecosystem the integration costs add up.
- **Tableau** remains strong for advanced visualization and pixel-perfect reports, but its licensing and learning curve are harder to justify if your existing Metabase usage was simple.
- **Omni** is a younger entrant that pairs a Looker-style semantic model with a Metabase-style usability layer. Worth testing if your team is split between SQL writers and consumers.


Use the more detailed[Metabase alternatives comparison](https://www.basedash.com/vs/metabase-alternatives) to narrow further, then run a structured proof of concept. The[BI tool POC framework](https://www.basedash.com/blog/how-to-run-a-bi-tool-proof-of-concept-a-30-day-evaluation-framework) covers a 30-day evaluation in detail.


### Use a scoring rubric, not a feature checklist


Score each candidate on six criteria, weighted by your situation:


Criterion What you are measuring


Time to rebuild a top-5 dashboard How long it takes one person to recreate a real Metabase dashboard end to end


Non-technical user success Whether a finance or ops teammate can answer a new question without help


Permission fit Whether you can model your real groups and customer-scoped data


Warehouse load Whether the tool pushes more or less work to your database under realistic load


Total cost of ownership All-in cost including seats, viewer add-ons, SSO upcharges, and any required services


Migration cost Estimated person-weeks to move the critical 20% of content


Whoever owns the decision should write this matrix before vendor demos start. Vendors will reorient toward whatever you score on. That is good; you want them solving your real problems, not selling you generic features.


## Phase 3: Rebuild and validate


Once you have picked a tool, the work is straightforward but easy to underestimate.


### Connect data sources first, then rebuild content


Start by connecting the same databases Metabase uses, in roughly the same configuration. If you used a read replica for Metabase, point the new tool at the same replica. Match the connection user’s permissions exactly so you do not discover halfway through that one dashboard depended on a privileged credential.


For each connection:


- Verify the new tool can see the same schemas, tables, and views Metabase exposes.
- Confirm that any custom roles, masking policies, or row-level security continue to apply.
- Check that warehouse cost limits and statement timeouts are in place. Migration is when new tools tend to issue accidental full-table scans.


### Rebuild in dependency order


Move content in this order:


1. **Shared models or semantic definitions.** If you used Metabase Models or have a dbt or SQLMesh project, set those up in the new tool first. Every downstream dashboard depends on them.
2. **Source-of-truth dashboards.** The ones executives and customer-facing teams rely on weekly. Rebuild these in the first sprint and validate with the people who actually use them.
3. **Operational dashboards.** Used daily by smaller teams. Rebuild in week two or three.
4. **Long tail.** Either rebuild on demand as requests come in, or archive and require teammates to recreate them in the new tool when they need them again.


Aim to rebuild rather than mechanically translate. A dashboard built in Metabase three years ago probably has questions nobody opens, filters nobody uses, and metrics that no longer match how the business measures itself. Migration is the cheapest moment in the next three years to clean that up.


### Validate before you celebrate


For each rebuilt dashboard, run a side-by-side check:


- Pull the same date range in both tools.
- Compare top-line numbers to the integer.
- Spot-check at least one segment and one drill-down.
- Have the dashboard’s primary user open both tools and confirm the answer matches their mental model.


Differences will surface. Most are good: filter logic that was wrong in Metabase, joins that excluded recent rows, timezones that did not match the warehouse. Document each one and resolve before sign-off.


## Phase 4: Cut over and decommission


The most common mistake at this stage is going too fast. The second most common is going too slow.


### Run in parallel for two to four weeks


Keep Metabase running and read-only for at least two weeks after the new tool has the critical content rebuilt. During this time:


- Redirect new development to the new tool. No new questions or dashboards in Metabase.
- Send announcements with links to the rebuilt dashboards, not to “the new tool’s homepage.”
- Track who is still opening Metabase. Each day of usage tells you which dashboards are not yet ported.


If a dashboard has zero Metabase opens for two consecutive weeks, it is safe to retire on the Metabase side. If a dashboard still has heavy Metabase usage after week three, find out why; either it is missing in the new tool or it does not work the same way and someone is quietly suffering.


### Communicate the deadline once


Pick a single decommission date, communicate it once, and stick to it. The pattern that drags out migrations is moving the date in response to a stakeholder who has not bothered to migrate their pet dashboard.


A reasonable cadence:


- Week 1: announcement and deadline.
- Week 2: rebuilt dashboards live; office hours for help.
- Week 3 to 4: side-by-side validation; targeted nudges to stragglers.
- Week 5: Metabase set to read-only.
- Week 6: Metabase shut down.


### Archive, do not delete


Before shutting Metabase down:


- Back up the application database. Even if you never restore it, a snapshot is cheap insurance.
- Export critical SQL questions and Models to a git repo, with the original Metabase question ID as a comment.
- Save any documentation or descriptions teammates wrote inside dashboards. They are often the only record of why a metric was defined a particular way.
- Cancel paid subscriptions and document the date for finance and security.


## A migration checklist


Use this in order. Anything skipped becomes a source of post-migration confusion.


- Query Metabase’s application database for usage stats by question and dashboard.
- Classify all content into critical, long tail, and abandoned.
- Snapshot every native SQL question and Model into a git repo.
- Define your top three replacement criteria in writing.
- Shortlist two to four candidates.
- Run a 30-day POC against your real data with at least one non-technical user.
- Pick the tool using a written rubric.
- Connect data sources with the same permissions Metabase used.
- Rebuild shared models or semantic definitions first.
- Rebuild critical dashboards in the first sprint.
- Side-by-side validate each rebuilt dashboard.
- Run parallel for two to four weeks.
- Set a single decommission date and communicate it once.
- Archive the Metabase database and SQL before shutting down.
- Cancel paid plans and update internal documentation.


## Common mistakes


- **Rebuilding everything.** The migration is also a cleanup. Aggressively cut.
- **Picking a tool before defining the pain.** You will end up with a different version of the same problem.
- **Skipping parallel run.** A cold cutover guarantees a credibility hit the first time the new tool returns a number that looks unfamiliar.
- **Trusting Metabase as the source of truth.** Several years of cumulative custom SQL probably contains some quiet bugs. The migration is your chance to fix them.
- **Treating the project as IT-only.** The hardest part is socializing a new tool inside the teams that consume dashboards. Pair the migration with a real enablement effort.
- **Forgetting alerts and scheduled pulses.** They are easy to miss in the audit and embarrassing to discover after Metabase is off.


## When not to migrate


A migration is real work. It is the right call when Metabase is the bottleneck and a replacement actually fixes the pain. It is the wrong call when:


- The real issue is data quality, not the BI tool. Migrating will not fix dirty joins or undefined metrics.
- You have not yet defined what “success” means in writing. A new tool will not invent the definition for you.
- Your team has fewer than five active BI users. The cost of context-switching tools is higher than the value of the upgrade.
- You are in the middle of a warehouse migration. Move one piece at a time.


If any of these describe you, fix the upstream problem first. The migration will be easier and cheaper later.


## Where Basedash fits in the migration


If the reason you are leaving Metabase is that business users still cannot answer follow-up questions without a SQL owner, put[Basedash](https://www.basedash.com/) in the proof of concept early. The strongest migration path is to connect Basedash to the same databases Metabase already uses, rebuild a handful of critical dashboards, then ask the non-technical teams who rely on those dashboards to answer new questions from the same data.


That test is different from asking whether a vendor can reproduce a chart. A useful Metabase replacement should help someone move from “what happened?” to “why did it happen?” without starting a new ticket. In Basedash, that means using the AI-native query flow, visual editor, governed database access, and shareable dashboards together instead of treating AI as a small feature beside the old dashboard workflow.


Basedash is especially worth testing when your migration goals include:


- Giving operators, customer success, finance, or product teams a self-serve way to explore production data without writing SQL.
- Replacing a pile of stale dashboards with fewer reusable dashboards and more ad hoc question answering.
- Keeping direct access to PostgreSQL, MySQL, Snowflake, BigQuery, Redshift, or ClickHouse while improving the interface on top.
- Serving both internal BI and customer-facing embedded analytics from the same modern analytics layer.


It is not automatically the right choice if your company already has a mature LookML estate, a Microsoft-first analytics stack, or a team that primarily wants notebook-based analysis. But if Metabase was attractive because it was simple and direct, and the problem is that it never became truly self-serve, Basedash is one of the few replacements that keeps that directness while moving the day-to-day experience toward AI-assisted BI.


## FAQ


### How long does a Metabase migration usually take?


Critical dashboards take two to three weeks with a dedicated owner. The long tail and decommission run another two to four weeks in parallel. Total elapsed time is typically four to eight weeks for a team of moderate Metabase usage. Teams with hundreds of dashboards or heavy custom SQL should plan for longer.


### Can I migrate Metabase questions automatically?


Not reliably. A few open-source scripts try to translate Metabase questions into other tools by reading the application database, but they rarely handle native SQL questions, custom expressions, or visualizations cleanly. Expect to rebuild manually; this is also the point at which deciding what to rebuild is the highest leverage.


### Should I keep Metabase as a read-only archive?


Usually not. If you are paying for Metabase Cloud or Enterprise, the cost rarely justifies a read-only archive. Snapshot the application database, archive the SQL to git, and shut the instance down. Spin it back up only if you need to investigate a historical dashboard.


### What about embedded Metabase dashboards in our product?


Treat embedded analytics as a separate migration with its own validation. The replacement should support the same iframe or SDK pattern, the same per-tenant filtering, and the same SLA expectations. Some teams choose a different tool for embedded use cases than for internal BI; that is fine if the cost is justified. The[embedded analytics guide](https://www.basedash.com/blog/embedded-analytics-for-saas-the-complete-guide-to-adding-dashboards-to-your-product) covers the tradeoffs.


### Do we need to move our warehouse too?


No. The point of the migration is to change the BI tool, not the data foundation. If you are also unhappy with your warehouse, separate that decision and sequence it after the BI migration. Doing both at once multiplies risk and makes it impossible to attribute problems.


### How do we make sure dashboards do not regress after migration?


Two practices help. First, write a small set of “golden numbers” (ARR, active customers, revenue by month, whatever your top KPIs are) into a doc with their expected values, and check them against any dashboard build. Second, treat the new tool’s dashboards like code: ownership, review, and a way to track changes over time.


The hard work of a Metabase migration is not connecting databases or rebuilding charts. It is taking the audit seriously, choosing a tool that solves the specific pain that pushed you out, and being patient enough to let the new tool earn trust before you switch the old one off.
