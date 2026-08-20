---
schema_version: "1.0.0"
document_id: "14a60d3501bc975d8bfc58b5443cfb17e30c1ceb8d15e70c9d855f404aa6586e"
company_key: "yc-metriport"
company: "Metriport"
source_id: "yc-metriport-news-import-cb7fc021ddeb"
canonical_url: "https://www.metriport.com/blog/the-hie-connectivity-gap-that-could-cost-access-participants"
published_at: "2026-06-05T12:00:00+00:00"
first_seen_at: "2026-07-22T04:12:30.400948+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:dceef77ff0b959eb959cd6175871ffa9bab0f2cf72df57f3a106f7e7798cd275"
---

# The HIE Connectivity Gap That Could Cost ACCESS Participants

CMS ACCESS is a 10-year program that enables tech-driven health companies to manage chronic conditions in Medicare patients, with payment tied directly to measurable health improvements. Where prior value-based care programs focused on utilization management, ACCESS pays on outcomes: blood pressure controlled, HbA1c reduced, kidney function preserved, behavioral health measures met.


> *"The ACCESS Model introduces an outcome-aligned payment option that rewards results. But we want outcomes, not engagement."* DR MEHMET OZ, CMS ADMINISTRATOR


That payment structure has a data corollary. If the outcome can't be measured, it can't be verified. If it can't be verified, it can't be reimbursed. ACCESS's Health IT requirements exist because outcomes-based payment only works when there's a complete, auditable clinical record to back it up.


ACCESS organizes its interoperability obligations into three HIT requirements:


- HIT Req 1: Standardized FHIR-based APIs for patient and population services
- HIT Req 2: Health information exchange connectivity
- HIT Req 3: Reporting outcomes to CMS via FHIR-based API


These aren't independent checkboxes. HIE connectivity is what surfaces the external lab results and medication history needed to substantiate OAP measures. It's also the mechanism that enables the outbound push to a patient's care team at required intervals. The FHIR reporting pipeline depends on that data being complete and correctly sourced before it reaches CMS. A gap in HIE connectivity creates downstream failures across all three requirements.


> *"Interoperability works on paper, but even after years of regulation, it's still too hard for providers and patients to get the information they need. The ACCESS Model is really the beginning of changing that."* JACOB SHIFF, CHIEF AI & TECHNOLOGY OFFICER, CMS INNOVATION CENTER


Many organizations entering ACCESS assume their existing HIE connection satisfies HIT Req 2, but not all connections do. Two considerations impact whether a connection is sufficient: coverage and capability. There isn't a single network that captures all patients, and a query initiation-only exchange doesn't satisfy the outbound push requirement regardless of how robust it is.


## The Outbound Push Many Organizations Haven't Built


ACCESS requires bidirectional exchange. Participants must be able to retrieve external clinical data needed for outcomes measurement and share patient updates with the broader care team at specific points to support coordinated care.


The inbound direction surfaces lab results, medication history, and prior records needed to substantiate OAP measures. The outbound direction satisfies ACCESS's care coordination requirement: care plan summaries sent within 10 days of care initiation, clinical updates within 10 days of a care transition, and outcomes summaries within 30 days of care completion. Each of those must go through Direct Secure Messaging, an HIE-supported push mechanism, or another HIPAA-compliant exchange method. Phone and fax don't count.1


For organizations that built their HIE integration for record retrieval, the outbound infrastructure may not exist.


## The Coordination Challenge Varies by Care Model


For organizations that have operated as closed systems, ACCESS makes secure outbound communication a compliance necessity. The practical challenge varies depending on how you deliver care:


### **Telehealth and virtual care**


**‍** Clinical workflows are in place; the question is whether their infrastructure can securely push updates to external PCPs and referring clinicians.


### **RPM and connected devices**


**‍** Continuous clinical data is an asset, but proactive care team communication infrastructure often doesn't yet exist.


### **Care management and navigation**


**‍** Already think in care team relationships, but updates often route through phone or fax rather than auditable electronic exchange.


### **Digital therapeutics and behavioral health**


**‍** Must meet the same coordination requirements as traditional providers, however asynchronous or app-based the care model.


CMS requires care coordination communications to include beneficiary identifiers, ACCESS participant contacts, baseline measures, treatment goals, active medications, and relevant OAP measures. Whatever your care model, the infrastructure to send that information electronically needs to be in place before patients are aligned.


## Coverage Against Your Patient Population


The major national networks (Carequality, CommonWell, and eHealth Exchange) provide broad coverage and operate alongside TEFCA, the federal framework for nationwide interoperability. Connecting through one or more of these networks will likely satisfy ACCESS's coverage requirement for your patient population.


Participants serving patients in a limited geography could consider a state or regional HIE as an alternative.


## Clinical Data Depth


For ACCESS, the relevant data is clinical: lab results (HbA1c, lipids, eGFR, uACR), medication history from pharmacy networks, outpatient visit notes, specialist records. Administrative or claims-adjacent data doesn't satisfy OAP requirements.


ACCESS allows participants to source OAP measures from HIEs or other CMS-approved partners, provided the collection date, data source, and measurement methodology are documented. CMS will withhold or recover payment if submitted values can't be substantiated2, so the data must be auditable. That means incomplete records aren't just a clinical liability, they're a compliance one.


> *"Without a complete patient record, you're trying to understand what might be wrong. With the data in hand, you can focus on what to do next." ‍* DAVID DUEL, FOUNDER & CEO, EASYHEALTH


A common failure mode: organizations assume their HIE returns a complete patient record, then discover at baseline submission that records are thin on the external clinical data that matters most for chronic condition management.


## Paths to Connectivity


For clinical organizations, connecting to an HIE is relatively straightforward. For digital health companies however, establishing a direct connection requires significant technical infrastructure, credentialing, legal agreements, and ongoing maintenance. Most ACCESS participants will likely connect through an existing technology provider:


### **EHRs**


**‍** Many major systems (such as Epic) have native connectivity to national networks and can serve as the connection layer for participants already operating on those platforms.


### **Network connectivity vendors**


**‍** Companies like Metriport provide HIE access via API, enabling participants to query and exchange data across networks without building direct integrations themselves.


The Health IT requirements are ultimately data requirements. The technical challenge isn't simply collecting data, it's connecting information across systems, care teams, and reporting workflows so outcomes can be measured, verified, and reimbursed. For organizations in the first cohort, the question is no longer whether this model works. It's whether your technical stack is ready to prove it.


## Key Considerations


Before assuming your current HIE connectivity satisfies ACCESS requirements:


### **Bidirectionality**


**‍** Can your platform push structured clinical updates to external providers via Direct Secure Messaging or an HIE push mechanism?


### **Coverage**


**‍** Has your vendor verified actual coverage against your patient population by geography, not just a stated network footprint?


### **Data depth**


**‍** Does your HIE connection return lab results, medication history, and outpatient clinical records? Administrative and claims data alone won't support OAP measure sourcing.


### **FHIR output**


**‍** Does the data arrive in FHIR R4, or does your team need to handle conversion from HL7 v2 or CCD/C-CDA? Receiving FHIR reduces the downstream integration burden for your CMS reporting pipeline.


---


We put together a technical guide to ACCESS interoperability for participants navigating these requirements. **The ACCESS Interoperability Playbook** covers all three HIT requirements, a compliance checklist organized by deadline, vendor evaluation criteria for HIE connectivity, and practical guidance on implementation sequencing.


[Download the ACCESS Interoperability Playbook →](https://go.metriport.com/access-playbook?utm_source=blog&utm_medium=organic&utm_campaign=access_playbook)


---


\[1\] CMS.[ACCESS Model: Request for Applications.](https://www.cms.gov/priorities/innovation/files/access-rfa.pdf) February 12, 2026. Data Reporting & Sharing: HIT Requirements and Care Coordination Requirements.
\[2\] CMS.[ACCESS Model Payment Amounts and Performance Targets.](https://www.cms.gov/priorities/innovation/files/access-payments-amts-perf-targets.pdf) February 2026.
