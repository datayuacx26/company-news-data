---
schema_version: "1.0.0"
document_id: "1ed047bafa807cd19cbce47464d92f37bb95179e297d787b1a2d8c803199de29"
company_key: "yc-algotest"
company: "AlgoTest"
source_id: "yc-algotest-rss-ebd738da757e"
canonical_url: "https://algotest.in/blog/covered-call-strategy/"
published_at: "2026-08-07T10:55:36+00:00"
first_seen_at: "2026-08-09T18:57:55.798384+00:00"
fetched_at: "2026-08-09T18:57:57.664591+00:00"
content_hash: "sha256:8ff06034a2cd99fdeef2d8fc6bca7b66fd265ea0a4afd96cc5963454913eb112"
---

# Covered Call Strategy: How to Use It with an Example

# Covered Call Strategy: How to Use It with an Example


[AlgoTest](https://algotest.in/blog/author/algotest/)


Aug 07, 2026


•


5 min read


•


[General](https://algotest.in/blog/category/general/)


•


[Markdown](https://algotest.in/blog/covered-call-strategy.md)


- [Twitter / X](https://twitter.com/intent/tweet?url=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fcovered%2Dcall%2Dstrategy%2F&text=Covered%20Call%20Strategy%3A%20How%20to%20Use%20It%20with%20an%20Example)
- [Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fcovered%2Dcall%2Dstrategy%2F)
- [LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fcovered%2Dcall%2Dstrategy%2F&title=Covered%20Call%20Strategy%3A%20How%20to%20Use%20It%20with%20an%20Example)
- [WhatsApp](https://api.whatsapp.com/send?text=Covered%20Call%20Strategy%3A%20How%20to%20Use%20It%20with%20an%20Example%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fcovered%2Dcall%2Dstrategy%2F)
- [Ask ChatGPT](https://chatgpt.com/?hints=search&q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fcovered%2Dcall%2Dstrategy%2F)
- [Ask Claude](https://claude.ai/new?q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fcovered%2Dcall%2Dstrategy%2F)
- [Ask Grok](https://grok.com/?q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fcovered%2Dcall%2Dstrategy%2F)
- Copy link


## What Is a Covered Call Strategy?


A covered call strategy is an options trading strategy where you own a stock and sell a call option on the same stock.


When you sell the call option, you receive a premium from the buyer. In return, you agree to sell your shares at a fixed price, called the strike price, if the buyer exercises the option before or on expiry.


Since you already own the shares, your position is "covered." This makes the strategy less risky than selling a call option without owning the underlying stock.


Most traders use a covered call strategy when they expect the stock price to remain stable or rise only slightly.


You may also hear a covered call strategy referred to as a **buy-write strategy** . The term "buy-write" is typically used when you buy the stock and sell the call option at the same time. The end position is the same as a covered call.


## How Does a Covered Call Strategy Work?


A covered call strategy has two parts.


First, you own the shares.


Second, you sell a call option against those shares.


Here's how it works.


-


Buy or already own the stock.


-


Sell a[call option](https://algotest.in/blog/call-writing-meaning) with your preferred strike price and expiry.


-


Receive the option premium immediately.


-


Wait until expiry.


After that, one of three things can happen.


-


If the stock stays below the strike price, the option expires worthless. You keep both your shares and the[premium.](https://algotest.in/blog/black-scholes-option-pricing-model)


-


If the stock moves above the strike price, your shares may be sold at the strike price. You still keep the premium.


-


If the stock falls, you continue to own the shares. The premium helps reduce your loss, but it does not eliminate it.


## Covered Call Strategy Example


Let's understand the strategy with a simple example.


Suppose you own 100 shares of ABC Ltd.


-


Buy Price = Rs. 1,000 per share


-


Current Market Price = Rs. 1,000


-


Strike Price = Rs. 1,050


-


Premium Received = Rs. 20 per share


-


Expiry = One month


Now let's see three possible outcomes.


Stock Price at Expiry


What Happens


Result


Rs. 1,020


Option expires worthless


You keep your shares and earn Rs. 2,000 premium


Rs. 1,080


Shares are sold at Rs. 1,050


You earn Rs. 5,000 from the price increase and Rs. 2,000 premium


Rs. 950


Option expires worthless


You lose value on the stock, but the Rs. 2,000 premium reduces the overall loss


This example shows why a covered call works best when the stock moves sideways or rises slightly.


Related:[How to Choose an Option Selling Strategy Based on Market Conditions](https://algotest.in/blog/option-selling-strategy/)


## When Should You Use a Covered Call Strategy?


A covered call is not suitable for every market condition.


You should consider using it when:


-


You already own the stock.


-


You expect the stock to remain sideways.


-


You expect only a small rise in price.


-


You want to generate additional income from your holdings.


You should avoid using it if:


-


You expect a strong rally.


-


The stock is highly volatile.


-


A major event like earnings is approaching.


In these situations, your upside may be limited because your shares could be sold at the strike price.


Popular read:[Butterfly Option Strategy: 4 Ways to Build the Same Payoff](https://algotest.in/blog/butterfly-option-strategy-call-put-iron-butterfly/)


## Advantages of a Covered Call Strategy


There are several reasons why traders use this strategy.


### 1. Earn Regular Premium Income


Every time you sell a call option, you receive a premium. This creates an additional source of income from stocks you already own.


### 2. Slight Downside Protection


The premium you receive reduces your overall cost of holding the stock. While it does not prevent losses, it offers a small cushion if prices fall.


### 3. Easy to Understand


Compared to many advanced[options strategies,](https://algotest.in/blog/options-trading-a-comprehensive-guide-with-algorithmic-strategies) a covered call is straightforward. This makes it suitable for beginners who already understand the basics of options trading.


### 4. Works Well in Sideways Markets


Many stocks spend weeks moving within a range. A covered call allows you to earn income even when prices do not move significantly.


## Risks and Limitations


Like every trading strategy, a covered call also has limitations.


### 1. Limited Profit


If the stock rises well above the strike price, your profit is capped. You must sell your shares at the agreed strike price.


### 2. Stock Price Risk


Owning the stock means you still face downside risk if the share price drops sharply.


### 3. Assignment Risk


If the option buyer exercises the contract, your shares may be sold even if you wanted to continue holding them.


Understanding these risks before trading helps you make better decisions.


Related:[Best Intraday Trading Strategies, Rules and Tips](https://algotest.in/blog/best-intraday-trading-strategies-rules-tips/)


## Common Mistakes Beginners Make


Many new traders lose money because they overlook simple details.


Avoid these common mistakes.


-


Selling a call with a strike price that is too close to the current market price.


-


Using the strategy during strong bullish markets.


-


Ignoring upcoming events like earnings announcements.


-


Chasing high premiums without understanding the risks.


-


Trading without testing the strategy first.


A little preparation can help you avoid costly mistakes.


Related: 5[Mistakes Traders Make in Algo Trading](https://algotest.in/blog/5-mistakes-traders-make-in-algo-trading)


## Can You Backtest a Covered Call Strategy?


Market conditions change over time. A covered call that performs well in one market may not work the same way in another.


That's why it's helpful to test your strategy before using real money.


With[AlgoTest](https://algotest.in/) , you can[backtest options strategies](https://algotest.in/blog/how-to-backtest-indicator-strategies-before-going-live/) using historical market data. You can compare different strike prices, expiries, and market conditions to understand how your covered call strategy would have performed in the past.


This gives you more confidence before taking a live trade.


Important read:[Trading Strategies that Don't Work](https://algotest.in/blog/types-of-strategies-that-do-not-work/)


## Build and Test Covered Call Strategies with AlgoTest


A covered call strategy works best when it is planned around the right strike price, expiry, and market outlook. Instead of relying on assumptions, it helps to test different combinations before trading with real capital.


**AlgoTest** , one of the[best options trading apps in India ,](https://algotest.in/) gives retail options traders access to institutional-grade tools to build, backtest, paper trade, and automate options strategies.


Whether you're testing a covered call, iron condor, straddle, or any other options strategy, you can analyze historical performance, compare different setups, and refine your approach before deploying it in live markets.


Start by backtesting your covered call strategy on[AlgoTest](https://algotest.in/) and see how it performs across different market conditions.


### **Additional Resources**


📚[Product Documentation](https://docs.algotest.in/?utm_source=blog&utm_medium=organic&utm_campaign=seo&utm_source=blog&utm_medium=organic&utm_campaign=seo)


**Trading Tools**


-


[Margin Calculator](https://algotest.in/margin-calculator)


-


[IVR & IVP](https://algotest.in/feature/ivr-ivp)


-


[VRP Analysis](https://algotest.in/feature/vrp-analysis)


-


[OpenBroker](https://openbroker.in/)


**Disclaimer:** *This article is for educational purposes only and does not constitute investment or trading advice. Options trading involves risk, and past performance or examples do not guarantee future results. Please evaluate your financial situation and consult a qualified financial advisor if needed before trading.*
