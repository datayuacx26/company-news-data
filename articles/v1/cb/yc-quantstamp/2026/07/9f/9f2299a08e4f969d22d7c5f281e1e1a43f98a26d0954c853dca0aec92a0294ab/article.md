---
schema_version: "1.0.0"
document_id: "9f2299a08e4f969d22d7c5f281e1e1a43f98a26d0954c853dca0aec92a0294ab"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/june-security-beat"
published_at: "2026-08-12T16:23:39+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:c2b5230f8d425b08f6a0c37f4ac7d41c58fea9455f48aa83b29e90fcb5c163a2"
---

# June Security Beat: Keys Over Code

$75.32M was lost across 32 crypto incidents in June, up from May's $59.52M. No coordinated campaign carried the month, but one targeted operation did. A targeted social-engineering attack against Humanity Protocol reached the keys behind the $H token and drained $32M, roughly 42% of every dollar lost in June. Quantstamp led the independent investigation and traced the tooling to a phishing campaign previously seen targeting macOS users. Offchain, a fresh npm supply chain wave hit Red Hat's packages on the first day of the month, and a PeopleSoft zero-day was exploited for two weeks before Oracle said a word.


Here's the month in security 👇


### **Crypto: $75.32M lost across 32 incidents**


Category Loss Incidents


Key / Credential Compromise ~$34.40M 2


Bridge / Cross-Chain ~$16.97M 6


Other Protocol Logic ~$14.52M 9


Smart Contract ~$3.79M 6


Infrastructure / Custody ~$3.01M 2


Access Control ~$1.58M 2


Price / Oracle / Flashloan ~$1.03M 5


**Total** **$75.32M** **32**


Source |[DeFiLlama](https://defillama.com/hacks)


### **The incidents that did most of the damage**


**Humanity Protocol, June 8: ~$32M.** The month's largest loss started with social engineering. Attackers targeted the team with a malicious file disguised as a routine token-lockup schedule update, delivering remote-access malware that reached the private keys behind the $H token. On Ethereum, the attacker upgraded the $H token contract and moved roughly 141M $H out of protocol control. On BNB Smart Chain, they seized a ProxyAdmin contract and minted more.


Quantstamp led the independent investigation. The malware was signed with a South Korean Hancom digital certificate. The Mac variant retrieved from the phishing link matched the attack vector documented in a known macOS AppleScript phishing campaign: the same style of phishing lure, plus a .scpt file with a hidden extension structured the same way, a fake banner at the top, heavy padding spaces, and the script payload at the bottom, pointing to a shared threat actor. This was the same social-engineering playbook that drove April's largest losses.


Source |[Quantstamp](https://x.com/Quantstamp/status/2065484441984573849) ,[Humanity](https://www.humanity.org/hincidentupdate)


**Syscoin Bridge, June 7: ~$8M.** A parsing error in the bridge relay's proof validation code let an attacker submit a malformed proof that the parser read as valid. The relay treated a nonexistent NEVM burn as confirmed, authorized the mint, and roughly 5 billion unbacked SYS materialized on the UTXO side. The bridge was paused. After the team traced the funds and made onchain contact, the attacker returned all 5 billion SYS, which were then burned. A cross-chain message that was never checked hard enough, the same failure class that ran through May.


Source |[Halborn](https://www.halborn.com/blog/post/explained-the-syscoin-bridge-hack-june-2026) ,[Rekt](https://rekt.news/syscoin-rekt)


**Smaller losses through the month: ~$13M combined.** A reverse MEV honeypot took ~$7.5M off JaredFromSubway, one of Ethereum's best-known sandwich bots. Two protocol-logic bugs did the rest: Secret Network lost $4.67M to an unbacked mint via an ICS-20 cross-chain transfer, and Polymarket International lost $3M to a front-end vulnerability.


Source |[DeFiLlama](https://defillama.com/hacks)


### **Miasma: the month's npm supply chain story**


On June 1, Wiz Research identified a supply chain compromise across the @redhat-cloud-services npm namespace. At least 32 package releases carried unauthorized modifications, cumulatively averaging around 80,000 weekly downloads. A compromised Red Hat employee GitHub account pushed malicious orphan commits to two RedHatInsights repositories, bypassing code review across two waves.


The payload, dubbed Miasma, is derived from the Mini Shai-Hulud malware that TeamPCP open-sourced, with Dune references swapped for Greek themes and the tradecraft left intact. Two changes make it worse than its predecessor. It added cloud identity collectors for GCP and Azure, a shift from stealing secrets toward taking the cloud accounts themselves. And it generates a uniquely encrypted payload per infection, so hash-based indicators only catch one package version at a time. The malicious workflow requested a GitHub OIDC token and published packages with valid SLSA provenance attestations, so the releases looked legitimately signed.


Source |[Wiz](https://www.wiz.io/blog/miasma-supply-chain-attack-targeting-redhat-npm-packages)


### **PeopleSoft zero-day: exploited before the advisory**


Oracle PeopleSoft carried an actively exploited zero-day, CVE-2026-35273, with exploitation observed in the wild between May 27 and June 9, roughly two weeks before Oracle's advisory. CISA added it to the Known Exploited Vulnerabilities catalog on June 12.


It did not stand alone. CISA added nine more vulnerabilities to the KEV catalog across June, spanning Linux, Android, Arista, Chromium, Cisco, and Ubiquiti. The lesson isn't PeopleSoft specifically, but the patch gap. Attackers exploited a live zero-day for two weeks before there was an advisory to act on, and the enterprise infrastructure behind most protocols and exchanges is exactly where that gap gets used.


Source |[Rapid7](https://www.rapid7.com/blog/post/etr-active-exploitation-of-oracle-peoplesoft-zero-day-cve-2026-35273/) ,[CISA](https://www.cisa.gov/news-events/alerts/2026/06/23/cisa-adds-four-known-exploited-vulnerabilities-catalog)


### **The pattern across the month**


Three things ran through June:


1. **The biggest loss came through social engineering.** Humanity's $32M began with a targeted social engineering operation, and it accounted for 42% of the month's losses.
2. **Bridges are still the recurring failure.** Six cross-chain incidents totaled ~$17M, led by Syscoin's proof-validation parsing bug.
3. **The build pipeline and the agent are inside the perimeter.** Miasma reached cloud accounts through a signed npm release, and a malicious MCP server exfiltrated data through code teams chose to install. The dependencies and agents you didn't write are still yours to secure.


### **Disclaimer**


This report aggregates publicly reported information as of the publication date and may be revised as investigations evolve and post-mortems are released. Recommendations are general guidance. Verify against primary sources before acting on any specific claim.


### **About this series**


Quantstamp publishes the Security Beat monthly. We've conducted 1,300+ audits and secured $500B+ in digital assets across 250+ clients, including Ethereum Foundation, Aave, Polymarket, Ethena, Visa, OpenSea, Maker, Curve, Compound, and Lido. If you'd like to chat about anything security or request an audit, check out[quantstamp.com](https://quantstamp.com/) .


‍
