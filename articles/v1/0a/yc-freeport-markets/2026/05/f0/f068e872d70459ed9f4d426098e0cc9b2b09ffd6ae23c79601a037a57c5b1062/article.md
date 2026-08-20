---
schema_version: "1.0.0"
document_id: "f068e872d70459ed9f4d426098e0cc9b2b09ffd6ae23c79601a037a57c5b1062"
company_key: "yc-freeport-markets"
company: "Freeport Markets"
source_id: "yc-freeport-markets-news-import-46c2f164323d"
canonical_url: "https://www.freeportmarkets.com/research-perpetual-future.html"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-25T05:46:52.249644+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:dd0aaa47fc670b2955ce1a54a44659f08a56dace31945467f5923e77e6092ac5"
---

# What Is a Perpetual Future? · Freeport Research

A perpetual future, or perp, is the most traded product in crypto, and the same structure is spreading to stocks, commodities, and currencies. Yet most people trading one could not tell you how it actually works. Here is the plain version: a perp is a contract that tracks the price of an asset and never expires. You do not own the asset. You hold a position that gains or loses as the price moves, and that single difference is what makes a perp so useful.


## What a perp actually is


When you buy Bitcoin the ordinary way, you own a coin. It sits in your wallet, and if the price doubles, your coin is worth twice as much. A perpetual future works differently. You never take delivery of the coin. Instead you and the venue hold a contract whose value follows Bitcoin's price, and you post a slice of cash, called margin, to keep it open. If Bitcoin rises, the contract pays you the difference. If it falls, you pay it.


Because you are holding a contract rather than the asset, two doors open that owning outright cannot. You can go short as easily as long, making money when the price falls instead of only when it rises. And you can use leverage, controlling a position larger than the cash you put down. Direction and size: those two levers are the whole reason perps exist.


## Why it never drifts: the funding rate


There is a puzzle hiding in that definition. An ordinary futures contract has an expiry date. On that day the contract and the real asset settle at the same price, so everyone knows any gap between them must close, and that expectation keeps the two roughly in line the whole way there. A perp has no expiry. So what stops a contract that never settles from floating away from the real price?


The answer is a small recurring payment between traders, called the funding rate. Every few hours, whichever side of the trade is crowded pays the other. If the perp is trading above the spot price, too many traders are long, so longs pay shorts. Holding a long now costs a little each period, which cools demand and nudges the price back down. If the perp trades below spot, shorts pay longs, pushing it back up. The venue does not keep this money, it only passes it from one side to the other. Funding never forces the perp to match spot exactly. It just makes drifting expensive, and that cost is enough to keep the contract honest.


## A quick example


Leverage is where a perp gets both its power and its risk, and a short walkthrough makes it concrete. Say you have $1,000 and you open a 5x long on Bitcoin.


1. Your $1,000 of margin now controls a $5,000 position, five times your cash.
2. Bitcoin rises 10 percent. The position gains $500, so you are up 50 percent on your $1,000.
3. Had Bitcoin instead fallen 10 percent, you would be down that same $500, half your stake, on a move of only 10 percent.
4. If Bitcoin keeps sliding toward 20 percent, your $1,000 of margin is used up and the position is liquidated: the venue closes it for you and the stake is gone.


Leverage multiplies the asset's percentage move onto your cash in both directions. The higher the leverage, the smaller the drop that wipes you out, which is why traders who stick around tend to use modest leverage and size each position so an ordinary bad day cannot liquidate them.


## What it feels like on Freeport


On Freeport, all of this sits behind one screen. You pick a market, choose long or short, set your leverage, and enter an amount. A single account and a single margin balance back every position, so you are never shuffling money between separate wallets to place different trades. Funding is handled quietly in the background, netted into your position rather than billed as a separate line.


What surprises most new traders is the range. Perps began in crypto, but the structure works for anything with a price. On Freeport that means BTC, ETH, SOL and hundreds of other tokens; equity perps on names like Nvidia and SK Hynix; broad indices and sector baskets, from the S&P 500 to semiconductors; commodities like gold, oil and wheat; forex and rates; and even private companies like SpaceX and OpenAI, traded through perps on their implied valuations. You can go long or short on any of it from the same balance, without ever owning the underlying asset.


## The bottom line


A perp is a bet on price, long or short, with leverage, that you can hold for as long as you like. It stays tied to reality through the funding rate instead of an expiry date, traders paying each other a small fee whenever the price drifts. That combination is what lets you trade a sector, a commodity, or even a private company you could never buy directly. If that last part is what brought you here, our guides to trading[SpaceX](https://freeportmarkets.com/research-spacex.html) ,[OpenAI](https://freeportmarkets.com/research-openai.html) , and[Anthropic](https://freeportmarkets.com/research-anthropic.html) exposure are the natural next read.


Read more from the Freeport research team on the Freeport Logbook.


[Freeport Logbook](https://freeportlogbook.substack.com/)
