---
schema_version: "1.0.0"
document_id: "82c777f658b4270bb76ac9fb74a0b31fd657250e2a24cd771f2c34d1ec513f9a"
company_key: "yc-algotest"
company: "AlgoTest"
source_id: "yc-algotest-rss-ebd738da757e"
canonical_url: "https://algotest.in/blog/closing-auction-session/"
published_at: "2026-07-27T17:22:05+00:00"
first_seen_at: "2026-08-09T18:57:55.798384+00:00"
fetched_at: "2026-08-09T18:57:57.664591+00:00"
content_hash: "sha256:be02f61f3387b1af4dbaba35829a42b85ac6b74246d8538b7f5ef4eb93c554cc"
---

# SEBI's Closing Auction Session (CAS) and Its Impact on F&O Stocks

# SEBI's Closing Auction Session (CAS) and Its Impact on F&O Stocks


[AlgoTest](https://algotest.in/blog/author/algotest/)


Jul 27, 2026


•


6 min read


•


[General](https://algotest.in/blog/category/general/)


•


[Markdown](https://algotest.in/blog/closing-auction-session.md)


- [Twitter / X](https://twitter.com/intent/tweet?url=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fclosing%2Dauction%2Dsession%2F&text=SEBI%27s%20Closing%20Auction%20Session%20%28CAS%29%20and%20Its%20Impact%20on%20F%26O%20Stocks)
- [Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fclosing%2Dauction%2Dsession%2F)
- [LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fclosing%2Dauction%2Dsession%2F&title=SEBI%27s%20Closing%20Auction%20Session%20%28CAS%29%20and%20Its%20Impact%20on%20F%26O%20Stocks)
- [WhatsApp](https://api.whatsapp.com/send?text=SEBI%27s%20Closing%20Auction%20Session%20%28CAS%29%20and%20Its%20Impact%20on%20F%26O%20Stocks%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fclosing%2Dauction%2Dsession%2F)
- [Ask ChatGPT](https://chatgpt.com/?hints=search&q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fclosing%2Dauction%2Dsession%2F)
- [Ask Claude](https://claude.ai/new?q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fclosing%2Dauction%2Dsession%2F)
- [Ask Grok](https://grok.com/?q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fclosing%2Dauction%2Dsession%2F)
- Copy link


From **3 August 2026** , eligible F&O stocks on NSE and BSE will close differently. Continuous trading will now end at **3:15 PM** , and the last 20 minutes of the trading day will be reserved for the **Closing Auction Session (CAS)** . Instead of using the old VWAP-based method, the exchange will determine the official closing price through an auction.


If you trade stock options or futures, this change is worth paying attention to. The official closing price is used for **expiry settlement, index calculation, mutual fund NAVs, and end-of-day risk calculations** , so it can affect your positions even if you never place an order during the auction.


## Key Takeaways


-


Continuous trading for eligible F&O stocks now ends at **3:15 PM** .


-


The **Closing Auction Session (CAS)** runs from **3:15 PM to 3:35 PM** .


-


The official closing price is determined using an **equilibrium price** , not the last traded price or the old 30-minute VWAP.


-


CAS directly affects stock F&O settlement and indirectly influences index options through the closing value of index constituents.


-


Most retail traders don't need to change their strategy, but expiry-day and algo traders should understand how the new process works.


## What is the Closing Auction Session?


The Closing Auction Session (CAS) is a 20-minute auction that takes place after regular trading ends for eligible F&O stocks. Instead of matching orders as they come in, the exchange collects all buy and sell orders and matches them at one common price where the maximum number of shares can be traded. This price becomes the stock's official closing price for the day.


CAS is held every trading day for eligible F&O stocks.


## Why Did SEBI Introduce the Closing Auction Session?


SEBI introduced the Closing Auction Session to make closing prices **fairer, more transparent, and less vulnerable to manipulation** .


Earlier, a stock's closing price was calculated using the[Volume Weighted Average Price (VWAP)](https://algotest.in/blog/backtest-an-option-buying-strategy-using-vwap-template) of trades executed during the last 30 minutes of continuous trading. Large trades placed during this period could influence the closing price, especially in stocks with lower liquidity.


This made the closing price more susceptible to influence, especially in less liquid stocks. CAS addresses this by determining one fair closing price through an auction.


### Benefits of CAS


-


**Reduces price manipulation** by making it harder for a few large trades near market close to influence the closing price.


-


**Improves price discovery** by matching all[buy and sell orders](https://algotest.in/blog/essential-intraday-trading-tips-for-traders) at one fair closing price.


-


**Makes F&O settlement more reliable** because settlement is based on a transparent auction price.


-


**Keeps index closing values more accurate** since constituent stocks also close through the auction.


-


**Helps mutual funds and ETFs value their portfolios more accurately** using a fair closing price.


-


**Brings Indian markets in line with global exchanges** like the NYSE and London Stock Exchange, which also use closing auctions.


### CAS currently applies to:


-


Stocks with active[Futures and Options](https://algotest.in/blog/futures-and-options-trading/) contracts.


-


Stocks without F&O contracts continue using the existing closing price mechanism.


-


SEBI may extend CAS to more securities in future.


## Closing Auction Session Timings


For eligible stocks, continuous trading ends at **3:15 PM** instead of **3:30 PM** . The next 20 minutes are divided into four stages.


Phase


Timing


What Happens


Reference Price Calculation


3:15 PM – 3:20 PM


The exchange calculates the reference price. No new orders are accepted.


Order Entry (Full Access)


3:20 PM – 3:25 PM


Traders can place, modify, or cancel limit and market orders.


Order Entry (Restricted)


3:25 PM – 3:30 PM


Only limit orders can be modified or cancelled. Market orders are frozen, and the window closes randomly during the final two minutes.


Order Matching


3:30 PM – 3:35 PM


Orders are matched at the equilibrium price. No further changes are allowed.


Stocks that aren't covered under CAS continue trading normally until **3:30 PM** .


Meanwhile, the **equity derivatives segment remains open until 3:40 PM** , giving futures and[options traders](https://algotest.in/blog/options-trading-a-comprehensive-guide-with-algorithmic-strategies) a few extra minutes to react after the official closing price is published.


## How Does the Closing Auction Session Work?


The Closing Auction Session takes place in four steps:


### 1. Order Collection


After continuous trading ends at **3:15 PM** , the exchange calculates the reference price. From **3:20 PM** , traders can place eligible auction orders.


During this stage:


-


Fresh[limit and market orders](https://algotest.in/blog/market-orders-banned-in-algo-trading-india) can be placed.


-


Unmatched limit orders from the regular trading session are carried forward into CAS.


-


[Stop-loss](https://algotest.in/blog/backtests-sl-mismatch) , IOC (Immediate-or-Cancel), and disclosed quantity (iceberg) orders are not allowed.


### 2. Equilibrium Price Calculation


The exchange compares all buy and sell orders to find the **equilibrium price** —the price at which the maximum number of shares can be matched while leaving as few unmatched orders as possible.


The equilibrium price must remain within a **±3% price band** around the reference price.


### 3. Order Matching


To prevent last-minute order manipulation, the order entry window closes at a random time during the final two minutes of the auction.


Between **3:30 PM and 3:35 PM** , all eligible orders are matched at the equilibrium price.


### 4. Official Closing Price


The **equilibrium price becomes the stock's official closing price** and is used for:


-


Stock F&O settlement


-


Index calculation


-


Mutual fund NAVs


-


End-of-day valuation and risk calculations


## How is the Closing Price Calculated?


The official closing price under CAS is the **equilibrium price** —the price at which the maximum number of buy and sell orders can be matched during the auction. It can be different from both the **reference price** and the **last traded price** .


Here's how it's calculated:


1.


The exchange calculates the **reference price** using the VWAP of trades between **3:00 PM and 3:15 PM** .


2.


A **±3% price band** is created around the reference price.


3.


During the auction, the exchange identifies the price where the highest number of shares can be matched.


4.


That price becomes the **official closing price** . If no valid equilibrium price is found, the reference price is used instead.


### Example


Suppose XYZ Ltd has a **reference price of ₹1,000** . The auction price band ranges from **₹970 to ₹1,030** .


During the auction:


-


Buy orders are concentrated between **₹1,003 and ₹1,008** .


-


Sell orders are concentrated between **₹995 and ₹1,005** .


The exchange finds that **₹1,006** allows the highest number of shares to be matched. Although the last traded price before 3:15 PM was **₹998** , **₹1,006** becomes the official closing price because it is the equilibrium price.


## How Does the Closing Auction Session Affect Options Traders?


CAS changes the **official closing price** of the underlying stock. While you continue trading options in the F&O segment as usual, this new closing price can influence settlement, end-of-day reporting, and expiry outcomes.


Trader


Impact


**Option Buyer**


No change to[intraday trading,](https://algotest.in/blog/best-intraday-trading-strategies-rules-tips) but the underlying's official closing price now comes from CAS.


**Option Seller**


Expiry settlement depends on the CAS closing price for stock options.


**Stock Futures & Options Trader**


Direct impact on expiry-day settlement.


**Index Option Trader**


Indirect impact through CAS-based closing prices of index constituents.


**Intraday Trader**


Earlier MIS square-off timings for eligible stocks.


**Algo Trader**


Closing-time execution logic may need changes.


Most retail traders don't need to change their trading style. CAS mainly affects expiry-day settlement, stock F&O traders, and strategies that execute near the market close.


If you use[AlgoTest](https://algotest.in/?utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_content=main-page&utm_term=algotest&utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_term=algotest&utm_content=main-page&utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_term=algotest&utm_content=main-page) to backtest or automate options strategies, review any closing-time logic and test your strategy under the new CAS framework before deploying it live.


## Should Options Traders Change Their Trading Strategy?


**Most retail traders don't need to change their strategy.** However, these traders should pay closer attention:


-


**Intraday traders:** Check your broker's revised MIS square-off timings, as eligible stocks now stop continuous trading at **3:15 PM** .


-


**Expiry-day traders:** Settlement is based on the **CAS closing price** , not the last traded price.


-


**Algo traders:** Review any closing-time automation, as **stop-loss, IOC, and iceberg orders** aren't allowed during CAS.


If you mainly trade index options and exit before market close, the impact is likely to be minimal. CAS matters most for stock F&O traders,[expiry-day strategies](https://algotest.in/blog/expiry-day-trading-expert-tips) , and[automated trading systems.](https://algotest.in/blog/fully-automated-trading-software-in-india)


## Does CAS Affect Nifty and Bank Nifty Options?


> **Not directly.** Nifty and Bank Nifty options don't participate in the Closing Auction Session.
>
>
> However, the closing value of these indices depends on the closing prices of their constituent stocks. Since eligible F&O stocks now close through CAS, index values—and therefore index option settlement—can be indirectly affected.


## Closing Auction Session vs Continuous Trading Session


Aspect


Closing Auction Session (CAS)


Continuous Trading Session (CTS)


**Timing**


3:15 PM – 3:35 PM (eligible F&O stocks)


9:15 AM – 3:30 PM (or 3:15 PM for CAS stocks)


**Order Execution**


Orders are collected and matched once at a single equilibrium price


Orders execute continuously throughout the session


**Price Discovery**


One equilibrium price


Continuous market-driven price changes


**Liquidity**


Concentrated in a 20-minute auction


Spread throughout the trading day


**Purpose**


Determines the official closing price


Facilitates regular market trading


**Order Types**


Limit and market orders only


All standard order types, including stop-loss and disclosed quantity


### **Test Your Strategies Before Trading Live**


The Closing Auction Session doesn't require most traders to change their approach, but it does change how the official closing price is determined for eligible stocks.


If your strategy relies on expiry-day settlement or closing-time execution, it's worth testing how it performs under the new framework.


With AlgoTest, you can:


-


Run your strategies through the[Options Simulator](https://algotest.in/feature/simulator) to analyze different market scenarios.


-


[Backtest](https://algotest.in/feature/backtest) expiry-day and closing-time strategies using historical data.


-


[Paper trade](https://algotest.in/feature/forward-test) updated strategies before deploying them in the live market.


### Additional Resources


[Product Documentation](https://docs.algotest.in/?utm_source=blog&utm_medium=organic&utm_campaign=seo&utm_source=blog&utm_medium=organic&utm_campaign=seo)


**Trading Tools**


-


[Margin Calculator](https://algotest.in/margin-calculator)


-


[IVR & IVP](https://algotest.in/feature/ivr-ivp)


-


[VRP Analysis](https://algotest.in/feature/vrp-analysis)


-


[OpenBroker](https://openbroker.in/)
