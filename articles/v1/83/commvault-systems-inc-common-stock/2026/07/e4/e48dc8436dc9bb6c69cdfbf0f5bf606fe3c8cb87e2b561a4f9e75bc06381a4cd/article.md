---
schema_version: "1.0.0"
document_id: "e48dc8436dc9bb6c69cdfbf0f5bf606fe3c8cb87e2b561a4f9e75bc06381a4cd"
company_key: "commvault-systems-inc-common-stock"
company: "Commvault Systems Inc."
source_id: "commvault-systems-inc-common-stock-news-import-d7ff9e033aa3"
canonical_url: "https://www.commvault.com/blogs/how-commvault-responds-to-third-party-incidents"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-27T16:39:35.443534+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:7c92efb16d045f028cc9fb14908c7a72cf0d6ea43e414bb19a6889108f4eb227"
---

# When the Risk Comes From Outside: How Commvault Responds to Third-Party Incidents

Security does not end at the edge of an organization’s own systems. Modern businesses connect a growing web of third-party applications to their core platforms to support sales, service, and collaboration. Each of these connections adds value. Each one also introduces exposure the organization does not fully control.


That risk is not hypothetical. In June 2026, a threat actor compromised OAuth tokens tied to Klue, a competitive intelligence platform used to sync sales and marketing data with Salesforce. The attacker used those tokens to reach the Salesforce environments of the many organizations that had authorized the integration, including Commvault’s.


As soon as we were notified of potential impact, our Security team activated our incident response process to determine what had occurred, contain the exposure, and assess whether customer information or Commvault services were affected.


Our investigation found that the activity was limited to certain business relationship and sales information maintained within our Salesforce environment. The investigation found no indication that any customer backup data, product data, product metadata, operational logs, or Commvault services were impacted.


### Acting Quickly When It Matters


Our response followed established security incident response procedures built to contain risk quickly while supporting a thorough investigation. Once we were notified of the incident, we disabled the Klue integration, revoked the associated access, and worked with the appropriate parties to conduct a full assessment of what happened.


Throughout the investigation, our teams worked to determine what information had been accessed, validate the integrity of our environment, and confirm the incident stayed within the scope we had already contained.


### A Pattern Worth Noting


This incident is one recent example of a pattern security teams have watched grow for several years: attackers targeting third-party applications connected to core business systems rather than attacking those systems directly. A single compromised integration can offer a trusted path into the environments of many downstream organizations at once, often with less resistance than a direct attack on any one of them.


This shifts where an organization’s defense actually has to live. Strong internal controls remain necessary, but they are no longer sufficient by themselves. They have to be paired with active oversight of every application an organization connects, and a response capability that is ready before an incident, not built during one.


### Building Resilience Beyond Our Own Environment


At Commvault, our security program includes ongoing assessment of the third-party applications connected to our environment. We review connected applications on a regular basis, evaluate the access each one holds, monitor for emerging risk, and reassess those integrations as business needs and the threat landscape change. When circumstances warrant, we act to reduce exposure and strengthen our posture, including disconnecting integrations that no longer meet our standards.


### Our Commitment to Transparency


Trust is built through openness and accountability. When an event affects our stakeholders, we believe it is important to communicate what we know, explain how we responded, and share the outcome of our investigation, even when the event originated outside our own systems.


We will continue to evaluate our security controls, refine our incident response processes, and strengthen our approach to third-party risk as part of our broader commitment to protecting our customers and partners.


For the official details of this incident, including the scope of the investigation and customer guidance, please refer to our[Trust Center Updates](https://trust.commvault.com/updates) .


[Will Galway](https://www.linkedin.com/in/william-galway-802a54/) *is Deputy CISO at Commvault.*
