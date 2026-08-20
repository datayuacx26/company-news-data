---
schema_version: "1.0.0"
document_id: "44e3fc17a86b736e9a47889abba6fd5e4a018a44cf45b2674c0493bef8100e49"
company_key: "yc-algotest"
company: "AlgoTest"
source_id: "yc-algotest-rss-ebd738da757e"
canonical_url: "https://algotest.in/blog/itm-atm-otm/"
published_at: "2026-07-24T10:11:27+00:00"
first_seen_at: "2026-08-09T18:57:55.798384+00:00"
fetched_at: "2026-08-09T18:57:57.664591+00:00"
content_hash: "sha256:b91e2de79a8e43c9ba4f917553145cc2020e9931855f19e76da1123f8f9fc5a4"
---

# ITM vs ATM vs OTM Options: How to Choose the Right Strike Price

# ITM vs ATM vs OTM Options: How to Choose the Right Strike Price


[AlgoTest](https://algotest.in/blog/author/algotest/)


Jul 24, 2026


•


6 min read


•


[General](https://algotest.in/blog/category/general/)


•


[Markdown](https://algotest.in/blog/itm-atm-otm.md)


- [Twitter / X](https://twitter.com/intent/tweet?url=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fitm%2Datm%2Dotm%2F&text=ITM%20vs%20ATM%20vs%20OTM%20Options%3A%20How%20to%20Choose%20the%20Right%20Strike%20Price)
- [Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fitm%2Datm%2Dotm%2F)
- [LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Falgotest%2Ein%2Fblog%2Fitm%2Datm%2Dotm%2F&title=ITM%20vs%20ATM%20vs%20OTM%20Options%3A%20How%20to%20Choose%20the%20Right%20Strike%20Price)
- [WhatsApp](https://api.whatsapp.com/send?text=ITM%20vs%20ATM%20vs%20OTM%20Options%3A%20How%20to%20Choose%20the%20Right%20Strike%20Price%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fitm%2Datm%2Dotm%2F)
- [Ask ChatGPT](https://chatgpt.com/?hints=search&q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fitm%2Datm%2Dotm%2F)
- [Ask Claude](https://claude.ai/new?q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fitm%2Datm%2Dotm%2F)
- [Ask Grok](https://grok.com/?q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Fitm%2Datm%2Dotm%2F)
- Copy link


## What Do ITM ATM OTM Mean in Options Trading?


This is what traders call option moneyness. It simply describes where a strike price sits compared to the current market price of the underlying.


-


**In the Money (ITM)** options already carry real value if you exercised them today. A call is ITM when its strike is below the spot price. A put is ITM when its strike is above the spot price.


-


**At the Money (ATM)** options have a strike price at or very close to the spot price. They carry no intrinsic value yet, only time value.


-


**Out of the Money (OTM)** options would be worth nothing if exercised today. A call is OTM when its strike is above the spot price. A put is OTM when its strike is below the spot price.


Moneyness isn't fixed. As Nifty moves through the day, a strike that was OTM in the morning can turn ITM by the afternoon, and vice versa.


If you're still getting comfortable with the basics of calls and puts, our[Options Trading Guide](https://algotest.in/blog/options-trading-a-comprehensive-guide-with-algorithmic-strategies/) is worth reading before you go further into strike selection.


## ITM, ATM & OTM Explained Using Nifty


Let's use one example through this guide: Nifty is trading at 25,000.


### Call Options


Strike


Status


24,800


ITM


25,000


ATM


25,200


OTM


A call option gives you the right to buy Nifty at the strike price. The 24,800 call lets you buy at 24,800 when the market is already at 25,000, so it carries ₹200 of built-in value. That makes it ITM.


The 25,200 call would let you buy at a price above where Nifty is trading, so nobody would exercise it today. That makes it OTM.


### Put Options


Strike


Status


25,200


ITM


25,000


ATM


24,800


OTM


Puts work in reverse. A put gives you the right to sell Nifty at the strike price, so the 25,200 put lets you sell above the current market price and carries ₹200 of value. That makes it ITM.


The 24,800 put would let you sell below where Nifty is trading, so it's OTM instead.


## ITM vs ATM vs OTM: What's the Difference?


Here's how the three compare across the factors that actually matter when picking a strike.


Factor


ITM


ATM


OTM


Premium


Highest


Moderate


Lowest


Intrinsic Value


Yes


None


None


Time Value


Smaller share of premium


Highest


Small, and it's the entire premium


Delta


High (roughly 0.6 to 0.9 for calls)


Near 0.5


Low (below 0.4)


Probability of Expiring ITM


Higher


Roughly even


Lower


Capital Required (buying)


Highest


Moderate


Lowest


Risk


Costs more upfront, moves closer to the underlying


Balanced, but Theta works fastest against it


Cheapest, but expires worthless more often


Suitable For


Conservative directional trades, hedging


Directional bets, straddles, volatility strategies


Premium selling, credit spreads, low-cost speculation


The Delta and probability figures above are general ranges. They shift with[volatility,](https://algotest.in/blog/implied-volatility-and-how-to-read-and-compute-iv) time to expiry, and how far the strike is from spot.


## Which Strike Price Should You Choose?


There's no single "best" strike. The right one depends on what you're trying to achieve with the trade.


**ITM options make sense when you want:**


-


A higher Delta, so the option's price moves closer to point-for-point with Nifty.


-


Less drag from time decay, since more of the premium is intrinsic value rather than time value.


-


A conservative directional trade, where you're fairly confident about the move but want the option to behave more like the underlying itself.


**ATM options make sense when you want:**


-


A balance between what you pay and how sensitive the option is to price moves.


-


A straightforward directional trade without paying the higher cost of an ITM strike.


-


To build strategies centred on the ATM strike, like a[Long Straddle](https://docs.algotest.in/financial-education/options-strategies/long-straddle/) or an Iron Fly, since that's where time value is highest.


**OTM options make sense when you want:**


-


The lowest possible premium outlay for a directional bet.


-


To sell premium in strategies like an[Iron Condor](https://docs.algotest.in/financial-education/options-strategies/iron-condor/) or a short strangle, where you collect money for taking on defined risk.


-


To write a covered call against shares you already hold, leaving room for the stock to move before you're assigned.


The trade-off is simple. ITM costs more but behaves predictably. OTM costs less but needs a bigger move to pay off. ATM sits in between, and it's where most of an option's time value lives.


Related:[How to Use Option Greeks in Trading: A Practical Guide for Options Traders](https://algotest.in/blog/option-greeks/)


## How Strike Selection Changes Your Trade


Say Nifty is at 25,000 and you're bullish. Here's how buying a call at each strike might play out, using illustrative premiums for a near-week expiry.


ITM (24,800 CE)


ATM (25,000 CE)


OTM (25,200 CE)


Illustrative Premium


₹250


₹120


₹50


Break-even at Expiry


25,050


25,120


25,250


Delta


~0.75


~0.50


~0.30


Capital Outlay


Highest


Moderate


Lowest


If Nifty Closes at 25,300


Solid gain, moves closely with spot


Bigger percentage gain, but needed the move to happen


Biggest percentage gain, but only past its farther breakeven


This is where the "cheap OTM option" trap shows up. The OTM call looks like a bargain at ₹50, but Nifty needs to close above 25,250 for you to break even, a bigger move than the other two strikes need. The ITM call costs five times as much but only needs Nifty to hold above 25,050.


Neither is wrong on its own. It depends on how strongly you expect Nifty to move and how much capital you're willing to risk to back that view.


You can check this kind of comparison instantly using the[Option Simulator](https://algotest.in/blog/best-options-simulator-india) , which shows payoff and Greeks for a single strike before you commit capital.


## ITM, ATM & OTM in Popular Option Strategies


Most option strategies are really just ITM, ATM, and OTM strikes combined in different ways.


Strategy


Typical Strike


Long Call


ATM / ITM


Long Put


ATM / ITM


Covered Call


OTM


[Iron Condor](https://algotest.in/blog/how-to-use-strategy-builder-iron-condor)


OTM


[Iron Butterfly](https://algotest.in/blog/butterfly-option-strategy-call-put-iron-butterfly)


ATM


Short Strangle


OTM


[Long Straddle](https://docs.algotest.in/financial-education/options-strategies/long-straddle/)


ATM


Directional buyers tend to lean ITM or ATM because they want Delta working in their favour. Premium sellers tend to lean OTM because they're collecting money for risk that's less likely to materialise.


ATM shows up in the Long Straddle and Iron Butterfly because that's where time value, and therefore Theta and Vega, are highest.


## Why Experienced Traders Compare Multiple Strikes Before Trading


Most retail traders pick a strike, check the premium, and place the trade. Experienced traders add one extra step first: they compare how the same strategy behaves across different strikes before committing capital.


Shifting a strategy from ITM to ATM to OTM doesn't just change the premium. It changes the payoff shape, the[option Greeks](https://docs.algotest.in/financial-education/options/greek-interactions/) like Delta, Theta and Vega, the probability of profit, the margin required, and the breakeven points.


This is exactly the kind of comparison AlgoTest's[Strategy Builder](https://algotest.in/blog/option-strategy-builder-algotest-tutorial/) is built for. You can build the same strategy using ITM, ATM and OTM strikes side by side, and instantly see how the payoff graph, Greeks and margin requirement shift between them.


From there, you can[backtest](https://algotest.in/blog/how-to-backtest-options-trading-strategies-with-examples/) each variation on historical data and[paper trade](https://algotest.in/paper-trading) it before putting real capital on the line.


## Common Mistakes Traders Make


-


Buying cheap OTM options because they look affordable, without checking how far the market needs to move to reach breakeven.


-


Ignoring Delta and looking only at the premium, which hides how much the option will actually move with Nifty.


-


Underestimating time decay, especially on ATM and OTM options bought close to expiry.


-


Confusing ITM with ATM, particularly when the spot price is hovering right around a strike.


-


Selecting a strike without considering what the strategy actually needs, since a straddle and a credit spread call for very different strikes.


-


Looking only at premium instead of the full picture: Delta, probability, margin and payoff together.


## Pick Your Strike With a Plan, Not a Guess


ITM, ATM, and OTM aren't just labels on the option chain. The strike price you choose affects your premium, risk, probability of profit, and how your strategy behaves as the market moves.


Skipping this step is how traders end up in positions that don't match what they expected.


Before placing a trade, it's worth comparing how ITM, ATM, and OTM strikes change your strategy's payoff, Greeks, and margin. With AlgoTest's[Strategy Builder](https://algotest.in/feature/strategy-builder) , you can build the same strategy across different strikes,[backtest](https://algotest.in/feature/backtest) each variation on historical data, and[forward test](https://algotest.in/feature/forward-test) it before risking real capital.


## **Join Us as We Simplify Algo Trading in India**


[AlgoTest](https://algotest.in/?utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_content=main-page&utm_term=algotest&utm_source=blog&utm_medium=internal&utm_campaign=seo&utm_term=algotest&utm_content=main-page) is built to take the guesswork out of options trading — from your first backtest to a fully automated strategy. Whether it's a calendar spread or a more complex multi-leg setup, you can test it, refine it, and deploy it, all without writing a single line of code.


[Sign up for free](https://algotest.in/register?utm_source=blogs&utm_medium=organics&utm_campaign=seo&utm_source=blogs&utm_medium=organics&utm_campaign=seo) and get 25 free backtests every week — join thousands of retail traders across India already using AlgoTest to trade smarter.


### **Additional Resources**


📚[Product Documentation](https://docs.algotest.in/?utm_source=blog&utm_medium=organic&utm_campaign=seo&utm_source=blog&utm_medium=organic&utm_campaign=seo)


**🛠️ Trading Tools**


-


[Margin Calculator](https://algotest.in/margin-calculator)


-


[IVR & IVP](https://algotest.in/feature/ivr-ivp)


-


[VRP Analysis](https://algotest.in/feature/vrp-analysis)


-


[OpenBroker](https://openbroker.in/)
