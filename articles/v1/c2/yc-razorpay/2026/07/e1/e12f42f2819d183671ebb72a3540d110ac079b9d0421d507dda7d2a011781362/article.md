---
schema_version: "1.0.0"
document_id: "e12f42f2819d183671ebb72a3540d110ac079b9d0421d507dda7d2a011781362"
company_key: "yc-razorpay"
company: "Razorpay"
source_id: "yc-razorpay-rss-1480baa13d4e"
canonical_url: "https://razorpay.com/blog/upi-mdr-for-merchants-in-payment-gateway-explained/"
published_at: "2026-07-09T05:14:48+00:00"
first_seen_at: "2026-07-20T23:24:06.804042+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:062f931f5c3e77f6647d462d5e2f8e1a3a46ea00d98536ec771d946fe29cd4f3"
---

# Understanding UPI MDR For Merchants: UPI MDR In Payment Gateway Explained

Every Indian merchant asks the same question when a settlement report lands short: “UPI is free, so why is my payment gateway charging me?” Standard bank-to-bank UPI carries zero MDR by law, yet your payment gateway can still charge a platform fee, and specific UPI-linked instruments still attract charges. This article decodes UPI MDR for merchants and reframes the cost conversation around total cost of ownership rather than the headline rate.


### Key Takeaways


- Standard bank-to-bank UPI (P2M) has carried zero MDR since January 2020.
- Zero MDR applies to the network layer, not the payment gateway layer. A platform fee is not MDR.
- RuPay credit card on UPI and wallet-funded UPI above Rs 2,000 can attract charges, but 99.9% of UPI transactions are not via PPIs.
- Total cost of ownership beats headline MDR: at roughly Rs 2.08L monthly GMV, a payment gateway with zero AMC outweighs a competitor’s lower transaction rate.
- As of July 2026, MDR-for-large-merchants remains a proposal, not enacted law.


## Is There MDR On UPI In 2026? The Short Answer


For standard UPI payments funded directly from a customer’s bank account, MDR is zero by government mandate, and has been since January 2020.


**Standard bank-to-bank UPI:** When a customer scans a QR code or pays through a UPI app from their bank account (P2M), the network MDR is 0%.


**RuPay credit card on UPI:** When the underlying instrument is a RuPay credit card, MDR can apply, particularly above the Rs 2,000 threshold.


**PPI or wallet-funded UPI:** When funded by a prepaid payment instrument above Rs 2,000, an interchange fee can apply. This affects a tiny fraction of volume, since[99.9% of UPI transactions are not done through PPIs](https://www.medianama.com/2023/04/223-explained-upi-interchange-fee-2/) .


### Did You Know?


UPI grew from just[2 crore transactions in FY 2016-17 to over 24,162 crore transactions in FY 2025-26](https://www.pib.gov.in/PressReleasePage.aspx?PRID=2257087&reg=3&lang=1) .


## What Zero MDR On UPI Actually Means


Zero MDR on UPI is a specific legal treatment, not a promise that accepting UPI costs a merchant nothing.


### The Legal Basis Since January 2020


MDR was made zero for RuPay debit card and BHIM-UPI payments through amendments in Section 10A of the Payments and Settlement Systems Act, 2007 and Section 269SU of the Income-tax Act, 1961. The government’s[cashless India framework](https://www.pib.gov.in/PressReleasePage.aspx?PRID=2114335&reg=48&lang=2) established this rule.


### What UPI MDR Used To Be Before Zero-MDR


Before the mandate, MDR of up to 0.30% applied for UPI P2M transactions. This historical[analysis of charges levied on digital payments](https://www.pwc.in/industries/financial-services/fintech/payments/analysis-of-charges-levied-on-digital-payments.html) matters because the 0.30% figure resurfaces in the current policy debate.


## Why Your Payment Gateway Still Charges You On “Free UPI”


Zero MDR applies to the network layer. Your payment gateway operates at a separate layer and charges a platform fee for the infrastructure it provides.


### MDR vs Platform Fee vs Interchange vs GST


Charge type What it covers Who receives it Applies to standard UPI?


Network MDR Moving money through the UPI rail Banks and NPCI No (0% by mandate)


Platform fee Dashboard, reporting, reconciliation, security Payment gateway Yes, may apply


Interchange Cost of PPI or credit-funded instruments Issuing bank Only above Rs 2,000


GST Tax on the service fee Government On the fee, not the transaction


The[difference between MDR and platform fees](https://razorpay.com/blog/upi-charges-explained-mdr-vs-platform-fees/) is the most misunderstood point in UPI MDR for merchants. A platform fee is a technology service fee, not a network charge.


### Why Zero Network MDR Does Not Mean Zero Acceptance Cost


A payment gateway advertising “free UPI” may recover costs through settlement delays, bundled plans, or added fees. A full breakdown of[convenience fee, TDR, MDR, platform fee, AMC and setup fee](https://razorpay.com/blog/convenience-fee-tdr-mdr-platform-fee-amc-setup-fee-technology-fee-of-payment-gateway/) shows how many line items sit beneath a headline rate. Some payment gateways charge Rs 4,999 per year in AMC, adding Rs 416 monthly. At Rs 2 lakh monthly GMV, that overhead erases a 0.25 point lower transaction rate. A payment gateway with zero AMC and zero setup fee, such as[Razorpay’s pricing structure](https://razorpay.com/blog/razorpay-payment-gateway-pricing-explained/) , avoids this drag.


## When UPI Transactions Are Not Free For Merchants


There are specific exceptions where UPI-linked payments carry a charge. Both hinge on the underlying instrument and the Rs 2,000 threshold.


### RuPay Credit Card On UPI Above Rs 2,000


A[Canara Bank merchant notice](https://www.canarabank.bank.in/documents/d/guest/mdr-on-rupay-credit-card-on-upi-payments) states that with effect from 01 June 2026, MDR applies on RuPay credit card payments via UPI exceeding Rs 2,000. The rate typically ranges from around 1.1% to 2%. If a customer pays Rs 5,000 at 1.1% MDR, the merchant incurs Rs 55 and receives Rs 4,945. Only the merchant bears this. See the[latest innovations on Turbo UPI, credit cards on UPI and more](https://razorpay.com/blog/latest-innovations-on-turbo-upi-credit-cards-on-upi-and-more/) .


### PPI Or Wallet-Backed UPI Above Rs 2,000


An interchange fee of up to 1.1% can apply on UPI transactions exceeding Rs 2,000 when funded through PPIs. This affects a negligible share of volume.


### Why These Are Exceptions, Not The Rule


For a broader view of[UPI transaction charges](https://razorpay.com/learn/upi-transaction-charges/) , the rule is zero and the exceptions are narrow.


### Did You Know?


In 2025, UPI processed[228.3 billion transactions worth Rs 299.7 lakh crore](https://www.pib.gov.in/PressReleasePage.aspx?PRID=2257087&reg=3&lang=1) .


## How Charges Appear In A Payment Gateway Settlement Report


Scenario Gross amount Network MDR Interchange What lands (illustrative)


Bank-account UPI Rs 1,000 Rs 0 Rs 0 Rs 1,000 minus platform fee + GST


RuPay credit card on UPI Rs 5,000 Rs 55 In MDR Rs 4,945 minus platform fee + GST


Wallet-funded UPI above Rs 2,000 Rs 3,000 Rs 0 Up to Rs 33 Rs 3,000 minus interchange minus fee


Identify each deduction by type. Understanding[refunds and MDR in payment gateways](https://razorpay.com/blog/refunds-and-mdr-in-payment-gateways/) prevents reconciliation surprises.


## Small Merchants Versus Large Merchants: What Changes


The Union Cabinet approved an incentive scheme for low-value BHIM-UPI P2M transactions for FY 2024-25 with an outlay of Rs 1,500 crore, covering P2M transactions up to Rs 2,000 at 0.15% per transaction. Large merchants receive zero MDR but no incentive, which makes them the focus of every free-payments cost model proposal.


## Will MDR Return On UPI For Merchants?


As of July 2026, standard UPI P2M remains zero MDR. In 2025 the Finance Ministry rejected claims of an impending UPI MDR, calling the speculation false, baseless, and misleading. A Parliamentary Standing Committee report tabled in March 2026 called for a return of MDR on large merchants, and the Payments Council of India has urged a 0.30% MDR on large-merchant UPI and RuPay debit transactions. No binding RBI or CBDT notification exists.


## How To Choose A Payment Gateway For UPI Beyond Just The Fee


Total cost of ownership, factoring in AMC, setup fees, and payment success rates, is the decisive metric.


### Transparent Pricing Versus Hidden Recovery Models


A lower advertised rate often gets recovered through fixed overheads. Some payment gateways charge Rs 4,999 per year, adding Rs 416 per month regardless of volume. Understanding[how payment gateway pricing eats into margins](https://razorpay.com/blog/payment-gateway-pricing-impact-on-margins/) reframes the comparison from rate to rupees.


### Setup Fee, AMC, Settlement Delay, Monthly Minimums


At roughly Rs 2.08L monthly GMV, a payment gateway with zero AMC outweighs a competitor’s lower rate in absolute rupees. The[low-cost payment gateway decision guide](https://razorpay.com/blog/low-cost-payment-gateway-in-india-the-complete-decision-guide/) walks through these tiers.


### Why Success Rate Can Matter More Than The Headline Fee


An 8 point success rate difference (93% versus 85%) at Rs 2L monthly GMV equals Rs 16,000 in recovered revenue monthly.[UPI success rate boosters](https://razorpay.com/blog/upi-success-rate-boosters/) and[Turbo UPI](https://razorpay.com/blog/turbo-upi/) close this gap.


## Razorpay Payment Gateway: The Highest ROI On Total Cost Of Ownership


The argument for[Razorpay’s UPI for business](https://razorpay.com/upi/) is that when AMC and success rate are factored in alongside the transaction rate, more money reaches the merchant’s bank account.


Factor Razorpay position Rupee impact for merchant


Annual maintenance charge Zero AMC Saves up to Rs 4,999 per year


Setup fee Zero setup fee No upfront onboarding cost


Payment success rate Up to 95% across scenarios Recovers up to Rs 16,000 per month at Rs 2L GMV


Turbo UPI Two-step in-app payment Reduces third-party app dependency


Dynamic routing Routes for higher success More captured revenue


Custom pricing Above Rs 5L monthly GMV Removes the rate ceiling for scaling merchants


[Razorpay charges zero annual maintenance](https://razorpay.com/learn/what-is-mdr-psp-fee-switching-fee-interchange-fee/) and zero setup fee, and a higher success rate retains more revenue independent of the advertised fee.


## Frequently Asked Questions


### Is UPI really free for merchants?


Standard bank-to-bank UPI (P2M) carries zero network MDR since January 2020. However, a merchant may still pay a platform fee, and RuPay credit card or PPI-funded UPI above Rs 2,000 can attract charges.


### Why is my payment gateway charging me if UPI MDR is zero?


Zero MDR applies to the network layer, not the payment gateway layer. The fee is a platform or technology service fee covering the dashboard, reconciliation, and security.


### Does the customer ever pay MDR on UPI?


No. For RuPay credit card on UPI above Rs 2,000, MDR is charged only to the merchant.


### What is the MDR on a RuPay credit card via UPI?


For payments above Rs 2,000, MDR typically ranges from around 1.1% to 2%, varying by merchant category and issuer.


### Are wallet UPI payments chargeable for merchants?


An interchange fee of up to 1.1% can apply above Rs 2,000, but 99.9% of UPI transactions are not routed through PPIs.


### Will UPI MDR return for large merchants?


As of July 2026, this is a proposal, not law. Small merchants would remain exempt under every current proposal.


### Is a platform fee the same as MDR?


No. MDR is paid to banks and NPCI. A platform fee is charged by the payment gateway for its technology and services.


## Final Takeaway


UPI can be legally zero-MDR at the network layer and still cost a merchant money at the payment gateway layer. Understanding UPI MDR for merchants means separating the zero network MDR on standard UPI, the instrument-specific charges above Rs 2,000, and the platform fee. A lower advertised rate loses its edge once a Rs 4,999 AMC or an 8 point lower success rate enters the math. Measure by rupees that reach your bank account, not by the number on the fee schedule.
