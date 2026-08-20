---
schema_version: "1.0.0"
document_id: "832ffb21535ccfd5d1091a9faecd3baa0139edeb6796648498fdd6c16777c693"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/vamp-2026-merchant-compliance"
published_at: "2026-04-02T13:00:00+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:9815ecca438ec8ef77f5ba649adffee07fcea6a29c2e517e1fd1e671e3a25536"
---

# Visa VAMP 2026: Your Compliance Math Changed on April 1

> **TL;DR**
>
>
> -
>
>
> Visa’s Visa Acquirer Monitoring Program (VAMP) merchant threshold dropped from 2.20% to 1.50% on April 1st, 2026. If you were sitting just under the old line, you may already be in violation without changing a thing.
>
>
> -
>
>
> The new formula merges fraud reports and disputes into a single ratio and one transaction can count against you twice. It’s measured by count, not dollars, so high-volume merchants carry more exposure.
>
>
> -
>
>
> Over-blocking fraud doesn’t help. Declining legitimate transactions shrinks the denominator of the ratio without reducing the numerator, which can push you further out of compliance.
>
>
> -
>
>
> The path forward is dual optimization: approve more real transactions to grow the denominator while cutting fraud and disputes to shrink the numerator. That’s a different discipline than chargeback management alone.


**On April 1st, 2026, the Visa Acquirer Monitoring Program (VAMP) merchant threshold for fraud-and-dispute ratio decreased from 2.20% to 1.50%.** That’s a 32% reduction, effective overnight. If your combined fraud-and-dispute ratio was sitting at 1.8% in March, you were compliant. So on April 2, with the exact same transaction volume and dispute count, you will be in violation and subject to penalties.


This isn’t an incremental policy tweak. The VAMP restructured how network-level risk enforcement works for card-not-present merchants. It unifies fraud reports and disputes into a single ratio, introduces a mechanism where one transaction can count twice against you, and creates cascading pressure from Visa to acquirers to merchants. For risk directors at high-volume eCommerce platforms, April 1st turned compliance from a backward-looking chargeback management task into a forward-looking optimization problem.


Here’s what the new math looks like, why it’s harder than the old math, and what you can do about it.


## The Threshold Shift: 420 Fewer Allowable Incidents at the Same Volume


The numbers tell the story clearly. Under the prior threshold, a merchant processing 60,000 transactions per month could absorb up to 1,320 combined fraud-and-dispute events before entering Visa's "Excessive" monitoring tier. Under the April 1 threshold of 1.50%, that ceiling drops to 900 events. That's 420 fewer allowable incidents for the exact same transaction volume (Source: Coinflow, 2026).


A merchant operating at 1,050 incidents per month (a 1.75% ratio, comfortably compliant under the old rules) now exceeds the threshold without any operational change (Source: Coinflow, 2026). The new limits apply to merchants in the US, Canada, EU, and APAC. The CEMEA region remains at 2.20% (Source: Basis Theory, 2026; Forter, 2025). Latin America and the Caribbean was already at 1.50% before April 2026 (Source: Basis Theory, 2026).


The enforcement timeline matters too. Visa began enforcing Above Standard fines against acquirers in January 2026, adding pressure to thresholds that have been in place since VAMP launched in June 2025. Merchant thresholds tightened further on April 1. First-time violators get a three-month grace period before fines begin (Source: Chargebacks911, 2025; Forter, 2025; Ravelin, 2025). (This applies to merchants not enrolled in VAMP monitoring within the prior 12 months.) That grace period is the window you're in right now if your ratio crossed the new line.


## What VAMP Actually Measures, and Why One Transaction Can Count Twice


The VAMP ratio formula is: (TC40 Fraud Reports + TC15 Disputes) / TC05 Settled Transactions (Source: Basis Theory, 2026; Chargebacks911, 2025; Forter, 2025). Three things about this formula deserve close attention.


**It unifies fraud and disputes into one number.** Prior programs tracked fraud monitoring and dispute monitoring separately. VAMP collapses them. A fraud alert (TC40) and a chargeback (TC15) from the same transaction both count against your ratio. One disputed transaction can generate two events, hitting your ratio twice. Forter's analysis notes this creates the potential for fines on a single disputed transaction from two separate penalty events (Source: Forter, 2025).


**It's count-based, not dollar-based.** Visa calculates VAMP ratios by transaction count, not transaction value. A marketplace with $200M in gross merchandise value across millions of low-price-point orders faces far more ratio exposure per dollar of revenue than a merchant with the same GMV concentrated in fewer high-value orders (Source: Basis Theory, 2026; Ravelin, 2025; MRC, 2025). The minimum monitoring threshold of 1,500 combined fraud-and-dispute events per month means high-volume operators are almost always in scope (Source: Basis Theory, 2026; Ravelin, 2025; Equifax, 2025).


**It only counts card-not-present transactions.** VAMP targets eCommerce specifically. Only CNP transactions enter the TC05 denominator (Source: Chargebacks911, 2025; MRC, 2025).


The double-counting mechanism is especially important for friendly fraud. Visa data indicates that approximately 75% of all disputes originate as friendly fraud (Source: Chargebacks911, 2025, citing Visa data). When a buyer receives an order, uses it, and files a fraud claim for an unauthorized transaction, both the TC40 fraud report and the TC15 dispute flow into the VAMP ratio. If you can't invoke Compelling Evidence 3.0 to exclude those counts (more on that below), both stand.


## The Cascade You May Not See Coming: Acquirer-Level Pressure


Your acquirer has its own VAMP thresholds, and they're tighter than yours. Visa set acquirer "Above Standard" at 0.50% and "Excessive" at 0.70% for portfolio-level VAMP ratios (Source: Equifax, 2025; Basis Theory, 2026). Acquirers exceeding those limits face fines per affected transaction across their entire portfolio (Source: Chargebacks911, 2025).


This creates strong financial incentive for acquirers to proactively restrict or offboard merchants whose individual ratios threaten portfolio compliance (Source: Equifax, 2025; Forter, 2025; Chargebacks911, 2025). You may not hear about this pressure directly. The first signal could be tighter processing restrictions, account reserves, or a settlement pause.


The terminal consequence for persistent non-compliance is MATCH list placement, which effectively ends your ability to process Visa payments. Settlement pauses often occur before merchant communication, creating fund freezes lasting three to 14+ days before any formal notice arrives (Source: Coinflow, 2026).


If your acquirer's own ratio is under pressure, your individual compliance buffer shrinks further. The margin for error narrows from both directions.


## Compliance Exclusions Exist, but They Require Infrastructure You May Not Have


VAMP does offer ratio relief through specific dispute resolution channels. Transactions resolved through Compelling Evidence 3.0 (CE3.0), Verifi CDRN, or Visa Rapid Dispute Resolution (RDR) can be excluded from the VAMP ratio calculation (Source: Forter, 2025; Chargebacks911, 2025; Basis Theory, 2026).


CE3.0 exclusions are the most valuable, and the hardest to qualify for. They require three things:


-


**Device ID and IP address captured on all prior transactions.** If you weren't collecting device fingerprints before the dispute, you can't retroactively produce matching data.


-


**120 days of transaction history.** You need at least four months of matching device and IP records for the disputed cardholder (Source: Forter, 2025).


-


**Resolution within the same calendar month as the dispute.** Both the dispute and its CE3.0 resolution must land in the same month to qualify for exclusion.


This creates an asymmetric advantage. Merchants who already have device fingerprinting infrastructure will systematically earn ratio exclusions that unprepared merchants simply cannot access (Source: Forter, 2025; Chargebacks911, 2025). The data capture has to be in place before the dispute happens. There's no way to build this retroactively once you're already in the monitoring window.


VAMP also introduced a separate Enumeration Ratio, tracking confirmed card-testing attempts as a share of all authorization attempts, including declined transactions. Merchants exceeding a 20% enumeration threshold face enrollment in a separate monitoring track (Source: Ravelin, 2025; Chargebacks911, 2025; Forter, 2025). Monitoring entry also requires a minimum of 300,000 enumerated authorization transactions per month (Source: Ravelin, 2025). For marketplace platforms where seller-side fraud or API-exposed checkout flows may invite card testing, this is a second compliance surface to monitor.


## The False Decline Trap Hiding Inside VAMP Compliance


Here's where VAMP creates a genuinely new problem. Under prior monitoring programs, over-aggressive fraud blocking could drive down chargebacks without direct regulatory consequence. You'd lose revenue from false declines, but you'd stay compliant. Under VAMP, that trade-off breaks.


The false decline problem under VAMP is a denominator problem. Every legitimate transaction you decline removes one settled transaction from TC05 without removing a single fraud or dispute event from your numerator. Meanwhile, the fraudulent transactions you do approve still generate TC40 and TC15 events. Over-aggressive blocking shrinks the denominator without proportionally reducing the numerator, which makes the ratio worse, not better. The only way to improve both sides simultaneously is to approve more legitimate transactions (growing TC05) while preventing fraud and disputes at their source (shrinking TC40 + TC15).


The false decline cost across the industry is already substantial: approximately $50 billion annually in lost revenue. Global false decline losses are estimated at $443 billion annually, roughly nine times the $48 billion lost to actual fraud (Source: Riskified, 2025).


VAMP turns this from a revenue problem into a compliance problem. The optimization it imposes is genuinely dual-sided: minimize (TC40 + TC15) / Settled Transactions while simultaneously maximizing approved volume (Source: Corgi Labs internal memo, March 2026). Tightening your fraud rules without precision doesn't help the ratio. It can make it worse.


## What Risk Directors Should Do Now


VAMP compliance is no longer about managing chargebacks in isolation. It's about optimizing a ratio where both the numerator and denominator matter, and where over-correction in either direction carries consequences.


Five concrete steps to take this month:


1.


**Calculate your current VAMP ratio.** Pull your TC40, TC15, and TC05 counts from the last 90 days. Know exactly where you stand against the 1.50% threshold.


2.


**Audit your CE3.0 readiness.** Confirm that your system captures device ID and IP address on every transaction. Verify you have 120 days of history. If you don't, start collecting now.


3.


**Quantify your friendly fraud exposure.** With approximately 75% of disputes originating as friendly fraud (Source: Chargebacks911, 2025), this is likely the largest single contributor to your VAMP ratio.


4.


**Measure your false decline rate alongside your fraud rate.** If your fraud rules are blocking legitimate transactions, you're shrinking your denominator without reducing your numerator.


5.


**Talk to your acquirer.** Understand where their portfolio ratio stands and whether they're tightening restrictions on merchants in your category.


The underlying challenge is a dual optimization: approve more real buyers to grow your denominator while reducing fraud and disputes to shrink your numerator. These two goals used to live in separate operational silos. VAMP forces them into a single formula.


Corgi Labs builds tools for exactly this kind of problem. Corgi Intelligence surfaces your fraud, dispute, and decline data in one view so you can see your actual VAMP ratio exposure across processors. Corgi Model uses custom machine learning trained on your transaction data to dig into both sides of the equation, approving more legitimate orders while reducing chargebacks. For one eCommerce merchant, that meant +22% payments accepted, an 18% reduction in realized fraud rate, and more than $2 million in recovered revenue (Source: Corgi Labs product documentation, 2026).


If you want to see where your VAMP ratio stands and where the optimization opportunities are,book a demo .


---


## Source Index


-


[Basis Theory, "VAMP 2026: What Changes on April 1"](https://blog.basistheory.com/visa-acquirer-monitoring-program-2026-updates)


-


[Chargebacks911, "Visa Acquirer Monitoring Program: Major Visa Updates in 2026"](https://chargebacks911.com/visa-acquirer-monitoring-program/)


-


[Forter, "Visa Updates VAMP Program: Key Changes and What They Mean for Merchants"](https://www.forter.com/blog/may-2025-visa-updates-vamp-program/)


-


[Ravelin, "New VAMP for 2025: Visa's Changes to Dispute Thresholds"](https://www.ravelin.com/blog/visa-vamp-changes-chargeback-disputes)


-


[MRC, "A Merchant's Guide to the New Visa VAMP Program"](https://merchantriskcouncil.org/learning/resource-center/member-news/blog/2025/flexpay-july-2-a-merchants-guide-to-the-new-visa-vamp-program)


-


[Equifax, "The Visa Acquirer Monitoring Program (VAMP): What New Rules Mean"](https://www.equifax.com/business/blog/-/insight/article/the-visa-acquirer-monitoring-program-vamp-what-new-rules-mean-for-acquirers-and-merchants/)


-


[Coinflow, "VAMP Is Getting Stricter in April 2026"](https://coinflow.cash/blog/vamp-changes-april-2026/)
