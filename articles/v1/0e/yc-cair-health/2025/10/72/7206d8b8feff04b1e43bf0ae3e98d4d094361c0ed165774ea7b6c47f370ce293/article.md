---
schema_version: "1.0.0"
document_id: "7206d8b8feff04b1e43bf0ae3e98d4d094361c0ed165774ea7b6c47f370ce293"
company_key: "yc-cair-health"
company: "Cair Health"
source_id: "yc-cair-health-news-import-6be5ee110f3f"
canonical_url: "https://www.cairhealth.com/blog/introducing-automated-chart-review-in-cair-health"
published_at: "2025-10-08T00:00:00+00:00"
first_seen_at: "2026-07-23T04:26:54.016115+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:4f52118a37665dc58603c1d96e230fd3f3e0fa1a31c12cb69626c5c07f319831"
---

# 📋 Introducing Automated Chart Review in Cair Health

# **Introducing Chart Review on Cair Health**


Chart review (sometimes called “chart auditing”) is one of the most overlooked yet important steps in the revenue cycle. It’s the process of checking documentation and coding before a claim is submitted, to make sure everything lines up—procedures, diagnoses, modifiers, payer rules, and even clinical requirements.


Done well, chart review helps prevent denials, reduces rework, and ensures providers are compliant with payer and regulatory policies. Done poorly—or skipped entirely—it often leads to underpayment, overpayment risk, or a backlog of denied claims.


‍


## **Why chart review matters**


- **In orthopedics:** A physician may document both a fracture treatment and a joint injection in the same encounter. Without a modifier, payers may bundle the services and deny payment for one. A pre-bill review would flag this before submission.


- **In mental health:** A psychiatrist may bill for psychotherapy with evaluation and management. Payers require specific documentation (time-based requirements, clear separation of services). If this isn’t documented properly, claims may be denied or flagged for audit.


## **Payers are auditing your notes**


Chart review isn’t just about getting paid quickly—it’s about protecting your organization from compliance risk. Payers (especially Medicare and Medicaid) regularly audit provider documentation to ensure services are billed correctly. If notes don’t support the level of service billed, organizations may be asked to refund payments or face penalties.


This risk is particularly acute if a large portion of your patient population is covered by government programs. Medicare and Medicaid carriers often apply stricter rules and documentation requirements than commercial payers. Pre-bill review gives you an opportunity to catch discrepancies early—before an auditor does.


‍


## **Clark, our coding agent, now does pre-bill review**


We’ve expanded **Clark** , our AI coding agent, to handle automated chart review. Customers can now run pre-bill checks either directly in the platform or programmatically through our new endpoint (documentation linked in our docs).


The initial version of Clark’s chart review includes the following reports — each of which are essentially 'checks' that our system will validate for each note that is sent through Cair:


- **Relevant CPT–ICD codes**
- **CPT/ICD Alignment Report**
- **Modifier Bundling Report**
- **Telehealth Documentation Report**
- **Time-Based Requirements Report**
- **Documentation Discrepancy Report**
- **Authentication Report**
- **NCD/LCD Analysis Report ‍**


Cair Health's advanced chart review tooling powered by AI


You can track and upload charts directly in our platform to review them, or call the pre-bill review endpoint for automation.


‍


## **Rules and customization**


Every organization’s workflow looks different. Our **custom rules engine** lets you add your own requirements into Clark’s pre-bill review. For example, you can enforce checks around certain payer-specific telehealth policies, or flag notes that are missing required elements.


‍


## **Getting started**


- **Existing customers:** Reach out to us and we’ll enable the feature in your account.


- **New customers:** If you’d like to see chart review in action, you can find time for a[demo here](https://calendly.com/ishan-pi4/cair-health-intro)
