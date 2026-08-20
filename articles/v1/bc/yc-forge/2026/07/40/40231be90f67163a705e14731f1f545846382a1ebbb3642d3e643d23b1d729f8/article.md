---
schema_version: "1.0.0"
document_id: "40231be90f67163a705e14731f1f545846382a1ebbb3642d3e643d23b1d729f8"
company_key: "yc-forge"
company: "Forge"
source_id: "yc-forge-news-import-941ab9fc4cad"
canonical_url: "https://forgehq.com/resources/how-to-calculate-duty-drawback"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-27T02:33:23.114785+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:93b049cbef412147459a715629f670a81c7b88b1cc77106006600eebaed679a6"
---

# How to Calculate Duty Drawback (With Examples)

Duty drawback can return **up to 99% of the duties you paid** at the border — but “up to 99%” is a ceiling, not a promise. What you actually recover depends on how much duty you paid, how much of the goods leave the U.S., and which drawback rules apply. This guide shows exactly how the number is built, with worked examples.


## Key takeaways


- **The formula** — Recoverable drawback = duties paid on qualifying goods × up to 99%.
- **The base** — Duty paid = entered (customs) value × duty rate (plus Section 301/232 tariffs).
- **The 1%** — CBP keeps ~1% to cover administrative costs.
- **Partial exports** — You only recover duty on the portion actually exported or destroyed.
- **Substitution** — Comparative-value rules can slightly lower the refund versus direct identification.


## The basic formula


Every drawback calculation starts from the duty that was actually paid on the import:


> **Duty paid = Entered value × Duty rate** (+ any Section 301 / 232 tariffs)
>
>
> **Recoverable drawback = Duty paid on exported/destroyed goods × up to 99%**


The “entered value” is the customs value declared on your entry summary (CBP Form 7501) — usually the transaction price of the goods. The duty rate comes from the item’s HTS classification, plus any additional tariffs stacked on top.


## Worked example 1: unused merchandise drawback


You import 10,000 units of a product:


- **Entered value:** $500,000
- **Duty rate:** 25% (base duty + Section 301)
- **Duties paid:** $500,000 × 25% = **$125,000**


Later, the goods don’t sell domestically and you export **all 10,000 units** in the same condition. Because the full quantity is exported:


- **Recoverable drawback:** $125,000 × 99% = **$123,750**


That’s the clean, best-case scenario — full export, direct identification, no excluded duties.


## Worked example 2: partial export


Same import as above ($125,000 duties paid on 10,000 units). This time you export only **6,000 units** and sell the rest in the U.S.


- **Duty attributable to exported units:** $125,000 × (6,000 / 10,000) = $75,000
- **Recoverable drawback:** $75,000 × 99% = **$74,250**


You recover duty only on what actually left the country. The 4,000 units that stayed in U.S. commerce keep their duties with CBP.


## Worked example 3: manufacturing drawback


You import components, use them to build a finished product, and export the product:


- **Imported components entered value:** $800,000
- **Duty rate:** 15%
- **Duties paid:** **$120,000**


If 80% of the finished goods made from those components are exported:


- **Recoverable drawback:** $120,000 × 80% × 99% ≈ **$95,040**


Manufacturing claims tie the recovery to how much of the imported input ends up in exported product, documented through a bill of materials.


## What counts in the base amount


Drawback isn’t limited to the base customs duty. The recoverable base can include:


- **Base customs duties** — the standard HTS duty rate.
- **Section 301 and Section 232 tariffs** — recoverable when export/destruction conditions are met.
- **Merchandise Processing Fee (MPF)** — recoverable on many claims, with limits on some substitution claims.
- **Certain internal revenue taxes** — for specific goods like distilled spirits.


**Not recoverable:** antidumping/countervailing (AD/CVD) duties and statutorily excluded merchandise. Excluding these is often the biggest reason a real refund lands below the theoretical 99%.


## How substitution changes the math


With[substitution](https://forgehq.com/resources/duty-drawback-eligibility) , you match commercially interchangeable goods at the 8-digit HTS level instead of tracing exact units. The tradeoff: CBP applies **comparative-value rules** , so the recovery is generally based on the *lesser* of the duty on the imported goods or the duty that would apply to the substituted goods. For most importers the difference is small, but it means a substitution claim can recover slightly less than a direct-identification claim on the same goods.


## Why the real number needs your entry data


The formulas above are the mechanics — but an accurate figure requires your actual import history: entry summaries, HTS codes, duty rates, and matched export or destruction records across the 5-year window. That reconciliation is where most of the work (and most of the missed money) lives.


[Forge](https://forgehq.com/) , an AI-native customs broker, ingests your import and export data, structures it automatically, and calculates your recoverable amount entry by entry — reviewed by a licensed broker before filing. It’s free to find out; success-based pricing means you only pay when the refund lands.


**Related guides:**[What is duty drawback?](https://forgehq.com/resources/what-is-duty-drawback) ·[Do you qualify?](https://forgehq.com/resources/duty-drawback-eligibility) ·[How to file a claim](https://forgehq.com/resources/how-to-file-duty-drawback-claim)


> This guide is general information, not legal or customs advice. Actual drawback amounts depend on your specific entries, classifications, and records. Talk to a licensed broker before filing.
