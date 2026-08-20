---
schema_version: "1.0.0"
document_id: "ec884a12e04ed4a165a0101d393b92b4ba411ed75ed1cb00c565cc420258a08a"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/defis-double-edged-sword"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:472b2b77a9ef196e7ed8094e6e97aa1811767c26aca3d05801fa409f980488bd"
---

# DeFi’s Double-Edged Sword

Composability allows for DeFi projects to leverage one another to create powerful new functionality. However this composability also introduces more risk.


A vulnerability in one DeFi application can have an impact on all the other projects that use it. Even if one of these money legos does not have an obvious vulnerability in its design, it may be mis-used. These issues have existed for a while, but have been made easier to exploit through the introduction of flash loans.


While flash loans themselves do not introduce new vulnerabilities, they level the playing field for attacks which previously required large amounts of capital.


### The bZx Attacks


Recently, there were two attacks involving the bZx protocol facilitated by flash loans.


In the first attack, a bug in bZx’s margin logic allowed the attacker to use leveraged ETH to artificially push up the price of WBTC on Uniswap. The attacker then used WBTC previously borrowed at market value from Compound to profit from the inflated price on Uniswap.


In the second attack, bZx relied on Uniswap(through KyberSwap) as a price oracle for valuing collateral within the bZx system. After pushing up the value of sUSD, this allowed the attacker to take out a loan that was valued way more than the market value of sUSD collateral. The attacker profited by keeping the loan and abandoning the bZx position.


For more reading on these vulnerabilities we recommend reading Peckshields’ walkthroughs of the[first](https://medium.com/@peckshield/bzx-hack-full-disclosure-with-detailed-profit-analysis-e6b1fa9b18fc) and[second attacks](https://medium.com/@peckshield/bzx-hack-ii-full-disclosure-with-detailed-profit-analysis-8126eecc1360) .


### Flash Loans Reduce the Barrier of Entry for Financial Attacks


Before the introduction of flash loans, these financial attacks still existed, but they required access to large financial reserves to be profitable. Now, anyone with the technical capability can pull off attacks such as market manipulation which usually require large amounts of funds.


In December of 2019,[a similar type of financial attack](https://www.reddit.com/r/ethfinance/comments/eexbfa/daily_general_discussion_december_24_2019/fby3i6n/) was performed on the Synthetix exchange by a hacker who manipulated the price of MKR while holding directional trades on Synthetix. This attack required about $340,000 ($62,600 in MKR, $163,125 in long MKR, $115,275 in short MKR). If the attacker had used smaller amounts, he wouldn’t have been able to move the price much relative to the liquidity in the Uniswap pool, and he wouldn’t have made much money. In comparison to that attack which required hundreds of thousands of dollars, the bZx hacks which used flash loans required just $8 in transaction fees in the first case, and $110 in the second case.


These attacks and even flash loans have parallels in traditional finance. Just like there are flash loan attacks in DeFi, traditional finance also has similar tactics. When I was working at Tower Research, each trading desk was allocated a pool of capital, but if we saw a particularly juicy opportunity, we could borrow tens of millions of dollars to take advantage of it.
‍


For the bZx attacks, from a trading perspective they can be viewed as arbitrage opportunities between the rate of assets on one platform, and the rate after causing massive slippage on a DEX. The attacker is using flash loans to close that arbitrage.


The other aspect of flash loans which is underappreciated is it makes it easier to profit from these attacks because it reduces the amount of illicit funds which need to be obfuscated.


A financial attack like this is basically illegal, so to pull it off, an attacker needs to obfuscate the origin of their funds. As exchanges now almost all uniformly implement KYC, this can be quite difficult for any attack requiring a large amount of capital.


Now, with flash loans, attackers have instant access to a large pool of capital which is returned at the end of the attack, so the only funds they need to obfuscate is the gains from the exploit. This vastly simplifies the logistics for executing and profiting from an attack.


Previously when we advised clients on possible financial attack vectors, it was mostly theoretical, but flash loans now make them much more likely and accessible.


### Security Can’t be an Afterthought


Move slow, and test things. For all smart contracts but especially for DeFi applications, security best practices need to be taken, including audits, testing, monitoring, and having emergency procedures ready. Progressive rollouts are also a good idea so that security issues can be spotted when the cost of an attack is still relatively small.


Besides auditing the code for bugs and implementation issues, financial attacks such as oracle manipulation also need to be analyzed. This is especially true as DeFi grows and these money legos handle more assets.


### Financial Attacks Should Be Part of Smart Contract Audits


People say that the bZx attacks are primarily financial attacks outside the scope of smart contract audits, but I disagree. Even considering the second attack which did not take advantage of a missing sanity check, oracle attacks and economic vulnerabilities should absolutely be considered as part of smart contract audits.
With


DeFi especially, the composability of these apps means that financial attack vectors need to be carefully considered.


### Growing Pains


While these attacks have highlighted weaknesses in some of the current DeFi systems, they also show us what needs to change for DeFi to grow stronger. We believe the industry will learn from these attacks and develop a better security culture as a result.


We also want to thank the work of altruistic actors such as[Samczsun](https://samczsun.com/taking-undercollateralized-loans-for-fun-and-for-profit/) . His discoveries, research, and collaborations with DeFi projects have helped teams proactively address security issues and have helped the industry improve its security practices.


‍


*This article originally appeared in*[The Defiant](https://defiant.substack.com/) *, DeFi's top daily newsletter.*


--


For more Quantstamp news or anything QSP crypto or QSP coin related, check out Quantstamp[Reddit](https://www.reddit.com/r/Quantstamp/) and QSP[Twitter](https://twitter.com/quantstamp) .
