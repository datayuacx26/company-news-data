---
schema_version: "1.0.0"
document_id: "54f533983a8ad7f5a781ef31de7a0233fb0526abc1bf9ee759143f3f1aa6caec"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/mcp"
published_at: "2026-04-07T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T22:16:00.015330+00:00"
content_hash: "sha256:d8102fe7b302a5548d962834bd29b78b299b4a052f16fb07300624012f90e6ed"
---

# Official Resend MCP Server

Today, we're excited to announce the[official Resend MCP server](https://github.com/resend/resend-mcp) .


What started as a simple send-email tool is now a full-featured integration that gives your AI agent native access to the entire Resend platform.


For more Resend agentic tools, see our[AI Onboarding Guide](https://resend.com/docs/ai-onboarding) . We also support key tools like the[Resend CLI](https://resend.com/changelog/cli) and[Resend Skills](https://resend.com/blog/introducing-email-skills) .


## Full platform coverage


The official Resend MCP server exposes 10 tool groups covering every Resend API: emails, contacts, broadcasts, domains, webhooks, segments, topics, contact properties, API keys, and received emails.


Your agent can now manage your entire email infrastructure through a single integration.


## Received emails


Agents can now list and read inbound emails, including downloading attachments to process incoming support requests, parse order confirmations, or forward messages.


## Broadcast management


Create, send, schedule, and manage broadcast campaigns directly from your MCP client. When combined with design MCPs (e.g., Figma), your agent can design and send emails without ever leaving the editor.


The tool supports personalization placeholders, preview text, and scheduling.


## Contact and audience tools


The MCP server supports full contact lifecycle management, including creating, updating, and removing contacts, managing segment memberships, and handling topic subscriptions.


Custom contact properties are also supported, giving agents the ability to build and maintain rich audience data.


## Domain and webhook management


Configure sender domains, verify DNS records, and manage tracking and TLS settings. Set up webhooks for event notifications using natural language commands with your agent.


## Attachments and advanced sending


Send emails with file attachments from local paths, URLs, or base64-encoded content. Batch sending, scheduling, CC/BCC, reply-to, tags, and topic-based sending are all supported out of the box.


## Install the Resend MCP server


To get started, install it manually for your coding agent.


We have also launched a[remote MCP server](https://resend.com/changelog/remote-mcp-server) , which includes OAuth authentication.


**Claude Code**


```text
claude mcp   add   resend   -e     RESEND_API_KEY  =  re_xxxxxxxxx -- npx   -y   resend-mcp
```


**Codex**


```text
codex mcp   add   resend   \        --env     RESEND_API_KEY  =  re_xxxxxxxxx   \      -- npx   -y   resend-mcp
```


**Cursor**


Go to Cursor Settings > MCP > Add new global MCP server.


```text
{        "mcpServers"  :     {          "resend"  :     {            "command"  :     "npx"  ,            "args"  :     [  "-y"  ,     "resend-mcp"  ]  ,            "env"  :     {              "RESEND_API_KEY"  :     "re_xxxxxxxxx"            }          }        }     }
```


For all other MCP clients, see the[MCP docs](https://resend.com/docs/mcp-server) .


## HTTP transport


In addition to the default stdio mode, the server also supports Streamable HTTP transport for remote and web-based integrations.


Start the server:


```text
npx   -y   resend-mcp   --http     --port     3000
```


The server exposes the MCP endpoint at` /mcp` . Each client authenticates by passing its own Resend API key as a Bearer token — no API key is needed at startup.


Connect from Claude Code:


```text
claude mcp add resend --transport http http://127.0.0.1:3000/mcp \
--header "Authorization: Bearer re_xxxxxxxxx"


```


Or from Cursor:


```text
{              "mcpServers"  :     {                "resend"  :     {                  "url"  :     "http://127.0.0.1:3000/mcp"  ,                  "headers"  :     {     "Authorization"  :     "Bearer re_xxxxxxxxx"     }                }              }            }
```


Optional server-side defaults (apply to all connecting clients):


- ` --sender` /` SENDER_EMAIL_ADDRESS` : default from address
- ` --reply-to` /` REPLY_TO_EMAIL_ADDRESSES` : default reply-to (comma-separated)
- ` --port` /` MCP_PORT` : port to listen on (default: 3000)


## What's next


We're continuing to expand the MCP server's capabilities as new Resend features ship. Check out the[MCP docs](https://resend.com/docs/mcp-server) or the[GitHub repo](https://github.com/resend/resend-mcp) to get started.
