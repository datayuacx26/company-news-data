---
schema_version: "1.0.0"
document_id: "cca253fbd2aeea01fa8657fc8778f9bde78c05b478fea1cc9e549c2473ad2d9c"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-news-import-cdeb59691175"
canonical_url: "https://docs.decentro.tech/changelog/fabric-release-notes-version-68"
published_at: null
first_seen_at: "2026-07-25T01:33:54.013008+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:fdf5572e837ccbcbf0718e3195be9084e8ad089a232dbd5730d06501b501c15b"
---

# Fabric Release Notes | Version 6.8

[Back to All](https://docs.decentro.tech/changelog)


added


**Summary**


Release introduces a unified CKYC Dashboard with an Overview page, enabling real-time KYC search (PAN, Aadhaar, Mobile, CKYC ID), activity tracking, and drop-off analysis. It improves onboarding efficiency, reduces manual effort, and enhances visibility while ensuring compliance through PII masking, audit logging, and data purging controls.


###


I. CKYC Dashboard – Overview, Search & Drop-off Analysis


**Description**
Enhanced CKYC Dashboard with a centralized Overview page and integrated Search capability to enable real-time KYC record retrieval and monitoring from a single interface.


**Key Highlights**


- New Overview page with recent Search and Download activity logs
- CKYC Search via Dashboard. Supported OVD IDs: PAN, Aadhaar, Mobile, CKYC Reference ID and CKYC Number
- Masked PII display (PAN, Aadhaar, Mobile) with strict data purging policies
- Drop-off Analysis with Issues dashboard (Overview, Search, Download, Upload)
- Day-wise error trends with drill-down to failed cases
- Standardized error handling and audit logging


**Benefit**
Reduces manual effort, improves onboarding TAT, enhances visibility into CKYC activity, and ensures compliance through secure, audit-ready operations.


**Action for Clients**
To pull and deploy the image` nor.26-03-26.a`
