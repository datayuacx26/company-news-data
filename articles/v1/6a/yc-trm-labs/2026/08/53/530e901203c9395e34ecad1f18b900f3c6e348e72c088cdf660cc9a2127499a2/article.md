---
schema_version: "1.0.0"
document_id: "530e901203c9395e34ecad1f18b900f3c6e348e72c088cdf660cc9a2127499a2"
company_key: "yc-trm-labs"
company: "TRM Labs"
source_id: "yc-trm-labs-news-import-b34814ebf689"
canonical_url: "https://www.trmlabs.com/resources/blog/the-largest-hardware-wallet-exploit-of-2026-inside-the-usd-116-million-coldcard-hack"
published_at: "2026-08-05T12:00:00+00:00"
first_seen_at: "2026-08-05T23:07:28.130817+00:00"
fetched_at: "2026-08-05T23:07:30.055529+00:00"
content_hash: "sha256:fb231c83633dd9dacde27552c2945a4e3e54bd2260ba48e9b5878f9b6e151188"
---

# The Largest Hardware Wallet Exploit of 2026: Inside the USD 116 Million Coldcard Hack

## Key takeaways


- A firmware bug from March 2021 weakened seed randomness on some Coldcard wallets, cutting key strength from 128 bits to as little as 40 — brute-forceable without physical access.
- Starting July 30, 2026, attackers drained roughly 1,816 BTC (~$116 million) from over 5,200 addresses across four waves.
- This is the third-largest crypto hack of 2026, bringing the year's total past $1.2 billion across 276 incidents.
- Stolen funds are pooling at a few attacker addresses with minimal laundering so far — no layering or mixing.
- Transaction patterns suggest multiple attackers may be involved, so TRM isn't attributing the theft to a specific actor yet.
- Updating firmware doesn't fix existing wallets. Anyone who generated a seed on a Coldcard between March 2021 and the patch should treat it as compromised and migrate to a new seed.


{{horizontal-line}}


Beginning July 30, 2026, an attacker exploited a five-year-old firmware flaw in Coinkite's Coldcard hardware wallet to systematically drain bitcoin from affected devices. The flaw traces to a March 2021 firmware release and a build configuration error that caused seed generation to fall back on a weak software random number generator rather than the device's hardware-based source of entropy. The result was a collapse in effective key strength, from a designed 128 bits down to as little as 40 bits on older devices, low enough to brute force with modern computing power and without ever needing physical access to the device itself.


At least four waves of theft have followed as of this writing, and Galaxy Research's running tally of losses stands near 1,816 BTC, worth close to USD 116 million, drained from more than 5,200 addresses.


TRM's own tracing shows the majority of victim funds pooling at a small number of attacker-controlled addresses with limited onward movement. Differences in transaction construction during each wave of thefts hint there may be multiple attackers at work. This will be the third largest attack of 2026 year to date, bringing the total hacked to over USD 1.2 billion across 276 incidents.


As with other large compromise cases, the true scale of the Coldcard exploit will likely take time to emerge. Funds are still moving and victims often come forward over months or years as thefts are noticed, so the current figures should be treated as preliminary rather than final. For the same reason, and with transaction patterns pointing to multiple attackers, TRM is not attributing the theft to a specific actor at this stage.


## Incident overview


On July 30, 2026, an attacker began sweeping bitcoin out of wallets secured by Coldcard hardware devices. Twenty-five minutes later, roughly 594 BTC, worth close to USD 38 million at the time, had moved out of approximately 500 wallets into a single consolidation address. That first sweep proved to be the opening move rather than the complete story. Three additional waves followed over the next four days, and Galaxy Research's running tally now stands near 1,816 BTC, worth close to USD 116 million, drained from more than 5,200 addresses. A fourth wave was still moving through the mempool at the time of this assessment.


## The vulnerability


The attack stemmed from a flaw in the Coldcard hardware wallet. Firmware version 4.0.1, released in March 2021, contained a bug that caused some devices to generate wallet seeds with significantly weaker randomness than intended. As a result, wallets created on affected firmware could have private keys that were vulnerable to brute-force attacks.


Updating the firmware fixes the issue for future wallet creation but does not protect wallets whose seeds were generated under the vulnerable firmware. Users who created wallets on affected devices must generate a new seed and transfer their funds to a new wallet.


## On-chain analysis


According to TRM, most victim funds are pooling at a small number of attacker-controlled addresses. The laundering of these funds so far has been limited to a single 64.9 BTC Wasabi deposit and 200 ETH deposited to Tornado Cash on August 4, 2026.


Where remaining funds have moved past the initial receiving address, it has amounted to a single additional hop of consolidation rather than any attempt at layering or mixing. This points to an attacker, or attackers, potentially still working out how to move a sum large enough to attract attention wherever it lands. In contrast, some professional crypto hackers, like North Korea's TraderTraitor, often begin aggressively laundering stolen funds within hours or days.


Analysis of the OP_RETURN fields identified numerous spam messages directed at the hackers. One such message offered to launder the stolen funds in exchange for a 7% fee. While this could have been an opportunistic scammer attempting to defraud the hackers, it also illustrates how quickly illicit service providers sought to capitalize on the incident by advertising money laundering services directly on-chain.


### Implications for self-custody


2026 has already produced major DeFi exploits, exchange breaches, and now a cold storage failure. The Coldcard incident reinforces that self-custody relocates risk rather than eliminating it. A wallet is only as trustworthy as the process that generated its key, making firmware and entropy generation just as critical to scrutinize as the device itself.


Open-source code and audits improve security but are not guarantees. Firmware and entropy generation require ongoing review, and multisignature setups that combine independently designed devices and independently generated entropy provide meaningful defense in depth by reducing reliance on any single implementation.


Anyone who generated a Coldcard seed between March 2021 and the recent patch should treat that seed as compromised regardless of current firmware. Users should generate a new seed on updated hardware, verify the new wallet fingerprint and a receive address, and migrate funds beginning with a small test transaction before transferring the remainder. TRM continues monitoring the on-chain movement of the stolen funds and will update this assessment as additional information becomes available.


{{horizontal-line}}


## Frequently asked questions (FAQs)


### 1. What caused the Coldcard hack?


A build configuration error in firmware version 4.0.1 (released March 2021) caused some devices to fall back on a weak software random number generator instead of the device's hardware-based entropy source when generating wallet seeds. This weakened effective key strength enough that affected private keys could be brute-forced.


### 2. Do I need physical access to a Coldcard device to exploit this vulnerability?


No. Because the vulnerability weakens the randomness of the seed itself, attackers can potentially brute-force the resulting private key without ever touching the physical device.


### 3. Does updating my Coldcard's firmware protect my existing wallet from this exploit?


No. A firmware update prevents new wallets from being generated with weak randomness, but it does not retroactively fix a seed that was already generated on vulnerable firmware. If your seed was created between March 2021 and the patch, you need to generate a brand-new seed on updated hardware and move your funds — the old seed should be treated as compromised.


### 4. How much bitcoin has been stolen in the Coldcard hack, and is that figure final?


Galaxy Research's running tally stands near 1,816 BTC (~$116 million) from over 5,200 addresses, across four waves of theft starting July 30, 2026. TRM treats this as preliminary: funds are still moving, a fourth wave was still in the mempool at time of writing, and victims often surface over months or years as thefts are noticed.


### 5. Has TRM attributed the Coldcard hack to a specific group, like a North Korea-linked actor?


No. Transaction construction differs across the four waves, suggesting more than one attacker may be involved, and the laundering pattern so far (limited consolidation, one Wasabi deposit) looks more exploratory than the fast, aggressive laundering typical of groups like North Korea's TraderTraitor. Given that and the early stage of the investigation, TRM is not attributing the theft to a specific actor at this time.
