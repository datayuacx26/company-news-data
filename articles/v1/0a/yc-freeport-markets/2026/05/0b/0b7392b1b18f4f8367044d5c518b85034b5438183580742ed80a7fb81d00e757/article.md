---
schema_version: "1.0.0"
document_id: "0b7392b1b18f4f8367044d5c518b85034b5438183580742ed80a7fb81d00e757"
company_key: "yc-freeport-markets"
company: "Freeport Markets"
source_id: "yc-freeport-markets-news-import-46c2f164323d"
canonical_url: "https://www.freeportmarkets.com/research-ai-beat-sp.html"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-25T05:46:52.249644+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:f0fcd97c7e0247d5c94b44dacb7cff92f42a990f35067249db653fac5d71661d"
---

# How Freeport's AI Beat the S&P by 46% in 16 Weeks

Users and investors kept asking the same question: do you actually know if your trading recommendations are working? Now that Freeport has crossed $50M in notional trading volume, we owed our users a real answer, not just our own conviction. So we ran a comprehensive backtest on every recommendation our notifications have pushed. The answer was better than we expected.


## Overview of results


Between Jan 20 and May 14, 2026, the Freeport ingestion pipeline took in 68,004 inputs: tweets, Substacks, podcast clips, news articles, and Reddit posts, the whole funnel. The analyst ranker filtered all of that down to 5,237 trades it actually surfaced to the feed, and 803 notifications pushed to users. We backtested buying the top-confidence assets the AI surfaced for each piece of content, in the direction it called, held for exactly seven days, then closed. Same dollar size on every trade, sized so that average leverage across the full window came out to 1x. The backtest starts a week after launch, on Jan 27, so every position is fully filled.


**+54.1% net versus the S&P's +7.6%** A 46.6 percentage-point beat, at a Sharpe of 3.92 against SPY's roughly 0.4 over the tested period. $10,000 placed in the strategy on Jan 27 was worth $15,413 on May 14. The same $10,000 in SPY was worth $10,757.


The strategy beats SPY by 47 percentage points over 108 days, with a Sharpe of nearly 4, and was positive during the period when SPY was bleeding 9%. The mechanism is concentrated exposure to the sub-cohorts where the engine has the densest credible-source coverage. The rest of this piece is about what happened in detail, and the macro and academic reasons it should keep working.


## Why didn't Freeport draw down when SPY did?


The biggest contiguous drawdown in the S&P during the observed period was the Feb 25 to Mar 30 episode, a 34-day grind where SPY drew down 8.83% peak to trough. Over those same 34 days, Freeport's trades returned plus 9.40%. The mechanical reason is that the feed was talking about Iran all the time. Every credible commodities desk, macro account, and Substack we ingested suggested oil prices were going higher for longer, so the recommendation engine surfaced oil longs often, and the long-energy part of the book returned plus 31% over the period, weighted at about 26% of the book, which offset the entire equity-side drawdown.


Was it luck? Honestly, maybe a little. But every credible expert covering the region was calling oil higher, and oil did go higher. Twitter at the time was deeply doomer; the consensus retail play was "sell everything." What the strategy actually did was hold oil and equities at the same time, and that turned out to be exactly the right call, because equities ripped higher in a historic rally right after the drawdown finished. Anyone who pulled out of the index entirely missed the snapback.


## Semiconductor melt-up


A real chunk of the strategy's overall outperformance came from semis, the largest weight in the book at 32.7%. The back-half melt-up in AI-capex and memory names did most of the heavy lifting on the overall return; the strategy's gains accelerated in late March and April mostly on the back of it. For the first two months the Freeport trades and SMH, the semiconductor ETF, moved together, both riding the AI-capex narrative roughly evenly. A gap opened in the late-March, early-April leg, when hyperscaler-earnings coverage started clustering into a specific handful of names where the engine's calls compounded.


SMH is market-cap weighted, which means most of its return is NVDA, AVGO, and TSMC, three names that did fine but did not lead the back-half move. The feed, by contrast, was loud about the smaller-cap and memory names that actually melted up: INTC, MRVL, MU, AMD. Twitter was deep in Intel's foundry turn, Marvell's hyperscaler-ASIC wins, Micron's HBM cycle, and AMD's accelerator share-take, and the engine surfaced those calls in volume. When those tickers ran hard in the back half of the window, the cohort caught most of it while the cap-weighted basket dragged behind, because its top three holdings did not participate the same way. The engine was successfully picking up smaller-cap signal at the speed publishers were writing it, which is exactly the kind of dispersion a cap-weighted ETF averages away.


## Energy, defense, and onchain finance


On commodities it was mostly oil doing the heavy lifting, earning plus 27.7% net with a Sharpe of roughly 2.0 and a 52% hit rate over 208 trades.


Non-semiconductor stocks did plus 11.9% versus SPY's plus 7.6%. The 207-trade group ran at a Sharpe of 1.49, about 4.3 percentage points ahead of SPY. Almost all of the returns were generated in the first half of the window, on feed-pushed buys into individual energy and defense names right as the Iran conflict was heating up: the same signal that drove the commodities cohort, just expressed in single-stock form. The less flattering half of the picture is that once market attention drifted off the conflict and rotated into other themes, the non-semi book actually underperformed SPY for the back half of the window. The cohort's overall beat is real, but it is concentrated in one narrative window where the feed had a clear informational edge, not evenly distributed across the period.


Crypto trades did plus 9.8% versus BTC's minus 9.2%, mostly driven by HYPE. The Hyperliquid token was getting heavy mention across the tier-1 crypto accounts the feed listens to, and the engine kept surfacing HYPE buys while BTC was drawing down. It is the crypto-side analog of the semis story: BTC is the obvious reference point, but the feed was loud about a different name inside the same universe, and that is what actually moved.


## Consistency


One number can be lucky; consistency is the higher bar. The strategy was positive in more than 60% of weeks across the window, with a longest losing streak of two weeks. The 14-day rolling alpha versus SPY stays positive across most of the window, but it is worth being precise about what kind of outperformance this is: the alpha clusters around macro-event windows, the February-to-March oil leg, the April recovery, the early-May earnings dispersion, not evenly across days. It is not a continuous-alpha machine. It is an episodic-alpha machine, well shaped around the moments where information actually matters.


The strategy's daily exposure to each asset class also scales automatically with news flow. When sharp desks and smart accounts were writing intensively about energy supply shocks, the commodity share of the book swelled and the semis share shrank; when hyperscaler earnings dominated the feed, the opposite happened. The book follows the conversation, which is the whole design.


## A thousand edges, or two macro calls?


Before we close out, the honest caveat: there is a real question about whether the alpha we just showed you is repeatable alpha or period alpha, and it is worth being upfront about the limits of what the math can prove. On its face we have 803 trades from thousands of ingested signals, but the issue is that the trades are not independent. Most of them cluster around two macro events: the AI-capex super-cycle, which lifted basically every name in the semis cohort together, and the Iran-conflict oil supply shock, which lifted oil, defense, and any escalation-adjacent equity in parallel.


Those are not 800 distinct calls. They are closer to two macro calls with 800 expressions. When trades are highly correlated, the effective number of independent observations collapses. The intuition: 100 people all betting on the same coin flip don't give you 100 independent data points on the coin; they give you one. The same logic applies here, scaled. This matters because statistical power, the ability to tell "this strategy is real" apart from "this strategy got lucky in a window," depends on the effective sample size, not the raw count, and under this kind of cohort-level correlation the effective sample shrinks fast. Two well-caught macro bets is still a great result. But it is a very different claim from 800 independent edges all clearing.


What we can say with confidence is that the engine caught both macro narratives early, surfaced them across the right asset classes, and did not pull you out of equities during the drawdown. What we cannot say from this data alone is whether that pattern will repeat.


## Why we think this keeps working


A backtest is only as durable as the mechanism behind it. The cynical reading of any backtest is "you fit it." The more durable reading is "there is a mechanism, and the data is consistent with the mechanism." Four decades of finance research give us a defensible set of reasons why credible-source-curated news strategies should earn alpha.


**1. The mechanism has not been "figured out" yet.** When companies report earnings, their stocks tend to keep drifting in the direction of the surprise for days afterward. Researchers noticed this in the 1960s and called it[post-earnings-announcement drift](https://freeportlogbook.substack.com/p/post-earnings-announcement-drift) ; it happens because the market needs time to actually digest news, not just react to it. Over the decades, big hedge funds figured out how to trade the simple version and mostly squeezed it out of existence. But the same delayed-reaction pattern still exists for harder-to-read news: tweets, rumors, leaked imagery, unusual options activity. These stories are tougher to interpret than a clean earnings number, and the market still under-reacts to them on day one. Research shows this language-driven version of the drift has not been arbitraged away, and the world is moving toward more news- and event-driven markets, not fewer.


**2. We trade in venues where big institutional traders are scarce.** Most of Wall Street's smart money, the quant funds and prop desks, runs in big, liquid markets like the S&P 500, where competition is brutal and price inefficiencies disappear in seconds. Smaller venues, like tokenized stocks, perpetual futures, and other crypto-adjacent markets, have far less of that pressure. With fewer professional traders racing to close a gap, the gap lasts longer.


**3. Being first on a story carries structural risk most participants cannot take.** We ran a test: what if we required a second trusted source to confirm a story before pushing the trade? It sounds safer, but it made the strategy meaningfully worse, because by the time a second source picks up a story, the market has typically already moved on the first one. For a career trader at a fund, betting hard on an unconfirmed story is a career risk: if the rumor is fake and they sized aggressively, they look reckless. Their incentive is to wait for confirmation, and by the time it arrives, the easy trade is gone. Freeport's users don't have that incentive structure. Acting on a credible-but-unconfirmed story carries genuine risk, sometimes the rumor is false, sometimes the photo is AI-generated, sometimes nothing happens, and over time the market pays a premium to whoever is willing to bear that risk instead of waiting for safety.


**4. Attention diffuses gradually across investor populations.** Decades of research show that attention itself is a tradeable signal: Google search volume predicting stock returns (2011), retail traders piling into attention-grabbing names and creating price pressure (2008), the tone of the Wall Street Journal's daily market column predicting next-day index moves (2007), and curated research distribution making the resulting order flow more predictive of where prices end up (2022). The mechanism underneath all of these findings is what academics call gradual information diffusion. News does not reach everyone at the same time: the first wave is people directly tuned to credible sources, the next is traders who notice the price moving, and the wave after that is mainstream coverage and the broader public. Our seven-day hold is the engine's mechanical attempt to sit inside that diffusion window: enter when the first credible voice surfaces a story, hold while the rest of the market catches up, exit before the late-stage overshoot.


## Our ask for readers


If you trade through Freeport, these calls land in your feed in real time. The fastest way to find out whether the edge holds up is to actually use the product. If our calls are good, you will see it in your own trades. If they are bad, if we whiff three in a row, or the engine pushes something that looks like noise, tell us. We would rather hear it from a user than have to run the analysis to find out ourselves. A backtest is still a simulation, though; for what real users actually earned, read[How Freeport Users Made 11.7% on $27M in the Last 45 Days](https://freeportmarkets.com/research-users-made-117.html) . This essay is the receipts for the last 108 days. The next 108 are yours to grade.


Read more from the Freeport research team on the Freeport Logbook.


[Freeport Logbook](https://freeportlogbook.substack.com/)
