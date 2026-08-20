---
schema_version: "1.0.0"
document_id: "cd8a3e5afe15c05d4e4bb36b6b90272a74aa4825a7ad82a97ad9d3af3edebf27"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/vamp-self-audit-checklist"
published_at: "2026-07-27T18:11:52.345+00:00"
first_seen_at: "2026-07-27T19:13:07.875793+00:00"
fetched_at: "2026-07-28T20:32:04.512542+00:00"
content_hash: "sha256:0c5229e2d86838dc2ce05b6ebe54b1e40175cf659a3bd730765d38cd3f41c679"
---

# How to Calculate Your VAMP Ratio: A Monthly Self-Audit Checklist

## You Probably Know Your Chargeback Rate. But Do You Know Your VAMP Ratio?


A June 2026 Chargebacks911 survey of enterprise merchants found that only 26.8% actively monitor TC40 fraud records. That means roughly three out of four payment operations teams are calculating their fraud exposure with incomplete data.


And they're doing it under a tighter threshold than ever before.


On April 1, 2026, Visa reduced the merchant "Excessive" threshold in its Acquirer Monitoring Program (VAMP) from 2.2% to 1.5%, a 32% reduction in the margin for error. There is no warning tier. There is no "Above Standard" level for merchants. You are either below 1.5% or you are in the Excessive category, facing $8 fines per violation.


Oscar Bello, Chief Sales Officer at Chargeback Gurus, put it plainly: "That adjustment narrows the margin for error and increases the likelihood that more businesses will fall into enforcement categories if their current practices remain unchanged."


This article walks you through the exact VAMP formula, shows you where to pull the numbers from your payment processor, and gives you a monthly checklist to catch problems before Visa (or your acquirer) does.


## The VAMP Formula: Simple Math, Hidden Complexity


The VAMP ratio looks straightforward on paper:


**VAMP Ratio = (TC40 Fraud Reports + TC15 Disputes) ÷ TC05 Settled CNP Transactions**


Three numbers. One division. But each component carries nuance that changes your result.


**TC40 (Fraud Reports):** These are early fraud warnings. When a cardholder tells their issuing bank a transaction is fraudulent, the issuer files a TC40 with Visa. No chargeback has been initiated yet. The issuer is simply flagging the transaction. You may never hear about it unless you actively check.


**TC15 (Disputes/Chargebacks):** These are formal chargebacks. The cardholder has disputed the charge, and the issuer has reversed the funds. Most payment teams track these closely because they hit your balance directly.


**TC05 (Settled CNP Transactions):** This is your denominator. It includes all card-not-present transactions that settled through VisaNet in the measurement month, both domestic and cross-border.


Two details matter here. First, this is a **count-based** calculation, not dollar-based. A $9 dispute counts exactly the same as a $9,000 dispute. Second, there is a volume floor: Visa only flags merchants whose combined TC40 plus TC15 count exceeds 1,500 per month (except in the CEMEA region, where the threshold is 150 disputes plus $75,000 in volume).


## A Worked Example (With the Trap Built In)


Let's run the numbers with a realistic scenario.


Suppose your company settled 10,000 Visa CNP transactions last month. Your dashboard shows 70 chargebacks, and your Radar or fraud alerts show 120 early fraud warnings. At first glance, you might calculate:


-


Chargeback rate: 70 ÷ 10,000 = **0.70%**


-


Fraud report rate: 120 ÷ 10,000 = **1.20%**


Both numbers look manageable. But VAMP combines them:


**(120 + 70) ÷ 10,000 = 1.90%**


That puts you squarely in the Excessive tier, above the 1.5% threshold.


What most merchants miss, the double-counting trap.


## The Double-Counting Trap


A single fraudulent transaction can generate **both** a TC40 and a TC15. The issuer files the fraud report (TC40) when the cardholder first complains. Then, if the dispute proceeds, the same transaction produces a chargeback (TC15). Both land in the VAMP numerator.


As Chargeback Gurus explains: "VAMP ratio effectively double-counts fraud-related disputes, and counts TC40 reports for transactions that were too small for the issuing bank to bother with a chargeback. This means most merchants see much higher ratios under the VAMP formula."


Metzli Sampat of ChargebackStop illustrates this clearly: "Imagine you have 10,000 settled transactions, 120 TC40 fraud reports and 70 chargebacks. Visa would calculate this as (120 + 70) ÷ 10,000 = 1.9%. But what if 50 of those 70 chargebacks actually originated from the same fraud reports?"


In that scenario, only 140 unique fraud events produced 190 VAMP-numerator entries. Your "true" unique fraud rate would be 1.4%, below threshold, but VAMP reports 1.9%.


**Friendly fraud amplifies this problem.** Visa data indicates that approximately 75% of all disputes originate as friendly fraud, where a legitimate cardholder falsely claims fraud to obtain a refund. When this happens, the issuer files a TC40, which typically becomes a TC15 as well. Both count against your VAMP ratio for the same underlying event.


## Where to Find Your Data (By Processor)


You cannot calculate your VAMP ratio from a single dashboard screen. Here's where each data point lives in the major processors.


### Stripe


Stripe provides three separate locations for the data you need:


1.


**TC15 (Chargebacks):** Navigate to **Payments → Disputes** in your Stripe Dashboard. Filter for Visa disputes in the measurement month.


2.


**TC40 (Early Fraud Warnings):** Navigate to **Radar → Early Fraud Warnings** . These are separate from disputes and easy to miss if you only check the Disputes tab.


3.


**VAMP Dashboard:** Stripe offers a dedicated VAMP view accessible from **Radar → Card Brand Monitoring Programs → VAMP** in your Stripe Dashboard (navigation path may vary by account configuration).


For Stripe Sigma or Data Pipeline users, you can query the` early_fraud_warnings` and` disputes` tables and join them against settled charges for a precise monthly count (verify exact table names in your Stripe Sigma schema, as names may vary by configuration). Most mid-market merchants without Sigma access should use the VAMP dashboard for estimates and manually cross-reference the two report locations.


### Braintree


-


**TC40 (Fraud Reports):** Available via SFTP data feeds. Check with your Braintree account team if you haven't enabled this.


-


**TC15 (Chargebacks):** Use the Dispute reports filtered for open Visa chargebacks. Note that Braintree's documentation states the full TC15 log "isn't available at this time" via SFTP, so the Dispute reports are your best approximation.


-


**TC05 (Settled transactions):** Pull from your transaction reports filtered for settled Visa CNP transactions.


### Adyen


Navigate to **Insights → Risk & dispute management → Card monitoring programs** . Adyen surfaces your VAMP ratio and projections directly in this view, consolidating the TC40, TC15, and TC05 data into a single dashboard.


### Other Processors (Worldpay, Checkout.com, etc.)


If your processor doesn't offer a built-in VAMP dashboard, request your monthly TC40, TC15, and TC05 reports directly from your acquirer. You are entitled to this data, and most acquirers can provide it within a standard reporting cycle.


## The Acquirer Squeeze: Why 0.5% Matters More Than 1.5%


Your acquirer has its own VAMP problem, and their thresholds are dramatically tighter than yours.


Visa enforces two acquirer-level thresholds:


-


**0.50% (Above Standard):** $4 per violation fines, enforced since January 1, 2026


-


**0.70% (Excessive):** $8 per violation fines


These are portfolio-level calculations. Your acquirer's VAMP ratio aggregates every merchant on their platform. If a handful of high-ratio merchants push the portfolio above 0.50%, the acquirer pays fines on every excess violation across their entire book.


**This creates a math problem your acquirer will solve at your expense.**


Industry observers report that payment providers are already terminating merchants with historically acceptable rates of 0.8% to 1.0%, well below Visa's 1.5% merchant threshold. An acquirer whose portfolio sits at 0.48% cannot afford to keep merchants running at 1.2%, even though those merchants are technically compliant with their own threshold.


First-time merchant violators do get a three-month grace period within a rolling 12-month window before fines begin. But your acquirer's grace period for their own portfolio ratio may not extend to you. By the time Visa notifies you directly, your acquirer may have already decided to restrict your account.


The takeaway: don't benchmark yourself against 1.5%. Ask your acquirer what internal threshold they expect from merchants on their portfolio.


## What Gets Excluded: CE3.0 and RDR


Not every TC40 or TC15 has to count against your ratio. Visa provides three exclusion mechanisms that can reduce your VAMP numerator.


### Rapid Dispute Resolution (RDR)


Disputes resolved through RDR before Visa's monthly data cutoff are excluded from your TC15 count. RDR automates the resolution of qualifying disputes by issuing a refund before the chargeback is formally processed.


**Important caveat:** RDR removes the TC15 (dispute) from your VAMP numerator, but any TC40 fraud report already filed by the issuer for that transaction still counts. RDR is most effective when the issuer has not yet filed a TC40 at the time of dispute resolution, which typically requires very fast response to pre-dispute alerts.


### Verifi CDRN (Chargeback Dispute Resolution Network)


Similar to RDR, alerts resolved through the CDRN network before the monthly extract can be excluded.


### Compelling Evidence 3.0 (CE3.0)


CE3.0 allows you to submit historical transaction data proving a disputed transaction is legitimate. If your evidence qualifies, the TC40 or chargeback is excluded from your VAMP ratio.


Effective April 18, 2026, Visa expanded CE3.0 to cover **non-disputed TC40s** . Previously, if an issuer filed a TC40 on a $15 transaction but never initiated a formal chargeback (because the transaction value was too low to justify processing costs), that TC40 still counted against your VAMP ratio with no way to contest it. Now, you can submit historical transaction evidence to prove the transaction's legitimacy and exclude the TC40.


**Note on CE3.0 costs:** Visa has signaled that a fee may be introduced for successful CE3.0 qualifications. Confirm the current fee structure with your acquirer or chargeback management provider before building a high-volume CE3.0 submission workflow.


### The Timing Caveat


All three exclusion mechanisms come with a critical timing clause. Visa's fact sheet specifies that exclusions are "contingent on the timing of the data extract." If a TC40 is resolved via CE3.0 or a dispute is handled via RDR **after** the monthly data cutoff, it may still appear in your VAMP numerator for that month.


Speed matters. Set up automated RDR rules and prepare CE3.0 evidence packages proactively so resolutions land before the monthly extract.


## Action Plan: What to Do If You're Approaching 1.5%


If your self-audit reveals a VAMP ratio above 1.0%, or trending upward month over month, take these steps now.


### 1. Get Your Baseline Right


Pull three months of TC40, TC15, and TC05 data from your processor. Calculate your ratio for each month. Look for trends, not just snapshots. A single bad month may be an anomaly. Three consecutive months of increases signals a structural problem.


### 2. Quantify the Double-Count


Cross-reference your TC40 and TC15 records to identify transactions that appear in both. This tells you the gap between your "reported" VAMP ratio and your "unique event" ratio. You cannot change how Visa counts, but understanding the overlap helps you prioritize which fraud vectors to address first.


### 3. Audit Your Billing Descriptors


Unclear billing descriptors are one of the most common drivers of friendly fraud. When customers don't recognize a charge, they call their bank instead of you. Review your descriptor for clarity:


-


Does it include your company name (as customers know it)?


-


Does it include a phone number or URL for support?


-


Is it consistent across all product lines?


### 4. Tighten Your Refund and Cancellation Flows


Make it easier for customers to get a refund from you than from their bank. Every dispute that becomes a self-service refund is one fewer TC15 in your numerator. For subscription businesses, this means:


-


Clear cancellation flows (no dark patterns)


-


Proactive dunning emails before retry failures


-


Transparent billing schedules


### 5. Enable RDR and CDRN


If you haven't set up automated dispute resolution through RDR or CDRN, do it now. These tools resolve qualifying disputes before they hit your VAMP numerator. Remember: **RDR removes the TC15, but any associated TC40 already filed still counts.** Pair RDR with fast pre-dispute alert response for maximum impact.


### 6. Build Your CE3.0 Evidence Library


Start collecting the historical transaction data CE3.0 requires:


-


Device fingerprints and IP addresses for prior successful orders


-


Shipping confirmation and delivery tracking from previous transactions


-


Account login history showing consistent customer activity


Having this data ready means faster submissions, which means exclusions land before the monthly data cutoff.


### 7. Monitor Weekly, Not Monthly


A monthly check gives you one data point. By the time you spot a spike, you've already reported a bad month to Visa. Move to weekly VAMP ratio estimates (even rough ones) so you can catch anomalies while you still have time to act.


## Your Monthly VAMP Self-Audit Checklist


Use this checklist at the start of each month to track your exposure.


**Data Collection:**


-


Pull TC40 count from your fraud alerts or early warning reports


-


Pull TC15 count from your disputes/chargebacks reports (Visa only)


-


Pull TC05 count of settled Visa CNP transactions


-


Calculate: (TC40 + TC15) ÷ TC05 = your VAMP ratio


**Threshold Check:**


-


Is your ratio below 1.5%? (Visa merchant threshold)


-


Is your ratio below your acquirer's internal threshold? (Ask if you don't know it)


-


Is your combined TC40 + TC15 count above 1,500? (Below this, Visa doesn't flag you, but your acquirer still might. CEMEA merchants: threshold is 150 combined + $75,000 volume.)


**Trend Analysis:**


-


Compare this month's ratio to the prior two months


-


Flag any month-over-month increase greater than 0.1 percentage points


-


Identify the top five products or transaction types generating TC40s


**Exclusion Verification:**


-


Confirm RDR is active and resolving disputes before the monthly data cutoff (note: RDR removes qualifying TC15s from VAMP, but associated TC40s already filed still count)


-


Check CE3.0 submission queue for any pending or late submissions


-


Verify CDRN alerts are configured and connected


**Escalation Triggers:**


-


If ratio exceeds 1.2%: escalate to payments team lead for review


-


If ratio exceeds 1.4%: implement emergency fraud rule tightening and contact acquirer


-


If ratio exceeds 1.5%: assume you are in Visa's Excessive tier and activate remediation plan


## What Comes Next


Tracking your VAMP ratio manually is a solid first step. It puts you ahead of the nearly three out of four merchants who aren't monitoring TC40s at all.


But manual tracking has limits. It's retrospective, it's slow, and it can't catch mid-month spikes before they lock in. If your business processes meaningful Visa volume, you need real-time visibility into both fraud signals and chargeback patterns, correlated against your settled transactions, updating continuously.


Corgi Intelligence surfaces exactly this kind of payments data. It pulls your fraud alerts, chargebacks, and transaction volume into a single view, highlights the double-count overlap, and flags ratio movement before the monthly cutoff, with no development work required.


If calculating your own VAMP ratio made you want better tooling,[Book a Demo →](https://www.corgilabs.ai/#book-demo) .


## Sources


1.


Visa Official VAMP Fact Sheet (2025):[https://corporate.visa.com/content/dam/VCOM/corporate/visa-perspectives/security-and-trust/documents/visa-acquirer-monitoring-program-fact-sheet-2025.pdf](https://corporate.visa.com/content/dam/VCOM/corporate/visa-perspectives/security-and-trust/documents/visa-acquirer-monitoring-program-fact-sheet-2025.pdf)


2.


Merchant Risk Council, Stricter VAMP Ratio Thresholds Are Now in Effect (April 2026):[https://merchantriskcouncil.org/learning/resource-center/member-news/blog/2026/stricter-vamp-ratio-thresholds-are-now-in-effect-heres-how-to-stay-compliant](https://merchantriskcouncil.org/learning/resource-center/member-news/blog/2026/stricter-vamp-ratio-thresholds-are-now-in-effect-heres-how-to-stay-compliant)


3.


Chargeback Gurus, Visa Acquirer Monitoring Program (VAMP) Guide (2026):[https://www.chargebackgurus.com/visa-acquirer-monitoring-program-vamp](https://www.chargebackgurus.com/visa-acquirer-monitoring-program-vamp)


4.


Chargebacks911, Visa Acquirer Monitoring Program 2026:[https://chargebacks911.com/visa-acquirer-monitoring-program/](https://chargebacks911.com/visa-acquirer-monitoring-program/)


5.


Chargebacks911/BusinessWire, Friendly Fraud Enterprise Survey (June 2026):[https://www.businesswire.com/news/home/20260630978374/en/Friendly-Fraud-is-Rising-for-More-Than-83-of-Enterprise-Merchants-New-Chargebacks911-Study-Finds](https://www.businesswire.com/news/home/20260630978374/en/Friendly-Fraud-is-Rising-for-More-Than-83-of-Enterprise-Merchants-New-Chargebacks911-Study-Finds)


6.


ChargebackStop, How Double Counting (TC40 + TC15) in VAMP Inflates Your Risk:[https://www.chargebackstop.com/blog/how-double-counting-tc40-tc15-in-visas-vamp-inflates-your-risk](https://www.chargebackstop.com/blog/how-double-counting-tc40-tc15-in-visas-vamp-inflates-your-risk)


7.


Corgi Labs, Visa VAMP 2026 New Merchant Compliance Thresholds:[https://www.corgilabs.ai/insights/vamp-2026-merchant-compliance](https://www.corgilabs.ai/insights/vamp-2026-merchant-compliance)


8.


Stripe Documentation, Dispute and Fraud Card Monitoring Programs:[https://docs.stripe.com/disputes/monitoring-programs](https://docs.stripe.com/disputes/monitoring-programs)


9.


Braintree Developer Docs, Visa Acquirer Monitoring Program:[https://developer.paypal.com/braintree/articles/risk-and-security/card-brand-monitoring-programs/visa-programs/visa-dispute-monitoring-program](https://developer.paypal.com/braintree/articles/risk-and-security/card-brand-monitoring-programs/visa-programs/visa-dispute-monitoring-program)


10.


Adyen Docs, Monitor Risk Performance / Card Monitoring Programs:[https://docs.adyen.com/risk-management/dispute-and-fraud-monitoring](https://docs.adyen.com/risk-management/dispute-and-fraud-monitoring)


11.


Equifax/Kount, The Visa Acquirer Monitoring Program: What New Rules Mean:[https://kount.com/blog/insight-visa-acquirer-monitoring-program-vamp](https://kount.com/blog/insight-visa-acquirer-monitoring-program-vamp)


12.


Ballerine, Faster Terminations Due to VAMP:[https://ballerine.com/glossary/visa-acquirer-monitoring-program-vamp](https://ballerine.com/glossary/visa-acquirer-monitoring-program-vamp)


13.


Visa Corporate Blog, Evolving the Visa Acquirer Monitoring Program:[https://corporate.visa.com/en/sites/visa-perspectives/security-trust/visa-vamp-program-update-fraud-disputes.html](https://corporate.visa.com/en/sites/visa-perspectives/security-trust/visa-vamp-program-update-fraud-disputes.html)


14.


Chargebacks911, Compelling Evidence 3.0 Update (April 2026):[https://chargebacks911.com/compelling-evidence-3-0-update-april-2026/](https://chargebacks911.com/compelling-evidence-3-0-update-april-2026/)


15.


Adaptiv Payments, Visa VAMP Guide 2025+2026:[https://adaptivpayments.com/blog/visa-vamp-2025](https://adaptivpayments.com/blog/visa-vamp-2025)
