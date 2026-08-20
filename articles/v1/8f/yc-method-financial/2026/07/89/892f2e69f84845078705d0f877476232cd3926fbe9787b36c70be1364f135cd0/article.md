---
schema_version: "1.0.0"
document_id: "892f2e69f84845078705d0f877476232cd3926fbe9787b36c70be1364f135cd0"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-24fa1ca79ef4"
canonical_url: "https://docs.methodfi.com/changelog/api-versions/2025-12-01"
published_at: null
first_seen_at: "2026-07-27T10:33:39.968597+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:81ee6d8fd7125c75ab43403b89e98bd987e7b61074c7242ac9242f400ca3db51"
---

# Introducing API Version 2025-12-01

We’re excited to announce the release of **API version` 2025-12-01`** , featuring the new **Payment Instruments API** for inbound funding workflows. This version enables liability accounts to receive direct payments via ACH or wire transfer — allowing third parties to send funds directly to liability accounts without requiring explicit Payment creation.


---


## ​


What’s New


### ​


Payment Instruments API


Method now supports **inbound funding workflows** with the introduction of a new` PaymentInstrument` type:` inbound_achwire_payment` . This feature allows liability accounts (e.g., loans, credit cards) to receive direct payments via ACH or wire transfer.


**Key features:**


- Provisions a unique virtual account number and routing number linked to a specific Method Account, effectively making the underlying liability routable
- Third parties can send funds directly to these credentials, and Method automatically applies the funds to the associated account, without requiring you to explicitly create a Payment
- Supports use cases such as payroll, refinancing, and other direct payment scenarios


**How it works:**


1. **Create Instrument** : Create a Payment Instrument of type` inbound_achwire_payment` for a specific Method Account
2. **Receive Banking Details** : Method returns unique` account_number` and` routing_number`
3. **Share with a Third Party** : Provide these banking details to a third party (e.g., payroll provider, refinance lender, or another bank)
4. **Inbound Transfer** : When funds are sent to these credentials, Method receives the inbound ACH or wire transfer
5. **Automatic Application** : Method automatically applies the received funds to the associated account


Available when passing` Method-Version: 2025-12-01` in the request header.


Example Request


```text
curl   https://production.methodfi.com/accounts/acc_4m9amk4KFiaQX/payment_instruments   \
-X   POST   \
-H   "Method-Version: 2025-12-01"   \
-H   "Authorization: Bearer sk_WyZEWVfTcH7GqmPzUPk65Vjc"   \
-H   "Content-Type: application/json"   \
-d   '{
"type": "inbound_achwire_payment"
}'


```


Example Response


```text
{
"id"  :   "pmt_inst_cLbaArHhfDBDf"  ,
"account_id"  :   "acc_GAzrD99cUqGEN"  ,
"type"  :   "inbound_achwire_payment"  ,
"card"  :   null  ,
"network_token"  :   null  ,
"inbound_achwire_payment"  : {
"account_number"  :   "2517228863"  ,
"routing_number"  :   "026015244"
},
"chargeable"  :   true  ,
"status"  :   "completed"  ,
"error"  :   null  ,
"created_at"  :   "2025-12-11T00:12:58.639Z"  ,
"updated_at"  :   "2025-12-11T00:12:58.639Z"
}


```


See[Payment Instruments](https://docs.methodfi.com/reference/accounts/payment-instruments/overview) for more information.


---


### ​


Simulate API


The[Simulate API](https://docs.methodfi.com/reference/simulations/overview) now supports simulating inbound ACH / wire payments for` inbound_achwire_payment` Payment Instruments — making it easier for developers to test inbound payment flows in the development environment.


**[Simulate Inbound ACH/Wire Payments](https://docs.methodfi.com/reference/simulations/payment_instruments/create)**
This endpoint allows developers to simulate receiving an inbound payment and triggers the same automatic payment application logic used in production.


Available when passing` Method-Version: 2025-12-01` in the request header.


This API is available in the development environment only.


For more information, visit the[Simulations API](https://docs.methodfi.com/reference/simulations/overview) documentation.


---


## ​


Breaking Changes


- **Payment Instrument product names changed**


- The` payment_instrument` product name has been split into three separate product names:


- ` payment_instrument.card`
- ` payment_instrument.network_token`
- ` payment_instrument.inbound_achwire_payment`


- In previous versions (` 2024-04-04` ,` 2025-07-04` ), the Account object’s` products` ,` restricted_products` ,` subscriptions` ,` available_subscriptions` , and` restricted_subscriptions` arrays contained the single product name` payment_instrument` .
- In` 2025-12-01` , these arrays now contain the three separate product names instead of the single` payment_instrument` name.


---


## ​


Why It Matters


This release enables new inbound funding workflows that simplify payment collection for liability accounts. By introducing` inbound_achwire_payment` Payment Instruments, developers can now:


- Enable third parties to send payments directly to liability accounts without explicit Payment creation
- Streamline payroll, refinancing, and other direct payment scenarios
- Automatically apply inbound transfers to the associated account
- Test inbound payment flows more easily with enhanced Simulate API support


---


## ​


Upgrading to 2025-12-01


The new API version is available to all existing teams. New teams are automatically pinned to **` 2025-12-01`** .


- **Request Header:** Set` Method-Version` to` 2025-12-01` . See[API Versioning](https://docs.methodfi.com/reference/versioning#api-versioning) for more information
- **Client Libraries:**` method-node` and` method-python` **v2.1.0+** default to this version


> Once upgraded, rollbacks to prior API versions are not supported. Contact your Method CSM for upgrade assistance.


---


### ​


Upgrade Support


Our technical integration team is available to guide you through the migration. Contact your Method CSM to schedule a review of your current payment integration or to receive a personalized upgrade checklist.
