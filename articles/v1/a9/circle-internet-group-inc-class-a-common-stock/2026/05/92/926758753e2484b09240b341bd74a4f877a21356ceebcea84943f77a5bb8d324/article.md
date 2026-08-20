---
schema_version: "1.0.0"
document_id: "926758753e2484b09240b341bd74a4f877a21356ceebcea84943f77a5bb8d324"
company_key: "circle-internet-group-inc-class-a-common-stock"
company: "Circle Internet Group Inc."
source_id: "circle-internet-group-inc-class-a-common-stock-news-import-6b20a10366df"
canonical_url: "https://www.circle.com/blog/introducing-circle-agent-stack-financial-infrastructure-for-the-agentic-economy"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-24T01:33:44.770994+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:b6f3f77b0e91d98abfd786e8eea1a4d18bfb94fcec57fbaf8efb95fffaca32da"
---

# Agent Stack: Financial Infrastructure for the Agentic Economy | Circle

Agents are increasingly able to reason, plan, and act, but their ability to participate in the economy remains underdeveloped. As agents begin to pay for APIs, data, compute, and services in real time, developers need infrastructure designed for machine-speed, programmatic transactions.


Today, we’re introducing[Circle Agent Stack](https://agents.circle.com/) : chain- and protocol-agnostic open infrastructure designed for the agentic economy. Agent Stack initially includes five products that help enable agents as autonomous economic actors, three of which are newly released today: Agent Wallets for controlled agent access to USDC and ERC-20 tokens, Agent Marketplace for discovering agentic services, and Circle CLI (command line interface) for executing agent financial actions through precise commands.


Alongside the previously announced[Nanopayments powered by Circle Gateway](https://www.circle.com/nanopayments) and[Circle Skills](https://github.com/circlefin/skills) , Agent Stack gives agents the ability to hold funds, discover services, and transact programmatically with[USDC](https://www.circle.com/usdc) — all while operating within defined permissions and guardrails.


## Why agents need a financial layer


The need for a purpose-built financial layer for agents is becoming more clear as applications move from concept to real-world use. As of April 29,[x402](https://developers.circle.com/gateway/nanopayments/concepts/x402) , an emerging protocol for agent payments, had processed $24.24 million in the prior 30 days1, with 99.8% of transaction value settled in USDC2.


That signal is early, but important. When agents pay for services in real time, they need money that is programmable, widely available, and built for internet speed. They also need infrastructure that supports small-value, high-frequency transactions without forcing teams to rebuild payment logic for every project.


Until now, that experience has been fragmented. Without an agent-native wallet, developers can be pushed toward hard-coded private keys, human-centric accounts, or manual approvals. Without a discovery layer, even an agent with access to funds has no structured way to find a service, evaluate it, and pay for it in the flow of a task. And without a consistent interface, agents and builders must piece together APIs, SDKs, documentation, signing flows, and multichain logic.


## Three new products for building agent systems


Agent Wallets give agents a way to hold and move USDC under human-defined policies. Users can create wallets designed for machine-initiated operation for their agents, then configure controls such as time-bound USDC spending limits for transfers and x402 services, as well as allowlists and blocklists for wallet and contract addresses. Policies are enforced at the wallet layer, allowing transactions to be checked against predefined rules before execution, designed to help agents operate in line with users’ preferences and intent.


Agent Marketplace gives users and agents a structured place to discover, evaluate, and integrate agentic services. This helps move service access from human-scale subscriptions and manual integrations toward usage-based, programmable economic activity.


CLI acts as the control plane. From a terminal or agent workflow, developers can create wallets, define policies, discover services, and trigger transactions through precise commands. CLI is designed to make agent execution more deterministic, reducing the risk that an AI system improvises around fragmented documentation or inconsistent API patterns.


Together, Agent Wallets, Agent Marketplace, and CLI provide a more complete economic loop for agents. With Agent Stack, agents can:


- Hold and move USDC through Agent Wallets, within predefined permissions
- Discover and evaluate services through Agent Marketplace
- Execute repeatable financial actions through CLI
- Settle payments using USDC as the programmable settlement asset


## Built around USDC and Circle infrastructure


These products are especially powerful when combined with Circle’s existing full-stack internet financial infrastructure. Nanopayments powered by Circle Gateway, a capability for moving very small amounts of USDC efficiently, is designed for gas-free, high-frequency transfers, including sub-cent payments that can make machine-to-machine economic activity practical at scale.


Meanwhile,[CCTP](https://www.circle.com/cross-chain-transfer-protocol) helps USDC move across supported blockchains.[Circle Skills](https://github.com/circlefin/skills) provide Circle-authored implementation patterns for developers building with AI coding tools. Agent Wallets, Agent Marketplace, and CLI connect to this broader foundation, giving anyone a practical way to create wallets, access services, and enable agents to participate in the agentic economy.


## An open approach to agentic economic activity


Circle’s approach is open, composable, and chain- and protocol-agnostic. Circle provides infrastructure and tools; users decide how their applications and agents behave, which services they use, and what policies govern economic activity.


The agentic economy will need a reliable financial layer. It will need wallets built for machine actors, marketplaces built for programmable discovery, and interfaces that translate human intent into repeatable execution. With Agent Stack, Circle is extending trusted, programmable internet-native dollars into the workflows powering the emerging agentic economy.


‍


1 According to[x402](https://www.x402.org/)


2 According to[Artemis](https://www.artemis.ai/company/crcl?tab=dashboard)


USDC is issued by regulated affiliates of Circle. See[Circle’s list of regulatory authorizations](https://circle.com/legal/licenses) .


Circle Technology Services, LLC (“CTS”) is a software provider and does not provide financial, advisory, or marketplace services. Agent Stack enables users and developers to interact with third-party applications and services, which are not controlled by Circle. Transactions initiated by agents are executed based on user-defined permissions and may occur without real-time human review. Circle does not guarantee the performance, availability, or outcomes of any third-party services or agent-initiated transactions. Users are solely responsible for their use of these tools and for evaluating associated risks.


For additional details, please see the Circle Developer terms of service, available at[agents.circle.com/terms-of-use.](https://agents.circle.com/terms-of-use)


‍
