---
schema_version: "1.0.0"
document_id: "b3af6e60b48eb7323fc656794ae45ad71df92edabe6d7015743bd527509c4df4"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-a-marketing-dashboard-connecting-ga4-ads-and-crm-data/"
published_at: "2026-06-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:81aa4717739018b3356249f43aaf17b42bbf6403d768b90cc373170101bc147e"
---

# How to build a marketing dashboard: connecting GA4, ad platforms, and CRM data

A marketing dashboard is only as good as the data behind it, and marketing data is the hardest kind to assemble. Ad spend lives in Google Ads and Meta, traffic and conversions live in GA4, leads and pipeline live in your CRM, and revenue lives in your billing system. Each platform reports only its own slice, and most of them quietly over-claim credit for the same conversions. The real work of a marketing dashboard is not picking charts. It is unifying these sources and agreeing on a single set of numbers.


This guide is for marketing ops, growth, and analytics leads who want one dashboard that answers how much was spent, what it produced, and whether the spend is paying back. It covers the data sources you have to connect, how to wire them together, the attribution and identity problems that make the numbers disagree, the metrics worth defining, and the tools that fit different stages.


## The reason marketing dashboards are hard


A sales dashboard pulls mostly from one system (the CRM). A[SaaS revenue dashboard](https://www.basedash.com/blog/how-to-build-a-saas-revenue-dashboard-metrics-data-sources-and-structure) joins two or three. A marketing dashboard routinely spans five or more, and the platforms actively disagree:


- **Each ad platform counts conversions it influenced.** Google Ads and Meta both use their own attribution windows and view-through rules. If you add up the conversions each platform claims, you will exceed the conversions that actually happened, sometimes by a wide margin.
- **GA4 and the ad platforms use different models.** GA4 deduplicates across channels with its own data-driven or last-click model. The numbers will not match what each ad platform reports for the same campaign.
- **Spend and outcome live in different systems.** Spend is in the ad platforms. The revenue that spend generated is in your CRM or billing system. No single tool sees both ends, so efficiency metrics like CAC and ROAS have to be assembled.


The practical consequence: you cannot build a trustworthy marketing dashboard by pasting each platform’s native numbers side by side. You have to bring the data into one place and define the metrics yourself.


## The data sources behind a marketing dashboard


Before building anything, map the sources and what each one is the authority for. A clean map prevents the most common failure, which is pulling the same metric from two systems and getting two answers.


Source What it is the authority for Typical connection method


Google Ads, Meta Ads, LinkedIn Ads Spend, impressions, clicks, campaign and ad metadata Platform API or managed connector


GA4 Sessions, traffic source, on-site conversions, engagement BigQuery export or GA4 Data API


CRM (HubSpot, Salesforce) Leads, MQLs, opportunities, pipeline, closed revenue Managed connector to a warehouse


Billing (Stripe, Chargebee) Actual paid revenue, subscriptions, refunds Managed connector to a warehouse


Product analytics or app database Signups, activation, trial-to-paid Direct connection or warehouse sync


The dividing line that keeps a dashboard honest: ad platforms own **spend** , your CRM and billing system own **outcome** , and GA4 owns **on-site behavior** between the two. When a metric needs both spend and outcome, like cost per customer, it has to be computed across sources rather than read from one.


## How to connect the sources


There are three ways to bring this data together, and the right one depends on how many sources you have and whether you already run a warehouse. These map directly to the patterns covered in[how BI tools combine data from multiple sources](https://www.basedash.com/blog/how-bi-tools-combine-data-from-multiple-sources-federation-blending-and-warehouses) .


### Direct connections per source


A BI tool connects to each platform’s API and queries it live. This is the fastest way to start and fine for a small number of sources and modest volumes. It breaks down when you need to join across platforms, because most ad and CRM APIs are not built for arbitrary joins, and live API calls hit rate limits and latency on every dashboard load.


Use direct connections when you have one or two sources, low volume, and no need to join spend against revenue yet.


### A marketing data warehouse


The durable pattern is to sync every source into a warehouse (BigQuery, Snowflake, Postgres, or similar) on a schedule, model the raw tables into clean facts, and point the dashboard at the modeled tables. Managed connectors like Fivetran, Airbyte, or Stitch handle the extraction. GA4 has a[native BigQuery export](https://support.google.com/analytics/answer/9358801) that lands event-level data, which is the cleanest way to get GA4 into a warehouse.


This is the right pattern once you need to join spend to revenue, want consistent definitions across teams, or have more than two or three sources. If you are not sure whether you have outgrown direct connections, the[signals that you need a warehouse](https://www.basedash.com/blog/when-to-add-a-data-warehouse-signals-your-startup-has-outgrown-its-production-database) apply here too.


### Blending in the BI layer


Some BI tools can blend results from multiple connected sources without a warehouse, joining them at query time on a shared key like date or campaign ID. This avoids standing up a warehouse but is fragile for marketing data, where the join keys (campaign names, UTM values) are messy and inconsistent across platforms. Treat blending as a stopgap, not the foundation.


## The attribution and identity problem


This is the part that makes marketing data different from every other dashboard, and the part most guides skip. Two technical problems sit underneath every efficiency metric.


### Stitching identity across the funnel


A person clicks a Meta ad, lands on the site (GA4 records a session), fills out a form (CRM creates a lead), and pays three weeks later (billing records revenue). To attribute that revenue back to the Meta campaign, you need to connect four records that live in four systems and share no common ID by default.


The connective tissue is UTM parameters and click IDs. Tag every campaign URL with consistent` utm_source` ,` utm_medium` , and` utm_campaign` values, and capture the platform click IDs (` gclid` for Google,` fbclid` for Meta) on the landing page. Store those values on the lead record in the CRM so the campaign that produced a lead travels with it all the way to closed revenue. Without this, spend and revenue can never be joined and your dashboard can only ever show platform-reported numbers.


### Choosing an attribution model


Once identity is stitched, you still have to decide how credit is assigned when a customer touches several channels. The common models:


- **Last-touch.** All credit to the final channel before conversion. Simple, but undervalues top-of-funnel channels like content and brand.
- **First-touch.** All credit to the first channel. Useful for understanding demand creation, misleading for optimizing spend.
- **Multi-touch.** Credit split across touches (linear, time-decay, or position-based). More accurate, much harder to maintain, and requires the full touch history that only a warehouse-backed model gives you.


For most teams, pick last-touch or first-touch as the dashboard default, label it clearly, and treat[multi-touch attribution](https://support.google.com/analytics/answer/10596866) as a separate analysis rather than the headline number. The worst outcome is a dashboard where nobody knows which model produced the numbers.


## Platform-reported vs measured metrics


Decide for every metric whether it comes from a platform’s own reporting or from your unified data, and never mix the two on the same chart.


- **Platform-reported** numbers (Meta’s reported conversions, Google’s reported ROAS) are easy to pull and useful for in-platform optimization, but they double-count across platforms and inflate results.
- **Measured** numbers come from your warehouse, where each conversion and dollar of revenue is counted once and assigned to a single channel by your attribution rules.


A marketing dashboard should lead with measured metrics for anything that drives budget decisions, and clearly tag any platform-reported figures as “as reported by \[platform\].” Blended metrics, which divide total spend by total measured outcomes, sidestep the attribution argument entirely and are the most defensible numbers on the page.


## The metrics worth defining


Keep the headline set small and define each one in one place. The table below shows the metrics that earn space on most marketing dashboards and where each comes from.


Metric Definition Sources required


Total spend Sum of spend across all paid channels Ad platforms


Blended CAC Total spend / new customers acquired in the period Ad platforms + billing


Channel CAC Channel spend / customers attributed to that channel Ad platforms + CRM/billing + attribution


ROAS Revenue attributed to spend / spend Ad platforms + billing + attribution


CAC payback Blended CAC / monthly gross margin per customer Billing + finance


Lead-to-customer rate Customers / leads in a cohort CRM + billing


Cost per lead (CPL) Spend / leads Ad platforms + CRM


Pipeline contribution Pipeline value sourced by marketing CRM


Two definitions deserve special care. **Blended CAC** divides all acquisition spend by all new customers, with no attribution required, which makes it the most trustworthy efficiency number and a good top-of-dashboard KPI.[CAC payback](https://www.basedash.com/blog/best-marketing-analytics-tools-compared-2026) tells you how many months of margin it takes to recover acquisition cost, and it requires margin data from finance, so confirm the margin assumption with whoever owns the model before publishing it.


## Data freshness and sync cadence


Marketing data has a quirk that trips up dashboards built like operational dashboards: it is not final when it first arrives.


- **Ad platforms backfill.** Conversions, and sometimes spend, get attributed retroactively as the platform’s attribution window closes. A campaign’s reported ROAS can keep moving for days after the spend happened. Do not treat today’s numbers as final, and label recent periods as provisional.
- **Different sources update at different rates.** Ad spend updates through the day, CRM data updates as reps work deals, and billing finalizes on its own cycle. Pick a single dashboard refresh cadence (daily is enough for most marketing decisions) and document the “data through” timestamp so viewers know how current each number is.


Real-time refresh is rarely worth the cost for a marketing dashboard, because the underlying attribution is not real-time anyway. A scheduled daily or hourly sync is almost always the right tradeoff.


## Tool options for the dashboard layer


The right tool depends on whether you have a warehouse, who consumes the dashboard, and how much you want to centralize definitions. The patterns below are the ones marketing teams choose most often.


Tool Best for Marketing data fit Notes


Looker Studio Small teams reporting straight from GA4 and Google Ads Strong inside the Google stack, weaker across non-Google sources Free, native Google connectors, joins across sources are limited


Power BI Microsoft-stack teams with a warehouse Strong once data is in a warehouse Many connectors, heavier authoring model


Tableau Enterprise teams with dedicated BI staff Strong on visualization, needs modeled data Best-in-class charts, heavyweight to maintain


A dedicated marketing platform (for example a reporting tool that ingests ad and CRM data) Teams that want pre-built ad connectors and templates Purpose-built for channel reporting Fast for channel views, less flexible for custom revenue joins


Basedash Startup and lean marketing teams that want fast, AI-assisted dashboards Works on a warehouse or[direct via 750+ connectors](https://www.basedash.com/data-sources) Natural-language queries, governed metric definitions, no separate BI stack required


For a full evaluation by tool, see our[comparison of marketing analytics tools](https://www.basedash.com/blog/best-marketing-analytics-tools-compared-2026) . If your marketing dashboard needs to sit next to a sales view, the[HubSpot analytics dashboard guide](https://www.basedash.com/blog/how-to-build-a-hubspot-analytics-dashboard-data-metrics-and-tools) covers the CRM side in detail.


## A reference layout


For most growth teams, a single screen organized top-down works well:


1. **Efficiency scorecard.** Four KPI cards: total spend, blended CAC, ROAS, and CAC payback, each with the trailing-period delta. These are the numbers leadership asks about.
2. **Spend and outcome by channel.** A table or grouped bar showing spend, leads, customers, and channel CAC per channel. This is where budget decisions get made.
3. **Funnel.** Sessions to leads to MQLs to customers, so you can see where the drop-off is, not just the endpoints.
4. **Trend.** Spend and new customers as two lines over the last six months, so you can see whether efficiency is improving as you scale.
5. **Campaign detail.** A sortable table of campaigns with spend, conversions, and measured CAC, for the people who manage spend day to day.


Resist adding more. The fastest way to lose trust in a marketing dashboard is to fill it with platform-reported vanity metrics that contradict the measured numbers above them.


## Common mistakes


- **Summing platform-reported conversions.** Adding Google’s and Meta’s claimed conversions double-counts shared credit and overstates performance. Use measured conversions for anything that drives budget.
- **No UTM discipline.** Inconsistent or missing UTMs make it impossible to join spend to revenue. Standardize a UTM scheme and enforce it before building the dashboard.
- **Mixing attribution models on one page.** One chart on last-touch and another on GA4’s model will never reconcile. Pick one default and label it.
- **Treating recent data as final.** Attribution backfills for days. Mark recent periods as provisional or you will field questions every time a number moves.
- **Reporting CAC without margin.** CAC payback needs gross margin from finance. Publishing it on a marketing assumption invites a fight with the finance team.
- **Building on blended queries instead of modeled data.** Query-time blending on messy campaign keys is fragile. Once you have more than two sources, model the data in a warehouse.


## A build checklist


Use this before you ship:


- Every source is mapped to the metrics it is the authority for.
- Spend, outcome, and on-site behavior come from the systems that own them, not duplicated.
- A consistent UTM and click-ID scheme is enforced on every campaign.
- Campaign source is captured on the lead record and travels to closed revenue.
- One attribution model is chosen as the dashboard default and labeled.
- Platform-reported figures are tagged separately from measured ones.
- Blended CAC sits at the top as the most defensible efficiency number.
- CAC payback uses a margin assumption confirmed with finance.
- A single refresh cadence is set and a “data through” timestamp is shown.
- Recent periods are marked provisional to account for attribution backfill.


## FAQ


**What is the difference between a marketing dashboard and a sales dashboard?**


A marketing dashboard tracks spend efficiency and top-of-funnel performance: spend, CAC, ROAS, lead volume, and channel mix. A sales dashboard tracks pipeline and deal execution: open pipeline, stage conversion, win rate, and rep performance. They share a few numbers (leads, MQLs) but answer different questions, so most teams build them separately and align on a common header.


**Why don’t the numbers in my ad platforms match GA4?**


Because each platform uses a different attribution model and window. Ad platforms count conversions they influenced within their own lookback period, GA4 deduplicates across channels with its own model, and none of them see your final billed revenue. The fix is to bring the data into one place and define the metrics yourself rather than trusting any single platform’s view.


**Do I need a data warehouse to build a marketing dashboard?**


Not to start. One or two sources at low volume can run on direct connections. You need a warehouse once you want to join spend to revenue across platforms, keep consistent definitions across teams, or handle more than two or three sources. A managed connector plus a warehouse is the durable pattern most teams settle on.


**What is blended CAC and why is it more trustworthy?**


Blended CAC is total acquisition spend divided by total new customers in a period, with no attribution required. Because it does not depend on assigning credit to specific channels, it sidesteps the attribution disagreements that make channel-level CAC contentious. It is the most defensible efficiency number and a good top-of-dashboard KPI, even though it cannot tell you which channel to scale.


**How fresh does marketing data need to be?**


Daily is enough for most marketing decisions. Attribution backfills for days after spend happens, so real-time refresh rarely changes a decision and adds cost and complexity. Set a scheduled refresh, show a “data through” timestamp, and mark recent periods as provisional.


A marketing dashboard does not need to be elaborate. It needs the sources mapped correctly, identity stitched across the funnel, one attribution model chosen and labeled, and a small set of measured metrics that the marketing and finance teams both agree on. Get that foundation right and the charts on top almost build themselves.
