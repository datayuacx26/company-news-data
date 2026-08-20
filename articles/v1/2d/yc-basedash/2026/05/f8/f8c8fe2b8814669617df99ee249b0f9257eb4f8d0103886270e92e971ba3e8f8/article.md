---
schema_version: "1.0.0"
document_id: "f8c8fe2b8814669617df99ee249b0f9257eb4f8d0103886270e92e971ba3e8f8"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-a-stripe-revenue-dashboard-data-model-metrics-and-tools/"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:90605ebc03745389cfd1d87e979bc4ee9684da142ed0d71a3b7bfc97a0e94360"
---

# How to build a Stripe revenue dashboard: data model, metrics, and tools

A Stripe revenue dashboard is a recurring view of MRR, churn, expansion, and net new ARR built directly from Stripe’s data. The hard parts are not the charts. They are choosing what counts as recurring revenue, dealing with refunds and proration, and deciding whether to query Stripe directly, sync it to a warehouse, or pay a metrics tool to do it for you.


This guide is for founders, finance leads, RevOps, and analytics engineers building a Stripe-driven revenue dashboard for the first time, or rebuilding one that nobody trusts. It covers the Stripe data model you actually need to understand, the metrics that matter, the calculation pitfalls that quietly break dashboards, and the tools that fit different team sizes.


## What Stripe gives you out of the box


Before building anything, it is worth understanding what Stripe already provides.


The Stripe Dashboard ships with a set of pre-built reports under “Revenue” and “Billing”: MRR, MRR growth, churn, customer count, and a few cohort views. For a small SaaS company on Stripe Billing with simple subscriptions, this is often enough for the first year.


Where it stops being enough:


- The MRR definition is fixed. You cannot exclude one-time fees, normalize discounted plans, or change how you treat downgrades.
- It does not blend Stripe with product, CRM, or marketing data.
- Cohort segmentation is limited. You cannot easily slice MRR by plan, segment, geography, or signup source.
- There is no way to add forecasts, targets, board metrics, or commentary.
- Multi-account or multi-entity Stripe setups have to be merged elsewhere.


Once any of those become a real constraint, you outgrow the built-in reports and need a dashboard built on top of Stripe data.


## The Stripe data model in practice


Stripe’s API is well documented, but a small set of objects do most of the work for revenue analytics. Knowing what each one represents and how they connect is the difference between a dashboard you trust and one you do not.


The objects you cannot avoid:


- **Customer.** A billable entity. One company may have multiple customers if you create them per workspace or per environment.
- **Subscription.** A recurring billing relationship between a customer and one or more prices. Has a status (` active` ,` trialing` ,` past_due` ,` canceled` ,` incomplete` , etc.) and one or more` subscription_items` .
- **Subscription item.** A line on a subscription tied to a` price` . Quantity matters here for seat-based plans.
- **Price and product.** Prices belong to products. The price stores` unit_amount` ,` currency` ,` interval` , and whether it is metered or licensed.
- **Invoice.** Issued for each billing period of a subscription, plus one-offs. Contains` amount_due` ,` amount_paid` ,` period_start` ,` period_end` , and line items that link back to subscription items, prices, and proration events.
- **Charge and payment_intent.** The actual money movement, with status (` succeeded` ,` failed` ,` refunded` ) and` amount_refunded` .
- **Refund and dispute.** Money going the other way.
- **Coupon and discount.** Applied to subscriptions or invoices, with either fixed or percentage off, and either repeating or one-time durations.


For a revenue dashboard, the most useful “shape” of this data is one row per subscription period and one row per invoice line item. Almost every meaningful metric can be derived from those two grains.


A few less obvious things to know:


- **Proration is its own line item.** When someone upgrades mid-cycle, Stripe writes a credit line and a new charge line on the next invoice. Aggregating raw` amount` will double-count if you are not careful.
- **A canceled subscription stays in the data forever.** Filter by` status` and` canceled_at` , not by deletion.
- **Trial periods produce zero-amount invoices.** They still affect MRR if you count trialing customers as recurring revenue (most companies do not).
- **Currency is per-charge.** If you sell in multiple currencies, you need a daily FX table to normalize to one reporting currency. Stripe does not normalize for you in the API.


## Where the data should live


The first real architectural decision is where Stripe data lives when your dashboard reads it.


There are four common options.


### 1. Query Stripe directly via API or Sigma


Stripe Sigma provides SQL access to your Stripe data inside the Stripe Dashboard, billed per query. It is the fastest way to start because there is no sync to set up, and it is fine for ad hoc analysis and small dashboards.


Limits: you cannot join Stripe to product or CRM data. You cannot embed Sigma queries in your own BI tool. Usage-based pricing gets expensive once a dashboard is refreshed often.


### 2. Use a Stripe-native metrics tool


ChartMogul, Baremetrics, ProfitWell (Paddle), and Maxio all sync from Stripe and give you pre-built MRR, churn, and cohort dashboards.


Strengths: opinionated metric definitions, fast to set up, support multi-currency. Weaknesses: rigid metric definitions you cannot easily change, limited blending with non-Stripe data, per-seat pricing that scales with revenue.


These tools are a good fit for early-stage SaaS companies that need credible board-quality metrics without a data team. They become a constraint once finance wants to define metrics differently or RevOps wants to blend with HubSpot or Salesforce.


### 3. Sync Stripe to a data warehouse


Use Fivetran, Airbyte, Stitch, or Stripe’s own data pipeline to land Stripe data in Snowflake, BigQuery, Redshift, or Postgres. Then model it with dbt or SQL views and read from a BI tool.


This is the dominant pattern at any company with a data team. It gives you full control of metric definitions, joins to product and CRM data, and a single source of truth for finance. It also has the highest setup cost.


Fivetran’s Stripe connector and the[stripe dbt package](https://hub.getdbt.com/fivetran/stripe/latest/) cover most of the modeling work. The package ships with` stripe__customer_overview` ,` stripe__balance_transactions` , and` stripe__subscription_line_items` models that handle proration, currency, and refunds.


### 4. A lightweight BI tool reading from your warehouse or Stripe


For teams that want flexibility without standing up a metrics platform, a BI tool reading from a warehouse (or directly from Stripe via the API) is often the right balance. Tools like Basedash, Metabase, Hex, and Mode can sit on top of either pattern.


Basedash specifically can connect to a warehouse that holds Stripe data, or to a Postgres database where Stripe data has been replicated. Its AI features let non-technical operators ask follow-up questions in plain English without writing SQL, which matters more for a revenue dashboard than for most other dashboards because finance, sales, and support all want to slice it differently.


A reasonable rule of thumb:


- Under 100 subscriptions and no data team: Stripe Sigma or ChartMogul.
- 100 to 5,000 subscriptions, lean team: warehouse + dbt + a flexible BI tool like Basedash or Metabase.
- 5,000+ subscriptions or multi-entity finance: warehouse + dbt + a BI tool, plus a metrics platform if board reporting needs more polish.


## The metrics that belong on a Stripe revenue dashboard


A revenue dashboard built on Stripe should cover four layers. Anything else is a different dashboard.


### Recurring revenue


MRR and ARR. The non-trivial part is the definition.


A clean MRR definition includes only normalized recurring subscription revenue from active paying customers, and excludes:


- One-time fees (setup, services, hardware)
- Usage-based overages that are not contractually committed
- Discounts that are time-bound (apply the post-discount rate)
- Trialing customers (most companies count them only once they convert)
- Refunded invoices (decrement MRR if the refund is for a recurring period)


Pick the definition once, write it down, and keep it consistent across the dashboard. The most common reason a Stripe dashboard loses trust is that two views of MRR do not reconcile because someone counted a setup fee in one place but not another.


### Net new ARR and the growth waterfall


The single most useful number on a SaaS revenue dashboard is net new ARR per period, broken into four buckets:


- **New business ARR** from customers who were not paying at the start of the period.
- **Expansion ARR** from existing customers (seat upgrades, plan upgrades, cross-sells).
- **Contraction ARR** from existing customers who downgraded but did not cancel.
- **Churned ARR** from customers who fully canceled.


Net new ARR = New + Expansion − Contraction − Churned. As a stacked bar chart over time, this is usually the most-referenced visual on the dashboard because it explains *why* ARR is moving.


In Stripe data, this requires comparing each customer’s MRR snapshot at the start and end of the period, then attributing the delta to one of the four buckets. The dbt Stripe package handles most of this; rolling your own takes a bit of care with proration and trial conversions.


### Churn and retention


There are two churn numbers worth tracking, and they answer different questions.


- **Logo churn** (customers canceled / customers at start) tells you how good your product is at keeping customers.
- **Net revenue retention (NRR)** ((starting ARR + expansion − contraction − churn) / starting ARR) tells you whether existing customers are growing or shrinking on a dollar basis.


For a B2B SaaS company, NRR above 100% is the headline benchmark for “good.” For consumer or SMB SaaS, logo churn matters more.


Both should be shown on a rolling monthly basis. Single-period churn numbers swing too much to be useful.


### Composition


Once the headline numbers are in place, finance and operators almost always want to slice them:


- MRR by plan or product
- New ARR by acquisition channel (requires joining Stripe to your CRM or marketing data)
- Churn by tenure cohort (customers who signed up in Q1, Q2, etc.)
- ARR by segment (SMB / mid-market / enterprise, ideally tagged in Stripe metadata)
- Failed-payment dollars and recovery rate


Composition charts belong below the headline section. Putting them on top makes the dashboard noisy.


## Common Stripe data gotchas


A handful of issues quietly break most Stripe dashboards. They are worth checking explicitly.


### Refunds and disputes


Stripe records refunds against the original charge. If you calculate revenue as` sum(invoice.amount_paid)` , you will overstate it. Subtract` sum(refund.amount)` and` sum(dispute.amount)` for fully resolved disputes. For partial refunds, use the line-item-level` amount_refunded` .


### Proration on plan changes


When a customer upgrades mid-cycle, Stripe issues a credit for the unused portion of the old plan and a new charge for the new plan. Both appear as line items on the next invoice. Aggregating raw amounts double-counts the upgrade. The fix is to look at the subscription’s MRR before and after the change date, not the invoice line items.


### Trials and zero-amount invoices


Trialing customers produce invoices for zero. Including them in MRR overstates revenue; excluding them from customer counts understates active customers. Most teams settle on “active paying customers” for revenue metrics and “active including trials” for usage metrics.


### Multi-currency


Stripe stores` amount` in the local currency’s smallest unit (cents, pence, etc.). To get a single MRR number, normalize to one reporting currency using a daily FX table. The Stripe API does not do this for you. If you sell in five or more currencies, do not skip this.


### Coupons and discounts


A 50% off forever coupon is part of MRR forever. A 50% off for three months coupon affects MRR only for three months. Get the duration right or expansion ARR will look fake when those discounts roll off.


### Multi-account Stripe


If you have separate Stripe accounts for different products or geographies, the data needs to be unified before metrics are computed. A single customer or company may exist as different` customer.id` values in each account.


### Annual vs monthly normalization


Annual subscriptions need to be normalized to monthly for MRR (` amount / 12` ), but the cash hits Stripe upfront. Without a clear convention, monthly MRR and cash collected will not agree, and finance will lose trust in the dashboard.


## A practical Stripe dashboard layout


A useful Stripe revenue dashboard is short. Aim for one page that loads quickly and answers the three questions a CEO actually asks: how big is recurring revenue, how fast is it growing, and where is the change coming from.


A layout that works well:


1. **Top row:** current ARR, monthly net new ARR, monthly NRR, customer count. One number each, with a 30-day delta.
2. **Growth waterfall:** stacked bar of net new ARR by component (new, expansion, contraction, churn) over the last 12 months.
3. **MRR over time:** smooth line of total MRR for the last 18 to 24 months.
4. **Churn:** logo churn and NRR, monthly, last 12 months.
5. **Composition:** MRR by plan or segment, plus failed-payment dollars and recovery rate.


Avoid splitting this across multiple pages. A revenue dashboard is most useful when it can be read in under a minute.


## When to graduate from a Stripe-only metrics tool


It is common to start on Baremetrics or ChartMogul and outgrow it. The signals are fairly consistent:


- Finance disagrees with the tool’s MRR definition and has built a parallel spreadsheet.
- Sales wants to blend ARR with HubSpot or Salesforce pipeline data, and the metrics tool cannot do it.
- Multi-entity reporting (separate Stripe accounts) needs manual reconciliation every month.
- Per-seat or revenue-tiered pricing has crossed the cost of a small data stack.
- The board is asking for cohort and segmentation views the tool does not support.


When two or more of those are true, the move is usually to a warehouse-backed dashboard. The Stripe-native tool can stay around for a while as a reference, but the source of truth shifts to the warehouse.


## A short implementation checklist


- Pick one MRR definition. Write it down. Reuse it everywhere.
- Decide where Stripe data will live (Sigma, metrics tool, warehouse, or BI tool reading the API).
- If using a warehouse, set up Fivetran or Airbyte and adopt the dbt Stripe package rather than rolling your own SQL.
- Normalize multi-currency before computing metrics.
- Handle refunds, disputes, proration, and trial conversions explicitly.
- Build the dashboard as one page with four sections: headline, waterfall, MRR, churn, composition.
- Set ownership. A Stripe dashboard with no owner drifts within a quarter.
- Reconcile to Stripe’s own MRR number once a month. If they diverge by more than a couple of percent, find the cause.


A good Stripe revenue dashboard is small and stable. Most of the work is upstream of the charts: defining what counts, modeling the data correctly, and choosing the right place for it to live. Get those right and the dashboard mostly takes care of itself.


If you are choosing tools for the BI layer, our guides on[the best AI-native BI tools compared](https://www.basedash.com/blog/best-ai-native-bi-tools-compared-2026) and[how to build a SaaS revenue dashboard](https://www.basedash.com/blog/how-to-build-a-saas-revenue-dashboard-metrics-data-sources-and-structure) cover related decisions in more depth.
