---
schema_version: "1.0.0"
document_id: "d8cf8012943bd7cc0f174df50eb6498f7e77629f808ddf2423f50763f06a97ae"
company_key: "yc-laurence"
company: "Laurence"
source_id: "yc-laurence-news-import-aad1c15d62c5"
canonical_url: "https://www.laurence.com/blog/laurence-mcp-launch"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-24T02:00:49.130192+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:ff2eecf2a3f52aa0aba2c6740a97ab66b7e4fe2e14d7a354916689b0dd43808c"
---

# Laurence MCP: Amazon Ads data in Claude Code, Cursor, and Codex

## What it is


**Laurence MCP** is available to all Laurence customers. It connects coding assistants to your advertising data through the **Model Context Protocol (MCP)** —the same pattern Cursor and Claude use for tools like browsers or databases. Instead of pasting exports or screenshots, the assistant can call a small set of **read-only tools** backed by Amazon Ads APIs and ClickHouse—the same sources Ask Laurence uses—so answers are grounded in **your** numbers. Because Laurence already ingests and stores your Amazon Marketing Stream data, there is nothing extra to set up.


**Connecting is a normal browser sign-in.** The first time a tool runs, you log in with your Laurence account and approve access once. After that, sessions stay usable for ongoing work in the IDE; you are not wiring tokens by hand.


Every tool call is **scoped to stores you belong to** : you pass an Amazon Ads` profile_id` , and the server only returns data you are allowed to see.


## Amazon Marketing Stream — the data Amazon’s MCP doesn’t cover


Amazon launched their own **Ads MCP Server** in February 2026 with 50+ tools for campaign management, reporting, DSP, and AMC. It is a big step for letting AI agents manage ad accounts. But it does **not** cover **Amazon Marketing Stream (AMS)** — the real-time hourly event firehose of impressions, clicks, cost, conversions, and placements at keyword granularity.


Even for the data Amazon’s MCP does cover, **getting performance metrics is slow** . Their reporting tools generate new reports on demand or execute AMC workflows from scratch — you submit a request, wait for it to process, poll until it’s ready, then retrieve the result. That round-trip can take minutes per question. Laurence queries return in seconds — **over 5× faster** than waiting for Amazon Ads report generation and polling cycles. It is the difference between a fast analytical conversation and a batch job.


AMS is the richest advertising performance data Amazon offers, but to use it you need to set up your own **SNS → SQS → Lambda pipeline** , land the data in a warehouse like ClickHouse, and build a way to query it. Most advertisers never get there.


**Laurence MCP gives AI agents direct access to pre-stored AMS data** — hourly and daily rollups, search term reports, bid observations, keyword state — all queryable in seconds from ClickHouse, not generated on the fly. Your data is already flowing through Laurence and landing in storage; the MCP server just lets your AI tools query it the same way Ask Laurence does internally. No pipeline setup, no report generation waits.


## Why use it in Cursor or Claude Code


MCP turns the assistant into a **research and analysis teammate** inside the repo: you stay in one place—terminal, chat, or inline—and iterate with code, notes, and live campaign context together. That is different from switching to a separate tab to copy tables or writing one-off scripts for every question.


**Ask Laurence in the web UI** is there when you are already in the product—but day to day, most people’s workflows still center on an **IDE or terminal** , not another browser tab. Laurence MCP **meets you where you can do your best work** by bringing the same data into **whichever AI agent you already use** (Cursor, Claude Code, Codex, and whatever comes next), so you get one continuous place to think, build, and reason over performance instead of splitting attention across disconnected surfaces.


**Example prompts you can actually use:** “Which campaigns had the highest spend last week for profile X?” “Pull hourly AMS metrics for this campaign and flag hours where ACOS spiked.” “Show me search terms with clicks but no orders in the last 14 days.” “Compare current keyword bids to recent observations for these campaign IDs.” “What Amazon Ads profiles am I allowed to query?”


Use **list_allowed_ads_profiles** when you are unsure which` profile_id` to use; every other tool expects a` profile_id` from that list.


## Tools


Tool Purpose


` list_allowed_ads_profiles` Every Amazon Ads` profile_id` you may query (call first).


` get_daily_sales` Daily units and revenue by profile; optional ASIN filter.


` get_campaigns` Campaigns for a profile (SP, SB, SD).


` get_ad_groups` Ad groups for a campaign.


` get_campaign_keywords` Keywords and bids for campaign IDs.


` get_campaign_negative_keywords` Negative keywords for ad group IDs.


` get_bids_and_observations` Bid updates with the same-hour performance snapshot (metrics when present). Optional` keyword_id` ,` campaign_ids` , Pacific date bounds, and row` limit` .


` get_ams_events` Sponsored Products stream metrics (Pacific). Pick granularity via` interval` (` daily` rollups per Pacific day, or` hourly` raw rows capped by` limit` ). Optional` match_type` ,` placement` ,` keyword_id` , and` campaign_ids` .


` get_search_term_data` Search term report (queries and attributed performance). Optional` keyword_id` ,` campaign_ids` , and` limit` .


## Install: Claude Code


Add the server once per machine; then in any Claude Code session you can ask for campaigns, metrics, or search terms and let the agent pull the data directly.


From a terminal, add the remote MCP server over HTTP (replace the URL if your deployment differs):


```text
claude mcp add --transport http laurence \
https://laurence-ai-68564--ask-laurence-agent-mcp-server.modal.run/mcp
```


## Install: Cursor


In **Cursor Settings → MCP** , register the Laurence server so Agent and chat can invoke the tools alongside your codebase—useful when you are debugging bidding logic or writing reports against live account data.


Add a server named` laurence` that proxies to the HTTPS endpoint via` mcp-remote` :


```text
{
"laurence": {
"command": "npx",
"args": [
"-y",
"mcp-remote",
"https://laurence-ai-68564--ask-laurence-agent-mcp-server.modal.run/mcp"
]
}
}
```


## Install: Codex


OpenAI **Codex** supports MCP servers via both the CLI and the IDE extension. Add Laurence as a streamable HTTP server — configuration is shared between the CLI and extension, so you only set it up once.


**Option 1 — CLI:**


```text
codex mcp add laurence --url https://laurence-ai-68564--ask-laurence-agent-mcp-server.modal.run/mcp
```


**Option 2 — config.toml** (edit` ~/.codex/config.toml` or a project-scoped` .codex/config.toml` ):


```text
[mcp_servers.laurence]
url = "https://laurence-ai-68564--ask-laurence-agent-mcp-server.modal.run/mcp"
```


Then run` codex mcp login laurence --scopes laurence:mcp` to start the browser OAuth flow. Once authenticated, run` codex mcp` or use` /mcp` in the terminal UI to verify the server is connected.


## After connecting


The first time the assistant needs data, your **browser opens** so you can sign in to Laurence and approve access. After that, you can keep asking questions in Cursor, Claude Code, or Codex without repeating that step for every message.
