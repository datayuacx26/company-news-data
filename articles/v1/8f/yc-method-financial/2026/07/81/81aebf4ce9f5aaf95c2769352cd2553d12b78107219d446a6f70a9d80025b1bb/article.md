---
schema_version: "1.0.0"
document_id: "81aebf4ce9f5aaf95c2769352cd2553d12b78107219d446a6f70a9d80025b1bb"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-24fa1ca79ef4"
canonical_url: "https://docs.methodfi.com/changelog/2026/june"
published_at: null
first_seen_at: "2026-07-29T01:47:33.026159+00:00"
fetched_at: "2026-07-29T01:47:33.918432+00:00"
content_hash: "sha256:085d4deb044c62e44c21cef29c60104b4d1819b5d020a1aac5be876aabca0e33"
---

# June Updates

## ​


New Features


### ​


Card Brand Details


The Card Brands endpoint now returns a structured` details` object on each brand, giving you the full marketing-level terms behind a user’s card, including APRs, fees, rewards, and promotional offers.


- **APRs** : Purchase and cash-advance APR ranges via` purchase_apr_min` /` purchase_apr_max` and` cash_advance_apr_min` /` cash_advance_apr_max` .
- **Fees** :` annual_fee` and` late_payment_fee` .
- **Rewards** : A full rewards object with program name, reward type (for example,` cash_back` ,` points` ,` miles` ), and per-category earn rates (dining, travel, groceries, gas, and more), including the base-rate category for all other purchases.
- **Promotions** : A` promotions` array covering sign-up bonuses, intro APR offers, statement credits, and more, each with value, spend requirement, qualifying period, and expiration.
- **Classification and freshness** :` card_category` classifies the card (for example,` travel` ,` cash_back` ,` store` ), and` data_as_of` tells you when the details were last captured.


**Key Use Cases**


- Power balance-transfer and refinance offers with accurate rate and promotion data.
- Enrich PFM experiences by showing users the detailed terms and benefits of the cards they carry.
- Optimize checkout and reward experiences by auto-routing purchases to the top-earning card.


Card Brands is available as both a Product (on-demand) and a Subscription (near real-time updates), and the details object is available on API version` 2025-07-04` and later. The endpoint is available to partners processing payments through the Payment Instruments endpoint. Contact your Method CSM to enable it. See the[Card Brands reference](https://docs.methodfi.com/reference/accounts/card-brands/overview) to get started.


### ​


Dashboard: Check Payment Management


Admins can now manage check payments directly from the payment detail page in the[Method Dashboard](https://dashboard.methodfi.com/) .


- **Stop payment** : Place a stop on a paper check that has been sent but not yet cashed. The check is stopped and funds are reversed to the source.
- **Check endorsement image** : View the front and back endorsed image of a check once it has been cashed.
- **Source & destination status** : The payment detail page now surfaces source status and destination status alongside the existing payment status for better visibility into where a payment is in its lifecycle.


Stop payment and check image viewing are available to admin users only.


For more information, visit the[Method Dashboard](https://dashboard.methodfi.com/) .


### ​


Preauth Signals


The new Preauth Signals endpoint returns the predicted likelihood that a transaction will succeed, alongside verified ownership of the card and recommended retry timing. Built for merchants and commerce platforms looking to optimize payment performance.


**Key Use Cases**


- Predict whether a card payment will go through before attempting the charge to reduce false declines at checkout.
- Identify the best date to retry a payment for recurring charges.


This endpoint is currently in Public Preview. Reach out to your Method CSM for access and documentation.


### ​


New Attributes: Delinquency, HELOC & Available Credit


New entity-level and account-level attributes give you a more complete, real-time picture of borrower risk and capacity.


- **Delinquency signals** : Aggregate borrower-level risk with the worst days-past-due bucket across accounts, whether delinquency is progressing or recently cured, total delinquent balance, and the highest-severity outcome (charge-off, collections, foreclosure, and more).
- **HELOC-specific attributes** : You can now separate out attributes related to HELOCs from mortgages and personal loans to isolate appetite for a new HELOC, CLI, or refi.
- **Available credit** : Understand a borrower’s total credit headroom in near real time to assess their riskiness and eligibility for new products.
- **Direct Pay enrichment** : Completed attribute responses now include Method Direct Pay history (recency, amount, and trailing 12-month count), providing a clearer picture of a borrower’s risk profile and appetite for debt consolidation.


For more information, visit the[Entity Attributes](https://docs.methodfi.com/reference/entities/attributes/overview) and[Account Attributes](https://docs.methodfi.com/reference/accounts/attributes/overview) API documentation.


### ​


Account Attribute Subscriptions


Method now triggers events whenever an account’s attributes change, so you can react to balance, utilization, and delinquency shifts in real time instead of polling. Subscribe to the attributes you care about and Method delivers updates to your configured webhooks as they occur.


- Manage what you listen to through the[Subscriptions endpoint](https://docs.methodfi.com/reference/accounts/subscriptions/overview) .
- Receive changes through[Events](https://docs.methodfi.com/reference/events/overview) and webhooks.


This pairs naturally with continuous, post-origination portfolio monitoring use cases.


### ​


Team IP Allowlisting


Teams can now restrict API access to a defined set of IP addresses. Once an allowlist is configured, requests from outside the approved ranges are rejected, with enforcement backed by a 24-hour cache for low-latency checks. See[IP Whitelisting](https://docs.methodfi.com/reference/ip-whitelisting) to configure.


## ​


Improvements


### ​


Payments


- **More accurate estimated completion dates** : The` estimated_completion_date` returned on a payment now always shows the correct date or a slightly conservative estimate, especially for same-day ACH source payments.
- **Destination posted date** : The` destination_posted_date` field is now populated once the destination posts, giving you a precise reconciliation signal that is distinct from the estimated completion date.


For more information, visit the[Payments API](https://docs.methodfi.com/reference/payments/overview) documentation.


### ​


Payment Limits


To protect platform safety and stability, per-transaction and 24-hour limits are now enforced on payment volumes.


A new Limits tab under Dashboard Settings surfaces your team’s money-movement capabilities and associated limits in one place. You can now see at a glance what your team is permitted to send and receive, and where caps apply.


Contact your customer success manager to learn more about your team’s limits.
