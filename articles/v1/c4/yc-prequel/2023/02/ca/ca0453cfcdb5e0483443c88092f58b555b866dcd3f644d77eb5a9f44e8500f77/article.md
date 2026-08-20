---
schema_version: "1.0.0"
document_id: "ca0453cfcdb5e0483443c88092f58b555b866dcd3f644d77eb5a9f44e8500f77"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/changelog-2-10-2023-staging-environments-multiple-sources-multiple-products/"
published_at: "2023-02-10T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:29e671c26cab56f71067088a763e2f2a576a8a804d2dc54481ef4787cc75b42b"
---

# Changelog 2/10/2023: Staging environments, multiple sources, & multiple products

## New Destinations: Google Sheets, Azure Blob Storage, Cloudflare R2, SQL Server


The platform added several new destination options in this release. Google Sheets was introduced as the first spreadsheet destination, offering particular value for teams lacking data infrastructure or preferring spreadsheet-based analysis. Additional object storage and database destinations were also included.


## Production & staging environments


All accounts now include two default modes: production and staging — for seamless environment toggling without requiring separate account creation.


## Multiple sources


The platform now supports adding multiple sources to a single account. Users can configure additional sources and specify which source to pull from in their configuration files.


## Multiple products


Accounts can now subscribe to multiple products for individual destinations, enabling data destinations to receive information from different product combinations.


## Improvements and fixes


- Postgres sources received significant optimization, achieving approximately 25x speed improvement for large data transfers.
- Regular API improvements and bug fixes were implemented.
