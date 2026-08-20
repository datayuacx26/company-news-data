---
schema_version: "1.0.0"
document_id: "8863c9f24ebae975b01efe5fe3fa08f66e111d22c6c9c8df932e1d1ef6a9eb38"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/cloud-marketplace-metrics/"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-10T02:28:43.687936+00:00"
fetched_at: "2026-08-10T02:28:45.704186+00:00"
content_hash: "sha256:cd31670cf69f1329e674c5858e7dcebf546df40df0aaabff359e114f63437188"
---

# Cloud Marketplace Metrics Worth a Dashboard

*Cloud marketplace metrics are the measures that describe a marketplace revenue motion end to end — offer velocity, entitlement consumption, renewal exposure, and the gap between invoiced, collected, and disbursed money. A metric is worth a dashboard only if a named person changes a decision when it moves.*


---


The first marketplace dashboard an ISV builds is almost always the wrong one. It reports revenue by marketplace, by month, and by product — three cuts of a number the finance team already had, presented more attractively.


It gets built because revenue is the easiest thing to query and the hardest thing to argue with. It gets ignored because nobody makes a decision from it.


A useful marketplace reporting layer starts from the other end: what decisions does this team make every week, and what would have to move for the decision to change? Below are the metrics that survive that test, the dataset each one actually comes from, and the decision it drives.


---


## **What metrics should a marketplace dashboard track?**


Group them by the decision, not by the department. Five groups cover most of what an ISV needs.


**Pipeline and offer velocity** — is the motion converting? **Entitlement and consumption** — are customers using what they bought? **Renewal exposure** — what is at risk, and when? **Money in three states** — invoiced, collected, disbursed. **Listing and discovery** — is anybody finding you?


The mistake is to build all five at once. Build the one your weakest decision depends on, and add the next when someone asks for it.


---


## **The metrics, their source, and the decision**


Metric Where it comes from The decision it drives


Offers issued vs accepted, by age Offer records Whether offers are stalling in buyer procurement or in your own desk. A rising unaccepted age is a process problem, not a demand problem


Median time from offer issued to accepted Offer records How much runway a renewal offer needs. Sets the T-minus date on the renewal calendar


Entitlement consumption against purchased quantity Entitlement records Expansion targets and downgrade risk. Under-consumption predicts a shrink at renewal — the only moment a contract can get smaller


Agreements expiring in the next 90 days, by value Agreement records Renewal staffing. This is the single highest-value marketplace report and the one most often missing


Auto-renewal setting by agreement Agreement records Whether a renewal is an administrative event or a new sale


Invoiced vs collected vs disbursed Billing and revenue records Cash forecasting, and whether a shortfall is a billing problem or a collections problem


Metering records accepted vs rejected Usage records On a usage-priced product, this *is* the revenue integrity check. Rejected records are unbilled revenue


Co-sell referrals by status and age Co-sell referral records Whether partner-sourced pipeline is progressing or accumulating


Listing traffic and search terms Marketplace listing analytics Whether the listing is a discovery surface or just a transaction endpoint


Buyer profile coverage Buyer records Whether you can actually attribute marketplace revenue to accounts in your CRM


Two of those deserve expanding, because they are the ones teams most often get wrong.


---


## **Invoiced, collected, and disbursed are three different numbers**


Marketplace revenue exists in three states, and reporting one as though it were another is the most common marketplace finance error.


**Invoiced** is what the marketplace billed the buyer. **Collected** is what the buyer actually paid. **Disbursed** is what reached your bank account, net of fees, on the marketplace’s schedule.


The gaps between them are real and they are not errors. AWS made this explicit in January 2026 when it added collection status to seller reporting, describing the goal as letting sellers “distinguish between invoiced, collected, and disbursed amounts” — and the benefits as improving “payment forecasting accuracy” and detecting “collection issues earlier.”


If your dashboard shows one revenue line, it is showing one of these three and quietly implying the other two. A cash forecast built on invoiced revenue will be wrong by the collection lag; a performance report built on disbursed revenue will be wrong by the fee and the disbursement cycle. Report all three, and report the gap between them as its own number. The reconciliation work that sits underneath is covered in[marketplace billing and revenue operations](https://www.suger.io/resources/guides/marketplace-billing/) .


---


## **Metering integrity is a revenue metric, not an engineering metric**


On a usage-priced product, the marketplace bills from records your software sends. Records that fail to transmit are not billed, and there is no later reconciliation that recovers them.


That makes “metering records accepted vs rejected, by day” a revenue metric that belongs on the finance dashboard, not buried in an engineering log. Two things make it actionable:


- **Alert on the absence of records** , not only on errors. A metering job that silently stops produces no errors at all — it produces silence, which looks identical to a customer who used nothing.
- **Reconcile metered quantity against product telemetry** monthly. Two independent counts of the same usage is the only way to catch a systematic undercount.


The billing consequences of getting this wrong differ by pricing model — a contract product barely meters at all, while a subscription product meters every hour.[AWS Marketplace contract pricing vs usage pricing](https://www.suger.io/resources/blog/aws-marketplace-contract-vs-usage-pricing/) covers which model puts you at which risk.


---


## **Where the data actually comes from**


A metric without a named source dataset is an aspiration. In Suger, the analytics datasets that back the metrics above are published and queryable:


- ` billing_revenue_record` — “detailed records of revenue generated through various billing channels”
- ` marketplace_entitlement` — “customer entitlements for marketplace products or services… the usage rights and access levels associated with your offerings”
- ` marketplace_offer` — “data on marketplace offers, including public and private offers”
- ` marketplace_product` — “product details, pricing, and availability” for each marketplace product
- ` identity_buyer` — “buyer profile data, such as company name, industry, contacts and locations”
- ` cosell_referral` — “data on cosell referrals… co-selling activities and referral performance between partners”


The marketplaces publish their own reporting alongside this. AWS provides seller dashboards on the Insights tab of the AWS Marketplace Management Portal, covering billed revenue, collections and disbursement, taxation, agreements and renewals, usage, and listing and search performance — what each one shows, and what it doesn’t, is covered in[what AWS Marketplace seller insights actually show](https://www.suger.io/resources/blog/aws-marketplace-seller-insights/) .


The reason to build a layer above the marketplace consoles is not that the consoles are bad. It is that each one reports its own marketplace, in its own vocabulary, on its own schedule — and no ISV’s board asks for revenue by marketplace console.


---


## **How to build it without building all of it**


**Start with the expiry report.** Agreements expiring in the next 90 days, by value, with an owner. It is the cheapest report to build and the most expensive one to lack.


**Add the money-in-three-states view next.** Invoiced, collected, disbursed, and the gaps.


**Add offer velocity third** , once there is enough offer history for a median to mean anything.


**Resist the executive summary tile.** A single “marketplace revenue” number on a leadership dashboard is where the invoiced/collected/disbursed confusion is born. Label which one it is, every time.


**Give every chart an owner and a threshold.** A metric nobody is accountable for moving is a chart, not a metric.


---


## **Frequently asked questions**


**What metrics should an ISV track on cloud marketplaces?** Offer issued-to-accepted velocity, entitlement consumption against purchased quantity, agreements expiring in 90 days by value, invoiced vs collected vs disbursed revenue, metering acceptance rate, and co-sell referral progression.


**What is the difference between invoiced, collected, and disbursed revenue?** Invoiced is what the marketplace billed the buyer. Collected is what the buyer paid. Disbursed is what reached your bank account, net of fees, on the marketplace’s schedule. All three differ at any moment.


**Which marketplace metric is most valuable to build first?** Agreements expiring in the next 90 days, by value, with a named owner. It drives renewal staffing, and it is the report most often missing entirely.


**Why is metering acceptance a revenue metric?** Because usage-priced products are billed only from the records you transmit. Rejected or missing records are revenue that is never invoiced, and no later reconciliation recovers it.


**Do the cloud marketplaces provide their own dashboards?** Yes. AWS provides seller dashboards on the Insights tab of its Management Portal covering revenue, collections, tax, agreements and renewals, usage, and listing performance. Each marketplace reports only itself.


**How many metrics should a marketplace dashboard have?** As few as have owners. Every chart should have someone accountable for it and a threshold that triggers action; anything else is reporting, not measurement.


---


## **Takeaways**


- A metric earns a dashboard only when someone changes a decision as it moves.
- Invoiced, collected, and disbursed are three different numbers. Label which one you are showing.
- Agreements expiring in 90 days, by value, with an owner, is the highest-value report and the most commonly missing.
- Metering acceptance belongs on the finance dashboard. Missing records are unbilled revenue, permanently.
- Every metric needs a named source dataset. Without one it is an aspiration.
- Marketplace consoles report their own marketplace. A cross-marketplace view has to be built above them.


---


Reporting is only as good as the data underneath it. See how[reporting and analytics in Suger](https://www.suger.io/platform/reporting/) unifies offers, entitlements, agreements, usage, and disbursements across every marketplace into datasets you can chart or export.
