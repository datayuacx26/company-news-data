---
schema_version: "1.0.0"
document_id: "8286e356ad9efc6c68b512167f81fb16eb95db5e274db21d08a707105579e823"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/soc-72-minute-race/"
published_at: "2026-06-15T23:00:19+00:00"
first_seen_at: "2026-07-22T17:15:05.756127+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:e2f79c848d1c0fc319a5061baba3788a421a1f08a35b5ebdb0c41e6eb0425e42"
---

# Inside the Modern SOC: The 72-Minute Race

## The Speed Gap: Where Strategy Meets Reality


This marks the beginning of our series, *Inside the Modern SOC: Trends and Insights from Unit 42 Managed Services* . This series draws directly from Unit 42 customer environments, security operations center (SOC) assessments, threat hunting engagements and frontline investigation experience to highlight the operational patterns shaping modern security operations.


Through our work helping organizations detect, investigate and respond to threats, one theme continues to surface: The speed gap has become one of the defining operational challenges facing today's SOC. Drawing on findings from the[2026 Unit 42 Global Incident Response Report](https://www.paloaltonetworks.com/resources/research/unit-42-incident-response-report) , we can see that attack timelines have compressed dramatically as adversaries use AI to move faster and automate more of the attack lifecycle. In the fastest cases, attackers moved from initial access to confirmed data exfiltration in just over an hour (72 minutes), representing a 4X year-over-year acceleration.


When security operations still rely on manual triage and fragmented workflows, defenders are forced to operate on a timeline modern attackers have already outpaced. This is not a personnel problem; it’s a process problem. By the time an alert is validated through manual steps, the adversary has often already achieved their objective.


## Anatomy of a Modern Identity-Driven Attack


Across recent Unit 42 investigations, we continue to see a consistent pattern: attackers leveraging compromised credentials, identity manipulation, privilege escalation and rapid lateral movement to compress attacks that once unfolded over days into hours, or even minutes. Threat actors such as[Muddled Libra](https://unit42.paloaltonetworks.com/muddled-libra/) (aka Scattered Spider) and Spoiled Scorpius, distributors of[RansomHub](https://unit42.paloaltonetworks.com/tag/ransomhub/) ransomware, exemplify this broader trend.


### The Attacker's Playbook in Action


**The Social Entry:** Initial access is often gained through compromised credentials, MFA manipulation, help-desk impersonation or other identity-based tactics. This pattern appeared across many of the investigations we handled over the past year. According to the 2026 Unit 42 Global Incident Response Report, 65% of initial access is driven by identity-based techniques.


**The Rapid Escalation:** Once inside, attackers frequently attempt privilege escalation and administrative account abuse within minutes or hours of gaining access. Unit 42 has observed suspicious identity activity quickly escalating into abnormal administrative behavior and signs of privilege escalation.


**The Multi-Surface Pivot:** Attackers increasingly move across identity, endpoint, cloud and Software as a Service (SaaS) environments. Once elevated privileges are obtained, they may provision cloud resources, create rogue virtual machines, mount virtual drives or establish persistence to support data staging and exfiltration.


**The Rapid Impact:** Unit 42 investigations continue to show attackers compressing the time between initial access and business impact. In some cases, threat actors such as Spoiled Scorpius have exfiltrated hundreds of gigabytes of data within hours of gaining access through improperly secured remote access infrastructure.


From a tooling perspective, the warning signs were often already present across the organization's identity and endpoint security controls. Multiple alerts had been generated, but without automated correlation, each appeared low priority in isolation. Connecting these signals manually takes time, a luxury attackers no longer allow.


### How Our Unit 42 Managed Services Team Responds


In investigations involving identity-driven attacks, our analysts use the[Cortex SecOps platform](https://www.paloaltonetworks.com/cortex) to quickly connect unusual privileged account activity, PowerShell execution, abnormal authentication patterns, privilege escalation attempts and lateral movement indicators to understand the full scope of an incident. Additional context, including device history, process activity, threat intelligence and behavioral analytics, helps determine whether activity is legitimate or indicative of attacker behavior. By analyzing these behaviors in context, our teams can quickly identify high-confidence incidents and contain compromised accounts before activity expands further across the environment.


Organizations using[Managed XSIAM](https://www.paloaltonetworks.com/cortex/managed-xsiam) extend this model through AI-driven correlation, integrated response workflows and continuous SOC engineering that helps reduce investigation and response times. This shift from sequential investigation to real-time correlation helps security teams keep pace with compressed attack timelines. Instead of spending critical minutes manually stitching together fragmented alerts, analysts can move quickly from detection to confident response.


## Advice for SOC Leaders: Re-Engineer for Velocity


Closing the speed gap requires evolving how your security operations function. Modern threats require an operating model that matches attackers’ velocity.


**Move Beyond Sequential Workflows:** Shift from linear "Triage → Investigate" models to workflows where enrichment happens automatically in parallel. Analysts should not need to manually search multiple tools to understand whether an alert is serious.


**Correlate by Default:** Related signals across identity, endpoint, cloud and network activity should automatically group into unified incidents. This reduces investigation time and helps eliminate analyst fatigue caused by fragmented tooling. Per the Unit 42 Global Incident Response Report, in 87% of incidents investigators reviewed evidence from two more distinct sources to establish what occurred. Complex cases drew on as many as 10 sources.


**Operationalize Response:** Predefine containment actions for common attack scenarios such as compromised accounts, suspicious PowerShell execution, malware activity or unauthorized remote access. When attackers move in minutes, response decisions cannot begin from scratch every time.


**Prioritize Behavior Over Indicators:** Focus on attacker behaviors such as rapid privilege escalation, impossible-travel logins, unusual access patterns or abnormal process execution chains. These behaviors often reveal malicious intent earlier than static indicators alone.


## What's Next


In our next entry in this series, we'll explore another trend keeping security leaders up at night: Attackers have stopped "breaking in" and started "logging in." We'll examine how identity-based attacks are rapidly replacing malware as the preferred path to compromise and what organizations can do to defend against them.


## The Unit 42 Managed Services Edge


*We help organizations close the speed gap by combining expert-led operations with real-time detection, investigation, and response.*[Unit 42 Managed Detection and Response (MDR)](https://www.paloaltonetworks.com/unit42/respond/managed-detection-response) *combines AI-driven automation with world-class threat hunters, analysts, and responders who proactively uncover threats, investigate high-risk activity, and act quickly when minutes matter. Together, these capabilities help organizations accelerate detection, investigation, and containment while improving security outcomes.*


*For organizations pursuing broader SOC modernization, Managed XSIAM extends these capabilities with 24/7 expert-led operations, integrated response, continuous SOC engineering, and a breach response guarantee that includes 250 hours of Unit 42 Incident Response support. Together, these capabilities help organizations reduce operational complexity, strengthen security outcomes, and build a more resilient security operation prepared for today's threat landscape.*


Learn more about[Unit 42 Managed Services .](https://go.paloaltonetworks.com/contactunit42mdr)
