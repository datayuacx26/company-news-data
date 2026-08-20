---
schema_version: "1.0.0"
document_id: "ed44038ffe73042326fec5a2bd58897b7f6b809064f0e0ad35f8a44688c6789e"
company_key: "yc-cointracker"
company: "CoinTracker"
source_id: "yc-cointracker-news-import-8eddc0d2ffc8"
canonical_url: "https://www.cointracker.com/blog/what-is-an-oco-order"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-25T11:57:28.919777+00:00"
fetched_at: "2026-08-02T22:35:48.219885+00:00"
content_hash: "sha256:e78d33f9c15bad49a138e6c3fccd8e60025e75a0a1ac88bfb904b86afe7022ec"
---

# What is an OCO order? How it works in crypto trading

Managing risk in crypto trading often means deciding in advance what you will do if a trade goes right and what you will do if it goes wrong. Doing that manually, while watching price levels shift in a volatile market, is stressful and easy to get wrong.


An OCO order solves that problem by letting you set two linked trades at once. One executes when a price target is hit. The other is automatically canceled. OCO orders are commonly used in crypto trading to automate buy and sell decisions at predefined price levels, so you do not have to monitor every move. For background on the broader mechanics of[spot trading](https://www.cointracker.io/blog/spot-trading) , that guide covers how crypto trades are executed on exchanges.


## **What is an OCO order? Explaining OCO in trading**


OCO stands for "one cancels the other." An OCO order is a pair of linked orders where executing one automatically cancels the other. Both orders are active at the same time, but only one can fill.


Key aspects of an OCO order:


- **Stop order.** One leg of the OCO is typically a stop order, which triggers a sell if the price falls to a set level, limiting downside.
- **Limit order.** The other leg is usually a sell limit or buy limit order, which executes at a target price to capture a gain.
- **Trigger conditions.** Both orders watch for their price levels simultaneously. The moment one condition is met and the order fills, the platform automatically cancels the other.


In practice, an OCO order lets you define two outcomes at the start of a trade: a best case and a worst case. Whichever happens first, the other order disappears. This reduces the risk of accidentally having both orders execute if prices move quickly.


## **How does an OCO order work?**


OCO orders automate your exit strategy by watching two price levels at once. Here is a typical flow for a[crypto trade](https://www.cointracker.io/blog/spot-trading) :


1. **Enter a position.** You buy an asset, for example Bitcoin at $60,000.
2. **Set a take-profit limit.** You place a sell limit order at $65,000 to capture your target gain if the price rises to that level.
3. **Set a stop-loss trigger.** You place a stop order at $57,000. If the price falls to that level, your position is sold to limit your loss.
4. **Confirm order details.** Both orders are linked as an OCO pair and submitted to the exchange. Both are active immediately.
5. **Monitor execution conditions.** If Bitcoin rises to $65,000, the sell limit fills and the stop order at $57,000 is automatically canceled. If Bitcoin falls to $57,000 first, the stop order fills and the sell limit is canceled.


This setup automates your exits and reduces the need for manual monitoring. Instead of watching charts and deciding when to act, the OCO order handles both scenarios in advance, which is especially useful in volatile markets where prices can move quickly.


## **What are common types of OCO orders?**


OCO orders can be adapted to different strategies depending on market conditions and trading goals. Here are the most common types.


### **Take-profit and stop-loss OCO orders**


The most common OCO setup. One order targets an upside price level to lock in a gain (take-profit), while the other sets a floor to limit loss if the trade moves against you (stop-loss). These two orders run simultaneously, and whichever price is hit first determines which one executes.


### **Stop-limit OCO orders**


A stop-limit OCO uses a stop-limit order as one leg instead of a simple stop order. When the stop price is reached, it triggers a limit order rather than a market sell. This gives you more control over the price at which the stop leg executes, though in fast-moving markets, a limit order may not always fill.


### **Breakout OCO order**


A breakout OCO is placed around a price range where neither direction is clear. You set a buy order above resistance and a sell order below support. If the price breaks out in either direction, the relevant order executes and the other is canceled. This strategy is useful when a volatile asset is consolidating and you want to capture the move whichever way it goes.


### **Trailing stop OCO order**


A trailing stop OCO uses a trailing stop as one leg, which adjusts automatically as the price moves in your favor. If the price rises, the stop trails upward. If the price reverses by a set amount, the trailing stop triggers. The other leg of the OCO, typically a take-profit limit, cancels if the trailing stop fires first.


### **Entry order/stop loss OCO order**


An entry order/stop loss OCO is used to enter a position and set a protective stop-loss at the same time. One leg places a buy limit or buy stop order at your intended entry price level. The other leg sets a stop order below entry to exit the trade immediately if it moves against you. This approach is useful when[ladder trading](https://www.cointracker.io/blog/ladder-trading) across multiple price levels, as it automates both the entry and the risk management in a single order pair.


### **"Good 'til canceled" vs. day OCO orders**


OCO orders can be set with different time-in-force conditions. A "good 'til canceled" (GTC) OCO stays active until one leg executes or you manually cancel it, which can be days or weeks. A day OCO expires at the end of the trading session if neither leg has filled. GTC is more common for longer-term strategies, while day orders suit traders who want to reset their levels each session.


## **What are the risks of using OCO orders?**


OCO orders improve automation and reduce the need for constant monitoring, but they also introduce risks that are worth planning around.


- **Risk-to-reward planning.** Setting your take-profit and stop-loss levels without a clear risk-to-reward ratio can lead to trades where potential losses outweigh potential gains. Each OCO pair should reflect a deliberate assessment of both sides before you place it.
- **Position sizing alignment.** If your position size is too large relative to your stop-loss distance, a single triggered stop order can result in a loss that is disproportionate to your account. OCO orders execute the plan you set, so the plan needs to account for position sizing before the order is placed.
- **Combining with technical indicators.** Placing OCO orders without reference to support, resistance, or other technical indicators can result in stop orders being triggered by normal price fluctuation rather than a genuine reversal. Price levels chosen without context are more likely to fire at the wrong time.


Volatile markets can also trigger unintended outcomes. A sharp price spike or flash crash can hit a stop-loss level and cancel the take-profit order before the price recovers, locking in a loss on a trade that would otherwise have been profitable. This is a known risk of automated order types and worth considering when setting price levels in highly volatile conditions.


## **OCO vs. OTO orders**


OCO and OTO are both conditional order types, but they work differently.


- **OCO (one cancels the other).** Two orders are active at the same time. When one executes, the other is automatically canceled. Used to manage both upside and downside simultaneously.
- **OTO (one triggers the other).** The first order is active. When it executes, it activates the second order. Used to set up a follow-on action that only makes sense after the first order fills, such as placing a stop-loss only after an entry order has been confirmed.


The key difference is sequencing. OCO runs two orders in parallel. OTO runs them in sequence. If you want to protect an existing position from both directions at once, OCO is the right tool. If you want to set up a conditional chain of trades, OTO is more appropriate.


## **When to consider placing an OCO order in crypto**


OCO orders are a practical tool for traders who want to automate their risk management without watching screens constantly. Consider using one when:


- You have a clear entry point and want to define both your exit target and your maximum loss before the trade starts.
- You are trading in a[volatile](https://www.cointracker.io/blog/spot-trading) market where prices can move quickly and manual reactions may be too slow.
- You want to lock in a gain if the price hits your target, without having to cancel your stop manually.
- You are stepping away from active monitoring and need both your upside and downside scenarios covered.
- You are managing a more complex strategy, such as[ladder trading](https://www.cointracker.io/blog/ladder-trading) across multiple price levels, where manual order management becomes error-prone.


OCO orders are not a guarantee of outcome. In fast-moving markets, slippage can mean a stop order executes at a price different from the one you set. But for structured risk management, they are a useful tool in any crypto trader's approach.


## **What is an OCO policy in trading?**


An OCO policy refers to how a trading platform handles linked orders and cancellations when one order in the pair executes. The general rule is consistent: when one leg fills, the platform cancels the other. But the specifics can vary.


Some platforms cancel the second order immediately when the first executes. Others may have a short processing window. Partial fills can also affect behavior: if one leg fills only partially, different platforms handle the remaining linked order differently, with some canceling it and others leaving it active.


If you are placing OCO orders on an exchange for the first time, it is worth reviewing that platform's documentation to understand exactly how it handles edge cases. Policies may vary across exchanges, and knowing the rules in advance can prevent unexpected outcomes in volatile conditions.


## **Keep your crypto trades record in one place on CoinTracker**


Every OCO order that executes is a transaction that may have tax implications. Whether your take-profit fires or your stop-loss triggers, that trade is part of your crypto record for the year.[CoinTracker](https://www.cointracker.io/) brings your transactions together across exchanges and wallets so you can review your gains, losses, and cost basis in one place.


If you trade on margin or use more advanced order types, the[CoinTracker guide to taxes on margin trading](https://www.cointracker.io/blog/taxes-on-margin-trading-ultimate-guide) covers how those transactions may be treated for tax purposes.


Add your wallets and exchanges to CoinTracker to keep your crypto trade records organized and be ready when tax season arrives.


**Disclaimer:** This post is informational only and is not intended as tax advice. For tax advice, please consult a tax professional.
