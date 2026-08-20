---
schema_version: "1.0.0"
document_id: "bfe43b80d795337c76a46b752669355a037bee40313adbd090fd8470dff65ee1"
company_key: "yc-cyble"
company: "Cyble"
source_id: "yc-cyble-rss-726d4f5fb2f2"
canonical_url: "https://cyble.com/blog/ransomware-threats-in-america-h1-2026/"
published_at: "2026-08-14T14:22:51+00:00"
first_seen_at: "2026-08-14T15:17:33.128748+00:00"
fetched_at: "2026-08-14T15:17:33.759250+00:00"
content_hash: "sha256:8f1ca599f10fc3f93fe4d37d0332fa8c7f90810d231e2f5fcc4f8cb5c006b1c5"
---

# Ransomware Threats in the Americas H1 2026: Dissecting the Regional Attack Patterns and Dominant Actors

Link copied!


[Ransomware Insights](https://cyble.com/blog/category/ransomware-insights/)[Dark Web Monitoring](https://cyble.com/blog/category/dark-web-monitoring/)[Ransomware](https://cyble.com/blog/category/ransomware/)[Threat Actor](https://cyble.com/blog/category/threat-actor/)


# Ransomware Threats in the Americas H1 2026: Dissecting the Regional Attack Patterns and Dominant Actors


The Americas carried the heaviest ransomware burden of any region on the planet in the first half of 2026 with 2,188 attacks.


August 14, 2026 · 22 min read


The Americas carried the heaviest ransomware burden of any region on the planet in the first half of 2026. According to Cyble Research and Intelligence Labs (CRIL), North and South America combined experienced **2,188 documented ransomware attacks** between January and June 2026.


That single figure — 2,188 attacks — represents **more than 57% of the 3,836 ransomware incidents CRIL tracked worldwide** , making the Americas the undisputed center of gravity for global ransomware operations.


But the Americas is not a single threat theatre — it is two. North America alone absorbed **1,981 attacks** , driven by a mature, multi-group Ransomware-as-a-Service (RaaS) economy competing for market share. South America, by contrast, recorded **207 attacks** concentrated around a much smaller set of operators, with one group — The Gentlemen — claiming nearly a quarter of all regional incidents outright. Understanding the Americas means understanding both halves of that story: a saturated northern market and a consolidating southern one.


## North America vs. South America: Two Distinct Ransomware Landscapes


Security leaders operating across the hemisphere cannot apply a single threat model to both sub-regions. The data shows meaningfully different attacker behavior, concentration, and monetization strategy north and south of the equator.


**Metric** **North America** **South America**


**Ransomware Attacks** 1,981 207


**Dominant Ransomware Actor** Qilin (370 attacks) The Gentlemen (46 attacks)


**Top Targeted Sector** Construction IT & ITES


**Top Targeted Nation** United States (1,721) Brazil (71)


**Distinct Ransomware Groups Active** 50+ 30+


**% of Attacks from Top 3 Groups** ~40% (Qilin, Akira, INC Ransom) ~57.5% (The Gentlemen, Qilin, LockBit)


**Why the split matters:** North America’s threat landscape is a genuine marketplace — dozens of RaaS operators compete for affiliate loyalty, and no single group commands more than a fifth of total volume. South America’s landscape is more consolidated, with three groups controlling well over half of all attacks.


For defenders, that means North American organizations need broad-spectrum threat intelligence covering a long tail of active groups, while South American organizations can build highly specific defenses against a short list of named adversaries.


## The Five Dominant Ransomware Groups Targeting Americas


Across both sub-regions combined, five ransomware operators account for the overwhelming share of documented activity: **Qilin, Akira, INC Ransom, Dragonforce, and The Gentlemen** . Together, these five groups are linked to roughly **1,148 of the Americas’ 2,188 attacks — approximately 52.5% of all regional ransomware activity.**


### **1. Qilin:** The Biggest Ransomware Threat in the Americas


**Attack Volume:** 410 documented incidents across the Americas (370 in North America, 40 in South America) — 18.7% of the regional total.


Qilin is the single most prolific ransomware actor operating in the hemisphere, and its dominance is not evenly spread — it is concentrated hardest in the United States.


**Geographic Concentration:**


- **United States:** 323 attacks (the single largest country-level concentration of any group, anywhere)
- **Canada:** 33 attacks
- **Argentina:** 13 attacks
- **Broader South America:** 40 attacks


**Worldwide Sectoral Targeting:** Qilin’s targeting logic is deliberate rather than opportunistic:


- **Construction:** 108 incidents (primary focus)
- **Professional Services:** 90 incidents (legal, accounting, consulting firms)
- **Manufacturing:** 67 incidents
- **Healthcare:** 53 incidents
- **IT & ITES:** 43 incidents


**Operational Characteristics:**


Qilin’s affiliate model is built for scale. Initial access brokers handle reconnaissance and compromise, mid-tier operators manage lateral movement, and dedicated crews execute encryption and exfiltration. This compartmentalization lets Qilin run dozens of concurrent operations across the United States without any single point of failure. The group’s near-total dominance of the American ransomware market (323 of 1,721 US attacks) suggests either an unusually large affiliate roster or a payout structure attractive enough to pull operators away from competing platforms.


**Why Qilin Dominates:**


- **Affiliate Loyalty:** Competitive payout splits keep operators recruiting and retaining talent
- **Rapid Exploit Weaponization:** Fast turnaround from vulnerability disclosure to active exploitation
- **Sector Fluency:** Deep understanding of which industries face the highest downtime cost
- **Established Data Brokerage Ties:** Exfiltrated data reliably reaches monetization channels


**Americas Security Implications:** Any organization in construction, professional services, manufacturing, or healthcare operating in the US or Canada should treat Qilin as a primary named threat, not a generic ransomware risk.


### **2. Akira:** North America’s Persistent Operator


**Attack Volume:** 268 documented incidents, almost entirely concentrated in North America — 12.2% of the regional total


[Akira](https://thecyberexpress.com/akira-ransomware-group-cisa-warning/) is the second most active group in the Americas, and unlike Qilin, its footprint is almost exclusively North American. CRIL’s data shows Akira’s South American presence is negligible to date.


**Geographic Concentration:**


- **United States:** 247 attacks (92% of Akira’s total Americas volume)
- **Canada:** Remaining North American activity
- **South America:** Minimal to no confirmed activity


**Worldwide Sectoral Targeting:**


- **Construction:** 57 incidents
- **Manufacturing:** 54 incidents
- **Professional Services:** 47 incidents
- **Consumer Goods:** 19 incidents
- **IT & ITES:** 16 incidents


**Operational Characteristics:**


Akira has built a reliable playbook around compromising small-to-medium-sized businesses through unpatched public-facing network devices, then pivoting into construction and manufacturing environments where downtime tolerance is lowest. The group’s consistency — rather than explosive growth — is its defining trait; it has neither the explosive scale of Qilin nor the geographic diversification of The Gentlemen, but it reliably executes against the same target profile month after month.


**Americas Security Implications:** North American SMBs in construction, manufacturing, and professional services should assume Akira is actively scanning for exposed remote access infrastructure. Its South American absence should not be mistaken for permanence — RaaS groups expand geographically once North American markets saturate.


### **3. INC Ransom:** The Law-Firm Specialist


**Attack Volume:** 171 documented incidents (164 in North America, 7 in South America) — 7.8% of the regional total


INC Ransom distinguishes itself through sector specialization rather than volume. The group shows a clear, repeated preference for Professional Services organizations — particularly law firms — leveraging the sensitive, high-stakes nature of legal client data.


**Geographic Concentration:**


- **United States:** 154 attacks
- **Canada:** 6 attacks
- **Brazil:** 4 attacks


**Worldwide Sectoral Targeting:**


- **Professional Services:** 58 incidents (primary focus, with a documented preference for law firms)
- **Construction:** 27 incidents
- **Manufacturing:** 26 incidents
- **Healthcare:** 21 incidents
- **Organisation/Non-profit:** 12 incidents


**Operational Characteristics:**


INC Ransom’s rapid operational pace and consistent targeting of law firms, healthcare providers, and transportation/energy operators reflects a strategy built entirely around double-extortion leverage. The sensitivity of the data matters more than the size of the victim. A regional law firm holding privileged client communications is, to INC Ransom, a more valuable target than a much larger manufacturer with less sensitive data.


**Americas Security Implications:** Law firms, accounting practices, and consulting shops across the US, Canada, and Brazil should assume INC Ransom is actively targeting client confidentiality as leverage — not just encrypting file servers for disruption.


### **4. Dragonforce:** The Cross-Border Supply-Chain Operator


**Attack Volume:** 153 documented incidents (148 in North America, 5 in South America) — 7.0% of the regional total


Dragonforce maintains an aggressive operational tempo focused heavily on the United States, with a strategy that suggests supply-chain-aware targeting rather than random opportunism.


**Geographic Concentration:**


- **United States:** 135 attacks
- **Canada:** 11 attacks
- **South America:** 5 attacks


**Worldwide Sectoral Targeting:**


- **Construction:** 48 incidents
- **Manufacturing:** 31 incidents
- **Professional Services:** 28 incidents
- **IT & ITES:** 18 incidents
- **BFSI:** 17 incidents


**Operational Characteristics:**


Dragonforce’s manufacturing and construction focus mirrors Qilin’s and Akira’s playbooks, but its concentration in the US combined with limited-but-present South American activity hints at interest in transnational manufacturing supply chains. North American organizations with manufacturing partners or subsidiaries in Latin America should treat this as a lateral-access risk, not just a direct-targeting one.


**Americas Security Implications:** Manufacturers and construction firms with cross-border operations — a common structure across USMCA supply chains — should extend Dragonforce-specific monitoring to subsidiaries and vendors, not just headquarters networks.


### **5. The Gentlemen:** South America’s Dominant Threat


**Attack Volume:** 146 documented incidents (100 in North America, 46 in South America) — 6.7% of the regional total, but the **single most active ransomware group in South America specifically.**


While The Gentlemen rank fifth across the combined Americas, they are the #1 threat actor in South America on their own — responsible for roughly 22% of every ransomware attack recorded in that sub-region.


**Geographic Concentration:**


- **United States:** 77 attacks
- **North America:** 100 attacks
- **Brazil:** 15 attacks
- **South America:** 46 attacks (largest single-group share in the sub-region)


**Worldwide Sectoral Targeting:**


- **Manufacturing:** 56 incidents
- **Construction:** 45 incidents
- **Healthcare:** 37 incidents
- **IT & ITES:** 36 incidents
- **Consumer Goods:** 34 incidents


**Operational Characteristics:**


The Gentlemen are a relatively new operator that has achieved outsized scale in a short window, and their South American concentration is the most important regional signal in this dataset. Unlike Qilin or Akira — which built North American dominance first and are only beginning to diversify — The Gentlemen appear to have prioritized South America as a primary theatre from early in their operational life, an unusual strategic choice that may reflect lower defensive maturity, less aggressive law enforcement cooperation, or simply less competitive pressure from other RaaS operators in the sub-region.


**Americas Security Implications:** South American organizations — especially in healthcare, manufacturing, and IT services — should treat The Gentlemen as their single highest-priority named adversary. North American organizations should not discount them either; 100 US-focused attacks is a substantial footprint for a group still building its brand.


### **Other Notable Threats:** Play, LockBit, and CL0P


Three additional groups warrant inclusion in any Americas threat model:


- **Play (144 attacks, North America only):** Continues its “Big Game Hunting” approach layered with high-volume SMB attacks via unpatched public-facing network devices, concentrated almost entirely on US and Canadian construction, professional services, and manufacturing targets.
- **LockBit (80 attacks combined — 47 in North America, 33 in South America):** Despite sustained international law enforcement pressure and repeated takedown attempts, LockBit remains operationally resilient across both sub-regions, notably compromising Chile’s Clínica Dávila in South America.
- **CL0P (93 attacks combined — 91 in North America, 2 in South America):** Operated differently from its peers, executing a large-scale campaign concentrated in January and February 2026 that exploited a single zero-day vulnerability across hundreds of organizations at once — reminiscent of the group’s historical MOVEit campaign.


##### **Also read:[The Most Active Threat Actors of H1 2026](https://cyble.com/blog/most-active-threat-actors-h1-2026/)**


## The Five Most Targeted Nations in the Americas


### **United States:** The Global Ransomware Epicenter


**Attack Volume:** 1,721 ransomware attacks — 78.7% of all Americas ransomware activity, and roughly 45% of every ransomware attack recorded worldwide.


No other country on Earth comes close to the volume of ransomware activity absorbed by the United States in H1 2026. The country functions as the default target for nearly every major RaaS operator active today.


**Threat Actor Concentration:**


- **Qilin:** 323 attacks
- **Akira:** 247 attacks
- **INC Ransom:** 154 attacks
- **Dragonforce:** 135 attacks
- **Play:** 134 attacks


**Sectoral Breakdown:** Manufacturing, Professional Services, Construction, and Healthcare bear the brunt, consistent with the broader North American pattern of operationally sensitive, low-downtime-tolerance industries.


#### Why the United States Faces Maximum Pressure


The scale of the US economy, its dense concentration of mid-market manufacturers, law firms, and healthcare providers, and its comparatively high ransom-payment history combine to make it the most economically rational target for every major ransomware operator. US organizations also frequently anchor cross-border supply chains stretching into Canada, Mexico, and South America — meaning a US compromise can cascade into lateral access against hemispheric partners.


**Defensive Priority:** US organizations across construction, manufacturing, professional services, and healthcare should assume Qilin, Akira, INC Ransom, Dragonforce, Play, and The Gentlemen are all actively scanning for exploitable entry points into their networks simultaneously — not sequentially.


### **Canada:** The Cross-Border Extension


**Attack Volume:** 179 ransomware attacks — 8.2% of the regional total.


Canada’s threat profile closely tracks the United States, reflecting deep economic integration and shared supply chains rather than a distinct targeting logic of its own.


**Sectoral Breakdown:** Manufacturing, professional services, and construction dominate, mirroring the US pattern almost directly.


#### Why Canada Faces Sustained Pressure


Canadian organizations are frequently subsidiaries, suppliers, or joint-venture partners of US enterprises, which means the same RaaS groups saturating the US market extend naturally northward. Cross-border manufacturing in particular creates lateral access opportunities that Dragonforce and Akira appear well-positioned to exploit.


**Defensive Priority:** Canadian organizations should not assume distance from US headquarters provides insulation — the same threat actors, exploiting the same vulnerability classes, are already active on both sides of the border.


### **Brazil:** The Financial Malware and Ransomware Convergence Point


**Attack Volume:** 71 ransomware attacks — 3.2% of the regional total, but the largest single concentration in South America.


Brazil represents South America’s most complex threat environment, combining traditional ransomware pressure with a maturing, sophisticated financial malware ecosystem.


**Threat Actor Concentration:** The Gentlemen (15 attacks), LockBit, and a fragmented tail of smaller operators.


**Sectoral Breakdown:** Government & Law Enforcement, BFSI, and Healthcare are the most consistently targeted sectors.


#### Why Brazil Faces a Dual Threat


Beyond ransomware, Brazil emerged in H1 2026 as a focal point for new Android banking trojan families — TCLBANKER and BTMOB RAT — which use self-propagation, evasion techniques, and Malware-as-a-Service (MaaS) distribution models to target banking and cryptocurrency users directly. Brazil also suffered an alleged 250-million-record breach of Serasa, one of the country’s largest credit bureaus, alongside an access sale allegedly targeting the Central Bank of Brazil — a listing that, if genuine, represents one of the most significant initial-access offerings tracked anywhere in the report.


**Defensive Priority:** Brazilian financial institutions should treat mobile banking malware and ransomware as converging risks rather than separate problems — the same underground economy is monetizing both. Government and BFSI entities should assume access-broker listings referencing critical national infrastructure require immediate incident-response-level validation, not routine monitoring.


### **Mexico:** The Emerging Nearshoring Risk


**Attack Volume:** 39 ransomware attacks — 1.8% of the regional total.


Mexico’s attack volume is meaningfully lower than the US, Canada, or Brazil, but its position within North American manufacturing supply chains — accelerated by ongoing nearshoring trends — makes it a nation to watch closely rather than dismiss.


#### Why Mexico Warrants Increased Attention


As global manufacturers continue relocating production closer to the US market, Mexican facilities increasingly sit inside the same supply chains that Dragonforce, Akira, and Qilin already target aggressively north of the border. Lower current attack volume may reflect earlier-stage targeting rather than lower risk — a pattern security teams should not mistake for durable safety.


**Defensive Priority:** Manufacturers with Mexican operations should extend the same OT/IT segmentation and vulnerability management discipline applied to US and Canadian facilities to their Mexican sites, rather than treating them as lower priority.


### **Colombia:** Where Hacktivism Meets Cybercrime


**Attack Volume:** 33 ransomware attacks — 1.5% of the regional total.


Colombia’s ransomware volume is modest, but the country stands out for the density of ideologically motivated activity layered on top of financially driven attacks.


#### Why Colombia Faces a Blended Threat


Groups such as **Anonymous Colombia (#OpColombia)** ran active campaigns throughout H1 2026 blending website defacement, DDoS attacks, and data leak activity — consistent with the broader South American pattern in which hacktivist-branded channels frequently overlap with financially motivated cybercrime infrastructure.


**Defensive Priority:** Colombian government and law enforcement entities — the most frequently targeted sector across South America overall — should treat hacktivist claims as credible threat intelligence signals rather than dismissing them as purely ideological noise.


## Where Americas Organizations Face Maximum Risk: A Sectoral Analysis


### **Professional Services:** One of the Top Targets


**Attack Volume:** The second most heavily impacted sector in North America.


Professional services firms — law, accounting, and consulting practices — are one of the top jobs on North America’s ransomware target list, driven overwhelmingly by INC Ransom and AiLock’s aggressive targeting of client-confidential data.


#### Why Professional Services Are Targeted


1. **Privileged Data Concentration:** Legal privilege and client confidentiality create existential regulatory and reputational exposure that threat actors exploit for maximum ransom leverage.
2. **Regulatory Pressure:** Breach notification requirements incentivize rapid ransom payment to avoid compounding disclosure penalties.
3. **Trust-Based Business Model:** A single confirmed breach can permanently damage client relationships built entirely on confidentiality.
4. **Documented Actor Preference:** INC Ransom has shown a specific, repeated preference for law firms — this is not incidental targeting.


**Notable Incident Pattern:** AiLock’s activity stood out for a coordinated wave of victim disclosures on a single day — March 3, 2026 — a pattern consistent with mass-exploitation of a shared vulnerability rather than individually researched targeting.


**Defensive Recommendations:**


- Segregate client data on separate network segments with distinct, audited access controls
- Deploy data loss prevention (DLP) with aggressive egress monitoring for client-data exfiltration
- Maintain comprehensive access logs for all sensitive client-data touchpoints
- Evaluate ransomware-specific cyber insurance addressing confidentiality exposure


### **Construction and Manufacturing:** The Downtime Economy


**Attack Volume:** Construction and Manufacturing rank first and third in North America; combined, they represent the largest share of Qilin, Akira, Dragonforce, and The Gentlemen’s worldwide targeting.


Constructions and manufacturing share a common vulnerability across the Americas: both operate on tight, contractually enforced timelines where downtime translates directly into cascading financial penalties.


#### Why Construction and Manufacturing Are Targeted


1. **Time-Sensitive Financial Exposure:** Missed construction deadlines trigger contractual penalties; halted production lines trigger lost revenue and breached delivery commitments.
2. **OT/IT Convergence:** Modern factories and job sites increasingly integrate operational technology with corporate IT, creating exploitation bridges unavailable in pure-IT industries.
3. **Supply-Chain Complexity:** Both industries depend on dense webs of subcontractors and suppliers — compromising one upstream partner can provide lateral access into prime contractors.
4. **Cross-Border Exposure:** US-Canada-Mexico manufacturing integration (and increasingly, US-Brazil trade relationships) means a single compromise can propagate across national borders.


**Defensive Recommendations:**


- Implement airgapped network segmentation between OT and corporate IT environments
- Prioritize vulnerability patching for network appliances and identity systems over blanket patch cycles
- Maintain fully offline, immutable backups of critical project and production data
- Extend third-party risk assessments to subcontractors, suppliers, and cross-border subsidiaries


### **Healthcare:** South America’s Critical Infrastructure Threat


**Attack Volume:** One of the top four most heavily impacted sectors in South America.


Healthcare organizations across the Americas — but particularly in South America — face a threat dynamic distinct from financial pressure alone: ransomware attacks against hospitals directly endanger patient safety.


#### Why Healthcare Is Targeted


1. **Patient Safety Leverage:** Downtime in diagnostic systems, pharmaceutical dispensing, and patient records directly threatens continuity of care, creating existential pressure to pay quickly.
2. **Documented Regional Incidents:** The Gentlemen’s claimed attack on Primero Medicina Privada and LockBit’s compromise of Chile’s Clínica Dávila both illustrate ransomware groups’ willingness to target hospital networks directly.
3. **Data Value:** Patient medical records and clinical data command premium prices on dark web marketplaces.
4. **System Complexity:** Healthcare IT environments blend legacy diagnostic equipment, electronic health records, and connected medical devices — each with distinct security postures.


**Defensive Recommendations:**


- Implement complete network isolation between clinical systems and corporate IT
- Deploy redundant diagnostic and pharmaceutical systems capable of manual fallback operation
- Encrypt all patient medical records end-to-end
- Build healthcare-specific incident response plans addressing patient notification and continuity of care


### **Agriculture & Livestock:** The Americas’ Emerging Supply-Chain Target


**Attack Volume:** 33% of all North American initial access listings — the second-most targeted sector in the region’s access brokeragemarket .


A distinctive Americas finding: initial access brokers targeting the region show unusually strong interest in Agriculture & Livestock, second only to Technology.


#### Why Agriculture & Livestock Is an Emerging Target


North America’s food supply chain increasingly depends on connected logistics, cold-chain monitoring, and precision agriculture technology — creating an attack surface that did not meaningfully exist a decade ago. Access brokers appear to be positioning themselves ahead of ransomware operators, selling footholds into agricultural operations before ransomware crews weaponize them. This mirrors a pattern seen elsewhere globally but is particularly pronounced in North America’s access brokerage data.


**Defensive Recommendations:**


- Treat agricultural technology platforms (precision ag, cold-chain IoT) with the same security rigor as manufacturing OT
- Monitor initial access broker markets specifically for agriculture and food-sector listings
- Build incident response plans accounting for food-supply-chain continuity, not just data confidentiality


## **Geopolitical and Ideological Dimensions:** Hacktivism Across the Hemisphere


### SOLDADOS DIGITALES – UNIÓN AMERICANA: A Hemispheric Hacktivist Collective


Unlike most hacktivist channels tracked in this report, **SOLDADOS DIGITALES – UNIÓN AMERICANA** operates across both North and South America, making it one of the few genuinely hemispheric threat actors identified in H1 2026 — a significant finding given how regionally siloed most hacktivist activity tends to be.


**Combined Hacktivism Metrics (North + South America):**


- **~140 confirmed data leak and dump posts** across both sub-regions
- **At least 932 unique domains impacted** (360 in North America, 572 in South America)
- **Primary targets:** Government & LEA, Technology, BFSI, Telecommunication, Education


**Notable Collectives by Sub-Region:**


- **North America:** SOLDADOS DIGITALES – UNIÓN AMERICANA, Anonymous #FreeTurtleIsland, KERALA HACKERS, LYSTIC TEAM #ID
- **South America:** SOLDADOS DIGITALES – UNIÓN AMERICANA, Anonymous Colombia (#OpColombia) Y.A.N, BLAZER TEAM ATTACK


**The Convergence Problem:** As with hacktivist activity documented elsewhere in CRIL’s global dataset, several Americas-based channels marketed as ideological collectives function as hybrid operations — logging DDoS attacks and defacement claims alongside stolen-data brokerage and DDoS-for-hire services. Security teams should treat these channels as credible threat intelligence sources rather than dismissing their claims as purely political theater.


## Regional Threat Actor Summary: Who Targets Your Americas Organization


**If You’re in Professional Services:**


- **Primary Threat:** INC Ransom, Qilin
- **Secondary Threat:** AiLock, The Gentlemen
- **Vulnerability:** Client data exfiltration, regulatory breach-notification pressure
- **Defensive Focus:** DLP, client data segregation, ransomware-specific cyber insurance, cyber threat intelligence


**If You’re in Manufacturing or Construction:**


- **Primary Threat:** Qilin, Akira, Dragonforce
- **Secondary Threat:** The Gentlemen, Play
- **Vulnerability:** OT/IT convergence, cross-border supply-chain exposure, contractual downtime penalties
- **Defensive Focus:** OT segmentation, immutable backups, cross-border third-party risk management


**If You’re in Healthcare:**


- **Primary Threat:** The Gentlemen (South America), Qilin (North America)
- **Secondary Threat:** LockBit
- **Vulnerability:** Patient-safety leverage, legacy medical device integration
- **Defensive Focus:** Clinical system isolation, redundant critical systems, patient-notification-ready incident response


**If You’re in BFSI:**


- **Primary Threat:** Data exfiltration actors, mobile banking malware (Brazil)
- **Secondary Threat:** Qilin, The Gentlemen
- **Vulnerability:** Financial data value, mobile malware convergence, regulatory exposure
- **Defensive Focus:** DLP with aggressive egress controls, mobile threat monitoring, data encryption


**If You’re in Agriculture & Livestock:**


- **Primary Threat:** Initial access brokers
- **Secondary Threat:** Downstream ransomware operators exploiting sold access
- **Vulnerability:** Precision agriculture and cold-chain IoT exposure
- **Defensive Focus:** OT-equivalent segmentation for agricultural technology, access-broker monitoring


**If You’re in Government & Law Enforcement (South America specifically):**


- **Primary Threat:** RALord/Nova, CoinbaseCartel, hacktivist-branded channels
- **Secondary Threat:** LockBit, The Gentlemen
- **Vulnerability:** Public-sector data value, hybrid ideological/financial targeting
- **Defensive Focus:** Treat hacktivist claims as credible intelligence, harden citizen-data repositories


## Strategic Defense Recommendations for Americas Organizations


Based on CRIL’s H1 2026 regional data, Americas security leaders should prioritize defensive investment in the following sequence.


**Phase 1: Critical Infrastructure Protection (30 days)**


- **Inventory Network Appliances:** Document every internet-facing firewall, VPN, and security gateway
- **Patch Critical CVEs:** Prioritize Ivanti, Fortinet, Cisco, SolarWinds, and Palo Alto Networks appliances — the vendors repeatedly appearing in both the CISA KEV catalog and active exploitation campaigns
- **Harden Remote Access:** Enforce phishing-resistant MFA on all administrative and remote access paths
- **Deploy Behavioral Monitoring:** Watch for anomalous activity on network appliances specifically


**Phase 2: Data Protection (60 days)**


- **Data Inventory:** Catalog sensitive holdings — client data, financial records, patient records, intellectual property
- **DLP Implementation:** Deploy data loss prevention with aggressive egress monitoring
- **Encryption Standards:** Enforce encryption in transit and at rest across all sensitive data stores
- **Access Auditing:** Maintain comprehensive logs for every access event touching sensitive data


**Phase 3: Operational Resilience (90 days)**


- **Immutable Backups:** Establish offline, immutable backup infrastructure isolated from production networks
- **Sector-Specific Incident Response:** Build playbooks addressing construction project continuity, manufacturing downtime, and healthcare patient-safety scenarios specifically
- **Cross-Border Continuity Planning:** For organizations with US-Canada-Mexico or US-Brazil operations, extend continuity plans across all connected facilities
- **Recovery Testing:** Conduct quarterly backup restoration drills to verify actual recovery capability


**Phase 4: Threat Hunting and Detection (Ongoing)**


- **Named-Actor Threat Intelligence:** Subscribe to intelligence feeds tracking Qilin, Akira, INC Ransom, Dragonforce, and The Gentlemen specifically
- **Access-Broker Monitoring:** Track listings for organizational exposure
- **Supply-Chain Monitoring:** Continuously assess vendor and subsidiary security posture across borders
- **Mobile Malware Awareness (Brazil-specific):** Financial institutions should monitor for TCLBANKER- and BTMOB RAT-style Android banking trojan activity targeting customers


## **Conclusion:** The Americas Ransomware Reality


The Americas is not just the largest ransomware theatre in the world by volume — it is two distinct threat environments operating under a single regional label. North America hosts a saturated, competitive RaaS marketplace where no single group dominates outright. South America is consolidating around a smaller set of operators, led decisively by The Gentlemen.


**Key Takeaways:**


1. **The Americas carries the global center of gravity:** 2,188 of the world’s 3,836 documented ransomware attacks (57%) struck North or South America in H1 2026.
2. **Five groups anchor the threat:** Qilin (410), Akira (268), INC Ransom (171), Dragonforce (153), and The Gentlemen (146) collectively account for over half of all Americas ransomware activity — but their dominance splits sharply by sub-region.
3. **North America and South America require different playbooks:** North America’s threat model demands broad coverage against a long tail of competing operators; South America’s demands deep, specific defense against The Gentlemen, Qilin, and LockBit.
4. **The United States remains the world’s single largest target:** 1,721 attacks — nearly 45% of global ransomware volume — makes the US the default target for virtually every major RaaS operator active today.
5. **Brazil’s threat is compounding, not singular:** ransomware, mass data breach, and mobile banking malware are converging in the same underground economy targeting the same financial institutions.
6. **Sector risk follows economic logic, not chance:** Professional Services, Manufacturing, Construction, Healthcare, and — distinctively for the Americas — Agriculture & Livestock face targeting because threat actors have identified specific, exploitable economic pressure points in each.
7. **Access brokers are a leading indicator:** a small number of sellers control the region’s initial access market and routinely precede ransomware deployment by weeks.


For security leaders across North and South America, the strategic imperative is the same even where the tactical details diverge: know which named actors are active in your specific country and sector, prioritize risk-based patching over blanket cycles, treat data exfiltration as inevitable rather than optional, and build recovery infrastructure that assumes an attack will happen — not one that hopes it won’t. The data confirms the Americas will remain the world’s most heavily targeted ransomware region through the remainder of 2026. The only open question is how prepared each organization chooses to be.


---


## **Frequently Asked Questions** (FAQs)


**How many ransomware attacks hit the Americas in H1 2026?**


2,188 documented ransomware attacks were observed across North and South America in H1 2026, according to Cyble Research and Intelligence Labs (CRIL) findings.


**Which ransomware group is most active in the Americas in H1 2026?**


Qilin is the most active group across the combined Americas, with 410 documented attacks (370 in North America, 40 in South America). Within South America specifically, however, The Gentlemen — not Qilin — is the dominant actor.


**How many ransomware attacks hit North America in H1 2026?**


CRIL recorded 1,981 ransomware attacks in North America during H1 2026, representing roughly 52% of all ransomware activity tracked worldwide.


**How many ransomware attacks targeted the US in H1 2026? Is it the highest?**


Yes. CRIL observed 1,721 ransomware attacks targeted at the US — which is 78.7% of the American continent (North and South, both), and nearly 45% of every ransomware attack recorded worldwide.


**Which sector was the most targeted in South America?**


IT & ITES remained the most targeted sector in South America for H1 2026.


**Ransomware actors targeted which country the most in South America?**


Brazil. With 71 attacks, it was the prime target of ransomware actors in H1 2026.


**Is Brazil a significant ransomware target?**


Yes. Brazil recorded 71 ransomware attacks — the highest total in South America — and additionally faced an alleged 250 million record breach at credit bureau Serasa, an access sale allegedly targeting the Central Bank of Brazil, and new Android banking trojan families (TCLBANKER, BTMOB RAT) targeting financial and cryptocurrency users.


**What is the most targeted industry in the Americas?**


Construction tops North America’s target list, while IT & ITES, Healthcare, and Professional Services top South America’s. Across the whole Americas, Construction and Manufacturing remain consistently high-risk due to their low tolerance for operational downtime.


CYBLE.


AI Threat Intelligence


### Stop Executive Threats
Before They Strike


Monitor dark web chatter, detect lookalike domains, and protect your C-suite from targeted impersonation — in real time, across 50+ countries.


[Request a Demo](https://cyble.com/request-demo/)


[North America](https://cyble.com/blog/tag/north-america/)[Ransomware gang](https://cyble.com/blog/tag/ransomware-gang/)[South America](https://cyble.com/blog/tag/south-america/)[Threat Actors](https://cyble.com/blog/tag/threat-actors/)


[Previous Post Ransomware Now Shows Up in Nearly Half of All Breaches: A Survival Playbook for Lean Security Teams](https://cyble.com/blog/ransomware-incident-response-plan/)


### More from Cyble


[View all posts](https://cyble.com/blog/)


[Threat Actor Ransomware Now Shows Up in Nearly Half of All Breaches: A Survival Playbook for Lean Security Teams Aug 10, 2026 · 6 min read](https://cyble.com/blog/ransomware-incident-response-plan/)[Cyber news Ransomware Threats in Europe H1 2026: A Deep Dive into Regional Attack Patterns and Dominant Threat Actors Aug 7, 2026 · 21 min read](https://cyble.com/blog/ransomware-threats-in-europe-h1-2026/)[Data Breach From Stolen Credentials to Full Breach: The 72-Hour Timeline Aug 5, 2026 · 6 min read](https://cyble.com/blog/72-hour-timeline-credential-based-cyberattack/)
