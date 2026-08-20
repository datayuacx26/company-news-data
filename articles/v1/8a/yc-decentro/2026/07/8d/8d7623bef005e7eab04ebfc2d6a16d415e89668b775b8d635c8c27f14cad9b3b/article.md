---
schema_version: "1.0.0"
document_id: "8d7623bef005e7eab04ebfc2d6a16d415e89668b775b8d635c8c27f14cad9b3b"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-news-import-cdeb59691175"
canonical_url: "https://docs.decentro.tech/changelog/flow-release-notes-391"
published_at: null
first_seen_at: "2026-07-25T01:33:54.013008+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:81a30cf2d8cde56d48937e3f9d76ff19172b4e144b60221185cf080046b0569b"
---

# Flow Release Notes | Version 3.9.1

[Back to All](https://docs.decentro.tech/changelog)


improved


This release notes contains updates related to **UPI Collections v3** & **Autopay** products.


##


UPI Collections v3


**Feature:** Enhanced Failure Reason Visibility for UPI Transactions


**Deployment date:** 28-05-2026


We've enhanced transaction failure visibility for UPI transactions by extending support for the` error_key_description` field across APIs and callbacks.


**What's changing?**


1. Failed upi transactions will now include a new field:` error_key_description` .
2. The value will contain the provider error code along with its mapped NPCI/PSP description (for example: U30-Beneficiary bank failed the transaction).
3. This enhancement is available across:


1. GTS Basic API
2. GTS Advance API
3. Terminal Status Callback


**Behaviour**


1. ` error_key_description` is populated only for FAILED transactions.
2. For SUCCESS and PENDING transactions, the field will remain null or be omitted.


**Benefits**


1. Improved visibility into payment failures.
2. Faster troubleshooting and support resolution.
3. Better customer communication through more descriptive failure reasons.


**Sample Response**


```text
{
"error_key_description": "U30-Beneficiary bank failed the transaction"
}


```


---


##


UPI Autopay


**Feature-1 :** PDN Request Cutoff Time
**Deployment Date:** 28-05-2026


For the Pre-debit Notification requests where the debit_date is T+1 :


- From 28 May 2026, Pre-debit Notification requests submitted via the Pre-debit Notification API will be blocked during a daily cutoff window. Requests submitted during this window will be rejected and with a proper response body will be returned.


**What Changed?**


- For the debit_date = T+1 (i.e., debit scheduled for the next calendar day), Any Pre-debit Notification request received at or after 23:50:00 and before 00:00:00 will be rejected.
- The cutoff does NOT apply to Pre-debit Notification requests where the debit_date= T+2 or later.


**Why are we doing this?**


- We are introducing the cutoff to ensure timely processing of already‑queued Pre-debit Notification requests for asynchronous APIs.
- A small number of requests that spill over into the following day were being rejected by NPCI, so the cutoff prevents such spill‑over and reduces rejections.
- This change will improve overall success rates for Pre-debit Notification submissions and reduce failed callbacks due to NPCI rejections.


Refer to the Docs -[Here](https://docs.decentro.tech/reference/payments_api-upi-autopay-pre-debit-notification)


**Feature-2 :** New optional parameter "` first_txn_amount_reference_id` " added to UPI Autopay SDK
**Deployment Date:** 01 June 2026


We added an optional parameter named` first_txn_amount_reference_id` to the UPI Autopay SDK.


- Previously, the SDK automatically generated the` reference_id` for first-time (instant presentation) debit flows and did not allow merchants to provide their own value.


**What Changed?**


- Introduced a new optional param` first_txn_amount_reference_id` in the autopay SDK.
- If the merchant passes` first_txn_amount_reference_id` , the SDK will use their value for the first debit’s` reference_id` .
- The parameter is optional. If you do not provide` first_txn_amount_reference_id` , the SDK behavior remains the same (SDK will auto-generate a` reference_id` ).


**How is it Useful for you ?**


Merchants can now pass their own reference_id for the first debit transaction. This gives you direct control over the identifier used for instant presentation (first-time) payments, enabling:


- Improved reconciliation and traceability
- Simplified settlement and bookkeeping
- Faster dispute resolution
- Clearer audit trails for first-time debit flows.


Refer to the docs -[here](https://docs.decentro.tech/reference/upi-autopay-sdk-android)
