---
schema_version: "1.0.0"
document_id: "3b90f3da9d3084bcf81e7a38e74bcde6944e917bec9e66fe915283256af1b140"
company_key: "yc-notabene"
company: "Notabene"
source_id: "yc-notabene-news-import-e4585f8c666e"
canonical_url: "https://notabene.id/post/notabene-flow-mcp-server-guide"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-13T11:52:36.733436+00:00"
fetched_at: "2026-08-13T11:52:38.575509+00:00"
content_hash: "sha256:3f956bf9e05f739d343a262fbcf2c18ee01379e060705ae99b8a3280ed5ac4bb"
---

# Notabene Flow MCP Server Guide

This guide explains what the Notabene Flow MCP server is, how to connect it to Claude on each surface ([claude.ai](http://claude.ai/) in the browser, the Claude Desktop app, and the Claude Code CLI), and how to use its tools once connected.


# What is the Notabene Flow MCP server?


[MCP (Model Context Protocol)](https://modelcontextprotocol.io/) is the open standard that lets AI assistants like Claude call tools on external services. Notabene Flow provides a hosted MCP server that brings **compliant stablecoin payment link generation** directly into your AI assistant.


In practice, that means you can hand Claude an invoice — a PDF, an image, pasted text, or just a description — and ask it to "create a payment link for this." Claude extracts the invoice data, helps you choose where the funds should settle, and mints a payment link you can share with your customer, with Notabene handling travel-rule compliance behind the scenes.


Key properties:


- **The assistant acts for the merchant** — the business getting paid. Your identity is your verified login email; it is never something the assistant (or an uploaded invoice) can override.
- **Login is simple.** Connecting from any Claude surface opens a login page where you enter your email and a one-time code sent to it. Sessions last up to 7 days, then you sign in again.
- **Interactive previews.** On[claude.ai](http://claude.ai/) and the Desktop app, previewing or creating an invoice renders a live invoice preview right in the conversation. In Claude Code, the tools return equivalent text summaries instead.


## Endpoint


` <https://flow.link/mcp>
`


A development environment is also available at` https://dev.flow.link/mcp` for trying things out — links created there are not for real payments.


Authentication endpoints are discovered automatically — this is the only URL you enter.


# Setting up the connection


## [claude.ai](http://claude.ai/) (web)


1. Click your initials (lower left) → **Customize** → **Connectors** (or go directly to` claude.ai/customize/connectors` ).
2. Click **Add** → **Add custom connector** .
3. Enter` https://flow.link/mcp` and click **Add** . No OAuth client ID or secret is needed.
4. Click **Connect** . A browser window opens the Flow login: enter your email, then the one-time code sent to it.
5. In a chat, click the **+** button → **Connectors** and make sure the connector is enabled for the conversation.


On **Team/Enterprise** plans, an org admin adds the connector first under **Admin settings** → **Connectors** ; members then connect with their own email from **Customize** → **Connectors** .


## Claude Desktop app


Same as the web: **Customize** → **Connectors** → **Add** → **Add custom connector** , enter` https://flow.link/mcp` , then **Connect** and complete the email login. Enable it in a conversation via the **+** menu → **Connectors** .


The Desktop app supports the interactive invoice preview, so previews and created payment links render inline in the conversation.


## Claude Code (CLI)


Add the server over HTTP transport:


` claude mcp add --transport http notabene-flow <https://flow.link/mcp>
`


Use` --scope user` to make it available in all your projects, or` --scope project` to write it to a shareable` .mcp.json` for your team.


Then, inside a session, run` /mcp` , select the server, and choose **Authenticate** — your browser opens the email login.` claude mcp list` shows connection status (` ! Needs authentication` means run` /mcp` and authenticate).


Claude Code doesn't render the interactive preview; the tools return text summaries instead.


# Using the tools


## Available tools


- ‍` update_profile -` Sets your merchant display name (and optionally tax id / address) shown on every payment link.
- ‍` get_invoice_extraction_guide -` Gives the assistant the exact extraction guide to follow when you upload or paste an invoice (PDF, image, or text).
- ‍` validate_invoice -` Normalizes and validates an invoice without creating anything.
- ‍` preview_invoice` /` preview_invoice_text -` Shows the invoice before committing — as an interactive preview, or as text in Claude Code.
- ‍` list_supported_assets -` Lists the stablecoin assets and chains a payment link can settle in.
- ‍` find_provider -` Looks up an exchange or custodian (VASP) to settle to — Kraken, Coinbase, Fireblocks, etc.
- ‍` request_wallet_proof -` Creates a single-use, 24-hour signing link you open to connect a self-hosted wallet and sign an ownership attestation (no funds move, nothing is spent).
- ‍` get_wallet_proof_status -` Checks a wallet-proof request; once signed, it returns a ready-to-use settlement address.
- ‍` create_invoice_payment_link -` Issues the payment link (valid for 60 days, or until 60 days after the invoice due date, capped at 2 years).


## Typical flow


1. **Set your profile once:** the assistant calls` update_profile` with your business display name.
2. **Get the invoice in:** upload or paste an invoice, or just describe it. The assistant follows the extraction guide so its reading of the invoice matches Flow's own parser. Your customer's email is required so the payer is identifiable.
3. **Preview:** the assistant validates and previews the invoice so you can confirm it before committing.
4. **Choose the settlement wallet** (where funds land) — in order of preference:


- a wallet you've used on a previous invoice (ownership is proven once per wallet, no re-signing);
- an account at an exchange or custodian, found via` find_provider` ;
- a self-hosted wallet that's new to Flow: the assistant sends you a signing link (` request_wallet_proof` ) — open it, connect your wallet, and sign. The assistant won't ask you to paste a wallet address: unproven addresses can't receive a link, and the signing link proves ownership in one step.


5. **Create the link:**` create_invoice_payment_link` returns the payment link URL — that's the deliverable. Share it with your customer, who opens it, picks an asset and chain, and pays. You are shown as the beneficiary.


Example prompts once connected:


"Here's an invoice PDF — create a payment link for it. Settle to my Kraken account."


"Create a $500 payment link for consulting services for[\[email protected\]](https://notabene.id/cdn-cgi/l/email-protection#8be1eae5eecbeef3eae6fbe7eea5e8e4e6) , settled to the wallet I used last time."


## Good to know


- After you sign a wallet-proof, it can take up to a minute for the signature to be reflected — the assistant will wait and re-check rather than report a failure.
- Wallet ownership proofs are per-wallet, not per-invoice: sign once, and later invoices skip the step.
- An optional bank account adds a fiat payment fallback to the link.
- Payment links are valid for 60 days by default; if the invoice has a due date, the link stays payable until 60 days after it (capped at 2 years from creation). Wallet-proof signing links are single-use and expire after 24 hours.
- Travel-rule compliance is handled by Notabene — that's why a never-before-seen self-hosted wallet needs a signed ownership proof before it can receive a link.


‍
