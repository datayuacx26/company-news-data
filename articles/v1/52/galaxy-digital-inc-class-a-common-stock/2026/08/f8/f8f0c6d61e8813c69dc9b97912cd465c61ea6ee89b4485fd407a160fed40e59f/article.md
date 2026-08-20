---
schema_version: "1.0.0"
document_id: "f8f0c6d61e8813c69dc9b97912cd465c61ea6ee89b4485fd407a160fed40e59f"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-39d9e80cc0e4"
canonical_url: "https://www.galaxy.com/insights/research/coldcard-hardware-wallet-hack-bitcoin-resilience-ethereum-solana-inflation-proposals"
published_at: null
first_seen_at: "2026-08-08T05:24:14.319136+00:00"
fetched_at: "2026-08-08T05:24:15.029476+00:00"
content_hash: "sha256:6ddc7abfcbd8c25d937890a84567d7f0b1b15daecca6ebc4ccb7efc1e8bafc5f"
---

# Weekly Research Brief: Coldcard Hacked; Bitcoin Price Resilient; Ethereum, Solana Reconsider Inflation Schedules

*Welcome to Galaxy Research's Weekly Research Brief.*[Subscribe](https://www.galaxy.com/subscribe-to-research?utm_campaign=Weekly%20Research%20Brief%20-%20Email&utm_medium=email&utm_source=hs_email&_hsenc=p2ANqtz--YWOkviRZJsa_uYNPjl2N4ahqTpHeV86Rs9VQQeL9MWn1U09L_xNxjRhoRyGaaBrCPLXuX) *to get this newsletter delivered to your inbox every Friday morning.*


*In this week's edition, Jianing Wu shares early takeaways from the Coldcard fiasco; Jianing and Marc Hochstein consider bitcoin’s price resilience in the face of a FUD storm; and Lucas Tcheyan reviews inflation schedule proposals for Solana and Ethereum.*


*Our head of firmwide research, Alex Thorn, has been*[tracking](https://x.com/glxyresearch/status/2084411904924045370) *the onchain movement of coins stolen in the Coldcard hardware wallet exploit and working with Bitcoin community members to gather intel that might help authorities catch the thieves and recover victims' BTC. If your Coldcard device was hacked, DM*[@intangiblecoins](https://x.com/intangiblecoins) *on X. More than 200 victims have reached out, but Alex's AI agent is triaging messages, and he will eventually reply to all.*


*Got feedback on this newsletter? Email*[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection#9eecfbedfbffecfdf6def9fff2ffe6e7b0fdf1f3) *. We’d love to hear from you.*


## 🔓 Your Keys, Not Your Coins: Coldcard Wallets Hacked for $130m and Counting


**Starting July 30, attackers have drained wallets hosted on Coldcard devices across multiple escalating waves: roughly 1,082 BTC by Aug. 1, then approximately 284 BTC in the next two waves, with a fourth wave**[possibly underway](https://x.com/glxyresearch/status/2084411904924045370?s=20) **.** Galaxy Research now estimates at least 15 separate attackers are exploiting the bug independently, up from the handful of coordinated operators we initially identified, with total losses[tracked](https://x.com/glxyresearch/status/2084411915745595527?s=20) near $130 million as of Aug. 4.


The vulnerability originated in Coldcard’s seed-generation process. Normally, the device would draw[entropy](https://cryptographyacademy.com/entropy/) (a measure of randomness) for a new seed phrase from a dedicated hardware random-number generator embedded in its chip. A firmware bug introduced in a March 2021 (yes, five years ago) release instead routed part of that process through a software[fallback](https://www.pcmag.com/encyclopedia/term/fallback-system) . That fallback was[seeded](https://engineering.block.xyz/blog/predictable-rng-fallback-and-32-bit-reseed-in-coldcard-firmware) only by the device’s fixed information and its timer/clock registers, rather than a strong source of new randomness.


As a result, affected seeds that should have contained approximately 128 bits of randomness may have had an effective entropy of *only around 40 bits for affected Mk2 and Mk3 devices and 72 bits for newer Mk4, Q, and Mk5 devices* . At that level, an attacker with an approximate understanding of the device identifier and seed-generation timing could generate candidate seeds offline and compare the corresponding addresses against the public blockchain. Physical access to the wallet was not required.


Coinkite, the Canadian manufacturer of Coldcard, released emergency firmware updates, but the remediation is prospective rather than retroactive. The patches can protect seeds generated after installation, but they cannot strengthen a seed that was already created using vulnerable firmware. Those seeds remain permanently susceptible to brute-force recovery. Affected users have therefore been advised to generate an entirely new seed on patched hardware and migrate their assets to new wallets.


### Our take


Self-custody is often presented as the cleanest way to eliminate counterparty risk: investors hold their own private keys and are therefore insulated from the failure of an exchange, custodian, or other intermediary. The Coldcard exploit exposes the other side of that trade-off. Removing a third-party custodian does not *remove* custody risk; it *transfers* that risk to the hardware, software, and key-generation processes on which the holder depends.


The incident complicates one of the central assumptions behind self-custody. The maxim of “not your keys, not your coins” frames control of the private key as the primary safeguard against theft or confiscation. In this case, however, the hardware wallet itself introduced the vulnerability at the point of key creation. One user said he kept a Coldcard in a bank safe-deposit box, never connected it to the internet, and nevertheless lost 18.25 BTC in seven minutes. As this victim[mused](https://x.com/itscoachgoodman/status/2083527082223563157?s=20) , “Perhaps the hardest part about this is that I did everything right.” The failure occurred before standard operational-security practices could offer any protection.


Institutional-grade custodians, such as Coinbase Custody and Fidelity Digital Assets, typically employ controls including offline key generation, multisignature authorization, geographically distributed key material, and segregated cold-storage accounts. These measures do not eliminate custody risk, but they are designed to prevent the failure of a single device or location from resulting in a total loss. In effect, they apply the same risk-distribution principle as multisignature self-custody, but embed it in a regulated and operationally controlled custodial framework.


The more durable lesson is narrower but still significant: possession of a private key is only as secure as the process and infrastructure used to create it. ****[– Jianing Wu](https://x.com/ninja_five_)


## 📈 BTC Price Shrugs Off Coldcard Debacle, Strategy Sale, CLARITY Struggles


**The historically volatile bitcoin price has had every reason to tank this week, yet it traded**[sideways](https://www.coingecko.com/en/coins/bitcoin?chart=type%3Dprice%26mode%3Dline%26timeframe%3Dd7) **.**


The ongoing Coldcard hardware wallet[fiasco](https://www.cbc.ca/news/world/bitcoin-coinkite-security-hack-9.7295582)[shook](https://bitcoinmagazine.com/culture/self-custody-is-dead-long-live-self-custody) hardcore Bitcoiners’ faith in self-custody. Strategy[sold](https://www.sec.gov/ix?doc=/Archives/edgar/data/0001050446/000119312526329565/mstr-20260803.htm) another chunk of BTC. Passage of the CLARITY Act remained a[longshot](https://polymarket.com/event/clarity-act-signed-into-law-in-2026/clarity-act-signed-into-law-in-2026) .


But as of Thursday afternoon in New York, the original and still most valuable cryptocurrency was trading $64,452.01, essentially flat on a 24-hour and 7-day basis and up about 1.2% from a month earlier. What gives?


### Our take


Though tragic, the Coldcard exploit affected a small amount of bitcoin – about $130 million worth, compared to a $1.2 trillion market cap – and a self-selected minority of hobbyist users. Self-sovereign HODLERs may be living up to the cypherpunk ideal, but they’re outliers. Bitcoin, for better or worse, has institutionalized, and institutional investors prefer exchange-traded products over direct ownership; the ones that do trade actual coins without paper wrappers typically keep them at third-party custodians.


So as shocking and saddening as it was to see hackers drain so many individuals’ life savings, market participants quite understandably treated the incident as a product-specific implementation failure.


Zooming out, crypto just finished its best month in 2026 so far, seemingly decoupling from the[turbulence in equities](https://apnews.com/article/stock-markets-rates-korea-ai-oil-e31b3a442bcb957a53f1823ef21e73e8) during the same period. Bitcoin rose roughly 7% in July, its second-best monthly gain of the year, and ETH climbed about 18%, its best month this year. Spot bitcoin ETFs, which logged their worst month on record in June, turned to net inflows in July with $194 million. The Crypto Fear & Greed Index held in "fear" territory for much of last month despite the price gains but is turning toward “neutral.”


BTC spent most of July range-bound between the high-$50,000 and mid-$60,000 range. The price repeatedly tested its 200-week moving average from both sides as Iran headlines, chip-sector risk-off spillover, and shifting ETF flows pushed it in and out of range, before closing the month firmer near $64,000.


All that happened even though Strategy was a net seller through most of July following broader pressure from the preferred-stock stress that forced a capital-structure[overhaul](https://am.galaxy.com/insights/research/june-2026-market-commentary) in late June. Michael Saylor’s company[sold](https://www.strategy.com/press/strategy-sells-3-588-btc-to-fund-digital-credit-dividends-now-holds-843-775-and-2-55b) 3,588 BTC (~$216 million) in early July – its largest single sale ever – to fund preferred-stock dividends, then raised roughly $466.7 million and $263.5 million in back-to-back weeks by selling MSTR common stock rather than touching its BTC holdings.


So, by Monday morning, when Strategy[disclosed](https://www.sec.gov/ix?doc=/Archives/edgar/data/0001050446/000119312526329565/mstr-20260803.htm) a further sale of 1,638 BTC (~$105 million), investors may well have grown used to it.


July was also the month we at Galaxy Research[cut our odds](https://www.galaxy.com/insights/research/clarity-act-30-percent-odds) of the CLARITY Act passing in 2026 passage odds to 30%, from a[coin-flip](https://www.galaxy.com/insights/research/weekly-top-stories-06-26-26-trump-quantum-eo-clarity-act-odds-gnosis-ens-dao-governance) in June. Yet the crypto market didn’t wobble on the bearish CLARITY developments that led us to change our probability assessment. Bitcoin finished July well above where it traded back when CLARITY's odds[stood at 60%](https://www.galaxy.com/insights/research/clarity-act-passage-odds-60-percent-senate-calendar-june-2026) .


As we’ve stated before, a CLARITY failure wouldn't leave the industry empty-handed, because regulators can still deliver most of what it wants through agency guidance over the next few years, just without statute permanence.


Supporting that view,[institutional](https://www.morganstanley.com/press-releases/etrade-completes-crypto-spot-trading-rollout)[building](https://www.pymnts.com/personnel/2026/bank-of-america-promotes-execs-to-champion-ai-and-crypto/)[continues](https://www.bny.com/corporate/global/en/about-us/newsroom/press-release/bny-launches-global-digital-transfer-agency-capabilities-extending-leadership-in-fund-servicing-to-digital-market.html) . It’s possible the market is pricing CLARITY as one lever among several rather than the sole gate to institutional adoption.


Perhaps it’s too soon to say the bottom is in. But this resilience is remarkable.[– Jianing Wu](https://x.com/ninja_five_) and[Marc Hochstein](https://x.com/MarcHochstein)


## 🪙 High on Their Own Supply? Ethereum, Solana Reconsider Inflation Schedules


[Ethereum](https://github.com/ethereum/EIPs/blob/5d64bacfa08b8a9190c6aa90d5bf4f8f74258173/EIPS/eip-8361.md) **and**[Solana](https://github.com/solana-foundation/solana-governance-proposals/pull/4) **developers advanced new initiatives this week that would adjust the blockchains’ inflation schedules.**


Six researchers, including the Ethereum Foundation's Justin Drake, submitted[EIP-8361 (Tapered Issuance Burn)](https://github.com/ethereum/EIPs/pull/12081) . This Ethereum improvement proposal would[burn a rising share of validator rewards](https://pintail.xyz/posts/2026/eip-tapered-issuance-burn/) as more ETH is staked, reaching 100% once half the supply is staked and thereby removing any issuance incentive to stake beyond that point. At today's ratio of roughly one-third of supply, consensus-layer yield would fall from about 2.6% to 1.2%. Maximal extractable value ([MEV](https://a16zcrypto.com/posts/article/mev-explained/) ) and priority fees would remain untouched. If approved, the reduction would phase in over 18 months. Combined with normal fork lead time, stakers would have close to two years to adjust. The EIP is still a draft, and nothing has been scheduled or voted on. It is being considered for inclusion in Hegotá (Ethereum’s next major upgrade after[Glamsterdam](https://ethereum.org/roadmap/glamsterdam/) , which is scheduled for this fall). Selection is expected to continue into November, and if approved, the upgrade is unlikely to ship before well into 2027.


Meanwhile, two Solana governance proposals are moving in parallel through the network's[new onchain governance system](https://github.com/solana-foundation/solana-governance-proposals) , the first real test of the process. SGP-0002 carries a Solana improvement document,[SIMD-0550](https://github.com/solana-foundation/solana-improvement-documents/pull/550) (from Helius engineers[lostintime101](https://x.com/__lostin__) and[0xIchigo](https://x.com/0xIchigo) ), which would double the annual disinflation rate to 30%. This in turn would pull the 1.5% terminal inflation floor forward to 2029 from 2032 and remove[an estimated 18.9 million SOL](https://forum.solana.com/t/simd-0550-proposal-to-double-disinflation/4874) of future emissions. SGP-0003 carries[SIMD-0553](https://github.com/solana-foundation/solana-improvement-documents/pull/553) (from Temporal's[cavemanloverboy](https://x.com/cavemanloverboy) ), which would replace Solana's flat per-signature fee with a resource-based fee that scales with the compute each transaction requests and is burned in full. Estimates put the potential increase in daily SOL burns from roughly 650 to between 7,500 and 9,000. Each SGP secured the required 15% support from active stake to move to a discussion phase, which will take place over the coming 16 days, followed by a[fixed 11-epoch (roughly 22-day) process](https://github.com/solana-foundation/solana-governance-proposals) of discussion, stake snapshot, and voting. Passage requires two-thirds of decisive stake to pass.


### Our Take


Ethereum and Solana are posing the same fundamental question to their stakeholders: how much security budget is required to secure their chains, and are the tradeoffs worth adjusting those parameters? The proposals differ in design and process but emerge in a similar market environment where both tokens have suffered large drawdowns and consistently underperformed.


The Ethereum proposal is the more contentious of the two. It seeks to reform a mature issuance curve that was already altered once during the 2022 transition from proof of work to proof of stake. Proponents argue the current model creates an open-ended incentive for staking rates to trend toward 100% ([the authors project roughly 55% by 2028](https://pintail.xyz/posts/2026/eip-tapered-issuance-burn/) ), raising the risk of concentration among liquid staking providers and large operators. Their design seeks to remove that permanent yield floor, limit dilution, and strengthen ETH's monetary properties.[Critics](https://ethereum-magicians.org/t/eip-8363-tapered-issuance-burn/29263) , including a number of high-profile voices across DeFi, staking, and the broader developer community, warn that the resulting yield compression could pressure solo stakers, gut Ethereum's DeFi ecosystem, and dampen institutional demand. They also question the compressed timeline for a material monetary policy change.


Solana's changes are narrower and more explicitly focused on scarcity. Accelerating the path to the terminal inflation rate would primarily benefit long-term holders by removing a meaningful amount of future supply with the cost falling on current stakers and validators through faster yield compression. The resource-based fee burn (SIMD-0553) would strengthen the link between network usage and token value accrual, again favoring holders by raising costs for more complex transactions. Both Solana proposals appear less controversial than Ethereum's, in part because similar ideas have been debated across multiple iterations over the past year and a half.


Inflation successfully bootstrapped both networks. It paid validators to secure chains that had almost nothing running on them and bought the runway that produced the ecosystems both now enjoy. That subsidy has an expiry date. The healthier long-term state is one in which validating is a thin-margin utility attached to a profitable onchain business or is funded by blockspace demand rather than emissions. Supply-side adjustments can help at the margin and may generate useful narratives, but they are not the binding constraint on either chain. Demand is what ultimately reprices these assets and earns them the legitimacy to move from emerging settlement layers to infrastructure used globally for large-scale finance and retail activity.


Both ecosystems have spent this cycle improving their technology stacks, advancing institutional adoption, and expanding retail products. That work should remain the priority. The security concerns that the Ethereum proposal raises may prove valid over a longer horizon, because neither chain is worth much if it cannot be distinguished from a centralized alternative. But, given current levels of adoption, they should not consume the oxygen the demand-side efforts still need.[-Lucas Tcheyan](https://x.com/Uptodatenow)


## Other News


-


🏦 Wells Fargo to roll out tokenized deposits on[‘proprietary blockchain](https://www.wsj.com/finance/banking/wells-fargo-to-roll-out-tokenized-deposits-for-corporate-clients-75c5d2cc?mod=Searchresults&pos=2&page=1) ’


-


🔮 Polymarket said to seek[>$20b valuation](https://www.bloomberg.com/news/articles/2026-08-04/polymarket-seeks-more-than-20-billion-valuation-in-funding-round) in funding round


-


🐎 Texas governor[pauses](https://www.reuters.com/business/energy/texas-governor-orders-pause-new-data-center-approvals-pending-audit-2026-08-04/) data center grid connections pending audit


-


🦑 Kraken affiliate, Broadridge give xStocks tokenholders[voice in proxy voting](https://www.prnewswire.com/news-releases/broadridge-and-payward-services-collaborate-to-give-xstocks-holders-a-voice-in-corporate-governance-302843541.html)


-


🤖 Sui blockchain’s co-founder and CTO is leaving to[join Anthropic](https://x.com/b1ackd0g/status/2085128160384000108?s=20)


-


🗳️ Crypto PAC Fairshake[scores](https://www.theblock.co/post/410744/fairshake-michigan-washington-primaries) primary wins in Mich., Wash. but one big[loss](https://www.coindesk.com/policy/2026/08/05/crypto-s-campaign-efforts-see-rare-loss-but-crypto-roster-in-congress-likely-to-grow)


-


🥊 Late Ondo Finance founder’s mother moves to[oust CEO](https://www.coindesk.com/policy/2026/08/06/power-struggle-erupts-at-ondo-finance-after-founder-s-death) of RWA issuer


-


🫆 FBI agent accused of[stealing $1m in crypto](https://www.nytimes.com/2026/08/03/us/politics/fbi-agent-cryptocurrency.html?searchResultPosition=2) tied to['adversarial nation](https://krdo.com/politics/cnn-us-politics/2026/08/03/fbi-agent-accused-of-stealing-nearly-1-million-in-cryptocurrency/) '


-


🗞️🤡🙄WSJ CLARITY[editorial](https://www.wsj.com/opinion/clarity-for-crypto-sort-of-3f5283a7?mod=Searchresults&pos=2&page=1) likens crypto networks to ... eBay?[C’mon](https://x.com/neerajka/status/2084769705907155183?s=46)


## Chart of the Week: Open-Weight Models Win Tokens, Closed Ones Keep Cash


Chinese open-weight AI models have turned inference into a commodity, calling into question the sustainability of the frontier labs’ 60%-80% markups. Yet the walls may not be closing in on OpenAI and Anthropic as quickly as benchmarks and public sentiment suggest.


Jesse Zhang, co-founder of Decagon, a maker of customer-service chatbots, recently[argued](https://decagon.ai/blog/everyone-is-wrong-about-open-source-ai-in-the-enterprise) that running open models requires technical finesse many corporations would rather pay to avoid. As the old adage goes, “no one gets fired for buying IBM.” Most enterprises could build the infrastructure in-house, but it's far easier to hand off the problem to Anthropic or OpenAI.


Data from OpenRouter, a single API endpoint that unifies inference providers from both open- and closed-weight models, supports this hypothesis. Since mid-May, open-weight models’ share of token usage has climbed nearly one-third to 75%. However, in terms of estimated dollars spent on the platform, closed-source models still crush open-weights.[– Taj Singh](https://x.com/taj_trader)


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, may hold financial interests in certain assets referenced in this content. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
