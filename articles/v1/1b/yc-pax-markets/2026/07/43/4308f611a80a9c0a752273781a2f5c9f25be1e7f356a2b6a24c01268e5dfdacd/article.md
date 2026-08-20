---
schema_version: "1.0.0"
document_id: "4308f611a80a9c0a752273781a2f5c9f25be1e7f356a2b6a24c01268e5dfdacd"
company_key: "yc-pax-markets"
company: "PAX Markets"
source_id: "yc-pax-markets-news-import-f168516c7dd7"
canonical_url: "https://pax.markets/blog/lambda-use-cases/"
published_at: null
first_seen_at: "2026-07-22T08:22:59.778613+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:1de4e3d5c2ed66562767d7c60b5b2c23c079da85049ee056331f88a68c34ce6f"
---

# PAX λ Use Cases

PAX offers both industry standard legacy trading APIs and its new ultra-low latency λ API. To use the λ API, a trader submits a λ program that consists of a predicate and an underlying order. The predicate specifies some discrete market event, such as a change in local market microstructure, remote venue price movement, or the result of an AI/ML inference published by PAX or by a third party. The underlying order is either a standard limit order or a cancellation or size down for a previously placed limit order. If the predicate condition is satisfied by actual market events, the underlying order is placed at that exact instant.


In this blog post, we share three use cases for the PAX λ API:
1. Market taking.
2. Market making.
3. Exchange integrated AI.


We further describe how the PAX λ API reduces exposure risk for market makers, thereby creating better and more liquid markets. Finally, we introduce the topic of prioritizing different λ responses that trigger on the same market event.


## Market taking


Market taking is a high frequency strategy where a HFT participant buys or sells based on some indication that price will move in the direction of their action. For example, if the HFT participant thinks that price will go up, they buy. Market taking is so-called because the HFT participant, to materialize this strategy, must cross the spread and remove – or "take" – liquidity from the order book.


Participants using a market taking strategy may base their trades on correlated price movements; e.g., price movement for the same asset on a different market, or price movement for a correlated asset on any market – or they may base their trades on various signals, that when evaluated by a predictive model, indicate that price may change.


Some signals that market takers respond to are strong market signals – so much so that many market participants are aware of those signals. Because so many are aware of the moment, HFT market participants must "race" for the trade. This speed race compels HFT market participants to invest in various technologies to reduce their tick-to-trade latency.


The PAX λ API facilitates market taking by placing orders in response to local price changes, in response to remote price changes, and in response to AI/ML inference. As an example, suppose that an HFT participant buys BTC at PAX every time BTC price ticks up at Binance. To materialize this strategy, the market participant uses a λ program that responds to a price change at Binance by placing its underlying order to buy BTC at PAX.


## Market making


Market making is a high frequency strategy where an HFT participant simultaneously places offers – resting quotes, or limit orders – both to buy and sell. A participant that is always willing to transact A market maker is simultaneously willing to buy at some price and to sell at some other (higher) price.


is said to be a "market maker" because they "form" a market that other participants can interact with.


The goal of a market maker is to profit from the "spread." When market makers show their quotes to buy and to sell, their selling price (the "ask") is higher than their buying price (the "bid"). The difference in price between the ask and the bid is the spread. Ideally, the market maker can transact with several other individual market participants such that both their bids and asks are executed.


Concretely, suppose a market maker (participant "A") offers to sell BTC for $100,000.01 and to buy BTC for $100,000.00. The spread is thus $0.01. If another participant "B" buys some BTC and another participant "C" sells some BTC, and both B and C interact with the quotes posted by A, then A realizes a profit of $0.01 multiplied by the quantity traded with B and C.


Participant Action Price (USD)


A Places Ask $100,000.01


A Places Bid $100,000.00


B Buys BTC $100,000.01


C Sells BTC $100,000.00


For this to work, market maker "A" needs their quotes to be "at the front" of the time priority queue. Thus, market makers must react quickly when new price levels are open, i.e. such that their limit orders form the new price level and sit at the front of the time priority queue.


The PAX λ API provides an event predicate for that moment when a "level" is removed from the order book. Market makers that want queue priority can use this event, as the predicate in a λ program, to place their quotes.


## Makers vs. takers


Market makers face exposure risk. If a market participant transacts a large quantity in a single order – removing one or more price levels – then the market makers are left either long or short (depending on whether the large transaction was selling or buying).


In the absence of large market moving orders, makers attempt to cancel and reprice their quotes as market signals indicate imminent price movement. Makers do not want "bad fills" i.e. to have their quotes executed when they know that price is moving, leaving them short as prices climb or long as they fall.


On the other hand, market takers, aware of the same market signals, want to transact with these temporarily mispriced quotes. Thus, based on certain market signals, makers attempt to cancel their quotes and takers attempt to take those same quotes. This scenario, where makers race to cancel and takers race to take, happens frequently and is often harmful to market makers. The makers evidently do not want to transact at these moments. Assuming that the makers and takers have roughly equivalent tick-to-trade latency (from their off-exchange servers to the exchange), then the takers are likely to win the race (by chance) because they outnumber the makers.


## PAX λs reduce market making exposure risk


Market makers can use PAX λ programs to cancel their quotes in response to market signals indicating imminent price change. Cancellations placed using the λ API do not have to race with market takers responding to the same market event using off-exchange algorithms: λ cancellations will always be faster than off-exchange takers. This helps market makers avoid bad fills and reduce exposure risk.


We expect that both makers and takers will use λ programs for certain indicative market events, which introduces the question: when multiple λ programs are triggered, Every PAX λ program is evaluated for every market event processed by the PAX matching engine. Thus λ programs from multiple participants may simultaneously have their predicates met.


which underlying orders should be placed first?


In the scenario where multiple λ programs are triggered by one market event, and one participant is attempting to cancel their quote while one or several others are attempting to interact with that quote, we believe that the cancellation should receive priority. PAX does not force a participant into a transaction when they have pre-declared their basis to not participate.


By enabling makers to cancel at the exact instant that their quotes appear to be mispriced, PAX helps market makers reduce their exposure risk. This reduced risk, in-turn, fosters deeper liquidity and tighter spreads: succinctly, a better market.


## Exchange integrated AI: PAX ɑ


Market signals used by HFTs can be found by others, otherwise the need for speed would not exist – traders that have found a truly "private" market signal need not race because there is no one to race with.


PAX is introducing PAX ɑ (alpha), an AI/ML price prediction model. We expect that PAX ɑ is indicative of the most relevant and well known market signals. PAX ɑ is computed at PAX for every market event we process, and market participants can use λ programs to respond on the basis of predictions made by PAX ɑ. Because PAX ɑ is fully integrated into the PAX exchange, its predictions are available before any off-exchange server has seen the latest market data.


PAX ɑ is available by opt-in subscription for our HFT customers.


### Key Takeaways


We presented specific use cases for PAX λ programs: market taking, market making, and exchange integrated AI, called PAX ɑ. PAX λs reduce exposure risk for market makers and thus create better and more liquid markets.
