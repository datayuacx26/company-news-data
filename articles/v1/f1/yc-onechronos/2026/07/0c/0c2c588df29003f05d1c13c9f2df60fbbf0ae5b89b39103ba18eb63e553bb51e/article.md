---
schema_version: "1.0.0"
document_id: "0c2c588df29003f05d1c13c9f2df60fbbf0ae5b89b39103ba18eb63e553bb51e"
company_key: "yc-onechronos"
company: "OneChronos"
source_id: "yc-onechronos-news-import-ed8558a9e34d"
canonical_url: "https://www.onechronos.com/insights/pairs-part-one/"
published_at: null
first_seen_at: "2026-07-25T17:48:26.237044+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:eab6f6adf0fdc529e457e910c95527d755182acc2a19e4c4ab44aa5ed9eecb46"
---

# Pairs Without Legging Risk: Leg 1

**OneChronos Markets**


*Data reflects trading activity from 04/13/2026 - 04/24/2026*


Pairs trading is an enduring strategy in equities markets. In its idealized form, a trader treats the spread between two securities as a single instrument. Today, pairs trading reigns in relative value strategies, which are a core allocation within multi-manager hedge funds designed to capture idiosyncratic alpha while minimizing market impact. In practice, however, paired execution introduces friction, and in this “two-leg” series we articulate why this friction is a problem.


If the legs of a pair do not execute simultaneously, the trader is exposed to legging risk. Legging risk arises when one side of the trade fills before the other, leaving the trader temporarily exposed to outright price movements rather than the intended spread. This is distinct from convergence risk (the risk that the spread itself fails to revert), which is the source of expected return in pairs trading. Convergence risk is generally taken deliberately. Legging risk is not.


OneChronos Atomic Pairs addresses this by enforcing simultaneous execution of both legs, or no execution at all, designed to preserve the integrity of the spread at the point of execution. Even in fragmented and noisy markets, atomic trading can ensure that the realized entry reflects the intended relative value, rather than the path-dependent sequence of individual fills.


Using a recent sample of TRF-reported executions sourced from the SIP, we quantify the economic cost of legging risk. Our results indicate that this cost is material across a range of common pairs, suggesting that execution mechanism, and not just signal quality, plays a meaningful role in realized performance.


We analyze pairs representative of common use cases: share classes (GOOG/GOOGL, BRK A/BRK B), highly correlated names in the same sector (KO/PEP, MA/V, CVX/XOM), and merger arbitrage (KMB/KVUE, CHTR/LBRDA, NSC/UNP).


## A Smart Market Designed To Eliminate Legging Risk


OneChronos operates periodic auctions, each of which runs across all securities simultaneously. The matching engine can optimize across multiple securities and enforce contingencies that are satisfied together. The engine is designed to eliminate legging risk and the associated execution costs from multi-security trading.


The liquidity opportunity for multi-security trading at OneChronos has grown considerably over time. The chart below shows the percentage of notional value cleared in auction events that clear two or more unique securities. Multi-symbol notional value has reached new highs across every threshold of symbol count. This suggests the largest and most execution-sensitive orders are leading the adoption:


Legging risk is pervasive across equity markets. The opportunity to execute pairs without legging risk at OneChronos has expanded meaningfully, reflecting both increased adoption and the breadth of the underlying problem. For widely traded pairs, traders at OneChronos can source simultaneous liquidity in both legs, often at sizes comparable to or exceeding average trade sizes observed in the market. This is demonstrated in the below figures, which describe how often pairs liquidity is present in OneChronos auctions at any size and at least as large as the tape’s average notional trade size (see the “Methodology” section for additional reference). In several pairs, simultaneous liquidity is available in more than 95% of auctions:


## Synchronous Execution is Rare


How frequently do pairs trade “together” when looking at trades across the US Equities markets? We measure how often the two legs of each pair trade synchronously, defined as both legs executing within one millisecond of each other. For each fill in one leg, we find the nearest (by timestamp) fill in the other leg, then compute the notional-weighted share where the gap fell within one millisecond. We call this the synchrony rate, or the percentage of notional synchronized within one millisecond.


The table below shows that synchrony rates never exceed 10% for any pair in our sample. The remaining 90% of notional trades with an incomplete hedge for some duration during which the lagging leg’s price may drift unfavorably for the trader, or “slip.” The right column quantifies this “slippage” as the average absolute deviation of the lagging leg’s fill price from its NBBO midpoint at the time the leading leg traded. This ranges from 1.5 to 14 basis points depending on the pair. When atomic execution is enforced using OneChronos Pairs, this slippage is eliminated.


The above synchrony rates suggest pairs traders get legged frequently, and when they do, the economic cost can be material. But slippage on the lagging leg is only one dimension of the problem. The trader also carries the spread position itself through the delay, and this exposure adds noise to her P&L even after both legs eventually fill. The chart below illustrates this dynamic for a stylized pair. Leg A fills first, and the trader is immediately exposed: Leg B’s NBBO drifts adversely during the delay, widening the gap between the intended entry and the actual entry:


## Asynchronous Execution May Put P&L at Risk


To measure the P&L noise induced by asynchronous execution, we compute spread returns (the change in` log(mid_A / mid_B)` ) at multiple horizons after both legs have filled, split by whether the execution was synchronous or asynchronous. The chart below plots the notional-weighted volatility of these spread returns at each horizon. The gap between the two curves is the excess noise injected by asynchronous execution. It is visible at every horizon and grows at longer holding periods, or the time the trader holds the spread position after both legs are filled:


This means that after both legs fill, the trader who entered asynchronously can carry noisier P&L than the trader who entered atomically. One response is to try to close the gap by forcing the lagging leg to execute immediately, for example by crossing the spread rather than waiting for the lagging leg to execute. But how should a trader determine when to do one or the other?


## Wait for the Other Shoe to Drop


In this first “leg”, we measured the impact of legging risk in commonly traded pairs, both in terms of return degradation and volatility.


In leg two, we turn to the economics of mitigation. What does it cost to reduce or avoid legging risk, and how effective are conventional approaches? We show that reactive tools, which manage fills after the fact, are inherently limited. From a portfolio perspective, eliminating legging risk at the point of execution through atomic trading at OneChronos is fundamentally different from managing it after it has already been taken on.


## Methodology


Analysis covers consolidated tape data for off-exchange trades (exchange code “D”) from 04/13/2026 to 04/24/2026 for eight representative pairs: GOOG/GOOGL, KO/PEP, MA/V, NSC/UNP, KMB/KVUE, CVX/XOM, BRK A/BRK B, and CHTR/LBRDA. NBBO quotes are sourced from the consolidated quotation feed over the same period, filtered for non-crossed markets. The multi-security trading growth chart covers OneChronos auction activity from inception (H2 2022) through 04/30/2026.


For each pair, we measure the frequency with which liquidity is simultaneously available in both legs across OneChronos auctions over the specified date range. “Any size” reflects the share of auctions in which executable interest was present in both legs at any quantity. The “average tape size” threshold reflects the share of auctions in which executable interest was present in both legs at a size at least equal to the average notional trade size for that symbol on the consolidated tape over the same period.


Synchrony is measured by matching each fill in leg A to the nearest fill in leg B by timestamp (and vice versa), then computing the notional-weighted share of fills where the absolute time gap falls within a one millisecond threshold. The synchrony rate for each pair is the notional-weighted average of both directions.


An asynchronous trade event is defined as a lead-leg fill whose nearest opposite-leg fill exceeds the one millisecond synchrony threshold and where the lead fill precedes the lag fill in time. For each event, the lag fill price and the lagging leg’s NBBO midpoint are measured as-of the lead fill timestamp against the consolidated quote feed. Slippage is computed as` |lag fill price − lag NBBO midpoint| / lag NBBO midpoint × 10,000` , expressed in basis points.


Spread returns are measured as the change in` log(mid_A / mid_B)` from entry to entry + horizon, expressed in basis points. For synchronous events, entry is the fill timestamp. For asynchronous events, entry is the lag fill timestamp (the point at which both legs are hedged). Horizons are 1s, 5s, 10s, 30s, 1min, and 5min. Spread return volatility at each horizon is the notional-weighted standard deviation of spread returns, pooled across all pairs and computed separately for synchronous and asynchronous executions using the weighting formula below.


All aggregate statistics (means, standard deviations, percentiles) are notionally-weighted: each event is weighted by` shares × fill price` for the lead leg. Notional-weighted standard errors are computed as` √(Σ w_i (x_i − μ)² / ESS)` , where` w_i = notional_i / Σnotional` and` ESS = 1 / Σw_i²` is the effective sample size. Where shown, error bars and confidence bands represent ±1 standard error.


Outlier filters are applied before all computations: slippage ≤ 500 bps, gap duration ≤ 10 minutes, NBBO midpoint ≥ $0.01, and notional > 0. Bins with fewer than 20 observations are excluded from binned charts.


## Disclaimer


OneChronos Markets, LLC, (“OneChronos”) is registered with the U.S. Securities & Exchange Commission as a broker-dealer, and is an NMS Stock ATS with an effective Form ATS-N on file with the SEC.


These materials have been prepared by OneChronos based on information, assumptions and data that it considers reliable at the time it was prepared. OneChronos does not represent, directly or indirectly, that these materials are accurate, current or complete, and they should not be relied on as such. The information and forward looking statements contained in these materials are subject to inherent risks, uncertainties and changes that could cause actual results to differ materially from what is contained herein. Past performance does not guarantee future results. OneChronos does not undertake any obligation to update or revise these materials even if changes are material.


These materials are not, and under no circumstances should they be construed as, an offer or recommendation, or the solicitation of an offer, to buy or sell any security, or any investment strategy involving securities. Nor do they take into account the particular investment objectives, financial situation or needs of any subscriber or user of the OneChronos ATS or any other investor. These materials are not, and under no circumstances should be construed as, a solicitation by OneChronos to act as a securities broker or dealer in any jurisdiction in which it is not legally permitted to carry on the business of a securities broker or dealer. These materials are for informational purposes only, and do not offer or constitute legal, tax, accounting or regulatory advice. To the fullest extent permitted by law, OneChronos does not accept any liability whatsoever for any direct or consequential loss arising from any use of these materials.


OneChronos preserves all rights, including copyrights, trademarks, service marks and patents, in connection with these materials and their contents.


These materials have been prepared solely for existing and prospective subscribers of the OneChronos ATS. Neither these materials nor any of its contents may be reproduced or copied by any means without the prior written consent of OneChronos. FOR INSTITUTIONAL & PROFESSIONAL CLIENTS ONLY – NOT INTENDED FOR RETAIL CUSTOMER USE.


OneChronos is a wholly owned subsidiary of OCX Group, Inc. OneChronos an independent, venture-backed company using cutting edge technology to connect the next generation of electronic trading. Contact us at[\[email protected\]](https://www.onechronos.com/cdn-cgi/l/email-protection#a0c9cec6cfe0cfcec5c3c8d2cfcecfd38ec3cfcd) .


Member FINRA/SIPC.


## Related Articles


[US Equities July 2026 Pairs Without Legging Risk: Leg 2 In this second "leg" of a two-part series, we examine the economics of mitigating legging risk. A pairs trader may go off the lit markets in search of unique liquidity, or unique spreads. Here we show legging risk can erode her ability to execute at those spreads: reaching a unique spread means holding both legs off the market at once, which is no longer in play upon getting legged. OneChronos executes the pair as a single trade, so execution serves the strategy rather than eroding it, and pairs traders may harvest more unique liquidity with reduced execution uncertainty. Read article](https://www.onechronos.com/insights/pairs-part-two/)[US Equities February 2026 Periodic Auctions - Unique Liquidity, Unique Execution Across all equity securities—and highlighted here in S&P 500 constituents—OneChronos takes speed out of the trading equation, delivering unique institutional liquidity and more stable execution at scale. Read article](https://www.onechronos.com/insights/periodic-auctions/)
