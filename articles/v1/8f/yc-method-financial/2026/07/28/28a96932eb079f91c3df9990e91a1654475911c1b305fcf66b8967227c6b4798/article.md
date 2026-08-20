---
schema_version: "1.0.0"
document_id: "28a96932eb079f91c3df9990e91a1654475911c1b305fcf66b8967227c6b4798"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-24fa1ca79ef4"
canonical_url: "https://docs.methodfi.com/changelog/2026/may"
published_at: null
first_seen_at: "2026-07-27T10:33:39.968597+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:4439bb00efd824838b7ddd03e44e660246c8b2e71a0eff8613fb961ee4efff72"
---

# May Updates

## ​


New Features


### ​


Snapshot Delinquency Data


[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) with` source: "snapshot"` now include a comprehensive set of delinquency fields sourced from the credit report. These fields give you a full picture of a borrower’s delinquency status without pulling a separate credit report for the user.


- **Delinquency Status & Period** :` delinquent_status` ,` delinquent_period` , and` delinquent_start_date` indicate the current delinquency state, severity bucket (` less_than_30` ,` 30` ,` 60` ,` 90` ,` 120` ,` over_120` ), and when the delinquency began.
- **Delinquent Actions** :` delinquent_action` surfaces servicer-level actions such as` charge_off` ,` foreclosure` ,` repossession` ,` chapter_7` ,` chapter_13` ,` wage_garnishment` ,` payment_agreement` , and` bankruptcy` .
- **Delinquent Amount** :` delinquent_amount` returns the dollar amount currently reported as delinquent.
- **Delinquency History** :` delinquent_history` provides a time-series array of historical delinquency periods, each with` start_date` ,` end_date` ,` status` ,` period` , and` action` , enabling trend analysis across multiple reporting cycles.


Delinquency fields are available on snapshot Updates for` credit_card` ,` auto_loan` ,` personal_loan` ,` mortgage` , and` student_loan` liability types. These fields are not included on Updates with` source: "direct"` .


For more information, visit the[Updates API](https://docs.methodfi.com/reference/accounts/updates/overview) documentation.


### ​


Receive Reversals via Wire


You can now receive payment reversals via wire. This functionality allows customers to quickly and easily access funds from reversed payments that previously could not run on ACH rails (such as inbound wire transfers via payment instrument).


These accounts are receive-only and do not support outbound payments.


For more information, visit the[Accounts API](https://docs.methodfi.com/reference/accounts/overview) documentation.


### ​


Dashboard: Webhook Management


The[Method Dashboard](https://dashboard.methodfi.com/) now supports full webhook lifecycle management. You can create, view, update, and delete webhook endpoints directly from the UI, with no API calls or support requests required.


- Register new webhook endpoints with custom event filters and authentication
- View all active webhooks and their status
- Update endpoint URLs and configuration in place
- Delete webhooks that are no longer needed


For more information, visit the[Method Dashboard](https://dashboard.methodfi.com/) .


## ​


Improvements


### ​


Rate Limiting


To protect platform stability and ensure consistent performance across all customers, all API requests are now subject to published, tiered rate limits. Documented limits mean your engineering team can design around known thresholds rather than hitting undefined behavior during peak operations.


- Rate limits are applied per API key within a 60-second rolling window, organized into tiers based on operation type.
- Each tier tracks independently, so usage against one tier does not affect capacity in another.
- Customers with high-volume integrations can request custom limits through their customer success manager.


For more information, visit the[Rate Limiting](https://docs.methodfi.com/reference/rate-limiting) documentation.


### ​


Webhooks


**Delivery Rate Limiting**


Outbound webhook deliveries are now rate-limited to 15 deliveries per second per webhook registration. This smooths delivery during high-volume events (e.g., bulk Connect completions or batch Update subscriptions) and reduces the likelihood of triggering rate-limit responses on your infrastructure.


**Failure Handling**


An individual webhook delivery is now retried up to 5 times before it is marked as failed, rather than remaining in a retry loop indefinitely. After 5 consecutive delivery failures, the webhook` status` is set to` disabled` , consistent with the documented automatic-disabling behavior. You can reactivate a disabled webhook from the Dashboard or via the[Update a Webhook](https://docs.methodfi.com/reference/webhooks/update) endpoint once the underlying issue is resolved.


For more information, visit the[Webhooks API](https://docs.methodfi.com/reference/webhooks/overview) documentation.


### ​


Payments API


**Destination Processing Window**


The destination processing window has been updated to 4:00 PM CT (5:00 PM ET). Payments submitted before this cutoff can be processed the same-day; payments submitted after are queued for next-business-day processing.


For more information, visit the[Payments API](https://docs.methodfi.com/reference/payments/overview) documentation.


### ​


Simulations API


**Account Closed Event**


The` POST /simulate/events` endpoint for` account.closed` events now also triggers the` account.update` webhook as part of the simulation, matching production behavior. This enables more complete end-to-end testing of account lifecycle workflows in the development environment.


POST /simulate/events


```text
{
"type"  :   "account.closed"  ,
"entity_id"  :   "ent_au22b1FBFJbp8"  ,
"account_id"  :   "acc_Dz3E8r4mJ7xKn"
}


```


When simulating` account.closed` , both webhooks now fire in sequence:


1. ` account.update` : the account snapshot is updated with the closed status
2. ` account.closed` : the account itself transitions to closed


For more information, visit the[Simulations API](https://docs.methodfi.com/reference/simulations/events/create) documentation.


### ​


Development Environment


**Mock Credit Reports for Connect**


Mock raw credit reports are now available for[Connect](https://docs.methodfi.com/opal/connect/overview) flows in the development environment, enabling end-to-end testing of credit report-based account discovery without live bureau data.


This feature is available in the development environment only.


**Payment Destination Details in Responses**


The development environment` POST /payments` response now includes the` destination_payment_method` field, aligning dev responses with production behavior. This field indicates how the payment is delivered to the recipient, either` electronic` or` paper` .


```text
{
"id"  :   "pmt_rPrDPEwyCVUcm"  ,
"status"  :   "pending"  ,
"source"  :   "acc_JMJZT6r7iHi8e"  ,
"destination"  :   "acc_AXthnzpBnxxWP"  ,
"destination_payment_method"  :   "electronic"
}


```
