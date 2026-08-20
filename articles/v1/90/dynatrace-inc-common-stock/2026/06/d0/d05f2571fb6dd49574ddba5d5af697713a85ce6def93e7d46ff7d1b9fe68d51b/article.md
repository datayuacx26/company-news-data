---
schema_version: "1.0.0"
document_id: "d05f2571fb6dd49574ddba5d5af697713a85ce6def93e7d46ff7d1b9fe68d51b"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/oneagent-release-notes-version-1-341/"
published_at: "2026-06-30T06:44:04+00:00"
first_seen_at: "2026-07-30T07:17:54.686242+00:00"
fetched_at: "2026-07-30T07:17:56.314455+00:00"
content_hash: "sha256:6e5c1b4d342efe237dce7d548350787f51561a2a8485493fdbb624cc867ef03f"
---

# OneAgent release notes version 1.341

# What's new in Dynatrace OneAgent 1.341


-


Release notes


-


8-min read


-
- Rollout start on Jun 30, 2026


This page showcases new features, changes, and bug fixes in Dynatrace OneAgent version 1.341. It contains:


- Feature updates : 30
- Breaking changes : 2
- Fixes and maintenance : 21 (1 vulnerability)


## Oldest supported versions


With this release, the following are the oldest supported OneAgent versions.


Support level


Oldest supported version


[Standard Support ﻿](https://www.dynatrace.com/services-support/)


1.323


[Enterprise Success and Support ﻿](https://www.dynatrace.com/services-support/)


1.317


For details, see[How long are versions supported following rollout?](https://docs.dynatrace.com/docs/whats-new#how-long-are-versions-supported-following-rollout) .


## Feature updates


Application Observability


### MBAgent: Timer node tagging support for IBM ACE TimeoutControl and TimeoutNotification


MBAgent now captures tagging information for IBM ACE TimeoutControl and TimeoutNotification timer nodes, enabling distributed trace context to be associated with timer-driven message flows.


Application Observability | Distributed Tracing


### Automatic Azure SDK support for Java


OneAgent now supports automatic tracing of the Azure SDK in Java applications (Azure SDK for Java versions 1.2.9+).


Application Observability | Distributed Tracing


### Yii framework support for PHP applications


We’ve added support for the Yii framework for PHP applications.


Application Observability | Distributed Tracing


### Azure Functions Timer trigger tracing in Python


Azure Functions Timer trigger tracing is now available in Python.


Application Observability | Distributed Tracing


### Resource exhausted JVMTI events enriched with Smartscape resource attributes


OneAgent Java enhances resource-exhausted JVMTI events with additional resource attributes.


Application Observability | Distributed Tracing


### Azure Functions Azure SQL trigger tracing in Python


Azure Functions Azure SQL trigger tracing is now available in Python.


Application Observability | Distributed Tracing


### Extended Apache Thrift supported versions


We’ve extended the support for Apache Thrift up to version 0.22.0.


Application Observability | Distributed Tracing


### Added support for Go library` aws-sdk-go-v2/service/sqs`


OneAgent now supports monitoring AWS SQS services in Go applications.


Application Observability | Distributed Tracing


### Memory-optimized instrumentation of .NET applications


To benefit from our new memory-optimized instrumentation of .NET applications, go to


**Settings** > **Collect and capture** > **General monitoring settings** > **OneAgent features** and activate **Advanced .NET Instrumentation Mode \[Opt-In\]** .


Application Observability | Distributed Tracing


### Added support for Go library aws-sdk-go-v2/service/dynamodb


OneAgent now supports monitoring AWS DynamoDB services in Go applications.


Application Observability | Open Telemetry


### OpenTelemetry version update


We’ve updated` opentelemetry-proto` to version 1.10.0.


Application Security


### Cloud-native tagging support for security events


Primary Grail tags and fields are now available on security finding and scan events for Runtime Vulnerability Detection and Runtime Application Protection.


Digital Experience | RUM Web


### Added support for load actions in RUM JavaScript


RUM JavaScript now sends user action events for hard navigations if the agent initializes before the page is loaded. If the agent is injected after the page has already loaded, it won't send a related user action event. Learn more about[user actions in web frontends](https://docs.dynatrace.com/docs/observe/digital-experience/rum/web-frontends/concepts/user-actions) .


Digital Experience | RUM Web


### Improved shadow DOM handling in RUM JavaScript


The RUM JavaScript agent now collects` dt` attributes from the host and parent tree when an interaction target is inside an open shadow root.


Digital Experience | RUM Web


### RUM JavaScript now supports custom trackers for user actions


You can now register custom trackers for all user actions globally or for specific action instances. These trackers enable you to use custom logic to delay action completion until asynchronous operations (such as GraphQL queries or images loading within the viewport) are fully resolved.


Infrastructure Observability


### Support for Node.js v26


OneAgent now automatically identifies Node.js v26 processes and injects them with the Node.js code module.


Infrastructure Observability


### Smartscape on Grail support for mainframe


Data ingest (logs, spans, etc.) from OneAgent installations on mainframe is now enriched with` dt.smartscape.<entity>` fields for easy correlation of data and Smartscape entities.


Infrastructure Observability


### OS module events enriched with Smartscape source information


Events from the OS module are now enriched with the full Smartscape vertical stack.


Infrastructure Observability | Extensions


### Unified analysis pages are shipped as documents within Extensions


Unified analysis pages used across different Dynatrace applications can now be directly packaged with each extension. The pages are provided to the respective applications at the extension installation time.


Infrastructure Observability | Hosts


### Support for new systemd service startup types


OS services monitoring now supports indirect, linked, and linked-runtime systemd unit file startup types.


Infrastructure Observability | Hosts


### New os_service.* property keys


New` os_service.*` property names for events and availability metrics are now available in addition to the old` dt.osservice.*` names. The existing` .osservice.*` names will be deprecated on Dynatrace SaaS in a future release.


Infrastructure Observability | Hosts


### OS services in Smartscape


OS services can now be queried from the Smartscape storage via` smartscapeNodes OS_SERVICE` .


For more information, see[Smartscape - Core Entities](https://docs.dynatrace.com/docs/semantic-dictionary/model/smartscape/core) .


Infrastructure Observability | Kubernetes


### Container entities indicate containerization type used to run them


Container entities in Smartscape on Grail have a new field,` container.containerization_type` , indicating which containerization type is used to run the container (for example,` cri-o` ).


Platform | OneAgent


### Extended Apache Thrift supported versions


Dynatrace now supports Apache Thrift up to version 0.22.0.


Platform | OneAgent


### Primary fields available on z/OS and for JMX metrics


OneAgent now extends source-level telemetry enrichment to additional domains:


- **JMX Extension metrics:** JMX-based metrics now support primary tags and primary fields enrichment.
- **Mainframe (z/OS):** Primary tags and primary fields can now be applied to spans and metrics produced by the z/OS agent, including Java processes. Enrichment can be defined via file or environment variable where supported.


Platform | OneAgent


### Trace IMS, JMSReply, and Timer nodes in IIB/ACE message flows with OneAgent


- **IMS Node Tagging:** OneAgent now instruments and tags IMS nodes in IBM IIB/ACE message flows, enabling end-to-end tracing for message flows that interact with IBM Information Management System (IMS) backend systems.
- **JMSReply Node Tagging:** OneAgent now tags JMSReply nodes, completing distributed tracing coverage for request/reply patterns over Java Message Service (JMS) in IIB/ACE flows.
- **Timer Node Tagging:** OneAgent now tags Timer nodes, enabling visibility into scheduled and timer-triggered message flows as distinct services in Dynatrace.


Platform | OneAgent


### Dynatrace Platform UX for OneAgent Log module configuration


OneAgent and Kubernetes Log module configurations are now available as a redesigned experience in


**Settings** . The new UIs give you visibility into discovered log sources, streamline onboarding and management, clarify how rules combine across scopes, and align with Dynatrace Platform applications such as OpenPipeline.


- **New Sources** overview with an environment-wide view of autodiscovered log sources, coverage indicators across host groups, Kubernetes clusters, and hosts, and a direct entry point to add missing custom log sources.
- New UIs for log ingest rules, custom log sources, sensitive data masking, timestamp and splitting patterns, and other advanced settings.


- Improved UX for inherited rules, hierarchy, and overrides, making it clear which configuration applies at each scope.


- Guided configuration flow from autodiscovered log sources to ingest rules.


Find it at


**Settings** > **Collect and capture** > **Log monitoring** > **Configure log module** .


The classic Log module settings UI remains available in


**Settings Classic** while you migrate to the new experience. We recommend migrating to the new experience, which is future-proof and where all future improvements will be delivered.


Platform | OneAgent


### IBM MQ v10 on z/OS is now supported


OneAgent now supports IBM MQ v10 on z/OS.


Platform | OneAgent


### Db2 metrics now labeled in the Data Explorer view


Handling of Db2 metrics has been improved in the **Data Explorer** view. They were previously labeled as **Unknown Process** .


Platform | OneAgent


### Timer nodes support for IBM Integration Bus / ACE


OneAgent now supports tagging for IBM IIB/ACE timer nodes. Timer nodes are now properly detected and tagged by the MBAgent, enabling end-to-end tracing visibility for message flows that are triggered by scheduled/timer-based events. Timer nodes in IIB/ACE message flows now show these entry points correctly, consistent with existing tagging support for other node types (IMS, Callable, JMSReply).


## Breaking changes


Digital Experience | RUM Web


### Update your user actions API calls: Deprecated identifiers removed


Starting with OneAgent version 341, the deprecated` userAction.finish()` ,` autoClose` , and` name` identifiers in the[User actions API ﻿](https://dt-url.net/7c237vk) are removed.


**Action plan:** Replace the deprecated identifiers in your code:


- ` userAction.finish()` >` userAction.complete()`
- ` autoClose` >` completeAutomatically`
- ` name` >` customName`


Infrastructure Observability | Hosts


### Dynatrace OneAgent now supports the Windows Redirection Guard on Windows editions that provide this feature


OneAgent now rejects paths and files that resolve through NTFS junctions created by non-elevated (regular) Windows user accounts. This affects all locations OneAgent accesses, such as during process technology detection.


This change reduces the attack surface for junction-based privilege escalation on Windows by preventing OneAgent from following junctions that non-privileged users can create.


The behavior is applied automatically on supported Windows versions, no action is required for typical deployments. If your setup relies on junctions created by non-elevated users for monitoring purposes, review and update those resources before upgrading.


## Fixes and maintenance


### General Availability (1.341.42.20260629-145200)


- Reverted the OneAgent .NET IBM MQ tracing sensor to the previous version. (OA-70255)


- Fixed a zRemote crash and potential data loss caused by a race condition between a zDC disconnecting and an in-progress log configuration update from the cluster. (OA-69647)


- Fixed an issue where the migration from` 40-coredump-dynatrace.conf` to` 99-coredump-dynatrace.conf` worked for fresh installations but failed for auto update. (OA-69073)
- Fixed an issue in the Python Azure SDK EventHub consumer instrumentation that caused certain operators or built-in functions (for example,` len` ) to fail when called on the event list passed to a user callback function. (OA-68632)
- Fixed an issue in which declarative process grouping rules were not applied for some types of processes, such as` dockerd` . (OA-68624)
- Improved support for Nginx versions 1.29.6+ (OA-68032)


- ` undici` now correctly loads via[import - Javascript | MDN ﻿](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) in ESM. (OA-67702)
- Fixed an issue where the installer, during uninstall, failed to restore` /etc/sysctl.conf` from backup when one existed, or remove the core pattern entry when no backup existed. Now the installer restores the core pattern entry from backup if one exists, or removes it otherwise. (OA-67618)
- Fixed a potential crash on statically linked Go applications containing` libc` that could occur if the **Go improved static injection** OneAgent feature was turned on. (OA-67175)
- Fixed a bug where the date search limit set in **Advanced log settings** was also applied when parsing timestamps from a CRI layer. (OA-66690)
- Fixed an issue where early logger initialization in OneAgent's Java code-level vulnerability and attack evaluation could cause application startup to fail. (OA-66505)


- Fixed a memory leak in old connection objects when using FlawFinder with asynchronous Java web APIs. (OA-66494)


- Fixed an issue where the DTSLPTF function of the CICS and IMS SDK occasionally returned error code` 8` . (OA-66469)
- Fixed an issue with OneAgent log rotation not working for some directories. (OA-66409)


- Go improved static injection \[opt-in\] is now deactivated by default for new configurations. If you’ve customized this setting, your configuration is preserved. This change applies to Dynatrace version 1.337+


. (OA-66375)
- Removed support for Linux OSs with the System V runtime. (OA-66058)


- ` oneagentctl.exe` supports querying and setting Windows redirection guard policy. (OA-65716)
- Fixed an issue where OneAgent incorrectly identified non-AWS hosts as AWS EC2 instances when the host's cloud provider (for example, Hetzner) exposed metadata on the same IP address as the AWS EC2 Instance Metadata Service. (OA-65703)


- Fixed the default configuration of W3C and Dynatrace tagging for AWS Lambda layers. (OA-65586)


- Fixed an issue where disabling log metadata enrichment at runtime left empty enrichment brackets` {}` in unstructured Log4j 2 log entries. (OA-65527)
-


**Vulnerability** : Dynatrace OneAgent now supports the Windows Redirection Guard on Windows editions. (OA-63228)
