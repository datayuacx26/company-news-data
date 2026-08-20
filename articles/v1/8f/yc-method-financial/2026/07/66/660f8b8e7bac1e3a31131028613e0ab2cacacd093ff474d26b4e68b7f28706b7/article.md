---
schema_version: "1.0.0"
document_id: "660f8b8e7bac1e3a31131028613e0ab2cacacd093ff474d26b4e68b7f28706b7"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-24fa1ca79ef4"
canonical_url: "https://docs.methodfi.com/changelog/2026/january"
published_at: null
first_seen_at: "2026-07-27T10:33:39.968597+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:56274b4330294f75f333501de4a232ffb0747203aba9362e95e4ef69576f13fc"
---

# January Updates

## ​


New Features


### ​


Forwarding Requests API


The[Forwarding Requests API](https://docs.methodfi.com/reference/forwarding-requests/overview) allows you to securely proxy sensitive data (such as card details) to third-party services without your system ever handling the raw data. This enables use cases like payment processing, tokenization, and partner integrations while maintaining the PCI compliance of existing solutions.


**Key capabilities:**


- **Secure data transmission** : Send templated HTTP requests to whitelisted third-party endpoints.
- **Dynamic data injection** : Use template placeholders (e.g.,` {{my_binding.card.number}}` ) instead of raw values.
- **Binding resolution** : Bind placeholders to Method resources (currently[Payment Instruments](https://docs.methodfi.com/reference/accounts/payment-instruments/overview) ), allowing Method to securely append the underlying data without ever touching your company database.
- **Destination whitelisting** : Requests can only be sent to pre-approved URLs to prevent data exfiltration.
- **Transparent responses** : The third-party response is forwarded directly back to your application without requiring you to make changes to the way you ingest third-party response data.


For more information, visit the[Forwarding Requests API](https://docs.methodfi.com/reference/forwarding-requests/overview) documentation.


### ​


Secrets API


The[Secrets API](https://docs.methodfi.com/reference/secrets/overview) allows you to securely store sensitive configuration values (such as API keys or credentials) for use within Method. Secrets are encrypted at rest and never exposed back to client applications.


**Key capabilities:**


- **Encrypted storage** : Store sensitive values and receive a Secret ID (` sec_...` ) in return.
- **No client exposure** : Secret values are never returned in API responses.
- **Forwarding integration** : Secrets can be inserted into[Forwarding Requests](https://docs.methodfi.com/reference/forwarding-requests/overview) for authenticating with third-party services.


For more information, visit the[Secrets API](https://docs.methodfi.com/reference/secrets/overview) documentation.


### ​


Opal Appearance Customization


You can now customize the visual appearance of[Opal](https://docs.methodfi.com/opal/overview) , Method’s embeddable UI, to better match your application’s branding or autofit to your users’ devices.


When opening Opal with the client SDK, specify an` appearance` option with one of the following values:


- ` light` : Standard light theme (default)
- ` dark` : Dark theme for low-light environments
- ` system` : Automatically matches the user’s device settings


If no appearance is specified, Opal defaults to the` light` theme.


For more information, visit the[Opal Libraries](https://docs.methodfi.com/opal/libraries) documentation.


## ​


Improvements


### ​


Payment Instruments API


This release introduces several improvements to the[Payment Instruments](https://docs.methodfi.com/reference/accounts/payment-instruments/overview) product, focused on lifecycle management and subscription control.


**Lifecycle Management for Inbound ACH/Wire**


A new endpoint allows you to close inbound ACH/Wire payment instruments when they are no longer needed. This is useful for decommissioning payment routes after a refinance completes or when a funding relationship ends.


- **New endpoint** :` DELETE /accounts/:accountId/payment_instruments/:pmtInsId`
- **Behavior** : Supported only for` inbound_achwire_payment` instruments. Updates instrument status to` closed` and sets` chargeable` to` false` .


For more information, see the[Payment Instruments API](https://docs.methodfi.com/reference/accounts/payment-instruments/overview) documentation.


**Granular Subscription Types**


The` payment_instrument`[subscription](https://docs.methodfi.com/reference/accounts/subscriptions/overview) has been split into two more specific subscription types, giving you more flexibility in determining which webhook events you receive:


- ` payment_instrument.card` : Triggered for card-level updates (e.g., PAN changed, expiration updated)
- ` payment_instrument.network_token` : Triggered for network token updates (e.g., token provisioned, status changed)


For API version` 2025-12-01` +, clients must use the new specific subscription names.


### ​


Accounts API


**Collection Account Support**


The[Account Sensitive](https://docs.methodfi.com/reference/accounts/sensitive/overview) product now supports the` collection` account type. This enables you to retrieve sensitive account details (such as account numbers) for accounts that have been sent to collections, expanding coverage for debt management and recovery workflows.


### ​


Attributes API


**Support for Closed Accounts**


You can now retrieve[Account Attributes](https://docs.methodfi.com/reference/accounts/attributes/overview) for closed liability accounts. Previously, querying Attributes for a closed account would return an error. With this update, derived financial insights (such as payment history summaries and balance trends) remain accessible even after an account has been closed—useful for historical reporting and analytics.
