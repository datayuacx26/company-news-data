---
schema_version: "1.0.0"
document_id: "e3065510f00373d715cd54fda4bc3e808d89cb92045da69c91398c7e9643cdcf"
company_key: "yc-coinrule"
company: "Coinrule"
source_id: "yc-coinrule-news-import-2d53545bc7e5"
canonical_url: "https://coinrule.com/blog/crypto-automated-trading/guide-bitcoin-ethereum-trading-bot/"
published_at: "2025-12-11T13:26:06+00:00"
first_seen_at: "2026-07-25T00:21:57.456860+00:00"
fetched_at: "2026-07-28T22:25:00.065453+00:00"
content_hash: "sha256:571b835232606dc6fcb1938b1a61e3933ec051c1711b8de51cef6bd8d72b9138"
---

# Create an Automated Crypto Trading Strategy

## Introduction to Coinrule and Rule-Based Trading


Welcome to the Recon Trader. In today’s video, I will show you how to configure and deploy a rule-based automated crypto trading bot on the[Coinrule](https://coinrule.com/) platform. Now, if you’re not familiar with Coinrule, their team recently reached out to me and asked me to take a look at their platform. I’ve done some recon, and so far it all looks pretty good. The team is a legitimate group of people, and much like most of the platforms that I utilize, you don’t actually send your money to Coinrule.


The platform simply connects your API keys from your exchange so that your bot can send buy and sell orders based on the rules that you configure. Your money stays on your exchange and is as secure as it can be on an exchange.


As far as configuration goes, it’s all rule-based. What does rule-based mean? The code is more like guidelines than actual rules. To me, it is the classic if this, then that. We can take a look real quick at the demo bot I first set up based on a couple of very basic rules.


If you look at the sequences toward the bottom:
My “if” is: if ETH has Moving Average 9 crossing above Moving Average 50 in a time frame of one hour, buy $1,000 of that coin, that coin being ETH, with my USDT wallet. Then, if that coin has Moving Average 9 crossing below Moving Average 50 in a time frame of one hour, sell 100% of the total balance of that coin (again ETH) to my USDT wallet. Or, if the coin from the first action (the buy of ETH) has RSI greater than 90 in a time frame of one hour, sell 100% of the total balance of that coin to my USD wallet.


Then I simply execute, telling it how many times I want this to run. I set a total of nine executions starting January 15th, and so far it’s up 10%. It has actually been triggered four out of the nine times since January 15th. Extremely simple to configure.


#### Setting Up Exchange Connections


Before you start configuring your first bot, you’ll probably want to connect your exchange. You do that over on the left-hand side menu by clicking on Exchanges. You connect your exchange just like you would on most platforms I’ve shown in past videos:
Go to your exchange, like Binance US, create your API and secret key, copy and paste them into the correct fields, and hit connect.


As you can see, my Kraken exchange API key has already been added and is live. If you need more details on how to connect your exchange, the Coinrule team has created tutorial videos to walk you through it.


#### Exploring Templates and Pre-Built Strategies


Now let’s get into the weeds and configure one of these rule-based automated trading bots. To start, click the red Create Rule button. The first thing to select is either the Demo Exchange or your actual exchange. In this case, I’m selecting Kraken. I now have the option to manually configure a strategy or use one of their templates. Across the top menu, click Templates, there are literally 31 pages of templates you can use or modify.


Let’s say you want to use Scalping on Trend since the market is trending. Click on it, read the description, and choose Select. Just like that, I now have a battle plan laid out. I do need to give it some instructions, like deciding what coins the strategy will trade. I’m not going to trade all the crap coins, I’ll select a coin I’ve actually done recon on and feel comfortable trading. In this case: ETH.


They already have the rules set out for me, but I can tweak them. The first step is: if ETH has Moving Average 50 lower than price in a time frame of 50 minutes… Let’s tweak that to: Moving Average 9 crossing above Moving Average 50 in a 15-minute time frame. Then it will buy. I need to give it some ammo, so I’ll allocate $100 of ETH using my USDT wallet at market price. Next, I’m looking at the take-profit and stop-loss logic:
If the coin increases 2.5% from the price at which I bought it…
Or if the coin decreases 2% from the price at which I bought it…
Then sell 100% of the amount bought to my USD wallet at market.


#### Adjusting Scalping Conditions


I’m going to modify this. Since the strategy is scalping, I want quick profits. I’ll set the take profit to 1.5% and the stop loss to 0.75%. That way, I get out fast if I’m wrong and take profit quickly if I’m right. I’ve made a couple of changes, and I’ll do an update video on these rule-based strategies later. Now is a good time to hit subscribe and smash the notification bell so you don’t miss those updates. I want to start immediately, although I could schedule it for later. I want it to execute four times, but not more than once every six hours, actually, I’ll lower that to once every one hour.


Last thing: launch the strategy. Confirm the settings and click Launch. The bot is now live and running.


#### Fine-Tuning Trade Logic


That was using a template. Now I’ll show you how I create my own rule from scratch. Choose Kraken as the exchange.
For the IF event, I’m not using “any coin”, I’m selecting ETH. Next, select an indicator: RSI greater than 30 in a one-hour timeframe.
Add another IF: the coin has price crossing above the 9-period moving average in the same one-hour timeframe. Now add a trade action based on that IF: Buy $100 of ETH with my USD wallet at market. Next, add the take profit: If the coin has a price increase of 3% from where I bought it, sell 100% of the balance to my USD wallet at market. Now the stop loss: OR, if the coin has a price decrease of 1% from where I bought it, sell 100% of the balance to my USD wallet at market. The rule-based strategy is now ready to deploy. I’ll start it immediately, set it to execute four times, name it RSAMA, and click Launch Live.


Now you can see all four rule-based bots I’ve deployed, three live-fire, one demo.


#### Closing Thoughts


That pretty much wraps it up. I’ll be doing update videos soon, so now is a great time to subscribe. If you liked this video, spike a like. If you have questions or comments, leave them below. I’ll include a link to **[Coinrule](https://coinrule.com/)** in the description. And remember: never send your money into battle without first doing your recon. See you in the next video.


**[https://www.youtube.com/watch?v=NJ6DxphfRVA](https://www.youtube.com/watch?v=NJ6DxphfRVA)**


**Download[Coinrule,](https://coinrule.com/) the Crypto trading bot on[IOS](https://apps.apple.com/us/app/coinrule-crypto-trading-bot/id1667293808) , and on the[Google Store](https://play.google.com/store/apps/details?id=com.coinrule.crypto_app_currency_stocks_shares_defi_trading_investing_auto_trade_bot_automated_coinrule&hl=en_GB)**
