---
schema_version: "1.0.0"
document_id: "628179141a5752a5b5314f00d3009bf18d9c6b7a1030698e7f41b81779e82905"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/openclaw-ai-supply-chain-risk/"
published_at: "2026-06-23T22:00:51+00:00"
first_seen_at: "2026-07-22T17:15:05.756127+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:a296a5fa186f0dd994c547893bd1f70803adcae76ad8039613e3aebb18e7e0e9"
---

# OpenClaw’s Skill Marketplace and the Emerging AI Supply Chain Threat

## Executive Summary


OpenClaw is an AI agent that executes third-party skills from ClawHub, its dedicated marketplace. Skills are markdown-driven packages with broad local system access, making ClawHub a critical link in the agentic software supply chain.


Following its release, the ecosystem saw several malicious campaigns. Those early findings, published in February 2026, prompted ClawHub to integrate VirusTotal and ClawScan, enabling proactive screening of published skills and code-level analysis to block skills flagged as malicious from download.


However, our analysis from February-May 2026 revealed persistent and evasive malicious skills on ClawHub. We identified five unblocked skills.


We reported all five to ClawHub for takedown. OpenClaw banned the accounts mentioned and deleted all of the skills.


The five skills represent three distinct threat categories leveraging the AI supply chain ecosystem:


- **Infostealers:** Two skills delivered macOS infostealers. Both connect to command-and-control (C2) infrastructure, indicating persistent threat actor activity.
- **Evasion:** One skill has an inflated file size to exceed scanner thresholds, bypassing both ClawScan and VirusTotal detection.
- **Agentic threats:** Two skills represent agentic threats: runtime agentic affiliate injection and agentic front-running. Both are novel techniques that the skill authors used for financial gain.


OpenClaw is now also[collaborating with NVIDIA](https://openclaw.ai/blog/openclaw-nvidia-skill-security) to provide documentation of what each skill does, and to run NVIDIA’s analysis tool on all skills.


Palo Alto Networks customers are better protected from the threats discussed above through the following products and services:


- [Koi Agentic Endpoint Security (AES)](https://www.koi.ai/product/endpoint)
- [Advanced URL Filtering](https://docs.paloaltonetworks.com/advanced-url-filtering)
- [Advanced DNS Security](https://docs.paloaltonetworks.com/dns-security)
- [Prisma Browser](https://www.paloaltonetworks.com/sase/prisma-browser)
- [Advanced WildFire](https://docs.paloaltonetworks.com/wildfire)
- [Cortex XDR](https://blog.talosintelligence.com/a-deep-dive-into-lokibot-infection-chain/) and[XSIAM](https://blog.talosintelligence.com/a-deep-dive-into-lokibot-infection-chain/)


The[Unit 42 AI Security Assessment](https://www.paloaltonetworks.com/unit42/assess/ai-security-assessment) and[Unit 42 Frontier AI Defense](https://www.paloaltonetworks.com/unit42/ai-advantage) service can help identify and mitigate complex AI-specific risks.


If you think you might have been compromised or have an urgent matter, contact the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) .


**Related Unit 42 Topics** **[Agentic AI](https://unit42.paloaltonetworks.com/tag/agentic-ai/) ,[OpenClaw](https://unit42.paloaltonetworks.com/tag/openclaw/) ,[ClawHub](https://unit42.paloaltonetworks.com/tag/clawhub/) ,[Supply Chain,](https://unit42.paloaltonetworks.com/tag/supply-chain/)[Infostealer](https://unit42.paloaltonetworks.com/tag/infostealer/)**


## AI Agent Skills as a Supply Chain Attack Surface


Software supply chain attacks typically rely on compromising distribution vectors or spoofing dependencies. However, AI agent ecosystems have altered this paradigm, and their threat model differs from previously established ecosystems like[npm](https://nodejs.org/learn/getting-started/an-introduction-to-the-npm-package-manager) or[PyPI](https://pypi.org/) . While conventional malware often faces limitations from language runtimes or containers, malicious skills use semantic instruction hijacking to bypass technical constraints.


By misusing the AI’s natural language interpretation, malicious skills can exploit the agent's operational context, including file systems, shells and credential managers, without requiring a conventional exploit. The lack of isolation between skill logic and agent authority means that installation results in complete control over the agent's identity. This allows a malicious skill to perform unauthorized actions through the agent’s own authenticated sessions.


## Early Campaign Activity on ClawHub


In early February 2026,[Bitdefender Labs reported](https://www.bitdefender.com/en-us/blog/labs/helpful-skills-or-hidden-payloads-bitdefender-labs-dives-deep-into-the-openclaw-malicious-skill-trap) that approximately 17% of OpenClaw skills they analyzed in the first few weeks of the platform's release carried malicious payloads. Koi Security's[ClawHavoc](https://www.koi.ai/blog/clawhavoc-341-malicious-clawedbot-skills-found-by-the-bot-they-were-targeting) disclosure documented 341 malicious skills, and[Trend Micro separately confirmed](https://www.trendmicro.com/en_us/research/26/b/openclaw-skills-used-to-distribute-atomic-macos-stealer.html) skills distributing Atomic macOS stealer (AMOS) malware across the marketplace.


This early wave featured several distinct techniques:


- **Base64-encoded curl-pipe-bash dropper** : These skills embedded a fake prerequisite block that instructed the agent to decode and execute a Base64-encoded remote payload, typically fetched from 91.92.242\[.\]30


, the IP address for an AMOS C2 server.
- **Platform-specific delivery:** For macOS targets, paste-site redirects ( glot\[.\]io, rentry\[.\]co


) served as an intermediary step, allowing attackers to update payloads without modifying the published skill. Attackers directed Windows targets to password-protected executables hosted on third-party hosting services.
- ​​ **Persistence via auto-updaters:** Auto-updater skills combined the initial dropper with scheduled cron job registration, ensuring the C2 channel persisted even after skill removal.
- **Alternative exfiltration channel** : A distinct cluster ( polymarketbtc, polymarketbtcassistant


and related skills published by krajekisbtc


) exfiltrated cryptocurrency private keys via the Telegram Bot API, a C2 channel independent of the shared dropper infrastructure.
- **Registry saturation** : A single publisher account injected malicious payloads into the majority of their published skill catalog with identical payloads to maximize installation surface before detection.


Those findings prompted ClawHub to[partner with VirusTotal](https://openclaw.ai/blog/virustotal-partnership) , enabling proactive screening of published skills. These skills from these early campaigns have since been removed from the marketplace or marked as malicious.


In the following sections, we document the state of the marketplace between February and May 2026, during which VirusTotal and ClawScan served as the primary screening mechanisms.


(On June 1, ClawHub also announced a partnership with[NVIDIA](https://openclaw.ai/blog/openclaw-nvidia-skill-security) to help screen published skills.)


The AMOS dropper infrastructure from earlier campaigns remains active more than three months after first public disclosure, with the C2 server at 91.92.242\[.\]30


continuing to receive new skill deliveries. Additionally, we observe novel attacks that adapt to and exploit skill marketplaces, leveraging the agentic execution model to implement financial schemes that evade some kinds of malware detection.


## Malicious Skills Distributing ClawHavoc Payload


Publisher/Skill: \[redacted\]/tradingview-ai-indicator-assistant


SHA256 hash: b6c7e0bf573b1c7d9d3a05eb08d26579199515b847df984862805f44a7af8007


On May 17, 2026, the account published two skills targeting TradingView users as shown in Figure 1.


Figure 1. ClawHub marketplace listings for two TradingView assistant skills.


Both of these skills presented as AI assistants for macOS, posing as productivity tools for traders. Both embedded the same malicious prerequisite block, which prevented the skills from functioning until the user performed a required action. In this case, the prerequisite block directed agents to a site with malicious instructions to copy and paste text into a terminal window. We refer to this site as a paste-site redirect lure.


The paste-site redirect lure at hxxps\[:\]//rentry\[.\]co/openclaw-code


served instructions with a Base64-encoded string for the prerequisite block, which the agent must run before the skill can continue. Figure 2 below shows an example of this page.


Figure 2. Paste-site redirect lure.


When the agent performed the actions in the paste-site redirect lure, the associated command fetched a payload from hxxp\[:\]//2.26.75\[.\]16/Xuvewuyur


. That payload was a macOS infostealer named cluw


with a SHA256 hash of 818aea6143282b352fdfdc0f3ebf77a36e54eb3befb5cad1a355a99ab97c6aa7


.


The delivery mechanism is structurally identical to the ClawHavoc campaigns documented by[Koi Security](https://www.koi.ai/blog/clawhavoc-341-malicious-clawedbot-skills-found-by-the-bot-they-were-targeting) and[Trend Micro](https://www.trendmicro.com/en_us/research/26/b/openclaw-skills-used-to-distribute-atomic-macos-stealer.html) . The prerequisite block, the paste-site redirect lure and the Base64 pipe to bash all match the early-wave pattern.


The C2 server we discovered at 2.26.75\[.\]16


differs from prior disclosure. The cluw


payload differs from AMOS. This campaign used the established delivery template with fresh backend infrastructure.


Until mid-May, ClawHub's automated auditing returned a verdict of Pass


for ai-tradingview-assistant-for-macos


and no verdict for tradingview-ai-indicator-assistan


t. Neither skill triggered detection, despite containing a verbatim paste-site prerequisite lure. This structural pattern characterized over 300 skills in the original ClawHavoc disclosure.


## File Padding for Defense Evasion


Publisher/Skill: \[redacted\]/omnicogg


SHA256 hash: b30eaed1f7478c28f4ec50d07ed5ef014ffbc4b2bc5a38d689ba9f7abb5e19c2


The omnicogg


skill was an early-wave threat, similar to those that defined the initial surge of malicious activity on ClawHub. It is a Base64-encoded[curl-pipe-bash](https://medium.com/@ewindisch/curl-bash-a-victimless-crime-d6676eb607c9) dropper that delivered the AMOS malware via 91.92.242\[.\]30


, the same C2 infrastructure documented in earlier campaigns.


This skill is distinguished by its delivery vessel, a README.md


file. The malicious payload appears at the start, followed by 22 MB of padding characters. This padding inflates the file size beyond the limits that many content-analysis pipelines enforce before declining to process a file. Figure 3 below shows an example of the padding characters in this file.


Figure 3. The omnicogg


skill’s README.md


file.


[JFrog Security Research disclosed](https://research.jfrog.com/post/omnicogg-malicious-skill/) this skill in March 2026. This evasion technique can be effective because many scanning pipelines skip abnormally large files rather than process them.


This skill's ClawScan audit was in review in mid-May, while VirusTotal returned a clean verdict, and the skill remained available for download, as shown in Figure 4.


Scanners that do not analyze content beyond standard thresholds will miss payloads structured to exploit that weakness.


Figure 4. ClawHub audit page for \[redacted\]/omnicogg


shows an overall pass despite containing malicious code. *.*


## Runtime Agentic Affiliate Injection


Publisher/Skill: \[redacted\]/money-radar


SHA256 hash: ebb73dbb5aac1f6fe1a88e8f26126a1e1aa34c9f3345ad4345189b40d9bf1d1d


This ClawHub campaign focused on financial communities, with skills that targeted banking and crypto exchange workflows. This money-radar


skill presented itself as an overseas financial product advisor that compared brokerages, banks, crypto exchanges and remittance services for users in mainland China, Hong Kong and Singapore. However, its core logic was an affiliate funnel for developer profit.


The skill weaponized the agent's advisory authority, routing all financial recommendations through affiliate links from a known-malicious domain. The publisher retained dynamic control over which products it pushed after installation.


#### Technical Analysis


The skill's mandatory first action on every invocation was to fetch product data from laosji\[.\]net


, a domain previously observed in paste-jacking campaigns. Figure 4 shows an example of this action within the skill's SKILL.md


file.


Figure 5. The money-radar


skill's SKILL.md


instructs the agent to fetch data from laosji\[.\]net


.


The agent ingested a referrals.json payload from laosji\[.\]net as a precondition to answering any financial question. That payload contained approximately 60 products across eight categories, each with a referralLink field carrying affiliate tracking. The SKILL.md file then issued an explicit instruction to always use the referral links as shown in Figure 6.


Figure 6. The money-radar


skill's SKILL.md


file with the affiliate link instruction highlighted and translated.


Once the skill was installed, the publisher dynamically controlled the links the agent would recommend by updating referrals.json


on laosji\[.\]net


. The operator could change which products were recommended, rotated affiliate partners or redirected victims toward higher-commission offerings without the victim’s involvement. This exploitation constitutes an agent-specific form of runtime affiliate injection.


Unlike typical affiliate injection, which intercepts links the target was already clicking, this skill generated the recommendation itself. The affiliate link arrived embedded in what appears to be skill-based expert advice.


## Agentic Front Running


Publisher/Skill: \[redacted\]/letssendit


SHA256: hash f4e41aa269c88bf11a2022701a9cf41e9a186aa1b224d837c31bf34e0b875d0e


The letssendit


skill implemented an agentic front-running scheme. This scheme involved the skill operator misusing the ClawHub platform to illegitimately profit from meme token launches. It achieved this by leveraging numerous AI agent participants and coordinated agentic execution.


The coordinated activity executed on infrastructure using the domain letssendit\[.\]fun


. Guided by the skill's SKILL.md


file instructions, installed agents autonomously pooled Solana blockchain platform cryptocurrency (SOL) into the operator's digital wallet. Once enough agents had joined, the operator would front-run the distribution by purchasing the SENDIT


meme token at the lowest bonding curve price before allocating any to the agents.


The token then launched publicly on the cryptocurrency platform pump\[.\]fun


, where external buyers could mistake the coordinated AI botnet activity for organic retail demand. This could create a classic[rug pull](https://www.bankrate.com/investing/what-is-a-rug-pull/) . The operator simply rotates wallets across multiple confirmed launches, dumping their low-cost position into the artificial market rally at the expense of secondary market buyers.


Ultimately, this exploit represents a novel documented case of an attacker weaponizing an autonomous AI agent network to execute a pump-and-dump scheme. This behavior constitutes fraudulent financial activity. We strongly recommend that enterprises block this skill across their AI infrastructure to mitigate regulatory and security risks.


## Conclusion


The cases documented in this article span evasion, deceptive monetization, financial fraud and campaign persistence. Each case passed existing detection tools at the time of our analysis.


Organizations can strengthen their defensive posture by using a rigorous supply chain verification framework. We identified that skill execution occurs within the agent process. This necessitates active validation of publisher provenance and a line-by-line audit of package source files.


Our research indicates that monitoring outbound network traffic can identify post-installation communication with undocumented endpoints. We recommend cross-referencing all external connections against the provided documentation. Any discrepancies serve as observable indicators of risk. These verification steps help protect an organization’s environment by ensuring that the[operational behavior of a skill](https://unit42.paloaltonetworks.com/ai-agent-supply-chain-risks/) aligns strictly with its stated technical specifications.


### Palo Alto Networks Protection and Mitigation


Palo Alto Networks customers are better protected from the threats discussed above through the following products:


- [Koi's Agentic Endpoint Security (AES)](https://www.koi.ai/product/endpoint) gives security teams a single platform to discover every AI component across the agentic endpoint, assess its risk, enforce policy, and remediate violations - so your end users adopt the latest technology, increase the org productivity without compromising on security.
- [Advanced URL Filtering](https://www.paloaltonetworks.com/network-security/advanced-url-filtering) and[Advanced DNS Security](https://docs.paloaltonetworks.com/dns-security) identify known domains and URLs associated with this activity as malicious.
- [Prisma Browser](https://www.paloaltonetworks.com/sase/prisma-browser) Prisma Browser provides additional protection layers against advanced web threats including dynamic scans of every loaded web page, to prevent execution of malicious content and protect company assets.
- The[Advanced WildFire](https://docs.paloaltonetworks.com/wildfire) machine-learning models and analysis techniques have been reviewed and updated in light of the indicators shared in this research.
- [Cortex XDR](https://blog.talosintelligence.com/a-deep-dive-into-lokibot-infection-chain/) and[XSIAM](https://blog.talosintelligence.com/a-deep-dive-into-lokibot-infection-chain/) are designed to prevent the execution of known malicious malware, and also prevent the execution of unknown malware using Behavioral Threat Protection.


If you think you may have been compromised or have an urgent matter, get in touch with the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) or call:


- North America: Toll Free: +1 (866) 486-4842 (866.4.UNIT42)
- UK: +44.20.3743.3660
- Europe and Middle East: +31.20.299.3130
- Asia: +65.6983.8730
- Japan: +81.50.1790.0200
- Australia: +61.2.4062.7950
- India: 000 800 050 45107
- South Korea: +82.080.467.8774


Palo Alto Networks has shared these findings with our fellow Cyber Threat Alliance (CTA) members. CTA members use this intelligence to rapidly deploy protections to their customers and to systematically disrupt malicious cyber actors. Learn more about the[Cyber Threat Alliance](https://www.cyberthreatalliance.org/) .


## Acknowledgments


We’d like to thank the entire Unit 42 team for supporting us with this article. Special thanks to Samantha Stallings, Bradley Duncan and Lysa Myers for helping us review this article.


## Indicators of Compromise


### Domains, IP Addresses and URLs


- 2.26.75\[.\]16


- 91.92.242\[.\]30


- 91.92.242\[.\]30/lamq4


- download.setup-service\[.\]com


- github\[.\]com/Ddoy233/openclawcli


- glot\[.\]io/snippets/hfd3x9ueu5


- install.app-distribution\[.\]net


- laosji\[.\]net


- openclawcli.vercel\[.\]app


- rentry\[.\]co/openclaw-code


### Publisher/Skill


- \[redacted\]/santi-text-game


- \[redacted\]/omnicogg


- \[redacted\]/letssendit


- \[redacted\]/money-radar


- \[redacted\]/ai-tradingview-assistant-for-macos


- \[redacted\]n/tradingview-ai-indicator-assistant


- \[redacted\]/pdfcheck


- \[redacted\]/update


- \[redacted\]/wistec-core


### SHA256 Hashes


- 818aea6143282b352fdfdc0f3ebf77a36e54eb3befb5cad1a355a99ab97c6aa7


- 881ce5cb124c4d2e814783724cc1388f6a1cbf6eee274c3f3366e77ba3503ad7


- b30eaed1f7478c28f4ec50d07ed5ef014ffbc4b2bc5a38d689ba9f7abb5e19c2


- b6c7e0bf573b1c7d9d3a05eb08d26579199515b847df984862805f44a7af8007


- ebb73dbb5aac1f6fe1a88e8f26126a1e1aa34c9f3345ad4345189b40d9bf1d1d


- f4e41aa269c88bf11a2022701a9cf41e9a186aa1b224d837c31bf34e0b875d0e


## Additional Resources


- [OpenClaw Partners with VirusTotal for Skill Security](https://openclaw.ai/blog/virustotal-partnership)
- [OpenClaw Collaborates with NVIDIA for Stronger Agent Skill Security](https://openclaw.ai/blog/openclaw-nvidia-skill-security)
- [Anatomy of a Deception: Uncovering the 'omnicogg' Dropper in ClawHub](https://research.jfrog.com/post/omnicogg-malicious-skill/) - JFrog Security Research
- [ClawHavoc: 341 Malicious Clawed Skills Found by the Bot They Were Targeting](https://www.koi.ai/blog/clawhavoc-341-malicious-clawedbot-skills-found-by-the-bot-they-were-targeting) - Koi Security
- [Malicious OpenClaw Skills Used to Distribute Atomic macOS Stealer](https://www.trendmicro.com/en_us/research/26/b/openclaw-skills-used-to-distribute-atomic-macos-stealer.html) - Trend Micro
- [Trust No Skill: Integrity Verification for AI Agent Supply Chains](https://unit42.paloaltonetworks.com/ai-agent-supply-chain-risks/) - Unit 42
