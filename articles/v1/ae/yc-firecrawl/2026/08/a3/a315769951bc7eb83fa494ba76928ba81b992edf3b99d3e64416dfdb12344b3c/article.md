---
schema_version: "1.0.0"
document_id: "a315769951bc7eb83fa494ba76928ba81b992edf3b99d3e64416dfdb12344b3c"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/best-mcp-servers-gemini-cli"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T21:23:15.984605+00:00"
fetched_at: "2026-08-18T21:23:19.071987+00:00"
content_hash: "sha256:5b691e8958d5e0cc2f4a0d63eb9914a4405947c40564d011bd51eeafbd0c3b74"
---

# Best MCP Servers for Gemini CLI in 2026

**TL;DR:** Best MCP servers for Gemini CLI


- **Firecrawl MCP** : web search, scrape, crawl, parse, and browser interaction in one server
- **Playwright MCP** : browser automation and E2E testing through the accessibility tree
- **Composio MCP** : 250+ SaaS integrations behind a single remote endpoint
- **Asana MCP** : read and update tasks, projects, and workflows straight from the CLI
- **Context7 MCP** : injects real, version-specific docs into your prompts so the model stops hallucinating APIs
- **Slack MCP** : post messages, read threads, and search channels without leaving the terminal
- **Supabase MCP** : manage Postgres schemas, auth, storage, and edge functions on your Supabase projects
- **Filesystem MCP** : scoped local file read and write, the safest way to give the model disk access
- **Ahrefs MCP** : pull live SEO data (backlinks, keywords, rankings, site audits) into your session
- **Hex MCP** : run analytical questions against your data warehouse through Hex's Threads agent
- **Profound MCP** : track brand visibility, citations, and sentiment across ChatGPT, Claude, and Perplexity


---


Gemini CLI is growing in popularity. The 1M-token context window is genuinely useful when I paste a whole codebase, a competitor's blog stack, or a set of Ahrefs exports into a session, but the base install is quiet. It knows how to fetch a URL and read a file. Not much else. The moment I want it to touch Slack, run a browser, or pull real SEO data for a keyword it has never heard of, I need MCP.


I lead growth marketing at Firecrawl, so the servers I keep configured lean marketing-heavy: SEO tools, brand visibility trackers, Slack, project management, and the analytics warehouse where our numbers live. But I'm also a builder (like most folks today), so the engineering and ops MCPs stay installed too: browser automation, filesystem, databases, live docs. This list covers both sides.


Anthropic released the Model Context Protocol in November 2024. Since then the ecosystem has grown to over 17,000 publicly listed servers on mcp.so, and Google folded MCP support into Gemini CLI in mid-2025. In Zuplo's[State of MCP survey](https://zuplo.com/mcp-report) , 70% of respondents said they already run between 2 and 7 servers in their daily setup, and 49% cited developer productivity as the primary ROI. Gemini CLI shipped native support (stdio, SSE, and streamable HTTP transports plus OAuth) so any server on the wider list works here too.


This post is Gemini-CLI-first: every install snippet is the exact JSON you paste into` ~/.gemini/settings.json` , every command is` gemini mcp` not` claude mcp` . These are the best MCP servers for Gemini CLI I keep connected across projects.


---


## What is an MCP server?


MCP (Model Context Protocol) is an open standard from Anthropic that lets an AI client talk to external tools and data sources through one shared interface, instead of a bespoke plugin per client.


Before MCP, every combination of AI client and tool needed its own integration: GitHub had to be wired separately for Claude, for Cursor, for ChatGPT, for VS Code. Anthropic called this the "N x M problem." MCP replaces all of that with a single JSON-RPC 2.0 protocol. You write the server once, and every compliant client (Gemini CLI, Claude Code, Cursor, Windsurf, Codex, VS Code, Cline) can use it.


The protocol defines three primitives your server can expose:


- **Tools** : actions the AI can invoke (run a search, deploy code, create a Slack message)
- **Resources** : data the AI can read (files, DB records, API responses)
- **Prompts** : reusable templates the server exposes to the client


And two transports Gemini CLI understands:


- **stdio** : the server runs locally as a subprocess, Gemini CLI starts and stops it for you
- **HTTP or SSE** : the server runs remotely or on localhost, communicated over a network endpoint, usually with OAuth


That's the whole model. The rest is picking servers that solve problems you have.


---


## How MCP servers work in Gemini CLI


Gemini CLI reads its MCP config from` ~/.gemini/settings.json` for user-scoped servers, or` .gemini/settings.json` in a project directory for project-scoped ones. The schema lives under a top-level` mcpServers` object.


### The config file


Every server entry needs exactly one of` command` ,` url` , or` httpUrl` , which picks the transport:


```text
{
"mcpServers"  :   {
"local-server"  :   {
"command"  :   "npx"  ,
"args"  :   [  "-y"  ,   "some-mcp-package"  ]  ,
"env"  :   {
"API_KEY"  :   "$MY_API_KEY"
}  ,
"timeout"  :   30000  ,
"trust"  :   false
}  ,
"remote-server"  :   {
"httpUrl"  :   "https://api.example.com/mcp"  ,
"headers"  :   {
"Authorization"  :   "Bearer $EXAMPLE_TOKEN"
}
}
}
}
```


A few Gemini-CLI-specific quirks worth knowing:


- **Server names should use hyphens, not underscores.** Tools get automatically namespaced as` mcp_serverName_toolName` .
- **Environment variables expand with` $VAR_NAME` or` ${VAR_NAME}` .** Keep API keys in your shell env, never in the JSON.
- **Set` trust: false` .** This makes Gemini CLI prompt before every tool call. Only flip it to` true` for read-only servers you've watched work.
- **Use` includeTools` and` excludeTools`** to allowlist or blocklist specific tools inside a big server.` excludeTools` wins over` includeTools` when both are set.


### Adding servers from the CLI


If you don't want to edit JSON by hand,` gemini mcp` mirrors the config schema:


```text
# Local stdio server
gemini   mcp   add   firecrawl   npx   -y   firecrawl-mcp


# Remote HTTP server
gemini   mcp   add   --transport   http   firecrawl   https://mcp.firecrawl.dev/v2/mcp


# Remote SSE server
gemini   mcp   add   --transport   sse   some-service   https://api.example.com/sse/


# Pass environment variables at add time
gemini   mcp   add   -e   API_KEY=xxx   -e   DEBUG=  true   my-server   npx   -y   my-package
```


Then list, enable, disable, or remove:


```text
gemini   mcp   list
gemini   mcp   enable   firecrawl
gemini   mcp   disable   firecrawl
gemini   mcp   remove   firecrawl
```


### The` /mcp` slash command


Inside a Gemini CLI session,` /mcp` shows every connected server, its status (` CONNECTED` ,` CONNECTING` ,` DISCONNECTED` ), and the tools each one exposes.` /mcp auth serverName` walks you through OAuth for remote servers. Use it as your smoke test after adding a new entry: if the server shows` CONNECTED` and lists tools, you're wired up.


---


## How I picked these


I only kept servers I actually reach for in Gemini CLI, not everything with a repo. Four filters:


1. **Solves a real gap in the base CLI.** Web fetch, disk access, and shell commands are already there. I wanted servers that add real capability: full-browser scraping, live docs, SaaS reach, database ops.
2. **Actively maintained.** Commits within the last 90 days, an official or well-established maintainer, and a working install today.
3. **Copy-pasteable install for Gemini CLI.** If the connection needs a two-day setup, it doesn't ship.
4. **Non-overlapping.** No two servers on the list do the same job. If two candidates overlap, I picked the one with the wider surface.


For the broader, client-agnostic set see[Best MCP Servers for Developers in 2026](https://www.firecrawl.dev/blog/best-mcp-servers-for-developers) . For search-only options, see[Best Web Search MCP Servers](https://www.firecrawl.dev/blog/best-web-search-mcp) .


---


## The best MCP servers for Gemini CLI


### 1. Firecrawl MCP: web context stack


**[Firecrawl MCP](https://docs.firecrawl.dev/mcp-server) turns Gemini CLI into an agent that can actually read the live web.**


Gemini CLI's built-in fetch tool works fine on plain HTML docs and struggles on anything else: JavaScript-rendered SPAs, PDFs, pages behind a login, dynamic dashboards. Firecrawl runs a real browser on its servers and returns clean markdown for any of them. It's the same web stack that powers Claude Code, Codex, and Cursor by default, and it slots into Gemini CLI as a single remote endpoint with no local install.


- ` firecrawl_search` :[search the live web and return full pages](https://www.firecrawl.dev/blog/mastering-firecrawl-search-endpoint) not just links, with time filters (` qdr:d` ,` qdr:w` )
- ` firecrawl_scrape` :[any URL to clean markdown or JSON](https://www.firecrawl.dev/blog/mastering-firecrawl-scrape-endpoint) , with JS waiting, mobile rendering, and tag filtering
- ` firecrawl_parse` : PDFs and documents to text, so Gemini can read reports and manuals alongside HTML ([announcement](https://www.firecrawl.dev/blog/introducing-parse) )
- ` firecrawl_crawl` and` firecrawl_map` :[crawl entire sites](https://www.firecrawl.dev/blog/mastering-the-crawl-endpoint-in-firecrawl) or discover URL structure before deciding what to scrape
- ` firecrawl_interact` : scrape a page and take actions in it (click buttons, fill forms, log in) with[natural language prompts or Playwright code](https://docs.firecrawl.dev/features/interact) ([browser automation API](https://www.firecrawl.dev/blog/introducing-interact-endpoint) )


You can use Firecrawl in Gemini CLI without a signup: paste the[Firecrawl keyless endpoint](https://www.firecrawl.dev/blog/firecrawl-keyless-launch) into your config and Search, Scrape, and Parse work immediately, rate-limited per IP.


**Install (keyless, no signup):**


```text
{
"mcpServers"  :   {
"firecrawl"  :   {
"httpUrl"  :   "https://mcp.firecrawl.dev/v2/mcp"
}
}
}
```


Or from the terminal:


```text
```


The keyless endpoint exposes Search, Scrape, and Parse rate-limited per IP. When you outgrow it, swap in the OAuth endpoint (` https://mcp.firecrawl.dev/v2/mcp-oauth` ) or pass an API key with an` Authorization: Bearer` header for the full Crawl, Map, Interact, and Agent set. The[Firecrawl MCP docs](https://docs.firecrawl.dev/mcp-server) walk through all three modes.


**Example prompts:**


```text
"Find the latest benchmarks comparing Bun and Node.js, then scrape the top 3 sources into structured JSON"
"Log into staging.acme.com, go to /admin/reports, and extract the last 7 days of KPIs"
```


**Honest take:** This is the server I'd install first on any Gemini CLI setup. The keyless mode means you can add it in one line and see it work before deciding whether to pay for anything. Pairing Search with Scrape in one call plays unusually well with Gemini's 1M-token context window: you can dump a whole result set back in and reason over it.


**Cons:** The keyless tier is fine for one-off tasks and too tight for a real workflow.


Reference:[Firecrawl MCP docs](https://docs.firecrawl.dev/mcp-server) |[Gemini CLI + Firecrawl walkthrough](https://www.firecrawl.dev/blog/gemini-cli-firecrawl)


---


### 2. Playwright MCP: browser automation


**[Playwright MCP](https://github.com/microsoft/playwright-mcp) from Microsoft gives Gemini CLI direct control of a real browser.**


Unlike screenshot-based[AI browser automation](https://www.firecrawl.dev/blog/best-browser-agents) approaches, Playwright MCP drives the browser through the accessibility tree, which is faster and more reliable than pixel matching. Any task that ends in "and then verify it worked in a browser," from local dev testing to reproducing a bug someone described in a ticket, becomes a single prompt.


- ` browser_navigate` : open a URL
- ` browser_click` ,` browser_type` ,` browser_select_option` : interact with any element
- ` browser_snapshot` : capture the accessibility tree, the model's primary way of understanding the page
- ` browser_take_screenshot` : full-page or element screenshots
- ` browser_evaluate` : run arbitrary JavaScript in the page context


**Install:**


```text
{
"mcpServers"  :   {
"playwright"  :   {
"command"  :   "npx"  ,
"args"  :   [  "-y"  ,   "@playwright/mcp@latest"  ]
}
}
}
```


**Example prompts:**


```text
"Navigate to localhost:3000/checkout and verify the form submits with card 4242 4242 4242 4242"
"Screenshot the dashboard at mobile viewport and flag any layout issues"
"Walk the signup flow and tell me the exact error text when I submit an invalid email"
```


**Honest take:** The accessibility-tree approach is what makes this actually usable. Screenshot-only browser MCPs make the model burn tokens describing what it sees before it can decide what to do. Playwright hands it a structured snapshot, so it clicks the right thing on the first try.


**Cons:** First launch installs Chromium and it's not fast. If you're on a corporate network with cert pinning or restricted binaries, this one is a headache to get working.


Repo:[github.com/microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) .


---


### 3. Composio MCP: 250+ SaaS integrations


**[Composio](https://composio.dev/) exposes one remote endpoint per toolkit (Gmail, GitHub, Slack, Notion, Linear, Salesforce, HubSpot, and 240-something others) with OAuth handled on their side.**


Instead of running a separate local server per SaaS product, you spin up a Composio MCP server per toolkit through their dashboard and get a URL. Composio manages the OAuth token dance, the refresh, and the tool schema, so your Gemini CLI config stays a two-line snippet even for services with painful auth flows.


- ` expose one toolkit per server` : pick Gmail, GitHub, HubSpot, Salesforce, or any of 250+ apps
- ` oauth-managed` : Composio stores the tokens and refreshes them, your CLI never sees them
- ` allowed_tools scoping` : restrict a server to a subset of actions rather than every tool in the toolkit
- ` works from any MCP client` : same URL powers Claude, Cursor, or Gemini CLI


**Install (after creating a server in the Composio dashboard):**


```text
{
"mcpServers"  :   {
"composio-gmail"  :   {
"httpUrl"  :   "https://backend.composio.dev/v3/mcp/YOUR_SERVER_ID?user_id=YOUR_USER_ID"  ,
"headers"  :   {
"x-api-key"  :   "$COMPOSIO_API_KEY"
}
}
}
}
```


**Example prompts:**


```text
"Search my Gmail for any unread messages about the API outage and summarize them"
"Look at my last 10 HubSpot deals and flag the ones with no activity in 14 days"
```


**Honest take:** Composio wins when your workflow spans three or four SaaS tools. The alternative is running that many separate MCP servers, each with its own OAuth setup and quirks. The tradeoff is you're routing through Composio's infrastructure, which some security teams will not accept for production accounts.


**Cons:** One MCP server per toolkit means you're editing settings.json a lot if you want ten tools. The docs push their SDK sessions over MCP for anything complex, so read carefully before assuming feature parity.


Reference:[docs.composio.dev/docs/mcp-overview](https://docs.composio.dev/docs/mcp-overview) .


---


### 4. Asana MCP: tasks and projects


**Asana ships an official remote MCP server at` https://mcp.asana.com/v2/mcp` that reads and updates tasks, projects, and workflows.**


If your team lives in Asana, this collapses the loop between "I finished the change" and "the ticket says so." Ask Gemini CLI to update a task's status, add a comment linking the PR, or spin up a follow-up task in the same project, without opening the tab.


- ` read tasks` : fetch task details, comments, subtasks, dependencies
- ` update tasks` : change status, assignee, due date, custom fields
- ` create tasks and subtasks` : with descriptions, tags, and section placement
- ` search` : by project, assignee, or custom filter


**Install:**


```text
{
"mcpServers"  :   {
"asana"  :   {
"httpUrl"  :   "https://mcp.asana.com/v2/mcp"
}
}
}
```


First use triggers OAuth in the browser through` /mcp auth asana` . Credentials never touch your settings.json.


I whipped up custom skills on top of this MCP for my team and it's been a game-changer: canned prompts for weekly planning, standup summaries, and quarterly rollups that all speak the same task shape. We actually jumped from Linear to Asana partly because the MCP + skills combo made Asana feel closer to the CLI than Linear did at the time.


**Example prompts:**


```text
"Find every Asana task assigned to me due this week and summarize them"
"Add a comment to task ABC-123 with the PR link and mark it as In Review"
"Create three subtasks under the 'Q3 launch' task, one per checklist item"
```


**Honest take:** Because it's an official Asana server on HTTP + OAuth, it's the lowest-friction integration on this list. No API key setup, no local install, no maintenance when Asana's API changes.


**Cons:** Read-through-comments works well, but the model sometimes over-eagerly updates task status if you don't scope the prompt tightly. Keep` trust: false` and confirm each write.


Reference:[developers.asana.com/docs/mcp-server](https://developers.asana.com/docs/mcp-server) .


---


### 5. Context7 MCP: live library docs


**[Context7](https://github.com/upstash/context7) pulls up-to-date, version-specific documentation and code examples directly into your prompt.**


Every model has a knowledge cutoff. If you ask Gemini CLI about a library released last month, or a breaking change in a major version, it will confidently invent an API that doesn't exist. Context7 solves this by injecting the actual current docs into the context on demand. It's at 60.9k GitHub stars, which is unusual for something that shipped in 2025.


- ` resolve-library-id` : turn a library name into the canonical Context7 slug
- ` get-library-docs` : pull docs for a specific library and version into context
- ` works across languages` : JS, Python, Rust, Go, and dozens more
- ` runs against the real source` : pulled from repos and official docs, not model memory


**Install (remote HTTP, no key needed):**


```text
{
"mcpServers"  :   {
"context7"  :   {
"httpUrl"  :   "https://mcp.context7.com/mcp"
}
}
}
```


Or local via npx:


```text
gemini   mcp   add   context7   npx   -y   @upstash/context7-mcp
```


**Example prompts:**


```text
"use context7 to look up Next.js 16 App Router caching, then rewrite this route"
"pull the latest Tailwind 4.2 docs and refactor these classes"
"grab Drizzle ORM's current relational query API and convert this raw SQL"
```


**Honest take:** The pattern that works best is telling the model "use context7" explicitly in the prompt. Left to its own devices, Gemini sometimes trusts its training data and skips the lookup. Once you build the habit, it kills a whole class of hallucinated-API bugs.


**Cons:** Coverage is best for popular libraries. Obscure or very new packages may not be indexed, and the model has to fall back to Firecrawl or web fetch.


Repo:[github.com/upstash/context7](https://github.com/upstash/context7) .


---


### 6. Slack MCP: post, read, search


**Slack MCP posts messages, reads threads, searches channels, and lists workspace members without leaving the terminal.**


Most of the daily context Gemini CLI needs about a decision (why we chose approach X, what the on-call said about the incident, who owns this service) lives in Slack threads. Piping that back into the model closes a lot of "wait, why did we do it this way" loops.


- ` slack_post_message` : post to a channel or DM
- ` slack_reply_to_thread` : keep threaded context intact
- ` slack_get_channel_history` : read recent messages in a channel
- ` slack_get_thread_replies` : pull a full thread into context
- ` slack_search_messages` : search across the workspace


**Install:**


```text
{
"mcpServers"  :   {
"slack"  :   {
"command"  :   "npx"  ,
"args"  :   [  "-y"  ,   "@modelcontextprotocol/server-slack"  ]  ,
"env"  :   {
"SLACK_BOT_TOKEN"  :   "$SLACK_BOT_TOKEN"  ,
"SLACK_TEAM_ID"  :   "$SLACK_TEAM_ID"
}
}
}
}
```


You need a Slack app with` channels:history` ,` chat:write` ,` users:read` , and` search:read` scopes; the bot token starts with` xoxb-` .


**Example prompts:**


```text
"Search #eng for messages about the API 502s last week and summarize the root cause"
"Post a short deployment summary to #releases, then update the pinned message with today's build"
```


**Honest take:** The setup step (creating a Slack app, granting scopes, installing to the workspace) is the friction point. Once it's done you rarely think about it again, and the payoff is that Gemini CLI can read the actual context around a decision instead of guessing.


**Cons:** Search quality depends on your workspace hygiene. Big, noisy workspaces return a lot of matches for common terms, and you have to prompt the model to filter.


Repo:[github.com/modelcontextprotocol/servers/tree/main/src/slack](https://github.com/modelcontextprotocol/servers/tree/main/src/slack) .


---


### 7. Supabase MCP: your Postgres and backend


**The[official Supabase MCP server](https://github.com/supabase-community/supabase-mcp) lets Gemini CLI manage schemas, run queries, and deploy edge functions on any Supabase project.**


If your app runs on Supabase, this is the fastest way to give Gemini CLI real ownership of the backend surface: schema migrations, RLS policy tweaks, seed data, and log inspection all in the same session where you're editing the frontend.


- ` list_tables` and` execute_sql` : read the schema, run arbitrary SQL
- ` apply_migration` : apply a named SQL migration to the project
- ` create_project` and` list_projects` : manage projects across your account
- ` get_logs` and` get_advisors` : pull runtime logs and security/perf advisor findings
- ` deploy_edge_function` : ship a Deno-based edge function


**Install:**


```text
{
"mcpServers"  :   {
"supabase"  :   {
"command"  :   "npx"  ,
"args"  :   [
"-y"  ,
"@supabase/mcp-server-supabase@latest"  ,
"--access-token"  ,
"$SUPABASE_ACCESS_TOKEN"
]
}
}
}
```


Generate a personal access token from your Supabase account settings and expose it as` SUPABASE_ACCESS_TOKEN` in your shell.


**Example prompts:**


```text
"Show me the schema for the users table and add a soft-delete deleted_at column via migration"
"Read the last hour of edge function logs and tell me what's failing"
"Draft an RLS policy for the invoices table so users only see their own rows"
```


**Honest take:** For solo projects and small teams this is a huge productivity unlock. It hands the model a working DB and a migration path in one config.


**Cons:** Give it a scoped token: a personal access token that owns everything in your Supabase account is a lot of power for an AI to hold. Use a service-role-scoped token per project when the server supports it, and set` trust: false` so writes still prompt.


Repo:[github.com/supabase-community/supabase-mcp](https://github.com/supabase-community/supabase-mcp) .


---


### 8. Filesystem MCP: safe scoped disk access


**The[Filesystem MCP](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem) from the official Anthropic reference set gives Gemini CLI read/write access to a specific directory allowlist.**


Gemini CLI already reads files, but the built-in tool has different semantics from the standard filesystem MCP, and once you have other agents (Claude Code, Cursor) touching the same directories you want one shared server with consistent behavior. Filesystem MCP is that server: predictable, scoped, and boring in the best way.


- ` read_file` and` read_multiple_files` : pull one or many files into context
- ` write_file` : write a file inside an allowed directory
- ` edit_file` : line-based edits, safer than full rewrites
- ` create_directory` ,` list_directory` ,` move_file` : standard file ops
- ` search_files` : recursive filename search inside allowed roots


**Install (scope to specific directories):**


```text
{
"mcpServers"  :   {
"filesystem"  :   {
"command"  :   "npx"  ,
"args"  :   [
"-y"  ,
"@modelcontextprotocol/server-filesystem"  ,
"/Users/you/projects"  ,
"/Users/you/notes"
]
}
}
}
```


Every path after the package name is added to the allowlist. Anything outside those roots is off-limits.


**Example prompts:**


```text
"Search ~/projects for every TODO comment mentioning auth and gather them into notes/auth-todos.md"
"Read every markdown file in ~/notes/meetings/2026-08 and give me a weekly summary"
```


**Honest take:** The value here isn't the feature set, it's the sandboxing. Path allowlisting means you can hand Gemini CLI write access without worrying it'll go touch` ~/.ssh` or your git config.


**Cons:** No git awareness. If you want the model to make PRs or check history, layer a git MCP on top of this one.


Repo:[github.com/modelcontextprotocol/servers/tree/main/src/filesystem](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem) .


---


### 9. Ahrefs MCP: live SEO data


**[Ahrefs MCP](https://ahrefs.com/mcp) hooks Gemini CLI directly into Ahrefs' backlink index, keyword database, rank tracker, and site audit reports.**


If any part of your work touches SEO (competitive research, keyword sizing, backlink audits, tracking a client's ranking), this collapses the "open Ahrefs, run the report, paste the CSV into the prompt" loop into a single question. The MCP is available on Ahrefs' Lite, Standard, Advanced, and Enterprise plans, with row limits scaling by tier.


- ` site_explorer_*` : pull organic keywords, top pages, backlinks, referring domains, and traffic history for any domain
- ` keywords_explorer_*` : search volume, difficulty, matching terms, and volume history for any keyword or list
- ` rank_tracker_*` : monitor tracked keywords across projects and competitors
- ` site_audit_*` : read audit issues, page content, and crawler findings on projects you own
- ` brand_radar_*` : track brand mentions and share-of-voice across AI answers and search
- ` web_analytics_*` and` gsc_*` : pull first-party analytics and Google Search Console data on connected properties


**Install:**


```text
{
"mcpServers"  :   {
"ahrefs"  :   {
"httpUrl"  :   "https://mcp.ahrefs.com/mcp"
}
}
}
```


First use triggers OAuth in the browser through` /mcp auth ahrefs` , so the token stays out of settings.json. Confirm the exact endpoint from your Ahrefs account under API & Integrations, as it can vary by workspace.


**Example prompts:**


```text
"Pull the top 20 organic keywords for firecrawl.dev and flag any where we rank 4-10"
"Get search volume, KD, and matching terms for 'headless browser'"
"Show me all site audit issues on my project tagged Critical, grouped by category"
"Compare backlink growth for us vs. our top 3 competitors over the last 90 days"
```


**Honest take:** Everyone I know in growth or SEO has three or four Ahrefs tabs open at any time. Piping that data into Gemini CLI means the model can actually reason across it (comparing pages, prioritizing keyword clusters, drafting content briefs grounded in real metrics) instead of you doing the analysis in your head. Monetary values come back in USD cents, not dollars, so remember to divide by 100 when the model quotes numbers.


**Cons:** It's a paid Ahrefs plan and not a small one. Row limits mean big queries need pagination, and the model sometimes truncates before you notice.


Reference:[ahrefs.com/mcp](https://ahrefs.com/mcp) .


---


### 10. Hex MCP: analytical questions against your data warehouse


**[Hex MCP](https://hex.tech/) lets Gemini CLI ask analytical questions of your data warehouse through Hex's Threads agent, and search across your existing Hex projects.**


Hex is where a lot of analytics teams live: notebooks with SQL, Python, charts, and shared apps built on top of Snowflake, BigQuery, or Postgres. The MCP exposes the Threads agent (an AI that writes and runs the SQL for you against your connected warehouse) and project search, so Gemini CLI can pull a real answer from your data without you leaving the CLI.


- ` create_thread` : kick off a new Threads conversation and get an analytical answer back
- ` continue_thread` : keep asking follow-up questions in the same thread
- ` get_thread` : fetch the messages and results of an existing thread
- ` search_projects` : find existing Hex projects by name or content, so you can point the model at prior analysis instead of redoing it


**Install:**


```text
{
"mcpServers"  :   {
"hex"  :   {
"httpUrl"  :   "https://mcp.hex.tech/mcp"
}
}
}
```


First use triggers OAuth in the browser through` /mcp auth hex` . Confirm the exact endpoint under your Hex workspace's MCP settings.


**Example prompts:**


```text
"Ask Hex: what was our week-over-week signup growth for the last 8 weeks, broken down by source?"
"Search Hex for projects related to onboarding conversion and pull the most recent one"
"Continue the Hex thread about churn cohorts and add a breakdown by plan tier"
```


**Honest take:** This is the read/analysis path, not a way to author Hex projects. If you want the CLI to build a dashboard, you still have to open Hex. But for the actual daily question ("give me the number, with the SQL, tied to real tables") it's the least-friction integration I've used across warehouse tools.


**Cons:** Only as useful as your Hex workspace's data model. If your semantic layer is messy or key metrics aren't defined, the Threads agent produces confidently wrong SQL. Ground the model in existing project results (via` search_projects` ) when accuracy matters.


Reference:[hex.tech](https://hex.tech/) .


---


### 11. Profound MCP: brand visibility across AI answers


**[Profound](https://tryprofound.com/) tracks how often your brand shows up (and how it's described) in ChatGPT, Claude, Perplexity, and Google's AI Overviews.**


Traditional SEO tools track your Google ranking. Profound tracks something newer and, for a lot of marketing teams, more important: whether an LLM recommends you when someone asks it "what's the best web scraping API?" or "how do I extract data from a site?" The MCP puts those answers, citation counts, and share-of-voice numbers directly in Gemini CLI, so you can turn a strategy question into a query without opening the dashboard.


- ` prompts_answersV2` : pull the actual AI answers your tracked prompts returned, with date, model, and topic filters
- ` reports_citations` : how often you (or a competitor) got cited across a date range
- ` reports_sentiment` : positive/negative sentiment breakdown across mentions
- ` reports_visibility` : share-of-voice against competitors on a set of prompts
- ` knowledgeBases_search` : search the content sources feeding your Profound workspace


**Install:**


```text
{
"mcpServers"  :   {
"profound"  :   {
"httpUrl"  :   "https://mcp.tryprofound.com/mcp"
}
}
}
```


First use triggers OAuth in the browser through` /mcp auth profound` . Grab the exact URL from your Profound workspace under Settings > Integrations.


**Example prompts:**


```text
"Pull Profound citations for Firecrawl over the last 30 days, broken down by model"
"Compare our share-of-voice vs. our top 3 competitors on 'web scraping API' prompts this quarter"
"Show me every ChatGPT answer where we got mentioned negatively in the last 14 days"
```


**Honest take:** If you own AI visibility (the answer to "are we in the model's recommendation set?") you basically have to track it, and Profound is the tool most marketing teams I know use for it. Wiring it into Gemini CLI turns a weekly review meeting into a five-minute chat.


**Cons:** New category, so the definitions (impressions, mentions, citations) don't map cleanly to any dashboard your team already knows. Expect a few weeks of calibrating what the numbers mean before you treat them as decision-grade.


Reference:[tryprofound.com](https://tryprofound.com/) .


---


## Building the top MCP servers for Gemini CLI into your workflow


My default Gemini CLI stack for growth work pairs five of these: **Firecrawl** for the live web (competitor sites, SERP research, launch coverage), **Ahrefs** for the SEO ground truth, **Profound** for AI-answer visibility, **Hex** for the warehouse numbers, and **Slack** for team context. That combination covers the whole "research → measure → tell the team" loop most marketing tasks live in.


The rest (Playwright, Supabase, Composio, Asana, Context7, Filesystem) rotate in based on the week. Context7 and Filesystem come out when I'm hands-on editing content in the repo. Asana and Composio show up during launch weeks where Slack, Asana, and Gmail all need updating in sync. Playwright and Supabase are for the engineering-adjacent days.


The two-transports distinction is worth internalizing. Local stdio servers (Playwright, Slack, Supabase, Filesystem) are best for anything touching your machine or holding a long-lived session. Remote HTTP servers (Firecrawl, Asana, Composio, Context7) are best for anything with a hosted API, because you skip the local install and get OAuth for free. Zuplo's survey found 59% of new MCP servers ship on streamable HTTP now, which matches what I see: the ecosystem is settling toward remote by default.


If MCP feels heavy for a simple task, remember the alternative:[MCP vs CLI for AI agents](https://www.firecrawl.dev/blog/mcp-vs-cli) covers when calling a CLI tool directly from the shell is a cleaner fit than adding an MCP server, and[why CLI agents beat direct API integrations](https://www.firecrawl.dev/blog/why-clis-are-better-for-agents) makes the broader case. For discovery,[mcp.so](https://mcp.so/) and[github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) are the two places I'd point anyone looking to go deeper.
