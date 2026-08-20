---
schema_version: "1.0.0"
document_id: "3670012b787ff0cd944afc0a50735722b84d55084a4d3ecd120967aa974d7be3"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-31-8/"
published_at: "2026-07-31T13:00:08+00:00"
first_seen_at: "2026-07-31T17:07:02.495385+00:00"
fetched_at: "2026-07-31T17:07:03.984563+00:00"
content_hash: "sha256:b103d7febe624b158d3dece22ef0a31d5c8e9f0fed95acea9f0e9d86e14f008d"
---

# The Good, the Bad and the Ugly in Cybersecurity – Week 31

## The Good | Authorities Disrupt “The Com”, Release Security Guidelines & Charge Telegram CEO


Europol and law enforcement partners from nine countries have


[flagged](https://www.europol.europa.eu/media-press/newsroom/news/europol-led-action-against-nihilistic-violent-extremist-network-com) over 4000 URLs for removal to disrupt the online ecosystem of The Com


. Operating as a decentralized network, The Com targets and recruits vulnerable youth across social media and gaming platforms.


Investigators report the syndicate’s content actively promotes self-harm, child exploitation, and physical attacks, while providing instructional manuals for swatting and arson. This multi-week[joint operation](https://home-affairs.ec.europa.eu/news/protecteu-commission-presents-new-counterterrorism-agenda-2026-02-26_en) builds upon[Project Compass](https://www.europol.europa.eu/how-we-work/operations/project-compass) , a year-long international initiative that previously resulted in 30 arrests and identified 179 suspects linked to the criminal network.


From U.S. and Australian governments, a new joint cybersecurity guidance urges critical infrastructure organizations to proactively prepare


[isolation](https://www.sentinelone.com/cybersecurity-101/cybersecurity/network-segmentation/) plans for operational technology systems


. The[advisory](https://www.cyber.gov.au/business-government/secure-design/operational-technology-environments/ci-fortify/ci-fortify-advice-for-isolating-vital-systems) provides recommendations for physically and logically disconnecting vital infrastructure from corporate networks during severe cyberattacks.


Since state-sponsored threat actors and cybercriminals continuously target these essential sectors to facilitate[espionage](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/cyber-espionage/) , data extortion, and disruptive operations, such resources help businesses shore up their operational resilience, documentation, and testing procedures.


The Russian Federal Security Service (FSB) has formally


[charged](https://www.reuters.com/world/russias-fsb-charges-telegram-founder-durov-with-facilitating-terrorism-ifax-says-2026-07-29/) Telegram founder Pavel Durov with aiding terrorist activities and violating federal laws regarding prohibited information


. Authorities accuse the messaging platform of failing to remove channels and automated bots allegedly operated by Ukrainian special services.


According to Russian intelligence, Ukrainian operatives leveraged a Telegram dating chatbot to psychologically manipulate and recruit young Russian men into sharing physical geolocations before coercing them into executing armed attacks and arson against domestic critical infrastructure.


This charge is the latest action against Telegram preceded by Durov’s[arrest](https://www.tribunal-de-paris.justice.fr/sites/default/files/2024-08/2024-08-28%20-%20CP%20TELEGRAM%20mise%20en%20examen.pdf) in 2024,[restrictions](https://www.m24.ru/news/17032026/883264) placed on the platform, and a[near-blockade](https://www.reuters.com/technology/russian-authorities-weigh-block-telegram-early-april-rbc-reports-2026-02-26/) from earlier this year.


## The Bad | Theft Victims Sue Apple Over Fraudulent Cryptocurrency Wallet Application


Three individuals have filed a[lawsuit](https://www.documentcloud.org/documents/28516878-apple-sparrow-complaint/) against Apple after losing approximately $1.8 million in Bitcoin to a fraudulent cryptocurrency application housed on the official App Store. Between May and August 2025, the plaintiffs downloaded a malicious app


[impersonating](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/spoofing/) “Sparrow Wallet”, a legitimate platform exclusively available on desktop operating systems


.


The fraudulent app instructed users to input their secret recovery seed phrases during the initial setup process. Once victims entered these credentials, scammers immediately transferred the cryptocurrency to unauthorized external addresses.


The legal complaint alleges that Apple failed to adequately monitor its software marketplace while falsely promoting the App Store as a secure environment. Despite warnings from the real developer behind Sparrow Wallet who called out impersonator applications more than a year before these specific incidents occurred, Apple had taken no action


.


Now, the plaintiffs claim the marketplace even featured the fraudulent software within curated cryptocurrency collections, effectively recommending the malicious tool to unsuspecting victims.


> There is still a scam ‘Sparrow Wallet’ app on the[@Apple](https://x.com/Apple?ref_src=twsrc%5Etfw) App Store, despite myself and others having reported it weeks ago.
>
>
> Worse, you have to install it to report it.
>
>
> Always go to the wallet’s website to find the link to download it. App stores will not protect you.
>
>
> — Craig Raw 🐦 (@craigraw)[January 6, 2024](https://x.com/craigraw/status/1743573811230863813?ref_src=twsrc%5Etfw)


Financial theft within mobile ecosystems continues to grow as researchers recently[identified](https://securelist.com/fakewallet-cryptostealer-ios-app-store/119474/) dozens of similar wallet impersonators actively targeting user recovery phrases and keys across the Apple App Store. In response to the allegations, Apple’s filing[stated](https://techcrunch.com/2026/07/27/apple-sued-after-alleged-app-store-crypto-scam-cost-users-1-8m/) that it takes swift action to remove infringing content and provides dedicated reporting mechanisms for suspected fraud.


The plaintiffs currently seek full financial reimbursement, compensatory damages, and a formal court mandate requiring the tech giant to publicly disclose and systematically improve its internal procedures for detecting and removing fraudulent software submissions.


## The Ugly | Anthropic Joins OpenAI in Finding Its Models Reached Real Companies During Cyber Evals


Anthropic[disclosed](https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals) this week that three of its models reached real production systems during cybersecurity testing


. After reviewing 141,006 evaluation runs, it identified three incidents in which a Claude model reached the open internet from inside the environment of one of its third-party evaluation partners, and then accessed the systems of three organizations.


Unlike OpenAI’s recent breach of[Hugging Face](https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-30-8/) , no zero-day was involved, according to Anthropic. A misconfiguration had left machines in the evaluation with live internet access.


In one incident, a model built a malicious Python package and published it to PyPI, the public registry for Python software. The package was live for about an hour, during which time it was downloaded and run on 15 real systems. The downloaders included a security vendor’s scanner that installs packages automatically to check them for malware, leading to the model’s code exfiltrating that company’s credentials.


Anthropic said it found no evidence of a model pursuing a goal of its own, characterizing the incidents as a harness and operational failure rather than a model alignment failure


, and noted that the safeguards it ships on released models would have blocked the behavior.


OpenAI also[updated](https://openai.com/index/hugging-face-model-evaluation-security-incident/) its account of the Hugging Face breach this week, revealing the same models had also used publicly exposed credentials to compromise accounts at four other services


.


OpenAI said the models configured one compromised account as an outbound relay and staging server and used a second for data storage. The remaining two accounts were accessed in read-only mode.


Full attack chain of the breach (Source: Hugging Face)


Although OpenAI’s models extracted partial datasets containing CyberGym solutions and operated multiple concurrent workloads, the activity ultimately left critical encryption keys behind, exposing the operation. OpenAI said it continues to review the incident alongside external auditors and has restricted its pre-release model from further internal research access.
