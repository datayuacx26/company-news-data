---
schema_version: "1.0.0"
document_id: "bddc5cdd905a8a74b6f68476b08d77a6fce52c50714f554d64c1f4670f862482"
company_key: "yc-skypher"
company: "Skypher"
source_id: "yc-skypher-news-import-856f7f683911"
canonical_url: "https://blog.skypher.co/blog/soc-2-ssae-18"
published_at: "2026-08-16T00:02:40.489+00:00"
first_seen_at: "2026-08-16T06:24:45.975996+00:00"
fetched_at: "2026-08-16T06:24:47.268049+00:00"
content_hash: "sha256:54bf1523682bac91468e1c71191f21f6ec576c97eef586bf2de828d08eb16545"
---

# SOC 2 and SSAE 18: What Every Compliance Team Must Know

SSAE No. 18 is the attestation standard; a SOC 2 report is the deliverable produced under it. That distinction matters because buyers who request "SSAE 18 certification" are asking for something that does not exist, while auditors who perform SOC 2 engagements follow SSAE No. 18 (AICPA) as their procedural framework. Getting this right protects you from misaligned vendor conversations and scope gaps that surface during fieldwork.


Four points that professionals routinely conflate:


- **SSAE No. 18 governs auditor procedures.** It defines how a CPA plans, executes, and reports on an attestation engagement. It is not a certification a vendor earns.
- **SOC 2 evaluates Trust Services Criteria (TSC).** The report covers security, availability, processing integrity, confidentiality, and privacy controls at a service organization.
- **Type 1 tests control design at a single point in time; Type 2 tests operating effectiveness over a defined period** , commonly 3–12 months.
- **Buyers should request a SOC 2 report issued under SSAE standards** , not claim their vendor is "SSAE 18 certified." The correct ask is a SOC 2 Type 1 or Type 2 report.


## Key Takeaways


SOC 2 is the report; SSAE No. 18 is the AICPA attestation standard that governs how auditors plan and perform the engagement, and confusing the two leads to misaligned vendor conversations and scope gaps.


Point Details


SSAE 18 is the standard, not the report Buyers should request a SOC 2 report issued under SSAE standards, not "SSAE 18 certification."


Security TSC is always required The other four TSCs (Availability, Processing Integrity, Confidentiality, Privacy) are selected based on service commitments.


Type 1 vs. Type 2 is a staging decision Type 1 tests design at a point in time; Type 2 tests operating effectiveness over a period, commonly 3–12 months.


Readiness is the dominant cost driver Scope definition, control documentation, and evidence collection consume more time than the audit itself.


Skypher accelerates evidence workflows Skypher's automation and Trust Center reduce manual evidence collection and questionnaire response time across audit cycles.


## Table of Contents


- What is SSAE No. 18 and why does it govern SOC 2 engagements?
- What is SOC 2 and who actually reads it?
- How do SOC 2 and SSAE No. 18 actually differ?
- What do the five Trust Services Criteria actually require?
- SOC 2 Type 1 vs. Type 2: which one should you pursue first?
- How does a SOC 2 audit actually work?
- How to read a SOC 2 report when a vendor sends you one
- Preparation checklist and the pitfalls that derail most programs
- Where SOC 2 sits in the broader compliance ecosystem
- A practitioner's perspective on what actually matters
- Automation makes SOC 2 readiness faster and less painful
- Sources


## What is SSAE No. 18 and why does it govern SOC 2 engagements?


SSAE No. 18 is the American Institute of Certified Public Accountants (AICPA) attestation standard that clarifies and recodifies prior SSAE guidance, replacing SSAE 16 and earlier standards. It establishes the professional requirements CPAs must follow when performing attestation engagements for service organizations. As[AuditBoard explains](https://www.auditboard.com/blog/ssae-18/) , SSAE 18 updated and consolidated the prior framework, positioning SOC 2 as the report focused on nonfinancial Trust Services Criteria.


What SSAE No. 18 actually controls:


- **Engagement planning and risk assessment.** The standard requires auditors to understand the service organization's system, identify relevant controls, and assess the risk of material misstatement.
- **Evidence evaluation.** Auditors must gather sufficient appropriate evidence to support their opinion, whether testing design only (Type 1) or operating effectiveness over time (Type 2).
- **Subservice organization treatment.** SSAE No. 18 specifies how auditors handle third-party subservice organizations, either carving them out of scope or applying an inclusive approach.
- **Reporting requirements.** The standard defines what must appear in the auditor's report, including the management assertion, scope boundaries, and opinion language.


When a buyer asks a vendor for "SSAE 18," the correct response is to provide a SOC 2 report. The standard itself is not a document vendors distribute; it is the professional framework the auditor follows. Pointing buyers to the AT-C sections of SSAE No. 18 (particularly AT-C Section 205 for agreed-upon procedures and AT-C Section 320 for reporting on controls) helps clarify the relationship between the standard and the report they will actually receive.


## What is SOC 2 and who actually reads it?


A SOC 2 report is an attestation report on controls at a service organization that are relevant to the[Trust Services Criteria](https://www.aicpa.org/cpe-learning/publication/soc-2-reporting-on-an-examination-of-controls-at-a-service-organization-relevant-to-security-availability-processing-integrity-confidentiality-or-privacy) . It is produced by an independent CPA firm and addresses how a vendor manages data security, system availability, and related operational commitments. The AICPA SOC 2 reporting guide provides CPAs with the examination framework and criteria mapping they need to conduct these engagements.


The five Trust Services Criteria categories:


- **Security** (CC series): always required; covers logical access, change management, and risk mitigation.
- **Availability** (A series): system uptime and performance commitments.
- **Processing Integrity** (PI series): complete, accurate, and timely processing.
- **Confidentiality** (C series): protection of information designated as confidential.
- **Privacy** (P series): collection, use, retention, and disposal of personal information.


Three audiences read SOC 2 reports regularly. Security and risk teams use them to evaluate vendor controls without conducting on-site assessments. Procurement and legal teams use them to satisfy third-party risk management requirements before contract execution. Sales teams share them proactively to shorten security review cycles, because a SOC 2 report often substitutes for lengthy security questionnaires during enterprise deals.


SOC 3 is worth a brief note here. It is a general-use summary of the SOC 2 examination, stripped of the detailed test results and auditor procedures. Vendors publish SOC 3 reports publicly; SOC 2 reports are restricted-use documents shared under NDA with customers and their auditors.


## How do SOC 2 and SSAE No. 18 actually differ?


The confusion between SOC 2 and SSAE 18 is understandable because one produces the other.[Microsoft's compliance documentation](https://learn.microsoft.com/en-us/compliance/regulatory/offering-soc-2) illustrates this clearly: a cloud provider's SOC 2 Type 2 report references SSAE No. 18 in its methodology section, showing how the standard and the report coexist in a single engagement.


Dimension SSAE No. 18 SOC 2 Report


What it is Attestation standard (AICPA) Attestation report (deliverable)


Purpose Governs auditor procedures and engagement conduct Documents controls and auditor findings for stakeholders


Controls tested Defines testing methodology for design and operating effectiveness Tests controls against Trust Services Criteria


Reporting period Not a report; sets requirements for point-in-time and period engagements Type 1: point-in-time; Type 2: defined period (commonly 3–12 months)


Typical users CPAs and auditors performing the engagement Customers, procurement teams, risk managers, regulators


When a buyer says "send us your SSAE 18," the correct response is: "We have a SOC 2 Type 2 report issued under SSAE standards. We can share it under NDA." That framing is accurate, professional, and avoids the false implication that SSAE 18 is a certification badge.


## What do the five Trust Services Criteria actually require?


Each TSC maps to specific control categories, evidence artifacts, and audit procedures. Security is mandatory in every SOC 2 engagement; the remaining four are selected based on the service organization's commitments to customers.


TSC Example Controls Common Evidence Artifacts


Security Multi-factor authentication, firewall rules, vulnerability scanning Access logs, firewall configuration exports, scan reports


Availability Redundancy architecture, incident response runbooks, uptime monitoring SLA reports, runbooks, monitoring dashboards


Processing Integrity Input validation, reconciliation procedures, error handling Processing logs, reconciliation reports, exception reports


Confidentiality Data classification policy, encryption at rest and in transit Encryption configuration, data classification register


Privacy Privacy notice, consent management, data retention schedules Privacy policy, consent records, retention logs


Selecting additional TSCs beyond Security expands the audit scope and the evidence collection burden proportionally. A vendor processing healthcare data will likely add Privacy and Confidentiality; a payment processor will add Processing Integrity. Choosing TSCs that do not reflect actual service commitments creates audit risk: auditors will test controls against those criteria regardless, and gaps become findings.


For teams managing[security and compliance documentation](https://solution4guru.com/security-and-compliance-in-itsm-what-you-need-to-know) within ITSM processes, mapping TSC requirements to existing control catalogs early reduces duplication and speeds evidence collection.


## SOC 2 Type 1 vs. Type 2: which one should you pursue first?


Type 1 tests whether controls are **designed** appropriately at a single point in time. Type 2 tests whether those controls **operated effectively** over a defined period,[commonly 3–12 months](https://governancedocs.com/soc-2-compliance/) , and many SaaS companies pursue Type 1 before Type 2 to stage their readiness.


**Decision checklist:**


1. **Product maturity.** If your control environment is less than six months old, Type 1 is the realistic starting point.
2. **Buyer expectations.** Enterprise procurement teams increasingly require Type 2; check your top five prospects' vendor questionnaires before deciding.
3. **Sales urgency.** A Type 1 report can unblock a pilot deal or proof-of-concept while you accumulate the observation period for Type 2.
4. **Resource readiness.** Type 2 requires sustained evidence collection across the full observation period, not just a point-in-time snapshot.
5. **Regulatory drivers.** If a customer contract or regulatory requirement specifies Type 2, that decision is already made for you.


**Scenario examples:**


- An early-stage SaaS vendor with a six-month-old security program pursues Type 1 to satisfy an enterprise pilot requirement, then moves to Type 2 after a 12-month observation period.
- A mid-market vendor with two years of documented controls goes directly to Type 2 because its top three customers have explicitly required it in their vendor agreements.
- A large cloud provider maintains annual Type 2 reports and shares them via a SOC 2 Type 2 overview in its trust portal, covering multiple TSCs across dozens of services.


For a deeper look at how report types affect sales cycles and procurement timelines, the SOC 2 Type reports guide covers both from a sales enablement angle.


## How does a SOC 2 audit actually work?


The audit lifecycle has five phases, and the observation period for Type 2 is where most organizations underestimate the operational discipline required.


**Audit lifecycle:**


1. **Readiness assessment.** Identify control gaps against selected TSCs before engaging an auditor. This phase often surfaces the most remediation work.
2. **Audit planning.** The CPA firm defines scope, system boundaries, subservice organization treatment, and testing procedures with management.
3. **Control operation period (Type 2 only).** Controls must operate consistently across the full observation window. Evidence must be collected and retained throughout, not reconstructed afterward.
4. **Fieldwork.** Auditors test controls through inquiry, observation, inspection of documents, and re-performance of procedures.
5. **Reporting.** The auditor issues the SOC 2 report, including the management assertion, system description, control objectives, test results, and opinion.


**Key roles:**


- **Management** prepares the system description and asserts that controls are fairly described and operating effectively.
- **Service auditor (CPA firm)** independently tests controls and issues the opinion.
- **Subservice organizations** are handled either by carving them out (scope excludes their controls) or inclusively (their controls are tested as part of the engagement).


**Typical cost drivers** include the number of TSCs in scope, the breadth of the system boundary, subservice organization complexity, the maturity of existing controls, the degree of evidence automation, auditor firm selection, and the volume of remediation work identified during readiness. For a detailed breakdown of SOC 2 compliance cost factors, the variables that move the budget most are scope breadth and evidence maturity.


## How to read a SOC 2 report when a vendor sends you one


Most vendor risk teams spend too long reading the wrong sections. The report has a defined structure; knowing where to look saves hours.


**Key sections to scan first:**


- **Management's description of the system.** Check that it matches what the vendor actually does for you. Vague or overly broad descriptions are a red flag.
- **Scope and system boundaries.** Confirm the services and infrastructure you rely on are included, not carved out.
- **Subservice organization disclosures.** Identify which third parties are carved out and whether their controls affect your risk posture.
- **Control objectives and tests performed.** Look for the specific controls tested and the evidence the auditor reviewed.
- **Auditor's opinion.** An unqualified opinion is the target. Any qualified opinion or adverse finding warrants a direct conversation with the vendor.
- **Complementary user-entity controls (CUECs).** These are controls the vendor expects you to implement. If you have not, the report's assurance does not fully apply to your environment.
- **Effective period (Type 2).** A report with a six-month observation window from two years ago tells you little about current operations.


**Follow-up questions when the report leaves gaps:**


- Which subservice organizations are carved out, and do you have their SOC reports?
- Have any exceptions or deviations been remediated since the report period ended?
- What monitoring is in place between audit periods?
- Are the CUECs documented and assigned to specific teams on our side?


A SOC 2 sample report breakdown walks through each section with annotated examples, which is useful for teams reading their first report.


## Preparation checklist and the pitfalls that derail most programs


Readiness is where the real work happens. Practitioners consistently observe that the heaviest lift in any SOC 2 program is preparation: defining scope, documenting controls, and collecting operational evidence across the observation period.


**Sequential readiness checklist:**


1. **Define scope.** Select TSCs, identify the system boundary, and list subservice organizations. Narrow scope reduces cost and audit risk.
2. **Map controls to TSCs.** For each selected criterion, document the specific controls in place and assign ownership.
3. **Document policies and procedures.** Written policies must exist and reflect actual practice. Auditors will compare documentation to observed operations.
4. **Build an evidence collection plan.** Identify what evidence each control requires, how often it is generated, and where it is stored.
5. **Implement monitoring and alerting.** Continuous monitoring demonstrates operating effectiveness far more convincingly than point-in-time screenshots.
6. **Conduct internal testing.** Test controls against TSC requirements before the auditor does. Identify gaps early.
7. **Remediate findings.** Prioritize by impact and likelihood. High-impact, high-likelihood gaps get fixed first; low-impact gaps may be accepted with documented rationale.
8. **Auditor readiness review.** Some organizations engage their auditor for a pre-audit walkthrough to confirm scope alignment and evidence readiness.


**Common pitfalls:**


- Vague system descriptions that do not match the services customers actually receive.
- Missing evidence for controls that exist in policy but were never operationalized.
- Inconsistent operational practices across teams or time periods.
- Unmanaged subservice gaps where a carved-out vendor's controls affect the service organization's commitments.
- Last-minute remediation that auditors can identify as reactive rather than systematic.


**Pro Tip:** *Automate evidence collection from day one of the observation period. Manual collection at the end of a 12-month window is error-prone and often incomplete. Tools that integrate with your existing infrastructure (cloud providers, ticketing systems, access management platforms) create continuous evidence trails that auditors can sample across the full period, rather than reconstructed exports.*


For a structured approach, the SOC 2 compliance checklist covers the seven steps in detail with practical templates.


## Where SOC 2 sits in the broader compliance ecosystem


SOC 2 is one report in a family of AICPA attestation reports, and it frequently coexists with other frameworks in enterprise vendor programs.


**The SOC report family:**


- **SOC 1:** Covers controls relevant to financial reporting (ICFR). Used by payroll processors, financial data handlers, and similar organizations where controls affect customers' financial statements. See the SOC 1 Type 2 reports guide for how the two report types interact in compliance workflows.
- **SOC 2:** Covers controls relevant to Trust Services Criteria. The standard for technology and SaaS vendors.
- **SOC 3:** A general-use, publicly shareable summary of the SOC 2 examination. No detailed test results; used for marketing and general assurance.


**Common framework crosswalks:**


- **ISO 27001 vs. SOC 2.** ISO 27001 is a certification standard for an information security management system (ISMS); SOC 2 is an attestation report on specific controls. The control sets overlap substantially, and organizations that have implemented ISO 27001 typically find SOC 2 readiness faster because many controls and evidence artifacts already exist. The key difference: ISO 27001 certifies a management system; SOC 2 attests to specific control effectiveness.
- **HIPAA.** Healthcare data processors often maintain both HIPAA compliance programs and SOC 2 reports. SOC 2's Privacy and Confidentiality criteria overlap with HIPAA's Privacy and Security Rules, but SOC 2 does not replace HIPAA compliance. The two are complementary.
- **SOC 2+.** When customers require additional criteria beyond the standard TSCs (such as HITRUST or NIST CSF criteria), auditors can perform a SOC 2+ engagement that incorporates those additional requirements into the same report.


Organizations that maintain multiple attestations typically find that a well-documented SOC 2 program reduces the marginal effort for ISO 27001 surveillance audits and HIPAA assessments, because the evidence collection and control documentation infrastructure is already in place.


## A practitioner's perspective on what actually matters


Most teams approach SOC 2 as a documentation exercise. That framing leads to the most common audit failure: controls that exist on paper but were never consistently operated. The auditor's job in a Type 2 engagement is specifically to find the gap between what management asserts and what the evidence shows.


Three tips that reflect real-world trade-offs:


**Scope narrowly, then expand.** Every additional TSC and every additional system component multiplies the evidence burden. Start with Security and the minimum system boundary that satisfies your buyers. Add TSCs in subsequent audit cycles as your program matures.


**Automate evidence before the observation period starts.** Retrofitting evidence collection after a 12-month window has closed is the single most expensive mistake in SOC 2 programs. Configure your logging, access reviews, and monitoring to generate auditable artifacts automatically from the first day of the period.


**Involve procurement early.** Your customers' vendor risk teams often have specific report requirements (observation period length, TSC coverage, subservice treatment) that are not visible until a deal is in late stages. Asking those questions at the start of a relationship, not during contract negotiation, prevents expensive scope changes mid-audit.


One caution worth stating plainly: a SOC 2 report attests to controls during a specific period. It does not guarantee continuous security posture between audit cycles. Overclaiming from a single report, especially one that is 18 months old, erodes the trust the report was meant to build. Pair your SOC 2 with continuous monitoring and operational controls that you can demonstrate to customers at any point in the year.


## Automation makes SOC 2 readiness faster and less painful


The heaviest cost in a SOC 2 program is not the audit fee. It is the internal time spent collecting, organizing, and presenting evidence across a 3–12 month observation period. Automation addresses that directly.


Skypher's[security questionnaire automation](https://skypher.co/security-questionnaires-automation) and Trust Center capabilities are built for exactly this workflow. When prospects or customers send security questionnaires tied to your SOC 2 controls, Skypher's AI-driven response engine pulls from your centralized knowledge base to answer accurately and consistently, reducing the manual back-and-forth that typically follows a report submission. Integrations with Slack, MS Teams, Confluence, Notion, Google Drive, OneDrive, and SharePoint mean your evidence and control documentation stays in the tools your team already uses, rather than scattered across email threads.


For teams preparing for a first SOC 2 audit or maintaining annual Type 2 cycles, the[Skypher Trust Center](https://skypher.co/trust-center) gives customers and auditors a single place to access your compliance posture, reducing the volume of ad hoc requests during fieldwork. Evaluate whether automation fits your readiness program by exploring Skypher's platform directly.


## Sources


The sources below are the primary references used throughout this article. Each is authoritative for the specific claim it supports.


- [SOC 2 reporting guide (AICPA)](https://www.aicpa.org/cpe-learning/publication/soc-2-reporting-on-an-examination-of-controls-at-a-service-organization-relevant-to-security-availability-processing-integrity-confidentiality-or-privacy)
- [SOC 2 Type 2 overview (Microsoft Learn)](https://learn.microsoft.com/en-us/compliance/regulatory/offering-soc-2)
- [SSAE 18: What You Need to Know - AuditBoard](https://www.auditboard.com/blog/ssae-18/)
- [SOC 2 Compliance Explained: A Complete 2026 Guide (GovernanceDocs)](https://governancedocs.com/soc-2-compliance/)


## Recommended


- [SOC 2 Compliance Requirements: Your 2026 Guide](https://blog.skypher.co/blog/soc-2-compliance-requirements-your-2026-guide)
- [Understanding SOC 2 AICPA: Your Guide to Compliance](https://skypher.co/post/understanding-soc-2-aicpa-compliance-en)
- [SOC 2 compliance guide for tech and finance in 2026](https://blog.skypher.co/blog/soc-2-compliance-guide-tech-finance-2026)
- [SOC 2 Certification: A Practical Guide for Tech and Finance](https://blog.skypher.co/blog/soc-2-certification-a-practical-guide-for-tech-and-finance)
