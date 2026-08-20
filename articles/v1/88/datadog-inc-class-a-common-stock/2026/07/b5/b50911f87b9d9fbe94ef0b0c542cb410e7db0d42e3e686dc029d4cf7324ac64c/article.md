---
schema_version: "1.0.0"
document_id: "b50911f87b9d9fbe94ef0b0c542cb410e7db0d42e3e686dc029d4cf7324ac64c"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/runtime-prioritization-engine/"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-08-01T15:12:05.089856+00:00"
fetched_at: "2026-08-01T15:12:07.874285+00:00"
content_hash: "sha256:51bd90ba9efe7c055e43c8cf67f05848ea629827961bb31b9dc344a5ed2cdf6e"
---

# Prioritize security findings with the Datadog Runtime Prioritization Engine

Christina DePinto


Senior Product Marketing Manager


Lucas Maley


Product Manager


Leo Wang


Product Manager


If you run a cloud security program, two questions follow almost every security finding: Who owns this? And how important is it? **** Many security tools answer those questions with static metadata such as owner tags, business criticality labels, and manually maintained inventories of critical assets, known as crown jewels. But cloud environments aren’t static. Teams reorganize, services change hands, and dependencies evolve. The result is stale tags, manual triage, and thousands of findings with little indication of which ones actually matter.


The[Datadog Runtime Prioritization Engine (RPE)](https://docs.datadoghq.com/security/cloud_security_management/triage_and_prioritize/runtime_prioritization_engine/) , a component of[Datadog Cloud Security](https://docs.datadoghq.com/security/cloud_security_management/) , helps you prioritize findings by identifying who should address them and whether they affect your most critical resources. It continuously analyzes the live telemetry data that you send to Datadog, combining runtime context with security signals to reduce alert noise.


In this post, we’ll explore how AI-powered capabilities in RPE automaticallyinfer ownership anddiscover crown jewels .


## Automatically infer ownership


The Runtime Prioritization Engine uses the[Ownership Agent](https://docs.datadoghq.com/security/cloud_security_management/guide/frontier_group/ownership_agent/) to identify the most likely owner for security findings, even when ownership metadata is incomplete or missing. The agent considers explicit ownership signals such as owner tags and ownership preferences when they’re available, and it uses observability and security telemetry data to fill in the gaps when that information is missing.


To infer ownership, the agent evaluates the following signals and combines them in a ranked evidence model:


-


Owner tags and ownership preferences


-


Ownership metadata as defined in the[Datadog Catalog](https://docs.datadoghq.com/internal_developer_portal/catalog/)


-


Cloud audit logs that identify who created or modified a resource


-


Container and host metadata


-


Naming conventions and organizational patterns


-


Source control integrations, including` CODEOWNERS` files


-


The affected resource and security finding


By combining these signals, RPE identifies the most likely owner and enriches that information with on-call schedules, team hierarchies, dashboards, ticketing and team member information, team messaging channels, and team-owned repositories. This context enables security teams to route findings and coordinate remediation by using their existing collaboration tools and engineering workflows.


Ownership information appears directly in the Cloud Security side panel for vulnerabilities and misconfigurations. You can review, confirm, or override suggested owners, helping the Ownership Agent continuously learn. You can also[define ownership preferences](https://docs.datadoghq.com/security/cloud_security_management/guide/frontier_group/ownership_preferences/) to map tags, exclude specific teams from being assigned ownership, and provide custom guidance directly to the agent.


## Automatically identify critical resources


Knowing who owns a finding is only half the equation. Security teams also need to understand what is actually worth protecting. Organizations typically maintain some version of a crown jewels inventory—a list of the applications, databases, and cloud resources whose compromise would have the greatest business impact. These inventories are often manually maintained and quickly become outdated as cloud environments evolve.


With[Datadog Crown Jewels](https://docs.datadoghq.com/security/cloud_security_management/crown_jewels/) , the Runtime Prioritization Engine uses observability data to build your inventory of critical resources automatically. Rather than relying on static classifications, RPE continuously analyzes runtime telemetry data to identify the services, databases, and cloud storage resources that matter most to your business. These assets typically handle sensitive data, process critical workloads, or occupy central positions in your production architecture.


RPE identifies crown jewels by using signals such as:


-


Sensitive data detected in[Datadog APM](https://docs.datadoghq.com/tracing/) spans, application logs, cloud storage, and API traffic


-


Database schemas that contain sensitive fields


-


Service dependency fan-in and architectural centrality from APM


Because RPE analyzes live observability data, the inventory of crown jewels continuously evolves. As services are deployed, workloads change, or instrumentation expands, RPE automatically updates the inventory to reflect which resources are critical.


Security teams remain in control with the ability to validate and refine the generated inventory. They can remove resources from the list and manually add assets that RPE didn’t include automatically.


## Start prioritizing findings with the Runtime Prioritization Engine


The Datadog Runtime Prioritization Engine, generally available, analyzes security findings to help you prioritize the issues that require remediation. By combining observability data with AI-powered ownership inference and Crown Jewels detection, RPE reduces alert noise and helps your teams focus on business-critical risks. To learn more,[read the Runtime Prioritization Engine documentation](https://docs.datadoghq.com/security/cloud_security_management/triage_and_prioritize/runtime_prioritization_engine/) .


If you’re new to Datadog, you cansign up for a 14-day free trial to get started.


-
-
-
