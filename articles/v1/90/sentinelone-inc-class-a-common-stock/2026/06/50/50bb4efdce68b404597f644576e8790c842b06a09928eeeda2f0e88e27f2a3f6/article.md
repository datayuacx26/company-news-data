---
schema_version: "1.0.0"
document_id: "50bb4efdce68b404597f644576e8790c842b06a09928eeeda2f0e88e27f2a3f6"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-25-7/"
published_at: "2026-06-19T13:00:54+00:00"
first_seen_at: "2026-07-20T23:17:13.754958+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:1c8d44ab42864e9240d70813f64805bf688087bb1bf4cda00ebb2bae4c95a6f5"
---

# The Good, the Bad and the Ugly in Cybersecurity – Week 25

## The Good | Authorities Dismantle PhaaS Network & Clean Sites Infected with SocGholish


A coordinated action between government and the private sector led by the FBI has led to the dismantling of Outsider Enterprise, a Chinese Phishing-as-a-Service (PhaaS) operation


.


Operating since 2023, the syndicate combined AI and distributed[phishing](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/phishing-scams/) kits to impersonate trusted brands across millions of fraudulent[SMS](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-smishing/) messages sent from major U.S. telecommunications carriers. Investigators are attributing an estimated $1.9 billion in financial losses to the operation, alongside the theft of roughly 3.8 million credit card records.


Source: FBI


Federal authorities were able to seize multiple administrative servers, a Shopify storefront, a Telegram bot containing customer data, and approximately $100,000 in cryptocurrency. Simultaneously, Google[disabled](https://blog.google/innovation-and-ai/technology/safety-security/combatting-ai-scams/) thousands of associated domains and filed a civil lawsuit against the infrastructure operators. The tech giant is now actively coordinating with AT&T, T-Mobile, and Verizon to aggressively block the fraudulent text messages before they reach targeted users.


In another joint effort involving Europol and Eurojust, law enforcement teams globally removed SocGholish malware infections from nearly 15,000 compromised WordPress websites and dismantled over 100 associated command servers


.


This initiative is another arm of[Operation Endgame](https://www.europol.europa.eu/how-we-work/operations/operation-endgame) , a broader campaign specifically targeting infrastructure linked to the notorious Russian-based cybercrime syndicate[Evil Corp](https://www.sentinelone.com/labs/sanctions-be-damned-from-dridex-to-macaw-the-evolution-of-evil-corp/) .


The[SocGholish](https://www.sentinelone.com/labs/socgholish-diversifies-and-expands-its-malware-staging-infrastructure-to-counter-defenders/) malware, a JavaScript-based downloader active since 2017, operates by hijacking legitimate websites to deceive visitors into installing malicious payloads disguised as routine browser updates. Upon installation, the malware establishes remote access, enabling threat actors to deploy secondary malware and ransomware families.


Following the extensive technical cleanup, authorities strongly[advised](https://www.politie.nl/en/news/2026/juni/18/11-international-law-enforcement-initiate-hunt-on-malware-group-socgholish.html) affected site administrators to implement[multi-factor authentication](https://www.sentinelone.com/cybersecurity-101/identity-security/what-is-multi-factor-authentication-mfa/) (MFA) and update their platform software. Officials also emphasize that this massive disruption actively prevents further damage to global networks and marks the beginning of sustained enforcement against the botnet.


## The Bad | DragonForce Abuses Microsoft Teams Relays to Conceal Backdoor Traffic


[DragonForce](https://www.sentinelone.com/blog/dragonforce-ransomware-gang-from-hacktivists-to-high-street-extortionists/) , a cartel-like ransomware operation established in 2023, is now using a custom Go-based malware, dubbed` Backdoor.Turn` , to conceal command-and-control (C2) communications within legitimate Microsoft Teams relay infrastructure.


Security researchers recently[observed](https://www.security.com/threat-intelligence/dragonforce-msteams-backdoor) an attack on a major U.S. services company where DragonForce actors obtained an anonymous Teams visitor token backed by Skype identity services.


The malware subsequently leverages Microsoft’s Traversal Using Relays around NAT ([TURN](https://techcommunity.microsoft.com/discussions/azurevirtualdesktopforum/turn-relay-regional-expansion-for-azure-virtual-desktop/4419721) ) protocol during connection setup, establishing a direct QUIC session to the attacker’s C2 server. Investigations learned that network defenders only observed outbound traffic directed toward trusted Microsoft servers, allowing the attackers to remain undetected on the compromised network for up to two months.


Researcher say the intrusion likely began in December 2025 following the exploitation of an SQL or MSSQL server vulnerability. Attackers at that point executed a[PowerShell](https://www.sentinelone.com/cybersecurity-101/cybersecurity/windows-powershell/) command to drop a ZIP archive disguised as a technical support hotfix, initiating a DLL side-loading sequence.


To achieve kernel-level privileges and actively terminate host security tools, the attackers employed extensive Bring Your Own Vulnerable Driver (BYOVD) techniques. They systematically deployed several vulnerable drivers, including a Huawei audio driver and a custom[ABYSSWORKER](https://www.elastic.co/security-labs/abyssworker) payload masquerading as legitimate Palo Alto software.


Source: Symantec/Carbon Black/Broadcom


The deployment of` Backdoor.Turn` marks the first documented in-the-wild abuse of Microsoft Teams TURN relays for C2 communications


, building upon the theoretical “[Ghost Calls](https://www.praetorian.com/blog/ghost-calls-abusing-web-conferencing-for-covert-command-control-part-2-of-2/) ” technique demonstrated in previous research.


Beyond deploying the DragonForce ransomware and exfiltrating data, the attackers injected the` Backdoor.Turn` remote access trojan into a legitimate debugger process to establish long-term persistence.


The backdoor provides the cartel with extensive post-exploitation capabilities, including network scanning, active directory[reconnaissance](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-cyber-reconnaissance/) , browser[credential theft](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-credential-theft/) , and[lateral movement](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/lateral-movement/) , rounding out the group’s sophisticated cyber tradecraft.


## The Ugly | PRC-Based Spies Breach REDCap Servers to Steal Medical Research


A China-linked espionage group, tracked as UNC6508, breached exposed REDCap servers to steal sensitive research data from a North American medical institution


. Cyber researchers[report](https://cloud.google.com/blog/topics/threat-intelligence/prc-targets-us-medical-research) that the attackers initially compromised the network back in September 2023 by probing vulnerable, legacy versions of the REDCap platform, which is widely utilized for managing scientific databases. Following the initial intrusion, the threat actors remained undetected within the victim’s environment until November 2025.


Three months after gaining access, the operators deployed a custom malware dubbed “InfiniteRed” specifically designed for REDCap environments. UNC6508 concealed these components by actively trojanizing the server’s underlying system files.


InfiniteRed operates through three primary modules: a persistence mechanism, a targeted credential harvester, and a versatile backdoor


. The harvester intercepts user logins,[encrypting](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-encryption/) and storing them directly within local database tables. In tandem, the backdoor receives commands via HTTP cookies, granting the attackers extensive capabilities to execute shell commands, run arbitrary SQL queries, and transfer files across the compromised infrastructure.


Source: GTIG


Considered a novel approach for China-linked actors, UNC6508 leveraged legitimate content compliance features within enterprise productivity tools to successfully exfiltrate data. By creating a compliance rule named “Patroit”, the attackers automatically forwarded emails containing specific keywords, such as those related to military readiness, molecular discovery, and geo-strategic policy, to an external, attacker-controlled email account. The operation maintained high operational security by utilizing U.S.-based residential proxies and compromised[routers](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-iot-security/) .


To mitigate these threats, administrators are strongly advised to immediately update all REDCap instances to the latest versions, enforce MFA, and implement


[Device Bound Session Credentials](https://developer.chrome.com/docs/web-platform/device-bound-session-credentials) (DBSC) to prevent ongoing session hijacking


. Indicators of compromise (IoCs) and YARA rules can be found[here](https://cloud.google.com/blog/topics/threat-intelligence/prc-targets-us-medical-research) .
