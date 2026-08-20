---
schema_version: "1.0.0"
document_id: "71b1b020166ad69d4733d2d954c9e7652ef568b11c34061c6ec90254d6caad2f"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/vamp-2026-two-months-later"
published_at: "2026-05-30T17:09:00+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:87a04ee1d5e35691c34c9dc9943d8cc64124e032c5ea93078caf1f2efd5e8800"
---

# VAMP 2026: Two Months Later, the Playbook Is Taking Shape

## A Quiet Expansion Changed the VAMP Equation


On April 18, 2026, Visa expanded Compelling Evidence 3.0 to cover fraud reports that never become chargebacks. Most merchants haven't heard about it yet. But for anyone managing a VAMP ratio, this single change opens a defense mechanism that didn't exist five weeks ago.


Two months after the new 1.5% "Merchant Excessive" threshold took effect, the VAMP conversation has shifted. The panic phase is over. What's replaced it is something more interesting: an emerging operational discipline around fraud management, dispute prevention, and the uncomfortable tension between blocking fraud and blocking revenue.


## Quick Refresher: What Changed in April


If you've been tracking VAMP since its October 2025 enforcement launch, you know the basics. But the April 2026 changes are worth restating because they tightened the screws considerably.


**The threshold dropped.** The "Merchant Excessive" ratio fell to 1.5% for most global regions, with a minimum monthly count of 1,500 combined fraud reports and disputes. CEMEA regions kept a higher 2.2% threshold.


**The ratio formula is what makes VAMP different.** Your VAMP ratio combines TC40 fraud reports and TC15 disputes into a single number: (TC40s + TC15s) / Total Settled Transactions. This matters because TC40 fraud reports can hit your ratio even when they never escalate to a chargeback. A customer's bank flags a transaction as suspicious, Visa records a TC40, and your ratio takes the hit regardless of whether a dispute follows.


**The enforcement timeline is now fully live:**


1.


October 1, 2025: "Excessive" level enforcement began


2.


January 1, 2026: "Above Standard" level enforcement began


3.


April 1, 2026: Merchant threshold dropped to 1.5%


First-time violators get a three-month grace period within a rolling 12-month window. After that, fines start at $8 per event.


## The Hidden Threat: Fraud Reports Without Chargebacks


Here's the scenario that's catching merchants off guard. Imagine you receive 100 TC40 fraud reports in a month but only 40 of those become actual chargebacks. Under older monitoring programs, the 60 non-disputed reports were background noise. Under VAMP, all 100 count against your ratio.


This creates a painful dynamic with friendly fraud, which accounts for roughly 75% of all disputes according to Visa's own data. A single friendly fraud incident can generate both a TC40 and a TC15. Both count. Your ratio absorbs the impact twice (with de-duplication only when both filings reference the exact same transaction).


**Before the CE3.0 expansion, merchants had no mechanism to challenge those 60 non-disputed TC40s.** They simply counted. You could refund them, but they still hit your ratio. That's the gap Visa addressed on April 18.


## CE3.0 Expansion: A New Line of Defense


Compelling Evidence 3.0 now lets you apply historical transaction data to TC40 fraud reports that lack a corresponding chargeback. If you can demonstrate at least two prior undisputed transactions from the same cardholder, with at least one matching data element being device ID or IP address, you can get those TC40s excluded from your VAMP calculation.


The evidence Visa accepts includes:


-


Device ID matching a previous undisputed transaction


-


IP address from a prior legitimate purchase


-


Shipping address tied to past successful orders


-


User account ID with transaction history


As Chargebacks911's Zak Matthews put it in April: merchants "finally have a mechanism to push back against TC40 fraud reports that never escalated to chargebacks." For the first time, you can protect your ratio from damage you previously couldn't prevent.


### The Limitations Are Real


CE3.0 isn't a blanket fix. The evidence requirements create meaningful gaps.


**First-time buyers can't be protected.** CE3.0 requires matching against historical transactions between 120 and 365 days old. If someone makes their first purchase and the bank files a TC40, you have no prior data to submit.


**Digital goods and early-cycle subscriptions are underserved.** If your disputes cluster in the first few months of a customer relationship, you won't have the 120-day evidence history CE3.0 demands.


**There's a cost.** Visa is introducing a fee for successful CE3.0 qualifications. Exact pricing isn't public yet, but it means even winning a CE3.0 challenge isn't free.


## Pre-Dispute Tools Are Now Table Stakes


CE3.0 is one layer. But the broader shift under VAMP is that pre-dispute tools have moved from "nice to have" to compliance infrastructure. Disputes resolved through Verifi CDRN, Ethoca, Rapid Dispute Resolution (RDR), and Order Insight are excluded from VAMP ratio calculations.


That exclusion changes the math entirely. **If a dispute never enters the VAMP formula, it's as if it never happened** from a compliance standpoint. This makes enrollment in pre-dispute programs a prerequisite for operating safely under VAMP, not an optimization.


The practical three-layer defense now looks like this:


1.


**Pre-dispute deflection:** CDRN, Ethoca, and RDR resolve disputes before they become chargebacks


2.


**Post-dispute representment:** CE3.0 challenges disputed transactions with historical evidence


3.


**Non-disputed fraud remediation:** The new CE3.0 expansion addresses TC40s that never became disputes


Each layer excludes resolved cases from your VAMP ratio. Together, they give you multiple chances to keep your numbers clean.


## How Acquirers Are Responding


VAMP doesn't just monitor merchants. Acquirers face their own thresholds: 0.5% to 0.7% for "Above Standard" status and 0.7%+ for "Excessive." Fines start at $4 per event at the lower tier and $8 per event at the upper.


This pressure flows downhill. As Chargebacks911 put it: "It's not hyperbole to say that the Visa Acquirer Monitoring Program will reshape — and in some cases, sour — the relationship between merchants and acquirers." What that looks like in practice:


-


**Tighter underwriting.** Acquirers are screening new merchants more carefully before onboarding, paying closer attention to industry vertical and historical chargeback patterns.


-


**Higher fees for high-risk accounts.** Merchants approaching VAMP thresholds can expect surcharges or penalty pricing.


-


**Faster terminations.** Acquirers are quicker to cut merchants who threaten their portfolio-level ratios. A single high-dispute merchant can drag an entire acquiring portfolio toward the threshold.


High-risk verticals feel this most acutely. European iGaming operators, for example, report card chargeback rates around 0.83%. That's well under the 1.5% merchant threshold, but it sits right in the danger zone for acquirer-level monitoring. Some operators are shifting volume to alternative payment methods where card-network chargeback rules don't apply.


## The False-Decline Trap


Here's where VAMP creates an uncomfortable paradox. The instinct when your VAMP ratio creeps up is to tighten your fraud rules. Block more. Decline more. Set lower thresholds on your fraud scoring.


**That instinct will cost you revenue.** Corgi Labs' analysis across merchant portfolios shows that for every $100 in false payment declines, merchants lose over $750 in lifetime value. Customers who get declined don't come back. They don't call support. They just leave and buy from someone else.


Worse, aggressive blocking can feed the cycle of friendly fraud. A customer whose legitimate order gets declined may try again with different details, get flagged again, and eventually dispute the charge out of frustration. That pattern is a recognized risk that compounds both revenue loss and dispute exposure.


The merchants adapting best to VAMP aren't blocking more. They're blocking smarter. They're using merchant-specific fraud models that can distinguish genuine buyers from actual fraud with enough precision to keep approval rates high while keeping chargebacks low. Blunt rules like "decline all orders over $500 from new customers" don't survive in a VAMP world.


## What to Do Now: A Practical Checklist


If you're managing VAMP compliance today, here's what the first two months have taught us about what works.


**Get visibility into your TC40s.** Most merchants track chargebacks. Fewer track the TC40 fraud reports that now feed their VAMP ratio. If you can't see your TC40 volume broken down by reason code and customer segment, you're flying blind.


**Enroll in pre-dispute programs.** If you aren't using CDRN, Ethoca, RDR, or Order Insight, every dispute that could have been deflected is counting against your ratio unnecessarily. The enrollment cost is trivial compared to VAMP fines.


**Audit your fraud rules for false declines.** Run a report on declined transactions from the past 90 days. How many were legitimate buyers? Every false decline is lost revenue today and a potential friendly-fraud dispute tomorrow.


**Monitor your acquirer relationship.** Ask your acquirer where you stand relative to their portfolio-level VAMP thresholds. If they're feeling pressure, you need to know before they make decisions about your account.


## From Compliance Burden to Competitive Advantage


Two months into the new VAMP thresholds, a pattern is emerging. Merchants who treat VAMP as a one-time compliance exercise are struggling. Merchants who've built it into their operational rhythm are finding something unexpected: the same discipline that keeps your VAMP ratio clean also makes your payments operation more profitable.


Precision fraud decisioning, trained on your own transaction data, does both jobs at once. It keeps chargebacks and fraud reports low enough to stay clear of VAMP thresholds. And it approves more real buyers that blunt fraud rules would have turned away. Corgi Model does exactly this: custom machine learning that blocks fraud, not buyers.


Your payments data holds the answer to both problems.Book a demo and see what precision looks like.


## Sources


1.


Chargebacks911, "Visa Acquirer Monitoring Program: Major Visa Updates in 2026."[https://chargebacks911.com/visa-acquirer-monitoring-program/](https://chargebacks911.com/visa-acquirer-monitoring-program/)


2.


Chargebacks911, "Compelling Evidence 3.0 Update, April 2026, Explained" (Zak Matthews, April 15, 2026).[https://chargebacks911.com/compelling-evidence-3-0-update-april-2026/](https://chargebacks911.com/compelling-evidence-3-0-update-april-2026/)


3.


Chargebacks911, "VAMP Enforcement October 1, 2025: Merchant Impact Analysis."[https://chargebacks911.com/vamp-enforcement/](https://chargebacks911.com/vamp-enforcement/)


4.


Finera, "iGaming Chargeback Protection: Strategies for Licensed Operators."[https://www.finera.com/blog/igaming-chargeback-protection-strategies-for-licensed-operators](https://www.finera.com/blog/igaming-chargeback-protection-strategies-for-licensed-operators)


5.


Visa, "Introducing the Visa Acquirer Monitoring Program."[https://usa.visa.com/visa-everywhere/blog/bdp/2024/08/29/introducing-the-visa-1724958906425.html](https://usa.visa.com/visa-everywhere/blog/bdp/2024/08/29/introducing-the-visa-1724958906425.html)


6.


Visa, "What Every Merchant Needs to Know About Friendly Fraud."[https://corporate.visa.com/en/sites/visa-perspectives/security-trust/what-every-merchant-needs-to-know-about-friendly-fraud.html](https://corporate.visa.com/en/sites/visa-perspectives/security-trust/what-every-merchant-needs-to-know-about-friendly-fraud.html)
