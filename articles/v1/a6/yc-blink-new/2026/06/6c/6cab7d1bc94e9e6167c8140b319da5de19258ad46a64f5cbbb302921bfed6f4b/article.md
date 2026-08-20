---
schema_version: "1.0.0"
document_id: "6cab7d1bc94e9e6167c8140b319da5de19258ad46a64f5cbbb302921bfed6f4b"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-second-brain"
published_at: "2026-06-11T00:16:06+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:161cc6e9fb4024612a37fbbf4564fc801299d2ba368e2a94910ab987b3ff9654"
---

# OpenClaw Second Brain: Turn Your Agent into a Memory System With Obsidian

## Step-by-Step: Set Up the Second Brain Skill


1


#### Configure SOUL.md for Memory


Open` SOUL.md` and add your knowledge domains explicitly. List specific subtopics, not broad categories. Write "LLM inference costs" instead of "AI." Write "product-led growth funnels" instead of "marketing." The agent tags captures against this list, so precision here pays off on every future retrieval.


2


#### Connect OpenClaw to Your Obsidian Vault


Set the agent's write path to your[Obsidian](https://obsidian.md/) vault folder. The agent writes standard markdown files there; Obsidian renders the graph view automatically. Your captures become graph-connected, backlinked notes without any manual formatting. For setup details, the[openclaw-getting-started guide](https://blink.new/blog/openclaw-getting-started) walks through the vault path configuration step by step.


3


#### Write Your Standing Capture Instruction


Add this capture rule to your agent's system prompt:


```text
When I send a note, link, or idea:
1. Extract the core insight in one sentence (max 25 words)
2. Assign 2–3 topic tags from my SOUL.md interest list
3. Add to memory.json with today's date and source
4. If it connects to an existing memory, add a cross-reference link
5. Write the entry to my Obsidian vault as a markdown file


```


This becomes the agent's default behavior for everything you send. One message triggers the full capture pipeline.


4


#### Install the obsidian-writer Skill


The` obsidian-writer` skill from the[OpenClaw skills registry on GitHub](https://github.com/openclaw-community/skills) handles the Obsidian frontmatter mapping automatically. It converts` memory.json` entries into properly formatted Obsidian notes with YAML frontmatter, tags, and backlink syntax. Enable it once and Obsidian graph views populate without any manual work.


5


#### Set Up the Weekly Digest


Add this line to your` HEARTBEAT.md` file:


```text
0 20 * * 0 weekly_knowledge_digest


```


Every Sunday at 8 PM, the agent groups that week's captures by topic, finds the 3 most-connected ideas, and sends a digest to Telegram. The digest replaces the mental overhead of remembering what you learned — it takes under 3 minutes to read.


6


#### Test Recall


Send 3 test notes across different topics. Then ask: "What do I know about \[topic from note 1\]?" Verify the agent returns the note, its tags, and any cross-references. If recall misses, tighten the search prompt. If tags are wrong, add more specific definitions to` SOUL.md` .


## Real Automation Examples


Five capture patterns deliver the highest value relative to setup effort.


**Meeting notes → action items.** Send your raw meeting notes after any call. The agent extracts action items, saves them to a` meeting-log.md` file tagged by project, and flags your next step. Missed follow-ups become structurally impossible.


**Article summaries.** Send a URL. The agent fetches the page, extracts 3 key points, saves them to` memory.json` with the source URL and tags. Your reading list becomes searchable knowledge instead of forgotten open tabs.


**Voice memo capture.** Record a voice memo on your phone; the Whisper API transcribes it ($0.006/minute), and the agent extracts the insight and stores it. Commute ideas and shower thoughts stop disappearing.


**Morning briefings.** The agent checks your calendar, scans` memory.json` for entries tagged to your active projects, and sends a 2-minute briefing at 8 AM via Telegram. The[AI agent morning routine guide](https://blink.new/blog/ai-agent-morning-routine) covers the full briefing setup with cron patterns.


**Book notes.** Export Kindle highlights or paste them directly. The agent distills highlights into atomic insights, links related concepts across books, and adds them to` book-notes.md` . Tiago Forte's CODE methodology (Capture, Organize, Distill, Express) works even better when the filing is automatic.


## Build This With Blink Claw


Memory capture has one hard dependency: the agent must be running when something happens. Your calendar events fire at 2 PM. Emails land at 3 AM. Voice memos upload from your phone at midnight.


A laptop-based OpenClaw setup misses every capture that happens when the machine is asleep. That's the central failure mode of self-hosted memory systems — they work well during active hours and fail at every other time.


[Blink Claw](https://blink.new/claw) runs your OpenClaw agent 24/7 on cloud infrastructure across 30+ global data centers. Your agent runs around the clock — not just when your laptop is on. No Docker needed, no VPS to configure or maintain. Blink Claw handles all of that automatically.


The pricing is $22/mo all-in — LLM costs included via a 200+ model router. You bring no API keys. Security patches apply automatically — you never track CVEs. Message your agent from Telegram, Discord, or Slack regardless of where you are. For most second brain setups, the all-in $22/mo plan costs less than a premium Notion subscription and delivers more automated capture than any manual system.


The[openclaw-getting-started guide](https://blink.new/blog/openclaw-getting-started) covers the full Blink Claw setup, including Telegram connection and initial vault configuration.


Before OpenClaw: chaotic tabs and notes. After: organized knowledge base that surfaces what you need


Blink


## Frequently Asked Questions


Yes. When you set the vault path in the agent's config, OpenClaw writes standard markdown files directly to that directory. Obsidian picks them up instantly — no sync lag, no special plugin required. The` obsidian-writer` skill handles frontmatter formatting so every file appears with proper tags and backlinks in the graph view.


` memory.json` stores atomic facts in a structured, agent-searchable format — optimized for recall queries. Custom files like` meeting-log.md` or` book-notes.md` hold richer, human-readable content organized by domain. The agent writes to both. When you ask "what do I know about X," it searches` memory.json` first for speed, then scans relevant custom files for depth.


Yes. OpenClaw writes standard markdown files — any markdown-compatible app reads them. Notion works via the official API with a` NOTION_API_KEY` . Logseq, Bear, and Dendron all read the same file format Obsidian does. The Obsidian` obsidian-writer` skill adds graph-view formatting; without it, files still capture correctly and remain fully searchable.


Add a quality filter to your capture instruction: "Only save this if the insight is novel, non-obvious, or immediately actionable." You can also add a confirmation step — the agent queues candidates and you approve before writes. Most users drop the confirmation after 2 weeks once the tagging quality is calibrated to their SOUL.md.


Your data lives in files you control:` memory.json` , markdown files in your Obsidian vault, or your Notion database. OpenClaw writes standard formats — no proprietary lock-in. Stop the agent and everything stays exactly where it is, readable without OpenClaw or any special tooling.


Local OpenClaw only captures when your machine is awake. A calendar event at midnight, a voice memo from your phone, an email at 5 AM — all missed. Blink Claw runs 24/7 on cloud infrastructure at $22/mo all-in with LLM costs included. No Docker, no VPS, no maintenance. The second brain works while you sleep instead of only while you work.
