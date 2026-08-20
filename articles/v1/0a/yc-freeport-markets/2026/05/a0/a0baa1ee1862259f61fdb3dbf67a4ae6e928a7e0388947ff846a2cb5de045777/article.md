---
schema_version: "1.0.0"
document_id: "a0baa1ee1862259f61fdb3dbf67a4ae6e928a7e0388947ff846a2cb5de045777"
company_key: "yc-freeport-markets"
company: "Freeport Markets"
source_id: "yc-freeport-markets-news-import-46c2f164323d"
canonical_url: "https://www.freeportmarkets.com/research-recs-make-money.html"
published_at: "2026-05-22T00:00:00+00:00"
first_seen_at: "2026-07-25T05:46:52.249644+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:db76f65852c357fc62bc42303103845ee1405f333f24c4965a083b1dd98fc6aa"
---

# Do Freeport's Trade Recs Make Money? · Freeport

This is the short version of two longer posts:[How Freeport's AI Beat the S&P by 46% in 16 Weeks](https://freeportmarkets.com/research-ai-beat-sp.html) and[How Freeport Users Made 11.7% on $27M in the Last 45 Days](https://freeportmarkets.com/research-users-made-117.html) . The full numbers, decompositions, and caveats live there. Here you get the core argument, the headline numbers, and the caveats you should weigh before you trust any of it.


## How the recommendations work


Freeport's recommendation feed is not a black box spitting out signals. It is a pipeline with a specific, explainable design, and it starts by listening to the people who move markets. A service continuously monitors finance X for the analysts, traders, and domain experts whose calls tend to lead price. An AI model, equipped with domain context and deep memory, reads each relevant post and converts it into a structured signal: a ticker, a direction, a confidence score, the reasoning, and a category.


Those signals flow into Freeport's recommender, which scores and ranks them across roughly a dozen weighted factors such as recency, the source's credibility tier, how many independent voices agree, engagement, and fit to your portfolio. Ideas confirmed by multiple credible sources rank higher. The output is a ranked feed of trade ideas, each carrying the reasoning that produced it; the ones you see are what the engine qualified as the best. So "do the recs make money" really means: if you had followed what this system surfaced, how would you have done? That is testable, so we tested it.


## The backtest


In our "buy the feed" backtest, over the window of January 27 to May 14, 2026 (about 108 days), the Freeport strategy returned +54.1% against the S&P 500's +7.6% over the same period. The risk-adjusted number is the part that genuinely surprised us: the strategy ran at a Sharpe ratio of 3.92 against the market's roughly 0.4, meaning the outperformance did not come from simply taking wild risk. And the most interesting part of the data is the drawdown: during the window's deepest S&P selloff, from February 25 to March 30, the market fell 8.83% peak-to-trough while the strategy was up +9.4%. In the market's ugliest stretch, the strategy did not just survive, it climbed.


**+54.1% net versus the S&P's +7.6%** A 46.6 percentage-point spread at a Sharpe of 3.92, against the market's roughly 0.4 over the tested period.


One honest flag: the backtest included over 800 individual trades, but they were not 800 independent bets. They clustered into essentially two macro themes, long oil during the Middle East escalation and long semis as the AI-capex narrative dominated the back half. The trades inside each theme were highly correlated, so the effective sample is much smaller than the raw count suggests, and the result should not be read as carrying the statistical power of 800 independent observations. The honest framing of what the feed produces is episodic alpha: a thesis-and-event-driven workflow that catches the right narrative as it forms and rides it across many expressions, not a permanent statistical edge that will simply print money over time. It is, by nature, cyclical: it should do well when there is real news to react to and credible voices doing the reacting, and it will likely perform poorly in newsless, drift-up or drift-down markets where there is nothing for a feed to be early on.


## What real users did


A backtest is a simulation. The more grounded question is what real people earned, so we looked at that too. Across roughly the first 45 days of trading, aggregate user profit-and-loss came to about +11.7%, slightly lower than the backtest's return over the same period.


The top 1% of users did notably better, earning roughly 18.2% over the same window. What set them apart was not better signals (everyone sees the same feed) but behavior: fewer trades (2.1 vs. 5.8 per active day), lower leverage (2.4x vs. 3.3x), and longer holds (31 vs. 19 hours). Selectivity and patience were the dominant predictors of outperformance.


## The caveats


A strong backtest is evidence, not a promise. The window was short, mostly a bull market, and measured in-sample, meaning the cohorts and rules were chosen with knowledge of the period. The 800+ trades concentrate into two macro themes, so the effective sample is much smaller than the trade count, and the edge is best framed as episodic alpha that is cyclical and likely to cool off in newsless markets. Past performance does not predict future results, and in this case we genuinely believe that.


That said, the mechanism is not a fluke of this window. Decades of research show markets under-react to qualitative, language-driven news such as tweets, leaked imagery, and escalation rumors far longer than to a clean earnings number, and the smaller venues Freeport trades into, perpetuals, tokenized stocks, crypto-adjacent markets, carry far less institutional competition pinning prices to fair value. Professional traders also face real career risk sizing into single-source rumors, so the market pays a premium to whoever acts on credible-but-unconfirmed information before the second and third sources arrive. Freeport's pipeline is built to sit inside exactly that diffusion window: listen to credible voices, surface the call early, hold while the rest of the market catches up. Used well, the recommendations should beat a passive benchmark over a full cycle; used poorly, they will not.


## How to use them well


The recommendations make money for people who use them as what they are: high-quality inputs to your own decision. The right mental model is a research team that reads the whole market for you and hands you its best ideas with reasoning attached. A research team does not place your trades; you do. The final call, whether to act, how much to size, when to exit, is yours, and it is supposed to be yours. Read the why on every recommendation, not just the ticker. If the reasoning does not convince you, that is information, and skipping a trade you do not understand is valid and often correct.


The framework we think is correct for actually pulling the trigger is stacking signals. A single voice saying buy is one input; conviction comes from combining sources of information that are independent of each other: news, opinion, fundamentals, technicals, and catalysts. The cleanest setup is when most of them point the same way. Someone you trust and believe has genuine edge is calling the name a buy. The fundamental picture, earnings trajectory, balance sheet, sector dynamics, leaves room for the market to support a higher valuation. The chart actually looks buyable: a clean trend, a base that just broke, a level that historically attracts flow. And there is a known catalyst in the near future, earnings, a product event, a macro print, that gives the price a specific reason to move in your window. When all of these line up, that is a high-conviction trade, and that is when you should be willing to size meaningfully.


Freeport's job in that stack is to compress the time it takes you to assemble it: the feed and analyst are the trusted-voice layer, the symbol detail page carries the fundamentals and catalyst context, and the chart lets you sanity-check the technical setup before you act. Most of the time, not every signal will line up, and that is where your judgment comes in rather than the feed's.


Some symbols are mostly driven by one input, and you can underweight the others: meme-heavy small caps trade on flow and narrative more than fundamentals, megacaps trade on earnings and rates more than chart shape, commodity-linked names trade on the underlying more than anything company-specific. Different regimes also reward different signals: in a high-volatility news cycle, the trusted-voice and catalyst layers do most of the work; in a sleepy drift-up tape, fundamentals and technicals matter more, because there is less news to be early on. Knowing which inputs to trust for which name in which regime is the discretionary skill the product cannot do for you, and it is exactly what the top users in our data are doing when they trade less often, hold longer, and beat the average.


If you just want to log in, tap follow, and never think, the honest advice is probably a low-cost index fund, and we will give you that advice for free. Freeport is built for people who want to participate in markets with judgment, using the best available inputs. The recommendations sharpen that judgment; they do not replace it.


Read more from the Freeport research team on the Freeport Logbook.


[Freeport Logbook](https://freeportlogbook.substack.com/)
