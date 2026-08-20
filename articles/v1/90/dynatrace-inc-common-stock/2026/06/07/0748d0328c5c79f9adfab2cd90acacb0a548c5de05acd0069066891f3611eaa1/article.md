---
schema_version: "1.0.0"
document_id: "0748d0328c5c79f9adfab2cd90acacb0a548c5de05acd0069066891f3611eaa1"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/dynatrace-managed-release-notes-version-1-340/"
published_at: "2026-06-08T06:27:06+00:00"
first_seen_at: "2026-07-20T03:31:43.037247+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:5583a53bc05889d38fd446e3bbc1958612694c7ba67fcf3f03842b4799710806"
---

# Dynatrace Managed release notes version 1.340

# What's new in Dynatrace Managed 1.340


-


Release notes


-


4-min read


-
- Rollout start on Jun 08, 2026


This page showcases new features, changes, and bug fixes in Dynatrace Managed version 1.340. It contains:


- Feature updates : 4
- Breaking changes : 2
- Fixes and maintenance : 6


Infrastructure Observability


## Modernized Network interface page for quicker insights


We’re excited to announce a completely redesigned **Network interface** page, bringing broader visibility, smoother navigation, and a more intuitive monitoring experience to your network performance workflows.


Network Interface page


## Feature updates


Infrastructure Observability


### Modernized Disk page for quicker insights


We’re excited to announce a completely redesigned **Disk** page, bringing broader visibility, smoother navigation, and a more intuitive monitoring experience to your infrastructure performance workflows.


Platform


### Configurable log retention and storage per environment


Dynatrace Managed admins can now configure log retention and storage limits per environment directly in the CMC UI under the **Environments** setting. Log retention can be set anywhere from 1 to 90 days (in daily increments), and storage can be capped at a specific GiB value or left unlimited. Previously, log retention was fixed at 35 days with no option to adjust it. Retention changes apply to new log records going forward, with daily cleanup enforcing the configured period.


Platform


### Free-text support for non-entity filters in Data Explorer


The **Filter by** field in


**Data Explorer** now accepts free-text input for non-entity filters. As you type, the free-text option appears with immediate feedback.


Platform | Platform Services


### Elasticsearch upgrade to version 8.19


The Elasticsearch nodes are upgraded to version 8.19 to provide bug and security fixes and leverage performance improvements introduced by this update.
No manual user intervention or downtime is required; the upgrade occurs via rolling updates as part of normal version updates.


## Breaking changes


Application Observability


### Upcoming OneAgent end of life version enforcement


Starting with Dynatrace Managed version 1.342


, the cluster will reject connections from OneAgent versions 1.141 and earlier


.


Action might be required:


If you are running OneAgent versions 1.141 or earlier


, upgrade to a supported OneAgent version before upgrading to Dynatrace Managed version 1.342


to avoid data loss and connectivity issues, and to benefit from enhanced security and features unavailable in earlier OneAgent versions.


For more details, see[End-of-life announcements](https://docs.dynatrace.com/managed/whats-new/technology/end-of-life-announcements) .


Infrastructure Observability | Infrastructure & Operations


### Deprecation of ActiveGate auto-update API


The[ActiveGate auto-update configuration API](https://docs.dynatrace.com/managed/dynatrace-api/environment-api/activegates/auto-update-config) is deprecated. Instead, use the Settings 2.0 API with the` deployment.activegate.updates` schema ID.


## Fixes and maintenance


### Resolved issues in this release


- Fixed an issue where the **Deep monitoring** / **Process group monitoring** settings that deactivated monitoring were ignored for standalone/application-only and manually injected OneAgents. (PS-42728)
- Fixed an issue where a message was being displayed stating that Python 3.14 is not supported on Linux. Message was incorrect as actual monitoring of the process works starting with OneAgent version 1.329+


. (MGD-11388)
- Fixed an issue causing the RUM JavaScript to add headers to cross-origin requests if the` base` tag points to a different origin, which triggered potentially broken preflight requests. (DEM-26180)
- Fixed an issue that resulted in consecutive visit generation if cookies cannot be set. (DEM-25392)


- Fixed the issue where the infographic on the Technology & Processes custom device subpage occasionally failed to render due to missing data not being handled correctly. (DAQ-24380)


## Operating systems support


- Added support for[Ubuntu](https://docs.dynatrace.com/managed/managed-cluster/installation/operating-system-requirements) 26.04


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


- [Dynatrace API changelog version 1.340](https://docs.dynatrace.com/managed/whats-new/dynatrace-api/sprint-340)
