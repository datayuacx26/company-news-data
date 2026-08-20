---
schema_version: "1.0.0"
document_id: "1558264b287f48b2d4df56adbbb4b120011d47f203561f040a0e6206c6bfbd2d"
company_key: "yc-beanstalk"
company: "Beanstalk"
source_id: "yc-beanstalk-news-import-a3f4f49fe20d"
canonical_url: "https://bean.money/blog/a-framework-for-stablecoins"
published_at: null
first_seen_at: "2026-07-24T19:21:00.865118+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:6ffebedc00bbd3b9660cf5a48ffd739a5e78a7a79c97d28ea950e8b066755453"
---

# A Framework for Stablecoins

## What is a Stablecoin?


A stablecoin is a fungible asset intended to maintain low price volatility relative to an arbitrary value peg. Oftentimes, stablecoins are pegged to the price of an off-chain currency. A currency’s utility is a function of:


- Trustlessness;
- Carrying costs;
- Low volatility; and
- Liquidity.


Low volatility is essential to the utility of a currency. Without looking at the theoretical reasons for why this is the case, the market has clearly demonstrated its value. Since its inception with the creation of Tether – a historically very[low volatility asset](https://coinmarketcap.com/currencies/tether/) – in 2014, the stablecoin industry has grown from a fringe backwater to a[$150B market cap](https://www.statista.com/statistics/1255835/stablecoin-market-capitalization/) decentralized finance ("DeFi") behemoth.


The history of money tells a story of markets seeking out the least volatile assets to use as currency. The relatively brief history of DeFi tells the same story.


## What is the Problem with Current Implementations of Stablecoins?


While the market has demonstrated clear demand for low volatility blockchain-native assets, the vast majority of current implementations suffer from a variety of risks and drawbacks, primarily with regard to a lack of trustlessness, negative carrying costs and thin liquidity. In other words, the only utility that has been created by current stablecoin implementations is low volatility. The absence of a decentralized currency with competitive carrying costs and deep liquidity is the main issue – in addition to high blockchain transaction costs – holding businesses producing real economic activity back from adopting decentralized financial primitives.


Businesses built exclusively on decentralized primitives have struggled to compete with businesses built on centralized financial systems like fiat USD because DeFi lacks a trustless and liquid currency with sufficiently low price volatility and competitive carrying costs.


For example, historically,[borrowing rates on USD stablecoins](https://app.aave.com/markets/) have significantly exceeded[borrowing rates on fiat USD](https://www.newyorkfed.org/markets/reference-rates/effr) . Until network-native borrowing rates are comparable to or lower than off-chain borrowing rates, DeFi will continue to struggle to attract meaningful economic activity away from TradFi.


## Stablecoin Features


Current implementations of stablecoins can be conceptualized along the following three axes: value source, convertibility and network nativity.


**1. Value Source: Exogenous value vs. Endogenous value**


Stablecoin value can come from exogenous assets or endogenous value creation.


A stablecoin with *exogenous* value is one whose value is derived from another asset or basket of assets which are held by the issuer as collateral. Reliance on exogenous value generates opportunity costs associated with collateral requirements, which creates non-competitive carrying costs.


A stablecoin with *endogenous* value is one whose value is derived from demand for the currency. Endogenous value has the potential to facilitate the creation of trustless currency with low price volatility, competitive carrying costs and deep liquidity.


**2. Convertibility: Convertible vs. Non-convertible**


Convertibility is the option to exchange an asset for its underlying collateral.


A *convertible* stablecoin protocol is one that facilitates convertibility between its stablecoin and its underlying collateral. Arbitrage opportunities created by convertibility ensure that the stablecoin price is rarely above or below the value of the underlying collateral, once frictions around convertibility are accounted for. Convertibility comes at the expense of low liquidity and non-competitive carrying costs because of the opportunity cost associated with locking up collateral to facilitate it.


A *non-convertible* stablecoin protocol is one that does not facilitate convertibility between its stablecoin and any form of underlying collateral. It is impossible to keep a stablecoin price equal to its value peg at all times without low-friction convertibility. Non-convertible stablecoin protocols without collateral requirements have the potential to create endogenous value that facilitates trustlessness, competitive carrying costs and deep liquidity at the expense of volatility.


**3. Network Nativity: Network-native vs. Non-network native**


Network nativity refers to the source of a stablecoin’s value with regard to its network.


A *network-native* stablecoin is one that derives its value exclusively from the network to which it is native. Network-nativity eliminates most points of centralization from a stablecoin’s value chain.


A *non-network-native* stablecoin is one that does not exclusively derive its value from the network to which it is native. Non-network-nativity requires a custodian that facilitates a bridge to the network, which can introduce significant costs, complexities and points of centralization.


The three axes above present a classification of eight potential stablecoin types, some of which currently lack successful implementations:


- Non-network-native exogenous value convertible stablecoin protocols;


- *Examples: USD Coin, Tether, BinanceUSD*


- Network-native exogenous value convertible stablecoin protocols;


- *Examples: wETH, Liquity, MakerDAO, Abracadabra*


- Non-network-native exogenous value non-convertible stablecoin protocols;


- *Examples: N/A*


- Network-native exogenous value non-convertible stablecoin protocols;


- *Examples: Olympus*


- Non-network-native endogenous value convertible stablecoin protocols;


- *Examples: N/A*


- Network-native endogenous value convertible stablecoin protocols;


- *Examples: Terra*


- Non-network-native endogenous value non-convertible stablecoin protocols; and


- *Examples: US Dollars*


- Network-native endogenous value non-convertible stablecoin protocols.


- *Examples: Beanstalk, Ampleforth*


Additionally, implementations of exogenous value convertible stablecoin protocols with fractional reserves are considered:


- Non-network-native exogenous value fractional reserve convertible stablecoin protocols; and


- *Examples: Bank Notes*


- Network-native exogenous value fractional reserve convertible stablecoin protocols.


- *Examples: Frax*


## Exogenous Value Stablecoins


**Non-network-native exogenous value convertible stablecoin protocols**


*Examples:[USD Coin](https://www.circle.com/en/usdc) ,[Tether](https://tether.to/en/) ,[BinanceUSD](https://www.binance.com/en/busd)*


Non-network-native exogenous value convertible stablecoin protocols issue tokens that are supposedly collateralized by, and require a custodian that facilitates the convertibility to, non-network-native exogenous value worth at least 100% of total outstanding protocol liabilities. These protocols function as low-volatility permissioned bridges between their respective networks and the rest of the world.


Users of non-network-native exogenous value convertible stablecoins sacrifice trustlessness and carry: third parties custody the non-network-native assets, can freeze the network-native assets unilaterally and can retain yield earned on collateral. The absence of protocol-native opportunities for carry limits liquidity.


*Trustlessness:* Permissioned


*Carrying Costs:* Non-competitive as compared to the asset being pegged to, but best among exogenous value convertible stablecoins


*Volatility:* Best among exogenous value stablecoins, but frictions around convertibility remain significant, leading to imperfect peg maintenance


*Liquidity:* Best among exogenous value stablecoins, but limited by opportunity cost of using off-chain collateral as capital


**Network-native exogenous value convertible stablecoin protocols**


*Examples:[wETH](https://weth.io/) ,[Liquity](https://www.liquity.org/) ,[MakerDAO](https://makerdao.com/) ,[Abracadabra](https://abracadabra.money/)*


Network-native exogenous value convertible stablecoin protocols use excess network-native collateral to remove most points of centralization. Overcollateralization removes most risk associated with the volatility of the collateral but by necessity requires the introduction of rent payments in order to prevent the value of the stablecoin from trending towards the value of the underlying collateral. The combination of collateral requirements and rent payments significantly limits the potential liquidity of these stablecoins.


[Liquity](https://www.liquity.org/) is an ideal simple iteration of this type of stablecoin protocol, without any points of centralization and with protocol-native positive carry (via[the Stability Pool](https://docs.liquity.org/faq/stability-pool-and-liquidations) ). In order to remove rent payments, Liquity does not target an exact price for its stablecoin, LUSD. The potential supply of LUSD is limited by the value of trustless network-native value.


*Trustlessness:* Can be totally permissionless; collateral distribution is the primary determinant of trustlessness


*Carrying Costs:* Non-competitive as compared to the asset being pegged to, unless at the expense of volatility


*Volatility:* Very low volatility when factoring in frictions around convertibility, unless in order to improve the competitivity of carrying costs


*Liquidity:* Limited by available network-native collateral, particularly trustless network-native collateral


**Non-network-native exogenous value non-convertible stablecoin protocols**


*Examples: N/A*


Convertibility is the main source of low price volatility and confidence in the custodian. Therefore, it is unlikely that a non-network-native exogenous value non-convertible stablecoin protocol will find product market fit.


*Trustlessness:* Permissioned


*Carrying Costs:* Non-competitive as compared to the asset being pegged to


*Volatility:* Speculation on the trustworthiness of the custodian of collateral will be higher due to the lack of convertibility, which should lead to more price volatility


*Liquidity:* Limited by opportunity cost of using off-chain collateral as capital, but less so because the custodian is unencumbered by the limited asset allocations necessary to facilitate convertibility


**Network-native exogenous value non-convertible stablecoin protocols**


*Examples:[Olympus](https://www.olympusdao.finance/) (convertible under certain market conditions)*


Convertibility is the main source of low price volatility. The main benefit of collateralization is low price volatility, the majority of which is forfeited in the absence of convertibility. Therefore, it is unlikely that a network-native exogenous value non-convertible stablecoin protocol will find product market fit.


*Carrying Costs:* Non-competitive as compared to the asset being pegged to, but potentially better than network-native exogenous value convertible stablecoin protocols because of the ability to take more risk with the underlying collateral at the cost of volatility


*Volatility:* There exists some tradeoff between carrying costs and volatility, dependent on protocol design


**Non-network-native exogenous value fractional reserve convertible stablecoin protocols**


*Examples: Bank Notes (not implemented on any decentralized networks to date)*


Credit-based non-network-native exogenous value convertible stablecoin protocols most closely resemble traditional bank notes. The introduction of fractional reserves to create competitive carrying costs is a well documented historical phenomenon. The lending out of collateral creates yield that can be passed onto the users of the fractional reserve notes. However, the combination of convertibility and fractional reserves creates an asset liability duration mismatch that can lead to complete collapse. Fractional reserves are another example of currencies trading volatility for carry.


*Trustlessness:* Permissioned


*Carrying Costs:* Competitive as compared to the asset being pegged to, at the expense of tail risk in volatility


*Volatility:* Low volatility when factoring in frictions around convertibility; exposure to bank runs


*Liquidity:* Limited by available collateral


**Network-native exogenous value fractional reserve convertible stablecoin protocols**


*Examples:[Frax](https://frax.finance/)*


The use of fractional reserves by network-native exogenous value convertible stablecoin protocols has been used in an attempt to help remediate the negative effect on carrying costs of collateral requirements. However, the network-native nature of the system makes lowering the collateralization ratio difficult without spurring a bank run. While fractional reserve implementations of network-native exogenous value convertible stablecoins offer competitive carrying costs as compared to non-fractional reserve implementations, they do so at the cost of tail risk in volatility.


*Trustlessness:* Can be totally permissionless; collateral distribution is the primary determinant of trustlessness *Carrying Costs:* Can be competitive as compared to the asset being pegged to at lower collateralization ratios *Volatility:* Low volatility when factoring in frictions around convertibility; exposure to bank runs *Liquidity:* Limited by available network-native collateral, particularly trustless network-native collateral


**Summary of Exogenous Value Stablecoins**


Convertibility to exogenous value is the primary source of low price volatility, but it comes at the cost of competitive carrying costs and liquidity. In most implementations, trustlessness is sacrificed for deeper liquidity.


Despite the shortcomings of exogenous value convertible stablecoin implementations, demand for their USD implementations continues to increase rapidly. Over the twelve months prior to the initial deployment of Beanstalk, the total market capitalization of exogenous value convertible USD stablecoins[increased more than 500% to over $100 billion](https://stablecoinindex.com/marketcap) .


However, despite this rapid increase in supply, the[borrowing rates on exogenous value convertible USD stablecoins](https://app.aave.com/markets/) have historically been higher than[borrowing rates on USD](https://www.newyorkfed.org/markets/reference-rates/effr) . Non-competitive carrying costs are structural due to the opportunity costs associated with requirements of exogenous value collateral. Attempts to improve carrying costs through the introduction of fractional reserves have failed to do so.


Businesses built on trustless primitives cannot compete with businesses built on centralized systems due to non-competitive carrying costs on low-volatility network-native trustless assets.


## Endogenous Value Stablecoins


**Non-network-native endogenous value convertible stablecoin protocols**


*Examples: N/A*


It is difficult for network-native protocols to create and facilitate the conversion to non-network-native value. Therefore, it is unlikely that non-network-native endogenous value convertible stablecoin protocols find product market fit.


In theory, this would be like a private company issuing a stablecoin that is convertible to company stock at market price.


*Trustlessness:* Permissioned


*Carrying Costs:* Can be competitive as compared to the asset being pegged to, but the issuer must create more than enough endogenous value to facilitate the carry


*Volatility:* Likely to maintain a tight peg due to convertibility, but exposed to collapse in the instance the issuer’s endogenous value source falters, or is expected to falter


*Liquidity:* Limited by the endogenous value of the issuer


**Network-native endogenous value convertible stablecoin protocols**


*Examples:[Terra](https://www.allcryptowhitepapers.com/terra-whitepaper/)*


To date, implementations of network-native endogenous value convertible stablecoin protocols have failed to regularly cross their stablecoin price over their value peg. This is primarily due to the fluctuation in value of endogenous value. The convertibility to endogenous value typically correlates with decreases in endogenous value, which creates excess reflexivity. It is unclear whether network-native endogenous value convertible stablecoin protocols can succeed.


*Trustlessness:* Permissionless


*Liquidity:* Limited by the endogenous value of the issuer


**Non-network-native endogenous value non-convertible stablecoin protocols**


*Examples: US Dollars*


The current financial system (i.e., fiat money) can be thought of as an implementation of credit-based non-network-native endogenous value non-convertible stablecoin protocols, where the fiat money is pegged to a time-weighted discount of its current purchasing power. The non-network-native nature of the credit of the issuer introduces significant sacrifices to trustlessness.


*Trustlessness:* Permissioned *Carrying Costs:* Very competitive *Volatility:* Determined by a variety of factors; sufficiently low volatility is a prerequisite to adoption *Liquidity:* Limited by the creditworthiness of the issuer


**Network-native endogenous value non-convertible stablecoin protocols**


*Examples:[Beanstalk](https://bean.money/) ,[Ampleforth](https://www.ampleforth.org/)*


Rebasing stablecoin protocols (e.g.,[Ampleforth](https://www.ampleforth.org/) ) have shown efficacy at crossing stablecoin prices over their value pegs, but without the regularity, low volatility or liquidity necessary to create utility. Extreme negative carrying costs during decreases in demand exacerbate difficulty of use.


The value of fiat currency is derived from its utility and the credit of its issuer. Utility of fiat currency is a function of trustlessness, carrying costs, stability and liquidity. Decentralized credit can be used to issue a permissionless fiat stablecoin with competitive carrying costs, low volatility and deep liquidity.


Credit-based network-native endogenous value non-convertible stablecoin protocols are network-native implementations of fiat currency. The value of such assets derive from the credit of its issuer, which exists in a network-native capacity. The network-native nature of the credit creates value in a trustless fashion. The creation of endogenous value through credit facilitates competitive carrying costs. A strong series of network-native incentives and diverse network of creditors can create price stability and deep liquidity without collateral requirements.


Credit-based network-native endogenous value non-convertible stablecoin protocols present an opportunity to achieve trustlessness, competitive carrying costs, low price volatility and deep liquidity. The use of network-native credit can eliminate the negative carrying costs and reduce the excess price volatility associated with rebasing stablecoin protocols.


## Summary


Convertibility to endogenous value prioritizes low price volatility at the expense of long term sustainability.


Rebasing tokens have shown some efficacy and peg maintenance, but not enough to create utility. The introduction of network-native credit can reduce volatility.


The current financial system runs on credit-based non-network-native endogenous value non-convertible stablecoin protocols.


Implementations of network-native endogenous value non-convertible stablecoin protocols present a viable option for the creation of trustless currency with competitive carrying costs, low price volatility and deep liquidity that can facilitate the competition between businesses built on decentralized primitives against businesses built on centralized ones.
