---
schema_version: "1.0.0"
document_id: "d43a95500b87d4be748bcd7a554692376d45b6efdd8847643e060025e347bbeb"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-news-import-cdeb59691175"
canonical_url: "https://docs.decentro.tech/changelog/flow-release-notes-388"
published_at: null
first_seen_at: "2026-07-25T01:33:54.013008+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:417e9f0eaff4f53704c5dc0c9b16c61aa844f9567b69964bd1814623184745f6"
---

# Flow Release Notes | Version 3.8.8

[Back to All](https://docs.decentro.tech/changelog)


improved


**Feature Name :** Remaining Representation Attempts Count for Presentations
**Deployment Date:** April 13th, 2026


We have enhanced the` remaining_representation_attempts` logic in Presentation Status API and callbacks to make it more accurate and aligned with NPCI rules.


- Representation attempts are now dynamic and error-code driven
- Only eligible failures will allow retries
- Maximum allowed = **2 retries (1 initial + 2 attempts)**


**Key Changes:**


- For non-eligible error codes:` remaining_representation_attempts` = 0


- No further presentations allowed
- PDN marked as Completed


- For eligible error codes:` remaining_representation_attempts` decreases with each retry


- Up to 2 retries allowed
- For instant presentations:` remaining_representation_attempts` = 0
- No retries allowed when` remaining_representation_attempts` = 0


- Same logic is applied across:


- Presentation **Status API**
- Presentation **callback payloads**


**Impact:**


- Eliminates unnecessary re-presentation attempts.
- Improves accuracy of retry eligibility.
- Helps merchants take correct actions based on failure reason.
