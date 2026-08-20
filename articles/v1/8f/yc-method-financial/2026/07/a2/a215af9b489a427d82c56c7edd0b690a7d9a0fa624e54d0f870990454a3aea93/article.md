---
schema_version: "1.0.0"
document_id: "a215af9b489a427d82c56c7edd0b690a7d9a0fa624e54d0f870990454a3aea93"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-24fa1ca79ef4"
canonical_url: "https://docs.methodfi.com/changelog/2026/march"
published_at: null
first_seen_at: "2026-07-27T10:33:39.968597+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:20861f72eaf69afc600fc990b259e5893de71333f3749763fbe3209d04134d44"
---

# March Updates

## ​


New Features


### ​


[API Guides](https://docs.methodfi.com/guides/overview)


A new API Guides section is now available in Method’s documentation. This comprehensive, concept-first companion to the API Docs helps product and business teams understand how Method’s platform works and how best to configure Method’s APIs for their use case. The API Guides include:


- **How Method Works** : How Method leverages multiple data sources to power identity verification, account discovery, and data retrieval.
- **Core Products** : In-depth guides for each product area, covering payload details, subscription behaviors, error handling, and edge cases.
- **Use Case Guides** : Implementation frameworks for the most common use cases: debt consolidation and balance transfers for lenders, card linking and checkout for commerce platforms, and liability dashboards and bill pay for personal finance apps, with step-by-step flows mapping Method products to each stage of the customer journey.


These guides are designed to complement the existing API Reference documentation by providing the conceptual foundation and product context that teams need to plan and build their integrations effectively.


### ​


[OpenAPI Specification](https://github.com/MethodFi/openapi)


Method now publishes an official OpenAPI specification for the Method API. The spec enables you to:


- **Generate typed API clients** in any language using OpenAPI-compatible code generators
- **Import endpoints** directly into tools like Postman or Insomnia
- **Integrate with any toolchain** that supports the OpenAPI standard


For more information, visit the[Libraries](https://docs.methodfi.com/libraries/overview) documentation.


## ​


Improvements


### ​


Webhook Idempotency Headers


You can now easily deduplicate retried or replayed webhook deliveries. All outbound webhooks now include a` method-webhook-delivery-id` header, containing a unique identifier for each webhook delivery. If your endpoint receives the same delivery twice (due to network timeouts, retries, or replays), the` method-webhook-delivery-id` value will be identical, letting you skip duplicate processing with a simple lookup.


Sample Headers


```text
{
"method-webhook-delivery-id"  :   "obwh_yBaCP3DQxJ4"  ,
"content-type"  :   "application/json"
}


```


For more information, visit the[Webhooks API](https://docs.methodfi.com/reference/webhooks/overview) documentation.


### ​


Simulations API


**Connect Webhook in Account Opened Simulation**


The` POST /simulate/events` endpoint for` account.opened` events now also sends the` connect.create` webhook as part of the simulation, aligned with the endpoint’s production behavior.


Previously, simulating an` account.opened` event only fired the` account.opened` webhook. This gap meant customers couldn’t fully test their Connect-to-Account-Opened integration pipeline in dev.


Now, when you simulate` account.opened` , both webhooks fire in sequence:


1. ` connect.create` — the Connect session that discovered the account
2. ` account.opened` — the account itself becoming available


This enables true end-to-end testing of workflows that listen for Connect completion and then act on the resulting accounts.


POST /simulate/events


```text
{
"type"  :   "account.opened"  ,
"entity_id"  :   "ent_au22b1FBFJbp8"  ,
"account_id"  :   "acc_Dz3E8r4mJ7xKn"
}


```


For more information, visit the[Simulations API](https://docs.methodfi.com/reference/simulations/events/create) documentation.
