---
schema_version: "1.0.0"
document_id: "c5688bdefb8b3b9208df69c33ad91386584fbe384ca7c5f3c4087d17b9694e1b"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/quantstamp-audits-gauntlets-updates-to-compound-governance-capabilities"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:d20d13ba5b5db93cb5412643c2b85680bf3970ad315e2779a82e299e5c4a4be7"
---

# Quantstamp Audits Gauntlet’s Updates to Compound Governance Capabilities

Quantstamp recently[audited updates to several Compound smart contracts](https://certificate.quantstamp.com/full/compound-protocol) proposed by[Gauntlet](http://gauntlet.network/) including updates to Compound governance capabilities. This security engagement included, but was not limited to, auditing:


- new vesting mechanisms
- a new mechanism allowing Compound to directly pay protocol contributors AND
- changes to the Comptroller contract


As a result of the audit, Compound changed the order that several methods were called so that` Comptroller.setCompSpeedInternal` would correctly update how Comp rewards are distributed to users. Compound also modified the` Comptroller._grantComp` functions to prevent governance proposals from passing when there are not enough funds available. All found issues were resolved.


### Optimizing DeFi Governance with Gauntlet


Gauntlet is pushing DeFi innovation forward by introducing data driven analysis into DeFi governance. Gauntlet’s team of experts simulate DeFi scenarios that take into account composability with other DeFi protocols in order to assess protocol risks and recommend improvements. After their analysis, they submit improvement proposals and engage in discussions with governance token holders in order to support a proposal's passage.


Gauntlet’s data driven analysis has resulted in improvements in leading protocols including Compound, Aave, and NuCypher. Gauntlet's long term goal is to create automated governance systems that analyze DeFi activity and automatically propose parameter suggestions to protocols.


*Gauntlet’s Intended Automated Governance System*


### Compound’s Impact on DeFi


Compound recently had a great impact on the DeFi space by decentralizing their governance through their liquidity mining program in June 2020. When borrowers lent or borrowed digital assets on Compound, they also received COMP governance tokens. One goal of this program was to improve governance by distributing tokens to those who actually use the platform. The Compound protocol is now completely run by COMP token holders.


Compound’s liquidity mining program was extremely successful. In 7 days, the total value of assets managed by Compound rose from 95 million USD to over 600 million USD. Other top DeFi projects including Uniswap, Curve, and Balancer also launched liquidity mining programs shortly after. From June 2020 to November 2020, the total value locked in DeFi applications rose from just above 1 billion USD to over 13 billion USD worth of digital assets. This sharp growth was a direct result of the liquidity mining programs across the DeFi ecosystem and was inspired by the success of Compound’s liquidity mining program.


*This summer’s liquidity mining craze was inspired by Compound in June. The red circle marks the sharp increase in Compound’s liquidity that took place immediately after their liquidity mining program went live.*


### Continually Improving Incentives


While many liquidity mining programs were successful, it became evident that certain programs facilitated better outcomes than others. In some instances, users would supply liquidity for a short period only to sell their governance tokens and transfer their liquidity elsewhere. Ideally, liquidity mining programs would incentivize users to supply liquidity for long periods of time.


In order to improve the incentives of participants in the Compound ecosystem, Gauntlet proposed a new vesting mechanism. This mechanism can be applied to future liquidity providers, governance participants, or any other actor in the ecosystem. Gauntlet also ensured that this vesting system was flexible enough that it would allow them to implement new incentivization strategies as they analyzed more market information. This flexible vesting system not only improved Compound’s governance, it will ultimately contribute to the improvement of DeFi at large.


### Compensating Ecosystem Contributors


Included in the Compound smart contracts audited by Quantstamp is also the ability for Compound to directly pay contributors through a token vote. This helps solve a variation of the tragedy of the commons problem that we often see in open source development. Developers build valuable and widely used software, but they are not compensated for their work. These developers also face an opportunity cost. Instead of dedicating their unique skill set to open source development for free, they can get paid elsewhere.


Gauntlet identified a unique opportunity to fix these misaligned incentives. Token holders can now compensate individuals for any valuable service that they can offer to improve the protocol. The ecosystem benefits from high quality contributors and the contributors themselves have a reason to stick around.


### Future of DeFi


Governance via token holders has led to the rapid iteration of leading decentralized protocols in DeFi. Liquidity mining programs seeded DeFi projects with a community of knowledgeable users who have a vested interest in the success of the protocol. Gauntlet has now provided token holders with data driven analysis that allows these vested token holders to make the informed decisions that propel the ecosystem forward. Quantstamp looks forward to continuing to secure the innovations that will form the foundation of tomorrow's economy.
