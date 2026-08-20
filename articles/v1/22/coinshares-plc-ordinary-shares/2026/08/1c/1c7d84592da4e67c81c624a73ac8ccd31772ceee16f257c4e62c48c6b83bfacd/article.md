---
schema_version: "1.0.0"
document_id: "1c7d84592da4e67c81c624a73ac8ccd31772ceee16f257c4e62c48c6b83bfacd"
company_key: "coinshares-plc-ordinary-shares"
company: "CoinShares PLC"
source_id: "coinshares-plc-ordinary-shares-news-import-4e1ad97aaad3"
canonical_url: "https://coinshares.com/insights/research-data/beyond-the-coldcard-exploit/"
published_at: null
first_seen_at: "2026-08-13T15:25:55.290305+00:00"
fetched_at: "2026-08-13T15:25:56.441829+00:00"
content_hash: "sha256:812032d1ab9404b6ec4be694d53ec8f41535f78465e8c5d427e009e1e8286bc7"
---

# Beyond the Coldcard exploit

[Home](https://coinshares.com/)[Insights](https://coinshares.com/insights/)[Research & data](https://coinshares.com/insights/research-data/)


# Beyond the Coldcard exploit


7 min read


Two seemingly unrelated security incidents have recently captured the financial industry’s attention. A serious vulnerability affecting Coldcard hardware wallets resulted in the compromise of user funds, while several of the world’s largest hedge funds, including Point72, Citadel, Millennium and Two Sigma


2 , were reportedly targeted by audio-based phishing attacks designed to compromise employee credentials.


One concerns Bitcoin self-custody, the other traditional finance, but both point towards the same underlying trend: artificial intelligence is beginning to reshape the economics of cybersecurity.


A vulnerability introduced into Coldcard firmware in March 2021 remained undiscovered for more than five years before being exploited against real users. Coinkite


1 confirmed that affected devices generated substantially less entropy than intended, weakening the security of seeds created by vulnerable firmware. Estimates from blockchain analytics researchers suggest approximately $116M to $130M of Bitcoin may have been compromised across thousands of addresses.


There is no public evidence that AI was responsible for finding the vulnerability. Coinkite has suggested AI-assisted code review may have played a role, but its own retrospective testing with multiple frontier models failed to identify the flaw. More importantly, what happens if AI continues reducing the time, expertise and cost required to find subtle software flaws across all of finance?


The IMF has warned that AI could accelerate vulnerability discovery and exploitation, while the UK National Cyber Security Centre has identified AI-assisted exploit development as a significant near-term risk. AI does not need to break cryptography to materially change financial security. It simply needs to make everything around it easier to attack.


## Cryptographic authorisation versus institutional trust


Bitcoin and traditional finance have different security architectures. Bitcoin’s base layer validates spending conditions cryptographically, generally without needing to establish the real-world identity of the person initiating a transaction. Traditional financial institutions also rely extensively on cryptography, including encryption, hardware security modules and digital signatures, but cryptography is only one layer.


Banks and asset managers must also determine whether customers are genuine, employees are authorised, counterparties are legitimate and payment instructions should be trusted. These are identity and institutional trust problems, and AI is becoming increasingly effective at attacking them. Large language models can personalise phishing, reproduce voices and video, automate reconnaissance and search for vulnerabilities with less human involvement.


The recent hedge fund attacks provide a useful example with Point72, Citadel, Millennium and other major investment firms being targeted by audio-based phishing schemes in which attackers impersonated trusted personnel and attempted to obtain sensitive credentials. Reports described the wider campaign as involving AI-enabled voice cloning. Public information about the precise role played by AI remains limited, but attackers were clearly seeking to exploit human trust rather than defeat the underlying cryptography.


Several attempts appear to have been detected before material damage occurred. Yet these firms have substantial cybersecurity budgets, specialist teams and mature controls. The fact that attackers still considered employees and identity processes as viable points of entry is itself significant.


## AI is changing the economics of cyberattacks


Some of the clearest evidence comes from the UK AI Security Institute. AISI tested frontier AI models against a simulated 32-stage corporate network attack involving reconnaissance, credential theft, exploitation, reverse engineering and data exfiltration. At a fixed 10 million-token inference budget, GPT-4o completed an average of 1.7 stages in August 2024. By February 2026, Claude Opus 4.6 averaged 9.8 stages, almost a six-fold increase in 18 months.


The best individual run completed 22 of 32 stages, roughly six hours of the estimated 14 hours of work AISI believes a human expert would require. The test contained no active defenders, but the direction is clear: capabilities that once required significant expertise are becoming cheaper and easier to access.


Figure 1: Average stages completed by frontier AI agents in AISI’s 32-stage corporate cyberattack benchmark at a fixed 10 million-token inference budget. Source: UK AI Security Institute.


Vulnerability exploitation is already becoming a more important route into corporate systems. Verizon's 2026 Data Breach Investigations Report found that it had become the leading initial access method, accounting for 31% of breaches, while third-party involvement was present in 48%.


That matters for finance, which depends on cloud providers, payment processors, identity providers, custodians and thousands of other third parties. The Financial Stability Board estimates that non-bank financial institutions alone held $256.8tr of assets in 2024. AI does not need to create a new category of cyberattack to change the risk profile of this infrastructure, it only needs to reduce the marginal cost of attacking existing weak points.


The same logic applies to fraud. The ECB and EBA found that payment fraud across the EU increased from €3.5B in 2023 to €4.2BColdcard Security Advisory in 2024. Strong customer authentication continued to perform well, but the institutions highlighted the growing importance of cases where legitimate users are manipulated into authenticating fraudulent transactions themselves.


AI potentially makes that attack surface larger. Voice generation, personalised phishing, synthetic identities and automated reconnaissance increase the sophistication and scale of social engineering. If an attacker cannot defeat authentication technically, the next option is to persuade the authenticated person to defeat it for them. The machine does not need to steal the key if it can persuade the person holding it to open the door.


## What Coldcard actually tells us


The Coldcard exploit was not a failure of Bitcoin's consensus mechanism or underlying cryptography, but an implementation failure in the process used to generate cryptographic entropy. Secure protocols still require secure implementations. Bitcoin can provide strong cryptographic assurances at the protocol level while the hardware, software and users around it remain vulnerable.


Bitcoin itself continued functioning as designed, the attacker did not break its signature algorithm or consensus mechanism. Instead, weaknesses in the implementation meant some users' keys were not as unpredictable as they should have been. This complicates the traditional framing of self-custody. Removing a third-party custodian removes one form of counterparty risk, but it transfers responsibility for hardware, software, backups and operational security to the holder.


For some investors, incidents like this may strengthen the case for institutional custody or ETP structures where key management is handled professionally and combined with segregated storage, cold-storage policies, multi-party authorisation, white-listed wallets and monitoring. Institutional custody is not immune to cyber risk, but the trade-off is less about which model is “secure” and more about which risks investors are willing and equipped to manage themselves.


One reason Bitcoin security failures receive disproportionate attention is that losses are immediately visible. When a self-custody wallet is compromised, the owner generally bears the loss directly and the movement of assets is observable. Within traditional finance, losses can instead be distributed between institutions, insurers, payment providers and customers.


There is also a more important asymmetry. Bitcoin's base security architecture is designed to minimise the need for discretionary trust. Traditional finance cannot remove identity from its operating model. Banks must know who their customers are, determine what employees are authorised to do, maintain recovery mechanisms and allow human intervention when exceptional circumstances occur. As machines become better at imitating humans, identifying vulnerabilities and automating attacks, maintaining identity-based trust will likely become more expensive.


That does not mean Bitcoin or decentralised networks are inherently secure while traditional finance is insecure. Coldcard is evidence of why that claim would be too simplistic. But it does suggest that AI could dramatically alter the relative economics of different security architectures.


The Coldcard exploit showed that implementation quality matters just as much as cryptographic design. At the same time, attacks against some of Wall Street's most sophisticated investment firms illustrate the challenge facing identity-dependent financial systems as convincing human impersonation becomes increasingly cheap to produce.


These incidents are not evidence that one financial system is secure while the other is not, but there is huge asymmetry at the moment, with traditional finance having far more attack vectors and far larger sums of money at stake. We also believe that further news of AI related attacks is likely to drive people towards decentralized blockchain solutions, which are far less vulnerable relative to the centralized public ledger systems in finance or government.


#### Sources:


1 Coinkite Security Advisory, 30 July 2026,


[blog.coinkite.com](http://blog.coinkite.com/)


2[Bloomberg.com](http://bloomberg.com/)


, 5 August 2026


Published on Aug 13th, 2026
