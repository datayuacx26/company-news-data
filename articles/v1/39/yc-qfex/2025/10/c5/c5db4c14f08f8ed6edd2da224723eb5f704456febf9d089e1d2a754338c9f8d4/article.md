---
schema_version: "1.0.0"
document_id: "c5db4c14f08f8ed6edd2da224723eb5f704456febf9d089e1d2a754338c9f8d4"
company_key: "yc-qfex"
company: "QFEX"
source_id: "yc-qfex-rss-5ea6110e160b"
canonical_url: "https://research.qfex.com/p/addressing-non-arbitrage-conditions"
published_at: "2025-10-30T21:28:49+00:00"
first_seen_at: "2026-07-25T20:12:49.931294+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:d510c12c1d5ab15c15d78919b6a9bc761d1bdc7efcb8c35f087375f714534a1b"
---

# Perp Problems When The Underlying is Closed

# Perp Problems When The Underlying is Closed


### Some discussion around the paper Non-Arbitrage Conditions for Perpetual Forwards by Jez (@izebel_eth)


[Annanay Kapila](https://substack.com/@annanaykapila)


Oct 30, 2025


Way back in 1992, Robert J. Shiller proposed a robust derivatives solution for illiquid spot markets. Until recently, low liquidity was fast becoming a distant memory, with crypto spot and perps markets trading hundreds of millions daily. Equity perps are on the horizon; but apart from a few ATS’s here and ADR’s there, US equities only trade 16 hours a day, five days a week. Talk about low liquidity!


Below, I examine the paper by Jez (


[@izebel_eth](https://x.com/@izebel_eth) ) to tease out the problems this causes perps exchanges, and how we’ve fixed them at


[QFEX](https://x.com/TradeQFEX) .


[Full paper](https://x.com/izebel_eth/status/1983192240072860049)


The paper talks about how


[@tradexyz](https://x.com/@tradexyz) gets the oracle index price for their XYZ-100 perp from CME Nasdaq-100 (NQ) futures, but needs a bit of a preamble. Why bother doing this at all? The underlying index (Nasdaq-100) is easy to find, and it’s hard to imagine that it’s legally more risky to consume Nasdaq-100 directly than NQ futures.


The answer: NQ futures trade 23 hours daily / 5 days weekly, whereas the underlying stocks only trade 16 hours daily / 5 days weekly. To the unaware, this is a bit weird. you can trade indices (via futures) long after (and before) the stocks in those indices themselves. Having oracle pricing for longer is worth the headache of deriving from the futures prices, because perps are dangerous to the exchange when the underlying is closed.


What can go wrong? Broadly, a well-capitalized trader can move the market a large amount away from its fair price (see the post below for an onchain example), capitalizing on the resulting liquidations. Usually, unless they can do this very quickly, they will be penalized for this via funding payments; but how do we even decide the funding payment when the underlying is closed?


[Link to post](https://x.com/lookonchain/status/1965239019148869978)


According to their docs,


[@tradexyz](https://x.com/@tradexyz) pays funding based on a synthetic oracle price derived from the perps book, but made slightly ‘sticky’ to the last known close price. This stickiness is hard to calibrate and imposes a belief that moves during market close should always revert. On the other hand,


[QFEX](https://x.com/TradeQFEX) does not pay funding during close, adhering to the principle that this ‘mean-reversion’ mindset isn’t always right. What if it’s a genuine move? Then the market could ‘gap’ at the next open, and the exchange would take the risk of not being able to liquidate losing users fast enough: a decidedly worse outcome.


[QFEX](https://x.com/TradeQFEX) and


[@tradexyz](https://x.com/@tradexyz) also both set minimum and maximum prices based on the last known open price. While not an ideal solution, it does prevent egregious moves while the market is closed.


As for the calculation itself, eagle-eyed readers might question the need for fixed constants in the calculation of the


[@tradexyz](https://x.com/@tradexyz) spot index. What if dividend payout changes, like it did during Covid? The answer is that the market will correct by changing the price of the perp to compensate. At


[QFEX](https://x.com/TradeQFEX) , we use the daily close prices of the index and future, with an adjustment for the price change in the future: a much simpler and cleaner approach, that mirrors how we did it when I was working as a market maker at Tower.


Janus Henderson


Finally, the situation is more bleak for stocks. Although the paper talks about deriving a stock price from a future, futures don’t exist on single US equities, so this discussion is purely hypothetical. To make matters worse, stocks are not 24/7 and only open during market hours, compounding the issues discussed above.
