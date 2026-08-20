---
schema_version: "1.0.0"
document_id: "e47a3491e5ed94e4da44410aea45890faeee2bd63bdd5d7b24ce8388c8c3276b"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-set-up-ecommerce-analytics-from-shopify-and-ad-data-to-real-profit/"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:1bb4bca6dc3cb3a0d0540c995fd233105630a0dfc64d7151cd11a5dfed9a9e86"
---

# How to set up ecommerce analytics that show real profit, not just revenue

Most ecommerce analytics break at the same point: they tell you how much you sold, not how much you kept. Shopify shows gross sales, the ad platforms each claim their own revenue, and the actual profit on an order, after discounts, returns, payment fees, shipping, cost of goods, and acquisition spend, lives in no single tool. Useful ecommerce analytics means pulling those sources into one place and computing the numbers your platforms refuse to: contribution margin, profit after ad spend, and new versus returning revenue.


This guide is for founders, operators, and analysts at DTC and online retail brands who have outgrown Shopify’s built-in reports and want analytics that reflect real economics. It covers why platform-native reporting falls short, the data sources you have to connect, a profit framework most dashboards skip, where the data should live, and the metrics worth defining.


## Why platform-native ecommerce reports fall short


Shopify Analytics, Meta Ads Manager, and Google Ads each report a partial, self-interested view. They are fine for a quick read on traffic and sales, but they cannot answer the questions that decide whether a brand is actually making money.


- **Revenue is not profit.** Shopify reports gross and net sales. It does not subtract your cost of goods, payment processing fees, pick-and-pack, shipping subsidies, or ad spend. A product line can be a top seller and still lose money on every order.
- **Every ad platform over-claims.** Meta and Google each count conversions they influenced using their own attribution windows. Add up what each platform reports and the total exceeds the orders that actually happened. This is the same double-counting problem covered in[how to build a marketing dashboard](https://www.basedash.com/blog/how-to-build-a-marketing-dashboard-connecting-ga4-ads-and-crm-data) , and it is worse in ecommerce because the cart is the conversion.
- **No platform sees the full order lifecycle.** A sale today can become a partial refund in three weeks. Platform-native ROAS is calculated on the order, not on what the customer ultimately kept. Returns silently erode the numbers you reported.
- **Returning customers are invisible to ad platforms.** Meta does not know that a “conversion” was a loyal customer who would have bought anyway. Blending new and returning revenue hides whether acquisition is actually working.


The practical consequence: you cannot judge profitability by reading any single dashboard. You have to bring spend, revenue, costs, and refunds into one model and define the metrics yourself.


## The data sources behind ecommerce analytics


Before connecting anything, map each source and what it is the authority for. The most common failure is pulling the same metric from two systems and getting two answers.


Source What it is the authority for Typical connection method


Shopify / WooCommerce / BigCommerce Orders, line items, discounts, refunds, customers, fulfillment Admin API or managed connector


Meta Ads, Google Ads, TikTok Ads Ad spend, impressions, clicks, campaign metadata Platform API or managed connector


Payment processor (Stripe, Shopify Payments) Processing fees, payouts, chargebacks Connector or platform export


Cost of goods (ERP, NetSuite, a spreadsheet) Unit COGS, landed cost, supplier data Connector, export, or manual table


Fulfillment (ShipBob, ShipStation, 3PL) Pick-pack, shipping cost per order API or CSV export


Email and SMS (Klaviyo, Attentive) Owned-channel attributed revenue Connector


The dividing line that keeps analytics honest: the commerce platform owns the order and the customer, ad platforms own spend, your COGS source owns unit costs, and the processor owns fees. Any profit metric needs at least three of these joined together, which is why no native dashboard can produce it.


## The profit layers most dashboards skip


The single most useful thing ecommerce analytics can do is walk an order from gross revenue down to the profit you actually keep. Most teams stop at revenue or blended ROAS. The number that should drive spend decisions is further down. Think of it as five layers.


Layer What it is What it subtracts


Gross revenue Order value at full price Nothing


Net revenue Revenue you actually recognize Discounts, returns, refunds, taxes


Gross margin Product profit Cost of goods sold (COGS)


Contribution margin Profit per order before marketing Payment fees, shipping, pick-and-pack


Profit after ad spend What acquisition leaves behind Advertising and promotion


Each layer changes the decision. Gross revenue tells you nothing about health. Net revenue exposes a discounting or returns problem. Contribution margin tells you whether a product or order is structurally profitable before you spend a cent acquiring the customer. Profit after ad spend, sometimes computed at the blended level as net revenue minus total marketing cost, tells you whether the whole machine makes money.


A brand can grow gross revenue 40 percent and shrink profit after ad spend at the same time. That only shows up if your analytics carry every order through all five layers instead of stopping at the top.


## Where ecommerce data should live: Shopify, a warehouse, or both


The right architecture depends on how many sources you have and how much history you need.


**Stay platform-native** if you sell on one channel, run light paid spend, and mostly need revenue and traffic. Shopify’s reports plus an app or two are enough. Adding a warehouse here is overhead you will not use.


**Move to a warehouse** once profitability requires joining three or more sources, you want order history that outlives a tool’s retention window, or different teams keep arguing about whose number is right. A central store in BigQuery, Snowflake, Redshift, or Postgres becomes the single source of truth, with ELT connectors (Fivetran, Airbyte) syncing Shopify, ad platforms, and processors into it. This is the same threshold described in[when to add a data warehouse](https://www.basedash.com/blog/when-to-add-a-data-warehouse-signals-your-startup-has-outgrown-its-production-database) .


**Run a hybrid** in the common middle ground: live-query Shopify for operational, near-real-time order monitoring, and use the warehouse for blended profitability and historical analysis. The tradeoffs between live queries, blending, and a warehouse are covered in[how BI tools combine data from multiple sources](https://www.basedash.com/blog/how-bi-tools-combine-data-from-multiple-sources-federation-blending-and-warehouses) .


One practical note on connections: the Shopify Admin API is rate-limited using a leaky-bucket model, so pulling large order histories means paginating and respecting throttles rather than one big extract (see the[Shopify API rate limits docs](https://shopify.dev/docs/api/usage/rate-limits) ). Managed connectors handle this for you, which is the main reason most teams use one instead of writing their own sync.


## The metrics worth defining


Define these once, in one place, so every report agrees. Vague labels like “revenue” are where dashboards diverge.


Metric Definition Why it matters


Net revenue Gross sales minus discounts, returns, and refunds The only revenue figure worth tracking over time


AOV Net revenue divided by orders Detects discounting and bundle effects


Contribution margin Net revenue minus COGS, fees, shipping, fulfillment Whether an order is profitable before acquisition


MER Total net revenue divided by total marketing spend A blended, platform-agnostic efficiency check


New customer CAC Ad spend divided by new customers acquired The cost that paid growth actually carries


CAC payback Months for contribution margin to recover CAC Whether acquisition is sustainable


Repeat purchase rate Share of customers with two or more orders The lever behind long-term profitability


Returning revenue share Revenue from prior customers as a share of total Separates real acquisition from base demand


Return rate Refunded order value as a share of gross A margin killer hidden in delayed refunds


Two of these deserve emphasis. MER (marketing efficiency ratio) sidesteps the attribution wars by ignoring which platform claimed which sale and asking only whether total revenue justifies total spend. And splitting new versus returning revenue is what stops a brand from celebrating “great ROAS” that is really loyal customers buying again.


## The ecommerce-specific data problems


Ecommerce data has quirks that generic analytics advice misses.


- **Guest checkout breaks customer identity.** The same person can order under multiple emails or as a guest each time, inflating “new customer” counts. Stitch identity on email plus address or phone before computing repeat rate or CAC.
- **Refunds arrive late.** An order recorded as revenue this week may be partly refunded next month. Model refunds against the original order date, not the refund date, or your historical net revenue will keep silently changing.
- **Discounts are not one thing.** Automatic discounts, codes, and price drops hit margin differently. Keep them as separate fields so you can see which promotions actually erode contribution margin.
- **COGS changes over time.** Landed cost shifts with supplier pricing and freight. Store COGS with an effective date so old orders keep their real cost instead of today’s.
- **Subscriptions and one-time orders mix.** If you sell both, separate them. Subscription churn and one-time AOV behave nothing alike and should not be averaged together.


## Common mistakes


- **Reporting blended ROAS as if it were profit.** ROAS ignores COGS, fees, and returns. A 3x ROAS on a low-margin product can still lose money.
- **Trusting each platform’s reported conversions.** Summing Meta and Google conversions double-counts. Reconcile against actual orders in Shopify.
- **Ignoring returns until quarter-end.** Returns belong in the daily model, attributed back to the original order, not bolted on later.
- **Averaging new and returning customers together.** It hides whether acquisition works and flatters CAC.
- **Defining metrics inside each chart.** When AOV is calculated three different ways across three dashboards, no one trusts any of them. Define metrics once, upstream.


## When platform-native analytics is enough


You do not need a warehouse or a BI tool for every store. Stay with Shopify Analytics and a focused app when you sell on a single channel, run little or no paid acquisition, carry simple and stable margins, and need mostly revenue and traffic visibility. The moment you start spending meaningfully on ads, selling across channels, or arguing about profitability, the gap between native reports and reality becomes the reason to build real analytics.


## Tools for ecommerce analytics


A few categories fit different stages, and a fuller breakdown lives in[the best ecommerce analytics tools compared](https://www.basedash.com/blog/best-ecommerce-analytics-tools-compared-2026) .


- **Platform-native dashboards** (Shopify Analytics) are free and instant but cannot join ad spend, COGS, or returns.
- **Purpose-built ecommerce analytics** (Triple Whale, Glew.io) connect Shopify and ad platforms directly and ship pre-built profit and attribution views, best for Shopify-centric brands without a warehouse.
- **Warehouse-native BI** (Looker, Sigma, Metabase, Basedash) connects to the warehouse where Shopify, ad, and cost data have been unified. This is where contribution margin and profit-after-ad-spend models live once your data is centralized.[Basedash](https://www.basedash.com/) adds plain-English querying on top, so an operator can ask “contribution margin by product over the last 90 days” without writing SQL.


The right choice tracks the architecture decision above. Platform-native for single-channel simplicity, purpose-built for Shopify-first brands, warehouse-native once profitability spans many sources.


## FAQ


### What is the difference between ROAS and MER?


ROAS (return on ad spend) is reported per platform and counts only the revenue that platform claims, so the numbers double-count across Meta and Google. MER (marketing efficiency ratio) is blended: total net revenue divided by total marketing spend across all channels. MER avoids the attribution arguments and is harder to game, which is why many DTC teams treat it as the headline efficiency metric.


### How do I calculate true profit per order in ecommerce?


Start from the order’s net revenue (after discounts and refunds), subtract cost of goods sold, then subtract variable costs such as payment processing fees, shipping, and pick-and-pack. That gives contribution margin. Subtracting attributed or blended acquisition cost gives profit after ad spend. You need data from the commerce platform, your COGS source, the processor, and the ad platforms to compute it, which is why it cannot come from one dashboard.


### Do I need a data warehouse for ecommerce analytics?


Not always. A single-channel store with light ad spend can run on Shopify’s reports. You need a warehouse once profitability requires joining three or more sources, you want order history beyond a tool’s retention window, or teams disagree about whose numbers are right.


### Why don’t my ad platform numbers match Shopify?


Each ad platform attributes conversions using its own window and view-through rules, and they all claim overlapping orders. Shopify records the actual order. Reconcile against Shopify as the source of truth for orders and revenue, and use blended metrics like MER instead of summing platform-reported conversions.


### How should I handle returns in ecommerce reporting?


Attribute refunds back to the original order date, not the date the refund is processed. This keeps historical net revenue and contribution margin stable instead of shifting every time a late refund lands. Track return rate as its own metric, since it is a common and easily hidden drag on margin.
