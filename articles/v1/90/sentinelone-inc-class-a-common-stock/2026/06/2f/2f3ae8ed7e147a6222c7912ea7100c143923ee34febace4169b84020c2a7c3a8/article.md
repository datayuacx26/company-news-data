---
schema_version: "1.0.0"
document_id: "2f3ae8ed7e147a6222c7912ea7100c143923ee34febace4169b84020c2a7c3a8"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-26-7/"
published_at: "2026-06-26T13:00:00+00:00"
first_seen_at: "2026-07-20T23:17:13.754958+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:65c6517c682b49da56460aa7342129fcbfc539b19a5b98f4702d5521c3b152cc"
---

# The Good, the Bad and the Ugly in Cybersecurity – Week 26

## The Good | Authorities Dismantle Malware Networks and Seize Cambodian Scam Infrastructure


Following the seizure of a major Phishing-as-a-Service[last week](https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-25-7/) , the latest move, part of Operation Endgame, dismantled operational infrastructure supporting the Amadey and StealC malware families


.


The joint effort by[Europol](https://www.europol.europa.eu/media-press/newsroom/news/global-cyber-strike-disrupts-socgholish-amadey-and-stealc-malware-networks) and private sector partners successfully took 326 servers and 142 malicious domains offline. Investigators recovered approximately 27 million stolen credentials and identified over $47 million in cryptocurrency tied to illicit activities.


Cybercriminals had extensively utilized the malware-as-a-service operation to establish initial network access, harvest sensitive data, and deploy secondary[ransomware](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-double-extortion/) payloads. Officials emphasize this disruption degrades the assembly lines fueling global cybercrime and financial fraud.


StealC panel build generation feature (Source: WeLiveSecurity)


Two members of the UNC3944 ( *aka* Scattered Spider) cybercrime syndicate have


[pleaded](https://www.nationalcrimeagency.gov.uk/news/cyber-criminals-who-hacked-into-transport-for-londons-computer-network-are-convicted) guilty to breaching the Transport for London systems in September 2024


. Owen Flowers and Thalha Jubair admitted to orchestrating the cyberattack, which caused an estimated £29 million in financial damage.


The intrusion disrupted the transportation agency’s operations, delaying customer refunds and forcing 28,000 employees to reset their corporate passwords in person. Investigators secured convictions after recovering incriminating evidence, including network access screenshots and Telegram chat logs from seized devices. A judge will officially sentence the duo this July.


A cloud computing account operated by subsidiaries of HuiOne Group has been officially taken down by the DoJ, with the U.S. Treasury unveiling new sanctions against nine associated individuals and 26 entities


. Authorities allege the infrastructure hosted HuiOne Guarantee, an illicit online[marketplace](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-are-darknets-dark-markets/) that processed billions of dollars to facilitate widespread cyber[scams](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/how-to-prevent-social-engineering-attacks/) and money laundering operations.


The platform provided essential services for Southeast Asian scam centers, selling stolen data,[deepfake](https://www.sentinelone.com/cybersecurity-101/cybersecurity/deepfakes/) software, and malicious web development tools. Seizing the account effectively froze the technological backbone supporting these transnational criminal organizations.


## The Bad | DPRK-linked macOS.Gaslight Makes AI Triage Agents Doubt Their Own Analysis


This week,[SentinelLABS](https://www.sentinelone.com/labs) detailed[macOS.Gaslight](https://www.sentinelone.com/labs/macos-gaslight-rust-backdoor-turns-prompt-injection-on-the-analyst-not-the-sandbox/) , a previously undocumented Rust-based macOS implant and infostealer linked with high confidence to North Korean threat actors. The implant carries the usual credential-theft machinery, but its standout feature is a 3.5 KB prompt-injection payload built to derail the LLM-assisted triage tools that increasingly sit in the reverse-engineering loop


.


That payload is a Markdown-fenced block of 38 fabricated “system” messages delimited with the same` {{DATA}}` tokens that mimic an LLM triage harness’s own prompt scaffold, blurring the line between sample data and trusted instructions.


The messages feed the agent with fake token-expiry notices, out-of-memory kills, disk-exhaustion warnings, and bogus static-analysis flags. The aim is to make the AI reviewer doubt its own session and abort, truncate, or refuse its work.


Some of the many fake LLM data messages embedded in the binary


Underneath the injection, macOS.Gaslight relies on established macOS tradecraft. It maintains[command and control](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-are-command-control-c2-servers/) through a Telegram Bot API polling loop, encrypts payloads with AES-GCM over certificate-pinned TLS to frustrate network inspection, and gives operators an interactive shell that can run commands, kill processes, and[exfiltrate](https://www.sentinelone.com/cybersecurity-101/cybersecurity/data-exfiltration/) files.


The implant also self-redacts its[Telegram bot token](https://www.sentinelone.com/blog/sep-2023-cybercrime-update-new-ransomware-threats-and-the-rising-menace-of-telegram/) at runtime, so the credential never surfaces in logs or crash artifacts and defenders lose an easy detection lead.


SentinelLABS researchers say earlier analyst-targeting LLM injections relied on a single injected block or header. Gaslight appears to be the first to use a cascade of fabricated failure messages to derail the analysis itself.


As AI-assisted analysis becomes routine, the researchers warn that defenders should treat everything inside a sample as hostile input, never as instructions, and keep it out of the model entirely.


## The Ugly | Threat Actors Exploit Two Cisco Vulnerabilities to Achieve Root Access


Threat actors are actively exploiting a critical server-side request forgery vulnerability to compromise Cisco Unified Communications Manager systems


. Tracked as[CVE-2026-20230](https://nvd.nist.gov/vuln/detail/CVE-2026-20230) , the flaw stems from improper input validation within the WebDialer service.


During intrusions, attackers dispatch crafted HTTP requests containing genuine-formatted file-write payloads to target devices. If the disabled-by-default WebDialer service remains active, unauthenticated adversaries can arbitrarily write files directly to the operating system and ultimately elevate privileges to root level.


Cisco says it[patched](https://www.cisco.com/c/en/us/support/docs/csa/cisco-sa-cucm-ssrf-cXPnHcW.html) the flaw in recent updates and advises administrators to disable the feature entirely if immediate patching remains impossible.


> 🚨 Over the weekend we observed exploitation of CVE-2026-20230 – Cisco Unified CM (CUCM) WebDialer SSRF → root file-write (CVSS 8.6)
>
>
> No previously recorded exploitation, and not yet listed in CISA KEV.
>
>
> This is currently being exploited from a single source using an unvetted…[pic.twitter.com/VBgJF8zJfj](https://t.co/VBgJF8zJfj)
>
>
> — Defused (@DefusedCyber)[June 22, 2026](https://x.com/DefusedCyber/status/2069074520057557244?ref_src=twsrc%5Etfw)


It was also reported this week that an unknown threat actor exploited another Cisco flaw, this time impacting Cisco Catalyst SD-WAN controllers at least two months before public disclosure


.


Researchers[report](https://cloud.google.com/blog/topics/threat-intelligence/zero-day-exploitation-cisco-catalyst-sd-wan-manager) how the attackers targeted a communications service provider to escalate a compromised administrative account to full root-level access. Following initial intrusions that likely leveraged earlier authentication bypass zero-days and stolen certificates, operators exploited the high-severity flaw, now tracked as[CVE-2026-20245](https://nvd.nist.gov/vuln/detail/CVE-2026-20245) . By uploading a maliciously crafted CSV file, attackers bypassed input validation to create a hidden, rogue user account named “troot” equipped with a root shell.


Throughout the intrusion, the adversary employed sophisticated anti-forensic techniques to mask their operational footprint. After exfiltrating the SD-WAN fabric configuration, operators reverted administrative passwords to their original values to prevent discovery. Subsequently, the attackers deleted modified files, reversed configuration changes, and executed validation scripts to confirm all malicious indicators were entirely erased.


Analysts highlight that advanced adversaries increasingly target edge network devices because these critical systems frequently lack deep forensic visibility, allowing malicious operators to maintain persistent access undetected.
