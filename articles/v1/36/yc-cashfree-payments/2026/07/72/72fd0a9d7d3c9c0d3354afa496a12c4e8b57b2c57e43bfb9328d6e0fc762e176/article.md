---
schema_version: "1.0.0"
document_id: "72fd0a9d7d3c9c0d3354afa496a12c4e8b57b2c57e43bfb9328d6e0fc762e176"
company_key: "yc-cashfree-payments"
company: "Cashfree Payments"
source_id: "yc-cashfree-payments-rss-98daff448d11"
canonical_url: "https://blogrevamp.cashfree.com/how-bharatpe-built-a-more-reliable-lending-repayment-experience-with-cashfree/"
published_at: "2026-07-22T11:38:44+00:00"
first_seen_at: "2026-07-22T11:51:58.877167+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:a1b535b569bf04b2fa20f53ae30cdbd3eb15d195654a8e9d00c63e3793ce2d35"
---

# How BharatPe Built a More Reliable Lending Repayment Experience with Cashfree Payments

Table of Contents


Toggle


##


About BharatPe


BharatPe is one of India’s leading fintech companies, originally built around enabling small merchants to accept digital payments through a single interoperable QR code. Over the years,[BharatPe](https://bharatpe.com/) has expanded into financial services, with lending becoming a significant part of its business – offering working capital loans and credit products to merchants and consumers across Tier 2 and Tier 3 cities.


##


The Challenges


Given the scale at which BharatPe operates, even small inefficiencies in the repayment flow compound quickly, making the reliability and flexibility of their collections infrastructure critical to their business. The standard[UPI AutoPay integration](https://www.cashfree.com/upi-autopay/) offered little room for that flexibility, leaving BharatPe with a rigid, system-driven repayment flow that couldn’t adapt to how their collections actually needed to work.


**BharatPe had two core needs:**


- BharatPe was tied to a fixed automated schedule of Post Debit Notifications (PDNs), pre-decided by[Cashfree’s backend](https://www.cashfree.com/docs/api-reference/payments/latest/subscription/payment/create-controlled-notification) . This did not allow BharatPe to decide the timing of PDNs and their retries to account for borrower behaviour.


- They wanted the ability to decide when to send a pre-debit notification and when to trigger the corresponding charge, independently and on their own schedule. All of this within NPCI’s regulatory framework of PDN/charge blackout windows as well as ensuring the need to send a PDN at least 24 hours before a charge.


- When a PDN or charge attempt failed, BharatPe had no control over how or when a retry would happen. Retries were handled entirely by Cashfree’s backend, which meant BharatPe could not factor in borrower behaviour, optimal timing, or their own collections logic when deciding the next attempts.


- They wanted full ownership of the retry strategy for both PDNs and charge executions , including the ability to spread retries across the next few days, while remaining within the bounds of the NPCI framework.


##


The Solution:


Cashfree Payments worked closely with BharatPe to build solutions that addressed their needs – not through a standard off-the-shelf integration, but through a deeply customised setup designed around the way BharatPe’s lending business actually operates.


### **Giving BharatPe Full Control Over Pre Debit Notification (PDN) and Charges**


Cashfree decoupled these two events into separate APIs, giving BharatPe independent control over both:


**PDN API:** BharatPe can now trigger the pre-debit notification independently, at a time of their choosing. If a notification fails, they can reschedule and retry it on their own terms, while staying compliant to NPCI guidelines.


**Charge API:** BharatPe can trigger the debit separately, at the most optimal moment for each borrower. They can also retry it at a different time, giving them a real shot at recovery that simply wasn’t possible before.


This decoupling transforms the debit flow from a rigid and time based trigger into a flexible one.


Now with API level control, BharatPe can build rules around when to send the PDN and when to charge, based on borrower profile.


Additionally, Cashfree also built a simulation environment in the sandbox that allowed BharatPe to test the new APIs end-to-end. This customization allows BharatPe to validate their integration thoroughly before going live, reducing the risk of production issues at scale.


##


Result


The combination of deep customisation, smarter mandate lifecycle management, and a high-performance UPI infrastructure delivered measurable improvements across BharatPe’s lending repayment stack:


- An overall Autopay **Success Rate of 91%**
- **4%** relative improvement from dynamic routing. When a payment is initiated, Cashfree automatically picks the best available payment route based on current success rates and network performance.
- Faster go-live for new API features thanks to a full simulation environment in sandbox, enabling BharatPe’s tech team to test against real-world debit scenarios before production deployment
