---
schema_version: "1.0.0"
document_id: "538bba8573b6aeb8795222a188a9dd1a5b1ea5beda8f1396aa401957618ab739"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/how-a-15-decline-rate-compounds-into-an-18-revenue-loss"
published_at: "2026-03-01T17:07:00+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:d1f93752649edca2e7b1c85759da90778cb7701e70bef7840794bbb60fa088c8"
---

# How a 15% Decline Rate Compounds Into an ~18% Revenue Loss

Roughly 15% of all ecommerce orders are declined during authorization. About 70% of those declined orders belong to legitimate customers who were qualified to buy. And between 27% and 33% of those falsely declined customers never come back. Each of those numbers is a problem on its own. Together, they compound into something much larger: approximately 18% of your addressable revenue, lost before it ever reaches your books.


This is not a single-source headline statistic. It’s derived math, with each component independently documented. This article breaks down that math, traces it to the three friction mechanisms responsible for most avoidable declines, and shows you what recovery looks like in practice.


## The Three-Part Equation: Where the ~18% Comes From


**Start with the broadest layer.** Across all markets, roughly 15% of ecommerce transactions fail to process successfully. That 15% includes both recoverable declines (false positives, routing failures, 3DS abandonment) and non-recoverable ones (insufficient funds, closed accounts, hard fraud). The 70% filter in the next layer isolates the recoverable portion. E-commerce baseline decline rates run 10–13%, with subscription and recurring billing models hitting 18–20% due to expired cards, changed billing details, and automated bank blocks (Wallid; Recurly, “Top Payment Decline Reasons for eCommerce”). The 15% figure represents a cross-market average.


**Now apply the second layer.** For the average merchant, issuers decline one in every 10 ecommerce dollars during payment authorization, and 70% of these declined orders are from good customers qualified to make the purchase.


**The math at this point:** 15% of orders declined, 70% of those from real customers. That gives you roughly 10.5% of all attempted orders from genuine buyers incorrectly rejected. This is immediate, same-day revenue loss.


**Now add the third layer:** the customers who don’t come back. Among customers who experience a false decline, 27%-41% never return to the merchant.


When you combine the immediate transaction loss (10.5% of revenue from real customers) with the permanent lifetime value destruction from non-return (27–33% of those customers gone forever), the total revenue impact across the customer lifecycle approaches 18% of addressable revenue.


## What “Authorization Friction” Actually Means: Three Root Causes


Authorization friction is not a synonym for “low auth rate.” It refers to specific mechanisms within the authorization flow that cause legitimate transactions to fail. Three friction sources drive the bulk of avoidable declines.


Issuer over-restriction: the “do not honor” black box. Issuers see the basics: card details, balance, amount, location, and results from their standard fraud checks. They don’t see behavioral patterns, order history, or device details. This information gap leads to systematic over-flagging. Banks falsely decline approximately 15% of legitimate orders. The “do not honor” response code represents 10–60% of all refusals depending on geography. American Express codes over 90% of its declines as “do not honor,” while Visa in the US codes approximately 10% this way (Churnkey, “Do Not Honor Decline”). You can’t fix what you can’t diagnose.


3DS challenge abandonment. Ravelin found that 22% of payments sent through 3DS were lost (Ravelin, “One Fifth of Payments Sent to 3D Secure Are Lost”). That data is from 2019, before widespread 3DS 2.x adoption, and the landscape has improved since: 64% of 3DS transactions now go through a frictionless flow globally, and the UK achieves 93% 3DS success rates (Ravelin, 2025 Global Payments Report). But the friction remains real and ongoing: in Ravelin’s 2019 data, 91% of 3DS transactions took over five seconds to authenticate, with an average of 37 seconds (Ravelin, 2019). European merchants still see a 2–3.5% conversion drop from poorly applied 3DS. In the US, where 3DS success rates average only 41%, merchants may lose up to 15% (DECTA, “Why Your 3DS Authentication Has Low Approval Rates”).


Routing inefficiency. Merchants relying on a single acquirer without fallback logic leave revenue on the table when network rules or processor conditions change (Solidgate, “Intelligent Payment Routing”). A transaction that fails through one processor might succeed through another. Without cascading fallback routing, that revenue is simply abandoned. Intelligent routing optimization can improve approval rates by 10–15% (Solidgate; FlyCode).


## The Customer Who Doesn’t Come Back: Why the Loss Keeps Growing


The immediate transaction loss is only the first impact. The compounding effect is what turns a $150 declined order into a multi-thousand-dollar problem.


Among loyal customers (those with three or more prior approved orders), a false decline triggers a 65% reduction in the number of future orders and a 16% drop in average order value. Consider what that means: a customer who spent $1,200 with you last year gets falsely declined on a $150 order. If they’re in the 27–33% who never return, you’ve lost $1,350 in year-one value alone, not $150. If they’re in the group that comes back but spends less, you’ve still lost hundreds in lifetime revenue from that single decline event.


The behavioral data reinforces this. Only 25% of declined customers try another payment card. Thirty-nine percent abandon the cart entirely (PYMNTS, November 2023). And up to 32% of falsely declined customers post negative feedback on social media (ClearSale / Sapio Research, 2020). That’s brand damage on top of revenue loss.


Fiserv’s research adds another dimension. Twenty percent of cardholders stop using their card entirely after experiencing two or more false declines within a six-month period, and average monthly spending drops 15% per card after two or more false positive denials (Fiserv, “Financial Institutions Ease Cardholder Frustration by Addressing Transaction False Declines”). The damage extends beyond a single merchant.


The financial scale is significant. US ecommerce merchants permanently lost $81 billion to false declines in 2023, with $157 billion in sales initially at risk (PYMNTS, November 2023). Riskified’s 2025 Ascend research, based on a practitioner survey of 130+ payment professionals, puts total ecommerce losses from false declines, fraud, and policy abuse at $448 billion annually. J.P. Morgan data shows that false positive losses (19% of total fraud cost) actually exceed actual fraud losses (7% of total fraud cost) (J.P. Morgan, “False Positives & Fraud Prevention Tools”). False declines cost merchants 13 times more than actual fraud (Fiserv Carat). One Aite Group / ClearSale estimate from 2019 puts the ratio at 75 times.


## Why Your Dashboard Doesn’t Show Any of This


You might expect your payments dashboard to surface this problem. It doesn’t.


Eighty-two percent of executives cannot pinpoint why their payments fail due to fragmented data (PYMNTS, August 2024). Only one-third of ecommerce merchants know whether fraud caused a failed payment (PYMNTS, November 2023). Sixty percent say failed payments are expensive to track and resolve (PYMNTS, 2024).


Standard PSP dashboards report total authorization rate. They don’t report false decline rate. They don’t track whether a declined customer returns or churns permanently. They don’t measure lifetime value erosion from individual decline events. For multi-PSP merchants, the problem multiplies: three different dashboards with three different authorization rate figures and no unified view of which declines are recoverable false declines versus genuine fraud.


This is why the ~18% revenue impact stays invisible. The data exists, but it sits in separate systems that don’t connect.


## What High-Performing Merchants Do About Authorization Friction


Recovery is structured around the three root causes.


**Enriching transaction data for issuers.** Sending additional context (device fingerprint, customer tenure, transaction history) gives issuers the confidence to approve transactions they would otherwise flag.


**Smarter 3DS application.** Applying 3DS selectively (exempting low-risk transactions, using risk-based authentication) reduces abandonment while maintaining compliance. The difference between blanket 3DS enforcement and intelligent exemption strategies can be the difference between a 2–3.5% conversion drop and minimal impact.


**Network tokenization.** Replacing stored card numbers with network-level tokens delivers a 4.6% global authorization rate lift and 26–30% fraud reduction (Visa Acceptance Solutions, “Tokens Are Key to Future Proofing Payments”).


The results at scale are real.[Checkout.com](http://checkout.com/) ’s Intelligent Acceptance raised merchants’ acceptance rates by an average of 3.8% in 2024, generating over $10 billion in additional merchant revenue since launch ([Checkout.com](http://checkout.com/) newsroom). Stripe’s Adaptive Acceptance recovered $6 billion in falsely declined transactions in 2024, a 60% year-over-year improvement in retry success rate (Stripe, “AI Enhancements to Adaptive Acceptance”).


## The Recovery Calculation: What This Is Worth for Your Business


Here’s the math applied to a specific scenario.


> If your business processes $10 million per month at an 87% authorization rate, $1.3 million per month is declining. If 70% of those declines are false declines from real customers, that’s $910,000 per month in legitimate buyers being turned away. A 3–5 percentage point improvement in authorization rate recovers $300,000 to $500,000 per month, or $3.6 million to $6 million per year. That recovery doesn’t require new customers, additional marketing spend, or pricing changes. It comes from approving real buyers who are already at checkout.


The first step is visibility: understanding which of your declines are recoverable, which friction mechanisms are driving them, and which customers you’re losing permanently. That’s the analytical layer most merchants are missing.


Corgi Intelligence surfaces exactly this data, unifying decline analytics across processors and quantifying the revenue impact of each friction source. Corgi Model takes it a step further with custom machine learning trained on your transaction data, approving more real buyers while reducing chargebacks.


Sources


Aite Group / ClearSale, “False Decline Cost Ratios” (2019), via[Greip.io](http://greip.io/)


[Checkout.com](http://checkout.com/) , “[Checkout.com](http://checkout.com/) Surpasses $10 Billion in Revenue Unlocked”


Churnkey, “Do Not Honor Decline: Meaning, Stats, and How To Fix”


ClearSale / Sapio Research via Digital Commerce 360, “33% of US Consumers Drop Retailers After a False Decline” (2020)


DECTA, “Why Your 3DS Authentication Has Low Approval Rates: 5 Optimisation Tips”


Fiserv, “Financial Institutions Ease Cardholder Frustration by Addressing Transaction False Declines”


Fiserv Carat, “False Decline”


FlyCode, “Smart Payment Orchestration: From Simple Rules to AI”


GR4VY, “Approval Rates in Payments: Meaning and Deep Dive for 2025”


J.P. Morgan, “False Positives & Fraud Prevention Tools”


LexisNexis, 2018 True Cost of Fraud Survey, via Sherwen


[Primer.io](http://primer.io/) , “How to Recover Lost Revenue with Cascading Payments”


PYMNTS, “82% of Merchants Don’t Have the Data to Pinpoint Why Payments Fail” (August 2024)


PYMNTS, “eCommerce Firms Will Lose $81B to False Declines in 2023” (November 2023)


PYMNTS, “Nearly 60% of Firms Say Failed Payments Are Expensive to Track and Resolve” (2024)


Ravelin, “One Fifth of Payments Sent to 3D Secure Are Lost” (2019)


Ravelin, “New Data in Payments Authentication & 3DS Released” (Global Payments Report 2025)


Recurly, “Top Payment Decline Reasons for Subscription eCommerce”


Riskified, “Unlock Revenue by Optimizing Payment Authorization Rates”


Riskified, Lorna Jane Case Study


Riskified / StockTitan, “85% of Merchants Battle to Balance Customer Experience and Fraud Prevention” (Ascend 2025)


Sherwen, “How False Declines Hurt More Than Actual Ecommerce Fraud” (LexisNexis data)


Signifyd, “5 Strategies to Increase Bank Authorization Rates for Merchants”


Signifyd, “False Declines Explained”


Solidgate, “Intelligent Payment Routing: Boost Conversion”


Stripe, “AI Enhancements to Adaptive Acceptance”


Visa Acceptance Solutions, “Tokens Are Key to Future Proofing Payments”


Wallid, “Where Payments Fail: Industries with Highest Decline Rates 2025”
