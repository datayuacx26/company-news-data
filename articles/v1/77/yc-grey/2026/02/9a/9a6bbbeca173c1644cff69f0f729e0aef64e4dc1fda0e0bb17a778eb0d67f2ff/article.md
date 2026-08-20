---
schema_version: "1.0.0"
document_id: "9a6bbbeca173c1644cff69f0f729e0aef64e4dc1fda0e0bb17a778eb0d67f2ff"
company_key: "yc-grey"
company: "Grey"
source_id: "yc-grey-news-import-fd6b968aa507"
canonical_url: "https://grey.co/blog/best-way-to-receive-us-dollar-payments-in-india"
published_at: "2026-02-28T00:00:00+00:00"
first_seen_at: "2026-08-11T01:00:01.704888+00:00"
fetched_at: "2026-08-11T01:00:02.917549+00:00"
content_hash: "sha256:6234129b5d55ea32b71436a34fade453776060d90f99a2e4f1d8662747755776"
---

# Best way to receive US dollar payments in India

The default way Indian banks handle inbound USD is: receive the wire, convert to INR immediately at their own rate, credit your account in rupees, and deduct fees at every step. You never see the dollars. You never choose the conversion rate. You never decide when to convert. The bank decides all three, and the bank's rate is never the mid-market rate. That automatic conversion is where freelancers, consultants, and remote workers lose the most money on international payments.


The cost of this setup is not small. A 2% to 3% FX spread on every transfer, intermediary deductions on SWIFT wires, and receiving bank charges add up to lakhs lost annually for anyone earning regular USD income. This guide covers the four main ways to receive US dollar payments in India, what each one actually costs on a real invoice, the compliance requirements (purpose codes, FIRC, FEMA) that most payment guides skip entirely, and how to set up an infrastructure that gives you control over when and at what rate your dollars become rupees.


## **The four main ways to receive USD in India**


### **1. US ACH or Fedwire transfers**


ACH (Automated Clearing House) and Fedwire are US domestic transfer systems. When a US client pays you via ACH or Fedwire, the money moves within the US banking system to a US-based account. This is important because US companies strongly prefer paying vendors through their domestic payment rails. Their finance teams process ACH payments the same way they pay local contractors, which means less friction on the client side and faster approval on yours.


ACH transfers typically cost the sender nothing or a small flat fee (usually under $1). Fedwire transfers may cost the sender $15 to $30, depending on their bank. On your end, the cost depends on where the funds land. If you are receiving into a virtual USD account from a fintech provider, the provider charges a deposit fee (Grey charges 0.8%, minimum $2, maximum $10). There is no intermediary bank deduction because the payment never leaves the US banking system.


ACH is best for recurring payments from US-based clients, such as monthly retainers, project milestones, or regular consulting fees. If most of your clients are American, setting up a[virtual USD account](https://grey.co/foreign-accounts) that supports ACH and Fedwire deposits is typically the lowest-cost option for regular income.


### **2. International wire transfers (SWIFT)**


SWIFT is the global messaging network that banks use to route cross-border payments. When a client outside the US (or a US client whose bank does not support ACH to your provider) sends you a wire transfer, it travels through the SWIFT network. This is the most universally accepted method, but it is also the most expensive.


The costs stack up at multiple points. The sending bank charges a wire fee, typically $20 to $50, depending on the country and bank. If the payment routes through an intermediary (correspondent) bank, that bank deducts another $10 to $25 from the transfer amount. Your receiving bank may charge an additional inward remittance fee of $5 to $15. On a $2,000 invoice, total fees can range from $45 to $90, which is 2.25% to 4.5% before any currency conversion. Settlement takes 2 to 5 business days.


SWIFT makes sense for large, infrequent payments where the fixed fees represent a smaller percentage of the total: invoices above $5,000, corporate contracts, or one-off project payments. For smaller recurring payments, the per-transfer cost makes SWIFT significantly more expensive than ACH.


**Also read:**[Top platforms to earn US dollars in India](https://grey.co/blog/top-platforms-to-earn-us-dollars-in-india)


### **3. Payment aggregators**


Payment aggregators collect funds from your client (usually via card or local bank transfer) and remit the proceeds to your Indian bank account. The most common ones Indian freelancers encounter are PayPal, Razorpay (for businesses accepting international payments), and Stripe (with limited availability for payouts in India).


PayPal charges 2.99% on incoming goods and services payments, plus a currency conversion spread of 3% to 4% above mid-market when you withdraw to INR. On a $2,000 payment, that is roughly $60 in transaction fees plus $60 to $80 in conversion loss, totalling $120 to $140. Razorpay's international payment gateway charges 3% per transaction for international cards. These platforms are convenient for clients who prefer paying by card or who are not set up for bank transfers, but the cumulative cost is the highest of all four methods.


Aggregators are best for small, one-off projects where the client insists on paying by card, or for marketplace payouts (Upwork, Fiverr) where the platform dictates the payment method.


### **4. Cross-border transaction caps**


India's cross-border payment framework includes transaction-level limits in certain categories. Some payment setups cap individual cross-border transactions at ₹25 lakh per unit of goods or services. If your invoice size approaches that range, bank-led rails such as SWIFT are usually more suitable than aggregator platforms. Having access to a USD receiving account through a provider like Grey gives you more flexibility when handling higher-value international payments, because the funds arrive in USD first and you control the conversion timing and method.


## **How the four methods compare**


## **The compliance layer most freelancers overlook**


Receiving USD payments in India is regulated under the Foreign Exchange Management Act (FEMA) and RBI reporting guidelines. Getting the compliance right upfront prevents delays, frozen transfers, and problems during audits.


### **Purpose codes**


Indian banks require a purpose code on every inward remittance to classify the transaction under FEMA. Common codes for freelancers include those for software development, consulting, and marketing services. These codes are defined under the RBI purpose code list for inward remittances. Providing the correct code on your invoice and communicating it to your client's bank reduces the risk of your payment being held for manual review. Vague or missing purpose codes are one of the most common reasons for delayed credits.


**You may also like:**[How to receive money from UK clients in India](https://grey.co/blog/how-to-receive-money-from-uk-clients-in-india)


### **FIRC and e-FIRC**


If you export services (which includes freelancing for foreign clients), you may need a Foreign Inward Remittance Certificate (FIRC or e-FIRC) for GST export documentation, audits, and platform compliance. Only authorised dealer banks issue e-FIRC documentation. Confirm your bank's process for issuing these certificates in advance, because requesting them after the fact can take weeks. If you receive payments through a virtual USD account and then transfer to your Indian bank, the e-FIRC is issued by the Indian bank that receives the final INR credit.


‍


### **Export realisation timelines**


India requires that export proceeds be realised (received in India) within a prescribed period. Long-pending invoices can create compliance issues. Keep your documentation organised and follow up on overdue payments early. For a complete explanation of FIRC requirements, tax implications, and RBI compliance rules, see Grey's[guide on FIRC, taxes, and compliance for Indians earning foreign currency](https://grey.co/blog/firc-taxes-and-compliance-for-indians-earning-foreign-currency) .


## **Which setup is right for your situation**


The right payment method depends on three factors: where your clients are located, your average invoice size, and whether you want to hold USD or convert to INR immediately.


**For most** **clients** **in** **the** **US,** a virtual USD account that supports ACH and Fedwire is typically the lowest-cost option. Your US client pays you the same way they pay a local vendor. You receive the funds in USD, hold them until you are ready to convert, and withdraw to your Indian bank at a rate and time you choose. SWIFT works as a backup for clients whose banks do not support ACH to their provider.


**Global clients across multiple countries:** Ensure your account can receive international SWIFT wires without intermediary deductions eating into your payment. A multi-currency account lets you hold USD, GBP, or EUR and convert only when the rate is favourable, rather than accepting your bank's automatic conversion on arrival.


**Large invoices ($5,000+):** Direct bank rails (SWIFT or Fedwire) are preferable to aggregator platforms. The fixed fees are lower as a percentage of the total than the percentage-based fees aggregators charge. A $10,000 invoice through PayPal costs roughly $600-$700 in combined fees and FX losses. The same invoice received via SWIFT to a virtual USD account costs $10 in deposit fees plus the conversion cost when you move to INR.


**Priority is maximising take-home INR. Compare the total cost at every step: incoming transfer fees, deposit fees, FX margin versus the** mid-market rate, and withdrawal fees. Small differences in FX margins compound significantly over a year of regular payments. A 1% difference on $3,000 per month is $360 annually.


**Also read:**[A guide for Indians getting paid by clients abroad](https://grey.co/blog/a-guide-for-indians-getting-paid-by-clients-abroad)


## **Common mistakes to avoid**


Relying on a single payment method leaves you exposed if that channel has downtime, compliance issues, or fee changes. The safest setup is ACH or Fedwire as your primary rail with SWIFT as a fallback. Ignoring FX spreads is the most expensive mistake: the difference between mid-market and a 3% markup on $2,000 is $60 per transfer. Waiting until a compliance request arrives to organise your documentation can delay your funds for days or weeks. Sending invoices without detailed service descriptions triggers manual review at Indian banks because the purpose of the remittance is unclear. Include the service type, project reference, and your client's details on every invoice.


## **How Grey works for Indian freelancers receiving USD**


Grey provides Indian freelancers, consultants, and remote professionals with USD, GBP, and EUR[foreign currency accounts](https://grey.co/foreign-accounts) that can receive payments via ACH, Fedwire, or international SWIFT wire transfers. The setup is fully digital: sign up at grey.co or download the Grey app, complete KYC verification with your PAN card, Aadhaar, and proof of address, and your account details are available within 24 hours.


When a payment arrives via ACH or Fedwire, Grey charges a 0.8% deposit fee (minimum $2, maximum $10). The funds sit in your USD wallet. You can hold them in dollars for as long as you need, which is useful if you want to wait for a more favourable exchange rate or if you have USD expenses (software subscriptions, SaaS tools, advertising spend) that you can pay directly using a Grey[virtual card](https://grey.co/cards) . The virtual card costs $5 to create with no monthly maintenance fee. Transactions on USD-priced sites incur no additional fee; purchases on non-USD websites carry a 2% + $0.50 cross-border fee.


When you are ready to convert to INR, Grey charges a 1% conversion fee capped at $6 per transaction. The conversion rate is displayed before you confirm, so you see exactly how much INR you will receive. Transfers to your Indian bank account require your IFSC code and typically arrive the same day. You can check Grey's[fee calculator](https://grey.co/fee-calculator) to see the exact cost for any transaction amount before you commit.


Instead of relying on automatic bank conversions that happen at arrival, or juggling multiple platforms for different currencies, you receive USD through one account, manage your balance in dollars, and convert to INR when the timing and rate suit you.


***Grey charges a 0.8% fee on deposits (minimum $2, maximum $10), a 1% conversion fee (capped at $6), and a withdrawal fee that varies by currency. Exchange rates on Grey are variable and include a margin over the mid-market rate. Cross-border card transactions on non-USD websites incur a 2% + $0.50 fee. Always review fees and the conversion rate before confirming a transaction.***
