---
schema_version: "1.0.0"
document_id: "9c16a5f84376b59a4e7ece115243564bb68d6923ba0ab0e92ad70f3ecf394bc1"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/quantstamp-stablecoin-case-study-formally-verifying-hedera-hashgraphs-stablecoin-framework"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:dfd4638dfd9440004b5f16cbfd4f90cbbd5173ffe6399347a96642ac9bad2d1e"
---

# Formally Verifying Hedera Hashgraph's Stablecoin Framework

Since 2017, over a quarter billion USD worth of digital assets have been lost or stolen due to errors in smart contract development. In order for enterprises to be comfortable leveraging the benefits of stablecoins, rigorous software verification and extensive auditing should be conducted. Quantstamp, the leader in blockchain security, developed a formal specification for stablecoins built on Hedera Hashgraph and deployed using the Hedera Consensus Service, rather than smart contracts or the Hedera Token Service, in order to provide security guarantees for all stablecoins on the platform.


The formal verification of a specification is one of the most comprehensive ways to ensure software performs as intended. Formal verification is used to secure software whose successful operation keeps people alive and / or is responsible for hundreds of millions of dollars worth of capital. For example, formal verification is used by NASA for code operating space flights as well as in the operation of commercial flights. Stablecoins also need formal verification because the various mechanisms that control them currently manage over 17 billion USD worth of digital assets.


Quantstamp’s experience includes securing over 5 billion USD worth of digital assets and working with over 140 startups, foundations, and enterprises. Quantstamp chose to develop the Hedera Hashgraph stablecoin standard in the[K framework](https://github.com/kframework/) because it is robust, extensible, and widely adopted amongst developers.


The framework shortens development time, provides formal guarantees about the correctness of functions, removes the need to develop new tests, and creates a shared understanding amongst developers concerning the architecture and functionality of implemented stablecoins. When stablecoins follow a secure framework, the framework facilitates the integration of implemented stablecoins into other financial applications. This type of modularity is the ideal environment to encourage the development of a healthy stablecoin ecosystem for enterprises.


Composability allows developers to create powerful financial applications by allowing smart contracts to easily integrate with one another. While the benefits of composability are clear, when smart contracts integrate with one another, they may inherit vulnerabilities from each other. By standardizing stablecoins built on Hedera Hashgraph, developers considering integrating a stablecoin implementation into their platforms have certain security guarantees that allow them to quickly evaluate the safety of the stablecoin and integrate the stablecoin into their platform.


### **Background**


Developers and organizations have multiple ways to approach[smart contract scaling](https://hedera.com/blog/hedera-technical-insights-hedera-consensus-service-scaling) to build decentralized applications with the trust of a public network, and the privacy and controls of a permissioned network using the *Hedera Consensus Service* . Previously Hedera discussed ways to explore[tokenized assets on Hedera](https://hedera.com/blog/exploring-tokenized-assets-on-hedera-consensus-service-part-1) , including[mapping an erc20 token](https://hedera.com/blog/exploring-tokenized-assets-on-hedera-consensus-service-part-3) to HCS.


In this blog we’ll be applying this logic to an update in the[Hedera Stablecoin](https://github.com/hashgraph/hedera-stable-coin) Github repo. Hedera partnered with Quantstamp to build an HCS-based stablecoin[demo](https://github.com/hashgraph/hedera-stable-coin-demo) application and associated[Formal Model](https://github.com/hashgraph/hedera-stable-coin-formal-model/blob/master/README.md) to accelerate time to value for stablecoin issuers.


‍


### **Motivation**


Hedera and its ecosystem partners set out to build an enterprise stablecoin, which could achieve thousands of transactions per second with low latency and ABFT security. The stablecoin codified a set of roles and behaviors including supply management, regulatory compliance, and decentralized access in line with enterprise and consumer scale requirements.


A stablecoin is a type of digital token that is pegged to the value of another asset. The asset can be a fiat currency, a cryptocurrency, a basket of fiat or cryptocurrencies, or any other asset. The peg can be maintained via off-chain collateral holding or algorithmic methods to control token and collateral supply.


Stablecoins present a compelling opportunity for consumers and businesses to access and use a digital token which provides price protection compared to tokens which may fluctuate in value. It can also operate with low fees, low latency, and high performance when built on Hedera. This opens opportunities for use in retail, B2B, and cross-border payments as well as a new industry of Decentralized Finance (DeFi).


Stablecoins can be extended to enable issuer based discount networks, peer-to-peer payments, retailer based onramps and offramps, similar to cashback for debit transactions, as well as programmable payments based on digitally recognized events.


‍


### **Approach**


Hedera and its ecosystem partners began with a[specification](https://github.com/hashgraph/hedera-stable-coin/blob/master/Token%20Specification.pdf) , based on the[Interwork Alliance](https://www.businesswire.com/news/home/20200602005297/en/InterWork-Alliance-Launches-to-Standardize-Token-Powered-Ecosystems-Worldwide) (IWA) Token Taxonomy Framework (TTF), and incorporated enterprise requirements that focus on compliance.


By using the TTF, Hedera begins with a strong definition that classifies tokens by characteristics


and defines their properties as well as behaviours in a way that allows tokens to be understood and designed in common language. This gives a token the ability to be constructed across Blockchain/DLT networks, which may be desirable in a case where interoperability is a requirement, as we showed in this[demonstration](https://github.com/hashgraph/hedera-stable-coin-demo) .


The specification was then[formally verified](https://github.com/hashgraph/hedera-stable-coin-formal-model/tree/39a4e28e408733e9c5fcd8a95ca8d9b4bf7952cb) for correctness, as well as[audited](https://github.com/hashgraph/hedera-stable-coin/blob/master/Audit.pdf) by Quantstamp. This level of rigor enables issuers using the specification, formal verification, and audit to **insure** the security of the stablecoin and avoid costly errors that lead to hacks or locking of funds.


Hedera then developed a front end user interface showing both how a consumer would interact with the stablecoin and a stablecoin issuer could manage their network. We deployed the front end along with the audited stablecoin implementation to verify the performance of the stablecoin implementation at scale. Hedera achieved over 2,700 tps with low latency over long test runs at multiple points.


‍


*The token implementation, front-end, formal model, and audit report are all*[open sourced in our Github repo](https://github.com/hashgraph/hedera-stable-coin) *for you to try out yourself.*


### **Implementation**


We defined a set of key roles necessary to deliver a decentralized and compliant stablecoin. These roles included:


- token holders who desire to have a more portable, secure, and financially stable mechanism for transacting real value through a trusted network.
- token issuers who seek to make financial interactions with their consumers and partners seamless and trusted.
- regulators who act to protect the network from malicious activity that may impact those they serve.
- service providers who provide a function of the system such as compliance monitoring, infrastructure hosting, liquidity, and custody.


These stakeholders form the **** core actors in the stablecoin network as the; administrator, token holder, compliance, supply, and enforcement.


- A token holder is a user of an application which interfaces with the token network and is the owner of tokens.
- The administrator is also the owner of the stablecoin token and is responsible for the overall operation of the token and network.
- Supply is responsible for the functions of minting and burning tokens on the network to maintain stability as it relates to the necessary collateral.
- Compliance monitors and evaluates network activity to identify suspicious or malicious token holders, freezing those who are not in compliance with the relevant regulation.


These roles all are represented in the token contract written in Java and running on a token node, which receives token messages from an HCS topic, validates the message, and mutates the state according to the logic of the IWA specification.


As shown in the[software design description](https://github.com/hashgraph/hedera-stable-coin/blob/master/Software%20Design%20Description.pdf) , the architecture also includes:


- A State API interface for reading the state from a token node
- A Payment API interface for facilitating payment of HCS messages
- A token bridge service that will facilitate movement of the stablecoin between the HCS stablecoin and a matching Ethereum stablecoin smart contract
- A sample client for buying, transferring, and selling stablecoins
- An administrative interface for monitoring the network and managing compliance-related functions on the network


### Building Safer Financial Applications


The power of financial applications using blockchain technology is their ability to easily integrate with one another: however, without standardizing processes such as the implementation of stablecoins, financial applications using blockchain technology face higher security risks. While enterprises are aware of the potential of blockchain technology, security issues have kept them on the sidelines.


By creating stablecoins through the Hedera Hashgraph framework, developers seeking to integrate Hedera Hashgraph stablecoins into their platforms have specific security guarantees that reduce complexity and create a predictable environment. For enterprises, this means it is easier for them to create safe and compliant stablecoins and easier for partners to safely incorporate their stablecoin into their own platforms.


‍


#### About Hedera


[Hedera](http://www.hedera.com/) is a decentralized public network on which developers can build secure, fair applications with near real-time finality. The platform is owned and governed by a council of the world's leading organizations including Avery Dennison, Boeing, Deutsche Telekom, DLA Piper, FIS (WorldPay), Google, IBM, LG Electronics, Magalu, Nomura, Swirlds, Tata Communications, University College London (UCL), Wipro, and Zain Group.


For more information, visit www.hedera.com, or follow us on Twitter at @hashgraph, Telegram at t.me/hederahashgraph, or Discord at www.hedera.com/discord. The Hedera whitepaper can be found at www.hedera.com/papers.


‍


#### About Quantstamp


[Quantstamp](https://quantstamp.com/) is the leader in blockchain security, having performed over 140 audits and secured over $5 billion of value. Our mission is to facilitate the mainstream adoption of blockchain technology through our security services. Quantstamp services include securing Layer 1 blockchains such as Ethereum 2.0 and Avalanche, securing smart contract powered applications such as Curve and Idle Finance, and formally verifying financial blockchain primitives. Enterprise companies and NGOs such as Siemens, Toyota, and World Economic Forum also trust Quantstamp to secure their blockchain ecosystems.
