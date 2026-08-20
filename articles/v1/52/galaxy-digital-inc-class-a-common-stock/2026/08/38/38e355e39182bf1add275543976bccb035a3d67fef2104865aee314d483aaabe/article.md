---
schema_version: "1.0.0"
document_id: "38e355e39182bf1add275543976bccb035a3d67fef2104865aee314d483aaabe"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-3264694a8f1e"
canonical_url: "https://www.galaxy.com/insights/research/solana-q2-2026-report-tokenized-economy-dex-rwa-stablecoins"
published_at: null
first_seen_at: "2026-08-11T02:09:34.974118+00:00"
fetched_at: "2026-08-11T02:09:36.774517+00:00"
content_hash: "sha256:eb1ec9d44ee171c8f2781d6d158668dbae2190534489bf944c41364a8eca557c"
---

# Solana Q2 2026 Update: Building for the Tokenized Economy

## Introduction


Solana’s Q2 performance extended the pattern described in our[Q4 2025](https://www.galaxy.com/insights/research/solana-q4-2025-etfs-perps-prediction-markets-internet-capital-markets) and[Q1 2026](https://www.galaxy.com/insights/research/solana-q1-2026-dex-rwa-stablecoins-market-share) reports. The blockchain retained a leading position across core metrics, including decentralized exchange (DEX) volume, application fees, and network fees, even as absolute activity continued to decline amid weaker market conditions.


The headline figures, however, provide an incomplete picture of the ongoing effort to reshape Solana’s onchain economy from one heavily dependent on memecoin speculation into a platform capable of supporting financial activity across a broader range of asset classes. Progress is occurring throughout the stack. In the core infrastructure and execution environment of the underlying chain. In the legal and technical frameworks required to bring traditional assets onchain. In new trading venues and DeFi integrations. And in the consumer-facing applications designed to reduce friction and expand access to those assets. Solana’s differentiation is increasingly extending beyond low-cost, high-throughput execution toward the combination of execution, liquidity, distribution, and composability. These developments strengthen the case that Solana can diversify beyond crypto-native speculation.


Yet capability continues to run ahead of adoption. Most tokenized value on Solana remains idle. Lending markets have not yet converted the network’s growing tokenized asset and stablecoin base into durable borrowing demand. Application fees remain concentrated in memecoin activity. Solana also continued to lose today’s highest-value standalone-venue contests in Q2. Hyperliquid retained the deepest perpetuals liquidity, while the prediction-market boom led by Polymarket and Kalshi occurred largely outside the Solana ecosystem.


The question heading into the second half of 2026 is therefore no longer whether the necessary infrastructure exists, but whether Solana can convert tokenized issuance and distribution into recurring collateral use, borrowing demand, trading activity, and fees. Q2 made the diversification thesis more credible. The primary benchmark for the remainder of 2026 will be whether that diversification becomes economically meaningful.


## Network Metrics


### Network Performance and Health


Solana maintained strong network health throughout Q2 2026. Median[slot duration](https://www.helius.dev/blog/solana-slots-blocks-and-epochs) remained at the 400-millisecond target for the entire quarter while the network recorded zero downtime for the ninth consecutive quarter. Continued work across the Agave client series has steadily improved consistency. Agave v4.0, which became the recommended mainnet version in late May, and Agave v4.1, which launched at the end of June, laid the groundwork for the next major performance step. Agave v4.2,[targeted for initial mainnet rollout in early-to-mid August](https://github.com/anza-xyz/agave/wiki/v4.2-Release-Schedule?ref=blog.colosseum.com) , includes notable shifts including higher[compute unit limits](https://docs.chainstack.com/docs/solana-compute-budget) (100M CUs),[XDP](https://solana.com/upgrades/xdp) networking enhancements,[direct mapping](https://anatomi.st/blog/2025_06_27_pwning_solana_for_fun_and_profit) , and better[repair mechanisms](https://solana.stackexchange.com/questions/6237/what-does-repair-do) . Its headline change is a reduction in slot times from 400 to 200 milliseconds under the Solana improvement document[SIMD-0525](https://solana.com/upgrades/reduced-slot-times) , which is expected to halve confirmation latency. The reduction is phased rather than a single step, moving down in four 50-millisecond decrements across successive epochs (400 to 350 to 300 to 250 to 200) and advancing to each new level only if block[skip rates](https://solanavalidators.wiki/glossary/skip-rate/) stay low enough, so full activation is expected to play out over the remainder of 2026. Because a shorter slot also narrows the window in which a single leader controls block building, the change doubles as a censorship-resistance measure.


Non-vote throughput continues to hold within a stable band under Solana’s maximum capacity, with median transactions per second (TPS) in the low thousands and the higher percentiles showing intermittent bursts rather than sustained pressure. Blocks had enough room to fit everyone’s transactions for most of the quarter with the current 60 million compute unit ceiling. On July 30, Solana mainnet[activated SIMD-0286](https://solana.com/news/solana-changelog-july-30-2026) , raising block compute limits to 100 million CUs, the second increase since limits were raised to 60 million in July 2025.


Nominal SOL staking yield drifted down roughly 7% over the quarter, continuing a slow decline from last quarter.


Inflation continues to account for close to 90% of staking yield, with maximal extractable value ([MEV](https://www.helius.dev/blog/solana-mev-an-introduction) ) and transaction fees making up the small remainder, and that real-yield share thinned further in Q2 as fees compressed. This, along with the native token SOL’s broader underperformance, is the backdrop for the disinflation proposal now moving through governance (covered in more detail below).


While client diversity remained consistent in Q2, with the Anza R&D labs’s[Agave](https://docs.anza.xyz/) version dominant, client adoption continues to diversify. Notably, Jito’s newest Agave JitoBAM version picked up market share, growing from 28% to 33%. BAM (which stands for Block Assembly Marketplace) enables the use of “plugins” for applications to control transaction ordering (otherwise referred to as application-controlled execution, or ACE) and enable the same functionality on Solana’s general-purpose blockchain as application-specific blockchains ([see prior Galaxy Research coverage here](https://www.galaxy.com/insights/research/solana-firedancer-anza-alpenglow-internet-capital-markets) ).


### Other Notable Network-Level Developments


Development work continues to focus on shipping the Alpenglow upgrade, with most core work going toward its prerequisites while the token-economics debate returned to the center of governance.


-


**Alpenglow:** Solana's largest protocol upgrade[reached public testing on May 11](https://www.coindesk.com/tech/2026/05/11/the-biggest-consensus-overhaul-in-solana-history-is-officially-live-for-testing) , when Anza ran it on a community test cluster and demonstrated a live migration off the old consensus mechanism. Alpenglow replaces[Proof of History](https://solana.com/news/proof-of-history) and[Tower BFT](https://solana.com/news/tower-bft--solana-s-high-performance-implementation-of-pbft) with a design that brings confirmation times down to about 150 milliseconds, close to a 100x improvement in finality; removes the onchain vote transactions that are the dominant recurring validator cost; introduces a 1.6 SOL Validator Admission Ticket per epoch; and uses a model that stays resilient even if up to 20% of stake is held by adversarial actors and another 20% is offline. Prerequisites landed across Agave 4.0 in May and 4.1 in June. Agave 4.2 includes the Alpenglow code for continued community testing with mainnet activation targeted for Agave 4.3 in October.


-


**SOL Economics:** SOL underperformance throughout the quarter drove more active discussions of tokenomics. Two specific proposals emerged.[SIMD-550](https://forum.solana.com/t/simd-0550-proposal-to-double-disinflation/4874) , from Helius, would double the annual disinflation rate to 30%, pulling terminal 1.5% inflation forward from 2032 to 2029 and cutting about 18.9 million SOL of future issuance, at the cost of a faster decline in nominal yield.[SIMD-553](https://temporal.xyz/writing/simd-0553-prices-everything) , from Temporal, would add a resource-based fee that is permanently burned and scales with usage, an estimated 7,500 to 9,000 SOL per day at current activity, roughly 10 times today's burn and equal to 12% to 15% of daily issuance. SIMD-550 would cut supply growth while SIMD-553 would link token value to network usage. Both proposals[advanced into formal review and early governance voting](https://x.com/mert/status/2084052274582815141?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E2084052274582815141%7Ctwgr%5E36bf56bb68cd1ae70476cc3e61fd3a0bc047c120%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fsolanafloor.com%2Fnews%2Fsimd-0550-0553-everything-you-need-know-about-weeks-sol-tokenomics-votes) on Aug. 3. Coupled with SIMD-123, they form the clearest agenda yet for routing network activity back to SOL holders rather than only to block producers.


-


**Multiple Concurrent Proposers and Quantum:** In late March, Anza[published Constellation](https://www.anza.xyz/blog/constellation) , a design for multiple concurrent proposers that would end a block producer's temporary monopoly over transaction ordering and curb MEV. The exact designs and implementation remain under debate and mainnet deployment timing is still unclear. Separately, on April 27, Anza and the Firedancer team each released post-[quantum](https://solana.com/news/quantum-readiness) migration plans, with Anza placing the odds of a cryptographically relevant quantum computer within five years at 3% to 5%.


## Network Activity


Solana DEX volumes declined by 45% in the second quarter, the second consecutive quarterly decline and their lowest aggregate levels since Q3 2024. DEX volumes have failed to regain January highs on a monthly basis throughout 2026, declining consistently until June when they saw a 20% increase on the back of an uptick in memecoin trading volume (due to the launch of the $ANSEM token) and listings of tokenized assets like the SPCX. On a quarter-to-quarter basis, SOL-stablecoin and stablecoin-to-stablecoin swaps continue to dominate most volumes while memes[account for](https://blockworks.com/analytics/solana/solana-spot-dexs) 15%-20% of trading volumes and tokenized stocks/foreign tokens ([wrapped](https://www.coinbase.com/learn/your-crypto/what-is-wrapped-crypto) ones native to other chains) hover around 10%.


Critics will point to these numbers as evidence that Solana remains overly dependent on memes. The reality is that Q2 demonstrated a continuing trend of teams launching projects and infrastructure needed for the non-meme onchain economy to grow. In May, Securitize, Jump Trading, and Solana “super-app” Jupiter launched what they billed as[the first fully onchain regulated equity stack](https://www.prnewswire.com/news-releases/securitize-jump-trading-group-and-jupiter-launch-fully-onchain-regulated-trading-for-tokenized-equities-302762248.html) . It paired Securitize's broker-dealer and alternative trading system (ATS) with Jump's proprietary automated market maker ([PropAMM](https://solana.com/news/understanding-proprietary-amms) ) liquidity and Jupiter's distribution, with issuance and settlement clearing atomically in a single block.[Backpack Securities and Sunrise](https://www.prnewswire.com/news-releases/backpack-launches-securities-platform-bridging-traditional-and-digital-asset-markets-302788563.html) brought broker-dealer ownership and tokenization live in June, held as security entitlements under Article 8 of New York's Uniform Commercial Code (UCC), the same legal structure behind a Schwab or Fidelity account, with eligibility for cash dividends and corporate actions. Jito announced[JTX](https://solanacompass.com/news/jtx-begins-rolling-out-to-first-users-as-jitos-self-custody-trading-platform-enters-early-access) , a self-custodial venue with professional order types and real-world asset (RWA) support and opened early access in late June ahead of a July launch. Galaxy's own[GLXY](https://www.prnewswire.com/news-releases/galaxy-and-superstate-launch-glxy-tokenized-public-shares-on-solana-302544834.html) , the first Nasdaq-listed equity tokenized on a public network,[went live as collateral on Kamino, a DeFi protocol,](https://www.thestreet.com/crypto/markets/solana-attracts-tokenized-equities-as-galaxy-enables-defi-lending) through transfer agent Superstate in April, turning a tokenized stock into a working DeFi asset rather than a static one. The infrastructure for tokenized stocks and foreign assets to scale past memes now exists on Solana,[which recorded as high as 95%+](https://x.com/toma_adv/status/2079246393177182398) of total tokenized equity trading throughout the quarter.


Solana extended its ranking as the No. 1 chain by DEX volumes for the seventh straight quarter, although market share dropped six points to 30%, its lowest since Q3 2025. The drop in market share was largely in-line with a broader decline in volumes across the market, with the miscellaneous “others” category picking up the most slack due to a smaller base and less variable activity.


> ***Solana’s edge will increasingly be its ability to host quality assets people want to trade rather than its status as the cheapest and most performant chain to trade on.***


Overall, DEX volumes and market share show little change quarter-over-quarter as onchain activity across crypto remains sluggish. The major focus looking forward is a continuing shift in the composition of onchain activity coupled with Solana’s ability to maintain its status as the leading chain by volumes. As onchain access points, such as[FOMO’s](https://www.galaxy.com/insights/research/social-trading-fomo) , abstract away the complexities, Solana’s edge will increasingly be its ability to house quality assets that people want to trade rather than just being the cheapest and most performant chain to trade on (see the[recent launch of Robinhood Chain](https://www.galaxy.com/insights/research/robinhood-chain-launch-analysis-base-comparison-memecoins-distribution-thesis) as an example of this dynamic).


## Network Fees


Solana's network fees fell sharply in Q2 2026, dropping ~44% quarter-over-quarter. Base fees and priority fees/tips continue to decline quarterly. This continues the multi-quarter unwind from Solana's Q4 2024–Q1 2025 peak, when quarterly network fees ran as high as $919m and MEV tips alone represented over 60% of the total. Fees are now running at roughly 6% of that peak level, reflecting the broader cooldown in memecoin-driven priority-fee auctions and MEV extraction that has defined Solana's fee cycle since early 2025.


The decline reflects cyclical and structural factors. Cyclically, the drop was driven by the ongoing cooldown in memecoin-driven activity, which has reduced congestion and the intensity of priority fee auctions and MEV since early 2025. Structurally, the shifting composition of onchain activity is also playing a role. Tokenized equities and other real-world assets, which contributed to the June volume rebound, tend to generate lower levels of bot-driven congestion and aggressive priority fee bidding compared to pure memecoin trading. Combined with gradual improvements in network efficiency, this suggests Solana’s fee generation per unit of economic activity may structurally be lower than during the prior peak.


Solana's share of network fees fell from 26.6% in Q1 2026 to 17.3% in Q2 2026, reversing the prior quarter's gain and landing below its Q4 2025 level of 18.9%. The pattern mirrors what we've[flagged in prior quarters](https://www.galaxy.com/insights/research/solana-q1-2026-dex-rwa-stablecoins-market-share) . Solana's network-fee base is disproportionately exposed to speculative trading cycles. As the broader crypto fee environment weakened across Q2 2026, Solana's fee decline outpaced the group, pulling its share down even as its DEX volume leadership (discussed above) held. However, network fees across the crypto landscape dropped, underscoring that the fee compression was industry-wide, not Solana-specific, even though Solana bore a disproportionate share of the decline.


## Application Fees


Solana application fees fell for the third straight quarter, dropping 31% QoQ to $552m. The current level is roughly a quarter of the Q1 2025 peak, when fees grew ~40% QoQ on the back of the January 2025 speculative surge (most notably the Trump memecoin launch).


Fees remain concentrated around meme-generated activity, with memecoin launchpad Pump alone generating $212m in Q2 2026, or roughly 38% of total ex-MEV/staking app fees network-wide. The leading products continue to be dominated by launchpad/DEX infrastructure and trading-terminal/aggregator apps. However, some green shoots pointing toward a more diversified fee-generating base are beginning to emerge. Collector Crypt (a tokenized collectibles also highlighted last quarter) remains the standout exception. Its fees nearly tripled QoQ and it was the fifth ranked app for the quarter, demonstrating the viability for businesses on Solana.


Solana has now led all chains in application fee share for 10 consecutive quarters, a streak stretching back to Q1 2024, though that share has compressed materially from a high of ~48% in Q1 2025 to 23% most recently. The compression is the mirror image of the network-fee story, as Solana’s application layer remains disproportionately tied to the speculative trading cycles that drove the 2024–2025 peaks. At the same time, the early non-meme green shoots—Collector Crypt’s near-tripling, the emerging tokenized-equity and RWA infrastructure, and regulated onchain equity stacks—show that a broader fee base is beginning to form. Whether that diversification can offset the ongoing meme unwind will determine if the Solana’s leadership streak survives the next few quarters.


## Perpetuals


On the surface, Solana perpetuals volume surged in Q2 2026, more than doubling to $111b. The headline is misleading. Effectively all the increase came from a single venue, GMTrade, which contributed roughly $90b while its open interest stayed close to flat, a pattern[consistent with](https://x.com/tomwanhh/status/2061775098260914323) points and airdrop farming on FX-pair perps rather than real demand. Excluding GMTrade from both quarters, volume fell 43%, continuing the decline that began after Q3 2025. Much of that drop traces to Drift, previously Solana's second-largest perpetuals venue, which suspended trading after a[$285m exploit on April 1](https://www.elliptic.co/blog/drift-protocol-exploited-for-286-million-in-suspected-dprk-linked-attack) , the largest DeFi hack of the year, and has not meaningfully resumed since.


The quarter was not uniformly negative. Phoenix, Solana's newest perpetuals venue, grew volume 14x to nearly $700m and open interest 5x to above $5m, expanding its asset list and introducing "Flight Codes," its version of builder codes that let any app, terminal, or bot route order flow to Phoenix for a fee.


Excluding GMTrade, Solana holds roughly 1% of both perpetuals volume and open interest, and the chain continues to lose ground to purpose-built venues like Hyperliquid and Lighter. Hyperliquid's recent success has come largely through TradeXYZ, the dominant builder on its permissionless HIP-3 markets, which now leads in listing the non-crypto assets, tokenized equities, indices, and commodities, where onchain trading demand has shifted. HIP-3 open interest reached roughly $3.2b in June, several times the size of Solana's entire perpetuals market. Lighter[signed on](https://www.galaxy.com/insights/research/weekly-top-stories-07-10-26-robinhood-chain-memecoins-openai-gpt-5-6-kraken-mazars-arbitration) in early July as the in-wallet perpetuals provider for Robinhood. It’s increasingly clear that asset selection and institutional distribution are now the baseline for any competitive perpetuals venue.


Phoenix's growth, the pending launch of venues like Bulk, a push to match the asset coverage of leaders like Hyperliquid, and better front ends such as JTX and Imperial are all tailwinds. On their own, however, they are not enough to make Solana a top three onchain perpetuals venue. In the beginning of Q3, for example, Phoenix[launched an incentives campaign](https://x.com/PhoenixTrade/status/2081831859760083422) paying out $15,000 in daily rewards on its platform. While the early results of the launch have successfully led to record volume and open interest on the platform, daily reporting from Phoenix indicates it has fewer than 1,500 traders, a far cry from the 50,000+ reports by leaders like Hyperliquid.


Solana's edge may instead come from letting traders post the tokenized equities Solana increasingly hosts as margin against perpetual positions. Spot tokenized stock and the perps written against it, settling on one composable chain, is a wedge available only to an onchain venue, and the most credible path to real perpetuals growth on Solana.


## Prediction Markets


Prediction markets stayed a small but advancing category on Solana through Q2 2026. Building on Q1’s early experiments with DFlow (tokenized Kalshi markets) and Jupiter aggregation, activity remained modest relative to spot DEX or perpetuals volumes. However, the quarter delivered clearer product differentiation and infrastructure progress across retail, event-driven, and professional trading use cases.


In June,[Jupiter launched Forecast](https://www.cryptotimes.io/2026/06/05/jupiter-unveils-forecast-to-power-solana-prediction-markets/) , Solana's first fully native prediction market, starting with 15-minute crypto markets where market makers compete to tighten pricing and each market carries its own token for composability.[Pascal](https://www.pascal.trade/) , a noncustodial prediction exchange that entered private beta in June and is built for professional traders rather than retail punters, also launched. Pascal pairs an offchain matching engine with onchain settlement, keeps collateral in Solana smart contracts, and runs an over-the-counter (OTC) desk for privately negotiated block trades, competing on execution quality and settlement reliability rather than token incentives. Just past quarter-end,[World](https://beincrypto.com/world-solana-prediction-market-phantom/) , a non-custodial protocol that replaced Kalshi as Phantom's in-wallet prediction offering, launched publicly on July 1. (This World should not be confused with Sam Altman’s eyeball-scanning cryptocurrency project of the same name.)


Native volumes remain small, with the Jupiter/Dflow integration dominating but only amounting to volumes in the tens of millions of dollars. Solana's clearest edge in the category remains the chain’s composability. DFlow's design turns a regulated Kalshi position into an SPL token (Solana’s equivalent of Ethereum’s ERC-20) that can be traded, lent, or posted as collateral elsewhere in Solana DeFi, something neither Polymarket's own app nor a centralized exchange offers. As with perpetuals, Solana has entered the fastest-growing version of a market as a distribution and tokenization layer for incumbents, and the unresolved test is whether a Solana-native venue can hold meaningful volume of its own.


## TVL, RWAs, and Stablecoins


Solana TVL fell for a third consecutive quarter, declining roughly 14% to $12.5b in Q2 (partially attributable to the decline in SOL price). The network's share of total tracked crypto TVL held in a narrow band, ending Q2 at roughly 7%, essentially flat and consistent with the broader market drawdown rather than a Solana-specific rotation out of the chain. The decline was broad-based rather than concentrated in any protocol. At the protocol level, the liquid-staking cohort (Jito, Marinade, Binance Staked SOL, Sanctum) and the lending/aggregation cohort (Kamino, Jupiter, Raydium) remain the backbone of Solana TVL.


Stablecoin supply continued to expand with total supply growing ~1.9% quarter over quarter to $15.6b. USDC's share of Solana stablecoin supply has fallen sharply, from roughly 77% as recently as Q1 2025 to under 47% by Q2 2026, as USDT, USD1, USDG, and PYUSD all captured meaningful share. Solana's share of the global stablecoin market ticked up to about 5.0%, in line with Q1's 4.9%, while Tron and Ethereum remain the dominant venues globally.


Solana RWAs continued their expansion throughout the quarter, crossing $3b for the first time since Solana’s launch in June. RWAs now account for nearly a quarter of Solana’s total TVL and its composition has also flipped. In line with the above discussion on tokenized stock volumes, public equities grew to become the largest single bucket in Q2, overtaking private credit, while bonds held roughly flat quarter over quarter.


Most of that value, though, sits idle. By 21Shares' custody analysis, only about 9% of Solana's tokenized RWA is working in DeFi as trading liquidity or loan collateral, rising to roughly 16% once non-circulating issuer reserves are set aside. Issuer reserves and personal wallets together hold more than three quarters of the total.


> ***The step that will differentiate Solana in RWAs is letting them do more than trade*** — ***serving as collateral, backing loans, compounding yield across DeFi*** — ***the edge a general-purpose chain holds over a standalone trading venue.***


Among lending apps, Jupiter Lend and Kamino lead the narrower tokenized-stock market with[roughly 83% of tokenized-stock collateral](https://thedefiant.io/news/defi/tokenized-stock-lending-tvl-reaches-23m-as-dex-volume-climbs) . Equities are the assets Solana users actually put to work with household names like QQQx, NVDAx, SPCX, and COINx each having mid-teens to above 30% of their circulating supply deployed in DeFi. Kamino also hosts other tokenized strategies. For example, Apollo's tokenized private credit fund[powers looping strategies](https://www.coindesk.com/business/2025/05/20/apollos-tokenized-credit-fund-set-for-solana-defi-debut-as-rwa-trend-expands) that let holders borrow against the position and amplify yield, and[more than $17m of Superstate's assets, including Galaxy's GLXY, sit as borrowable collateral](https://solanacompass.com/news/superstates-tokenized-funds-and-stock-cross-17m-in-active-collateral-on-kamino) .


Continued growth in RWA adoption ranks alongside share gains in perpetuals and prediction markets as a primary focus for the Solana ecosystem in the second half of 2026. Building the issuance rails and landing early tokenized-equity volume is a start. The step that will differentiate Solana is making tokenized assets do more than trade, serving as collateral, backing loans, and compounding yield across DeFi, the edge a general-purpose chain holds over a standalone trading venue. Solana needs to win as the layer where these assets are not only traded but composed across the entire DeFi stack.


## Conclusion


Solana ended Q2 with a clearer path forward, but also a higher bar for success. Continued improvements to execution, consensus, and token economics can make the network faster, more resilient, and better aligned with SOL holders. Yet those upgrades alone will not determine the chain’s next phase. As blockspace becomes cheaper and increasingly abundant, Solana’s advantage will depend less on raw performance and more on assets, liquidity, and the applications that choose to build on top of it.


> ***As blockspace becomes cheaper and increasingly abundant, Solana’s advantage will depend less on raw performance and more on assets, liquidity, and the applications that choose to build on top of it.***


The opportunity is to become the settlement and coordination layer for a broader onchain financial system—one where tokenized equities, stablecoins, prediction markets, and other assets are not merely issued or traded, but reused across collateral, lending, margin, and yield. Q2 showed that the ecosystem is assembling the pieces required to compete for that role. The next test is whether those pieces begin reinforcing one another strongly enough to create durable demand and economic value.


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, hold a financial interest in Solana. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
