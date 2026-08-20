---
schema_version: "1.0.0"
document_id: "318269f37e7c6999a9bea7091bbfa90a08a80cf24dbc93bb8f9c45b5d5ec417a"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/what-a-marketplace-dollar-costs-you/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T06:20:29.574702+00:00"
fetched_at: "2026-08-07T06:20:30.243690+00:00"
content_hash: "sha256:1d5f886862847bea7fac273c16486611ad154b1a983237cf9d1b44f4fea3bd47"
---

# What a Marketplace Dollar Actually Costs You

*The AWS Marketplace listing fee is a percentage of pre-tax total contract value that varies by product type, offer type, contract size, channel involvement, and buyer region. There is no single marketplace fee — and the difference between the best and worst case on the same revenue is large enough to change a pricing decision.*


---


“What does the marketplace take?” is the first question finance asks, and the honest answer is a table, not a number.


That answer is unsatisfying but useful, because the variables are ones you control. Deployment method, offer type, contract size, whether a partner transacts, and how you structure a renewal all move the rate — sometimes by more than an order of magnitude on the same dollar of revenue.


Everything below comes from AWS’s published listing fee schedule, effective 5 January 2024, with the professional services change of June 2026. No modelled figures, no internal numbers.


---


## **How much does AWS Marketplace charge sellers?**


AWS Marketplace charges a listing fee calculated on the **pre-tax total contract value (TCV)** of a transaction. There is no fee for having a product available — you pay when you sell.


For public offers, the fee is set by deployment method:


- **SaaS — 3%**
- **Server (Amazon Machine Image, container, and machine learning) — 20%**
- **AWS Data Exchange — 3%**


For private offers, the fee is set by contract value instead:


- **Less than $1M — 3%**
- **$1M to less than $10M — 2%**
- **$10M or more — 1.5%**
- **All renewals — 1.5%**


Two modifiers stack on top. **Channel Partner Private Offer (CPPO)** products carry a **0.5% uplift on the listing fee, regardless of offer type or deployment method** . And **professional services** offerings have a **0.5% listing fee for private offers** — reduced from 2.5% on 16 June 2026, with new private offers receiving the reduced rate and existing offers and subscriptions keeping their original terms.


Regional fees are additive where they apply. AWS publishes an additional **1% for South Korea** , effective 1 April 2025 — so a SaaS private offer under $1M to a South Korean buyer is 4%, not 3%.


---


## **The arithmetic, deal by deal**


The rates only mean something applied to a deal. Five worked examples, using the published schedule.


**1. A $250,000 SaaS private offer, direct.** TCV is under $1M, so the private offer rate is 3%.` $250,000 × 3% = $7,500` — you net **$242,500** .


**2. The same $250,000 deal, sold through a channel partner at 15% margin.** CPPO adds 0.5%, taking the rate to 3.5%. Critically, AWS calculates the listing fee **on the discounted price the ISV offers the channel partner** , not the customer’s price.` $250,000 − 15% = $212,500` to you.` $212,500 × 3.5% = $7,437.50` You net **$205,062.50** — the channel cost you $37,437.50 against the direct case, of which the marketplace uplift is a rounding error and the partner margin is everything.


**3. A $2,000,000 SaaS private offer.** TCV crosses into the second tier, so the rate is 2%.` $2,000,000 × 2% = $40,000` . Had that same revenue arrived as four separate offers of $500,000, each would sit in the 3% band:` $2,000,000 × 3% = $60,000` . **Structuring one contract instead of four saved $20,000** — the tiers reward consolidation, and splitting a deal for administrative convenience has a price.


**4. Renewing that $2,000,000 agreement.** All renewals are 1.5%.` $2,000,000 × 1.5% = $30,000` , against $40,000 on the original. Marketplace revenue gets cheaper as it ages, which is worth knowing before you model marketplace as a permanently expensive channel.


**5. A $250,000 transaction on a server product, public offer.**` $250,000 × 20% = $50,000` . The same revenue as example 1, at **6.7× the fee** , because of deployment method alone. This is the single largest fee lever on AWS Marketplace, and it is a product packaging decision, not a pricing one.


And for services: a **$100,000 professional services private offer** now costs` $100,000 × 0.5% = $500` , against` $2,500` under the previous 2.5% rate. If you have been keeping services off the marketplace on fee grounds, that assumption is out of date —[selling professional services on AWS Marketplace](https://www.suger.io/resources/blog/selling-professional-services-on-aws-marketplace/) covers what qualifies.


---


## **The cost lines nobody puts in the model**


The listing fee is the visible cost. Four others are real and routinely omitted.


**The discount you gave to win the deal.** Marketplace transactions frequently carry a negotiated discount that direct deals would not. If a marketplace deal closes at 10% off list to secure committed-spend drawdown, that 10% dwarfs a 3% listing fee. Model them together or you are optimising the smaller number.


**Channel margin.** As example 2 shows, partner margin is the dominant cost in any resold deal. The marketplace uplift is 0.5%; the partner is fifteen or twenty times that.[CPPO vs MPO](https://www.suger.io/resources/blog/cppo-vs-mpo-multiparty-private-offers/) covers how each cloud handles it.


**Working capital.** Marketplace revenue arrives as a disbursement on the marketplace’s schedule, net of fees, after the buyer pays. That gap between recognising revenue and holding cash is a financing cost even when it never appears as one.[Marketplace billing and revenue operations](https://www.suger.io/resources/guides/marketplace-billing/) covers the disbursement cycle.


**Operations.** Every marketplace adds a console, an offer format, an entitlement model, and a reconciliation job. That cost is a headcount question long before it is a tooling question — and the reason to compare[platform pricing](https://www.suger.io/pricing/) against the hours it replaces rather than against the listing fee.


---


## **What the fee buys**


Set against all of it: a marketplace transaction can draw down the customer’s committed cloud spend, arrives through a procurement path they already approved, and skips the vendor onboarding cycle that stalls enterprise deals. For a customer sitting on an[AWS EDP](https://www.suger.io/resources/blog/aws-edp-what-sellers-need-to-know/) commitment, buying through the marketplace is spending money they have already promised to spend.


That is what the percentage is for. The question is never “is 3% expensive” in the abstract — it is whether 3% buys a shorter cycle and a larger deal than the alternative. For most enterprise motions it does. For a high-volume, low-value self-serve product delivered as a server image at 20%, it may not, and the correct response is to change the packaging rather than abandon the channel.


---


## **Frequently asked questions**


**What are AWS Marketplace fees for sellers?** AWS charges a listing fee on the pre-tax total contract value. Public offers are 3% for SaaS and AWS Data Exchange and 20% for server products. Private offers are tiered by contract value: 3%, 2%, or 1.5%.


**Does AWS charge to list a product?** No. There is no fee for having a product available on AWS Marketplace. The listing fee applies only when a transaction happens.


**How much does a CPPO deal cost?** Channel Partner Private Offer products carry a 0.5% uplift on the listing fee regardless of offer type or deployment method. The fee is calculated on the discounted price the ISV offers the channel partner.


**What is the AWS Marketplace fee for professional services?** All professional services offerings have a 0.5% listing fee for private offers, reduced from 2.5% on 16 June 2026. Existing offers and subscriptions keep their original terms.


**Are renewals cheaper on AWS Marketplace?** Yes. All private offer renewals are charged at 1.5%, regardless of the tier the original contract fell into.


**Are there extra fees by country?** Yes, and they’re additive. AWS publishes an additional 1% regional listing fee for South Korea, effective 1 April 2025, on top of the standard rate.


---


## **Takeaways**


- There is no single marketplace fee. Deployment method, offer type, contract value, channel, and buyer region each move it.
- Deployment method is the biggest lever: 3% for SaaS against 20% for server products on the same revenue.
- Tiers reward consolidation. One large private offer can cost materially less than several smaller ones for identical revenue.
- Renewals are 1.5%, so marketplace revenue gets cheaper as it ages.
- The professional services rate is now 0.5%. If services stayed off the marketplace on fee grounds, revisit that decision.
- Model the discount, the channel margin, and the disbursement lag alongside the fee. They are usually larger than it is.


---


Fees are the visible cost; reconciliation is the expensive one. See how[marketplace billing and metering in Suger](https://www.suger.io/platform/billing-metering/) ties every transaction, fee, and disbursement back to the agreement it came from.
