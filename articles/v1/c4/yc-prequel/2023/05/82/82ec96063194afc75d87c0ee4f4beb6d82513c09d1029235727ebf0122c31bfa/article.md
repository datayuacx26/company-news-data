---
schema_version: "1.0.0"
document_id: "82ec96063194afc75d87c0ee4f4beb6d82513c09d1029235727ebf0122c31bfa"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/changelog-5-9-2023-data-import-webhooks-schema-tenanting/"
published_at: "2023-05-09T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:f49ce84a73d8b2b664d896b1d235e851fd08a80b95b31e11a420e552ad95c1a9"
---

# Changelog 5/9/2023: Data import, webhooks, schema-tenanting

## New Product: Data Import


Prequel has expanded its platform to support importing data from external customer sources. For many SaaS products, this is a critical capability to effectively onboard and maintain connections to customer data. Most export features are now available for imports, with a largely symmetrical API structure.


## Webhooks, PagerDuty, & Slack Notifications


The platform now supports webhook monitoring for transfer failures. Users can subscribe to generic webhooks or push alerts directly to PagerDuty and Slack. Additional webhook event types are planned for future releases.


## Schema Tenanting


Previously, Prequel defaulted to table-level multi-tenancy using an` organization_id` column. This update adds support for schema-level tenanting, accommodating customers with different data architecture requirements while maintaining backwards compatibility.


## Source Access Control: Role-Based Access


Advanced deployments can now authenticate to data sources as separate users per tenant, supporting scenarios where security policies mandate role-based query access controls.


## Improvements and Fixes


Several enhancements shipped with this release:


- **Protected column validation** alerts users to potentially reserved column names in destination systems.
- **Custom source queries** enable advanced use cases with improved predicate pushdown and partition pruning.
- **Optimized column comments** prevent rate limiting on datasets with hundreds of columns.
- General API improvements and bug fixes.
