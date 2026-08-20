---
schema_version: "1.0.0"
document_id: "db737b6accd3641de8498cd6c11c20bb4f3ef0f16a03c082c36cdfdc75607f61"
company_key: "yc-razorpay"
company: "Razorpay"
source_id: "yc-razorpay-rss-1480baa13d4e"
canonical_url: "https://razorpay.com/blog/razorpay-shopify-payment-gateway-pricing-explained/"
published_at: "2026-07-09T05:35:13+00:00"
first_seen_at: "2026-07-20T23:24:06.804042+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:fc6daac8481078155367d549f888b6deaaa38a5055c4f0ea4ce524f75118f367"
---

# Razorpay Shopify Payment Gateway Pricing Explained – Low Cost Shopify Payment Option in 2026

Indian Shopify merchants asking “what does Razorpay cost” are asking the wrong question. The headline transaction rate (TDR) is only one line in a longer bill. The real number is total cost of ownership (TCO): the transaction rate plus annual maintenance charges (AMC), setup fees, and revenue lost to failed payments. A shopify payment gateway advertising a lower TDR but charging Rs. 4,999/year in AMC and running an 85% success rate can cost more in absolute rupees than one charging a fractionally higher rate with zero AMC. This guide breaks down[Razorpay payment gateway pricing](https://razorpay.com/blog/razorpay-payment-gateway-pricing-explained/) for Shopify in 2026, the extra fees Shopify stacks on top in India, the GST layer, and the break-even math that decides which gateway keeps more of your money.


### Key Takeaways


- **Razorpay standard domestic pricing is 2% plus 18% GST** , a 2.36% effective rate on common domestic payment methods.
- **Razorpay charges zero setup fee and zero annual maintenance charge** under standard pricing, unlike gateways that levy Rs. 3,600 to Rs. 4,999 per year regardless of volume.
- **Shopify adds a third-party transaction fee** of 2% (Basic), 1% (Grow), or 0.6% (Advanced) because Shopify Payments is not available in India.
- **On a Rs. 1,000 order on Shopify Basic** , total deductions reach Rs. 43.60: Rs. 20 to Shopify, Rs. 20 to Razorpay, and Rs. 3.60 GST.
- **The TCO break-even sits near Rs. 2.08L/month GMV** – below that, zero AMC beats any lower TDR in absolute rupees.
- **Payment success rate is the silent cost lever** – an 8-point gap at Rs. 2L GMV equals Rs. 16,000 in monthly recovered revenue.


## What does Razorpay charge on Shopify in India?


Razorpay applies a usage-based pricing model on Shopify. You pay only when a transaction succeeds, with no charge for a failed or abandoned payment. Under standard pricing, Razorpay charges no setup fee and no annual maintenance charge, removing the fixed overhead that some gateways bill whether you process Rs. 1 or Rs. 1 crore.


The standard domestic rate is 2% plus 18% GST. Premium methods cost more. Here is the rate card by payment type.


Payment type Rate (before GST) Effective rate (incl. 18% GST)


Domestic cards, UPI, netbanking, wallets 2% 2.36%


Premium methods (EMI, corporate cards, Amex/Diners, Pay Later) 3% 3.54%


International cards 3% 3.54%


International bank transfers 1% 1.18%


Standard pricing differs from negotiated enterprise pricing. For merchants processing above Rs. 5L/month in GMV, Razorpay offers custom pricing that is not published on the pricing page and requires a sales conversation.


### Did You Know?


A gateway charging Rs. 4,999 in annual AMC adds Rs. 416 to monthly operating costs – enough to erase the TDR advantage of a 0.25 point lower rate at Rs. 2 lakh monthly GMV.


## Why do Shopify stores in India pay more than stores in many other countries?


The cost problem for Indian Shopify merchants is structural. It has nothing to do with which gateway you choose and everything to do with how Shopify treats India.


### Shopify Payments is not available in India


Shopify Payments is not among the supported countries that include India. Where Shopify Payments operates, merchants pay a single processing fee. In India, every store owner must integrate a third-party gateway to accept online payments. There is no first-party fallback.


### Shopify adds third-party transaction fees on top of your gateway charges


Because you use an external provider,[Shopify applies a third-party transaction fee](https://help.shopify.com/en/manual/your-account/manage-billing/billing-charges/types-of-charges/third-party-charges/third-party-transaction-fees) on top of what your gateway already charges. This is the “double tax” that makes India-specific math essential. The fee falls as you move up tiers: 2% on Basic, 1% on Grow, and 0.6% on Advanced. This surcharge is separate from your gateway’s MDR and the GST on it. For a deeper breakdown, see the[Shopify payment gateway cost in India](https://razorpay.com/blog/shopify-payment-gateway-cost-india/) guide.


### Manual payment methods like COD are treated differently


Not every order triggers the surcharge. Manual payment methods, including cash on delivery, are exempt from Shopify third-party transaction fees. POS orders, bank deposits, cheques, and test orders are also excluded. This makes COD a cost lever most merchants underuse, though it carries its own return-to-origin risk.


## What is the real effective rate after Shopify fee plus Razorpay fee plus GST?


Headline rates mislead. The number that hits your bank account is the sum of Shopify’s surcharge, Razorpay’s MDR, and GST on that MDR. Here is the actual math.


### Example on a Rs. 1,000 order


On Shopify Basic, a standard domestic order of Rs. 1,000 breaks down as: Rs. 20 to Shopify (2% surcharge), Rs. 20 to Razorpay (2% MDR), and Rs. 3.60 GST on Razorpay’s fee. That is Rs. 43.60 total, an effective rate of 4.36% on a payment where the sticker rate looked like 2%.


### Effective cost on Basic vs Grow vs Advanced


The stacked rate falls sharply as you upgrade plans, because the surcharge drops.


Shopify plan Shopify surcharge Razorpay effective (2% + GST) Total effective rate


Basic 2% 2.36% 4.36%


Grow 1% 2.36% 3.36%


Advanced 0.6% 2.36% 2.96%


### Example on Rs. 10 lakh monthly sales


At Rs. 10L/month in standard domestic sales on Basic, the 4.36% rate deducts roughly Rs. 43,600 monthly before COD share. On Advanced, the same volume costs about Rs. 29,600 monthly – a Rs. 14,000 difference driven entirely by the surcharge tier.


### Example on Rs. 50 lakh annual sales


Across Rs. 50L in annual GMV, moving from Basic to Advanced saves roughly Rs. 70,000 in surcharge alone, before the plan cost difference is subtracted. This is why plan choice, not just gateway choice, determines your low cost Shopify payment option in 2026.


### Did You Know?


GST at 18% turns Razorpay’s 2% domestic rate into 2.36% and its 3% premium rate into 3.54% – every gateway fee in India attracts this 18% GST, so no provider escapes it.


## Is Razorpay actually a low-cost Shopify payment option?


This is where TCO replaces TDR as the correct lens. Razorpay’s standard domestic TDR of 2% is not the lowest advertised rate in the market. Some gateways advertise 1.7% to 1.75%. But TDR is a starting point, not a conclusion.


### When the cheapest-looking gateway is not actually the cheapest


Consider two fixed costs the sticker rate hides. First, AMC. Some gateways charge Rs. 3,600/year (Rs. 300/month) and others Rs. 4,999/year (Rs. 417/month) in annual maintenance, billed whether you process a rupee or a crore. Razorpay’s standard pricing carries zero AMC and zero setup fee. Second, success rate. A gateway running an 80% success rate loses one in five attempted transactions, and the merchant bears that lost revenue in full.


Here is the break-even. **Merchants processing under approximately Rs. 2.08L/month in GMV pay less with Razorpay in absolute rupees, even when a competing gateway advertises a lower TDR, because zero AMC outweighs the rate gap at that volume.** Most Indian SMBs and early-stage D2C brands operate below this threshold.


### Why total cost of ownership matters more than headline MDR


The revenue lost to failed transactions usually outweighs minor savings from a fractionally lower TDR. A gateway with an 85% success rate and a Rs. 4,999 AMC can cost a sub-Rs. 2L merchant more per year than one at 2% with no fixed fees. For the full framework, see the[low cost payment gateway in India](https://razorpay.com/blog/low-cost-payment-gateway-in-india-the-complete-decision-guide/) decision guide.


### Did You Know?


A gateway with a 93% success rate retains roughly Rs. 16,000 more per Rs. 2L in GMV than one at 85% – revenue that never appears on any pricing page comparison.


## How can you reduce your Shopify payment costs in 2026?


Four levers move the blended cost more than chasing a lower headline rate.


- **Push more UPI transactions.** UPI processed 23.2 billion transactions worth Rs. 29.9 trillion in May 2026, and it is the world’s largest real-time payments platform. UPI generally carries lower MDR than credit cards, so a higher UPI share lowers your blended rate. See[UPI charges explained](https://razorpay.com/blog/upi-charges-explained-mdr-vs-platform-fees/) and the[UPI payment gateway for India](https://razorpay.com/upi-payment-gateway-india/) page.
- **Choose the right plan for your volume.** Run the break-even: annual plan cost difference versus annual surcharge savings.
- **Use COD strategically.** COD orders skip Shopify’s third-party fee entirely. Balance this against return-to-origin cost.
- **Reduce failed transactions.** Success rate is a revenue lever, not a cost line. Fewer failures means more captured GMV at the same TDR.


## Which Razorpay features affect ROI beyond price?


The argument for Razorpay is not lowest per-transaction rate. It is that zero AMC and higher payment success rates combine to put more money in the merchant’s bank account. Based on optimised routing, Razorpay reports payment success rates of up to 95%, and India’s average transaction success rate sits at 85 to 88% per the PwC India Payments Handbook 2024, meaning a higher success rate directly recovers revenue that a lower-rate but lower-success gateway loses.


Feature What it does Why it affects TCO


Zero AMC and zero setup fee No fixed annual or upfront charge under standard pricing Removes overhead that some gateways bill at Rs. 3,600 to Rs. 4,999/year


100+ payment methods, 180+ currencies Domestic and international acceptance built in[Supports Shopify payments in India and globally](https://razorpay.com/shopify-payment-gateway/)


Success rates up to 95% Higher capture on attempted transactions Recovers revenue lost to failures elsewhere


Custom pricing above Rs. 5L/month GMV Negotiated rate available on request Removes the 2% TDR ceiling at scale


Automated reconciliation Daily reconciliation without manual effort Reduces operational cost, not just transaction cost


[Magic Checkout and COD tools](https://razorpay.com/docs/payments/magic-checkout/shopify/) One-click checkout, COD-to-prepaid conversion Case data: Borosil reported 70% higher prepaid share and 36.36% lower RTO


### Did You Know?


With[Razorpay COD and Magic Checkout](https://apps.shopify.com/razorpay-checkout) , merchants can prompt COD shoppers to convert to prepaid post-order using incentives, reducing return-to-origin losses.


## How do you set up Razorpay on Shopify?


### Documents you need


Before processing a live transaction, RBI rules require identity and business verification. You will need a PAN card, GSTIN certificate, business registration certificate, a verified business bank account, and proof of address. Any gateway handling Indian payments must also hold PCI-DSS certification.


### Integration steps


Setup is done through the Razorpay Shopify integration steps. You can also follow[how to activate a payment gateway for Shopify](https://razorpay.com/blog/how-to-activate-payment-gateway-for-shopify/) and[how to add a payment gateway in Shopify](https://razorpay.com/blog/how-to-add-payment-gateway-in-shopify/) for a no-code walkthrough. Onboarding uses fully digital KYC.


### Common activation issues


For activation errors and India-specific problems, refer to the[Shopify payment gateway FAQs](https://razorpay.com/docs/payments/payment-gateway/ecommerce-plugins/shopify/faqs/?preferred-country=IN) and the guide to[fix Shopify checkout issues in India](https://razorpay.com/blog/shopify-checkout-issues-india-fixes/) .


## FAQ


### What does Razorpay charge on Shopify in India?


Razorpay charges 2% plus 18% GST on standard domestic methods, a 2.36% effective rate. Premium methods like EMI and corporate cards are 3% plus GST. There is no setup fee and no annual maintenance charge under standard pricing.


### Does Shopify charge extra on top of Razorpay?


Yes. Because Shopify Payments is not available in India, Shopify applies a third-party transaction fee of 2% on Basic, 1% on Grow, and 0.6% on Advanced, added on top of Razorpay’s MDR and its GST.


### Does GST apply to payment gateway fees?


Yes. All gateway fees in India attract 18% GST. This turns Razorpay’s 2% domestic rate into 2.36% and its 3% premium rate into 3.54%.


### Does Shopify charge third-party fees on COD?


No. Manual payment methods, including cash on delivery, bank deposits, and cheques, are exempt. Only online payments processed through a third-party gateway trigger the surcharge.


### Is Razorpay cheaper on Grow or Advanced than Basic?


The effective rate is lower on higher plans because the surcharge falls. A standard domestic order costs 4.36% on Basic, 3.36% on Grow, and 2.96% on Advanced. Whether upgrading pays off depends on your monthly volume versus the plan cost difference.


### Can Razorpay accept international payments on Shopify?


Yes. Razorpay supports international payments through its Shopify gateway across 180+ currencies. International cards are 3% plus GST and international bank transfers 1% plus GST.


### Are Razorpay charges billed through Shopify or separately?


For the Razorpay Secure Shopify listing, external charges may be billed by Razorpay separately from your Shopify invoice, in USD. Your Shopify subscription and third-party surcharge appear on your Shopify bill.
