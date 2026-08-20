---
schema_version: "1.0.0"
document_id: "9281d9425b045a5f738c7ba5366539f506bb8497e5bc4e0944c43fb1817372f3"
company_key: "yc-qfex"
company: "QFEX"
source_id: "yc-qfex-rss-5ea6110e160b"
canonical_url: "https://research.qfex.com/p/your-first-hft-alpha-from-flow-prediction"
published_at: "2026-06-05T14:53:14+00:00"
first_seen_at: "2026-07-25T20:12:49.931294+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:c55a93247e46c076b6ffa3d99583cfcd8f5c4adb95fd1c7fb0bc314191ec1ceb"
---

# Your First HFT Alpha: From Flow Prediction to Order-Book Imbalance

# Your First HFT Alpha: From Flow Prediction to Order-Book Imbalance


### The simplest market-making alphas start with flow. The hard part is defining the flow, the horizon, and the signal.


[Annanay Kapila](https://substack.com/@annanaykapila)


Jun 05, 2026


A common question people ask is: what does a real market-making alpha look like? The answer is usually simpler than people expect. The starting point is not a secret data feed or an external price. It is the public order book and the public trade flow.


On the most liquid venues, Tier 1 HFTs build models without relying on external prices. They anticipate flows, originate moves, and others follow. The intuition behind this is straightforward: market makers set prices, and market makers do not want to hold long-term positions.


Thanks for reading QFEX Research! Subscribe for free to receive new posts and support my work.


If a big buyer comes in, market makers are now short. They need to de-risk, so they move their bids up until a seller is enticed enough to sell to them. This is the core idea behind many market-making alphas.


The business of price prediction is the business of predicting flow.


## The Core Problem: Turning Flow Into A Signal


Take a single symbol and define:


\\( x = qty_{bought}- qty_{sold} \\)


If more quantity was bought than sold, (x) is positive. If more was sold than bought, (x) is negative. This variable is correlated with future price movement, but it is not properly defined yet. The first problem is deciding over what time frame we should measure the quantity.


We could use a fixed window, like five minutes. But trading intuition says that a lot of quantity probably will not move the price from now if it happened five minutes ago. A big chunk of quantity is likely to overload the market makers, whereas quantity in small chunks gives them time to find counter-flow.


An exponentially decayed sum is better. On each trade, update the variable as:


```text
onTrade(qty) {
x *= e^(-lambda * (current_time - last_time));
x += qty;
}
```


here λ controls how quickly the past decays. In practice, λ is set based on what worked well historically.


This is not enough on its own. It does not capture enough price movement to be competitive today. Better alphas are not purely reactionary; they predict future flow rather than only react to known flow. Still, this is the right starting point because it shows how the problem is framed.


Price prediction begins with flow prediction.


## A First Order-Book Alpha: OBI


Now take the same intuition and look at the order book. Tier 1 HFTs create models to make markets on the most liquid venues without relying on external prices. They anticipate flows, originate moves, and others follow.


A very well-known alpha is order-book imbalance:


\\(OBI = \\frac{best_{BidQty} - best_{AskQty}}{best_{BidQty} + best_{AskQty}}\\)


This captures a simple idea: if there are more passive buyers than passive sellers, the price should go up. If the best bid quantity is larger than the best ask quantity, the top of book is leaning toward buyers. If the best ask quantity is larger than the best bid quantity, the top of book is leaning toward sellers.


So the


*Future price - Current pric* e is correlated to OBI, or:


\\( Future_{price} = current_{price} + (\\frac{best_{BidQty} - best_{AskQty}}{best_{BidQty} + best_{AskQty}}).A\\)


What is


*A* ? The answer can fill a textbook. In broad terms,


*A* is chosen so that the equation has low error across historical data. But choosing


*A* is not the interesting part, the interesting part is how we define “future.”


## The Horizon Is The Strategy


When we write


*Future price* , how do we define ‘future‘? One second from now? One minute? One hour? Not even a unit of time?


This choice defines the whole strategy. A very short-term signal tries to fit every up and downtick. It churns positions quickly for small profits, which is more like an HFT strategy. As the horizon gets longer, the strategy holds positions for longer, gets into bigger positions, and tries to capture larger moves.


This also means it needs more capital and will have lower Sharpe, because predicting further ahead is harder. The formula itself may need to change with the horizon too. For a longer term alphas,


*Best BidQty → Bid Qty* over top 10 levels, etc


And similarly on the ask side. The idea is still imbalance, but the definition changes with the horizon. This is why “future” is not a small implementation detail. It defines the strategy.


## Simple Does Not Mean Easy


A lot of people are surprised by how simple the alpha generation process can look. For example, asking ChatGPT to come up with C++ code for order-book-based BTC-USDT perps alphas on Binance API produced a first set of ideas that were great and almost completely bug-free.


That does not mean alpha is easy. It means the first layer can look simple. The hard part is not always writing down the formula. The hard part is defining the formula correctly.


What exactly is the signal? What exactly is the horizon? What exactly is the future price? Which flow should count?


Take the flow alpha:


\\(Exponential\\,Decay\\,Sum(bid_{qty}−ask_{qty},halflife)\\)


The idea is simple: measure the difference between bid quantity and ask quantity, apply exponential decay, and let the half-life decide how quickly old information fades. This is the same intuition as before. Recent flow should matter more than old flow.


## Finding Real Alpha


To give an idea of how people actually dig for alpha, consider this example.


An interviewee said that the Decayed flow alpha mentioned below,


\\(ExponentialDecaySum(bid_{qty}−ask_{qty},halflife)\\)


worked really well on a particular market, but only when restricting to trades coming from IOC orders.


IOC means immediate-or-cancel. IOCs take liquidity up to a specified price and quantity. Market orders take liquidity at any price, like an IOC sell with price=0. HFT firms never use market orders, only retail. Hence restricting to IOCs reduced the noise from uninformed flow.


At first, this is confusing. From public market data, you usually cannot tell the difference between market orders and IOC orders.


But in this particular case, he had dug deep and found a subtle, systematic labelling difference in the aggressive order IDs between market and IOC orders.


That is real alpha. The formula was simple. The edge was knowing which trades to include.


## The Lesson


The story starts with market makers. Market makers set prices, and market makers do not want to hold long-term positions. When flow pushes them too far in one direction, they move their prices to de-risk.


So we start with flow:


\\(x = qty_{bought}- qty_{sold} \\)


Then we make old flow fade:


```text
onTrade(qty) {
x *= e^(-lambda * (current_time - last_time));
x += qty;
}
```


Then we look at the order book:


\\( OBI = \\frac{best_{BidQty} - best_{AskQty}}{best_{BidQty} + best_{AskQty}} \\)


From there, the real questions are definitional. What is “future”? One second, one minute, one hour, or something other than time? Should the feature use only the best bid and ask, or the top 10 levels? Which flow should count? Can we isolate IOC flow? Can we reduce noise from uninformed flow?


And most importantly, are we just reacting to current flows, or predicting future ones?
