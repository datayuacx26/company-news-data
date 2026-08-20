---
schema_version: "1.0.0"
document_id: "45d3086994ff64fc5b142ecf901283893ae3ef38ff7316cdb262d8107bb2838e"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/oneagent-release-notes-version-1-343/"
published_at: "2026-07-28T10:39:18+00:00"
first_seen_at: "2026-07-29T12:28:02.276544+00:00"
fetched_at: "2026-07-29T12:28:03.729033+00:00"
content_hash: "sha256:52083bfab45992e42687956c3f03f063b36d8ea0f4162f041c324310529f42a0"
---

# OneAgent release notes version 1.343

# What's new in Dynatrace OneAgent 1.343


-


Release notes


-


7-min read


-
- Rollout start on Jul 28, 2026


This page showcases new features, changes, and bug fixes in Dynatrace OneAgent version 1.343. It contains:


- Feature updates : 16
- Fixes and maintenance : 32 (2 vulnerabilities)


Application Observability | Distributed Tracing


## Azure SDK tracing


We have introduced automatic Azure SDK tracing support for:


- [Service Bus ﻿](https://learn.microsoft.com/en-us/azure/service-bus-messaging/) for sending and receiving messages


- Java (Azure SDK for Java version 1.2.31+)


- Node.js


- Python


- [Event Hub ﻿](https://learn.microsoft.com/en-us/azure/event-hubs/) for:


- Java (Azure SDK for Java version 1.2.26+)


- Node.js


- [Cosmos DB ﻿](https://learn.microsoft.com/en-us/azure/cosmos-db/) for:


- Python


- Generic for Node.js


## Oldest supported versions


With this release, the following are the oldest supported OneAgent versions.


Support level


Oldest supported version


[Standard Support ﻿](https://www.dynatrace.com/services-support/)


1.325


[Enterprise Success and Support ﻿](https://www.dynatrace.com/services-support/)


1.319


For details, see[How long are versions supported following rollout?](https://docs.dynatrace.com/docs/whats-new#how-long-are-versions-supported-following-rollout) .


## Feature updates


Application Observability


### OpenFeature SDK support for Java


The OneAgent code module for Java now enriches distributed traces with information about feature flag evaluations of the OpenFeature SDK.


Application Observability


### Include Smartscape identifiers in process metadata file


The virtual file` dt_metadata_e617c525669e072eebe3d0f08212e8f2.json/properties` now contains Smartscape identifiers calculated by the Dynatrace OneAgent.


These can be used by applications to enrich their data and ensure the data is correctly associated with Smartscape entities.


For details, see[Enrichment Files](https://docs.dynatrace.com/docs/ingest-from/extend-dynatrace/extend-data#oneagent-virtual-files) .


Application Observability | Distributed Tracing


### Azure Functions for Python on Flex Consumption Plan for Linux


We have extended instrumentation for[Azure Functions ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/) (tracing, log correlation) for the Flex Consumption Plan for Linux for Python.
It supports the following trigger types (version 4.x):


- [HTTP and webhooks ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook)
- [Service Bus ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus)
- [Event Hubs ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs)
- [Timer ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-timer)
- [Blob Storage ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-blob-trigger?tabs=python-v2%2Cisolated-process%2Cnodejs-v4%2Cextensionv5&pivots=programming-language-python)
- [Event Grid ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-grid-trigger?tabs=python-v2%2Cisolated-process%2Cnodejs-v4%2Cextensionv3&pivots=programming-language-python)
- [Azure SQL ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-azure-sql-trigger?tabs=isolated-process%2Cpython-v2%2Cportal&pivots=programming-language-python)


Application Observability | Distributed Tracing


### Azure Functions for Java and Node.js on Consumption, Premium, and Dedicated Plan for Windows


We have extended instrumentation for[Azure Functions ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/) (tracing, log correlation) with:


- Consumption, Premium, and Dedicated Plan for Windows


- Technologies: Java and Node.js


- Trigger types (version 4.x):[HTTP and webhooks ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook) ,[Service Bus ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus) ,[Event Hubs ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs) ,[Timer ﻿](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-timer)


Application Observability | Distributed Tracing


### Azure Flex Consumption Plan for Linux Python app tracing


We have added tracing for Python Azure Functions on the Azure Flex Consumption Plan for Linux.


Application Observability | Distributed Tracing


### gRPC status code support for .NET applications


Support for .NET applications now includes the gRPC status code in traces.


Application Observability | Distributed Tracing


### Monitoring support for .NET single-file, self-contained applications


The OneAgent feature “Instrument .NET Single-File Self-Contained applications“ allows you to monitor[single-file, self-contained .NET applications ﻿](https://learn.microsoft.com/en-us/dotnet/core/deploying/single-file/overview) . This applies to[framework-dependent deployment ﻿](https://learn.microsoft.com/en-us/dotnet/core/deploying/?pivots=visualstudio#publish-framework-dependent) and[self-contained applications ﻿](https://learn.microsoft.com/en-us/dotnet/core/deploying/?pivots=visualstudio#publish-self-contained) .


Application Observability | Log Analytics


### Added support for quickly rotated or removed log files on local drives on Linux


OneAgent now supports the rotation patterns in which the files are compressed or removed immediately after the rotation. The supported scope is limited to Linux and only log files that are located on local drives.


Application Security


### softwareComponentFileHashes now available for classic .NET software component entities


Now hashes are available for classic .NET software component monitored entities, and can be queried via DQL:


```text
fetch   dt.entity.software_component   |        fields   packageName, softwareComponentVersion, softwareComponentFileHashes
```


This returns rows like:


```text
{           "packageName"  :     "newtonsoft.json"  ,           "softwareComponentVersion"  :     "13.0.4"  ,           "softwareComponentFileHashes"  :     {             "SHA1"  :     "420b9c95d22126470efd9e36c258ed3b727dcc19"           }        }
```


Data Observability


### Correct summarizing of the` “db.cosmosdb.request_charge”` attribute values


Corrected summarizing of the` “db.cosmosdb.request_charge”` attribute values of aggregated nodes/spans.


Digital Experience | RUM Web


### Capture actions for your users' hard navigations


OneAgent now captures actions for your users' hard navigations.


Infrastructure Observability | Extensions


### Timestamp with fractional seconds Unix epoch format supported by extensions


The Extension Execution Controller now accepts a dot-separated fractional part on Unix epoch seconds (for example,` 1772778391.123456` ), rounded to the nearest millisecond, both as JSON
strings and JSON floating-point numbers.


Platform | OneAgent


### Support for Redis ServiceStack for .NET


We’ve added tracing support for ServiceStack.Redis in .NET applications.


Platform | OneAgent


### Ingest enrichment configuration support from OneAgent


OneAgent now enriches telemetry data at the source based on a central enrichment configuration defined in the platform. You can define rules that conditionally apply primary tags, Cost Allocation (` dt.cost.costcenter` ,` dt.cost.product` ), and` dt.security_context` to all data from matching entities.


- **Conditional enrichment** : Define rules using the pattern` IF <condition on input fields> THEN enrich with <fields and values>` . Conditions are expressed with DQL matcher functions (equality, phrase, and existence checks) and support` NOT` ,` OR` , and` AND` operators.
- **Input fields** : Conditions can evaluate primary fields (clouds and Kubernetes metadata,` dt.host_group.id` ) and OneAgent resource attributes such as` host.name` ,` host.tags.<key>` ,` process.custom_metadata.<key>` , and` dt.process_group.detected_name` .
- **Static and derived enrichments** : Rules can add fixed key-value pairs, or use DPL transformations to derive tag values from fields OneAgent reads.
- **Enrichment scope:** Host-scoped rules enrich the host and all entities running on it (processes, containers, disks, network interfaces), while process-scoped rules enrich only the matching process.


Enrichment is applied to all data generated by a matching entity: metrics, spans, logs, Davis events, and Smartscape entities (` HOST` ,` DISK` ,` NETWORK_INTERFACE` ,` CONTAINER` ,` PROCESS` ). Configuration is fetched from the platform on startup and refreshed periodically, so changes apply without an agent restart.


Platform | OneAgent


### Support for trimmed containers and self-contained applications


Added support for trimmed containers and self-contained .NET applications.


The **Instrument .NET Single-File Self-Contained applications** OneAgent feature allows you to monitor[single-file, self-contained .NET applications ﻿](https://learn.microsoft.com/en-us/dotnet/core/deploying/single-file/overview) . This applies to[framework-dependent deployment ﻿](https://learn.microsoft.com/en-us/dotnet/core/deploying/?pivots=visualstudio#publish-framework-dependent) and[self-contained applications ﻿](https://learn.microsoft.com/en-us/dotnet/core/deploying/?pivots=visualstudio#publish-self-contained) .


For details, see available packages on[Dynatrace NuGet profile ﻿](https://www.nuget.org/profiles/Dynatrace) .


Platform | OneAgent


### Support for Kong Gateway Enterprise 3.10–3.14


The OneAgent NGINX module now supports Kong Gateway Enterprise versions 3.10–3.14. Runtime instrumentation must be enabled by setting the environment variable` DT_NGINX_FORCE_RUNTIME_INSTRUMENTATION=1` when starting Kong (see NGINX runtime instrumentation documentation).


## Fixes and maintenance


### General Availability (1.343.74.20260724-160807)


- A previous change introduced namespace-prefixed keys for` host.tags` and` host.custom_metadata` fields in the agent's Identification Message resource attributes (for example,` host.tags.dt.security_context` instead of` dt.security_context` ). This inadvertently broke downstream consumers that relied on the original, non-prefixed key names, affecting security context enrichment on metrics and Smartscape, cost allocation attributes, and general metric filtering by tag value. Both the prefixed and non-prefixed variants of all` host.tags` and` host.custom_metadata` fields are now written into the Identification Message resource attributes simultaneously, restoring compatibility while preserving the new prefixed format. A debug flag is also available to fully revert to the old behavior (non-prefixed keys only) without requiring additional backports. (OA-70727)
- Improved handling of quotes and escape characters for exception messages for[ASP.NET ﻿](https://dotnet.microsoft.com/en-us/apps/aspnet) Core applications. (OA-70642)
- Fixed an issue where zRemote could leak socket file descriptors when rejecting a zDC connection (because zRemote wasn't connected to a cluster), which could eventually exhaust file descriptors and prevent zRemote from accepting new inbound connections. (OA-70584)


- Fixed an issue that caused OneAgent to not be able to create and send an identification message to the cluster in case the environment variable` VCAP_APPLICATION` was present but did not contain a valid JSON object. (OA-70001)
- Fixed a zRemote crash and potential data loss caused by a race condition between a zDC disconnecting and an in-progress log configuration update from the cluster. (OA-69647)


- Added the` __oa.attr2att.trigger_mappers` attribute set to false on every generic Azure SDK span,
preventing the cluster from running attribute-to-attribute mapping mechanisms on them. (OA-69252)
- Fixed an issue where, in certain scenarios, Real User Monitoring (RUM) for ASP.NET Core could cause truncated or incomplete page responses. (OA-69215)


- Fixed an issue where the migration from` 40-coredump-dynatrace.conf` to` 99-coredump-dynatrace.conf` worked for fresh installations but failed for auto update. (OA-69073)
- Fixed an issue with the primary Grail fields and primary Grail tags input-output loop that made it impossible to remove them from dimensions after they were added. (OA-68996)


- Fixed an issue in the Python Azure SDK EventHub consumer instrumentation that caused certain operators or built-in functions (for example,` len` ) to fail when called on the event list passed to a user callback function. (OA-68632)
- Fixed an issue that caused the` If-None-Match` header to be incorrectly validated in Spring Framework 6.1+. (OA-68606)
- Improved handling of unexpected inputs in Live Debugger. (OA-68239)


- The newest version 3.X of the python library[kafka-python ﻿](https://pypi.org/project/kafka-python/3.0.0/) is now supported. (OA-68163)
-


**Vulnerability** : Hardened permissions of loganalytics storage directory. (OA-67849)
- Fixed an issue where MS SQL instance shutdowns were not detected on Windows. (OA-67832)


- For the IBM MQ sensor, missing JMS destination headers no longer trigger` NullReferenceException` . Now, instead of crashing, the empty destination field is displayed. (OA-67806)
- Fixed an issue in which the installer, on uninstall, failed to restore` /etc/sysctl.conf` from backup or remove the core pattern entry when no backup existed. Now the installer removes the core pattern entry from` /etc/sysctl.conf` , or restores its value from backup if one exists. (OA-67618)
- Fixed a potential crash on statically linked Go applications containing` libc` that could occur if the **Go improved static injection** OneAgent feature was turned on. (OA-67175)
-


**Vulnerability** : The CRI-O hook installation no longer allows privilege escalation from` dtuser` to` root` . Hook files are now always owned by` root` and protected against symlink attacks. (OA-66988)
- Fixed a bug in the Go SQS and SNS producer sensors that could cause concurrent access. (OA-66766)


- Fixed a bug in the Go SQS and SNS producer sensors that could cause incorrect distributed trace linking when Go applications reuse SendMessage or PublishEvent input structs and reuse all message/event attributes. (OA-66764)


- Resolved an issue where log enrichment produced invalid JSON when nested PatternLayout elements were present. (OA-66695)


- OneAgent now mitigates an issue where injecting script files into a Node.js process via` --require=A` or` --require "A"` could trigger a problem in the Next.js framework. This issue caused Next.js to transform the argument into an invalid form, for example,` --require "A A"` (since only one file per` require` is allowed), which in turn caused child processes or worker threads to fail on startup. This mitigation prevents crashes that could otherwise appear to be caused by OneAgent. (OA-66693)
- Resolved an issue that caused an IBM HTTP crash on AIX due to memory alignment issues in IBM libc. (OA-64690)


- Fixed a corner case that caused OTel spans not to be linked when OneAgent was present. (OA-62949)


- Fixed an issue where the resource attribute` zos.total_general_purpose_processors` was not populated (returned null) on z/OS HOST entities. (MGD-12807)
- Improved the RUM JavaScript SDK to better detect the RUM JavaScript object on the page. (DEM-28307)


- Fixed an issue where RUM JavaScript would potentially send duplicate CSP violation events on Firefox 149+ if the report-to header was set on the document. (DEM-27031)


- Process group names no longer revert to executable filename after JVM/.NET detection. On Windows, process groups having human-readable display names (sourced from the application's file description, for example,` Google Chrome` ) could unexpectedly revert to the raw executable filenames (for example,` _chrome.exe_` ) shortly after a Java or .NET agent was detected in a co-located process. The affected name would persist until the next full process discovery cycle. Now the display name is correctly preserved. (OA-67719)
- NGINX module measurements are now disabled for NGINX 1.29.6+. (OA-67680)


- We have changed the default order for resource attributes so that enrichment metadata now takes precedence over host file metadata. Previously, host file metadata was always collected first and could be overwritten by enrichment metadata; the order is now reversed. A new runtime flag allows restoring the previous behavior. (OA-67158)


- Traces for AWS Lambda functions using MySQL Connector/J no longer show unfinished spans caused by missing internal cleanup. (OA-66453)
