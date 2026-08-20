---
schema_version: "1.0.0"
document_id: "fb14ae8ecc68b37f64d139e72edfec6bf2a3790db09d9570ac4c7df9ab27b0b2"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-second-brain-obsidian-memory"
published_at: "2026-06-07T12:24:58+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:23.535371+00:00"
content_hash: "sha256:f4c0eea54a32f0702f47800bb731556024023eb3fabd259151678a74eedf59eb"
---

# OpenClaw Second Brain: Turn Your Agent Into a Memory System

## Step 1: Enable Obsidian Local REST API


Install the[Obsidian Local REST API plugin](https://github.com/coddingtonbear/obsidian-local-rest-api) from Obsidian Community Plugins.


Enable it. Note the port (default 27123) and generate an API key in the plugin settings.


Add both to your OpenClaw SOUL.md or environment:


```text
OBSIDIAN_PORT=27123
OBSIDIAN_API_KEY=your-key-here


```


## Step 2: Write Your SOUL.md Memory Section


Your SOUL.md defines who your agent is and what it values. Add a memory section:


```text
## Memory System


I maintain a persistent knowledge base in Obsidian at vault path ~/Notes.


Daily behavior:
-   Morning: read /Daily/{{date}}.md for today's priorities
-   During sessions: capture decisions, insights, and action items to /Capture/inbox.md
-   Evening: summarize the day to /Daily/{{date}}.md under "## Session Log"


When asked about past work: search Obsidian before answering. Never say "I don't remember" without checking first.
```


## Step 3: Set Up HEARTBEAT.md for Daily Capture


HEARTBEAT.md controls your agent's scheduled behaviors. Add these triggers:


```text
## 07:00 — Morning Brief
Read today's Obsidian daily note. Summarize priorities. Send via Telegram.


## 21:00 — Evening Capture
Ask: "What did you accomplish today?" Write the response to today's Obsidian daily note under "## Session Log".


## 23:00 — Weekly Review (Sundays only)
Read the last 7 daily notes. Write a weekly summary to /Weekly/{{week}}.md.
```


## Step 4: Test the Memory Connection


Ask your agent directly:


> "Check Obsidian for any notes about my current projects."


If the Local REST API is connected, your agent returns matching notes. If it errors, check that Obsidian is running and the port is correct.


## What Gets Captured Automatically


Once the setup is complete, your agent captures:


- Every action item you mention in conversation →` /Capture/inbox.md`
- Morning briefing summaries →` /Daily/{{date}}.md`
- Decisions made during work sessions → appended to the relevant project note
- Code snippets and commands →` /Reference/snippets.md`


After 30 days, your Obsidian vault becomes a searchable log of everything your agent helped you do.


## The Missing Piece: Always-On Hosting


An Obsidian second brain only works if your agent runs 24/7. If it runs only when your laptop is open, the evening capture fires only when you're awake — which defeats the purpose.


Blink Claw runs your OpenClaw agent on managed infrastructure. Your agent runs 24/7, messages you on Telegram, and captures to Obsidian whether your laptop is on or not.


[Run OpenClaw without the hassle — Blink Claw handles everything from $22/mo → blink.new/claw](https://blink.new/claw)


## Skill: ObsidianMemory


The OpenClaw community has a dedicated` ObsidianMemory` skill in ClaWHub. Search for it by name. It wraps the Local REST API calls so your agent doesn't need raw API knowledge — just install the skill and it handles the vault read/write.


See also:[OpenClaw skills guide](https://blink.new/blog/openclaw-skills-guide) for how to find and install community skills.


## What Skill to Start With


Start with daily notes only. Do not try to capture everything from day one. The inbox fills fast and becomes noise.


The order that works:


1. **Week 1** : morning brief only (HEARTBEAT.md 07:00 trigger)
2. **Week 2** : add evening capture
3. **Week 3** : add inbox capture during sessions
4. **Month 2+** : add weekly review and project-specific notes


Gradual buildup means your vault stays useful instead of overwhelming.


Obsidian works best because of the Local REST API plugin. Notion has a native API you can use similarly — OpenClaw can write to Notion pages via the Notion API skill. Roam Research is more complex. If you use Apple Notes or Bear, you'll need a workaround via Shortcuts or a local script. Obsidian is the path of least friction.


The Local REST API only works when Obsidian is open. If it's closed, the API call fails and the agent logs the failure but does not lose the note — it queues it in SOUL.md context. To avoid this entirely, run Obsidian as a background process on startup (Mac: add to Login Items; Windows: add to Task Scheduler). Alternatively, use Blink Claw managed hosting with a cloud sync tool so the vault is always accessible.


Plain text notes are tiny. 365 daily notes + 52 weekly reviews + 1,000 captured notes ≈ under 5MB. Even with embedded images or code blocks, most active Obsidian users stay under 100MB after years of use. Storage is not a concern.


Yes, with the right skill. The community` ObsidianVector` skill embeds your notes and enables semantic search. Your agent can answer "what were my thoughts on the project from two weeks ago?" without knowing the exact filename. This requires running the indexing step once when you set it up. See the ClaWHub skill page for setup instructions.
