---
schema_version: "1.0.0"
document_id: "4242bc46cce8203d270050693acdd627d2f7daad3c6db70a43313a00916e6b25"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-news-import-cdeb59691175"
canonical_url: "https://docs.decentro.tech/changelog/flow-release-notes-387"
published_at: null
first_seen_at: "2026-07-25T01:33:54.013008+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:6f3990a6e12d71c944e68a406af3fd8f7fe853d865da864dfc1ece3c46773868"
---

# Flow Release Notes | Version 3.8.7

[Back to All](https://docs.decentro.tech/changelog)


added


This release note contains changes related to **UPI Collections V3** & **UPI Autopay**


**Product** - UPI Collections V3


**Release date** - 26th March'26


**New: Unified MIS Reports for Static QR (Push Transactions)**


We have extended our reporting capabilities to give you full visibility into your offline collections. You can now generate and download MIS reports specifically for transactions initiated via[Static QRs](https://docs.decentro.tech/docs/upi-collections-offline-qr) .


- **Granular Filtering:** A new Consumer URN filter has been added to the report generation screen, allowing you to pull data for specific consumers.
- **Simplified Reconciliation:** Under the UPI report type, you will now see Static QR as a collection type alongside Payment Links, Collect Requests and Dynamic QRs.
- **How to access:** Navigate to Reports > MIS Reports > Generate Report, select UPI as the report type, and choose Static QR from the collection types dropdown.


---


**Enhanced: Automated Settlement for Held Transactions (T+5)**


- **Added:** Automated T+5 Capture Logic for transactions stuck in HOLD status. The system now "sweeps" and captures these funds 5 days after the transaction date.
- **Improved:** Settlement efficiency - manual intervention is no longer required to release held funds from expired or duplicate payment scenarios.
- **Changed:** Transaction lifecycle updates - eligible HOLD transactions now automatically transition to SUCCESS with the action tag CAPTURED.


---


**Product** - UPI Autopay


**Release date** - 26th March'26


**Feature 1 - Introduction of Async Flow for Pre-Debit Notification (PDN) API**


**What is changing?**
Decentro have introduced an asynchronous execution model for the Pre-Debit Notification (PDN) API to improve reliability, scalability, and reduce dependency on provider response times.


A new optional parameter` is_async` is being introduced in the PDN request.


**What merchants need to do-**


1. Opt-in to async flow (recommended)


1. Pass` is_async` = true in PDN API request
2. If not passed, default behavior will remain synchronous (no change)
3. Support of synchronous flow for PDN api will be discontinued on April 23rd 2026.
4. After April 23rd, by default only Asynchronous will be made available.


2. Handle API response


1. For async requests:


1. API will return immediate response with status of Pending
2. This does NOT mean PDN is successful
3. It only means request is accepted and queued by Decentro.


3. Rely on callbacks for final status


1. Merchants must:


1. Consume PDN callbacks reliably
2. Treat callback as source of truth
3. Final status:` SUCCESS` /` FAILURE`


4. Use Status Check API (fallback)


1. Status API will return:` PENDING` (if still processing)
2. Final status once processed successfully will be either` SUCCESS` /` FAILURE`
3. Recommended as backup mechanism, not primary


**Pre-debit Notification API Docs** -[Here](https://docs.decentro.tech/reference/payments_api-upi-autopay-pre-debit-notification)


**Sample PDN API response-**


```text
{
"decentro_txn_id": "CCB8XXXXXX6FCD79AEE",
"api_status": "SUCCESS",
"message": "Successfully initiated the pre-debit notification.",
"data": {
"notification_status": "PENDING"
},
"response_key": "success_pre_debit_notification_initiated"
}


```


---


**Feature 2 - Introduction of Instant Presentation Reference ID**


**What’s New?**
Merchants can now pass their own` reference_id` for instant presentation (` is_first_txn_amount` /` is_downpayment` ) in Create Mandate API.


**New optional fields:**
` first_txn_amount_reference_id`
` downpayment_reference_id`


**Behavior-**


1. If provided, the respective` reference_id` will be used for -


1. Instant presentation (5mins presentations)
2. Merchant callbacks
3. Presentation status check


2. If not provided:


1. Existing Decentro-generated` reference_id` will be used for presentations.
2. Create mandate reference ID can be used fetch instant presentation status.


**Validations-**


1. Only one of the below can be passed:


1. ` first_txn_amount_reference_id`
2. ` downpayment_reference_id`


2. Allowed only when:` is_first_txn_amount` = true OR` is_downpayment` = true
3. If flags are false and` first_txn_amount_reference_id` or` downpayment_reference_id` is passed → request will be rejected
4. All reference IDs must be unique (same as existing rules)


**Create Mandate API Docs** -[Here](https://docs.decentro.tech/reference/payments_api-upi-autopay-create-mandate)
