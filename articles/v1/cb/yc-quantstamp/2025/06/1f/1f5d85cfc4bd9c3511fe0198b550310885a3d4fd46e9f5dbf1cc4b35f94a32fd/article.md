---
schema_version: "1.0.0"
document_id: "1f5d85cfc4bd9c3511fe0198b550310885a3d4fd46e9f5dbf1cc4b35f94a32fd"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/risks-on-the-farm-how-to-yield-farm-safely"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:0888ac4c03e827efb81d33268ce889cf29c18b98ddf8536127d98deeca23de7b"
---

# Risks on the Farm - How to Yield Farm Safely

“Yield Farming” is on the rise. Users are getting money simply by using their favorite DeFi projects. But Yield Farming isn't just free money - users need to be aware of the Risks on the Farm.


Since Compound started their COMP liquidity mining program, over $500M in crypto-assets flowed into their platform,[according to DeFiPulse](https://defipulse.com/compound) . With eye-popping APRs, it’s no wonder that people are piling into this new craze. But is it safe?


You’ve probably heard the term “High risk, high reward”. With yield farming, this is certainly the case as well. Smart contract risk, liquidation risk, impermanent loss, and composability risk are all things farmers should be aware of, and take precautions against.


### High Risk, High Reward


“There’s no such thing as a free lunch.” This is true in life, and in DeFi as well. When farming DeFi there are both financial and technical risks to be aware of.


### Liquidation Risk


In case you are using a collateralized loan through Compound, MakerDAO or Aave, you need to be aware of your liquidation risk. Liquidation happens when your collateral is no longer enough to cover the amount of your loan, causing you to suffer a liquidation penalty to your collateral. Liquidation can happen if either the value of your collateral drops, or if the value of your loan increases.


To reduce your chance of liquidation, borrow or supply less volatile assets. With volatile assets, the market can move against you, causing liquidation. This can be exacerbated if both your collateral and your loan are volatile. It is better if either the collateral or the loan is a less volatile asset such as WBTC, or a stablecoin like USDC. If both the collateral and the loan are less volatile assets or stablecoins, such as borrowing USDC against DAI Collateral, your liquidation risk will be greatly reduced.


*DeFiSaver's Automation feature can save your MakerDAO Vault from Liquidation*


If you are using MakerDAO to take out a collateralized loan,[DeFiSaver](https://defisaver.com/) can also help you reduce the chance of liquidation. DeFiSaver is an application which uses flash loans along with your collateral to automate the repayment of your loan. To use it, you connect your wallet to DeFi Saver, and set a Boost point on the automation screen. When DeFiSaver detects that your loan has reached that point, it will automatically start the repayment process.


DeFiSaver is very good, but it’s not a guarantee. In case of a severe price drop, or severe network congestion, there is a chance that it may not work correctly. Always maintain a healthy collateral ratio and keep track of the status of your loan.


It’s tempting to borrow as much as you can, but especially when volatile assets are involved, you’re tempting fate. Try to keep healthy borrow ratios in case prices move against you. Compound Finance shows this as your loan being in the green zone. For MakerDAO, most users try to keep a 200 or 250% collateralization ratio or sometimes even more.


### **Impermanent Loss**


Besides earning tokens for supplying or borrowing liquidity, some farmers will supply their funds into liquidity pools to earn yield. These pools, popularized by Uniswap, allow users to collect trading fees from decentralized exchanges. This can be a source of market-neutral returns that helps users to offset the risk of price fluctuations in their portfolio.


*Impermanent Loss on Uniswap- source:*[Uniswap documentation](https://uniswap.org/docs/v2/advanced-topics/understanding-returns/)


Once again, there is no free lunch. While supplying liquidity on Uniswap or other AMMs can prove lucrative in some situations, if the price moves too much, you can actually lose money compared to holding the underlying assets. This concept, dubbed[Impermanent Loss](https://medium.com/@pintail/understanding-uniswap-returns-cc593f3499ef) , is non-intuitive but important for liquidity providers to understand. You can find more details about Impermanent Loss[here](https://uniswap.org/docs/v2/advanced-topics/understanding-returns/) .


To avoid issues with impermanent loss, liquidity providers should choose the pools they enter wisely, and also consider using protocols besides Unsiwap such as Curve or Balancer.


[Curve](https://www.curve.fi/) only trades assets which trade within a tight band with each other. This includes stablecoins or stable-pairs such as WBTC with SBTC and renBTC. Since these assets generally do not move much in price relative to each other, impermanent loss is reduced to be almost negligible.


Balancer is another project which also can be used to address impermanent loss. While Uniswap uses 50/50 pools, Balancer allows other weights. By constructing a heavily weighted 90/10 pool in favor of your favorite asset, you can retain most of the upside in case it shoots up in price.


### Smart Contract Risk


*Source:*[Anthony Sassano](https://twitter.com/sassal0x)


Yield farming requires supplying assets to smart contracts for lengthy amounts of time. If those smart contracts are successfully attacked, your funds could be compromised. This is not a theoretical scenario. Earlier this year, dForce, a lending platform,[was attacked for $25M](https://quantstamp.com/blog/how-the-dforce-hacker-used-reentrancy-to-steal-25-million) .


DeFi users need to especially care about smart contract risk because of composability. Composability is the ability for DeFi applications to leverage other DeFi applications for additional functionality.


For example, it is possible to combine COMP farming with BAL farming by supplying Compound cTokens into Balancer pools. However doing so means that yield is experiencing smart contract risk from both Compound and Balancer. These projects are both audited, which reduces risk - just be aware that audits do not completely eliminate the risk of a hack.


*Audits reduce smart contract risk*


The best way to mitigate smart contract risk is to look for audits from reputable companies. Look for audits from firms that have audited a large number of projects with a good track record.


When you see a project has been audited, actually click through and read the audit report. Did the auditors have time to thoroughly look over the code? Was it a single engineer looking at the code or did multiple people look at the code? Were the concerns raised addressed? Did the project have good code quality, testing and follow best practices?


Look for projects that audit their whole codebase, give time for auditors to check the code properly, and projects that audit new features and updates as they arise.


### Farming Safely


With projects subsidizing usage, it’s tempting to go all out with yield farming, but you need to be aware of the risks. Maximizing your loan will give you a higher yield - but it will also wipe out all your gains, and then some, if you get liquidated.


While there are risks in Yield Farming, there are many things that users can do to manage and mitigate them. Keeping healthy borrow ratios, borrowing/lending non-volatile assets, and reducing their smart contract risk can help.


Perhaps the single most effective step Yield Farmers can take to address smart contract risk is to choose projects that take security seriously. Look for projects that have audits from reputable firms, publicize it openly, and audit new features as they are added as well.
