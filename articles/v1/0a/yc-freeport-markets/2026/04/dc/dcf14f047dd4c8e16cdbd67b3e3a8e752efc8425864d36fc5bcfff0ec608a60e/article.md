---
schema_version: "1.0.0"
document_id: "dcf14f047dd4c8e16cdbd67b3e3a8e752efc8425864d36fc5bcfff0ec608a60e"
company_key: "yc-freeport-markets"
company: "Freeport Markets"
source_id: "yc-freeport-markets-news-import-46c2f164323d"
canonical_url: "https://www.freeportmarkets.com/research-seven-trades.html"
published_at: "2026-04-22T00:00:00+00:00"
first_seen_at: "2026-07-25T05:46:52.249644+00:00"
fetched_at: "2026-07-28T21:45:30.754431+00:00"
content_hash: "sha256:75329506033336736e3f6ed1a22ea26b4cefdf8ee07f10a117be61054741bbdb"
---

# 7 Trades on Pre-IPO Stocks that Returned 183%

From time to time we reach out to users whose onchain activity reflects the kind of trading we think everyone can learn from, and ask whether they would walk us through their process. We never publish or reveal anything about a user without their explicit permission. The piece below is drawn from a long conversation with one such user. All names and personally identifying details have been changed, the piece has been stylistically edited for readability, and while we trust our source, we cannot independently verify every claim he makes in the course of a night's conversation. Where possible we have grounded the narrative in publicly available onchain data, and all figures are produced from public data for illustrative purposes and do not constitute investment advice.


## A portrait of Trader 7


Trader 7 picks a restaurant near 79th and Columbus because he can walk there from his apartment, and because he says the Parisian sea bass is the only thing above 72nd he isn't bored of. He is already at the table when I arrive. Gray fleece zip-up over a Patagonia sun hoodie, the haircut expensive in the way that's supposed to look cheap. I have met him exactly once before, and I am still getting used to the fact that his name is not actually Trader 7.


He found me a month ago through a LinkedIn post I'd written, half serious, and DMed me twenty minutes later. He thought the post was funny enough to download my app. That is the entire origin story. He says the word serendipity without irony, which is something I notice about him.


He grew up on the Upper West Side, two blocks from where we're sitting. He traded at a Chicago prop shop for three years and then quit, a thing he tells people within the first three minutes of meeting them. He now trades Kalshi and Polymarket full-time, and also Ventuals perps and PreStocks tokens and anything else onchain where he can find edge.


## Dinner, 7:34 p.m.: fading large moves on thin books


"Can I tell you about Christmas?"


"Please."


"I was at my parents' apartment. Two blocks that way," he points west. "My family was trying to get me to play Catan. But I get a notification on[Anthropic perps](https://freeportmarkets.com/research-anthropic.html) spiking. The book was, I'm telling you, two dozen bids, three dozen asks, total depth maybe a few hundred grand of notional. Some guy walks in with market orders for a mil. The mark prints minus twenty-eight percent in an hour."


"And the oracle."


"The oracle can't move. The oracle is half offchain and half onchain, and the offchain component can't even move! So the mark is in another zip code and the oracle's barely budged. The funding rate crosses negative two thousand percent annualized on the hour."


"So I fade the move and go long. In size. No thinking. The discount has to fade, it's math. Thirty minutes, I cover half. Ninety minutes, I cover the rest."


"How much did you make."


"Business-class ticket," he says. "To somewhere nice."


The bread arrives. He orders the sea bass and an IPA. I order lamb over rice and a Diet Coke and try to write this down in my head without making it obvious that I am writing it down in my head.


## Between courses: the oracle and funding, explained


The thing about Trader 7, and this is the thing you notice about anyone who has traded for a living, is that he loves talking about trading. He keeps explaining until you tell him, in plain English, that you understand; if you nod without saying it, he takes the nod as a request to go again from the top. I haven't said the words yet, so he describes the trade again.


Here is what a perp is. It's a contract on an exchange that tracks the price of something else, in this case Anthropic, a private company that doesn't itself list a share anywhere. The contract doesn't expire. Two people are on either end of it, a long and a short, swapping exposure to whatever Anthropic is worth. Because the thing it tracks doesn't trade on the exchange, the exchange needs a rule for what the contract is supposed to be worth. That rule is called the oracle.


The oracle is not a person, it's an average. Ventuals takes two numbers and splits the difference: an offchain feed of recent secondary transactions in the actual shares, and a moving average of whatever the perp itself last traded at. Half and half. The result publishes every few seconds. The[funding rate](https://freeportmarkets.com/research-perpetual-future.html) is what keeps the two sides honest. If the perp drifts rich against the oracle, longs pay a fee to shorts every eight hours. If it drifts cheap, shorts pay longs. Small gap, small fee. Big gap, big fee. The point is to make sitting on the wrong side of the gap expensive, so somebody walks in and closes it.


Thin-book moments break the system. The mark jumps because the mark is whatever the last market order printed. The oracle moves less, since half of it is the offchain data, which doesn't update. For the first few minutes the mark is far below the oracle and funding prints more than two thousand percent annualized in favor of the dislocated side. And the gap has to close. Funding is continuous: you get paid every eight hours, every day, as long as you're on the right side, and as long as the underlying secondary market isn't actually going down. Anthropic, which had just raised at three hundred and eighty billion at the time, was really not about to be marked down.


Somebody with a clear head eventually notices the exchange is offering a king's ransom to sit on the right side of a mispriced trade. They come in. The mark drags back to the oracle. Funding falls. Trade ends. It is not a story about what Anthropic is worth. It is a story about how long prices can stay dislocated without anyone paying attention. Christmas morning, the answer was roughly a few hours.


## Bar, 9:20 p.m.: the basis trade


We walk over to a bar on Amsterdam because he says they make good negronis there. The bar is half-full and nobody looks up.


"The other trade is the basis."


"Explain."


"Same company, two different venues. PreStocks token on Solana. Ventuals perp on Hyperliquid. Different flows, different liquidity, different crowds. The number on one side drifts away from the number on the other side. Sometimes it drifts a lot. And, in theory, it should come back."


"In theory."


He shrugs at the ceiling in that way you do when you have been losing arguments with yourself for a while. "Look. I think there's edge here. I'm not sure there's edge here. What I know is that when I am on this trade and funding is in my favor, I get paid to wait, and the waiting is most of the P&L."


"How much of the P&L?"


"More than half of it. I make more on the funding than I do on the prices actually converging. If I stripped the funding out I'd be at about half of what I'm up on the basis book overall."


"Which is."


"Call it low-forties. On prices alone, low-twenties. The rest is the funding carry. And the composition is bumpy. I'm currently down on SpaceX, but my Anthropic position has made more than enough back."


## Walking up Amsterdam, 11:40 p.m.: buy and hold


After a few rounds we pay and walk north on Amsterdam in search of additional food, because the sea bass, while excellent, was portioned for people who've never been hungry. There is a Joe's on 74th. We agree on a slice.


"Let me tell you what actually made me the most money. I just held Anthropic and SpaceX spot. PreStocks. Bought in October, never sold. That's the trade."


"Not fading the move on Christmas?"


"That trade was great, had a Sharpe of four and a half. But it's not every day that someone comes in and smashes through the order book. I've been long Anthropic and SpaceX spot every hour since October."


## Joe's Pizza: trading funding-round announcements


Around midnight we've finished our first slice and ordered a second, because we are still hungry and because the first one went down too quickly to count. The second one is hotter and we eat it more carefully.


"The Microsoft-Nvidia thing. November 18th. I was long perps and spot when it came out. Not just that one. I went long on most positive funding and partnership headlines."


"How did you know."


He points at his phone.


"You have a group chat?"


"I have a group chat with some college friends, yes. But I also have," he opens up my app, "Freeport on my phone. The SpaceX IPO headline was on my lock screen before it was on Bloomberg."


"The rule," he says, "is you are probably early enough if you long within a few minutes of the headline coming out, and you definitely want to sell within at most a few days. Flow keeps coming in for days after the announcement from folks who FOMO, and that's your exit."


"Do you ever miss on these?"


"Some of these are real. Sometimes the rumor dies. Sometimes the round gets pushed. Sometimes the price just doesn't move. But the point is that it works more often than it doesn't, and your risk is asymmetric."


## Joe's, more slices: launch-day pumps


It is now something like 12:40 a.m. and we are the only people in Joe's.


"Tell me about the launch pumps."


"OK. New PreStocks listings. Whenever a new private company gets tokenized on Solana, I buy in the first minute of listing. Kalshi listed; spot went plus twenty-something percent in the first few hours. Neuralink listed; same thing, maybe forty. Both of those were basically free money if you were willing to buy at the first print."


"I think I tried the same thing, but it didn't work on Polymarket," I said.


"Polymarket did not pump. But two of three in the small sample is enough conviction for me. Long the launch and get out in a day regardless of whether it's red or green, rinse and repeat."


"This doesn't seem scalable."


"No. A new listing is maybe once every few months, and the size I can put through the first thirty minutes is limited by the AMM's depth. But it's free money, who cares if it is scalable?"


## Waiting for the Uber, 1:08 a.m.: long SpaceX perps into the IPO


It's late. He suggests I call an Uber. The app shows the driver a few minutes away.


"Do you mind if I ask? All-in, five months in, everything: how much."


He laughs. "Enough to retire anywhere in the world. Except New York City."


"At least give me the percentage."


"I'd say I've roughly 3x'd my money."


I had one more thing to ask him. "Give me a trade rec. One real one. Right now."


He nods like he's been waiting for me to ask.


"[SpaceX](https://freeportmarkets.com/research-spacex.html) . The IPO is coming. Here is the trade. You long the SpaceX perp and TWAP it over the weeks leading into the IPO. Scale in. Don't buy it all at one print."


"Why perps and not spot."


"At IPO, the venue swaps the oracle from the offchain valuations feed to the real stock; the perp has to price to the public-market tape. And the public-market tape at IPO is not at the IPO price, because IPOs pop. The float is tiny. The insiders are locked up for six months. The people who actually own the company can't supply. You are going to have every retail brokerage in the world piling in at once to buy, and no one who can sell."


"So the perp has to mark up?"


"The perp has to mark up toward wherever the real stock prints, and the real stock prints above the IPO price. Fifteen to twenty-five percent above is my conservative estimate. That is what I am long for."


"What's the downside?"


"Cost of carry. If funding gets to a few thousand percent annualized while you're in the trade, you're bleeding on the long. If funding runs hot for a few weeks without the IPO timeline tightening up, you get out. The trade works only if the IPO actually happens on the rumored horizon. If it slips by a quarter, the carry eats into your PnL."


My phone beeps. The Uber is here.


"I'm writing a post. Can I put your name in?"


He laughs. "I'd prefer not."


"The trades?"


"The trades you can put on whatever you want. That I don't mind."


We split the tab and walk out into the cold. He goes north up Columbus. I step into the car. The driver takes me south on Amsterdam, and I watch him get smaller in the side mirror, already on his phone, already back at it.


*This is an article about a conversation with a trader, not a recommendation that you do any of what he does. All figures in this piece are produced from public data for illustrative purposes and do not constitute investment, legal, or tax advice. Pre-IPO tokens and perpetual futures carry risk including total loss of principal.*


Read more from the Freeport research team on the Freeport Logbook.


[Freeport Logbook](https://freeportlogbook.substack.com/)
