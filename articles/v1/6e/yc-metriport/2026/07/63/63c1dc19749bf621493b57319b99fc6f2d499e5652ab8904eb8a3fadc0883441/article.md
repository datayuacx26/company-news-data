---
schema_version: "1.0.0"
document_id: "63c1dc19749bf621493b57319b99fc6f2d499e5652ab8904eb8a3fadc0883441"
company_key: "yc-metriport"
company: "Metriport"
source_id: "yc-metriport-news-import-cb7fc021ddeb"
canonical_url: "https://www.metriport.com/blog/fhir-r4-resources-guide"
published_at: "2026-07-14T12:00:00+00:00"
first_seen_at: "2026-07-22T04:12:30.400948+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:ae118bf604d20d57f148a81932f724cd4c91ec36ddf9fbf702c22377c3bb96dd"
---

# FHIR R4 Spec Guide: Resources & Standards (July 2026)

FHIR R4 (Fast Healthcare Interoperability Resources, Release 4) is the current regulatory baseline for healthcare interoperability in the U.S. This guide covers the core FHIR R4 resources, including` Patient` ,` Practitioner` ,` Medication` ,` Organization` , and` Appointment` .


**TLDR:**


- FHIR R4 is the HL7 version referenced by current ONC and CMS interoperability regulations, making it the standard for federally required interoperability APIs
- R4 organizes 140+ resources into 7 categories: Foundation, Administration, Clinical, Diagnostic, Medications, Financial, and Workflow.
- US federal interoperability rules are written against R4, making it the safe choice over R5 for compliance-driven teams.
- The full spec can be downloaded at hl7.org/fhir/R4.


## What Is FHIR R4?


FHIR R4 is HL7's 2019 update to the[FHIR standard](https://metriport.com/blog/what-is-hl7-fhir) . It serves as the current normative baseline for health data exchange, defining how clinical and administrative information gets structured, queried, and shared across systems using a REST-based API approach.


R4 introduced the first normative content in FHIR's history, meaning certain core resources, like` Patient` and` Observation` , became stable enough that teams could build implementation commitments against them.


## Why FHIR R4 Became the Industry Standard


The ONC 21st Century Cures Act Final Rule and the CMS Interoperability and Patient Access Final Rule ([CMS-9115-F](https://www.federalregister.gov/documents/2020/05/01/2020-05050/medicare-and-medicaid-programs-patient-protection-and-affordable-care-act-interoperability-and) ) established FHIR R4 as the foundation for federally required interoperability APIs, accelerating health data exchange across the industry.


## FHIR R4 Resource Categories


Resources are the core building blocks of FHIR R4. Every piece of clinical or administrative data maps to a typed resource, and HL7 organizes all of them into seven functional categories.


Category Example Resources


Foundation CapabilityStatement, OperationOutcome, Bundle


Administration Patient, Practitioner, Organization, Encounter


Clinical Condition, Observation, AllergyIntolerance, Procedure


Diagnostic DiagnosticReport, ImagingStudy, Specimen


Medications Medication, MedicationRequest, MedicationStatement


Financial Coverage, Claim, ExplanationOfBenefit


Workflow Task, Appointment, ServiceRequest, Communication


These categories are not rigid silos. A` Condition` resource lives in Clinical, but a` ServiceRequest` referencing that condition sits in Workflow.


## Core Administrative Resources: Patient, Practitioner, Organization, and RelatedPerson


These four resources form the identity layer that everything else in FHIR R4 points back to.


- ` Patient` : carries demographic and identity data, including name, date of birth, gender, address, and identifiers like MRN. Nearly every clinical resource links back to the patient by referencing this resource's ID, separate from identifiers like MRN.
- ` Practitioner` : represents an individual provider, with fields for name, identifiers (such as NPI), and qualifications. Referenced by` Encounter` ,` Observation` ,` DiagnosticReport` , and others.
- ` Organization` : describes a healthcare entity, such as a hospital or clinic, with type, address, and organizational identifiers. Practitioners are linked to organizations via` PractitionerRole` .
- ` RelatedPerson` : captures someone with a personal relationship to the patient, like a guardian or spouse. Used when that person participates in care or serves as an emergency contact.


` PractitionerRole` is a join resource that connects a` Practitioner` to an` Organization` with a specific role and specialty. It's a good example of[interoperability in healthcare](https://www.metriport.com/blog/what-is-interoperability-in-healthcare) because it standardizes how provider affiliations are represented across systems.


## Core Clinical Resources: Condition, Observation, and DiagnosticReport


These form the clinical data layer in most FHIR R4 implementations.


- ` Condition` represents a diagnosis or clinical problem, coded in SNOMED CT or ICD-10. Key fields include` clinicalStatus` (` active` ,` resolved` ,` inactive` ),` verificationStatus` (` confirmed` ,` unconfirmed` ,` refuted` ), onset date, and a patient reference. Problem lists and encounter reason fields both draw from this resource.
- ` Observation` handles point-in-time measurements: lab results, vital signs, social history responses, and clinical scores. Each instance carries a code, a value (numeric, string, or coded), and a status.
- ` DiagnosticReport` groups related` Observation` resources into a structured report. A metabolic panel becomes one` DiagnosticReport` referencing each individual result as its own` Observation` resource.


The` Observation` -to-` DiagnosticReport` relationship is where modeling decisions matter most.` Observation` resources can stand alone for something like a blood pressure reading, but a panel or imaging study belongs inside a` DiagnosticReport` so the clinical context is preserved.


## The FHIR R4 Encounter Resource


The` Encounter` resource tracks every interaction between a patient and the healthcare system, whether that's an office visit, emergency department stay, telehealth call, or inpatient admission. Each` Encounter` record captures` status` ,` class` ,` type` ,` subject` ,` participant` ,` period` , and` reason` , giving downstream systems a structured account of what occurred, when, and who was involved.


` Encounter` status values include` planned` ,` arrived` ,` in-progress` ,` finished` , and` cancelled` .


## Workflow Resources: Task, ServiceRequest, Appointment, and Communication


These resources cover the moving pieces of a care workflow, from scheduling through follow-up.


- ` Appointment` : a scheduled time slot for an upcoming patient-provider interaction, which can be cancelled or rescheduled.
- ` ServiceRequest` : an order or referral triggering a downstream clinical action, such as a lab test, imaging study, or specialist consultation, referencing the requesting practitioner, patient, and coded service.
- ` Task` : a general-purpose work item assigned to a person, team, or system, for anything from reviewing a result to coordinating steps in a care workflow. Each` Task` tracks its own` status` ,` owner` , and` priority` .
- ` Communication` : records a message or notification passed between parties, such as a care team alert or patient outreach. Unlike a` Task` , it documents what was communicated, not what work to assign.


## Document and Coverage Resources: DocumentReference, Coverage, and Procedure


These resource types handle documentation, financial coverage, and clinical procedures in FHIR R4.


` DocumentReference` describes an existing clinical document, such as a[C-CDA](https://www.metriport.com/blog/what-is-c-cda) , PDF, or scanned record, storing metadata like document type, author, and a URL or base64-encoded content.` Coverage` captures insurance details, including payer, subscriber, and benefit period.` Procedure` records actions performed on or for a patient, such as surgeries or counseling sessions, and supports both completed and in-progress statuses.


## FHIR R4 Bundle: Grouping and Exchanging Resources


The` Bundle` resource wraps multiple FHIR resources into a single HTTP payload. R4 defines several bundle types, and the ones you'll most commonly work with are:


- ` transaction` bundles are atomic, meaning all entries succeed or none do, making them useful for writes that must stay consistent.
- ` batch` bundles process each entry independently, so one failure does not block the rest.
- ` document` bundles represent a clinical document anchored by a` Composition` resource. It's worth comparing this format with C-CDA (see[C-CDA vs FHIR differences](https://www.metriport.com/blog/c-cda-and-fhir-key-differences) ).
- ` message` bundles support event-driven exchange and always begin with a` MessageHeader` .
- ` collection` bundles group resources without implying any processing behavior.


Each entry carries a` resource` object and a` request` field specifying the HTTP verb and target URL. When a server needs to report an error, warning, or informational response, it returns an` OperationOutcome` resource containing coded issue entries that describe exactly what happened.


## FHIR R4 RESTful API, HTTP Interactions, and Formats


FHIR R4 uses REST as its primary interaction model, though the spec also supports Messaging and Document exchange. Resources are exchanged over HTTP using standard methods: GET for retrieval, POST for creation, PUT for updates, and DELETE, when supported. (For a deeper look, see[how FHIR APIs work](https://www.metriport.com/blog/how-fhir-apis-work) .)


Supported formats include JSON and XML, with JSON being the more widely adopted choice in modern implementations. Every request and response carries a` Content-Type` header specifying` application/fhir+json` or` application/fhir+xml` .


## FHIR R4 Profiles, Implementation Guides, and Validation


Profiles narrow base FHIR R4 resources for specific use cases, extending elements or restricting cardinality to fit real-world needs. Implementation guides package these profiles with examples, terminology bindings, and usage rules into a deployable specification.


The US Core Implementation Guide is the most widely adopted profile set in the US, built on FHIR R4 and required for ONC certification (currently US Core 6.1.0 for new certifications under HTI-1 and USCDI v3; the applicable version depends on the certification edition). Teams that need to query this data downstream often rely on[FHIR conversions for analytics](https://metriport.com/blog/introducing-fhir-transforms-for-analytics) . Validation tools like the official HL7 FHIR validator check resources against both base specs and custom profiles.


## FHIR R4 vs R5 vs R6: Choosing the Right Version


R4B, a targeted patch release published in May 2022, fixed specific issues in resources like` Citation` and` Evidence` but saw limited adoption. R5 followed in March 2023, though production uptake remained limited. US federal interoperability rules are written against R4, including[CMS-0057-F](https://www.federalregister.gov/documents/2024/02/08/2024-00895/medicare-and-medicaid-programs-patient-protection-and-affordable-care-act-advancing-interoperability) (2024), which mandates FHIR R4-based APIs for prior authorization and payer-to-payer exchange, giving compliance-driven teams little practical reason to migrate. Many organizations plan to skip R5 entirely and move directly from R4 to R6 once it finalizes. For any system subject to ONC or CMS rules today, R4 remains the safe compliance choice. Teams building outside those mandates may evaluate R5 or early R6 drafts.


## FHIR R4 Download, Tooling, and Getting Started


The official FHIR R4 specification package is available at[hl7.org/fhir/R4](https://hl7.org/fhir/R4) , where you can download the full spec as a ZIP archive containing definitions, schemas, examples, and validator tooling.


For development, two open-source libraries dominate:


- HAPI FHIR: a Java-based library with a built-in FHIR server, widely used for building FHIR endpoints and running local test environments.
- Firely SDK: a .NET library covering parsing, validation, and server-side resource handling.


Both let you spin up a local FHIR R4 server in minutes, so you can test resource creation and search queries without a production endpoint.


Terminology lookups can be handled with an[open-source healthcare terminology server](https://www.metriport.com/blog/introducing-open-source-healthcare-terminology-server) . Two more resources worth knowing early:` Questionnaire` , which defines a structured form or survey, and` QuestionnaireResponse` , which captures a patient's answers. Both appear in patient-reported outcome tools and intake workflows, and neither requires a custom profile.


## Final Thoughts on Understanding FHIR R4 and Its Core Resources


Learning FHIR R4's resource model pays off quickly. Once you see how` Patient` ties to` Encounter` , how` Observation` feeds into` DiagnosticReport` , and how` Bundle` wraps it all for transport, the rest of the spec is easier to navigate.


## FAQ


### What is FHIR R4 and why do US healthcare systems still use it over R5?


FHIR R4 is HL7's Fast Healthcare Interoperability Resources standard, published in 2019. It introduced normative content, meaning core resources like` Patient` ,` Observation` , and` Bundle` had their structure locked in, with HL7 guaranteeing backward compatibility.


US healthcare systems still use R4 because federal interoperability rules from ONC and CMS are written against it, and those rules predate R5, which arrived in 2023 with limited vendor adoption. HL7 has since decided to skip R5 for US regulatory purposes and build the next version of US Core directly on R6, so future rules are expected to target R6 once it publishes.


### What is the difference between a FHIR R4 Encounter and a FHIR R4 Appointment?


An` Appointment` represents a scheduled interaction before the visit occurs and can be cancelled or rescheduled, while an` Encounter` is created once the patient arrives and the interaction begins. The` Encounter` resource captures` status` ,` class` ,` period` ,` participants` , and` reason` , giving downstream systems a structured record of what happened, when, and who was involved.


### Should I use a FHIR R4 transaction Bundle or a batch Bundle for writing patient data?


Use a` transaction` bundle when all writes must succeed together: it processes entries atomically, so one failure rolls back the entire operation. Use a` batch` bundle when entries are independent and a single failure should not block the rest from processing.


### How do I search for a FHIR R4 Patient resource using the RESTful API?


Send a GET request to your FHIR R4 endpoint at` \[base\]/Patient?` with search parameters like` family` ,` birthdate` ,` identifier` , or` gender` . The server returns a` Bundle` of type` searchset` containing matching` Patient` resources, and any errors or warnings come back as an` OperationOutcome` with coded issue entries describing what went wrong.
