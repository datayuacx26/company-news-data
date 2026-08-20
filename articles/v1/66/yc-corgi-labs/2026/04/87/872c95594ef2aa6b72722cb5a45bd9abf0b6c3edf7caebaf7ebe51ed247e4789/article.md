---
schema_version: "1.0.0"
document_id: "872c95594ef2aa6b72722cb5a45bd9abf0b6c3edf7caebaf7ebe51ed247e4789"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/resources/overview-dashboard"
published_at: "2026-04-23T15:15:34.987+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-22T22:57:19.890747+00:00"
content_hash: "sha256:aff0224dd0afdbdc6d974590b1c951ad0b8d030fa549c176a001d8b4af4e3e8d"
---

# Overview Dashboard

The Overview Dashboard is your home base in Corgi Intelligence. It provides an at-a-glance summary of your business health, powered by AI-generated Key Insights that surface the most important trends and opportunities in your payment data.


## Dashboard Modes: Retail vs. Subscription


Corgi Intelligence automatically tailors your Overview Dashboard based on your business model:


-


**Retail/e-commerce merchants** see metrics focused on Gross Merchandise Value (GMV), transaction counts, refund rates, average order value, customer lifetime value, and top products by revenue.


-


**Subscription/usage-based merchants** see metrics focused on Monthly Recurring Revenue (MRR), net revenue retention, gross revenue retention, logo churn rate, dunning recovery rate, deferred revenue balance, and revenue at risk.


Corgi Labs auto-detects the best dashboard mode for your business during onboarding. If a different view would be more appropriate, reach out to us and we will update your configuration.


## Dashboard Metrics


### Retail Dashboard


If your business primarily sells one-time products or services (e-commerce, marketplaces, physical goods), you will see the **Retail** overview.


Metric


Definition


Gross Merchandise Value (GMV)


Total value of all transactions processed in the selected period.


Number of Transactions


Total count of payment attempts.


Refund Rate


Percentage of transactions that were refunded, with trend comparison.


Number of Unique Customers


Distinct customers who made at least one purchase.


Repeat Purchase Rate


Percentage of customers making 2 or more purchases.


Customer Lifetime Value (CLV)


Average revenue per customer over their relationship with your business.


Average Order Value (AOV)


Mean transaction amount per order.


Top 10 Products by GMV Contribution


A ranked table showing your best-performing products by revenue, including purchase count, repeat customers, and percentage of total revenue.


### Subscription / Usage-Based Dashboard


If your business operates on a recurring or usage-based billing model (SaaS, subscriptions, AI token usage), you will see the **Subscription** overview.


Metric


Definition


Monthly Recurring Revenue (MRR)


Your predictable monthly revenue from active subscriptions.


Net Revenue Retention (NRR)


Measures expansion, contraction, and churn relative to your starting revenue. An NRR above 100% indicates growth from existing customers.


Gross Revenue Retention (GRR)


Revenue retained from existing customers, excluding expansion. Reflects how well you hold onto revenue before upsells.


Logo Churn Rate


Percentage of customers who canceled during the period.


Dunning Recovery Rate


Percentage of failed subscription payments that were successfully recovered through retry and dunning flows.


Deferred Revenue Balance


Revenue collected but not yet recognized, important for accounting and forecasting.


Revenue at Risk


Revenue associated with customers showing churn signals or failed payments.


## Key Insights


At the top of the Overview page, Corgi surfaces **AI-generated Key Insights** : automated observations about your most important payment trends. These insights are generated from your latest data and highlight items such as:


-


**Approval Upside:** Estimated additional revenue you could unlock by improving your approval rate, with specific dollar estimates.


-


**Disputes Down / Up:** Notable changes in your dispute rate compared to the previous period, including breakdowns by primary vs. comparison range.


-


**Churn Spike:** Alerts when churned revenue exceeds historical norms, with recommended actions (for example, aligning dunning and card updater efforts).


-


**Rule Bias:** Warnings when your fraud rules may be generating too many generic declines, with suggestions to refine rules and avoid suppressing legitimate customers.


Key Insights update automatically. They are designed to draw your attention to the most impactful changes without requiring you to dig through every chart.


## Payments & Fraud Overview


Both dashboard modes include a **Payments & Fraud Overview** section at the bottom of the page, showing:


Metric


Definition


Authorization Rate


Percentage of payment attempts approved by the issuing bank.


Dispute Rate


Percentage of transactions that resulted in a customer dispute.


Block Rate


Percentage of transactions blocked by your fraud rules or risk screening.


Abandonment Rate


Percentage of checkout sessions that were abandoned before payment completion.


Each metric includes a trend comparison against the previous period so you can spot changes at a glance. For deep dives, refer to Payment Analytics and Dispute & Fraud metrics, respectively.
