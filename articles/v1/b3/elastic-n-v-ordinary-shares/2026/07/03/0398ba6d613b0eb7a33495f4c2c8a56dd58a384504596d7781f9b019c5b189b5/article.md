---
schema_version: "1.0.0"
document_id: "0398ba6d613b0eb7a33495f4c2c8a56dd58a384504596d7781f9b019c5b189b5"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/axonius-integration"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:670c3d9e860c0fed8659e6155a0d6cd6c58226d08deae835f6c387a6eeff1940"
---

# Elastic and Axonius integrate to deliver unified asset intelligence for security teams

# Elastic and Axonius integrate to deliver unified asset intelligence for security teams


Visibility across devices, identities, SaaS, and exposures inside Elastic Security


By


[Chaison Griffin](https://www.elastic.co/blog/author/chaison-griffin)


July 16, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


Security teams cannot protect what they cannot see, such as reconciling devices, identities, applications, SaaS, and exposures across hybrid cloud, on-prem, and OT from dozens of tools. Axonius specializes in aggregating and correlating that asset intelligence so that IT, security, and governance, risk, and compliance (GRC) teams get a coherent view of the environment.


Today, we are announcing an integration with Axonius that brings normalized asset context into Elastic, so analysts can search, correlate, and investigate Axonius data alongside endpoint alerts, network telemetry, identity events, and the rest of your Elastic Security data.


## Agentless setup


The


[Axonius integration](https://www.elastic.co/docs/reference/integrations/axonius) supports both agent-based and agentless deployments. Agentless ingestion is supported on Elastic Cloud Serverless and Elastic Cloud Hosted, which reduces the operational overhead of maintaining and managing dedicated agents and the underlying infrastructure to support them.


## What the integration collects


The Axonius integration periodically queries Axonius APIs and writes normalized documents into Elasticsearch across several data streams. Each stream represents a distinct category of asset intelligence, and together, they fill gaps that traditional agent-based telemetry alone can’t cover.


### Application visibility (axonius.application)


This is the richest data stream for security analysts. Rather than a single endpoint, it aggregates across Axonius's full application surface: installed software, SaaS applications, licenses, browser extensions, audit activity, business application metadata, secrets, and URLs spanning both managed and unmanaged territory.


Key fields include


audit_activities


(actions like


login


,


create


,


update


,


delete


,


reset_password


tied to specific users and applications), extension metadata differentiating IT-deployed versus user-installed tooling, and SaaS footprint data across your identity and application landscape.


### Users (axonius.user)


User records are normalized to ECS


user.*


[fields](https://www.elastic.co/docs/reference/ecs/ecs-user) . Because Axonius aggregates across HR systems, directories, SaaS platforms, and security tools, these records frequently surface accounts not visible through any single source like orphaned identities, service accounts, or users present in one system but absent in another.


### Network (axonius.network)


The network records are the core asset intelligence data stored within Axonius by using specialized Source Adapters. The Source Application is the specific connected security, IT, or cloud tool that detected, reported, or provided the record for an asset, software, or application. Network records allow for full scope asset inventory and data enrichment using ECS fields


user.*


and


related.ip


.


### Vulnerabilities and exposures (axonius.exposure)


These vulnerability-oriented events with CVE and CVSS fields are mapped to ECS


vulnerability.*


. Exposure data reflects what's been correlated across your connected tools, meaning the same CVE can be tied to asset ownership, business criticality, and control coverage in the same record.


### Security incidents and findings (axonius.incident, axonius.alert_finding)


Incidents and findings from connected tools, including cloud workload alerts, adapter health failures, and platform-level findings, are enriched with severity, recommendation text, and MITRE-oriented labels where applicable. These records are tagged


event.kind: alert


, making them compatible with Elastic detection rule workflows.


### Infrastructure and operational context (axonius.storage, axonius.ticket, axonius.adapter, axonius.gateway)


Cloud and object storage assets, ITSM tickets with status and priority timelines, adapter connection health, and gateway metadata are also included. The adapter health stream (


axonius.adapter


) is worth particular attention as it indicates when a data source goes silent, which is often the first signal that coverage has drifted.


## From alert to investigation with ES|QL and Workflows


[Elastic Workflows](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4) can automate asset enrichment so that every Elasticsearch Query Language (ES|QL) query runs against a live, pre-built lookup index, removing manual lookups and tool switching. Workflows periodically searches the Axonius asset data and writes it to a dedicated index set to


[lookup mode](https://www.elastic.co/docs/explore-analyze/workflows) . That lookup index can then be referenced in ES|QL queries to enrich logs and alerts with asset context on the fly, correlating events with attributes like device owner, business unit, or risk score without any manual lookups.


When threat hunting or working an investigation, raw log data can’t tell you who owns a device, what software is running on it, and whether it meets compliance baselines. Axonius supplies that context.


### ES|QL LOOKUP JOINS


Here’s an example ES|QL query enriching context from Axonius asset data onto failed authentication events using the lookup index created by the workflow.


### Elastic Agent Builder


Analysts can also use the


[Elastic Agent Builder](https://www.elastic.co/elasticsearch/agent-builder) to build workflows through natural language: describing the automation they need, iterating in chat, and skipping the manual configuration entirely.


## Operationalize asset intelligence directly inside Elastic Security


Integrating Axonius with Elastic gives security teams a unified view of their environment by bringing normalized asset intelligence, including covering devices, identities, applications, vulnerabilities, and exposures, directly into Elastic Security. With automated lookup index enrichment via Elastic Workflows and ES|QL for on-the-fly correlation, analysts can move faster from alert to investigation with full asset context at their fingertips. The result: analysts spend less time assembling context and more time on the investigations that matter.


[Learn more about the Axonius integration and try it out today](https://www.elastic.co/docs/reference/integrations/axonius) .


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, and associated marks are trademarks, logos or registered trademarks of elasticsearch B.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
