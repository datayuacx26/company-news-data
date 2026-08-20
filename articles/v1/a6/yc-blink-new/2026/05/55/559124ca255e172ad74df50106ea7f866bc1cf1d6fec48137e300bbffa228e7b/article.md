---
schema_version: "1.0.0"
document_id: "559124ca255e172ad74df50106ea7f866bc1cf1d6fec48137e300bbffa228e7b"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/mcp-tutorial-claude-code"
published_at: "2026-05-14T00:48:03+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:55.365924+00:00"
content_hash: "sha256:194883dbc484b8054448593cc08859ac098d43ab3abdde8b5e3bb12dd5458eb5"
---

# MCP Tutorial: How to Connect Claude Code to Any Tool in 10 Minutes

## Step 1: Understand the MCP Config File Locations


Claude Code stores MCP server configurations in three places depending on scope.


Scope Stored in Who sees it


**Local** (default)` ~/.claude.json` under current project Just you, current project only


**Project**` .mcp.json` in project root Everyone — commit to version control


**User**` ~/.claude.json` globally Just you, all projects


**When to use each scope:**


Use **local** for personal development servers with credentials you don't want in version control — GitHub tokens, database passwords, API keys. Use **project** when you want your whole team to share the same tools: commit` .mcp.json` to the repo and everyone gets the same MCP setup on` git pull` . Use **user** for servers you rely on across every project — a filesystem server or a personal knowledge base.


The` claude mcp add` command writes to the correct config file automatically based on the` --scope` flag. You never need to hand-edit JSON unless you need advanced environment variable substitution.


## Step 2: Add Your First MCP Server


GitHub's remote HTTP MCP server is the best starting point. It connects Claude Code to your repositories, issues, pull requests, and code review — instantly.


1


#### Generate a GitHub Personal Access Token


Go to[github.com/settings/tokens](https://github.com/settings/tokens) and click **Generate new token (fine-grained)** . Select the repositories you want Claude to access and grant read/write access to code, issues, and pull requests. Copy the token value.


2


#### Add the GitHub MCP server


```text
claude   mcp   add   --transport   http   github   https://api.githubcopilot.com/mcp/   \
--header   "Authorization: Bearer YOUR_GITHUB_PAT"
```


Replace` YOUR_GITHUB_PAT` with the token from step 1. The` --transport http` flag tells Claude Code this is a remote HTTP server — no local process required.


3


#### Confirm it was added


```text
claude   mcp   list
```


You should see` github` in the list with the URL and transport type. The config lands in` ~/.claude.json` under your current project's entry.


The resulting config entry looks like this:


```text
{
"projects"  : {
"/your/project"  : {
"mcpServers"  : {
"github"  : {
"type"  :   "http"  ,
"url"  :   "https://api.githubcopilot.com/mcp/"
}
}
}
}
}
```


The Authorization header is stored securely by the CLI — it does not appear in` ~/.claude.json` in plain text.


## Step 3: Verify MCP Is Working in Claude Code


Open Claude Code in your terminal and run the` /mcp` command:


```text
/mcp


```


You'll see every connected server, its status (connected / failed / pending), and the tool count. A healthy GitHub connection shows something like` github (42 tools)` .


Test the connection immediately with a real query:


> "What are the open pull requests assigned to me in this repo?"


Claude calls the GitHub MCP tools directly and returns live data. If the server shows` failed` , run` claude mcp get github` — it returns the full config and the last error so you can debug the token or URL without guessing.


Developer configuring MCP servers in their terminal — connecting Claude Code to external tools


Blink


## Step 4: Add More MCP Servers


Every additional server follows the same pattern. Three examples that cover the most common development use cases:


**Filesystem access** — read and write files anywhere on your machine, not just inside the current project:


```text
claude   mcp   add   --transport   stdio   filesystem   --   \
npx   -y   @modelcontextprotocol/server-filesystem   ~/projects
```


**Production error monitoring** (Sentry):


```text
claude   mcp   add   --transport   http   sentry   https://mcp.sentry.dev/mcp
```


After adding, run` /mcp` in Claude Code to complete OAuth authentication in your browser. Sentry then accepts natural-language queries like "Which deployment introduced the most errors in the last 24 hours?"


**PostgreSQL database** — query your database without writing SQL:


```text
claude   mcp   add   --transport   stdio   postgres   --   \
npx   -y   @bytebase/dbhub   \
--dsn   "postgresql://readonly:password@host:5432/mydb"
```


Use a read-only database user here. Claude can then answer "Show me the schema for the orders table" or "Find customers who haven't purchased in 90 days" — writing and running the SQL on your behalf.


For servers your team should share, add` --scope project` to any of the commands above. The config writes to` .mcp.json` in the project root. Commit that file and teammates automatically get the same tool set.


## 5 Most Useful MCP Servers for Developers


The[Anthropic MCP Directory](https://mcp.run/) lists hundreds of servers. These five provide the highest signal-to-noise for most engineering teams.


### 1. GitHub


```text
--header   "Authorization: Bearer YOUR_PAT"
```


Enables: creating issues, reviewing PRs, searching code, and implementing features directly from ticket descriptions. Tell Claude "implement the feature described in issue #42" — it reads the ticket, writes the code, and opens a draft PR.


### 2. PostgreSQL (via Bytebase dbhub)


```text
claude   mcp   add   --transport   stdio   postgres   --   npx   -y   @bytebase/dbhub   \
--dsn   "postgresql://user:pass@host:5432/dbname"
```


Enables: natural-language database queries, schema inspection, and data debugging. Ask "What's our total revenue this month?" without writing a single line of SQL.


### 3. Filesystem


```text
claude   mcp   add   --transport   stdio   filesystem   --   \
npx   -y   @modelcontextprotocol/server-filesystem   /path/to/files
```


Enables: reading and writing files outside the active project directory. Essential for agents working across a monorepo or multiple services on the same machine.


### 4. Sentry


```text
claude   mcp   add   --transport   http   sentry   https://mcp.sentry.dev/mcp
```


Enables: asking "What are the top errors in the last 24 hours?" and "Which deployment introduced error ID abc123?" — directly from your Sentry account without opening a browser tab.


### 5. Notion


```text
claude   mcp   add   --transport   http   notion   https://mcp.notion.com/mcp
```


Enables: pulling spec documents, technical designs, and product requirements directly into a coding session. Claude reads the spec and writes the implementation without you pasting anything.


Add servers with` --scope project` once you want team consistency. Commit` .mcp.json` to version control and everyone gets identical tool access on` git pull` . For credentials, use environment variable expansion in` .mcp.json` — each developer sets their own token locally with` export GITHUB_TOKEN=...` .


After MCP setup — your agent can access all your tools autonomously in a single session


Blink


## Build the Full Stack With Your MCP-Connected Agent


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your Claude Code agent:


> "Build me a full-stack app using Blink Cloud, connect it to the MCP servers I've configured, and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Local scope (the default) stores the server config in` ~/.claude.json` under your current project path — only you see it, only in that project. Project scope writes to` .mcp.json` in the project root so the whole team shares the config via version control. User scope stores in` ~/.claude.json` and makes a server available across every project on your machine, still private to your account.


Add servers with` --scope project` and commit the resulting` .mcp.json` file. When teammates pull the branch, Claude Code automatically loads those servers. For servers that need credentials, use environment variable expansion: set` "${GITHUB_TOKEN}"` in the JSON config and each developer exports their own token in their shell profile. Claude Code supports` ${VAR}` and` ${VAR:-default}` syntax in` .mcp.json` .


Cursor and Claude Code both support MCP but use different config files — Cursor reads from its own settings; Claude Code reads from` ~/.claude.json` and` .mcp.json` . If you've already set up servers in Claude Desktop, run` claude mcp add-from-claude-desktop` and Claude Code will import them with an interactive selection prompt. Cursor 2.6 also added a Marketplace for MCP plugins, so many popular servers are now one-click installs in Cursor.


HTTP servers run remotely — you connect to a URL like` https://mcp.sentry.dev/mcp` . Claude Code handles reconnection automatically if the connection drops. Stdio servers run as local processes on your machine, started by Claude Code as child processes when a session begins. Use HTTP for cloud services that provide their own hosted endpoint; use stdio for local tools that need direct filesystem or system access, or when you're running your own server.


Claude Code supports as many servers as you need. As of May 2026, Tool Search is enabled by default — it defers loading tool schemas until Claude actually needs them, so adding more servers no longer inflates your context window at session start. The practical limit is the number of credentials and API keys you can manage. Most teams run 3–6 servers: a code host, a database, an error tracker, and a team knowledge tool.
