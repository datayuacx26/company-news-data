---
schema_version: "1.0.0"
document_id: "5da478ee6b00a5e5935a8fca294443c25270691c887c0a8b1b42f0c3800a1645"
company_key: "yc-quilt-data"
company: "Quilt Data"
source_id: "yc-quilt-data-rss-730eaa07f229"
canonical_url: "https://blog.quilt.bio/tamper-evident-ngs-provenance-s3"
published_at: "2026-05-12T22:06:58+00:00"
first_seen_at: "2026-07-25T20:21:21.106413+00:00"
fetched_at: "2026-07-28T22:13:25.865061+00:00"
content_hash: "sha256:4ccfa849aa8e24cce3e46b5c1a629bd15d23d5714dbe16a196768a787a0bdd8c"
---

# How to Build Tamper-Evident NGS Provenance on S3

An NGS run produces hundreds of gigabytes that may be referenced in regulatory submissions or downstream ML training years after the data was produced. If any link in the chain breaks (a FASTQ rewritten without notice, a parameter undocumented, a pipeline version lost) the defensibility of every downstream result becomes uncertain. NGS data is a good stress test for provenance precisely because the files are large, the pipelines are long, and the time horizon is long.


This post is a hands-on walkthrough of the pattern we use with biotech teams to make NGS outputs on S3 tamper-evident: every run signed, every file hashed, every step inspectable. The implementation works whether the runtime is AWS HealthOmics, plain Batch, or a self-hosted Nextflow cluster, as long as the data lands in S3.


## The goal


For every NGS run, we want a single addressable artifact (a Quilt package) that contains, atomically:


- The raw and processed reads, or pointers to them.
- The sample manifest.
- The QC report and key QC metrics in structured form.
- The pipeline name, version, parameters, and container image digests.
- A human-readable README.
- Lineage edges to upstream packages (raw FASTQs, reference genome, and so on).
- A cryptographic hash that covers all of the above.


And we want that artifact stored such that nobody (including the engineer who registered it) can alter it after the fact, every access is logged and attributable, and an inspector can review it in a browser in human-readable form without engineering help.


## Step 1: Carve out the registry bucket


Create a dedicated S3 bucket for regulated NGS records, separate from the pipeline's scratch bucket. This is the system of record.


```text
RegulatedRegistryBucket:
Type: AWS::S3::Bucket
Properties:
BucketName: acme-quilt-ngs-registry
VersioningConfiguration: { Status: Enabled }
ObjectLockEnabled: true
ObjectLockConfiguration:
ObjectLockEnabled: Enabled
Rule:
DefaultRetention:
Mode: COMPLIANCE
Years: 7
BucketEncryption:
ServerSideEncryptionConfiguration:
- ServerSideEncryptionByDefault:
SSEAlgorithm: aws:kms
KMSMasterKeyID: alias/regulated-data-cmk
PublicAccessBlockConfiguration:
BlockPublicAcls: true
BlockPublicPolicy: true
IgnorePublicAcls: true
RestrictPublicBuckets: true
ReplicationConfiguration:
Role: !GetAtt ReplicationRole.Arn
Rules:
- Status: Enabled
Destination:
Bucket: arn:aws:s3:::acme-quilt-ngs-registry-dr
EncryptionConfiguration:
ReplicaKmsKeyID: alias/regulated-data-cmk-dr


```


Three properties enforced at the storage layer: versioning, Object Lock in compliance mode for seven years, and KMS encryption with a customer-managed key. The replication target inherits the same posture in the DR region. An SCP at the AWS Organizations level prevents any account from disabling Object Lock or deleting the bucket.


## Step 2: Define the NGS package schema


The schema is the contract for what an NGS package must contain. Pipelines that do not satisfy it cannot register.


```text
{
"$schema": "http://json-schema.org/draft-07/schema#",
"title": "ngs-package",
"type": "object",
"required": [
"project_id", "study_id", "subject_id", "sample_id",
"assay", "instrument", "instrument_run_id",
"pipeline_name", "pipeline_version", "pipeline_parameters_hash",
"container_image_digest", "qc_status", "release_state"
],
"properties": {
"project_id":              {"type": "string", "pattern": "^[A-Z]{2,6}-[0-9]{3,6}$"},
"study_id":                {"type": "string"},
"subject_id":              {"type": "string"},
"sample_id":               {"type": "string"},
"assay":                   {"enum": ["RNA-seq", "ATAC-seq", "WGS", "WES", "scRNA-seq"]},
"instrument":              {"type": "string"},
"instrument_run_id":       {"type": "string"},
"pipeline_name":           {"type": "string"},
"pipeline_version":        {"type": "string"},
"pipeline_parameters_hash":{"type": "string", "pattern": "^[a-f0-9]{64}$"},
"container_image_digest":  {"type": "string", "pattern": "^sha256:[a-f0-9]{64}$"},
"qc_status":               {"enum": ["pass", "warn", "fail"]},
"release_state":           {"enum": ["draft", "released", "retracted"]},
"upstream_packages":       {"type": "array", "items": {"type": "string"}}
}
}


```


Two design choices worth calling out. The schema requires a SHA-256 of the pipeline parameters, not the parameters themselves; the parameters JSON lives inside the package and the hash gives a fast equality check across runs. The schema also requires the container image digest rather than a tag, because tags move and digests do not. The combination of parameters hash and image digest makes "the same pipeline produced this" a verifiable claim.


## Step 3: Configure the Quilt workflow


The workflow wires the schema to required files and a release state machine.


```text
workflows:
ngs:
name: NGS output package
description: |
Tamper-evident, signature-ready NGS run record.
Used for all regulated outputs landing in
s3://acme-quilt-ngs-registry.
metadata_schema: ngs-package
required_files:
- "manifest.csv"
- "qc/multiqc_report.html"
- "qc/metrics.json"
- "params/pipeline-parameters.json"
- "README.md"
release_states:
- draft
- released
- retracted
transitions:
draft_to_released:
requires_role: bioinformatics-lead
requires_signature: true
signature_meaning_options:
- "approved for downstream use"
- "approved for submission"
released_to_retracted:
requires_role: bioinformatics-lead
requires_signature: true
signature_meaning_options:
- "retracted, do not use"


```


No package lands in` released` without the right files, the right metadata, the right role, and a typed signature meaning.


## Step 4: Register from the pipeline


The registration step runs at the end of every successful pipeline. The example below is Python on AWS Batch with Nextflow, but the pattern is identical on HealthOmics or any other runner.


```text
import json
import hashlib
import subprocess
import quilt3
from pathlib import Path


RUN_DIR = Path("/scratch/run_42")
REGISTRY = "s3://acme-quilt-ngs-registry"


def sha256_of(path: Path) -> str:
h = hashlib.sha256()
with path.open("rb") as f:
for chunk in iter(lambda: f.read(8192 * 128), b""):
h.update(chunk)
return h.hexdigest()


def container_digest() -> str:
return subprocess.check_output(
["aws", "ecr", "describe-images",
"--repository-name", "ngs-pipeline",
"--image-ids", "imageTag=v1.4.2",
"--query", "imageDetails[0].imageDigest", "--output", "text"],
text=True,
).strip()


params = json.loads((RUN_DIR / "params/pipeline-parameters.json").read_text())
params_hash = hashlib.sha256(
json.dumps(params, sort_keys=True, separators=(",", ":")).encode()
).hexdigest()


pkg = quilt3.Package()
pkg.set_dir("fastqs",  str(RUN_DIR / "fastqs"))
pkg.set_dir("aligned", str(RUN_DIR / "aligned"))
pkg.set_dir("variants",str(RUN_DIR / "variants"))
pkg.set("manifest.csv",                str(RUN_DIR / "manifest.csv"))
pkg.set("qc/multiqc_report.html",      str(RUN_DIR / "qc/multiqc_report.html"))
pkg.set("qc/metrics.json",             str(RUN_DIR / "qc/metrics.json"))
pkg.set("params/pipeline-parameters.json", str(RUN_DIR / "params/pipeline-parameters.json"))
pkg.set("README.md",                   str(RUN_DIR / "README.md"))


pkg.set_meta({
"project_id": "KRAS-001",
"study_id":   "S-2026-04",
"subject_id": "SUBJ-9921",
"sample_id":  "SAMP-44219",
"assay":      "RNA-seq",
"instrument": "NovaSeq-X-A",
"instrument_run_id": "20260410-NVX-A-0042",
"pipeline_name":            "nf-core/rnaseq",
"pipeline_version":         "3.14.0",
"pipeline_parameters_hash": params_hash,
"container_image_digest":   container_digest(),
"qc_status":     "pass",
"release_state": "draft",
"upstream_packages": [
"raw/kras-001-fastqs@9f8e7d6a5c4b3a29...",
"refs/grch38@b1c2d3e4f5a6b7c8d9..."
],
})


result = pkg.push(
"ngs/kras-001-rnaseq-SAMP-44219",
registry=REGISTRY,
workflow="ngs",
message="KRAS-001 RNA-seq, SAMP-44219, run 20260410-NVX-A-0042",
)


print(f"Registered: {result.top_hash}")


```


The push establishes several properties at once. Every file is hashed. The metadata is validated against the schema. Required files are checked. Upstream package hashes are recorded as lineage edges, by content rather than name. A new top-level package hash is generated covering all files and metadata. The objects land in the Object-Locked registry. A package event is written to the audit log.


## Step 5: Sign on release


Draft packages can be edited within the workflow rules. Once a package is released, it should be signed by the bioinformatics lead with an explicit meaning of signature.


```text
quilt3.api.transition_state(
name="ngs/kras-001-rnaseq-SAMP-44219",
top_hash="a1b2c3d4...",
registry=REGISTRY,
target_state="released",
signature_meaning="approved for submission",
)


```


The signature event records signer (from authenticated SSO identity), UTC timestamp, signature meaning string, and the top-level hash. The signature artifact is stored under Object Lock in a` signatures/` prefix. Once released, the package hash itself is no longer mutable.


## Step 6: Inspect a package


Building this carefully pays off at inspection time. From the Quilt Web Catalog, an auditor can open` ngs/kras-001-rnaseq-SAMP-44219` , see every revision in order with hashes, click revision 3 to see the README, the manifest (column-aware viewer), the QC report (embedded HTML), the FASTQ list, and the pipeline parameters; see the lineage panel with clickable upstream packages; see the events panel with registration, state transitions, and signatures; and click "Export inspection PDF" for a report ready to hand over.


From` quilt3` :


```text
p = quilt3.Package.browse(
"ngs/kras-001-rnaseq-SAMP-44219@a1b2c3d4...",
REGISTRY,
)
print(p.meta)               # full metadata including lineage
print(p.top_hash)           # the cryptographic claim
print(list(p.keys()))       # files in the package


```


## Step 7: Run a mock-inspection drill


Quarterly, hand a colleague a short scenario: find the released NGS package for project KRAS-001 and sample SAMP-44219; prove it has not changed since it was signed; show everyone who accessed it in the last ninety days; export the audit trail and README as PDF for an inspector; trace it back to its raw FASTQ package and confirm that upstream is also released. If a competent colleague can do all of this in under fifteen minutes without an engineering ticket, the system is working.


## What this pattern provides


When NGS provenance is built this way, several properties hold by construction. Reproducibility is mathematical. The pipeline parameters hash and the container image digest together make a rerun deterministic. Compliance is operational, not theatrical: workflow contracts enforce required artifacts and signatures at registration. Trust compounds. Every package downstream of a released, signed package inherits a defensible chain back to the raw inputs. And AI agents can consume the packages via MCP with the same guarantees, so the agent work does not undo the provenance work.


To walk through this on your current NGS workflow, the Quilt team is happy to schedule a working session:[quilt.bio/demo](https://www.quilt.bio/demo) .
