---
schema_version: "1.0.0"
document_id: "97b88217dfe686d7692a30f357d8615d07fa70e7326ce9ba2f6d2156f8cd455a"
company_key: "yc-quilt-data"
company: "Quilt Data"
source_id: "yc-quilt-data-rss-730eaa07f229"
canonical_url: "https://blog.quilt.bio/how-to-21-cfr-part-11-amazon-s3"
published_at: "2026-05-12T22:06:52+00:00"
first_seen_at: "2026-07-25T20:21:21.106413+00:00"
fetched_at: "2026-07-28T22:03:45.188546+00:00"
content_hash: "sha256:209d361868103cc55e6531cdb170895400035b272631f347b9ca20e1941efa3d"
---

# How to Make Amazon S3 Data 21 CFR Part 11 Ready

21 CFR Part 11 was written for paper-era systems and applied to every electronic record since. Discussions of it tend to swing between abstract theology and panicked vendor claims, neither of which helps when an inspection date is on the calendar. This post sits in between. It walks through what Part 11 requires for electronic records, what S3 provides out of the box, what gaps need to be closed at the storage layer, and what gaps require a higher-level system on top of S3.


One framing up front: no AWS service or third-party platform is "21 CFR Part 11 certified." Part 11 compliance is a property of how a system is configured, documented, and operated, not a sticker on a product. Vendors who claim otherwise are best treated as the second source you talk to.


## What Part 11 actually requires


The rule clusters into three areas. The summaries below are paraphrased for usefulness; your QA team will own the formal mapping.


**Closed systems (§ 11.10).** Validation of the system to ensure accuracy, reliability, and the ability to detect altered records. The capacity to generate accurate and complete copies of records in human-readable form. Protection of records through the retention period. Access limited to authorized individuals. Secure, computer-generated, time-stamped audit trails that record changes without obscuring prior information. Operational checks enforcing permitted sequencing of steps. Authority checks, device checks, training, and accountability.


**Open systems (§ 11.30).** Everything in § 11.10 plus additional controls (encryption, digital signatures) appropriate to keep records confidential and intact from creation through destruction.


**Electronic signatures (§§ 11.50, 11.70, 11.100–11.300).** Signatures unambiguously linked to their records. Signer's printed name, date and time, and meaning of the signing event captured with the record. Identity verification before issuing a signature. Periodic checks of signature controls. Unique signatures that are not reusable or reassignable.


The next question is how much of that is provided by AWS, how much is on you, and how much requires a layer on top of S3.


## What S3 provides as primitives


S3 and the surrounding AWS services give you a good foundation. None of the primitives is sufficient alone, but they are what everything else builds on.


AWS capability Maps to


S3 Versioning Records protection; prior-version retention


S3 Object Lock (governance or compliance mode) Tamper-evidence; preventing record alteration


KMS encryption at rest Open-system protection of records


TLS in transit, VPC endpoints Confidentiality during transmission


IAM, bucket policies, ACLs Access limited to authorized individuals


CloudTrail data and management events Secure, time-stamped audit trails


AWS Config Configuration drift detection


AWS Backup and Cross-Region Replication Retention and disaster recovery


AWS shoulders a lot of weight. The remaining work is making each "record" identifiable, attributable, and inspectable as a unit, because Part 11 cares about records rather than objects.


## A working checklist


Each step below is concrete enough to assign as a ticket.


### 1. Define what a "record" is


Before configuring any bucket, sit with QA and write a one-page definition of a Part 11–regulated record in your environment. Examples might include "an NGS run package: FASTQs, sample manifest, QC report, pipeline parameters, version-of-record metadata," or "a submission-supporting dataset: analysis outputs plus README plus source data references," or "an eSignature event: the signed dataset hash plus signer identity plus timestamp plus meaning of signature." Teams that can't write that page in a morning aren't blocked by S3 configuration; they're blocked by an undefined records model. That comes first.


### 2. Carve out a compliance bucket or prefix


Don't try to make all of S3 Part 11. Create a separate bucket, or a separate prefix with its own bucket policy, that holds only regulated records. Scratch data, intermediate pipeline outputs, and experiments belong elsewhere. Configure the bucket with:


- Versioning enabled.
- Object Lock with default retention. Compliance mode where retention is mandatory; governance mode if you need an admin escape hatch and can document why.
- SSE-KMS with a customer-managed key, with rotation enabled.
- Block all public access at the bucket and account level.
- Replication to a second region with the same Object Lock posture.


### 3. Lock down access


Two principles cover most of what matters. First, no human writes directly to the compliance bucket. Writes come from a small set of validated service accounts (the registration service, the eSignature service, the replication agent). Second, reads are scoped by role. Auditors read but cannot delete. Scientists read packages they are entitled to. Admins manage policies but cannot bypass Object Lock. Reinforce both with SCPs at the AWS Organizations level so no account, including the master, can disable Object Lock or delete the bucket.


### 4. Configure a tamper-evident audit trail


CloudTrail is necessary but not sufficient. To pass an inspection, the audit trail needs to be continuous (multi-region trail, log file validation enabled, deliveries to an Object-Locked bucket), attributable (every event traceable to a real human or a documented service principal, with AssumeRole chains reconstructible), inspectable (exportable to PDF or CSV in human-readable form, scoped to a single record or signer), and reliably time-stamped. CloudTrail covers the timestamp side. The other three are where a higher-level catalog earns its keep: CloudTrail tells you that an object was put. You need a layer that tells you "version 3 of the KRAS-001 NGS package was registered by Alice on 2026-04-12 at 14:22 UTC under the standard NGS workflow."


### 5. Make the records human-readable


§ 11.10(b) is the requirement most teams forget. You have to be able to produce records in human-readable form. That means every regulated package should include a README (Markdown or PDF), structured metadata using the fields from the records definition you wrote in step 1, and an inline preview in your catalog that an auditor can open without installing your CLI. If the only way to inspect a record is to ask the bioinformatics team to write a script, the system is not Part 11 ready.


### 6. Enforce workflow at the catalog layer


Part 11 expects operational checks on permitted sequencing of steps. In S3 terms, a record should not be registered until it satisfies a workflow contract. Common contracts include NGS packages that require a sample manifest, a QC report, and a pipeline version metadata field, or signed records that require a signer identity, an explicit meaning-of-signature string, and a hash of the record being signed, or submission datasets that require a project ID, a study ID, and an explicit submitted-to agency field. Quilt customers express these as JSON schemas inside their workflow configuration. Equivalent patterns exist in any mature data catalog. The point is that the check happens at registration, not after.


### 7. Map eSignature requirements to a real workflow


For Cluster C, you need a signing service that requires MFA at the moment of signing. The signature event itself should be written into the record: signer's printed name, UTC timestamp, meaning of signature (approval, review, authorship), and a hash of the signed record. Once signed, the record is protected by Object Lock so it cannot be altered. There also needs to be a revocation process for compromised accounts.


You can build this from primitives (IAM Identity Center with MFA, a Lambda that writes the signature event into a dedicated` signatures/` prefix under Object Lock) or use a higher-level platform that owns the workflow and the resulting signature artifact.


### 8. Produce validation documentation


Part 11 expects validation of the system. That means documents, not just configuration. The standard set:


- User Requirements Specification (URS): what the records system must do.
- Functional and Design Specifications (FS, DS): how it does it.
- Installation, Operational, and Performance Qualifications (IQ, OQ, PQ): evidence that it does what it says.
- Risk assessment: what can go wrong, what's mitigated, what's accepted.
- Standard Operating Procedures (SOPs): how operators actually use the system, including signing, registration, and deletion.


Teams tend to under-invest here. The documentation is the artifact an inspector reviews first. The companion post on[21 CFR Part 11 Validation Docs for S3 Records](https://blog.quilt.bio/blog/21-cfr-part-11-validation-docs-for-s3-records) goes through each document.


### 9. Run a mock inspection


The cheapest useful exercise this quarter: hand a colleague a list of questions and ask them to find the answers using only the live system. Some examples worth practicing: "Show me revision 3 of the KRAS-001 NGS package and prove it has not changed since it was signed." "Who signed it, when, and what did the signature mean?" "Show me everyone who accessed it in the last ninety days." "Give me a PDF of the README, the manifest, and the audit trail for this record." If a competent colleague can do all four in ten minutes, the system is operationally ready. If they can't, the missing answers are the gap list.


### 10. Document the living system


Part 11 is not a one-time event. Maintain a change log for meaningful configuration changes to the compliance bucket, the catalog, and the signing service. Re-validate when major upstream components change. Review the records definition, the workflow contracts, and the SOPs at least annually.


## Where Quilt fits


The Quilt Data Platform was built with regulated workflows in mind. On top of the S3 primitives above, it adds immutable hashed packages as the unit of record (so "version 3" is a verifiable claim, not a folder path), configurable workflows that enforce required files, metadata, and approvals before registration, human-readable inspection views with PDF and CSV export, audit trails that combine CloudTrail with package-level events into a single inspectable record, and an eSignature surface that binds signer identity, timestamp, and meaning to the package hash.


You can stitch the same posture together from primitives. Teams that prefer not to often find a working session useful. The Quilt team will walk through your records definition and produce the gap list against the checklist above:[quilt.bio/demo](https://www.quilt.bio/demo) .
