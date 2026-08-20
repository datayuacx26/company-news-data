---
schema_version: "1.0.0"
document_id: "d508d2d2172d2736b63b4aee6fdedf735b3decf1a66fc4686032d7c07fd0c941"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/launching-nao-mcp/"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:db15daf2d24ab1b3d3152945627f524e51c33bf66e382e3d3a8151ff972978ca"
---

# Launching the nao MCP

[Blog](https://getnao.io/blog/) /


product updates


# Launching the nao MCP


nao now exposes itself as an MCP server. Run governed analytics from Claude, Cursor, Codex, or any MCP-compatible tool.


12 May 2026


By Claire Gouze


Founder @ nao


We just launched the nao MCP.


Run analysis from your favorite agents. Create and fetch[nao stories](https://getnao.io/blog/how-to-turn-chat-with-data-into-shareable-stories) from anywhere. Use the nao[context layer](https://getnao.io/blog/how-to-build-context-stack-for-agentic-analytics) with your own agent and LLM tokens.


This is the next step in making nao a[headless analytics agent](https://getnao.io/blog/introducing-headless-analytics-agent) . We already support[Slack](https://getnao.io/blog/setup-ai-analytics-slack-bot-open-source) ,[Teams](https://getnao.io/blog/setup-ai-analytics-teams-bot-open-source) , WhatsApp, and Telegram. Now we're adding the tools where people code, write, and think.


## Why not just a warehouse MCP?


You could plug a warehouse MCP into Claude or Cursor and start querying data. So why nao MCP?


One word: **governance** .


When you connect a warehouse MCP directly, every agent writes its own SQL with no shared context, no business rules, no definitions. One agent says churn is 5%, another says 8%, and nobody knows which one is right.


With the nao MCP, every conversation sources from the same[context layer](https://getnao.io/blog/how-to-do-context-engineering-for-data-teams) - the one your data team curated. Same rules, same metric definitions, same data model documentation. Whether someone asks a question in Claude, Cursor, Codex, or Slack, the agent reasons with the same governed context.


And every conversation made through the MCP is stored in your nao app. You can review them, audit them, and use them to loop on context quality. That's how reliability improves over time - not by hoping the LLM gets better, but by systematically refining the context it reasons with.


## What the nao MCP exposes


The MCP endpoint has three modes, each toggleable independently from nao settings.


### Sub-agent mode


One tool: **` ask_nao`** .


Send a natural language analytics question to your nao agent. It runs the full agentic loop -[context assembly](https://getnao.io/blog/how-to-do-context-engineering-for-data-teams) , SQL generation, execution, visualization - and returns the answer with a link to the chat in nao.


text


> ask_nao("What was our revenue last week compared to the week before?")


Revenue last week: $142,300 (+8.2% vs prior week). \[View in nao UI\](https://your-nao-instance.com/chat/abc123)


You can continue a conversation by passing the` chatId` from a previous call - same as threading in Slack.


### Context-layer mode


Three tools: **` execute_sql`** , **` grep`** , **` ls`** .


This mode gives your external agent direct access to nao's data infrastructure:


- **` execute_sql`** - run SQL against your connected warehouse. Returns rows as JSON with a` query_id` you can use to embed charts in stories.
- **` grep`** - search across your nao project's context files (rules, schemas, metadata, docs).
- **` ls`** - list files and directories in the project context.


Use this when you want your agent to reason about the data itself - browse the context, write its own SQL, and build on the results. Your agent uses your LLM tokens, but nao's context layer keeps it grounded.


### Story mode


Six tools: **` list_stories`** , **` get_story`** , **` create_story`** , **` update_story`** , **` archive_story`** , **` delete_story`** .


Full CRUD on[nao Stories](https://getnao.io/blog/how-to-turn-chat-with-data-into-shareable-stories) - markdown dashboards with embedded charts, tables, and narrative text.


The typical workflow:


1. Run` execute_sql` to get data + a` query_id`
2. Call` create_story` with markdown content that includes` <chart>` or` <table>` blocks referencing that` query_id`
3. The story renders in the nao UI with live charts


Supported chart types: bar, stacked bar, line, area, stacked area, pie, KPI card, scatter, radar.


This means you can build dashboards programmatically from any AI tool. A PM in Claude Desktop can ask for a story on feature adoption, and the agent creates it directly in nao.


## How to connect


The nao MCP uses Streamable HTTP transport. Your nao instance exposes the endpoint at` /mcp` . Authentication is via bearer token, which you can generate from **Settings > MCP Endpoint** in the nao UI.


### Cursor


Go to **Settings > Tools & MCP > New MCP Server** and paste:


json


{


"mcpServers"


:


{


"nao"


:


{


"type"


:


"http"


,


"url"


:


"<your-nao-url>/mcp"


}


}


}


Cursor will prompt you to authenticate in the browser.


### Codex


Go to **Settings > MCP servers > + Add server > Streamable HTTP** . Paste the URL:` <your-nao-url>/mcp` . Authenticate in the browser when prompted.


### Claude Code


Add to your project's` .mcp.json` (or` .claude/mcp.json` ):


json


{


"mcpServers"


:


{


"nao"


:


{


"type"


:


"http"


,


"url"


:


"<your-nao-url>/mcp"


,


"headers"


:


{


"Authorization"


:


"Bearer <your-token>"


}


}


}


}


### Claude Desktop


Two options:


**Via Settings UI:** Go to **Settings > Connectors > Add custom connector** . Set Remote MCP Server URL to` <your-nao-url>/mcp` .


**Via config file** (` ~/Library/Application Support/Claude/claude_desktop_config.json` ):


json


{


"mcpServers"


:


{


"nao"


:


{


"command"


:


"npx"


,


"args"


:


\[


"-y"


,


"mcp-remote"


,


"<your-nao-url>/mcp"


\]


}


}


}


## Try it


And it's all open source.


1. Deploy nao (or update to the latest version):[github.com/getnao/nao](https://github.com/getnao/nao)
2. Go to **Settings > MCP Endpoint** and enable the endpoint
3. Add the config to your preferred AI tool (see setup guides above)
4. Ask your first question


Full docs:[docs.getnao.io/nao-agent/connectors/mcp](https://docs.getnao.io/nao-agent/connectors/mcp)


Star us on GitHub if this is useful:[github.com/getnao/nao](https://github.com/getnao/nao)


## Related articles


[insights The Agentic Analytics Playbook is out Learn how to choose your harness, build your context layer, plan your rollout, measure success, and get examples from 7 real-life companies.](https://getnao.io/blog/agentic-analytics-playbook/)[product updates We're launching the first Open Source Analytics Agent Builder We're open sourcing nao — an analytics agent framework built on context engineering. Here's our vision for what comes after black-box BI.](https://getnao.io/blog/open-source-analytics-agent-launch/)[product updates nao has a new look We rebuilt the nao interface from the ground up. New home screen, a prompt queue, visible agent reasoning, redesigned charts and stories.](https://getnao.io/blog/nao-redesign/)


Claire


For nao team
