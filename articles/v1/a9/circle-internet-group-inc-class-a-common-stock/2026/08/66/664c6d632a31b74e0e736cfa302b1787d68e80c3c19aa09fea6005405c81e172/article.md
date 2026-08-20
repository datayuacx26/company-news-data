---
schema_version: "1.0.0"
document_id: "664c6d632a31b74e0e736cfa302b1787d68e80c3c19aa09fea6005405c81e172"
company_key: "circle-internet-group-inc-class-a-common-stock"
company: "Circle Internet Group Inc."
source_id: "circle-internet-group-inc-class-a-common-stock-news-import-6b20a10366df"
canonical_url: "https://www.circle.com/blog/gateway-adds-erc-1271-support-for-programmable-authorization"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T18:16:53.859207+00:00"
fetched_at: "2026-08-04T18:43:13.169222+00:00"
content_hash: "sha256:f31a2233d6846dd2d5bfe3f525fd5baaa5ae20adb3268fc17be62ee5f3f1bfd8"
---

# Gateway now supports ERC-1271 for Programmable Authorization | Circle

‍


DeFi protocols, payment systems, treasury tools, smart wallets, and other applications rely on programmable authorization to define who can move funds, when, where, and under what conditions. Smart contracts increasingly sit at the center of these applications and provide configurable authorization, policy controls, and governance mechanisms.


[Circle Gateway](https://www.circle.com/gateway) now supports programmable authorization models through[ERC-1271 signatures](https://eips.ethereum.org/EIPS/eip-1271) , allowing smart contracts and smart contract wallets to use Gateway directly. Developers can access unified USDC balances through their existing smart contract authorization logic, without setting up delegate approvers.


‍


## Direct Gateway access for programmable authorization


Gateway gives developers a unified USDC balance across supported blockchains. After USDC is deposited to Gateway Wallet contracts, applications can view their crosschain balance and mint USDC on[supported destination chains](https://developers.circle.com/gateway) using Gateway’s API and contracts.


Until now, Gateway integrations required[externally owned accounts (EOAs)](https://developers.circle.com/wallets/account-types) to submit burn and mint intents. Smart contracts and smart contract wallets needed delegate-based flows to authorize Gateway transactions, adding setup steps, additional signature management, and operational overhead.


With ERC-1271 support, Gateway can validate signatures from smart contracts using the standard method for contract-based signature verification. This expands Gateway access to applications that rely on programmable authorization, including smart contracts and smart wallets. This lets builders using Gateway rebalance[crosschain liquidity](https://www.circle.com/blog/a-practical-guide-to-building-with-circle-gateway) more efficiently, automate recurring payments and workflows, and[deliver chain-abstracted experiences](https://www.circle.com/blog/coming-soon-circle-gateway-and-the-future-of-chain-abstracted-usdc) to their apps’ users.


‍


## Preserve existing smart wallet authorization


ERC-1271 support lets teams keep the authorization models they already use. Smart contract wallets often do more than check a private key. They can apply multisig approvals, role-based permissions, passkeys, time-bound controls, allowlisted recipients, spending limits, and other programmable policies.


Gateway now works with those models directly. Developers can integrate smart contracts with Gateway while preserving the contract’s native validation logic. This reduces reliance on separate EOA delegates or signing infrastructure. **** ERC-1271 support provides a cleaner integration path for teams building sophisticated onchain systems.


Policy stays where it belongs: inside the smart contract or smart wallet infrastructure that already governs the users, treasury, protocol, or application. ERC-1271 support allows app developers to apply their own custom approval rules (e.g., address whitelists, asset restrictions) before authorizing a Gateway action.


‍


## More crosschain USDC workflows


For DeFi protocols, Gateway support for ERC-1271 enables contracts to[move USDC crosschain](https://www.circle.com/cross-chain-transfer-protocol) without relying on bridges or pre-funded balances on every supported network. For payment infrastructure, it can support payment contracts that route USDC across chains while keeping existing approval logic intact.


Treasury systems can use Gateway to manage USDC from a unified balance across supported chains, supporting recurring payments, crosschain operations, and policy-controlled disbursements. Smart wallet infrastructure can give users access to one USDC balance across chains without adding delegate setup flows. Other smart contract applications that have programmable authorization can apply their existing policy controls while accessing Gateway, without redesigning their authorization architecture around separate delegate accounts.


‍


## A simpler path for existing Gateway integrators


For existing Gateway integrators using delegates, ERC-1271 support creates a path toward direct smart contract access. Instead of maintaining additional delegate flows, teams can define Gateway authorization with the same smart contract logic that governs their product today.


This is a meaningful improvement for Gateway integrators: less setup friction, fewer operational dependencies,[streamlined crosschain workflows](https://www.circle.com/blog/choosing-between-circle-gateway-and-cctp-with-forwarding-service-for-crosschain-usdc) , and more direct support for the way modern onchain applications already authorize activity.


Gateway now supports programmable authorization through ERC-1271. Smart contracts, smart wallets, and other applications with programmable authorization can now connect to Gateway on their own terms.


[Integrate Gateway](https://developers.circle.com/gateway) into your[smart contracts.](https://developers.circle.com/gateway)


‍


Circle Technology Services, LLC ("CTS") is a software provider and does not provide regulated financial or advisory services. You are solely responsible for services you provide to users, including obtaining any necessary licenses or approvals and otherwise complying with applicable laws. For additional details, please see the Circle Developer terms of service, available at console.circle.com/legal/developer-terms.


USDC is issued by regulated affiliates of Circle. See[Circle’s list of regulatory authorizations](https://circle.com/legal/licenses) .
