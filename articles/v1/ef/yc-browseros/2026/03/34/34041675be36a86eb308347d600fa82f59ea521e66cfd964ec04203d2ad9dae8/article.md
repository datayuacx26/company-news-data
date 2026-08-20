---
schema_version: "1.0.0"
document_id: "34041675be36a86eb308347d600fa82f59ea521e66cfd964ec04203d2ad9dae8"
company_key: "yc-browseros"
company: "BrowserOS"
source_id: "yc-browseros-news-import-502741f826e3"
canonical_url: "https://www.browseros.com/blog/soul-memory-engineering"
published_at: "2026-03-13T00:00:00+00:00"
first_seen_at: "2026-07-24T23:09:45.310103+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:4e6e4d5bb7333a5d8506517926372d25cbb5abb65b37ab9692e4dd20c4ddafa9"
---

# Your Browser Agent Now Has a Soul (And It Never Leaves Your Machine)

**Introducing SOUL.md and local memory for BrowserOS — how we made your agent personal without compromising privacy.**


When you use an AI agent in your browser, it sees everything. Your tabs. Your emails. Your calendar. Your bank account. The last thing you want is that agent shipping your behavioral data to a remote server.


But you also want the agent to remember you. To know that you prefer concise answers. That you're a developer. That last Tuesday you were researching flights to Tokyo and you'd like a follow-up.


We just shipped the feature that makes both possible: **SOUL.md** for agent personality, and **local memory** for long-term recall. Everything lives in plain markdown files on your machine. Here's how.


## SOUL.md: One File, One Personality


Every BrowserOS agent now has a` SOUL.md` file at` ~/.browseros/SOUL.md` . This file defines how the agent talks, what it prioritizes, and how it behaves.


During onboarding, you pick a personality preset:


- **Balanced** — clear, helpful, moderate tone
- **Professional** — formal, precise, business-appropriate
- **Friendly** — warm, conversational, uses emoji
- **Minimal** — terse, action-first, no fluff


The agent reads this file at the start of every session. It shapes every response. And because it's a markdown file on your disk, you can open it in any text editor and change it however you want.


Want the agent to always respond in bullet points? Add that to SOUL.md. Want it to never suggest products? Add that boundary. The agent's personality is literally a file you own.


## How SOUL.md Evolves


The preset is just the starting point. SOUL.md goes through three stages:


1. **Default template.** The agent starts with a simple baseline: *"Be genuinely helpful. Have opinions when asked. Be resourceful before asking. Earn trust through competence."*
2. **Observation.** As you interact, the agent picks up on your communication style and preferences — whether you like detailed explanations or terse answers, whether you want confirmation before taking actions, how formal you want things.
3. **Rewrite.** The agent updates SOUL.md to reflect what it's learned. Crucially, it tells you when it does this. Your soul file is capped at 150 lines to prevent bloat, so it stays focused.


Over time, a SOUL.md might grow to include sections like:


```text
## Personality
- Direct and opinionated when asked
- Light humor is fine, sarcasm is welcome


## Boundaries
- Never post to Slack without confirming first
- Never make purchases or click "buy" buttons
- Don't modify files without asking


## Preferences
- Default to bullet points over paragraphs
- Prioritize primary sources over summaries
- When writing code, prefer TypeScript
```


This is a real separation that matters: **SOUL.md defines how the agent acts. Memory stores facts about you and the world.** The agent doesn't conflate "who I am" with "who you are." Its personality rules live in one place. Your data lives in another.


## Memory: The Agent Remembers (Locally)


The memory system has two tiers:


1. **Daily logs** (` ~/.browseros/memory/2026-03-04.md` ) — the agent writes notable facts from each session. These auto-delete after 30 days.
2. **Core memory** (` ~/.browseros/memory/CORE.md` ) — permanent facts the agent promotes from daily logs. Your name, your projects, the tools you use, people you frequently mention. Core memory persists forever and is never automatically deleted.


When you start a new session, the agent proactively searches its memory before responding — not just when you ask it to recall something, but whenever context from a prior session might be relevant. It finds entries using fuzzy matching and weaves them into its response naturally.


You also have explicit control. Tell the agent "remember that I'm using the Hono framework for this project" and it writes it down. Ask "what did we discuss about the API migration?" and it searches. Say "forget my old email address" and it removes it.


The key architectural decision: **no remote calls** . Memory search uses[fuse.js](https://fusejs.io/) for local fuzzy matching. No embeddings sent to OpenAI. No data leaving your machine. The tradeoff is slightly less sophisticated retrieval — but for a browser agent, privacy is non-negotiable.


And we mean it: **memory is never uploaded to the cloud, even if you have Sync to Cloud enabled.** Cloud sync handles preferences and settings. Your memory files stay on your machine, period.


## Inspired by OpenClaw, Built for the Browser


We openly credit[OpenClaw](https://github.com/openclaw/openclaw) as our inspiration. Their approach to agent memory — "just markdown files that the agent reads and writes" — is elegant, and we adopted the core philosophy directly.


OpenClaw is a personal AI assistant platform that connects to messaging channels and gives the agent a persistent workspace with file tools, memory, and browser control. Their memory system is sophisticated: a background indexer watches for file changes, chunks content into ~400-token segments, generates vector embeddings via providers like OpenAI or Gemini, and stores everything in SQLite with both vector search and full-text search indexes. Retrieval uses a hybrid scoring model — 70% vector similarity, 30% keyword matching — with temporal decay on date-named files so recent memories surface first.


Their SOUL.md is generated through a "bootstrap ritual" — a conversational onboarding where the AI asks the user about their name, personality preferences, and boundaries, then writes the answers to files and deletes the bootstrap script.


It's impressive engineering. But it's built for a different context.


Where we diverged:


Decision OpenClaw BrowserOS Why


Soul creation Conversational ritual One-click presets + ongoing evolution Lower onboarding friction


Memory search Remote vector embeddings + keyword search Local fuzzy matching Privacy first


Memory storage SQLite with vector index Plain markdown files Simpler, debuggable, human-readable


Old memory Temporal decay in search scoring Hard deletion after 30 days Predictable cleanup


Identity files 7 files (SOUL, IDENTITY, USER, AGENTS, TOOLS, HEARTBEAT, BOOTSTRAP) 1 file (SOUL.md) + app settings Less overhead


Write safety Standard file writes + file watcher Atomic appendFile for concurrent tool calls Prevents race conditions


The biggest divergence is privacy. OpenClaw's memory pipeline sends content to remote embedding APIs by default (with a fallback to local models). For a browser agent that has access to your most sensitive online activity, that's a line we wouldn't cross. Every byte of BrowserOS memory stays on your filesystem — in a directory you can open, read, and delete with standard tools.


The second divergence is simplicity. OpenClaw's retrieval pipeline — chunking, embedding, SQLite with vec0 and FTS5, hybrid scoring, MMR re-ranking for diversity — is genuinely best-in-class for a general-purpose assistant. But BrowserOS memory files are small. Months of daily agent usage might produce a few hundred kilobytes of markdown. For that scale, fuzzy keyword matching is fast and accurate. We'd rather ship something simple that works than something sophisticated that phones home.


## Why Plain Markdown?


We keep coming back to this decision because it has surprising second-order benefits.


**Debuggability.** When something goes wrong with memory — the agent forgot something, or remembered something incorrectly — you open` ~/.browseros/memory/` in a text editor. The entire state is right there. Try debugging a vector database when your agent hallucinates a memory.


**Portability.** Moving to a new machine? Copy the` ~/.browseros/` directory. That's it. No database export, no migration script, no cloud account to link.


**User trust.** When we say "your data stays local," users can verify it. The files are right there. There's no hidden database, no binary format, no encryption they can't inspect. Transparency isn't just a feature — it's the architecture.


**Editability.** Power users can (and do) edit their SOUL.md and memory files directly. The agent picks up changes on the next session. This turns the agent's personality and memory into something the user truly owns and controls, not a black box managed by the product.


## The Privacy Line


A browser agent occupies a unique position in the AI landscape. It's not a chatbot you visit — it lives where you live. It sees your banking tabs, your medical searches, your private messages. The bar for data handling isn't "good enough." It's "would I be comfortable if every user could see exactly where their data goes?"


With plain markdown files in a user-owned directory, the answer is yes. There's no ambiguity. No "we anonymize before sending" or "encrypted in transit." The data doesn't transit. It sits in` ~/.browseros/` on macOS,` %APPDATA%/.browseros/` on Windows, and` ~/.browseros/` on Linux, and it stays there.


Your browser agent now has a personality and a memory. Both live on your machine. Both are markdown files you can read, edit, and delete.


That's how it should be.
