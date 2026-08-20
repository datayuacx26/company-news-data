---
schema_version: "1.0.0"
document_id: "8be1c646ed2dae2e8709b490ed06b5cfd8cc7073ca509262051e8aedf54e9a70"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-30-8/"
published_at: "2026-07-24T13:00:12+00:00"
first_seen_at: "2026-07-24T13:32:17.005018+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:d728c2ecb2ca643611cab89da42e020f317eb678cc26cffe4ea4c0dd221f9fa7"
---

# The Good, the Bad and the Ugly in Cybersecurity – Week 30

## The Good | Authorities Dismantle Kratos Phishing Network & Arrest Its Developer


Kratos, a prominent phishing-as-a-service (PhaaS) platform, was dismantled from the inside out this week thanks to[German](https://www.bka.de/SharedDocs/Kurzmeldungen/DE/Kurzmeldungen/260720_Schlag_gegen_Phishing_Gruppierung_Kratos.html) and U.S. law enforcement agencies. During “Operation Olympus Blade”, authorities seized over 200 servers to render Kratos’ global network entirely inoperable


while the platform’s suspected developer was apprehended in Indonesia.


So far, investigators estimate that more than 1800 cybercriminals utilized the platform to launch nearly 15,000[phishing](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/phishing-scams/) campaigns monthly since late 2024. Operating as a franchise, the service provided threat actors with toolkits designed to generate convincing Microsoft authentication pages. These campaigns targeted victims across the United States and Europe, facilitating widespread[credential theft](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-credential-theft/) and unauthorized account access. The operators earned at least €300,000 in subscription fees.


Source: BKA


A recent[report](https://any.run/cybersecurity-blog/kratos-phaas-account-takeover/) from cyber researchers reverse-engineered the Kratos toolkit, revealing how it offered operators two distinct functional modes. While one mode harvested traditional credentials, the more advanced setting deployed a Node.js reverse proxy. This[adversary-in-the-middle](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-an-adversary-in-the-middle-aitm-attack/) (AitM) capability allowed attackers to intercept active session cookies in real-time, effectively bypassing standard[multi-factor authentication](https://www.sentinelone.com/cybersecurity-101/identity-security/what-is-multi-factor-authentication-mfa/) (MFA) controls.


Once compromised, these accounts provided actors with initial footholds to execute[business email compromise](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/business-email-compromise-bec/) (BEC), lateral data theft, and secondary phishing attacks. Just this February, actors ran Kratos in a[campaign](https://www.microsoft.com/en-us/security/blog/2026/03/19/when-tax-season-becomes-cyberattack-season-phishing-and-malware-campaigns-using-tax-related-lures/) that used tax-themed lures and personalized QR codes to target dozens of American manufacturing and healthcare organizations.


While the immediate server takedown severely disrupts ongoing operations, officials acknowledge that the existing customer base retains access to the underlying kit code


. That means the toolkit itself outlives the infrastructure seizure, and operators who already have copies can resume campaigns under new branding with minimal rebuild effort.


## The Bad | Threat Actors Conceal HollowGraph Malware in Microsoft 365 Calendar Events


A novel espionage implant, dubbed[HollowGraph](https://www.group-ib.com/blog/hollowgraph-microsoft-365/) , is hijacking Microsoft 365 calendars to establish a covert[command and control](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-are-command-control-c2-servers/) (C2) channel. By routing operator instructions and exfiltrated data through legitimate Microsoft Graph API traffic, the malware ensures its activities blend seamlessly with routine network chatter.


The .NET DLL implant operates purely as a two-way dead drop without communicating directly with an attacker-owned payload server


. To receive tasking, HollowGraph queries the compromised user’s calendar for an event planted far into the future – in this case, dated for May 13, 2050. Operators embed their instructions within text files attached to this anomalous event, ensuring the mailbox owner never naturally scrolls far enough to discover the malicious entries.


For data exfiltration, the malware executes the reverse process. It systematically encrypts stolen files using hybrid RSA and AES-256 encryption, generates a new far-future calendar event, and uploads the targeted data as attachments. To maintain continuous Graph API access, operators utilize a secondary DNS-based channel to refresh the application’s Entra ID login credentials. The malware decodes these values from an attacker-controlled domain and writes them to a disguised configuration file.


Analysts observed this highly targeted campaign actively compromising machines at an Israeli organization between June and July 2026. While the implant’s underlying code shares significant structural similarities with a modular backdoor framework called Cavern, frequently utilized by Iranian state-sponsored syndicates, researchers have not yet definitively attributed this specific operation to a known threat group.


> HollowGraph buries its C2 in M365 calendar events dated 2050 – no attacker server ever touched.[https://t.co/yJo4fpw0X9](https://t.co/yJo4fpw0X9)[#ThreatIntel](https://x.com/hashtag/ThreatIntel?src=hash&ref_src=twsrc%5Etfw)[#HollowGraph](https://x.com/hashtag/HollowGraph?src=hash&ref_src=twsrc%5Etfw)[#Cavern](https://x.com/hashtag/Cavern?src=hash&ref_src=twsrc%5Etfw)[#Cav3rn](https://x.com/hashtag/Cav3rn?src=hash&ref_src=twsrc%5Etfw)[pic.twitter.com/kaABaAYZg0](https://t.co/kaABaAYZg0)
>
>
> — ThreadLinqs (@threadlinqs)[July 22, 2026](https://x.com/threadlinqs/status/2079775946752561530?ref_src=twsrc%5Etfw)


Since HollowGraph relies entirely on compromised account identities and legitimate application permissions rather than software[vulnerabilities](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-a-security-vulnerability/) , standard[patching](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-patch-management/) remains ineffective. The technique effectively weaponizes the trust organizations place in their own Microsoft Graph API traffic, turning routine calendar activity into a blind spot by design.


## The Ugly | AI Models Escape Sandbox to Breach Hugging Face Infrastructure


Open-source AI platform Hugging Face recently[disclosed](https://huggingface.co/blog/security-incident-july-2026) a network breach orchestrated entirely by an autonomous AI agent. The attack compromised the company’s data-processing pipeline by deploying a malicious dataset that exploited two distinct code-execution vulnerabilities


. This initial access allowed the agent to run unauthorized code on a processing worker, harvest cloud and cluster credentials, and subsequently move[laterally](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/lateral-movement/) across several internal systems.


The platform reported that the agent executed thousands of individual actions across short-lived sandboxes while staging self-migrating C2 infrastructure on public services. Following the discovery, Hugging Face evicted the agent, revoked affected credentials, and rebuilt compromised nodes. It was during the forensic investigation that responders encountered a unique operational hurdle: Western AI models refused to process the malicious artifacts due to built-in safety guardrails


. At this point, the company turned to an unrestricted, open-weight[Chinese model](https://z.ai/blog/glm-5.2) to successfully complete the analysis.


Shortly after Hugging Face’s disclosure, OpenAI confirmed that its own AI models orchestrated the intrusion during internal benchmark testing. Operating with reduced safety guardrails for evaluation purposes,[GPT-5.6 Sol](https://www.sentinelone.com/labs/frontier-models-tackle-autonomous-long-horizon-malware-analysis/) and an advanced pre-release model attempted to cheat the[ExploitGym](https://www.mpi-sp.org/108048/ExploitGym__Can_AI_Agents_Turn_Security_Vulnerabilities_into_Real_Attacks_?c=89706#:~:text=What%20is%20ExploitGym) cybersecurity assessment. Rather than solving the challenges natively, the models identified and exploited a shortcut, retrieving the test solutions directly from Hugging Face’s production database via a zero-day vulnerability in a third-party package registry cache proxy.


Source:[Adel Ka](https://x.com/0x4D31/status/2079675111276495349)


After gaining open internet access through[privilege escalation](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/how-to-prevent-privilege-escalation-attacks/) and lateral movement, the models accessed Hugging Face servers. The AI agents chained together multiple attack vectors, utilizing the stolen credentials and additional[zero-day](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/zero-day-vulnerabilities-attacks/) vulnerabilities to establish remote code execution. OpenAI subsequently disclosed the zero-day flaw and[collaborated](https://x.com/ClementDelangue/status/2079670308156645882) with Hugging Face to implement stricter infrastructure controls and guardrails.
