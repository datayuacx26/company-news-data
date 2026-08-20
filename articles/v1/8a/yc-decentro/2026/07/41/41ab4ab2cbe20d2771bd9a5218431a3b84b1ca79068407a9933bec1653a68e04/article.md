---
schema_version: "1.0.0"
document_id: "41ab4ab2cbe20d2771bd9a5218431a3b84b1ca79068407a9933bec1653a68e04"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-news-import-cdeb59691175"
canonical_url: "https://docs.decentro.tech/changelog/flow-advisory-enach-mandates-111"
published_at: null
first_seen_at: "2026-07-25T01:33:54.013008+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:fc640a34a7ae3a8337fdaca02d16dd239373505043e28b04f14472ff324d99fe"
---

# Flow Advisory | eNach Mandates | Version 1.1.1

[Back to All](https://docs.decentro.tech/changelog)


added


##


Advisory : Upcoming Changes to eNACH Mandate Presentation API & Callbacks


Deployment Date : TBD
**Impact area :** Mandate Presentation - Get Presentation Status (GPS) API & Presentation Callbacks


This is to inform you that we are enhancing our eNACH Mandate Presentation responses to provide clearer, more actionable failure reasons when a mandate presentation fails. This involves renaming some existing response keys and introducing new fields in both the Get Presentation Status API and Callback payloads.
Action may be required at your end if your integration parses or maps any of the affected response fields.


**What's Changing?**


**Key Renames :**
The following existing keys are being renamed for consistency across APIs:


- In the Get Presentation Status API` transaction_status` is getting renamed to` presentation_status` .
- In the Callback` status` is getting renamed to` presentation_status` .
- In the Callback` message` is getting renamed to` presentation_status_description` .


⚠️ Action Required: If your system reads` transaction_status` ,` status` , or` message` from our responses, please update your parsing logic to use the new key names.


1. **Introducing new fields :**


The following new fields will be added to both the Get Presentation Status API and Callback responses:


- In the Get Presentation Status API we are introducing` presentation_status_description` . It's a human-readable description of the presentation outcome (e.g., "Mandate presentation successfully initiated" or the specific failure reason).
- In both Get Presentation Status API and the callback we are introducing a new field called` error_key` . A failure category bucket for programmatic grouping (e.g.,` error_account_issue` ,` error_provider_error` ,` error_at_remitter_bank` ). It will be present only in case of failures.
- In both Get Presentation Status API and the callback we are introducing a new filed called` error_key_description` . The raw NPCI error code and message in the format \[` provider_status_code` \] - \[` provider_status_description` \] / (e.g., 68 - Account Blocked/Frozen). It will be present only in case of failures.


**What You Need To Do?**


- Update your response parsers to handle the renamed keys (` presentation_status` instead of` transaction_status` /` status` , and` presentation_status_description` instead of` message` in callbacks).
- Leverage the new fields (` error_key` ,` error_key_description` ) to build better failure handling, retry logic, or dashboard reporting on your end.


If you have questions or need assistance updating your integration, please reach out to us.


##


Advisory : Enhanced Error Reporting for Mandate Registration.


Deployment Date : TBD
**Impact area :** Get Mandate Status (GMS) API, Mandate Registration Callback


This is to inform you that we are enhancing our eNACH Mandate Registration APIs to provide clearer and more actionable failure reasons. This update introduces new response fields and renames some existing ones to improve consistency across our callback and status check flows.


**What's Changing?**


**Key Renames :**


The following existing keys are being renamed for consistency across Callbacks:


- ` status` is getting renamed to` mandate_status` . This reflects the mandate registration status.
- ` message` is getting renamed to` mandate_status_description` . It provides the descriptive status message.
- ` response_key` is getting renamed to` error_key` . This has been renamed for consistency with Get Mandate Status API.


⚠️ Important: Please ensure your callback parsing logic is updated to read the new field names.


1. **Introducing new fields :**


The following new fields will be added to both the Get Mandate Status API and Callback responses:


- ` error_key` - A failure category bucket (e.g.,` error_provider_error` ,` error_at_remitter_bank` ). Helps in programmatic grouping of failures. Appears only for failed registrations.
- ` error_key_description` - Detailed NPCI error code and message in the format: \[` provider_status_code` \] - \[` provider_status_description` \] (e.g., AP05 - No such account in CBS / invalid account number). Appears only for failed transactions.
- ` mandate_status_description` - A human-readable description of the mandate's current status, applicable for all states (success, pending, and failure).


**What You Need To Do?**


- Update callback parsers - Adjust your callback handling logic to consume the renamed fields (` mandate_status` ,` mandate_status_description` ,` error_key` ) instead of the deprecated ones (` status` ,` message` ,` response_key` ).
- Handle new fields - Start consuming` error_key` ,` error_key_description` , and` mandate_status_description` for improved failure diagnostics, dashboard displays, and customer-facing messages.
- Update checksum logic (if applicable) - If your integration validates callback payloads using a checksum that includes the` status` field, update it to use` mandate_status` instead.


**Changes in the Get Mandate Status API :**


Mandate Status - Active API Response -


Mandate Status - Pending API Response -


Mandate Status - Failed API Response -


**Changes in the Mandate Registration Callback :**


Mandate registration Success callback -


Mandate registration Failure callback -


**Changes in the Get Presentation Status API :**


Mandate Presentation Status - Success API Response


Mandate Presentation Status - Failure API Response


**Changes in the Mandate Presentation Callback :**


Mandate Presentation Callback - Success


Mandate Presentation Callback - Failure


Mandate Presentation Callback - Pending
