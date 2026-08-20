---
schema_version: "1.0.0"
document_id: "fa567add801a08535fe79212197ab3408ef222d2ab63359237184b5c5318f42d"
company_key: "yc-xendit"
company: "Xendit"
source_id: "yc-xendit-rss-9a882734f77a"
canonical_url: "https://www.xendit.co/en/blog/a-complete-guide-to-payment-fees-what-your-business-is-actually-paying-for/"
published_at: "2026-06-25T02:42:01+00:00"
first_seen_at: "2026-07-20T23:21:00.355601+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:c4b0cbb944f2a595514cdd0bc142e6f3a67b54bb42e6fa82513ce84e3e92e258"
---

# A Complete Guide to Payment Fees: What Your Business Is Actually Paying For

Most businesses know they pay fees to accept payments. Fewer know exactly how many different types of fees are involved, how each one is calculated, or how they stack on top of each other to form the true cost of running payment operations.


Payment fees are not a single line item. They are a collection of charges applied at different points in the payment lifecycle: when a transaction is attempted, when it succeeds, when it fails, when it is disputed, when it is refunded, and even when an account sits idle. Understanding each category separately is the foundation for managing payment costs accurately and identifying where optimization is possible.


This guide covers every major type of payment fee your business should know, what drives each one, and how Xendit structures its fees across these categories.


**Key Takeaways**


- Payment fees cover multiple categories: transaction fees, processing fees, dispute fees, refund fees, platform fees, and account fees


- Each fee type applies at a different point in the payment lifecycle and needs to be tracked separately for accurate cost accounting


- The payment method, transaction volume, account type, and platform integrations all affect which fees apply and how much they cost


- Fees on failed transaction attempts are a commonly overlooked cost that adds to your effective processing rate


- Understanding all fee categories - not just the headline processing rate - is what gives businesses an accurate picture of their total payment costs


## **What are payment fees?**


Payment fees are charges applied by payment providers, card networks, banks, and platforms for the services involved in processing, managing, and maintaining digital payment operations. They exist because accepting a digital payment involves a chain of parties, each performing a distinct role and each charging for that role.


For most businesses, the visible payment fee is the processing rate deducted from each successful transaction. But that rate is just one component. A complete picture of payment costs includes fees applied to failed attempts, disputed transactions, refunds, third-party platform integrations, and account maintenance.


## **Transaction and processing fees**


Transaction and processing fees are the most familiar category. They apply every time your business processes a digital payment whether by credit card, debit card, bank transfer, e-wallet, QR payment, or any other payment method.


These fees are made up of three components:


1. **Network or scheme fee**


Charged by the card network or payment scheme for operating the infrastructure that routes and settles the transaction. For card payments, this is Visa or Mastercard. These fees are set by the network and are non-negotiable.


2. **Issuing bank or wallet provider fee**


For card transactions, the issuing bank charges an interchange fee for authorizing the transaction. For other payment methods, equivalent fees may apply depending on the scheme structure. According to the


[Nilson Report](https://nilsonreport.com/articles/merchant-processing-fees-in-the-united-states-2023/) , interchange fees represent the single largest cost component in payment processing for most card-based merchants globally.


3. **Payment provider fee** **** On top of network and bank fees, your payment provider charges a fee for the services it provides including payment infrastructure, fraud detection, security compliance, reporting, and settlement. This is the only component that varies between providers and the primary lever for cost optimization.


## **Processing fees on transaction attempts**


A cost that many businesses overlook is the fee applied to transaction attempts, not just successful transactions.


When a payment is attempted and declined, whether due to insufficient funds, fraud detection, or a technical issue, the payment infrastructure still processes that attempt. Depending on your payment provider's policy, a fee may apply to the attempt regardless of whether it resulted in a successful payment.


At Xendit, a processing fee applies to all transaction attempts, including pay-in, pay-out, and refund attempts. The specific fee varies by country and payment method.


## **Chargeback and dispute fees**


When a customer disputes a transaction through their bank, a chargeback is filed. This triggers a formal dispute process that involves the merchant, the payment provider, the acquiring bank, and the card network - and it comes with its own fee.


A chargeback fee is applied when a dispute case is opened, regardless of whether the merchant wins or loses. It covers the administrative and operational cost of processing the dispute through the card network.


At Xendit, a chargeback dispute fee of


USD 25


applies for every chargeback on card transactions. This fee is separate from the disputed transaction amount and applies per case.


***Read also:***[What is A Chargeback Fee?](https://www.xendit.co/en/blog/what-is-a-chargeback-fee-a-complete-guide-for-businesses/)


## **Refund fees**


Issuing a refund is not free. When a merchant processes a refund, the payment provider reverses the transaction through the same payment infrastructure used to process the original payment which incurs its own costs.


At Xendit, the following applies when a refund is processed:


- The original payment method fee is retained and not returned


- The Xendit processing fee on the original transaction is retained and not returned


- An additional processing fee applies for the refund (also known as a refund fee for some providers)


The specific refund processing fee amount varies by country and payment method. This means that for every refunded transaction, your business absorbs the cost of the original payment plus the cost of reversing it.


***Read also:***[What Is a Refund Fee and Why Do Payment Providers Charge It?](https://www.xendit.co/en/blog/what-is-a-refund-fee-and-why-do-payment-providers-charge-it/)


## **Third-party platform and integration fees**


When your business accepts payments through a third-party e-commerce platform - such as Shopify - additional fees may apply from both the platform and the payment provider.


- Platform transaction fees


Platforms like Shopify charge an additional transaction fee when merchants use a payment provider other than their built-in solution. On Shopify's Basic plan, this is 2.0% per transaction. The fee decreases as the plan tier increases.


- Payment provider integration fees


Payment providers may charge a fee for maintaining the technical integration with the platform. This covers the ongoing cost of building, updating, and supporting the connection between the provider's infrastructure and the platform.


At Xendit, merchants using Shopify are subject to a


Shopify Partner Fee of 0.50% of total transaction volume


of each transaction processed through the Xendit integration on Shopify. This is in addition to Shopify's own transaction fee and Xendit's standard transaction fees.


***Read also:***[Third-Party Integration Fees Explained: What E-Commerce Businesses Need to Know](https://www.xendit.co/en/blog/third-party-integration-fees-explained-what-e-commerce-businesses-need-to-know/)


## **Account and maintenance fees**


Beyond transaction-level fees, some payment providers charge fees for maintaining access to their platform or for accounts that fall below certain activity thresholds.


- Monthly maintenance fee


At Xendit, a monthly maintenance fee of


USD 250


applies to merchants who are still using Xendit's legacy API. This fee reflects the ongoing cost of supporting older infrastructure while newer API versions are available. Merchants using current API versions are not subject to this fee.


- Monthly minimum fee for dormant accounts


Accounts that are inactive or fall below minimum activity thresholds may be subject to a monthly minimum fee. At Xendit, a monthly minimum fee of


USD 50


applies to dormant and inactive accounts. This fee ensures that the cost of maintaining an account is covered even when transaction volumes are negligible.


Dormant account fees are an often-overlooked cost for businesses with multiple accounts or sub-accounts, particularly those operating platform or marketplace structures.


## **How to manage payment fees effectively**


Knowing your fee structure is the starting point. Here is how businesses can actively manage each category.


1. **Transaction and processing fees**


Understand your payment method mix and evaluate whether your current pricing model reflects your transaction profile. Higher volumes often unlock lower provider markups through negotiation.


2. **Attempt fees**


Monitor your authorization rate. A high decline rate inflates the cost per successful transaction when fees apply to attempts. Improving authorization rates directly reduces effective processing costs.


3. **Chargeback fees**


Reduce dispute volume through proactive fraud detection, clear billing descriptors, and accessible customer service. Each chargeback prevented is a USD 25 fee avoided.


4. **Refund fees**


Factor retained fees and refund processing costs into your returns policy. Understanding the true cost of a refund helps your team make more informed decisions about when to issue one versus when to offer an alternative resolution.


5. **Platform and integration fees**


Map out the full fee stack before choosing a payment provider for any platform integration. The total cost includes the platform's transaction fee, the provider's processing fee, and any integration or partner fees.


6. **Account fees**


Review your account portfolio regularly. Dormant accounts generate fees without generating revenue. Consolidating or closing inactive accounts reduces unnecessary costs.


## **Simplify payment operations with Xendit**


Xendit provides payment infrastructure built for businesses that want clarity and control over their payment costs. With transparent fee reporting and wide payment method coverage, your business has everything it needs to understand and manage every category of payment fee in one place.


Here is what Xendit offers:


- **Transparent fee reporting**


Every fee, processing, refund, dispute, or platform, appears as a clear line item in your transaction records so your finance team always has an accurate view


- **Wide payment method coverage**


Accept credit and debit cards, bank transfers, e-wallets, virtual accounts, QR payments, and other local payment methods across multiple markets


- **Fraud detection built in**


Xendit's fraud detection system monitors transactions in real time, reducing chargeback exposure and improving authorization rates


- **Dispute management support**


When chargebacks occur, Xendit's


[dispute management process](https://docs.xendit.co/docs/handling-disputes-and-chargeback?highlight=chargeback) gives your business the tools to respond efficiently and minimize losses


- **Multi-market support**


For businesses operating across multiple markets, Xendit provides payment infrastructure through licensed entities in each region


Ready to get a clear picture of what your business is actually paying for payments?


[Create a Xendit account](https://dashboard.xendit.co/register) and see how transparent, well-structured payment infrastructure changes the way your business manages payment costs.


## **Frequently Asked Questions**


### **What types of fees do businesses pay for digital payments?**


Businesses pay several categories of fees: transaction and processing fees on each payment, fees on failed transaction attempts, chargeback fees when disputes are filed, refund fees when transactions are reversed, platform integration fees when using third-party e-commerce platforms, and account maintenance fees for dormant or inactive accounts.


### **What is the difference between a processing fee and a transaction fee?**


A transaction fee is charged per successful transaction and varies by payment method, some are a fixed amount, others are a percentage of the transaction value. A processing fee is a separate charge for initiating a transaction through the payment provider's infrastructure, applied per attempt regardless of whether the transaction succeeds. Both may apply on the same transaction.


### **Does Xendit charge fees on failed transactions?**


Yes. At Xendit, a processing fee applies to all transaction attempts, including failed ones. The specific fee varies by country and payment method. This reflects the cost of processing the attempt through the payment infrastructure regardless of the outcome.


### **What is a chargeback fee and how much does Xendit charge?**


A chargeback fee is applied when a customer disputes a transaction and a formal dispute case is opened. At Xendit, a chargeback dispute fee of USD 25 applies per card chargeback, regardless of whether the merchant wins or loses the dispute.


### **What happens to my processing fees when I issue a refund?**


When a refund is processed through Xendit, the original payment method fee and Xendit processing fee are retained and not returned. An additional processing fee also applies to the refund. The


[specific fees](https://www.xendit.co/en/pricing/) vary by country and payment method.


### **What is a dormant account fee?**


A dormant account fee is a monthly charge applied to inactivae accounts that fall below minimum activity thresholds. At Xendit, a monthly minimum fee of USD 50 applies to dormant and inactive accounts.


### **How can I reduce my total payment fees?**


The most effective approaches are improving your authorization rate to reduce fees on failed attempts, reducing chargebacks through fraud detection and clear refund policies, reviewing your pricing model and payment method mix, and consolidating dormant accounts to avoid maintenance fees.


### **Where can I see all my fees in Xendit?**


Every fee category appears as a clear line item in your Xendit transaction records and can be exported for reconciliation.


[Log in to your Xendit dashboard](https://dashboard.xendit.co/) to view your transaction history and fee details.
