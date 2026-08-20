---
schema_version: "1.0.0"
document_id: "8227825510c73cb9b6b617160a1e24246b224aaf9b17ec71c803fe2ed0d9ddf9"
company_key: "yc-xendit"
company: "Xendit"
source_id: "yc-xendit-rss-9a882734f77a"
canonical_url: "https://www.xendit.co/en/blog/how-payment-processing-fees-are-calculated/"
published_at: "2026-06-24T21:26:09+00:00"
first_seen_at: "2026-07-20T23:21:00.355601+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:d591494817eef3f4d93d01866afdf68101d2a9113cac0329521cef686d56fc4a"
---

# How Payment Processing Fees Are Calculated

For most businesses, payment fees appear as a single deduction per transaction. In practice, two distinct fee types are at work and understanding the difference between them is foundational to accurate payment cost management. The transaction fee applies when a payment is completed successfully. The processing fee applies each time a transaction is initiated, regardless of outcome.


This article focuses on the processing fee: what it is, how it is calculated, and how it interacts with your transaction fee to form your total payment costs.


**Key Takeaways**


- A processing fee is charged per transaction attempt, including failed ones, and is separate from the transaction fee charged on successful payments


- The processing fee amount varies by country and payment method


- A high decline rate means processing fees accumulate on attempts that generate no corresponding revenue, raising the effective cost per successful transaction


- Both fee types appear as separate line items in your transaction records and must be tracked independently for accurate reconciliation


## **What Is a Processing Fee and When Does It Apply?**


A processing fee is a charge levied for using a payment provider's infrastructure to initiate a transaction. Each time your system initiates a transaction to process a payment - whether a pay-in, pay-out, or refund - the provider's infrastructure is engaged: routing the request, communicating with the relevant payment network, and returning an authorization response. This occurs regardless of whether the transaction is ultimately approved or declined.


This is the defining distinction between a processing fee and a transaction fee. A transaction fee, also referred to as a payment method fee or payout fee, applies only upon successful completion of a payment. A processing fee applies to every attempt, because the underlying infrastructure cost is incurred the moment the transaction is initiated.


For most payment providers, a processing fee is charged on every transaction attempt, including pay-in, pay-out, and refund attempts, regardless of outcome.The applicable fee varies by country and payment method.


## **How the Processing Fee Is Calculated**


The total processing fee your business incurs in a given period is determined by two variables: the number of transaction attempts made, and the applicable per-attempt rate. A higher volume of attempts including declined ones, results in a higher total processing fee. This is why your authorization rate directly affects your payment costs, not just your revenue.


Xendit processing fee is calculated on a per-attempt basis. Each pay-in, pay-out, or refund attempt incurs the applicable processing fee for your country and payment method, regardless of whether the transaction succeeds.


## **Processing Fees on Failed Transactions**


Because the processing fee applies to every transaction attempt regardless of outcome, declined transactions generate a direct cost with no corresponding revenue. The financial significance of this grows with transaction volume and becomes more pronounced when decline rates are high.


A business with a high decline rate pays processing fees on a disproportionate share of attempts that generate no return. This raises the effective processing cost per successful transaction above what the per-attempt rate alone would suggest. Conversely, improving your authorization rate, the percentage of attempts that result in a completed payment, directly reduces your effective processing cost without any change to the per-attempt rate itself.


## **How Processing Fees Interact with Your Transaction Fee**


Processing fees and transaction fees are independent charges, each applying under different conditions and together constituting your total payment costs.


1. **Transaction fee (payment method fee or payout fee)**


Applied per successful transaction. Covers the cost of accepting a completed payment, encompassing network fees, issuing bank fees, and the payment provider's service fee for infrastructure, fraud detection, and settlement.


2. **Processing fee**


Applied per transaction attempt, regardless of outcome. Covers the cost of using Xendit's payment infrastructure to initiate the transaction.


For a successful transaction, both fees apply. For a declined transaction, only the processing fee applies, no transaction fee is charged because no payment was completed.


The practical implication is that a failed transaction is not a zero-cost outcome. It carries the processing fee, in addition to the indirect costs associated with the decline: retry operations, customer service overhead, and potential revenue loss where the customer does not complete the purchase through an alternative method.


## **Factors That Affect Your Total Processing Fee**


Several variables determine the total processing fee your business incurs each month.


1. **Payment method**


Different payment methods - cards, bank transfers, e-wallets, QR payments - carry different per-call rates, reflecting the distinct infrastructure involved in processing each type.


2. **Transaction volume**


Higher transaction volumes result in greater total processing fee exposure. Processing fees should be incorporated into payment cost models alongside transaction fees rather than treated as a secondary consideration.


3. **Decline rate**


A higher decline rate increases the proportion of processing fees attributable to attempts that generate no revenue. Improving your authorization rate has a direct and proportional impact on your effective processing cost per successful transaction.


## **Manage Payment Costs with Xendit**


Xendit provides payment infrastructure built for businesses that need clarity and control over their processing costs.


Every fee processing, refund, or dispute appears as a clear line item in your transaction records, giving your finance team an accurate and complete view of payment costs without manual reconciliation. This transparency is the foundation of effective cost forecasting and optimization.


Here is what Xendit offers to help businesses manage processing costs:


- Wide payment method coverage.


Accept credit and debit cards, bank transfers, e-wallets, virtual accounts, QR payments, and other local payment methods - giving you flexibility in how your payment mix is structured and the costs associated with it


- Transparent fee reporting.


Every cost component is visible in your dashboard and exportable for reconciliation - no blended rates that obscure what you are actually paying


- Fraud detection built in.


Xendit's fraud detection system monitors transactions in real time, improving authorization rates and reducing the chargeback exposure that inflates your effective processing cost


- Dispute management support.


When chargebacks occur, Xendit's


[dispute management process](https://docs.xendit.co/docs/handling-disputes-and-chargeback?highlight=chargeback) gives your business the tools to respond efficiently and minimize losses


- Multi-market support.


For businesses operating across multiple markets, Xendit provides payment infrastructure through licensed entities in each region


Ready to get a clearer picture of what your business pays to process payments?


[Create a Xendit account](https://dashboard.xendit.co/register) and see how transparent payment infrastructure changes the way your business manages processing costs.


##


## **Frequently Asked Questions**


### **What Is a Payment Processing Fee?**


A processing fee is charged each time a transaction is initiated through a payment provider's infrastructure, regardless of whether it succeeds. At Xendit, this applies to all transaction attempts for pay-in, pay-out, and refund attempts. It is distinct from the transaction fee, which is charged only on successful payments.


### **How Is the Processing Fee Different from the Transaction Fee?**


A transaction fee is charged per successful transaction and covers the cost of accepting a completed payment. A processing fee is charged per transaction attempt regardless of outcome, covering the cost of engaging the payment provider's infrastructure. Both appear as separate line items in your transaction records.


### **Why Does Xendit Charge a Processing Fee on Failed Transactions?**


The processing fee reflects the infrastructure cost of initiating a transaction: routing the request, engaging the payment network, and returning an authorization response. This cost is incurred the moment a transaction is initiated, regardless of whether the issuing bank approves the transaction.


### **Where Can I View My Processing Fees in Xendit?**


Every transaction attempt is recorded as a clear line item in your


[Xendit dashboard](https://dashboard.xendit.co/) , with processing fees reported separately from transaction fees.
