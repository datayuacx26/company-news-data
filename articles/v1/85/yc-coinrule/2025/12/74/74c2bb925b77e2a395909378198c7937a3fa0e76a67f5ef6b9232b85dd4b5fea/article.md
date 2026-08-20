---
schema_version: "1.0.0"
document_id: "74c2bb925b77e2a395909378198c7937a3fa0e76a67f5ef6b9232b85dd4b5fea"
company_key: "yc-coinrule"
company: "Coinrule"
source_id: "yc-coinrule-news-import-2d53545bc7e5"
canonical_url: "https://coinrule.com/blog/crypto-automated-trading/crypto-trading-bot-profit-eth/"
published_at: "2025-12-11T15:42:39+00:00"
first_seen_at: "2026-07-25T00:21:57.456860+00:00"
fetched_at: "2026-07-28T22:25:00.065453+00:00"
content_hash: "sha256:80354440b40726c0585405ce2db2b0851f0efe3260da3058411605a8ecd2e8f0"
---

# I Made Profit With This Crypto Trading Bot

## **Introduction to the Coinrule Bot Results**


Welcome to the Recon Trader. In today’s video, I will share with you the recent results of the rule-based algo trading bots that I deployed on the Coinrule platform just over a week ago. Before we get into the weeds and break down those numbers, if you’re not familiar with Coinrule, it is a platform that allows you to configure and deploy rule-based algo trading bots. If you’re not familiar with the term “rule-based,” it is basically an *IF this THEN that* strategy. When we jump into the numbers, I will show you an example of my rule-based strategies.


But first, let’s take a look at what your minimum investment would be to get started. You can actually start for free, that’s right zero dollars. I will leave a link in the description, and you can get started for free if you follow that link. With the free account, you’ll get two live rules (essentially two live bots), two demo rules, several templates to help you get started, and the ability to connect one exchange. Like most bot platforms, you are not sending money to Coinrule. You are simply connecting your exchange via API keys. Your money stays on the exchange, and your bot sends buy or sell orders on your behalf. Your funds remain as secure as they can be when held on an exchange.


If you decide to upgrade your plan, you will get more features: more bots, more templates, leverage strategies, and live Telegram/text notifications. So, it *can* be worthwhile to justify that $30/month investment.


#### **Reviewing Bot Performance and Initial Outcomes**


From my main dashboard, you can see I have two rules currently live and active. I also have a couple that finished. If you want to see the video where I configured all these rules, I’ll leave a link in the description below. Also, now is a good time to hit the subscribe button so you don’t miss future videos, as I will be configuring more bots on this platform. The two finished bots ended almost as a wash, one slightly up and one slightly down. The scalping bot definitely needs some tweaking, but it has a ton of potential. My next bot will likely be a revised version of this scalper.


Now let’s look at the currently active bots.


#### **Detailed Breakdown of the Demo Bot Strategy**


The first bot I deployed was a demo bot, used while I was doing recon on the platform. Here are its details:


-


Exchange: Demo Exchange


-


Deployment: A few weeks ago


-


Strategy logic:


**IF**
ETH has Moving Average 9 crossing above Moving Average 50 on the 1-hour timeframe
→ **BUY** $1000 of ETH using my USDT wallet.


**THEN IF**
ETH has MA9 crossing below MA50 on the 1-hour timeframe
→ **SELL** 100% of ETH balance to USDT.


**OR IF**
ETH has RSI greater than 90 on the 1-hour timeframe
→ **SELL** 100% of ETH balance to USDT.


The execute function defines how many times the bot will run this sequence, in this case, nine times. So far, it has triggered 7 out of 9 times and achieved nearly 26% profit (≈260 USDT) in less than a month.


A strong early result.


Scrolling down shows the order history:


-


Buy → Sell (profit)


-


Buy → Sell (profit)


-


Buy → Sell (loss)


-


And the most recent buy, which is still open.


#### **Live Trading Results from the Kraken Bot**


Now let’s look at a live-fire bot running on my Kraken exchange. This bot was deployed on January 24th, so it’s been live for just over a week and is already up almost 15%. Again, winning the battle. Since this is a new platform, I’m starting small. Once stability is proven, I’ll give the bot more capital.


Here is the rule sequence:


**IF**
ETH has MA9 crossing above MA50 on the 1-hour timeframe


**AND IF**
ETH has RSI greater than 40 on the 1-hour timeframe
→ **BUY** $100 of ETH using my USD wallet at market.


**THEN IF**
ETH has MA9 crossing below MA50 on the 1-hour timeframe
→ **SELL** $100 of ETH to USD at market.


**OR IF**
ETH price decreases by 4% within 4 hours
→ **SELL** all amount bought to USD at market.


This bot executes four times total. It has triggered three of those so far.


Looking at the order history:


-


Buy → profitable sell


-


Buy → loss


-


Buy → strong profit


The equity curve shows small dips but strong upside, helped significantly by ETH’s recent run.


[https://www.youtube.com/watch?v=RNXa8-UlTTQ](https://www.youtube.com/watch?v=RNXa8-UlTTQ)


Download the Crypto trading bot on[IOS](https://apps.apple.com/us/app/coinrule-crypto-trading-bot/id1667293808) , and on the[Google Store](https://play.google.com/store/apps/details?id=com.coinrule.crypto_app_currency_stocks_shares_defi_trading_investing_auto_trade_bot_automated_coinrule&hl=en_GB) .
