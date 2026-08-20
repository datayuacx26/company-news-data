---
schema_version: "1.0.0"
document_id: "141f668f74149b51eec18ed606dc4d0922207876402b1502de73f1d01ca4b395"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-healthlake/"
published_at: "2026-07-24T21:00:00+00:00"
first_seen_at: "2026-07-31T16:43:36.463186+00:00"
fetched_at: "2026-07-31T16:43:37.565078+00:00"
content_hash: "sha256:576df041283a3e3aff7abe0acbd4b14529453030009ea488ec4bf387d744ecd9"
---

# AWS HealthLake identifies and links duplicate patient, provider, and organization records (Preview)

Duplicate patient records are one of the costliest problems in healthcare data management, scattering a patient's information, leading to redundant tests, missed diagnoses, billing errors, manual reconciliation, and broken analytics that count one patient as many.


[AWS HealthLake](https://aws.amazon.com/healthlake/) now supports resource matching, which automatically identifies and links duplicate records in a datastore. Healthcare organizations can build accurate longitudinal patient records and trustworthy population health datasets without specialized master data management tooling.
Resource matching works across seven Fast Healthcare Interoperability Resources (FHIR) resource types: Patient, Practitioner, Organization, Location, Device, RelatedPerson, and PractitionerRole. It matches high-confidence healthcare identifiers such as Social Security, medical record, and national provider numbers, applying each identifier's real-world scope and filtering out placeholder values to avoid false matches.


Once enabled, every record that is created or updated is automatically evaluated and matches are connected through FHIR Linkage resources. Resource matching requires no configuration, matching rules, or third-party tools. Original records are never modified or deleted, preserving full provenance. Linkages are automatically re-evaluated as source records change and removed when records no longer match.


AWS HealthLake resource matching is available in gated preview in the US East (N. Virginia), US East (Ohio), US West (Oregon), Asia Pacific (Mumbai), Europe (London), Europe (Ireland), Asia Pacific SouthEast (Sydney), and Canada (Central) Regions. To turn it on for your datastore during the preview period, request to be added to the allowlist[here](https://pulse.amazon/survey/0B005AOL?p=0) .


To learn more, see[Matching duplicate FHIR resources](https://docs.aws.amazon.com/healthlake/latest/devguide/resource-matching.html) documentation in the AWS HealthLake developer guide.
