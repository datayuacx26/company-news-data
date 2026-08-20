---
schema_version: "1.0.0"
document_id: "47bf54ca4155ae008349b73534be48b8b452f0ca36f1c62ecdf21f41609ef1d1"
company_key: "yc-qfex"
company: "QFEX"
source_id: "yc-qfex-rss-5ea6110e160b"
canonical_url: "https://research.qfex.com/p/a-deep-dive-into-clob-microstructure"
published_at: "2025-12-02T18:28:37+00:00"
first_seen_at: "2026-07-25T20:12:49.931294+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:a4de3fadd2db62a7498db4dfa121368891ecd4e0b402c18b1b2e66a468880485"
---

# A Deep Dive into CLOB Microstructure

# A Deep Dive into CLOB Microstructure


### Beyond Price-Time Priority


[Annanay Kapila](https://substack.com/@annanaykapila)


Dec 02, 2025


We often spend a lot of time discussing alpha - signals, features, and prediction. But today, I want to take a break from alpha to talk about the machinery that actually executes those ideas: CLOB (Central Limit Order Book) microstructure. Most traders assume that if they place an order, it gets treated the same way regardless of the venue. But the rules governing how orders match can vary wildly between exchanges, and those rules drastically change the optimal strategy.


**The “Vanilla” Baseline: Price-Time Priority**
Let’s start with the standard model that most crypto and equity traders are familiar with. Consider the following top-of-book bid level ($100), where Alice arrived first:


-


**Alice:** 5 lots


-


**Charlie:** 5 lots


-


**Bob:** 10 lots


-


**Total Bid Depth:** 20 lots @ $100


Below them, Dan is bidding for 30 lots at $99.


Now, imagine


**Josh** enters the market and tries to sell 5 lots at $99. What happens?


In a standard “Vanilla” CLOB, the answer is simple:


**Alice buys 5 from Josh at a price of $100.**


The principles here are:


1.


**Price-Time Priority:** Makers are rewarded for improving the market. The first maker to form a new price level (Alice) is rewarded by being filled first.


2.


**Taker Benefit:** The taker (Josh) gets filled at $100, even though he was willing to sell at $99.


This is the baseline. But exchanges can, and do, change these rules to incentivize different behaviors. Let’s look at the alternatives.


**Variation 1: The Pro-Rata Model**
In some markets, specifically CME STIRS (Short-Term Interest Rate) markets, the exchange might decide to split the fill among everyone at the price level. Using our example above, if the exchange used a


**1:2:2 split** , Alice, Bob, and Charlie would share Josh’s sell order proportionally to their size. Some venues use a hybrid called


**GTBPR (Good-Till-Best-Price-Rata)** , like Eurex German debt, where Alice might get a slightly larger allocation for arriving early (e.g., a 2:1:2 split), but size still dominates time.


**Why do exchanges do this?**
First, a technical caveat: if someone fires a single lot into a pro-rata book, the exchange obviously can’t split one contract. It usually defaults to the front of the queue. Pro-rata really kicks in for larger flows. But why design a book this way? Usually, it is because the product’s tick size is too large relative to volatility. The market barely moves, and the exchange refuses to decrease the tick size (TradFi exchanges are notoriously horrific at updating specs with the times). In this environment, market making devolves into a pure speed game to get to the top of the queue. It becomes inherently monopolistic. By switching to pro-rata, the exchange incentivizes participants to join the


*back* of the level with size, rather than racing to the front. Personally, I’ve never been a fan. In a thick, time-priority queue, the middle of the queue is actually the most dangerous place to be - you face the risk of adverse selection from a massive informed order without the ability to cancel quickly. Pro-rata mitigates this, but it feels like a band-aid solution. Why not just decrease the tick size?


**Variation 2: Going Dark**
Now, let’s get into the funkier constructions:


**Dark Orders.** These are common in FX and US Equities, though rare in crypto. The premise is simple: If a firm like Citadel is generating alpha, they don’t want to broadcast their intent to the world. If they display their orders on a lit book, other traders can observe the strategy, front-run it, or cannibalize the alpha.


**Dark orders allow traders to rest liquidity without it appearing in the market data.**


-


**The Upside:** Takers (like Josh in our example) might try to sell at $99, but suddenly get filled at $101 because a dark bid was resting there. This provides unexpected price improvement.


-


**The Downside:** It opens a can of worms regarding fairness. If the whole book goes dark, price discovery breaks. Who gets to use these orders? Do we show the matches in the data feed?


The logical extreme of this is the


**Bilateral Feed** , which is how much of the FX market works. You only show your price to whitelisted counterparties who agree not to leak your data. The public CLOB is wide and full of sharks; dark mechanisms are the shield.


**Variation 3: Icebergs and the CME “Hack”**
Icebergs are a “lite” version of dark orders. You show a small tip (e.g., 1 lot) and hide the rest. When the tip fills, the exchange immediately reloads another lot from your hidden size to the


*back* of the queue. The trade-off here is severe: You lose queue priority. Because standard market data (Level 3) usually allows sophisticated traders to infer when an iceberg has reloaded, you are effectively signaling your presence while sitting at the back of the line. The adverse selection risk is often higher than just showing the full size.


**The CME Edge Case**
Years ago, there was a fascinating edge case on the CME involving icebergs.


CME would disseminate fill messages to the private feed (involved traders) slightly faster than the public feed.


-


**Front-of-queue** orders knew a trade happened


*first* but didn’t know the total size.


-


**Back-of-queue** orders knew


*last* , but they knew how many orders in front of them had vanished.


HFTs realized that by using a


**1+1 Iceberg** (show 1, hide 1), they could get the best of both worlds. The private fill message for their first lot would tell them exactly when the trade occurred and give them information advantages over the rest of the book. CME eventually updated their specs to kill this, but it remains a classic example of how microstructure quirks create alpha.


**Variation 4: Quote Domination (The “Anti-Dark” Order)**
Finally, let’s look at the weirdest interaction:


**Quote Domination** , found on the SIX Swiss Exchange. In our original example, Josh sells 5 @ $99. In a normal world, he gets filled at the maker’s price ($100). In a Quote Domination world, Josh gets filled at 99.


The exchange differentiates between “Orders” (retail/normal flow) and “Quotes” (approved Market Makers). If a Quote is interacting with an Order, the execution happens at the


*Quote* price. This seems hostile to the Market Maker, but it comes with lower fees. The exchange is essentially saying:


*“If you want cheap fees, you have to act like a true Market Maker and absorb flow at the crossed price.”*


This creates headaches for MMs:


-


They can’t easily cross the spread without paying higher “Order” fees.


-


They risk missing better prices on deeper levels.


-


If a normal order lands while their quote is in-flight, they lose the price improvement they would have received in a standard CLOB.


**Conclusion**
Microstructure is rarely as simple as “highest bid meets lowest offer.” Whether it’s Pro-Rata distributions, Dark pools, Icebergs, or Quote Domination, exchanges design these rules to manipulate participant behavior, usually to balance the needs of liquidity providers against the need for volume. Understanding these nuances determines whether your alpha actually captures the spread, or just gets run over.
