---
schema_version: "1.0.0"
document_id: "2faceee96a658e229d55fbc3d4ea8e6db994b4cf201c5ae244547c1cc1d03496"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/market-dynamics-of-the-1st-bzx-hack-part-1"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:3de594be58743f3701a13a299d991a14f8f7b7e1bc67783ee141ef32d5dcb38e"
---

# Market Dynamics of the 1st bZx Hack: Flash Loans and the Insolvent Loan

# Part 1 : Flash Loans and the Insolvent Loan


DeFi composability is enabling the creation of powerful applications, but it also introduces new security risks. Although a logical bug was exploited in the first bZx hack, the hack would not be successful without also manipulating markets. In this series, we describe the market dynamics of the 1st bZx attack so we can avoid attacks with market manipulation components in the future.


Part 1 will explore flash loans and the insolvent bZx position created by the attacker.


‍


## Understanding the Overarching Strategy


Before diving into the details of the attack, we first need to understand the attacker’s overarching strategy.


At a high level, the attacker took advantage of a bug in the bZx protocol in order to steal ETH belonging to bZx in a roundabout way. The bug allowed the attacker to create an arbitrage opportunity in the wBTC/ETH Uniswap market using leveraged ETH from bZx. The attacker understood that by opening a leveraged wBTC position on bZx, bZx would purchase the wBTC from Uniswap using leveraged ETH. This would cause wBTC to become overvalued in that specific market. The attacker then profited from this opportunity by trading wBTC that was acquired from Compound at market prices for ETH. This all took place within a single transaction.


Now let’s start from the beginning and dive into the details.


‍


## Financing the Attack with a Flash Loan


Flash loans require 0 collateral, and they are both lent out and paid back within the same transaction.


‍


The attack began with a flash loan of 10,000 ETH from dYdX.


A flash loan is a loan that is both lent and paid back within the same transaction (hence the term flash loan). These loans do not require the borrower to submit any collateral because the lender -- in this case, dYdX -- does not face any risk of the borrower defaulting. Normally, loans require collateral to keep trust-minimized systems solvent in the event that a borrower defaults. With flash loans, borrowers are unable to default because, if the loan is unable to be paid back within a single transaction, the transaction fails and the Ethereum state reverts. From the perspective of Ethereum, this loan never took place.


After initiating the flash loan, the attacker sent 1300 ETH to bZx and 5500 ETH to Compound.


‍


## Preparing for arbitrage on Compound


No vulnerabilities were exploited on Compound. 4,300 of the 5,500 ETH sent to Compound was used to borrow 112 wBTC at market rate. The attacker will later use this wBTC to take possession of stolen ETH. We will discuss this more in Part 2 and 3 of this series.


‍


## Making bZx insolvent


### How is a short with 5x leverage supposed to work?


The 1300 ETH sent to the bZx protocol was used to open a 5x leveraged short on ETH in favor of wBTC.


When you short ETH in favor of wBTC, you are betting that the value of wBTC will rise in relation to ETH: in other words, you are borrowing wBTC because you aim to sell it for more ETH in the future. The attacker also borrowed with 5x leverage. This means the protocol lent the attacker extra ETH so that he or she could acquire even more wBTC.


‍


### How is the system intended to stay solvent with margin loans?


In order for any protocol to provide loans in a trust minimized manner, it must have a system to keep itself solvent. When a protocol is solvent, we are saying that the protocol always controls enough borrower collateral to pay off borrower debt in the event that the borrower does not pay back the loan.


#### How does a protocol stay solvent during an open 5x leveraged position?


On bZx, it was *intended* to work like this: when a user shorts ETH in favor of wBTC with 5x leverage, they first transfer ETH to bZx, which bZx holds as collateral. bZx then provides the users with wBTC equivalent to 5 times the value of the user’s collateralized ETH. The user’s debt is denominated in ETH because bZx uses ETH to acquire the wBTC.


Although the user technically borrowed this wBTC, they do not have complete control over it. bZx also holds this wBTC as collateral along with the ETH the user submitted. bZx does this to stay solvent. Two scenarios can happen from here:


1. If the value of ETH falls in relation to wBTC, the user can sell for a profit.


1. If the value of wBTC falls enough in relation to ETH, some of the user's position gets liquidated (aka sold to pay off debt).


Remember, bZx must always hold enough collateral to completely clear a borrower's debt. bZx liquidates positions in order to ensure that they can always pay off borrower debt with their collateral. In bZx, borrowers must overcollateralize their loan by 20% (aka the collateralization ratio is 120%).


When the collateralization ratio falls below 120%, bZx liquidates (aka sells) borrower collateral in order to pay off debt. The 20% overcollateralization provides a buffer for the liquidation process. This gives the protocol enough time to sell off collateral in the event that the value of the collateral is dropping quickly.


At least this is how bZx was supposed to work.


This entire system failed as soon as the attacker took out the leveraged position.


‍


### The bug that burned bZx


When the attacker opened their position, the attacker’s goal was to cause massive slippage on the wBTC/ETH market on Uniswap using leveraged ETH from the bZx protocol. The attacker was able to successfully create this slippage because they were aware of 4 things:


1. bZx would acquire the wBTC that would be loaned to the attacker from the wBTC/ETH Uniswap market. Although loaned to the attacker, this would also be held as collateral by bZx.
2. Liquidity on the wBTC/ETH Uniswap market was low. Low liquidity markets are vulnerable to slippage.
3. A bug would prevent bZx from taking the massive slippage into account before finalizing the loan. Detecting slippage is important because it ensures that the protocol is correctly valuing the underlying collateral.
4. If the borrower went 5x with lots of collateral, bZx would use a huge amount of Ether to buy wBTC on Uniswap.


When the attacker initiated the loan, the bug occured when bZx miscalculated the value of the borrowers collateral. bZx authorized the attackers loan because they failed to check for slippage after buying 51 wBTC with 4338 Ether.


The position was instantly insolvent. After receiving the loan, the attacker’s collateral (51 wBTC and 1300 ETH) was valued at approximately 888,000 USD at the time. The value of the debt was approximately 1,218,154 USD. The position was underwater by approximately 330,154 USD.


‍


### Look out for Part 2


In Part 1, we learned about how the attacker created an arbitrage opportunity in the wBTC/ETH Uniswap market using bZx’s money. The attacker would later capitalize on this opportunity. In part 2, we explain how the attacker retrieved this value, and more about how price and arbitrage works on Uniswap.


See you next week.


‍


--


For more Quantstamp news or anything QSP crypto or QSP coin related, check out Quantstamp[Reddit](https://www.reddit.com/r/Quantstamp/) and QSP[Twitter](https://twitter.com/quantstamp) .


‍
