---
schema_version: "1.0.0"
document_id: "076f2f25f779632cc3d2d5c9bc7912ca16105165d9c536899aefda9aaf99718c"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/agentic-coding-on-supabase-with-opencode"
published_at: "2026-06-30T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:6143031cc09d8beaa67e7c4db29b4a257854a53c8beda293b6dbf7fe4e53056d"
---

# Agentic Coding on Supabase with OpenCode

The OpenCode integration for Supabase is now available.


Type` /supabase` , authenticate, and your agent can work with Supabase account/project management APIs plus bundled Supabase skills. For database, Edge Functions, logs, and other project-scoped capabilities, the plugin guides users through connecting Supabase MCP. *Requires OpenCode 1.3.4 or later.*


[OpenCode](https://opencode.ai/) is an open source AI coding agent with more than 180,000 GitHub stars and 7.5M monthly active users. It runs in your terminal, IDE, or desktop, supports 75+ LLM providers, and lets you run multiple agents in parallel on the same project.


AI agents are taking on more of the coding workflow, and they need access to more than just the codebase. Teams like[Ramp](https://builders.ramp.com/post/why-we-built-our-background-agent) ,[JFrog](https://jfrog.com/blog/the-agent-has-entered-the-supply-chain-opencode/) ,[Cloudflare](https://blog.cloudflare.com/ai-code-review/) , and[Apple](https://developer.apple.com/videos/play/wwdc2026/232/) are already running OpenCode at scale. This integration gives those agents direct access to your Supabase backend, so they can go beyond writing code and verify their own work, query data, check logs, and deploy functions.


## What the OpenCode integration includes#


Once connected, the integration can:


- Monitor agent activity in real time from OpenCode
- List your Supabase organizations and projects
- Create new Supabase projects from your coding session
- Open the Supabase MCP setup flow for a chosen project
- Bundle Supabase agent skills so your agent gets Supabase-specific guidance without a separate install step


Here's the integration running inside Minecraft. OpenCode connected to Supabase via MCP, created a table, populated it with data, and deployed an Edge Function without ever leaving a survival world. Everything you see is running on a real Supabase project. The dirt house, however, is optional.


The plugin is open source and built in collaboration with[Jumski](https://github.com/jumski) from[pgflow](https://www.pgflow.dev/) . You can find it[here](https://github.com/supabase-community/opencode-supabase) .


## Get started#


[→ Connect OpenCode to your Supabase project](https://github.com/supabase-community/opencode-supabase)
