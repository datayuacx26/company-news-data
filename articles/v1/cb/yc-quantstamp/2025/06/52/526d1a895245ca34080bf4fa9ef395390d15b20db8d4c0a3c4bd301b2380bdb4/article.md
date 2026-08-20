---
schema_version: "1.0.0"
document_id: "526d1a895245ca34080bf4fa9ef395390d15b20db8d4c0a3c4bd301b2380bdb4"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/defis-composability-more-possibility-more-risk"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:03132e21fbcf34fdba7c6e7beaf29b107ce4f2453825287a885ad6913e65f5d6"
---

# DeFi’s Composability: More Possibility, More Risk

Decentralized Finance (DeFi) is quite possibly the strongest and most obvious use case for the Ethereum blockchain. It's reinventing the traditional financial system, making sophisticated financial products accessible to anyone with an internet connection.


DeFi is only possible thanks to smart contract technology. However, their very nature means that smart contracts are irreversible and immutable after deployment. And one of their key features—composability—means that DeFi's seemingly endless potential comes with a myriad of risks.


In the Ethereum world, smart contract bugs could be thought of as the original risk—many people will remember the infamous DAO Hack in 2016, ultimately due to a reentrancy vulnerability. Yet, as the space evolves and interactions become increasingly complex, even more risk vectors have emerged.


DeFi’s composability is often described as a “double-edged sword.” Smart contracts and pools of capital frequently interact with many others to compound their functionality: much more can be accomplished by integrating multiple smart contracts, creating endless combinations with these “money legos”. Unfortunately, this increase in possibility also opens the door to new, unknown attack vectors.


In 2020 and 2021, Ethereum’s decentralized finance sector lost over $150 million to economic attacks. In 2021, this number has already surpassed $200 million. These hacks and exploits have involved rug pulls, malicious use of admin keys, phishing, smart contract vulnerabilities, and more.


The open nature of smart contracts means that loopholes and vulnerabilities are visible to everyone. And, with billions of dollars in value locked in DeFi protocols, they are an increasingly lucrative target for hackers.


### **Increasing Complexity**


Added to the temptation of billions in value to potentially be exploited, a recurring challenge in DeFi is its increasingly complex interactions. While DeFi’s money legos facilitate more sophisticated transactions, this also increases the number of interactions with external—and possibly untrusted—code. Smart contracts rely on external transactions to trigger their functions. Any transaction on the Ethereum blockchain—other than simply sending ETH—will interact with one or more smart contracts, commonly referred to as a “contract call”. In 2019, the average number of smart contract calls triggered by each transaction was 1.19. As of April 2021, this number had risen to 2.40.


Decentralized apps often involve multiple smart contracts interacting across multiple protocols. While this is an incredible feature, each new link presents new complexity and new attack vectors. A vulnerability with one smart contract could have far-reaching consequences for multiple protocols across the DeFi space. The total number of[external contract calls](https://studio.glassnode.com/metrics?a=ETH&category=&chartStyle=line&m=transactions.ContractCallsExternalCount) has risen dramatically as the sector grows, and with each additional smart contract, the chance of a bug increases. In addition to increasing numbers of external smart contract calls,[internal calls](https://studio.glassnode.com/metrics?a=ETH&category=&m=transactions.ContractCallsInternalCount) are increasing as well, indicating the increasing complexity of smart contracts themselves. And, a big picture view is becoming an essential skill: understanding how various protocols or pools could be used together to orchestrate an attack is key in identifying vulnerabilities.


### **Flash Loans and Price Manipulation**


In addition to the increased complexity of internal and external calls across a myriad of different protocols, another frequent risk within the DeFi space is price manipulation attacks. Smart contracts rely on oracles, which provide an interface between the contracts and an external source to pull the required data. For example, lending protocols rely on on-chain oracle price data to properly price assets. With flash loans, anyone can take advantage of arbitrage opportunities in the market. Unfortunately, bad actors may use flash loans to manipulate or corrupt prices.


By manipulating the price of an asset, someone can arb the difference and take advantage of bugs in a protocol. Hundreds of millions have been lost in flash loan exploits.


### **Avoiding the Dangers in DeFi**


While most people may understand the importance of a proper security audit, there is still a lot of nuance around what this means. The first thing to understand is that getting an audit in no way guarantees that a protocol is secure. Some projects have been through an external audit yet have poor documentation or unresolved vulnerabilities. Teams may make code changes without getting another audit done, opening the door to previously out-of-scope attack vectors. Or, in some cases, exploits occur due to code that wasn’t part of the initial audit at all.


While getting an audit is an essential part of mitigating risk, there are steps that teams can take—both before and after the audit—to stack the odds in their favor:
‍


**1) Information Security and Communication**


Often, DeFi protocols use admin keys, which allow certain individuals—often the core team—to upgrade contracts or otherwise make moves in the case of an emergency. Having this access can let a founding team stay agile in the case of an emergency. However, it also introduces operational risk—if these keys are compromised, malicious third parties (or even rogue team members) could gain control of the smart contracts. Teams can mitigate these risks through features such as multisig and timelocks.
‍


**2) Documentation Quality**


Conduct proper requirements engineering and write a good technical specification, including all use cases. While some people may feel writing is an onerous task that gets in the way of a more agile approach, it is crucial to security. A proper technical specification ensures everyone is on the same page and that all the bases are covered. By taking the time to think through complex issues and potential obstacles, you’ll end up further ahead. Another factor to consider is that clear, high-quality documentation will ensure that external auditors can audit your project in an effective and timely manner.
‍


**3) Quality Testing**


[Quality testing is crucial](https://quantstamp.com/blog/securing-your-defi-project-starts-with-quality-testing) . In addition to getting an audit, teams should make sure to write complex functional tests that involve multiple users and long interaction scenarios. After conducting over 300 audits, our team has found functional tests to be a continuously undervalued security practice. In addition to decreasing risks, testing will also save your team hassle down the road in terms of maintaining code or adding new functionality. And, prioritizing testing can make an impact in other areas: helping your team develop a more security-focused mindset and even improving code quality.
‍


**4) Continuous Evolution**


The best projects are continuously evolving. Be sure to update documentation as the code changes. Keep in mind that the work doesn’t end at the completion of the audit. Ensure a bounty program is in place, and continue monitoring. Identifying vulnerabilities and bugs is an ongoing process, rather than a one-time occurrence.
‍


While DeFi’s risks cannot be completely eliminated, adhering to best practices and doing your due diligence as a team can help mitigate risk and reduce the likelihood of your protocol being compromised. Being on the bleeding edge of new technology will always come with heightened risks, and as the DeFi space continues to eat traditional finance, auditors and builders alike will need to be vigilant in prioritizing security. A strong offense is the best defense, so taking a proactive approach and staying ahead of the curve will undoubtedly pay off.
