---
schema_version: "1.0.0"
document_id: "d34e04410dc4558c9a2e11c904c23f08777ba490610e4599ec5ceb337a29ffa5"
company_key: "yc-strac"
company: "Strac"
source_id: "yc-strac-news-import-28a26672fe0a"
canonical_url: "https://www.strac.io/blog/k-anonymity"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T06:41:40.731982+00:00"
fetched_at: "2026-08-05T06:41:42.783161+00:00"
content_hash: "sha256:b1a9c803fe9e9ef2ce3392c5d87d67335356b33aa0835052c8ad0daf6901517c"
---

# K-Anonymity and Quasi-Identifiers Explained (2026)

Last updated: August 2026


**K-anonymity** is a privacy model where every record is indistinguishable from at least *k−1* others based on its quasi-identifiers — so no individual can be singled out. It is a foundation of statistical de-identification.


- **Quasi-identifiers:** fields like ZIP, birthdate, and gender that identify in combination.
- **The mechanism:** generalization and suppression until each combination appears at least k times.
- **Not enough alone:** l-diversity and t-closeness address its blind spots.


## What Is K-Anonymity?


K-anonymity is a way to measure and enforce how hard it is to single someone out in a dataset. A dataset is *k-anonymous* if, for every record, there are at least *k−1* other records that share the same values across the quasi-identifiers. With k=3, any combination of quasi-identifiers points to at least three people — so an attacker can’t tell which one is their target.


## Quasi-Identifiers — The Hidden Risk


Removing names and SSNs is not enough. **Quasi-identifiers** are fields that aren’t unique on their own but become identifying in combination. The classic example: roughly 87% of Americans can be uniquely identified by just **ZIP code + date of birth + gender** . Direct identifiers (name, SSN) are obvious; quasi-identifiers are the ones that quietly break de-identification.


## ✨ How K-Anonymity Works


You achieve k-anonymity through two operations on the quasi-identifiers:


- **Generalization** — make values less specific (age 34 → 30–39; ZIP 98104 → 981**).
- **Suppression** — remove outlier values that can’t be grouped.


You repeat until every quasi-identifier combination appears at least k times. The table below shows a dataset generalized to k=3.


Age ZIP Gender Condition


30–39 981** F Asthma


30–39 981** F Diabetes


30–39 981** F Asthma


40–49 980** M Hypertension


40–49 980** M Asthma


40–49 980** M Diabetes


Each quasi-identifier group (age range + ZIP prefix + gender) now contains three records, so no individual stands out.


## Beyond K-Anonymity: L-Diversity and T-Closeness


K-anonymity has blind spots. If all three people in a group share the same sensitive value (say, all have the same diagnosis), an attacker still learns it — the group is k-anonymous but not diverse. **L-diversity** requires each group to contain at least *l* distinct sensitive values; **t-closeness** further requires the distribution of sensitive values in each group to resemble the overall distribution.


## K-Anonymity vs Pseudonymization


K-anonymity is an *anonymization* technique — it generalizes data so individuals can’t be singled out, at the cost of precision.[Pseudonymization](https://www.strac.io/blog/data-pseudonymization) takes a different path: it keeps data realistic and record-level by swapping identifiers for consistent fakes, which is what testing and AI need. See[pseudonymization vs anonymization](https://www.strac.io/blog/pseudonymization-vs-anonymization) for the full comparison.


## ✨ How Strac Helps


Any k-anonymity or[de-identification](https://www.strac.io/blog/hipaa-de-identification) effort starts by finding the quasi-identifiers — and that detection is the hardest part. Strac’s engine identifies direct and quasi-identifiers across records, documents, and images, so you know exactly what has to be generalized, suppressed, or pseudonymized.


Strac identifies direct and quasi-identifiers across every source before de-identification.


## 🌶️ Spicy FAQs for K-Anonymity


### What is a good value of k?


Higher k means stronger privacy but more generalization (less useful data). k=5 is a common baseline; sensitive releases use higher. The right value depends on the data and the risk tolerance.


### Does k-anonymity make data safe to release?


It helps, but not on its own — you also need l-diversity/t-closeness to prevent attribute disclosure, and you must correctly identify every quasi-identifier first.
