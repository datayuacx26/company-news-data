---
schema_version: "1.0.0"
document_id: "2764a2da9deec23e387264bcb9633907aecf6256c3fb1ecd27bae3b1d3f4e310"
company_key: "yc-onechronos"
company: "OneChronos"
source_id: "yc-onechronos-news-import-ed8558a9e34d"
canonical_url: "https://www.onechronos.com/insights/periodic-auctions/"
published_at: null
first_seen_at: "2026-07-25T17:48:26.237044+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:fd4bd5a008ce9a76dfe2d3ff9fd9d391e2b422af2b98cb941ef59d0c8e63ac74"
---

# Periodic Auctions - Unique Liquidity, Unique Execution

## Executive Summary


Much of U.S. equities trading takes place in continuous price-time priority markets, where reaction time of market participants influences execution outcomes. This may create arbitrage opportunities that result in adverse selection for providers of natural liquidity. OneChronos offers a “Smart Market” alternative that bridges academic research and emerging technology designed to take speed out of the equation. The goal is to deliver execution outcomes optimized to foster competition on price and size, resulting in unique institutional liquidity. In this analysis, we examined a recent sample of these outcomes in S&P 500 constituents. Our findings highlight two primary advantages at OneChronos:


1. OneChronos’s time-randomized periodic auctions create trading opportunities that are unique, since they may remain fragmented by time at other trading venues.
2. OneChronos executions have stronger price stability, as measured by mid-to-mid markouts, compared to the rest of off-exchange trading even in S&P 500 constituents which have the highest notional turnover.


[Subscribe](https://onechronos.us9.list-manage.com/subscribe?u=943aece135770ab3b5f9a4218&id=dd051932c5)


### When Milliseconds Cost Millions


In continuous “price-time” priority markets, participants who react quickly to price changes may be rewarded over participants providing more stable liquidity. This may create arbitrage opportunities that are only visible at millisecond horizons.


This phenomenon is shown in the figure below, which depicts the price paths of the two largest financial instruments that track the S&P 500 index, the SPDR S&P 500 exchange traded fund (ticker: SPY) and the S&P 500 E-mini futures contract (ticker: ES), on a sample trading day in 20111 .


Price swings at millisecond frequencies can expose trades to adverse selection risk, even without new information or participants entering the market. This short-term adverse selection may compound over time, potentially creating significant variance in portfolio outcomes for institutional traders pursuing longer-term alpha strategies.


### Batching Out The Noise


OneChronos runs time-randomized periodic auctions approximately 10-20 times per second, independent of order interest and without time-priority in matching. These periodic trade matching events are designed to reduce the likelihood that participants encounter adverse selection pressures created by mechanical arbitrage opportunities. The objective is to enable institutional traders to execute efficiently over longer horizons.


The figure2 below shows how periodic auctions can create price stability across millisecond horizons. In continuous price-time markets, bid, ask, and midpoint prices fluctuate nearly continuously. Periodic auctions instead aggregate trading interest over discrete time intervals and clear at a single price that is more robust to these temporary fluctuations. These auctions decouple execution from the noisy continuous market, reducing adverse selection risk for institutional traders. In this example of a 500ms time window, the auction execution prices averaged $99.9930 - nearly the equivalent of the time-weighted average of the midpoint prices $99.9952.


### Creating Unique Liquidity


Arguably, S&P 500 constituents are the best test for markets designed for institutional traders. These securities are commonly considered the “backbone” of institutional risk transfer in index replication and portfolio rebalancing. Within this segment, OneChronos has established a meaningful and growing presence. As of recently, OneChronos represents more than 1.6% of all off-exchange notional value in S&P 500 securities3 .


One differentiating factor of a trading venue is the uniqueness of its liquidity: whether the venue offers liquidity opportunities that would not have existed elsewhere at the same moment in time. We quantified the uniqueness of OneChronos liquidity by examining how often OneChronos trades occur in close time proximity to other off-exchange trades.


The chart below shows, on a security-by-security basis, the percentage of OneChronos executions in S&P 500 constituents that are not accompanied by a trade at any other off-exchange venue within 1-second. This is charted against the median number of off-exchange trades over all 1-second windows with any off-exchange trade. **OneChronos trades are at least ~50% unique4 by this definition, including in securities of the highest trade turnover.**


OneChronos auctions are designed to bring together liquidity that would otherwise be fragmented by time. S&P 500 constituents are amongst the most actively traded names market-wide, yet the chart above shows that OneChronos can create unique trading opportunities via batch aggregation. For institutional traders evaluating routing destinations, this means OneChronos may offer access to liquidity that isn’t trading simultaneously on other venues off-exchange.


### Delivering Execution Quality


OneChronos is also designed to deliver superior execution quality and in turn limit adverse selection risk. One method to assess execution quality is “markouts” – the price evolution before and after the trade, measured relative to an objective benchmark such as the midpoint of the NBBO. Markouts can provide a proxy for adverse selection and information leakage, where significant pre- or post-trade price movement indicates executions may be correlated with short-horizon price dynamics rather than fair price discovery. In S&P 500 names, where spreads are tight and information propagates quickly, even small differences in short-horizon markouts may translate into meaningful implicit costs at institutional scale.


We measure the markout of a trade as the absolute value of the difference in midpoint quote at horizons relative to the prevailing midpoint quote at time of trade, normalized by the trade price and expressed in basis points (1/100th of a percent). Absolute value markouts measure execution quality without requiring trade side assumptions, whereas signed markouts depend on whether a trade was a buy or sell and necessarily sum to zero across all participants. We weight each trade by its notional value transacted. Large values would indicate less stable executions, since the price would have moved more, either up or down.


The below chart of notionally weighted markouts5 shows **OneChronos executions in S&P 500 constituents experience 25% less price movement compared to all other off-exchange executions** , even out to +5 seconds post-trade. The shading around each line represents the standard error of the measurement at each horizon. Non-overlapping standard errors indicate statistically rigorous differences.


### Better Where It Matters Most


In securities where institutional traders move significant notional value, execution quality hinges on price stability and consistency. As trading conditions become noisier and price movements more pronounced, execution outcomes grow increasingly exposed to market impact.


To illustrate how OneChronos is designed to maintain execution quality even in volatile conditions, we examine +1-second markouts along two dimensions. First, we plotted markouts against the average spread at trade of each S&P 500 constituent, a simple proxy for short-term price variability. Even as spreads widen and conditions become noisier, OneChronos executions remain consistently more stable than the off-exchange average. We also plotted markouts against the average trade price of each S&P 500 constituent. Higher-priced securities embed greater notional value per share, magnifying the economic consequence of even small basis-point deviations. Across the upper end of the S&P 500 price distribution, OneChronos continues to exhibit lower markouts relative to the off-exchange benchmark, reinforcing the stability of its execution outcomes in precisely those names where precision matters most.


We argue these results6 indicate OneChronos performs well where execution quality risk tends to emerge, and where that risk is most expensive for institutional traders.


Examining individual securities avoids aggregation effects driven by episodic market conditions or concentration in a handful of names.


Security-level +1-second markouts7 for a select group of highly liquid S&P 500 constituents show the same pattern. Arguably, these are the hardest securities to demonstrate an execution advantage in: they have the tightest spreads, the fastest price discovery, and the most intense competition amongst speed-sensitive participants. Yet even at this level, OneChronos delivers lower markouts on a security-by-security basis. The auction’s time randomization is designed to neutralize latency advantages, which we contend are a primary source of microstructural noise:


### The Bottom Line


Taken together, we contend these results suggest


1. OneChronos has unique liquidity in securities of high institutional focus.
2. OneChronos has empirically strong performance in securities of high institutional focus.


**Discuss with your broker how OneChronos can help you source unique institutional liquidity with stable execution outcomes. Contact[\[email protected\]](https://www.onechronos.com/cdn-cgi/l/email-protection#4e3b3d112f3a3d113d2f222b3d0e21202b2d263c2120213d602d2123) for more information on the OneChronos US Equities ATS offering.**


---


## Methodology


- Analysis covers all S&P 500 constituents as of December 2025 using consolidated tape data for off-exchange trades from 01/12/2026 to 01/28/2026. OneChronos trades are compared against the aggregate of all other off-exchange executions.
- Markouts are measured as the absolute value of midpoint quote differential between execution time and specified horizons, up to 5 seconds before or after trade time and at 500 millisecond increments, normalized by trade price and expressed in basis points. Markouts plotted in aggregate of a dimension (security, average trade price, average trade spread, etc.) are notionally weighted by the notional value transacted of each dimension.
- A OneChronos trade is considered unique if no off-exchange trade in the same security occurs at any other dark venue within 1 second of the OneChronos execution timestamp.
- Standard errors represent the notionally-weighted standard deviation of individual trade markouts divided by the square root of trade count at the security-date-lag level, then aggregated across securities and dates, notionally weighted (or at the fixed security level, for standard error measurements plotted per security). Shaded bands or lines illustrating statistical significance on all charts represent 1 standard error.


---


## Disclaimer


OneChronos Markets, LLC, (“OneChronos”) is registered with the U.S. Securities & Exchange Commission as a broker-dealer, and is an NMS Stock ATS with an effective Form ATS-N on file with the SEC.


These materials have been prepared by OneChronos based on information, assumptions and data that it considers reliable at the time it was prepared. OneChronos does not represent, directly or indirectly, that these materials are accurate, current or complete, and they should not be relied on as such. The information and forward looking statements contained in these materials are subject to inherent risks, uncertainties and changes that could cause actual results to differ materially from what is contained herein. Past performance does not guarantee future results. OneChronos does not undertake any obligation to update or revise these materials even if changes are material.


These materials are not, and under no circumstances should they be construed as, an offer or recommendation, or the solicitation of an offer, to buy or sell any security, or any investment strategy involving securities. Nor do they take into account the particular investment objectives, financial situation or needs of any subscriber or user of the OneChronos ATS or any other investor. Subscribers to OneChronos must be U.S. registered broker-dealers. These materials are not, and under no circumstances should be construed as, a solicitation by OneChronos to act as a securities broker or dealer in any jurisdiction in which it is not legally permitted to carry on the business of a securities broker or dealer. These materials are for informational purposes only, and do not offer or constitute legal, tax, accounting or regulatory advice. To the fullest extent permitted by law, OneChronos does not accept any liability whatsoever for any direct or consequential loss arising from any use of these materials. OneChronos preserves all rights, including copyrights, trademarks, service marks and patents, in connection with these materials and their contents.


OneChronos is a wholly owned subsidiary of OCX Group, Inc. OneChronos an independent, venture-backed company using cutting edge technology to connect the next generation of electronic trading. Contact us at[\[email protected\]](https://www.onechronos.com/cdn-cgi/l/email-protection#5b32353d341b34353e3833293435342875383436) .


Member FINRA/SIPC. Check the background of OneChronos Markets, LLC at[https://brokercheck.finra.org](https://brokercheck.finra.org/)


---


## Footnotes


1.


Eric Budish, Peter Cramton, John Shim, The High-Frequency Trading Arms Race: Frequent Batch Auctions as a Market Design Response, *The Quarterly Journal of Economics* , Volume 130, Issue 4, November 2015, Pages 1547–1621,[https://doi.org/10.1093/qje/qjv027](https://doi.org/10.1093/qje/qjv027)↩︎


2.


Stylistic example for illustrative purposes only. This figure does not reflect realized trading data or outcomes.↩︎


3.


These market share figures represent OneChronos’ notional share of all off-exchange (TRF-reported) executions. Date range: 01/12/2026 to 01/28/2026. Sources: OneChronos, SIP (UTP/CTA).↩︎


4.


% of OneChronos trades where no off exchange trade is observed within (pre or post) +1-second of trade time, plotted against the median number of off-exchange trades over all +1-second windows with any trade. Measured per security for all securities in the S&P 500 as of December 2025. Date range: 01/12/2026 to 01/28/2026. Sources: OneChronos, SIP (UTP/CTA).↩︎


5.


Markouts are the notionally weighted absolute value in midpoint quote differential at each lag (seconds, here +- 5 seconds around trade time) relative to prevailing midpoint quote at time of trade. Markouts aggregated over multiple securities are notionally weighted by security notional value transacted - the notional weighting per security is determined by the ‘Rest of Off Exchange’ sample and used for both ‘CGXS’ and ‘Rest of Off Exchange’ series. Standard errors represent the notionally-weighted standard deviation of individual trade markouts divided by the square root of trade count at the security-date-lag level, then aggregated across securities and dates, notionally weighted. Outliers of >= 200 bps of markout are excluded from the sample.↩︎


6.


Markouts with standard errors are measured as described earlier in this document, and at +1-second. The dimensions of average trade price and average trade spread are grouped into 4 categories, listed on the x axes of each dimension’s respective facet. Outliers of >= 200 bps of markout are excluded from the sample. Date range: 01/12/2026 to 01/28/2026. Sources: OneChronos, SIP (UTP/CTA).↩︎


7.


Markouts are measured as described earlier in this document, and at +1-second. The securities plotted represent a selected subset of the most traded S&P 500 constituents by notional value transacted in the stated date range. Outliers of >= 200 bps of markout are excluded from the sample. Date range: 01/12/2026 to 01/28/2026. Sources: OneChronos, SIP (UTP/CTA).↩︎


## Related Articles


[US Equities May 2026 Pairs Without Legging Risk: Leg 1 In this first "leg" of a two-part series, we show how pairs trading can break down due to timing gaps between legs, introducing unintended transaction costs and risk. OneChronos, built on a design philosophy of coordinated and intentional execution, enables atomic multi-security trades that eliminate legging risk at its source. Read article](https://www.onechronos.com/insights/pairs-part-one/)[US Equities July 2026 Pairs Without Legging Risk: Leg 2 In this second "leg" of a two-part series, we examine the economics of mitigating legging risk. A pairs trader may go off the lit markets in search of unique liquidity, or unique spreads. Here we show legging risk can erode her ability to execute at those spreads: reaching a unique spread means holding both legs off the market at once, which is no longer in play upon getting legged. OneChronos executes the pair as a single trade, so execution serves the strategy rather than eroding it, and pairs traders may harvest more unique liquidity with reduced execution uncertainty. Read article](https://www.onechronos.com/insights/pairs-part-two/)
