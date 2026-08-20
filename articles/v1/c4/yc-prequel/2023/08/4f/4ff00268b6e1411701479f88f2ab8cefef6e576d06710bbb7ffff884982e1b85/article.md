---
schema_version: "1.0.0"
document_id: "4ff00268b6e1411701479f88f2ab8cefef6e576d06710bbb7ffff884982e1b85"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/changelog-8-28-2023-role-based-auth-and-new-advanced-features/"
published_at: "2023-08-28T00:00:00+00:00"
first_seen_at: "2026-07-24T10:10:51.593417+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:cdde91f5036666afa93ed08e41301a7dad26850e81c103cc7050d2a36ca36126"
---

# Changelog 8/28/2023: Role-based auth and new advanced features

## Role-based Authentication


Prequel now supports role-based authentication for AWS or GCP destinations, including Redshift, Athena, S3, BigQuery, and Google Cloud Storage. This secure method allows customers to grant access directly to a given AWS IAM Role or GCP Service Account rather than sharing credentials. The platform will continue supporting credential-based authentication, and existing connections can be upgraded safely.


## Frontend improvements


We’ve enhanced usability by adding the ability to sync and force refresh specific models on a per-destination basis, plus options to edit destination connection details directly in the interface.


## Magic link improvements


For teams using magic links, two optional fields were added to the generation flow:


- **Create as disabled** : Magic link submissions can create destinations as “disabled” for manual validation before backfill starts.
- **Redirect after submit** : Users can be directed to a specified URI after submitting.


## Advanced features


Several power-user features rolled out over the summer:


- **Backfill windows** : Configure maximum data batch sizes to process backfills in fixed chunks, minimizing resource bursts.
- **Max concurrent transfers** : A throttling mechanism limits simultaneous transfers (default: 10).
- **Broader source query support** : Custom query specification for reading source data, useful for partition pruning and predicate pushdown.


## Bug fixes


- Duplicate form submission prevention
- Enhanced validation for error-prone fields like` host` ,` region` , and` bucket_name`
- Improved SSH tunnel performance and connection stability
