---
schema_version: "1.0.0"
document_id: "6d03255f86b3c5f43f808862b873933f867a29704839e078c4426536d3fb090e"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/security-with-maker"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:bba65af80068ebbdfcb8971c766fc09d98ed1b342fce75ba5a2867980c440d2a"
---

# Security with Maker

Quantstamp recently completed 2 audits with Maker. We audited Liquidations 2.0 (MIP 45), a proposed update that heavily innovates upon existing liquidation mechanisms, and we also audited the Dai Peg Stability Module, a recently implemented mechanism that helps defend Dai’s peg to the US dollar. In addition to these audits, Quantstamp has previously worked with Maker to provide security services for recipients of[Maker Community Grants](https://community-development.makerdao.com/en/funding/development-grants/) including, but not limited to, Sablier, PoolTogether, and Ramp.


### Maker: The Leader of DeFi


*Dai marketcap is now over 2 billion USD. source:*[CoinGecko](https://www.coingecko.com/en/coins/dai)


Maker is foundational to DeFi. In December 2017, the Maker protocol went live and began offering loans in newly minted Dai, a stablecoin pegged to the US dollar. Dai is unique in comparison to other stablecoins because it is the most trustless: Dai balances cannot be frozen by third parties.


Although stablecoins and collateralized lending may seem like the norm now, Dai inspired—or at least influenced—the current status of DeFi by showing others that trustless lending and stablecoins were possible. Today, the Maker ecosystem continues to thrive and innovate. As of March 2021, Dai has massive liquidity with over 2.7 billion Dai in existence, the highest TVL in DeFi with over 6 billion USD worth of assets locked, and is heavily utilized by other top DeFi projects.


*Maker has the most value locked in its protocol compared to any other DeFi project. source:*[DeFi Pulse](https://www.coingecko.com/en/coins/dai)


### Liquidations Keep DeFi Healthy


Liquidating collateral plays an essential role in keeping DeFi lending applications running. With Maker, in order to receive a trustless loan, a borrower needs to overcollateralize the loan. This means that if a borrower wants to receive a loan of 100 Dai, they need to deposit over 100 USD worth of digital assets (such as ether) as collateral. Overcollateralization is necessary in lending platforms that allow users to anonymously borrow because it is the only way for lending platforms to remain solvent and profitable in the event that a borrower defaults on their loan.


In order for Maker to remain solvent, a borrower's collateral needs to be liquidated (aka sold off) when the value of the collateral falls below a certain threshold. When it falls beneath this threshold, the collateral is auctioned off to liquidators at a discount, which effectively pays off the loan on behalf of the defaulted borrowers. Liquidators play a key role in DeFi because they eliminate bad debt.


Lending protocols are most vulnerable when the value of collateral is rapidly dropping. When the crypto market at large experiences a rapid sell off, borrowers rush to pay off their loan before it defaults or provide more collateral. When borrowers sell off other digital assets to pay off the loan, it further contributes to the decline of collateral value and leads to further defaults.


### Next Gen Liquidations


In some situations, the rapid decline in value of collateral deposited by borrowers can threaten the solvency of a lending protocol if liquidators decide that purchasing collateral from bad debt is no longer profitable. When the price of collateral is rapidly dropping, liquidators risk losing money if they cannot sell the collateral fast enough to make a profit. If enough liquidators believe they cannot make a profit, lending protocols end up with insolvent loans.


In order to enhance the liquidation process, the Maker community drafted a proposed update called Liquidation 2.0, which included the following changes and features:


- Dutch style auction replaced the English auction and enables instant settlement
- Instant settlement enables flash loan liquidations
- Instant settlement provides liquidators with greater access to DeFi composability


The Dutch style auction mechanism is designed to speed up the existing liquidation process to make Maker more resilient in the event of market volatility. The flashloan component is particularly interesting because liquidators no longer need to be a whale (have lots of capital) to participate in a liquidation. This is likely to enhance the number of liquidators which contributes to healthier DeFi markets.


Quantstamp is proud to have audited Liquidations 2.0. We will continue to provide updates[via Twitter](https://twitter.com/Quantstamp) as Liquidations 2.0 progresses through the governance process.


## Dai Peg Stability Module


*When Dai falls off its peg, Maker governance uses monetary policy tools to enforce the peg. image source:*[CoinGecko](https://www.coingecko.com/en/coins/dai)


When borrowers take out a loan from Maker, they are also simultaneously creating new Dai. In order to make sure that Dai remains pegged to the US dollar, Maker governance has monetary policy tools at their disposal. For example, if the price of Dai rose above 1 dollar, Maker governance may choose to increase the debt ceiling or decrease the stability fee (aka the interest rate to receive a loan in Dai) in order to put downward pressure on the free market value of Dai. If the price of Dai falls below 1 dollar, Maker governance may choose to increase the Dai savings rate or increase the stability fee in order to remove Dai supply from the market, which puts upward pressure on Dai price.


On January 28th, 2021, Maker implemented the Dai Peg Stability Module, a new monetary policy tool designed to enhance the strength of the Dai peg. This mechanism helps Dai maintain its peg by enabling Dai to be swapped for stablecoins like USDC at close to a 1 to 1 ratio. This provides arbitrageurs an opportunity to profit by supporting the Dai peg. When Dai is above 1 dollar, arbitrageurs can mint Dai at a value below the market rate by depositing USDC and then selling their newly minted Dai at the market rate. When Dai is below 1 dollar, arbitrageurs can swap Dai for USDC and make a profit as well.


Quantstamp is proud to have audited the Dai Peg Stability Module.


### DeFi Innovation Continues


While the crypto market rages on, the DeFi ecosystem continues to aggressively innovate and Maker continues to demonstrate itself as an influential leader. Maker has impacted the DeFi ecosystem since the beginning, and we will continue to feel Maker’s impact as DeFi slowly merges with traditional finance. Quantstamp has secured over $45 billion USD worth of digital assets and we look forward to continuing to secure the future of finance and the assets in your digital nation.
