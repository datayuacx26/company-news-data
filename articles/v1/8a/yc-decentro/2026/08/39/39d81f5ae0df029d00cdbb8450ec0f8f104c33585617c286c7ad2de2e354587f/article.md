---
schema_version: "1.0.0"
document_id: "39d81f5ae0df029d00cdbb8450ec0f8f104c33585617c286c7ad2de2e354587f"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-news-import-cdeb59691175"
canonical_url: "https://docs.decentro.tech/changelog/flow-advisory-autopay-mandates-version-113"
published_at: null
first_seen_at: "2026-08-10T12:35:49.286651+00:00"
fetched_at: "2026-08-10T12:35:49.786512+00:00"
content_hash: "sha256:d1c5362a3e62c0338790211919decc50ff8fc8dabb2e06313ce0bd6a6d2c52df"
---

# Flow Advisory | Autopay Mandates | Version 1.1.3

[Back to All](https://docs.decentro.tech/changelog)


##


Advisory: Enhanced Error Reporting for UPI AutoPay Mandate Registration


###


Deployment Date: 27th Aug 2026


###


Impact Area: Get Mandate Status (GMS) API, Mandate Registration Callback


This is to inform you that we are enhancing our UPI AutoPay Mandate Registration APIs to provide clearer and more actionable failure reasons. We are introducing two new fields` error_key` and` error_key_description` in the Get Mandate Status API and Mandate Status Callback for the failed transactions.


##


What is changing ?


###


Introducing new fields :


The following new fields will be added to both the Get Mandate Status API and Callback responses:


- ` error_key` - A high-level failure category (e.g.,` error_at_provider` ,` error_invalid_rule` ) that groups related failure codes into a single bucket. Appears only for failed registrations.
- ` error_key_description` - Detailed NPCI error code and message in the format:\[` provider_status_code` \] - \[` provider_status_description` \] (e.g., QM - PAYER VPA IS INCORRECT (PAYER)). Appears only for failed transactions.


###


Enhancing the already present field :


- ` mandate_status_description` - A human-readable description of the mandate's current status, applicable for all states (success, pending, and failure). Eg- "Payer VPA is incorrect (payer)".


###


Changes to the Get Mandate Status API :


**Mandate Status - Failed :-**


```text
{
"decentro_txn_id": "800BBD69B5E34CDB99DE780AEDB94EE3",
"api_status": "SUCCESS",
"message": "Mandate status retrieved successfully",
"data": {
"mandate_status": "FAILED",
"mandate_status_description": "PAYER VPA IS INCORRECT (PAYER)",
"decentro_mandate_id": "5D6AA45472534505B868A1909EACF985",
"error_key": "error_account_issue",
"error_key_description": "QM - PAYER VPA IS INCORRECT (PAYER)"
},
"response_key": "success_mandate_status_fetched"
}


```


###


Changes to the Mandate Registration Callback : -


**Failed Callback :-**


```text
{
"mandate_status": "FAILED",
"mandate_status_description": "PAYER VPA IS INCORRECT (PAYER)",
"error_key": "error_account_issue",
"error_key_description": "QM - PAYER VPA IS INCORRECT (PAYER)",
"mandate_attempt_status": "FAILED",
"mandate_attempt_status_description": "Mandate action update is failed",
"decentro_txn_id": "1EFA7FXXXXX9DF05035F67FF",
"callback_attempt": 2,
"callback_timestamp": "2025-01-15 15:30:39.902290",
"consumer_urn": "A05DEB1DXXXB94A8B98D86",
"reference_id": "a8dbe6cb-5840-4031-9234-170d2159de79",
"mandate_creation_timestamp": "2025-01-15 15:30:09.919890",
"bank_reference_number": "392207903002",
"payer_vpa": "nk@mybank",
"decentro_mandate_id": "BDF32B36XXXXB11DCF541E97D56B",
"amount": "100.0000",
"amount_rule": "MAX",
"payer_name": "XXXX",
"payee_name": "Merchant onboarding account",
"next_debit_date": "2020-08-24 00:00:00.000000",
"mandate_name": "DecfXXXting",
"frequency": "MONTHLY",
"start_date": "2025-01-15",
"end_date": "2025-07-05",
"presentation_summary": {
"total": 0,
"success": 0,
"failed": 0,
"skipped": 0,
"remaining": 0
},
"rule_type": "AFTER",
"block_fund": false,
"is_revocable": true,
"last_action": "CREATE",
"last_action_timestamp": "2025-01-15 15:30:10.171777",
"last_action_auth_timestamp": "2025-01-15 15:30:25.790501",
"npci_transaction_id": "MGS53EXXX3495343C31B9B9B",
"original_callback_txn_id": "AA2CXX1094E3CDFBFA537A94",
"callback_txn_id": "5AF4DB0XX08774488F66BCF"
}


```


###


What you need to do ?


- Handle the new fields - Start consuming` error_key` and` error_key_description` in your callback handlers and Get mandate status API integrations to improve failure diagnostics, dashboard displays, and customer-facing messages.
- Review` mandate_status_description` usage - If your integration surfaces` mandate_status_description` directly to end users or logs it like earlier, note that failure descriptions will now be more specific (e.g., "PAYER VPA IS INCORRECT (PAYER)" instead of "Mandate has failed"). Adjust any hardcoded string matching accordingly.
- No parser changes required for existing fields - All existing field names and their behaviour for SUCCESS and PENDING states remain unchanged.
- Since there are no changes in the field names, apart from the addition of the 2 new fields (` error_key` and` error_key_description` ) still check the checksum logic at your end used for validating the callback.
