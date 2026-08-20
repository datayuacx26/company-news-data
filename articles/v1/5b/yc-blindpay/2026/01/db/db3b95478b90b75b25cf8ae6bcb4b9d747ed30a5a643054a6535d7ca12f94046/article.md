---
schema_version: "1.0.0"
document_id: "db3b95478b90b75b25cf8ae6bcb4b9d747ed30a5a643054a6535d7ca12f94046"
company_key: "yc-blindpay"
company: "BlindPay"
source_id: "yc-blindpay-rss-90649a445bc9"
canonical_url: "https://blindpay.com/blog/mcp"
published_at: "2026-01-13T00:00:00+00:00"
first_seen_at: "2026-07-26T23:51:50.435678+00:00"
fetched_at: "2026-07-28T20:54:51.009577+00:00"
content_hash: "sha256:3bd32bb66d6631c0fbebae5c9fb9a2099cc030d949afd4a007d2013e1eb14736"
---

# Talk to Your Payment API

The BlindPay MCP Server connects AI coding assistants to BlindPay's global payment infrastructure, allowing developers to interact with payment APIs using natural language instead of manual API calls.


## Overview


The BlindPay MCP (Model Context Protocol) acts as a bridge between your AI coding assistants and BlindPay's payment infrastructure. Instead of writing integration code or navigating dashboards, you can ask your AI assistant to execute payment operations directly.


This means you can handle cross-border payments, manage customers and process transactions without leaving your code editor or memorizing API documentation.


## Capabilities


The MCP server provides access to BlindPay's core functionality:


- **Quote generation** for international transfers
- **Customer management** for individuals and businesses
- **Payout processing** across multiple blockchains
- **Virtual account creation** for receiving payments
- **Webhook configuration** for payment notifications
- **Real-time FX rates** for currency conversions


All operations are executed through conversational requests to your AI assistant.


## Example Usage


**Traditional API call:**


Bash


**With the MCP server:**


> "Get me a quote for sending 1000 USDC to a bank account in Brazil"


The AI assistant interprets the request and executes the appropriate API calls.


## Setup


Configuration requires three steps:


1. Get your API key from the BlindPay dashboard
2. Add the MCP server configuration to your AI assistant
3. And you're ready to begin making requests in natural language


## Documentation


Complete setup instructions and API reference documentation are available in the[README on GitHub](https://github.com/blindpaylabs/blindpay-mcp) .
