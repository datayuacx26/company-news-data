---
schema_version: "1.0.0"
document_id: "69fb7bb98f91cc06a9bb6348c4c5eac4538130fa34383d1d76f531414fcee2cd"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-24-7/"
published_at: "2026-06-12T13:00:56+00:00"
first_seen_at: "2026-07-20T23:17:13.754958+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:4f432bce7caac2971436a9daf559a03315f4dc7edd74d421e75af8371981c19c"
---

# The Good, the Bad and the Ugly in Cybersecurity – Week 24

## The Good | Authorities Dismantle Crypto Laundering Empire & Seize Espionage Domains


Europol has[dismantled](https://www.europol.europa.eu/media-press/newsroom/news/ransomware-gangs-cut-eur-336-million-audia6-crypto-laundering-pipeline) a major cryptocurrency laundering network called “AudiA6”, known for actively facilitating illicit transactions for


[ransomware](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-double-extortion/) syndicates and cybercriminals worldwide


. Since 2022, the platform allegedly laundered more than $380 million by obscuring the origin of cybercrime proceeds through complex transaction routes for a 3-10% service commission. The joint operation, spanning 11 countries and supported by Eurojust, successfully seized multiple domains and froze a substantial amount of AudiA6’s digital assets.


Following forensic analysis stemming from a prior arrest in Poland, investigators were able to identify and[apprehend](https://pog.gov.ge/news/saqartvelos-prokuraturis-amerikis-sheertebuli-shtatebis-saidumlo-samsakhuris-polonetis-respublikis-kiberdanashaultan-brdzolis-tsentraluri-biuros-shinagan-saqmeta-saministros-da-finansta-saministros-sagamodziebo-samsakhuris-tanamshromlebis-mier-saqartveloshi-fartomasshtabiani-ghonisdzieba-chatarda-5757?lng=geo) the platform’s two senior administrators in Georgia. The industrial-scale infrastructure relied on thousands of fraudulent exchange accounts, all registered by recruited money mules using stolen[identities](https://www.sentinelone.com/cybersecurity-101/identity-security/identity-security/) . The suspects, who also managed the “Dark2Web” cybercrime forum, now face potential 20-year prison sentences for operating the illicit service.


The FBI has


[seized](https://www.justice.gov/opa/pr/justice-department-fbi-disable-13-websites-backed-suspected-chinese-agents-sought-sensitive) 13 fraudulent websites operated by suspected Chinese intelligence agents attempting to recruit U.S. citizens holding sensitive government security clearances


. The campaign used AI-generated photographs and stolen identities to construct fake consulting firms that advertised generic analyst and consultant roles across major professional networking platforms including Upwork, HUbstaff Talent, and Wellfound.


When targets applied, operatives then[pressured](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-social-engineering/) the candidates to disclose confidential or non-public information in exchange for lucrative compensation. To obscure their identities and the origin of funds, the recruiters used cryptocurrency and online payment systems.


Federal authorities have now successfully identified and dismantled the network after several targeted individuals reported the suspicious payment methods to investigators. Officials continually[urge](https://www.ic3.gov/CSA/2026/260603.pdf) current and former government personnel to exercise extreme caution regarding unsolicited recruitment offers promising easy income for vague consulting work.


## The Bad | JDY Botnet Expands Scope to Target U.S. Military Networks for Cyber Reconnaissance


A malware network previously associated with PRC-based threat groups like[Volt Typhoon](https://www.sentinelone.com/labs/chinas-influence-ops-twisting-tales-of-volt-typhoon-at-home-and-abroad/) is expanding its cyber reconnaissance operations and target scope. Known as “JDY botnet”, the network has grown rapidly from approximately 650 active bots in early 2024 to over 1,500 compromised small office/home office (SOHO) and Internet of Things (IoT) devices today


. While operators maintain a global footprint, they are now heavily concentrating efforts within the United States, specifically focusing on the military and its associated networks.


Unlike traditional[distributed denial-of-service](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-a-distributed-denial-of-service-ddos/) (DDoS) botnets, JDY functions primarily as a distributed scanning and fingerprinting network. Operators weaponize the network to quickly locate vulnerable infrastructure immediately following public[vulnerability](https://www.sentinelone.com/cybersecurity-101/cybersecurity/cyber-security-vulnerabilities/) disclosures.


The malware then registers with a central dispatch service hosted on hidden Tor networks to receive scanning assignments. Once deployed on compromised edge devices, including hardware from Cisco, Ubiquiti, and Hikvision, the botnet executes comprehensive service discovery, service banner grabbing, TLS certificate collection, and protocol fingerprinting. When it has enough administrative privileges, JDY performs exceptionally fast and stealthy SYN scanning using custom-crafted TCP packets to batch-process thousands of potential targets


.


A snippet of the JDY malware dropper that downloads and executes the malware (Source:[Black Lotus Labs](https://www.lumen.com/blog/en-us/expanded-jdy-iot-and-soho-botnet-enables-rapid-vulnerability-exploitation) )


Federal agencies previously[warned](https://www.cisa.gov/sites/default/files/2024-01/SbD-Alert-Security-Design-Improvements-for-SOHO-Device-Manufacturers.pdf) about the risks to unprotected routing infrastructure. To prevent hardware from being recruited into these vast[reconnaissance](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-cyber-reconnaissance/) networks, administrators must consistently ensure all edge devices run the latest security patches


. Organizations can proactively reduce their external attack surfaces by disabling unnecessary internet-exposed management interfaces, fully replacing default administrative credentials, and thoroughly monitoring for any unusual outbound scanning activity originating from local networks.


## The Ugly | Miasma Supply Chain Worm Continues Propagation Across Microsoft & PyPI Repositories


The ongoing Miasma self-replicating supply chain worm recently[compromised](https://opensourcemalware.com/blog/miasma-reaches-azure) 73 Microsoft GitHub repositories, including projects related to Azure, prompting GitHub to rapidly disable access. An evolution from the “Mini Shai-Hulud”[malware](https://www.sentinelone.com/blog/defending-against-sha1-hulud-the-second-coming/) , threat actors are now directly pushing malicious configuration files into legitimate source repositories


.


The hidden payloads automatically trigger code execution whenever developers open the compromised projects using popular AI coding assistants or integrated development environments (IDEs). The latest intrusions most notably involve the re-compromise of the “durabletask” PyPI package, indicating attackers retained previously stolen developer[credentials](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-credential-theft/) to seamlessly propagate the worm through automated contributor workflows.


Miasma continues to infect more packages on GitHub (Source:[TheHackerNews](https://thehackernews.com/2026/06/miasma-worm-hits-73-microsoft-github.html) )


Since the series of Microsoft repo breaches, the campaign has[evolved](https://socket.dev/blog/shai-hulud-descends-to-hades-miasma-pypi-wave) into a fresh attack wave dubbed “Hades”, actively targeting the PyPI registry. Attackers poisoned 19 PyPI packages with malicious wheel artifacts containing hidden` .pth` setup files. This mechanism executes silently during Python interpreter startup, entirely eliminating the need for victims to explicitly import the compromised packages.


The payload then downloads the standalone Bun JavaScript runtime to evade traditional network proxies, subsequently deploying a heavily obfuscated credential stealer. The malware aggressively harvests cloud access tokens, SSH keys, shell histories, and Docker configurations


while introducing new, tailored memory scrapers specifically targeting macOS and Windows environments.


Advanced in its defensive evasion, the Hades variant incorporates novel plain-text prompt injections deliberately designed to deceive[LLM-based package analysis tools](https://www.sentinelone.com/labs/building-an-adversarial-consensus-engine-multi-agent-llms-for-automated-malware-analysis/) into incorrectly classifying the malicious packages as safe.


Ultimately, these cascading supply chain attacks successfully exploit fundamental trust models within open-source ecosystems, leveraging compromised, authenticated maintainer accounts to embed persistence mechanisms directly into standard developer environments.
