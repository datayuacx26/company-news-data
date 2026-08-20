---
schema_version: "1.0.0"
document_id: "86fafff3edccc22e8f184ae89230e2a5b0136cf72268711d8e3acdb576e58258"
company_key: "yc-lua-global-inc"
company: "Lua Global Inc"
source_id: "yc-lua-global-inc-rss-87d3f9cc68e5"
canonical_url: "https://blog.heylua.ai/lua-now-ships-inside-claude-code/"
published_at: "2026-05-03T10:03:04+00:00"
first_seen_at: "2026-07-24T10:03:36.530538+00:00"
fetched_at: "2026-07-28T21:56:02.721351+00:00"
content_hash: "sha256:effa8a20b6030e5afdd0ccdc8653dadbefd0942fe073bfdc28cfd7bcac2632af"
---

# Lua now ships inside Claude Code

Today we're shipping the Lua plugin for[Claude Code](https://code.claude.com/?ref=blog.heylua.ai) — the fastest way to go from "I have an idea for an agent" to a deployed Lua AI agent without ever leaving your terminal.


If you've been building on Lua already, you know the loop: scaffold a skill, wire it up, run it locally, push it, debug, repeat. The plugin collapses that loop into conversation. Describe what you want, let Claude do the busywork, ship it.


## What's in the box


The plugin installs a complete agent-building toolchain into Claude Code:


- **14 slash commands** covering the full project lifecycle — from` /lua-init` and` /lua-new` through` /lua-test` ,` /lua-chat` ,` /lua-logs` , and` /lua-deploy` .
- **5 specialised subagents** — including` /lua-architect` for turning a goal into an architecture, and` /lua-qa` for systematic quality checks.
- **9 safety hooks** that intercept risky operations before they touch production.
- **An MCP server** exposing 5 read-only tools so Claude can introspect your Lua project and platform state directly.


## Why we built this


Agent development has too many moving parts: project layout, skill manifests, environment config, deploy gates, logs in three places. Most of it isn't interesting work — it's overhead between you and the thing you're actually trying to build.


We wanted to remove the overhead. With the plugin installed, "build me an agent that books appointments and follows up over WhatsApp" becomes a conversation. Claude plans the architecture, scaffolds skills, runs tests, surfaces failures with root-cause analysis, and walks you through deployment with a five-gate safety sequence so you don't push something half-finished into production.


## Install it


From inside Claude Code:


```text
/plugin marketplace add lua-ai-global/claude-code-lua-plugin
/plugin install lua-agent-builder@claude-code-lua-plugin
/reload-plugins
```


Then` /lua-auth` to sign in (email + OTP, or paste an API key from[admin.heylua.ai](https://admin.heylua.ai/?ref=blog.heylua.ai) ) and` /lua-doctor` to verify everything is wired up correctly.


Prerequisites: Node.js 18 or newer, lua-cli 3.12.3 or newer, and a Lua account.


## Where to go from here


- Plugin source and full docs on GitHub:[lua-ai-global/claude-code-lua-plugin](https://github.com/lua-ai-global/claude-code-lua-plugin?ref=blog.heylua.ai)
- Marketing page and walkthrough:[heylua.ai/claude-code](https://heylua.ai/claude-code/?ref=blog.heylua.ai)
- The full[User Guide](https://github.com/lua-ai-global/claude-code-lua-plugin/blob/main/docs/USER_GUIDE.md?ref=blog.heylua.ai) covers slash commands, hooks, MCP tools, the safety model, and troubleshooting.


This is the first release — we're shipping fast, so if you hit something rough or want a command that isn't there yet, open an issue or ping us. We're listening.


## Up next: Cursor


Claude Code is the first editor we've shipped a first-class Lua plugin for, but it won't be the last. **Cursor is next** — same toolchain, same conversational workflow, native to where you already write code. Stay tuned.


— Stefan
