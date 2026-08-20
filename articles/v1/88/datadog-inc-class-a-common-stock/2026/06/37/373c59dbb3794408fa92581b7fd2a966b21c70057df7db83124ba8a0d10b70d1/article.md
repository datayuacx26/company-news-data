---
schema_version: "1.0.0"
document_id: "373c59dbb3794408fa92581b7fd2a966b21c70057df7db83124ba8a0d10b70d1"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/bits-threat-hunting/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:89c45655226ee23527778435fa906df95764e43186ab4375c24510f46cd48e28"
---

# Automate threat hunting with Datadog Cloud SIEM

Vera Chan


Sean Storer


Detection-based security is inherently reactive. Detection rules identify behavior that security teams have already anticipated and modeled. While detections remain critical to security operations, they cannot account for every attacker technique, environmental change, or emerging campaign, especially when[AI-driven attacks are increasing the volume and sophistication of threats](https://securitylabs.datadoghq.com/articles/2025-q3-threat-roundup/#attackers-are-quickly-adopting-ai-tooling-to-dynamically-generate-malicious-payloads-from-infected-hosts) that security teams must defend.


Proactive threat hunting—the practice of searching for adversary behavior before an alert fires—can help teams identify threats earlier in the attack life cycle. But threat hunting requires deep security expertise, familiarity with internal systems and individual business context, and sustained analyst attention, which makes continuous hunting difficult. For many organizations, threat hunting happens periodically during incident response engagements or after a security event rather than continuously as part of daily operations.


To make proactive hunting more accessible and integrate into your environment, we’re introducing[Bits Threat Hunting](https://www.datadoghq.com/product-preview/bits-threat-hunting) , an autonomous agent in Datadog Cloud SIEM that’s designed to help teams:


-


Extend threat hunting coverage with AI-driven investigations


-


Apply layered threat intelligence across security workflows


-


Adapt detections and investigations to their own environments


## Extend threat hunting coverage with Bits Threat Hunting


Effective threat hunting requires developing hypotheses about where attackers might operate, analyzing telemetry data for anomalous patterns, and investigating behaviors that differ from established baselines. Bits Threat Hunting conducts hypothesis-driven hunts across your environment. Instead of waiting for a rule to trigger, the agent analyzes telemetry data—including logs, network flows, identity events, and endpoint activity—to identify patterns associated with the hypothesized attacker behavior, emerging campaigns, and notable deviations from baseline activity.


For security operations teams, this capability expands proactive coverage without requiring analysts to manually investigate every potential lead. Analysts can spend more time validating critical findings and less time manually correlating telemetry data across systems. For security leaders, Bits Threat Hunting helps extend threat hunting capacity without requiring additional headcount.


## Bring layered threat intelligence into investigations


Threat hunting relies on vetted intelligence and a complete understanding of risks to an organization and attacker patterns. Because Bits Threat Hunting operates directly within Datadog Cloud SIEM, investigations can incorporate telemetry data and context from across cloud, hybrid, and on-premises environments. Datadog Cloud SIEM also[combines multiple layers of threat intelligence](https://docs.datadoghq.com/security/threat_intelligence) to provide more relevant context during investigations and hunting workflows.


### Bring Your Own Threat Intelligence


No external provider fully understands the threats that are unique to your organization, infrastructure, or industry. Bring Your Own Threat Intelligence (BYOTI) enables teams to import private feeds, industry-specific indicators, and internally discovered indicators of compromise (IOCs) into Datadog Cloud SIEM by[using reference tables](https://www.datadoghq.com/blog/add-context-with-reference-tables-in-cloud-siem/) .


With Reference Tables, organizations can ingest data from Information Sharing and Analysis Centers (ISACs), prior incident investigations, or other internal intelligence sources and incorporate that data directly into detection and hunting workflows.


### Datadog Research


Datadog’s security research team routinely monitors attacker infrastructure, malware campaigns, and cloud-focused attack techniques. Research findings are integrated directly into Datadog Cloud SIEM so customers can benefit from updated intelligence without manually managing feeds or custom enrichment pipelines. When Datadog identifies new infrastructure patterns, attacker behaviors, or evasion techniques, that intelligence becomes available across the platform, including in the[Indicators of Comprise (IOC) Explorer](https://docs.datadoghq.com/security/cloud_siem/triage_and_investigate/ioc_explorer/#use-the-ioc-explorer) .


### Recorded Future


[Datadog Cloud SIEM also integrates with Recorded Future](https://www.datadoghq.com/blog/recorded-future-content-pack/) to provide additional threat intelligence enrichment. Recorded Future aggregates intelligence from open web, dark web, and technical sources to provide context about threat actors, malware families, vulnerabilities, and malicious infrastructure. Within Cloud SIEM, this integration helps analysts quickly answer questions such as whether an IP address is associated with known attacker infrastructure, whether a domain has appeared in phishing campaigns, or whether a vulnerability is being actively exploited.


### Spur Intelligence


One of the most persistent challenges in modern threat investigation is distinguishing legitimate anonymous traffic from adversarial use of VPNs, residential proxies, and relay infrastructure. Attackers deliberately operate through this noise to obscure activity and complicate investigations.


[Datadog’s integration with Spur](https://spur.us/) helps teams track VPN providers, proxy services, hosting providers, and other infrastructure commonly associated with account takeover and fraud. When anomalous authentication attempts or API activity involve known anonymizing services, analysts can immediately incorporate that context into triage and investigation workflows.


## Adapt security operations to your environment


Modern environments include cloud infrastructure, SaaS applications, containers, identity systems, edge services, and increasingly, AI workloads. These environments generate organization-specific risks that predefined detection models aren’t built to address. Datadog Cloud SIEM combines out-of-the-box detections with configurable intelligence sources, partner integrations, and AI-assisted investigation workflows. BYOTI, integrations with Recorded Future and Spur Intelligence, and Datadog Research provide layered intelligence that reflects your environment and threat landscape.


Bits Threat Hunting extends those capabilities by applying AI-driven reasoning across your telemetry data to surface suspicious behavior that predefined rules may not yet identify. This capability is a part of Datadog’s broader investment in autonomous Security Operations Center (SOC) workflows—from proactive threat hunting to large-scale triage, investigation, and response with[Bits Security Analyst](https://www.datadoghq.com/product/ai/bits-ai-security-analyst/) .


To learn more about Datadog’s proactive threat hunting,[sign up for the Preview.](https://www.datadoghq.com/product-preview/bits-threat-hunting)


To get started with Datadog Cloud SIEM,[read the Cloud SIEM documentation](https://docs.datadoghq.com/security/cloud_siem/) . If you don’t already have a Datadog account, you can[sign up for a free 14-day trial](https://www.datadoghq.com/free-datadog-trial/) .


-
-
-
