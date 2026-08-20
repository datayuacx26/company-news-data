---
schema_version: "1.0.0"
document_id: "eb878ecb9dae77b1c00889f574c3b16651ead049da105cbbff13fec8906181a1"
company_key: "yc-algotest"
company: "AlgoTest"
source_id: "yc-algotest-rss-ebd738da757e"
canonical_url: "https://algotest.in/blog/iron-condor-strategy/"
published_at: "2026-07-22T07:52:52+00:00"
first_seen_at: "2026-08-09T18:57:55.798384+00:00"
fetched_at: "2026-08-09T18:57:57.664591+00:00"
content_hash: "sha256:126e3c838f272881c04139283321ad034280296796c89529540e850599bc8662"
---

# Iron Condor Strategy Explained: How It Works, Payoff & Example

# Iron Condor Strategy Explained: How It Works, Payoff & Example


[AlgoTest](https://algotest.in/blog/author/algotest/)


Jul 22, 2026


•


7 min read


•


[Strategies](https://algotest.in/blog/category/strategies/)


•


[Markdown](https://algotest.in/blog/iron-condor-strategy.md)


- [Twitter / X](https://twitter.com/intent/tweet?url=https%3A%2F%2Falgotest%2Ein%2Fblog%2Firon%2Dcondor%2Dstrategy%2F&text=Iron%20Condor%20Strategy%20Explained%3A%20How%20It%20Works%2C%20Payoff%20%26%20Example)
- [Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Falgotest%2Ein%2Fblog%2Firon%2Dcondor%2Dstrategy%2F)
- [LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Falgotest%2Ein%2Fblog%2Firon%2Dcondor%2Dstrategy%2F&title=Iron%20Condor%20Strategy%20Explained%3A%20How%20It%20Works%2C%20Payoff%20%26%20Example)
- [WhatsApp](https://api.whatsapp.com/send?text=Iron%20Condor%20Strategy%20Explained%3A%20How%20It%20Works%2C%20Payoff%20%26%20Example%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Firon%2Dcondor%2Dstrategy%2F)
- [Ask ChatGPT](https://chatgpt.com/?hints=search&q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Firon%2Dcondor%2Dstrategy%2F)
- [Ask Claude](https://claude.ai/new?q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Firon%2Dcondor%2Dstrategy%2F)
- [Ask Grok](https://grok.com/?q=Summarize%20this%20article:%20https%3A%2F%2Falgotest%2Ein%2Fblog%2Firon%2Dcondor%2Dstrategy%2F)
- Copy link


## What Is an Iron Condor Strategy?


An iron condor strategy is a four-leg, market-neutral options strategy that combines a bear call spread and a bull put spread on the same underlying and expiry.


It profits when the underlying stays within a defined price range until expiry, making it a popular choice for traders expecting sideways market movement.


The strategy is opened for a net credit by selling one out-of-the-money (OTM) call and one OTM put, while buying a further OTM call and put to cap risk on both sides. The credit received is the maximum possible profit, which you keep if the underlying closes between the two short strikes at expiry.


This version, where the nearer strikes are sold and the farther strikes are bought for protection, is known as the short iron condor and is the variation most options traders use. (A long iron condor is a less common debit strategy built in the opposite direction.)


Let's see how the iron condor strategy works, its payoff, when to use it, how to choose strikes, and how to build and backtest one on[AlgoTest.](https://algotest.in/)


## How the Iron Condor Is Structured


-


Sell one OTM call above the current price to collect premium on the upside.


-


Buy one further OTM call above your short call to cap your upside risk.


-


Sell one OTM put below the current price to collect premium on the downside.


-


Buy one further OTM put below your short put to cap your downside risk.


-


All four legs share the same expiry date, and the position is usually opened as a single combined order rather than four separate trades.


You can learn more about[ATM, ITM, OTM here.](https://docs.algotest.in/financial-education/options/moneyness/#itm-atm-otm-explained)


## Iron Condor Payoff


The payoff on an iron condor is defined and capped on both sides, and you can work it out before you ever place the trade.


**Maximum Profit:** Net premium (credit) received when you open the position. You keep this in full if the underlying closes between your two short strikes at expiry.


**Maximum Loss:** Width of either spread minus the net premium received. Both spreads are usually kept equal in width, so this figure holds on either side.


**Breakeven Points**


-


Upper breakeven: Short call strike + net credit


-


Lower breakeven: Short put strike − net credit


## Iron Condor Example on Nifty


> **Example: Nifty at 24,500, weekly expiry**
>
>
> -
>
>
> Sell the 24,700 CE and the 24,300 PE
>
>
> -
>
>
> Buy the 24,900 CE and the 24,100 PE
>
>
> -
>
>
> Net credit received: Rs. 45 per share
>
>
> With a Nifty lot size of 65:
>
>
> -
>
>
> **Maximum profit:** Rs. 2,925 (45 x 65), if Nifty closes between 24,300 and 24,700
>
>
> -
>
>
> **Maximum loss:** Rs. 10,075 (155 x 65), if Nifty closes beyond 24,100 or 24,900
>
>
> -
>
>
> **Upper breakeven:** 24,745 | **Lower breakeven:** 24,255
>
>
> Numbers are illustrative, not a trade recommendation.


## When to Use an Iron Condor


-


The underlying has been trading in a well-defined range with no clear trend.


-


You expect the market to stay range-bound rather than make a strong directional move, since that's what determines whether your short strikes hold, not the VIX level on its own.


-


Many traders prefer opening an iron condor when implied volatility is relatively high compared to its recent levels, since that means richer premiums for the same strike distance. Low IV can still work, but it also means less credit for the risk you're taking on.


-


No major scheduled events fall within the trade's holding period, such as an RBI policy announcement, the Union Budget, or a heavyweight earnings release.


-


You want defined, capped risk instead of the open-ended exposure that comes with naked option selling.


Related:[Option Trading Strategies](https://algotest.in/blog/options-trading-strategies)


## When Not to Use an Iron Condor


-


A major scheduled event, such as RBI policy, the Union Budget, or a big earnings release, falls inside your holding period.


-


Implied volatility is expected to expand sharply rather than stay flat or contract.


-


The underlying is trending strongly in one direction rather than moving sideways.


-


A breakout is already underway by the time you're looking to enter.


-


The options you'd need to trade have poor liquidity, which widens your entry and exit costs on all four legs.


Related:[Mistakes Traders make in Algo trading](https://algotest.in/blog/5-mistakes-traders-make-in-algo-trading)


## Strike Selection and Risk Management


-


Many traders anchor their short strikes using delta as a proxy for probability, commonly placing the short call and put in the 20 to 30 delta range so each side has a reasonable statistical chance of expiring worthless.


-


Some traders use the expected move, nearby support and resistance levels, or[implied volatility](https://algotest.in/blog/implied-volatility-and-how-to-read-and-compute-iv) instead of delta alone when picking strikes, especially around events where delta-based probabilities can be misleading.


-


Wider wings (the distance between your short and long strikes) increase your credit and your maximum loss together, so widen them in proportion to how much risk you're comfortable holding, not just to chase a bigger premium.


-


Keep your two spreads symmetric unless you have a mild directional view, since a heavily skewed iron condor starts to behave more like a single credit spread with extra legs.


-


Size the position against the margin it blocks, not just the premium it pays, so you have room left to adjust if the market moves against one side.


## Adjusting an Iron Condor Position


-


Roll the untested side closer to the current price to collect additional credit if one side of the range is clearly safe and the other is under pressure.


-


Close the position early once you've captured a large share of the maximum profit, since the remaining credit often isn't worth the residual risk of holding to expiry.


-


Exit or reduce the threatened side if the underlying approaches your short strike well before expiry, rather than waiting for the breakeven point to be tested.


-


Avoid adjusting reactively on every small move. Decide your adjustment triggers before you enter the trade so you're not making risk decisions in the middle of a fast market.


## Iron Condor vs Iron Butterfly


An iron condor sells out-of-the-money strikes, which gives it a wider profit range but a smaller credit. An iron butterfly (also called an iron fly) sells at-the-money strikes instead, which means a narrower profit zone but a larger credit for the same risk taken.


Feature


Iron Condor


Iron Butterfly


Short strikes


OTM


ATM


Credit received


Lower


Higher


Profit range


Wider


Narrower


Risk


Defined


Defined


Best suited for


Mildly range-bound markets


Tightly range-bound markets


In practice, traders reach for an iron condor when they expect the underlying to stay within a broader range, and an iron butterfly when they have a more precise view on where it will settle.


If you want the full breakdown of strike selection and payoff mechanics for the butterfly side,[AlgoTest's butterfly option strategy guide](https://algotest.in/blog/butterfly-option-strategy-call-put-iron-butterfly) covers that in detail.


## Common Mistakes to Avoid


-


Setting your range too tight relative to the underlying's realistic movement, which increases the odds of a breach before expiry.


-


Ignoring scheduled events like RBI policy days, Budget day, or major earnings that fall inside your holding period.


-


Entering the trade without a predefined adjustment plan, which tends to turn small losses into large ones.


-


Oversizing the position relative to available capital, leaving no room to adjust the threatened side if the market moves.


-


Holding an untouched position all the way into expiry day without accounting for how fast gamma risk builds up in the final session.


## How to Build and Backtest an Iron Condor with AlgoTest


Constructing an iron condor by hand across four legs leaves room for error, especially when you're trying to keep both spreads symmetric and track combined margin in real time.


[AlgoTest's Strategy Builder](https://algotest.in/feature/strategy-builder) lets you add all four legs from the option chain, see the combined payoff graph, Greeks, and margin requirement as you build, and adjust strikes before you commit capital.


Pre-built Iron Condor template in AlgoTest Strategy Builder


Once the structure is set, you can backtest this iron condor trading strategy against historical data to see how it would have performed across different market conditions, then paper trade it before going live. For a step-by-step walkthrough of building a monthly iron condor in the Strategy Builder, see[How to Use AlgoTest Strategy Builder for a Monthly Iron Condor](https://algotest.in/blog/how-to-use-strategy-builder-iron-condor/) .


If you trade shorter timeframes,[Intraday Iron Condor Built Using Straddle Width](https://algotest.in/blog/intraday-iron-condor-built-using-straddle-width/) shows how to size your wings using the day's straddle price instead of fixed points.


## Trade Iron Condors With More Confidence


An iron condor strategy can be an effective way to generate income in range-bound markets while keeping risk defined.


Success, however, depends less on the strategy itself and more on choosing the right strikes,[sizing positions](https://algotest.in/blog/why-im-continuing-my-failed-volatility-trading-community) appropriately, and having a clear exit or adjustment plan.


Before trading with real capital, test your setup using historical data and[paper trading.](https://algotest.in/paper-trading)


With AlgoTest's Strategy Builder, you can build, backtest, and refine your iron condor strategy to evaluate different strike combinations and adjustment rules before taking it live.


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
