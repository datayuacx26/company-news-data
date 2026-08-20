---
schema_version: "1.0.0"
document_id: "03a647026b73b2a4164bae0308f770c6ad61b387c247bb733ea108648e692b8c"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/cloud-siem-claude-compliance-api-integration/"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:4afb3890a46257587e29a964162d34dbc8372522b53898668a68337142491e05"
---

# Monitor Claude Enterprise activity with Datadog Cloud SIEM

Kyra Abbu


Andréa Piazza


Thor Kell


Benjamin Goldberg


Leandro Almeida


Zander Mackie


Shreya Batra


Vera Chan


As Claude adoption expands across enterprises and workflows, security and compliance teams need to understand who is using Claude Enterprise, how it is accessed, and how it is administered and configured across the organization. The[Claude Compliance API](https://platform.claude.com/docs/en/manage-claude/compliance-api) gives organizations access to valuable activity data that supports security monitoring, investigations, and governance initiatives. However, organizations also need a way to use that data within their existing security workflows and correlate it with signals from other systems.


The[Datadog Claude Compliance API integration](https://docs.datadoghq.com/integrations/anthropic-compliance-logs/) helps you monitor Claude Enterprise activity at scale by collecting and analyzing events such as sign-ins, admin actions, API key life cycle events, and configuration changes. You can use[Datadog Cloud SIEM](https://www.datadoghq.com/product/cloud-siem/) detection rules, dashboards, and Open Cybersecurity Schema Framework (OCSF) normalization to quickly deploy, understand, and extend security coverage for Claude Enterprise activity.


In this post, we’ll explore how the Claude Compliance API integration helps you:


-


Ingest and normalize Claude Enterprise compliance events


-


Detect risky activity with prebuilt Cloud SIEM rules


-


Monitor Claude Enterprise compliance and administrative activity


-


Investigate Claude Enterprise activity alongside your broader security telemetry data


## Ingest and normalize Claude Enterprise compliance events


Events that are collected through the Claude Compliance API include details about user access, administrative actions, API keys, and configuration changes. When organizations bring this activity into broader security operations workflows, they often normalize the data so that it can be searched, correlated, and investigated consistently alongside other security telemetry data.


The Claude Compliance API integration’s prebuilt[log pipeline](https://docs.datadoghq.com/logs/log_configuration/pipelines/?tab=source) simplifies ingestion and normalization by parsing events into structured, searchable fields and aligning them with[Datadog’s OCSF-based common security data model](https://docs.datadoghq.com/security/cloud_siem/ingest_and_enrich/open_cybersecurity_schema_framework/) . This process standardizes key attributes such as actor, event type, workspace, source IP, user agent, affected resource, and action.


Standardized Claude compliance event fields enable analysts to search, filter, and correlate activity in[Log Explorer](https://docs.datadoghq.com/logs/explorer/) without writing custom parsing logic or wrangling unstructured event data. The resulting structured Claude activity data provides a reliable, consistent way to investigate compliance events, correlate related activity through Cloud SIEM signals, and monitor usage and governance trends through dashboards.


## Detect risky activity with prebuilt Cloud SIEM detection rules


Not every Claude compliance event warrants investigation, making it difficult for analysts to quickly identify activity that might indicate misuse or policy violations. Cloud SIEM prebuilt[detection rules](https://docs.datadoghq.com/security/detection_rules/) automatically flag events worth investigating, such as unexpected administrative changes, unusual API key creation, suspicious access patterns, and changes to sensitive workspaces.


Because Claude Enterprise activity is ingested alongside your existing security telemetry data, Cloud SIEM detection rules can correlate such activity with signals from other sources. For example, an API key created by an unusual actor, from an unexpected source IP, or shortly after suspicious authentication activity in your identity provider can automatically trigger a Cloud SIEM signal. Teams get more context per alert, fewer false positives, and a consistent process for monitoring AI-related activity across the organization.


## Monitor Claude Enterprise compliance and administrative activity


The Claude Compliance API integration also includes a dashboard that gives security and compliance teams a consolidated look at Claude Enterprise activity across their organization.


### Overview


The Overview section of the dashboard provides a high-level compliance snapshot: total event volume, unique actors, and a count of distinct source IPs. Summary metric tiles help teams quickly spot anomalous spikes or drops in Claude Enterprise event traffic, and an event volume timeline broken down by event name provides immediate visibility into which activities are driving those changes.


### Recent Activity


The Recent Activity section, designed for exploratory analysis, offers a broad view of compliance events organized by actor type, source IP, browser, and event name. Teams can quickly identify unusual patterns and pivot into logs for deeper investigation.


### SSO & SAML Activity


The SSO & SAML Activity section of the dashboard surfaces authentication events, including sign-in locations, per-user activity, and top event types. The geographic map makes it easy to spot logins from unexpected regions, a common early signal of account compromise. Additionally, the per-user and per-country breakdown provides the detail needed to investigate further.


### Admin & SSO Activity Streams


The Admin & SSO Activity Streams section focuses on high-impact security events: actions taken by admin API keys and changes to SSO/SAML configuration. Each log is designed to map back to a known operator or change ticket, giving compliance teams an audit-ready record of administrative actions and making it straightforward to flag anything that doesn’t correspond to an expected change.


## Investigate Claude Enterprise activity alongside your broader security telemetry data


Teams can also correlate Claude Enterprise activity with identity provider logs, cloud events, endpoint telemetry data, and application logs in Datadog to understand the broader context around any given event. This unified view helps analysts reduce tool switching, build a more complete timeline, and determine whether Claude Enterprise activity reflects expected behavior, a policy concern, or a potential incident.


For example, if a user creates an API key shortly after a suspicious authentication event in your identity provider, analysts can investigate both events from the same platform. If unusual file access appears in Claude Compliance API logs, teams can pull in related endpoint or network activity to determine whether further action is needed.


## Start monitoring Claude Enterprise activity with Datadog Cloud SIEM


When used with Cloud SIEM, the Datadog Claude Compliance API integration helps security and compliance teams ingest Claude activity, detect risky behavior, monitor usage trends, and investigate events alongside the rest of their security telemetry data. To configure the integration,[see the Claude Compliance API integration guide](https://docs.datadoghq.com/integrations/anthropic-compliance-logs/#setup) .


If you’re new to Datadog, you cansign up for a 14-day free trial to start monitoring your Claude Enterprise activity.


-
-
-
