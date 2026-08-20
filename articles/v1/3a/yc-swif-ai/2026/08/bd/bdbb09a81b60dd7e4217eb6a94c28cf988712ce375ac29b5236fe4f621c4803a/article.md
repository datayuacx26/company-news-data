---
schema_version: "1.0.0"
document_id: "bdbb09a81b60dd7e4217eb6a94c28cf988712ce375ac29b5236fe4f621c4803a"
company_key: "yc-swif-ai"
company: "Swif.ai"
source_id: "yc-swif-ai-news-import-789a3488158e"
canonical_url: "https://www.swif.ai/blog/malware-statistics"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T10:00:33.682122+00:00"
fetched_at: "2026-08-05T10:00:35.732088+00:00"
content_hash: "sha256:71e0e03d29118c0a6cf0e3bb7fe1a41c71cc58f87f2ea90ef40e28ec594300f5"
---

# Malware Statistics for 2026: Volumes, Variants, and the Quiet Shift to Identity Attacks

Malware is everywhere and nowhere at the same time. Endpoint trackers still record more than 500,000 new malicious files and potentially unwanted applications hitting their honeypots every day, and the cumulative count of known malware samples has now passed 1.5 billion. Yet on the corporate front lines, 82% of detections in 2025 were malware-free, according to the[CrowdStrike 2026 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) . Attackers increasingly skip custom payloads in favor of stolen credentials, signed binaries, and SaaS tokens harvested by industrial-scale infostealers. IBM observed more than 16 million devices infected by infostealers like Lumma, Acreed, and Vidar last year, per the[IBM 2026 X-Force Threat Intelligence Index](https://newsroom.ibm.com/2026-02-25-ibm-2026-x-force-threat-index-ai-driven-attacks-are-escalating-as-basic-security-gaps-leave-enterprises-exposed) . These are the malware statistics that matter for IT, security, and compliance teams heading into 2026.


### **Key malware statistics at a glance**


- **6.06 billion** malware attacks were recorded in the most recent full-year SonicWall dataset, the highest volume since 2019 and up 11% year over year, per the[SonicWall 2025 Cyber Threat Report](https://www.sonicwall.com/resources/white-papers/2025-sonicwall-cyber-threat-report) .
- **210,258** "never-before-seen" malware variants were identified across SonicWall sensors in 2024, an average of 637 brand-new strains per day, per[SonicWall](https://www.sonicwall.com/resources/white-papers/2025-sonicwall-cyber-threat-report) .
- **16 million+** devices were infected by infostealer malware (Lumma, Acreed, Vidar) in 2025, per the[IBM 2026 X-Force Threat Intelligence Index](https://newsroom.ibm.com/2026-02-25-ibm-2026-x-force-threat-index-ai-driven-attacks-are-escalating-as-basic-security-gaps-leave-enterprises-exposed) .
- **82%** of detections in 2025 were malware-free, with attackers using valid credentials and trusted identity flows instead of custom binaries, per the[CrowdStrike 2026 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) .
- **44%** year-over-year increase in exploitation of public-facing applications, now a leading initial-access cause, per[IBM](https://newsroom.ibm.com/2026-02-25-ibm-2026-x-force-threat-index-ai-driven-attacks-are-escalating-as-basic-security-gaps-leave-enterprises-exposed) .
- **56%** rise in Android banking trojan attacks in 2025, with 255,090 new banker installation packages (a 271% jump), per[Kaspersky](https://www.kaspersky.com/about/press-releases/the-number-of-trojan-banker-attacks-on-smartphones-increased-by-56-in-2025) .
- **40%** of all macOS protection updates Sophos shipped in 2025 were tied to a single malware family, AMOS (Atomic macOS Stealer), per[Sophos](https://www.sophos.com/en-us/blog/why-amos-matters-the-macos-malware-stealing-data-at-scale) .


**29 minutes** average eCrime breakout time in 2025, down from 48 minutes the prior year, with the fastest measured at 27 seconds, per[CrowdStrike](https://www.crowdstrike.com/en-us/global-threat-report/) .


‍


### **Global malware volume and new-variant production**


The headline number that journalists most often cite for the scale of the malware problem is malware attack volume. The[SonicWall 2025 Cyber Threat Report](https://www.sonicwall.com/resources/white-papers/2025-sonicwall-cyber-threat-report) recorded 6.06 billion malware attacks across its global sensor network in the most recent full year, up 11% year over year and the highest figure since 2019. Malware then climbed another 8% in 2024 with a particularly sharp 92% spike in May 2025.


Volume tells only part of the story. SonicWall flagged 210,258 "never-before-seen" malware variants in 2024, which works out to roughly 637 brand-new strains every day. Independent endpoint trackers see even more once they count potentially unwanted applications: industry telemetry consistently places daily new sample submissions in the 450,000 to 560,000 range, and cumulative known-sample counts have crossed 1.5 billion. The takeaway is that defenders cannot rely on signature lists alone. Most new strains live for only days or weeks, which makes signature-only antivirus a losing game.


IoT made the biggest relative jump.[SonicWall](https://www.sonicwall.com/resources/white-papers/2025-sonicwall-cyber-threat-report) recorded a 124% year-over-year increase in IoT malware attacks, and encrypted threats climbed 93%. Roughly 45% of malware now hides inside SSL/TLS traffic, which means organizations without TLS inspection are flying blind on nearly half the malicious flows hitting their networks.


### **Infostealers are now the dominant malware category**


If you read any 2026 threat report, infostealers will be the first malware family discussed.[IBM X-Force](https://www.ibm.com/think/x-force/threat-intelligence-index-2026-securing-identities-ai-detection-risk-management) counted more than 16 million devices infected by infostealers in 2025, with Lumma, Acreed, and Vidar leading the pack. Those infections feed an industrial credential market. IBM reported that over 300,000 ChatGPT credential sets were advertised on the dark web in 2025, sold alongside browser passwords, session cookies, and API tokens that go for as little as $10 per victim on marketplaces like Russian Market.


Microsoft confirms the pattern. The[Microsoft Digital Defense Report 2025](https://www.microsoft.com/en-us/security/security-insider/threat-landscape/microsoft-digital-defense-report-2025) identifies Lumma Stealer as the most prevalent infostealer observed between October 2024 and October 2025. Microsoft describes a fully specialized cybercrime supply chain: infostealer operators harvest credentials and browser session tokens, then sell them to initial-access brokers, who in turn sell to ransomware and extortion crews.


ESET telemetry tells the same story from a different vantage point. The[ESET H1 2025 Threat Report](https://www.welivesecurity.com/en/eset-research/eset-threat-report-h1-2025/) shows SnakeStealer overtaking Agent Tesla as the most-detected infostealer globally, with almost one-fifth of all infostealer detections worldwide attributed to that single family. ESET researchers also contributed to the international takedowns of Lumma Stealer and Danabot in mid-2025, and to the late-2024 disruption of Redline and Meta Stealer. Even after those operations, new families like Acreed and DigitStealer filled the gap within weeks.


### **The rise of malware-free attacks**


One of the most consequential numbers in any 2026 threat report comes from[CrowdStrike's 2026 Global Threat Report](https://www.crowdstrike.com/en-us/global-threat-report/) : 82% of detections in 2025 were malware-free, up from 79% the prior year. Instead of dropping custom binaries, adversaries log in with valid credentials, abuse OAuth tokens, and live off legitimate cloud and SaaS tools. CrowdStrike also recorded a 37% increase in cloud-based intrusions, with valid-account abuse making up 35% of the cloud intrusions it observed.


Average eCrime breakout time, the window between initial access and lateral movement, fell to 29 minutes in 2025, down from 48 minutes the prior year. The fastest observed breakout was 27 seconds. Combined with a 130% surge in North Korea-linked intrusions and a 38% rise in China-linked intrusions, the data argues for behavior-based detection over file-based defenses.


IBM frames the same shift through a vulnerability lens. The[2026 X-Force Threat Intelligence Index](https://www.ibm.com/think/x-force/threat-intelligence-index-2026-securing-identities-ai-detection-risk-management) recorded a 44% year-over-year increase in exploitation of publicly facing applications, now one of the leading initial-access vectors. Supply chain compromise and identity attacks were the other two top causes. The pattern is consistent across IBM, Microsoft, and CrowdStrike: attackers prefer identity and code execution paths that look like normal business activity, with custom malware reserved for the moments when nothing else works.


### **How malware actually gets in**


The[Verizon 2025 Data Breach Investigations Report](https://www.verizon.com/business/resources/reports/dbir/) offers the cleanest read on delivery vectors. Roughly 60% of breaches involved a human element, whether through a malicious click, social engineering, or misdelivery. Credentials remained the leading initial-access category, and ransomware (a downstream malware outcome) appeared in 44% of breaches, up from 32% the prior year. Verizon also found that 8% of employees account for 80% of incidents, which means targeted user-segment training has outsized leverage.


Email remains the dominant delivery surface. Roughly 41% of malware reaches victims via attachment or link in email, with maliciously altered websites contributing another 23% and software vulnerabilities 17%. Removable media accounts for 9% and supply-chain compromise 7%. New social engineering twists have inflated those email numbers further: telemetry from[Hornetsecurity's 2026 Cybersecurity Report](https://www.hornetsecurity.com/en/blog/cybersecurity-report-2026-press-release/) shows a 131% increase in malware-laden emails year over year, plus a 34.7% rise in email scams and a 21% rise in phishing attempts.


Fake-CAPTCHA "ClickFix" lures grew 563% during the same period, prompting users to paste attacker-supplied commands into their own terminals. Sophos researchers observed the same pattern delivering[AMOS macOS stealers](https://www.sophos.com/en-us/blog/why-amos-matters-the-macos-malware-stealing-data-at-scale) through cracked-software downloads and ClickFix-style verification prompts.


### **Mobile malware and the banking trojan surge**


Mobile devices are now a primary target.[Kaspersky's Securelist team](https://securelist.com/mobile-threat-report-2025/119076/) blocked 14,059,465 mobile attacks in 2025, an average of roughly 1.17 million per month, and detected over 815,000 malicious installation packages across Android and iOS, including 255,000 mobile banking trojans.


Adware accounts for the largest share of mobile detections, at 62% of all blocked threats, led by the MobiDash (39%), Adlo (27%), and HiddenAd (20%) families. Banking trojans are the fastest-growing category.[Kaspersky](https://www.kaspersky.com/about/press-releases/the-number-of-trojan-banker-attacks-on-smartphones-increased-by-56-in-2025) reported a 56% rise in Android banking trojan attacks in 2025 and a 271% jump in new banker installation packages, with the Mamont family alone responsible for almost half (49.8%) of all detections in that category.


Bring-your-own-device practices amplify the exposure. SentinelOne polling in early 2026 found that 20% of companies experienced a malware outbreak on a device IT did not monitor, and 50% admitted they could not tell whether an outbreak had happened on an unmanaged device. For organizations that have not centralized mobile policy, modern mobile device management is no longer optional.


### **Malware by operating system: Windows, macOS, Linux**


Windows is still the largest malware target by raw volume. Industry telemetry shows the cumulative Windows malware sample count has grown roughly 8% year over year, with the live Windows-only corpus now in the 900-million-to-1-billion sample range. Trojans and file infections account for around 70% of total malware detections across endpoints.


macOS has shed its "safer by default" reputation.[Sophos reports](https://www.sophos.com/en-us/blog/why-amos-matters-the-macos-malware-stealing-data-at-scale) that AMOS (Atomic macOS Stealer) was tied to nearly 40% of all macOS protection updates Sophos shipped in 2025, with detections spiking 300% in August 2025 alone. AMOS does not rely on zero-days; it tricks users into pasting Terminal commands that harvest the Keychain, browser credentials, and session tokens. Almost half of all Sophos customer reports of macOS stealers in the back half of 2025 involved AMOS or close variants, and infostealer attacks on Macs grew over 17% year over year.


Linux malware is a smaller piece of the pie by volume, but punches above its weight on impact because of where Linux runs (servers, cloud workloads, edge appliances). Independent threat reports increasingly cite Linux-focused families like Cloud Snooper, HiatusRAT, and Linux variants of well-known Windows ransomware. We cover Linux-specific malware separately in a dedicated article.


### **Emerging trends and what is new in 2026**


**AI-generated malware and lures.** Per[IBM's 2026 X-Force](https://newsroom.ibm.com/2026-02-25-ibm-2026-x-force-threat-index-ai-driven-attacks-are-escalating-as-basic-security-gaps-leave-enterprises-exposed) , cybercriminals are using AI tools to identify weaknesses and generate samples faster than ever. Roughly 37% of new malware samples now use AI-enhanced techniques to evade detection, and AI-generated phishing lures have lifted click-through rates by up to 54% in recent vendor telemetry.


**Malware-as-a-service is now the norm.** Lumma, AsyncRAT, XWorm, and the post-takedown Redline successors are all distributed via subscription on Russian-language and Telegram-based markets.[Microsoft](https://www.microsoft.com/en-us/security/security-insider/threat-landscape/microsoft-digital-defense-report-2025) describes the modern cybercrime economy as a "specialized supply chain" where infostealer operators, access brokers, and ransomware crews each focus on their own slice. Operationally, this means new families ship faster, scale wider, and recover from takedowns within weeks.


**Polymorphic and fileless malware.** Roughly 90% of detected malware now uses some form of polymorphism to change its code or signature, and fileless techniques account for more than 70% of the serious malware incidents tracked by major incident-response firms. Both trends reward behavior-based EDR over signature-matching antivirus.


**Cloud and SaaS as the new front line.**[CrowdStrike](https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-global-threat-report-findings/) recorded a 266% increase in cloud intrusions by state-linked actors year over year, and Microsoft observed that token theft, OAuth abuse, and "living off the cloud" tactics have largely supplanted classic on-host malware in cloud-native breaches. The[Microsoft Digital Defense Report 2025](https://blogs.microsoft.com/on-the-issues/2025/10/16/mddr-2025/) notes that extortion and ransomware now drive more than half of cyberattacks, even though only a minority of those incidents involve traditional malware files.


**Public sector and critical infrastructure under sustained pressure.** Government and public sector entities accounted for 19% of global malware incidents in 2026 telemetry, with year-over-year ransomware incident growth of 65% in those sectors. Manufacturing is the single most-targeted vertical, absorbing 34.7% of malware incidents, with ransomware specifically halting production lines in 31% of confirmed cases.


For broader context on the trends above, see our[ransomware statistics](https://www.swif.ai/blog/ransomware-statistics) and[phishing statistics](https://www.swif.ai/blog/phishing-statistics) roundups.


### **How swif.ai helps IT teams reduce malware exposure**


swif.ai gives IT and security teams a single console to enforce device, identity, and compliance controls across the macOS, Windows, and Linux endpoints behind the numbers above. Explore[swif.ai unified endpoint management](https://www.swif.ai/products/unified-endpoint-management-uem) to see how it works.


If you are looking for more detailed[Cybersecurity statistics](https://www.swif.ai/blog/cybersecurity-statistics) , check out our detailed article here.
