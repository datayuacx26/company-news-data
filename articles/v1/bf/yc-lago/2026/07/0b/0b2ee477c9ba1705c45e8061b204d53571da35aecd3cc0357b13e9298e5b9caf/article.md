---
schema_version: "1.0.0"
document_id: "0b2ee477c9ba1705c45e8061b204d53571da35aecd3cc0357b13e9298e5b9caf"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/pricing-billing-glossary"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:3e614bbb28a4249c6c612a98286935c4e6918b3f5050ec2370da881b4f5e0270"
---

# Pricing and Billing Glossary (2026)

26 terms. The ones that come up in billing system design, pricing strategy discussions, SaaS finance reviews, and investor conversations. Cmd+f for what you need.


## Coupons


Promotional discounts applied at the invoice or subscription level. Can be fixed-amount, percentage, or recurring. Typically time-limited and usage-capped to prevent stacking or misuse.


In a billing system, coupons need to handle expiration gracefully. When a coupon expires, the subscription should transition to full pricing without manual intervention. Tracking coupon redemptions and their revenue impact is table stakes for measuring campaign ROI.


---


## Billable Metric


A[billable metric](https://getlago.com/docs/guide/billable-metrics/create-billable-metrics) defines how incoming events are aggregated to measure consumption. It's the bridge between raw usage data and what gets billed. You define the aggregation method (sum, count, max, unique count) and the field to measure. API calls, gigabytes stored, active users, tokens processed: each is a billable metric with its own aggregation logic and billing rules.


The granularity of billable metrics determines the accuracy of usage-based billing. A poorly defined metric (counting unique users by day rather than by month, for example) changes what customers are charged and creates billing disputes.


---


## Credit Notes


Formal documents reducing the amount a customer owes. Issued for billing corrections, overcharges, refunds, or service credits. Credit notes create an audit trail and adjust outstanding invoice balances.


A billing system needs to generate credit notes correctly, sync them to accounting, and apply them automatically against future invoices or outstanding amounts. Manual credit note processes at scale create reconciliation errors.


---


## Dunning


The[process of recovering failed payments](https://getlago.com/blog/dunning-management) through automated communication sequences. A payment fails; dunning kicks in: retry schedules, email sequences, grace periods. The goal is to recover the revenue before the account is suspended or churned.


Good dunning reduces involuntary churn. Involuntary churn is customers lost not because they wanted to cancel, but because their card expired or was declined. Most companies underinvest in dunning and overcount their "real" churn as a result.


---


## Grandfathering


Letting existing customers keep old pricing when you raise prices or change plans for new customers. Common during major price increases, pricing model transitions, or plan restructures.


The operational challenge: tracking which customers are on grandfathered terms and for how long. At scale, this becomes a billing system requirement. The platform needs to support multiple active pricing structures simultaneously without manual overrides.


---


## Freemium


A permanent free tier that coexists with paid plans. Not a trial. No expiry date. The free version provides real value and serves as acquisition; revenue comes from users who need more than the free tier offers.


Freemium economics require the free tier to be cheap enough to operate at scale (low support burden, low infrastructure cost per user) while being compelling enough to drive organic acquisition. The conversion math: if 5% of free users convert to paid, and your paid ARPU is $100/month, every 100 free users is worth $500 MRR.


---


## Free Trial


Time-limited access to a paid product before purchase. Usually full-featured or functionally complete. Converts to paid, downgrades to freemium, or ends. Depends on what the billing system is configured to do.


Trial-to-paid conversion rate is the key metric. Most B2B SaaS companies see 15-25% trial conversion when trials are well-structured with proper onboarding. The billing system needs to handle the trial-to-paid transition smoothly: credit card collection, invoicing start dates, and grandfathered pricing for early converters.


---


## Subscription


A recurring billing arrangement where a customer pays at regular intervals (monthly, annually) for continued access to a product or service. The commercial foundation of most SaaS businesses.


Subscriptions create predictable MRR and ARR. The tradeoff: flat fees don't align with actual value when usage varies significantly across accounts. A customer using 10x more than another pays the same. That misalignment is the primary driver of the shift toward usage-based and[hybrid pricing models](https://getlago.com/solutions/use-cases/hybrid-plans) .


---


## Per Seat Pricing


[Billing based on the number of users](https://getlago.com/blog/billing-models) with access to the product. Simple to explain, predictable for both vendor and customer, easy to forecast.


Breaks down in three situations: when users share credentials (billing is gamed), when value doesn't track to headcount (a 10-person team using your product heavily vs. a 100-person team with 5 active users; the larger company is a worse customer on a per-seat model), and when the product is infrastructure that doesn't have a "user" concept at all.


---


## Volume Pricing (Bulk Pricing)


A discount structure where a single unit price applies to the entire quantity purchased, based on the total volume tier reached. Buy 600 units where the "500+ units" tier is $0.60: all 600 units are billed at $0.60.


The incentive structure: customers who commit to larger volumes get a better rate on everything. The customer bears no risk. They already bought the volume and get the discount automatically. Compare to Graduated Pricing, where the discount only applies to units above each threshold.


---


## Graduated Pricing


A tiered pricing structure where each unit is priced at the tier it falls in, not the total volume. First 100 units at $1.00, units 101-500 at $0.80, units 501+ at $0.60. A customer buying 600 units pays: (100 x $1.00) + (400 x $0.80) + (100 x $0.60) = $480. In volume pricing, all 600 units at $0.60 = $360.


Graduated pricing rewards incremental usage without giving away margin on early units. It's more complex to communicate to customers but more favorable to vendors at lower usage volumes. Most billing system implementations of "tiered pricing" are graduated. Verify which mechanism your platform uses before signing up.


---


## Usage-Based Pricing


Customers are[billed based on actual usage](https://getlago.com/blog/the-full-playbook-how-to-design-usage-based-pricing-models) of a product or service rather than a flat subscription fee. Revenue scales with customer value delivered.


The attraction for customers: pay for what you use, no upfront commitment. The challenge for vendors: revenue becomes variable and harder to forecast. Usage-based pricing requires metering infrastructure capable of tracking consumption accurately at scale. It also changes the sales motion. Instead of a fixed contract, you're selling a usage relationship that compounds over time as the customer grows.


---


## Metered Billing


Billing based on real-time measurement of resource consumption. Usage is tracked at the event level: each API call, each GB transferred, each message sent, aggregated over a billing period, and invoiced based on total consumption.


Metered billing is the technical mechanism; usage-based pricing is the commercial model. Reliable metered billing requires a[usage metering](https://getlago.com/platform/usage-metering) infrastructure capable of handling high-volume event streams without data loss or drift. Aggregation errors in metering translate directly to billing errors: either undercharging customers or generating disputes.


Metered billing can also be layered onto a subscription as an overage charge. A base fee covers the first N units; usage above that is metered. That's the foundation of most hybrid pricing models.


---


## Pay-As-You-Go


A[usage-based pricing model](https://getlago.com/blog/pay-as-you-go-pricing) with no minimum commitment. Customers are billed after consumption, typically at end of each billing cycle, with no prepayment and no contract.


Maximally flexible for customers. Maximally risky for vendors: no committed revenue, high sensitivity to usage fluctuation. Common in infrastructure and communications products where the use case is clear and the switching cost is low. Often used as the entry point before graduating customers to committed or prepaid plans.


---


## Value-Based Pricing


[Pricing set based on perceived value](https://getlago.com/blog/value-based-pricing) to the customer, not cost-plus margins or competitive benchmarking. A product that saves customers $1M/year can be priced at $100K/year and still deliver strong ROI. Cost-based pricing would miss this entirely.


Value-based pricing is the theoretically correct approach and the hardest to implement. It requires deep customer research to understand willingness to pay, customer segmentation (different buyers get different value), and organizational willingness to charge different prices to different customers for the same product. Most companies benchmark against competitors instead.


---


## Credit-Based Pricing


Customers[purchase credits](https://getlago.com/blog/credit-based-pricing) upfront (tokens, points, a wallet balance) that are redeemable against products or services. Revenue is recognized as credits are consumed.


Credit-based pricing is useful when: (1) you want customers to pre-commit spending, (2) the consumption unit is abstract (tokens vs. dollars), or (3) you need to support a marketplace where multiple services draw from one balance. Common in AI products, communication APIs, and developer platforms. The billing complexity: tracking credit balances in real time, handling credit expiration, and reconciling credits against actual usage.


---


## Prepaid Credits


Customers purchase a balance upfront (credits, tokens, or wallet funds) that is drawn down by usage. Revenue is recognized as credits are consumed, not when purchased.


Prepaid credits decouple the purchase act from consumption. They provide vendors with upfront cash flow and customers with usage flexibility. Unlike a subscription, prepaid credits don't automatically renew on a fixed cycle unless the system is configured to do so.


Common in AI products (token packs), communication APIs (message credits), and developer platforms.[Lago's wallet feature](https://getlago.com/platform/cash-collection) handles prepaid credit management: balance tracking, real-time drawdown, top-up notifications, and expiration handling.


---


## Dynamic Pricing


[Real-time price adjustments](https://getlago.com/blog/dynamic-pricing) based on demand, supply, or time. Prices change in response to conditions rather than being fixed in advance.


Common in ride-sharing, travel, and ad tech. Rare in B2B SaaS. Customers negotiating annual contracts expect price stability. Where it does appear in SaaS: spot pricing for cloud resources, auction-based ad inventory, and some API pricing that adjusts with demand. Implementing dynamic pricing requires real-time pricing logic in the billing system and customer-facing transparency about how prices are calculated.


---


## Hybrid Pricing


A[pricing model combining multiple mechanisms](https://getlago.com/blog/hybrid-pricing-models) , typically a fixed platform fee plus usage-based overage. Customers pay a predictable base cost and an additional metered charge for usage above a threshold.


Hybrid pricing solves the tension between subscription predictability (vendors like it) and usage-based fairness (customers like it). The most common structure: a monthly seat or platform fee that covers a usage allowance, then metered charges beyond that. Common in AI products, communication APIs, and enterprise SaaS where accounts have variable workloads.


---


## MRR / ARR / Committed MRR


Three related metrics, often conflated.


**MRR (Monthly Recurring Revenue):** the sum of all active subscription values normalized to one month. A $1,200/year contract contributes $100 MRR. A $50/month plan contributes $50 MRR. One-time fees and variable usage charges are typically excluded.


**ARR (Annual Recurring Revenue):** MRR x 12. Used for annual planning and investor reporting. Not the same as total bookings or total revenue. It's the normalized annual run rate of recurring contracts.


**Committed MRR:** MRR locked in via annual or multi-year contracts. Higher-confidence than month-to-month MRR. Customers can't cancel without contract penalties. Investors and finance teams weight committed MRR more heavily. A business with 80% committed MRR has far more predictable revenue than one with 80% month-to-month.


---


## NRR


[Net Revenue Retention](https://getlago.com/blog/usage-based-pricing-nrr) : (Starting MRR + Expansion MRR - Contraction MRR - Churned MRR) / Starting MRR x 100.


Above 100% means expansion revenue from existing customers is outpacing churn and downgrades. The business grows from its installed base before counting new logos. NRR above 120% is a strong signal of product-led growth or effective expansion sales. Below 100% means the base is shrinking even as new customers come in.


NRR is the metric most correlated with long-term SaaS valuation multiples. It captures both sides of the existing-customer revenue equation: what's being lost (churn, contraction) and what's being gained (expansion, upsell). Gross churn alone misses the expansion side entirely.


---


## Churn


The rate at which customers cancel or don't renew. Two types: customer churn (percentage of customers lost in a period) and revenue churn (percentage of MRR lost). These diverge when small customers cancel at higher rates than large ones.


Involuntary churn happens when payments fail: card expired, bank decline, insufficient funds. It's recoverable with properly sequenced[dunning](https://getlago.com/blog/dunning-management) automations. Voluntary churn happens when customers actively cancel. Most companies undercount churn by measuring only voluntary cancellations and ignoring failed-payment attrition.


The compounding math matters. 3% monthly revenue churn = roughly 30% ARR lost per year. At that rate, a business must grow new ARR 30%+ just to stay flat.


---


## ACV


Annual Contract Value: total contract value / contract years. A $150,000 three-year deal has $50,000 ACV. A $50,000 one-year deal also has $50,000 ACV.


ACV normalizes multi-year deals to a per-year figure, making it possible to compare deal sizes across different contract lengths. Used for sales capacity planning, quota setting, and investor reporting. Different from ARR: ARR is the current run rate of all active contracts; ACV typically measures the value of new bookings.


---


## True-Up


End-of-period reconciliation between estimated or committed usage and actual consumption. Common in enterprise software licensing, cloud infrastructure, and seat-based contracts.


The mechanics: a customer commits to a minimum seat count or usage tier at contract start. Actual usage is tracked throughout the contract period. At the end (quarterly or annually), if actual usage exceeded the commitment, the customer pays the difference.


True-up provisions protect vendor revenue in contracts where initial estimates are conservative. For customers, they create a reconciliation exposure that needs to be managed with usage monitoring throughout the contract period.


---


## Proration


Adjusting charges for partial billing periods. When a subscription starts, ends, or changes mid-period, the charge is calculated proportionally to days active.


Formula: (days active in period / total days in period) x plan price.


Example: a $50/month plan activated on August 10. August has 31 days. Days remaining: 22. Prorated charge: 22 / 31 x $50 = $35.48.


The same logic applies to mid-period upgrades: the old plan is credited pro-rata, the new plan is charged pro-rata for the remaining days. Proration prevents double-billing and ensures customers pay only for the service period they received. Getting proration wrong is one of the most common sources of billing disputes.


---


## Value Metric


The specific unit of measurement a pricing model is tied to. "Per seat," "per API call," "per GB stored," "per active user": these are all value metrics. The value metric defines what scales with the customer's usage and what they pay for.


Choosing the right value metric is one of the most consequential pricing decisions. A well-chosen value metric aligns cost with value delivered. Customers who get more value pay more, without the vendor leaving money on the table with flat-rate pricing. A poorly chosen metric creates friction (customers try to game it) or misalignment (customers who use the product heavily pay the same as light users).


---


*Built by*[Lago](https://getlago.com/) *, open-source billing infrastructure for usage-based and hybrid pricing.*[Explore the docs](https://getlago.com/docs/welcome) *or*[book a demo](https://getlago.com/book-a-demo) *.*
