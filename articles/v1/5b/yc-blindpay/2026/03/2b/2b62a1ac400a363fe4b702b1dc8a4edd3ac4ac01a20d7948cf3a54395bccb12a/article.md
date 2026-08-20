---
schema_version: "1.0.0"
document_id: "2b62a1ac400a363fe4b702b1dc8a4edd3ac4ac01a20d7948cf3a54395bccb12a"
company_key: "yc-blindpay"
company: "BlindPay"
source_id: "yc-blindpay-rss-90649a445bc9"
canonical_url: "https://blindpay.com/blog/cli"
published_at: "2026-03-06T00:00:00+00:00"
first_seen_at: "2026-07-26T23:51:50.435678+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:1dca02a21a5a59bc15cfaabf557324851df8070ed59860608673da6c8fe7b370"
---

# BlindPay CLI

BlindPay is the most AI-native stablecoin infrastructure available today. Every layer of the platform, the API, the[MCP Server](https://www.blindpay.com/blog/mcp) , the[Agent Skills](https://www.blindpay.com/blog/agent-skills) , and now the CLI, is built so that AI agents and developers can operate global payment infrastructure with equal ease. No other stablecoin platform offers this depth of integration.


The BlindPay CLI brings the full power of that infrastructure to your terminal. You or any AI agent can manage customers, execute payouts, create virtual accounts and configure webhooks without leaving your shell. It works for developers typing commands by hand, and it works just as well for AI agents like Claude, Codex, and automation scripts that need structured, predictable output.


## Installation


Install globally with npm:


Bash


Or run without installing:


Bash


## Setup


Grab your API key and instance ID from the[BlindPay dashboard](https://app.blindpay.com/) and configure:


Bash


## What You Can Do


The CLI covers the full BlindPay API surface. Every command supports` --help` for usage details and` --json` for machine-readable output.


### Customers and Bank Accounts


Create and manage customers, attach bank accounts across multiple rails (ACH, Wire, PIX, SPEI, SWIFT), and configure blockchain wallets.


Bash


### Payouts and Payins


Generate quotes with real-time FX rates, then execute payouts and payins in a single flow.


Bash


### Virtual Accounts and Offramp Wallets


Create virtual accounts for receiving payments and list offramp wallets for on-chain settlement.


Bash


### Webhooks, API Keys, and Partner Fees


Configure webhook endpoints, manage API keys, and set up partner fees directly from the CLI.


Bash


### Reference Data


Check supported payment rails and the bank detail fields required for each one.


Bash


## Built for Humans and AI Agents


The CLI is designed from the ground up to be used by both humans and AI agents. If you use Claude, Codex, or any LLM-based coding assistant, they can run BlindPay CLI commands directly as part of their workflow.


What makes it agent-friendly:


- **` --json` flag** on every command returns structured JSON output that agents can parse without guessing
- **` blindpay schema`** returns full field definitions, types, defaults, and enums as JSON, so an agent can discover the API surface and build valid commands without external documentation
- **Predictable exit codes** :` 0` for success,` 1` for user error,` 2` for API error
- **Structured errors** when` --json` is active:


JSON


An AI agent can install the CLI, run` blindpay schema` to understand what resources and fields are available, then construct and execute commands autonomously. Combined with the[MCP Server](https://www.blindpay.com/blog/mcp) for direct tool-use and[Agent Skills](https://www.blindpay.com/blog/agent-skills) for domain knowledge, BlindPay gives agents a complete integration surface that no other stablecoin platform provides.


## Updating


Bash


## Get Started


Install the CLI and connect it to your BlindPay instance in under a minute:


Bash


Source code and full command reference are available on[GitHub](https://github.com/blindpaylabs/blindpay-cli) .
