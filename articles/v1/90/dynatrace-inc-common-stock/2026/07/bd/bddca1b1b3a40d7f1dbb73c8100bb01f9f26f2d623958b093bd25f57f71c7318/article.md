---
schema_version: "1.0.0"
document_id: "bddca1b1b3a40d7f1dbb73c8100bb01f9f26f2d623958b093bd25f57f71c7318"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/dynatrace-managed-release-notes-version-1-342/"
published_at: "2026-07-06T06:39:58+00:00"
first_seen_at: "2026-07-30T07:17:54.686242+00:00"
fetched_at: "2026-07-30T07:17:56.314455+00:00"
content_hash: "sha256:3dea6e7be4c128ae828f902577d6be74a02ad61dbe7ba3242e04e32383438c7e"
---

# Dynatrace Managed release notes version 1.342

# What's new in Dynatrace Managed 1.342


-


Release notes


-


5-min read


-
- Rollout start on Jul 06, 2026


This page showcases new features, changes, and bug fixes in Dynatrace Managed version 1.342. It contains:


- Feature updates : 3
- Breaking changes : 1
- Fixes and maintenance : 6


## Feature updates


Platform


### New availability metrics without maintenance windows


Two new metrics —` builtin:host.availabilityWithoutMaintenance` and` builtin:pgi.availabilityWithoutMaintenance` — let you calculate host and process group instance availability without counting maintenance window time. This gives you a cleaner picture of true operational availability, separate from planned downtime. The existing` host.availability.percent` and` pgi.availability.percent` metrics continue to reflect complete availability.


Platform


### Rerun metric query with many metric dimension tuples in Data Explorer


In Data Explorer, if the limit of 100,000 dimension tuples is reached for a query, you now have the option to rerun the query with this limit removed.


The rerun option allows fetching of data for all available metric dimensions.


Other query limits, such as the maximum number of fetched data points, still apply.


Platform | Dashboards


### Selectable text in Dashboard markdown tiles


Text in Dashboard markdown tiles can now be selected and copied — making it easier to reuse content from dashboards in other documents and tools.


## Breaking changes


Application Observability


### OneAgent end-of-life version connection enforcement


Starting with this release, Dynatrace rejects connections from OneAgent versions 1.141 and earlier


.


If you’re running OneAgent version 1.141 or earlier


, upgrade to a supported OneAgent version to avoid data loss and connectivity issues, and to benefit from enhanced security and features unavailable in earlier OneAgent versions.


For details, see[End-of-life announcements](https://docs.dynatrace.com/managed/whats-new/technology/end-of-life-announcements) .


## Fixes and maintenance


### Resolved issues in this release


- Resolved an issue where custom metric data became invisible in queries after approximately three hours. This occurred when a custom metric was deleted and then re-created with the same metric key. To resolve the issue for currently affected metrics, restart the server. However, older data can't be recovered. (MGD-12314)


- Fixed a bug on the Oracle Insights **View Statements** page. (MGD-11991)
- Fixed the Maintenance Window filter entity type lookup to avoid matching all entities for unknown entity types. (DI-28768)


- Fixed repeated` WARNING` logs from` DavisGenericEventBuilder` about` smartscape.rootcause_entity` not being part of the` davis.event` Semantic Dictionary model during problem update ingestion. (DI-28569)
- Fixed various charts which lead to crashes in the UI (DEM-28819)


- The extensions` /monitoring-configurations` endpoint now accepts only fully-formed semver version properties when creating a new monitoring configuration. (DAQ-24971)


## Operating systems support


### Future Dynatrace Managed operating systems support changes


##### The following operating systems will no longer be supported starting 01 November 2026


- Linux: Red Hat Enterprise Linux 9.4, 9.7


- x86-64


- [Vendor announcement ﻿](https://access.redhat.com/support/policy/updates/errata)


- Linux: Ubuntu 16.04


- x86-64


- [Vendor announcement ﻿](https://ubuntu.com/about/release-cycle)


##### The following operating systems will no longer be supported starting 01 January 2027


- Linux: Amazon Linux 2


- x86-64


- [Vendor announcement ﻿](https://aws.amazon.com/linux/)


### Past Dynatrace Managed operating systems support changes


##### The following operating systems are no longer supported since 01 December 2025


- Linux: Red Hat Enterprise Linux 8.8, 9.2, 9.5


- x86-64


- Linux: Oracle Linux 9.5


- x86-64


- [Vendor announcement ﻿](https://www.oracle.com/a/ocom/docs/elsp-lifetime-069338.pdf)


- Linux: Rocky Linux 9.5


- x86-64


- [Vendor announcement ﻿](https://endoflife.date/rocky-linux)


##### The following operating systems are no longer supported since 01 January 2026


- Linux: Debian 10


- x86-64


- [Vendor announcement ﻿](https://wiki.debian.org/DebianReleases)


##### The following operating systems are no longer supported since 01 June 2026


- Linux: Oracle Linux 9.6


- x86-64


- Linux: Rocky Linux 9.6


- x86-64


- [Vendor announcement ﻿](https://endoflife.date/rocky-linux)


##### The following operating systems are no longer supported since 01 July 2026


- Linux: SUSE Enterprise Linux 15.3


- x86-64


- [Vendor announcement ﻿](https://www.suse.com/lifecycle/)


## Dynatrace API


To learn about changes to the Dynatrace API in this release, see:


- [Dynatrace API changelog version 1.341](https://docs.dynatrace.com/managed/whats-new/dynatrace-api/sprint-341)
