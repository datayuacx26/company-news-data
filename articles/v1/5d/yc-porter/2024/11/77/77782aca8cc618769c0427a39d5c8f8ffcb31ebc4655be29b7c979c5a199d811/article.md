---
schema_version: "1.0.0"
document_id: "77782aca8cc618769c0427a39d5c8f8ffcb31ebc4655be29b7c979c5a199d811"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-d854f87d935e"
canonical_url: "https://www.porter.run/changelog/datastores-revamp-aurora-fast-clones-for-preview-environments-and-non-ha-aurora-clusters"
published_at: "2024-11-21T00:00:00+00:00"
first_seen_at: "2026-07-25T19:39:33.596384+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:85e09d26e88e0e433267a220e198ce32055fcb645ea27fc6ff72b6d0adfebe14"
---

# Datastores Revamp, Aurora Fast Clones for Preview Environments, and Non-HA Aurora Clusters

## **Datastores Revamp**


We've streamlined the dashboard - users will now find all datastores under the Add-ons tab.


When spinning up new RDS databases, users have way more flexibility: more options for resources, storage size, Postgres version, and the ability to choose database and user names.


## **Aurora Fast Clones for Preview Environments**


Porter-managed datastores now support AWS Aurora Fast Database Cloning, so users can quickly and cost-effectively create copy-on-write clones of their Aurora databases.


Rather than the slow process of restoring production backups to a fresh Postgres instance, users can now use AWS Aurora Fast Cloning to instantly spin up preview environments with a database that mirrors production data.


## **Non-HA Aurora Clusters**


Previously, all Porter-provisioned Aurora clusters included both reader and writer instances for high availability. Users can now choose single-instance Aurora clusters to reduce costs where high availability isn't critical.
