---
schema_version: "1.0.0"
document_id: "a320113cc0699b2dc63a4754687592ada740fcdc952c00684b358c1b8147b8f3"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-rss-e6182ea9811a"
canonical_url: "https://decentro.tech/blog/upi-charges/"
published_at: "2026-08-05T09:39:03+00:00"
first_seen_at: "2026-08-05T09:58:34.246257+00:00"
fetched_at: "2026-08-05T09:58:35.653519+00:00"
content_hash: "sha256:f5b38f972f0a03cf7f5ec676dc77c82123836084d4ab5aca4e12632f2693ccce"
---

# UPI Charges Explained: MDR, Platform Fees & Merchant Costs

Table of Contents


**Summary**


Zero MDR on UPI doesn’t mean zero cost for merchants. Two distinct charges sit inside every UPI settlement: MDR, the network-level fee tied to instrument type, and the platform fee, charged by whichever payment gateway or PA/PG processes the transaction. This piece breaks down both, maps out how credit-linked UPI instruments (RuPay Credit Card and UPI Credit Line) change the fee structure, covers the small-merchant incentive scheme and the current MDR policy debate for large merchants, and outlines how businesses across industries should evaluate their payment stack.


##


The Scale That Makes This Worth Understanding


UPI processed[241.62 billion transactions worth Rs 314 lakh crore in FY 2025-26](https://www.thehindu.com/business/Economy/upi-hits-record-scale-24162-crore-transactions-worth-314-lakh-crore-in-fy26/article70927618.ece) , up nearly 20.6% in value year-on-year. Monthly volume has settled in the 22 to 23 billion transaction range through mid-2026, with June 2026 alone accounting for Rs 28.92 lakh crore across 22.72 billion transactions.


For a business processing any meaningful share of that volume, the difference between understanding your fee structure and guessing at it shows up directly in reconciliation, margin planning, and finance reporting. This is not a rounding-error line item anymore.


##


MDR and Platform Fee Are Not the Same Charge


**MDR (Merchant Discount Rate)** is the network-level fee paid to issuing banks, acquiring banks, and NPCI for the movement and settlement of funds. It is set by regulatory or network policy and varies by the funding instrument behind the transaction, not by which gateway processes it.


**Platform fee** is charged independently by the payment gateway or Payment Aggregator (PA/PG) handling the transaction. It covers routing, uptime, fraud checks, dashboarding, reconciliation, and API infrastructure. Two merchants using the same UPI instrument can pay different platform fees depending on their provider and negotiated volume tier.


The confusion most businesses run into is treating these as one number. They aren’t set by the same party, they don’t move together, and they show up as separate deductions on your settlement report even when your dashboard displays a single blended figure.


##


Anatomy of a UPI Fee: What’s Actually Inside It


Beyond the MDR versus platform fee split, a full UPI transaction fee (where MDR applies) breaks into four components:


**Component** **What it compensates** **Who sets it**


Interchange Issuing bank, for enabling the funding instrument and carrying any credit risk NPCI operating circular


PSP/app commission The UPI app used to initiate the transaction Defined within NPCI’s interchange framework


Acquirer fee Routing, authorization, and settlement on the merchant’s side Acquiring bank


Platform fee Gateway infrastructure: routing, fraud detection, dashboards, API access Set independently by the PA/PG


For standard bank-account UPI, interchange and acquirer fees are zero by mandate. Only the platform fee applies. For credit-linked UPI instruments, all four components can be active.


##


Standard UPI vs. Credit-Linked UPI: Two Different Cost Structures


Not all UPI payments draw from a savings account. Two credit-linked instruments now run on UPI rails, and each carries its own MDR logic.


### RuPay Credit Card on UPI


A RuPay credit card linked to a UPI ID. The customer authorises payment the same way as any UPI transaction, but the funding source is a bank-issued credit limit, not a deposit account. Because the issuing bank extends credit (typically with an interest-free window of up to 45 days) and carries default risk, a standard MDR applies, generally in the 1.1% to 2% range depending on merchant category and card variant.


### UPI Credit Line


A pre-sanctioned credit facility, approved by a bank and linked directly to a customer’s UPI ID, without a physical card. RBI’s 2023 directive permitted banks to offer this, and institutions including HDFC, ICICI, SBI, Axis, and Yes Bank have since operationalised it. NPCI’s operating circular on Credit Line MDR (OC No. 202/2024-25, effective October 16, 2024) sets a baseline rate of 1.3% for merchants outside specific category exemptions, with variation by Merchant Category Code.


**Parameter** **Standard UPI** **RuPay Credit Card on UPI** **UPI Credit Line**


MDR 0% (mandated) ~1.1% to 2% ~1.3%, varies by MCC


Platform fee Applicable Applicable, often slightly higher Applicable


Funding source Bank account Bank-issued credit limit Pre-sanctioned credit facility


Regulatory basis Zero-MDR mandate Standard card network MDR logic NPCI OC No. 202/2024-25


Merchant risk None None (risk sits with issuer) None (risk sits with issuer)


##


Worked Example: Fee Split Across Instruments


On a ₹5,000 transaction, here is how the deduction differs by instrument, assuming a typical platform fee structure:


**Instrument** **MDR** **Platform fee (approx.)** **Net to merchant (before GST on fees)**


Standard UPI (bank account) ₹0 ~2% (₹100) ₹4,900


RuPay Credit Card on UPI ~2% (₹100) ~2.15% (₹107.50) ₹4,792.50


UPI Credit Line ~1.3% (₹65) ~0.3 to 0.5% (₹15-25) ~₹4,910 to ₹4,920


The exact split between interchange, PSP commission, acquirer fee, and platform fee will vary by provider and MCC, but the pattern holds: standard UPI stays cheapest, RuPay Credit Card sits highest, and UPI Credit Line lands in between, closer to standard UPI than either card-based option.


##


The Small Merchant Incentive Scheme


Zero MDR does not mean zero support for the ecosystem processing these transactions. The government runs an incentive scheme that reimburses acquiring banks and PSPs for low-value UPI transactions:


- UPI P2M transactions up to ₹2,000 carry zero MDR and generate a 0.15% incentive on the transaction amount, paid to the ecosystem, not the merchant directly.
- Transactions above ₹2,000 remain at zero MDR but do not carry this incentive.
- The scheme has been extended year over year, with the Union Cabinet approving continued funding (Rs 1,500 crore allocated for FY 2025-26) specifically to support low-value digital transactions.


This detail matters for businesses with high transaction counts but low average order values, since it directly shapes how sustainable your acquiring bank or PA/PG finds it to process your specific transaction mix.


##


MDR for Large Merchants: What’s Actually on the Table


This is the single biggest policy shift worth tracking if you run a high-volume business, because it directly targets merchants like you, not the broader UPI ecosystem.


**Why it’s back on the agenda:** A Standing Committee on Finance report (March 2026) flagged that the zero-MDR policy, while effective at driving adoption, has created a funding gap for banks and payment operators covering infrastructure, cybersecurity, and merchant onboarding costs at scale. The committee recommended a graded MDR structure rather than continuing blanket zero-MDR indefinitely.


**What’s being proposed:** Reports through mid-2026 point to a structure that applies only to large merchants (reported turnover threshold: roughly ₹1 crore to ₹1.5 crore annually) and only on transactions above ₹2,000. Small merchants and P2P transfers stay untouched regardless of the outcome. The proposed rate itself is still unsettled. Some reports cite a cap as high as 0.5%, others point to a much smaller 5 to 7 basis points (0.05% to 0.07%), and the Payments Council of India has separately pushed for 0.30% in its own representations to the Finance Ministry. No single number has been confirmed.


**Why it’s opposed:** Critics argue that reintroducing MDR, even in a narrow form, risks unwinding years of merchant trust built around zero-cost UPI acceptance, particularly for businesses that priced their margins assuming the fee stays at zero. The Finance Ministry has publicly pushed back on earlier versions of this story when they surfaced, calling some reports speculative, which is part of why nothing has moved past the discussion stage.


**The latest update:** As of July 2026, this remains an active proposal under evaluation, not a notified rule. No implementation date has been set.


**How much of your business this would actually touch:** Transactions above ₹2,000 account for only around 4% of total P2M UPI volume; roughly 86% of merchant UPI transactions are under ₹500. Even in a large-merchant-only scenario, this policy would apply to a narrow, high-ticket slice of transaction volume rather than your everyday UPI acceptance. That’s worth keeping in perspective before assuming a blanket cost hit.


Given how unsettled this is, revisit it each quarter rather than assuming today’s zero-MDR status is permanent for high-volume acceptance, and don’t build pricing decisions around any single proposed rate until something is officially notified.


##


Matching UPI Instruments to Business Type


- **E-commerce and D2C:** Standard bank-account UPI will carry most of your volume. Success rate and checkout speed matter more here than instrument-level MDR optimization.
- **High-ticket and considered-purchase categories (electronics, travel, education, healthcare):** RuPay Credit Card and UPI Credit Line both tend to lift average order value and conversion on larger transactions, enough in many cases to offset the added MDR.
- **Subscription and recurring billing (SaaS, OTT, insurance, lending):** UPI Autopay mandates largely follow standard UPI’s zero-MDR logic, but platform-level fees for mandate management and retry logic can add up across volume. Model this at the mandate level, not the aggregate settlement level.
- **Marketplaces and platforms with both collections and payouts:** The bigger cost driver is often not MDR at all, it’s whether your PA/PG can handle collection, escrow-style holding, and payout from a single stack, or whether you’re stitching together multiple providers and absorbing reconciliation overhead as a result.


##


Managing UPI Charges: What Finance and Payments Teams Should Track


1. **Break every settlement report down by instrument type** , not just total deduction. MDR-bearing and zero-MDR transactions should never be reviewed as one blended number.
2. **Ask providers for the full fee schedule in writing** , covering MDR, platform fee, GST treatment (18% applies to platform fees), and volume-based tiering, before signing.
3. **Model UPI Credit Line and RuPay Credit Card selectively** , not universally. They earn their MDR on transaction sizes where credit availability changes buying behavior; below a certain ticket size, the added cost isn’t offset by any behavioral lift.
4. **Track policy developments on large-merchant MDR quarterly.** If your business crosses the reported turnover thresholds under discussion, this could change your unit economics within the next few budget cycles.
5. **Evaluate on total cost and reliability, not headline MDR alone.** A marginally lower fee from a provider with weaker success rates or slower settlement usually costs more in lost transactions than it saves in fees.


##


How Decentro Enables Businesses via PA and TSP


Everything covered so far, MDR, platform fees, onboarding costs, and the funding gap regulators are now trying to close, points to the same underlying problem: processing UPI at scale is expensive to build and compliance-heavy to run, whether the cost shows up as a transaction fee or as onboarding and verification overhead. Decentro addresses both sides of that problem, operating in two distinct capacities depending on what a business needs: as a licensed Payment Aggregator (PA) for fund flow and settlement, and as a Technology Service Provider (TSP) for identity, KYC, and verification infrastructure that sits upstream of the transaction itself.


### As a Payment Aggregator: Lowering the Cost of Collection and Settlement


As an RBI-licensed PA/PG, Decentro directly manages fund flow and settlement under RBI’s regulatory framework, rather than only providing a technology layer on top of someone else’s licensed infrastructure. That distinction shows up directly in cost and settlement speed.


**Case study:**[Shiprocket.](https://decentro.tech/blog/shiprocket-case-study/) India’s largest eCommerce enablement platform integrated Decentro’s UPI Payment Gateway for seller onboarding and payouts. The results: over 80% cost reduction versus traditional gateways, real-time settlements in place of the T+2/T+3 cycles typical of legacy providers, a 10X faster integration timeline, and 20+ new payment-enabled merchants going live every month. For a platform managing payouts to thousands of sellers, that combination of lower per-transaction cost and same-day settlement compounds directly into working-capital efficiency, exactly the kind of margin protection this article’s fee breakdowns point to.


### As a Technology Service Provider: Solving Onboarding Before the Transaction Happens


The Standing Committee’s funding-gap argument for large-merchant MDR partly rests on the cost of onboarding and verifying merchants and users at scale. Decentro’s TSP stack, covering KYC, DigiLocker, OCR, PAN verification, and bank account validation, addresses that cost directly for platforms that need to onboard high volumes of users or merchants before any payment is even initiated.


- **Dhan:** One of India’s leading stockbroking platforms (top 10, 7.4 lakh+ active traders) uses Decentro’s KYC and identity verification stack to strengthen onboarding compliance and fraud prevention across its trading user base.


- [Zypp Electric](https://decentro.tech/blog/zypp-case-study/) **:** India’s No.1 EV rental platform (20,000+ active riders, serving Zomato, Swiggy, Zepto, Uber, and Rapido) integrated Decentro’s DigiLocker, OCR, PAN, and Bank Account Validation APIs, cutting rider onboarding from roughly 30 minutes to under 3 minutes, while processing 98,000+ bank validations and 87,000+ PAN checks to date.


- [BookMyForex](https://decentro.tech/blog/bookmyforex-case-study/) **:** India’s largest online forex marketplace (65+ cities, USD 1.5 Bn+ processed, 6.5 lakh+ customers) cut PAN verification time from hours or days down to under 5 seconds using Decentro’s PAN and Bank Account Validation APIs, logging 172,500+ lifetime API hits since go-live.


Read together, these aren’t unrelated case studies bolted on for name recognition. They map onto the two cost centres this article has walked through: the PA layer determines what you pay per transaction and how fast that money reaches you, and the TSP layer determines what it costs to bring a user or merchant onto your platform in the first place. Businesses evaluating their payment stack usually optimize one and ignore the other. The ones managing both well are the ones with room to absorb whatever the large-merchant MDR conversation eventually lands on.


For businesses that also operate across borders, whether collecting from international customers, managing cross-border vendor payouts, or expanding beyond India, this same infrastructure-first approach extends to Decentro’s cross-border payment capabilities, giving finance teams one provider to reconcile against instead of separate domestic, international, and verification stacks.


##


Conclusion


MDR and platform fees answer two different questions. MDR reflects the funding instrument behind a transaction and whether that instrument carries credit risk. Platform fee reflects the infrastructure your provider builds to make sure the transaction succeeds, reconciles, and settles reliably. As UPI edges toward a possible large-merchant MDR and credit-linked instruments capture a growing share of volume, treating these as one blended number will only get more expensive. Businesses that track them separately, by instrument and by provider, will be the ones that adapt fastest when the policy landscape shifts.


[Let’s Connect](https://decentro.tech/signup?)


---


#


Frequently Asked Questions


**1. If UPI has zero MDR, why does my payment gateway still charge a fee?**


Zero MDR applies only to standard bank-account UPI transactions and covers the network cost of moving funds. The platform fee is separate, charged by your payment gateway or PA/PG for infrastructure, routing, fraud checks, and reconciliation, and applies regardless of MDR.


**2. What’s the difference between RuPay Credit Card on UPI and UPI Credit Line?**


RuPay Credit Card on UPI links a physical credit card to a UPI ID. UPI Credit Line is a pre-sanctioned bank credit facility linked directly to a UPI ID, with no card involved. Both carry MDR because both involve a bank extending credit, but their rates and regulatory basis differ (card-network MDR logic versus NPCI’s Credit Line circular).


**3. Will large merchants have to pay MDR on standard UPI transactions soon?**


It’s under active discussion. A Standing Committee on Finance report in March 2026 recommended a graded MDR structure, and reports suggest the government is evaluating a 5 to 7 basis point MDR on transactions above ₹2,000 for large merchants specifically. No final rule has been notified as of mid-2026, so this remains a policy proposal to track, not a confirmed charge.


**4. Do small merchants get any benefit under the zero-MDR policy?**


Yes. UPI P2M transactions up to ₹2,000 remain at zero MDR and generate a 0.15% incentive paid to the acquiring bank or PSP, funded through a government scheme that has continued into FY 2025-26. This indirectly supports better service and infrastructure investment for small-ticket merchants.
