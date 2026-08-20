---
schema_version: "1.0.0"
document_id: "d344afca4b5bb792e29737032cc9dcc2a7f008a45096b2506018456409a127ff"
company_key: "yc-strac"
company: "Strac"
source_id: "yc-strac-news-import-28a26672fe0a"
canonical_url: "https://www.strac.io/blog/hipaa-de-identification"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T06:41:40.731982+00:00"
fetched_at: "2026-08-05T06:41:42.783161+00:00"
content_hash: "sha256:1fb7edb06a86837963e4e5c1b8dd64b18d8d9fdad860b4c9fbce014886c3f793"
---

# HIPAA De-identification: Safe Harbor vs Expert Determination (2026)

Last updated: August 2026


**HIPAA de-identification** removes protected health information (PHI) so data is no longer subject to HIPAA. There are exactly two approved methods: **Safe Harbor** (remove 18 specific identifiers) and **Expert Determination** (a qualified expert certifies that re-identification risk is very small).


- **Safe Harbor:** a checklist — strip the 18 listed identifiers.
- **Expert Determination:** a statistician certifies low re-identification risk.
- **The hard part:** quasi-identifiers (ZIP + birthdate + gender) can re-identify people even without a name.


## What HIPAA De-identification Means


Under HIPAA, health data that has been properly de-identified is no longer PHI and falls outside the Privacy Rule — it can be used and shared much more freely. The catch is doing it correctly. The regulation defines two, and only two, valid methods.


## Method 1 — Safe Harbor


Safe Harbor is a checklist: remove **18 specific identifiers** and the data is considered de-identified. They include names, all geographic subdivisions smaller than a state, all dates (except year) tied to an individual, phone/fax numbers, email, SSN, medical record numbers, account numbers, biometric identifiers, full-face photos, and any other unique identifying number or code.


**Limitation:** Safe Harbor is blunt — it can over-remove useful data, and stripping the 18 identifiers does not always eliminate re-identification risk from combinations of remaining fields.


## Method 2 — Expert Determination


Expert Determination is statistical: a person with appropriate knowledge and experience applies accepted methods to certify that the risk of re-identification is "very small," and documents the analysis. This is where[k-anonymity and quasi-identifiers](https://www.strac.io/blog/k-anonymity) come in — the expert has to account for fields like ZIP, birthdate, and gender that can single someone out in combination.


## Safe Harbor vs Expert Determination


Safe Harbor Expert Determination


Method Remove 18 specified identifiers Statistical risk certification


Who does it Anyone following the checklist A qualified expert


Data utility Lower (blunt removal) Higher (keeps more, risk-controlled)


Handles quasi-identifiers Not explicitly Yes


Best for Simple, low-risk datasets Rich datasets where utility matters


## ✨ Pseudonymization and De-identification


De-identification and[pseudonymization](https://www.strac.io/blog/data-pseudonymization) are related but distinct: de-identification aims to make data non-PHI, while pseudonymization keeps data realistic and reversible for testing and AI. Both start with the same hard step — **finding every identifier and quasi-identifier** across records, documents, and images. That is exactly what Strac’s detection engine does, and it can then suppress, generalize, or[redact](https://www.strac.io/blog/deidentification-phi-tokenization-redaction-approach) PHI consistently across every format.


Strac finds every identifier and quasi-identifier across records, documents, and images.


## 🌶️ Spicy FAQs for HIPAA De-identification


### Is masking the same as HIPAA de-identification?


Not automatically. Masking a name helps, but Safe Harbor requires removing all 18 identifiers, and Expert Determination requires a certified risk analysis. Masking is a tool used within de-identification, not a substitute for the method.


### Can de-identified data ever be re-identified?


It can, if quasi-identifiers are ignored. That is why Expert Determination explicitly accounts for them — combinations like ZIP + birthdate + gender have famously re-identified individuals.
