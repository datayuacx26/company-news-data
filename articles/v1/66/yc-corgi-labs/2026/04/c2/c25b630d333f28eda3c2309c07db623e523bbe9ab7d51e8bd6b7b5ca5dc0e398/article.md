---
schema_version: "1.0.0"
document_id: "c25b630d333f28eda3c2309c07db623e523bbe9ab7d51e8bd6b7b5ca5dc0e398"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/resources/payment-analytics"
published_at: "2026-04-23T15:14:57.045+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-22T22:57:19.890747+00:00"
content_hash: "sha256:1f6625e711f48eb13a8d75a2a5aaf8281bc33b598369e777b2d27cd82ba9d131"
---

# Payment Analytics

The Payment Analytics page provides a comprehensive view of your payment performance from checkout to final outcome. Use it to understand your payment funnel, identify where transactions fail, and find opportunities to improve your authorization rate.


## Payment Funnel


The **Payment Funnel** visualizes the complete transaction flow from checkout through risk screening, 3D Secure authentication, and final outcome. Each stage shows both the count and the dollar volume, so you can see both the frequency and financial impact at every step. The funnel has four sequential stages:


1.


**Checkout:** Total checkout sessions initiated by customers, and how many lead to attempted payments.


2.


**Risk Model:** Transactions evaluated by fraud prevention services, typically offered by your payment provider or a 3rd party, including by Corgi Labs. Shows how many transactions were approved vs. blocked.


3.


**3DS:** Transactions routed through 3D Secure authentication, performed by the card issuer. Shows No 3DS (not required), 3DS Passed, and 3DS Failed counts.


4.


**Outcome:** Final result of the payment: succeeded or declined by the issuer.


## Core Payment Metrics


Metric


Definition


Payment Success Rate


The overall percentage of payment attempts that resulted in a successful charge. This metric accounts for all failure points, including fraud blocks, 3DS failures, and issuer declines. Trend comparison against the previous period is shown.


Authorization Rate


The percentage of payment attempts that were authorized (approved) by the issuing bank, after passing through risk screening and 3DS. This isolates network-level acceptance from fraud blocks performed by your payment and fraud prevention providers.


## Block, Decline & Abandonment Rates


Metric


Definition


Block Rate


Percentage of transactions blocked by your fraud rules or risk model before reaching the payment network.


Decline Rate


Percentage of transactions declined by the card network or issuing bank.


Abandonment Rate


Percentage of checkout sessions abandoned by the customer.


## Block Reasons


This section breaks down why transactions were blocked during risk screening. A stacked bar chart shows monthly volumes by block source.


## 3D Secure Authentication


This section examines 3DS challenge rates and authentication performance.


Metric


Definition


3DS Rate


Percentage of transactions routed through 3D Secure.


3DS Success Rate


Percentage of 3DS that were successful.


Challenge Rate


Percentage of 3DS transactions that triggered an active challenge (as opposed to frictionless authentication).


Challenge Success Rate


Percentage of 3DS challenges that were successful.


A monthly breakdown chart shows Successful Challenges, Successful Frictionless, and Failed Challenges over time. A summary table shows the count, share, and request volume for each outcome.


## Decline Reasons Distribution


A stacked bar chart shows decline counts by reason category over time. Use this to identify whether declines are driven by preventable issues (like expired cards) or systemic problems (like issuer-side fraud flags).


## Geographic Payment Distribution


A world map visualization shows payment volume by card-issuing country. This helps you understand the geographic distribution of your customers and identify regions with unusually high decline or fraud rates.


## Deduplication Setting


Payment metrics can be viewed as deduplicated or raw:


-


**Deduplicated:** Count only the final outcome per payment, which removes duplicate retries. Multiple attempts are considered the same payment when they have:


-


Same invoice_id for subscription payments, or


-


Same Customer + close-in-time + same amount, or


-


Same card number + close-in-time + same amount


-


**Raw:** Count all payment attempts.


Tracking the same metrics across the same setting allows a fair evaluation of the metrics' performance over time. Depending on your business model, a significant difference between deduplicated metrics and raw metrics may indicate increased customer checkout friction.
