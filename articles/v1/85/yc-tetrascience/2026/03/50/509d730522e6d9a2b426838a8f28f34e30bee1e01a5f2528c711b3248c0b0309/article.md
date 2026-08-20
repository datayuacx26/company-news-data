---
schema_version: "1.0.0"
document_id: "509d730522e6d9a2b426838a8f28f34e30bee1e01a5f2528c711b3248c0b0309"
company_key: "yc-tetrascience"
company: "TetraScience"
source_id: "yc-tetrascience-news-import-63b926bd0a66"
canonical_url: "https://www.tetrascience.com/blog/metadata-only-ingestion-find-any-experiment-without-moving-a-single-file"
published_at: "2026-03-16T00:00:00+00:00"
first_seen_at: "2026-07-22T16:09:26.237182+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:305da25b396f57cc573bbd2ce5171a0295c0de244e215a3ccf3f991aa725715a"
---

# Metadata-Only Ingestion: Search Everything. Migrate Nothing. | TetraScience

If you support a scientific organization, you already know the ticket. A researcher needs data from an experiment run six months ago. They don't remember exactly where it was saved. It might be on a NAS, a shared drive, an S3 bucket, or a CRO hard drive that arrived two weeks late. The search begins. Time gets wasted. Sometimes the experiment just gets re-run.


The default answer to this problem is "move everything into a central repository." But for Scientific IT, that answer carries a cost most organizations aren't ready to pay: new storage infrastructure, migration projects, duplicate copies of petabyte-scale imaging, NGS, and mass spec datasets, and a long conversation about compliance and storage tiers.


There is a different approach. It doesn't require moving a single file.


### The concept: index the metadata, leave the files in place


Metadata-only ingestion — sometimes called index-only ingestion — works by pointing a Tetra File Log Agent or S3 connector at your existing storage locations. Instead of copying files into the Tetra Scientific Data and AI Cloud, the connector scans the folder structure, extracts metadata from file paths, names, and directory conventions, and creates a zero-byte index record in the platform.​


The original files stay exactly where they are — on the NAS, in S3, in local instrument storage, wherever your organization already keeps them and however your compliance requirements dictate.​


What changes is discoverability. Scientists can now search across all of those sources from a single interface, filtering by experiment ID, study ID, batch, lot number, or whatever metadata matters to their workflow. When they find what they need, the platform surfaces the exact file path so they can open it directly in their existing storage or analysis environment — no hunting through folder structures, no guessing at naming conventions from two years ago.​


### See it in action


In this live technical showcase, TetraScience product manager Nicole Rose and solution architect Brent Wilson walk through two real customer scenarios where metadata-only ingestion dramatically improved scientific data findability — including a CRO imaging workflow where physical hard drives took one to two weeks to arrive, and manual file contextualization was consuming hours of researcher time every week.


### What this means for IT


The operational relief for IT is just as concrete as the scientific benefit:​


- No new storage to provision. Files don't move, so you're not paying for duplicate petabyte-scale datasets.


- No migration project. There's no cutover, no sync jobs, no compliance review of a new storage tier.


- Existing compliant storage stays in place. Digital pathology teams, bioinformatics groups, and others who need data close to their analysis environment can keep it there.


- "Where is my data?" tickets go down. Scientists find their own data through search instead of filing requests.


Configuration is straightforward. Metadata-only monitoring is a single toggle in the File Log Agent setup, and bulk configuration across multiple paths is supported — so rolling it out across a large NAS environment doesn't require configuring each path by hand.​


### A note on scope


Metadata-only ingestion isn't the right approach for every data type — it's most valuable for large files like microscopy images, NGS data, and mass spec files that need to stay close to analysis pipelines or specialized storage. For most instrument data, full replatforming into Tetra delivers more downstream value. Think of index-only as a practical tool for the edge cases: the large files, the CRO data, the legacy archives that would be expensive or disruptive to move.​


If you'd like to see how this fits into a broader instrument data strategy — including how to scale onboarding across hundreds or thousands of instruments — we're running a follow-up showcase on April 16 focused on instrument onboarding at scale.[Reserve a spot now](https://www.tetrascience.com/lp/technical-showcase?utm_source=website&utm_medium=blog&utm_campaign=26-q1-showcase) .
