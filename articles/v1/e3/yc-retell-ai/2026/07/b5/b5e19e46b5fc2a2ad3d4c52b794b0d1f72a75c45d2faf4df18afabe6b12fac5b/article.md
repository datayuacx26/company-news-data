---
schema_version: "1.0.0"
document_id: "b5e19e46b5fc2a2ad3d4c52b794b0d1f72a75c45d2faf4df18afabe6b12fac5b"
company_key: "yc-retell-ai"
company: "Retell AI"
source_id: "yc-retell-ai-news-import-48ab15cc20a2"
canonical_url: "https://www.retellai.com/blog/retell-mcp-server"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:02.485527+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:b8b5575a42090d5058a93c542df9d1c7d6b80a5c268e31264d460bfc307e969b"
---

# Meet Retell MCP Server

Retell now supports the Model Context Protocol, or MCP, so you can build and manage voice agents directly from the AI tools you already use. That includes MCP-compatible clients like Cursor, Claude Desktop, Claude Code, Codex-style tools, and more.


With a single MCP connection, your assistant can work directly with the Retell platform to:


- create and update agents
- launch calls
- manage phone numbers
- attach knowledge bases
- run QA


If you already use Retell through our API or SDKs, MCP is the same platform through a simpler, more agent-friendly interface.


## What Retell MCP Server Can Do


Retell MCP Server gives you direct access to the Retell platform. In short, the Retell MCP Server turns your AI agent into a working interface for your voice stack so you can actually take action across your voice workflow, like creating agents, launching calls, updating phone numbers, managing knowledge bases, and running QA, all from one connection. And more specifically, here's how it can help:


### Agents


- Create agents
- Update agents
- Publish versions
- List agents
- Fetch versions


### Calls


- Start phone and web calls
- Fetch call details
- List calls
- Update call metadata
- Delete calls
- Run QA and reviews


### Phone Numbers


- Import numbers
- Provision numbers
- List numbers
- Fetch number details


### Knowledge Base


- Create knowledge bases
- Attach sources
- Remove sources
- List knowledge bases


### Voices


- List voices
- Clone voices
- Search community voices


### Chats


- Create chats
- Create chat agents
- End chats
- Update chat metadata


### Testing and QA


- Create test cases
- Run tests
- List QA runs
- Rerun QA
- Submit scores


### Alerts and Webhooks


- Create alert rules
- List alert rules
- List incidents
- Test[webhooks](https://www.retellai.com/glossary/webhook)


## Why This Matters


Building[voice AI](https://www.retellai.com/glossary/voice-ai) in production still takes too many context switches. Instead of bouncing between your agent, the Retell dashboard, your docs, scripts, and QA tools, Retell MCP Server brings those workflows into a single interface so you can build, test, and manage voice agents faster and easier.


## How It Works


Setup is simple. Your MCP client connects to Retell's hosted MCP endpoint over HTTP, authenticates with your Retell API key, and calls tools. It can also discover available tools automatically through` tools/list` , including:


- tool name
- description
- JSON input schema


So instead of hardcoding every action, your agent already knows what Retell tools exist and it knows how to use them correctly.


## What You Need to Get Started


Make sure you have:


- a Retell API key
- an MCP-compatible client like Cursor, Claude Desktop, Claude Code, or Codex
- the Retell MCP server URL: https://mcp.retellai.com


Auth format:


```text
Authorization: Bearer <RETELL_API_KEY>


```


## Simple Setup


Most MCP clients support remote MCP servers over HTTP.


Use:


- URL: https://mcp.retellai.com
- Header: Authorization: Bearer


## Example Setup


### Cursor


Open: Command Palette → Cursor Settings → MCP → Add new global MCP server


Add:


```text
{
"mcpServers": {
"retell": {
"url": "https://mcp.retellai.com",
"headers": {
"Authorization": "Bearer <RETELL_API_KEY>"
}
}
}
}


```


### Claude Desktop


Open: Settings → Developer → Edit Config


Add:


```text
{
"mcpServers": {
"retell": {
"command": "npx",
"args": [
"mcp-remote@latest",
"https://mcp.retellai.com",
"--header",
"Authorization:Bearer <RETELL_API_KEY>"
]
}
}
}


```


### Claude Code


```text
claude mcp add --transport http retell https://mcp.retellai.com --header "Authorization: Bearer <RETELL_API_KEY>"


```


### Codex


Add this to` ~/.codex/config.toml` :


```text
[mcp_servers.retell]
url = "https://mcp.retellai.com"
bearer_token_env_var = "RETELL_API_KEY"


```


Then set:


```text
export RETELL_API_KEY="<RETELL_API_KEY>"


```


### Other MCP Clients


If your client supports remote MCP URLs and headers, use:


- URL: https://mcp.retellai.com
- Header: Authorization: Bearer


## Example Prompts


Once connected, your assistant can start working with Retell right away.


Try prompts like:


- "List my agents and summarize what each one does."
- "Create a new agent for inbound sales qualification and publish it."
- "Show me the last 20 calls and flag any with low QA scores."
- "Create a knowledge base, attach these sources, and update my agent to use it."
- "Import this phone number and assign it to my agent."
- "Rerun QA on this call and summarize the failure reasons."


Now this is the fun part where MCP can really flex some power. You are not just chatting with an assistant anymore. You are giving it real platform access so it can help you build, test, analyze, and improve voice agents in place.


## Why Teams Will Love It


Retell MCP Server helps teams:


- build faster without leaving their AI client
- reduce dashboard and API overhead
- move from prototype to production faster
- run QA and operations with less manual work
- keep voice infrastructure closer to the tools developers already use


For developers, it cuts down manual integration work. For product teams, it speeds up testing and iteration. For ops teams, it makes it easier to manage calls, QA, and day-to-day agent workflows.


## Security and Safe Usage


Connecting LLMs to operational tools is huge, but it also comes with risk. The biggest risk is prompt injection. That means untrusted content like transcripts, user messages, or knowledge base documents can include hidden instructions that try to make the model do something you did not ask for. Most MCP clients support "confirm before running tools."


**Keep that on.**


## Best Practices


Treat MCP like giving your assistant real operator access.


We recommend:


- **Use least privilege** — Create API keys with only the permissions you need.
- **Keep keys out of prompts** — Store API keys in client settings or secrets. Never paste them into chat.
- **Prefer read-first workflows** — Fetch the current state before updating or deleting anything.
- **Review destructive actions** — Actions like delete *and publish* should require clear user intent.
- **Be careful with sensitive data** — Call logs and transcripts may contain PII. Only expose what you need.
- **Use non-production environments when possible** — Start in development before connecting production systems.


## Troubleshooting


If something isn't working, here are the first things to check.


### Tools aren't showing up


- Make sure the MCP server URL is correct
- Confirm the Authorization header is set correctly
- Check that your client can reach the Retell MCP endpoint


### 401 Unauthorized


- Verify your header is` Authorization: Bearer <RETELL_API_KEY>`
- Make sure your API key is active and valid


### Tool call errors


- Try again with a smaller or simpler input
- Check the returned error message for details
- Many MCP clients show structured tool errors directly, which can help pinpoint the issue


## Start Building with Retell MCP


Retell MCP Server makes it easy to use Retell from the AI tools your team already works in. That means less switching between tools, faster iteration, and a more direct way to build and manage production voice agents.


If you already use Retell, MCP gives you a faster workflow. If you are new to Retell, it is one of the simplest ways to get started. Connect your client. Add your API key. Start building. Let's go!


## Need Help?


If you run into issues or want to request more MCP capabilities, reach out to support@retellai.com.
