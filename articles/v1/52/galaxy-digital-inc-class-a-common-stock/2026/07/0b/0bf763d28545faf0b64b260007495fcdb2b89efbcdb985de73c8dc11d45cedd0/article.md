---
schema_version: "1.0.0"
document_id: "0bf763d28545faf0b64b260007495fcdb2b89efbcdb985de73c8dc11d45cedd0"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-3264694a8f1e"
canonical_url: "https://www.galaxy.com/insights/research/social-trading-fomo"
published_at: null
first_seen_at: "2026-07-25T06:06:49.370534+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:697dadb867af3172306096fba5859ee4b9d14444d283fdc8f43dda79ee58f86a"
---

# Crypto's Next Meta: Social Trading and Internet Finance

## Executive Summary


Crypto has repeatedly found ways to monetize internet attention. NFTs monetized cultural status. Memecoins monetized virality. Prediction markets monetize informational edges. Perpetual futures allow directional leveraged conviction on any asset. Together, these three primitives (onchain tokens, perps, and prediction markets) represent the most direct ways for internet-native individuals to express and profit from an edge.


Until recently, participating in any of these markets required fragmented tooling:[terminals](https://axiom.trade/discover?chain=sol) , wallet trackers,[Telegram bots](https://docs.bonkbot.io/) , and constant context-switching across platforms. The experience was inaccessible to anyone outside the power-user demographic.


A new class of applications is changing this. Social wallets like[fomo](https://fomo.family/) are collapsing discovery, execution, and identity into a single surface. Instead of trading being organized purely around charts, it’s increasingly becoming oriented around people. Early traction suggests meaningful demand: since launching in early 2025, fomo has onboarded ~400,000 users, facilitated roughly $2.6 billion in cumulative trading volume, and processed around $12 million in fiat inflows via Apple Pay. Robinhood has begun testing its own social trading features. The pattern is converging from both the crypto-native and TradFi sides.


This note argues that the convergence of social identity, mobile-first design, and onchain financial primitives represents the next major consumer meta, or trend, in crypto. We will also examine the meaningful risks this model introduces, including accelerated herd behavior, shorter time horizons, and information asymmetry between top traders and newer participants.


FOMO top traders leaderboard (24h)


## Introduction


The internet has fundamentally changed how people discover, process, and act on information. Social media algorithms now dictate what millions of people pay attention to on any given day. That shift has clearly already reshaped entertainment, media, and commerce. Finance is next.


Crypto has (so far) been the testing ground. Over the past several years, the industry has cycled through successive waves of attention-driven financial products. Non-fungible tokens (NFTs) turned cultural relevance into a tradeable asset. Memecoins compressed the cycle further, allowing anyone to launch a token named for a trend, a joke, or a viral moment. Platforms like Pump.fun made the cost of doing so effectively zero ([see our memecoin report](https://www.galaxy.com/insights/research/memecoins-pump-fun-solana-kols) ). More recently, prediction markets like Polymarket have given traders a way to monetize informational edges on real-world events, while perpetual futures have become the default instrument for expressing conviction onchain, with leverage ([Hyperliquid](https://app.hyperliquid.xyz/trade) is the prime example of this).


These three primitives (onchain tokens, perps, and prediction markets) now form the core toolkit for internet-native speculation. Each serves a different type of edge: memecoins reward speed and cultural fluency, perps reward conviction and risk management, and prediction markets reward information and judgement. Together, they cover nearly every way a trader might want to express a view.


What hasn’t kept pace is the experience of actually using them. Until recently trading across these primitives meant stitching together different blockchains, terminals, Telegram bots, wallet trackers, and X (formerly Twitter) feeds. This workflow was effective for power users but invisible and inaccessible to everyone else.


There are only so many people in the world willing to set up an Axiom account or run a copy-trading bot, but a lot more who are willing to download a social app because it significantly reduces the friction of use.


That gap is now closing. A new generation of social trading applications (most notably fomo) is rebuilding the interface layer around people instead of charts. Charts are still central to the experience, but they sit alongside the social layer. Users follow traders by username, observe activity in real time, and execute from the same surface. X is experimenting with[Smart Cashtags](https://finance.yahoo.com/news/elon-musks-x-plans-feature-164642951.html) that display real-time price data when users type a ticker symbol, and the platform has[hinted](https://x.com/nikitabier/status/2010277345651990564?s=20) that in-app trading functionality is in the works.[Robinhood is testing](https://robinhood.com/us/en/social/) social feeds with verified trade sharing. The convergence is happening from both sides: crypto-native apps are becoming more social, and social platforms are becoming more financialized. This note traces how we got here and where social trading is headed.


## A Brief History of Social Trading


Social trading is not a new idea. What is new is the combination of real-time social distribution and onchain financial primitives. Each wave solved part of the problem, but only recently have the pieces begun to converge.


### eToro and Copy Trading


Trading the flows (trying to anticipate where capital will move based on other participants’ behavior and reaction functions) has always been a strategy in financial markets. Social trading is really just a more legible, consumer-facing version of that same instinct.


The first meaningful attempt at social trading came from TradFi. Platforms like[eToro](https://www.etoro.com/copytrader/) allowed users to publicly display their stock portfolios and automatically copy other investors’ trades. Leaderboards and performance rankings introduced a simple but powerful idea: other people's behavior could itself be a trading signal.


The model was limited, though. Social interaction was secondary to the trading experience, and participation skewed toward a relatively narrow demographic. Most importantly, the system was not culturally native to the internet. It treated social features as overlays on top of finance rather as the core interface. Still, eToro did prove an important point: there is real demand for trading as a shared, observable activity rather than a solitary one.


### Crypto: Wallet Tracking and Group Chats


Crypto reintroduced social trading in a more organic form. Because most blockchains are fully transparent, anyone can observe anyone else’s trading activity in real time. This created a grassroots version of social trading built on wallet trackers, Telegram bots, Discord servers, and private group chats.


On the Solana blockchain in particular, an entire subculture developed around monitoring high-performing wallets, copy trading “smart money,” sharing token contract addresses through X and Telegram, and reacting to group-chat-driven narratives in real time. Accounts like[Moby](https://x.com/whalewatchalert) built large followings by surfacing whale (large investor) activity as it happened.


This model was powerful but deeply fragmented. It required constant switching between platforms and a level of technical sophistication that excluded most potential participants. Social trading existed, but it was buried behind poor UX and accessible only to elite users. As fomo[co-founder](https://x.com/seyong) Se Yong Park put it when I spoke with him: the total addressable market of people willing to set up an advanced trading terminal has a hard ceiling.


### Attention Becomes Financialized


As crypto matured, speculation increasingly aligned with internet attention. NFTs monetized cultural relevance and status, though that craze was short-lived and[extremely frothy](https://www.coindesk.com/business/2021/03/12/how-bananas-and-mortgages-can-explain-the-nft-craze) . Memecoins compressed the cycle further, monetizing virality and trends directly. Pump.fun lowered the cost of turning attention into a liquid tradeable asset to effectively zero.


Attention cycles, visualized. (RicHard-59/Wikimedia Commons)


In parallel, two additional primitives (perps and prediction markets) gained significant traction.


By this point, the financial primitives were mature. The missing piece was the interface.


### The Missing Piece: Interface


Despite the sophistication of the underlying instruments, the experience of using them remained fragmented and bewildering. Every perp DEX looked the same. Social context lived on Twitter and Telegram while execution lived elsewhere. Unless you had multiple monitors and belonged to the right group chats, you were structurally disadvantaged.


Newer applications represent a fundamental shift. Instead of tracking wallets, you can follow people by username, akin to Instagram or Twitter. Instead of private group chats, activity is public by default. Trading becomes part of an identity rather than an isolated action. This shift mirrors a broader internet pattern: social platforms evolved from feeds of content to graphs of identity. Finance is now following the same path.


Importantly, the shift is not confined to crypto. Robinhood has begun testing integrated social trading features, allowing users to follow traders, view verified trades in real time, and discuss strategies directly inside the app. The difference, as the fomo founder noted, is that Robinhood’s social layer is opt-in: users choose to publish their trades. In the social wallet model, transparency is the default. That distinction matters because opt-in systems tend to skew toward performance signaling, while default-public systems give users a fuller picture of how someone actually trades.


The social wallet may still be early, but it does not appear to be a temporary niche experiment. It reflects a broader recognition that discovery, identity, and execution are collapsing into a single surface across finance.


## Social Trading in Practice


While social trading has historically existed in fragmented and informal ways, a newer class of onchain applications is beginning to unify identity and execution into a single interface. Multiple crypto-native platforms are integrating social trading features into the user experience.


*Note: This list is not comprehensive*


-


[legend.trade](https://www.legend.trade/)


-


[OKX Orbit](https://www.okx.com/en-us/learn/orbit-traders-social-network)


-


[Moonshot](https://x.com/moonshot)


-


[Hype](https://x.com/HypeHQ)


-


[BYDFi](https://x.com/BYDFi)


-


[Dreamcash](https://x.com/Dreamcash)


fomo (cheekily named for the “fear of missing out” that motivates herd behavior) is by far the clearest example of this model. Rather than centering the experience around order books and charts, fomo organized activity around people. Users follow other traders by username and observe their activity in real time. From there, they can choose how (or whether) to act on that information. The experience is closer to a familiar social platform than traditional trading software.


Early traction has been significant. Since its debut about a year ago, fomo has onboarded ~400,000 users, facilitated roughly $2.6 billion in cumulative trading volume, and processed around $12 million in fiat inflows via Apple Pay. For context, Apple Pay onboarding was a key factor in the January 2025 surge of activity tied to the[$TRUMP memecoin on Moonshot,](https://x.com/moonshot/status/1880659326395666759?s=20) a mobile trading app tailored for retail users. It remains probably the single most effective mechanism for bringing non-crypto-native users onchain. These figures represent activity that historically required considerably more technical sophistication. Lowering friction at the interface layer expands participation without fundamentally altering the underlying financial primitives.


*Note: The fomo charts below show Solana-based activity only. fomo supports trading across multiple blockchains.*


What’s notable about these numbers is composition. This is activity that historically required configuring complex systems: fragmented DEX interfaces, Telegram bots, multiple wallets. The fact that volume comparable to Moonshot’s is flowing through a social media-esque app suggests the interface bottleneck was real. And that removing it unlocks a meaningfully different user base.


Because activity is public by default, users can evaluate not just what someone is trading but how they trade. Metrics like average hold time provide context that was largely absent in the wallet-tracking era, where you could see an address’s activity but had no reliable way to assess the individual behind it. fomo has introduced average hold time as a visible metric (average hold times across Solana trading have been declining sharply...[see our memecoin report](https://www.galaxy.com/insights/research/memecoins-pump-fun-solana-kols) ). This seems like a small addition but is one that helps users distinguish between traders with aligned time horizons and those who are likely to scalp tokens and exit quickly.


This matters because the core tension of social trading is information asymmetry. The same transparency that helps informed users make better decisions also makes it trivially easy for newer participants to pile into trades without understanding the context or thesis. When a top trader’s activity is one tap away from execution, the line between “informed following” and “exit liquidity” gets thin. The platform’s team is aware of this dynamic, but it’s a structural feature of any system where visibility and execution are compressed into the same surface, not a bug unique to fomo.


As the platform moves to integrate perps, prediction markets, and yield on idle balances, the data picture will become more complex. But the early signal is clear: when you make speculation social, mobile, and frictionless, people will show up.


## This Is Inevitable


The shift toward social trading is the natural outcome of structural changes in how financial markets are accessed and consumed.


The first structural shift is that finance has become API-native. Brokerages, derivatives protocols, and crypto infrastructure are now modular and programmable. Execution is no longer scarce. Anyone can build a trading interface on top of existing rails. AI is just accelerating this process. When execution is commoditized, the competitive surface moves upstream to distribution and discovery. Social graphs are better at these things than order books.


The second is generational. The demographic that grew up on TikTok, Instagram, and YouTube does not distinguish between content consumption and interaction the way previous generations did. For this cohort, a feed is the default interface for everything: news, entertainment, shopping, and increasingly, finance. Asking them to use a Bloomberg-style terminal or even a traditional DEX frontend is asking them to speak a different language. A social wallet that looks and feels like the apps they already use is not a novelty to them; it’s what they expect.


The third structural shift is the blurring of finance and entertainment. Sports betting apps like FanDuel normalized the idea that speculation can be casual, social, and mobile. Prediction markets extended that model to politics, culture, and current events. Memecoins made it native to crypto. The throughline is clear: when speculation becomes entertainment, it follows the distribution patterns of entertainment, which means feeds, follows, shares, and virality. The platforms that win will be the ones that treat trading as a social activity first and a financial one second.


These dynamics reinforce each other. API-native infrastructure makes it easy to build social trading apps. A mobile-native (and internet-native) generation provides the user base. The entertainment-ification of finance provides the behavioral pull.


None of these forces are reversing anytime soon.


The question is not whether social trading becomes a dominant interface for internet-native finance. It’s how quickly the remaining primitives (perps, prediction markets, yield) get integrated into the same surface, and which platforms consolidate the experience first.


## Risks


Social trading is a natural evolution of internet-native finance, but the same design choices that ease friction and expand participation also amplify behavioral dynamics that have long plagued speculative markets.


The most prominent risk is groupthink. When trading activity is highly visible and one tap away from execution, herding behavior intensifies.


Average FOMO users seeing their favorite KOL buy a new token (Gagan Raj Rai/Wikimedia Commons)


Users (particularly less experienced ones) tend to overweight the actions of popular traders or well-known accounts, especially during periods of heightened volume and volatility. This is not a new dynamic in crypto, but social feeds compress the feedback loop. What previously required finding a wallet address, loading it into a tracker, and manually executing a trade now happens in seconds. Speed is the product’s selling point, but it is also what makes uninformed following so easy.


Closely related is the exit liquidity problem. Social trading creates an inherently asymmetric structure between top traders (and key opinion leaders, or KOLs) and their followers. A skilled trader who enters a position early benefits directly and instantly from the wave of followers who pile in after (regardless of the trade’s fundamental merits). When that trader exits, the followers absorb the impact. The more visible and popular a trader becomes, the more pronounced this dynamic gets. Their PnL is partially a function of audience size, not just skill. Platforms can surface metrics like average hold time to help users identify misaligned incentives, but the asymmetry is baked into any system where visibility drives capital flows.


There is also a transparency problem. A trader’s activity on any single platform does not necessarily reflect their full position. A top trader could accumulate a token on a separate wallet, then buy on their fomo wallet to signal conviction to followers, exit from the hidden wallet into the liquidity their followers just created, and only later sell on fomo. This kind of multi-wallet manipulation is straightforward to execute and difficult for followers to detect.


Social trading also compresses time horizons. As we documented in[our memecoin report](https://www.galaxy.com/insights/research/memecoins-pump-fun-solana-kols) , average token hold times on Solana have already decreased sharply (mainly because of platforms like Axiom). When you can see in real time that a trader you follow just exited a position, the impulse to sell immediately is stronger than it would be in a less transparent environment. Over time, this behavior risks pushing the entire user base toward shorter and shorter holding periods, which benefits the fastest hyper-scalping participants at the expense of everyone else.


> *The model works in part because it makes trading feel less consequential than it really is.*


Finally, there is a real risk that social trading deepens the gamification of finance in ways that are difficult to walk back. The same mechanics that make the experience engaging (leaderboards, aesthetic PnL sharing, real-time feeds) also borrow heavily from the playbook of addictive product design. When speculation feels like doomscrolling on TikTok, the psychological boundary between entertainment and financial risk-taking erodes. This is not unique to crypto but is worth describing honestly: *the model works in part because it makes trading feel less consequential than it actually is.*


None of these risks are reasons to dismiss social trading. They are reasons to take its design seriously.


## The Endgame


The logical endpoint of everything described in this piece is convergence. Not more fragmentation across specialized tools, but a single surface where all of internet-native finance lives together (see Robinhood).


Imagine one app where you can earn yield on idle cash, trade perps with leverage, bet on elections or cultural events through prediction markets, buy memecoins the moment they start trending, and do all of it in a social environment where your trading activity builds a public identity and is linked to your social media accounts. Your financial and social graphs merge. You follow people, not tickers. Your profile is your track record (PnL, win rate, average hold time, all visible by default). The people you meet onchain become the people you connect with offline.


Every piece of this scenario exists in some form, just scattered across different platforms. Hyperliquid handles perps. Polymarket handles prediction markets. Pump.fun and Axiom handle memecoins. Various protocols offer yield. X and Telegram provide the social layer. What doesn’t exist yet is the full unified experience that pulls it all into one place and makes each primitive feel native rather than bolted on.


> *“We want to build the best place to speculate on anything, ever"*
>
>
> **Se Yong Park, co-founder, fomo**


The platforms building towards this (fomo being the most visible example) are betting that the bundling is the product. That users don’t want five apps for five financial activities or to bridge assets across several different fragmented blockchains. They want one surface that handles everything and is organized around the people they trust and the information they care about. Park, the FOMO co-founder, told me directly: “We want to build the best place to speculate on anything, ever.” That ambition is the clearest articulation of where this is all headed.


Whether fomo or someone else gets there first is an open question. What seems far more certain is the direction. Social trading is the natural interface for internet-native finance. The primitives are mature. The infrastructure is programmable. The user base is ready. The convergence is underway.


This is the next meta, and we are at the beginning.


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, may hold financial interests in certain assets referenced in this content. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
