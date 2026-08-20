---
schema_version: "1.0.0"
document_id: "86877fe60b49dffde3cbad5af3e01408b17cbb27f614587c315a30ac230885d6"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-news-import-a6b69c49a265"
canonical_url: "https://www.tenable.com/blog/coordinated-cyberattack-on-minnesota-water-utilities-what-you-need-to-know"
published_at: "2026-07-29T03:19:08+00:00"
first_seen_at: "2026-07-29T10:39:22.393309+00:00"
fetched_at: "2026-07-29T10:39:23.905365+00:00"
content_hash: "sha256:cf6da04eba7ee979e9ccf61c4db5753f758589bb89b85242909752b015135e11"
---

# Minnesota & other US Water Cyber Attacks, CISA AA26-097A | Tenable®

**A coordinated cyber attack disrupted water systems across more than 30 Minnesota communities. Here is what defenders need to know about the attack so far. This FAQ also details recent cyberactivity targeting internet-exposed PLCs, and how to protect exposed infrastructure.**


## Key Takeaways


1. A coordinated "cyberattack" targeted water and wastewater systems across more than 30 Minnesota communities on July 26-27, 2026; attribution remains pending a federal investigation, though the timing aligns closely with escalating Iranian-affiliated PLC exploitation activity documented in CISA Advisory AA26-097A.
2. The July 22, 2026 update to CISA Advisory AA26-097A expanded the scope of observed PLC exploitation to include Schneider Electric and Siemens devices alongside Rockwell Automation, documented project file exfiltration for the first time, and added detection guidance for manipulation of reusable code modules embedded in PLC programs.
3. [CVE-2021-22681](https://www.tenable.com/cve/CVE-2021-22681) (CVSS 9.8), a critical authentication bypass in Rockwell Automation Logix controllers with no available vendor patch, was added to CISA's Known Exploited Vulnerabilities catalog in March 2026 following confirmed exploitation by Iranian-affiliated actors.


## Background


Tenable's Research Special Operations (RSO) team has compiled this FAQ in response to the coordinated cyber attack against Minnesota water utilities and the July 22, 2026 update to CISA Advisory AA26-097A.


On April 9, 2026, we published[a comprehensive FAQ about CyberAv3ngers](https://www.tenable.com/blog/what-to-know-about-cyberav3ngers-the-irgc-linked-group-targeting-critical-infrastructure) , the IRGC-linked group at the center of the PLC exploitation activity documented in CISA Advisory AA26-097A. That post covers the group's history, four-phase capability escalation, IOCONTROL malware, and a full technical breakdown of CVE-2021-22681. This post addresses what has changed since then.


## FAQ


**What happened to water utilities in Minnesota?**


Between Sunday, July 26, and Monday, July 27, 2026, a coordinated cyber attack disrupted water and wastewater utility operations across more than 30 communities in Minnesota. Four cities publicly disclosed attacks: Braham, Plymouth, South St. Paul, and Maple Plain. Minnesota IT Services described the incident as a coordinated cyber attack targeting technology at community water systems across the state.


In Braham, a community of approximately 1,700 people, the attack disabled computerized operating controls and temporarily shut down the city's well and water treatment plant. Public works crews restored the plant within approximately two hours. In Plymouth (population approximately 80,000), the city's IT division disconnected cellular-connected equipment at two water towers and multiple wastewater lift stations to stop the attack and prevent retargeting while equipment was reconfigured. Crews continued operating through manual procedures. South St. Paul reported that some automated water utility controls were affected, while Maple Plain declared a local state of emergency to expand its response capabilities.


In all confirmed cases, officials said drinking water quality was not affected and no boil-water advisories were issued. The Minnesota Department of Health is working directly with targeted water systems to protect public health.


**Who is behind the attack?**


Federal and state officials have not publicly attributed the Minnesota attacks to any specific actor. However, the operational pattern is consistent with the CyberAv3ngers threat ecosystem, a state-directed group the U.S. government has formally attributed to Iran's Islamic Revolutionary Guard Corps Cyber-Electronic Command.


In February 2024, the U.S. Treasury Department sanctioned six IRGC-CEC officials for directing CyberAv3ngers operations, and the State Department offered up to $10 million for information on the group through the Rewards for Justice program. The group has been active since at least 2020; the security community tracks it under multiple designations, including Storm-0784 (Microsoft), Bauxite (Dragos), Hydro Kitten, UNC5691 (Mandiant), and MITRE ATT&CK group ID G1027. In April 2026, Tenable Research Special Operations published a[comprehensive FAQ on CyberAv3ngers](https://www.tenable.com/blog/what-to-know-about-cyberav3ngers-the-irgc-linked-group-targeting-critical-infrastructure) detailing the group's history, capabilities, and targeting profile.


The timing of the Minnesota attacks is significant. CISA updated Advisory AA26-097A on July 22, just four days before the attacks began, warning that Iranian-affiliated actors had been compromising internet-connected PLCs across U.S. water, energy, and government sectors. The advisory documented confirmed disruptions and financial losses at multiple organizations.


**What changed in the July 22, 2026 update to CISA Advisory AA26-097A?**


The original advisory, published April 7, 2026, documented Iranian-affiliated actors exploiting internet-exposed Rockwell Automation/Allen-Bradley PLCs to cause disruptions across the Government Services, Water and Wastewater, and Energy sectors. CISA confirmed disruptions at victim organizations have occurred since at least March 2026. Three significant additions were made in the July 22 update.


The advisory expanded its scope to include observed targeting of Schneider Electric and Siemens PLCs alongside Rockwell Automation devices. The update also documented a new exfiltration tactic: using vendor engineering software (Rockwell Automation's Studio 5000 Logix Designer, Schneider Electric's EcoStruxure Control Expert, and Siemens' TIA Portal) on leased, third-party hosted infrastructure, actors have been observed pulling PLC project files out of victim environments and transferring them to systems under their control. Finally, the update provided detection guidance for manipulation of Add-On Instructions (AOIs), reusable code modules embedded in PLC programs. The FBI observed that at one victim, a malicious project file retained normal downstream ladder logic while inserting modified AOIs that disabled safety shutdown and alarm systems; actors also manipulated data on HMI and SCADA displays, allowing equipment to operate in unsafe conditions without alerting operators.


A new set of indicators of compromise was released alongside the update, covering IP addresses associated with actor activity from September 2025 through July 2026. The advisory directly references observed targeting of Schneider Electric BMX P34/Modicon M340 PLCs and Siemens S7-1200 PLCs, and notes that activity on ports associated with other OT vendor protocols suggests broader opportunistic targeting beyond the named manufacturers.


**How has CyberAv3ngers evolved over time?**


CyberAv3ngers has demonstrated a deliberate capability escalation across four documented phases:


In **Phase One (2020–2022)** , the group operated as a propaganda persona, claiming responsibility for infrastructure disruptions in Israel that were later assessed as fabricated. DomainTools Investigations demonstrated that several claims reused imagery from earlier data leaks.


In **Phase Two (October 2023–January 2024)** , the group compromised at least 75 Unitronics Vision Series PLCs across the United States, Israel, the United Kingdom, and Ireland by exploiting default passwords on internet-exposed devices. The Municipal Water Authority of Aliquippa, Pennsylvania, was the highest-profile victim.


In **Phase Three (2024–2025)** , the group deployed IOCONTROL, a custom-built malware platform for IoT and OT devices. OpenAI disclosed in October 2024 that CyberAv3ngers had used ChatGPT to assist with target reconnaissance and code debugging.


In **Phase Four (March 2026 to present)** , the group pivoted to exploiting[CVE-2021-22681](https://www.tenable.com/cve/CVE-2021-22681) , a critical authentication bypass in Rockwell Automation Logix controllers. Actors connected to internet-facing PLCs from foreign hosting providers using the same manufacturer engineering software (Studio 5000 Logix Designer) that legitimate operators use. Once connected, they downloaded and modified controller project files and altered operator display data. The July 22 advisory update expanded the manufacturer scope to include Schneider Electric and Siemens PLCs alongside Rockwell Automation.


**What vulnerability is being exploited?**


CISA Advisory AA26-097A does not name specific CVE IDs. The advisory characterizes the access method as exploitation of internet-exposed, misconfigured PLCs through vendor engineering software, rather than identifying a discrete software flaw. The vulnerability most directly associated with the documented exploitation is[CVE-2021-22681](https://www.tenable.com/cve/CVE-2021-22681) , identified through external research as the authentication weakness enabling Phase 4 of the Iranian-affiliated PLC exploitation campaign, and added to CISA's Known Exploited Vulnerabilities catalog in March 2026.


Two additional CVEs relevant to the Rockwell Automation ControlLogix platform are included below as defensive context for organizations hardening these environments:


**CVE** **Description** **CVSSv3**


[CVE-2021-22681](https://www.tenable.com/cve/CVE-2021-22681) Authentication bypass in Rockwell Automation Logix controllers via insufficiently protected cryptographic key 9.8


[CVE-2023-3595](https://www.tenable.com/cve/CVE-2023-3595) Remote code execution in Rockwell ControlLogix 1756 communication modules 9.8


[CVE-2024-6242](https://www.tenable.com/cve/CVE-2024-6242) Trusted Slot feature bypass in Rockwell ControlLogix controllers 8.4*


*CVE-2024-6242: CVSS 3.x score of 8.4 is vendor-assigned (Rockwell Automation); NVD assigns a CVSS 4.0 score of 7.3.


[CVE-2021-22681](https://www.tenable.com/cve/CVE-2021-22681) is a critical authentication bypass in Rockwell Automation's Logix controller ecosystem caused by an insufficiently protected cryptographic key used to verify communications between Studio 5000 Logix Designer and Logix PLCs. Anyone who can obtain or intercept that key can pose as legitimate engineering software and gain direct, unauthenticated access to affected controllers. For the full technical breakdown of this vulnerability, see our[April 9 FAQ](https://www.tenable.com/blog/what-to-know-about-cyberav3ngers-the-irgc-linked-group-targeting-critical-infrastructure) .


[CVE-2023-3595](https://www.tenable.com/cve/CVE-2023-3595) affects the 1756 EN2 and EN3 series ControlLogix communication modules. Exploitation via maliciously crafted CIP messages could allow an unauthenticated remote attacker to execute arbitrary code and maintain persistent access, including modifying controller memory. This CVE has not been confirmed as exploited in connection with the AA26-097A campaign; it is included because the affected hardware is central to the ControlLogix platform being targeted.


[CVE-2024-6242](https://www.tenable.com/cve/CVE-2024-6242) , identified by Claroty Team82, affects the Trusted Slot feature in the Rockwell Automation 1756 ControlLogix chassis. Exploitation allows an attacker already on the chassis to execute CIP commands that bypass Trusted Slot protections, potentially modifying user projects. This CVE has not been confirmed as exploited in connection with the AA26-097A campaign and is included as hardening context for ControlLogix deployments.


**Was CVE-2021-22681 exploited as a zero-day?**


No. CVE-2021-22681 was originally disclosed in February 2021 and mitigated at that time. However, in-the-wild exploitation was not confirmed until March 2026, when Rockwell Automation updated its advisory (SD1672) to note active exploitation. The five-year gap between disclosure and confirmed exploitation reflects the persistent challenge of patching OT environments, where operational continuity often takes precedence over security updates.


**Is there a patch available?**


No. Rockwell Automation has stated that CVE-2021-22681 cannot be fully addressed with a software patch. There is no update to deploy and no patch cycle to wait for. Rockwell directs customers to apply defense-in-depth mitigations instead, including network segmentation, engineering workstation isolation, CIP (Common Industrial Protocol) Security enablement, and physical mode switch hardening.


This is a critical operational detail for vulnerability management teams: organizations that rely on patch-based remediation workflows will not resolve this vulnerability through their standard processes. Architectural controls are the only available remediation path.


**Are there indicators of compromise?**


Yes. CISA released an updated STIX-formatted indicator bundle alongside the July 22 advisory revision covering IP addresses associated with Iranian-affiliated actor activity targeting PLCs from September 2025 through July 2026. Historical indicators from the April 7 advisory also remain valid. Both sets are available for download at the[AA26-097A advisory page](https://www.cisa.gov/news-events/cybersecurity-advisories/aa26-097a) .


Defenders can monitor for inbound traffic on ports 44818 (EtherNet/IP), 2222 (SSH), 102 (S7comm), 502 (Modbus), and port 22 on OT-connected modems, particularly from foreign-hosted IP ranges. CISA noted that in at least one incident, actors deployed Dropbear SSH on victim modems to maintain persistent remote access over port 22. Unexpected connections to PLC management interfaces from unregistered workstations or overseas infrastructure warrant investigation, as do unauthorized PLC project file modifications or AOI changes occurring outside documented maintenance windows.


**Has Tenable Research classified these vulnerabilities as part of Vulnerability Watch?**


Yes. Tenable Research has classified[CVE-2021-22681](https://www.tenable.com/cve/CVE-2021-22681) as part of[Vulnerability Watch](https://www.tenable.com/blog/reducing-remediation-time-remains-a-challenge-how-tenable-vulnerability-watch-can-help) . We are actively monitoring exploitation activity, tracking new developments, and will update this post as additional information becomes available.


**How large is the exposure surface?**


Censys analysis conducted in April 2026 identified 5,219 internet-exposed hosts globally that responded to industrial protocols and self-identified as Rockwell Automation/Allen-Bradley devices. The United States accounts for 74.6% of global exposure, with 3,891 hosts. A disproportionate share of these devices operate on cellular carrier networks, consistent with field-deployed PLCs connected via cellular modems, which is precisely the attack vector described in the Plymouth, Minnesota, incident.


The systemic nature of this exposure extends beyond Rockwell devices. The EPA warned in 2024 that more than 70% of U.S. water systems were failing to comply with a provision of a 2018 law requiring them to develop or update risk assessments and emergency response plans. An audit of 1,000 water systems serving 193 million people found 97 systems with critical or high-risk vulnerabilities. The United States has between 150,000 and 170,000 water systems, many of them small, rural, and with limited cybersecurity resources.


**Why do small water utilities keep getting targeted?**


CyberAv3ngers has repeatedly compromised small water utilities and municipal facilities, and the pattern is structural rather than coincidental. Many of these organizations manage their OT environments with consumer-grade remote access tools such as TeamViewer or AnyDesk, or by exposing PLC management interfaces directly to the public internet. These access methods bypass enterprise security controls entirely, creating an attack surface that is invisible to conventional security monitoring.


The problem is compounded by inadequate network segmentation between IT and OT environments. When a PLC is reachable from the same network as email servers and employee workstations, the blast radius of any compromise extends well beyond the initial entry point. Small utilities typically lack dedicated OT security staff and operate under constrained budgets that make comprehensive security architecture difficult to implement.


**Is there a broader geopolitical context?**


Yes. The United States and Iran have been in active armed conflict since[Operation Epic Fury](https://www.centcom.mil/MEDIA/PRESS-RELEASES/Press-Release-View/Article/4418396/us-forces-launch-operation-epic-fury/) commenced on February 28, 2026. Iranian cyber operations have escalated in parallel with kinetic hostilities, with confirmed targeting of U.S. critical infrastructure. In June 2026, U.S. strikes along Iran's southern coast[destroyed a water facility near the Strait of Hormuz](https://www.aljazeera.com/news/2026/6/10/us-bombs-irans-water-facilities-why-thats-so-significant) , cutting water access to more than 20,000 Iranians. The following day, the Handala threat group (linked to Iran's Ministry of Intelligence and Security) claimed it had breached water utility systems in several California cities as a retaliatory warning.


CyberAv3ngers' exploitation techniques have also proliferated to 60+ affiliated hacktivist groups that have adopted its playbook, coordinated through an “Electronic Operations Room” established at the start of the conflict. These proxy groups are replicating CyberAv3ngers' ICS exploitation techniques, creating a distributed threat that persists even if the core CyberAv3ngers group is degraded. This proliferation increases the risk of unintended physical consequences from operators who may lack the discipline or understanding to control the effects of PLC manipulation.


**What should organizations do?**


Organizations operating internet-exposed PLCs, particularly Rockwell Automation, Schneider Electric, and Siemens devices, can take the following steps:


1. **Disconnect PLCs from direct internet exposure.** Any internet-accessible Rockwell Logix controller is exploitable via CVE-2021-22681 without authentication, and there is no vendor patch. If remote access is operationally necessary, deploy a secure gateway with multifactor authentication. This is the single highest-impact remediation action.
2. **Set physical mode switches to "Run."** This prevents remote modification of PLC logic and configurations, blocking the primary impact technique documented in CISA Advisory AA26-097A.
3. **Ingest IOCs from the updated CISA Advisory AA26-097A.** Download the STIX-formatted indicators of compromise from the[July 22, 2026, advisory update](https://www.cisa.gov/news-events/cybersecurity-advisories/aa26-097a) and deploy them in SIEM, IDS, and firewall platforms. Monitor for traffic on ports 44818, 2222, 102, 22, and 502 from overseas hosting providers.
4. **Implement IT/OT network segmentation.** Isolate engineering workstations running Studio 5000 Logix Designer, EcoStruxure Control Expert, and TIA Portal from untrusted network segments. Deploy allowlisting so only authorized workstations can communicate with controllers.
5. **Audit cellular OT connections.** The Plymouth attack specifically targeted cellular-connected infrastructure. Verify that all cellular-connected PLCs are behind gateway firewalls with logging. Replace consumer cellular modems with industrial cellular gateways supporting VPN tunnels and multifactor authentication.
6. **Back up all PLC logic and configurations offline.** Store backups on secured physical media and test restore procedures. CyberAv3ngers' documented TTPs include PLC project file theft and ladder logic manipulation, making offline backups the last-resort recovery mechanism.


**Has Tenable released any product coverage for these vulnerabilities?**


A[Tenable plugin](https://www.tenable.com/plugins/ot/500451) is available for CVE-2021-22681, last updated June 2026. Tenable One OT Exposure detects this vulnerability in Rockwell Automation Logix controller environments.


A list of Tenable plugins for these vulnerabilities can be found on the individual CVE pages as they're released:


- [CVE-2021-22681](https://www.tenable.com/cve/CVE-2021-22681/plugins)
- [CVE-2023-3595](https://www.tenable.com/cve/CVE-2023-3595/plugins)
- [CVE-2024-6242](https://www.tenable.com/cve/CVE-2024-6242/plugins)


This link will display all available plugins for these vulnerabilities, including upcoming plugins in our[Plugins Pipeline](https://www.tenable.com/plugins/pipeline) .


Tenable customers can use[Tenable One Exposure Management Platform](https://www.tenable.com/products/tenable-one) to identify affected Rockwell Automation assets in their environment and prioritize remediation based on the active exploitation context documented in this post. Tenable One OT Exposure provides specialized visibility into industrial control system environments, including internet-exposed PLC detection and vulnerability assessment.


### Get more information


- [CISA Advisory AA26-097A: Iranian-Affiliated Cyber Actors Exploit PLCs Across US Critical Infrastructure](https://www.cisa.gov/news-events/cybersecurity-advisories/aa26-097a) (Updated July 22, 2026)
- [Minnesota IT Services: MNIT activates statewide cybersecurity response to support affected communities and protect critical infrastructure](https://mn.gov/mnit/media/blog/?id=761869)
- [StateScoop: Coordinated cyberattack disrupts water utilities in 30+ Minnesota communities](https://statescoop.com/coordinated-cyberattack-disrupts-water-utilities-in-30-minnesota-communities/)
- [CyberAv3ngers: FAQ About Iran-Linked Threat Group Targeting Critical Infrastructure](https://www.tenable.com/blog/what-to-know-about-cyberav3ngers-the-irgc-linked-group-targeting-critical-infrastructure)
- [CISA Advisory AA23-335A: IRGC-Affiliated Cyber Actors Exploit PLCs in Multiple Sectors](https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-335a) (Updated December 2024)
- [Rewards for Justice: CyberAv3ngers](https://rewardsforjustice.net/rewards/cyberav3ngers/)


***Join***[Tenable's Research Special Operations (RSO) Team](https://connect.tenable.com/category/news-you-need/discussions/vulnerability-watch) ***on Tenable Connect for further discussions on the latest cyber threats.***


***Learn more about***[Tenable One](https://www.tenable.com/products/tenable-one) ***, the Exposure Management Platform for the modern attack surface.***
