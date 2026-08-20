---
schema_version: "1.0.0"
document_id: "09006994f0e69fd94941a8398336282b710bb3fb5643d8aac81474b9f7c16b75"
company_key: "yc-blindpay"
company: "BlindPay"
source_id: "yc-blindpay-rss-90649a445bc9"
canonical_url: "https://blindpay.com/blog/agent-skills"
published_at: "2026-02-12T00:00:00+00:00"
first_seen_at: "2026-07-26T23:51:50.435678+00:00"
fetched_at: "2026-07-28T20:54:21.606053+00:00"
content_hash: "sha256:8706fe7579d1d73480cf367fe08775cbb18a769dd35255b37f32c11ea549daa2"
---

# BlindPay Agent Skills

AI coding assistants are powerful, but they need domain knowledge to build payment integrations effectively. BlindPay Agent Skills fill that gap. They teach your AI assistant how BlindPay's stablecoin payment infrastructure works so you can ask questions, prototype integrations, and debug flows in plain English instead of digging through API docs.


## Overview


BlindPay Agent Skills are the official[Agent Skills](https://agentskills.io/) for BlindPay. Once installed, they provide AI assistants (Cursor, Claude Code, Codex and others) with domain expertise for:


- Payout flows (stablecoin to fiat)
- Payin flows (fiat to stablecoin)
- Customer creation and KYC/KYB requirements
- Bank account setup across rails (ACH, Wire, PIX, SPEI, SWIFT)
- Quote generation and token approvals
- Virtual accounts and offramp wallets
- Webhooks and event handling


Skills work alongside your usual workflow. When you ask a question or start a task that touches payment logic, the assistant automatically uses this knowledge to give accurate, context-aware answers. No need to copy-paste docs or remember parameter names.


## How It Differs From MCP


You may have seen our[BlindPay MCP Server](https://www.blindpay.com/blog/mcp) . It connects AI assistants directly to BlindPay's API so they can execute operations. Agent Skills are different:


- **MCP** = tools. It gives your assistant the ability to *do* things (create customers, generate quotes, process payouts).
- **Skills** = knowledge. They give your assistant the ability to *understand* BlindPay concepts, flows and best practices.


Use Skills when you want to understand *how* something works or brainstorm integration design. Use MCP when you want the assistant to perform operations on your behalf. Many developers use both together.


## Installation


Installation is the same across supported platforms. Just run:


Bash


After installation, skills activate automatically when relevant tasks are detected. No extra configuration required.


## Example Prompts


Once installed, you can ask things like:


- "Explain the complete payout flow from quote to execution"
- "Walk me through creating a customer with enhanced KYC"
- "How do I create a SWIFT bank account?"
- "What are the KYC requirements for a business customer?"
- "How do I test payouts using the USDB test token?"


The assistant uses the skills to answer with accurate, up-to-date guidance tied to BlindPay's API and flows.


## Conclusion


BlindPay Agent Skills bridge the gap between powerful AI coding assistants and specialized payment infrastructure knowledge. By teaching your AI assistant how BlindPay works, you can build integrations faster, onboard developers more efficiently, and spend less time context-switching between docs and code.
