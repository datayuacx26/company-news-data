---
schema_version: "1.0.0"
document_id: "162b3bb14ceb24a6591c798f30898f07bb158be5f8607b5d7f6d5eec10e7f3f0"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/malware-bypass-dns-direct-to-ip/"
published_at: "2026-08-04T12:50:53+00:00"
first_seen_at: "2026-08-04T13:25:28.791548+00:00"
fetched_at: "2026-08-04T14:03:42.322861+00:00"
content_hash: "sha256:a00ac31762a1c402cde61d3fa5c045bdd9f0f816d01199b07399a7b6dc965acb"
---

# Almost Half of Malware Samples Communicate Direct to IP

## **Executive Summary**


Malware samples often bypass DNS entirely, communicating directly to IP addresses instead. Our analysis of 4 million dynamic analysis reports indicates that almost half (45.32%) of malware samples with any command-and-control (C2) activity made at least one direct-to-IP (D2IP) address connection. Measured as a fraction of all C2 connection attempts, D2IP traffic accounts for 23.17% of the total.


A wide variety of threats — including ransomware droppers, peer-to-peer (P2P) botnets and supply chain risks — communicate directly with hard-coded IP addresses, bypassing DNS entirely and evading DNS-based defenses altogether.


This article introduces zero trust IP (ZT-IP), which is a network-level enforcement approach that applies zero trust principles to IP-based traffic. The enforcement approach verifies whether outbound connection destinations were ever sanctioned by a DNS response. We validate this approach against real-world network traffic and samples, demonstrating how ZT-IP successfully surfaces threats including:


- Phorpiex ransomware droppers connecting directly to C2 IP addresses
- A persistent data exfiltration campaign using a custom obfuscated HTTP GET


request
- Mozi P2P botnet payloads delivered to internet-of-things (IoT) devices without DNS


Palo Alto Networks customers are better protected from the threats discussed here through the following products and services:


- [Advanced WildFire](https://docs.paloaltonetworks.com/wildfire)
- [Advanced URL Filtering](https://docs.paloaltonetworks.com/advanced-url-filtering) and[Advanced DNS Security](https://docs.paloaltonetworks.com/dns-security)
- [Cortex XDR](https://www.paloaltonetworks.com/cortex/cortex-xdr?_gl=1*13pmp8e*_ga*NzQyNjM2NzkuMTY2NjY3OTczNw..*_ga_KS2MELEEFC*MTY2OTczNjA2MS4zMS4wLjE2Njk3MzYwNjEuNjAuMC4w) and[XSIAM](https://www.paloaltonetworks.com/resources/datasheets/cortex-xsiam-aag)


If you think you might have been compromised or have an urgent matter, contact the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) .


**Related Unit 42 Topics** **[Malware](https://unit42.paloaltonetworks.com/category/malware/) ,[DNS](https://unit42.paloaltonetworks.com/category/dns/) ,[Ransomware](https://unit42.paloaltonetworks.com/category/ransomware/)**


## **The DNS Visibility Gap**


DNS security has become a cornerstone of enterprise threat defense. By monitoring and filtering DNS queries, security teams can block malware from reaching known-bad domains and use sinkholing to disrupt C2 communications before they establish a foothold. This approach is effective when malware plays by the rules of relying on DNS for domain resolution.


However, many types of malware do not. For example, a[backdoor malware sample](https://www.virustotal.com/gui/file/574e112d91c62155fb725415ee4096a9b36487fb6a4d9851baceef0973a22540/) made no DNS query at all before initiating a WebSocket connection directly to an IP address. Disassembly via Ghidra revealed why. The destination address wss://154.92.19\[.\]71:39989


was hard coded into the binary as a Unicode string.


This is not an isolated edge case. Without the DNS resolution step in network communications, malware is invisible to DNS-based security controls, protective DNS sinkholing and DNS anomaly detection systems. The connection simply appears as raw IP traffic with no prior context.


### Prevalence of IP Traffic in Malware


We analyzed over 4 million Advanced WildFire dynamic analysis reports for a 30-day period to quantify how prevalent this behavior truly is. We filtered out connections to common legitimate services, internal addresses and DNS resolvers. After doing that, we found that 20.11% of malware samples exhibited C2 activity.


The contrast with benign samples is striking. Only 1% of benign samples establish connections to untrusted IP addresses after applying the same filtering criteria. Those that do average just 1.6 such connections per sample. This is a small fraction of the activity observed in malware.


Among malware with C2 connections, TCP dominates (94.43% prevalence, averaging 4.17 unique C2 IP addresses per sample). UDP is present in 17.50% of cases but contacts far more IP addresses per sample (average 13.79%), consistent with scanning and P2P mesh behavior.


Most critically for DNS-based defenses, 45.32% of malware samples with any C2 activity made at least one direct-to-IP (D2IP) address connection — a raw IP contact with no preceding DNS query. Even after excluding bulk port-scanning behavior, the figure remains 41.97%.


Measured as a fraction of all C2 connection attempts, D2IP traffic accounts for 23.17% of the total.


## **Threats Discovered Through ZT-IP Analysis**


By searching for D2IP connections (ZT-IP analysis), we were able to uncover multiple threats.


### Phorpiex Ransomware Dropper


Among traffic flagged by ZT-IP, we observed the following suspicious HTTP GET


request toward the destination IP address at 178.16.54\[.\]109


as shown below in Figure 1.


Figure 1. Suspicious HTTP GET request to 178.16.54\[.\]109


. The observed request exhibits multiple variations, including sequential numeric


GET


paths (e.g.,


/1


through


/6


) and specific file downloads (e.g.,


/sex/k/n.txt


,


/new.php


).


We found that these requests correlate with several malware samples (e.g., the


[binary](https://www.virustotal.com/gui/file/bf24277400cc453d530e4277d3bd24e96c5e409adef6970518bdc59205aa0241) retrieved from


hxxp\[:\]//178.16.54\[.\]109/st.exe


) associated with Phorpiex (aka Trik), a long-running malware family used primarily as a dropper and spam botnet. Figure 2 illustrates the many malicious samples hosted on


178.16.54\[.\]109


in a flow chart of this activity.


Figure 2. The IP address 178.16.54\[.\]109


is used to deliver multiple threats.This traffic aligns with Phorpiex's staged payload delivery mechanism. This mechanism is characterized by initially fetching configuration, and then retrieving additional malicious components such as a ransomware payload.


A key indicator of malicious D2IP activity was the absence of DNS queries preceding the connections to 178.16.54\[.\]109


. Since the request bodies appear benign, they often bypass standard heuristic detectors.


### The \\GET Exfiltration Campaign


One of the most distinctive findings was a persistent campaign using a non-standard HTTP request we call the \\GET


protocol. This is an obfuscated request format designed to evade security detection while exfiltrating data.


Unlike standard HTTP requests, these requests begin with \\GET


(backslash- GET


) rather than standard GET


followed by a backslash. The \\GET


is followed by a long encoded string ranging from 250–666 characters, as shown in Figure 3.


Figure 3. \\GET


protocol request to 18.228.188\[.\]56


.Statistical analysis of the encoded payloads reveals a structured encoding scheme:


- The first and last four characters: alphanumeric ( 0-9


and A-Z


), evenly distributed
- Middle characters: hex-like ( 0-9


and A-F


), evenly distributed
- The payload length ranges from 250–666 characters


Disassembly of associated malware samples (e.g., cc43cdbe8eb9874f55fffbe23b560b673eb9f31fb9a953926bba29464fd2dd07


) confirms there are no hex-format IP addresses hard coded in the binaries. Instead, multiple hard-coded strings follow the same encoding rule, consistent with the payload lengths observed.


The C2 infrastructure is hosted on public cloud infrastructure in Brazil. Samples that point to it were found impacting high-value sectors including government, airlines and universities. Because the threat leverages shared cloud resources, simple IP-based blocking is ineffective.


Furthermore, the malware employs a notable operational security measure. It rotates both its destination port and IP address on a regular schedule, which evades static IP-blocking defenses.


### SectopRAT


We observed two separate attackers operating infrastructure at 87.120.107\[.\]33


and 194.76.227\[.\]94


, respectively, deploying the same malware family of SectopRAT against educational institutions. Both actors leverage SectopRAT's in-browser proxy capability to silently mirror all victim browser traffic to attacker-controlled servers in real time.


The proxy operates via two endpoints. The first, /churl


(shown in Figure 4), relays every URL the victim visits, including:


- Authenticated session pages
- Single sign-on (SSO) redirects
- Learning management system content


Figure 4. HTTP GET request using /churl


.


The second, /fsave


(shown in Figure 5), exfiltrates form fields including usernames and plaintext passwords at the moment of submission.


Figure 5. HTTP GET request using /fsave


.


An identifier ( pcid/clid


) in every request allows each actor to track individual compromised devices across sessions. Both attackers’ campaigns communicate entirely over D2IP connections, bypassing DNS-based visibility layers entirely.


### IoT Botnets


IoT malware families often exploit network vulnerabilities to self-propagate, relying on D2IP communication that makes them invisible to DNS-based security layers.


Mozi is a Mirai-derived P2P botnet known for targeting IoT devices with MIPS, ARM and x86 architectures. We observed its characteristic exploitation payload embedded directly in HTTP requests. Figure 6 shows an example of the exploitation payload for Mozi.


Figure 6. Example of an IoT exploitation payload embedded in an HTTP request.


In ASCII text, this translates to the three commands shown in Figure 7.


Figure 7. Commands from the IoT exploitation, listed in sequential order.


The Mozi campaign showed highly distributed patterns with thousands of source and destination IP addresses, consistent with the Mozi botnet's P2P mesh structure. However, because this IoT malware propagates by making D2IP connections without DNS resolution, blocking this type of connection can effectively prevent it from spreading within sensitive environments.


Similarly, we identified a Mirai variant that we named Boatnet, which was distributed from the IP address 2.26.98\[.\]67


.


Boatnet demonstrates thorough platform targeting, covering at least 14 different architectures. Beyond standard IoT architectures, Boatnet includes binaries for m68k (Motorola 68000). This processor family is common in legacy industrial control systems (ICS) and aging embedded hardware, which current malware typically overlooks. Unlike generic variants, Boatnet fragments its ARM coverage into three discrete sub-variants (ARM5, ARM6 and ARM7) to ensure optimal performance across different hardware generations.


Binaries are hosted under a /hiddenbin/


directory, as noted in Figure 8, showing a deliberate attempt to mimic legitimate system paths and evade basic file-path detection.


Figure 8. HTTP GET request showing a Boatnet binary under a /hiddenbin/


directory.


We observed infected devices fetching payloads with a User-Agent string of Wget/1.13.4


(released in 2012). This serves as a significant fingerprint, suggesting the botnet is specifically harvesting older, unpatched hardware. Boatnet also demonstrates more operational maturity than “vanilla” Mirai through its use of HTTP Range headers.


## **Conclusion**


D2IP communication from malware won’t be detected by approaches that focus specifically on securing requests at the time a hostname is resolved. This is illustrated by threats like the Phorpiex ransomware dropper, the obfuscated \\GET


exfiltration campaign and the P2P Mozi botnet.


ZT-IP closes this critical gap by applying strict zero trust principles to outbound D2IP connections, ensuring only DNS-sanctioned traffic is permitted. It is a necessary network-level enforcement mechanism to block what DNS-based detection cannot see.


- [Advanced WildFire](https://docs.paloaltonetworks.com/wildfire) machine-learning models and analysis techniques have been reviewed and updated in light of the IoCs shared in this research.
- [Advanced URL Filtering](https://docs.paloaltonetworks.com/advanced-url-filtering) and[Advanced DNS Security](https://docs.paloaltonetworks.com/dns-security) identify known domains and URLs associated with this activity as malicious.
- [Cortex XDR](https://www.paloaltonetworks.com/cortex/cortex-xdr?_gl=1*13pmp8e*_ga*NzQyNjM2NzkuMTY2NjY3OTczNw..*_ga_KS2MELEEFC*MTY2OTczNjA2MS4zMS4wLjE2Njk3MzYwNjEuNjAuMC4w) and[XSIAM](https://www.paloaltonetworks.com/resources/datasheets/cortex-xsiam-aag) help to prevent the threats described in this article, by employing the[Malware Prevention Engine](https://docs-cortex.paloaltonetworks.com/r/Cortex-XDR/Cortex-XDR-4.x-Documentation/Malware-protection) . This approach combines several layers of protection, including[Advanced WildFire](https://docs.paloaltonetworks.com/wildfire) , Behavioral Threat Protection and the Local Analysis module, to prevent both known and unknown malware from causing harm to endpoints.


If you think you may have been compromised or have an urgent matter, get in touch with the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) or call:


- North America Toll-Free: 866.486.4842 (866.4.UNIT42)
- EMEA: +31.20.299.3130
- APAC: +65.6983.8730
- Japan: +81.50.1790.0200


Palo Alto Networks has shared these findings with our fellow Cyber Threat Alliance (CTA) members. CTA members use this intelligence to rapidly deploy protections to their customers and to systematically disrupt malicious cyber actors. Learn more about the[Cyber Threat Alliance](https://www.cyberthreatalliance.org/) .


## **Indicators of Compromise**


**Malicious destination IP addresses**


- 2.26.98\[.\]67


- 62.60.179\[.\]230


- 87.120.107\[.\]33


- 91.92.243\[.\]29


- 103.245.236\[.\]146


- 178.16.54\[.\]31


- 178.16.54\[.\]109


- 194.76.227\[.\]94


- 206.189.229\[.\]43


**Malware samples**


- 01a96eeafb72042b3f69afd21b4c9155dbfe7f97ab3dca392972ad531a075ac2


- 9639f7ebc6a6d69d7bf5b8bc869e7783a1406088f192868624ad8919e9bfd1d4


- bf24277400cc453d530e4277d3bd24e96c5e409adef6970518bdc59205aa0241


- e310476c41ae4f6e3c4ed9bb88303ee6e5e1455bd7afe51cf48965ea7599e6e5


- e3513922666c202c1ae5c06eea277ba10477868d6d89ce2819f4f8ff9070bc85


- e5715e6611ef6bcb233f5d2098510dab3db408abbb728b00e1821bb255829373


## **A** dditional Resources


- [Phorpiex (Malware Family)](https://malpedia.caad.fkie.fraunhofer.de/details/win.phorpiex) – Malpedia
- [Old Wine in the New Bottle: Mirai Variant Targets Multiple IoT Devices](https://unit42.paloaltonetworks.com/mirai-variant-iz1h9/) – Palo Alto Networks Unit 42
- [It Was Not Me! Malware-Initiated Vulnerability Scanning Is on the Rise](https://unit42.paloaltonetworks.com/malware-initiated-scanning-attacks/) – Palo Alto Networks Unit 42


## **Appendix: What Is ZT-IP?**


ZT-IP, which we have also called[No-DNS](https://docs.paloaltonetworks.com/advanced-ip-defense/getting-started/introducing-advanced-ip-defense/advanced-ip-defense-no-dns-detection) or[D2IP detection](https://docs.paloaltonetworks.com/advanced-ip-defense/getting-started/introducing-advanced-ip-defense) , extends the zero trust security model to IP-based traffic. This ensures that network services only connect to IP addresses previously resolved and approved by a trusted DNS service. Outbound connections to an IP address without a prior, valid DNS query are flagged as suspicious and blocked.


In Figure 9, ZT-IP secures the network and firewall level through a simple enforcement mechanism:


Figure 9. The security model of ZT-IP.


**The mechanism:**


- Tracking legitimate IP addresses: The system monitors all valid DNS responses to learn which IP addresses are approved for outbound connections
- Building a trust list: These approved IP addresses are tracked in a secure list, along with a time limit for how long they remain valid
- Enforcement: Whenever a device attempts an outbound connection to an IP address, the firewall checks this list
- Blocking bypass attempts: The connection is only allowed if the destination IP address was recently approved by DNS. If an IP address is not on the list or its approved time limit has expired, the connection is immediately flagged as a potentialNo-DNS threat and blocked.


Because some legitimate network protocols (such as DNS, Neighbor Discovery Protocol, VoIP, P2P and certain IoT services) connect directly to IP addresses by design, ZT-IP incorporates mechanisms to prevent false positives. It uses allowlists that exempt private intranet IP addresses, specific trusted network protocols and well-known benign public IP addresses from these strict DNS checks.


**ZT-IP delivers the following key advantages for network defense:**


- Stops patient zero attacks: It proactively blocks unknown C2 callbacks and malware attempting to phone home using hard-coded IP addresses
- Secures shared infrastructure: It differentiates between legitimate traffic and malicious connections targeting the same shared cloud hosting environments, securely blocking the malicious traffic because it lacks the necessary DNS context
- Network-level protection: By operating at the firewall level, ZT-IP secures environments like IoT and operational technology (OT) without requiring endpoint agents to be installed on every single device
