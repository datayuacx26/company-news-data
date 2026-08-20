---
schema_version: "1.0.0"
document_id: "f54640d25504b64fed4a12b279d26adf582aefb9ee49b3d28753c822ce78abc2"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-second-brain-memory"
published_at: "2026-06-11T12:32:30+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:2cf97570a4d33b90830e186d4016591b6d2c6ddcd3c325ab8bbfff1e5f326845"
---

# OpenClaw Second Brain: Turn Your Agent Into a Persistent Memory System

## Setting Up Your Memory System


This takes about 15 minutes. You'll configure SOUL.md, create the MEMORY.md file, and optionally connect Obsidian.


### Step 1: Configure SOUL.md


SOUL.md lives in your OpenClaw project directory. This is where you define your agent's identity and standing instructions. Add a` \[MEMORY\]` section:


```text
# MEMORY PROTOCOL


## On every session start:
Read MEMORY.md completely before responding to any request.
Reference relevant past entries when they apply to the current topic.


## When to capture:
-   User shares a meeting summary → extract key decisions, file under [  MEETINGS  ]
-   User pastes a book highlight → file under [  READING  ] with source title
-   User expresses an idea, belief, or intention → file under [  THINKING  ]
-   User shares a project update → file under [  PROJECTS/<project-name>  ]


## Capture format:
Append to MEMORY.md using this exact structure:
### [  CATEGORY  ] YYYY-MM-DD
**Source:**   (conversation / Telegram / Readwise / etc.)
**Content:**   (the insight or note, max 3 sentences)
**Tags:**   #tag1 #tag2


## On retrieval:
When user asks "what do I know about X?" — search MEMORY.md for relevant entries and summarize them chronologically.
Always cite the date and category of retrieved notes.
```


This instruction set tells the agent when to store, what format to use, and how to recall. You can customize every part of it.


### Step 2: Create MEMORY.md


Create an empty file in your project root:


```text
touch   MEMORY.md
```


Add a header so the agent understands the file structure:


```text
# Agent Memory


This file is maintained by your OpenClaw agent. Do not edit manually.
Entries are appended chronologically by the agent.


---
```


That's the complete setup for the base system. The agent will append entries below the` ---` as you feed it information.


### Step 3: Optional — Obsidian Integration


If you use Obsidian, configure the agent to write individual note files to your vault instead of (or in addition to) MEMORY.md.


Add to SOUL.md:


```text
## Obsidian vault:
Vault path: /Users/yourname/Documents/ObsidianVault
For [  READING  ] entries: create a new file at /Inbox/reading-YYYY-MM-DD-slug.md
For [  MEETINGS  ] entries: create a new file at /Meetings/YYYY-MM-DD-summary.md
Always include [[  backlinks  ]] to related notes when relevant.
```


The agent uses its file system tools to write directly to your vault directory. Obsidian picks up the new files on next sync. No plugin required — the agent writes standard markdown.


The OpenClaw memory system: capture via Telegram, store in MEMORY.md, retrieve on demand — a persistent knowledge base


Blink


## Using Your Memory System


### Capturing via Telegram


The fastest capture method is Telegram. Connect OpenClaw to Telegram, then send thoughts while commuting, in meetings, or after reading.


A typical session:


```text
You (Telegram): Just finished chapter 3 of Range by David Epstein. Key idea:
specialists are better in predictable environments, generalists outperform
in complex changing ones. Directly relevant to our hiring debate.


Agent: Got it. Filed to MEMORY.md under [READING]:


### [READING] 2026-06-10
**Source:** Range — David Epstein, Ch. 3
**Content:** Specialists outperform in stable, predictable domains.
Generalists outperform where environments are complex and changing.
Relevant to hiring debate — supports the generalist case for ambiguous roles.
**Tags:** #books #strategy #hiring #range


```


You can also send voice notes. The agent transcribes, extracts the key point, and files it — same format.


### Querying Your Memory


Once you've built a few weeks of entries, retrieval becomes the high-value feature.


```text
You: What do I know about fundraising?


Agent: Found 7 relevant entries across [MEETINGS], [READING], and [THINKING]:


**[MEETINGS] 2026-04-22** — Call with Y Combinator partner. Key point:
they weight founder-market fit above traction at pre-seed. Suggested
framing our background in the deck introduction.


**[READING] 2026-04-30** — Zero to One, Ch. 5. "The best startups are
secrets — things you believe that most people don't." Related to our
positioning debate on niche vs. broad market.


**[THINKING] 2026-05-14** — Realized our current deck leads with the
problem rather than the insight. Consider restructuring.


... (4 more entries)


Want me to synthesize these into a fundraising prep brief?


```


The agent searches by keyword, date range, or tag. It surfaces notes you'd never find manually — because you've forgotten you wrote them.


### Weekly Summary


Instruct the agent to create a weekly digest automatically. Add to SOUL.md:


```text
## Weekly summary:
Every Sunday, create a file weekly-summary-YYYY-WW.md with:
1.   New entries this week by category
2.   Patterns or recurring themes
3.   Open questions flagged in [  THINKING  ] entries
4.   Three "connections" — notes from different categories that relate to each other
```


This transforms your memory system from a search engine into an active thinking partner. The connection layer is where second brain value compounds — when the agent notices that your hiring notes from April relate to your strategy reading from May.


## Example MEMORY.md Entry Format


After a few weeks, your MEMORY.md looks like this:


```text
# Agent Memory


This file is maintained by your OpenClaw agent. Do not edit manually.
Entries are appended chronologically by the agent.


---


### [  MEETINGS  ] 2026-05-03
**Source:**   Weekly team sync
**Content:**   Decided to delay Q3 launch by 6 weeks. Reason: engineering
lead flagged that authentication module needs security audit first.
Next review date: June 15.
**Tags:**   #product #launch #authentication #decisions


### [  READING  ] 2026-05-07
**Source:**   The Mom Test — Rob Fitzpatrick, Ch. 2
**Content:**   Customers lie (unknowingly) when you ask about their problems
directly. Better to ask about past behavior and actual spending. Relevant
to our upcoming user research interviews.
**Tags:**   #books #research #customer-discovery #interviews


### [  THINKING  ] 2026-05-09
**Source:**   Telegram voice note, morning commute
**Content:**   Wonder if our freemium conversion problem is actually a
positioning problem — wrong users signing up, not weak conversion flow.
Should test messaging changes before touching the funnel.
**Tags:**   #product #growth #hypothesis #freemium
```


Each entry is self-contained. The date and category make it searchable. The tags enable cross-referencing across months of entries.


## The 24/7 Advantage


A second brain agent only compounds in value if it's always running. Proactive features — weekly summaries, pattern detection, scheduled reminders — require the agent to be online even when you're not at your desk.


This is where infrastructure becomes the bottleneck. Running OpenClaw yourself means managing Docker, a VPS, uptime monitoring, and manual updates every time a new version ships.


[Blink Claw](https://blink.new/claw) removes all of that. For **$22/mo all-in** — LLM costs included — your agent runs 24/7 on managed infrastructure. No Docker needed, no VPS to configure. Auto-patching is applied automatically, so you never fall behind on OpenClaw updates.


The 24/7 uptime matters specifically for memory work. When you send a Telegram message at 7am, the agent processes it immediately — not when you next open your laptop. When the weekly summary runs on Sunday, it runs whether or not you're at your desk. **The agent keeps building your knowledge base even when you're offline.**


You can also message it from Telegram to add memories on the go — capture the insight the moment you have it, not hours later when you've forgotten the nuance.


Community members running Blink Claw report memory systems with 500–1,000 entries after three months of consistent use. That's where the compound value kicks in: the agent starts making connections between notes you've forgotten, and retrieval surfaces context you'd never find manually.


For OpenClaw users who want to go deeper on automation, the[OpenClaw personal assistant setup guide](https://blink.new/blog/openclaw-personal-assistant-setup) and[OpenClaw Telegram integration guide](https://blink.new/blog/openclaw-telegram-integration) cover the full configuration stack.


OpenClaw recalls relevant memory on demand — ask a question, get a structured summary of what you've captured


Blink


## Frequently Asked Questions


Not by default. OpenClaw has no built-in persistent memory — each session starts fresh. The SOUL.md + MEMORY.md setup described in this guide creates persistent memory by teaching the agent to read and write a file it can access across sessions. Once configured, the agent reads MEMORY.md at the start of every session and appends new entries during the session.


Yes. OpenClaw supports Notion integration via its connector tools. Configure the agent in SOUL.md to write new entries to a Notion database or page instead of (or alongside) MEMORY.md. The Obsidian approach is simpler to set up since it writes plain markdown files directly — no API key required. Notion requires a Notion integration token and slightly more configuration.


Context window limits apply when the agent reads the full file. A well-structured MEMORY.md with 300–500 entries (around 50–80KB) works without issue. Beyond that, consider splitting by category: MEMORY-READING.md, MEMORY-MEETINGS.md, MEMORY-THINKING.md. Instruct the agent in SOUL.md to read only the relevant file based on the current query type. Obsidian integration avoids this limit entirely since the agent searches individual files rather than loading one large file.


MEMORY.md and SOUL.md are plain text files you own. They survive any OpenClaw version update. If you run Blink Claw, auto-patching updates the agent runtime while leaving your configuration and data files intact. The only thing that changes between versions is the agent's capability — your memory data stays in place.


Yes, with some structure. Create separate memory files per person (MEMORY-alice.md, MEMORY-bob.md) and configure the agent to route entries by Telegram sender ID or by a prefix in messages. For team knowledge — shared decisions, project notes, meeting summaries — a shared MEMORY-team.md works well. Individual notes stay private; team notes go to the shared file. This setup works particularly well on Blink Claw since the agent runs as a persistent service accessible to multiple users simultaneously.


---


*External references:[Building a Second Brain](https://www.buildingasecondbrain.com/) by Tiago Forte ·[OpenClaw documentation](https://openclaw.ai/docs) ·[Obsidian](https://obsidian.md/) — local-first markdown knowledge base ·[Readwise](https://readwise.io/) — highlight sync for Kindle, web, and podcasts*
