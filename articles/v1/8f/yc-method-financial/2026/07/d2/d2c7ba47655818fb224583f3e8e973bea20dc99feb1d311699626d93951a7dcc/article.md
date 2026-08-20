---
schema_version: "1.0.0"
document_id: "d2c7ba47655818fb224583f3e8e973bea20dc99feb1d311699626d93951a7dcc"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-24fa1ca79ef4"
canonical_url: "https://docs.methodfi.com/changelog/api-versions/2025-07-04"
published_at: null
first_seen_at: "2026-07-27T10:33:39.968597+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:742a18861ee7ef1f9e373a7fd97419e9e5e19a7e39e426c2dc7c778373d56762"
---

# Introducing API Version 2025-07-04

We’re excited to announce the release of **API version` 2025-07-04`** , featuring key improvements to our **Card Brand** model and the introduction of the new **Card Products API** . This version enhances data consistency across card networks, issuers, and products — enabling more structured integrations and cleaner brand experiences for your users.


---


## ​


What’s New


### ​


Card Brand API


The[Card Brand API](https://docs.methodfi.com/reference/accounts/card-brands/overview) has been expanded with a **richer, more structured object** . This change makes it easier to **identify, display, and integrate** specific card products while ensuring consistent metadata across networks and issuers.


**Key improvements:**


- Brand data is now **fully nested** , encapsulating` network` ,` issuer` , and visual assets under each brand.
- New fields such as` card_product_id` ,` description` , and` type` provide more granular control when mapping and displaying cards.
- Removed legacy fields (` last4` ,` shared` ) for a cleaner, product-centric response format.


Available when passing` Method-Version: 2025-07-04` in the request header.


Previous Response


```text
{
"id"  :   "cbrd_eVMDNUPfrFk3e"  ,
"account_id"  :   "acc_yVf3mkzbhz9tj"  ,
"network"  :   "visa"  ,
"issuer"  :   "Chase"  ,
"last4"  :   "8623"  ,
"brands"  : [
{
"id"  :   "brand_AMxtjQzAfDCRP"  ,
"name"  :   "Chase Sapphire Reserve"  ,
"url"  :   "https://static.methodfi.com/card_brands/1b7ccaba6535cb837f802d968add4700.png"
}
],
"status"  :   "completed"  ,
"shared"  :   false  ,
"source"  :   "network"  ,
"error"  :   null  ,
"created_at"  :   "2024-03-19T01:05:37.247Z"  ,
"updated_at"  :   "2024-03-19T01:05:37.247Z"
}


```


New Response


```text
{
"id"  :   "cbrd_Accm7P6t6mYQP"  ,
"account_id"  :   "acc_LxwEqNicr66yP"  ,
"brands"  : [
{
"id"  :   "pdt_15_brd_1"  ,
"card_product_id"  :   "pdt_15"  ,
"description"  :   "Chase Sapphire Reserve"  ,
"name"  :   "Chase Sapphire Reserve"  ,
"issuer"  :   "Chase"  ,
"network"  :   "visa"  ,
"network_tier"  :   "infinite"  ,
"type"  :   "specific"  ,
}
],
"status"  :   "completed"  ,
"source"  :   "method"  ,
"error"  :   null  ,
"created_at"  :   "2025-08-12T00:56:50.139Z"  ,
"updated_at"  :   "2025-08-12T00:56:50.139Z"
}


```


---


### ​


Card Products API


We’re introducing the new[Card Products API](https://docs.methodfi.com/reference/card-products/overview) — a **directory of card products** that allows you to retrieve all associated` Card Brand` objects for a given product.


This endpoint gives developers a unified way to reference and display card metadata (art, network, tier, and issuer) across all variants of a card family.


Available when passing` Method-Version: 2025-07-04` in the request header.


Example Request


```text
curl   https://production.methodfi.com/card_products/pdt_17   \
-H   "Method-Version: 2025-07-04"   \
-H   "Authorization: Bearer sk_WyZEWVfTcH7GqmPzUPk65Vjc"


```


Example Response


```text
{
"id"  :   "pdt_17"  ,
"name"  :   "Chase Freedom"  ,
"issuer"  :   "Chase"  ,
"type"  :   "specific"  ,
"brands"  : [
{
"id"  :   "pdt_17_brd_1"  ,
"description"  :   "Chase Freedom"  ,
"network"  :   "visa"  ,
"network_tier"  :   "signature"  ,
"default_image"  :   "https://static.methodfi.com/card_brands/fb5fbd6a5d45b942752b9dc641b93d1f.png"
},
{
"id"  :   "pdt_17_brd_2"  ,
"description"  :   "Chase Freedom"  ,
"network"  :   "visa"  ,
"network_tier"  :   "standard"  ,
"default_image"  :   "https://static.methodfi.com/card_brands/6cb697528b0771f982f7c0e8b8869de3.png"
}
]
}


```


---


## ​


Breaking Changes


- **Card Brand response shape updated**


- ` network` and` issuer` are now nested under each` brand` .
- New fields:` card_product_id` ,` description` ,` type` .
- Removed fields:` last4` ,` shared` .


---


## ​


Why It Matters


This release unifies how card data is modeled and consumed across the Method platform. By standardizing the` Card Brand` object and introducing a canonical` Card Products` directory, developers can now:


- Reliably map and display card details across networks and issuers
- Build consistent card selection and branding experiences in their apps
- Reduce dependency on ad-hoc naming or inferred card data


---


## ​


Upgrading to 2025-07-04


The new API version is available to all existing teams. New teams are automatically pinned to **` 2025-07-04`** .


- **Request Header:** Set` Method-Version` to` 2025-07-04` . See[API Versioning](https://docs.methodfi.com/reference/versioning#api-versioning) for more information.
- **Client Libraries:**` method-node` and` method-python` **v2.0.0+** default to this version.


> Once upgraded, rollbacks to prior API versions are not supported. Contact your Method CSM for upgrade assistance.


---


### ​


Upgrade Support


Our technical integration team is available to guide you through the migration. Contact your Method CSM to schedule a review of your current card integration or to receive a personalized upgrade checklist.
