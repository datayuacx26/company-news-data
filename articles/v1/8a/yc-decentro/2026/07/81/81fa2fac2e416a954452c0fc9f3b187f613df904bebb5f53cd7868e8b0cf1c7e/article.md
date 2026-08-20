---
schema_version: "1.0.0"
document_id: "81fa2fac2e416a954452c0fc9f3b187f613df904bebb5f53cd7868e8b0cf1c7e"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-news-import-cdeb59691175"
canonical_url: "https://docs.decentro.tech/changelog/flow-release-notes-390-1"
published_at: null
first_seen_at: "2026-07-25T01:33:54.013008+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:fee4c7a22e700775e4db8c2b5ef44ce2bc890638917433e5e9d586900d8e2acf"
---

# Flow Release Notes | Version 3.9.0

[Back to All](https://docs.decentro.tech/changelog)


added


This release notes contain changes related to **UPI Collections V3** .


###


UPI Collections V3


**Release Date:** 25th May 2026


**Feature 1 -** Merchant UI Template Enhancements (Shorten Link / QR Page)


To make the payer experience more intuitive and information-rich, new UI components and styling enhancements have been introduced on the payment page.


**What’s changed?**


- Merchant Logo Support


- Merchant logo will now be displayed on the payment page, if provided.


- Purpose Message


- The` purpose_message` passed during Payment Link creation will now be displayed on the payment page.


- Payment Attempt Number


- For auto-retry use cases, the current payment attempt number will now be displayed.
- Example: Payment Attempt 2
- Rendered below the expiry timer.


- Cautionary Message


- A hardcoded payer warning has been introduced to reduce payment failures caused by screenshots:
` Please SCAN and PAY & Payments initiated via screenshots will lead to failures.`


**Feature 2 -** Intent Link Merchant Name Standardisation


To improve consistency between merchant information propagated by Decentro and the merchant details displayed by TPAPs/NPCI, the intent link generation logic has been updated.


- **What’s changed?**


- The payee name parameter in intent links will now use the merchant's **legal/full name** instead of bank account name.
- The same legal name will be propagated in the TPAP while making payments.
- Applicable across:


- Payment Links (PL)
- QR
- RPD
- Autopay – Register Mandate


- No action required from merchants.
