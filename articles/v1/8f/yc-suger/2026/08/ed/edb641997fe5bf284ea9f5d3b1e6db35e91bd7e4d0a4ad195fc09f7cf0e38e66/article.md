---
schema_version: "1.0.0"
document_id: "edb641997fe5bf284ea9f5d3b1e6db35e91bd7e4d0a4ad195fc09f7cf0e38e66"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/aws-marketplace-seller-insights/"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-10T02:28:43.687936+00:00"
fetched_at: "2026-08-10T02:28:45.704186+00:00"
content_hash: "sha256:9a9fe621bf394a0f8cf518429e312887a014ae480699ae728c3acc908dcdc110"
---

# What AWS Marketplace Seller Insights Actually Show

*AWS Marketplace Seller Insights is the set of dashboards on the Insights tab of the AWS Marketplace Management Portal, covering finance operations, sales operations, and marketing. There are eight, they are free, and by default only system administrators can see them.*


---


Ask a marketplace ops lead which AWS seller dashboards they use and you will usually hear about one: billed revenue. Ask which ones exist and the answer is often a pause.


That is a shame, because AWS ships eight of them, they cost nothing, and two of the least-used ones answer questions teams routinely build spreadsheets for.


What follows is every dashboard AWS publishes, what it actually reports in AWS’s own words, and — more usefully — what each one cannot tell you. The blind spots are the point. A dashboard you trust for something it does not measure is worse than no dashboard.


---


## **What are AWS Marketplace seller dashboards?**


AWS Marketplace provides dashboards, powered by Amazon Quick, “with charts, graphs, and insights that help you access and analyze financial, sales, and marketing data.” They are grouped into three families.


**Finance operations**


- **Billed revenue dashboard** — “Provides information about billed revenue for accounting and other financial reporting purposes.”
- **Collections and disbursement dashboard** — “Provides information about funds that AWS collected and disbursed to your bank accounts since the previous disbursement.”
- **Taxation dashboard** — “Provides information about taxes for seller transactions.”


**Sales operations**


- **Agreements and renewals dashboard** — “Provides information about agreements and renewals within 24 hours of signing an agreement in AWS Marketplace.”
- **Usage dashboard** — “Provides visualizations and fine-grained data for customers using SaaS and server usage-based products.”


**Marketing**


- **Customer agreements dashboard** — “Provides data about the agreements and customers who subscribe to your products.”
- **Listing performance dashboard** — “Provides data about the traffic and user behavior in your AWS Marketplace listings.”
- **Search performance dashboard** — “Provides data about the keywords applied to AWS Marketplace listings.”


Access is the first thing to fix. AWS: “By default, AWS Marketplace system administrators for seller accounts have access to all dashboards on the Insights tab in the AWS Marketplace Management Portal.” Everyone else sees nothing until an administrator writes an IAM policy granting` aws-marketplace:GetSellerDashboard` , either against all marketplace resources or against named dashboard ARNs.


That default is why the finance team is emailing the alliances lead for a revenue figure. It is a ten-minute IAM change.


---


## **Each dashboard and its blind spot**


Dashboard What it answers well The blind spot


Billed revenue What AWS invoiced buyers, by product and period, for accounting Invoiced is not collected and neither is disbursed. Read alone, it overstates cash


Collections and disbursement What arrived in your bank since the last disbursement It reports movement, not exposure. It will not tell you which invoices are ageing


Taxation Tax treatment across transactions It is per-transaction, not per-entity. Consolidated tax positions still need your own system


Agreements and renewals New agreements and upcoming renewal activity 24-hour latency after signature, and it is AWS-only. Your Azure and Google Cloud renewals are elsewhere


Usage Fine-grained consumption for SaaS and usage-based products It shows metered usage — what AWS *received* . Records you failed to send are invisible, and so is the revenue they represent


Customer agreements Which customers subscribe to what Marketplace-account identity, not CRM identity. Mapping a buyer account to your account record is on you


Listing performance Traffic and behaviour on your listings Correlating traffic with pipeline requires attribution the dashboard does not carry


Search performance Keywords buyers used to reach your listings Search terms inside AWS Marketplace only. It is not a substitute for keyword research


Three of those are worth expanding.


---


## **The usage dashboard’s blind spot is the expensive one**


The usage dashboard reports metering records AWS received. That is not the same as usage your customers had.


On a usage-priced product, AWS bills from records your software transmits, and there is no reconciliation that recovers unsent records after the fact. A metering job that silently stops produces a usage dashboard that looks like a quiet week. The dashboard cannot distinguish “the customer used nothing” from “we sent nothing.”


The check has to come from your side: reconcile metered quantity against your own product telemetry, and alert on the *absence* of records rather than only on errors. Which pricing model exposes you to this is covered in[contract pricing vs usage pricing](https://www.suger.io/resources/blog/aws-marketplace-contract-vs-usage-pricing/) .


---


## **Billed revenue got materially better in January 2026**


Until recently, the gap between an invoice and the money was something sellers inferred. In January 2026 AWS added collection visibility to seller reporting, adding payment collection status to the **Billed Revenue Dashboard** and the **Billing Event Data Feed** .


AWS’s framing: the change lets sellers “distinguish between invoiced, collected, and disbursed amounts,” which supports better “payment forecasting accuracy” and detects “collection issues earlier.” It sits under **Insights → Finance Operations** in the Management Portal.


If your marketplace revenue reporting predates that change, it is probably still treating one of the three states as the revenue number. Worth revisiting —[cloud marketplace metrics](https://www.suger.io/resources/blog/cloud-marketplace-metrics/) covers why all three belong on the same view.


---


## **The agreements and renewals dashboard is underused**


Of the eight, this is the one most likely to change a quarter, and the one fewest teams have open.


It reports agreements and renewals within 24 hours of signature. That makes it the closest thing AWS provides to a renewal exposure report — which agreements exist, when they end, and what renewed. Given that renewal is the only moment an AWS contract can shrink, a dashboard that surfaces upcoming renewal activity is a revenue-protection tool, not a reporting one.[AWS Marketplace renewals](https://www.suger.io/resources/blog/aws-marketplace-renewals/) covers the operating process it should feed.


Its limitation is scope, not quality: it reports AWS. If you sell on more than one marketplace, this dashboard can never be your renewal view.


---


## **What none of them can tell you**


Four questions sit outside all eight dashboards by construction.


**Anything about another marketplace.** Every AWS dashboard reports AWS. Microsoft and Google Cloud have their own consoles with their own vocabulary and their own refresh schedules. There is no cross-marketplace view because no marketplace has an incentive to build one.


**Anything joined to your CRM.** These dashboards know marketplace accounts. They do not know your account records, your opportunity stages, or which rep owns the relationship. Every “which of our customers bought through the marketplace” question requires a join AWS cannot perform.


**Anything about your offer pipeline before acceptance.** Offers issued and not yet accepted — the leading indicator of a stalling motion — are agreement activity that has not happened yet.


**Anything about your true cost.** Billed revenue is gross. Listing fees vary by product type, offer type, contract value, channel, and buyer region, as[what a marketplace dollar actually costs you](https://www.suger.io/resources/blog/what-a-marketplace-dollar-costs-you/) works through.


None of this makes the dashboards bad. They are well-built, free, and closer to source than anything you would construct. They are simply single-marketplace instruments, and most ISVs stopped being single-marketplace companies some time ago.


---


## **Frequently asked questions**


**What is AWS Marketplace Seller Insights?** It is the set of seller dashboards on the Insights tab of the AWS Marketplace Management Portal, powered by Amazon Quick, covering finance operations, sales operations, and marketing data.


**How many AWS Marketplace seller dashboards are there?** Eight: billed revenue, collections and disbursement, taxation, agreements and renewals, usage, customer agreements, listing performance, and search performance.


**Who can see AWS Marketplace seller dashboards?** By default, only AWS Marketplace system administrators for the seller account. Administrators grant access to others with an IAM policy allowing` aws-marketplace:GetSellerDashboard` , for all dashboards or named ones.


**Does the billed revenue dashboard show cash received?** Not on its own. Since January 2026 it carries collection status, letting sellers distinguish invoiced, collected, and disbursed amounts. Disbursement detail sits in the collections and disbursement dashboard.


**How current is the agreements and renewals dashboard?** AWS states it provides information about agreements and renewals within 24 hours of an agreement being signed in AWS Marketplace.


**Can AWS seller dashboards show revenue across marketplaces?** No. Every AWS dashboard reports AWS Marketplace only. A cross-marketplace view has to be assembled outside the marketplace consoles.


---


## **Takeaways**


- AWS ships eight seller dashboards on the Insights tab, and by default only system administrators can open them.
- Grant dashboard access with IAM before finance emails alliances for a revenue number again.
- The usage dashboard shows records AWS received, not usage that occurred. Reconcile against your own telemetry.
- Billed revenue now carries collection status, so invoiced, collected, and disbursed are finally distinguishable.
- The agreements and renewals dashboard is the most underused and the most likely to protect revenue.
- No AWS dashboard can join to your CRM or report another marketplace. Both gaps have to be filled above the console.


---


Marketplace consoles each report their own marketplace. See how[reporting and analytics in Suger](https://www.suger.io/platform/reporting/) brings agreements, entitlements, offers, usage, and disbursements from every marketplace into one set of datasets — joined to the accounts in your CRM.
