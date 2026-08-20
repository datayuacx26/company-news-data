---
schema_version: "1.0.0"
document_id: "ff6ed16b8ae4ab730280a50ea9dcbed739cd3320b93041f9c350be938775bf33"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-24fa1ca79ef4"
canonical_url: "https://docs.methodfi.com/changelog/api-versions/2026-03-30"
published_at: null
first_seen_at: "2026-07-27T10:33:39.968597+00:00"
fetched_at: "2026-07-28T21:16:46.713740+00:00"
content_hash: "sha256:c0c6c2be9afd77ff51c9a3acdf30000c4e5b8f513d6a2687a331d395d899e459"
---

# Introducing API Version 2026-03-30

We’re excited to announce the release of **API version` 2026-03-30`** , featuring redesigned **Attributes** for both **Entities** and **Accounts** . This release expands the attributes product with richer computed outputs, a unified per-attribute result shape, and support for asynchronous attribute computation workflows.


---


## ​


What’s New


### ​


Entity Attributes


The[Entity Attributes API](https://docs.methodfi.com/reference/entities/attributes/overview) now returns a broader set of aggregated financial attributes computed across an Entity’s connected accounts. These attributes are organized across categories such as revolving credit cards, personal loans, mortgages, installment balances, and overall utilization.


**Key improvements:**


- Introduces a richer set of financial attributes, including balances, utilization trends, deltas, delinquency signals, and payment behavior metrics
- Returns attributes in a simplified` { value, error }` shape —` error` uses the standard` { type, sub_type, code, message }` format when the attribute could not be computed
- Computes all supported entity attributes automatically as part of the request lifecycle
- Supports asynchronous processing so teams can poll or rely on webhook-driven workflows for completion


Available when passing` Method-Version: 2026-03-30` in the request header.


Example Response


```text
{
"id"  :   "attr_dADraNgLBrhgL"  ,
"entity_id"  :   "ent_EQ3FCTzDUmmCb"  ,
"status"  :   "completed"  ,
"attributes"  : {
"revolving_credit_card_balance_total"  : {
"value"  :   968400  ,
"error"  :   null
},
"credit_limit_total"  : {
"value"  :   8100000  ,
"error"  :   null
},
"credit_card_utilization"  : {
"value"  :   11.96  ,
"error"  :   null
},
"overall_utilization"  : {
"value"  :   70.5  ,
"error"  :   null
}
},
"error"  :   null  ,
"created_at"  :   "2026-04-09T17:02:47.910Z"  ,
"updated_at"  :   "2026-04-09T17:04:35.220Z"
}


```


---


### ​


Account Attributes


The[Account Attributes API](https://docs.methodfi.com/reference/accounts/attributes/overview) has been expanded into a dedicated account-level attributes product. Returned attributes now vary by liability type, including support for credit cards, personal loans, and mortgages.


**Key improvements:**


- Adds account-level financial attributes such as utilization trends, delinquency flags, and installment estimates
- Returns attributes in a simplified` { value, error }` shape with standardized error objects
- Supports asynchronous attribute computation workflows for account requests
- Adds webhook support for account attribute lifecycle events


Available when passing` Method-Version: 2026-03-30` in the request header.


Example Response


```text
{
"id"  :   "acc_attr_cWBKqwVP87kim"  ,
"account_id"  :   "acc_4m9amk4KFiaQX"  ,
"status"  :   "completed"  ,
"attributes"  : {
"type"  : {
"value"  :   "credit_card"  ,
"error"  :   null
},
"usage_pattern"  : {
"value"  :   "revolver"  ,
"error"  :   null
},
"delinquency_flag"  : {
"value"  :   false  ,
"error"  :   null
},
"utilization"  : {
"value"  :   11.96  ,
"error"  :   null
}
},
"error"  :   null  ,
"created_at"  :   "2026-04-09T17:02:47.910Z"  ,
"updated_at"  :   "2026-04-09T17:04:35.220Z"
}


```


---


### ​


Account Attribute Subscriptions


Accounts can now be enrolled in an` attribute`[Subscription](https://docs.methodfi.com/reference/accounts/subscriptions/overview) for continuous monitoring of an Account’s financial attributes. Once enrolled, Method automatically computes new[Account Attributes](https://docs.methodfi.com/reference/accounts/attributes/overview) and notifies your team via webhooks as they change.


```text
curl   https://production.methodfi.com/accounts/{acc_id}/subscriptions   \
-X   POST   \
-H   "Method-Version: 2026-03-30"   \
-H   "Authorization: Bearer sk_WyZEWVfTcH7GqmPzUPk65Vjc"   \
-H   "Content-Type: application/json"   \
-d   '{
"enroll": "attribute"
}'


```


- Available for credit card, personal loan, and mortgage accounts
- The` attribute` subscription appears in an Account’s` available_subscriptions` once eligible


Available when passing` Method-Version: 2026-03-30` in the request header.


---


### ​


New Webhook Events


This version adds webhook coverage for the new account attribute resource lifecycle.


**New events:**


- ` entity_attribute.create`
- ` entity_attribute.update`
- ` account_attribute.create`
- ` account_attribute.update`


See[Webhooks](https://docs.methodfi.com/reference/webhooks/overview) for the complete event list.


---


## ​


Breaking Changes


- **Entity Attributes request semantics changed**


- ` POST /entities/{ent_id}/attributes` now computes all supported attributes automatically
- The legacy request body for selecting specific` credit_health_*` attributes is no longer documented for` 2026-03-30`


- **Entity Attributes response shape changed**


- Legacy attribute payloads that returned` value` and` rating` have been replaced with a simplified` { value, error }` shape
- On success,` error` is` null` . On failure,` error` contains a standard` { type, sub_type, code, message }` object (e.g.` ENTITY_ATTRIBUTE_INSUFFICIENT_DATA` code` 27001` for entity attributes,` ACCOUNT_ATTRIBUTE_INSUFFICIENT_DATA` code` 28001` for account attributes)
- Attribute names now reflect the new attribute model


- **Account Attributes response shape changed**


- Account attribute responses now use the same simplified` { value, error }` result structure
- Attribute availability now depends on the account’s liability type


- **Attributes create endpoints are asynchronous**


- Create requests now return an in-progress resource that must be polled or observed via webhook until completion


---


## ​


Why It Matters


This release makes the attributes product more useful for underwriting, servicing, monitoring, and decisioning workflows. With this release, developers can:


- Retrieve a richer set of entity- and account-level financial signals
- Build against a consistent result format across attributes
- Handle unavailable computations explicitly using standardized` error` objects with` type` ,` sub_type` ,` code` , and` message`
- Use asynchronous workflows that better fit long-running computations


---


## ​


Upgrading to 2026-03-30


The new API version is available to all existing teams. New teams are automatically pinned to **` 2026-03-30`** .


- **Request Header:** Set` Method-Version` to` 2026-03-30` . See[API Versioning](https://docs.methodfi.com/reference/versioning#api-versioning) for more information.
- **Client Libraries:** Check your Method SDK version and upgrade guidance before moving production traffic to the new API version.


> Once upgraded, rollbacks to prior API versions are not supported. Contact your Method CSM for upgrade assistance.


---


### ​


Upgrade Support


Our technical integration team is available to help review your current usage of Attributes and plan a migration to` 2026-03-30` . Contact your Method CSM for upgrade support.
