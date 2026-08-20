---
schema_version: "1.0.0"
document_id: "7358eecacad27b6687bc880e1ce51f36bc4c06114421e32e0494733d1fb869ba"
company_key: "yc-bits-2"
company: "Bits"
source_id: "yc-bits-2-news-import-8ae56e6aae1c"
canonical_url: "https://klausai.com/blog/openclaw-memory-how-to-make-your-agent-actually-remember/"
published_at: "2026-04-09T00:00:00+00:00"
first_seen_at: "2026-07-24T21:07:46.493176+00:00"
fetched_at: "2026-07-28T22:15:57.305389+00:00"
content_hash: "sha256:3baba8d7bb5c9b40ab353e1f78a6285d245ef68acfb5fd96f9bd7c3504b30766"
---

# OpenClaw Memory: How to Make Your Agent Actually Remember

One of the most common questions I hear from OpenClaw users is: “Why doesn’t OpenClaw remember what I told it?”


I ask them what they did to get it to remember. The answer is almost always the same: they assumed it would just work. But this is where I see the most avoidable frustration, and the fixes are straightforward once you understand what’s actually happening.


I wrote a[short primer on this topic](https://klausai.com/blog/why-doesnt-openclaw-remember/) a few weeks ago. This article goes deeper: what each storage layer does, what to put where, and what breaks when you get it wrong.


## Why LLMs Forget Everything


Large language models have no built-in persistent memory. Every conversation starts from zero. The model does not remember your name, your preferences, or the three hours you spent explaining your business workflow yesterday. This is a fundamental constraint of the technology, not a bug in OpenClaw ([Zhang et al., 2024](https://arxiv.org/abs/2404.13501) ).


OpenClaw solves this by writing important context to plain Markdown files on disk and loading them back at the start of each session ([OpenClaw Docs](https://docs.openclaw.ai/concepts/memory) ). The system is simple, but it has specific behaviors that trip people up.[3.2 million people](https://openclawvps.io/blog/openclaw-statistics) now use OpenClaw, and memory configuration is the most common support question we get at Klaus.


If you want to understand[how OpenClaw works at a higher level](https://klausai.com/blog/how-openclaw-actually-works-architecture-for-non-engineers/) , start there. This article is specifically about memory.


## The Four Places OpenClaw Stores Information


OpenClaw has four storage layers. Each one loads differently, persists differently, and has different limits. Most memory problems come from putting the right information in the wrong layer.


Storage Layer When It Loads Persistence Character Limit


**Bootstrap files** (AGENTS.md, TOOLS.md, MEMORY.md, SOUL.md, USER.md) Every conversation, automatically Permanent until you edit them 20,000 per file; 150,000 aggregate


**Daily memory files** (memory/YYYY-MM-DD.md) Today and yesterday, automatically Permanent on disk; older files searchable only No hard limit, but search quality degrades with size


**Skills** On demand, when the agent decides one is relevant Permanent until deleted No limit (not counted against bootstrap budget)


**Filesystem** (~/.openclaw/workspace/) Only when the agent explicitly reads a file Permanent Disk space


The bootstrap files are the most important layer for most users. They load into every conversation automatically, so anything you put there, the agent sees every time ([OpenClaw Docs](https://docs.openclaw.ai/concepts/memory) ).


The limits matter more than people realize. If your bootstrap files exceed 150,000 characters combined, content gets silently truncated. The agent behaves as if those instructions don’t exist. You can check whether this is happening by running` /context list` in an OpenClaw session, which shows what’s actually loading and whether content is being cut.


## What to Put in MEMORY.md (and What Doesn’t Belong)


MEMORY.md is for durable facts that should be true across every session: your name, your preferred communication style, your timezone, recurring workflows, contact preferences, tool configurations.


What doesn’t belong: running context, session-specific notes, large reference documents, or detailed task instructions. These bloat the file and push past the 20,000-character per-file limit.


A 25,000-character MEMORY.md means the last 5,000 characters get truncated. If your most important instructions are at the bottom, the agent never sees them. The truncation is silent. The agent doesn’t tell you it’s missing context, because from its perspective, that context doesn’t exist.


The practical rule: MEMORY.md should stay small and stable. If something changes every week, it probably belongs in a daily file. If it’s a detailed workflow, it probably belongs in a skill.


At Klaus, we pre-configure MEMORY.md with a few essentials: the user’s name, their timezone, and a note telling the agent to aggressively save new information to the right place. The less the agent has to load by default, the more room it has for the conversation itself.


## How Daily Memory Files Actually Work


At the start of each session, OpenClaw loads today’s daily file and yesterday’s. That’s it. Older daily files stay on disk but they are not pre-loaded ([OpenClaw Docs](https://docs.openclaw.ai/concepts/memory) ).


When a conversation gets long, OpenClaw runs a pre-compaction memory flush: a silent turn that reminds the agent to save important context to memory files before the conversation gets summarized. This is on by default and requires no configuration. It catches a lot of context that would otherwise be lost.


But “a lot” is not “everything.” If you rely on the agent remembering something from last Tuesday and you don’t search for it explicitly, it won’t be there. The agent only sees today and yesterday by default.


I’ve looked through my daily memory logs and Bailey’s. Honestly, most of the automatic entries aren’t very useful. They tend to be vague summaries rather than actionable notes. The pre-compaction flush is better than nothing, but I wouldn’t rely on it as your primary memory strategy.


What works better: explicitly telling the agent to save things. “Write this down in MEMORY.md: always use gogcli for reading Google Drive, never use the browser.” That sticks. The automatic daily logs are a safety net, not a strategy.


## When to Write a Skill Instead of a Memory Entry


If you want the agent to do something specific and repeatable, make a skill. If you want it to know something general, update a bootstrap file.


Skills have a key advantage: the agent only loads the full skill instructions when it decides one is relevant. Skills don’t consume your bootstrap character budget. You can have hundreds of skills without affecting how much context loads into a conversation.


Putting detailed task instructions in MEMORY.md means the agent reads them in every conversation, whether relevant or not. That’s wasted context window. A skill only loads when the agent decides it’s needed.


Example: “Sign all emails as ‘Klaus, on behalf of Robbie’ and always ask for confirmation before sending” belongs in a` send-email` skill. “Always use formal tone in client emails” belongs in USER.md. The distinction is specificity: general preferences go in bootstrap files, task-specific workflows go in skills.


I use this pattern for everything at Klaus. “Update the CRM skill: always push to git after making an update.” “Write down in TOOLS.md: always use gogcli for reading Google Drive.” Every time I want OpenClaw to remember something, I explicitly tell it which method to use. The agent is surprisingly good at following these instructions if you’re specific about where to store the information.


## How Memory Search Works (and How to Configure It)


OpenClaw provides two memory tools:` memory_search` for semantic search over indexed files, and` memory_get` for reading specific files or line ranges ([OpenClaw GitHub](https://github.com/openclaw/openclaw/blob/main/docs/concepts/memory.md) ).


` memory_search` uses hybrid search by default: 70% vector similarity (semantic meaning) and 30% keyword matching (exact terms like IDs and code symbols) ([OpenClaw Config Reference](https://docs.openclaw.ai/reference/memory-config) ). This means the agent can find “meeting preferences” even if you stored the note as “schedule settings.”


The search requires an embedding provider. OpenClaw auto-detects from available API keys: OpenAI, Gemini, Voyage, or Mistral. If you have any of these configured, memory search enables automatically ([OpenClaw Config Reference](https://docs.openclaw.ai/reference/memory-config) ).


Without an embedding provider,` memory_search` falls back to keyword-only matching. The agent can still find exact terms, but it can’t make the semantic leap between related concepts. If your agent seems to forget things that are clearly in its memory files, check whether search is actually enabled.


Three backend options exist:


Backend Best For Trade-offs


**Builtin (SQLite)** Most users; works out of the box Default choice; no extra setup needed


**QMD** Power users who want reranking and query expansion Requires sidecar process; can index directories outside workspace


**Honcho** Multi-agent setups needing cross-session memory Plugin install; adds user modeling and multi-agent awareness


You can check your current setup from the command line:


```text
openclaw   memory   status      # Shows index status and provider
openclaw   memory   search   "query"    # Test search from the terminal
openclaw   memory   index   --force     # Rebuild the index if needed
```


At Klaus, we enable semantic search over session logs and instruct the agent to use this whenever a user insists they’ve already explained something. We also ship a deep-recall skill that wraps this with more aggressive search strategies for when the built-in search doesn’t surface it.


## Common Memory Pitfalls (and What Actually Happens)


These are the failures I see most often across Klaus instances.


**Bootstrap file truncation.** Files over 20,000 characters get silently cut. The aggregate cap is 150,000 characters across all bootstrap files. Run` /context list` to check whether your files are being truncated. If injected characters don’t equal raw characters, content is being cut.


**Compaction loss.** When conversations get long, OpenClaw summarizes older messages to free up context window. Details that weren’t saved to disk before compaction disappear permanently. The pre-compaction memory flush helps, but it catches general themes better than specific details. If something matters, tell the agent to write it down before the conversation gets long.


**Session boundary confusion.** Switching messaging platforms (WhatsApp to Slack) or starting a new session resets conversation history. Only what’s written to files persists across sessions. Your files — MEMORY.md, daily logs, skills — all carry over. Your conversation history does not.


**Memory search not enabled.** Without an embedding API key, the agent can’t do semantic search over older daily files. It can still read them with` memory_get` if you know the filename, but it can’t search by meaning.


**Stale memory.** Old instructions in MEMORY.md that no longer apply, but the agent still follows. When your agent contradicts itself or does something you stopped wanting weeks ago, check MEMORY.md for outdated entries. Memory files need occasional maintenance, just like any configuration.


## Frequently Asked Questions


### Does OpenClaw remember things automatically?


Partially. OpenClaw writes daily memory logs and runs a pre-compaction flush to save context before conversations get summarized. But the most reliable memory comes from explicit instructions: skills and bootstrap files you write yourself. Automatic memory is a safety net, not a primary strategy.


### How much can MEMORY.md hold?


Each bootstrap file has a 20,000-character limit. The aggregate limit across all bootstrap files (AGENTS.md, TOOLS.md, MEMORY.md, SOUL.md, USER.md) is 150,000 characters. Exceeding these limits causes silent truncation ([OpenClaw Docs](https://docs.openclaw.ai/concepts/memory) ).


### Can I use an external memory system instead?


Yes. OpenClaw supports three backends: Builtin (SQLite, default), QMD (local-first with reranking), and Honcho (cross-session with multi-agent awareness). The plugin system also allows custom memory providers ([OpenClaw Docs](https://docs.openclaw.ai/concepts/memory) ).


### What happens to memory when I switch from WhatsApp to Slack?


Your files persist: MEMORY.md, daily logs, skills, everything in the workspace. Your conversation history does not. The agent starts a new session but loads the same memory files. From the agent’s perspective, it knows who you are and what you’ve configured. It just doesn’t remember what you said ten minutes ago on WhatsApp.


## Key Takeaways


- OpenClaw stores memory in four layers: bootstrap files, daily logs, skills, and the filesystem. Each loads differently and has different limits.
- MEMORY.md loads every session but truncates at 20,000 characters. Keep it for durable facts, not running context.
- Daily files load for today and yesterday only. Older context requires` memory_search` with an embedding provider configured.
- Skills are better than memory entries for repeatable tasks because they load on demand and don’t consume bootstrap character budget.
- Run` /context list` and` openclaw memory status` to diagnose what your agent actually sees.
- The pre-compaction memory flush catches some context automatically, but explicit “write this down” instructions are more reliable.
- At Klaus, we ship with memory pre-configured: aggressive save instructions, semantic session search, and a deep-recall skill for when your agent can’t find something you know you told it.


---


Want to skip the configuration?[Klaus](https://klausai.com/) ships with all of this out of the box.


## Sources


- [OpenClaw Docs: Memory Overview](https://docs.openclaw.ai/concepts/memory) . Official documentation on MEMORY.md, daily files, memory tools, and backends.
- [OpenClaw Docs: Memory Configuration Reference](https://docs.openclaw.ai/reference/memory-config) . Full configuration reference for memory search, embedding providers, and backend settings.
- [OpenClaw GitHub: memory.md](https://github.com/openclaw/openclaw/blob/main/docs/concepts/memory.md) . Source documentation for memory tools, CLI commands, and DREAMS.md experimental feature.
- [OpenClaw Statistics 2026](https://openclawvps.io/blog/openclaw-statistics) . Ecosystem metrics: 346K GitHub stars, 3.2M monthly users, 44K ClawHub skills.
- Zhang et al. “[A Survey on the Memory Mechanism of Large Language Model based Agents](https://arxiv.org/abs/2404.13501) .” 2024. Academic survey on why LLMs require external memory systems.
