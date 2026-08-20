---
schema_version: "1.0.0"
document_id: "c86181b9fb7894195df25c62d4e4d02711ec16c6e414c2094db394ade8f35c6f"
company_key: "yc-medplum"
company: "Medplum"
source_id: "yc-medplum-atom-baaaecda9acc"
canonical_url: "https://www.medplum.com/blog/everself-case-study"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:32.588032+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:fd08481907885b45d4f7df5996bc0aec99411320d47ff259aacedc45717a3c22"
---

# Everself: Enhanced Obesity Care

[Everself](https://everself.com/) is building the **second line of defense for obesity care** . Every month, more than a million patients drop off GLP-1 medications, and most regain the weight within three to six months. Until now, the only durable alternative has been invasive bariatric surgery, which can require weeks away from work. Everself fills the gap with an **outpatient endoscopic weight loss procedure** that delivers bariatric-level results without surgery, wrapped in a longitudinal care program delivered by a full care team.


To run that program, Everself built **Orbit** , a custom EHR on Medplum. The name captures the design philosophy: the care team orbits around the patient, who is the central focus of everything the system does.


## Problem​


Everself's care model is longitudinal, not episodic. Each patient works with a dietician, a nurse practitioner, a physician assistant, and a care coordinator over months, across a mix of formal visits, text messages, phone calls, lab results, notes from third-party providers, and readings from a smart scale shipped to the patient's home.


Traditional EHRs treat each episode of care as self-contained, forcing providers to hop between tabs and systems to reconstruct a patient's story. Everself also operates as a platform: its physicians are 1099 contractors, and procedures happen at partner hospitals and ambulatory surgery centers (ASCs), so scheduling and partner communications have to work across organizations, not just inside one clinic.


## Solution​


Orbit organizes everything around a single principle: **one chronological timeline per patient** . Prescriptions, chart notes, internal team chat, SMS with the patient, upcoming appointments, lab orders, phone calls, and smart scale readings all appear in one stream, filterable by type. Every role on the care team, from intake coordinator to physician, uses the same interface and gets full context without tab-hopping.


Key capabilities the Everself team built on Medplum:


- **Unified[communications](https://www.medplum.com/docs/communications)** : Telephony is built into the EHR on Twilio. Providers call and text patients directly from Orbit; calls are recorded and transcribed automatically, and inbound calls route to the patient's assigned care team. The same timeline model extends to facility partners, giving Everself a built-in mini CRM for each hospital and ASC relationship.
- **Complex multi-site[scheduling](https://www.medplum.com/docs/scheduling)** : Booking a procedure means lining up a clinician, a room, and equipment at a partner facility — not just an open slot on one calendar. Orbit tracks availability for 1099 physicians and for each partner hospital and ASC, and books virtual visits, in-person appointments, and the endoscopic procedure itself through one flow.
- **[Charting](https://www.medplum.com/docs/charting) with custom templates** : Procedure note templates capture granular clinical detail such as stomach physiology and suture counts. This feeds device inventory tracking and, critically, lets Everself correlate procedure details with long-term patient outcomes.
- **Integrated[labs](https://www.medplum.com/docs/labs-imaging) and[medications](https://www.medplum.com/docs/medications)** : Lab orders, results, and prescriptions land in the same patient stream as everything else.
- **A triaged provider inbox** : One centralized inbox per care team member, with a triage system deciding what each role needs to see across chat, SMS, calls, faxes, and internal notes, a direct answer to information overload in healthcare.


> "With Medplum, it's just a lot more flexible, and we have a big vision for what we want our EHR to look like: integrating all the modern communications, a newsfeed style that shows longitudinal care rather than episodes of care, and a data structure we own that we can integrate more AI into."
>
>
> — Petch Jirapinyo, Everself


## What's Next​


With increasing demand from both individuals and institutions, Everself has a set of planned enhancements on the Orbit roadmap:


1. **[Care plans](https://www.medplum.com/docs/careplans)** : A native care plan system, launching now, to track each patient's journey across Everself's programs.
2. **Revenue cycle management** : Integrating[billing](https://www.medplum.com/docs/billing) and insurance reimbursement workflows into Orbit.
3. **AI** : With the data foundation in place, layering in LLM-powered summarization that ties together notes, calls, texts, and device data into the latest picture of each patient.


## Conclusion​


Orbit shows what a purpose-built EHR looks like when the data layer gets out of the way: a longitudinal timeline instead of episodic charts, communications and scheduling that span organizations, and a structure ready for AI. Medplum is proud to be the foundation, and we are excited to support Everself through its next phase of growth.


## References​


- [Everself](https://everself.com/)
- [Medplum Communications](https://www.medplum.com/docs/communications)
- [Medplum Scheduling](https://www.medplum.com/docs/scheduling)
