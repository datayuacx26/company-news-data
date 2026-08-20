---
schema_version: "1.0.0"
document_id: "bbf1d9968be54836fe24891773d5f5d700bd339e53f8c0ad74cf0bee4e18f8f3"
company_key: "yc-quilt-data"
company: "Quilt Data"
source_id: "yc-quilt-data-rss-730eaa07f229"
canonical_url: "https://blog.quilt.bio/searchable-versioned-s3-data-catalog-2026"
published_at: "2026-05-12T22:06:59+00:00"
first_seen_at: "2026-07-25T20:21:21.106413+00:00"
fetched_at: "2026-07-28T22:13:25.865061+00:00"
content_hash: "sha256:12d58bd3f6b32b3ad1da63796d89b322d8cdc37c051f87f52ef12dde924bcc1e"
---

# Building a Searchable, Versioned S3 Data Catalog in 2026

Most teams that run on S3 long enough end up building a data catalog one way or another. The good news for teams starting now is that the architectural patterns are settled. Earlier waves of catalog builders made the obvious mistakes (forklift to a separate database, embed everything in vectors and forget about humans, write a metadata model that collapses on contact with a real workflow), and the patterns that survived are well documented. There is no need to retrace those steps.


This is the playbook we run with biotech R&D teams. It assumes the case for a catalog is already made (the companion post on the[ten features teams need in a versioned S3 data catalog](https://blog.quilt.bio/blog/10-features-versioned-s3-data-catalog) covers the why). What follows is the how, step by step, with the design choices and trade-offs marked.


## The mental model


S3 is staying. Your buckets are staying. The catalog is a layer on top of S3 that introduces three things S3 alone does not have: datasets as units of meaning distinct from the objects underneath them, metadata as a first-class queryable property of every dataset, and versioning at the dataset level rather than only at the object level. Everything else (search, lineage, governance, previews) is a derivative of getting those three right.


## Step 1: Define the dataset


Sit with three groups (the scientists who produce data, the scientists who consume it, and the QA team that audits it) and agree on what a "dataset" means in your environment. Write it down. Real examples we have seen:


- NGS team: "a dataset is one sample's worth of pipeline output: FASTQs, alignments, variants, QC, manifest, README."
- Imaging team: "a dataset is one experiment plate's OME-TIFFs, a segmentation result, and a metadata sheet."
- Submissions team: "a dataset is the complete bundle of files and analyses supporting one IND module."


The rest of the catalog design becomes easier once each dataset has a clear definition, because each dataset gets a namespace, a schema, a workflow, and a set of consumers. Teams that cannot reach agreement on the dataset definition will inherit the disagreement in the catalog.


## Step 2: Choose the namespace pattern


Namespaces are how scientists refer to datasets in code, notebooks, and Slack. Good namespace design outlives reorgs. What works in practice: two-level namespaces of the form` <team-or-domain>/<dataset>` , with stable team identifiers (` genomics` ,` imaging` ,` submissions` , not` bobs-team` ) and human-readable, ASCII, sentence-case-avoided dataset names.


The pattern that breaks: namespaces that mirror the S3 bucket layout. One of the points of a catalog is to decouple the logical model from the storage layout, so when buckets are reorganized later, namespaces can stay stable.


## Step 3: Model metadata per workflow


A common temptation is to define a single global metadata schema for every dataset in the company. This rarely survives contact with reality, because required fields are genuinely workflow-specific. Schemas should live at the workflow level: one schema for NGS packages, another for imaging packages, another for submission packages.


Every workflow schema benefits from covering five categories: project and study identifiers that survive reorgs, process metadata (pipeline name, version, parameters hash, container digest), scientific metadata (what was measured, on what sample, with what assay), quality metadata (QC pass or fail and key metrics in structured form), and lifecycle metadata (release state, signing, retention class). Schemas are validated at registration, not after the fact. A dataset that does not satisfy the schema is not registered.


## Step 4: Choose the storage posture


The per-bucket configuration we recommend for a 2026 S3 catalog: versioning always on; KMS encryption with a customer-managed key with rotation enabled; Object Lock in compliance mode on any prefix holding regulated or signed records (governance mode where an explicit admin escape hatch is required); block public access at the bucket and account level; cross-region replication for any bucket holding records you'd hate to lose; and lifecycle rules to move infrequently accessed data to S3 Glacier Instant Retrieval while hot index data stays on Standard.


Consolidating everything into one mega-bucket is usually a regret. Per-team or per-domain buckets are easier to reason about, easier to apply Object Lock to selectively, and easier to deprecate when teams split.


## Step 5: Wire the producers


The catalog is only as good as what flows in. For every pipeline, instrument, or human workflow that produces datasets, decide how the data lands. Pipeline outputs register packages on completion via a registration step at the end of the workflow. Instrument outputs land in a staging prefix via S3 sync, with an EventBridge-triggered Lambda registering them into the catalog with the right schema. ELN- or LIMS-managed outputs use vendor integrations (Benchling, LabVantage) that emit events the catalog can consume. Ad-hoc human-produced datasets register via the web UI or a` quilt3` Python call, with workflow contracts enforcing required content. Every dataset gets in through a documented path. No back-door registrations.


## Step 6: Make search a first-class UX


Once datasets land with consistent metadata, search becomes the catalog feature scientists actually use. The properties that matter: a faceted UI where users pick from facets (project, assay, tissue, instrument, release state) and the result set updates immediately; full-text search across README content, manifest CSVs, and parquet column names; cross-namespace results, so a search returns relevant packages from any team that produced them; and sub-second latency across millions of objects, with Elasticsearch or OpenSearch doing the work under the hood.


Search that feels like an internal LDAP tool drives scientists to use Slack as the catalog instead. Search that feels like Google replaces Slack as the catalog.


## Step 7: Track lineage without a lineage project


Lineage does not need a separate initiative. It needs a metadata field. Every package declares its upstream packages by hash. The catalog renders the lineage graph from those declarations. Two important patterns: reference upstreams by hash rather than name (names move, hashes don't, and referencing by name silently breaks lineage when an upstream is rewritten), and emit OpenLineage events if you have a broader data observability stack. The catalog should play nicely with Marquez, DataHub, or whichever observability tool is already in use, rather than try to replace it.


## Step 8: Layer governance on the same substrate


At this point you have searchable, versioned, lineage-aware datasets landing through documented paths. Governance is a relatively thin layer on top: workflow contracts for required content per dataset type; role-based release transitions that require named roles and signatures for state changes; audit views exposing every event per dataset in human-readable form; and SCPs and bucket policies enforcing storage posture at the AWS layer.


Retrofitting governance onto a catalog after the fact is reliably expensive. Building it in from the first workflow, even one that does not need it yet, is reliably cheaper.


## Step 9: Build for collaboration


A catalog used only by the people who built it is a private filesystem. The collaboration features that matter: the same datasets reachable through web (for scientists), Python (for engineers), and MCP (for AI agents); citable revisions, so a scientist drops` genomics/kras-001-rnaseq@a1b2c3` into a notebook and a colleague can install exactly that revision; comments and annotations attached to specific revisions, either through the catalog or via linked tooling; and notification subscriptions, so a person who cares about a dataset gets told when it changes.


## Step 10: Plan for year three


The catalog that works in year one is rarely the catalog that works in year three. Choices that tend to age well: versioned schemas, so v1 does not have to cover every future field and can evolve without breaking historical packages; logical namespaces decoupled from physical paths, so bucket reorganizations don't break references; the catalog running in your AWS account, avoiding vendor lock-in over your most strategic data; an API surface and MCP server, because AI agents reading the catalog will be a load-bearing use case soon if they aren't yet; and a data-quality dashboard tracking the share of new packages that satisfy their schema cleanly and the share of workflows producing retractions, which catches structural problems early.


## How Quilt implements the playbook


The Quilt Data Platform is the implementation we work on, and this playbook reflects how it is designed. Datasets are Quilt Packages, atomic and versioned and hash-addressed. Metadata is JSON Schema enforced at registration. Storage stays on your S3, configured to the posture above. Search runs through the Quilt Web Catalog, Elasticsearch-backed, faceted, and full-text. Lineage is rendered from package metadata as a clickable graph. Workflows are the governance contract. The same packages are reachable via Python, web, and MCP for AI agents.


Whether to build or buy depends on whether cataloging is itself a strategic differentiator for the team. For most life sciences R&D groups, the math does not favor building this from scratch; engineering time is usually more valuable on the science. For teams where it is a differentiator, building deliberately is the right call.


To walk three real datasets through the playbook together, the Quilt team is glad to schedule a working session:[quilt.bio/demo](https://www.quilt.bio/demo) .
