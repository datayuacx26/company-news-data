---
schema_version: "1.0.0"
document_id: "8a130cc18115ee574a65f709ce591a33d02c1d4678532a9daa3c630456d68fe0"
company_key: "yc-quilt-data"
company: "Quilt Data"
source_id: "yc-quilt-data-rss-730eaa07f229"
canonical_url: "https://blog.quilt.bio/govern-versioned-datasets-on-s3-with-quilt"
published_at: "2026-05-12T22:06:53+00:00"
first_seen_at: "2026-07-25T20:21:21.106413+00:00"
fetched_at: "2026-07-28T22:03:45.188546+00:00"
content_hash: "sha256:7b3f9af32926c57cfe94fd8fdc6f897f90490f95d9bc9465a6ab1ec15fb65f44"
---

# How to Govern Versioned Datasets on S3 With Quilt

S3 versioning protects against accidents and accidental overwrites. It does not give datasets the schemas, lineage, or workflow checks that real governance requires, and it does not tell you which version of a dataset went into a regulated submission six months ago. This post is a working playbook for the layer that sits on top: schemas, packages, workflows, access controls, and lineage that hold up under audit.


The examples use the Quilt Data Platform, but the architecture is portable. Teams who are evaluating other tooling can adapt the pattern; the value comes from the discipline, not the brand.


## The mental model: a dataset is a package


The most important shift in moving from file-level governance to dataset-level governance is treating a dataset as one atomic, versioned, hash-addressed unit. In Quilt's terms, that unit is a Package. A package contains data files (FASTQs, parquet, CSVs, images, whatever lives in S3), metadata (sample manifests, assay parameters, instrument IDs) that has been schema-validated, documentation (README, methods notes, plots) versioned alongside the data, and lineage (upstream packages by hash, pipeline version, parameters) embedded in metadata.


The whole bundle is addressed by a single top-level hash. If anything changes, the hash changes. That is what turns "version 3 of the dataset" from a fragile naming convention into a verifiable claim.


## Step 1: Define the logical layout


Before installing anything, agree with the teams that produce and consume data on a logical namespace. Quilt namespaces look like Docker tags:` <team>/<dataset>` . Common examples in production:


```text
genomics/kras-001-rnaseq      # team genomics, KRAS-001 RNA-seq study
imaging/microscopy-cell-line  # team imaging, cell line microscopy
submissions/2026-ind-aurora   # team submissions, the Aurora IND


```


Two rules of thumb. Namespaces do not have to mirror your S3 bucket structure; one purpose of the catalog is to decouple the logical model from physical paths. And the names need to read sensibly in a citation, because` genomics/kras-001-rnaseq@a1b2c3` will appear in notebooks and Slack messages for years.


## Step 2: Define a schema per workflow


For every workflow that produces datasets, define a JSON Schema for the required metadata. Quilt enforces the schema at registration, so packages that don't satisfy it are rejected before anything lands in the registry.


```text
{
"$schema": "http://json-schema.org/draft-07/schema#",
"title": "ngs-package",
"type": "object",
"required": ["project_id", "study_id", "assay", "instrument", "pipeline_version"],
"properties": {
"project_id":       {"type": "string", "pattern": "^[A-Z]{2,6}-[0-9]{3,6}$"},
"study_id":         {"type": "string"},
"assay":            {"enum": ["RNA-seq", "ATAC-seq", "WGS", "WES"]},
"instrument":       {"type": "string"},
"pipeline_version": {"type": "string"},
"tissue":           {"type": "string"},
"release_state":    {"enum": ["draft", "released", "retracted"]}
}
}


```


Two side benefits fall out of this. Metadata stays consistent, so` tissue: liver` doesn't end up next to` Tissue: Liver` and` tissue_type: hepatic` across searches. And the schema fields are exactly what the catalog uses as search facets, so search is good by construction.


## Step 3: Configure a Quilt workflow


A Quilt Workflow is the contract that combines a schema with required files and a release state machine. It is configured once per team and applied automatically at every registration.


```text
workflows:
ngs:
name: NGS output package
description: Required structure for any NGS pipeline result
metadata_schema: ngs-package
required_files:
- "manifest.csv"
- "qc/multiqc_report.html"
- "README.md"
release_states:
- draft
- released
- retracted
transitions:
draft_to_released:
requires_role: bioinformatics-lead
requires_signature: true


```


The transition block is where the governance lives. Moving from` draft` to` released` requires a specific role and a signature. The release state is part of the package metadata, so downstream systems can filter on it and the audit trail captures the transition.


## Step 4: Register packages from pipelines


Every pipeline that produces datasets registers a package on completion. The pattern is the same on Nextflow, Airflow, AWS Batch, and HealthOmics.


```text
import quilt3


pkg = quilt3.Package()
pkg.set("manifest.csv", "/scratch/run_42/manifest.csv")
pkg.set_dir("fastqs", "/scratch/run_42/fastqs/")
pkg.set("qc/multiqc_report.html", "/scratch/run_42/qc/multiqc_report.html")
pkg.set("README.md", "/scratch/run_42/README.md")


pkg.set_meta({
"project_id": "KRAS-001",
"study_id": "S-2026-04",
"assay": "RNA-seq",
"instrument": "NovaSeq-X-A",
"pipeline_version": "nf-core/rnaseq@3.14.0",
"tissue": "liver",
"release_state": "draft",
"upstream": ["raw/kras-001-fastqs@9f8e7d6"],
})


pkg.push(
"genomics/kras-001-rnaseq",
registry="s3://acme-quilt-registry",
workflow="ngs",
message="KRAS-001 RNA-seq output, run 42",
)


```


Several governance properties are established by that single push. Metadata was validated against the schema (failure would have rejected the push). Required files were checked (missing files would have rejected the push). The upstream package was recorded as a lineage edge, by hash, so any downstream rerun can resolve the exact upstream. The package landed in S3 under the registry bucket, addressable by a new top-level hash. And the push generated an audit event tying the user or service to the registration.


## Step 5: Wire access controls


Access control on S3 is powerful and easy to misconfigure. The Quilt model layers a few patterns on top of the underlying primitives. At the bucket level, the registry bucket should be private, versioned, KMS-encrypted, with Object Lock where appropriate. At the catalog level, role-based permissions decide who can browse, push, or change release state per namespace, tied to SSO via OIDC or SAML. Specific workflow transitions can require named roles, not just authenticated users. And service accounts, not humans, push to production namespaces under tightly scoped IAM. The combined posture: humans read across many namespaces, pipelines write under deterministic roles, and state transitions require named, signed approvals.


## Step 6: Surface lineage and search


Once packages start landing with consistent metadata and lineage edges, the Quilt Web Catalog gives you faceted search across the schema fields with sub-second response time, package previews (IGV for genomics, column-aware viewers for tabular data, rendered Markdown for READMEs, embedded HTML for custom visualizations), lineage graphs showing upstream and downstream packages by hash with each node clickable, and cross-package queries via Athena over the metadata index when you need real SQL.


## Step 7: Make the audit trail inspectable


At this point you have two layered audit trails. S3 plus CloudTrail records every object operation, every IAM change, every key-policy update. Quilt package events record every registration, every metadata change, and every release-state transition, with user attribution. The two are complementary. The Quilt Web Catalog surfaces both, including exportable PDF inspection views, so a question like "show me everything that happened to` genomics/kras-001-rnaseq` in the last ninety days" takes about four seconds to answer.


## Step 8: Govern AI-agent access


This is the newer governance dimension, and the one most frameworks have not caught up to. Once bioinformatics teams start asking AI agents to summarize, compare, or annotate packages, there is a side door into the data layer unless agent access is governed the same way human access is. The pattern we recommend: agents reach data through the Quilt MCP server, which inherits the same role-based permissions as humans. Any agent action that writes back into the catalog goes through a workflow, just like a human registration. The agent identity, the human operator, and the parent workflow all appear in the audit trail.[Why AI Agents Fail Without a Persistent Context Layer](https://blog.quilt.bio/blog/ai-agents-context-layer-versioned-packages) covers the broader reasoning.


## Step 9: Practice a failure drill


Once a quarter, exercise the system with a scenario you'll be glad you rehearsed: "Restore` genomics/kras-001-rnaseq` as of ninety days ago and confirm the hash matches the version cited in the IND." Or: "Show me every package downstream of` raw/kras-001-fastqs@9f8e7d6` and confirm none of them have been retracted." Or: "An auditor wants the audit trail of every package created by the genomics team in March. Export it as PDF." If any of those takes more than thirty minutes, the gap is the thing to fix before a real version of the question shows up.


## Starting points


Whole-platform migrations rarely succeed. The pattern that does work: pick one high-value workflow (NGS outputs, assay registration, submission datasets), define its schema and workflow contract, wire the producing pipelines to push packages, onboard three to five power users, and run a mock audit. The gap list from that exercise becomes the roadmap for the next workflow. By the second or third workflow, the template is reusable.


To do this exercise with three of your own datasets, the Quilt team is glad to set up a working session:[quilt.bio/demo](https://www.quilt.bio/demo) .
