---
schema_version: "1.0.0"
document_id: "e8b510018cd790e0785fb661e9311af131e7ae35848b759f03d8cfe814c0a458"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/45827-deprecation-notice-support-for-postgres-14-ending-on-1st-july-2026"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:b2374625f08e01c0fa14a0e98d9181aa882dd34ddaf1c3c30e508971f9b13057"
---

# Deprecation Notice: Support for Postgres 14 ending on 1st July 2026

Supabase support for Postgres 14 is deprecated as of **1st July 2026** and support for it will be fully removed from this date on.


All projects still on a deprecated Postgres version on the **1st July 2026** will automatically be upgraded to the latest Postgres version available. If extensions that are no longer supported are being used, the projects will be paused instead and no longer serve traffic.


## Why upgrade?#


Postgres 17 brings significant improvements to performance, security, and reliability. Depending on your current version, upgrading gives you access to between 2–3 years of cumulative fixes and enhancements—over 700 individual improvements if you're coming from Postgres 14.


### Key benefits include:#


- **Performance** : Faster vacuuming, improved query planning, better parallel query execution, and optimised memory handling
- **Security** : Multiple CVE fixes and enhanced authentication options including configurable SCRAM iteration counts
- **New capabilities** : The MERGE command, logical replication on standbys, incremental backup support, and improved partitioning performance
- **Better observability** : New pg_stat_io view and enhanced monitoring across the board


## Before you upgrade#


### 1. Plan for downtime#


The upgrade process requires taking your project offline temporarily. Smaller databases typically complete in around 15 minutes, while larger databases can take an hour or more depending on your compute size and disk configuration (an estimate of the downtime is provided when you begin the upgrade in the dashboard). We recommend scheduling your upgrade during a low-traffic period. If you're unsure what to expect for your project, please reach out to our Support team for guidance.


### 2. Test your applications#


Using a new Postgres 17 project, confirm that your existing applications work and that there are no compatibility issues or breaking changes that impact you.


### 3. Check for deprecated extensions#


If your project uses any of the following extensions, you'll need to remove them before upgrading to Postgres 17:


- ` timescaledb`
- ` plv8` ,` pls` ,` plcoffee`
- ` pgjwt`


These extensions are no longer included in Postgres 17 on Supabase (see[changelog entry](https://github.com/orgs/supabase/discussions/35851) ) You can check which extensions are currently installed by running` SELECT * FROM pg_extension;` or by visiting your[Extensions dashboard](https://supabase.com/dashboard/project/_/database/extensions) .


For guidance on alternatives and migration paths, please see our[upgrade documentation](https://supabase.com/docs/guides/platform/upgrading) .


### 4. Prepare your project#


You can reduce the time it takes for your project to upgrade by performing a[vacuum](https://supabase.com/docs/guides/platform/database-size) on larger tables,[reindexing](https://supabase.com/docs/guides/database/postgres/indexes) larger indexes and removing/archiving any unused data in the database.


## How to upgrade#


Those organization owners with active projects on Postgres 14 will receive a series of emails, starting from this week, listing the projects they own that need upgrading and outlining the steps required.


You can upgrade directly from your dashboard:


1. Go to Project Settings → Infrastructure
2. Click Upgrade project
3. Follow the prompts to complete the upgrade


Direct link:[https://supabase.com/dashboard/project/_/settings/infrastructure](https://supabase.com/dashboard/project/_/settings/infrastructure)


## Questions?#


If you're unsure whether your project is ready to upgrade, or if you have questions about the deprecated extensions,[our Support team](https://supabase.help/) is here to help.


Thanks for building with Supabase.


— The Supabase Team
