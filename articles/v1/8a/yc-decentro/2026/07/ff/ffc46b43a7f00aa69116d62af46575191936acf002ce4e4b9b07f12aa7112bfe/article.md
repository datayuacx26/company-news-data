---
schema_version: "1.0.0"
document_id: "ffc46b43a7f00aa69116d62af46575191936acf002ce4e4b9b07f12aa7112bfe"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-news-import-cdeb59691175"
canonical_url: "https://docs.decentro.tech/changelog/flow-release-notes-version-386"
published_at: null
first_seen_at: "2026-07-25T01:33:54.013008+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:f2519e996df07f6947a1d429acae1179a3cff34245819985fe575eb199c2c5ad"
---

# Flow Release Notes | Version 3.8.6

[Back to All](https://docs.decentro.tech/changelog)


added


This release notes contains changes related to **UPI Autopay** product.


##


UPI Autopay


**Feature:** Enhanced PDN Failure Error Propagation
**Applicable API:** Pre-Debit Notification (PDN) API -[docs](https://docs.decentro.tech/reference/payments_api-upi-autopay-pre-debit-notification)
**Impact:** Failure responses only
**Deployment Date:** 26th February, 2026


**Summary**


PDN failure responses have been enhanced to surface provider-level error codes and descriptions directly to merchants.


**What Has Changed**


- A new field` error_key_description` has been introduced in PDN failure responses.
- ` error_key_description` will be populated in the following format:


- \[NPCI Error Code\] - \[NPCI Error Message\]


- This field will be returned only when notification_status = FAILED.


```text
{
"decentro_txn_id": "AEF14BC90XXXXXXX14",
"api_status": "FAILURE",
"message": "Unexpected error received from the underlying provider.",
"response_key": "error_provider_error",
"error_key": "error_mandate_already_completed",
"status_description": "Mandate already honoured.",
"error_key_description": "QB - API Request Failed"
}


```


##


UPI Collections


###


What’s New?


We have enhanced the[Get Transaction Status (Advance) API](https://docs.decentro.tech/reference/get-transaction-status-advance) to provide better visibility into payment sources. You can now retrieve specific payer identity information directly within the transaction attempt details.


###


Changes at a Glance


- New Response Fields: Added` payer_vpa` and` payer_name` to the` transaction_attempt_description` object.
- Enhanced Reconciliation: Seamlessly map incoming funds to specific customers without cross-referencing external reports.
- Example GTS Response Payload with newly added fields.


```text
{
"decentro_txn_id": "F9B3F89E05E44B4E8C8664A64D0C7B66",
"api_status": "SUCCESS",
"message": "Transaction status retrieved successfully",
"data": {
"transaction_description": {
"transaction_status": "PENDING",
"transaction_status_description": "Transaction status pending",
"request_decentro_txn_id": "0BB40475AFD94D9D94E7396569CBC2B9",
"reference_id": "bccc5f72-a237-4e07-b773-150a65de0441",
"transaction_amount": 1
},
"transaction_attempt_description": [
{
"transaction_attempt_number": 1,
"decentro_txn_id": "98FC009EA6A240669A2412FD2D59523D",
"transaction_attempt_status": "FAILED",
"transaction_attempt_status_description": "Failed Payment Attempt",
"bank_reference_number": "838172494261",
"transaction_authentication_timestamp": "2025-06-13 13:50:25",
"amount": 1,
"npci_txn_id": "YBL5b97e09e1d4b4c59815b6984be1899eb",
"payer_vpa": "9041216212@superyes",
"payer_name": "Joe Sebastian",
"error_key": "error_at_remitter_bank",
"error_key_description": "Z6 - NUMBER OF PIN TRIES EXCEEDED"
}
]
},
"response_key": "success_transaction_status_fetched"
}


```
