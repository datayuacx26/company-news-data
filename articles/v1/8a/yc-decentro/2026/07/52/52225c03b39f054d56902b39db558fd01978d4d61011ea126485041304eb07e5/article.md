---
schema_version: "1.0.0"
document_id: "52225c03b39f054d56902b39db558fd01978d4d61011ea126485041304eb07e5"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-news-import-cdeb59691175"
canonical_url: "https://docs.decentro.tech/changelog/flow-release-notes-389"
published_at: null
first_seen_at: "2026-07-25T01:33:54.013008+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:3373fcb2a272ebd6e74990bdb9be56b4b76dd517428ada95a31eb7bf35331de9"
---

# Flow Release Notes | Version 3.8.9

[Back to All](https://docs.decentro.tech/changelog)


added


This release notes contain changes related to **UPI Autopay** & **UPI Collections V3** .


###


UPI Collections V3


**Features Name :** Transaction Level Split Settlements, Purpose Message Propagation, Enhanced Failure Visibility
**Deployment Date:** April 27th, 2026


###


Transaction Level Split Settlements


- **New Parameter:** Introduced an optional` split_settlement_rule_urn` in the[Generate Payment Link](https://docs.decentro.tech/reference/payments_api-collectionsv3-paymentlink) ,[Generate Dynamic QR](https://docs.decentro.tech/reference/payments_api-collectionsv3-dynamicqr) , and[Issue Collect Request](https://docs.decentro.tech/reference/payments_v3-collectrequest) APIs.
- **Flexible Distribution:** Enables automated fund splitting at the individual transaction level (via percentage or fixed amount) instead of relying solely on daily aggregate settlements.


###


Purpose Message Propagation


- **Enhanced GTS Responses:** The` purpose_message` is now propagated across all status values in the[GTS Basic](https://docs.decentro.tech/reference/get-transaction-status-basic) (within the data block) and[GTS Advance](https://docs.decentro.tech/reference/get-transaction-status-advance) (within` transaction_description` ) APIs.
- **Channel Coverage:** Supported for[Generate Payment Link](https://docs.decentro.tech/reference/payments_api-collectionsv3-paymentlink) ,[Generate Dynamic QR](https://docs.decentro.tech/reference/payments_api-collectionsv3-dynamicqr) ,[Issue Collect Request](https://docs.decentro.tech/reference/payments_v3-collectrequest) and[Refunds APIs](https://docs.decentro.tech/reference/payments_api-collectionsv3-refund) and .


###


Enhanced UPI Failure Descriptions


- **Dashboard & MIS Integration:** Added a new` error_key_description` field to the Dashboard View Transactions table and UPI MIS downloadable reports.
- **Human-Readable Errors:** For failed transactions, the field displays a formatted string combining the error code and reason (e.g., Z9 - INSUFFICIENT FUNDS IN CUSTOMER ACCOUNT).


---


###


UPI Autopay


**Feature:** PDN API – Migration to Asynchronous Flow


**Deployment Date** - 24/04/2026


- All PDN requests are now processed asynchronously by default.
- Synchronous execution is completely removed.
- ` is_async` flag is now ignored in PDN API.
- API will always return` api_status` =` SUCCESS` when the PDN request is successfully accepted &` notification_status` =` PENDING` .
- Final PDN status will be available only via:


- Callback
- Status Check API


- ` presentation_sequence_id` which is used for presentations, is shared via callbacks.


PDN Doc -[here](https://docs.decentro.tech/reference/payments_api-upi-autopay-pre-debit-notification)
PDN Callback Doc-[here](https://docs.decentro.tech/reference/upi-autopay-mandate-pre-debit-notification-callback)
PDN Status Check Doc -[here](https://docs.decentro.tech/reference/upi-autopay-get-mandate-pre-debit-notification-status)
