---
schema_version: "1.0.0"
document_id: "69106ce53c189b619467fe67a8619e9a58494513cb3332882f1b864e203bc0d1"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-a-saas-revenue-dashboard-metrics-data-sources-and-structure/"
published_at: "2026-05-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:15:07.431097+00:00"
content_hash: "sha256:ee6a422daa45b71cab28788cd0e22cd697fd61e564e569243a6278fd455017b0"
---

# How to build a SaaS revenue dashboard: metrics, data sources, and structure

A good SaaS revenue dashboard answers three questions in under thirty seconds: how much recurring revenue do we have, how fast is it changing, and where is the change coming from. Anything else is supporting detail.


This guide walks through how to build that dashboard from scratch — which metrics to include, where the underlying data comes from, how to lay it out, and the mistakes that quietly make most revenue dashboards untrustworthy. It is aimed at founders, finance leads, and operators at SaaS companies between roughly $50K and $50M in ARR who want a single source of truth for revenue without buying a heavyweight finance platform.


## What a SaaS revenue dashboard should actually do


A revenue dashboard has a narrower job than people often assume. It is not a finance close tool, a forecasting model, or a customer success workbench. Its job is to give the leadership team and operators an accurate, current view of recurring revenue and the levers that move it.


In practice, that means three layers:


1. **Top-line health.** Current MRR or ARR, growth rate, and net new ARR for the period.
2. **Composition.** Where new ARR is coming from (new business, expansion, contraction, churn) and how that mix is shifting.
3. **Segments.** Revenue by plan, customer cohort, segment, or channel — enough to explain why the top line is moving.


If your dashboard answers those three things clearly, it is doing its job. Adding pipeline coverage, support tickets, NPS, or product usage tends to dilute it. Those belong on adjacent dashboards.


## The metrics that belong on a SaaS revenue dashboard


Pick a small core set and resist the temptation to add more. The following metrics cover most SaaS businesses and map directly to investor and board reporting.


### Recurring revenue: MRR and ARR


MRR (monthly recurring revenue) and ARR (annual recurring revenue) are the same number expressed at different cadences. ARR is just MRR multiplied by twelve.


The non-trivial part is defining what counts. A clean definition includes only normalized recurring subscription revenue from active paying customers — typically excluding:


- Usage-based overages that are not contractually committed
- One-time setup fees, professional services, and implementation revenue
- Free trials and unpaid pilots
- Discounts that are clearly time-bound (apply the post-discount rate, or annualize correctly)


Pick the definition once, write it down, and apply it consistently. The most common reason a revenue dashboard loses trust is that two views of MRR don’t reconcile because someone counted a pilot or a one-time fee in one place but not another.


### Net new ARR and the growth waterfall


Net new ARR for a period is the most useful single number for understanding momentum. It breaks down into four components — the “growth waterfall”:


- **New business ARR** — ARR from customers who were not paying customers at the start of the period.
- **Expansion ARR** — additional ARR from existing customers (seat upgrades, plan upgrades, additional products).
- **Contraction ARR** — reductions in ARR from customers who downgraded but did not churn.
- **Churned ARR** — ARR lost from customers who fully cancelled or were involuntarily churned.


Net new ARR = New + Expansion − Contraction − Churned. This single chart, shown as a stacked bar over time, is often the most-referenced visual on a SaaS revenue dashboard because it explains *why* ARR is moving.


### Retention metrics: GRR and NRR


Gross revenue retention (GRR) measures what percentage of starting ARR you retained from existing customers, ignoring expansion. Net revenue retention (NRR) measures the same thing but counts expansion.


- **GRR** = (Starting ARR − Churned ARR − Contraction ARR) / Starting ARR
- **NRR** = (Starting ARR + Expansion ARR − Churned ARR − Contraction ARR) / Starting ARR


Both should be calculated on a trailing-twelve-month basis to smooth out monthly noise. NRR above 110% is generally considered strong for B2B SaaS; below 90% is a warning sign.


### Customer counts and ARPA


Logo growth complements ARR growth. Track:


- **Active customers** — paying accounts at the end of the period.
- **New logos** — paying accounts added in the period.
- **Churned logos** — paying accounts that fully cancelled.
- **ARPA** (average revenue per account) = total ARR / active customers.


ARPA is useful because the same ARR growth can come from many small customers or a few large ones, and the implications for sales motion, support cost, and concentration risk are very different.


### Optional but useful


Depending on your business, also consider:


- **CAC payback** — months of gross margin needed to recover acquisition cost. More relevant if you have meaningful sales and marketing spend.
- **Quick ratio** — (New ARR + Expansion ARR) / (Churned ARR + Contraction ARR). A simple efficiency signal; a healthy SaaS business is typically above 4.
- **Bookings vs. ARR** — bookings include new contracts not yet live; useful if you have meaningful contract length variance.
- **Billings or cash collected** — actual cash, distinct from accrued recurring revenue. Important if your subscription terms vary (monthly, annual prepaid, multi-year).


For most teams, CAC, LTV, and unit economics belong on a separate growth or finance dashboard rather than on the revenue dashboard itself. Mixing them in dilutes the revenue narrative.


## Where the data actually comes from


This is the part most teams underestimate. A SaaS revenue dashboard typically pulls from three or four systems, and reconciling them is where most of the work lives.


### Stripe (or your billing system)


For most SaaS companies, the billing system — Stripe, Chargebee, Recurly, Maxio, Paddle — is the system of record for what was actually charged. It is the cleanest source for cash collected and for recurring charges, but it is **not** automatically a clean source for MRR.


Stripe in particular requires care. A few specific gotchas:


- A subscription’s` plan.amount` is not always equal to its effective monthly revenue. Annual plans, custom prices, and quantity-based pricing all complicate the math.
- Refunds, disputes, and credit notes need to be subtracted from cash collected, but should usually not reduce MRR retroactively unless they reflect a true cancellation.
- Trials, paused subscriptions, and unpaid invoices need explicit rules — does an unpaid past-due account still count as MRR for thirty days? Sixty?
- Proration on plan changes can create misleading one-time line items that look like expansion or contraction.


Stripe’s own[revenue recognition reports](https://stripe.com/docs/revenue-recognition) help, as do tools like[ChartMogul](https://chartmogul.com/) ,[Maxio](https://www.maxio.com/) , or[Mosaic](https://www.mosaic.tech/) that specialize in SaaS metrics out of Stripe data. If you are doing it yourself, the cleanest pattern is to compute MRR from a subscription snapshot table rather than from Stripe events.


### Your application database


Your app database tells you which accounts are active, what plan they are on, when they signed up, and any internal flags (paying / trial / churned / paused). This is essential for segment analysis — revenue by plan, by signup cohort, by region, by self-serve vs. sales-led, etc.


The dashboard usually needs to join billing data with application data on a customer or workspace identifier. This join is where definitions matter most — does “customer” mean a Stripe customer, a workspace, an organization, or a user?


### Your CRM (for sales-led motions)


If you have a sales team, the CRM (Salesforce, HubSpot, Attio) is where new ARR, expansion, and downgrades get attributed to deals, segments, and reps. Pull from the CRM if you want revenue by segment, by industry, or by sales rep. Skip it if you are pure self-serve.


### A data warehouse, if you have one


For anything beyond a few thousand customers, the practical pattern is to centralize Stripe, your app database, and your CRM in a warehouse — Snowflake, BigQuery, Redshift, ClickHouse, or Postgres — using an ELT tool like Fivetran, Airbyte, or Stitch, then build the dashboard on top of clean modeled tables.


For smaller teams, you can build a credible revenue dashboard directly on Postgres with a few well-designed views and a[BI tool that connects natively to your database](https://www.basedash.com/blog/best-bi-dashboarding-tools-for-postgresql-2026) . The warehouse is not strictly required until your data volume or query complexity demands it.


## A reference structure for the dashboard


The structure below works for most B2B SaaS companies and matches how leadership teams actually scan the data.


### Section 1: top-line scorecard


The first thing visible — large KPI cards, no scrolling required.


- Current ARR (or MRR), with month-over-month and year-over-year change
- Net new ARR for the current month, with comparison to plan or prior period
- Active customers, with delta
- NRR (trailing twelve months)
- GRR (trailing twelve months)


If someone has thirty seconds, this section should be sufficient for them to know whether things are on track.


### Section 2: ARR over time


A single line chart of ARR (or MRR) over the past 12–24 months, with a clear marker for the current period. Include a comparison line to plan if you have one.


This is the chart investors and the board will look at first. Keep it clean, don’t add a second axis, and don’t try to overlay every metric onto it.


### Section 3: the growth waterfall


A stacked bar chart by month showing new business, expansion, contraction, and churn. This is the most analytically dense chart on the dashboard — most “why is ARR slowing?” conversations get answered here.


Pair it with the same data shown as a small table for the most recent period, with absolute numbers and percentages.


### Section 4: retention


NRR and GRR by trailing twelve months, plotted over time. Optionally a cohort retention table — starting cohorts as rows, months since signup as columns, with retained ARR as the value. Cohort tables are dense; only include one if your audience knows how to read it.


### Section 5: segmentation


The “why” charts. Choose two or three breakdowns that match how your business actually segments revenue:


- ARR by plan (e.g., Free, Team, Business, Enterprise)
- ARR by acquisition channel (self-serve, sales-led, partner)
- ARR by industry, geography, or company size
- ARPA over time


If you have more than four segmentation views, consider splitting them into a separate “revenue segments” dashboard. The main revenue dashboard should stay tight.


### Section 6: a small table of recent customer changes


Often overlooked, but very useful: a small table at the bottom listing recent material customer events — new deals over $X, expansions over $Y, churns over $Z. This grounds the abstract numbers in specific accounts and is the part the CEO will read most carefully.


## Refresh cadence and freshness


Most revenue dashboards do not need to be real-time. Daily is plenty for nearly every SaaS team. The exceptions:


- **High-velocity self-serve businesses** with hundreds of signups and churns per day may want an intra-day refresh on top-line ARR.
- **The last day of the month or quarter** , when finance is reconciling — temporarily worth more frequent refreshes.


A standard pattern is a daily refresh at 6 AM in the company’s primary time zone, so the dashboard is fresh before the morning standup. Pair this with an[automated alert](https://www.basedash.com/blog/best-bi-tools-for-ai-anomaly-detection-and-smart-alerting-2026) that flags large day-over-day changes in MRR or churn so people don’t have to manually scan for surprises.


## Common mistakes that quietly break revenue dashboards


The technical setup is usually not the hard part. The hard part is keeping the numbers trustworthy over time.


**Inconsistent MRR definition.** The dashboard says one number, the board deck says another, the investor update says a third. Pick one definition, document it, and rebuild any view that disagrees. A revenue dashboard whose numbers don’t match the board deck is worse than no dashboard.


**Counting one-time revenue as MRR.** Setup fees, professional services, and one-time charges sneak into MRR through Stripe metadata or product mappings. Audit the source data quarterly. Anything that is not contractually recurring should be excluded.


**Mixing booked and recognized revenue.** Bookings (signed contracts) and recognized revenue (delivered) are different numbers and should not be summed together on the same chart. If you show both, label them clearly.


**Treating annual prepayments incorrectly.** A customer who pays $12,000 for an annual plan is $1,000 of MRR, not $12,000. This sounds obvious but is one of the most common errors in homegrown dashboards built directly off Stripe charges.


**Churn timing ambiguity.** Decide whether churn is recognized when the customer cancels, when their access ends, or when their last paid period ends. All three are defensible, but only one should be used. Document it.


**Not handling currency.** If you have customers in multiple currencies, decide whether ARR is reported in USD using the period-end FX rate, the period-average rate, or contract-locked rates. Inconsistency here will quietly make trends look wrong.


**Dashboards without owners.** A revenue dashboard without an owner becomes inaccurate within a quarter. Pricing changes, plan renames, and new product launches all change the source data. Assign ownership — usually finance or revenue ops — and review it monthly.


## Build vs. buy


There are three reasonable paths, depending on team size and complexity:


**Path 1: Specialist SaaS metrics tools.** ChartMogul, Maxio, Baremetrics, and similar tools plug directly into Stripe and produce a usable revenue dashboard within a day. They are the fastest route for teams that do not have a data team and run primarily on Stripe. The tradeoff is limited customization — they enforce their own definitions of MRR and segments, which may or may not match how you want to report.


**Path 2: A general BI tool on top of your own data.** Tools like Basedash, Metabase, Hex, Mode, or Looker Studio let you build a fully customized dashboard from your warehouse or production database. This is the right path if you want to control the definitions, segment by your own business logic, and combine billing data with product and CRM data. It requires more setup but produces a dashboard that genuinely matches how your team thinks about the business.


**Path 3: A hybrid.** Use a specialist tool for raw MRR plumbing and a BI tool for everything segment-, cohort-, and product-related. Common at companies between roughly $5M and $50M in ARR where finance owns the SaaS metrics tool and the data team owns the BI layer.


For teams already working in a database-connected BI tool,[Basedash](https://www.basedash.com/) lets you describe the dashboard you want — “show me ARR by month with a stacked bar of new, expansion, contraction, and churn, broken down by plan” — and build it from your Stripe and application data without having to wire each chart manually. That works well when you want a customized revenue dashboard but don’t want to spend a week of analyst time on it.


## A short checklist before you ship


Before publishing a SaaS revenue dashboard internally, verify:


- One documented definition of MRR / ARR exists and is linked from the dashboard.
- The top-line numbers reconcile to the most recent board deck or investor update.
- The growth waterfall components sum to net new ARR exactly.
- Annual subscriptions are correctly normalized to monthly.
- Trials, pauses, and unpaid past-due accounts are handled by an explicit rule.
- Churn is defined by a single event (cancellation date vs. access-end date) and applied consistently.
- The dashboard has a named owner and a monthly review on the calendar.
- A daily refresh runs at a fixed time, and someone is alerted if it fails.


A revenue dashboard built this way is not glamorous — most of the work is in the definitions, not the visualizations — but it is the one dashboard the leadership team will actually open every morning, and the one investors will ask to see during diligence. Getting it right is worth the time.


For teams thinking through how this dashboard fits into a broader BI setup, the principles in[how to build dashboards that drive decisions](https://www.basedash.com/blog/how-to-build-dashboards-that-drive-decisions-a-practical-guide) apply directly: start with the decisions the leadership team needs to make, keep the metric set small, and design for the audience scanning the dashboard rather than the analyst building it.
