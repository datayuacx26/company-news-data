---
schema_version: "1.0.0"
document_id: "421b7a8a687b66fb3ac9cf81dfd4c2f9d440a6b97c8ca9d1d5abd524200a8157"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/soc-1-vs-soc-2-report"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-10T16:33:34.318118+00:00"
fetched_at: "2026-08-10T16:33:35.954060+00:00"
content_hash: "sha256:6e0222155dfbdd009b8971f3af30709385569e2a459c1657ab102ca2cfa29cf1"
---

# SOC 1 vs SOC 2 Report: Differences, When Each Applies, and Which One You Need

A **SOC 1 report** examines a service organization's internal controls over financial reporting controls that touch a customer's financial statements. A **SOC 2 report** examines controls over data security, availability, processing integrity, confidentiality, and privacy controls that touch a customer's data.


Both are attestation reports issued by a licensed CPA firm under standards set by[AICPA](https://us.aicpa.org/interestareas/frc/assuranceadvisoryservices/serviceorganization-smanagement) . Both come in Type 1 and Type 2 variants. And most service organizations[accounting firms](https://www.finlens.app/accountants) , SaaS companies, payroll processors, benefits administrators, medical billing companies, cloud infrastructure providers end up needing one or both as they move upmarket.


The choice is not arbitrary. SOC 1 and SOC 2 audit different control environments, are read by different audiences, and cost meaningfully different amounts. Picking wrong one wastes six to twelve months of audit prep, so definition is worth getting right.


## What is a SOC report


**SOC** stands for **System and Organization Controls** a framework AICPA maintains for third-party attestation over a service organization's control environment.


A service organization is any company that operates a system, process, or platform that a user organization (customer) relies on. If your customers use your service in a way that affects either (a) their financial statements or (b) their data security posture, they will eventually ask you to demonstrate that your controls are working. That is what a SOC report is: an independent CPA firm's written opinion that your controls are designed appropriately and (in a Type 2 report) operating effectively.


The current governing standard is **SSAE No. 21** , which superseded SSAE 18 in 2022. SSAE 21 lives under two AT-C sections:


- **AT-C 205** used for SOC 1 examinations
- **AT-C 105 + subject-matter guidance** used for SOC 2 examinations


Only a licensed CPA firm can issue a SOC report. Audit fees run from roughly $15,000 at small end to well into six figures for enterprise-scale examinations a materially higher line item than typical[bookkeeping and CAS pricing](https://www.finlens.app/blogs/bookkeeping-services-fees) , and one that most service organizations budget for annually.


There are three SOC report types in framework:


- **SOC 1** controls over financial reporting (ICFR)
- **SOC 2** controls over security, availability, processing integrity, confidentiality, and privacy
- **SOC 3** a general-use public summary of a SOC 2 (fewer details, safe to publish externally)


Most service organizations focus on SOC 1 or SOC 2 (or both). SOC 3 is a marketing artifact a shorter version of SOC 2 that can be posted on a public website. This guide covers SOC 1 and SOC 2 in depth.


## SOC 1 explained


A **SOC 1 report** examines a service organization's controls that could affect a user organization's **Internal Controls over Financial Reporting (ICFR)** . It is report a customer's financial-statement auditor uses to gain comfort that outsourced processes are not introducing errors into customer's books.


If your service touches your customer's ledger, accounts receivable, payroll, accruals, revenue recognition, or any process that flows into a financial statement, you are a SOC 1 candidate.


**Common SOC 1 candidates:**


- **Payroll processors** ADP, Paychex, Gusto because payroll data flows into customer's compensation expense and payroll liability accounts.
- **Benefits administrators** 401(k) recordkeepers, health plan administrators same reason.
- **Medical billing companies** claims processed become AR on customer's books.
- **Claim-processing companies** insurance claims that generate loss reserves.
- **SaaS platforms that touch financial data** billing platforms, revenue-management platforms, and platforms that sync with[QuickBooks](https://www.finlens.app/resources/quickbooks-automation) or Xero on a customer's behalf, along with[Client Accounting Services](https://www.finlens.app/blogs/client-accounting-services) firms operating as service organizations for their client base.
- **Loan servicers and transfer agents** cash and receivables flow through them.


The controls tested in a SOC 1 are ones service organization identifies as **relevant to user auditors** . Typical control domains include:


- Authorization and approval of transactions
- Completeness and accuracy of data processed
- Timeliness of processing
- Change management on underlying system
- Access controls that segregate duties
- [Reconciliation](https://www.finlens.app/blogs/reconciliation-in-accounting) between service organization's records and user organization's records


The primary audience for a SOC 1 is user organization's **financial-statement auditor** . The user CFO or controller may read it, but report is written for auditor.


## SOC 2 explained


A **SOC 2 report** examines a service organization's controls against AICPA **Trust Services Criteria** a defined set of principles that govern how systems handle data. Where SOC 1 is scoped to financial statement impact, SOC 2 is scoped to data-related risk.


There are **five Trust Services Criteria** (defined in[AICPA Trust Services Criteria document](https://us.aicpa.org/content/dam/aicpa/interestareas/frc/assuranceadvisoryservices/downloadabledocuments/trust-services-criteria.pdf) ), and organization being audited selects which apply to scope of its report:


- **Security** (mandatory in every SOC 2) system is protected against unauthorized access, both physical and logical.
- **Availability** system is available for operation as committed in service-level agreements.
- **Processing Integrity** system processing is complete, valid, accurate, timely, and authorized.
- **Confidentiality** information designated as confidential is protected.
- **Privacy** personal information is collected, used, retained, disclosed, and disposed of in conformity with commitments and applicable regulations.


Security is required. The other four are optional and are selected based on what service organization commits to its customers.


**Common SOC 2 candidates:**


- SaaS and cloud software vendors of every kind
- Data centers and cloud infrastructure providers
- Managed service providers (MSPs) and MSSPs
- Healthcare technology handling PHI
- FinTech platforms handling sensitive customer data
- Analytics and data platforms


Enterprise buyers now routinely require a SOC 2 Type 2 in security-review phase of procurement. For most B2B SaaS, "we have a SOC 2" is table stakes for a mid-to-large-enterprise sale. Companies without a SOC 2 either pass on those deals or spend sales cycle answering security questionnaires in place of a report.


## Type 1 vs. Type 2 same concept for both SOC 1 and SOC 2


Both SOC 1 and SOC 2 come in Type 1 and Type 2 variants. The distinction is same in both:


**Type 1** an examination of **design** of controls **at a single point in time.** The auditor opines that, as of a specific date, controls are suitably designed to achieve control objectives. A Type 1 is faster and less expensive; it is often first report an organization pursues.


**Type 2** an examination of **operating effectiveness** of controls over a **defined period** , typically six to twelve months. The auditor opines that controls are both suitably designed and operating effectively throughout period. A Type 2 requires organization to have been running those controls for full period, which is why organizations usually pursue a Type 1 first and follow up with a Type 2 next audit cycle.


Enterprise buyers almost always want a **Type 2** in due diligence a point-in-time snapshot is not enough to prove controls actually run. Small and early-stage service organizations often ship a Type 1 as a stopgap while they generate operating history needed for a Type 2.


Cost roughly doubles between Type 1 and Type 2 because of added evidence-gathering across operating period.


## SOC 1 vs SOC 2: side-by-side


Attribute


SOC 1


SOC 2


What it examines


Internal controls over financial reporting (ICFR)


Controls against Trust Services Criteria


Standard


SSAE 21; AT-C 205


SSAE 21; AT-C 105


Primary audience


User organization's financial-statement auditor


User organization's security, risk, and procurement teams


Common candidates


Payroll processors, benefits admins, CAS firms, medical billing, transfer agents


SaaS, cloud infrastructure, MSPs, FinTech, HealthTech


Trigger question


"Does your service affect your customer's financial statements?"


"Does your service store or process your customer's data?"


Type 1


Design at a point in time


Design at a point in time


Type 2


Design + operating effectiveness over 6–12 months


Design + operating effectiveness over 6–12 months


Who can issue


Licensed CPA firm


Licensed CPA firm


Typical annual fee


$15,000–$100,000+


$15,000–$150,000+


An organization can pursue both. Payroll processors typically hold both SOC 1 for customer's auditor, SOC 2 for customer's security team.


## How to decide which report you need


Not every service organization needs both. Four questions get most organizations to right answer:


### **1. Does your service directly affect your customer's financial statements?**


**‍** If your service creates, records, calculates, or transmits amounts that flow into customer's ledger, you are a SOC 1 candidate. If answer is no, you probably do not need SOC 1.


### **2. Does your service store or process sensitive customer data?**


If answer is yes, you are a SOC 2 candidate. Almost every SaaS company answers yes.


### **3. What are your customers actually asking for?**


The strongest signal is report your enterprise customers request during procurement or security reviews. If customers keep asking for SOC 2 in security questionnaires, do SOC 2. If customers' auditors keep asking for SOC 1 during their annual audit, do SOC 1.


### **4. What is your regulatory or contractual environment?**


Some verticals have de facto standards. Payroll and benefits SOC 1. Cloud infrastructure SOC 2. Healthcare data SOC 2 with Confidentiality and Privacy TSCs. Financial-services technology often both.


A useful heuristic: if you touch **money** , expect SOC 1. If you touch **data** , expect SOC 2. If you touch both, expect both.


## When to start preparing


Regardless of which report you pursue, timeline from decision to issued Type 2 report is typically **9 to 15 months.** That breaks down roughly as:


- **1–3 months** control-environment gap analysis and remediation
- **1–2 months** control documentation and policy writing
- **3–6 months** Type 1 audit + issuance (optional; skipped if going straight to Type 2)
- **6–12 months** operating period (evidence collection for Type 2)
- **1–2 months** Type 2 fieldwork and report issuance


Service organizations that start SOC preparation only after a customer demands a report typically lose deal. The report cannot be produced retroactively Type 2 requires demonstrable operating history.


## How Finlens supports SOC audit preparation


For accounting firms, CAS providers, and financial-adjacent SaaS platforms subject to a SOC 1 examination, audit substrate is general ledger and its associated controls. Auditors examine authorization evidence,[reconciliation](https://www.finlens.app/blogs/reconciliation-in-accounting) documentation, change logs, and control-activity trails to conclude on ICFR.


[Finlens](https://www.finlens.app/founders) generates that substrate automatically. The tamper-evident **audit log** records every categorization, edit, and posting with user, timestamp, and change delta evidence a SOC auditor requests in walkthrough and testing. Automated bank[reconciliation](https://www.finlens.app/blogs/reconciliation-in-accounting) and month-end close on top of[QuickBooks](https://www.finlens.app/resources/quickbooks-automation) or Xero produce completeness and accuracy control documentation without a firm having to build a separate audit-evidence workflow.


For services firms that also produce compliance-relevant financial statements (revenue recognition under ASC 606,[lease accounting](https://www.finlens.app/blogs/asc-842-lease-accounting) under ASC 842), Finlens's automated GAAP schedules become supporting workpaper for controls testing rather than being reconstructed from spreadsheets at audit time. The audit prep hours that would otherwise consume partner's calendar collapse into a review pass on already-generated documentation.


## Conclusion


**SOC 1 audits money. SOC 2 audits data.** The right report for your organization depends on which of those your service actually affects and often answer is both.


Whichever you pursue, audit is only as smooth as underlying control evidence. Firms that automate substrate audit trail, close documentation, reconciliation spend audit cycle on control walkthroughs and remediation, not on reconstructing evidence.


[Book a 20-minute walkthrough with Finlens](https://cal.com/finlens/intro) and bring last SOC audit request-list your customer sent you. We will show you which items automation covers, and which still require a manual workflow.


## FAQ


### **What is difference between a SOC 1 and SOC 2 report?**


**‍** SOC 1 examines internal controls over financial reporting controls affecting a customer's financial statements. SOC 2 examines controls against Trust Services Criteria security, availability, processing integrity, confidentiality, and privacy. SOC 1 is for financial-statement auditors; SOC 2 is for security and procurement teams.


### **Does SOC 2 replace SOC 1?**


**‍** No. They cover different control domains. An organization can need both. Payroll processors and CAS firms serving enterprise clients often hold both SOC 1 for client's auditor, SOC 2 for client's security review.


### **Who needs a SOC 1 Type 2 report?**


Service organizations whose activities affect their customers' financial statements payroll processors, benefits administrators, medical billing, claims processing, CAS firms, transfer agents where those customers' financial-statement auditors need to rely on service organization's controls.


### **What is difference between Type 1 and Type 2?**


Type 1 examines design of controls at a single point in time. Type 2 examines both design and operating effectiveness over a defined period, typically six to twelve months. Enterprise customers usually require Type 2.


### **How much does a SOC audit cost?**


Fees vary with scope, control count, and organization size. A first-time SOC 2 Type 1 for a small SaaS company commonly runs $15,000 to $40,000. Type 2 examinations run higher. Enterprise-scale SOC 1 or SOC 2 examinations can exceed $100,000 annually.
