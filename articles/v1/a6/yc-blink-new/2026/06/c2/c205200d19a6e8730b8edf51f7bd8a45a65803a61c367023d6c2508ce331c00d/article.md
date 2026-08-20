---
schema_version: "1.0.0"
document_id: "c205200d19a6e8730b8edf51f7bd8a45a65803a61c367023d6c2508ce331c00d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-second-brain-obsidian"
published_at: "2026-06-12T14:01:22+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:83bd5d217a78b9493ff3b63d79bf96afba77d395c0e2552fcaff4684daf275f8"
---

# OpenClaw Second Brain: Turn Your Agent into a Memory System with Obsidian

You mentioned a startup idea in passing. Something about subscription pricing for indie developers. Three weeks later, you bring it up again.


Your OpenClaw agent responds: "You talked about this on March 14th. You wrote: 'indie devs need metered billing, not flat-fee models.'"


That's not magic. That's a second brain working.


Most people use AI agents as fancy search boxes. Ask a question, get an answer, close the tab. Next session starts blank.


OpenClaw can do something different. It writes to your Obsidian vault, reads your past notes, connects dots across conversations, and surfaces what's relevant before you ask. The agent gets smarter about you over time.


OpenClaw as a second brain — agent connecting memories, notes, and ideas into a living knowledge graph


Blink


*OpenClaw as a second brain — agent connecting memories, notes, and ideas into a living knowledge graph*


## What OpenClaw needs to be your second brain


Four things make this work.


**Memory writing** — the agent must create and update files in your vault. Without this, every insight vanishes when the chat ends.


**Vault access** — give the agent a path to your Obsidian folder. It reads your notes and writes new ones like any other file.


**SOUL.md context** — this tells the agent when to write memories, how to format them, and what counts as worth keeping.


**Daily consolidation** — HEARTBEAT.md triggers a memory review every morning. The agent connects dots between recent notes and older context.


None of this requires external APIs. Everything lives in local markdown files you own.


## How it works: the full architecture


Your Obsidian vault lives at a path like` /Users/kai/obsidian-vault` . The agent gets that path as an environment variable.


When you share something worth remembering — a decision, a project name, a preference — the agent writes a markdown file to an` agent-memory/` subfolder. That file includes the date, topic, key details, and tags.


Every morning, the HEARTBEAT.md cron runs. The agent scans recent memory files, looks for recurring themes, and creates a summary note when the same idea appears across multiple conversations.


The result: your agent knows you better with every session. Not through opaque neural embeddings — through actual markdown files you can read, edit, and fully control.


OpenClaw second brain architecture — agent reads and writes Obsidian vault notes on a daily consolidation cycle


Blink


*OpenClaw second brain architecture — agent reads and writes Obsidian vault notes on a daily consolidation cycle*


1


#### Set up Obsidian vault access


Give your agent the path to your Obsidian folder. In your agent settings, add this as an environment variable:


```text
OBSIDIAN_VAULT_PATH  =  /Users/yourname/obsidian-vault
```


Obsidian has 1M+ users as of 2026. Most vaults share a common structure: a root folder, topic subfolders, and a daily-notes folder. You'll add one more:` agent-memory/` .


Create that folder inside your vault now. The agent writes all its memory files there.


Keep` agent-memory/` separate from your own notes. You can review what the agent remembered without it mixing into your personal writing.


2


#### Configure SOUL.md for memory behavior


SOUL.md is the persistent identity file for your OpenClaw agent. It tells the agent who it is, what it cares about, and how to behave across all conversations.


Add this section to your SOUL.md:


```text
# Memory Behavior


When a user shares information that seems important to remember:
-   Create a note in /Users/[  username  ]/obsidian-vault/agent-memory/
-   Use the format:   `YYYY-MM-DD-[topic].md`
-   Include: what was discussed, key decision, relevant context
-   Tag with #agent-memory and any relevant topic tags


When starting a conversation, check the agent-memory folder for relevant notes.
Surface connections between current topics and past notes proactively.
```


Replace the path with your actual vault location. The date format keeps files sorted chronologically in Obsidian's file explorer.


The more specific your SOUL.md instructions, the more consistently the agent writes useful memories. Tell it exactly what counts: "key decisions, project names, preferences, things the user wants to follow up on."


3


#### Add a memory-writing skill


Skills are the tools that let your OpenClaw agent take actions. You need one that creates and edits files.


Add this skill to your agent's skills folder (for more on building custom skills, see[OpenClaw Skills for Developers](https://blink.new/blog/openclaw-skills-for-developers) ):


```text
# memory-writer


## What you can do
-   Create a new markdown file at a given path
-   Append content to an existing file
-   List files in a directory
-   Read a file's contents


## When to use this
Use this skill whenever the user shares something that should be remembered.
Also use it when the user asks you to recall something from past conversations.


## Format
Filename: YYYY-MM-DD-[  short-topic-slug  ].md
Location: $OBSIDIAN_VAULT_PATH/agent-memory/
Tags: #agent-memory #[  topic-1  ] #[  topic-2  ]
```


OpenClaw's file-write skills work on local paths. No plugin, no API key. About 23% of advanced OpenClaw users run file-writing skills — it's the most powerful capability most people never configure.


4


#### Configure HEARTBEAT.md for memory consolidation


HEARTBEAT.md runs on a cron schedule. It tells the agent what to do proactively — without waiting for you to start a conversation. (The[OpenClaw Morning Briefing guide](https://blink.new/blog/openclaw-morning-briefing-telegram) covers how HEARTBEAT.md powers Telegram notifications using the same cron pattern.)


Add this block to your HEARTBEAT.md:


```text
# Daily Memory Consolidation


schedule: 0 7 * * *


Every morning at 7am, do the following:
1.   List all files in $OBSIDIAN_VAULT_PATH/agent-memory/ created in the last 7 days
2.   Identify recurring themes or topics across those files
3.   If a theme appears in 3+ files, create a summary note:
YYYY-MM-DD-weekly-theme-[  topic  ].md
4.   Check if any memories relate to current projects in $OBSIDIAN_VAULT_PATH/projects/
5.   If yes, add a backlink from the project note to the relevant memory files
```


This cron runs at 7am daily. Your agent reviews its own memory, spots patterns you might have missed, and keeps your project notes linked to relevant context.


5


#### Make it run 24/7 on Blink Claw


Here's where most people hit a wall. The HEARTBEAT.md cron only runs when your OpenClaw instance is active. Close your laptop and you miss your 7am consolidation.


Blink Claw runs your agent 24/7 — even when your laptop is off. Your memory consolidation fires on schedule, every day, regardless of whether your machine is on.


Blink Claw handles everything — no Docker, no VPS, $22/month all-in. LLM costs are included via a 200+ model router. You manage your skills and SOUL.md; Blink handles the infrastructure.


To deploy: go to[blink.new/claw](https://blink.new/claw) , create a new agent, and upload your SOUL.md, skills, and HEARTBEAT.md. The agent boots in your chosen region and starts running immediately.


Pick a region close to you for lower latency. Blink Claw runs in 30+ data center regions. Your vault stays on your machine — the agent writes files locally, so nothing leaves your system.
