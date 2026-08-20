---
schema_version: "1.0.0"
document_id: "77dc28eaf5b6f6d8bcd9d1a7a1bbd2c811787fe74ae170465fc041188bec360f"
company_key: "yc-trm-labs"
company: "TRM Labs"
source_id: "yc-trm-labs-news-import-b34814ebf689"
canonical_url: "https://www.trmlabs.com/resources/blog/how-shelbit-became-a-usd-6-3-billion-settlement-layer-for-irans-illicit-economy"
published_at: "2026-08-07T21:36:00+00:00"
first_seen_at: "2026-08-08T07:14:45.137298+00:00"
fetched_at: "2026-08-08T07:14:46.146587+00:00"
content_hash: "sha256:80c508e90cc83c55728545de5be757b38083a87173b7a071de091816096547b9"
---

# How Shelbit Became a USD 6.3 Billion Settlement Layer for Iran's Illicit Economy

## Key takeaways


- On August 7, 2026, the US Treasury Department's Office of Foreign Assets Control (OFAC)[sanctioned](https://home.treasury.gov/news/press-releases/sb0598) Shelbit, its founder Siavash Kayvanpour, and a network of affiliated entities across the UAE, Poland, and Georgia, along with the separately designated Iran-based exchange Aban Tether, citing transfers between Shelbit and IRGC-controlled wallets and Shelbit's role in laundering proceeds from an Iranian gambling network.
- TRM Labs has traced more than USD 6.3 billion in blockchain flows through Shelbit, an unlicensed Dubai-registered cryptocurrency exchange, between May 2024 and March 2026.
- Shelbit's wallets held virtually no balances. Value entering the platform left almost immediately, with inbound and outbound amounts matching to within 0.1% — a pattern consistent with a settlement conduit rather than an exchange holding customer funds.
- Approximately 88% of activity, around USD 5.56 billion, moved on TRON, almost entirely in dollar-pegged stablecoins, at an average of roughly USD 54,500 per transfer.
- Shelbit rebuilt its wallet infrastructure every one to four months across two years of operation, a rotation pattern consistent with deliberate efforts to limit traceability.
- TRM identified direct exposure of approximately USD 5.6 million across 36 transfers to wallets associated with the Islamic Revolutionary Guard Corps (IRGC), a US-designated foreign terrorist organization.
- In September 2025, Shelbit sent approximately USD 2 million in four transfers on a single day to a wallet that Israel's National Bureau for Counter Terror Financing subsequently designated as Hamas infrastructure.
- Shelbit also has substantial exposure to Russian sanctions-evasion infrastructure. TRM traced approximately USD 318 million to A7, a sanctioned Russian payment network, alongside further exposure to Grinex, Rapira, and other sanctioned Russian and Central Asian services, indicating the conduit served more than one sanctioned economy.
- TRM traced approximately USD 72.6 million in exposure spread across 55 separate online gambling platforms, consistent with a conduit servicing gambling settlement at scale.
- Monthly volumes peaked at approximately USD 735 million in November 2025, having more than doubled in a single month in July 2025, and held above USD 600 million for six consecutive months.
- Shelbit's website no longer functions and the entity presents as inactive, but there is no indication that the sanctions evasion and terrorism financing it supported has ceased.


‍


{{horizontal-line}}


## US Treasury sanctions


On August 7, 2026, the US Department of the Treasury's Office of Foreign Assets Control[designated](https://home.treasury.gov/news/press-releases/sb0598) Shelbit, a Republic of Georgia-based entity operating the Shelbit Exchange, along with its founder Siavash Kayvanpour and a network of affiliated companies spanning the UAE, Poland, and Georgia. OFAC's action names Kayvanpour, UAE-based Shelbit General Trading LLC, Shelbit Technologies Ltd, Crypto Home DMCC, and NFT Home DMCC, and separately designates Aban Tether, an Iran-based exchange with its own transaction history involving previously sanctioned Iranian platforms.


Treasury cited digital currency transfers between Shelbit and IRGC-controlled addresses, transfers from Kayvanpour-controlled addresses to the US-designated exchange Nobitex, and Shelbit's role servicing a Persian-language online gambling network. The designations were issued under Executive Order 13224, with Aban Tether designated separately under Executive Order 13902 for operating in Iran's financial sector.


TRM Labs' own analysis, developed independently of Treasury's designation, traces a far larger picture. TRM has identified more than USD 6.3 billion in blockchain flows through Shelbit's infrastructure between May 2024 and March 2026, a figure that dwarfs the specific transfers Treasury cited and points to an operation functioning less as an exchange than as a settlement layer for Iran's illicit economy. What follows is TRM's full accounting of that infrastructure: how it moved money, who it touched, and what its on-chain signature means for compliance teams trying to catch the next one.


## A watch shop that moved USD 6.3 billion


In Deira, one of Dubai's older commercial districts, a company called Velorix Watches Trading LLC occupies three rooms on the fourth floor of a building reached through the lobby of a budget hotel. Behind a locked door fitted with a buzzer and a camera are a desk, a cash-counting machine, and a small display of watches that aren’t for sale.


Dubai corporate filings show that Velorix shares its registered address, and its listed owner, with another business: Shelbit, a cryptocurrency exchange through which TRM Labs has traced more than USD 6.3 billion (as of August 2026).


## What was Shelbit?


Shelbit operated as a cryptocurrency exchange registered in Dubai without a license to conduct virtual asset business. It ran a public-facing website at shelbit.com and accepted transactions — but with negligible marketing, poor customer service, and[Know Your Customer (KYC)](https://www.trmlabs.com/glossary/know-your-customer-kyc) checks that were effectively a sham. It had the outward form of an exchange and almost none of the substance, and the blockchain record shows none of the behavior either.


Its founder, per those same filings, is Siavash Kayvanpour, an expatriate Iranian.


### The Farsi-language gambling network behind the flows


Shelbit's principal customers included a Farsi-language online gambling network of more than 2,000 websites. According to TRM's analysis, it is one of the largest illegal gambling operations yet identified anywhere, and by some distance the largest ever found in Iran. Two Iranian social media figures front the sites publicly: Sasha Sobhani, son of a former senior Iranian diplomat and government minister, posting from a villa in Madrid; and Pooyan Mokhtari, an influencer and singer. Both advertise conspicuous wealth to audiences in the millions, with a link to a betting site pinned at the top of the profile.


Gambling is illegal in the Islamic Republic, punishable by imprisonment and lashing; since 2023, the prohibition has explicitly covered online betting. That has not prevented the network from obtaining access to Iran's domestic payments system, which Iran's central bank closely oversees.


In 2023, an Iranian court convicted Sobhani, Mokhtari, and Kayvanpour in the same illegal gambling case, sentencing the promoters in absentia to two years, and the exchange operator to three months for assisting them. The three were, in the court's finding, partners.


Sobhani and Mokhtari deny wrongdoing. Sobhani says he categorically rejects any involvement in money laundering, sanctions evasion, or terrorism financing, and that his role was limited to paid advertising. Mokhtari denies the allegations made against him in Dubai and says he has no affiliation with the IRGC. Both say they did not know Kayvanpour and were unfamiliar with Shelbit. Kayvanpour has not responded to requests for comment.


### VARA's cease-and-desist order and the wider Iran enforcement wave


On July 24, 2026, Dubai's Virtual Assets Regulatory Authority (VARA) ordered Shelbit to "cease and desist immediately from all unlicensed Virtual Asset activities," citing violations of money-laundering and terrorism-financing law, and warning that the exposure it had identified went beyond consumer protection to "egregious cross-border transactions with the propension to impact the integrity of the UAE financial system." It was the second action against the entity, after a 2025 penalty for operating without a license.


The order followed 18 months of sustained pressure on Iran's crypto infrastructure. In January 2026, the US Office of Foreign Assets Control[(OFAC) designated UK-registered Zedcex and Zedxion](https://www.trmlabs.com/resources/blog/ofac-sanctions-zedcex-and-zedxion-in-first-ever-designation-of-an-irgc-linked-digital-asset-exchange) as front companies for the Islamic Revolutionary Guard Corps. In June,[OFAC designated four Iranian domestic exchanges](https://www.trmlabs.com/resources/blog/three-enforcement-layers-in-five-months-ofac-designates-irans-domestic-crypto-exchanges) —[Nobitex](https://www.trmlabs.com/resources/blog/understanding-nobitex-irans-largest-crypto-exchange) , Bit Pin, Wallex, and Ramzinex — which together accounted for roughly 78% of Iran's attributed 2025 cryptocurrency volume, per TRM data.[Iran's attributed volumes](https://www.trmlabs.com/resources/blog/how-coinex-became-irans-primary-gateway-to-global-cryptocurrency-markets) have held at approximately USD 10 billion a year regardless.


None of that enforcement reached the settlement layer underneath it. TRM Labs has traced more than USD 6.3 billion in blockchain-verified flows through Shelbit infrastructure over 23 months.


## Shelbit did not function as an exchange


Custody is the defining function of a cryptocurrency exchange. Customers deposit assets and leave them on the platform between trades, so exchange wallets carry balances, frequently substantial ones, that are visible on-chain.


Shelbit's wallets behaved in the opposite manner. Across every high-volume address TRM analyzed, inbound and outbound value match to within 0.1%, and residual balances are effectively zero. The busiest single wallet received approximately USD 357.59 million and sent approximately USD 357.58 million across more than 16,500 transactions, ending with no meaningful balance. The pattern repeats across all four blockchains the operation used.


On an operation of this size, residual holdings of that scale are a rounding error. The behavior is inconsistent with intermediating trades and consistent instead with relaying payments: value accepted at one end and delivered at the other, with nothing taken into custody.


## Nearly all activity moved on a single blockchain


Total traced volume by blockchain, May 2024 to March 2026


Approximately 88% of traced volume — around USD 5.56 billion — moved on TRON. Ethereum accounted for approximately USD 382 million, Bitcoin for approximately USD 235 million, and BNB Smart Chain for approximately USD 140 million. Four other networks carried negligible amounts.


‍


Blockchain Traced volume Share of total


TRON ~USD 5.56 billion ~88%


Ethereum ~USD 382 million ~6%


Bitcoin ~USD 235 million ~4%


BNB Smart Chain ~USD 140 million ~2%


Four other networks Negligible <1%


‍


The asset moving across TRON is overwhelmingly USDT-TRC20, the TRON-native version of Tether. As a stablecoin pegged to the US dollar, it settles within seconds at negligible cost and functions as a dollar substitute that does not transit the correspondent banking system. This makes it attractive to both lawful and illicit actors who are looking to move funds efficiently – faster and cheaper.


TRON transfers averaged approximately USD 54,500, and Bitcoin transfers approximately USD 249,000 across fewer than 1,000 transfers — figures inconsistent with retail activity and more consistent with settlement between businesses.


## Infrastructure was rebuilt every few months


Shelbit rotated its wallets continuously. It retired and replaced high-volume addresses on a rolling cadence of roughly one to four months throughout the operation's life, with successor wallets each carrying between approximately USD 100 million and USD 350 million before falling dormant. Of the TRON addresses TRM attributed to the operation, only around seven in ten ever transacted, consistent with wallets provisioned in advance and cycled through in sequence. Rotation of this kind limits the continuity that makes a money-movement operation straightforward to follow.


## Monthly volume peaked at USD 735 million in November 2025


Monthly traced volume by blockchain, May 2024 to March 2026


Volumes grew steadily through 2024 and the first half of 2025, from single-digit millions to approximately USD 230 million per month. In July 2025, monthly volume more than doubled and then held between approximately USD 604 million and USD 735 million every month through December 2025. That is a step change in scale rather than incremental growth.


The February 2026 figure of approximately USD 114 million represents a marked single-month decline, coinciding with the[outbreak of hostilities](https://www.trmlabs.com/resources/blog/how-irans-crypto-market-is-reacting-to-conflict) between Iran, the United States, and Israel.[Reuters reported](https://www.reuters.com/investigations/illicit-iranian-gambling-network-helped-pull-off-4-billion-sanctions-dodge-2026-07-31/) that during the same period, a drone struck less than a kilometer from Shelbit's registered Dubai office. Activity recovered to approximately USD 340 million the following month.


## Direct exposure to the IRGC and sanctioned Iranian platforms


TRM identified approximately USD 5.6 million in transactions across 36 transfers between Shelbit and wallets associated with the IRGC, spanning July 2024 to July 2025. The IRGC is the branch of the Iranian state that oversees commercial enterprises worth billions of dollars, and is a US-designated foreign terrorist organization.


In TRM's assessment, 36 transfers distributed across a full year is more consistent with a sustained relationship than with isolated activity passing a compliance check. Blockchain analysis establishes that funds moved between these addresses; it does not establish the knowledge or intent of any party to those transfers.


Shelbit also has direct exposure to sanctioned Iranian cryptocurrency platforms, including approximately USD 2.6 million with Aban Tether, USD 1.9 million with Nobitex across 101 transfers, USD 156,000 with Ramzinex, and smaller amounts with Wallex, Bit Pin, and Bit24. TRM traced a further approximately USD 2.2 million with Zedcex, the UK-registered entity OFAC designated in January 2026 as an IRGC front company.


Notably, Shelbit's direct exposure to Iran's domestic exchanges is modest relative to the scale of the operation. When traced through intermediary wallets, however, exposure to those same platforms rises substantially — to approximately USD 10.7 million for Nobitex, USD 5.8 million for Wallex, and USD 4.3 million for Ramzinex. This suggests Shelbit generally sat downstream of Iran's exchange layer, receiving value that had already moved through one or more intermediate hops, rather than transacting with those platforms wallet-to-wallet.


## Approximately USD 2 million to a Hamas-designated wallet


On September 17, 2025, Shelbit sent approximately USD 2 million to a single wallet, across four transfers in one day.


Israel's National Bureau for Counter Terror Financing subsequently designated that wallet as Hamas infrastructure. It had no transaction history prior to that date. Over its operational life it received approximately USD 2.69 million and sent an equivalent amount, meaning Shelbit's four transfers account for roughly 74% of everything the wallet ever received, delivered on the day it first became active.


Blockchain analysis establishes that funds moved between these addresses and that the recipient was subsequently designated. It does not establish the knowledge or intent of any party to those transfers.


## Russian sanctions-evasion exposure: USD 318 million to A7


Iran remains the operation's primary nexus. Shelbit is Iranian-run, its documented off-chain relationships are Iranian, and the gambling network it served is Iranian. Against that backdrop, the scale of its exposure to Russian sanctions-evasion infrastructure is a notable secondary finding.


TRM traced approximately USD 318 million involving[A7](https://www.trmlabs.com/resources/blog/the-a7-leaks-trms-on-chain-analysis-of-russias-cryptocurrency-connections) , a sanctioned Russian payment network, the largest traced exposure to any single named sanctioned entity in the dataset. A further approximately USD 16.3 million involves[Grinex](https://www.trmlabs.com/resources/blog/garantex-grinex-and-the-a7a5-token-a-deep-dive-into-sanctions-evasion-networks) , the operational successor to the seized Russian exchange Garantex, alongside exposure to[Rapira](https://www.trmlabs.com/resources/intel-library/rapira) ,[TokenSpot](https://www.trmlabs.com/resources/blog/sanctioned-russian-exchange-grinex-and-kyrgyzstani-exchange-tokenspot-hit-in-usd-15-million-theft) , and other sanctioned Russian and Central Asian services.


An operation moving Iranian gambling proceeds and IRGC-linked value alongside Russian sanctions-evasion flows through common infrastructure is more consistent with a settlement service serving multiple clients than with a conduit dedicated to a single network. That has a practical implication for disruption: degrading such infrastructure affects several distinct threat actors simultaneously.


## Gambling exposure across 55 platforms


Consistent with the[Reuters findings](https://www.reuters.com/investigations/illicit-iranian-gambling-network-helped-pull-off-4-billion-sanctions-dodge-2026-07-31/) on the Farsi-language gambling network, TRM traced approximately USD 72.6 million in exposure between Shelbit and online gambling services, distributed across 55 separate platforms, with the largest single relationship accounting for approximately USD 46.4 million.


Breadth of this kind, spread across dozens of platforms rather than concentrated in one, is consistent with a conduit handling gambling settlement as a line of business. Exposure figures describe where value travelled, and are not an indication that the platforms concerned had knowledge of the origin of funds reaching them.


## Minimal use of mixing services


TRM traced approximately USD 370,000 in Shelbit exposure to cryptocurrency mixing services, which pool and shuffle funds specifically to sever transaction trails. On an operation of more than USD 6.3 billion, that figure is negligible.


Crypto-facilitated money laundering is commonly associated with mixers,[privacy coins](https://www.trmlabs.com/glossary/privacy-coins) , and chain-hopping. Shelbit made almost no use of any of them. Concealment appears instead to have been structural: layers of intermediary wallets and continuous rotation of infrastructure.


The practical implication for compliance teams is that screening on direct counterparties alone would have surfaced very little of this activity. Across most of the Iranian entities examined here, exposure traced through intermediaries runs from twice to several hundred times the direct figure.


## Where Shelbit stands as of August 2026


Shelbit's website no longer functions, and the entity presents as inactive. VARA's cease-and-desist order of July 24, 2026 remains in force as of publication.


There is no indication, however, that the sanctions evasion and terrorism financing this infrastructure supported has ceased.[Reuters reporting](https://www.reuters.com/investigations/illicit-iranian-gambling-network-helped-pull-off-4-billion-sanctions-dodge-2026-07-31/) in July 2026 described the network as continuing to operate. The website is dormant; the network is not.


The demand this conduit served — gambling settlement, Iranian value transfer, and sanctions evasion across more than one jurisdiction — persists independently of any single piece of infrastructure, as does the operation's demonstrated practice of rebuilding that infrastructure every few months.


## What the on-chain signature means for compliance teams


The wallets Shelbit used exhibit a distinctive and observable signature: high throughput, matched inflows and outflows, no residual holdings, and short operational lifespans. Any wallet cluster showing all four at once warrants review regardless of whether its direct counterparties screen clean.


That signature is what makes the operation detectable, because its concealment was structural rather than technical. Shelbit's near-total avoidance of mixing services, combined with reliance on intermediary layering and infrastructure rotation, illustrates a laundering model that conventional direct-counterparty screening is poorly positioned to catch.


Taken together, the structure of Shelbit's on-chain activity is consistent with purpose-built settlement infrastructure for illicit finance rather than with a cryptocurrency exchange that failed to manage compliance risk: more than USD 6.3 billion moved across 23 months through wallets that held no balances, concentrated on a single blockchain, rotated every few months, and carrying direct exposure to the IRGC, sanctioned Iranian exchanges, a Hamas-designated wallet, and sanctioned Russian payment infrastructure.


‍


{{horizontal-line}}


## Frequently asked questions (FAQs)


### 1. What was Shelbit?


Shelbit was a cryptocurrency exchange registered in Dubai without a license to conduct virtual asset business. It operated a public website at shelbit.com between at least May 2024 and March 2026. TRM Labs traced more than USD 6.3 billion in blockchain flows through its infrastructure over 23 months.


### 2. Has Shelbit been sanctioned?


Yes. On August 7, 2026, OFAC designated Shelbit, founder Siavash Kayvanpour, and affiliated entities including Shelbit General Trading LLC, Shelbit Technologies Ltd, Crypto Home DMCC, and NFT Home DMCC pursuant to Executive Order 13224. OFAC separately designated Aban Tether pursuant to Executive Order 13902. The action cited digital currency transfers between Shelbit and IRGC-controlled addresses, transfers from Kayvanpour-controlled addresses to the US-designated exchange Nobitex, and Shelbit's laundering of proceeds from a Persian-language online gambling network.


### 3. Why does TRM Labs conclude Shelbit was not a real exchange?


Custody is the defining function of an exchange, and Shelbit's wallets held virtually no balances. Across every high-volume address TRM analyzed, inbound and outbound value match to within 0.1%. The busiest single wallet received approximately USD 357.59 million and sent approximately USD 357.58 million across more than 16,500 transactions, ending with no meaningful balance.


### 4. Which blockchains and assets did Shelbit use?


Approximately 88% of traced volume, around USD 5.56 billion, moved on TRON, overwhelmingly in USDT-TRC20. Ethereum accounted for approximately USD 382 million, Bitcoin for approximately USD 235 million, and BNB Smart Chain for approximately USD 140 million.


### 5. What sanctioned entities does Shelbit have exposure to?


TRM identified approximately USD 5.6 million across 36 transfers to wallets associated with the IRGC, approximately USD 2 million to a wallet subsequently designated as Hamas infrastructure by Israel's National Bureau for Counter Terror Financing, and approximately USD 318 million involving A7, a sanctioned Russian payment network. It also carries exposure to sanctioned Iranian exchanges including Nobitex, Ramzinex, Wallex, Bit Pin, and Zedcex.


### 6. Why would direct counterparty screening have missed this activity?


Shelbit's concealment was structural rather than technical: layers of intermediary wallets and continuous infrastructure rotation, with almost no use of mixers or privacy coins. Across most of the Iranian entities TRM examined, exposure traced through intermediaries runs from twice to several hundred times the direct figure.
