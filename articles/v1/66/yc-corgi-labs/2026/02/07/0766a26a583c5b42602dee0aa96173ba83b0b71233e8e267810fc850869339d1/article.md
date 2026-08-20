---
schema_version: "1.0.0"
document_id: "0766a26a583c5b42602dee0aa96173ba83b0b71233e8e267810fc850869339d1"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/your-authorization-rate-is-a-vanity-metric-what-that-91-is-actually-hiding"
published_at: "2026-02-09T20:00:00+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-28T22:21:12.159869+00:00"
content_hash: "sha256:d27d13daf55336986a6029e0b1ac1276d6b482bc20b0050678d0637de668d2ee"
---

# Your Authorization Rate Is a Vanity Metric: What That 91% Is Actually Hiding

**For payment operations leaders, the authorization rate has become the metric you check but never question.** It sits on a dashboard, it trends in a reasonable range, and it gives you a false sense of control. The problem isn’t that the number is wrong. The problem is that it’s incomplete, and the gap between what it shows and what’s actually happening is where your revenue disappears.


According to Worldpay, for a business processing $1 billion in annual transactions, a single percentage point improvement in authorization rate equals $10 million in recovered revenue. That lift comes without acquiring new customers, increasing marketing spend, or changing pricing. Yet most merchants lack the visibility to diagnose where those lost percentage points go.


Here’s what your authorization rate is hiding, how to quantify what you’re losing, and what the highest-performing merchants do to close the gap.


## The Composite Metric Problem: Why Top-Line Auth Rates Mislead


Industry authorization rates for e-commerce range from 85% to 95%, according to data from Worldpay and GR4VY. These benchmarks primarily reflect North American and European markets; authorization rates in LATAM and APAC regions often trend lower due to different issuer practices and fraud patterns. That said, even within this range, a “good” rate for one merchant may represent millions in lost revenue for another at similar scale. The top-line number tells you your approval percentage, but it doesn’t tell you why transactions fail, which failures you can fix, or how much money sits on the table.


Your authorization rate is a composite of multiple failure modes blended into one figure. It includes hard declines (stolen cards, closed accounts) that you can’t recover. It includes soft declines (insufficient funds, processor timeouts, missing authentication data) that you often can recover. It includes transactions flagged by issuer fraud models for patterns that aren’t actually fraudulent. And it includes a massive bucket of “do not honor” responses that carry no diagnostic value at all.


When you look at a single number on a dashboard, you’re averaging all of these together. A stable 91% might mean your soft decline recovery is excellent and your hard decline rate is creeping up. Or it might mean your fraud flags are increasing but your retry logic is compensating. You can’t tell, because the metric doesn’t decompose itself.


## What Your Dashboard Isn’t Showing You: The Anatomy of a Decline


Here’s the first thing most dashboards obscure: 80% to 90% of all payment declines are soft declines, meaning they’re potentially recoverable. Soft declines can often be resolved through retry logic, updated card credentials, or richer transaction data sent to the issuer. Yet most dashboards don’t clearly separate soft declines from hard declines (Spreedly; GR4VY).


The second thing your dashboard hides is even more frustrating. According to a 2016 Visa Global Declines Analysis, over 76% of Visa’s global declined transaction volume fell into just two categories: “insufficient funds” and “do not honor.” Industry sources confirm this pattern persists today (Churnkey). The “do not honor” code (response code 05) is a catch-all that can represent anywhere from 10% to 60% of all refused payments depending on geography (Churnkey). As American Banker reported, issuing banks put most declines into one large bucket of “do not honor,” giving merchants almost no information about why a transaction actually failed.


Think about what that means in practice. Your dashboard shows a decline. The decline code says “do not honor.” You have no idea whether the issue was a fraud rule, a velocity check, an address mismatch, or something else entirely. **You can’t fix what you can’t diagnose.**


If you run payments across multiple processors, the problem compounds. Each PSP uses different data formats, reporting structures, decline code taxonomies, and settlement timelines. A merchant using Stripe, Adyen, and Braintree may see three different decline rates for the same region with no way to normalize or compare them (Payrails). Every PSP dashboard shows different metrics, with fields that don’t match and varying timeframes, creating fragmented reporting that hides revenue opportunities.


For multi-PSP merchants, your authorization rate isn’t just a vanity metric. It’s three or four different vanity metrics that don’t talk to each other.


## The Hidden Revenue Leak: Quantifying What You’re Losing


The revenue buried beneath your authorization rate is larger than most payment teams realize.


False declines (legitimate transactions incorrectly rejected) cost merchants an estimated $308 billion globally in 2023, according to industry estimates compiled by Riskified and others. That figure exceeds actual fraud losses. A more conservative 2026 estimate from PYMNTS places the number at $50 billion. Either way, merchants lose more money to false declines than to fraud itself, a point Chargebacks911 has emphasized (CrowdFund Insider).


The customer impact is just as stark. In 2024, 56% of U.S. consumers reported experiencing a false payment decline in the prior three months (PYMNTS). Among loyal customers who experience a false decline, subsequent order volume drops by 65% (Riskified). Riskified’s research shows that 27% of loyal customers never return to the merchant after a false decline. Broader studies suggest 32% to 33% of all consumers abandon a merchant entirely after the experience (Riskified; Signifyd).


For subscription and SaaS businesses, the math gets worse. The average SaaS business loses approximately 9% of its recurring revenue to failed payments annually, effectively negating a full month of growth each year (Stripe analysis, via Baremetrics). Involuntary churn, caused by failed payments rather than customer decisions, accounts for 20% to 40% of total churn in subscription businesses (ProfitWell research, via Userpilot).


Consider what that means if you’re running a $40 million ARR SaaS company. Nine percent of recurring revenue is $3.6 million per year lost to payment failures. Only about 70% of failed payments are ever recovered on average (Recurly Research). The remaining 30% becomes permanent revenue loss that your dashboard attributes to “churn” without distinguishing whether the customer chose to leave or their payment simply failed.


Your authorization rate doesn’t tell you any of this. It shows you a percentage. It doesn’t show you the customers who left because their renewal was declined, the revenue that could have been retried, or the fraud rules that are blocking your own subscribers.


## What High-Performing Merchants Do Differently


The gap between average and top-performing merchants isn’t luck. It’s instrumentation and process. Here are the specific techniques that move authorization rates by meaningful amounts, backed by data.


**Network tokenization.** Replacing stored card numbers (PANs) with network-level tokens delivers a measurable 2 to 6 percentage point lift in authorization rates. Visa reported a 4.6% global authorization rate lift for tokenized transactions versus PAN-based transactions, along with a 30% reduction in fraud (Visa Acceptance Solutions). Mastercard reports a 3 to 6 percentage point improvement (Mastercard). By 2024, 47% of merchants had adopted tokenization, up from 44% in 2023. By 2025, six in 10 merchants using tokenization cited authorization rate improvement as a primary benefit (MRC Global Reports).


**Intelligent retry logic.** Smart retry engines, combined with card account updater services, recover 60% to 70% of failed payments (Slickerhq; Cleverbridge). The timing, sequencing, and data enrichment of retries matters enormously. A retry sent at the wrong time or without updated card information fails just like the original attempt. A retry sent with fresh credentials, when the cardholder’s account is more likely to have funds, succeeds at significantly higher rates.


Stripe’s Adaptive Acceptance recovered $6 billion in falsely declined transactions in 2024, reflecting a 60% year-over-year increase in retry success rate (Stripe). Their average authorization rate lift across merchants is roughly 2.2%.


**Richer transaction data for issuers.** Issuers decline transactions when they lack confidence that the transaction is legitimate. Sending additional data fields (device fingerprint, customer tenure, transaction history) gives issuers more context to approve. One athletic apparel brand working with Riskified lifted authorization rates from 82% to 95% by enriching transaction data and optimizing the fraud decisioning layer before transactions reached issuers (Riskified).


**Unified cross-processor analytics.** Merchants who normalize their decline data across processors can spot patterns invisible in siloed dashboards. Is one processor declining more transactions in a specific BIN range? Are issuer fraud rules triggering differently depending on which acquirer routes the transaction? Without unified analytics, these questions go unanswered.


The proof points are compelling. Zapier achieved a 4% authorization rate uplift by combining Adaptive Acceptance, network tokens, and card account updater, translating to over $3 million in additional revenue (Stripe Newsroom). GAIA, a streaming company, moved from 80% to 89%+ authorization rates after gaining visibility into why transactions were failing and applying targeted optimization (Stripe). Worldpay reports that their optimization tools deliver a 1.5% revenue uplift within 90 days for participating merchants.


## From Vanity Metric to Revenue Recovery: Five Steps to Start


Closing the authorization rate blind spot doesn’t require a full platform migration. It starts with visibility.


**1. Disaggregate your authorization rate.** Break your top-line number down by decline type (soft vs. hard), decline code, issuer, BIN range, card brand, and processor. This single step often reveals that 80% or more of your declines are soft and potentially recoverable.


**2. Build a soft decline retry strategy.** Not all retries are equal. Map your most common soft decline codes to specific retry actions: timing adjustments, credential updates, data enrichment, or alternative routing. Target a 60% to 70% recovery rate on soft declines as your benchmark.


**3. Adopt network tokens.** If you haven’t moved to network tokenization, you’re leaving a 2 to 6 percentage point authorization rate lift on the table. The merchant adoption curve is accelerating, and the data on authorization rate improvement is consistent across card networks.


**4. Unify your payments data across processors.** If you run multiple PSPs, you need a single view that normalizes decline codes, standardizes reporting periods, and lets you compare performance across processors for the same transaction types. Fragmented dashboards make optimization guesswork.


**5. Measure what matters.** Track false decline rate, soft decline recovery rate, involuntary churn rate, and revenue recovered per retry cycle. These metrics tell you whether your authorization rate is improving because you’re actually approving more real buyers, or just because your transaction mix shifted.


Each of these steps moves you from treating your authorization rate as a number to check toward treating it as a system to optimize. The merchants recovering millions in previously lost revenue aren’t doing anything mysterious. They’re looking at data that was always there, just buried beneath a single percentage on a dashboard.


**For payment teams ready to dig into their decline data across processors and pinpoint exactly where revenue is leaking, Corgi Intelligence and Corgi Model provide this level of visibility and optimization, with results in days and no development work required.**


The 91% on your dashboard isn’t wrong. It’s just not telling you the whole story.


---


## Sources


-


[American Banker, “05: Do Not Honor Card Refusals Are Confusing to Merchants”](https://www.americanbanker.com/payments/opinion/05-do-not-honor-card-refusals-are-confusing-to-merchants)


-


[Baremetrics, “5 Ways to Prevent Involuntary Churn in SaaS”](https://baremetrics.com/blog/involuntary-churn) (citing Stripe analysis on SaaS failed payment revenue loss)


-


[CrowdFund Insider, “False Declines Costing Merchants More Than Fraud, Report Claims”](https://www.crowdfundinsider.com/2025/10/254284-false-declines-costing-merchants-more-than-fraud-report-claims/) (Chargebacks911)


-


[Churnkey, “Do Not Honor Decline: Meaning, Stats, and How To Fix”](https://churnkey.co/blog/do-not-honor-decline/) (citing Visa Global Declines Analysis, 2016)


-


[Cleverbridge, “Recover Failed Payments and Prevent Involuntary Churn with AI-powered Retry Logic”](https://grow.cleverbridge.com/blog/failed-payment-recovery-dynamic-retries)


-


[GR4VY, “Approval Rates in Payments: Meaning and Deep Dive for 2025”](https://gr4vy.com/posts/approval-rates-in-payments-meaning-and-deep-dive-for-2025/)


-


[GR4VY, “What Is the Difference Between Hard and Soft Decline in Payments?”](https://gr4vy.com/posts/what-is-the-difference-between-hard-and-soft-decline-in-payments/)


-


[MRC, “2025 Global eCommerce Payments and Fraud Report”](https://merchantriskcouncil.org/learning/mrc-exclusive-reports/global-payments-and-fraud-report)


-


[MRC, “2024 Global eCommerce Payments and Fraud Report”](https://merchantriskcouncil.org/learning/mrc-exclusive-reports/global-payments-and-fraud-report/2024-global-payments-and-fraud-report)


-


[Optimized Payments, “Network Tokenization: A Strategic Advantage in Modern Payments”](https://optimizedpayments.com/insights/card-fees/network-tokenization-a-strategic-advantage-in-modern-payments/) (Mastercard data)


-


[Payrails, “From Data Fragmentation to Strategic Control”](https://www.payrails.com/blog/unified-payment-analytics)


-


[PYMNTS, “56% of US Consumers Experienced a False Payment Decline in Last 90 Days” (2024)](https://www.pymnts.com/news/payments-innovation/2024/56-of-us-consumers-experienced-a-false-payment-decline-in-last-90-days)


-


[PYMNTS, “47% of Merchants Say False Declines Cost Them Sales” (2026)](https://www.pymnts.com/fraud-prevention/2026/47-percent-of-merchants-say-false-declines-cost-them-sales/)


-


[Riskified, “The True Cost of Declined Orders”](https://www.riskified.com/blog/true-cost-declined-orders/)


-


[Riskified, “Unlock Revenue by Optimizing Payment Authorization Rates”](https://www.riskified.com/blog/payment-authorization-rates/) (athletic apparel brand case study, 82% to 95% lift)


-


[Signifyd, “5 Strategies to Increase Bank Authorization Rates for Merchants”](https://www.signifyd.com/blog/increase-authorization-rates/)


-


[Slickerhq, “Cut Involuntary Churn by 70% in 2025”](https://www.slickerhq.com/blog/cut-involuntary-churn-70-percent-ai-retry-engines-vs-static-billing-logic-2025)


-


[Spreedly, “How to Improve Soft and Hard Decline Rates”](https://www.spreedly.com/blog/how-to-improve-soft-and-hard-decline-rates)


-


[Stripe, “AI Enhancements to Adaptive Acceptance”](https://stripe.com/blog/ai-enhancements-to-adaptive-acceptance) ($6B recovery in 2024)


-


[Stripe, GAIA Customer Case Study](https://stripe.com/en-jp/customers/gaia) (80% to 89%+ authorization rate)


-


[Stripe Newsroom, “Zapier sees 4% uplift in auth rates with Stripe”](https://stripe.com/newsroom/stories/zapier) ($3M+ additional revenue)


-


[Userpilot, “Involuntary Churn vs Voluntary Churn in SaaS”](https://userpilot.com/blog/involuntary-churn/) (citing ProfitWell research, 20-40% of total churn)


-


[Visa Acceptance Solutions, “Tokens Are Key to Future Proofing Payments”](https://www.visaacceptance.com/en-us/blog/article/2025/tokens-are-key-to-future-proofing-payments.html) (4.6% auth rate lift, 30% fraud reduction)


-


[Worldpay, “The C-Suite’s Guide to Payment Authorization Rates”](https://www.worldpay.com/en/insights/articles/c-suite-guide-to-auth-rates) ($10M per percentage point at $1B volume)


-


[Worldpay, “Smarter Payments, More Revenue”](https://www.worldpay.com/en/insights/articles/authorization-rates-insights) (1.5% revenue uplift within 90 days)
