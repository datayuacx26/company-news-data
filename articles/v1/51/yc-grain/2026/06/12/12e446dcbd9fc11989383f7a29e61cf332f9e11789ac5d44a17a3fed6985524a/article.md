---
schema_version: "1.0.0"
document_id: "12e446dcbd9fc11989383f7a29e61cf332f9e11789ac5d44a17a3fed6985524a"
company_key: "yc-grain"
company: "Grain"
source_id: "yc-grain-news-import-81739ddbe75a"
canonical_url: "https://grain.com/blog/ai-workflows-grain-claude"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-09T22:47:29.145404+00:00"
fetched_at: "2026-08-09T22:47:30.801934+00:00"
content_hash: "sha256:fe1617282470036069e5ca03fcfdef16567c6223344b9fc9a535226f04f5b9ff"
---

# 4 AI Workflows Built With Grain + Claude (And How to Copy Them)

*How three Grain leaders use the new Claude connector to run meeting prep, daily engineering briefs, knowledge graphs, and Slack alignment workflows. With folder structures, exact prompts, and the verbatim quotes from each speaker.*


## TL;DR


- Grain's new Claude connector lets Claude pull meeting transcripts, notes, action items, and clips on demand. No copy-pasting.
- Mike Adams (co-founder) runs an AI knowledge vault: three folders (` Raw` ,` Wiki` ,` Output` ) plus a` CLAUDE.md` file. It generates sales decks, marketing assets, and product strategy from a knowledge graph built out of his meetings.
- Jeff Whitlock (CEO) replaced two of five weekly standups with a scheduled Claude task that posts an engineering daily brief to his Slack DMs.
- Ben DeMordaunt (Head of Ops) built a daily meeting prep skill live in about 10 minutes. It pulls Google Calendar, searches Grain for past recordings with the same attendees, and posts a brief to Slack every morning.
- Mike also demoed a trigger-based Slack alignment workflow that combines Grain MCP, Slack MCP, Amplitude MCP, and Figma in a single Claude prompt.
- Thomas Tabi (Customer Success Lead) closed the session with the most important reminder: *"I'm not technical, but I get Claude to do whatever I want."* You don't need an engineering background to build any of these.
- The pattern across all four: output lives in tools where work already happens (Slack, Linear, local files), meetings are the context layer, and every workflow started simple before it got powerful.


## Why most AI rollouts stall


Most AI rollouts impress in the demo and then quietly disappear from your weekly routine. The problem usually isn't the tool. It's that nobody wired it into the work that actually happens every day.


Last week, three of us at Grain spent 30 minutes walking through the AI workflows we use every week. Not demos. Not hypotheticals. The ones running right now.


- **Mike** showed how he built a personal knowledge graph from his meetings.
- **Jeff** showed how he replaced two of five weekly standups with a daily brief that runs in Claude Cowork.
- **Ben** built a daily meeting prep skill live, in about ten minutes.


This post walks through all four with enough detail that you can copy any of them this week.


### What is the Grain Claude connector?


The Grain Claude connector is a one-click integration that lets Claude access your Grain meeting data through MCP (Model Context Protocol). Once installed, Claude can pull transcripts, notes, action items, and clips from any meeting you have access to in Grain. You install it once in Claude's connector directory, sign in with your Grain account, and Claude reads your meetings on demand inside any chat, skill, or scheduled task.


It's the thing that makes the four workflows below possible without manual exports.


## Why meetings are the right context layer


Before we walk through the workflows, a quick note on why all four start with meetings.


Ben put it cleanly:


> "The challenge with one-off tasks and workflows is they're one-off. They don't have the context that we sometimes need. Whereas when we add that context layer, which we believe comes from meetings, ... we're unable to unlock even more richer content."


Mike said something similar, but with a longer arc:


> "I started another company eight years ago and had this vision ... meeting data is the most underutilized asset that companies are sleeping on. This was in 2018. And now, with large language models, that original vision is finally being realized in ways I didn't even imagine were possible."


The practical version: customer pushback, internal decisions, what someone actually committed to in standup. Most of that lives in calls. Almost none of it makes it into Notion, Linear, or HubSpot. Agents that can read your meetings stop guessing.


## Workflow 1: Mike's AI knowledge vault


**What it is:** A Claude Code workflow that ingests meeting transcripts and documents into a personal knowledge graph, then uses that graph as context to generate work products like sales decks, marketing assets, and strategy memos.


**Inspired by:** A tweet from Andrej Karpathy about treating Claude Code as a knowledge graph.


### The folder structure


```text
Vault/
├── CLAUDE.md           # tells Claude how to traverse the vault
├── Raw/                # dump everything here (meeting transcripts, docs, notes)
├── Wiki/               # Claude organizes Raw into a queryable knowledge base
└── Output/             # Claude generates work products using the Wiki
```


Three folders. One file. That's the whole foundation.


### How it runs


1. A small server listens for **Grain webhooks** and drops new meeting transcripts into` Raw/` automatically.
2. Mike runs a **compile skill** in Claude Code. It processes new content in` Raw/` , organizes it into the` Wiki/` , and connects new topics to existing ones.
3. When Mike needs to build something, he asked Claude to use the Wiki/ as context. Claude saves the output to Output/.


He uses **Claude Code hooks** (pre-tool-use, post-tool-use) so Claude reads the` Wiki/` at the start of every new session. The master index of the` Wiki/` is 56 lines long. That's all Claude needs to know how to traverse the rest.


### What it produces


The slides Ben presented during the webinar came out of this workflow. So did the sales deck Mike was about to present to a prospect an hour after the session ended.


Mike's framing:


> "This whole thing started really, really simple. It started with just a CLAUDE.md file. It basically has three folders. Raw, Wiki, and Output. The Wiki is the knowledge graph that ultimately results from all of that."


His next step is a team-level version: a shared voice-of-the-customer vault built from sales calls, intercom messages, support emails, and CRM data.


### Why this is worth copying


It scales with your meetings. The more calls you have, the better the` Wiki/` gets. The better the` Wiki/` gets, the better every downstream output becomes. No manual prompt engineering after setup.


## Workflow 2: Mike's Slack alignment workflow


**What it is:** A trigger-based Claude workflow that combines Grain MCP, Slack MCP, Amplitude MCP, and Figma to resolve internal disagreements with the right meeting clip, the right product data, and the right design artifact.


Mike used this the day before the webinar to resolve a real misalignment with his team.


### The five-step flow


1. Mike opened the Slack thread where there was a disagreement. He asked Claude to find the meeting where the original decision was discussed.
2. Claude searched Grain via MCP, found the meeting (three months old), and **created a clip** of the relevant moment.
3. Mike asked Claude to back the point up with product data. Claude queried **Amplitude via MCP** and pulled the relevant numbers.
4. Mike asked Claude to check the related Figma files for additional context.
5. Claude drafted the Slack reply with the clip embedded, the product data summarized, and the Figma references linked.


Mike's framing:


> "It went through, did all of that work, analyzed the data, and then generated some reports. Where should all of this work go? Should we go put it in the Slack channel? Should we send a DM? Now that we have a strong conclusion, where do we want to close this workflow loop?"


### Why this is worth copying


Most teams have these moments every week. Disagreements that would resolve instantly if someone could find the right meeting clip, the right metric, and the right design artifact. That used to take 30+ minutes of digging across tools. With the right MCPs connected, it takes a single prompt.


## Workflow 3: Jeff's Engineering Daily Brief


**What it is:** A scheduled Claude task that pulls Linear, Slack, and Grain standup transcripts every morning to produce an engineering daily brief delivered as a Slack DM canvas.


Jeff built this in **Claude Cowork** . It replaced two of his five weekly standups.


### What it pulls


- **Linear:** daily ticket movement (started, completed, blocked)
- **Slack:** engineering channel discussions from the last 24 hours
- **Grain:** transcripts from the three standups that still happen
- **Persistent markdown file:** the previous day's brief, so context carries forward


### What it produces


- A scannable summary of what moved, what's stuck, and what's waiting on a decision
- Accountability flags where commitments and delivery diverge. Not punitive. Jeff explicitly framed it that way.
- Positive shout-outs for things shipped
- A pinned canvas in his Slack DM with the full long-form brief
- A short summary DM that links to the canvas


Jeff's framing:


> "I think a lot of times in companies, leaders use standups as a way to serve themselves and not necessarily serve the team. At Grain, for example, we only have three standups a week. And this is a way for me to keep up with what's going on without necessarily having a daily 30-minute meeting just to meet my needs."


### Why this is worth copying


Daily standups are often a tax. A leader-targeted daily brief, generated automatically from data you already have, gives the leader what they need without forcing the whole team into a meeting. Jeff has **about 12 recurring tasks running** in Claude Cowork. This is one of them.


## Workflow 4: Ben's Daily Meeting Prep skill


**What it is:** A Claude Cowork skill that runs every morning, pulls today's Google Calendar, searches Grain for past recordings with the same attendees, and posts a structured prep brief to a private Slack channel.


**Build time:** About 10 minutes, live on screen during the webinar.


### The prompt Ben used to build it` ‍
`


```text
Build a daily meeting prep skill that runs each morning and does the following:


- Pulls today  's meetings from my Google Calendar
- Filters for events with external attendees
- For each meeting, searches Grain for past recordings with the same attendees
- Creates clips of relevant moments from those past calls
- Writes a structured brief per meeting: themes, action items, follow-ups,
one-sentence prep notes, past calls referenced
- Posts the full brief to my private Slack channel as a daily message
- Keeps each meeting in its own clearly separated section
```


Claude Cowork asked clarifying questions (which Slack channel? what time? how to format clips?). Ben answered them inline. The skill saved. Now it can run every morning.


Ben's framing:


> "Honestly, I iterated to this prompt with Claude itself. I didn't even write all this. I'm just giving structure for how simple it is to build one of these skills in Claude Cowork."


### Why this is worth copying


Meeting prep is one of the highest-ROI uses of meeting context. If you have a 9 AM call with a customer you spoke to two months ago, you want the relevant clips, the open action items, and the last commitment surfaced before you join. This skill does that automatically.


## The pattern across all four


Workflow Trigger Context source Output destination


Mike's AI knowledge vault New meeting webhook Grain transcripts Local file in` Output/`


Mike's Slack alignment Manual prompt Grain + Slack + Amplitude + Figma Slack thread


Jeff's daily brief Scheduled (daily AM) Linear + Slack + Grain standups Slack DM canvas


Ben's meeting prep Scheduled (daily AM) Google Calendar + Grain Slack private channel


Three things they share:


1. **Output lives in tools where work already happens.** Slack, Linear, local files. No new app to open.
2. **Meeting context is in every one.** Even the workflows that pull from Linear, Slack, or Amplitude all use Grain transcripts as the foundation.
3. **They all started simple.** Mike's vault "started with just a CLAUDE.md file." Ben built his in 10 minutes. Jeff iterated his over weeks.


## The 4 levels of AI execution (Jeff's framework)


Jeff laid out four levels of AI execution during the session. They map directly to how these workflows evolved.


1. **One-off tasks.** "Help me draft this follow-up email." Useful, but doesn't compound.
2. **Reusable skills.** Write a clear instruction set once, save it, run it on a new input.
3. **Scheduled workflows.** A skill that runs on a cadence (daily, weekly) without you triggering it.
4. **Trigger-based workflows.** A skill that runs when something happens. A meeting ends. A ticket moves. A webhook fires.


Most teams are stuck at level 1. The leverage starts at level 2.


Jeff's daily brief is a level 3. Ben's meeting prep is a level 3. Mike's vault is a level 4 (Grain webhooks trigger the sync). Mike's Slack alignment workflow is somewhere between 1 and 4 depending on how he runs it.


You don't need to start at level 4. Levels 2 and 3 already create leverage most teams don't have.


## Tabi's reminder: you don't need to be technical


The most quietly important moment of the session came at the end, from Thomas Tabi, Grain's Customer Success Lead:


> "I am not that technical, but I get Claude to do whatever I want. I just get in there, tell it what I want, and then try it out. So please don't feel like you need to be extremely technical to do anything. The key is: know what you want, then explore it in Claude, and you'll build a skill for you. No intimidation. Just tell it what you want."


Every workflow above involves things that sound technical. Webhooks. MCPs. Hooks. Scheduled tasks. Folder structures.


None of them required Tabi-level writing skill in those topics to start. Mike's vault started with a single` CLAUDE.md` file. Ben built his skill in Claude Cowork by writing a plain-English prompt that Claude turned into a working skill. Jeff iterated his daily brief by reading the output, telling Claude what was wrong, and letting Claude fix it.


If you can explain what you want in plain language, you can build any of these. That's the practical version of Tabi's point.


## What to try this week


Pick one workflow. Don't try to build all four at once.


- **New to Claude + Grain?** Start with **Ben's daily meeting prep skill** . Simplest setup, broadest payoff. You'll have it running in 15 minutes.
- **Leader trying to reduce standups?** Start with **Jeff's daily brief** . First version doesn't have to read all your sources. Start with Linear + Slack, add Grain when you're ready.
- **Power user?** Steal **Mike's vault structure** . Three folders, a` CLAUDE.md` , a compile skill. Build the rest as you go.
- **Not technical?** Re-read Tabi's quote above. Then start anyway.


## Frequently asked questions


### What is the Grain Claude connector?


The Grain Claude connector is a one-click integration that lets Claude access your Grain meeting data through MCP. Once installed, Claude can pull transcripts, notes, action items, and clips on demand from any meeting you have access to in Grain.


### What can the Grain MCP do?


The Grain MCP supports fetching meetings, transcripts, notes, and action items. It can also create clips of specific moments and generate playlists that group multiple meetings under a single shareable link.


### Do I need an enterprise Claude plan to use the Grain connector?


No. You can use the Grain MCP on any Grain plan. For scheduled and trigger-based workflows, you'll want Claude Pro, Team, or Enterprise so you have access to Claude Cowork.


### What is Claude Cowork?


Claude Cowork is Claude's feature for scheduled and recurring tasks. You define a task once (e.g., "every morning, build me a meeting prep brief"), and Claude runs it on a schedule. Both Jeff's daily brief and Ben's meeting prep skill run in Claude Cowork.


### How do I make a Claude workflow run automatically when a meeting ends?


Use Grain webhooks (via the Grain API) or the Grain Zapier integration. Both expose triggers like "meeting finished" and "meeting uploaded." Connect that trigger to a Claude workflow via the Zapier SDK or a small custom server, and the workflow runs the moment a meeting wraps.


### Can the Grain Claude connector access my coworkers' private meetings?


No. Each person connects the Grain MCP to their own Grain account. The connector only accesses meetings that you personally have access to in Grain.


### What's the difference between a scheduled workflow and a trigger-based workflow?


Scheduled workflows run on a fixed cadence (e.g., every weekday at 8 AM). Trigger-based workflows run when an event happens (e.g., a meeting ends, a ticket moves to "complete"). Scheduled workflows are easier to build. Trigger-based workflows respond faster to real-world events.


### How long does it take to build one of these AI workflows?


Ben built his daily meeting prep skill live during the webinar in about 10 minutes. Jeff's engineering daily brief took longer because he iterated on the structure over several weeks. Mike's knowledge vault started as a single` CLAUDE.md` file and grew over months.


### Do I need to be technical to build AI workflows with Grain and Claude?


No. As Thomas Tabi (Grain's Customer Success Lead) put it during the session: "I am not that technical, but I get Claude to do whatever I want." If you can explain what you want a workflow to do in plain English, Claude can help you build it. You don't need to write code.


## Resources


- [Watch the full session (60 min)](https://grain.com/share/recording/8421e934-e8ce-4528-acc3-b1e406152246/EeaV1prG2YXRvVqYw3tX4dIwZ5WZP5DX4xUEAsz8?tab=transcript)
- [Connect Grain to Claude](https://support.grain.com/en/articles/14811968-how-to-use-the-grain-mcp-server)


If you build one of these for yourself, we'd love to hear about it. We've started spotlighting customers who are building real workflows on top of Grain. Reach out.
