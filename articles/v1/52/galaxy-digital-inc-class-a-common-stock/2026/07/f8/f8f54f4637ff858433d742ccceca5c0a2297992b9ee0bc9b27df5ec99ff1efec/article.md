---
schema_version: "1.0.0"
document_id: "f8f54f4637ff858433d742ccceca5c0a2297992b9ee0bc9b27df5ec99ff1efec"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-3264694a8f1e"
canonical_url: "https://www.galaxy.com/insights/research/hyperliquid-tradexyz-oracle-liquidations"
published_at: null
first_seen_at: "2026-07-31T23:15:35.285855+00:00"
fetched_at: "2026-07-31T23:15:36.493107+00:00"
content_hash: "sha256:32b4245697416feb35e80b3afed1a3e3dd20c857b5a339a3d01ee492c9a1cc42"
---

# How One Share in Seoul Broke Hyperliquid’s Largest Equity Market

*This article originally appeared in Galaxy Research's weekly newsletter.*[Subscribe](https://www.galaxy.com/subscribe-to-research?utm_campaign=Weekly%20Research%20Brief%20-%20Email&utm_medium=email&utm_source=hs_email&_hsenc=p2ANqtz--YWOkviRZJsa_uYNPjl2N4ahqTpHeV86Rs9VQQeL9MWn1U09L_xNxjRhoRyGaaBrCPLXuX) *to get timely insights delivered to your inbox every Friday morning.*


**TradeXYZ, the largest deployer of HIP-3 perpetual futures markets on Hyperliquid, is**[reimbursing](https://x.com/tradexyz/status/2082260930751082821?s=20) **traders for liquidations resulting from an accurate but anomalous third-party price feed reading. Notably, the hiccup came from the traditional financial system, not crypto.**


At 8:00 a.m., Seoul time on July 28, a single share of chip manufacturer SK Hynix changed hands for 1,272,000 won (roughly $868) in the opening seconds of South Korea’s NextTrade pre-market session. That was 29.96% below the previous close of 1,816,000 won, right at the daily lower price limit. Buy orders arrived about two minutes later and the stock recovered to the 1.7 million won range. By then the print had already traveled 5,500 miles, onto an order book in a different asset class, and taken out roughly $60 million in leveraged long positions.


The market in question is xyz:SKHYNIX, a USDC-margined perpetual offering up to 10x leverage, deployed on Hyperliquid by TradeXYZ under the HIP-3 upgrade. HIP-3 has been live on mainnet since October 2025 and allows any team that stakes 500,000 HYPE (~$27 million at recent prices) to deploy its own perpetual futures markets on HyperCore, Hyperliquid’s onchain order book and matching layer. The deployer defines the market (oracle, leverage limits, margin mode, and contract spec). Hyperliquid supplies execution and settlement, splits the fees, and does not pick the price.


TradeXYZ, the perps arm of tokenization protocol Unit, was the first HIP-3 deployment and essentially has a monopoly on the category. It accounts for more than 90% of HIP-3 open interest and ~98% of builder-market volume. That matters at scale: HIP-3's share of total Hyperliquid perp volume has climbed from roughly 2% at the start of the year to around half today, with 30-day builder volume of ~$98 billion and network-wide HIP-3 open interest at ~$3.6 billion. SK Hynix is the top HIP-3 market in open interest, with $638 million as of July 30.


The contract does exactly what its documentation says: it tracks the U.S. dollar value of one SK Hynix common share. TradeXYZ splits Korean pricing into two regimes: an internal phase, where the oracle drifts off its own order book, and an external phase that begins the moment NXT opens at 8:00 a.m. Korea Standard Time. The anomalous print landed precisely at that handoff. Onchain records show the oracle component submitting an external price of $868.17 seconds after the switch.


TradeXYZ’s mark price is the median of three inputs: the oracle price, the oracle plus a 150-second exponential moving average of the book’s deviation from it, and the median of best bid, best ask, and last trade. That smoothing absorbed about 11 percentage points of a 30% corrupted input. It was not enough. The mark fell 18.7%, effectively instantaneously. Open interest in the contract dropped from $481 million to $331 million in a few minutes. Onchain analysts put liquidated notional amounts between $57 million and $80 million across roughly 960 long accounts; profitable shorts were auto deleveraged on the way through (similar to what happened during[10/10](https://www.galaxy.com/insights/research/cryptos-flash-crash-liquidation-binance-adl-auto-deleveraging) ), and a system backstop address absorbed 406 long positions before being liquidated itself.


TradeXYZ[published a response](https://x.com/tradexyz/status/2082260930751082821?s=20) on July 29, saying it would cover liquidation losses attributable to the anomaly, framed explicitly as a “one-time discretionary decision” rather than a standing policy, with eligibility criteria and distributions to follow. It also maintained that the oracle “worked as intended according to its specification” (which is true) and said it is accelerating work on price formation during tail events, including weighting its own order books, which it argues now carry meaningful depth relative to external sources.


SK Hynix was genuinely under pressure on AI-spending and Chinese memory competition fears. The stock closed the regular Seoul session down 14.65% at 1.55 million won. It reported Q2 results the next day and fell again, triggering circuit breakers on consecutive trading days for the first time in the history of the Korean market. A trader watching the tape that morning had every reason to believe the crash was real.


#### Our take


The oracle worked. The risk system didn’t. Traditional markets separated last trade, index price, and fair value for risk purposes decades ago, precisely so one local execution cannot decide the fate of a leveraged account. TradeXYZ had the architecture (a three-input median, EMA smoothing, discovery bounds) and it absorbed roughly 11 percentage points of a corrupted print before giving up. The protection was real. It was just calibrated for a reference market with more depth than the one it was actually pointed at. Correct price discovery is not the same thing as sound liquidation design, and a spec can be followed faithfully all the way into a bad outcome.


The failure mode itself is not a crypto problem. NexTrade is already patching it from the other side with a static volatility interruption in September. What onchain added was transmission speed and leverage on the receiving end. The print was real, the data providers were correct, and every module did its job. This is why “add more oracles” is the wrong fix. Every provider was reading the same NXT book and the median across them converges on the same $868.


The JELLYJELLY memecoin is a useful historical example on the accountability question. In March 2025, Hyperliquid’s own vault was underwater, validators froze the market and force-settled positions at a chosen price, and the protocol spent the next year answering for it. In the South Korea case, the core protocol didn’t touch anything. The deployer that chose the oracle absorbed the cost. What made traders whole is the generosity of the TradeXYZ team. Traders will be made whole because a company with a 90%-plus share of a category it spent nine months building decided its franchise was worth more than the check. TradeXYZ would have had a totally valid argument for paying nothing and didn’t make it.


It will be interesting to watch what happens next. TradeXYZ says it will weight its own order books more heavily against external feeds. Onchain venues mature by referencing the real-world market at the outset and then progressively becoming the reference. Equity perps are further along that curve than most people assume, and this incident shows it. The external (TradFi) feed was the fragile input, and the internal (onchain) book was the stable one.


Hyperliquid’s shared infrastructure is what makes this transition survivable. No builder here has to write a matching engine, a margin system, or a liquidation engine. TradeXYZ had exactly one hard problem to solve, and it got a $60 million tuition bill on the one thing it owned. Against a category doing $98 billion a month, that’s cheap.


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, may hold financial interests in certain assets referenced in this content. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
