---
schema_version: "1.0.0"
document_id: "9c6d46686c75d7dba68afd0bae6b78eed76f1c107fd273cb2fe68ded55dfaa95"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/insights/stripe-radar-vs-custom-ml-fraud"
published_at: "2026-05-11T14:00:00+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-28T22:15:07.431097+00:00"
content_hash: "sha256:df899007c8e581e2bc43c47920312fd3cf8419ad5846a3e1721f61e304d48759"
---

# Stripe Radar vs Custom ML: Why Network-Wide Fraud Models Miss Your Best Customers

She's bought from you 14 times in the past two years. She just upgraded her phone and is checking out from a hotel in Barcelona. She's shipping a birthday gift to her sister back home. New device. Foreign IP. Billing and shipping address mismatch. Your fraud system fires three rules at once and blocks the transaction.


She doesn't call support. She doesn't retry. She just leaves.


That wasn't a $100 problem. Factor in the lost sale, the lifetime value walking out the door, the wasted acquisition cost, and the brand damage. That single false decline cost you between $978 and $1,028.


Now multiply that across your entire customer base. US merchants lost $81 billion to false declines in 2023. Total global ecommerce fraud losses were $48 billion in the same year. Your fraud prevention system is blocking more legitimate revenue than fraudsters are stealing.


The ratio tells the story: for every $1 lost to actual fraud, merchants forfeit $30 by declining real buyers.


## Generic Fraud Rules Are Built to Over-Decline


Rules-based fraud systems seem logical on the surface. A new fraud pattern appears, so your team adds a rule to catch it. Then another pattern, another rule. Over months and years, the ruleset grows. It never shrinks.


Each rule makes the system more restrictive. The fraud filter gets tighter with every update, blocking more transactions with every new layer.


Here's what those rules actually flag:


-


**Billing and shipping mismatch:** Could be fraud. More often, it's a gift.


-


**IP and billing country mismatch:** Could be fraud. More often, the customer is traveling.


-


**Multiple orders in quick succession:** Could be fraud. More often, it's a repeat buyer restocking.


-


**High order value:** Could be fraud. More often, it's your best customer finding a product they love.


-


**AVS mismatch:** Could be fraud. Stripe's own documentation notes that a card issuer might still consider a payment that fails CVC or postal code verification legitimate, and therefore approve it.


These rules don't know your business. They're calibrated to generic fraud patterns across all merchants. A supplement company's reorder pattern looks nothing like a luxury fashion brand's weekend splurge. But the same rules evaluate both.


Only 64% of ecommerce merchants even track their false decline rate. The other 36% can't see what their fraud system is doing to real buyers. When you can't measure false declines, you can't price them. Most finance teams model a $100 false decline as a $100 loss. The true cost is 10 times higher.


## Stripe Radar Is Strong. But It's Trained on Millions of Merchants, Not Yours.


Stripe Radar is a genuinely powerful fraud tool. It trains on data from millions of global companies processing more than $1.4 trillion in payments per year. There's a 92% chance any given card has been seen before on the Stripe network. Across all merchants, Radar reduces fraud by 38% on average.


Those numbers are impressive for good reason. Stripe has built one of the most data-rich fraud detection networks in payments.


But "on average" carries a lot of weight. Radar evaluates risk signals against patterns across all merchant verticals. A card that looks suspicious to the network model may be completely normal for your customer base. A luxury retailer's $2,000 Saturday transaction looks different from a SaaS company's $49 monthly renewal. Radar's model scores both against the same cross-merchant patterns.


Stripe knows this, too. Their documentation is direct: "The integration you choose affects the completeness of the risk factors you send to Stripe. The more payment data you capture, the better Radar can detect and prevent fraud".


Stripe Radar does excellent work with network-level signals. What it can't do is learn what YOUR buyers specifically look like. That's not a flaw. It's the nature of a model trained across millions of merchants. The opportunity is adding a layer that fills that gap.


## What "Trained on Your Data" Actually Means


"Custom machine learning trained on your data" isn't a marketing phrase. It describes a specific process.


Here's how it works. Your full transaction history becomes labeled training data. Every transaction carries a signal: was it later confirmed fraudulent? Did it result in a chargeback? Or was it approved, fulfilled, and never disputed? A classification model learns which patterns in YOUR data actually predict fraud for YOUR business.


That model then scores each incoming transaction in real time, alongside Stripe's authorization flow.


What changes when the model knows your buyers:


-


A luxury fashion buyer spending $2,000 on a Saturday afternoon from a new device doesn't look like fraud. It looks like a weekend shopping pattern that repeats every 30 days.


-


A supplement subscriber placing an identical order from a different country doesn't look like fraud. It looks like a traveler restocking.


-


A first-time buyer with a geo mismatch who visited your site 12 times before purchasing doesn't look like fraud. It looks like high purchase intent.


-


A long-term customer who upgrades their phone and presents a new device fingerprint doesn't look like fraud. It looks like a phone upgrade.


Generic models score each transaction against population-level patterns. Merchant-specific models score each transaction against YOUR population. The difference matters most for your best customers. Their behavior looks "unusual" to a generic model but completely predictable in the context of your business.


## The Difference in Outcomes: Your Best Customers Pay the Highest Price


False declines don't hit all customers equally. The data on loyal customers is striking.


When you false-decline a customer who has made three or more previous purchases, their future order volume drops by 65%. Even the loyal customers who do return spend 16% less per order. The relationship doesn't fully recover.


Across all customers, 40% to 42% never return after a false decline. And 32% post about the experience publicly online.


Rules-based systems and cross-merchant network models can't factor in customer tenure from your platform. They don't know that the transaction they're about to block is from someone who has purchased from you 14 times. They evaluate each transaction in isolation. Custom ML trained on your data learns that tenure is predictive. A buyer with 14 successful transactions is a fundamentally different risk profile than a first-time buyer, even when both trigger the same flags.


The outcomes from merchant-specific models reflect this. One ecommerce company processes $40 million in annual revenue across US and Singapore markets. After adding Corgi Model alongside Stripe, they saw a 22% increase in payments accepted, an 18% reduction in realized fraud, and more than $2 million in additional revenue.


Across Corgi's merchant base, results include 3% to 12% revenue increases from recovered false declines and 70% to 95% chargeback reductions without rejecting good customers. False declines drop by up to 45%.


## What to Do Next


Start by measuring what you can't currently see.


If you're among the 36% of merchants who don't track false declines, that's step one. Pull your decline data from Stripe and categorize it. How many blocked transactions came from returning customers? How many matched common false-flag patterns like address mismatches or geo discrepancies?


Then do the math with your own numbers. Take your average order value, your customer lifetime value, and your acquisition cost. Apply the $978-per-$100 cost framework. The number will likely surprise you.


If you process on Stripe and want to see what a model trained on your transaction data would catch, Corgi Labs can run that analysis. No development work on your side. Corgi Model works alongside Stripe Radar, adding merchant-specific intelligence on top of Stripe's network-level fraud detection. You keep everything that makes Radar strong and add a layer that learns what your buyers actually look like.


Your fraud system should know your customers as well as you do. When it doesn't, the cost isn't theoretical. It's $1,000 per decline, compounding with every good buyer you turn away.


Book a Demo →


---


### Sources


1.


PYMNTS Intelligence & Nuvei, "Fraud Management, False Declines and Improved Profitability" (November 2023) —[https://www.pymnts.com/study/fraud-management-false-declines-improved-profitability-ecommerce](https://www.pymnts.com/study/fraud-management-false-declines-improved-profitability-ecommerce)


2.


Forter, "2023 Consumer Trust Premium Report" —[https://explore.forter.com/2023trustpremiumreport/p/1](https://explore.forter.com/2023trustpremiumreport/p/1)


3.


ClearSale, "State of Consumer Attitudes on Ecommerce, Fraud, & CX 2023-2024" —[https://en.clear.sale/blog/report-state-of-consumer-attitudes-on-ecommerce-fraud-cx-2023-2024](https://en.clear.sale/blog/report-state-of-consumer-attitudes-on-ecommerce-fraud-cx-2023-2024)


4.


Stripe Radar product page —[https://stripe.com/radar](https://stripe.com/radar)


5.


Stripe Radar documentation, "Optimize Risk Factors" —[https://docs.stripe.com/radar/optimize-risk-factors](https://docs.stripe.com/radar/optimize-risk-factors)


6.


Stripe Radar Rules documentation —[https://docs.stripe.com/radar/rules](https://docs.stripe.com/radar/rules)


7.


Stripe, "AI Enhancements to Adaptive Acceptance" (2024)


8.


Corgi Labs, "Your Fraud System Is Your Most Expensive Revenue Leak" —[https://www.corgilabs.ai/insights/false-declines](https://www.corgilabs.ai/insights/false-declines)


9.


Corgi Labs case study and product documentation —[https://www.corgilabs.ai](https://www.corgilabs.ai/)
