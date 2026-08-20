---
schema_version: "1.0.0"
document_id: "9afeab63e1b55080539c7bb23970b30319d1d165cb7e43484b243fb930f085d1"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-24fa1ca79ef4"
canonical_url: "https://docs.methodfi.com/changelog/2026/february"
published_at: null
first_seen_at: "2026-07-27T10:33:39.968597+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:a4fb56ea0f6a30bcfcf4555d724a538539a1302ae751cdb6c7557061e1ce8930"
---

# February Updates

## ​


New Features


### ​


Forwarding Requests: Entity Bindings


The[Forwarding Requests API](https://docs.methodfi.com/reference/forwarding-requests/overview) now supports` entity` as a binding type. Previously, the` bindings` object only accepted` payment_instrument` and` secret` resource IDs. You can now bind an Entity and reference its data in your forwarding request templates using the` {{entity.field}}` syntax.


This enables use cases where an individual’s first and last name need to be forwarded to third-party services alongside the associated account number, without exposing raw PII to your system.


Sample Request


```text
{
"bindings"  : {
"payment_instrument"  :   "pmt_inst_7TNTTRQwQxWc"  ,
"secret"  :   "sec_au22b1fbFrmfp"  ,
"entity"  :   "ent_au22b1FBFJbp8"
},
"url"  :   "https://sample-url/v1/tokens"  ,
"method"  :   "POST"  ,
"headers"  : {
"Content-Type"  :   "application/json"  ,
"Authorization"  :   "Bearer {{secret.value}}"
},
"body"  :   "{  \"  first_name  \"  :  \"  {{entity.individual.first_name}}  \"  ,  \"  last_name  \"  :  \"  {{entity.individual.last_name}}  \"  ,  \"  number  \"  :  \"  {{payment_instrument.card.number}}  \"  }"
}


```


For more information, visit the[Forwarding Requests API](https://docs.methodfi.com/reference/forwarding-requests/overview) documentation.


### ​


Credit Score Simulations


The[Simulate API](https://docs.methodfi.com/reference/simulations/overview) now supports direct event creation for Credit Score simulations.


- **New endpoint** :` POST /simulate/events` with` type` set to` credit_score.increased` or` credit_score.decreased`
- This endpoint allows developers to trigger credit score change events for an Entity with an active Credit Score subscription in the development environment, streamlining the testing of credit score-driven workflows


This API is available in the development environment only.


For more information, visit the[Simulations API](https://docs.methodfi.com/reference/simulations/events/create) documentation.


## ​


Improvements


### ​


Updates API


**Student Loan — Opened At**


The[Updates API](https://docs.methodfi.com/reference/accounts/updates/overview) now surfaces the` opened_at` field for` student_loan` liability accounts when` source` is` direct` . This field returns the date the student loan was originally opened, providing additional context for loan lifecycle analysis, refinance eligibility assessments, and portfolio segmentation.


For more information, visit the[Updates API](https://docs.methodfi.com/reference/accounts/updates/overview) documentation.


### ​


Webhooks


**New Accounts Surfaced Event**


The` entity.new_accounts_pending_consent` webhook fires when a previously undiscovered liability account surfaces after a user progresses through[Connect](https://docs.methodfi.com/opal/connect/overview) , but only for users who haven’t opted into automatic consent for future accounts. For customers using Opal or the Element, this enables you to surface the new account to your end user for explicit consent, and optionally offer them the ability to auto-consent to future accounts going forward. This removes the need to poll the[Accounts API](https://docs.methodfi.com/reference/accounts/overview) to discover new accounts.


For more information, visit the[Webhooks API](https://docs.methodfi.com/reference/webhooks/overview) documentation.


### ​


Connect API


**Filter Accounts by Product**


You can now filter accounts by product type when using[Connect](https://docs.methodfi.com/opal/connect/overview) . This allows you to scope a Connect session to only surface accounts relevant to a specific product (e.g., only accounts eligible for payments or only accounts with update subscriptions), reducing noise and improving the end-user experience in targeted workflows.


For more information, visit the[Connect API](https://docs.methodfi.com/opal/connect/overview) documentation.


### ​


Payment Instruments API


**Card Billing Zip Code**


A new` billing_zip_code` field is now available on` card` type[Payment Instruments](https://docs.methodfi.com/reference/accounts/payment-instruments/overview) . This field is returned as part of the` card` object when retrieving a Payment Instrument.


- ` card.billing_zip_code` : The billing zip code associated with the card


This addition supports address verification (AVS) workflows and reduces payment failures caused by billing address mismatches when forwarding card details to third-party processors.


For more information, visit the[Payment Instruments API](https://docs.methodfi.com/reference/accounts/payment-instruments/overview) documentation.


### ​


Payments API


**Inbound ACH/Wire Trace Numbers & Metadata**


Inbound ACH and wire payments received via` inbound_achwire_payment`[Payment Instruments](https://docs.methodfi.com/reference/accounts/payment-instruments/overview) now include trace number and metadata fields on the[Payment](https://docs.methodfi.com/reference/payments/overview) object.


- ` source_trace_id` : enables end-to-end payment tracking and reconciliation for inbound transfers
- ` destination_trace_id` : provides the underlying bank transaction ID on the destination Account for outbound reconciliation
- ` metadata` : allows clients to associate custom reference information with incoming transactions (e.g., you can now add internal reference IDs, ​​customer IDs, and notes in the metadata)


For more information, see the[Payments API](https://docs.methodfi.com/reference/payments/overview) documentation.


### ​


Versioned API Reference Docs


The API Reference documentation now supports **version-aware navigation** . A version dropdown in the navigation bar lets you switch between API versions (` 2025-12-01` ,` 2025-07-04` ,` 2024-04-04` ) to view the documentation specific to your pinned API version.


Each version renders its own copy of the API Reference with version-specific request examples, response schemas, and available fields — so the docs you see always match the behavior of the API version you’re using.


For more information, visit the[Versioning](https://docs.methodfi.com/reference/versioning) documentation.
