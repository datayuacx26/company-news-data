---
schema_version: "1.0.0"
document_id: "4af77d06a0efba3a30c7dc45356e5ad53a40af689dfa2edb152af65733429cf4"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/best-mcp-servers-for-developers-2026/"
published_at: "2026-08-16T11:27:12+00:00"
first_seen_at: "2026-08-17T00:30:25.930029+00:00"
fetched_at: "2026-08-17T00:30:27.894519+00:00"
content_hash: "sha256:02b2ad7d8dea82843b70b2596a1b0f715dc3d718e087a95cb8bd371da225f905"
---

# Best MCP Servers for Developers [2026]

Most developers need six MCP servers, not fifty: a filesystem server, Git, GitHub, one database server in read-only mode, Playwright for browsers, and something that supplies current documentation. Everything after that is situational. This guide sorts MCP servers by the job they do rather than by vendor logo, and it says who maintains each one.


That last part matters more than it sounds. Many servers still recommended by name in "top MCP servers" lists were **archived in 2025** and now sit in a repository whose README states, in capitals, that no security guarantees are provided for them. If a list still tells you to install the reference PostgreSQL or GitHub server, it was written from memory rather than from the repository.


## What Is an MCP Server?


An MCP server is a small program that exposes tools and data to an AI agent through the Model Context Protocol. Your agent calls those tools instead of you copying output between windows.


It solves a boring but expensive problem. Before MCP, every agent needed a bespoke integration for every tool. Now one server works with every client that speaks the protocol, including Claude Code, Codex, and Cursor.


The[specification](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports?ref=scrimba.com) defines exactly two standard transports: **stdio** for servers that run as a local process, and **Streamable HTTP** for servers you connect to at a URL. The older HTTP plus SSE transport is[formally deprecated](https://modelcontextprotocol.io/specification/2026-07-28/deprecated?ref=scrimba.com) , so a setup guide telling you to add a server with` --transport sse` is out of date.


The split you care about is *local* versus *remote* . A local server runs on your machine, sees your files, and needs installing. A remote server is a hosted URL behind OAuth, and needs nothing installed. Remote has become the default for anything a company runs itself.


If you want to understand the protocol rather than pick servers off a shelf, our roundup of[MCP tutorials and courses](https://scrimba.com/articles/best-mcp-tutorials-and-courses/) is the companion piece. This article is about what to run.


## Official, Vendor-Built, or Community: How to Read Any MCP Server List


Every MCP server falls into one of three maintenance tiers, and the tier predicts reliability better than the star count.


Tier Who maintains it What that gets you Example


Reference server The MCP project itself Small, simple, tracks the spec, no product behind it Filesystem, Git, Memory


Vendor-built The company whose product it wraps Ships with the product, first to new features, usually OAuth GitHub, Linear, Supabase


Community Someone who does not own the product Often the only option, quality varies, read the source Postgres MCP Pro


The[reference servers](https://github.com/modelcontextprotocol/servers?ref=scrimba.com) are what people mean by "official," and there are currently seven: Everything, Fetch, Filesystem, Git, Memory, Sequential Thinking, and Time. Thirteen others were retired to a[separate archive repository](https://github.com/modelcontextprotocol/servers-archived?ref=scrimba.com) , including the GitHub, GitLab, PostgreSQL, SQLite, Slack, Sentry, Google Drive, Puppeteer, and Brave Search servers.


Only two of those entries carry a redirect to a successor, and in Slack's case the README is already behind reality, because Slack has since shipped a server of its own. Treat the archive as a list of things to replace, not as a directory.


There is also an[official registry](https://modelcontextprotocol.io/registry/about?ref=scrimba.com) , still labelled preview rather than generally available. It gives you provenance, since reverse-DNS namespaces are tied to a verified GitHub account or domain. It does not vet code. The registry's own documentation says it *delegates security scanning* to the underlying package registries.


Before installing anything, run four checks:


1. **Who maintains it.** Vendor-built beats community for anything touching production.
2. **When it last shipped.** The protocol changed materially in 2026, and a server untouched for a year may not speak the current transport.
3. **What access it asks for.** A token scoped to every repository you can see is very different from one scoped to a single repository.
4. **Whether it has a read-only mode.** If it does, start there.


## The Best MCP Servers by Job


### Files


**Filesystem** is the reference server for reading, writing, moving, and searching files inside directories you explicitly allow, published as` @modelcontextprotocol/server-filesystem` . Most agents already have file access built in, so the real use case is giving one scoped access to a directory *outside* your project, such as a notes folder or a design export.


### Git, GitHub, and GitLab


**Git** is the reference server for local repository work: status, diffs, log, branches, commits, published to PyPI as` mcp-server-git` . It works on any repository on disk, with no account involved.


The[GitHub MCP server](https://github.com/github/github-mcp-server?ref=scrimba.com) is GitHub's own, and it is the most useful single server on this list. It covers issues, pull requests, code search, Actions, and releases, as a local install or a hosted endpoint. Two features deserve deliberate use: read-only mode, which disables every write tool regardless of configuration, and toolsets, which expose only the parts you use. GitLab publishes its own server per instance.


### Databases


The reference PostgreSQL and SQLite servers are both archived. For Postgres, the widely used replacement is[Postgres MCP Pro](https://github.com/crystaldba/postgres-mcp?ref=scrimba.com) , a third-party server with a restricted access mode that limits the agent to read-only transactions, plus index tuning and health checks that go beyond running queries. SQLite has no vendor successor at all; multi-database servers such as[DBHub](https://github.com/bytebase/dbhub?ref=scrimba.com) are the practical route.


If your database is hosted, prefer the vendor's server.[Supabase](https://supabase.com/blog/defense-in-depth-mcp?ref=scrimba.com) ships one with read-only and project-scoped modes, and its own security guidance is blunt about the boundary: never connect AI agents directly to production data.


### Browser automation and testing


[Playwright MCP](https://github.com/microsoft/playwright-mcp?ref=scrimba.com) is Microsoft's, and it is the strongest server in this category. Rather than feeding the model screenshots, it drives pages through Playwright's accessibility tree, so the agent works from structured element roles and text instead of pixels. That makes it deterministic in a way vision-based automation is not, though the package is still pre-1.0.


[Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp?ref=scrimba.com) is the Chrome team's, and it answers a different question. It records performance traces, lists network requests, and reads console messages from a live browser, which turns "the page feels slow" into something an agent can investigate.


### Search, fetch, and live documentation


**Fetch** is the reference server that retrieves a web page and converts it to markdown, published as` mcp-server-fetch` , and it respects robots.txt by default. For search proper, Brave, Exa, Tavily, and Perplexity all maintain their own servers, most of which want an API key.


[Context7](https://github.com/upstash/context7?ref=scrimba.com) from Upstash solves the problem where your agent confidently writes code against a library version that shipped two years ago. It pulls version-specific documentation on demand, locally or hosted.


### Team tools


The pattern here is uniform: the vendor hosts it, OAuth handles the login, nothing is installed. **Linear** runs one at` https://mcp.linear.app/mcp` covering issues, projects, and comments, with a separate read-only URL if you want the agent to look without touching. **Notion** , **Sentry** , **Atlassian** , and **Stripe** publish hosted endpoints too, and Sentry's is the example Anthropic uses for connecting an OAuth server. Slack now runs an official server as well, after a long stretch where community forks were the only option.


There is no good reason to run a local process to read your own issue tracker.


### Cloud and deployment


[Cloudflare](https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/?ref=scrimba.com) runs a catalog of managed remote servers split by product area, all OAuth-secured. AWS, Azure, Vercel, and Netlify each followed some version of the same pattern. The rule of thumb: if the vendor publishes a hosted MCP endpoint, use it rather than a community wrapper around their REST API.


### Memory and context


**Memory** is the reference server that maintains a local knowledge graph across conversations, and **Sequential Thinking** exposes one tool that lets a model work through a problem in revisable steps. Both are small, both are useful, and both get installed and then quietly forgotten.


### Explanation and codebase walkthroughs


This category answers "explain this to me" rather than "do this for me," and it is newer than the rest.


[DeepWiki](https://docs.devin.ai/work-with-devin/deepwiki-mcp?ref=scrimba.com) , from the team behind Devin, is a free remote server with no authentication that lets an agent read generated documentation for a public GitHub repository and ask questions about it. The limit is public repositories only.


**Scrimba Explain** works against your own code instead. It is an MCP plugin at` https://scrimba.com/explain/mcp` that runs with Claude Code, Codex and ChatGPT, and any MCP-capable agent. You ask a question about your codebase, your agent researches it using the context it already has, and what comes back is a narrated video walkthrough rather than a wall of text. It is free during open beta, and like any AI tool it can be wrong, so check anything important.


Job Server Maintainer Local or remote


Files Filesystem Reference Local


Repositories GitHub MCP server Vendor Both


Local Git Git Reference Local


Postgres Postgres MCP Pro Community Local


Browsers Playwright MCP Vendor Local


Web performance Chrome DevTools MCP Vendor Local


Library docs Context7 Vendor Both


Issue tracking Linear Vendor Remote


Public repo Q&A DeepWiki Vendor Remote


Codebase walkthroughs Scrimba Explain Vendor Remote


## What MCP Servers Actually Cost You in Security


An MCP server runs with whatever access you grant it, and the model decides when to use it. Those two facts together are the entire security story.


The mechanism worth understanding is *prompt injection through tool results* . Your agent reads whatever a tool returns, and it cannot reliably tell data from instructions. A bug report, a web page, or a support ticket can contain text written to look like a command. Simon Willison calls the dangerous combination the[lethal trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/?ref=scrimba.com) : access to private data, exposure to untrusted content, and the ability to communicate externally. Connect enough servers and you assemble all three without deciding to.


This is not theoretical.[Invariant Labs demonstrated](https://invariantlabs.ai/blog/mcp-github-vulnerability?ref=scrimba.com) an attack where a malicious issue filed on a public repository coerced an agent into reading private repositories and publishing the contents in a public pull request. Their conclusion is the uncomfortable part: it was not a bug in the server's code.


The specification is candid about where the guarantees stop:


> MCP itself cannot enforce these security principles at the protocol level.


Anthropic says something similar, noting that it reviews connectors against listing criteria but[does not security-audit or manage any MCP server](https://code.claude.com/docs/en/security?ref=scrimba.com) . What you get instead are controls that shrink the blast radius:


1. **Use read-only mode wherever a server offers one.** GitHub, Linear, Supabase, and Postgres MCP Pro all have one.
2. **Scope tokens narrowly.** One repository, one project, one database, not everything the token can reach.
3. **Keep production data out of it.** Point servers at staging.
4. **Let your client ask.** Permission prompts exist for this, and switching them off is the fastest way to lose the protection.
5. **Prefer servers you can read.** For community servers, the source is the audit.


None of this is a reason to avoid MCP. It is a reason to run six servers you understand rather than thirty you collected.


## How Do You Add an MCP Server?


Adding an MCP server takes one command in most clients. In Claude Code, a remote server is` claude mcp add --transport http <name> <url>` , and a local one is` claude mcp add <name> -- npx -y <package>` .


Servers install at three scopes: *local* to you, *project* (written to` .mcp.json` and shared through git), or *user* across all your projects. Run` /mcp` to check status and handle any OAuth login. The[official documentation](https://code.claude.com/docs/en/mcp?ref=scrimba.com) has the details, and our guide to[using Claude Code](https://scrimba.com/articles/how-to-use-claude-code/) covers where MCP fits in the workflow. Other clients differ in syntax, not concept, and[Claude Code, Codex, and Cursor](https://scrimba.com/articles/claude-code-vs-codex-vs-cursor/) all run the same servers.


## How Many MCP Servers Should You Actually Run?


Fewer than you think. Every connected server loads its tool definitions into the model's context before you type anything, so each one costs tokens and makes tool selection harder.


Anthropic's engineering team[put numbers on the problem](https://www.anthropic.com/engineering/code-execution-with-mcp?ref=scrimba.com) : rewriting one multi-tool workflow so results stopped passing through the model dropped it from roughly 150,000 tokens to 2,000. Connected surface area is not free.


Sensible starting points:


- **Learning to code:** none. Get comfortable directing an agent first, and our guide to[agentic coding](https://scrimba.com/articles/what-is-agentic-coding/) is a better next step than any server.
- **Building side projects:** GitHub in read-only mode, plus Context7.
- **Working on a team:** add your issue tracker, a scoped database server, and Playwright if you own tests.


Add the fourth when you have felt the absence of it, not before.


## Frequently Asked Questions


### What are MCP servers used for?


MCP servers give an AI agent controlled access to real systems: your files, repositories, databases, browser, issue tracker, and documentation. Instead of pasting output into a chat window, the agent calls a tool directly, reads the result, and continues working.


### Are MCP servers free?


Most are open source and free to run. The reference servers, Playwright, Chrome DevTools, and Context7 all cost nothing. What you may pay for is the underlying service, since a hosted server still requires an account, and several search servers need a paid API key.


### Which MCP servers work with Claude Code?


All of them. MCP is a shared protocol, so any server works with any compliant client, including Claude Code, Codex, Cursor, and Claude Desktop. The differences are in setup syntax and how each client handles permissions, not in which servers are compatible.


### Are MCP servers safe to use?


They are as safe as the access you grant them. The specification states plainly that it cannot enforce security at the protocol level, so the real controls are read-only modes, narrowly scoped tokens, keeping production data out, and leaving your client's permission prompts switched on.


### Which MCP server can explain a codebase to me?


DeepWiki answers questions about public GitHub repositories through generated documentation. For your own code, Scrimba Explain returns a narrated video walkthrough of whatever you asked about, using the context your agent already has. Both sit alongside Context7, which supplies current library documentation rather than explanations.


## Key Takeaways


- Six servers cover most developers: Filesystem, Git, GitHub, a read-only database server, Playwright, and Context7.
- Thirteen reference servers were archived in 2025, including GitHub, PostgreSQL, SQLite, and Slack, and the archive states that no security guarantees are provided for them.
- Seven remain active: Everything, Fetch, Filesystem, Git, Memory, Sequential Thinking, and Time.
- The official MCP registry is still in preview and gives you provenance, not a security audit.
- An MCP server runs with whatever access you grant it, and prompt injection through tool results is a real attack, not a hypothetical one.
- Read-only modes, narrow tokens, and staging data are the mitigations that exist today and cost nothing to use.
- Every connected server spends context before you type, so a small deliberate set beats a large collected one.


To build a server rather than install one, Scrimba's[Intro to Model Context Protocol](https://scrimba.com/intro-to-model-context-protocol-mcp-c0sake4uir?ref=scrimba.com) spends 37 minutes registering tools and resources on a Weather server in Node.js, and the[AI Engineer Path](https://scrimba.com/the-ai-engineer-path-c02v?ref=scrimba.com) places MCP alongside embeddings, agents, and context engineering. Both sit on the Pro plan at $24.50 per month billed annually, discounts are available, and Scrimba's free courses include completion certificates.


## Sources


Model Context Protocol, accessed August 2026:


- Reference servers:[https://github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers?ref=scrimba.com)
- Archived servers:[https://github.com/modelcontextprotocol/servers-archived](https://github.com/modelcontextprotocol/servers-archived?ref=scrimba.com)
- Transports:[https://modelcontextprotocol.io/specification/2026-07-28/basic/transports](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports?ref=scrimba.com)
- Deprecations:[https://modelcontextprotocol.io/specification/2026-07-28/deprecated](https://modelcontextprotocol.io/specification/2026-07-28/deprecated?ref=scrimba.com)
- Security best practices:[https://modelcontextprotocol.io/specification/2026-07-28/basic/security_best_practices](https://modelcontextprotocol.io/specification/2026-07-28/basic/security_best_practices?ref=scrimba.com)
- Registry:[https://modelcontextprotocol.io/registry/about](https://modelcontextprotocol.io/registry/about?ref=scrimba.com)


Vendors and research, accessed August 2026:


- Anthropic, Claude Code security:[https://code.claude.com/docs/en/security](https://code.claude.com/docs/en/security?ref=scrimba.com)
- Anthropic, MCP in Claude Code:[https://code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp?ref=scrimba.com)
- Anthropic, code execution with MCP:[https://www.anthropic.com/engineering/code-execution-with-mcp](https://www.anthropic.com/engineering/code-execution-with-mcp?ref=scrimba.com)
- GitHub MCP Server:[https://github.com/github/github-mcp-server](https://github.com/github/github-mcp-server?ref=scrimba.com)
- Playwright MCP:[https://github.com/microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp?ref=scrimba.com)
- Chrome DevTools MCP:[https://github.com/ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp?ref=scrimba.com)
- Postgres MCP Pro:[https://github.com/crystaldba/postgres-mcp](https://github.com/crystaldba/postgres-mcp?ref=scrimba.com)
- DBHub:[https://github.com/bytebase/dbhub](https://github.com/bytebase/dbhub?ref=scrimba.com)
- Supabase, defense in depth for MCP:[https://supabase.com/blog/defense-in-depth-mcp](https://supabase.com/blog/defense-in-depth-mcp?ref=scrimba.com)
- Context7:[https://github.com/upstash/context7](https://github.com/upstash/context7?ref=scrimba.com)
- Cloudflare MCP servers:[https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/](https://developers.cloudflare.com/agents/model-context-protocol/cloudflare/servers-for-cloudflare/?ref=scrimba.com)
- Linear MCP server:[https://linear.app/docs/mcp](https://linear.app/docs/mcp?ref=scrimba.com)
- Slack MCP server:[https://docs.slack.dev/ai/slack-mcp-server/](https://docs.slack.dev/ai/slack-mcp-server/?ref=scrimba.com)
- DeepWiki MCP:[https://docs.devin.ai/work-with-devin/deepwiki-mcp](https://docs.devin.ai/work-with-devin/deepwiki-mcp?ref=scrimba.com)
- Invariant Labs, GitHub MCP exploited:[https://invariantlabs.ai/blog/mcp-github-vulnerability](https://invariantlabs.ai/blog/mcp-github-vulnerability?ref=scrimba.com)
- Simon Willison, the lethal trifecta:[https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/?ref=scrimba.com)
