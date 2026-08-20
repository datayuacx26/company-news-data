---
schema_version: "1.0.0"
document_id: "c07fded0628d83f5ea5a8ee214135855bef9dfc5dc99d73b06a944a61004bf8b"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-a-hubspot-analytics-dashboard-data-metrics-and-tools/"
published_at: "2026-05-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:49a3522c74c465bc3fb401721161b32d6c6c6529f297cced0f6a1e56c0884d57"
---

# How to build a HubSpot analytics dashboard: data, metrics, and tools

A useful HubSpot analytics dashboard answers four questions in a single screen: how big is the pipeline, how fast is it converting, where are the best deals coming from, and which reps and segments are performing. Anything past that is supporting detail.


This guide is for revenue ops, marketing ops, and analytics leads at B2B SaaS companies running HubSpot as their CRM. It covers what belongs on the dashboard, where the underlying data actually comes from, the joins and properties that trip teams up, and the tool options worth considering when HubSpot’s native reporting hits its limits.


## TL;DR


- HubSpot’s native dashboards are fine for activity tracking and simple stage funnels. They start to break down when you need cross-object metrics, custom attribution windows, or joins with billing or product data.
- The cleanest pattern is to sync the core CRM objects (contacts, companies, deals, engagements, owners, pipelines) to a warehouse, model them with dbt or SQL views, and build dashboards on top with a modern BI tool.
- The five metrics that belong on most HubSpot dashboards are pipeline by stage, conversion rate between stages, sales cycle length, win rate by source, and pipeline velocity. Everything else is segmentation.
- The biggest data-quality pitfalls are deal stage history (HubSpot only stores it as property history), lifecycle stage transitions that move backwards, attribution overwrites, and timezone misalignment between properties and event timestamps.
- Use HubSpot’s built-in reporting if your team is fewer than ten people, you do not need cross-object metrics, and you are happy with their custom report builder. Sync to a warehouse the day either of those changes.


## What belongs on a HubSpot analytics dashboard


A HubSpot dashboard has a narrower job than a full revenue dashboard. It tells the team what is happening inside the funnel that HubSpot owns: marketing capture, lead qualification, and the deal lifecycle through close. It is not the place to track recurring revenue, churn cohorts, or product usage. Those belong on a[SaaS revenue dashboard](https://www.basedash.com/blog/how-to-build-a-saas-revenue-dashboard-metrics-data-sources-and-structure) that joins HubSpot with your billing system and application data.


For most teams, four sections cover the work:


1. **Top of funnel.** New contacts, MQL volume, source mix, and lead-to-MQL conversion.
2. **Pipeline.** Open deals by stage, total pipeline value, average deal size, and aging.
3. **Velocity and conversion.** Sales cycle length, stage-to-stage conversion, and win rate by source or segment.
4. **Owner and team performance.** Activity counts, deals worked, and quota attainment by rep.


If a chart does not fit one of those four buckets, it probably belongs somewhere else.


## The metrics that actually matter


Pick a small set and define each one explicitly. The most common reason a HubSpot dashboard loses trust is that two charts label the same metric the same way but use slightly different filters.


### Pipeline value by stage


Sum of` amount` on open deals (` dealstage` not in any closed-won or closed-lost stage), grouped by` dealstage` . The default chart is a horizontal bar with the pipeline stages in order. Filter by pipeline if you have more than one (sales-led vs self-serve, expansion vs new business, geography). Many teams quietly let closed-lost deals leak into “open pipeline” because they forget to include all closed-lost stages in the filter, so it is worth looking up the full list of closed stage IDs in your portal rather than relying on label text.


### Stage-to-stage conversion rate


For each pair of consecutive stages, the percentage of deals that progressed from the earlier to the later stage within a defined window (typically the trailing 90 days). This is where most teams discover that HubSpot’s default funnel report is misleading: it counts deals currently sitting in a stage rather than the rate at which deals historically moved through it. The correct calculation requires deal stage history, which HubSpot stores in the` dealstage` property history rather than as a separate event stream. More on that below.


### Sales cycle length


Median (not average) days between deal creation and closed-won, calculated on deals closed in the trailing 90 or 180 days. Use median because a few enterprise deals can stretch the average by months. Track it as a single line over time so you can tell whether deals are speeding up or slowing down.


### Win rate by source


Closed-won deals divided by closed-won plus closed-lost deals, segmented by` hs_analytics_source` (the original source on the associated contact or deal) or by a custom source property your team maintains. Keep a separate “unknown” bucket; do not bury the deals that have no source attached, because they tell you something about your tracking quality.


### Pipeline velocity


Pipeline velocity = (number of opportunities × average deal size × win rate) / sales cycle length. It collapses four numbers into one and is useful as a single trend line on the executive view of the dashboard. Calculate it on a trailing-90-day basis and segment it the same way you segment win rate.


### Optional but useful


- **Lead response time.** Median minutes between contact creation and the first logged engagement (call, email, meeting). Predicts MQL-to-SQL conversion in most B2B businesses.
- **Activity per opportunity.** Calls, emails, and meetings logged on each open deal. Flags deals that have gone cold.
- **Pipeline coverage.** Open pipeline value divided by remaining quarter quota. A simple sanity check on whether the team has enough deals in flight.
- **Multi-touch attribution.** HubSpot’s[revenue attribution reports](https://knowledge.hubspot.com/reports/use-multi-touch-revenue-attribution-reports) handle this natively in Marketing Hub Enterprise. If you are on a lower tier or want a different attribution model, you will rebuild it in your warehouse.


CAC, payback period, NRR, and product usage do not belong on a HubSpot dashboard. They require data HubSpot does not have.


## Where the data actually comes from


HubSpot’s API exposes everything you need, but the right pattern depends on how much data you have and what else you want to join.


### Option 1: HubSpot’s native reporting


For teams under roughly ten people with simple deal pipelines, the[custom report builder](https://knowledge.hubspot.com/reports/create-and-edit-reports-with-the-custom-report-builder) is fine. It lets you cross-object across contacts, companies, deals, and engagements, and it handles most stage-based and source-based reporting without writing SQL. The constraints kick in when you need:


- Joins to data outside HubSpot (Stripe, your app database, product analytics).
- Attribution models other than the ones HubSpot ships with.
- Cohort analysis on deals by created month.
- Stage durations calculated from property history, not just current state.
- Dashboards that mix in board-level revenue metrics next to top-of-funnel.


If you need any of those, native reporting will not stretch.


### Option 2: HubSpot API directly


The CRM v3 API gives you full access to contacts, companies, deals, engagements, owners, and pipelines, plus property history endpoints. It works for narrow custom dashboards or for scripts that compute one or two metrics. Two practical limitations:


- **Rate limits.**[Burst and daily limits](https://developers.hubspot.com/docs/api/usage-details) apply per portal. Backfilling deal stage history at scale will hit them.
- **Property history pagination.** History endpoints page through changes one property at a time, which is slow when you want the full timeline of every deal.


For anything beyond a single-metric script, syncing to a warehouse is more sustainable.


### Option 3: Sync to a warehouse, build BI on top


This is what most analytics teams settle on. The pattern is:


1. **Sync.** A managed connector ([Fivetran](https://www.fivetran.com/connectors/hubspot) ,[Airbyte](https://airbyte.com/connectors/hubspot) ,[Stitch](https://www.stitchdata.com/integrations/hubspot/) , or HubSpot’s[Snowflake Data Share](https://app.hubspot.com/l/data-sync/snowflake-data-share) ) lands the raw HubSpot objects in your warehouse on a regular schedule, typically every 15 to 60 minutes.
2. **Model.** A handful of dbt models or SQL views collapse the raw tables into clean fact tables:` fct_deals` ,` fct_deal_stage_history` ,` fct_engagements` ,` dim_contacts` ,` dim_companies` . This is where you encode the business definitions of MQL, SQL, opportunity, and so on.
3. **Visualize.** A BI tool reads the modeled tables and renders the dashboard. Because everything is SQL on top of governed models, the same metrics power the executive view, the rep view, and any cross-functional dashboard that needs CRM data.


Most managed connectors materialize property history into a separate table (Fivetran calls it` deal_property_history` , Airbyte calls it` deals_property_history` ). This is the table you want for stage durations and stage-to-stage conversion, because the live` deals` table only shows the current stage.


## A reference data model


A minimal star schema for a HubSpot dashboard looks like this:


- ` dim_owner` : one row per HubSpot owner with name, team, hire date, and quota.
- ` dim_company` : one row per company with industry, segment (SMB, mid-market, or enterprise), and original source.
- ` dim_contact` : one row per contact with current lifecycle stage, original source, and acquisition timestamp.
- ` fct_deal` : one row per deal with` deal_id` ,` owner_id` ,` primary_company_id` ,` pipeline` ,` current_stage` ,` amount` ,` created_at` ,` close_date` ,` is_closed_won` ,` is_closed_lost` ,` source_id` .
- ` fct_deal_stage_history` : one row per deal-stage transition with` deal_id` ,` from_stage` ,` to_stage` ,` changed_at` , and a flag for whether the stage moved forward or backward. This table powers sales cycle, stage durations, and conversion rates.
- ` fct_engagement` : one row per call, email, meeting, or note with` engagement_id` ,` deal_id` ,` contact_id` ,` owner_id` ,` type` ,` created_at` .


Most analyses join one or two facts to one dimension. The deals fact is the trunk.


## Pitfalls that quietly break HubSpot dashboards


These are the issues that show up after the dashboard ships and you have to explain why the numbers look wrong.


### Lifecycle stage transitions that move backwards


HubSpot’s default behavior is to set a contact’s lifecycle stage forward when triggered (a new deal moves the contact to “opportunity” or “customer”), but it will not automatically reset it backwards. Workflows or manual edits can move it back. If you compute MQL volume from current` lifecyclestage` , you will undercount any contact that was an MQL but later moved to “customer” and back to “lead” for a new motion. Always compute lifecycle metrics from the property history table or from the` hs_lifecyclestage_<stage>_date` property, which records the first time a contact reached each stage.


### Deal stage history is not a separate event stream


HubSpot does not expose deal stage transitions as a separate object. You reconstruct them from` dealstage` property history. Most managed connectors do this for you, but if you write the SQL yourself, watch for two cases: deals that were created directly in a later stage (no transition record from “appointmentscheduled”), and deals that bounced backwards. Both will distort cycle time if you assume linear progression.


### Attribution overwrites


` hs_analytics_source` and related properties on a contact reflect the[original source HubSpot inferred at first touch](https://knowledge.hubspot.com/reports/hubspot-s-source-properties-explained) . Workflows, integrations, or manual edits can overwrite them. If your numbers swing month over month for no apparent reason, check whether someone is bulk-updating source on imported contacts. The safer pattern is to snapshot source at MQL conversion and store it on a custom property that nothing else can mutate.


### Timezones


HubSpot stores timestamps in UTC, but the UI displays them in the portal owner’s local time. Date filters built in HubSpot’s UI use local time; SQL queries against the warehouse default to UTC. A “yesterday” comparison between the HubSpot dashboard and a warehouse-backed dashboard can drift by up to a day for users on the wrong side of the date line. Pick a single timezone for the dashboard, document it, and apply it everywhere.


### Deduplication


Contact deduplication in HubSpot is rule-based and partial. A single human can show up as two contacts with different emails. If you compute funnel conversion at the contact level, you will undercount conversions whenever the duplicate gets created during the funnel. Resolve this by joining on company plus normalized email, or by using HubSpot’s[duplicate management tool](https://knowledge.hubspot.com/contacts/manage-duplicate-contacts) and accepting some manual cleanup.


### Closed-lost reasons that are free-text


If` closed_lost_reason` is a free-text property, “Pricing”, “pricing”, “too expensive”, and “Cost” all become separate buckets. Convert it to a dropdown property, or build a categorization layer in dbt that maps the free text to a stable set of reason categories before charting it.


## Tool options for the visualization layer


The right BI tool depends on whether you have a warehouse, who is consuming the dashboard, and how much modeling you want to centralize. The table below covers the patterns we see most often.


Tool Best for HubSpot connection Notes


HubSpot custom report builder Teams under 10, no warehouse, simple deal pipelines Native Fast to set up, weak on cross-object joins, no SQL escape hatch.


Looker Mid-market and enterprise teams already on Google Cloud Sync to BigQuery + LookML Strong governance, slow to iterate, expensive.


Tableau Enterprise teams with dedicated BI staff Sync to warehouse + native connector Best-in-class viz, heavyweight authoring.


Power BI Microsoft-stack companies Sync to warehouse + native connector Strong if you are already on Fabric or Synapse.


Mode Analyst-led teams that write SQL Sync to warehouse Notebook-style flow, good for ad hoc analysis, less suited for ops dashboards.


Hex Data science teams that want SQL plus Python Sync to warehouse Good for cohort analysis, less optimized for daily ops dashboards.


Sigma Spreadsheet-heavy revenue ops teams Sync to warehouse Familiar to spreadsheet users, warehouse-native.


Basedash Startup and lean revenue ops teams that want fast, AI-assisted dashboards Sync to warehouse, or[direct via 750+ connectors](https://www.basedash.com/data-sources) Natural-language queries, governed metrics, works with or without a separate warehouse.


For deeper comparisons by team type, see our breakdowns of[BI tools for sales teams](https://www.basedash.com/blog/best-bi-tools-for-sales-teams-compared-2026) and[marketing analytics tools](https://www.basedash.com/blog/best-marketing-analytics-tools-compared-2026) .


## A reference dashboard layout


For a B2B SaaS revenue ops team, the layout that holds up well in practice is:


**Section 1: Pipeline scorecard.** Six KPI cards along the top: open pipeline value, open deal count, weighted pipeline (` amount × stage probability` ), pipeline coverage versus remaining quota, average deal size, win rate (last 90 days). Each card shows the trailing-30-day delta.


**Section 2: Pipeline by stage.** Horizontal bar of open deals by stage, in funnel order. Below it, a small table with deal count, total amount, and average age in stage.


**Section 3: Conversion funnel.** Stage-to-stage conversion rate over the last 90 days, calculated from deal stage history. Use a Sankey or a connected bar chart, not a “pyramid” funnel that double-counts deals sitting in earlier stages.


**Section 4: Velocity.** Sales cycle (median days created to closed-won), pipeline velocity, and lead response time, each as a single line over the last six months. Three small charts in a row.


**Section 5: Source and segment.** Win rate, average deal size, and pipeline by original source, segmented by company size. A heatmap or a small multiples grid works better here than a stacked bar.


**Section 6: Team performance.** Per-rep table with deals worked, deals closed, win rate, and average cycle time. Sort by closed-won amount in the trailing 90 days. Keep this section access-controlled if you do not want every viewer to see every rep’s numbers.


If your team also runs a customer success motion in HubSpot, build that as a separate dashboard rather than mixing health scores into the revenue funnel.


## When to stay native vs sync to a warehouse


There is no prize for adopting a heavier stack early. Stay native when:


- You are under roughly ten people on the GTM team.
- You only need to report on data that lives inside HubSpot.
- HubSpot’s custom report builder covers your current questions.
- You do not yet have a data warehouse and adding one would be your first dbt project.


Sync to a warehouse when:


- You need to join HubSpot data to billing, product, or finance data on a regular basis.
- You want to compute cohort metrics on deals or contacts by created month.
- Multiple teams are starting to publish “their own” version of the same metric.
- You are paying enough for HubSpot reporting tiers that the cost of a warehouse plus a BI tool is comparable.


The migration from native HubSpot reporting to a warehouse-backed BI tool is incremental. Start with the deals and contacts objects, model the five metrics above, ship one dashboard, and only then expand. Teams that try to replicate every HubSpot report on day one usually stall.


## FAQ


**Does HubSpot have a real BI layer?**


The custom report builder is closer to a flexible report wizard than a BI layer. It does not support SQL, custom modeling, or joins with non-HubSpot data, and the more advanced features are gated to Marketing Hub or Sales Hub Enterprise tiers. For BI work past simple pipeline reporting, syncing the data out and building dashboards in a dedicated BI tool is the standard pattern.


**Can I connect a BI tool directly to HubSpot without a warehouse?**


Yes, but with caveats. Most BI tools either ship a HubSpot connector or accept the[HubSpot OData feed](https://knowledge.hubspot.com/reports/connect-to-the-hubspot-odata-feed) . Direct connections work for low-volume dashboards on small portals. For anything more than a few thousand deals or contacts, sync to a warehouse to avoid hitting API rate limits and to make modeling possible. Tools like Basedash can[sync HubSpot through Fivetran](https://www.basedash.com/data-sources) into a managed warehouse without you having to set up Snowflake or BigQuery yourself.


**How do I track deal stage history in HubSpot?**


HubSpot stores stage history as property history on the` dealstage` property, not as a separate event table. The CRM v3 API exposes it via the property history endpoint, and managed connectors typically materialize it into a` deal_property_history` (or similarly named) table in your warehouse. Build your own` fct_deal_stage_history` model on top with one row per transition, then compute cycle and conversion metrics against that.


**What is the cleanest way to define MQL and SQL?**


Pick a single property that signals each stage and never overwrite it. The conventional pattern is to use HubSpot’s` lifecyclestage` plus a custom timestamp property like` mql_date_first_set` and` sql_date_first_set` that workflows write to once and never update. That gives you a stable cohort definition for every contact, even if their lifecycle stage moves around later.


**Should I build separate dashboards for marketing and sales?**


For most B2B SaaS teams, yes. A marketing dashboard focuses on top-of-funnel volume, source mix, and MQL-to-SQL conversion. A sales dashboard focuses on pipeline value, conversion within the deal stages, and rep performance. Putting both on one page makes both worse. Use a common header strip with the three or four numbers everyone agrees on (new MQLs, open pipeline, closed-won, win rate) and let the rest of each dashboard be specific.


**How do I avoid the “report builder paywall”?**


Some HubSpot reporting features (custom funnels, attribution reports, deeper cross-object reports) are gated to higher Hub tiers. If you are on a lower tier and your team keeps hitting that wall, the cheapest path is usually to sync the data to a warehouse and use a BI tool you already pay for, rather than upgrading to Marketing Hub Enterprise just for reporting.


A HubSpot dashboard does not need to be elaborate to be useful. It needs to use the right properties, treat stage history correctly, and show the four or five numbers the team actually decides on. Build that first, and only add segments, attribution, and cohort views once the core view is trusted.
