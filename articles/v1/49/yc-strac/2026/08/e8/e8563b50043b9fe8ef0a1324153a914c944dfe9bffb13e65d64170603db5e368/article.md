---
schema_version: "1.0.0"
document_id: "e8563b50043b9fe8ef0a1324153a914c944dfe9bffb13e65d64170603db5e368"
company_key: "yc-strac"
company: "Strac"
source_id: "yc-strac-news-import-28a26672fe0a"
canonical_url: "https://www.strac.io/blog/referential-integrity"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-04T04:47:28.222446+00:00"
fetched_at: "2026-08-04T05:15:07.724576+00:00"
content_hash: "sha256:712cbb683e9844af7c9fff264bded226dfb2af92ff4f055bda6771808a88a598"
---

# Referential Integrity in Data Masking: Keep Joins Valid (2026)

Last updated: August 2026


**Referential integrity** is the guarantee that relationships between records stay valid — every foreign key still points to a real parent row. In data masking and pseudonymization, it means the *same real value always becomes the same fake value* , everywhere it appears, so joins never break.


- **Why it matters:** mask a customer ID inconsistently and every JOIN, report, and test against that data silently breaks.
- **The rule:** a value like` customer_id 123` must map to the same fake (e.g.` 8842` ) in all five tables it appears in.
- **How to keep it:** deterministic tokenization or a stored, versioned mapping — never random-per-occurrence replacement.


## What Referential Integrity Means (and Why Masking Breaks It)


In a relational database, referential integrity means a foreign key in one table always matches a primary key in another — orders point to real customers, invoices point to real orders. Naive data masking destroys this: if you replace` customer_id 123` with a random value in the` orders` table but a different random value in` invoices` , the two no longer join. The masked data is now worthless for testing or analytics because the relationships are gone.


## ✨ The Fix: Consistent, Deterministic Replacement


The only way to preserve referential integrity while masking is to make replacement **deterministic** : the same input always yields the same output. Strac does this with deterministic tokenization —` token = HMAC(customer_secret, data_type + normalized_value)` — or a stored mapping table, so a value maps to the same pseudonym in every table, file, and format, and across every snapshot over time.


The same value maps to the same fake everywhere — foreign keys and joins remain valid.


## Beyond the Database: Cross-Format Consistency


Real referential integrity is not just table-to-table. A customer named in a Postgres row, a support PDF, a Slack export, and a spreadsheet must become the *same* fake in all of them, or cross-source analysis breaks. Strac keeps the mapping consistent across[every format it pseudonymizes](https://www.strac.io/blog/data-pseudonymization) , which is what makes the safe copy genuinely usable.


## 🌶️ Spicy FAQs for Referential Integrity


### Can I get referential integrity with random fake data?


Only if the randomness is *seeded deterministically* from the original value. Pure per-occurrence randomness breaks joins. Deterministic tokenization gives you realistic-looking data that is still perfectly consistent.


### Does this work across 24 snapshots?


Yes — the same customer secret and mapping version are reused, so pseudonyms stay stable over time. See[SAP data masking](https://www.strac.io/blog/sap-data-masking) .
