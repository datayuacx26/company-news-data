---
schema_version: "1.0.0"
document_id: "36b33a851557386c5266cc7b6a9e08c1f103dd6b893177b25b21ecefb6da3a9c"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/changelog/formance-mcp-server"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T19:56:11.459711+00:00"
fetched_at: "2026-08-05T19:56:12.929801+00:00"
content_hash: "sha256:1ca0781cccb2a81c4c45debb78c80767f23503d43e0f95649ceea8ad1aa84f06"
---

# What is the Formance MCP server?

**Today, we're launching the Formance MCP server: a read-only AI agent that accesses your ledger, payments, and reconciliation data, as well as Numscript validation, from MCP-compatible clients like Claude Code and Codex.**


We built it on purpose: AI agents are becoming part of how engineering and finance teams investigate ledger data, and financial infrastructure needs a hard read/write boundary.


When you're tracing a $40,000 discrepancy across two payment service provider (PSP) settlement accounts, the transaction history lives in your ledger, but the reasoning lives in an AI window. So you copy the balances from the application, paste them into Claude, ask a question, and repeat for the next batch of accounts.


The Formance Model Context Protocol (MCP) server removes this repetitive loop. As a read-only MCP server, it gives agents context while keeping execution outside the tool surface, so you don’t compromise your ledger's traceability by handing a non-deterministic process the keys to your postings.


This guide explains everything you need to know about the Formance MCP server.


### What is the Formance MCP server?


The[Formance MCP server](https://docs.formance.com/v3.2/mcp?deployment=cloud&license=ee) exposes selected Formance platform modules to MCP-compatible clients such as coding assistants and local agent runtimes.


The server is read-only for stack data and intended for exploration, assisted diagnostics and development workflows. No tool exposed by the server can create a transaction or trigger a payout, and the metadata writes are outside the surface.


MCP support requires Stack v3.2 or later and fctl v3.4.0 or later, and is currently available for Enterprise Edition customers on cloud deployments.


### How does the Formance MCP server work?


The server runs via fctl, a local subprocess launched by your MCP client. It speaks JSON-RPC 2.0 over standard input and output, with the stdio transport defined in the MCP specification. Setup varies by component and deployment path. The steps below cover a standard installation.


#### Check your fctl version


Before enabling MCP, confirm your local fctl version supports it:


fctl version


#### Turn on the MCP module on your stack


Enabling MCP is a separate step from running it. First, enable the module on the target stack:


fctl stack module enable mcp


Run this in an authenticated fctl context targeting the organization and stack where MCP should be enabled.


#### Start the server over stdio


Once the module is enabled, start the server:


` fctl stack mcp serve --transport=stdio \\`


` --organization=YOUR_ORGANIZATION_ID \\`


` --stack=YOUR_STACK_ID`


The --organization and --stack flags are optional only when your fctl context contains exactly one of each.


Otherwise, pass both explicitly, or your fctl context exits with an organization not specified (or stack not specified) error. You can pass the flags either after the subcommand, as shown above, or as global flags before it:


` fctl --organization=YOUR_ORGANIZATION_ID --stack=YOUR_STACK_ID stack mcp serve --transport=stdio`


Both forms are equivalent; use whichever fits how you're already invoking fctl elsewhere in your scripts.


#### Configure your MCP client


Your MCP client needs to know how to launch fctl as the server process. The exact configuration format depends on the client. Claude Code and Codex both wrap the same underlying fctl command, but each expects it in its own config format and file.


**Claude Code** uses a JSON config with an mcpServers object:


```text
{
"mcpServers"  :{
"formance"  :{
"command"  :  "fctl"  ,
"args"  :[
"--organization=YOUR_ORGANIZATION_ID"  ,
"--stack=YOUR_STACK_ID"  ,
"stack"  ,
"mcp"  ,
"serve"  ,
"--transport=stdio"
]
}
}
}
```


**Codex** uses a TOML config instead, with an mcp_servers table:


```text
[  mcp_servers  .  formance  ]
command   =   "fctl"
args   = [
"--organization=YOUR_ORGANIZATION_ID"  ,
"--stack=YOUR_STACK_ID"  ,
"stack"  ,
"mcp"  ,
"serve"  ,
"--transport=stdio"
]
```


Both configs launch the same command, fctl stack mcp serve --transport=stdio, using your organization and stack.


The only difference is the file format and key naming each client expects (mcpServers JSON for Claude Code, mcp_servers TOML for Codex).


Replace the --organization and --stack values with your own environment in either case.


#### Verify the server responds


A stdio MCP server appears to hang because it is waiting for JSON-RPC on stdin. You can verify the handshake by piping an initialization request into the process:


```text
{
"jsonrpc"  :   "2.0"  ,
"id"  :   1  ,
"method"  :   "initialize"  ,
"params"  : {
"protocolVersion"  :   "2024-11-05"  ,
"capabilities"  : {},
"clientInfo"  : {
"name"  :   "test"  ,
"version"  :   "1.0"
}
}
}
```


` | fctl stack mcp serve --transport=stdio --organization=YOUR_ORGANIZATION_ID --stack=YOUR_STACK_ID`


A healthy server responds with a single JSON line advertising its identity (the version string will match your current build):


```text
{
"jsonrpc"  :  "2.0"  ,
"id"  :  1  ,
"result"  :{
"capabilities"  :{
"logging"  :{


},
"tools"  :{
"listChanged"  :  true
}
},
"protocolVersion"  :  "2024-11-05"  ,
"serverInfo"  :{
"name"  :  "stack-mcp"  ,
"version"  :  "vX.Y.Z"
}
}
}
```


If you see an organization not specified or stack not specified error instead, add the flags shown above. If the command hangs with no response, check that the mcp module is enabled on the stack and that your fctl context is authenticated.


**One security note:** MCP tools can expose sensitive financial data to the client who starts them. Only configure the MCP server in trusted clients and environments.


### How the MCP server connects with Formance products


The Formance MCP server connects with four Formance modules as its available capabilities: Ledger, Payments, Reconciliation, and Numscript. All are intended for exploration, diagnostics, and assisted development workflows.


**Module** **Capabilities**


**Ledger** Read ledgers, accounts, balances, volumes, transactions, and related metadata


**Payments** Read payment data and inspect payment-related resources


**Reconciliation** Read reconciliation data to investigate matching and reconciliation state


**Numscript** Validate Numscript before running it against a ledger workflow


#### Ledger


[Formance Ledger](https://www.formance.com/modules/ledger) is a programmable, double-entry, immutable system of record for financial transactions. Balances are computed at read time by aggregating postings per account, so the agent reads values derived from the full posting history; cached numbers can drift from it.


Through MCP, an agent reads ledgers, accounts, balances, volumes, transactions, and related metadata. Practical queries: "Which accounts under users: went negative this week?" or "Show me every posting touching the[omnibus settlement account](https://www.formance.com/glossary/omnibus-account) since Tuesday."


#### Payments (Connectivity)


[Formance Connectivity](https://www.formance.com/modules/connectivity) ingests transactions and account balances across bank accounts, payment rails, wallets, exchanges, and custodians into a single, uniform data model.


The MCP server reads payment data and payment-related resources from those connected providers, so an agent can inspect what each rail reported without you having to write provider-specific export scripts.


#### Reconciliation


[Formance Reconciliation](https://www.formance.com/modules/reconciliation) compares balances between your ledger and external cash pools, and reports drift: discrepancies between expected and actual balances, with the exact difference by currency.


Through MCP, an agent reads reconciliation data to investigate the matching state. When drift appears, the agent can pull the reconciliation report and the underlying ledger postings in the same conversation, which is exactly the cross-referencing work that eats an afternoon when done by hand.


#### Flows & Wallets


Formance Flows automates the fund lifecycle and chains steps such as holding funds while Know Your Customer (KYC) checks are complete before payout. Formance Wallets is a fully managed wallet service that holds user funds and provides built-in support for complex flows such as hold and capture.


**However, neither Flows nor Wallets is a capability area in Formance MCP server v3.2** . Both are outside the MCP surface, so if your team depends on inspecting workflow run history or wallet state via an agent, that data isn't yet reachable through MCP.


#### Numscript validation through MCP


[Numscript is Formance's domain-specific language (DSL)](https://www.formance.com/blog/engineering/numscript) for programmable ledgers, a way to describe money movement that's both human-readable and machine-safe.


The Formance MCP server validates Numscript, so the agent can check a script for errors before it ever touches a ledger, and supports assisted development.


Suppose a buyer in your marketplace makes a $15,000 purchase, owes the merchant 90%, and pays 10% as the platform fee:


In the ledger, the buyer's spendable funds sit in @buyers:carol:wallet, the merchant payable sits in @merchants:8452:payable, and the platform fee revenue sits in @platform:revenue:fees.


```text
// MARKETPLACE_SETTLEMENT
// Event: marketplace sale splits to merchant payable and platform fee revenue
send   [USD/2 1500000]   (
source   =   @buyers:carol:wallet
destination   = {
90  %   to   @merchants:8452:payable
remaining   to   @platform:revenue:fees
}
)
set_tx_meta  (  "event_type"  ,   "marketplace_settlement"  )
set_tx_meta  (  "order_id"  ,   "ord8452"  )


```


The script moves $15,000 atomically: $13,500 to @merchants:8452:payable and $1,500 to @platform:revenue:fees. Numscript's arithmetic is deterministic with no floating-point math. When a split doesn't divide evenly, the remainder is allocated from the top of the destination list down, so no fraction of a cent is ever created or lost.


With the MCP server connected, you can inspect financial data and validate Numscript within your workflow.


You draft the script in Claude Code or Codex, and the agent validates it against the stack before anything executes. A typo in an account path is detected during validation in the editor. So does a malformed asset code like USD, where you meant USD/2, or a structural error in the destination block. Those errors appear before execution against production postings.


The agent can also read the current balance of @buyers:carol:wallet in the same session to sanity-check that the source can cover the send. Under any prompt, the server cannot execute the transaction. Committing postings stays with your deployment pipeline and your humans.


### Why you need the Formance MCP server now


Enable the Formance MCP server to give your team read-only agent access to ledger, payments, and reconciliation data, plus Numscript validation, with a hard write boundary enforced at the tool surface.


It replaces copy-and-paste investigation workflows with direct, in-session queries, so that settlement discrepancies, reconciliation drift, and closed investigations are resolved in a single conversation rather than across multiple tools.


### Frequently asked questions about the Formance MCP server


#### What is MCP (Model Context Protocol)?


The[Model Context Protocol](https://modelcontextprotocol.io/introduction) is an open-source standard for connecting AI applications to external systems. Under the MCP architecture, a host runs clients, each with a dedicated connection to one MCP server. They communicate over JSON-RPC 2.0.


#### Can the Formance MCP server write to my ledger?


No. The server is read-only for data across Ledger, Payments, and Reconciliation, and validate-only for Numscript. No tool it exposes can create postings, modify metadata, or initiate payments because the server enforces the boundary at the tool surface.


#### What version do I need to use MCP?


Stack v3.2 or later and fctl v3.4.0 or later.


#### Is the Formance MCP server available on all deployment types?


The Formance MCP server is available for Enterprise Edition customers on cloud deployments, and requires Stack v3.2 or later and fctl v3.4.0 or later. If you're on a different tier or deployment model and want to evaluate MCP access,[get in touch](https://www.formance.com/demo) with our team for a custom quote.
