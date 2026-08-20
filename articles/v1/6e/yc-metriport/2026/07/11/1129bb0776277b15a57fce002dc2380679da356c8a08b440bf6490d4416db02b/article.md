---
schema_version: "1.0.0"
document_id: "1129bb0776277b15a57fce002dc2380679da356c8a08b440bf6490d4416db02b"
company_key: "yc-metriport"
company: "Metriport"
source_id: "yc-metriport-news-import-cb7fc021ddeb"
canonical_url: "https://www.metriport.com/blog/adt-feed-hospital-data"
published_at: "2026-07-07T12:00:00+00:00"
first_seen_at: "2026-07-22T04:12:30.400948+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:a2d5e0eb1ba239e858e83fc40d02eea4c58f97903660dea71d9afe7252d35a0c"
---

# What Is an ADT Feed in Healthcare? (July 2026)

ADT data moves fast and shows up across a range of healthcare workflows, so it's worth understanding exactly how it works. At its core, an ADT feed is a stream of real-time notifications that tells connected systems whenever a patient is admitted, discharged, or transferred. If you're building workflows on top of this data or just trying to make sense of it, here's what you need to know.


**TLDR:**


- ADT in healthcare tracks patient admissions, discharges, and transfers via real-time HL7 v2 messages sent to billing, care, and admin systems.
- An ADT feed is a live notification stream, not a database.
- CMS requires hospitals to make a reasonable effort to send ADT notifications to a patient’s primary care physician and other identified treating clinicians at the time of admission, discharge, or transfer ([42 CFR § 482.24(d)(5)](https://www.ecfr.gov/current/title-42/chapter-IV/subchapter-G/part-482/subpart-C/section-482.24) ).
- ADT alerts provide limited clinical detail: they typically lack lab results, medication details, or care plans, and offer no historical record. Feed quality also varies widely by hospital and EHR vendor.Metriport's real-time notifications alert care teams the moment an ADT event occurs, and can be paired with a network query to pull the discharge summary and other transition documents into the patient's record.


## What ADT Means in Healthcare


ADT stands for Admission, Discharge, and Transfer. Every time a patient enters a hospital, leaves one, or moves between care settings, an ADT is recorded and broadcast to connected systems.


Think of it as the hospital's running log of patient movement. A patient arrives at the emergency department, gets admitted to a med-surg unit, transfers to the ICU, and eventually discharges home. Each step generates an ADT with timestamps, patient demographics, location data, and attending provider details. That event then flows, in real time, to billing systems, care teams, and any external partners subscribed to the feed.


These records feed downstream workflows across the health system, including bed management, care coordination, and regulatory reporting, giving staff a shared, up-to-date view of where each patient is and what stage of care they are in.


## How ADT Events Are Triggered and Transmitted


A registration clerk admitting a patient, a nurse transferring them to the ICU, or a discharge planner finalizing paperwork all trigger a discrete HL7 message. That message gets routed through the facility's ADT system.


These messages travel over interfaces built on[HL7 v2](https://metriport.com/blog/ehr-interoperability-standards-hl7-fhir-health-data-exchange) , a widely deployed legacy messaging standard used by U.S. hospitals. Each message carries a structured payload: patient identifiers, event type codes, timestamps, and location details that downstream systems parse and act on automatically.


### HL7 v2 vs. FHIR for ADT Messages


ADT notifications are almost exclusively sent using HL7 v2. It has been the established clinical messaging standard since the 1980s and is widely used by hospitals to broadcast patient movements to downstream systems such as pharmacy, billing, and HIE networks.


FHIR, the federal standard for APIs and patient data access, is capable of representing ADT workflows but its adoption for ADT feeds is still growing. Because most hospital infrastructure is built on HL7 v2, modern systems typically translate traditional HL7 v2 ADT feeds into FHIR formats using middleware.


## Common ADT Event Types and HL7 Message Codes


Each ADT event code is mapped to a discrete clinical trigger, and a handful of message types account for the majority of these event codes.


### The Most Common ADT Message Types


Code Event Trigger


A01 Admit/Visit Notification Patient is admitted to inpatient care


A02 Transfer a Patient Patient moves to a different unit or bed


A03 Discharge/End Visit Patient is discharged or the visit closes


A04 Register a Patient Patient registers for an outpatient or ED visit


A08 Update Patient Information Demographics or insurance details change


A11 Cancel Admit An admission event is reversed


A13 Cancel Discharge A discharge event is reversed


‍


The cancel events (A11 and A13) are frequently overlooked but matter for downstream accuracy, since any system acting on an A01 or A03 needs to know if that event is later reversed.


‍


## What Data Lives Inside an ADT Feed


Each ADT message packages a snapshot of the patient's current state at the moment the event fires. The core fields present in most messages include:


- Patient demographics: name, date of birth, and gender
- Visit information: admission and discharge timestamps, visit identifiers, and encounter status
- Diagnosis: a diagnosis code and description tied to the encounter
- Patient location: facility name, and for transfers, the source and destination unit or department
- Attending physician: name and credentials


One distinction worth drawing clearly: an ADT feed is a notification stream, not a database. Each message represents a point-in-time event pushed to subscribers as it happens. A static ADT database, by contrast, is a record store built from those accumulated events over time. The feed tells you something just changed; the database tells you the full history of changes.


## How ADT Systems Improve Hospital Operations


Real-time ADT data gives hospitals a clearer picture of patient flow, which directly affects staffing, bed allocation, and discharge planning. When a patient is admitted, transferred, or discharged, that event triggers downstream coordination across nursing, housekeeping, billing, and care management teams. Without timely ADT feeds, these handoffs rely on manual communication that slows response times and creates gaps in care.


## ADT Feeds and Care Coordination Across Settings


When a patient moves between care settings, that transition is one of the highest-risk moments in their care journey. ADT feeds give care coordinators, health plans, digital health services, and post-acute providers real-time visibility into those moments, so follow-up can happen before gaps form.


For example, when a patient is discharged from a hospital, a payer receiving that ADT alert can trigger an outreach workflow within hours instead of days. Community health workers, care managers, and primary care teams can all act on the same event simultaneously.


## ADT Data and Value-Based Care


Value-based care models like ACOs and bundled payment arrangements tie reimbursements to outcomes, and ADT data is one of the few real-time signals available to act on. When a patient is admitted to an outside facility, a timely ADT notification gives care managers a window to intervene before a gap in care widens into a costly complication.


## CMS Requirements for ADT Notifications


The Centers for Medicare & Medicaid Services issued a rule ([42 CFR § 482.24(d)](https://www.ecfr.gov/current/title-42/chapter-IV/subchapter-G/part-482/subpart-C/section-482.24) ) requiring hospitals to send ADT notifications to a patient's primary care provider and other treating clinicians at the time of admission or registration, and immediately prior to or at the time of discharge or transfer. (The obligation falls on the hospital to send these notifications; there is no corresponding requirement that the receiving provider monitor or act on them.)


Hospitals that fail to meet this requirement risk losing Medicare and Medicaid participation. The rule applies to most Medicare-participating hospitals, pushing health systems to invest in reliable ADT data infrastructure to stay compliant. For a detailed breakdown, see the[CMS ADT notification requirements FAQ](https://www.cms.gov/priorities/burden-reduction/overview/interoperability/frequently-asked-questions/admission-discharge-transfer-patient-event-notification-conditions-participation-cop-42-cfr-482-24d) .


## Limitations and Challenges of ADT Data


ADT feeds have real constraints that teams should account for before building workflows around them.


- ADT data is administrative by design: it typically excludes vitals, imaging results, physician notes, and problem/allergy lists, the clinical context needed to understand what actually happened during the visit.
- Feed quality varies by hospital and EHR vendor, meaning missing fields, inconsistent formatting, and duplicate messages are common.
- Interoperability gaps persist across health systems, making it harder to align patient records, including ADTs, across organizations.
- HIPAA compliance requirements add governance overhead for any team receiving or storing ADT data.


## How Metriport Uses ADT Data for Real-Time Patient Monitoring


Metriport's ADT monitoring connects to feeds from Bamboo Health and[state and regional HIEs](https://metriport.com/blog/a-network-of-networks-the-new-internet-of-healthcare-data) , delivering real-time admission, discharge, and transfer notifications across the US healthcare system. What separates this from a standard alert feed is what happens immediately after the ADT fires: Metriport ties the alert to a[Patient Encounter Bundle](https://docs.metriport.com/medical-api/handling-data/patient-encounter-bundle) with the patient, encounter, and diagnosis data, and can trigger a network query to pull the discharge summary, medication list, and other transition documents into the patient's full longitudinal record.


Because the alert is tied directly to the encounter data and can pull in the fuller record, care teams can act during an encounter instead of learning about the admission days later through a claims file. For[value-based care organizations](https://metriport.com/blog/how-cercanos-care-solved-hie-connectivity-for-cms-access) managing attributed populations, that window is where readmissions get prevented and transitional care management billing gets captured.


## FAQ


### What is an ADT feed in healthcare and how does it differ from an ADT database?


An ADT feed is a real-time notification stream that pushes a message every time a patient is admitted, discharged, or transferred. An ADT database is a static record store built from those accumulated events over time; the feed tells you something just changed, while the database holds the full history of changes.


### What HL7 message types should I know for hospital ADT data?


The seven message types that cover most real-world ADT traffic are A01 (admit), A02 (transfer), A03 (discharge), A04 (register), A08 (update demographics), A11 (cancel admit), and A13 (cancel discharge). The cancel events are frequently overlooked, but any system acting on an A01 or A03 needs to receive A11 and A13 messages or its records will drift out of sync.


### ADT alerts vs. claims data for tracking patient transitions: which is better for value-based care?


ADT alerts give you real-time visibility into admissions and discharges, often within minutes of the event. Claims data arrives weeks or months later, long after the window to intervene has closed, making ADT the only practical signal for preventing readmissions and capturing transitional care management billing.


### How do I know if my organization meets the CMS ADT notification requirements?


CMS requires most Medicare-participating hospitals to send ADT notifications to a patient's treating clinicians at the time of admission or registration, and immediately prior to or at the time of discharge or transfer, with non-compliance risking loss of Medicare and Medicaid participation. If your hospital lacks a reliable outbound ADT feed connected to primary care providers and care teams, you are likely out of compliance with the rule that took effect in 2021.


### Why does ADT data alone often fall short for care coordination?


ADT messages capture movement events and a diagnosis code, but typically lack deeper clinical context like lab results, vitals, or care plans, which limits how actionable the alert is by itself. Connecting an ADT notification to a patient's full longitudinal record (including discharge summaries and medication lists) turns an alert into something a care team can actually act on.
