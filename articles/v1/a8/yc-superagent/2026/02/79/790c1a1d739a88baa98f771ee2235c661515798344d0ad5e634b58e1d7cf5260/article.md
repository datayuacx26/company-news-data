---
schema_version: "1.0.0"
document_id: "790c1a1d739a88baa98f771ee2235c661515798344d0ad5e634b58e1d7cf5260"
company_key: "yc-superagent"
company: "Superagent"
source_id: "yc-superagent-news-import-3c33ed06ece7"
canonical_url: "https://www.superagent.sh/blog/launching-brin-sh"
published_at: "2026-02-17T12:00:00+00:00"
first_seen_at: "2026-07-24T02:47:34.464363+00:00"
fetched_at: "2026-07-28T22:19:49.030055+00:00"
content_hash: "sha256:92ce8aedde4c7cab2c9b800154c3f0a4f9473b595e3229b51e748d082e456111"
---

# Launching brin.sh — realtime threat detection for agents

Launching[brin.sh](https://brin.sh/) — realtime threat detection for agents. Brin scores everything your agent is about to consume before it does. Free to use, no auth, no SDK, no signup.


## Agents trust everything by default


Cursor, Claude Code, Codex, Gemini CLI — agents are writing production software everywhere. Some companies are already majority agent-written.


This shift happened fast, and our infrastructure hasn't caught up. Agents fetch packages, load web pages, install MCP servers, read contributor profiles, and clone repositories without reviewing any of it. They reduce the distance between "find external content" and "execute untrusted code" to zero.


That creates three categories of failure:


- **Agents leak your data.** A bad package or prompt injection tricks your agent into sending credentials or internal docs to the wrong place.
- **Agents get scammed.** Your agent pays a fake invoice, follows a spoofed link, or trusts a fraudulent vendor page.
- **Agents break things.** A malicious instruction gets your agent to delete files, drop tables, or push destructive code.


Brin catches all of this before it happens.


## Securing context, not agents


The default approach to agent security is guardrails — restrict what the agent can do, sandbox file access, limit network calls. It works, but it cripples the agent. The more you constrain it, the less useful it becomes.


Brin takes a different approach: let agents operate with full capability and score every piece of external context they consume. A package, a web page, a repo, an MCP server — Brin checks it before the agent trusts it.


You get safety without sacrificing capability.


## How it works


Every entity is scored across four dimensions: who published it, how it behaves, what's actually in it, and who else trusts it. Cached results come back in under 50ms. No auth, no SDK, no signup.


Brin integrates directly with the tools you already use —[Cursor](https://brin.sh/docs/integrations/cursor) ,[Claude Code](https://brin.sh/docs/integrations/claude-code) ,[Gemini CLI](https://brin.sh/docs/integrations/gemini-cli) ,[Codex](https://brin.sh/docs/integrations/codex) ,[LangChain](https://brin.sh/docs/integrations/langchain) ,[AI SDK](https://brin.sh/docs/integrations/ai-sdk) ,[Mastra](https://brin.sh/docs/integrations/mastra) , and more.


## Open data


Every score Brin produces is public and the tools are open source. The API requires no auth and costs nothing. If you prefer not to depend on the API, download the full dataset and host it on your own infrastructure. The more widely trust signals are available, the harder it becomes for malicious context to spread.


Get started at[brin.sh/docs/get-started/quickstart](https://brin.sh/docs/get-started/quickstart) .


Source at[github.com/superagent-ai/brin](https://github.com/superagent-ai/brin) .
