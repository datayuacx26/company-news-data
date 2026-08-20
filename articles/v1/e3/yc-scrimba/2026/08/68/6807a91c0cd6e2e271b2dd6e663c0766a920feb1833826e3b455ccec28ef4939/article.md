---
schema_version: "1.0.0"
document_id: "6807a91c0cd6e2e271b2dd6e663c0766a920feb1833826e3b455ccec28ef4939"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/best-claude-code-plugins-2026/"
published_at: "2026-08-17T08:46:37+00:00"
first_seen_at: "2026-08-17T13:50:30.429549+00:00"
fetched_at: "2026-08-17T13:50:31.611741+00:00"
content_hash: "sha256:92e2ba430d1290f0a30a1586bfe0edc2a63c04049747459a6675cfc386326f45"
---

# Best Claude Code Plugins [2026]

Most lists of Claude Code plugins are really lists of MCP servers. The two overlap, but they are not the same object, and that confusion is why installs fail in ways the error message does not explain.


A **Claude Code plugin** is a bundle of skills, agents, hooks, MCP servers and language servers that installs as a single unit through the` /plugin` command. Anthropic ships a curated official marketplace plus a much larger community one. The plugins worth your time cluster into six jobs: code intelligence, source control, browser control, database access, documentation lookup, and explanation.


What follows: what a plugin actually is, how installation and scoping work, which ones earn their place, and what they cost in context and trust. It assumes Claude Code is already running. If not, start with the guide to[using Claude Code](https://scrimba.com/articles/how-to-use-claude-code/) .


## What Is a Claude Code Plugin?


A Claude Code plugin is a directory of components that Claude Code loads as one unit, packaged so it can be versioned, shared, and installed by name.


That directory holds considerably more than a set of tools. Anthropic's[plugin documentation](https://code.claude.com/docs/en/plugins?ref=scrimba.com) lists the pieces, each of which lives at the plugin root:


Component What it adds


` skills/` Instructions Claude reaches for on its own, based on the task


` agents/` Custom subagents with their own prompts, tools, and model


` hooks/` Event handlers that fire on tool use, edits, or session end


` .mcp.json` MCP server configurations, started automatically


` .lsp.json` Language server connections for code intelligence


` monitors/` Background watchers that push events into the session


` bin/` Executables added to the Bash tool's PATH while enabled


The manifest at` .claude-plugin/plugin.json` names the plugin and sets its namespace. That namespace shows up in daily use: plugin skills are always invoked as` /plugin-name:skill-name` , so two plugins can both ship a` commit` skill without colliding.


The alternative is standalone configuration in your project's` .claude/` directory. Anthropic's guidance is to *start* standalone while you experiment, then convert to a plugin once you want to share it, version it, or use it across repositories.


## Plugin vs MCP Server: What Is the Difference?


An MCP server is one component a plugin can contain. A plugin can bundle several servers, or none at all, and still be a plugin.


The difference shows up when you try to remove something. A plugin-bundled server is added and removed by installing or uninstalling the plugin, not with` /mcp` commands, though you can still toggle it off in the` /mcp` panel ([Anthropic](https://code.claude.com/docs/en/mcp?ref=scrimba.com) ). A bare server you added with` claude mcp add --transport http <name> <url>` is managed entirely through` /mcp` and knows nothing about plugins.


Plugin Standalone MCP server


**Install with**` /plugin install name@marketplace`` claude mcp add`


**Can contain** Skills, agents, hooks, MCP and LSP servers, monitors Tools, prompts, and resources from one service


**Remove with**` /plugin uninstall`` claude mcp remove`


**Shared via** A marketplace, or project settings` .mcp.json` at project scope


**Best for** A whole workflow, packaged One integration on its own terms


If the protocol itself is new to you, Scrimba's roundup of[MCP tutorials and courses](https://scrimba.com/articles/best-mcp-tutorials-and-courses/) is a better starting point than any plugin list.


## How Do You Install and Manage Plugins?


Run` /plugin` to open the plugin manager, browse a marketplace, pick a plugin, and choose an installation scope. Claude Code handles the rest.


1. **Open the manager.**` /plugin` opens four tabs: *Discover* , *Installed* , *Marketplaces* , and *Errors* .
2. **Use the official marketplace.** Claude Code registers` claude-plugins-official` the first time you start it interactively, so it is usually already there. Install by name:` /plugin install github@claude-plugins-official` .
3. **Add the community marketplace for the long tail.** Run` /plugin marketplace add anthropics/claude-plugins-community` , then install with the` @claude-community` suffix.
4. **Pick a scope.** *User* installs it for you everywhere, *project* writes it into` .claude/settings.json` so collaborators get it, and *local* keeps it to you in this repository only.
5. **Activate it.** If the install summary tells you to run` /reload-plugins` , run it.


Read the details pane before you press enter. It shows a **context cost** estimate in tokens per turn, the last updated date, and a "Will install" inventory of every command, agent, skill, hook, and server it brings ([Anthropic](https://code.claude.com/docs/en/discover-plugins?ref=scrimba.com) ).


Housekeeping takes three commands:` /plugin list` shows what you have,` /plugin disable` switches one off,` /plugin uninstall` removes it. Claude Code also groups anything unused for two weeks under a *Not used recently* header, a useful nudge for plugins still charging you context every turn.


## The Best Claude Code Plugins in 2026


The official marketplace carries more than 250 plugins and the community catalog more than 2,000, both counted from Anthropic's public catalogs in August 2026. Grouped by job, these are the ones that change how a session goes.


### Code intelligence, the boring pick that wins


The LSP plugins are the least exciting entry here and the one most likely to improve your output. Anthropic ships one per major language:` typescript-lsp` ,` pyright-lsp` ,` rust-analyzer-lsp` ,` gopls-lsp` ,` clangd-lsp` , plus Java, Kotlin, PHP, Lua, Swift, and C#.


Two things change once one is running. Claude gets **automatic diagnostics** after every edit, so a type error or a missing import surfaces immediately instead of after a build. And it gets real navigation: jump to definition, find references, trace a call hierarchy, rather than guessing with grep.


There are two catches. You install the language server binary yourself, and heavyweights like` rust-analyzer` and` pyright` can eat memory on a large repository.


### Source control and review


` github` wraps the official GitHub MCP server: issues, pull requests, code search, and the rest of the API from inside a session.` commit-commands` adds git skills for commit, push, and PR creation. For review,` pr-review-toolkit` runs agents that each look at one dimension, and` code-review` scores findings by confidence so it filters its own false positives. Together they remove the copy and paste between the issue tracker and the terminal, where the minutes actually go.


### Browser control and front-end feedback


An agent that cannot see the page it is building is guessing. Three plugins fix that:


- **` playwright`** , Microsoft's browser automation server, for clicking, filling forms, screenshotting, and end to end tests.
- **` chrome-devtools-mcp`** , which drives a live Chrome and hands back performance traces, network requests, and console errors with source-mapped stack traces.
- **` browser-use`** , which points Claude at your own Chrome or a hosted browser for scraping and web workflows.


This category has the clearest payoff for web developers. It turns "the layout looks right" into a screenshot Claude can check.


### Databases


` supabase` covers database operations, auth, and storage against your projects.` mongodb` ships an official server plus skills for schema and query work. The managed-cloud family reaches Cloud SQL, AlloyDB, BigQuery, ClickHouse, and CockroachDB.


Handle this group carefully. A read-write database connection is the largest blast radius on the list, and the sensible default is a read-only role at local scope.


### Documentation, search, and explanation


` context7` pulls version-specific documentation and code examples straight from source repositories, which is the fix for an agent confidently calling an API that changed two releases ago.` exa` adds web search, content extraction, and research skills.


**Scrimba Explain** belongs in this group. It is an MCP plugin: you ask a question about your codebase, the agent researches it, and Explain returns a narrated video walkthrough instead of another wall of text. It installs from Scrimba's own site rather than either Anthropic marketplace, and works with Codex and other MCP-capable agents too. It is free during open beta, and its FAQ is candid that it can make mistakes, so double-check anything important.


For a lighter touch,` explanatory-output-style` and` learning-output-style` change how Claude narrates its work, adding reasoning about implementation choices or asking you to write the interesting parts yourself.


### Monitoring and security


` sentry` brings error reports and stack traces into the session, so "fix the crash in production" becomes a real instruction.` security-guidance` reviews each change Claude makes for injection, XSS, SSRF, hardcoded secrets, and other common classes, then has Claude fix what it finds before you see it.


### Meta plugins


Four cover the ecosystem itself:` plugin-dev` for building plugins,` mcp-server-dev` for scaffolding servers,` claude-md-management` for project memory files, and` claude-code-setup` , which reads a codebase and recommends automations that fit it.


## Which Plugin for Which Job


Plugin What it adds Source Best for


` typescript-lsp` and siblings Diagnostics after every edit, code navigation Official Any typed codebase


` github` Issues, PRs, code search via the GitHub API Official Ticket to PR in one place


` playwright` Browser automation and end to end tests Official Front-end work needing verification


` chrome-devtools-mcp` Live Chrome, traces, console errors Official Performance and runtime bugs


` supabase` Database, auth, and storage operations Official Full-stack projects on Supabase


` context7` Version-specific docs from source repos Official Fast-moving libraries


Scrimba Explain A narrated video answer to a codebase query Scrimba's site Reading unfamiliar code


` security-guidance` Vulnerability review of Claude's own diffs Official Anything touching user input


` sentry` Production errors and stack traces in session Official Debugging what already shipped


## What Plugins Cost You: Context, Trust, and Prompt Injection


Every enabled plugin is a standing charge against your context window and your trust boundary. Both bills come due quietly.


**Context.** A plugin's components load into context every turn, which is why the details pane estimates the cost before you install. MCP tools are cheaper than they were: tool search defers tool definitions until Claude needs one, so only the tools actually used enter context. Tool output is still capped, with a warning above 10,000 tokens and a 25,000-token default ([Anthropic](https://code.claude.com/docs/en/mcp?ref=scrimba.com) ).


**Trust.** Anthropic's warning is worth reading twice:


> Plugins and marketplaces can execute arbitrary code on your machine with your user privileges. ([Claude Code documentation](https://code.claude.com/docs/en/discover-plugins?ref=scrimba.com) )


Community plugins pass automated validation and safety screening and are pinned to a specific commit, which is a real filter but not a code review. Anthropic reviews connectors against listing criteria before adding them to its directory, and states plainly that it does not security-audit MCP servers ([Anthropic](https://code.claude.com/docs/en/security?ref=scrimba.com) ).


**Prompt injection.** This is the failure mode most developers underrate. Every tool result is untrusted input: a GitHub issue body, a scraped page, a database row, a comment in a dependency. Any of them can carry text aimed at the agent rather than at you. Claude Code's safeguards help, including permission prompts, a separate context window for web fetch, and trust verification for new servers. They reduce the risk without removing it.


Five rules keep the bill small:


1. Install narrow. A plugin per job beats a suite you use one skill from.
2. Choose scope deliberately: local for anything carrying credentials, project for what your team should share.
3. Prefer read-only credentials, especially for databases and monitoring.
4. Read the "Will install" inventory before you press enter.
5. Uninstall what you stopped using.


> Usefulness is the easy part. The real test is whether a plugin is useful enough to justify standing access to your machine.


## What to Install First


Four plugins cover most of what a working web developer needs, and adding them one at a time makes it obvious what each one changed.


1. **The LSP plugin for your main language.** Highest ratio of benefit to effort, every time.
2. **` github` .** Removes the manual step between the issue tracker and the terminal.
3. **A browser plugin.**` playwright` for testing,` chrome-devtools-mcp` for debugging.
4. **One documentation or explanation plugin.**` context7` if your stack moves fast, an explainer if you read unfamiliar code more than you write new code.


Skip the rest until a specific frustration justifies it. Still deciding which agent to standardize on? The comparison of[Claude Code, Codex, and Cursor](https://scrimba.com/articles/claude-code-vs-codex-vs-cursor/) is the more useful read first.


## The Skill No Plugin Replaces


Plugins widen what an agent can reach. None of them make its output correct, and none of them tell you when a plausible diff is quietly wrong.


For developers who already write JavaScript and want to build on the protocol rather than install things that use it, Scrimba's[AI Engineer Path](https://scrimba.com/the-ai-engineer-path-c02v?ref=scrimba.com) runs 11.4 hours and includes a dedicated Model Context Protocol module alongside embeddings, agents, and context engineering. It sits on the Pro plan at $24.50 per month billed annually, or $294 a year, with location-based, student, and promotional discounts.


If the foundations are the gap,[Learn to Code with AI](https://scrimba.com/learn-to-code-with-ai-c02m?ref=scrimba.com) is a free 4.5-hour course with Guil Hernandez covering HTML, CSS, and JavaScript with AI assistance. It is *not* a Claude Code course and does not teach agentic tooling. Free courses include completion certificates. For tooling-specific material, the roundup of[Claude Code tutorials and courses](https://scrimba.com/articles/best-claude-code-tutorials-and-courses-in-2026/) fits better.


## Frequently Asked Questions


### What is the difference between a Claude Code plugin and an MCP server?


An MCP server is one component a plugin can bundle. A plugin can also contain skills, subagents, hooks, language servers, and background monitors, and it installs as one unit from a marketplace. A standalone MCP server is added with the claude mcp add command and managed on its own.


### Are Claude Code plugins free?


The plugins in Anthropic's official and community marketplaces are free to install. Some connect to paid services, so a plugin for a database, monitoring platform, or search API needs an account with that provider. Claude Code itself requires a paid Claude plan or Console credits.


### How do I see which plugins are installed?


Run the plugin list command, or open the plugin manager and go to the Installed tab. The tab groups plugins by scope, shows load errors first, and collects anything you have not used in two weeks under a separate header so you can prune it.


### Are Claude Code plugins safe to install?


Treat them like any dependency. Anthropic warns that plugins and marketplaces can execute arbitrary code with your user privileges, and it does not security-audit MCP servers. Install from sources you trust, review the components list first, and prefer read-only credentials where offered.


### Can I use Claude Code plugins with other AI coding tools?


The plugin format is specific to Claude Code, but the MCP servers inside plugins are not. Any agent that supports the Model Context Protocol can connect to the same servers, which is why tools like Scrimba Explain document a plain MCP endpoint alongside their Claude Code install.


## Key Takeaways


- A Claude Code plugin bundles skills, agents, hooks, MCP servers, and language servers into one installable unit. MCP servers are only one part of that.
- The official marketplace registers itself on first interactive launch. The community marketplace is opt-in and much larger.
- Scope matters: user for yourself everywhere, project for your team, local for credentials you do not want shared.
- LSP plugins give the biggest quality improvement for the least configuration, at the price of installing the language server binary yourself.
- Browser plugins matter most for web developers, because they give the agent a check it can actually run.
- Plugins execute arbitrary code with your user privileges, and Anthropic does not security-audit MCP servers.
- Every tool result is untrusted input. Prompt injection through an issue body or a scraped page is the failure mode to design against.


## Sources


Anthropic, Claude Code documentation, accessed August 2026:


- Plugins:[https://code.claude.com/docs/en/plugins](https://code.claude.com/docs/en/plugins?ref=scrimba.com)
- Discover and install plugins:[https://code.claude.com/docs/en/discover-plugins](https://code.claude.com/docs/en/discover-plugins?ref=scrimba.com)
- MCP:[https://code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp?ref=scrimba.com)
- Security:[https://code.claude.com/docs/en/security](https://code.claude.com/docs/en/security?ref=scrimba.com)
- Anthropic. "Official plugin marketplace catalog." Accessed August 2026.[https://github.com/anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official?ref=scrimba.com)
- Anthropic. "Community plugin marketplace catalog." Accessed August 2026.[https://github.com/anthropics/claude-plugins-community](https://github.com/anthropics/claude-plugins-community?ref=scrimba.com)
- Scrimba. "Explain." Self-reported product information. Accessed August 2026.[https://scrimba.com/explain](https://scrimba.com/explain?ref=scrimba.com)
