---
schema_version: "1.0.0"
document_id: "c1e4bbe41edf2b955b5523868839d0a93471c3610687255f1b89c732c0de841b"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-24fa1ca79ef4"
canonical_url: "https://docs.methodfi.com/changelog/api-versions/2024-04-04"
published_at: null
first_seen_at: "2026-07-27T10:33:39.968597+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:0164861f5f56c3efedd83d0f8d10ebd9b74cafce62d29e719510a0a8342180c0"
---

# Introducing API v2

We are excited to announce the launch of API v2 (release` 2024-04-04` ), the highly anticipated second version of Method’s API. Over the past year, our team has been dedicated to gathering feedback and working closely with our clients to create an even more powerful and developer-friendly API.


This release is packed with new features and enhancements that build upon Method’s credential-less consumer permissioned financial network.


Expanded integrations with Visa/Mastercard power new features such as real-time credit card transaction notifications, and enabling 1-click checkout experiences for merchants. This is in addition to our real-time financial institution liability updates, which have been significantly upgraded to provide better accuracy, expanded coverage, and reduced latency.


### ​


Upgrading to 2024-04-04


The new API version is available to all existing teams. New teams are automatically pinned to the API version` 2024-04-04` . During the transition phase, teams will be allowed to utilize any API version in tandem.


Note that once upgraded manually, it’s not possible to revert to older versions. Contact your Method CSM for more information on upgrade rollbacks.


- **Request Header** : Set` Method-Version` to` 2024-04-04` . See[API Versioning](https://docs.methodfi.com/reference/versioning#api-versioning) for more information.
- **Client Libraries** : For Method Libraries, the version is locked to the dated version of the API. For example, method-node v0.x.x is locked to 2020-12-06, while method-node v1.x.x is locked to 2024-04-04 and onwards.


#### ​


Upgrade Support


Our technical integration team is here to help! Contact your Method CSM for a 1:1 on best practices and support migrating to` 2024-04-04`


Personalized upgrade guides have been created for enterprise customers. Contact your Method CSM to receive your guide.


**✨ Ask Method:** The search bar of our docs now includes a conversational LLM search powered by ChatGPT. Feel free to ask any questions and receive a personalized answer.


*Sample questions:*


> How are auth_sessions different in the new version of the API?


> What are the new products in 2024-04-04?


## ​


Entities


New endpoints have been introduced for verification sessions, identities, connect operations, credit scores, products, and subscriptions. Several endpoints, including auth session, credit scores, and sensitive information retrieval, have been deprecated.


The connect endpoint now fetches all liability accounts across Method’s network of financial institutions, replacing auth_session. The verification sessions endpoint manages methods for verifying an entity’s phone and identity, while the identities endpoint retrieves the underlying identity (PII) of an entity. The products and subscriptions endpoints provide an overview of the products and continuous updates available for entities.


### ​


New Features


#### ​


The Connect endpoint


[Connect](https://docs.methodfi.com/reference/entities/connect/overview) identifies and connects all the liability accounts (e.g., credit card, mortgage, auto loans, student loans, etc.) for an entity across Method’s network of 1500+ financial institutions/lenders.


- **The Connect endpoint replaces:**


- Auth Session:` POST /entities/{ent_id}/auth_session`


See[Connect](https://docs.methodfi.com/reference/entities/connect/overview) for more information.


---


#### ​


The Credit Scores endpoint


[Credit Scores](https://docs.methodfi.com/reference/entities/credit-scores/overview) returns the latest credit score and score factors for an entity.


- **The Credit Scores endpoint replaces:**


- Credit Score:` GET /entities/{ent_id}/credit_score`


See[Credit Scores](https://docs.methodfi.com/reference/entities/credit-scores/overview) for more information.


---


#### ​


The Verification Sessions Endpoint


[Entity Verification Sessions](https://docs.methodfi.com/reference/entities/verification-sessions/overview) manage the methods for verifying an entity’s phone and identity. Entities need to verify their identity and/or phone to be used throughout the Method API. The status of an entity’s verification is returned via the` verification` field in the Entity object.


- **The Verification Sessions endpoint replaces:**


- KBA questions via Auth Session:` POST /entities/{ent_id}/auth_session` is replaced by[kba](https://docs.methodfi.com/reference/entities/verification-sessions/create-kba) entity verification session.
- Non-KBA authentication via Auth Session:` POST /entities/{ent_id}/auth_session` is replaced by[byo_kyc](https://docs.methodfi.com/reference/entities/verification-sessions/create-byo-kyc) entity verification session.
- ` individual.phone_verification_type` and` individual.phone_verification_timestamp` via the create Entity request are replaced by[byo_sms](https://docs.methodfi.com/reference/entities/verification-sessions/create-byo-sms) phone verification requirements.
- Utilizing` capabilities` ,` available_capabilities` , and` pending_capabilities` in the Entity object to verify if an entity’s identity has been matched or if more PII is required.


- This flow is now replaced by` verification.identity.verified` and` verification.identity.matched` in the` verification` field in the Entity object.


- **Upgrade notes:**


- The` verification` field in the Entity object returns the status of Identity Verification (` verification.identity` ) and Phone Verification (` verification.phone` ).
- The` method` key in the` entity.verification` object enumerates the phone and identity verifications available for your entity.


- Available verification methods differ on a team-by-team basis and are further defined by the PII provided during entity creation. See[PII Requirements](https://docs.methodfi.com/reference/entities/overview#pii-requirements) for more information.
- Teams with pre-defined PII requirements can utilize` byo_kyc` to skip KBA, and` byo_sms` to skip phone verification.


- Teams upgrading from a prior API version might have some verifications auto-generated. Contact your Method CSM for more info.
- The` status` key in an entity will only transition to` active` when all verification requirements have been completed.


See[Verification Sessions](https://docs.methodfi.com/reference/entities/verification-sessions/overview) for more information.


---


#### ​


The Identities endpoint


Entity[Identities](https://docs.methodfi.com/reference/entities/identities/overview) endpoint is used to retrieve the underlying identity (PII) of an Entity.


- **The Identities endpoint replaces:**


- Entity Sensitive:` GET /entities/{ent_id}/sensitive`


See[Identities](https://docs.methodfi.com/reference/entities/identities/overview) for more information.


---


#### ​


The Products endpoint


Entity[Products](https://docs.methodfi.com/reference/entities/products/overview) endpoint outlines the products ( *capabilities* ) an entity has access to and provides an overview of the status of all the products.


Products are Method endpoints available for an entity. Access to most products requires an entity to be active. However, some products have restricted access requiring team-by-team enablement. See[List all Products](https://docs.methodfi.com/reference/entities/products/list) for more information.


**Entity Products:**


Name Use-Case Resource Doc


` connect` On-Demand comprehensive view of an Entity’s outstanding liabilities.[Connect](https://docs.methodfi.com/reference/entities/connect/overview)


` credit_score` On-Demand view of an Entity’s credit score.[Credit Scores](https://docs.methodfi.com/reference/entities/credit-scores/overview)


` identity` On-Demand retrieval of the full Identity (PII) for any Entity[Identities](https://docs.methodfi.com/reference/entities/identities/overview)


- **The Products endpoint replaces:**


- Utilizing` capabilities` ,` available_capabilities` , and` pending_capabilities` in the Entity object to determine the capabilities an entity has access to. These fields have been deprecated from the Entity object and removed in favor of:


- ` products` (products available for this entity)
- ` restricted_products` (products not currently available, but can be made available)


See[Products](https://docs.methodfi.com/reference/entities/products/overview) for more information.


---


#### ​


The Subscriptions endpoint


Entity[Subscriptions](https://docs.methodfi.com/reference/entities/subscriptions/overview) endpoint controls the state of all subscriptions for an entity. Subscriptions are products that can provide continuous updates via webhooks (e.g., *Credit Score Subscription provides updates on an entity’s credit score* ).


**Entity Subscriptions:**


Name Use-Case Resource Doc


` connect` Comprehensive view of an Entity’s outstanding liabilities and continuous near real-time updates on new liabilities.[Connect](https://docs.methodfi.com/reference/entities/connect/overview)


` credit_score` Continuous near real-time updates on an Entity’s credit score.[Credit Scores](https://docs.methodfi.com/reference/entities/credit-scores/overview)


- **The Subscriptions endpoint replaces:**


- Custom configuration for continuous credit report pulls via your Method CSM. This has been deprecated and replaced by[Create a Subscription](https://docs.methodfi.com/reference/entities/subscriptions/create) endpoint.


See[Subscriptions](https://docs.methodfi.com/reference/entities/subscriptions/overview) for more information.


---


### ​


Breaking Changes


#### ​


Entity object changes


- ✨ Corporation entity types have been simplified to` corporation` during[Create a Corporation](https://docs.methodfi.com/reference/entities/create-corporation) entity.


- ` c_corporation` ,` s_corporation` ,` llc` ,` partnership` , and` sole_proprietorship` have been deprecated and removed in favor of` corporation`


- ✨ Providing SSN is now supported during[Create an Individual](https://docs.methodfi.com/reference/entities/create-individual) entity.


- ` individual.ssn` , and` individual.ssn_4`


- ✨ Entity verification status is now returned by the Entity object. See[Entity object](https://docs.methodfi.com/reference/entities/overview) for more information.


- ` verification`


- ✨ Products and subscription status are now returned by the Entity object. See[Entity object](https://docs.methodfi.com/reference/entities/overview) for more information.


- ` products`
- ` restricted_products`
- ` subscriptions`
- ` available_subscriptions`
- ` restricted_subscriptions`


- ⛔ Phone verification has been replaced and no longer returned by the Entity object.


- ` individual.phone_verification_type` , and` individual.phone_verification_timestamp` have been deprecated and removed in favor of[Entity Verification Sessions](https://docs.methodfi.com/reference/entities/verification-sessions/overview)


- ⛔ Entity capabilities have been deprecated and removed from the Entity object.


- ` capabilities` ,` available_capabilities` , and` pending_capabilities` have been deprecated and removed in favor of[Products](https://docs.methodfi.com/reference/entities/products/overview) ,[Subscriptions](https://docs.methodfi.com/reference/entities/subscriptions/overview) , and[Entity Verification Sessions](https://docs.methodfi.com/reference/entities/verification-sessions/overview)


- The` products` array returns the products the entity has access to, replacing` capabilities` .
- The` restricted_products` array returns the products the entity does not have access to, replacing` pending_capabilities` .
- TheEntity Products endpoint outlines the products ( *capabilities* ) an entity has access to and provides an overview of the status of all the products.


- The following fields have been modified in the[Entity object](https://docs.methodfi.com/reference/entities/overview) :


Fields removed from Entity


- ` individual.phone_verification_type`
- ` individual.phone_verification_timestamp`
- ` capabilities`
- ` available_capabilities`
- ` pending_capabilities`


Fields added to Entity


- ` individual.ssn`
- ` individual.ssn_4`
- ` verification`
- ` products`
- ` restricted_products`
- ` subscriptions`
- ` available_subscriptions`
- ` restricted_subscriptions`


#### ​


Deprecated endpoints


The following endpoints have been deprecated and removed in` 2024-04-04`


- Auth Session:` /entities/{ent_id}/auth_session` removed in favor of[Connect](https://docs.methodfi.com/reference/entities/connect/overview)
- Credit Score:` /entities/{ent_id}/credit_score` removed in favor of[Credit Scores](https://docs.methodfi.com/reference/entities/credit-scores/overview)
- Entity Sensitive:` /entities/{ent_id}/sensitive` removed in favor of[Identities](https://docs.methodfi.com/reference/entities/identities/overview)
- Refresh Capabilities:` /entities/{ent_id}/refresh_capabilities` is no longer supported in` 2024-04-04`
- Manual Auth Session:` /entities/{ent_id}/manual_auth_session` is no longer supported in` 2024-04-04`
- Entity Vehicles:` /entities/{ent_id}/vehicles` is no longer supported in` 2024-04-04`


## ​


Accounts


New endpoints have been introduced for updates, card brands, payoffs, transactions, balances, verification sessions, and sensitive information. Several endpoints, including account syncs, account verification, and sensitive information retrieval, have been deprecated.


The updates endpoint now provides real-time account data directly from financial institutions, replacing syncs, and making liability account data available through the updates endpoint.


The verification process for ACH and liability accounts has been streamlined with Account Verification Sessions.


### ​


New Features


#### ​


The Updates endpoint


[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) endpoint retrieves in real-time account data including balance, due dates, APRs, directly from the account’s financial institution. Updates is Method’s flagship endpoint and replaces the` sync` endpoint by enabling push notifications directly from the financial institution.


As part of the` 2024-04-04` release the Account object has been modified, and most Account data is now returned by an Update object. See[Account object changes](https://docs.methodfi.com/changelog/api-versions/2024-04-04#account-object-changes) for more information.


- **The Updates endpoint replaces:**


- Syncing an account using` /accounts/{acc_id}/syncs` has been deprecated and removed in favor of[Create an Update](https://docs.methodfi.com/reference/accounts/updates/create)
- Enrolling in auto syncs using` /accounts/{acc_id}/sync_enrollment` or via your Method CSM has been deprecated and removed in favor of[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) and[Subscriptions](https://docs.methodfi.com/reference/accounts/subscriptions/overview)
- Sync status fields in the Account response have been deprecated and removed in favor of[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) . Removed fields include:


- ` data_status`
- ` data_sync_type`
- ` data_last_successful_sync`
- ` data_status_error`
- ` data_source`
- ` data_updated_at`


- Enrollment into credit report-based updates via your Method CSM has been deprecated and removed in favor of the` update.snapshot` subscription type. See[Subscriptions](https://docs.methodfi.com/reference/accounts/subscriptions/overview) for more information.
- Liability data at the account level (` account.liability.<liability_type>` ) has been deprecated and removed in favor of an Update object and an expansion. See[Account](https://docs.methodfi.com/reference/accounts/overview) for more information.
- Utilizing` data_status` to determine real-time financial institution coverage has been deprecated and removed in favor of the` products` array including` updates` . See[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) for more information.


- **Upgrade notes:**


- New updates sources:` direct` and monthly` snapshot` updates.


- ` direct` : Near real-time account update (balance, due dates, etc.) from the account’s financial institution (replaces a` sync` ).
- ` snapshot` : Monthly snapshot update (balance, due dates, etc.) from the account’s financial institution (replaces credit report-based updates).


- Receiving continuous updates requires a subscription to` update` or` update.snapshot` . See[Subscriptions](https://docs.methodfi.com/reference/accounts/subscriptions/overview) for more information.
- Any account connected using Entity Connect will receive an initial` snapshot` update and a` direct` update (when available).
- Legacy accounts connected via` auth_session` have been migrated with an Update object pre-populated.


See[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) for more information.


---


#### ​


The Card Brand endpoint


[Card Brand](https://docs.methodfi.com/reference/accounts/card-brands/overview) endpoint retrieves the associated credit card metadata (Product / Brand Name, Art, etc.) directly from the card issuer. In the future, card brand webhooks will notify in real-time any card changes (e.g., card downgraded, card lost, etc.)


- **The Card Brand endpoint replaces:**


- Derived card names in the liability account field` name` . Migrating to card brand will provide accurate names across any Visa / Mastercard card regardless of issuing bank.


See[Card Brand](https://docs.methodfi.com/reference/accounts/card-brands/overview) for more information.


---


#### ​


The Payoffs endpoint


[Payoffs](https://docs.methodfi.com/reference/accounts/payoffs/overview) endpoint retrieves a payoff quote in real-time from the Account’s financial institution / lender. Payoffs are currently only available for Auto Loan and Mortgage accounts.


- **The Payoffs endpoint replaces:**


- ` payoff_amount` and` payoff_amount_term` fields within the` auto_loan` Account type response. See[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) for more information on the Account object changes.


See[Payoffs](https://docs.methodfi.com/reference/accounts/payoffs/overview) for more information.


---


#### ​


The Transactions endpoint


[Transactions](https://docs.methodfi.com/reference/accounts/transactions/overview) endpoint retrieves real-time transaction (authorization, clearing, etc) notifications for Credit Card Accounts directly from the card networks (Visa, MC).


Enrollment for transactions requires a subscription and additional consent using Method’s` connect` element.


- **The Transactions endpoint replaces:**


- Enrollment for transactions using the` auth` element has been deprecated and removed in favor of the` connect` element. See[Connect Overview](https://docs.methodfi.com/elements/connect/overview) for more information.
- Global transactions endpoint` /transactions` has been deprecated and removed in favor of account-scoped transactions. See[Transactions](https://docs.methodfi.com/reference/accounts/transactions/overview) for more information.
- ` transaction:stream` capability has been deprecated and removed in favor of the` transactions` product. See[Products](https://docs.methodfi.com/reference/accounts/products/overview) for more information.


See[Transactions](https://docs.methodfi.com/reference/accounts/transactions/overview) for more information.


---


#### ​


The Balances endpoint


[Balances](https://docs.methodfi.com/reference/accounts/balances/overview) endpoint retrieves the real-time balance from the account’s financial institution. Balance is now available for non-liability accounts such as checking accounts. Contact your Method CSM for early access to expanded Balance coverage.


- **The Balances endpoint replaces:**


- The` balance` field in` liability.<liability_type>.balance` has been deprecated and removed in favor of:


- Retrieving` balance` using the[Balances](https://docs.methodfi.com/reference/accounts/balances/overview) endpoint
- Retrieving` balance` and additional liability information using the[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) endpoint


See[Balances](https://docs.methodfi.com/reference/accounts/balances/overview) for more information.


---


#### ​


The Verification Sessions endpoint


[Account Verification Sessions](https://docs.methodfi.com/reference/accounts/verification-sessions/overview) manage the methods for verifying an Account to enable specific products for ACH or Liability accounts. For example, ACH Accounts require a verified Account Verification Session before they can be used as a source for Payments.


- **The Account Verification Sessions endpoint replaces:**


- ACH Account verification via` /accounts/{acc_id}/verification` has been deprecated and removed in favor of:


- An Account Verification Session of one of the available methods:` micro_deposits` ,` plaid` ,` mx` , or` teller` .
- Teams with corporate disbursement ACH accounts can skip verification and a verification will be auto-provisioned with the type of` auto_verify` .
- Teams with managed DDAs can whitelist a routing number to skip verification across all their DDAs (routing / account number pair).
- See[Verification Types](https://docs.methodfi.com/reference/accounts/verification-sessions/overview#verification-types) for more information. A verification of` trusted_provisioner` will be auto-provisioned. Contact your Method CSM for more information.


- Liability account number verification for AMEX, Apple Card, etc. is now handled by a` standard` Account Verification Session. See[Update a Standard Verification](https://docs.methodfi.com/reference/accounts/verification-sessions/update-standard) for more information.
- Instant Link CVV verification is now handled by a` pre_auth` Account Verification Session. See[Update a Pre-auth Verification](https://docs.methodfi.com/reference/accounts/verification-sessions/update-preauth) for more information.


- **Upgrade notes:**


See[Account Verification Sessions](https://docs.methodfi.com/reference/accounts/verification-sessions/overview) for more information.


---


#### ​


The Sensitive endpoint


[Sensitive](https://docs.methodfi.com/reference/accounts/sensitive/overview) endpoint returns underlying sensitive account information (e.g., PAN, CVV, account number). This product is only available for liabilities and requires verification on a team-by-team basis.


- **The Sensitive endpoint replaces:**


- ` GET /accounts/{acc_id}/sensitive` is deprecated and removed in favor of[Create a Sensitive](https://docs.methodfi.com/reference/accounts/sensitive/create) .


See[Sensitive](https://docs.methodfi.com/reference/accounts/sensitive/overview) for more information.


---


#### ​


The Products endpoint


Account[Products](https://docs.methodfi.com/reference/accounts/products/overview) endpoint outlines the products ( *capabilities* ) an account has access to and provides an overview of the status of all the products.


Products are Method endpoints available for an account. Most products are accessible by default. However, some products have restricted access requiring team-by-team enablement and elevated account verification.


**Account Products:**


Name Use-Case Supported Types Resource Doc


` update` On-Demand real-time account update (balance, due dates, etc.) from the account’s financial institution All liabilities[Updates](https://docs.methodfi.com/reference/accounts/updates/overview)


` balance` On-Demand real-time balance from the account’s financial institution All liabilities[Balances](https://docs.methodfi.com/reference/accounts/balances/overview)


` card_brand` On-Demand retrieval of credit card metadata (Product / Brand Name, Art, etc.) directly from the card issuer Credit Cards[Card Brand](https://docs.methodfi.com/reference/accounts/card-brands/overview)


` payoff` On-Demand retrieval of auto loan payoff (amount, per diem, etc.) from the account’s financial institution Auto Loans[Payoffs](https://docs.methodfi.com/reference/accounts/payoffs/overview)


` payment` Next day electronic push payments to the account’s financial institution All liabilities[Payments](https://docs.methodfi.com/reference/payments/overview)


` sensitive` On-Demand retrieval of underlying sensitive account information (PAN, CVV, account number) All liabilities[Sensitive](https://docs.methodfi.com/reference/accounts/sensitive/overview)


- **The Products endpoint replaces:**


- Utilizing` capabilities` ,` available_capabilities` , and` pending_capabilities` in the Account object to determine the capabilities an account has access to. These fields have been deprecated from the Account object and removed in favor of:
- ` products` (products available for this Entity)


See[Products](https://docs.methodfi.com/reference/accounts/products/overview) for more information.


---


#### ​


The Subscriptions endpoint


Account[Subscriptions](https://docs.methodfi.com/reference/accounts/subscriptions/overview) endpoint controls the state of all subscriptions for an account. Subscriptions are products that can provide continuous updates via Webhooks. ( *e.g., Transaction Subscription provides real-time updates on a Credit Card’s transactions* ).


**Account Subscriptions:**


Name Use-Case Supported Types Resource Doc


` transactions` Real-time transaction (authorization, purchases, etc.) notifications for credit card accounts Credit Cards[Transactions](https://docs.methodfi.com/reference/accounts/transactions/overview)


` update` Near real-time account update (balance, due dates, etc.) from the account’s financial institution All liabilities[Updates](https://docs.methodfi.com/reference/accounts/updates/overview)


` update.snapshot` Monthly account snapshot update (balance, due dates, etc.) from the credit bureau All liabilities[Updates](https://docs.methodfi.com/reference/accounts/updates/overview)


- **The Subscriptions endpoint replaces:**


- Custom configuration for continuous syncs via your Method CSM. This has been deprecated and replaced by[Create a Subscription](https://docs.methodfi.com/reference/accounts/subscriptions/create) endpoint.
- Account Sync Enrollment` /accounts/{acc_id}/sync_enrollment` for enrollment in auto syncs. This has been deprecated and replaced with an Updates subscription. See[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) and[Create a Subscription](https://docs.methodfi.com/reference/accounts/subscriptions/create) for more information.
- ` data_sync_type` has been removed from the Account endpoint. See[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) and[List all Subscriptions](https://docs.methodfi.com/reference/accounts/subscriptions/list) for more information.


See Account[Subscriptions](https://docs.methodfi.com/reference/accounts/subscriptions/overview) for more information.


---


### ​


Breaking Changes


#### ​


Account object changes


- ✨ Product properties are now included in the Account object. By default, the ID of the latest product resource is returned and can be expanded in-line using the` expand` query param. See[Expanding Resources](https://docs.methodfi.com/reference/expanding) for more information.


- ` latest_verification_session`
- ` update`
- ` balance`
- ` card_brand`
- ` payoff`


- ✨ Products and Subscription status are now returned by the Account object. See[Account object](https://docs.methodfi.com/reference/accounts/overview) for more information.


- ` products`
- ` restricted_products`
- ` subscriptions`
- ` available_subscriptions`
- ` restricted_subscriptions`


- ✨ Liability name and fingerprint are now returned by the Account object. See[Account object](https://docs.methodfi.com/reference/accounts/overview) for more information.


- ` liability.name`
- ` liability.fingerprint`


- ✨ New account liability types are now supported and returned by the Account object. See[Account Liability Types](https://docs.methodfi.com/reference/accounts/overview#account-liability-types) for more information.


- ⛔ Liability data under the` account.liability.<liability_type>` property has been deprecated and removed in favor of[Updates](https://docs.methodfi.com/reference/accounts/updates/overview)


- The` updates` field returns the ID of the latest update and can be expanded to return liability account data in-line using the` expand` query param. See[Expanding Resources](https://docs.methodfi.com/reference/expanding) for more information.


- ⛔ Syncing liability data using` /accounts/{acc_id}/syncs` has been deprecated and removed in favor of[Updates](https://docs.methodfi.com/reference/accounts/updates/overview)


- Additionally, data sync status fields at the account liability object have been deprecated and removed in favor of status fields within an[Update](https://docs.methodfi.com/reference/accounts/updates/overview)
- ` liability.data_status` ,` liability.data_sync_type` ,` liability.data_sync_type` ,` liability.data_last_successful_sync` ,` liability.data_source` ,` liability.data_updated_at` ,` liability.data_updated_at` ,` liability.data_status_error`


- ⛔ Account capabilities have been deprecated and removed from the Account object.


- ` capabilities` ,` available_capabilities` , and` pending_capabilities` have been deprecated and removed in favor of[Products](https://docs.methodfi.com/reference/accounts/products/overview) ,[Subscriptions](https://docs.methodfi.com/reference/accounts/subscriptions/overview) , and[Account Verification Sessions](https://docs.methodfi.com/reference/accounts/verification-sessions/overview)


- The` products` array returns the products the account has access to, replacing` capabilities` .
- The` restricted_products` array returns the products the account does not have access to, replacing` pending_capabilities` .
- The account[Products](https://docs.methodfi.com/reference/accounts/products/overview) endpoint outlines the products ( *capabilities* ) an account has access to and provides an overview of the status of all the products.


- ⛔ Account payment status (` liability.payment_status` ) has been deprecated and removed in favor of[Products](https://docs.methodfi.com/reference/accounts/products/overview)


- ⛔ Account clearing (` clearing` ) and liability hash (` liability.hash` ) has been deprecated and removed in` 2024-04-04`


- The following fields have been modified in the[Account object](https://docs.methodfi.com/reference/accounts/overview) :


Fields removed from Account


- ` liability.payment_status`
- ` liability.data_status`
- ` liability.data_sync_type`
- ` liability.data_last_successful_sync`
- ` liability.data_source`
- ` liability.data_updated_at`
- ` liability.data_status_error`
- ` liability._liability_type_` - where` liability_type` is any of the[Account Liability Types](https://docs.methodfi.com/reference/accounts/overview#account-liability-types) .
- ` liability.hash`
- ` clearing`
- ` capabilities`
- ` available_capabilities`
- ` pending_capabilities`


Fields added to Account


- ` liability.name`
- ` liability.fingerprint`
- ` latest_verification_session`
- ` update`
- ` balance`
- ` card_brand`
- ` payoff`
- ` products`
- ` restricted_products`
- ` subscriptions`
- ` available_subscriptions`
- ` restricted_subscriptions`


#### ​


Deprecated endpoints


- Account Syncs:` /accounts/{acc_id}/syncs` removed in favor of[Updates](https://docs.methodfi.com/reference/accounts/updates/overview)
- Account Sync Enrollment:` /accounts/{acc_id}/sync_enrollment` removed in favor of[Updates](https://docs.methodfi.com/reference/accounts/updates/overview) and[Subscriptions](https://docs.methodfi.com/reference/accounts/subscriptions/overview)
- Account Verification:` /accounts/{acc_id}/verification` removed in favor of[Account Verification Sessions](https://docs.methodfi.com/reference/accounts/verification-sessions/overview)
- Account Sensitive:` /accounts/{acc_id}/sensitive` removed in favor of[Sensitive](https://docs.methodfi.com/reference/accounts/sensitive/overview)
- Account Card:` /accounts/{acc_id}/card` removed in favor of[Card Brand](https://docs.methodfi.com/reference/accounts/card-brands/overview) ,[Sensitive](https://docs.methodfi.com/reference/accounts/sensitive/overview) and Credit Card[Account Verification Sessions](https://docs.methodfi.com/reference/accounts/verification-sessions/overview)
- Account Payment History:` /accounts/{acc_id}/payment_history` is no longer supported in` 2024-04-04`
- Account Details:` /accounts/{acc_id}/details` is no longer supported in` 2024-04-04`
- Bulk Sensitive:` /accounts/{acc_id}/bulk_sensitive` is no longer supported in` 2024-04-04`
- Bulk Sync:` /accounts/{acc_id}/bulk_sync` is no longer supported in` 2024-04-04`
- Account Vehicle:` /accounts/{acc_id}/match_vehicle` is no longer supported in` 2024-04-04`
- Routing Numbers:` /routing_numbers` is no longer supported in` 2024-04-04`
- BINs:` /bins` is no longer supported in` 2024-04-04`


## ​


Additional Changes


### ​


Expand Parameter


New` expand` query parameter for expanding certain properties from an ID to their respective objects in-line in the response body.


Applicable to both Entities and Accounts, allowing expansion of fields such as` connect` ,` credit_score` ,` update` ,` balance` ,` payoff` ,` sensitive` ,` transactions` ,` card_brand` , and` latest_verification_session` .


See[Expanding Resources](https://docs.methodfi.com/reference/expanding) for more information.


### ​


Elements


Introducing the[Connect element](https://docs.methodfi.com/elements/connect/overview) as the primary way to connect user accounts and complete necessary verifications.


The Auth element has been deprecated in favor of Connect.


See[Connect guide](https://docs.methodfi.com/elements/connect/overview) for step-by-step instructions on leveraging Connect.
