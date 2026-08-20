---
schema_version: "1.0.0"
document_id: "b8a000cc10bf7ed1d3b745616cb11dee4e46a41980dfb8cd9c55cb3536aea206"
company_key: "yc-trm-labs"
company: "TRM Labs"
source_id: "yc-trm-labs-news-import-b34814ebf689"
canonical_url: "https://www.trmlabs.com/resources/blog/ofac-sanctions-firstvpn-and-ransomware-enablers-behind-attacks-on-americans"
published_at: "2026-07-13T20:56:00+00:00"
first_seen_at: "2026-07-22T17:19:07.599741+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:e774410e46c543476e39841ea9c3c1fe4b13d29c40affc5a431d5cab7186aabe"
---

# OFAC Sanctions FirstVPN and Ransomware Enablers Behind Attacks on Americans

## Key takeaways


- On July 13, 2026, OFAC designated three parties enabling ransomware attacks against Americans: the FirstVPN Service (1VPNS), a VPN provider selling anonymizing infrastructure to ransomware groups; its administrator, Dmytro Rashevskyi; and Yevgeniy Vladimirovich Silayev, a Belarusian national who sells "cryptors" — tools that disguise malware so security systems fail to detect it.
- The action targets the enabling layer of the ransomware economy rather than a ransomware group itself. VPNs and cryptors are the infrastructure that lets operators hide the origin of attacks and slip malware past defenses, and OFAC is now treating those service providers as sanctionable in their own right.
- TRM Labs already attributes on-chain addresses to FirstVPN, and blockchain tracing shows ransomware operators paying the service directly: the Anubis ransomware group sent a total of USD 715 to FirstVPN in December 2025 and March 2026 — small in dollar terms, but direct evidence that ransomware crews relied on the service for operational infrastructure.
- The designation was coordinated with the United Kingdom's FCDO, which sanctioned other cybercriminals and their enablers the same day, and follows a May 2026 takedown of 1VPNS's website and infrastructure by European law enforcement with support from the FBI's Boston Field Office.
- Compliance teams should screen the newly listed addresses now and watch the enabler pattern — anonymity and obfuscation services paid in crypto across multiple chains — not just ransomware-group wallets themselves.


{{horizontal-line}}


## Overview


On July 13, 2026, OFAC[designated](https://home.treasury.gov/news/press-releases/sb0559) two individuals and one entity for enabling ransomware actors and other cybercriminals. The action names the FirstVPN Service (FirstVPN, 1VPNS), a virtual private network provider whose principal clients include ransomware groups; its administrator, Dmytro Rashevskyi; and Yevgeniy Vladimirovich Silayev, a cryptor who sells a service designed to obfuscate malware. Ransomware groups using these services have caused billions of dollars in losses to US businesses and critical infrastructure, according to OFAC.


This action goes after the services ransomware groups buy to operate — an anonymizing VPN service, and a malware-obfuscation service — rather than a ransomware group itself. OFAC issued it in furtherance of Executive Order 14390, the March 2026 order directing US agencies to harden financial and digital systems against foreign cybercrime, and pursuant to E.O. 13694 as amended, the standing cyber-sanctions authority.


## OFAC targets the infrastructure behind ransomware attacks


OFAC designated FirstVPN and Rashevskyi for providing material and technological support to cyber-enabled activity, including ransomware attacks, against US persons and critical infrastructure. Silayev was designated on the same basis for supplying encryption and obfuscation services to ransomware operators targeting US and allied entities.


The action was coordinated with the United Kingdom's Foreign, Commonwealth & Development Office (FCDO), which sanctioned other cybercriminals and their enablers the same day. It also follows a May 2026 takedown of 1VPNS's website and other infrastructure by European law enforcement authorities, supported by the FBI's Boston Field Office. The FBI released a cybersecurity advisory describing 1VPNS's tactics, techniques, and procedures to help businesses detect and prevent ransomware attacks.


## The designated parties


### FirstVPN Service (FirstVPN, 1VPNS)


FirstVPN is a VPN provider whose principal clients include ransomware actors and other cybercriminals. VPNs have legitimate privacy and security uses, but FirstVPN was built to serve the other side of that line. Since 2014 it has advertised on cybercriminal forums, including Exploit and XSS, marketing itself on the promise that it keeps no logs of users' identities or activity and refuses to cooperate with law enforcement investigations into activity originating from the servers it rents. Its operators also ran a peer-to-peer Jabber messaging service.


Numerous ransomware groups purchased infrastructure from FirstVPN and used it to hide the origin of attacks, deploy malware, and manage exfiltrated data. According to OFAC, victims of attacks that involved FirstVPN infrastructure have included US businesses, financial services companies, hospitals, and municipal governments. To keep the service running, Rashevskyi used false identities ("Maksim Sorin" and "Roman Chabanenko") to buy infrastructure from providers that might otherwise have refused him because of abuse complaints tied to FirstVPN servers.


Today’s action included five cryptocurrency addresses spanning Bitcoin, Ethereum, Litecoin, and Tron, all of which appear to be located at the high-risk exchange,[Cryptomus](https://www.trmlabs.com/resources/blog/russia-linked-payment-processor-cryptomus-likely-behind-launch-of-parallel-service-heleket) .


### Dmytro Rashevskyi


Rashevskyi is FirstVPN’s administrator and was designated alongside the service for providing technological support to illicit actors. OFAC listed 15 cryptocurrency addresses attributed to Rashevskyi spanning Bitcoin, Ethereum, Tron, Litecoin, Dogecoin, Dash, Zcash, and Solana.


### Yevgeniy Vladimirovich Silayev


Silayev, a Belarusian national, is a cryptor designated alongside Rashevskyi and FirstVPN. A cryptor is built specifically to make malware stealthier by disguising it as a harmless file so that security systems fail to detect or deactivate it. Silayev supplied these obfuscation services to ransomware operators targeting U.S. and allied entities. While his listing did not include any cryptocurrency addresses, paired with the FirstVPN action, it shows OFAC treating both halves of the attack-enablement stack, anonymity on the network and evasion on the endpoint, as sanctionable.


## The on-chain trail from ransomware groups to FirstVPN


TRM Labs began tracing FirstVPN’s blockchain activity before the designation landed. That tracing shows ransomware operators paying the service directly. The Anubis ransomware group sent a total of USD 715 to FirstVPN across two windows, on December 13, 2025, and again on March 15–16, 2026. On January 11, 2026, Qilin Ransomware sent a total of USD 120 to FirstVPN; on February 8, 2026, Sinobi Group sent USD 58 to FirstVPN. The dollar amounts are small, as infrastructure subscriptions tend to be, but shows that named ransomware groups paid a named service for operational infrastructure, on-chain, and that payment is visible. Enabling services get paid in the same rails their customers extort victims through, and those payments are traceable.


‍


{{horizontal-line}}


## Frequently asked questions (FAQs)


### 1. Who exactly did OFAC designate, and under what authority?


OFAC designated the FirstVPN Service (1VPNS), an entity; its administrator, Dmytro Rashevskyi; and Yevgeniy Vladimirovich Silayev, a Belarusian cryptor vendor. The designations were issued in furtherance of Executive Order 14390 (March 2026) and pursuant to E.O. 13694 as amended — the U.S. cyber-sanctions authority. All property of the listed parties in the U.S. or held by U.S. persons is blocked, and the 50 percent rule extends the block to entities they own.


### 2. Why sanction a VPN service and a "cryptor" instead of a ransomware group?


Because they are the infrastructure ransomware groups depend on. A VPN like 1VPNS hides where an attack comes from; a cryptor disguises the malware so it evades detection. Targeting these enablers raises the cost and difficulty of running ransomware operations at all, rather than pursuing one crew at a time. It reflects the E.O. 14390 mandate to harden systems against the broader cybercrime ecosystem.


### 3. What is a cryptor?


A cryptor is a tool built to make malware stealthier by disguising it as a harmless file, so that antivirus and other security systems fail to detect or deactivate it. Unlike legitimate encryption, which protects data and its owner's privacy, a cryptor's purpose is to defeat defenses. Silayev sold these services to ransomware operators.


### 4. Does TRM have visibility into FirstVPN's on-chain activity?


Yes. TRM already maintains an attributed entity for FirstVPN, and its blockchain activity was tracked prior to the designation. Tracing shows ransomware operators paying the service directly — for example, the Anubis ransomware group sent a total of USD 715 to FirstVPN in December 2025 and March 2026. The newly listed SDN addresses are covered in TRM's data across the chains on which they were issued.


### 5. What should compliance teams do right now?


Screen the newly designated FirstVPN and Rashevskyi addresses across all listed chains and identify any direct or indirect exposure. Beyond the specific addresses, treat payments to anonymity and obfuscation infrastructure as a risk signal, and build an SDN check into ransomware incident-response tracing so infrastructure payments surfaced during an investigation are screened against the sanctions list.
