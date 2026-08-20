---
schema_version: "1.0.0"
document_id: "d87b2b83fe951650257e8fd7b666c40d1f71925f771cbda0ebfba5f06c38a659"
company_key: "yc-freeport-markets"
company: "Freeport Markets"
source_id: "yc-freeport-markets-news-import-46c2f164323d"
canonical_url: "https://www.freeportmarkets.com/research-how-freeport-works.html"
published_at: "2026-06-05T00:00:00+00:00"
first_seen_at: "2026-07-25T05:46:52.249644+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:95761b92762d87f8e139b0a3b929101274d54b92166b57001bb06603421bb113"
---

# Freeport: Leverage, Global Markets, 24/7

If you have heard about Freeport and want to understand what it actually does before you download it, this is the post to read. The one-paragraph version: Freeport is a mobile app that lets you trade tokenized stocks, crypto, perpetual futures, and pre-IPO names from a single account, onchain with no KYC, wrapped in an AI layer that reads the market for you. You sign in with Google or Apple, a wallet is created for you automatically, you fund it with a card or crypto, and you trade. The rest of this guide unpacks every part of that sentence.


## What Freeport is, and what it is not


Freeport is a trading interface. It is the app on your phone, the feed you scroll, and the routing layer that sends your order to wherever it gets filled. It is not, itself, an exchange or a bank. When you tap buy, Freeport routes that order to established on-chain venues and aggregators, and the asset settles into a wallet that belongs to your account. In other words, Freeport is an efficient router and an interface sitting on top of permissionless blockchain infrastructure.


It is not a registered broker-dealer, your assets are not SIPC-insured, and tokenized stocks are not the same legal instrument as shares in a traditional brokerage account. What you get instead is speed, breadth, and a genuinely unified account: stocks, crypto, leverage, and private-company exposure in one place, trading nearly around the clock, without the walled gardens that separate those products everywhere else.


## From download to first trade


Onboarding is deliberately short, and it has four steps.


**Download the app.** Freeport is available on both the App Store and the Google Play Store. The file is small and downloads quickly.


**Sign in.** You log in with Google, Apple, or an email. There is no seed phrase to write down. Behind the scenes, an embedded wallet is created for you on both Solana and Ethereum using Privy, a wallet provider trusted across the industry and backed by Stripe.


**Add funds.** You can fund your account with Apple Pay, Google Pay, a debit or credit card, bank ACH, or a crypto transfer. Card and bank routes go through regulated partners and may require KYC. Crypto deposits are routed through temporary wallets to enhance privacy.


**Trade.** Once funded, every asset is one tap away.


The whole flow is built so that someone who has never touched a blockchain can be trading in a minute, while the on-chain settlement that makes it fast and global runs invisibly underneath.


## What you can trade


Freeport supports four distinct asset types from a single account.


**Tokenized stocks.** When you buy NVDA on Freeport, you are buying a token that is wrapped 1:1 against a real Nvidia share held by Ondo Finance. The token tracks the share's price and gives you economic exposure to it. The practical upside is that it trades on-chain, nearly 24/7, in fractional sizes, alongside everything else in your account.


**Crypto.** Spot BTC, ETH, SOL, HYPE, and others, the same assets you would expect on any crypto exchange. Freeport routes your flow for best pricing across AMMs.


**[Perpetual futures](https://freeportmarkets.com/research-perpetual-future.html) .** Leveraged contracts with no expiry date; you can hold a position, long or short, for as long as you like, while a recurring funding payment between traders keeps the contract's price tracking the underlying asset. The underlyings span stocks, indices, currencies, commodities, rates, and crypto.


**Pre-IPO.** Positions in private companies such as[SpaceX](https://freeportmarkets.com/research-spacex.html) ,[OpenAI](https://freeportmarkets.com/research-openai.html) ,[Anthropic](https://freeportmarkets.com/research-anthropic.html) , Anduril, and Neuralink before they list publicly. Because these companies are private, you are not buying registered shares: the exposure is delivered through tokenized wrappers that track a company's value, either as a spot token backed by shares held in a special-purpose vehicle, or as a synthetic contract on the company's implied valuation.


## The Freeport feed


Open Freeport and the first thing you see is a feed of trade ideas. This is not a generic news ticker. It is built by a pipeline that does something specific: it reads the people who actually move markets, or, if you choose to customize it, the people you trust and follow, and turns what they say into structured trade ideas.


**Listening and filtering.** A service continuously monitors Twitter/X, Substacks, Reddit, podcasts, mainstream news, and insider and hedge-fund trades to discern what analysts, traders, and domain experts are saying. A proprietary AI model reads each relevant post, learns from it, and converts it into a structured signal: which ticker, buy or sell, a confidence score, and the reasoning behind it. Irrelevant posts are filtered out and never shown at all.


**Scoring and ranking.** Those raw signals flow into Freeport's recommender, which ranks them on dozens of weighted factors: how recent the signal is, how confident it is, how many independent sources are saying the same thing, the credibility tier of the author, the engagement around it, and how well it fits your existing portfolio and interests. The purpose is to surface only the most relevant information, and to surface it first.


The feed surfaces ideas, with the reasoning attached, from sources worth listening to. It does not decide for you, and it is built to make you a faster, better-informed trader, not to replace your judgment. Whether the ideas it surfaces actually make money is a fair question, and we test it honestly in[Do Freeport's Trade Recs & Users Actually Make Money?](https://freeportmarkets.com/research-recs-make-money.html)


## The AI analyst


The job the analyst does, at its core, is the same job a junior analyst does at a hedge fund: read everything that came in, figure out what actually matters, and put it in front of the people who have to make decisions today. Concretely, that means pulling from a curated set of trusted sources in your feed and asking a simple question of all of it: what story is the market actually telling right now?


Those stories are what we call narratives. A narrative is not a stock, and it is not a headline. It is the connective tissue that explains why a particular set of things is moving together: "memory pricing is booming," "AI capex is concentrating in the semi-cap names," "regional banks are getting squeezed by commercial real estate." Identifying these narratives is the first thing the analyst does and the thing it spends the most effort on, because once you have the right narrative, almost everything else, which tickers matter, which catalysts to watch, which prints to trust, falls out of it.


For each narrative, the analyst then builds out the two-sided picture. There is a bull case and a bear case for everything that is actively trading, and the analyst lays out both, with the evidence for each side and, importantly, who is on each side. This last part matters more than it sounds. Knowing that a respected macro fund is long while a well-known short seller is short tells you something price alone cannot: smart, informed people have looked at the same facts and reached opposite conclusions. That is usually a healthy sign; it means the trade is contested, not crowded. What you want to watch out for is when every name you respect is sitting on the same side. That can mean the thesis is correct, but it can also mean the trade is consensus, the easy money has been made, and the next move is more likely to be the painful one.


The analyst also tracks how a narrative is actually performing, not by watching a single ticker but by watching the basket of names the narrative would move. The memory thesis, for example, is not just Micron. It is Micron, Samsung, SK Hynix, SanDisk, Western Digital, and Seagate, treated as one object. Looking at any single name in isolation will mislead you: Samsung might be down on a workers' strike, Western Digital might be up on a spin-off rumor, Micron might gap on guidance. None of those tell you whether the underlying narrative is working. The basket does. If five of six names are bid on the same day, the narrative is alive; if the leader rallies while the rest fade, the narrative is breaking.


Put simply, the feed is the raw, ground-level view: what is being said, traded, and reacted to in the last few hours. The analyst is the layer above it, asking what it all adds up to: which narratives are forming, which are accelerating, which are quietly dying, and where the smart money is positioned on each.


A working trader needs both. The feed without the analyst is noise you cannot prioritize. The analyst without the feed is a story you cannot verify. Used together, they let you move fluidly between the micro and the macro, checking a single print against the broader thesis, or testing whether a thesis still holds up against what is actually printing, which is, in the end, how the good traders we have watched actually work.


## Security, custody, and fees


**Security.** When you sign in, embedded wallets are created for you on Solana and EVM through Privy, and your assets live on-chain in those wallets. What makes the setup different from a traditional "we hold the keys for you" model is that nobody, including us, actually holds the full key. The private key is split into shares: one sits with Privy, now part of Stripe, and one is tied to your Google or Apple account. A transaction can only be signed when those shares come together. For an attacker to move your funds, they would have to compromise Privy, your Google or Apple account, and your physical device at the same time. Any one of those failing in isolation is not enough.


The same property cuts the other way for recovery. You do not have a seed phrase to lose, because you do not need one. As long as you can sign back into your Apple or Google account, you can rebuild your share and recover the wallet. The thing that scares most people away from crypto, writing twelve words on a piece of paper and praying you never lose it, is simply not part of the experience.


**Custody.** This is a middle path. It is not a traditional brokerage holding shares for you, and it is not raw self-custody where you alone hold the keys. It is an embedded-wallet model: the convenience of an app login, with the security of a multi-party key, on assets that genuinely sit on public chains. For users who want full self-custody in the traditional sense, you can always export your private key and take it with you. We treat that as non-negotiable: if the assets are really yours, you have to be able to walk away with them.


**Fees.** There are no platform fees on basic token swaps, and no account, deposit, or maintenance fees. Network costs and onramp fees, the cost of a card purchase, for example, are set by the third parties involved, not by Freeport. Perpetuals are where we do charge a fee, but the structure is worth explaining, because the headline number is misleading on its own. All of our users' perp trades route through a single API connection into Hyperliquid, which lowers fees as cumulative volume grows. Because every Freeport user's volume pools into the same account, the platform sits at the top of Hyperliquid's fee schedule, in a tier almost no individual trader would reach on their own. We pass that lower rate through and add our fee on top, and the two combined come out roughly equal to what you would pay trading directly on Hyperliquid from a cold-start account. So yes, we charge a fee. But you are not paying more than you would pay trading the same product yourself. The only users for whom this is not true are people personally doing billions a week in volume, who would already qualify for the top tier on their own and do not need anyone aggregating on their behalf.


## Where the real risks are


Anything held on-chain inherits a class of risks that traditional brokerages do not have. Smart contracts can have bugs. Oracles can misprice an asset. The custodians sitting behind tokenized products can fail. Regulatory changes can restrict access to specific products in specific jurisdictions. These risks are usually small in expectation and we work continuously to minimize them, but they are not zero, and any honest view of trading on Freeport has to start by acknowledging them.


Perpetual futures carry two product-level risks worth singling out. The first is liquidation: perps are leveraged contracts, so if the market moves against your position by enough that your posted collateral no longer covers the loss, the position is automatically closed at the prevailing price and the collateral is gone. The second is auto-deleveraging, or ADL: in extreme dislocations, when the liquidation engine cannot find a willing counterparty to absorb a position at any reasonable price, the venue may forcibly close out profitable positions on the opposite side to balance the book. ADL is rare, but it is the mechanism that backstops a perp venue in the worst conditions, and as a trader you can end up on the receiving end of it. Both risks apply to standard perps on liquid markets and to pre-IPO perps alike, though pre-IPO perps, with thinner reference prices, are more prone to the sharp dislocations that trigger them in the first place.


Beyond perps, each tokenized product has its own wrapper-specific profile. For tokenized stocks on highly liquid names such as Apple, Tesla, Nvidia, and the broad index ETFs, the wrapper risk is small: a regulated issuer holds the actual share against the actual token, and the chain of custody is straightforward. The story changes as you move down the liquidity curve. Tokenized pre-IPO equity introduces SPV and legal-wrapper risk: your claim runs through a special-purpose vehicle whose terms govern whether and how you ever realize the value, and an unfavorable structure can sit between you and the underlying for a long time. Tokenized private credit carries opacity risk: unlike a public bond, the borrower's financials, covenants, and recovery profile are often not fully visible, and you are trusting the originator's underwriting more than you would in a public market. Yield-bearing stablecoin protocols add protocol risk on top of whatever the underlying yield strategy is; the smart-contract layer that mints, redeems, and accrues yield is itself software that has to keep working correctly for the asset to behave as advertised.


None of this makes any of these products bad. It means each one has a different cost-of-being-wrong, and a serious user should know which one they are holding.


## Why traders use Freeport anyway


A reasonable reader could finish the previous section and conclude that the risk-adjusted answer is to stay on a traditional broker. For many people that is the right answer, and we will not pretend otherwise. But for a real and growing number of traders, the other side of the ledger is heavy enough that the trade is worth taking. Here is what sits on the other side.


**Access.** Entire asset classes that used to be locked away from non-institutional accounts: pre-IPO equity in the most valuable private companies in the world, private credit, catastrophe bonds, international equities a U.S. brokerage account would never let you touch. The on-chain wrapper is what makes any of this reachable.


**An AI layer that earns its keep.** The feed surfaces what the smartest voices in the market are actually paying attention to, and the analyst turns that flow into structured, narrative-level reads on what is happening. For many users this is the part that converts Freeport from "another trading app" into something they check every day.


**A market that does not close.** Earnings drop after the bell, geopolitical news breaks on a Sunday, a crypto position needs hedging at 3 a.m., and on Freeport you can act on it now, not at 9:30 a.m. Eastern. On top of that, the leverage that high-conviction trades used to require a prime broker for is available at retail-friendly cost.


None of these eliminate the risks in the section above. They sit alongside them. The point is that accepting a different, in some ways more demanding, but also more rewarding risk profile, in exchange for access and tooling that traditional brokerages cannot match, is a trade a lot of people are now actively choosing to make. We built Freeport for them.


Read more from the Freeport research team on the Freeport Logbook.


[Freeport Logbook](https://freeportlogbook.substack.com/)
