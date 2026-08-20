---
schema_version: "1.0.0"
document_id: "5d68008d010be114d5f70dd2899c9a67d2284bacea4fa37d3ee495d8666185e8"
company_key: "yc-swif-ai"
company: "Swif.ai"
source_id: "yc-swif-ai-news-import-789a3488158e"
canonical_url: "https://www.swif.ai/blog/hacking-statistics"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-07-22T15:23:23.396754+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:cb185e1f1145812cd74a74c6d79299562f47e7e82dd41248268a981bfbeecd7a"
---

# Hacking Statistics for 2026: Intrusion Tradecraft, Dwell Time, and the New Speed of Attack

Hacking has compressed. The average eCrime breakout time, the window between an attacker landing on one host and pivoting to a second, dropped to 29 minutes in 2025, with the fastest recorded breakout clocking in at 27 seconds, according to the[CrowdStrike 2026 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) . Initial access brokers are now handing fresh footholds to ransomware affiliates a median of 22 seconds after compromise, per[Mandiant M-Trends 2026](https://cloud.google.com/blog/topics/threat-intelligence/m-trends-2026) . Yet defenders rarely notice for two weeks. The same Mandiant report puts global median dwell time at 14 days for 2025, up from 11 the year before. These are the hacking statistics IT, security, and compliance teams need to know in 2026.


### **Key hacking statistics at a glance**


- **29 minutes** was the average eCrime breakout time in 2025, a 65% jump in attacker speed compared to 2024, per the[CrowdStrike 2026 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) .
- **27 seconds** was the fastest observed breakout time in 2025, per[CrowdStrike](https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-global-threat-report-findings/) .
- **22 seconds** was the median time between initial compromise and handoff to a secondary threat group in 2025, down from more than eight hours in 2022, per[Mandiant M-Trends 2026](https://cloud.google.com/blog/topics/threat-intelligence/m-trends-2026) .
- **14 days** was the global median attacker dwell time in 2025, per[Mandiant](https://cloud.google.com/blog/topics/threat-intelligence/m-trends-2026) .
- **82%** of detections CrowdStrike investigated in 2025 were malware-free, with adversaries relying on valid credentials and trusted access paths, per the[CrowdStrike 2026 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) .
- **20%** of breaches in the past year began with vulnerability exploitation, a 34% increase year over year and approaching the share started with stolen credentials (22%), per the[Verizon 2025 Data Breach Investigations Report](https://www.verizon.com/business/resources/reports/dbir/) .
- **32%** of Mandiant intrusions in 2025 began with an exploit, making vulnerability exploitation the leading initial infection vector for the sixth straight year, per[M-Trends 2026](https://cloud.google.com/blog/topics/threat-intelligence/m-trends-2026) .
- **7,000 password attacks per second** were observed on average against Microsoft customers, with 97% of identity attacks taking the form of password spray, per the[Microsoft Digital Defense Report 2025](https://www.microsoft.com/en-us/security/security-insider/threat-landscape/microsoft-digital-defense-report-2025) .
- **75** zero-day vulnerabilities were exploited in the wild in 2024, with 44% targeting enterprise security and networking products, per[Google Threat Intelligence Group](https://cloud.google.com/blog/topics/threat-intelligence/2024-zero-day-trends) .
- **$81 million** was paid out to ethical hackers through HackerOne bug bounty programs in the most recent 12-month period, per[HackerOne](https://www.hackerone.com/press-release/hackerone-report-finds-210-spike-ai-vulnerability-reports-amid-rise-ai-autonomy) .
- **$16.6 billion** in cybercrime losses were reported to the FBI in 2024, a 33% jump year over year across 859,532 complaints, per the[FBI IC3 2024 Internet Crime Report](https://www.ic3.gov/AnnualReport/Reports/2024_IC3Report.pdf) .


**168%** year-over-year increase in network-layer DDoS attacks tracked across Radware customers in 2025, fueled in large part by hacktivist activity, per the[Radware 2026 Global Threat Analysis Report](https://www.globenewswire.com/news-release/2026/02/19/3240861/0/en/Radware-2026-Global-Threat-Report-Shows-DDoS-Attacks-Jump-168-as-Cyber-Threats-Escalate-Across-Networks-and-Applications.html) .


‍


### **Breakout time and dwell time: defenders have minutes, not days**


Breakout time is the single most important number in modern intrusion analytics. It measures how quickly an attacker pivots from the initially compromised endpoint to a second system inside the same environment. Once that pivot happens, the intrusion is no longer a contained incident, it is a network-wide problem. According to the[CrowdStrike 2026 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) , the average eCrime breakout time fell to 29 minutes in 2025, a 65% acceleration from 48 minutes in 2024 and 62 minutes the year before. The fastest breakout CrowdStrike recorded was 27 seconds, faster than most monitoring tools can finish parsing an authentication event.


The other half of the equation is dwell time, the number of days between initial compromise and detection.[Mandiant M-Trends 2026](https://cloud.google.com/blog/topics/threat-intelligence/m-trends-2026) puts global median dwell time at 14 days for 2025, three days worse than 2024. The increase was driven primarily by a surge in cyber espionage and North Korean IT worker investigations, where median dwell time was 122 days. Some of those intrusions persisted undetected for over a year.


Detection source matters.[Mandiant](https://cloud.google.com/blog/topics/threat-intelligence/m-trends-2026) reports that the median dwell time was 26 days when an external party notified the victim, but only 10 days when the organization detected the activity itself, and just 5 days when the adversary made themselves known, usually through a ransomware note. Internal telemetry is still the single biggest accelerant on detection.


The window for hands-on-keyboard defenders has also collapsed.[Mandiant](https://www.helpnetsecurity.com/2026/03/24/mandiant-m-trends-2026-report/) found that initial access brokers are now handing off compromised networks to secondary groups, usually ransomware affiliates, a median of 22 seconds after the first foothold, down from more than eight hours just three years earlier. A division-of-labor model where one cluster gains access and another performs the impact appeared in 9% of 2025 investigations, up from 4% in 2022.


### **Top initial access vectors: exploits, credentials, and voice phishing**


Hackers have largely settled into three repeatable ways into a corporate network. Two of them have been around for years. One climbed sharply in 2025.


The[Verizon 2025 Data Breach Investigations Report](https://www.verizon.com/business/resources/reports/dbir/) analyzed roughly 12,000 confirmed breaches and found that the exploitation of vulnerabilities accounted for 20% of initial access in the past year, a 34% increase year over year and a hair below the 22% share attributed to stolen credentials. Vulnerability exploitation now sits within a couple of points of credential abuse as the leading intrusion technique, and for critical infrastructure it has already overtaken it.


Edge devices are the dominant target inside that growth. Per the[Verizon DBIR](https://www.verizon.com/business/resources/reports/dbir/) , edge devices and VPN appliances were involved in 22% of vulnerability-exploitation actions, up nearly eightfold from 3% in the prior year. Only 54% of those edge-device vulnerabilities were fully remediated over the full year of observation, and the median time to remediate was 32 days.


tells the same story from a different dataset. Across more than 500,000 hours of frontline incident response in 2025, exploitation of public-facing applications was the leading initial infection vector at 32% for the sixth consecutive year. The second-most common vector was a surprise: voice phishing, which appeared in 11% of investigations, climbing rapidly as Scattered Spider and similar groups industrialized help-desk impersonation.


Web application infrastructure is doing most of the heavy lifting in the credential category. Eighty-eight percent of basic web application attacks tracked by[Verizon](https://www.verizon.com/business/resources/reports/dbir/) involved the use of stolen credentials. Of the breaches that started with vulnerability exploitation, 42% used a web application as the path in. JSON Web Tokens, API keys, and passwords were among the most leaked credential types found in public repositories.


### **Hacking the identity layer: 7,000 password attacks per second**


Identity is where most hacks start. The[Microsoft Digital Defense Report 2025](https://www.microsoft.com/en-us/security/security-insider/threat-landscape/microsoft-digital-defense-report-2025) tracks 600 million attacks per day across Microsoft's telemetry and reports an average of more than 7,000 password attacks per second observed in 2024. Ninety-seven percent of those identity attacks were password spray, the technique of trying a single common password against thousands of accounts at once to evade lockout thresholds. The remaining few percent are the more dangerous categories: token theft, adversary-in-the-middle phishing, SIM swapping, and MFA fatigue.


Bots are now most of the attack surface. Out of 15.9 billion account creation requests Microsoft saw in the first half of 2025, more than 90% came from bots. Microsoft blocked roughly 2 million fake account signup requests per hour, per the[Microsoft Digital Defense Report 2025](https://www.microsoft.com/en-us/security/security-insider/threat-landscape/microsoft-digital-defense-report-2025) . The same dataset shows that about 45% of failed sign-ins had the right username but wrong password, evidence that usernames leak everywhere even when passwords do not.


Hacking is no longer something humans do at scale by hand. With AI-assisted spraying, credential stuffing infrastructure rentable for tens of dollars an hour, and large infostealer botnets feeding the front end, the identity perimeter sustains continuous, automated probing every minute of the day.


### **Nation-state hacking: China, North Korea, and the AI accelerant**


State-sponsored intrusions surged in 2025.[CrowdStrike](https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-global-threat-report-findings/) reported a 38% year-over-year increase in China-nexus intrusions across all sectors, with logistics targeting up 85%. North Korea-nexus incidents jumped 130%, fueled in part by FAMOUS CHOLLIMA, whose activity doubled, and STARDUST CHOLLIMA, whose operational tempo increased through 2025.


Sixty-seven percent of vulnerabilities exploited by China-nexus adversaries provided immediate system access, and 40% targeted edge devices that typically lack comprehensive monitoring, per[CrowdStrike](https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-global-threat-report-findings/) . The strategic pattern is consistent: compromise the network appliance, the VPN, or the firewall, then live off the land using legitimate credentials and trusted channels for months at a time.


Zero-day exploitation continues to skew toward state-aligned operators.[Google Threat Intelligence Group](https://cloud.google.com/blog/topics/threat-intelligence/2024-zero-day-trends) tracked 75 zero-day vulnerabilities exploited in the wild in 2024, with 44% targeting enterprise technologies including Ivanti Connect Secure, Cisco ASA, and Palo Alto PAN-OS. Eighteen unique enterprise vendors accounted for 90% of all targeted vendors. China-linked and North Korea-linked actors each exploited five zero-days, while customers of commercial surveillance vendors exploited another eight.


AI is now part of the offensive toolkit. CrowdStrike observed an 89% year-over-year increase in attacks by AI-enabled adversaries in 2025, per the[CrowdStrike 2026 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) . Threat actors used generative AI for social engineering lures, malware authoring, and reconnaissance. They also attacked the AI layer itself, injecting malicious prompts into legitimate GenAI tools at more than 90 organizations to steal credentials and cryptocurrency.


### **Hacktivism, DDoS, and the geopolitical hacking surge**


Hacktivism has shifted from sporadic protest activity to industrial-scale disruption. The[Radware 2026 Global Threat Analysis Report](https://www.globenewswire.com/news-release/2026/02/19/3240861/0/en/Radware-2026-Global-Threat-Report-Shows-DDoS-Attacks-Jump-168-as-Cyber-Threats-Escalate-Across-Networks-and-Applications.html) tracks a 168% year-over-year increase in network-layer DDoS attacks, with peak volumes approaching 30 Tbps. In the second half of 2025 alone, the average Radware customer absorbed more than 25,000 network-layer DDoS attacks, an average of 139 per day.


A single hacktivist collective drove an outsized share of activity. The pro-Russian group NoName057(16) claimed 4,693 attacks in 2025, the highest annual total ever recorded for a single hacktivist entity, per[Radware](https://www.globenewswire.com/news-release/2026/02/19/3240861/0/en/Radware-2026-Global-Threat-Report-Shows-DDoS-Attacks-Jump-168-as-Cyber-Threats-Escalate-Across-Networks-and-Applications.html) . Europe absorbed 48.4% of claimed attacks, with Israel, the United States, and Ukraine the three most-targeted countries individually. Government services were the most-targeted vertical, hit by 38.8% of claimed attacks.


The implication for security teams is that DDoS is no longer a niche concern handled by network engineering. It is a strategic intrusion adjacent threat, often used to mask data theft or extortion attempts running in parallel.


### **Ethical hacking: $81 million in bug bounties and a 210% AI vulnerability spike**


Not all hacking is criminal.[HackerOne](https://www.hackerone.com/press-release/hackerone-report-finds-210-spike-ai-vulnerability-reports-amid-rise-ai-autonomy) paid out $81 million in bug bounties across its platform between July 2024 and June 2025, a 13% year-over-year increase. The top 100 bug bounty programs accounted for $51 million of that total, and the top 10 alone for $21.6 million. Individual researchers are now consistently surpassing six-figure annual earnings, with the top 100 all-time earners pulling in $31.8 million combined.


The economics are tilting in defenders' favor. For every dollar spent on bounties,[HackerOne](https://www.hackerone.com/press-release/hackerone-report-finds-210-spike-ai-vulnerability-reports-amid-rise-ai-autonomy) estimates customers save an average of $15, totaling roughly $3 billion in mitigated financial losses from potential breaches over the program year. The platform received 85,000 valid bug bounty submissions in 2025, a 7% year-over-year increase.


AI is reshaping the discipline. The number of AI-related vulnerabilities submitted to[HackerOne](https://www.hackerone.com/press-release/hackerone-report-finds-210-spike-ai-vulnerability-reports-amid-rise-ai-autonomy) jumped more than 210% year over year, and prompt-injection vulnerabilities specifically surged 540%. Ethical hackers are increasingly hunting in two new territories: the prompts and pipelines of large language models, and the agentic AI workflows now being deployed in production at most large enterprises.


### **What hacking costs: $16.6 billion in U.S. losses alone**


Direct financial losses from hacking in the United States hit a new record in 2024. The[FBI IC3 2024 Internet Crime Report](https://www.ic3.gov/AnnualReport/Reports/2024_IC3Report.pdf) logged 859,532 complaints totaling $16.6 billion in reported losses, a 33% jump from 2023.


The complaint mix tells the story of where hacks land. Phishing and spoofing topped the list with 193,407 complaints, more than double the next most-reported category, extortion, per the[FBI IC3 report](https://www.ic3.gov/AnnualReport/Reports/2024_IC3Report.pdf) . Business email compromise was only the seventh-most-reported crime by complaint count but ranked second by dollar losses, at nearly $2.8 billion. Cumulative reported BEC losses for 2022 through 2024 came in just under $8.5 billion.


Ransomware remained the most pervasive cyber threat to critical infrastructure. The IC3 logged 3,156 ransomware complaints, a 9% year-over-year increase, and more than 4,800 complaints overall from organizations classified as critical infrastructure, per the[FBI](https://www.ic3.gov/AnnualReport/Reports/2024_IC3Report.pdf) . Older Americans were disproportionately hit; victims over the age of 60 reported nearly $5 billion in losses, the largest share of any age band.


### **What's new in 2026: malware-free intrusions and the disappearance of the attack chain**


The most consequential shift in hacking is the steady disappearance of malware as the primary artifact of intrusion. Eighty-two percent of detections CrowdStrike investigated in 2025 were malware-free, per the[CrowdStrike 2026 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) . Attackers prefer valid credentials, approved SaaS integrations, trusted identity flows, and inherited software supply chains. The intrusion moves through authorized pathways, where it blends into normal activity and never trips an antivirus signature.


Cloud-conscious intrusions, where attackers explicitly target cloud-resident assets rather than treating them as collateral, climbed 37% in 2025, per[CrowdStrike](https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-global-threat-report-findings/) . State-nexus actors drove the trend with a 266% increase. Valid account abuse accounted for 35% of cloud incidents, reinforcing the identity perimeter as the new domain controller.


Social engineering also evolved fast. Fake CAPTCHA lures, which trick users into running PowerShell commands they paste from a phony verification page, drove a 563% year-over-year increase in incidents, per[CrowdStrike](https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-global-threat-report-findings/) . Voice phishing climbed to the second-most-common initial infection vector tracked by[Mandiant](https://cloud.google.com/blog/topics/threat-intelligence/m-trends-2026) , appearing in 11% of investigations.


Third-party involvement in breaches doubled. Per the[Verizon 2025 DBIR](https://www.verizon.com/business/resources/reports/dbir/) , 30% of breaches now involve a third party in some way, up from 15% the year before. Supply chain compromise, contractor access, and managed-service provider intrusions are no longer edge cases; they are routine.


CrowdStrike's zero-day data underscores the shift in initial-access economics. The firm observed a 42% year-over-year increase in zero-days exploited prior to public disclosure in 2025, per[CrowdStrike](https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-global-threat-report-findings/) . Buying or developing a working zero-day is now within reach of well-funded eCrime operators, not just nation-states.


For broader context on the trends above, see our[cyber attack statistics](https://www.swif.ai/blog/cyber-attack-statistics) and[password statistics](https://www.swif.ai/blog/password-statistics) roundups.


### **How swif.ai helps**


swif.ai gives IT and security teams a single console to enforce device, identity, and compliance controls across the macOS, Windows, and Linux endpoints behind the numbers above. Explore[swif.ai mobile device management](https://www.swif.ai/products/mobile-device-management-mdm) to see how it works.
