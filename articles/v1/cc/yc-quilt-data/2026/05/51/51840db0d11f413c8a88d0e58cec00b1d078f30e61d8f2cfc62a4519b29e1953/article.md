---
schema_version: "1.0.0"
document_id: "51840db0d11f413c8a88d0e58cec00b1d078f30e61d8f2cfc62a4519b29e1953"
company_key: "yc-quilt-data"
company: "Quilt Data"
source_id: "yc-quilt-data-rss-730eaa07f229"
canonical_url: "https://blog.quilt.bio/21-cfr-part-11-validation-docs-s3-records"
published_at: "2026-05-12T22:06:57+00:00"
first_seen_at: "2026-07-25T20:21:21.106413+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:f160dd9d502c37b1a42f75236098997a90ed87623168f7d628bdf181c749f479"
---

# 21 CFR Part 11 Validation Docs for S3 Records

The technical posture for 21 CFR Part 11 on Amazon S3 is reasonably well understood. The documentation is where most pharma data teams slow down. The engineering side moves quickly (bucket policies, Object Lock, KMS, IAM), then a quarter goes by while engineering and QA learn each other's vocabulary and converge on the documents an inspector expects to see.


This post is a document-by-document walkthrough of the validation artifacts an S3-backed regulated records system needs, written so that engineering and QA can use it to align on the same set of deliverables before drafting begins. The framing assumes the GAMP 5 risk-based approach, which is the de facto industry standard for computerized system validation. Teams that use a different framework will see the document names shift, but the substance is the same.


## The documentation stack


```text
Validation Plan                                    strategy


User Requirements Specification (URS)              what
Functional / Design Specifications (FS / DS)       how
Risk Assessment                                    risk


Installation Qualification (IQ)                    installed correctly
Operational Qualification (OQ)                     functions correctly
Performance Qualification (PQ)                     fit for intended use


SOPs                                               procedures
Training records                                   people
Validation Summary Report (VSR)                    capstone


```


Risk-based validation lets the depth scale to the risk, so not every document needs to be exhaustive. Every Part 11 effort, however, needs some form of each layer, and the engineering work has to align with the artifacts.


## Validation Plan


**Owner:** QA, with input from regulatory.
**Purpose:** the strategy document. Names the system in scope, the team, the framework, and the deliverables.


For an S3-backed records system, the plan needs to be specific about scope. A useful template: "The system in scope is the records system for \[project / program\], comprising a Quilt Data Platform deployment in AWS account X, registry buckets` ...-us-east-1` and` ...-us-west-2` , the Quilt Web Catalog, the` quilt3` Python client, IAM roles X/Y/Z, KMS CMK X, and CloudTrail trail Y. Regulated records are defined as packages with` release_state ∈ {released, retracted}` registered into the` regulated/` namespace. Out of scope: development buckets, ad-hoc scientist scratch data, intermediate pipeline outputs not registered as released."


The plan should also identify the standards being claimed (21 CFR Part 11, EU Annex 11, GAMP 5) and the GAMP category the system falls under, typically Category 4 or 5 for configured or customized systems.


## User Requirements Specification (URS)


**Owner:** the business or scientific user group.
**Purpose:** what the system needs to do, expressed from the user's perspective.


For S3-backed records, the URS captures functional, non-functional, and compliance requirements in plain language. A representative sample:


ID Requirement Type


URS-001 The system shall store NGS pipeline outputs as immutable, versioned datasets. Functional


URS-002 Users shall be able to retrieve any prior version of a regulated dataset within five minutes. Functional


URS-003 The system shall generate a tamper-evident audit trail for every registration, modification, and deletion attempt. Compliance


URS-004 Access to regulated records shall be limited to users authenticated via the corporate SSO with appropriate roles. Compliance


URS-005 Electronic signatures shall include signer identity, UTC timestamp, and explicit meaning of signature. Compliance


URS-006 The system shall enable export of audit-trail data to PDF for inspection scopes specified by record, user, or date range. Functional


Aiming for 20 to 50 numbered requirements covering registration, search and retrieval, versioning, access control, audit, signing, backup and recovery, and reporting works well. Every later document traces back to these IDs.


## Functional Specification and Design Specification (FS, DS)


**Owner:** engineering or vendor.
**Purpose:** how the system meets each user requirement.


FS describes what the system does, function by function. DS describes how it does it, in technical detail. For smaller systems the two are often combined. For each URS requirement, the FS or DS should describe the function or feature that satisfies it, the components involved (Quilt Web Catalog UI,` quilt3` client, registration Lambda, S3 registry bucket with Object Lock, and so on), the configuration parameters and policies that enforce it, and any assumptions or dependencies.


A sample entry for URS-003:


```text
FS-003: Tamper-evident audit trail


URS mapping:   URS-003
Implementation:
- AWS CloudTrail (multi-region, log file validation enabled) writes
management and S3 data events to s3://acme-cloudtrail-audit-...
- The Quilt Data Platform writes package-level audit events to a
dedicated event log stored under Object Lock.
- Both audit destinations have S3 Object Lock (compliance mode),
retention 7 years (per records policy).
- Audit data is exportable via the Quilt Web Catalog
(PDF / CSV) and via direct CloudTrail queries by authorized
users only.
Configuration parameters:
- CloudTrail: log-file-validation-enabled = true
- Audit S3 bucket: object-lock-mode = COMPLIANCE,
retention = 7y, kms-key = alias/audit-cmk
Dependencies:  IAM role 'audit-reader' for export access.


```


## Risk Assessment


**Owner:** QA, with engineering input.
**Purpose:** identify what can go wrong, how likely it is, how bad it would be, and what mitigates it.


GAMP 5 risk assessment usually follows a pattern of scenario, severity, likelihood, detectability, priority, and mitigation. Representative scenarios for an S3-backed records system:


- An authorized user accidentally registers a package with incomplete metadata. Mitigation: the workflow contract enforces metadata validation at registration.
- A privileged admin attempts to disable Object Lock. Mitigation: SCP at the AWS Organizations level forbids the operation, with a CloudTrail alarm on attempts.
- An eSignature is bypassed by direct S3 access. Mitigation: scoped IAM forbids direct writes to the regulated namespace; the signing service is the only authorized writer.


The output of the risk assessment is the testing focus. High-priority risks get exhaustive coverage in IQ, OQ, and PQ. Lower-priority risks get sampled.


## Installation Qualification (IQ)


**Owner:** engineering, witnessed by QA.
**Purpose:** documented evidence that the system was installed as designed.


For an S3-backed records system, the IQ artifacts include the infrastructure-as-code (CloudFormation or Terraform) that defines buckets, KMS keys, IAM roles, CloudTrail, and the Quilt deployment, a signed installation log showing the version of every component (Quilt version, Lambda code hashes, container image digests), evidence that S3 Versioning, Object Lock, KMS encryption, and CloudTrail are actually enabled (typically via AWS Config rule evaluations or an IQ script that queries the AWS API and renders a PDF report), and a network architecture diagram covering VPC, subnets, security groups, and VPC endpoints.


The deliverable is a packet that says: "This system, configured to this baseline, was installed on this date, by this person, with these checks confirming the result."


## Operational Qualification (OQ)


**Owner:** engineering, witnessed by QA.
**Purpose:** evidence that the system functions according to its specifications.


OQ test cases derive from the FS. Every FS function gets at least one test. A representative set:


- OQ-001: Register a valid package via` quilt3` . Confirm it lands under the expected hash and metadata is queryable.
- OQ-002: Attempt to register a package missing a required file. Confirm the registration is rejected.
- OQ-003: Attempt to overwrite an existing Object-Locked object. Confirm the API rejects.
- OQ-004: Trigger an eSignature event. Confirm the signature record includes signer, UTC timestamp, meaning, and package hash.
- OQ-005: Run the audit-trail export. Confirm the PDF contains the expected events and signatures.
- OQ-006: Simulate an admin attempt to disable Object Lock. Confirm the SCP blocks it and the alarm fires.


Each OQ test records expected result, actual result, evidence (screenshots, log excerpts), pass or fail, and signatures. This is usually the largest single component of the validation effort.


## Performance Qualification (PQ)


**Owner:** business or scientific user group, with engineering support.
**Purpose:** evidence that the system performs in its intended environment with intended users and data.


PQ tests are end-to-end. A few useful scenarios for an S3-backed records system:


- PQ-001: A bioinformatics scientist registers a complete NGS package end-to-end via the standard pipeline. Confirms findability, integrity, and audit trail.
- PQ-002: A reviewer signs the package. Confirms signature presence and binding to the package hash.
- PQ-003: A QA reviewer exports the full audit trail and validates it against the OQ test scenarios.
- PQ-004: A disaster-recovery test. Simulate a region failure and confirm the secondary region preserves records under the same Object Lock posture.


PQ closes the loop. The system is not only installed and functioning. It is fit for purpose in your environment, with your users.


## SOPs and training


**Owner:** QA and department leads.
**Purpose:** procedures for human operators.


Part 11 is also about how humans use the system. SOPs that we recommend for an S3-backed records system: a procedure for registering a regulated package; a procedure for signing a package; a procedure for releasing or retracting a package; a procedure for handling an audit or inspection request; a procedure for system change management (deploying a new Quilt version, modifying a workflow); a procedure for incident response (suspected tamper, lost credentials, disclosed signature). Each SOP needs role-specific training and signed training records. The audit trail of who is allowed to do what needs to be as defensible as the audit trail of what they did.


## Validation Summary Report (VSR)


**Owner:** QA.
**Purpose:** the capstone document. Summarizes the validation effort, lists deviations, declares the system fit for production.


The VSR is usually the first document an inspector opens. It should reference the Validation Plan and confirm every planned deliverable is complete, list every IQ, OQ, and PQ test with results and any deviation reports, declare the system validated for the scope defined in the plan, and be signed by QA and the system owner.


## How Quilt reduces the documentation burden


No technology eliminates Part 11 documentation. Quilt makes several of the above artifacts meaningfully easier to produce: pre-built templates for URS, FS, DS, IQ, OQ, and PQ aligned to a Quilt deployment; infrastructure-as-code scaffolding for the S3 plus KMS plus CloudTrail posture the IQ has to verify; workflow contracts that double as functional specifications, version-controlled in the Quilt configuration repo; audit views in the Web Catalog that produce the inspection-ready evidence OQ and PQ depend on; and direct working sessions with the Quilt team to map the templates onto your records definition and risk profile.


To walk through the templates with your QA team, the Quilt team is glad to schedule a session:[quilt.bio/demo](https://www.quilt.bio/demo) .
