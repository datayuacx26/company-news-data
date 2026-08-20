---
schema_version: "1.0.0"
document_id: "0c944d50e417b8105e9ca14aafcbe9927adf14b74fb0749ed837f72d5cf57aa8"
company_key: "yc-freeport-markets"
company: "Freeport Markets"
source_id: "yc-freeport-markets-news-import-46c2f164323d"
canonical_url: "https://www.freeportmarkets.com/research-users-made-117.html"
published_at: "2026-04-18T00:00:00+00:00"
first_seen_at: "2026-07-25T05:46:52.249644+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:66c3e1151986ce9d5cf0a023f000dfc1ceb4ffe901d32e5802e959c30732697b"
---

# Freeport Users Made 11.7% on $27M in 45 Days · Freeport

Most financial products optimize for attention. Freeport was built around a different bet: that our news feed should not only inform users but help them act faster and more precisely. So the question we care about is not whether the feed is engaging, but whether users who trade through it actually make money. Forty-five days in, the early answer is encouraging.


Since launch, Freeport users have generated roughly $27 million in trading volume across indices, commodities, single stocks, crypto, and pre-IPO tokens. On a money-weighted basis, aggregate user returns stand at 11.7%. That is a meaningful number, but it is not, by itself, proof of anything. This essay is about being honest about what it does and does not show.


**$27M in volume, +11.7% money-weighted** Aggregate user returns over the first 45 days since launch, spanning indices, commodities, single stocks, crypto, and pre-IPO tokens.


## What our users traded


The first thing that stands out in the data is what users chose to trade. NASDAQ 100 and S&P 500 perpetuals together accounted for about a third of volume, WTI crude for roughly 15%, with the rest spread across single stocks, crypto, and pre-IPO tokens. Users are not speculating on meme-coins; they are trading the kind of real assets that populate the books of global macro hedge funds and commodity trading advisors.


Just as notable is the leverage discipline. Average leverage across the user base sits in the 2-4x range, far below the 10-200x maximums the platform allows. That is the measured sizing you would expect from someone expressing a market view with a defined risk budget.


## Trading the escalation


Numbers without context are hard to interpret. To understand why users made money, you need the macro environment they traded through and how they responded to it in real time. Freeport launched at the end of February, amid sharply escalating Middle East tensions, and over the following weeks WTI crude moved from the low $60s to above $100. The feed surfaced conflict headlines from newswire sources within milliseconds of publication, and users who recognized the implications went long oil via[perpetual futures](https://freeportmarkets.com/research-perpetual-future.html) , capturing a substantial portion of the move.


What may be surprising is that the majority of oil-regime PnL came from users who entered hours and days later, as the escalation deepened. Most of the money was not made on the initial headline but on the sequence of escalations that followed. Each new development pushed crude higher, and users who read the feed's real-time coverage recognized that each event made the next escalation more likely.


Were our users merely lucky, or does present news carry implications for future news that the broader market has not yet digested? Research suggests the latter is possible, through a mechanism closely related to[post-earnings announcement drift](https://freeportlogbook.substack.com/p/post-earnings-announcement-drift) : Bernard and Thomas (1989) showed that investors systematically underreact to the serial correlation in earnings surprises. Here the "earnings surprise" is a geopolitical escalation, and the under-reaction is the market's failure to price in that one action raises the probability of the next. More generally, users were exposed to event-driven momentum of the kind Hong and Stein (1999) predict should generate persistent returns when information diffuses slowly across investor populations. Not all market participants see the same information at the same time, and those who see it first can position before the market fully adjusts. Freeport's feed compresses the information lag; users who acted on it were, in effect, trading against slower participants.


## Trading peace rumors


The other major contributor was the de-escalation trade, and it is worth explaining in detail, because what users did not do matters as much as what they did. When diplomatic channels reopened in late March, the obvious trade was to short oil. That is not what happened on Freeport. Users overwhelmingly went long NASDAQ and S&P instead, and in the following days the NASDAQ 100 rallied roughly 15% off its lows.


This is partly the architecture of the recommender, which is biased against suggesting shorts unless a short is genuinely the best expression of the trade, and toward going long anti-correlated assets instead, in this case equity indices. The reason is that financial media has a well-documented negativity bias: Tetlock (2007) showed that negative media content predicts downward pressure on prices, but the effect reverses, because the negative framing systematically overstates actual deterioration. Headlines skew negative; equities climb walls of worry.


We also want to be honest about the counterfactual: we do not know what would have happened if the feed had surfaced oil-short analysis instead. Users might have shorted crude and still made money, or gone long equities anyway on their own conviction. What we can say is that the feed's editorial weighting toward index-long analysis coincided with NASDAQ and S&P longs being the single largest driver of aggregate returns in the second regime.


## Decomposing the returns


Claiming 11.7% in 45 days and walking away would be intellectually dishonest. So let us break it down. There are three obvious contributors, and a positive residual.


**Market beta, roughly 4-5 points.** The S&P is up since launch, and users had meaningful equity exposure: NASDAQ 100 and S&P 500 perps together were 33% of volume. If you are long equities during a rally, you make money regardless of skill. This is real, and we do not hide it.


**Short-term momentum, roughly 3-4 points.** Users traded in the direction of recent price moves: long oil while oil rose, long indices while they rallied. Momentum is the most robust anomaly in empirical finance, and broad trend-following benchmarks performed well over the same months. We separate momentum from discretionary timing with a standard factor regression; whatever the factors explain is mechanical, and whatever they do not is the residual, which captures rotation timing, position sizing, and the choice of which signals to follow.


**Oil concentration, roughly 2-3 points.** WTI crude was 15% of total volume, and oil had a strong directional move during the first half of the period. Concentrated exposure to an asset that moves 15-20% will mechanically contribute several points to aggregate returns. We flag it explicitly.


**The residual, roughly 2-3 points.** After controlling for beta, momentum, and oil, some return remains. The residual is positive, but the t-statistic is approximately 0.3, which is not statistically significant on 45 days of data. It is, however, economically meaningful: even a percentage point of excess return over 45 days, if sustained, would annualize to a figure that exceeds most hedge fund performances. And the mechanism is plausible: faster information leads to faster action, which captures the early portion of a move before slower participants adjust, especially on onchain rails overnight and over weekends, when prices are much less efficient. This is not a novel claim; Jame, Johnston, Markov, and Wolfe (2022) found that curated research articles made retail order flow more predictive of future returns. Freeport's feed is the same mechanism accelerated: verified sources, AI-filtered for actionable signals, with one-tap execution.


## What the best traders had in common


The top 25 users by PnL, roughly 1% of the trading base, generated a combined money-weighted return of 18.2% over the 45 days. What set them apart was not access to better signals, since everyone sees the same feed, but behavior. They traded less often, averaging 2.1 trades per active day versus 5.8 for the median active user. They were more disciplined with leverage, at a median of 2.4x versus 3.3x. And they held positions longer, a median of 31 hours versus 19, letting winners run. Across the cohort, selectivity and patience were the dominant predictors of outperformance.


This is the behavior Freeport is designed to encourage. When we design the product, we think explicitly about the difference between empowering users and exploiting them. Every social media feed in existence optimizes for time spent; we want to optimize for results. That means fewer notifications, not more. It means surfacing why something matters, not just that it happened. And it means giving users the context to decide not to trade, because the best traders in our data sat out more often than they acted. We believe the best trading app is the one that makes you a better trader, not the one that keeps you glued to a screen. Forty-five days of data suggests that philosophy is working, and we intend to keep proving it. The companion post,[How Freeport's AI Beat the S&P by 46% in 16 Weeks](https://freeportmarkets.com/research-ai-beat-sp.html) , tests the recommendation engine itself over a longer window.


Read more from the Freeport research team on the Freeport Logbook.


[Freeport Logbook](https://freeportlogbook.substack.com/)
