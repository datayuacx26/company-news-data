---
schema_version: "1.0.0"
document_id: "0ddcae0dcf80a7b84de693f3117bc6ae8c475f190cfbbfc013c18f693e869dc5"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-33-8/"
published_at: "2026-08-14T15:21:37+00:00"
first_seen_at: "2026-08-14T16:03:16.987617+00:00"
fetched_at: "2026-08-14T16:03:18.676314+00:00"
content_hash: "sha256:7cae6223bef1cd1ad8f5b202d9f8d9a587ac60cbe3a0ec979b31cb6b667de75c"
---

# The Good, the Bad and the Ugly in Cybersecurity – Week 33

## The Good | Courts Sentence “The Com” Online Syndicate Member for Blackmail & Sextortion


A court in the UK has[sentenced](https://www.nationalcrimeagency.gov.uk/news/com-group-member-sentenced-for-campaign-of-abuse-against-117-victims-worldwide) a member of the decentralized online cybercrime collective known as “The Com” to two years in prison following an investigation by the National Crime Agency (NCA). Justin Swaddle, who operated under the digital aliases ‘Epstein’, ‘Rugen’, and ‘Moscow’ across Discord, Snapchat, and Telegram, pleaded guilty to multiple criminal charges of blackmail and child abuse


. In addition to his sentence, the court ordered Swaddle’s placement on the National Sex Offenders Register and imposed a ten-year Sexual Harm Prevention Order.


Investigators revealed that Swaddle systematically targeted and groomed young, vulnerable victims globally, using popular chat platforms to exploit his targets. The prosecution identified 117 female victims worldwide, aged thirteen to seventeen, whom Swaddle[coerced](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-social-engineering/) into performing severe acts of self-harm and generating explicit material. Rather than seeking financial gain, Swaddle was reportedly motivated by the online status and notoriety he obtained by sharing the media within exclusive subgroups


. When victims resisted his demands, he used video recordings,[home addresses](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-personally-identifiable-information-pii-personal-health-information-phi/) , and school details to[blackmail](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/cyber-extortion/) them into compliance.


The investigation, which the NCA initiated in January 2024 following Swaddle’s initial arrest by West Yorkshire Police, required extensive cross-border coordination. British officers collaborated closely with law enforcement agencies in the United States, Australia, Canada, Norway, and New Zealand to identify and safeguard affected children worldwide.


Authorities emphasize that The Com functions as a highly dangerous, loose-knit global network subdivided into specialized factions, including groups dedicated to physical violence, sexual coercion, financial extortion, and high-profile corporate[ransomware](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-double-extortion/) operations.


## The Bad | Agencies Warn of Expanding Gunra Ransomware Operations Targeting Critical Infrastructure


U.S., U.K., and South Korean intelligence and law enforcement agencies have issued a joint cybersecurity


[advisory](https://content.govdelivery.com/accounts/USDHSCISA/bulletins/4244745) warning global


[critical infrastructure](https://www.sentinelone.com/blog/securing-the-nations-critical-infrastructure-action-plans-to-defend-against-cyber-attacks/) organizations about escalating threats by Gunra ransomware


. First appearing in April 2025 as a variant specializing in double extortion, the group uses malware derived from leaked[Conti](https://www.sentinelone.com/anthology/conti/) source code. Gunra targets public health, financial, and government sectors worldwide, with a heavy concentration of victims in Australia, East Asia, and Europe.


To establish initial access, operators exploit critical authentication vulnerabilities, specifically[CVE-2024-55591](https://nvd.nist.gov/vuln/detail/cve-2024-55591) and[CVE-2025-24472](https://nvd.nist.gov/vuln/detail/CVE-2025-24472) , in FortiOS and FortiProxy software, alongside security flaws in VPN gateways. While campaigns initially focused on Windows environments, the threat actors expanded to cross-platform operations by introducing a Linux variant


. In January 2026, the group launched a formal[Ransomware-as-a-Service](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-ransomware-as-a-service-raas/) (RaaS) affiliate program under the brand “Golden Community”, actively recruiting penetration testers to serve as initial access brokers. Attackers deploy their payloads via[phishing](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/phishing-scams/) and conduct ransom negotiations via WhatsApp.


Once inside a network, the actors utilize Impacket tools for credential dumping and[lateral movement](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/lateral-movement/) . They execute malicious tasks during nighttime hours, exfiltrating stolen documents to cloud services and deleting critical backup and archived data across primary and recovery centers. The malware leverages advanced ciphers like Salsa20 or ChaCha20 to encrypt terabytes of data in a limited timeframe.


Strong links have been


[identified](https://asec.ahnlab.com/en/93421/) between Gunra and North Korean state-backed threat actors


, observing overlapping infrastructure and techniques, such as the exploitation of zero-day flaws in certificate signing software. Despite its sophistication, a catastrophic[cryptographic flaw](https://intel.breakglass.tech/post/gunra-ransomware-s-linux-variant-has-a-fatal-flaw-time-seeded-rand-makes-encrypted-files-recoverable-without-paying#recovery-procedure-for-incident-responders) in Gunra’s Linux variant allows victims to fully recover encrypted files.


## The Ugly | New ‘ShieldBreak’ Zero-Day Exploit Bypasses Microsoft Defender Protections


A security researcher known as ‘Nightmare Eclipse’ has released a novel Microsoft Defender zero-day exploit dubbed


‘[ShieldBreak](https://blog.projectnightcrawler.dev/posts/2026-08-11-shieldbreak-august-2026-disclosure/) ’ shortly after this month’s Patch Tuesday


[update](https://msrc.microsoft.com/update-guide/releaseNote/2026-Aug) . The vulnerability operates as a direct patch bypass for[RoguePlanet](https://msrc.microsoft.com/update-guide/en-US/advisory/CVE-2026-50656) , a separate privilege escalation flaw in Microsoft’s malware protection engine that was patched in July.


ShieldBreak PoC exploit demo (Source: Nightmare Eclipse)


Although both flaws lead to SYSTEM-level compromise, researchers[confirm](https://cyberplace.social/@GossiTheDog/117082629057300721) the underlying exploitation techniques differ significantly. While the original RoguePlanet bug exploits a filesystem race condition using virtual disks to overwrite system files, ShieldBreak hijacks cloud-hydration processes


.


Specifically, the exploit leverages user-mode callback hooks to modify file contents during a cloud-hydration scan via the Cloud Filter API. To achieve privilege escalation, an attacker first places a standard test file and utilizes Object Manager symbolic links to redirect Defender’s path to the system32 directory. During scanning, the exploit uses the Common Log File System to swap the file identity and plant a malicious DLL,` phoneinfo.dll` , where a default system file does not exist. Triggering a scheduled Windows Error Reporting task subsequently forces the system to load this rogue library, spawning a shell with highest privileges.


The proof-of-concept operates with a 100% success rate on fully[patched](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-patch-management/) installations of Windows 11 25H2 and Windows Server 2025. Although Windows 10 remains vulnerable to the flaw, the current code does not natively support those legacy systems. Analysts


[note](https://infosec.exchange/@wdormann/117079587486018149) that Microsoft Defender must be actively enabled for the exploit chain to function


.


The release intensifies an ongoing[dispute](https://www.microsoft.com/en-us/msrc/blog/2026/05/a-shared-responsibility-protecting-customers-through-coordinated-vulnerability-disclosure) between Microsoft and the researcher over bug bounty policies and recent[threats](https://x.com/msftsecresponse/status/2061293718942908925) of legal action.
