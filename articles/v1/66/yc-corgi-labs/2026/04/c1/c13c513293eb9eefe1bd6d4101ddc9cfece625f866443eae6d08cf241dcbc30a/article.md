---
schema_version: "1.0.0"
document_id: "c13c513293eb9eefe1bd6d4101ddc9cfece625f866443eae6d08cf241dcbc30a"
company_key: "yc-corgi-labs"
company: "Corgi Labs"
source_id: "yc-corgi-labs-atom-a99da208b4cc"
canonical_url: "https://www.corgilabs.ai/resources/disputes-and-fraud"
published_at: "2026-04-23T15:12:23.094+00:00"
first_seen_at: "2026-07-20T23:20:24.663691+00:00"
fetched_at: "2026-07-22T22:57:19.890747+00:00"
content_hash: "sha256:b03724cd9adeeeb03b484ed749750d3bb7dac0ba72a0d07ae06c688df48a2793"
---

# Disputes & Fraud

The Disputes and Fraud page gives payment operations teams a single place to monitor dispute activity, fraud patterns, and the financial drag both create. Use it to spot emerging trends, trace fraud back to its source, and act on the levers that reduce chargebacks before they compound.


## Dispute, Fraud, and Chargeback Rate & Count


The top of the page surfaces two headline metrics side by side. The **Dispute, Fraud, and Chargeback Rate** expresses the share of transactions that ended in a dispute, fraud case, or chargeback as a percentage. The **Count** shows the raw number of cases behind that percentage, so you can tell whether a rate shift reflects a handful of incidents or a meaningful volume change.


Both metrics include a trend comparison against the previous period. A rising rate paired with a rising count points to a real increase in dispute pressure, while a rising rate on a falling count usually reflects lower transaction volume rather than worsening fraud exposure.


## Dispute Win Rate


The **Dispute Win Rate** is the percentage of disputes you have won. A low win rate is a signal worth investigating. It often points to gaps in the evidence you submit, delays in your response workflow, or documentation that does not map cleanly to the reason codes issuers expect. Treat a sustained dip as a prompt to audit your representment process rather than a one-off outcome.


## Estimated Loss Analytics


This section quantifies the financial impact of disputes and fraud. The estimated loss is calculated as three times the disputed amount, a multiplier that captures the operational and financial costs that sit alongside the face value of the dispute itself: fees, recovery labor, merchandise loss, and downstream risk exposure.


Watching this figure next to raw dispute counts helps you prioritize. A small number of high-value disputes can outweigh a larger volume of low-value ones, and the loss view makes that tradeoff explicit.


## Fraud Breakdowns


### Dispute Reason Breakdown


A stacked bar chart plots dispute counts by reason category over time. The categories are Fraud, Product Not Received, Product Unacceptable, Duplicated Charge, and All Other Reasons. The shape of the stack tells you whether your disputes are primarily fraud-driven or operational, which in turn tells you whether to lean on fraud tooling or on fulfillment and billing fixes.


### Payment Method Distribution


This view breaks disputes down by payment method, including JCB, Visa, Mastercard, and American Express. Three callouts sit above the chart:


-


**Highest Fraud Rate** by method


-


**Most Disputes** by method


-


**Safest Payment Method**


A bar chart beneath the callouts compares Fraud versus Other Disputes for each payment method, so you can see whether a method's risk profile is driven by fraud specifically or by a broader mix of dispute types.


### Payment Method Details Table


The details table gives you the full numeric picture for each payment method:


Column


What it shows


Dispute Rate


Share of transactions on this method that became disputes


Dispute Count


Number of disputes on this method


% of Total Disputes


This method's share of all disputes


Fraud Rate


Share of transactions on this method flagged as fraud


Fraud Count


Number of fraud cases on this method


% of Total Fraud


This method's share of all fraud cases


### Country Breakdown


Switch to the Countries tab to see the same patterns mapped by geography. Country-level concentration often surfaces risk signals that payment-method slicing hides, especially for cross-border volume.


## Rate Basis Setting


You can toggle the basis used to calculate the rates on this page:


1.


**Dispute date.** Rates are calculated based on when the dispute was filed.


2.


**Transaction date.** Rates are calculated based on when the original transaction occurred.


Dispute-date basis reflects what your operations team is handling right now. Transaction-date basis reflects the quality of the transactions you accepted in a given period, which is the right lens for evaluating acceptance and fraud-screening changes.
