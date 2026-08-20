---
schema_version: "1.0.0"
document_id: "60be9635663e74b38eea5a6b3a6b5c0842f1518a95c06ca793a2ce6fb8d82954"
company_key: "ryan-specialty-holdings-inc-class-a-common-stock"
company: "Ryan Specialty Holdings Inc."
source_id: "ryan-specialty-holdings-inc-class-a-common-stock-rss-d8a5ac55d317"
canonical_url: "https://blog.ryanspecialty.com/wiper-vs-ransomware"
published_at: "2026-06-29T20:44:17+00:00"
first_seen_at: "2026-07-20T03:33:08.775828+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:5c47e10eabecc2698130933ddec075e2a5cde0ee7c349a4b31685816b4c99735"
---

# Wiper vs. Ransomware: Same Policy, Completely Different Claim

James Mula


Assistant Vice President


I Cyber & Technology E&O
RT ProExec


Ransomware has dominated the cyber insurance conversation for years. It is the loss scenario carriers underwrite to, the risk policyholders fear most, and the event brokers build coverage around. But the March 2026 Iran-linked cyberattacks introduced a different kind of destructive event at Fortune 500 scale: the wiper attack. No ransom demand. No decryption key. No negotiation. Just destruction.


For cyber insurance policyholders, the distinction between a ransomware event and a wiper event is not academic. The same policy can respond very differently depending on which type of attack occurs. Different first party coverage triggers activate, different sublimits may apply, and certain coverage grants that are central to a ransomware response may be entirely irrelevant in a wiper scenario.


This article is a companion to RT ProExec’s recent publications on the Iran-linked cyberattacks, including “Iran, Cyber War, and Your Policy” (analyzing war exclusion frameworks) and “When the Management Tool Becomes the Weapon” (analyzing MDM weaponization and triggering event definitions). This piece addresses a third coverage question: when the attack is destructive rather than extortive, how does the first-party coverage architecture of a cyber policy respond?


1. Two Attacks, Side by Side


To understand how the coverage differs, it helps to see what actually happens in each scenario.


Ransomware


The attacker gains access to the insured’s network, typically through phishing, credential theft, or exploitation of a vulnerability. Once inside, they deploy encryption malware across servers, workstations, and sometimes backups. The insured’s data is locked but still exists, it is encrypted, not deleted. The attacker delivers a ransom note demanding payment, usually in cryptocurrency, in exchange for a decryption key. The insured faces a decision: pay the ransom and hope the decryption key works or refuse and attempt to restore from backups. Either way, the organization experiences business interruption during the period of encryption and restoration.


Wiper


The attacker gains access to the insured’s network or, as seen in the March 2026 incident, to a cloud-based management platform. Instead of encrypting data, the attacker destroys it. Devices are factory reset, hard drives are overwritten, servers are wiped. There is no ransom note because the attacker’s objective is destruction, not payment. The data does not exist in an encrypted state waiting to be unlocked, it is gone. Recovery depends entirely on the integrity and availability of offline backups. If backups were also compromised or connected to the wiped environment, the data loss may be permanent.


Both attacks cause business interruption. Both require forensic investigation. Both may trigger notification obligations. But the recovery path and the coverage triggers are fundamentally different.


2. How the Policy Responds Differently


The following table compares how key first-party coverage components typically respond to a ransomware event versus a wiper event. The specifics will vary by carrier and policy form, but the structural differences are consistent across most standalone cyber policies.


3. Key Implications for Policyholders


Extortion-Heavy Policies May Underperform


Many cyber policies are structured with significant capacity allocated to the extortion tower high sublimit for ransom payments, dedicated negotiation and response services, and pre-arranged relationships with ransomware response vendors. In a wiper scenario, that entire coverage architecture sits unused. If the policy’s data recovery, business interruption, and hardware replacement sublimit are lower than its extortion sublimit, the policyholder may find that the coverage available for the actual loss is substantially less than the policy’s headline limit would suggest.


Data Recovery Becomes the Central Battleground


In a ransomware event, the fastest path to recovery is often paying the ransom and obtaining the decryption key. Data recovery from backups is the fallback plan. In a wiper event, data recovery from backups is the only plan. The cost, complexity, and duration of that recovery particularly if backups were also compromised can be significantly greater than the cost of decrypting systems. Policies with standalone data recovery and restoration coverage are typically better positioned to respond. In contrast, where data recovery is treated as a sub‑component of extortion coverage, the scope of recovery may be more constrained.


Business Interruption May Run Longer


Ransomware incidents can often be resolved in days or weeks once a decryption key is obtained or backups are deployed. A wiper event that destroys hundreds of thousands of devices may take significantly longer to remediate. Reimaging laptops, reconfiguring servers, re-enrolling mobile devices, and recreating lost data extends the business interruption period and the associated BI claim well beyond what a typical ransomware event would produce. Policyholders should understand their BI waiting period, the period of restoration definition, and any aggregate sublimit that could cap recovery.


Bricking Coverage Is No Longer Optional


When ransomware encrypts a device, the hardware is generally fine once it is decrypted or reimaged. When a wiper factory resets a device, the hardware may need to be physically recovered, reconfigured, or replaced. For organizations with large device fleets, this cost can be material. Carriers that include affirmative bricking or hardware replacement coverage provide a meaningful advantage in a wiper scenario. Carriers that exclude it or that limit first-party coverage to data restoration only may leave a significant portion of the loss uncovered.


Forensic Challenges Compound the Claim


In a ransomware event, the encrypted systems often preserve forensic evidence — investigators can analyze access logs, lateral movement, and exfiltration activity on the locked systems once they are decrypted or imaged. In a wiper event, the destruction of systems may also destroy the forensic evidence needed to answer critical questions: What data was accessed? What was exfiltrated? How did the attacker gain access? This can complicate both the forensic investigation and the notification analysis, potentially increasing costs and extending the claims process.


4. Broker Considerations


The shift from ransomware to wiper attacks introduces coverage considerations that may warrant review in connection with current and upcoming cyber placements:


1.


Review Sublimit Architecture:


Compare the sublimit for extortion, data recovery, business interruption, and hardware replacement. If the policy allocates significantly more capacity to extortion than to the other components, the coverage may be misaligned with the risk profile of a wiper event.


2.


Confirm Standalone Data Recovery:


Determine whether Data Recovery / Restoration is triggered independently or primarily in connection with extortion-related events. Policies that treat data recovery as a standalone coverage grant are better positioned for wiper scenarios.


3.


Evaluate Bricking Coverage:


Determine whether the policy affirmatively addresses the cost of restoring, reimaging, or replacing devices that have been factory reset or rendered inoperable. This coverage component can materially affect how the policy responds in a loss scenario.


4.


Assess the BI Period of Restoration:


Review how the policy defines the period of restoration for business interruption coverage. Wiper events may produce longer restoration periods than ransomware events, and policies with narrow restoration definitions or short time limits could cap recovery.


5.


Consider Forensic Evidence Destruction:


For clients in regulated industries with notification obligations, discuss the scenario where a wiper attack destroys the forensic evidence needed to determine what data was exfiltrated. The costs of conducting a notification analysis without intact forensic evidence can be substantial.


Conclusion


The cyber insurance market has spent years calibrating its products around ransomware. Extortion coverage, negotiation services, decryption support, and ransomware-specific incident response have become standard features. That calibration served the market well when ransomware was the dominant destructive threat.


The March 2026 wiper attack demonstrated that the threat landscape has expanded. State-aligned actors with geopolitical objectives are not seeking payment — they are seeking destruction. A single coverage approach may not address both scenarios, as the frameworks that respond to ransomware events do not necessarily apply to wiper‑type incidents in the same way.


Retail brokers who understand this distinction and can evaluate their clients’ policies through both lenses — extortion and destruction — are better positioned to add value as the threat environment evolves. The question is no longer just “are you covered for ransomware?” It is “are you covered for what comes after ransomware?”


For further analysis of the broader insurance implications of the Iran-linked cyberattacks, see RT ProExec’s companion articles: “Iran, Cyber War, and Your Policy: War Exclusions, Coverage Implications, and What Policyholders Need to Know Now” and “When the Management Tool Becomes the Weapon: MDM Weaponization and the Cyber Coverage Question.”


Sources and Citations


*The factual assertions in this article are based on the following publicly available sources and the author’s analysis of cyber insurance coverage structures.*


Krebs, Brian. “Iran-Backed Hackers Claim Wiper Attack on Medtech Firm.” KrebsOnSecurity. March 12, 2026.[https://krebsonsecurity.com/2026/03/iran-backed-hackers-claim-wiper-attack-on-medtech-firm-stryker/](https://krebsonsecurity.com/2026/03/iran-backed-hackers-claim-wiper-attack-on-medtech-firm-stryker/)


Cimpanu, Catalin. “Medtech Giant Offline After Iran-Linked Wiper Malware Attack.” BleepingComputer. March 11, 2026.[https://www.bleepingcomputer.com/news/security/medtech-giant-stryker-offline-after-iran-linked-wiper-malware-attack/](https://www.bleepingcomputer.com/news/security/medtech-giant-stryker-offline-after-iran-linked-wiper-malware-attack/)


Collier, Kevin. “Iran Appears to Have Conducted a Significant Cyberattack Against a U.S. Company.” NBC News. March 11, 2026.[https://www.nbcnews.com/world/iran/iran-appears-conducted-significant-cyberattack-us-company-first-war-st-rcna263084](https://www.nbcnews.com/world/iran/iran-appears-conducted-significant-cyberattack-us-company-first-war-st-rcna263084)


Kovacs, Eduard. “Iran-Linked Hacker Attack Disrupted Manufacturing and Shipping.” SecurityWeek. March 13, 2026.[https://www.securityweek.com/iran-linked-hacker-attack-on-stryker-disrupted-manufacturing-and-shipping/](https://www.securityweek.com/iran-linked-hacker-attack-on-stryker-disrupted-manufacturing-and-shipping/)


“The Attack: Enterprise Resiliency Plans Can’t Ignore UEM.” Forrester Research. March 2026.[https://www.forrester.com/blogs/the-stryker-attack-enterprise-resiliency-plans-cant-ignore-uem/](https://www.forrester.com/blogs/the-stryker-attack-enterprise-resiliency-plans-cant-ignore-uem/)


“Stryker Attack Raises Concerns About Role of Device Management Tool.” Cybersecurity Dive. March 2026.[https://www.cybersecuritydive.com/news/stryker-attack-device-management-microsoft-iran/814816/](https://www.cybersecuritydive.com/news/stryker-attack-device-management-microsoft-iran/814816/)


Otto, Emily. “Shamoon to Stryker: Iran Wields Wiper Attacks.” Center for European Policy Analysis (CEPA). March 16, 2026.[https://cepa.org/article/shamoon-strikes-stryker-iran-wields-wiper-attacks/](https://cepa.org/article/shamoon-strikes-stryker-iran-wields-wiper-attacks/)


Zetter, Kim. “Iranian Hacktivists Strike Medical Device Maker in ‘Severe’ Attack that Wiped Systems.” Zero Day (Substack). March 11, 2026.[https://www.zetter-zeroday.com/iranian-hacktivists-strike-medical-device-maker-stryker-in-severe-attack-that-wipedsystems/](https://www.zetter-zeroday.com/iranian-hacktivists-strike-medical-device-maker-stryker-in-severe-attack-that-wipedsystems/)


Palo Alto Networks, Unit 42. “Threat Brief: March 2026 Escalation of Cyber Risk Related to Iran.” March 2026.[https://unit42.paloaltonetworks.com/iranian-cyberattacks-2026/](https://unit42.paloaltonetworks.com/iranian-cyberattacks-2026/)


Mula, James. “Iran, Cyber War, and Your Policy: War Exclusions, Coverage Implications, and What Policyholders Need to Know Now.” RT ProExec. March 2026.[https://blog.ryanspecialty.com/iran-cyber-war-and-your-policy](https://blog.ryanspecialty.com/iran-cyber-war-and-your-policy)


This article is for general information purposes only and does not constitute legal or professional advice. No warranties, promises, and/or representations of any kind, express or implied, are given as to the accuracy, completeness, or timeliness of the information provided in this article. Every insured’s circumstances differ, and coverage needs and priorities vary based on an insured’s unique risk profile and operations. Whether a loss is covered by insurance depends on the specific facts of the loss and the terms and conditions of the actual insurance policy or policies involved. References to typical coverage provisions or market approaches are illustrative only and may not apply to a particular policy or situation. No user should act on the basis of any material contained herein without obtaining proper legal or other professional advice specific to their situation.


RT ProExec is a part of the RT Specialty division of RSG Specialty, LLC, a Delaware limited liability company based in Illinois. RSG Specialty, LLC, is a subsidiary of Ryan Specialty, LLC. RT ProExec provides wholesale insurance brokerage and other services to agents and brokers. RT ProExec does not solicit insurance from the public. Some products may only be available in certain states, and some products may only be available from surplus lines insurers. In California: RSG Specialty Insurance Services, LLC (License #0G97516). ©2026 Ryan Specialty, LLC
