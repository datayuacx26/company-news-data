---
schema_version: "1.0.0"
document_id: "b8161a2799e9073555b6676f215d5ca6fb5de5d0d539be667d3de309300ca1c9"
company_key: "yc-pax-markets"
company: "PAX Markets"
source_id: "yc-pax-markets-news-import-f168516c7dd7"
canonical_url: "https://pax.markets/blog/lambdas/"
published_at: null
first_seen_at: "2026-07-22T08:22:59.778613+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:314692158b4ab5cf1ee9404d7ce23b0acbceb5b081341bbc2697523c47890ef0"
---

# The PAX λ API: fastest tick-to-trade in any asset class.

## A new async trading paradigm


PAX is building an ultra-low latency crypto exchange. We offer zero-fee trading Better than zero-fee. PAX pays a rebate on every order filled, for both "making" and "taking" liquidity.


through industry standard APIs and instant no-latency Orders using the λ API are placed the exact instant that the market ticks and will always be faster than orders placed from off-exchange.


order placement using fee-based access to our λ API.


The PAX λ API provides instant order placement in response to any market event. Traders program the λ API by submitting λ programs instead of orders. A λ program contains two things: an event predicate, and an underlying order. If the event predicate is realized in actual market data, the underlying order is placed immediately.


Traders can use PAX λs to respond to both local and remote market events. In either case, orders placed by λ response are faster than orders placed from trading algorithms running in off-exchange servers.


## The mechanics of PAX λ program evaluation


All active or "in force" PAX λ programs are evaluated for every market We use the term "market event" here to refer to exchange published market data *and* any other relevant market event or information.


event processed by the PAX exchange. The PAX exchange naturally produces its own market events but PAX also subscribes to market data streams from remote venues such as Binance and Coinbase. PAX λ participants can submit many individual λ programs and have multiple λ programs in force at any given moment. Thus, a trader that has some basis to trade using both local and remote events can materialize their thesis through two or more λ programs in force simultaneously.


A PAX λ Program


\[ Predicate \]


\[ Underlying Order or Cancellation \]


The predicate specifies an event that, if realized in the future, will trigger the placement of the underlying order. The underlying order is a standard limit order or order cancellation.


The event predicate specifies an event type, and a minimum threshold, or hurdle, that must be cleared for the predicate to be satisfied. Event types include liquidity added or removed from the PAX order book, liquidity added or removed from a remote venue order book, and a change in price quoted by a remote venue.


PAX λs inherently have the first look at local market events: the local market event is generated inside of the PAX matching engine and is then processed by the PAX λ response unit immediately. Thus, PAX λs are guaranteed to be faster than any trading algorithm running on an off-exchange server.


For remote events, PAX λs remove one hop of network latency (from participants to PAX) and bypass the PAX frontend. We expect that some clients will run existing strategies using off-exchange servers to respond to remote market events, but that competition to take arbitrage opportunities will soon compel traders to use our λ API to capture these remote-to-PAX trades.


## A PAX λ program example


Suppose a trader is running a market taking Called "taking" because the strategy takes liquidity off of the order book. Whether the strategy decides to buy or sell, it interacts with limit orders that are "resting" on the book. The resting limit orders are said to provide or "make" liquidity.


Thus, the orders placed by this strategy (because they execute against the resting limit orders) are said to be "taking" liquidity.


strategy that responds to price changes in BTC by buying or selling ETH. Specifically, their thesis is that if the price of BTC ticks up (down) they will buy (sell) ETH.


### \[ Predicate \]


Token Event Threshold


BTC LocalMidPx > 100,000.00


### \[ Underlying Limit Order \]


Token Action Limit-Px Qty


ETH Buy 3,000.00 0.5


The PAX λ response unit indexes its programs by "token" and by "event type." Thus, if a BTC event is processed, this λ is evaluated (along with all other BTC λ programs). If the event indicates that BTC mid-price is above $100,000.00, then the λ response unit places the underlying limit order, to buy ETH, by sending that order to the PAX matching engine.


## Multiple λ responses on the same basis


We expect that certain well known trades will be materialized as λ programs by multiple participants. In the event that multiple λ predicates are met simultaneously, the PAX λ response unit will place the responding underlying orders according to the following prioritization. PAX λ response on equivalent basis is a topic that will be discussed further in a future blog post.


1. Underlying orders that cancel existing offered liquidity will be placed first, and
2. Underlying orders that add liquidity will be placed next, and
3. Underlying orders that remove liquidity will be placed last.


If there are multiple orders in any of the above groups, those orders will be split into smaller units of quantity and then randomly interleaved, i.e., such that the effect of the opportunity is shared amongst participants responding on the same basis.


The PAX λ API opens the door to new competition in HFT market making. Today, only the very fastest HFTs can compete to make markets, but at PAX, anyone with a good price model or inter-venue arbitrage strategy can compete as a peer. Thus, at PAX, traders compete on the basis of market insight and risk tolerance – an outcome that is good for markets, and thus good for us all.


### Ultra-low latency market response with the PAX λ API


PAX is building a crypto exchange that provides algorithmic traders a new event-based API to place orders "instantaneously," the very moment a market event occurs. The speed of order placement is valuable to high frequency market participants, and the PAX λ API provides the ultimate end-state of ultra-low latency market response.
