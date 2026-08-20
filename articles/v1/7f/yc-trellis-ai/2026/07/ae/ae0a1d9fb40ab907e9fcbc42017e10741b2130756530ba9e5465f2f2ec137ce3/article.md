---
schema_version: "1.0.0"
document_id: "ae0a1d9fb40ab907e9fcbc42017e10741b2130756530ba9e5465f2f2ec137ce3"
company_key: "yc-trellis-ai"
company: "Trellis AI"
source_id: "yc-trellis-ai-news-import-ef87914578b7"
canonical_url: "https://runtrellis.com/blog/ndc-to-hcpcs-crosswalk-guide-avoid-j-code-errors-on-infusion-claims"
published_at: null
first_seen_at: "2026-07-22T17:10:20.137242+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:70a4e06bb1e3010296eaaf53eaec10455c84f20b3fd0d153f4a296bfd1f2d83e"
---

# NDC‑to‑HCPCS Crosswalk Guide: Avoid J‑Code Errors on Infusion Claims

Ensuring the correct mapping between a drug’s National Drug Code (NDC) and its HCPCS J-code is critical to clean infusion claims. **The NDC/HCPCS crosswalk** is an official resource listing each NDC that corresponds to a HCPCS billing code (J-code). This crosswalk is updated monthly by the Pricing Data Analysis and Coding (PDAC) contractor to reflect new drugs and changes. Using the latest crosswalk helps avoid **common J-code errors** such as using an NDC that doesn’t match the J-code’s drug form or strength. For example, UnitedHealthcare found instances where providers billed saline solution J7040 but submitted an NDC for lidocaine in dextrose, which wasn’t even listed for that code. Such **mismatches lead to denials** because the payer’s system flags the J-code/NDC combination as invalid.


### Best practices


Always verify that the NDC on the vial matches the HCPCS description (route, dosage) and use the correct billing units. Many J-codes represent a specific **drug amount per unit** (e.g., J-code per 10 mg), so convert the administered dose into the proper units and **round up if needed** (when rounding, do not separately bill that remainder as waste). Submit the 11-digit NDC (no dashes) with the correct unit of measure and quantity on the claim. If your billing system is integrated with the NDC-HCPCS crosswalk, it can alert staff to discrepancies before claims go out.


Finally, **keep documentation** of the drug administered and any wasted portion, including **lot numbers and waste volume** in the medical record. If an audit arises, having precise documentation will support the claim. Remember the maxim: *“If it was not documented, it was not done.”* Missing infusion records are a top cause of denials. By diligently using the crosswalk and documenting administration details, infusion centers can avoid J-code billing errors and get paid faster with fewer payer rejections.
