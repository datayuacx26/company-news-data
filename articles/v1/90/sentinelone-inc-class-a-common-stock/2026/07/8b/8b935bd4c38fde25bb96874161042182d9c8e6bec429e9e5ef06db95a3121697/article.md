---
schema_version: "1.0.0"
document_id: "8b935bd4c38fde25bb96874161042182d9c8e6bec429e9e5ef06db95a3121697"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-29-8/"
published_at: "2026-07-17T16:40:44+00:00"
first_seen_at: "2026-07-20T23:17:13.754958+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:d75fabd24ce9a21f85da2800e277a02e296efaa41a19d47a1e6efba91cb5351a"
---

# The Good, the Bad and the Ugly in Cybersecurity – Week 29

## The Good | Authorities Sanction Cybercriminals & Dismantle Russian Bulletproof Hosting Infrastructure


The[EU](https://www.consilium.europa.eu/en/press/press-releases/2026/07/13/cyber-russia-statement-by-the-high-representative-on-behalf-of-the-european-union-denouncing-russia-s-malicious-cyber-ecosystem-targeting-the-eu-its-member-states-and-international-partners/) and the[United Kingdom](https://www.gov.uk/government/news/uk-and-eu-strike-russian-cyber-networks-with-new-sanctions) have jointly sanctioned multiple Russian individuals and entities for targeting government networks and critical infrastructure across Europe. The sanctions specifically target senior Russia military intelligence (GRU) officers and operators, as well as four entities linked to the Federal Security Service (FSB)


.


Officials say that the Russian government actively utilizes these state-sponsored units alongside recruited cybercriminals and private companies to systematically destabilize international partners and compromise key infrastructure across the continent.


From the U.S. Treasury Department, two individuals and a virtual private network (VPN) provider face sanctions for actively enabling ransomware attacks against American organizations


.


OFAC designated[First VPN Service](https://home.treasury.gov/news/press-releases/sb0559) (1VPNS) and its administrator, Dmytro Rashevskyi, for supplying infrastructure that helped cybercriminals obscure their identities and manage stolen data. The service, which law enforcement[dismantled](https://www.europol.europa.eu/media-press/newsroom/news/cybercriminal-vpn-used-ransomware-actors-dismantled-in-global-crackdown) last May, notoriously ignored abuse complaints and maintained zero user logs.


Yegeniy Silayev was also[sanctioned](https://www.state.gov/releases/office-of-the-spokesperson/2026/07/sanctioning-ransomware-enablers-in-coordinated-international-action/) for developing cryptors designed to conceal malware. Investigators estimate these specific tools and services directly facilitated billions of dollars in financial losses across critical sectors.


U.S. Federal prosecutors also unsealed indictments this week against three Russian nationals for operating


[bulletproof hosting](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/bulletproof-hosting/) services that facilitated over $62 million in global ransomware damages


.


Defendants Aleksandr Volosovik, Yulia Pankova, and Kirill Zatolokin allegedly managed “Media Land” and “ML Cloud”, providing essential infrastructure to syndicates like[Lockbit](https://www.sentinelone.com/anthology/lockbit-3-0-lockbit-black/) ,[Play](https://www.sentinelone.com/anthology/play/) , and[Blacksuit](https://www.sentinelone.com/anthology/blacksuit/) . These hosting platforms actively shielded cybercriminals by disregarding victim complaints and ignoring law enforcement takedown requests.


To disrupt this supply chain, the State Department is offering a $10 million[reward](https://rewardsforjustice.net/rewards/media-land/) for actionable information regarding foreign government links to these hosting providers.


## The Bad | Attackers Trojanize Popular Remote User Platforms to Deploy Starland Malware


Cybersecurity researchers[identified](https://blog.talosintelligence.com/uat-11795-deploys-novel-starland-rat-and-bespoke-wldr-c2-implant-in-financially-motivated-campaign/) a financially-motivated Russian threat actor tracked as UAT-11795. Active since June 2025, the actor has utilized trojanized applications to harvest user


[credentials](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-credential-theft/) and cryptocurrency while primarily targeting users across the United States, Germany, Romania, and Venezuela


.


To distribute their payloads, UAT-11795 operators disguise malicious installers as legitimate software, including WebEx, Zoom, MobaXterm, DBeaver, and FaceIT. Researchers suspect the attackers likely deploy these files via[ClickFix](https://www.sentinelone.com/blog/how-clickfix-is-weaponizing-verification-fatigue-to-deliver-rats-infostealers/) social engineering.


The infection chain typically starts when a victim executes a malicious HTA file. This file retrieves an altered NSIS installer harboring a hidden Python loader disguised as a standard text document. The loader then modifies the Windows Registry to ensure persistent access before decrypting and deploying the Starland remote access trojan (RAT).


Upon execution, Starland verifies whether it is operating within a sandbox before creating scheduled tasks and attempting to escalate its system privileges. The malware scans compromised systems for browser data, cryptocurrency wallet assets, detailed system configurations, any antivirus products, and Active Directory infrastructure such as domain structure and controllers


.


Beyond data theft, Starland possesses extensive capabilities to capture desktop screenshots, execute arbitrary shell commands, and fetch secondary payloads. Depending on system architecture, the malware can inject a 64-bit[shellcode chain](https://www.sentinelone.com/blog/malicious-input-how-hackers-use-shellcode/) to deliver the CastleStealer information stealer or a 32-bit chain to deploy the Remcos remote access trojan.


UAT-11795-controlled Telegram channels (Source: Cisco Talos)


To maintain resilient[command and control](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-are-command-control-c2-servers/) (C2) communications, the operators integrate a redundancy mechanism that queries a Polygon smart contract for a fallback domain, and control two Telegram bots to receive notification beacons, including messages with the victim’s machine fingerprints and cryptowallet inventories.


Users are reminded to avoid executing unidentified commands online and should only download confirmed software from official vendor sources.


## The Ugly | Nearly 300 Imposter GitHub Repositories Distribute Infostealing Malware to Collect Sensitive Data


Threat actors have published almost 300 fabricated GitHub repositories to distribute an[information stealer](https://www.sentinelone.com/cybersecurity-101/cybersecurity/infostealer/) from the BoryptGrab malware family. The actors systematically impersonated premium security products, cryptocurrency tools, and developer utilities to deceive victims searching for free software downloads


.


As part of the lure, the malicious landing pages employ highly sophisticated client-side scripts that parse referral URLs to render customized branding and spoofed trust badges, significantly increasing the likelihood of successful[social engineering](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-social-engineering/) .


Once a targeted victim clicks the download link, the infrastructure delivers a constantly rotating ZIP archive containing a legitimate, signed WinGUP updater paired with a trojanized dynamic link library file. When the user executes the updater, the program side-loads the malicious file, which then decodes and reflectively executes the BoryptGrab-variant payload directly into system memory.


Operating without establishing long-term persistence, the malware is designed to exfiltrate maximum data in a single execution cycle. The stealer targets passwords, payment details, and[session cookies](https://www.sentinelone.com/blog/session-cookies-keychains-ssh-keys-and-more-7-kinds-of-data-malware-steals-from-macos-users/) across 19 different web browsers and 32 cryptocurrency wallet brands, alongside messaging tokens from Discord, Steam, and Telegram.


The infostealer’s execution workflow (Source: Arctic Wolf)


To maximize collection, operators utilize direct code injection to bypass Chrome’s native App-Bound Encryption. All newly harvested data is compressed and routed to a Russian-based C2 server. Although the malware leaves behind forensic evidence by failing to wipe temporary staging directories, the scale of the impersonation campaign poses significant risks to unsuspecting developers.


GitHub has already removed a large portion of the false repositories, though several of the malicious redirector pages remain actively online


. Researchers advise users to independently verify software authenticity and exercise extreme caution when navigating unofficial portals, sharing this[YARA rule](https://github.com/rtkwlf/wolf-tools/tree/main/threat-intelligence/fake-github-repositories-deliver-boryptgrab-lineage-infostealer/) to help detect BoryptGrab activity and IoCs.
