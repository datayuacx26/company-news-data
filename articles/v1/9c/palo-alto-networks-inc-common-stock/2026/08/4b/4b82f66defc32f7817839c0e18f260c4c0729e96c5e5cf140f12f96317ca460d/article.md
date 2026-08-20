---
schema_version: "1.0.0"
document_id: "4b82f66defc32f7817839c0e18f260c4c0729e96c5e5cf140f12f96317ca460d"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/soc-identity-front-door/"
published_at: "2026-08-07T23:00:01+00:00"
first_seen_at: "2026-08-08T01:31:26.677639+00:00"
fetched_at: "2026-08-08T01:31:27.665339+00:00"
content_hash: "sha256:787ba4c042a32b42fa5a704d89fa8daecd30cf98560bbfcb14144466e38122fe"
---

# Inside the Modern SOC: The Identity Front Door

## The Identity Gap: Why Trust Has Become the New Attack Surface


In[The 72-Minute Race](https://unit42.paloaltonetworks.com/soc-72-minute-race/) , we explored how attackers are compressing the time between initial access and business impact. But as attacks continue to accelerate, another trend has emerged: Attackers are increasingly gaining access through compromised identities rather than exploiting technology vulnerabilities.


According to the[2026 Unit 42 Global Incident Response Report](https://www.paloaltonetworks.com/resources/research/unit-42-incident-response-report) , identity weaknesses played a role in nearly 90% of incidents investigated by Unit 42. The report also found that 65% of initial access activity involved identity-based techniques, underscoring how credential theft, multifactor authentication (MFA) manipulation, session hijacking and social engineering have become some of the most effective ways to gain access to enterprise environments.


## Anatomy of a Modern Identity-Driven Compromise


Across recent Unit 42 investigations, we see a consistent pattern. Attackers often gain initial access through:


- Phishing campaigns
- Social engineering calls
- MFA fatigue attacks
- Compromised third-party accounts
- Misuse of help desk processes


Once inside, attackers establish persistence, elevate privileges and move laterally across various environments. These activities often resemble legitimate administrative behavior. Malicious activity can remain hidden long enough for attackers to broaden their foothold before security teams recognize the full scope of the incident.


## The Attacker's Playbook in Action


**A social-first entry:** Threat groups such as Muddled Libra (aka Scattered Spider) demonstrate how many attackers increasingly rely on social engineering and identity abuse as part of their toolkit.


**Expansion through identity:** Once inside, attackers exploit identity weaknesses to establish persistence, compromise additional accounts and escalate privileges to strengthen their foothold. Each action expands their access and makes the compromise more difficult to contain.


**The escalating access:** As highlighted in the 2026 Unit 42 Global Incident Response Report, 87% of incidents span multiple attack surfaces. An initial compromised identity can quickly become a multi-domain investigation that requires defenders to connect activity across the environment.


**The objective:** Whether the objective is ransomware deployment, data theft, financial fraud or long-term persistence, identity compromise often serves as the foundation for broader attacker objectives.


From a tooling perspective, the warning signs are often already present across the organization’s security controls. Without automated correlation, these signals can appear low priority in isolation, allowing attackers to expand their access before defenders recognize the full scope of the incident.


## How Our Unit 42 Managed Services Team Responds


When investigating identity-driven attacks, our Unit 42 analysts use the[Cortex SecOps](https://www.paloaltonetworks.com/cortex) platform to unify security telemetry into a single investigative view, allowing them to quickly validate suspicious activity and understand the full scope of the attack. Our[24/7 Managed Detection and Response](https://www.paloaltonetworks.com/unit42/respond/managed-detection-response) (MDR) team continuously investigates suspicious activity while our threat hunters proactively search for signs of identity compromise that may not yet have generated an alert. AI-driven correlation, behavioral context and Unit 42 threat intelligence help our teams quickly validate high-confidence incidents and determine the full scope of attacker activity.


Organizations using[Managed XSIAM](https://www.paloaltonetworks.com/cortex/managed-xsiam) extend this approach through AI-driven correlation, integrated investigation and response workflows and continuous SOC engineering delivered by Unit 42 experts. Rather than requiring internal teams to continuously engineer and optimize the platform as attacker techniques evolve, our experts refine:


- Data integrations
- Custom detections
- Correlation rules
- Automated response playbooks


This helps organizations identify identity-driven attacks earlier and accelerate response before attackers can expand their access.


## Advice for SOC Leaders: Look Beyond the Login


As identity attacks continue to evolve, security leaders should focus on the operational challenges that often prevent teams from detecting identity-driven attacks early.


### Prioritize Identity Context


A successful login alone is not enough to indicate normal user activity. Correlating identity activity with endpoint, cloud, SaaS and network telemetry provides the behavioral context needed to distinguish legitimate users from compromised accounts.


### Reduce Manual Investigation


Consolidate telemetry and investigations into a unified view to reduce analyst pivots between disconnected tools. Centralized visibility enables security teams to identify attacker activity faster and respond with greater confidence.


### Continuously Improve Detection


Attackers continuously adapt their techniques. Regularly refining detections, correlation rules and response playbooks helps ensure defenses evolve alongside emerging identity-based threats.


### Protect Time for Threat Hunting


Dedicated threat hunting helps uncover credential abuse, privilege escalation and hidden persistence before they escalate into larger incidents.


## What's Next


In our next entry in this series, we'll examine why modern attacks increasingly cross security domains and why unified visibility has become essential for detecting and stopping multi-surface attacks before they escalate into business impact.


## The Unit 42 Managed Services Edge


*Identity attacks have become one of the most effective ways for adversaries to bypass traditional security controls. Unit 42 combines expert-led Managed Detection and Response (MDR), proactive threat hunting, continuous SOC engineering and frontline incident response expertise to help organizations identify identity-driven attacks earlier and respond with greater confidence.*


Learn more about[Unit 42 Managed Services](https://go.paloaltonetworks.com/contactunit42mdr) .
