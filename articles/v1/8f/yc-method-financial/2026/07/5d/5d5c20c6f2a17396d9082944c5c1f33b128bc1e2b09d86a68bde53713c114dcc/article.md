---
schema_version: "1.0.0"
document_id: "5d5c20c6f2a17396d9082944c5c1f33b128bc1e2b09d86a68bde53713c114dcc"
company_key: "yc-method-financial"
company: "Method Financial"
source_id: "yc-method-financial-news-import-24fa1ca79ef4"
canonical_url: "https://docs.methodfi.com/changelog/2026/april"
published_at: null
first_seen_at: "2026-07-27T10:33:39.968597+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:83c09a679e6c92d2958aa31ed8254c0ee8d2b35a232015a946c1a376377c72a6"
---

# April Updates

## ​


New Features


### ​


Attributes V2: Entity & Account Attributes


A new version of the[Attributes API](https://docs.methodfi.com/reference/entities/attributes/overview) introduces 50+ new computed attributes across both Entities and Accounts, giving you a more complete, up-to-date picture of a user’s financial health:


- **Utilization & Credit Health** : See a user’s credit utilization broken out by account type, with 30/60/90-day trend lines and delta calculations that show whether utilization is rising or falling over time. Includes delinquency flags and payment-to-minimum ratios that help identify users who may be falling behind.
- **Balance & Debt Movement** : Track how balances are changing across a user’s full liability portfolio with 30/60/90-day balance change attributes. Understand whether a user is paying down debt or accumulating it, across credit cards, personal loans, mortgages, and installment accounts.
- **Cost of Debt** : Weighted average APR calculations across liability types help you understand what a user is actually paying in interest, useful for refinance targeting, rate optimization, and cross-sell opportunities.
- **Payment Behavior** : Monthly installment estimates, minimum payment totals, and autopay detection give visibility into how a user is managing their obligations and whether they’re likely to stay current.


V2 attributes are available at both the Entity level (aggregated across all of a user’s accounts) and the Account level (per-account metrics). V2 attributes support webhook delivery and subscriptions, and can be simulated in the development environment via the[Simulations API](https://docs.methodfi.com/reference/simulations/attributes/create) .


For more information, visit the[Attributes API](https://docs.methodfi.com/reference/entities/attributes/overview) documentation.


### ​


Opal Embeddable Mode


[Opal](https://docs.methodfi.com/opal/overview) now supports a fully embeddable rendering mode for JavaScript and React integrations. The new embeddable mode allows Opal to be rendered inline within your application’s DOM rather than in a modal overlay, giving you full control over layout and positioning.


- **React SDK** : The` @methodfi/opal-react` package has been updated to v1.5.0 with embeddable mode support.
- **JavaScript SDK** : Native JavaScript integrations can now pass` mode: "embeddable"` when initializing Opal.


For more information, visit the[Opal Libraries](https://docs.methodfi.com/opal/libraries) documentation.


### ​


Managed Accounts API


A new[Managed Accounts API](https://docs.methodfi.com/reference/managed-accounts/overview) provides programmatic access to Method-managed FBO accounts. You can now retrieve managed account balance information and list transactions for reconciliation:


- ` GET /managed_accounts` — list all Managed Accounts on your team
- ` GET /managed_accounts/{macc_id}` — retrieve a specific Managed Account
- ` GET /managed_accounts/{macc_id}/transactions` — list transactions for a Managed Account


For more information, visit the[Managed Accounts API](https://docs.methodfi.com/reference/managed-accounts/overview) documentation.


## ​


Improvements


### ​


Opal UI


- **CVV Autofill Detection** : Browsers that autofill CVV fields were sometimes causing silent submission failures because Opal didn’t detect the autofilled value. This is now handled correctly, so users who rely on password managers or browser autofill won’t hit unexpected errors.
- **Input Trimming** : Whitespace is now automatically stripped from user inputs before submission. This catches a common issue where users copy-paste account numbers or card details with trailing spaces, which would previously cause validation failures.


For more information, visit the[Opal](https://docs.methodfi.com/opal/overview) documentation.


### ​


Webhooks


**Improved Duplicate Detection**


Previously, webhooks with the same endpoint URL and different authentication credentials (for example, separate webhook configs for staging and production that share a URL) were treated as duplicates, delivering to only one of them. Deduplication now accounts for authentication configuration, so each distinct webhook registration receives its own deliveries as expected.


For more information, visit the[Webhooks API](https://docs.methodfi.com/reference/webhooks/overview) documentation.
