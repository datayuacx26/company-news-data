---
schema_version: "1.0.0"
document_id: "292d7b023c37cd9ddf601d522ece7311de4c56748e374d084fe213582462f203"
company_key: "circle-internet-group-inc-class-a-common-stock"
company: "Circle Internet Group Inc."
source_id: "circle-internet-group-inc-class-a-common-stock-news-import-6b20a10366df"
canonical_url: "https://www.circle.com/blog/eco-builds-on-circle-gateway-to-improve-crosschain-execution"
published_at: "2026-05-14T00:00:00+00:00"
first_seen_at: "2026-07-24T01:33:44.770994+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:02c95a49cc0310d3b8a86f75faf4f5e1557d422adef1e9df18d0096248edf04f"
---

# How Eco Uses Circle Gateway and Circle’s Interop Stack | Circle

For businesses building across multiple blockchains, crosschain execution can break down as liquidity fragments across networks and settlement speed varies by chain. As a result, funds can sit on the wrong network, users may need to switch chains, and teams often have to pre-fund balances across ecosystems just to keep transactions moving.


[Eco](https://eco.com/) , an infrastructure layer for stablecoin movement, is solving for those core inefficiencies with[Circle Gateway](https://www.circle.com/gateway) . And by building faster deposit flows with Circle’s broader interop stack, including[CCTP](https://www.circle.com/cross-chain-transfer-protocol) -assisted paths, Eco is showing how protocols can extend Gateway to deliver a better crosschain experience.


## How Eco builds on Gateway


Eco uses Gateway to access a[unified USDC balance across supported chains](https://developers.circle.com/gateway/quickstarts/unified-balance-evm) , reducing the need to manage fragmented inventory on a chain-by-chain basis. That gives Eco a stronger liquidity foundation for the developers and businesses building on its infrastructure.


In that sense, Eco builds on top of Gateway by integrating it as a foundational liquidity layer, then adding its own routing logic and transaction handling to create a comprehensive solution for developers.


For Eco and its users, Gateway enables:


- A unified,[chain-abstracted USDC](https://www.circle.com/blog/coming-soon-circle-gateway-and-the-future-of-chain-abstracted-usdc) balance
- Less need to pre-fund and rebalance every supported chain
- More capacity for larger or less predictable crosschain orders
- A faster path into usable liquidity when deposits begin on slower-finality chains


It also points to a broader opportunity for the onchain ecosystem: protocols can use Gateway as a foundation, then extend it with additional interoperability flows to create better crosschain products.


Eco is an early example of that model in practice, turning that infrastructure approach into a more scalable crosschain execution experience.


Eco’s Routes API uses an intent-based model, which means a user signs for the outcome they want and the network fulfills it. Solvers, the participants that compete to complete those orders, can deliver better results when liquidity is easier to access at the moment it is needed and when deposit paths into usable liquidity are more efficient.


That approach is delivering results that are visible in Eco’s operating metrics. Thanks to its Gateway integration, Eco reported that its Routes API order size limits increased by 10 times — enabling support for millions of dollars in crosschain transactions. Since integrating Gateway in February 2026, Eco said it has scaled its solver capacity to fulfill 7-figure,[crosschain USDC orders](https://bridge.usdc.com/) . That order profile had been materially harder to support when liquidity first had to be managed chain by chain.


## Cohesive and streamlined crosschain experiences


For Eco users handling larger transaction sizes, the gains Eco delivers through its Gateway integration are meaningful — including both larger transaction sizes and a cleaner crosschain operating model.


Eco’s Gateway integration gives Eco users more flexible access to liquidity, helping reduce fulfillment friction when speed matters. That is especially important in environments where liquidity demand shifts quickly or activity begins on slower-finality chains. Eco’s Gateway integration also supports its broader goal of making multichain applications feel less fragmented. By combining Gateway with other Circle primitives, Eco estimates that its API can reduce wait times for certain Gateway deposit flows to an estimated 30 seconds, down from approximately 15 minutes.


In plain terms, Eco is helping its users deliver more cohesive and streamlined crosschain experiences. Eco users do not need to think through every network switch, bridge step, or gas token requirement themselves. More of that complexity can stay inside the infrastructure layer, where Eco is combining Circle primitives into a simpler experience for builders and end users.


When[USDC liquidity](https://www.circle.com/case-studies/arf) can be accessed across supported chains from one balance, and when deposit flows into that liquidity can be made more efficient, Eco can make it easier for its users to offer simpler routing, smoother deposits, and more consistent onboarding flows. The end result is better performance, less operational drag, and a model that scales more cleanly as activity expands.


## Building on Gateway for multichain growth


Eco’s integration shows how protocols can build on top of Gateway to extend its capabilities. Gateway provides the unified liquidity foundation, and[Circle’s broader interop stack](https://www.circle.com/blog/building-the-interop-stack-for-the-internet-financial-system) can help integrators go further, especially when they need faster deposit paths from slower-finality chains.


As more businesses work to serve users across multiple blockchains, infrastructure like Gateway offers a foundation for solving fragmented liquidity. Eco's approach demonstrates how developers can combine this foundation with other interoperability tools to create seamless, scalable multichain products that grow with demand.


‍


[Get started with Circle Gateway](https://developers.circle.com/gateway) ‍


Reference to any specific company, product, service, or website of any third party does not constitute an implied or express endorsement, recommendation, favoring or validation by Circle. The content presented is intended for informational purposes only. Reliance upon any content or information presented is at the sole discretion of the audience; Circle shall not be liable for any damage or loss relating to the use of or reliance upon any such content or information presented. The views and opinions expressed herein do not necessarily state or reflect those of Circle.


‍


Circle Technology Services, LLC ("CTS") is a software provider and does not provide regulated financial or advisory services. You are solely responsible for services you provide to users, including obtaining any necessary licenses or approvals and otherwise complying with applicable laws. For additional details, please see the Circle Developer terms of service, available at console.circle.com/legal/developer-terms.


USDC is issued by regulated affiliates of Circle. See[Circle’s list of regulatory authorizations](https://circle.com/legal/licenses)
