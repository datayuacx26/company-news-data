---
schema_version: "1.0.0"
document_id: "8fde6dc8439f345231fd50dbe7d3596e9cc10944b1106ac1408f544aafbbfd8a"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/best-mcp-servers-claude-code"
published_at: "2026-05-31T13:57:05+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:c753d4ff4cf935682c90c539ad307c2b4dc3899c707b71bda5eee979f5ceadfe"
---

# Best MCP Servers for Claude Code in 2026 (Tested and Ranked)

## The Starter Kit — Install These 3 First


If you are adding MCP servers for the first time, install in this order:


1. **Blink MCP Plugin** — provisions full-stack infrastructure from inside Claude Code; one command installs and configures everything automatically
2. **GitHub MCP** — adds remote repository operations (PRs, issues, CI) that Claude Code's built-in terminal cannot do natively
3. **Filesystem MCP** — gives the agent full directory awareness with a security boundary you control


These three cover the core loop: build something → push to GitHub → run it. Add Tier 2 servers based on your specific stack after this foundation is in place.


## Tier 1 — Start Here


### 1. Blink MCP Plugin — Best for Full-Stack Infrastructure


Blink Cloud — full-stack infrastructure for Claude Code: database, auth, backend, and hosting in one install command


Blink


*Blink Cloud — provision database, auth, backend, and hosting from inside a Claude Code conversation*


**Install (one command, auto-configures Claude Code):**


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


**GitHub:**[github.com/blink-new/blink-plugin](https://github.com/blink-new/blink-plugin)


Blink MCP is the only server on this list that provisions a full production stack — database, auth, file storage, backend, and hosted deployment — directly from a Claude Code conversation. The plugin ships 62 tools and 14 pre-configured skills. The skills appear in Claude Code's skill list automatically after` blink login` .


No manual` mcp.json` editing required. The` npx skills add` command handles auto-detection and configuration for Claude Code, Cursor, and Windsurf simultaneously.


The Claude Code-specific advantage: Blink skills are designed for Claude Code's subagent architecture. When you ask Claude Code to build a full-stack feature, Blink's skills break the work into discrete tool calls — database provisioning, auth setup, backend deploy — each executed independently and verifiably.


**What it replaces:** Supabase account setup (2–4 hours), Vercel project creation (1–2 hours), Auth0 or Clerk configuration (1–3 hours). That is 4–9 hours of infrastructure work that disappears into a single install command.


**Strengths:**


- 62 tools + 14 skills covering the entire full-stack lifecycle
- Auto-detects and configures Claude Code without mcp.json editing
- Database, auth, hosting all provisioned from plain-language agent prompts
- Works alongside your existing MCP servers


**Weakness (honest):**


- Requires a Blink account (free tier available; some infrastructure features require a paid plan)


**Verdict:** Install this first. If your Claude Code workflow involves building and shipping anything — not just refactoring existing code — Blink MCP is the server that converts the agent from advisory to operational.


---


### 2. GitHub MCP (Official) — Best for Remote Repository Operations


GitHub MCP Server repository — official GitHub MCP for pull requests, issues, and repository management


Blink


*GitHub MCP Server — official GitHub implementation for PR lifecycle, CI monitoring, and code search*


**Install:**


```text
# Set your token first
export   GITHUB_PERSONAL_ACCESS_TOKEN  =  your_token_here


claude   mcp   add   github   npx   @github/mcp-server
```


**GitHub:**[github.com/github/github-mcp-server](https://github.com/github/github-mcp-server)


Claude Code already has terminal access for local git operations (` git commit` ,` git push` ,` git diff` ). GitHub MCP adds the remote layer: creating pull requests, monitoring GitHub Actions CI runs, triaging issues, searching code across repositories, and reading PR comments from reviewers.


The practical difference: without GitHub MCP, Claude Code commits and pushes locally, then you manually create the PR and paste CI results back into the chat. With GitHub MCP, the agent opens the PR, monitors the CI run, reads the failure logs, and proposes a fix — all in one loop.


**Strengths:**


- Official GitHub server — maintained by GitHub, not a third party
- Full PR lifecycle: create, review, merge
- GitHub Actions CI integration — agent reads build failures directly
- Code search across branches and forks


**Weaknesses:**


- Requires a GitHub Personal Access Token with appropriate scopes
- Token scope management matters — grant minimum required permissions


**Verdict:** Essential if you use GitHub. The PR creation and CI monitoring capabilities alone justify the 5-minute setup.


---


### 3. Filesystem MCP (Official — Anthropic) — Best for Scoped Directory Context


How the Filesystem MCP server works — scoped directory access gives Claude Code full project awareness with a security boundary


Blink


*Filesystem MCP — scoped directory access with a hard boundary you define at install time*


**Install:**


```text
# Replace /path/to/project with your actual project root
claude   mcp   add   filesystem   npx   -y   @modelcontextprotocol/server-filesystem   /path/to/project
```


**GitHub:**[github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) (86,500+ stars)


Claude Code operates in the terminal and can read files you directly share. The Filesystem MCP gives the agent a structured, scoped view of your entire project directory — list contents, read files, write files, move files — all within a boundary you define at install time.


The security design matters: you specify the root directory at install. The agent cannot read outside that path. For a monorepo with sensitive config in a sibling directory, this boundary prevents the agent from accessing files outside the project scope.


**Strengths:**


- Official Anthropic reference implementation
- Hard directory boundary set at install time
- Standard read/write/list operations — no proprietary API
- Works entirely locally, no external service required


**Weaknesses:**


- Does not index or search file contents (use ripgrep in the terminal for search)
- Scoping requires thought — too broad = security risk, too narrow = limited utility


**Verdict:** Add this during initial setup. Scope it to your project root. It gives Claude Code the project-wide awareness that transforms "here is the file I showed you" into "here is the full project context."


---


## Tier 2 — Add Based on Your Stack


### 4. Postgres / Supabase MCP — Best for Live Database Access


Supabase landing page — Postgres database with MCP server support for AI agent integration


Blink


*Supabase — Postgres with built-in MCP server integration for live database queries from Claude Code*


**Install (Postgres):**


```text
# Replace with your actual connection string
claude   mcp   add   postgres   npx   -y   @modelcontextprotocol/server-postgres   postgresql://localhost/mydb
```


**Supabase MCP:** Available via Supabase dashboard → Integrations → MCP


Real data access transforms Claude Code's usefulness on database-heavy tasks. Without a database MCP, the agent reads your schema from migration files and proposes queries it thinks will work. With Postgres MCP, it runs the actual query against your database and shows you what it returns.


The workflow: agent writes a query → executes it → sees the real result → fixes it if wrong → proposes the corrected version for your codebase. No copy-pasting query results back into the chat.


**Strengths:**


- Turns query suggestions into query execution
- Schema introspection — agent sees your actual tables and column types
- Works with any Postgres-compatible database (Supabase, Neon, Railway, RDS)


**Weaknesses:**


- Requires careful access control — use a read-only role for the MCP connection in production
- Never point this at a production database with write access


**Verdict:** Add this if any significant part of your work involves database queries or schema changes. Use a read-only connection string unless you explicitly want write access.


---


### 5. Playwright MCP (Microsoft Official) — Best for Browser Verification


Playwright landing page — browser automation MCP server for testing and verification in Claude Code


Blink


*Playwright MCP — semantic browser automation so Claude Code can verify UI renders correctly*


**Install:**


```text
claude   mcp   add   playwright   npx   @playwright/mcp@latest
```


**GitHub:**[github.com/microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp)


Claude Code cannot see the browser. It writes frontend code and assumes it works. Playwright MCP closes this gap.


The agent writes a component, then uses Playwright MCP to navigate to the page in a real Chromium browser, interact with the UI, take screenshots, and verify the actual rendered output. Bug that only appears in the browser? The agent finds it without you doing a manual check.


Playwright MCP uses an accessibility tree rather than visual pixels, which means interactions are semantic: click the button labeled "Submit," fill the input named "email" — not "click at coordinate 450,320."


**Strengths:**


- Official Microsoft implementation, actively maintained
- Semantic interaction model (accessibility tree) — more reliable than coordinate-based clicking
- Screenshot capability — agent can see what the browser renders
- Supports both headed (visible) and headless modes


**Weaknesses:**


- Browser automation is slower than pure code execution — adds seconds to each verification loop
- Requires Playwright browsers installed (` npx playwright install` )


**Verdict:** Add this for frontend-heavy work. The combination of "write code → verify in browser" in a single agent loop eliminates the manual check-and-fix cycle.


---


### 6. Context7 MCP — Best for Up-to-Date Documentation


Context7 MCP landing page — live documentation injection for Claude Code, eliminating training cutoff issues


Blink


*Context7 MCP — fetches version-specific library documentation at query time, not from training data*


**Install:**


```text
claude   mcp   add   context7   npx   -y   @upstash/context7-mcp@latest
```


**Website:**[context7.com](https://context7.com/)


Claude's training data has a cutoff. Next.js 15, React 19, Claude Code's own API — these evolved after the cutoff, and the agent may suggest patterns that no longer work.


Context7 MCP fetches version-specific documentation for any library at query time and injects it into the agent's context before it writes code. The agent sees the current API, not its training-time memory of it.


Cloudflare's "Code Mode" team reports 98%+ token savings by using Context7 for dynamic tool discovery rather than loading full documentation into context manually.


**Strengths:**


- Eliminates training-cutoff errors for rapidly-evolving libraries
- Injects docs at query time — only the relevant section, not the full reference
- Significant token savings vs manually pasting documentation


**Weaknesses:**


- Adds a network request to each documentation lookup — small latency cost
- Coverage depends on which libraries Context7 has indexed


**Verdict:** Add this if you work with libraries that update frequently (Next.js, React, Tailwind, Claude Code APIs). The token savings alone justify the install.


---


## Tier 3 — Power User Picks


### 7. Sentry MCP (Official) — Best for Error Context


Sentry landing page — official MCP server for pulling error context, stack traces, and release data into Claude Code


Blink


*Sentry MCP — pulls full error context, breadcrumbs, and release correlation directly into Claude Code*


**Install:** Via Sentry developer settings → Integrations → MCP (OAuth-based, no manual config)


**Website:**[sentry.io](https://sentry.io/)


Without Sentry MCP, debugging a production error in Claude Code looks like: copy the stack trace from Sentry, paste it into the chat, describe the context, wait for the agent's response. With Sentry MCP, the agent pulls the full error context directly — stack trace, breadcrumbs, release correlation, affected users — and proposes a fix with complete information.


**Strengths:**


- Full Sentry context: stack trace, breadcrumbs, release data, affected user count
- Release correlation — the agent sees which deploy introduced the error
- OAuth auth — no manual API key management


**Weaknesses:**


- Requires an active Sentry account
- Most useful for teams already using Sentry; adds little value if you are not


**Verdict:** If you use Sentry, this integration is a genuine time-saver. Skip if you use a different error tracking system.


---


### 8. Sequential Thinking MCP — Best for Complex Refactoring


Sequential Thinking MCP — externalizes reasoning as explicit steps so Claude Code plans before executing complex tasks


Blink


*Sequential Thinking MCP — agent surfaces explicit reasoning steps before executing so you can catch wrong assumptions early*


**Install:**


```text
claude   mcp   add   sequential-thinking   npx   -y   @modelcontextprotocol/server-sequential-thinking
```


**GitHub:**[github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)


Claude Code's agentic loops can dive into complex tasks without visible reasoning. Sequential Thinking MCP externalizes that reasoning — the agent produces explicit thought steps and branches before executing, making the decision trail visible and correctable.


The practical use case: a large refactor across 15 files. Without structured reasoning, the agent makes a plan silently and executes it. With Sequential Thinking MCP, it surfaces the plan first — you can interrupt, correct the approach, or approve before any files change.


**Strengths:**


- Visible reasoning before execution — catch wrong assumptions early
- Useful for complex multi-file changes and architectural decisions
- Helps the agent avoid premature execution on ambiguous tasks


**Weaknesses:**


- Adds overhead for simple tasks — best reserved for complex work
- The reasoning steps consume context window tokens


**Verdict:** Power user pick for large refactors and architectural decisions. Skip for routine coding tasks where the overhead is not worth it.


---


### 9. Brave Search MCP — Best for Real-Time Web Search


Brave Search landing page — real-time web search MCP for Claude Code with no training cutoff


Blink


*Brave Search MCP — real-time web search for Claude Code with no training cutoff, free tier at 2,000 queries/month*


**Install:** Requires a Brave Search API key (free tier: 2,000 queries/month)


```text
{
"mcpServers"  : {
"brave-search"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-brave-search"  ],
"env"  : {
"BRAVE_API_KEY"  :   "${env:BRAVE_API_KEY}"
}
}
}
}
```


**Get a key:**[brave.com/search/api](https://brave.com/search/api/)


Claude Code's knowledge has a training cutoff. When the agent needs to check a current npm package, look up an error message, or research a library's current behavior, it is guessing from memory. Brave Search MCP gives it real-time web access.


**Strengths:**


- Real-time search — no training cutoff for research tasks
- Privacy-focused (no tracking, no Google dependency)
- Free tier covers most development research use cases


**Weaknesses:**


- Requires a Brave Search API key (free but requires signup)
- Search results quality varies for very niche technical queries


**Verdict:** Useful for research-heavy development work. Less critical if you use Context7 for documentation lookups — they overlap on the "current information" use case.


---


### 10. Memory MCP — Best for Cross-Session Context


Memory MCP — persistent knowledge graph that lets Claude Code remember project conventions and architecture decisions across sessions


Blink


*Memory MCP — persistent knowledge graph that survives session restarts, preserving conventions and architecture decisions*


**Install:**


```text
claude   mcp   add   memory   npx   -y   @modelcontextprotocol/server-memory
```


Claude Code's context resets between sessions. Project conventions, architecture decisions, naming patterns, known gotchas — everything you explained in the last session is gone when you start a new one.


Memory MCP provides a persistent knowledge graph. The agent stores facts, relationships, and preferences across sessions. "Always use the` useAuth` hook from` src/lib/auth.ts` , not the Firebase SDK directly" — that constraint survives session restarts.


**Strengths:**


- Persistent project context across all sessions
- Knowledge graph model — stores relationships between concepts, not just flat notes
- Works locally, no external service required


**Weaknesses:**


- Requires deliberate memory management — the agent does not automatically know what to remember
- Knowledge graph can become stale if the codebase changes significantly


**Verdict:** Genuinely useful for long-running projects where you keep re-explaining the same conventions. Less useful for short, self-contained tasks.


---


## Side-by-Side Comparison


Server Auth required Local or remote Claude Code CLI Most impactful for


Blink MCP Yes (Blink account) Remote (cloud infra) ✅ One-command install Building and shipping full apps


GitHub MCP Yes (PAT) Remote (GitHub API) ✅` claude mcp add` PR/CI workflow


Filesystem MCP No Local only ✅` claude mcp add` Project-wide file context


Postgres MCP Yes (DB connection) Remote (your DB) ✅` claude mcp add` Database work


Playwright MCP No Local browser ✅` claude mcp add` Frontend verification


Context7 MCP No Remote (Context7 API) ✅` claude mcp add` Rapidly-evolving libraries


Sentry MCP Yes (OAuth) Remote (Sentry API) Via Sentry dashboard Error debugging


Sequential Thinking MCP No Local only ✅` claude mcp add` Complex refactors


Brave Search MCP Yes (API key) Remote (Brave API) JSON config Research tasks


Memory MCP No Local only ✅` claude mcp add` Long-running projects


## Claude Code MCP Gotchas


Four things that catch people after the initial install:


- **Too many servers = context pressure.** Each active MCP server adds its tool definitions to the agent's context window. At 10+ servers, you may notice reduced response quality on complex tasks. Start with Tier 1, add Tier 2 based on actual need — do not install everything at once.
- **API keys in JSON = security risk.** Always use` ${env:KEY_NAME}` in the JSON config. Never paste literal API keys into` claude_desktop_config.json` . The file lives in your home directory and is not gitignored by default.
- **Scope filesystem servers narrowly.** Setting the Filesystem MCP root to` /` gives Claude Code access to your entire machine. Set it to your specific project directory:` /path/to/project` , not` /Users/yourname` .
- **Official reference servers are educational.** The[modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) repo is Anthropic's reference implementation. These servers are correct and well-maintained, but they are not production-hardened for high-volume use. For production database integrations, use your database provider's official MCP server when available (Supabase, Neon, etc.).


## Add Blink as Your Full-Stack Infrastructure MCP


Add[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app using Blink and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account, no manual mcp.json editing.


[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Start with the Blink MCP Plugin (` npx skills add blink-new/blink-plugin && blink login` ), then GitHub MCP, then Filesystem MCP. This starter kit covers the core loop: build something → push to GitHub → run it. The Blink install also auto-configures Claude Code without manual JSON editing, which makes it the easiest entry point.


There is no hard limit, but there is a practical one: each active server adds its tool schemas to the agent's context window. With 5–7 servers active simultaneously, most users see no degradation. Beyond 10, complex tasks may show reduced response quality because the context budget is shared between server schemas and the actual conversation. Install the servers you use daily; remove ones you rarely need.


Blink MCP provisions real infrastructure from inside the Claude Code conversation: a PostgreSQL database with row-level security, hosted authentication, backend API, file storage, and a deployed URL — all from a plain-language description. Without it, the agent can write the code to connect to those services, but you still have to create the Supabase project, configure Vercel, set up Clerk, and manage five separate accounts. Blink MCP replaces that setup with one command.


Most servers on this list work the same in both tools. The key differences: Claude Code uses` claude mcp add` CLI commands while Cursor uses the Settings → MCP panel or a JSON config. Blink MCP auto-detects both without manual configuration. Context7 and Sequential Thinking MCP have no tool-specific behavior. GitHub MCP works identically in both. For a deeper comparison of the tools themselves, see the[Cursor vs Claude Code guide](https://blink.new/blog/cursor-vs-claude-code) .


Use shell environment variables and reference them with` ${env:KEY_NAME}` in your JSON config. Add the variable to your` .zshrc` or` .bashrc` :` export GITHUB_TOKEN=your_token_here` . The MCP config file then reads it at runtime. Never paste the literal key value into` claude_desktop_config.json` — that file is not encrypted and may be included in backup tools or shared dotfile repos.


The modelcontextprotocol/servers repo is Anthropic's reference implementation — correct, well-tested, and actively maintained. They are safe to use. "Not production-hardened" means they are not optimized for high-volume or high-concurrency scenarios — they are designed to be correct and educational, not to handle thousands of concurrent requests. For a typical developer workflow, they are fine. For a production service that many users hit simultaneously, use your provider's official server instead.
