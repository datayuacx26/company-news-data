---
schema_version: "1.0.0"
document_id: "81e31fbf8e4618b22ac1831966063a46a61c063bceefd4680ef605d5e8e7b4a3"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:9e45b0713f1cc616f4d0af9724ff6629016a873fba29ae5761732708d5f78100"
---

# From API key to live threat detections in minutes: how Elastic Security ingests Google Threat Intelligence

2 June 2026 •


[Jamie Hynds](https://www.elastic.co/security-labs/author/jamie-hynds) •


[Mia LaVada](https://www.elastic.co/security-labs/author/mia-lavada)


# From API key to live threat detections in minutes: how Elastic Security ingests Google Threat Intelligence


Find out how Elastic Security ingests Google Threat Intelligence for continuous detection and uses AI-driven workflows to enrich alerts in real time, from API key to live detections in minutes.


5 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates)


Elastic Security natively ingests Google Threat Intelligence: known-malicious IPs, domains, URLs, and file hashes matched against your telemetry the moment they appear, each carrying a verdict and a 0–100 threat score. The setup consists of an API key and two data streams, with no extra infrastructure. When an indicator is ambiguous, workflows built on Agent Builder query VirusTotal in real time, enrich the alert, correlate with your telemetry, and summarize findings in real time.


##


How threat intelligence works in Elastic Security


In modern security operations, threat intelligence must work across detection, investigation, and response, not sit in a reference table.


Elastic Security supports this in two ways. Ingested intelligence via[integrations](https://www.elastic.co/docs/reference/integrations/threat-intelligence-intro) drives continuous detection and historical hunting. Agentic workflows, built on Elastic Workflows and Agent Builder, provide on-demand enrichment and investigative reasoning during an active investigation. This post focuses on how Elastic's Google Threat Intelligence (GTI) integration powers ingestion-based detection and hunting, and how it fits into a broader, more dynamic SOC model where AI-driven workflows use that intelligence at alert time.


##


What Google Threat Intelligence provides


The Google Threat Intelligence integration brings curated threat intelligence directly into Elastic Security, making it actionable across detection and investigation. GTI combines intelligence from Google's global security visibility with VirusTotal data to deliver enriched context on indicators of compromise, with coverage across malware, ransomware, phishing, infostealers, malicious infrastructure, threat actors, and other adversary activity.


Each indicator is returned with: a verdict (Malicious, Suspicious, or Undetected), a severity, and a composite threat score from 0–100. Because that score is derived from multiple signals, security teams can prioritize indicators based on confidence rather than their presence alone.


##


How the Google Threat Intelligence integration works in Elastic Security


Setup takes only a few minutes. You provide your GTI API key in the Elastic integration, and ingestion begins on a scheduled polling interval, with no additional infrastructure or collectors required. The integration ingests two primary data streams.


Purpose Threat List IOC Stream


Purpose High-confidence detection Threat hunting + early visibility


Volume Curated, lower volume Broader, higher volume


Best for Precision-critical alerting Emerging and exploratory activity


As data is ingested, indicators are standardized using the Elastic Common Schema (ECS), along with GTI context, such as verdict, severity, score, malware families, threat actor associations, and campaign metadata (where available). This enables GTI to be searched and correlated consistently alongside other ECS-compliant intelligence sources (including TAXII feeds), custom intelligence, and the broader security telemetry already present in Elastic Security. Elastic also manages indicator lifecycle automatically, including expiration and revocation, which reduces matches against stale intelligence. Once ingested, GTI indicators become part of the same searchable dataset as logs, endpoint, and cloud telemetry, enabling unified correlation across the environment.


##


Using Google Threat Intelligence for indicator match detections


Elastic's[indicator match rules](https://www.elastic.co/docs/solutions/security/detect-and-alert/indicator-match) use GTI data to detect when known malicious IPs, domains, URLs, or file hashes appear in security telemetry, continuously correlating intelligence against observed activity and surfacing matches for investigation. Because GTI provides structured fields such as score, verdict, and severity, teams can tune detections by confidence: high-confidence indicators can trigger immediate escalation, while lower-confidence indicators can be routed for review or further validation.


##


Threat hunting with GTI indicators in Elastic Security


With GTI metadata, analysts can pivot from a single IOC to all associated infrastructure and search historical telemetry; not just check if an indicator appeared, but understand what campaign it belongs to.


GTI enriches indicators with metadata such as threat actor associations and malware family context, allowing analysts to move beyond single-IOC searches. Hunters can pivot from an adversary or campaign to all associated indicators (IPs, domains, and file hashes) and search across historical telemetry using ES|QL. This makes it straightforward to determine whether known malicious infrastructure has ever interacted with the environment.


##


Monitoring threat intelligence activity with GTI dashboards


The integration includes prebuilt dashboards that provide visibility into threat intelligence activity and the detections GTI drives. Using saved searches and aggregated metrics, these dashboards summarize observed threats across malware families, campaigns, threat actors, toolkits, and vulnerabilities, helping SOC teams understand which threat types are most active in their environment and how intelligence is being operationalized.


###


Google Threat Intelligence feed categories and coverage


GTI includes 14 categorized feed categories, so organizations can tailor coverage to their needs and subscription level. Supported categories include:


- Cryptominers
- Trending threats
- Initial access and delivery vectors
- Infostealers
- IoT threats
- Linux malware
- Malicious infrastructure
- General malware
- Mobile threats
- macOS threats
- Phishing
- Ransomware
- Threat actors
- Vulnerability exploitation and weaponization


Availability depends on your Google Threat Intelligence subscription tier, and additional feeds can be enabled without changes to the Elastic configuration.


##


Agentic enrichment and real-time triage with Elastic Workflows


For ambiguous or emerging indicators not yet in an indexed feed, Elastic Security supports AI-driven investigation through Agent Builder and Elastic Workflows, which complement intelligence ingestion by enabling real-time enrichment and reasoning during an investigation.


With workflows, an analyst is no longer limited to the intelligence already in the index. During alert triage, a workflow can query external intelligence and reputation services such as VirusTotal in real time, enrich an alert with fresh context about the IPs, domains, or file hashes involved, correlate that live intelligence against Elastic telemetry, and summarize the findings into a structured investigation context that the analyst can act on. Agent Builder extends this further: teams can compose reusable, task-specific capabilities, such as agent skills for alert triage, enrichment, or case handling, so the assistant executes multi-step investigative tasks with the consistency of traditional automation, through a natural-language interface.


This introduces a complementary model. Ingested intelligence (GTI, TAXII, and custom feeds) provides continuous detection and historical hunting against indicators you already hold. Agentic workflows provide on-demand enrichment and investigative reasoning at alert time, reaching out to live sources and assembling context on the fly. Together, they enable teams to detect known threats at scale and provide context to investigations.


##


Getting started with Google Threat Intelligence in Elastic Security


To use the[Google Threat Intelligence integration](https://www.elastic.co/docs/reference/integrations/ti_google_threat_intelligence) in Elastic Security, you need an active GTI license and API key.


1. **Install:** open Integrations catalog in Kibana → search "Google Threat Intelligence" → add integration → enter your API key
2. **Configure the data streams:** enable Threat List (high-confidence detections) and IOC Stream (hunting coverage) → set polling frequency to match API limits and operational needs
3. **Tune:** prebuilt indicator match rules activate automatically; if alert volume is high, start by filtering on confidence threshold


All indicators are stored in Elasticsearch and accessible through the GTI threat intelligence data view, enabling search, correlation, and custom detection logic. Full configuration details and troubleshooting guidance are available in the official documentation.


##


Tying it all together


Threat intelligence only matters if a team can act on it. By bringing Google Threat Intelligence into Elastic Security, SOC teams get ingestion-based detection running continuously across their telemetry and agent-driven investigation reasoning over that intelligence in real time. The combination lets threat intelligence operate continuously and contextually, helping analysts move from indicators to confident decisions faster.


#### Jump to section


- [How threat intelligence works in Elastic Security](https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence#how-threat-intelligence-works-in-elastic-security)
- [What Google Threat Intelligence provides](https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence#what-google-threat-intelligence-provides)
- [How the Google Threat Intelligence integration works in Elastic Security](https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence#how-the-google-threat-intelligence-integration-works-in-elastic-security)
- [Using Google Threat Intelligence for indicator match detections](https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence#using-google-threat-intelligence-for-indicator-match-detections)
- [Threat hunting with GTI indicators in Elastic Security](https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence#threat-hunting-with-gti-indicators-in-elastic-security)
- [Monitoring threat intelligence activity with GTI dashboards](https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence#monitoring-threat-intelligence-activity-with-gti-dashboards)
- [Google Threat Intelligence feed categories and coverage](https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence#google-threat-intelligence-feed-categories-and-coverage)
- [Agentic enrichment and real-time triage with Elastic Workflows](https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence#agentic-enrichment-and-real-time-triage-with-elastic-workflows)
- [Getting started with Google Threat Intelligence in Elastic Security](https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence#getting-started-with-google-threat-intelligence-in-elastic-security)
- [Tying it all together](https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence#tying-it-all-together)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=From%20API%20key%20to%20live%20threat%20detections%20in%20minutes:%20how%20Elastic%20Security%20ingests%20Google%20Threat%20Intelligence&url=https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence&title=From%20API%20key%20to%20live%20threat%20detections%20in%20minutes:%20how%20Elastic%20Security%20ingests%20Google%20Threat%20Intelligence)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/elastic-security-google-threat-intelligence&title=From%20API%20key%20to%20live%20threat%20detections%20in%20minutes:%20how%20Elastic%20Security%20ingests%20Google%20Threat%20Intelligence)
