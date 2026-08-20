---
schema_version: "1.0.0"
document_id: "16e413f8eb9666b67fbba03b2cd95241f2a3d4e59718a07255cbb83371815deb"
company_key: "yc-coinrule"
company: "Coinrule"
source_id: "yc-coinrule-rss-98629e270b11"
canonical_url: "https://coinrule.com/blog/crypto-automated-trading/coinrule-profit-binance-btc/"
published_at: "2025-12-11T17:21:29+00:00"
first_seen_at: "2026-07-20T23:23:45.191933+00:00"
fetched_at: "2026-07-28T20:55:05.658267+00:00"
content_hash: "sha256:3c91f3082a99dd3b47da47380afb6465271d8015bd7bfb9ff8f30a1145bf9bef"
---

# The Best Indicators to Use in Your Crypto Strategy

##


## **Setting Up the Trading Strategy on TradingView**


Welcome back to the channel. In today’s guide, we’re going through the full process of creating a profitable automated crypto trading bot using CoinRule, powered by TradingView indicators. This walkthrough will take you from chart setup to automated execution so your strategy can trade for you 24/7 without any coding.


The first step is preparing the chart on TradingView. Load the trading pair you want to automate; for this example, BTC/USDT on the one-hour timeframe works well because it filters a lot of noise while still capturing strong market moves. Add the RSI indicator and set it to the standard 14-period input. Then add a moving average cross, with the fast MA set to 9 and the slow MA set to 50. The goal is to identify moments when momentum and trend align. A typical setup is to enter a long trade when RSI pushes above the mid-line around 50 while the MA9 crosses above the MA50, signalling trend strength. Exits are triggered when RSI falls back below the mid-line or when the moving averages cross the opposite way. This combination often reduces false signals and gives smoother entries.


#### **Connecting TradingView Alerts to CoinRule**


Once the indicators are ready, open the alerts panel in TradingView. Create one alert for the bullish MA cross, which will serve as the buy trigger, and type the simple message “BUY” into the alert text. Then create a second alert for the bearish cross, using “SELL” as the message. In CoinRule, start a new rule and choose TradingView Signal as your trigger type. CoinRule will generate a webhook URL and the exact alert message format required. Copy that information back into TradingView so the alerts can send instructions directly to your bot. Whenever your TradingView indicators detect the right conditions, the alert fires automatically, and CoinRule executes the order according to your rule.


#### **Building the Automated Rule in CoinRule**


Inside CoinRule, configure the buy action so that when the system receives a BUY signal, it enters the market using a percentage of your chosen wallet. You can adjust this allocation depending on how aggressive you want the strategy to be. For the exit, set the rule so that when a SELL message comes through, the bot closes the entire position. This keeps the logic simple and avoids overlapping trades. Risk management can also be added through trailing take-profit and trailing stop-loss features. These help lock in gains during strong moves while automatically protecting you from sharp reversals without needing constant manual oversight.


#### **Testing and Going Live**


Before running the bot with real funds, it’s best to test everything on the CoinRule Demo Exchange. This allows you to confirm that the alerts trigger correctly, the trades open and close as intended, and the overall strategy behaves the way you expect. After a few successful tests, switch the rule to your live exchange. CoinRule will continue operating autonomously, reacting instantly to the signals you designed in TradingView.


#### **Improving Performance Over Time**


This type of strategy usually produces steady, consistent trades by focusing on momentum and confirmed trend shifts. If you prefer faster trading and more signals, you can switch to a shorter timeframe like the 15-minute chart. If you want fewer but more reliable setups, the four-hour chart may deliver cleaner trends.As market conditions change, you can always fine-tune the entry criteria, adjust risk management, or test variations without touching a single line of code. TradingView handles the analysis, and CoinRule handles the execution.


[https://www.youtube.com/watch?v=09w6ROLOrJQ](https://www.youtube.com/watch?v=09w6ROLOrJQ)


Download the Crypto trading bot on[IOS](https://apps.apple.com/us/app/coinrule-crypto-trading-bot/id1667293808) , and on the[Google Store](https://play.google.com/store/apps/details?id=com.coinrule.crypto_app_currency_stocks_shares_defi_trading_investing_auto_trade_bot_automated_coinrule&hl=en_GB) .
