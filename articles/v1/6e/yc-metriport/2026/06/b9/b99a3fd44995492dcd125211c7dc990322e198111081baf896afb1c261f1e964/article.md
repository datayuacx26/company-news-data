---
schema_version: "1.0.0"
document_id: "b99a3fd44995492dcd125211c7dc990322e198111081baf896afb1c261f1e964"
company_key: "yc-metriport"
company: "Metriport"
source_id: "yc-metriport-news-import-cb7fc021ddeb"
canonical_url: "https://www.metriport.com/blog/cms-access-interoperability-checklist"
published_at: "2026-06-10T12:00:00+00:00"
first_seen_at: "2026-07-22T04:12:30.400948+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:8097f585e19965db17094bc92cbf25f2f0dd74cdafdbe8d17c27f213ada8ad36"
---

# The CMS ACCESS Interoperability Checklist

ACCESS has four Health IT deadlines across the first year of the program. This checklist covers what's due at each one (assuming a July 5, 2026 start), the implementation pitfalls to watch for, and how to sequence the work.


The full compliance checklist, vendor evaluation criteria, and FHIR reporting guidance are available in the[ACCESS Interoperability Playbook](https://go.metriport.com/access-playbook) .


*This checklist is based on CMS ACCESS Model RFA requirements as of publication. Requirements may be clarified or modified by CMS sub-regulatory guidance. This does not constitute legal or compliance advice.*


## **The HIT Requirements**


ACCESS's interoperability obligations are organized into three Health IT requirements:


- **HIT Req 1:** Standardized APIs for patient and population services
- **HIT Req 2:** Health information exchange connectivity
- **HIT Req 3:** Reporting outcomes to CMS via FHIR-based API


These aren't independent checkboxes. HIE connectivity is what surfaces the external lab results and medication history needed for OAP measure reporting. It's also the mechanism that enables the outbound push to a patient's care team. The FHIR reporting pipeline depends on that data being complete and correctly sourced before it reaches CMS. A gap in any one requirement creates downstream risks in the others.


‍


## **Deadline 1: July 5, 2026 | Program Start**


### **HIT Requirement: Confirm API Compliance**


ACCESS requires participants to support FHIR-based APIs meeting ONC standards and the current version of USCDI. Certified EHR Technology (CEHRT) isn’t required, but standardized API support is.


For most modern digital health platforms, this is a lower-lift requirement than HIE connectivity, but verify your implementation against ACCESS specifications before program start.


**What to confirm:**


- FHIR R4 support meeting ONC standards
- USCDI current version support
- Patient and provider-facing access enabled


**Pitfalls:** Organizations that built FHIR APIs for a specific use case (CMS interoperability rules, payer access) sometimes find their implementation is narrower than ACCESS requires. Confirm scope before program start.


**Data collection workflows**


Before the program start date, validate that your OAP measure collection methods meet ACCESS requirements. CMS will withhold or recover payment for submitted values that can't be substantiated, and collection method is part of what gets audited. \[1\]


Key requirements by data type:


- **Blood pressure:** Must use a validated upper-arm cuff device with timestamped, source-verifiable transmission. Manual entry is prohibited.
- **Lab values:** HbA1c, lipids, etc., must come from an HIE, CLIA-compliant point-of-care device, or lab network. Collection date and source must be documented.
- **Patient-reported outcomes:** Must be collected via web portal or web-first mixed-mode approaches, without modifying wording, response options, or layout.


Every reported value must include a documented collection date and method of collection.


**Pitfalls:** Organizations that currently collect BP through manual entry workflows or that haven't confirmed their device data includes the metadata needed for audit (timestamp, source, patient attribution) will have gaps to close before program start.


## **Deadline 2: July 15, 2026* | Care Coordination Infrastructure**


### **HIT Requirement: Care Coordination Capability**


ACCESS is designed to break down siloed care by requiring participants to proactively share clinical updates with a patient's broader care team.


Care Initiation updates are required within 10 days of a patient's first enrollment. That means your outbound communication infrastructure must be operational *before* patients start aligning, not after.


Three required reporting moments under ACCESS: \[2\]


- **Care Initiation (10 days):** Send a care plan summary to the identified care team covering baseline measures, treatment goals, and a care coordination contact.
- **Care Escalation (10 days):** Following any care setting transition, send a clinical update to the receiving clinician.
- **Care Completion (30 days):** Send an outcomes summary at the end of the performance period or upon disenrollment.


All three must go through a secure electronic method: Direct Secure Messaging, an HIE-supported push mechanism, or another HIPAA-compliant exchange. Phone and fax don't count.


**What to confirm:**


- Your platform can send Direct Secure Messages or use an HIE push mechanism
- You can look up each patient's PCP and referring clinician via an HIE or trusted directory at enrollment
- Care Initiation, Completion, and Escalation triggers are mapped into workflows with defined owners and timelines


**Pitfalls:** Organizations that have operated as closed systems, delivering care within their own platform without proactive outbound communication, often don't have this infrastructure. This is a buildout, not a configuration change.


## **Deadline 3: September 3, 2026* | Baseline Submission**


### **HIT Requirement: FHIR Reporting Pipeline**


Participants must submit baseline clinical measures to CMS via FHIR API within 60 days of each patient's alignment date. The submission window is per patient, not per cohort, so the clock restarts with each new alignment.


CMS uses the baseline snapshot to calculate improvement over time for OAP payment. A missed or incomplete baseline submission creates downstream payment risk.


**What to confirm:**


- The full data flow from collection to the CMS FHIR reporting server is mapped
- Submissions follow the Person-Centered Outcomes (PCO) HL7 FHIR Implementation Guide
- Measures that depend on external data (prior lab results, medication history) have confirmed HIE retrieval paths
- Enrollment workflow triggers baseline data collection immediately


**Pitfalls:** Organizations that haven't confirmed external data retrieval is automated might find themselves manually chasing records at the 60-day mark. If HIE connectivity isn't live and pulling records at alignment, the baseline may be incomplete.


** Dates calculated from July 5, 2026 program start. Actual deadlines are per patient: care coordination within 10 days of first enrollment; baseline submission within 60 days of patient alignment date.*


## **Deadline 4: July 5, 2027 | HIE Connectivity**


### **HIT Requirement: Full HIE Connectivity**


All ACCESS participants must establish and maintain a connection to an HIE that enables bidirectional exchange of patient records across all geographies where they deliver care. The deadline is one year from program start.


This is the longest lead-time requirement in ACCESS, and the one most commonly underestimated.


**What to confirm:**


- **Bidirectional:** Both query/retrieve and outbound push.
- **Coverage:** Actual coverage against your patient population by geography, not just a stated network footprint.
- **Data depth:** Lab results, medication history, and outpatient clinical records. Administrative or claims data alone won't support OAP measure sourcing.
- **FHIR output:** Data delivered in FHIR R4 reduces downstream integration burden for your CMS reporting pipeline.


**Pitfalls:** Onboarding timelines. Whether you're connecting through an EHR, a connectivity partner, or directly to a national network, the process involves credentialing, legal agreements, technical integration, and testing. Organizations that start this process in 2027 may be cutting it close.


## **Evaluating HIE Vendors**


If you're not yet connected to an HIE with sufficient coverage and depth for your patient population, vendor selection is a critical near-term decision. Four things to confirm before committing:


**Coverage.** Ask your vendor to show you coverage against your specific patient markets. "We cover all 50 states" and "we cover your patients in those states" are different claims.


**Bidirectionality.** Confirm the vendor supports outbound push via Direct Secure Messaging or HIE push mechanism, not just record retrieval.


**FHIR compatibility.** Older HIE integrations deliver data in HL7 v2 or CCD/C-CDA. Receiving FHIR R4 output reduces conversion work upstream of your CMS reporting pipeline.


**Onboarding timeline.** Ask specifically: how long from signed agreement to live bidirectional exchange? What has caused delays for other customers? A vendor that can't answer the second question may not have done enough implementations to know.


## **Get the Full Playbook**


The ACCESS Interoperability Playbook includes the complete compliance checklist with all milestones, full vendor evaluation criteria, data collection requirements by measure type, and an explanation of each HIT requirement.


[Download the ACCESS Interoperability Playbook →](https://go.metriport.com/access-playbook)


Metriport helps digital health organizations establish HIE connectivity quickly and securely, with connections to major national networks and multiple state and regional HIEs through a single, FHIR-native API. If you're working through implementation sequencing or need to close a connectivity gap before an ACCESS deadline, we can help.


[Talk to Metriport →](https://metriport.com/contact-us)


*This checklist and the ACCESS Interoperability Playbook are intended to support organizational readiness planning for the CMS ACCESS Model. They do not constitute legal, regulatory, or compliance advice. Organizations should review the official*[CMS ACCESS Model documentation](https://www.cms.gov/priorities/innovation/files/access-rfa.pdf) *and consult internal compliance stakeholders as appropriate.*


Sources:
\[1\] CMS. ACCESS Model: Request for Applications. February 12, 2026. Data Reporting & Sharing: Data Reporting Requirements.
\[2\] CMS. ACCESS Model: Request for Applications. February 12, 2026. Data Reporting & Sharing: Care Coordination Requirements.
