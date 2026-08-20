---
schema_version: "1.0.0"
document_id: "c81c67ec49a40cdc7b5231606a3825402203f32b2e65d0c557384e2bbd70a9f9"
company_key: "yc-razorpay"
company: "Razorpay"
source_id: "yc-razorpay-rss-1480baa13d4e"
canonical_url: "https://razorpay.com/blog/debit-card-mdr-in-payment-gateway-explained/"
published_at: "2026-07-08T08:27:16+00:00"
first_seen_at: "2026-07-20T23:24:06.804042+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:dab70cfb08b21b82caa0fd2bbeaf95b9006bcc8648e2832fafd9c797031ce49b"
---

# Understanding Debit Card MDR For Merchants: Debit Card MDR In Payment Gateway Explained

Merchant Discount Rate (MDR) is the percentage fee a merchant pays to accept a card payment. But headline MDR is the wrong number to optimise. What actually determines how much stays in your bank account is Total Cost of Ownership: MDR plus annual maintenance charges, setup fees, and the payment success rate of your gateway.


Two facts reframe debit card MDR in India immediately. First, not all debit MDR is equal – RuPay debit carries zero MDR by statute, while non-RuPay debit still falls under RBI caps. Second, a zero-MDR instrument can still show a fee if your payment gateway applies a flat platform charge. This guide explains the rules, the leakage, and how to calculate the real cost.


### Key Takeaways


- **RuPay debit and UPI carry zero MDR by law** since 1 January 2020, but many payment gateways still apply a flat platform fee on that volume.
- **RBI caps non-RuPay debit MDR** at 0.4% for merchants with turnover up to Rs 20 lakh and 0.9% above that, subject to a maximum of Rs 1,000 per transaction.
- **Total Cost of Ownership beats headline MDR.** A gateway charging Rs 4,999/year in AMC costs Rs 416/month regardless of volume, which can erase a lower TDR advantage.
- **The break-even point is roughly Rs 2.08 lakh/month GMV** – below it, a zero-AMC gateway costs less in absolute rupees even against a lower advertised TDR.
- **Payment success rate is a silent revenue driver.** An 8-percentage-point gap at Rs 2 lakh GMV equals Rs 16,000 in monthly revenue kept or lost.


## What Is Debit Card MDR?


Debit card MDR is the fee a merchant pays for accepting a debit card payment, expressed as a percentage of the transaction value. Every time a customer taps, swipes, or enters debit card details online, a slice of that transaction is deducted before the money settles into the merchant account.


The merchant does not pay MDR directly to a single party. It is a bundled charge distributed across the banks and networks that process the payment. For most Indian businesses, MDR shows up as a line item in the settlement report from their payment gateway, and it is one of the recurring costs of accepting digital payments.


Merchants should care because MDR compounds. On thin retail margins, a fraction of a percent across thousands of transactions is a material cost. But as this guide will show, MDR alone is an incomplete picture. Fixed fees such as annual maintenance charges and the payment success rate of your gateway often move the real cost more than the MDR percentage itself.


### Did You Know?


RuPay debit card transactions carry zero MDR by law under Section 10A of the Payment and Settlement Systems Act, 2007, effective 1 January 2020, yet many merchants still see a platform fee deducted on that same volume.


### MDR vs TDR vs Interchange Fee


Indian merchants encounter three overlapping terms. Here is the distinction.


Term What it means Scope


MDR (Merchant Discount Rate) The complete merchant-facing fee for processing a card transaction Total fee, includes all components


TDR (Transaction Discount Rate) The discount rate applied to transaction value Used interchangeably with MDR in practice


Interchange Fee The charge paid to the card-issuing bank One component inside MDR


Interchange is a charge for processing card transactions between banks, whereas MDR is the complete charge merchants pay, including interchange, payment processor fees, and card network fees. When you see TDR on a pricing page, treat it as the same merchant-facing percentage as MDR. For a deeper breakdown, see our guide on interchange fee vs MDR.


## Who Gets The Fee In A Debit Card Transaction?


MDR is split among four parties. Understanding this split explains why merchants cannot simply demand zero fees and why the number varies by card network and payment channel.


The core formula is straightforward: **MDR = Interchange + Assessment + Markup.**


### Issuing Bank


The issuing bank is the customer’s bank that issued the debit card. It receives the interchange fee, the largest single component of MDR. This compensates the issuer for funding the transaction and bearing fraud and operational risk.


### Acquiring Bank


The acquiring bank is the merchant’s bank that receives the settled funds. It takes a portion of the MDR for facilitating the merchant side of the transaction and managing settlement.


### Card Network


The card network – RuPay, Visa, or Mastercard – charges an assessment or network fee for routing the transaction across its infrastructure. The network fee is typically the smallest of the three bank-side components.


### Payment Gateway Or Aggregator


The payment gateway or aggregator adds a markup, which can be a flat fee, a percentage, or both. This is the component merchants have the most control over, because it is set commercially rather than by regulation. When comparing options, understanding payment gateway vs payment processor roles helps clarify who charges what.


### Did You Know?


On a Rs 1,000 transaction at 2% MDR, the merchant receives Rs 980 – the remaining Rs 20 is split across the issuing bank, acquiring bank, card network, and payment gateway, not pocketed by any single party.


## What Are The RBI Rules On Debit Card MDR In India?


The Reserve Bank of India rationalised debit card MDR in December 2017 to promote wider acceptance, especially among small merchants. RBI categorised merchants by annual turnover and set differentiated caps by acceptance mode. These caps still define the ceiling for non-RuPay debit MDR in 2026.


### Merchants With Turnover Up To Rs 20 Lakh


For merchants with annual turnover up to Rs 20 lakh, MDR is capped at 0.4% for physical POS transactions and 0.3% for QR-code based transactions. Banks cannot charge more than 0.4% for transactions up to a maximum of Rs 200 per transaction, and for QR-code payments the cap is 0.3% up to a maximum of Rs 200.


### Merchants With Turnover Above Rs 20 Lakh


For merchants whose annual turnover exceeds Rs 20 lakh, MDR is capped at 0.9% for swipe machine transactions and 0.8% for QR-code based sales, subject to a maximum of Rs 1,000 per transaction. This is the tier most growing businesses fall into.


### POS, QR, And Online Caps


The rules vary by acceptance mode. Here is the consolidated regulatory summary.


Merchant turnover POS / online cap QR-code cap Per-transaction ceiling


Up to Rs 20 lakh 0.4% 0.3% Rs 200


Above Rs 20 lakh 0.9% 0.8% Rs 1,000


These figures come from the[RBI notification on MDR for debit card transactions](https://www.rbi.org.in/commonman/English/scripts/Notification.aspx?Id=2620) . These caps apply to non-RuPay debit cards. RuPay debit is treated separately, as the next section explains. For a related channel breakdown, see QR code MDR for merchants.


## Is There MDR On RuPay Debit Cards?


No. RuPay debit cards carry zero MDR by statute. This is the single most important distinction most generic MDR explainers miss, and it directly affects what an Indian merchant actually pays.


### What Zero-MDR Means In Law


The removal of MDR on RuPay debit was implemented through amendments to the Payment and Settlement Systems Act, 2007 and the Finance Act, 2019. Section 10A of the PSS Act provides that no bank or system provider shall impose a charge on a payer or beneficiary for payments made through prescribed electronic modes. Those prescribed modes – UPI and RuPay debit cards – were specified by Rule 119AA to the Income Tax Rules, with the zero-MDR rule effective 1 January 2020.


To keep this operationally viable, the government reimburses banks. The incentive scheme reimburses processing[typically at the rate of 0.15% per transaction for transactions up to Rs 2,000 for small merchants](https://www.pib.gov.in/PressReleasePage.aspx?PRID=2114335&reg=48&lang=2) .


### RuPay Debit vs Visa/Mastercard Debit


Merchants must not treat debit card MDR as a single bucket. The instrument determines the statutory rate.


Instrument Statutory MDR Typical merchant reality What to audit


RuPay debit Zero MDR Possible platform or software fee Settlement labelling


Visa / Mastercard debit MDR within RBI cap Payment gateway blended rate Effective rate vs cap


UPI Zero MDR Possible platform or software fee UPI fee treatment


Credit card Commercial MDR Usually the highest rate Separate pricing needed


The Payments Council of India has cited existing structures of[approximately 0.9% for non-RuPay debit cards and approximately 2% for credit cards](https://www.business-standard.com/industry/news/payments-body-pci-urges-pmo-to-levy-mdr-on-upi-rupay-debit-cards-125032401010_1.html) . RuPay debit sits at zero. For merchant-specific detail, see RuPay debit card charges for merchants.


## How Debit Card MDR Works Inside A Payment Gateway


Here is where theory meets the settlement report. The statutory zero-MDR on RuPay debit and UPI does not automatically flow to the merchant. How your gateway packages fees decides what you actually pay.


### Blended Pricing vs Split Pricing


Most payment gateways charge a blanket platform fee or software fee, typically around 2%, across all payment methods. This is blended pricing – one rate regardless of instrument. The alternative is split pricing, where UPI, RuPay debit, non-RuPay debit, and credit cards each carry a separate rate. To capture the zero-MDR benefit of RuPay debit and UPI,[a merchant must negotiate split pricing](https://razorpay.com/blog/upi-charges-explained-mdr-vs-platform-fees/) .


### Why A Zero-MDR Instrument Can Still Show A Fee


UPI bank-account transactions and RuPay debit carry zero network MDR under the PSS Act. Yet the fee still appears. As one analysis documents, the leakage hides in plain sight where payment gateways apply their flat platform percentage against zero-MDR volume, mislabel the instrument, or fold a platform-fee line into something the settlement report calls MDR. A finance controller running a per-network effective-rate audit on a UPI-heavy merchant almost always finds zero-MDR volume being charged a flat percentage.


### What Merchants Should Check In Settlement Reports


Request settlement reporting broken down by instrument and network. Confirm whether a platform fee is being applied to RuPay debit and UPI volume. Calculate your effective rate per network and compare it against the statutory rate. For a structured approach, see how to audit settlement reports and our guide to payment gateway charges in India.


### Did You Know?


UPI accounted for 85.5% of total payment volumes but only 9.5% of total transaction value in the second half of 2025, which means a merchant’s fee mix is dominated by rails that should carry zero MDR – if the gateway is not quietly charging them.


## How To Calculate Debit Card MDR


MDR calculation is simple arithmetic once you separate the instrument and add GST. Below are worked examples in rupees.


### Example On A Rs 1,000 Non-RuPay Debit Transaction


Assume a merchant with turnover above Rs 20 lakh accepts a Rs 1,000 Visa debit payment at the 0.9% cap.


- MDR = 0.9% of Rs 1,000 = Rs 9
- Merchant receives Rs 1,000 – Rs 9 = Rs 991 (before GST on the fee)


### Example On A RuPay Debit Transaction


Assume the same merchant accepts a Rs 1,000 RuPay debit payment.


- Statutory MDR = zero
- Merchant should receive the full Rs 1,000
- Any deduction shown is a platform fee applied to zero-MDR volume, not lawful MDR


### GST On Payment Gateway Charges


GST at 18% applies on the service-fee component charged by the gateway. On a service fee of Rs 10,[the GST is Rs 1.80](https://razorpay.com/learn/gst-on-upi-payments/) , taking the total to Rs 11.80. Because standard UPI and RuPay P2M transactions carry no MDR, most merchants do not see GST on that volume. For the full treatment, see GST on payment gateway charges.


## What Affects The Rate A Merchant Actually Pays?


The advertised MDR is a starting point. Several variables move the real cost.


### Card-Present vs Online


Card-present POS transactions and online transactions can carry different caps and different risk-based pricing. The RBI caps distinguish physical POS, QR, and online modes, and payment gateways price accordingly.


### Merchant Turnover And Transaction Size


Your annual turnover determines which RBI cap tier applies for non-RuPay debit. Transaction size interacts with the per-transaction rupee ceiling – the Rs 200 cap for small merchants and the Rs 1,000 cap above Rs 20 lakh turnover.


### Network Mix And Payment Gateway Pricing Model


Your instrument mix matters. A merchant with mostly RuPay debit and UPI volume should pay close to zero on those rails – if the gateway uses split pricing. A blended rate flattens this benefit away.


### Add-On Charges Such As AMC, Platform Fee, Refund Fee


This is the Total Cost of Ownership layer that headline MDR hides. Some gateways charge Rs 4,999/year in annual maintenance charges. Others charge Rs 3,600/year, which adds Rs 300/month regardless of volume. There may also be setup fees and refund fees. A gateway advertising a lower TDR with a fixed AMC can cost more in absolute rupees than a zero-AMC option.


### Did You Know?


A gateway charging Rs 4,999 in annual AMC adds Rs 416 to monthly operating costs – enough to erase the advantage of a 0.25 percentage point lower TDR at Rs 2 lakh monthly GMV.


## How Merchants Can Reduce Debit Card Payment Costs


Reducing cost is not about chasing the lowest MDR. It is about optimising Total Cost of Ownership across MDR, fixed fees, and success rate.


### Negotiate Per-Instrument Pricing


Ask for split pricing. Request separate slabs for UPI, RuPay debit, non-RuPay debit, and credit cards. A single blended rate almost always overcharges a merchant with heavy zero-MDR volume.


### Separate Zero-MDR Instruments From Paid Rails


Confirm explicitly whether a platform fee is charged on zero-MDR volume. Ask the question directly in the contract: is any fee applied to RuPay debit and UPI transactions? See our companion piece on[UPI charges explained](https://razorpay.com/blog/upi-charges-explained-mdr-vs-platform-fees/) .


### Compare Total Cost Of Ownership


Model the full annual cost, not the headline rate. Include AMC, setup fee, and refund treatment. A gateway charging Rs 4,999/year in AMC adds fixed cost that a zero-AMC gateway does not. For structured comparison, see how to compare payment gateway pricing.


### Run A Monthly Effective-Rate Audit


Divide total fees charged by total volume, per network. Compare the result against the statutory rate. Any gap on RuPay debit or UPI is leakage. Reconcile refunds separately via our guide on do payment gateways return MDR on refunds.


## How Razorpay Delivers The Highest ROI On Total Cost Of Ownership


The argument for Razorpay is not the lowest per-transaction rate. It is that when you account for annual maintenance charges and payment success rate alongside TDR, Razorpay puts more money in the merchant’s bank account.


Factor Razorpay Typical alternatives


Annual maintenance charge (AMC) Rs 0 / year Rs 3,600 to Rs 4,999 / year


Setup fee Rs 0 Varies, often applicable


Payment success rate Up to 95% across various scenarios Approximately 80% to 90%


Standard TDR 2% 1.7% to 2%


Custom pricing above Rs 5 lakh/month GMV Available on request Varies


Two numbers drive the ROI case:


- **Zero AMC.** A fixed fee of Rs 4,999/year is money a merchant pays whether they process Rs 1 or Rs 1 crore. The[break-even point between a zero-AMC gateway and one with a lower TDR but Rs 4,999 AMC is approximately Rs 2.08 lakh/month in GMV](https://razorpay.com/blog/convenience-fee-tdr-mdr-platform-fee-amc-setup-fee-technology-fee-of-payment-gateway/) . Most Indian SMBs and early-stage D2C brands operate below that threshold, meaning zero AMC wins in absolute rupees.
- **Success rate.** Payment failure means the merchant bears the cost – the customer may not retry. At Rs 2 lakh/month GMV, an 8-percentage-point difference is worth roughly Rs 16,000 per month in revenue that never appears on any pricing-page comparison.


For merchants processing above Rs 5 lakh/month GMV, custom pricing is available on request, which removes the fixed 2% TDR ceiling. Combined with zero setup fee, this means no upfront risk for early-stage merchants evaluating options before they have volume.


### Did You Know?


At Rs 2 lakh/month GMV, an 8 percentage point difference in payment success rate (95% vs 87%) is worth about Rs 16,000 per month in recovered revenue.


## Is Debit Card MDR Still Relevant In 2026?


Yes, but the framing has shifted. Debit card MDR is increasingly a reconciliation and gateway-pricing problem rather than a large standalone cost centre.


### Debit Cards Are Declining, UPI Is Dominating


The scale of decline is significant. Debit card transaction volumes fell to 133.6 crore in 2025 from 408.7 crore in 2021, while transaction value dropped to Rs 4.5 lakh crore from Rs 7.4 lakh crore. RBI attributes the decline to competition from UPI, digital wallets, and rising credit card adoption. For a merchant, the fee conversation is now dominated by UPI – which should be zero MDR if the gateway is not applying a platform fee.


### Will Zero-MDR Stay?


The policy is contested. The Payments Council of India has urged the government to introduce MDR on UPI and RuPay debit cards, arguing existing allocations cover only a fraction of the annual cost of maintaining UPI. On the other side, the Centre continues to fund the zero-MDR model – Budget 2026-27 allocated Rs 2,000 crore for RuPay and low-value BHIM-UPI incentives, nearly five times the Rs 437 crore budgeted for FY26. For now, zero-MDR on RuPay debit and UPI remains in force.


## Frequently Asked Questions


### What is MDR on a debit card?


MDR on a debit card is the fee a merchant pays to accept a debit card payment, expressed as a percentage of the transaction amount. It bundles the interchange fee paid to the customer’s bank, the card network assessment fee, and the payment gateway markup. On a Rs 1,000 transaction at 2% MDR, the merchant receives Rs 980.


### Is MDR charged on RuPay debit cards?


No. RuPay debit cards carry zero MDR by law under Section 10A of the Payment and Settlement Systems Act, 2007, effective 1 January 2020. If a merchant sees a deduction on RuPay debit volume in a settlement report, it is typically a platform fee applied to zero-MDR volume, not lawful MDR.


### What is the maximum debit card MDR in India?


For non-RuPay debit cards, RBI caps MDR at 0.4% for merchants with annual turnover up to Rs 20 lakh and 0.9% above Rs 20 lakh, subject to a maximum of Rs 1,000 per transaction. QR-code based transactions carry lower caps of 0.3% and 0.8% respectively. RuPay debit is exempt at zero MDR.


### Why does my payment gateway charge on UPI or RuPay if MDR is zero?


Because most gateways apply a flat platform fee, typically around 2%, across all payment methods regardless of the statutory MDR on each instrument. The zero-MDR benefit only reaches the merchant if split pricing is negotiated. Merchants should audit settlement reports per network to identify this leakage.


### Does GST apply to payment gateway charges?


Yes. GST at 18% applies on the service-fee component charged by the gateway. On a Rs 10 service fee, GST adds Rs 1.80, taking the total to Rs 11.80. Because standard UPI and RuPay P2M transactions carry no MDR, most merchants do not see GST on that volume.


### Can merchants pass MDR to customers in India?


Merchants can levy a convenience fee when offering additional payment channels, but must comply with RBI guidelines and card network rules. Any convenience fee must be clearly disclosed to the customer before the transaction is completed. See our guide on whether merchants can pass payment charges to customers.
