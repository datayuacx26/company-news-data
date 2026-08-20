---
schema_version: "1.0.0"
document_id: "fbb51a0680ebfab3ffb267a804b17fe75a8348b3350a69c553fa524ffe7ae1f6"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/whats-new-in-dynatrace-saas-version-1-345/"
published_at: "2026-08-11T16:10:36+00:00"
first_seen_at: "2026-08-12T17:40:56.901810+00:00"
fetched_at: "2026-08-12T17:40:58.221028+00:00"
content_hash: "sha256:9c6392b80dc917a9e5076f8d0e0e748abe9c6b49e740ad3446f79e9ed30dda4f"
---

# What’s new in Dynatrace SaaS version 1.345

# What's new in Dynatrace SaaS 1.345


-


Release notes


-


12-min read


-
- Rollout start on Aug 11, 2026


This page showcases new features, changes, and bug fixes in Dynatrace SaaS version 1.345. It contains:


- Feature updates : 21
- Breaking changes : 3
- Fixes and maintenance : 19


Platform | OpenPipeline


## Enrich signals at ingest with the new Inline lookup processor


With the new **Inline lookup** processor, you can map an existing attribute on a record to a new or updated value using a lookup table you define directly in the pipeline, without connecting to an external system. Use it instead of complex DQL statements for common enrichment tasks—such as adding a description based on an error code, mapping an app ID to a business unit or cost center, or adding security context based on an existing attribute—so enrichment stays simple and readable.


## Feature updates


Application Observability | Distributed Tracing


### Dynatrace links are no longer persisted as span links


Dynatrace links between subpaths are no longer persisted as span links.


The Semantic Dictionary field` DT_TRACING_LINK_ID` is now deprecated.


Application Observability | Services


### Backtrace for database statements


From any database statement in the[Database queries](https://docs.dynatrace.com/docs/observe/application-observability/services/services-app#database-queries) view of


**Services** , you can now open a **Service backtrace** modal to understand the upstream call chain. This allows you to see from where specific statements are triggered, how load is distributed across callers, and whether any callers carry errors.


You can:


- See which services and endpoints are calling a specific database statement and how the load is distributed across them.


- Spot **1:n** fan-out patterns where a single upstream call triggers multiple database executions.
- Identify which upstream callers carry errors to gauge the blast radius of a failing query.


Application Security | Vulnerabilities


### Stay ahead of security risks in your AI services with automated vulnerability alerts


The **Notify on New Critical & High Vulnerabilities** workflow template automatically monitors your generative AI services for newly detected critical and high vulnerabilities and sends a Slack notification as soon as they appear. Each alert includes a breakdown of affected services, vulnerable components, and direct links to the security findings


**Vulnerabilities** and


**AI Observability** views—so your team can investigate and act without leaving their existing tools. To use it, install the template, connect your Slack workspace, and turn on the schedule trigger to get continuous coverage with no manual checks required.


Cost Intelligence


### Davis AI events are now usage tracked when routed to a non-default bucket


Davis AI events are now usage tracked if they're routed to a bucket other than` default_davis_events` .


When the events ingest usage tracking is updated, this can be visible on environment-level usage tracking, where one 15-min timeframe has less usage, and the next 15-min interval has more usage. The total usage stays the same.


Digital Experience | Users & Sessions


### Session Replay skips to the first replay view


Session Replay now skips directly to the first replay view when loading.


Infrastructure Observability


### Streamlined Dynatrace Snowflake Observability Agent deployment via installation wizard


Introduced a Dynatrace Snowflake Observability Agent (DSOA) installation wizard that streamlines the setup process within Business Observability.


- Simplified installation through a guided, step-by-step workflow.


- Reduced manual configuration effort.


- Faster and more consistent onboarding to DSOA capabilities.


Infrastructure Observability | Discovery & Coverage


### Network coverage now uses Smartscape queries


**Discovery & Coverage** network coverage now uses Smartscape on Grail queries. This means faster queries at a large scale. Additionally, we've added cross-linking to


**Smartscape** so you can quickly see network device relationships.


Infrastructure Observability | Infrastructure & Operations


### ActiveGate support for raw installers


The ActiveGate installer now supports raw installers on both Linux and Windows.


Infrastructure Observability | Kubernetes


### New centralized telemetry metadata enrichment


We've introduced a[new tagging strategy](https://docs.dynatrace.com/docs/manage/tags/tags-strategy) that allows you to centrally manage metadata enrichment across all telemetry data and data sources. With this central configuration, you can:


- Define custom key-value pairs as primary tags.


- Derive primary tags from Kubernetes namespace annotations and labels.


- Resolve and attach domain tags.


- Set fields such as security context, cost center, and cost product.


- Use Dynatrace Operator to enrich key-value pairs directly through DynaKube resource attributes.


This configuration supports metadata enrichment of Kubernetes signals and metadata enrichment of host/process signals (collected by OneAgent). Enrichment of cloud signals will be released in an upcoming Dynatrace version.


For more information, see[Enrich Kubernetes telemetry with primary Grail fields and tags](https://docs.dynatrace.com/docs/manage/tags/tags-domain-k8s) and[Enrichment of OneAgent telemetry](https://docs.dynatrace.com/docs/manage/tags/tags-domain-oneagent) .


Infrastructure Observability | Kubernetes


### Dynatrace Operator now supports platform tokens


Dynatrace is introducing platform tokens as the successor to access tokens, offering a more secure and unified way to authenticate with the Dynatrace platform.


We encourage you to start migrating from access tokens to platform tokens at your earliest convenience. Existing access tokens will continue to be accepted for the time being, but they will eventually be removed from Dynatrace Operator. We will inform you about dates and best practices with appropriate lead time in our release notes.


Infrastructure Observability | Kubernetes


### Auto-update for ActiveGate and OneAgent container images on public registries


Dynatrace now supports auto-update for ActiveGate and OneAgent images hosted on supported public registries, bringing the same seamless update experience previously available through the Dynatrace built-in registry. Supported public registries offer multi-arch images compatible with ARM64 (AArch64), x86-64, s390x, and PPC64le architectures on Linux, adhering to best practices for immutability and signing to strengthen supply chain security.


If you source images from a private registry, Dynatrace now emits events that can be used in Workflows to trigger custom image replication pipelines. This allows teams to keep their private registry automatically in sync, without sacrificing control over their internal image management processes.


For configuration details, a component breakdown, and more, see[Use a public registry](https://docs.dynatrace.com/docs/ingest-from/setup-on-k8s/guides/container-registries/use-public-registry) .


Platform


### Record deletion improvements


You can now track record-level deletion tasks more easily and get clearer status details while deletions are in progress. Broader deletion windows also make overall deletion progress more efficient, reducing the need to split a single deletion into many smaller requests (smaller than 24h). Status reporting now includes submission time and indicates when a deletion completed incompletely.


Platform | Dashboards


### Scanned bytes now displayed in GiB to align with pricing


In


**Dashboards** and


**Notebooks** , the **Scanned bytes** query metadata is now always formatted as GiB, aligning directly with the pricing rate card.


Platform | Dashboards


### Automatically format DQL queries in


**Dashboards** and


**Notebooks**


To automatically format a DQL query in


**Dashboards** or


**Notebooks**


1. Edit a DQL dashboard tile or notebook section.


2. In the upper-right corner of the DQL editor, select


>


**Format query** (or use the keyboard shortcut **Ctrl/Cmd+Shift+F** ) to instantly clean up your query for better readability.


Additionally, the


**Explain query** command has moved into the DQL editor menu.


Example "Format query" command


Platform | Dashboards


### Updated layout for


**Dashboards** and


**Notebooks**


In


**Dashboards** and


**Notebooks** , we've made some minor layout changes for consistency across apps and to support upcoming mobile improvements.


- The


**New dashboard/notebook** button has moved to the right in the app header and is now combined with the **Upload** button.
- In the left panel, the **All dashboards/notebooks** button has moved to the top. The full list now loads in the main content area, making it easier to get back to your last opened items.
- Direct entry to ready-mades has been removed from the panel but is still accessible as a tab in the dashboards and notebooks lists.


Platform | Grail


### Cost allocation for lookup data in Grail


You can now assign cost allocation metadata to your[lookup files](https://docs.dynatrace.com/docs/platform/grail/lookup-data) in Grail. This allows you to track and attribute storage and ingest costs to specific cost centers or products across your organization:


- The upload endpoint accepts two new optional parameters` dt.cost.costcenter` and` dt.cost.product` to associate a file with a cost center or product at upload time.
- The output of` fetch dt.system.files` includes` dt.cost.costcenter` and` dt.cost.product` fields so that you can query and filter your files by cost allocation metadata directly in DQL.
- The **Files - Ingest & Process** and **Files - Retain** billing usage events break down billed bytes per assigned cost center or product.
- Audit events capture the cost allocation configuration.


Platform | OpenPipeline


### Improved UX for pipeline stages


In OpenPipeline, we have improved the UX for pipeline stages:


- Accordion-based vertical pipeline UX with clear stage configuration status.


- Order of stages now reflects the processing sequence in OpenPipeline.


- Processor search across names and existing configuration values.


Platform | OpenPipeline


### Temporary fields within OpenPipeline


During OpenPipeline extraction and assignment, fields prefixed with` dt.temp` are now considered temporary and won't be stored in Grail, but are available throughout OpenPipeline processing. This allows easier creation of custom metrics and events without impact on the original data (such as logs and spans).


Platform | Platform Services


### EdgeConnect now supports Dynatrace domain patterns


You can now configure Dynatrace domains as host patterns in EdgeConnect, including wildcards like` *.dynatrace.com` and absolute hosts like` myenvironment.apps.dynatrace.com` . This enables cross-environment routing through a fixed IP, which is useful when the target environment enforces IP allowlisting.


This requires EdgeConnect version 1.744.0+


.


Platform | Platform Services


### Add document labels in


**Dashboards** and


**Notebooks**


In


**Dashboards** and


**Notebooks** , you can now add and delete labels on your dashboards and notebooks, and use them to search and filter your content list.


- To manage labels, use


( **Actions menu** ) on any dashboard or notebook and select **Info & labels** . Then use the **Add labels** field to create or assign labels.
- To use labels, list all dashboards or notebooks, and then use the **Filter by labels** dropdown to narrow results by one or more labels.


Threat Observability | Security events


### Know which AI workloads are vulnerable


When you ask about vulnerabilities on your AI workloads, the` dt-sec-insights` skill returns vulnerability insights for the monitored GenAI services in your environment. You get either confirmed vulnerable services or a clear response that there are no monitored AI workloads—never misleading results based on guesswork.


## Breaking changes


Application Observability


### OneAgent end-of-life version connection enforcement


Starting with this release, Dynatrace rejects connections from OneAgent versions 1.215 and earlier


.


If you’re running OneAgent version 1.215 or earlier


, upgrade to a supported OneAgent version to avoid data loss and connectivity issues, and to benefit from enhanced security and features unavailable in earlier OneAgent versions.


For details, see[End-of-life announcements](https://docs.dynatrace.com/docs/whats-new/technology/end-of-life-announcements) .


Application Observability | Services


### Improved derived` http.route` for enhanced endpoints


Enhanced endpoints now derive an` http.route` when OneAgent sends the` /*` fallback value. For details, see[Automatic naming for services without http.route](https://docs.dynatrace.com/docs/observe/application-observability/services/service-detection/service-detection-v1/enhanced-endpoints-sdv1#automatic-naming-without-http-route) .


Digital Experience


### Updated naming for hard navigation user actions


Hard navigation user actions are now named` Load <page name>` instead of` Loading of <page name>` , providing a more concise and consistent naming format across all user action types.


## Fixes and maintenance


### Resolved issues in this release (SaaS)


- Fixed a login issue where users with Time-based One-Time Password (TOTP) enabled could not sign in using **Sign in with Microsoft** without SAML federation, resulting in a redirect loop. (PS-47395)
- Fixed a pin-to-dashboard issue that caused HTTP 400 errors. (PRISM-13091)


- Added a fix to prevent SIGSEGV from happening when executing a gRPC call with a static metadata argument. (OA-69725)


- Fixed the cell actions for the **Type** column in


**Clouds** . (INFOBS-11615)
- Fixed an issue where the event would not be processed for problem creation if` event.severity` was set through a custom Davis event OpenPipeline processor to a type that was not the` long` Grail data type (as stated in[Semantic Dictionary](https://docs.dynatrace.com/docs/semantic-dictionary) ). (DI-30305)
- Fixed an issue where logs for failing tools did not display the tool name. (DI-29143)


- Fixed conditional permissions in


**Users & Sessions** . (DEM-30157)
- Updated session classification to distinguish standalone navigations from navigations that occur as part of a user action, resulting in more accurate session handling. (DEM-29922)


- Updated Session Replay to suppress the status cloak when a view has operations. (DEM-29568)


- Fixed an issue in settings where adding a new health alert for a frontend application did not show the unsaved-changes banner, making it impossible to save the new alert. (DEM-29307)


- Fixed an issue where named-target` POST` forms failed to submit when the target window was already open, causing the action to time out. If you applied the` disableNewWindowPostFormsHandling` experimental property as a workaround, you can now remove it. (DEM-28422)
- Fixed linking AWS MSK Connect metrics to entities in AWS monitoring in Latest Dynatrace. (DAQ-27361)


- Reduced amount and size of messages sent to the enrichment-processor by dropping metrics with no datapoints. (DAQ-27290)


- Fixed an issue in


**Live Debugger** where the **Edit breakpoint** pop-up remained open when you hovered over other context-menu items. (APPOBS-37406)
- Fixed an issue in which selecting **Open in Bitbucket** on a file opened the repo instead of the specific file. (APPOBS-37405)
- In


**Live Debugger** , added a new message stating, for non-code file types, that this file type is not supported for viewing. (APPOBS-37399)
- Fixed an enablement check in


**Live Debugger** that showed the incorrect "Live Debugger not enabled" notice. (APPOBS-37289)
- Fixed an error where the chart crashed if there were more than 50 legend items. (APPDEV-18778)


- Fixed an issue where certain Latest Dynatrace


environments could not access deprecated data. (PPX-13444)
