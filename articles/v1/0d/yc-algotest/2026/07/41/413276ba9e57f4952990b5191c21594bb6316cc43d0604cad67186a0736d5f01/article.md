---
schema_version: "1.0.0"
document_id: "413276ba9e57f4952990b5191c21594bb6316cc43d0604cad67186a0736d5f01"
company_key: "yc-algotest"
company: "AlgoTest"
source_id: "yc-algotest-rss-ebd738da757e"
canonical_url: "https://algotest.in/blog/nifty-lot-size/"
published_at: "2026-07-21T08:26:56+00:00"
first_seen_at: "2026-08-09T18:57:55.798384+00:00"
fetched_at: "2026-08-09T18:57:57.664591+00:00"
content_hash: "sha256:6848fb99010ce94680c1ff0cc13b656015070fe5ae1b1c2364e747a4af5b939f"
---

# Nifty Lot Size Explained: Nifty, Bank Nifty, FINNIFTY & Midcap Nifty Lot Sizes

# Nifty Lot Size Explained: Nifty, Bank Nifty, FINNIFTY & Midcap Nifty Lot Sizes


[AlgoTest](https://algotest.in/blog/author/algotest/)


Jul 21, 2026


•


5 min read


•


[General](https://algotest.in/blog/category/general/)


•


[Markdown](https://algotest.in/blog/nifty-lot-size.md)


- [Twitter / X](https://twitter.com/intent/tweet?url=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fnifty%2Dlot%2Dsize%2F&text=Nifty%20Lot%20Size%20Explained%3A%20Nifty%2C%20Bank%20Nifty%2C%20FINNIFTY%20%26%20Midcap%20Nifty%20Lot%20Sizes)
- [Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fnifty%2Dlot%2Dsize%2F)
- [LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fnifty%2Dlot%2Dsize%2F&title=Nifty%20Lot%20Size%20Explained%3A%20Nifty%2C%20Bank%20Nifty%2C%20FINNIFTY%20%26%20Midcap%20Nifty%20Lot%20Sizes)
- [WhatsApp](https://api.whatsapp.com/send?text=Nifty%20Lot%20Size%20Explained%3A%20Nifty%2C%20Bank%20Nifty%2C%20FINNIFTY%20%26%20Midcap%20Nifty%20Lot%20Sizes%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fnifty%2Dlot%2Dsize%2F)
- [Ask ChatGPT](https://chatgpt.com/?hints=search&q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fnifty%2Dlot%2Dsize%2F)
- [Ask Claude](https://claude.ai/new?q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fnifty%2Dlot%2Dsize%2F)
- [Ask Grok](https://grok.com/?q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fnifty%2Dlot%2Dsize%2F)
- Copy link


**The current Nifty lot size is 65 units (effective from the January 2026 expiry series).** Every Nifty futures and options contract represents 65 units of the index.


Since NSE revises lot sizes periodically, it's important to use the latest value before placing a trade or backtesting a strategy.


In this guide, you'll find the latest **Nifty, Bank Nifty, FINNIFTY, and Midcap Nifty lot sizes** , learn why NSE changes them, and understand how they affect margin, risk, and contract value


## What Is Nifty Lot Size?


**Nifty lot size** is the fixed number of units in one Nifty futures or options contract.


Unlike stocks, index derivatives can't be traded one unit at a time. Instead, NSE defines a fixed lot size for every contract. The current **Nifty lot size is 65 units** , so every futures or options trade must be placed in multiples of 65.


## Latest NSE Lot Sizes (2026)


Here's where things stand right now, based on NSE's most recent revision:


Instrument


Current Lot Size


Applicable Since


Nifty 50


65


January 2026 series


Bank Nifty


30


January 2026 series


FINNIFTY (Nifty Financial Services)


60


January 2026 series


Midcap Nifty (Nifty Midcap Select)


120


January 2026 series


The current Nifty lot size is **65** , while Bank Nifty, FINNIFTY, and Midcap Nifty have different lot sizes based on their contract values.


NSE reviews these numbers periodically, roughly every six months, under a SEBI-mandated framework. Don't treat these as permanent.


Always confirm the lot size on your trading terminal before placing an order. A mental note from even a year ago can already be outdated.


Related:[Nifty Expiry Day Explained](https://algotest.in/blog/nifty-expiry-day)


## Why Does NSE Change Lot Sizes?


NSE revises lot sizes to keep the value of index derivatives within SEBI's prescribed contract value range. As index prices rise or fall, lot sizes are adjusted so contracts don't become too expensive or too small.


That's why Nifty, Bank Nifty, FINNIFTY, and Midcap Nifty all have different lot sizes.


## How Lot Size Affects Your Trading


Your lot size affects much more than just the number of units you trade. It directly impacts:


-


**Margin requirement:** A larger lot size usually requires more margin to open a position.


-


**Profit and loss:** Every point the option premium moves is multiplied by the lot size.


-


**Position sizing:** A change in lot size changes your overall market exposure, even if you trade just one lot.


-


**Capital required:** If you trade with a fixed budget, a lot size revision can increase or reduce the capital needed for a single trade.


When comparing indices, don't look at the lot size alone. A **120-unit Midcap Nifty lot** and a **30-unit Bank Nifty lot** represent very different contract values because the underlying indices trade at different price levels.


### Example


Suppose you sell **1 lot of Bank Nifty options** at a premium of **₹150** . Since the current Bank Nifty lot size is **30 units** , the premium you receive is:


**₹150 × 30 = ₹4,500**


Now, if the option premium increases by **₹50** , your loss becomes:


**₹50 × 30 = ₹1,500**


This is one of the most common mistakes new traders make. They calculate profit or loss based on the[premium per unit](https://algotest.in/blog/black-scholes-option-pricing-model/) and forget that every point is multiplied by the entire lot size.


## How to Calculate Nifty Contract Value


The formula is simple:


### Contract Value = Index Price × Lot Size


For example, if **Nifty 50 is trading at 24,500** and the **lot size is 65** , the contract value is:


**24,500 × 65 = ₹15,92,500**


This is the **notional value** of one contract, not the amount you pay upfront.


-


**Options buyers** pay only the option premium for 65 units.


-


**Futures traders** and **options sellers** need to maintain margin, which is typically **10–15% of the contract value** , depending on market volatility and your broker.


For a contract worth around **₹16 lakh** , the required margin could range from **₹1.6 lakh to ₹2.4 lakh** . Instead of estimating, you can use the[AlgoTest Margin Calculator](https://algotest.in/margin-calculator) to check the latest margin requirements before placing a trade.


## Nifty Futures vs Nifty Options Lot Size


**Nifty futures and Nifty options have the same lot size.** If the current Nifty lot size is **65** , every[futures and options](https://algotest.in/blog/futures-and-options-trading/) contract represents **65 units** of the index.


The difference isn't the lot size—it's the capital required to trade.


-


**Options buyers** pay only the option premium.


-


**Futures traders** and **options sellers** need to maintain margin, which is significantly higher.


Although both contracts have the same lot size, futures and short options generally require much more capital than buying an option.


## Nifty Lot Size History


Lot sizes aren't fixed forever. Here's how they've moved for the major indices over the last two years:


Date


Nifty 50


Bank Nifty


FINNIFTY


Midcap Nifty


What Changed


Before April 2024


50


15


40


75


Baseline


April 2024


25


15 (no change)


25


50


Periodic review as index levels had risen


November 2024


75


30


65


120


SEBI's new rule raising minimum contract value to around ₹15 lakh


April 2025


75 (no change)


35


65 (no change)


140


Periodic review as Bank Nifty and Midcap Nifty prices had moved


January 2026 (current)


65


30


60


120


Periodic review as index levels shifted again


The pattern is clear: lot sizes aren't a one-way street. They go up when an index price falls or a regulatory floor is introduced.


They come down when the index price climbs and NSE wants to keep contract values from getting too large. Expect this cycle to continue roughly twice a year.


Related:[Nifty Midcap 150: Complete Stocks List, Weightage, and Trading Guide (2026)](https://algotest.in/blog/nifty-midcap-150)


## Common Mistakes Traders Make


When trading Nifty futures or options, avoid these common mistakes:


-


**Confusing 1 lot with 1 unit** , leading to incorrect risk and capital calculations.


-


**Ignoring the contract value** and focusing only on the option premium.


-


**Calculating profit or loss per unit** instead of multiplying by the lot size.


-


**Using outdated lot sizes** , as NSE revises them periodically.


-


**Assuming all indices have the same lot size** , even though Nifty, Bank Nifty,[FINNIFTY](https://algotest.in/blog/guide-to-finnifty-financial-services-index-trading) , and Midcap Nifty each have their own contract specifications.


## Why Lot Size Matters When Backtesting Strategies


Accurate backtesting depends on using the **correct lot size** . If your platform uses outdated contract specifications, your margin requirements, position sizing, and profit or loss calculations can be inaccurate.


A strategy that looks profitable with an old lot size may require much more capital when traded in the live market.


Platforms like[AlgoTest](https://algotest.in/?utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_content=main-page&utm_term=algotest&utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_term=algotest&utm_content=main-page&utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_term=algotest&utm_content=main-page) automatically use updated contract specifications for Nifty, Bank Nifty, FINNIFTY, and Midcap Nifty. This helps ensure your[backtests](https://algotest.in/blog/how-to-backtest-options-trading-strategies-with-examples/) , margin estimates, and position sizing reflect current market conditions before you deploy a strategy live.


## Trade With the Right Numbers, Every Time


The Nifty lot size affects every futures and options trade you place, from margin and position sizing to profit and loss. Since NSE revises lot sizes periodically, it's a good habit to verify the latest values before entering a trade or running a backtest.


If you're testing or building options strategies, make sure you're using updated contract specifications.[AlgoTest](https://algotest.in/?utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_content=main-page&utm_term=algotest&utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_term=algotest&utm_content=main-page&utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_term=algotest&utm_content=main-page) helps you backtest, paper trade, and[simulate strategies](https://algotest.in/feature/simulator) using the latest lot sizes so your results stay as close to real market conditions as possible.


*Read More:*[8 Best Algo Trading Platforms in India in 2026](https://algotest.in/blog/10-best-algo-trading-software-in-india-2025)


[Best Brokers for Algo Trading in India in 2026: API, Speed & Compliance](https://algotest.in/blog/best-brokers-for-algo-trading-in-india/)


### Additional Resources


📚[Product Documentation](https://docs.algotest.in/?utm_source=blog&utm_medium=organic&utm_campaign=seo&utm_source=blog&utm_medium=organic&utm_campaign=seo)


🛠️ Trading Tools


-


[Margin Calculator](https://algotest.in/margin-calculator)


-


[IVR & IVP](https://algotest.in/feature/ivr-ivp)


-


[VRP Analysis](https://algotest.in/feature/vrp-analysis)


-


[OpenBroker](https://openbroker.in/)
