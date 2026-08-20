---
schema_version: "1.0.0"
document_id: "3e7bc0398547083c979457f6e3fede9a0c4758051c7b94eb0e3841f509d2944f"
company_key: "netscout-systems-inc-common-stock"
company: "NetScout Systems Inc."
source_id: "netscout-systems-inc-common-stock-news-import-adeff42e5f7b"
canonical_url: "https://www.netscout.com/blog/asert/161-days-eleven11"
published_at: null
first_seen_at: "2026-07-25T16:47:01.498587+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:dfcbe601946e77f3da4ca6d66e50cd02ff6765263fcb3b2e2a809ef797fe2d62"
---

# 161 Days of Eleven11

## Executive Summary


As a major player in the field of distributed denial-of-service (DDoS) detection and mitigation, it is crucial for us to closely monitor evolving threats in the DDoS landscape. Over the last year, NETSCOUT analysts observed a handful of newly emerged botnets, incapable of packet spoofing but potent sources of direct-path DDoS attacks. One of the most notable is the[Aisuru botnet](https://www.netscout.com/blog/asert/asert-threat-summary-aisuru-and-related-turbomirai-botnet-ddos) , which our ASERT group recently reported. Prior to Aisuru, another significant botnet, Eleven11, emerged with distinct command-and-control (C2) infrastructure characteristics.


## Key Findings


- Eleven11 botnet was discovered through the analysis of unusually large bandwidth patterns during DDoS attacks in February 2025, classifying it as part of the novel TurboMirai Internet of Things (IoT) botnet family.
- Another botnet, “RapperBot”, discovered in 2021, shares the same C2 infrastructure, indicating Eleven11 was active for far longer than initially assumed and was already previously identified as RapperBot.
- Digital traces tell a story of how the operators kept innovating the botnet to increase resiliency. One of these resiliency features is the use of OpenNIC, an alternative DNS root.
- NETSCOUT analysts saw a drop of activity in late July and early August. In mid-August, authorities revealed an arrest and the dismantling of the botnet.


## Discovery of Eleven11 Botnet


In late February 2025, a security researcher in the field posted about a newly discovered botnet with a proclaimed record-breaking firepower exceeding 6Tbps. NETSCOUT observed portions of the attack through our ATLAS telemetry. Building on existing tracking technology, we created an innovative pipeline that incorporates attack metadata to monitor direct-path infrastructure, using the Eleven11 botnet as an initial test case.


## Technical Analysis


The botnet’s activity already had been tracked since 2021 and was reported under the name RapperBot in 2022. A post from[researchers in 2022](https://www.fortinet.com/blog/threat-research/new-rapperbot-campaign-ddos-attacks) indicates that the botnet’s initial C2 servers were hardcoded as IPv4 addresses in the malware itself.


In the search for more traces, ASERT discovered malware samples of this same botnet family on sandbox platforms. The reports originated in July ([any.run](https://any.run/report/784a025d39e3bd4efc1d5b07075bf07ac294b6ce7d63400429b4f65ac9f360e1/683868b5-8053-4b7f-8155-ff6b2c0fcb8b) and[joesandbox.com](https://www.joesandbox.com/analysis/1476565/0/html) ) and December 2024 ([joesandbox.com](https://www.joesandbox.com/analysis/1575431/0/lighthtml) ), revealing once more that Eleven11 existed prior to the initial announcement on Mastodon in February 2025. These reports also reveal that the botnet’s source code matured, leveraging registered domain names instead of hardcoded IPv4 addresses to connect to C2 infrastructure. These domain names, present in the .libre zone, allowed the culprits behind this botnet to dynamically reconfigure C2 servers, once compromised, without redistributing new versions of the malware. The domain names “registered” in .libre are administered by[OpenNIC](https://opennic.org/) , an alternative DNS root known for its lax moderation that attracts illicit activity.


Analysts at ASERT investigated the domain names from the malware sandbox reports and found that C2 server IP addresses were configured in DNS TXT records of the alternative DNS root. In the early days, addresses were encapsulated between < and > brackets, later separated by pipe characters, indicating yet another implementation on how C2 IP addresses were processed by clients.


[Reverse-engineering researchers](https://blog.xlab.qianxin.com/rapperbot-en/) also shared versions of the malware that opted for domain names registered as ICANN domain names in the new generic top-level domains (gTLDs) .live and .info. The domain names resemble a domain-generation algorithm. Although the TXT records of the OpenNIC domain names held the C2 server IPs in plain text, the ICANN records encapsulated C2 server IPs in an encrypted string.


NETSCOUT’s threat intelligence team steadily gathered intelligence about the C2 servers. Attempts to identify the networks in which C2 infrastructure was home revealed that many of those poorly moderated networks were not only known, but also already shared with NETSCOUT’s customers via our ATLAS Intelligence Feed (AIF). AIF protects customers by enabling them to identify and block traffic originating from or destined for compromised infrastructure within their own perimeter.


## Leveraging Layer 3 Telemetry


The botnet is known to be a modified Mirai variant. One important characteristic of Mirai-like botnets is that many of them lack the ability to spoof source addresses while generating attack traffic. This inadvertently reveals the compromised host’s true IP address when participating in attacks. We leveraged this characteristic to identify migration patterns over the internet based on known sources from the past and feed that into novel ways to attribute DDoS attacks to the botnet.


**Figure 1:** Machine learning–enhanced analysis of Layer 3 telemetry enables NETSCOUT’s novel methodology for detecting and characterizing botnet DDoS operations, and also for attribution—in this case to the Eleven11 botnet.


With the help of the machine learning (ML) toolbox, the threat intelligence team implemented novel methodologies to identify DDoS events linked to the botnet in question. A noticeable decline was observed in late July; the botnet went dark in early August with no apparent explanation.


## Attack Insights


Visible in Figure 1, NETSCOUT’s ML-based attribution of DDoS events to the botnet shows a peak of weekly DDoS activity in March 2025, slowly fading off until attribution stops by the end of July. The small bars in August are likely false positives. The botnet’s C2 servers had been dismantled by authorities weeks earlier.


**Figure 2:** A closer look at the DDoS event impact (in Gbps) reveals that the Eleven11, or RapperBot, generated particularly high volumes of traffic.


Between late February and August, NETSCOUT attributed ~3,600 DDoS events to this botnet. High-impact DDoS attacks with hundreds of Gbps were frequently observed, as illustrated in Figure 2. The high-bandwidth characteristic was also observed in a later discovered botnet called Aisuru. This notable DDoS characteristic forms a new class of IoT botnets. Due to IP address rotation, exact counts of involved hosts are difficult to determine. Estimates characterize this botnet as a moderately sized network of infected systems in the mid five-digit range of infected hosts at peak.


## Events in August 2025


Mid-August, the renowned cybersecurity journalist Brian Krebs published an article[on the Eleven11 botnet](https://krebsonsecurity.com/2025/08/oregon-man-charged-in-rapper-bot-ddos-service/) , revealing that one of the operators was caught and arrested by authorities—an assertion confirmed via a[press release](https://www.justice.gov/usao-ak/pr/oregon-man-charged-administering-rapper-bot-ddos-hire-botnet) . Our data validates that authorities subsequently seized C2 infrastructure and started to dismantle the botnet’s infrastructure based on a lack of continued activity following the arrest.


Although the botnet has likely been rendered inoperable, compromised devices remain vulnerable. It is likely a matter of time until hosts are hijacked again and conscripted as a compromised node for the next botnet. Therefore, ASERT continues to analyze attack details and the migration of compromised hosts to protect our customers from future threats of this magnitude.


## Recommendations on Alternative DNS Roots


For a large part of RapperBot’s operation, the C2 servers were configured in TXT records of an alternative DNS root with the name OpenNIC. For ordinary businesses, there is no value to having access to OpenNIC domain names. The experiment mainly attracts internet niche technologists, networking geeks, researchers, or simply malicious actors. Consequently, we recommend that customers simply block any name resolution of non-official ICANN domain names. The OpenNIC domain names are:


- .bbs
- .chan
- .cyb
- .dyn
- .geek
- .gopher
- .indy
- .libre
- .neo
- .null
- .o
- .oss
- .oz
- .parody
- .pirate


ASERT recommends blocking these domain names on an enterprise’s resolver.


## References


**ASERT Threat Summary:**


- **Aisuru and related TurboMirai Botnet DDoS:**[https://www.netscout.com/blog/asert/asert-threat-summary-aisuru-and-related-turbomirai-botnet-ddos](https://www.netscout.com/blog/asert/asert-threat-summary-aisuru-and-related-turbomirai-botnet-ddos)
- **New RapperBot Campaign:**[https:](https://www.fortinet.com/blog/threat-research/new-rapperbot-campaign-ddos-attacks)[//www.fortinet.com/blog/threat-research/new-rapperbot-campaign-ddos-attacks](https://www.fortinet.com/blog/threat-research/new-rapperbot-campaign-ddos-attacks)
- **Any.run:**[https://any.run/report/784a025d39e3bd4efc1d5b07075bf07ac294b6ce7d63400429b4f65ac9f360e1/683868b5-8053-4b7f-8155-ff6b2c0fcb8b](https://any.run/report/784a025d39e3bd4efc1d5b07075bf07ac294b6ce7d63400429b4f65ac9f360e1/683868b5-8053-4b7f-8155-ff6b2c0fcb8b)
- **Joe Sandbox:**[https://www.joesandbox.com/analysis/1476565/0/html](https://www.joesandbox.com/analysis/1476565/0/html)
- **Joe Sandbox:**[https://www.joesandbox.com/analysis/1575431/0/lighthtml](https://www.joesandbox.com/analysis/1575431/0/lighthtml)
**OpenNIC Parallel DNS Root:**[https://opennic.org/](https://opennic.org/)
- **XLab’s Analysis of RapperBot:**[https://blog.xlab.qianxin.com/rapperbot-en/](https://blog.xlab.qianxin.com/rapperbot-en/)
- **Krebs on Security Article on RapperBot:**[https://krebsonsecurity.com/2025/08/oregon-man-charged-in-rapper-bot-ddos-service/](https://krebsonsecurity.com/2025/08/oregon-man-charged-in-rapper-bot-ddos-service/)
- **Law Enforcement Press Release:**[https://www.justice.gov/usao-ak/pr/oregon-man-charged-administering-rapper-bot-ddos-hire-botnet](https://www.justice.gov/usao-ak/pr/oregon-man-charged-administering-rapper-bot-ddos-hire-botnet)
