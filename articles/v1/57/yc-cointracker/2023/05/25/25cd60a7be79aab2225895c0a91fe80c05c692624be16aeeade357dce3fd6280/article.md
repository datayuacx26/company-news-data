---
schema_version: "1.0.0"
document_id: "25cd60a7be79aab2225895c0a91fe80c05c692624be16aeeade357dce3fd6280"
company_key: "yc-cointracker"
company: "CoinTracker"
source_id: "yc-cointracker-news-import-8eddc0d2ffc8"
canonical_url: "https://www.cointracker.com/blog/the-ultimate-tax-guide-for-crypto-staking"
published_at: "2023-05-03T00:00:00+00:00"
first_seen_at: "2026-07-24T23:51:12.353224+00:00"
fetched_at: "2026-08-20T01:03:53.992319+00:00"
content_hash: "sha256:01ca222f30291adce755cd92f93d211a5ab1b30625452cb344ae2ef82be1177a"
---

# Cryptocurrency Staking Tax Guide

## What is staking & how does it work?


Staking is the process of locking up cryptocurrency to secure a Proof of Stake (PoS) blockchain network. In return for locking up your cryptocurrency, you will receive staking rewards. The rewards are determined by randomly assigning the right to validate the next block to participants in the PoS blockchain. The more you stake, the better your odds are of being chosen to validate the next block and the resulting reward. Some examples of PoS networks are Ethereum (officially as of September 15, 2022), Cardano, and Solana.


## Types of cryptocurrency staking


### Self-staking


Self-staking involves operating your own validator node, which secures the PoS network. This process is complex; you may need to be tech-savvy to maintain your node. One of the risks with self-staking is the node going offline. The node may incur a penalty if it goes offline for whatever reason.


### Delegated/Exchange staking


Delegated staking is the most convenient way to stake cryptocurrency and earn staking rewards. This method involves handing over your coins to an intermediary (an exchange, wallet, or another platform) that manages the staking process on your behalf and distributes staking rewards periodically. These intermediaries will keep a percentage of the staking rewards in return for providing you with this service. You can stake your cryptocurrency in popular exchanges & platforms like Coinbase, Kraken, Exodus, and Lido.


Although delegated staking is user-friendly, it is not entirely risk-free. If the intermediary files for bankruptcy or operate a fraudulent staking operation, you could lose your funds. Self-staking and delegated staking can come in two forms: illiquid and liquid staking.


#### Illiquid staking


Most platforms don’t allow you to use the tokens while staked; they are locked and unusable until you unstake. This form of staking is called illiquid staking.


For example, if Jim stakes 10 Solana (SOL) on Exodus, he won’t be able to transfer SOL to another wallet or sell it while staked. He will need to unstake his SOL to start transacting with it again. He doesn’t receive a different token after staking his assets.


#### Liquid staking


Liquid staking is a relatively new development in the crypto space. This method allows you to access liquidity while your assets are staked.


For example, if Jill stakes 10 ether (ETH) on Lido, she receives 10 staked ether (stETH) tokens in return. The stETH is the tokenized version of the ETH you staked. She can sell or utilize the stETH tokens while the original ETH is staked. The ETH stays staked, collecting rewards, while the issued stETH represents a transferable claim on that staked ETH and the rewards.


## Tax implications


Each step of the staking process will have tax complexities. The IRS hasn’t issued any staking-specific tax guidance. Without this tax guidance, we must infer the tax implications of staking by referring to the general tax code.


### Staking assets


The act of staking assets is nontaxable in most illiquid staking cases. The reason is that a disposition event has not occurred.


A disposition event occurs when you meet the following criteria ([Reg §1.1001-1](https://www.law.cornell.edu/cfr/text/26/1.1001-1?ref=blog.ghost.cointracker.io) ):


- There must be an exchange (Exchange requirement) **and** ,
- Property received must differ materially from the property given up (Material difference requirement)


#### Exchange requirement


The exchange requirement, generally speaking, is met when the “tax ownership” of the property is transferred to a new tax owner. There is no direct definition for what constitutes tax ownership. That said, according to tax literature, the party with the “benefits and burdens” of a particular piece of property is the tax owner of that property.


#### Material difference requirement


Again, there’s no clear-cut way to determine if this requirement is met. Questions you can ask are:


- Is the property received substantially different from the property forgone?
- Has the taxpayer's economic position changed meaningfully after the exchange?


**Example:** Chris stakes 100 Tezos (XTZ) on Coinbase. Chris **did not** receive any new tokens representing the staked assets on Coinbase. Even though an exchange may have occurred, he did not receive new tokens that were materially different. Since the material difference requirement was not met, this event won’t be taxable.
However, there could be situations (especially in the case of liquid staking) where you transfer assets to a platform, and the platform gives you another token representing your stake. Although the tax implications here are open to debate, conservatively speaking, the receipt of a new separate token may be deemed a disposition of the original token.


The IRS could compare liquid staking to[Cottage Savings Assn vs. Commissioner.](https://supreme.justia.com/cases/federal/us/499/554/?ref=blog.ghost.cointracker.io) Cottage Savings was allowed to take a tax loss when exchanging participation interests in different mortgage loans. The court deemed that the underlying interests in the loans were materially different enough because they consisted of different debtors and were secured by other homes. This fact pattern was enough for a taxable disposition to have occurred. The IRS cites this case as an example in[CCA 202316008's](https://www.irs.gov/pub/irs-wd/202316008.pdf?ref=blog.ghost.cointracker.io) analysis of protocol upgrades and the exchange requirement. The IRS could note this case when they analyze staking.


**Example** : Chris stakes 1 ETH on Lido and receives 1 stETH. Chris purchased this ETH for $1,000 several years ago. When he trades 1 ETH for 1 stETH, stETH is worth $1,500.


The exchange requirement is met because Chris turns over the ownership of ETH. He owns stETH now, not ETH. The material difference requirement is met as well. ETH and stETH have different prices in the open market and are entirely separate tokens. In addition, stETH gives you new legal entitlements. It allows you to earn additional rewards by participating in liquidity pools and other yield farms that you can’t do with ETH.


If you follow the conservative approach, converting ETH to stETH is a taxable disposition event where Chris has to report $500 ($1,500 - $1,000) of capital gains. The cost basis of the stETH will be $1,500.


Whether locking up assets in a platform is taxable is based on the facts and circumstances of each case. We highly recommend consulting a qualified tax advisor regarding your specific situation.


### Unstaking assets


The act of unstaking is not taxable if a disposition fails to occur when you withdraw the asset.


**Example** : Chris unstakes 100 XTZ on Coinbase and does not trade a different token to receive the XTZ. This step was not a taxable transaction. This transaction is not a disposition since the material difference requirement is unmet.


Some unstaking transactions can lead to a taxable event if considered a disposition.


**Example** : Continuing with the Lido staking example, Chris returns 1 stETH to Lido and redeems the original 1 ETH he staked. ETH is worth $1,700 when he sends his stETH back to Lido. Assuming this is a disposition event (conservative approach), Chris will have a $200 ($1,700 - $1,500 (cost basis from staking example)) capital gain from unstaking the assets.


Taking the same approach when entering and exiting staking positions is important. If you don’t follow the same approach, it could lead to inaccurate tax filings.


### Earning staking rewards


Conservatively speaking, staking rewards are taxed at the time you “constructively” receive income. Based on[Regulation 1.451-2](https://www.law.cornell.edu/cfr/text/26/1.451-2?ref=blog.ghost.cointracker.io) , constructive receipt of income occurs when “it is credited to your account, set apart, or otherwise made available so that you may draw upon it at any time, and there are no substantial limitations or restrictions” If there are no restrictions, you **can** have total dominion & control (D&C) over the rewards. D&C is established when you have the ability to sell or transfer the asset without any restrictions.


For example, Chris stakes 100 XTZ on Coinbase and receives a reward of 0.5 XTZ (worth $10 at the time of receipt). Chris has instant access to these rewards with no restrictions. Here, he has to report $10 as ordinary income. The reported income will be the cost basis when he sells the reward in the future.


#### Joshua & Jessica Jarrett vs. United States


A Tennessee couple (Joshua and Jessica Jarrett) amended their tax return, removed the staking rewards, and received their requested refund. They tried to decline the refund and take the IRS to court. They argued that staking rewards should not be taxable upon receipt because they are newly created property. Newly created property is taxed only at the time of sale under the current tax code. For example, a painter doesn’t pay tax when they paint a new painting; tax is due when they sell the artwork for money.


Although this is a compelling argument, the court dismissed this case recently due to mootness. Basically, the court said there was no point in discussing the case because they had received their refund as requested. Therefore, we don’t recommend relying on this position when reporting your staking rewards.


However, If you want to take this aggressive approach in CoinTracker, you can edit the cost basis to $0 and not report the staking rewards on the Transactions page as income when you file your tax return. If you take this aggressive approach, you wouldn’t report any ordinary income on Schedule 1. You will only pay capital gains tax when you sell your staking rewards. As an example, you have one ETH of staking rewards. When you sell it, the fair market value (FMV) is $2,000. You will have a $2,000 capital gain ($2,000 - $0 = $2,000).


We highly recommend talking to a tax advisor before following this approach.


## Tax forms


Coinbase and other exchanges treat staking rewards as income upon receipt. If you earn more than $600 of staking rewards, you will get a Form[1099-MISC](https://www.irs.gov/pub/irs-pdf/f1099msc.pdf?ref=blog.ghost.cointracker.io) reflecting your annual earnings. You must report this amount on[Schedule 1](https://www.irs.gov/pub/irs-pdf/f1040s1.pdf?ref=blog.ghost.cointracker.io) line z as “Other income.” You must also report staking income even if you don’t receive a 1099-MISC from an exchange.


You will report the capital gains on[Form 8949](https://www.irs.gov/pub/irs-pdf/f8949.pdf?ref=blog.ghost.cointracker.io) and[Schedule D](https://www.irs.gov/pub/irs-pdf/f1040sd.pdf?ref=blog.ghost.cointracker.io) when you sell your rewards.


## What are the tax rates for staking income?


### Ordinary income tax


When you earn staking rewards, they are considered ordinary income and taxed at your effective ordinary tax rate.


[Source Tax Foundation / Internal Revenue Service](https://taxfoundation.org/2022-tax-brackets/?ref=blog.ghost.cointracker.io)


For example, Lisa is a single taxpayer with $1,000 in staking rewards and over $540,000 in other income. Since her income is already over $540,000, any additional income is taxed at 37%. That means the ordinary income tax on her staking rewards is $370 ($1,000 x 37% = $370).


### Capital gains tax


Disposing of staking rewards is subject to capital gains tax. The cost basis is the amount you reported as ordinary income. The short-term (owned < 366 days) capital gains tax rate is the same as your ordinary tax rate. In contrast, long-term (owned > 366 days) capital gains receive preferential tax rates.


For example, Lisa sells 2 XTZ tokens she has owned for over a year. These 2 tokens have a cost basis of $120 ($60 each). She also sells 8 XTZ tokens she has owned for under a year. These 8 tokens have a cost basis of $880 ($110 each).


XTZ’s FMV at the time of sale is $150.


Lisa has a short-term capital gain of $180 ($300 ($150 x 2) - $120 =$180) and a short-term capital gains tax of $67 ($180 x 37% = $67). Lisa also has a long-term capital gain of $320 ($1,200 ($150 x 8) - $880 = $320). Lisa is subject to the highest long-term capital gain tax rate because her income exceeds $445,850. Lisa’s long-term capital gains tax comes to $64 ($320 x 20% = $64).


### Net investment income tax


The Net Investment income tax (NIIT) started on January 1, 2013. It is an additional 3.8% tax on any investment income if your modified adjusted gross income exceeds the threshold.


[Source (IRS)](https://www.irs.gov/newsroom/questions-and-answers-on-the-net-investment-income-tax?ref=blog.ghost.cointracker.io#:~:text=Threshold%20Amount-,Married%20filing%20jointly,%24250%2C000,-Taxpayers%20should%20be)


According to code[Section 1411(c)(1)(A)](https://www.law.cornell.edu/uscode/text/26/1411?ref=blog.ghost.cointracker.io) , the NIIT applies to interest, dividends, capital gains, rental & royalty income, annuities, and passive business income.


Since capital gains are subject to NIIT, when you dispose of staking rewards, the capital gain on the disposal will be subject to NIIT if you exceed the threshold.


Joe has $1,000 capital gains from selling staking rewards and is subject to the NIIT. His NIIT tax on the $1,000 capital gain is $38 ($1,000 x 3.8%).


Generally speaking, staking rewards don’t meet the definition of net investment income as defined by code section 1411, this law which is intended to tax passive investment income. That said, some self-staking individuals could be operating a passive trade or business subjecting the staking rewards to the NIIT. We suggest that you consult a tax advisor on the applicability of this law to staking rewards.


## Simplify your taxes with CoinTracker


Crypto taxes can seem daunting, with multiple accounts and CSV files that may be hard to understand. Tracking staking rewards is challenging - even if you're only dealing with one account! Utilizing CoinTracker can help simplify the process. CoinTracker detects staking rewards, allows you to tag your rewards as staking, calculates the income you earned, and tracks the cost basis of those rewards to make this task manageable.


If you have any questions or comments about crypto taxes, let us know on Twitter[@CoinTracker](https://twitter.com/CoinTracker?ref=blog.ghost.cointracker.io) .


---


[CoinTracker](https://www.cointracker.io/?utm_source=blog) integrates with 300+ cryptocurrency exchanges, 8,000+ cryptocurrencies and makes crypto tax calculations and portfolio tracking simple.


Disclaimer: This post is informational only and is not intended as tax advice. For tax advice, please consult a tax professional.
