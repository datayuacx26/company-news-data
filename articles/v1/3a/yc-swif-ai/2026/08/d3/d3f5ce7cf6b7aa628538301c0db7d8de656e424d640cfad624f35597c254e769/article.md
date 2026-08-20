---
schema_version: "1.0.0"
document_id: "d3f5ce7cf6b7aa628538301c0db7d8de656e424d640cfad624f35597c254e769"
company_key: "yc-swif-ai"
company: "Swif.ai"
source_id: "yc-swif-ai-news-import-789a3488158e"
canonical_url: "https://www.swif.ai/blog/iot-security-statistics"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-07-22T15:23:23.396754+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:b0009c66507b2fa0aaf6878b55e3a937401aeb8e0a60d57245bc4d59beaee337"
---

# IoT Security Statistics for 2026: Devices, Botnets, and the New Regulatory Floor

The world will end 2026 with more connected devices than ever, and more attacks against them. The number of connected IoT devices reached 18.5 billion in 2024 and is on pace to hit 21.1 billion by the end of 2025, according to[IoT Analytics](https://iot-analytics.com/number-connected-iot-devices/) , with a 13.2% compound annual growth rate carrying the installed base to 39 billion by 2030. As that fleet grew, attacker interest grew faster:[Nokia](https://www.nokia.com/cybersecurity/threat-intelligence-report/) reported a fivefold increase in malicious IoT botnet activity in the last year, with compromised devices climbing from roughly 200,000 to 1 million.[Cloudflare](https://blog.cloudflare.com/ddos-threat-report-2025-q3/) mitigated a record 29.7 Tbps DDoS attack from a single IoT botnet (Aisuru) in Q3 2025. These are the IoT security statistics that IT, security, and compliance leaders should carry into 2026.


### **Key IoT security statistics at a glance**


- **21.1 billion** active IoT devices were connected worldwide by the end of 2025, up 14% year over year, and the installed base is forecast to reach 39 billion by 2030, per[IoT Analytics State of IoT 2025](https://iot-analytics.com/number-connected-iot-devices/) .
- **5x increase** in malicious IoT botnet activity over the past year, with compromised devices climbing from 200,000 to roughly 1 million and accounting for more than 40% of all DDoS traffic, per the[Nokia Threat Intelligence Report 2025](https://www.nokia.com/cybersecurity/threat-intelligence-report/) .
- **29.7 Tbps** was the size of the world record DDoS attack launched by the Aisuru IoT botnet in Q3 2025, with an estimated 1 to 4 million infected hosts, per[Cloudflare](https://blog.cloudflare.com/ddos-threat-report-2025-q3/) . Cloudflare blocked 8.3 million DDoS attacks in Q3 alone and 36.2 million year to date.
- **99%** of analyzed healthcare organizations had IoMT devices containing Known Exploited Vulnerabilities (KEVs), and 89% had ransomware-linked KEVs on devices that were also insecurely connected to the internet, per the[Claroty State of CPS Security 2025: Healthcare Exposures](https://claroty.com/blog/clarotys-state-of-cps-security-report-healthcare-exposures-2025) report.
- **111,000 OT devices** across manufacturing, logistics, and natural resources organizations carry KEVs, and 68% of those flaws are linked to active ransomware groups, per the[Claroty State of CPS Security 2025: OT Exposures](https://claroty.com/blog/clarotys-state-of-cps-security-2025-ot-exposures) report. Manufacturing alone accounts for 96,000 of those vulnerable devices.
- **Routers** now account for more than 50% of devices that carry the most dangerous vulnerabilities, overtaking endpoints as the riskiest IT device category, per[Forescout Vedere Labs](https://www.forescout.com/research-labs/the-riskiest-devices-of-2025/) . Average device risk rose 15% year over year.
- **124% jump** in IoT malware attacks in 2024, with more than 17 million attacks blocked against IP cameras alone and encrypted threats against IoT up 93%, per the[SonicWall 2025 Cyber Threat Report](https://www.sonicwall.com/resources/white-papers/2025-sonicwall-cyber-threat-report) .


**January 4, 2027** is the deadline for every vendor supplying consumer IoT products to the U.S. federal government to carry the new[U.S. Cyber Trust Mark](https://www.fcc.gov/CyberTrustMark) , the voluntary FCC label that launched in January 2025. In the EU, the[Cyber Resilience Act](https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act) starts incident reporting on 11 September 2026 and full obligations on 11 December 2027.


‍


### **How many IoT devices are out there in 2026**


The headline number every journalist covering this beat needs is the installed base. According to[IoT Analytics](https://iot-analytics.com/number-connected-iot-devices/) , the number of connected IoT devices reached 18.5 billion in 2024 (a 12% rise over 2023), and based on first-half 2025 data the firm expects 21.1 billion devices to be connected by the end of 2025, a 14% year over year gain. The 2030 forecast sits at 39 billion devices, reflecting a 13.2% CAGR. Wi-Fi (32% of all connections), Bluetooth (24%), and cellular (22%) together carry roughly 78% of the installed base.


Two notes for security buyers reading those numbers. First, IoT Analytics excludes phones, tablets, and PCs from its count, which means almost every device in the 21.1 billion fleet is a fixed-function endpoint with limited or no native security agent. Second, the fastest cellular IoT growth is in 5G and LTE Cat-1 bis, both of which keep devices always on, always reachable, and (often) outside the corporate perimeter. The next sections show what attackers have done with that surface.


### **IoT botnets, Aisuru, and the new ceiling on DDoS**


IoT-driven DDoS is the single biggest change in the threat landscape since 2024. The[Nokia Threat Intelligence Report 2025](https://www.nokia.com/cybersecurity/threat-intelligence-report/) documents a fivefold increase in malicious IoT botnet activity year over year, with the number of compromised IoT devices used in DDoS climbing from roughly 200,000 to 1 million. IoT botnets now account for more than 40% of all observed DDoS traffic, and 78% of DDoS attacks now end within five minutes (more than one third end in under two minutes), which is faster than most on-demand scrubbing services can react.


The 2025 botnet ecosystem is also more sophisticated than the original Mirai era. Nokia points to Mirai-descendant variants such as Eleven11bot/RapperBot and Aisuru, with infrastructure-level scale above 30,000 compromised IoT devices per cluster. Cloudflare goes further. In[Cloudflare's Q3 2025 DDoS Threat Report](https://blog.cloudflare.com/ddos-threat-report-2025-q3/) , the Aisuru botnet was credited with an estimated 1 to 4 million infected hosts globally and routinely launched attacks above 1 Tbps and 1 Bpps. The peak was 29.7 Tbps and 14.1 Bpps, the largest DDoS attack ever recorded. Aisuru hyper-volumetric attacks surged 54% quarter over quarter, averaging 14 per day. Cloudflare blocked 8.3 million DDoS attacks in Q3 alone, 36.2 million across the first three quarters of 2025 (170% of the full-year 2024 total), with UDP floods up 231% QoQ and attacks above 1 Tbps up 227% QoQ. Roughly 2 in every 100 network-layer attacks still trace back to Mirai variants almost ten years after Mirai's public debut, which is a useful reminder that old IoT malware does not retire when patches ship.


### **Routers, cameras, and the riskiest connected devices in 2026**


When attackers pick IoT targets, they pick the devices with the largest install base, the weakest credentials, and the longest patch tail. The[Forescout Vedere Labs Riskiest Devices of 2025](https://www.forescout.com/research-labs/the-riskiest-devices-of-2025/) report logged a 15% year over year increase in average device risk and ranked routers as the single riskiest category, with network equipment now accounting for more than 50% of the devices carrying the most dangerous vulnerabilities. Forescout adds that network equipment has overtaken traditional endpoints as the most dangerous IT category for the first time in the report's history.


IP cameras have followed the same arc. The[SonicWall 2025 Cyber Threat Report](https://www.sonicwall.com/resources/white-papers/2025-sonicwall-cyber-threat-report) recorded a 124% jump in IoT malware attacks in 2024, with more than 17 million attacks against IP cameras alone, many targeting devices in government facilities and other sensitive locations. Encrypted threats against IoT rose 93%. The same report flags a brutal timing gap: attackers weaponize publicly available exploit code within 48 hours of release in 61% of cases, while organizations typically take 120 to 150 days to apply the matching patch. For consumer-grade IoT (routers, cameras, DVRs, gateways), the gap is even wider because patches often ship only via firmware updates that users never trigger.


### **IoT in healthcare and the IoMT problem**


Healthcare is where IoT vulnerabilities turn into patient safety incidents. The[Claroty State of CPS Security 2025: Healthcare Exposures](https://claroty.com/blog/clarotys-state-of-cps-security-report-healthcare-exposures-2025) report analyzed more than 2.25 million Internet of Medical Things (IoMT) devices and 647,000 OT devices across 351 healthcare organizations. The findings are stark. 99% of the analyzed organizations had IoMT devices with Known Exploited Vulnerabilities. 89% had devices that carried ransomware-linked KEVs and were also insecurely connected to the internet. Imaging systems (X-ray, CT, MRI, ultrasound) ranked as the single riskiest device category; 8% of imaging systems carried ransomware-linked KEVs while exposed to the public internet, an exposure pattern that touched 85% of organizations analyzed.


The OT side of healthcare is no better. Of the 647,000 OT devices Claroty examined, 78% of organizations had OT devices with KEVs, and 65% were managing devices that carried confirmed KEVs and were insecurely internet-connected. 96% of analyzed organizations had IoMT devices with KEVs linked specifically to ransomware operations. The[Forescout](https://www.forescout.com/research-labs/the-riskiest-devices-of-2025/) research adds context with four new IoMT device categories added to its 2025 list (imaging devices, lab equipment, healthcare workstations, infusion pump controllers), reflecting how rapidly the medical device threat surface is expanding. Forescout notes that infusion pump controllers, in particular, sit in a category where a compromise can directly tamper with drug delivery settings.


### **OT, ICS, and manufacturing: where IoT becomes physical risk**


Operational technology (OT) and industrial control systems (ICS) used to live on isolated networks. The IoT migration ended that. The[Claroty State of CPS Security 2025: OT Exposures](https://claroty.com/blog/clarotys-state-of-cps-security-2025-ot-exposures) report analyzed close to one million OT devices across 270 organizations in manufacturing, logistics, transportation, and natural resources. Researchers found 111,000 OT devices that contain KEVs, with 68% of those vulnerabilities tied to active ransomware groups. Manufacturing carried the largest share by far at over 96,000 vulnerable devices. 12% of all examined OT devices contained KEVs, and 40% of organizations had a subset of those assets insecurely connected to the internet. In other words: the physical floor of a typical factory now has thousands of devices that ransomware operators know how to compromise and can reach without ever touching the IT network.


The cross-industry view from[Forescout](https://www.forescout.com/research-labs/the-riskiest-devices-of-2025/) supports the same pattern. Retail topped Forescout's 2025 sector ranking for riskiest devices on average, followed by financial services, government, healthcare, and manufacturing. The takeaway for IT and security leaders is that IoT and OT no longer sit in a separate compliance bucket. They sit on the same network, share the same identity systems, and increasingly carry the same regulatory weight.


### **Smart home IoT and the default credentials problem**


The botnet pipeline starts in the home.[Nokia](https://www.nokia.com/cybersecurity/threat-intelligence-report/) notes that residential proxy networks now span more than 100 million hijacked home devices, with Mirai-descendant botnets like Eleven11 and Aisuru recruiting consumer DVRs, IP cameras, and home gateways at industrial scale. Most of those recruits are not zero-day exploits. They are devices reachable on the public internet with default or weak credentials, running firmware the vendor stopped patching years ago.[SonicWall](https://www.sonicwall.com/resources/white-papers/2025-sonicwall-cyber-threat-report) adds that encrypted IoT threats rose 93% in 2024, which means even when a smart home gateway is compromised, much of its outbound traffic now hides inside TLS sessions that home routers and ISPs cannot inspect.


Security teams should assume that every employee's home network sits adjacent to a corporate one through a VPN, a remote desktop session, or a personal device that doubles as a work device. The same[Cloudflare](https://blog.cloudflare.com/ddos-threat-report-2025-q3/) data on Aisuru shows that a single botnet feeding off home devices can disrupt entire U.S. ISP regions even when the ISP itself is not the target of the attack. Securing the corporate fleet is no longer sufficient if employees' home IoT is the launchpad for the next Tbps-scale flood.


### **Regulation: U.S. Cyber Trust Mark and the EU Cyber Resilience Act**


2026 is the year IoT regulation finally has teeth. In the United States, the FCC launched the[U.S. Cyber Trust Mark](https://www.fcc.gov/CyberTrustMark) on 7 January 2025 as a voluntary cybersecurity label for wireless consumer IoT products, based on NIST IR 8425. Eligible categories include smart cameras, voice assistants, smart appliances, fitness trackers, garage door openers, and baby monitors. The label is paired with a QR code that links to a public registry showing the device's security details. By 4 January 2027, every vendor that sells consumer IoT to the U.S. federal government will need to carry the label, turning what is technically a voluntary scheme into a procurement requirement for any manufacturer that wants to remain on federal buying lists.


In Europe, the[Cyber Resilience Act](https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act) has been in force since 10 December 2024 and applies to virtually every product with digital elements sold in the EU, including IoT devices, embedded operating systems, network equipment, and the software components inside them. Incident reporting obligations begin on 11 September 2026, and the main obligations (security by design, vulnerability handling, lifetime patching) apply from 11 December 2027. Manufacturers must deliver products without known exploitable vulnerabilities and maintain security throughout the expected product lifetime. Combined with the FCC label, these two regimes form the first cross-jurisdictional regulatory floor for IoT security ever shipped.


### **Emerging trends and what is new in 2026**


Three trends define IoT security heading into 2026.


**IoT botnets have set a new performance ceiling for DDoS.** The[Cloudflare](https://blog.cloudflare.com/ddos-threat-report-2025-q3/) Q3 2025 record of 29.7 Tbps from Aisuru is not an outlier; it sits on top of a curve where attacks above 1 Tbps grew 227% quarter over quarter and hyper-volumetric attacks averaged 14 per day. For any IoT-heavy organization (ISPs, healthcare networks, manufacturing OT), assuming a multi-Tbps attack is a tail-risk event no longer holds.


**Network equipment is now a more dangerous category than endpoints.** The[Forescout](https://www.forescout.com/research-labs/the-riskiest-devices-of-2025/) finding that routers account for more than 50% of the most dangerous device vulnerabilities reverses a decade of security spending that focused on user laptops. Edge IoT (routers, firewalls, VPN appliances, IoT gateways) now needs the same patch discipline, asset inventory, and vulnerability management that endpoints have had for years.


**Healthcare is the proving ground for IoT regulation.** With 99% of analyzed healthcare organizations carrying KEV-laden IoMT devices per[Claroty](https://claroty.com/blog/clarotys-state-of-cps-security-report-healthcare-exposures-2025) , the sector is the first where regulators (FDA in the U.S., MDR/IVDR plus the CRA in the EU) explicitly tie cybersecurity to device approval and post-market surveillance. The next two years will determine whether healthcare can pull its IoMT exposure down before the same pattern (insecure devices with public internet exposure) plays out in retail, manufacturing, and consumer markets at full scale.


For broader context on the trends above, see our[cyber attack statistics](https://www.swif.ai/blog/cyber-attack-statistics) and[malware statistics](https://www.swif.ai/blog/malware-statistics) roundups.


### **How swif.ai helps IT and security teams manage connected device risk**


swif.ai gives IT and security teams a single console to enforce device, identity, and compliance controls across the macOS, Windows, and Linux endpoints behind the numbers above. Explore[swif.ai unified endpoint management](https://www.swif.ai/products/unified-endpoint-management-uem) to see how it works.
