---
schema_version: "1.0.0"
document_id: "6bc922d5e4a4116171ce12f9a3a9d8a65294cb0bb1bad77a53b466b8fb4517b7"
company_key: "forian-inc-common-stock"
company: "Forian Inc."
source_id: "forian-inc-common-stock-rss-1c01cf5ce14b"
canonical_url: "https://blog.forian.com/beyond-enrollment-what-closed-claims-miss-and-hybrid-reveals"
published_at: "2026-05-07T17:34:01+00:00"
first_seen_at: "2026-07-20T23:19:46.527638+00:00"
fetched_at: "2026-07-28T21:14:13.187831+00:00"
content_hash: "sha256:9d8569695b3bc1d52070d31f904860316d8672b875e5e32fc1c780e684b08c5c"
---

# Beyond Enrollment: What Closed Claims Miss and Hybrid Reveals

Closed claims are the workhorse for serious HEOR work, and for good reason. Enrollment continuity is the precondition for longitudinal analysis. If you can't see when a patient enters and leaves a payer, you can't follow them. Closed-only is the defensible default.


It is also incomplete. The gaps don't show up in your dashboards, because closed-only data, by definition, doesn't tell you what it isn't capturing.


To quantify those losses, we took 9.3 million patients with full continuous closed enrollment in 2024 and pulled their open-claims records over the same period. Same patients, same window, two views. The procedures, diagnoses, and drug-level signals visible in open but absent in closed are exactly what closed-only misses, even for fully enrolled patients. They are also what hybrid claims data is designed to recover by layering open data onto closed data for the same patients over the same time period.


*This blog is part of a series on hybrid claims data and real-world evidence. Read the first post*[here](https://blog.forian.com/hybrid-claims-data-essential-strategy-rwe-heor-studies) *.*


### **What Closed Claims Misses, Hybrid Recovers**


We compared procedures present in the open data but absent from the closed data, patient by patient, in the overlap cohort. Four categories accounted for the majority of the gap. Each category corresponds to a clinical signal HEOR teams routinely build cohorts around.


Hybrid Recovery From a Closed-Only Cohort


Top procedures present in open claims but absent from closed for the same patients, by category


Top procedures recovered by adding open claims to a closed-enrolled cohort, by category


- **Routine primary care visits**


.


The four most-recovered codes were 99213, 99214, 99203, and 99204, which represent standard established and new-patient office visits. Together, more than 2.6 million patient-visits in the open data had no matching closed-claim record for the same patients. These are the most common billed encounters in the U.S. A closed-only cohort that misses them is systematically undercounting primary care contacts for patients who clearly had them.


- **Lab panels and blood draws**


.


Routine venipuncture (36415), CBC (85025), comprehensive metabolic panel (80053), lipid panel (80061), A1C (83036). Standard diagnostic workups happen and never reach the payer record. Plausible mechanisms are cash-pay reference labs, out-of-network lab work, and diagnostic activity tied to conditions patients prefer to keep off their insurance record. For screening, early-disease detection, and treatment monitoring, the open layer is often the only place this work appears at all.


- **Performance-tracking F-codes**


.


3078F (diastolic BP <80), 3074F (systolic BP <130), 1159F (med list documented). These are clinical quality indicators, not reimbursable services. Providers record them; payers have no economic reason to capture them, and frequently don't. Any analysis that depends on vitals, medication reconciliation, or HEDIS-style indicators in claims data needs the open layer to see them.


- **High-acuity ED visits**


.


99284 and 99285, moderate and high-complexity emergency visits, show hundreds of thousands of patient-visits absent on the closed side. Some are high-deductible out-of-pocket care. Some are rebundling inside facility charges. The closed-only cohort is missing a meaningful volume of the most expensive care these patients receive.


The pattern is consistent:


***closed claims record what got paid and adjudicated. Open claims record what happened*** **.** For closed-enrolled patients, hybrid data shows both, and the gap between them is doing more work than most HEOR analyses acknowledge.


## **Unspecified J-codes and the Limit of Closed Claims Drug Analytics**


Drug-level analysis on a J-code line requires the NDC, the eleven-digit identifier that distinguishes one drug from another. Closed claims do not carry the NDC on J-code lines. Drug-level identification is therefore an open-claims exercise, full stop. For closed-enrolled patients, the only way to get there is to add the open layer.


This matters most on the unspecified J-codes, where the procedure code itself doesn't identify the drug:


- J3490 — Drugs, unclassified injection


- J3590 — Unclassified biologicals


- J7999 — Compounded drug, not otherwise classified


- J8499 — Oral prescription drug, non-chemotherapeutic, NOS


- J8999 — Oral prescription drug, chemotherapeutic


- J9999 — Chemotherapy drug, not otherwise classified


These codes are used for new drug launches before a permanent code is assigned, for compounded products, and when the biller cannot map a drug to a specific code. The procedure code conveys almost no clinical information on its own, which is precisely why the NDC is essential.


In our open-claims data, NDCs are present on these codes at fill rates of 80% or higher, with one exception (J7999, compounded drugs, at 33%). In closed claims, the fill rate is zero across all six.


**Drug-Level Identification on Unspecified J-codes**


NDC fill rate, open vs. closed. Closed claims carry no NDCs on these lines, so any drug-level analysis on a closed-only cohort cannot proceed.


##


NDC fill rate by code, open vs closed (left). Per-line charge range, median to maximum, log scale (right). The same procedure code spans four to seven orders of magnitude in charge, leaving any aggregate analysis without an NDC vulnerable to a single mis-mapped outlier.


The right panel of the chart shows why the missing NDC matters for cost work, not only for drug attribution. On J3490, per-line charges range from $0.01 to $3.83 million, with a median of $32.40 and a coefficient of variation of 56. J3590 reaches $824,500. J7999 reaches $161,856. J9999 reaches $130,575. The same code is being used for trivial generic injections, compounded specialty preparations, and high-cost biologics or gene therapies, all of which collapse together in any cut that aggregates by procedure code. Without an NDC, a closed-only analysis cannot separate the $32 generic from the $3.8M outlier. For the same patients in a hybrid cohort, the open layer makes that separation possible.


For comparative effectiveness, biosimilar uptake, real-world regimen mapping, and market-share work on specialty drugs, an unspecified J-code is not a usable proxy for a drug. Without the open layer, the analysis cannot proceed. With it, the same closed-enrolled patients become available for drug-level inference.


## **Same Patients, More Signal**


Closed-only analysis is defensible, and the enrollment continuity it provides is not negotiable for longitudinal work. The argument here is not to give that up. It is to keep the closed-enrolled cohort and add the open-claims layer for the same patients over the same time window. Same denominator, more signal. That is what hybrid claims data is: a more complete view of the same patients.


Every HEOR team that takes claims data seriously should run this comparison on the codes their own work depends on, and ask what closed-only is missing. The answer will likely change how the question is scoped.


##
