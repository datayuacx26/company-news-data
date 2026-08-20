---
schema_version: "1.0.0"
document_id: "b3b5fb551d1f807cff58f9001045b8b33aeda211f943d0da9addb8c0fa34c549"
company_key: "yc-quilt-data"
company: "Quilt Data"
source_id: "yc-quilt-data-rss-730eaa07f229"
canonical_url: "https://blog.quilt.bio/10-features-versioned-s3-data-catalog"
published_at: "2026-05-12T22:06:51+00:00"
first_seen_at: "2026-07-25T20:21:21.106413+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:ddbea9ac15636580b33ff3ec23bd21f9bc1e3e51670836a0ac09dbbe9e3642b9"
---

# 10 Features Teams Need in a Versioned S3 Data Catalog

S3 is an excellent object store and an inadequate catalog. Most teams discover this the same way: one bucket becomes a per-team bucket, then per-project prefixes, then a naming convention nobody follows, then a spreadsheet of where the good data lives, then the person who maintained the spreadsheet leaves the company.


By the time anyone uses the word "catalog," the problem isn't storage. It's findability, version control, and trust. The layer that fixes those problems is sometimes built in-house and sometimes purchased. Either way, the requirements are similar. The list below describes the ten capabilities we look for when evaluating any versioned S3 data catalog, written from the perspective of a biotech R&D team that has to support scientists, engineers, and QA from the same substrate.


## 1. Immutable, versioned packages


S3 versioning gives you per-object history. That's necessary but not sufficient. What teams actually need is collection-level versioning: the ability to point at "v3 of the KRAS RNA-seq dataset" and get back the exact bundle of FASTQs, sample manifest, QC report, and README that existed at that revision, even if individual files have since been rewritten or moved.


The right primitive is a single cryptographic hash per dataset revision. The hash addresses the whole bundle. If anything inside changes, the hash changes. The hash is the thing you cite in your validation document or your paper.


## 2. A schema for metadata


"You can attach metadata" appears on every catalog vendor's website. The question is whether the catalog enforces a schema, so that` tissue: liver` doesn't appear alongside` Tissue: Liver` and` tissue_type: hepatic` in the same search results.


What works in production:


- JSON Schema (or equivalent) describing required and optional metadata fields per workflow.
- Validation at registration time, with rejected packages rather than warnings after the fact.
- Versioned schemas, so the model can evolve without breaking historical data.


## 3. Faceted search across metadata, paths, and content


A scientist looking for "every RNA-seq dataset from the KRAS-001 project, on liver tissue, processed with nf-core/rnaseq 3.14 or later" should not be writing Athena queries. The catalog should index structured metadata, path components, and file content (README, manifest CSV, parquet column names), with sub-second response times across millions of objects. If search feels like Google, scientists will use it. If it feels like a niche internal tool, they will fall back to Slack.


## 4. Inline previews for scientific formats


Downloading a 12GB BAM file to see what's in it is how data reuse dies. A catalog that earns its keep includes:


- Embedded IGV, VCF tables, and FASTQ summary statistics for genomics.
- OME-TIFF, NIfTI, and DICOM rendering for imaging, with channel toggling.
- Column-aware viewers for parquet and CSV, with type detection and filtering.
- Rendered Markdown, PDF, and notebook previews.
- A way to embed custom HTML or JavaScript visualizations for data types a team has built tooling around.


## 5. README-style documentation versioned with the data


The underrated feature of a real catalog is the property that makes a good Python package usable: a README inside the artifact, in the same revision, accessible with the same permissions. When the data changes, the README changes in the same commit. Markdown is the right format because the same file is readable by a new team member, an external collaborator, and an LLM. The format already works for code; there is no reason it shouldn't work for data.


## 6. Code-first and no-code access to the same packages


Splitting scientists into "Python users" and "web users" with different tools is one of the fastest ways to fragment a data culture. Both audiences should reach the same packages, with the same metadata, through interfaces appropriate to them. In practice that means a Python client that can install a package the way pip installs a library, a web UI that exposes the same package with previews and search, and an API surface (including MCP) so AI agents inherit the same access patterns rather than getting a parallel one.


## 7. Lineage that travels with the package


"Where did this come from?" should be a one-click answer on every dataset. The information that needs to travel with the package includes the producing pipeline (and version, and parameters), the upstream packages by hash, the user or service account that registered it, and for derived data a clickable path back to raw inputs. Emitting OpenLineage events is useful for teams that have invested in broader data observability, but the lineage itself should live with the package, not in a parallel system.


## 8. Configurable workflows with required outputs


Most catalogs let teams store anything. Catalogs that hold up under regulatory scrutiny let teams refuse to store certain things. The pattern is a workflow contract: an NGS output package must contain a sample manifest, a QC report, and a pipeline version metadata field, with specific schema requirements satisfied, before it can be registered as` releasable` . This is how the dataset that ends up in front of an FDA reviewer still has the artifacts it needs six months later. Inari runs this pattern in production with workflows defined in a Python configuration deployed alongside their Quilt instance.


## 9. Audit trails an inspector can read


Three properties separate a usable audit trail from a CloudTrail dump:


1. Human-readable, so a non-engineer auditor can use it without a query tool.
2. Exportable as CSV, JSON, or PDF, scoped to a dataset, a user, or a date range.
3. Tamper-evident, backed by cryptographic hashes and ideally S3 Object Lock, so a claim about "the state of this record on day X" survives hostile review.


If the inspection story requires writing a sixty-line SQL query against CloudTrail logs, there isn't an audit trail yet. There are raw materials for one.


## 10. AWS-native architecture inside your account


The last item is architectural, and it determines whether the previous nine are even possible. A real S3 catalog runs inside your AWS account, under your IAM, KMS, and CloudTrail. It stores data in your S3 buckets, in standard formats. It works with the AWS services your stack already uses (Glue, Athena, SageMaker, Bedrock, HealthOmics). And it lets your security team enforce posture using the controls they already audit. A catalog that requires shipping your data into someone else's account to be searchable is not a catalog of your data.


## How Quilt covers the list


We built the[Quilt Data Platform](https://quilt.bio/) and the[Quilt Web Catalog](https://docs.quilt.bio/) against this list, so it would be odd to claim anything else. The honest map:


- Items 1, 2, 5, 6, and 7 are core to Quilt Packages: atomic, versioned, schema-enforced bundles with READMEs, accessed the same way from Python and the web.
- Items 3 and 4 are the Quilt Web Catalog: Elasticsearch-backed search and built-in previews including IGV.
- Items 8 and 9 are Quilt workflows together with cryptographic hashing on top of S3 Object Lock and CloudTrail.
- Item 10 is non-negotiable for us. Quilt runs entirely inside your AWS account, on your S3.


## Building it yourself


Some teams should build this internally; cataloging is a strategic differentiator in a few specific contexts. The teams we've talked to who tried it without that strategic reason describe a similar arc: a thin Python wrapper in year one, a web UI in year two, a request for lineage and audit trails in year three that requires rewriting the metadata model, and by year four a small product owned by three engineers who would prefer to be doing science. If cost is the primary reason to build, the math rarely holds. If the catalog is the differentiator, build it deliberately.


To walk specific datasets through the list together, the Quilt team is happy to set up a working session:[quilt.bio/demo](https://www.quilt.bio/demo) .
