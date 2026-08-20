---
schema_version: "1.0.0"
document_id: "12a8f79d5ecb8d6bab7056c5882bbd0235218b9e49bb7c6be57b817dd1985d5a"
company_key: "yc-pinnacle"
company: "Pinnacle"
source_id: "yc-pinnacle-news-import-eb12d43fa2a4"
canonical_url: "https://pinnacle.sh/blog/send-messages-with-mcp"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-22T09:08:45.431702+00:00"
fetched_at: "2026-07-28T22:15:36.111958+00:00"
content_hash: "sha256:5f1ceae84e2b40ce08fcf6d85097e330127a5c80b97897378113c27e510a3665"
---

# Send and Manage Messages with the Pinnacle MCP

Your AI agent can now send a text message. Not by calling a function you wrote — by picking up the phone itself.


The[Pinnacle MCP server](https://docs.pinnacle.sh/mcp) is a[Model Context Protocol](https://modelcontextprotocol.io/) server that exposes 95 tools covering the full Pinnacle platform. Claude, Cursor, Codex, or any other MCP-compatible agent can send messages, manage conversations, spin up audiences, fire blasts, check compliance status, and more — all through tool calls, no glue code required.


## What is MCP?


Model Context Protocol (MCP) is an open standard that lets AI assistants talk to external services through a structured tool interface. Think of it as the API layer between "an AI that can reason" and "a system that can act."


With Pinnacle's MCP server, you connect once and your AI agent gets direct access to your messaging infrastructure. No custom function wrappers. No prompt engineering to format API payloads. Just tools the agent can call by name.


## Setup in 60 Seconds


### Cursor (Remote — recommended)


Add to` .cursor/mcp.json` :


JSON


```text
{
"  mcpServers  "  :   {
"  pinnacle  "  :   {
"  url  "  :   "  https://mcp.pinnacle.sh/mcp  "  ,
"  headers  "  :   {
"  PINNACLE-API-KEY  "  :   "  pnclk_...  "
}
}
}
}
```


No install needed — Cursor connects directly to Pinnacle's hosted MCP server.


### Claude Code


Bash


```text
claude   mcp   add   pinnacle   --transport   http   https://mcp.pinnacle.sh/mcp   \
-H   "  PINNACLE-API-KEY: pnclk_...  "
```


### Claude Desktop (Local)


Add to your Claude config (` claude_desktop_config.json` ):


JSON


```text
{
"  mcpServers  "  :   {
"  pinnacle  "  :   {
"  command  "  :   "  npx  "  ,
"  args  "  :   [  "  @pinnacle-rcs/mcp  "  ],
"  env  "  :   {
"  PINNACLE_API_KEY  "  :   "  pnclk_...  "
}
}
}
}
```


## What the 95 Tools Cover


### Messaging


The core: send any channel to any recipient.


Tool What it does


` send_sms` Send a plain-text SMS


` send_mms` Send MMS with images, video, or files


` send_rcs` Send RCS with cards, carousels, quick replies, and buttons


` send_typing_indicator` Show a typing indicator to the recipient


` react_to_message` React to a message with an emoji


` validate_sms` Check SMS payload before sending


` validate_mms` Check MMS payload before sending


` validate_rcs` Check RCS payload before sending


### Conversations


Manage live message threads.


Tool What it does


` list_conversations` List all conversations with filters


` get_conversation` Get a single conversation


` update_conversation` Mark as read, archive, or assign metadata


` list_conversation_messages` Paginate messages in a thread


` list_messages` Query messages across all conversations


` get_message` Fetch a specific message with full payload


### Contacts


Your contact book as a tool.


Tool What it does


` create_contact` Create a contact with name and metadata


` upsert_contact_card` Set or update a contact card


` update_contact` Edit contact fields


` get_contact` Fetch a contact by phone number or ID


` list_contacts` List contacts with search and filters


### Audiences and Blasts


Reach segments at scale.


Tool What it does


` create_audience` Create a named audience with contact IDs


` add_audience_contacts` Append contacts to an existing audience


` remove_audience_contacts` Remove contacts from an audience


` update_audience` Rename or modify audience metadata


` delete_audience` Delete an audience


` list_audiences` List all audiences


` blast_sms` Send SMS to an entire audience


` blast_mms` Send MMS to an entire audience


` blast_rcs` Send RCS to an entire audience


` list_blasts` List all past blasts with stats


### Phone Numbers


Tool What it does


` search_phone_numbers` Search available numbers by area code or pattern


` buy_phone_numbers` Purchase a number


` list_phone_numbers` List owned numbers


` get_phone_number_details` Get metadata for a specific number


` get_phone_number_status` Check registration and compliance status


### RCS Agents and Testing


Tool What it does


` list_agents` List your RCS agents


` create_test_agent` Create a sandbox test agent


` get_test_agent` Fetch test agent details


` update_test_agent` Modify test agent config


` whitelist_test_agent` Whitelist a number for sandbox testing


` get_rcs_capabilities` Check if a number can receive RCS


` generate_rcs_link` Generate a deep link to open an RCS conversation


### Brands and Compliance


Tool What it does


` get_brand` Fetch brand registration details


` autofill_brand` Auto-fill brand fields from website or EIN


` validate_brand` Check brand registration for errors


` vet_brand` Pre-screen brand before formal submission


` submit_brand` Submit brand for 10DLC registration


` upsert_brand` Create or update a brand


Similar tools exist for 10DLC campaigns (` upsert_dlc_campaign` ,` submit_dlc_campaign` ,` get_dlc_campaign_status` ), RCS campaigns (` upsert_rcs_campaign` ,` submit_rcs_campaign` ), and toll-free campaigns.


### Webhooks and Utilities


Tool What it does


` get_webhooks` List configured webhooks


` attach_webhook` Register a new webhook endpoint


` detach_webhook` Remove a webhook


` create_shortened_url` Create a tracked short link


` get_shortened_url` Fetch click stats for a short link


` upload_file` Upload a media file for use in messages


` search_pinnacle_docs` Search Pinnacle documentation


` cancel_scheduled_message` Cancel a scheduled message


## Real Agent Workflows


### Automated Appointment Reminders


> "Create an audience from the list of patients with appointments tomorrow, then blast them an RCS message with a Confirm and Reschedule button."


The agent will: create the audience → blast RCS with quick reply buttons → return delivery stats. You write zero code. View results in the[analytics dashboard](https://app.pinnacle.sh/dashboard/analytics) .


### Inbound Lead Qualification


> "Check the last 20 conversations for anyone who said 'interested' or 'how much'. Send them an SMS with our pricing page link."


The agent will: list conversations → filter by keyword → send SMS with link → summarize what it sent. Track all threads in the[conversations dashboard](https://app.pinnacle.sh/dashboard/conversations) .


### Compliance Setup


> "I need to register a 10DLC brand for my company. Our website is acme.com."


The agent will: call` autofill_brand` to pull your EIN and company details from your website → validate → walk you through approval step by step. Manage your brand from the[brands dashboard](https://app.pinnacle.sh/dashboard/brands) .


### Send a Quick Test Message


> "Send an RCS message to my test number +14155551234 with a card showing our new product launch."


Done. No SDK, no API docs, no JSON to format.


## Key Takeaways


- Pinnacle's MCP server has 95 tools covering messaging, conversations, contacts, audiences, blasts, phone numbers, RCS agents, brands, compliance, webhooks, and more.
- Setup takes one config entry in Claude Desktop, Cursor, or any MCP host.
- Agents can execute multi-step workflows — audience creation, blast, status check — autonomously.
- The` search_pinnacle_docs` tool means your agent can look up API behavior on the fly.


## FAQ


**1. Which AI clients support MCP?** Claude Desktop, Claude.ai, Cursor, Cline, Codex, and any application that implements the MCP client spec. The list is growing fast.


**2. Is my API key safe in the MCP config?** Your API key is stored in your local config file and passed as an environment variable to the MCP server process — it's never sent to the AI model itself.


**3. Can I use the MCP server in production automation?** Yes. You can run the Pinnacle MCP server as a sidecar process in your agent infrastructure. Point your MCP client at it and your agent can message users as part of any workflow.


**4. What's the difference between using the MCP and the SDK?** SDK is for programmatic control in your code. MCP is for AI agents that reason about what to send and when. They're complementary.


**5. How do I get a Pinnacle API key?** Sign up at[app.pinnacle.sh](https://app.pinnacle.sh/) , then navigate to **Settings → API Keys** in the[dashboard](https://app.pinnacle.sh/dashboard) .


[Book a 30-minute call](https://cal.com/rcs/30min?notes=Interested+in+Pinnacle+MCP+integration) with the Pinnacle team — we'll help you integrate Pinnacle into your AI agent stack and get you live fast.
