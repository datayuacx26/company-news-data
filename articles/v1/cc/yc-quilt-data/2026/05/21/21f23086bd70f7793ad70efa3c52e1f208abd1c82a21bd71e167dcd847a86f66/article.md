---
schema_version: "1.0.0"
document_id: "21f23086bd70f7793ad70efa3c52e1f208abd1c82a21bd71e167dcd847a86f66"
company_key: "yc-quilt-data"
company: "Quilt Data"
source_id: "yc-quilt-data-rss-730eaa07f229"
canonical_url: "https://blog.quilt.bio/cryptographic-signing-s3-data-provenance"
published_at: "2026-05-12T22:06:56+00:00"
first_seen_at: "2026-07-25T20:21:21.106413+00:00"
fetched_at: "2026-07-28T22:03:45.188546+00:00"
content_hash: "sha256:f3fd434f5f821c1929e1fd8a3697995bd3b3c5d510264c3c968817a2ab7dc5ab"
---

# How Cryptographic Signing Secures S3 Data Provenance

Scientists and security people use the word "provenance" to mean different things. Scientists ask where the data came from. Security people ask whether it has been altered. In a regulated environment, both meanings have to work together. A defensible result depends on being able to show, on demand, that the dataset under it has not changed since it was signed and that it traces cleanly back to its inputs.


This post is an explainer of three primitives that combine to give that property on Amazon S3: cryptographic hashing and signing, S3 object versioning with Object Lock, and immutable audit logs. Each one is useful on its own. The composition is where the defensibility comes from. The post ends with what a catalog layer (Quilt, in our case) adds on top.


## The three properties to prove


For any regulated dataset, three claims need to be supportable. First, integrity: the data has not been altered since it was registered. Second, attribution: it was produced by a specific party using a specific pipeline. Third, a temporal claim: it existed in this form on this date and in this state. A system that can support all three has provenance. A system that supports two of three does not.


## Hashing and signing


A cryptographic hash function (SHA-256 is the right default in 2026) maps any input to a fixed-length fingerprint. The hash is deterministic, so the same input produces the same hash, and collision-resistant, so different inputs almost certainly produce different hashes. Publishing the hash of a file today and recomputing it a year later is a near-perfect integrity check.


What hashing handles well is single-file integrity. What it does not handle is the fact that a regulated record is usually a bundle: a sample manifest, the raw reads, the QC report, the pipeline parameters, and a README, all of which have to be intact together for the record to be meaningful. The natural extension is top-level hashing: hash every file in the bundle, then hash the sorted list of those hashes (plus the metadata) to produce one fingerprint for the whole package. That single hash is what you cite when claiming "version 3 of the KRAS-001 NGS package." Change one byte in any file and both the leaf hash and the top-level hash change.


```text
# Conceptual model of a package hash
leaf_hashes = [sha256(f) for f in package_files]
metadata_hash = sha256(json.dumps(metadata, sort_keys=True).encode())
package_hash = sha256(b"".join(sorted(leaf_hashes) + [metadata_hash]))


```


Signing layers identity on top of hashing. The hash is encrypted with a private key controlled by a single party, and anyone with the corresponding public key can verify two things: that the signature came from the key holder, and that the hash still matches the data. For 21 CFR Part 11 eSignatures, what gets signed is the package top-hash plus a signature manifest containing the signer's identity, UTC timestamp, and meaning of signature (approval, review, authorship). The signed bundle becomes a durable claim of authorship over a specific record state.


## S3 object versioning and Object Lock


Hashing proves after the fact that data has not changed. Storage controls handle the harder problem of refusing to let it change in the first place.


S3 Versioning makes every` PutObject` create a new version. Deletes become "delete markers" that hide the latest version without destroying prior ones, and restoration is a single API call. Versioning solves the accidental-overwrite problem and the ransomware problem cleanly. It does not solve the privileged-deletion problem: an admin with sufficient privileges can still permanently delete versions or disable versioning. Versioning gives continuity, not tamper-evidence.


S3 Object Lock adds the tamper-evidence layer. Configured at the bucket level, it offers three modes. Compliance mode prevents anyone (including the root account) from deleting or altering an object before its retention period expires, and is appropriate for records with hard regulatory retention windows. Governance mode lets privileged accounts override the lock with a documented bypass, useful when an explicit admin escape hatch is needed. Legal hold is indefinite and removable only by a specific permission, intended for litigation or active investigations.


The pattern that holds up for regulated data: compliance-mode Object Lock on a dedicated registry bucket, KMS-encrypted, versioned, replicated to a second region with the same posture. Records land once and stay for the retention period. That storage commitment is what backs up the cryptographic hashes. Saying "the hash of` genomics/kras-001-rnaseq@a1b2c3` is X" only matters if the underlying bytes cannot be quietly retracted.


## Immutable audit logs


The third primitive is the temporal claim: proving when things happened and who did them. A log that can be modified by a sufficiently privileged user is not an audit log. It's a comment box. An audit log that lives in the same place as the data, under the same permissions, has the same problem.


The pattern that works on AWS:


- CloudTrail writes management and data events to a dedicated S3 bucket. Multi-region, log file validation enabled.
- The CloudTrail destination bucket has its own Object Lock in compliance mode, separate from the data bucket, with Lifecycle policies aligned to the retention window.
- For defense in depth, events also ship to a write-only sink (CloudWatch Logs with retention policies, or an external SIEM).
- A higher-level catalog (Quilt) writes package-level events into a separate Object-Locked event log.


Why two logs? CloudTrail records what happened at the AWS API layer ("an object was put"). The catalog log records what happened at the records layer ("version 3 of the KRAS package was released by Alice"). Both matter. Both should be tamper-evident. Both should survive privileged users.


## The composition: chain of custody on S3


None of the primitives is sufficient alone. The useful property is what emerges when they compose. A chain of custody for a single regulated dataset under this architecture:


1. A pipeline produces output. Files land in a staging prefix in S3.
2. The catalog registers a package. Every file is hashed. The package gets a top-level hash. Required metadata is validated. The workflow contract is enforced.
3. Storage is committed. Files are moved (or referenced) into an Object-Locked registry bucket, KMS-encrypted, with cross-region replication.
4. Audit events are written. CloudTrail records the underlying S3 operations. The catalog records the package event with user attribution and workflow context. Both go to Object-Locked sinks.
5. A signature is applied if the workflow requires one. The package hash and signature manifest are signed by the approver. The signature artifact is also stored under Object Lock.
6. An auditor opens the package in the catalog UI and sees the README, the metadata, the lineage to upstream packages, the registration event, the signer, the signature meaning, and the hash. They can verify the hash against the underlying objects and export the inspection view as PDF.


Each step makes a claim backed by a primitive that does not depend on the others to be trustworthy. If a primitive fails (an admin disables CloudTrail for a window, say), the other layers still hold, and the gap itself is evidence in the catalog's event log.


## Common mistakes that break the chain


Three patterns reliably undermine an otherwise sound posture. Logs stored in the same place as the data, under the same permissions, are not logs. S3 versioning treated as tamper-evidence creates a false sense of security; it protects against accidents and not against privileged deletion, which is what Object Lock exists for. And hashing files without hashing the package leaves dataset-level integrity uncertain; "version 3" remains a label rather than a verifiable claim.


## What a catalog adds


The primitives above are AWS-native. They can be composed by hand. The Quilt Data Platform exists to make that composition operational without rebuilding it for every workflow. The pieces it brings:


- Package-level hashing, so a dataset revision is verifiable, not just labeled.
- Workflow contracts that bind hashing, metadata validation, signing, and Object Lock into a single registration step.
- An inspection UI that shows hash, lineage, attribution, signing, and audit trail in one view.
- Cross-package lineage, so the chain extends backward to raw inputs and forward to derived results.
- MCP-accessible packages, so AI agents working over the data inherit the same provenance guarantees.


The right starting point for any provenance posture on S3 is the three primitives. The catalog comes after. To walk one regulated dataset through the full chain together, the Quilt team is happy to do a working session:[quilt.bio/demo](https://www.quilt.bio/demo) .
