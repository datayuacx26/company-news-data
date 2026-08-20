---
schema_version: "1.0.0"
document_id: "b3a37cb8609c032cf3bea049e0f4731c53e2532199c80dac93a47b0f1f08a0a9"
company_key: "yc-bits-2"
company: "Bits"
source_id: "yc-bits-2-news-import-8ae56e6aae1c"
canonical_url: "https://klausai.com/blog/how-to-give-your-openclaw-agent-a-personality/"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-24T21:07:46.493176+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:2f92d78daea8f6f04d84f14f4a4f946fd5cfdf8237327f4e139849f42ce77f67"
---

# How to Give Your OpenClaw Agent a Personality (SOUL.md and Bootstrap Files)

# How to Give Your OpenClaw Agent a Personality (SOUL.md and Bootstrap Files)


When we onboard a new Klaus customer, we don’t start with skills or integrations. We start with the bootstrap files. Specifically, we start with SOUL.md. That file determines whether the agent sounds like a professional researcher, a casual assistant, or a confused intern that keeps apologizing for things it didn’t do.


Most people skip these files entirely. They run through the bootstrap process, answer a few questions, and move on. Then they spend weeks wondering why their agent feels generic. The answer is almost always the same: the bootstrap files are either empty or full of defaults that don’t match how you actually want to work.


Here’s what each file does, what to put in them, and what happens when you get it wrong.


## What Are Bootstrap Files?


Bootstrap files are plain Markdown files that load into every conversation automatically. They’re the first thing your agent reads before you say anything. Whatever you write in these files becomes part of how the agent thinks, responds, and behaves for the entire session.


On self-hosted OpenClaw, they live in` ~/.openclaw/workspace/` . On Klaus, you can[edit SOUL.md directly in your dashboard](https://klausai.com/klaus/files?path=/root/.openclaw/workspace/SOUL.md) without touching a terminal.


There are[seven bootstrap files](https://docs.openclaw.ai/concepts/context) that OpenClaw injects by default:


File Purpose When It Loads


**SOUL.md** Personality, values, tone, boundaries Every session


**AGENTS.md** Operating instructions, procedures, safety rules Every session


**USER.md** Your name, timezone, preferences, work context Every session


**IDENTITY.md** Agent’s name, emoji, vibe Every session


**TOOLS.md** Notes on which tools exist and when to use them Every session


**HEARTBEAT.md** Lightweight checklist for routine/scheduled runs Every session


**BOOTSTRAP.md** First-run setup Q&A (deleted after completion) First session only


Two character limits govern these files. The[per-file cap is 20,000 characters](https://docs.openclaw.ai/concepts/context) (You can configure this number). The total cap across all bootstrap files is 150,000 characters. When a file exceeds its limit, the content gets silently truncated. The agent behaves as if those instructions aren’t there.


You can check what’s actually loading by running` /context list` in any OpenClaw session. It shows every injected file, its size, and whether anything was cut.


## SOUL.md: Who Your Agent Is


SOUL.md defines your agent’s personality, values, and boundaries. It is not a list of instructions for what to do. It’s a description of who the agent is.


The[official template](https://docs.openclaw.ai/reference/templates/SOUL) has four sections:


1. **Core Truths** : foundational principles the agent follows
2. **Boundaries** : ethical guardrails and things the agent won’t do
3. **Vibe** : personality tone and communication style
4. **Continuity** : how the agent maintains identity across sessions


The template’s philosophy is clear: “Be genuinely helpful, not performatively helpful.” Skip the corporate filler. Have opinions. Earn trust through competence, not pleasantries. The template explicitly invites evolution: “This file is yours to evolve. As you learn who you are, update it.”


That’s a good default. But a default is all it is.


### What Actually Works in SOUL.md


The difference between a SOUL.md that changes behavior and one that gets ignored comes down to specificity.[Research on AI agent persona design](https://thenewstack.io/how-to-define-an-ai-agent-persona-by-tweaking-llm-prompts/) confirms that specificity and defined boundaries are the two biggest factors in effective personality configuration. Vague instructions produce vague behavior.


Compare these two lines:


- “Be concise.” (vague, gets ignored after a few turns)
- “Reply in 2-3 sentences unless I ask for detail. If a question needs a longer answer, start with the short version and expand below.” (specific, consistently followed)


Or these:


- “Be professional.” (means nothing concrete)
- “Use a direct, matter-of-fact tone. No emojis. No exclamation points. Address me by first name.” (the agent actually does this)


The community has recognized this need for structured personality configuration. The[soul.md framework](https://github.com/aaronjmars/soul.md) offers three approaches to building agent personalities: interview mode (an agent questions you), data analysis (feed in your writing and it extracts patterns), and manual templates. It produces separate files for identity, voice, and operating modes. The project demonstrates how much demand exists for going beyond OpenClaw’s defaults.


We’ve iterated on SOUL.md files across our customer base, and the practical advice is consistent: start with 10-15 lines. Add rules only when you notice the agent doing something you don’t want. Most effective SOUL.md files land somewhere between 50 and 150 lines. Longer files waste context window. Shorter files leave too much behavior undefined.


## AGENTS.md: How Your Agent Operates


If SOUL.md is personality,[AGENTS.md is procedure](https://docs.openclaw.ai/concepts/agent-workspace) . It tells the agent how to behave: what rules to follow, what memory management to use, what safety policies to enforce, what scope boundaries to respect.


The distinction matters. “Never share financial data with third parties” is a SOUL.md boundary. “Before answering financial questions, check the latest portfolio spreadsheet in Google Drive” is an AGENTS.md procedure.


What belongs in AGENTS.md:


- **Session startup sequences** : what to read first, what to check
- **Memory management rules** : when to write to daily files, when to update MEMORY.md
- **Safety policies** : what actions require confirmation, what’s off-limits
- **Scope boundaries** : what topics to engage with, what to redirect
- **Escalation rules** : when to ask you instead of acting autonomously


### Keeping AGENTS.md and SOUL.md Separate


Mixing personality with procedures makes both harder to maintain. The agent reads both files every session. If AGENTS.md has personality rules scattered through procedures, and SOUL.md has operational steps mixed into identity statements, you’ll end up editing both files every time you want to change one thing.


The rule of thumb: if it’s about *who* the agent is, it goes in SOUL.md. If it’s about *what* the agent does, it goes in AGENTS.md.


At Klaus, we keep these files cleanly separated for every customer. When someone asks “why does my agent keep doing X?”, the first place we check is whether the instruction is in the right file. About half the time, the fix is just moving a line from one file to the other.


## USER.md and IDENTITY.md: The Other Half


USER.md stores information about you: your name, timezone, communication preferences, work context, and anything else the agent should know about the person it’s talking to.[It’s your profile](https://docs.openclaw.ai/concepts/agent-workspace) , loaded fresh every session.


IDENTITY.md stores the agent’s own identity: its name, emoji, and “vibe.” This file is[generated during the bootstrap process](https://docs.openclaw.ai/start/bootstrapping) , when BOOTSTRAP.md walks you through a Q&A that populates both USER.md and IDENTITY.md.


Why USER.md matters more than most people think: if USER.md says you prefer direct communication and short messages, the agent adapts its style automatically. If it says you work in real estate, the agent defaults to industry-relevant context when you ask open-ended questions. If it says your timezone is ET, the agent schedules things in your local time without you having to specify.


An empty USER.md means the agent treats you like a stranger every session. It doesn’t know your timezone, your tools, or how you prefer to communicate. Every conversation starts with a blank slate instead of building on what’s already known.


The bootstrap process is designed to fill these files automatically. But most people rush through it or skip questions. If your USER.md is thin, you can edit it directly. Open the file, add what matters, and the agent will pick it up next session.


## TOOLS.md: What Your Agent Knows About Its Tools


[TOOLS.md documents which tools your agent has access to](https://docs.openclaw.ai/concepts/agent-workspace) and any usage notes specific to your setup. Without it, the agent discovers tools through trial and error.


This file is often overlooked. On Klaus, we pre-populate TOOLS.md with entries for every integration the customer has enabled, but even then, people add tools and forget to update the file. The result: the agent has Exa configured for AI-native web search but uses a generic search tool instead. Or it has Apollo for lead enrichment but doesn’t know when to reach for it versus running a manual search.


Keep TOOLS.md short: tool name, what it does, when to use it. Not a manual. A cheat sheet. Ten lines is often enough.


Example:


```text
- **Exa**: AI-native search. Use for company research, market analysis, finding recent articles. Prefer over generic web search.
- **Apollo**: Lead enrichment and contact data. Use when I ask about a person or company's contact info.
- **AgentMail**: Dedicated email inbox. Use for sending outbound emails. Never use my personal inbox.
```


## Common Mistakes That Make Bootstrap Files Useless


### Leaving Everything at Defaults


The[bootstrap process](https://docs.openclaw.ai/concepts/agent) generates starter files. They’re generic by design because the process doesn’t know anything about you yet. Most people never edit them beyond that first run. The agent works, technically, but it sounds like every other OpenClaw agent with factory settings.


### Putting Everything in One File


We see this constantly: a 15,000-character AGENTS.md with personality rules, procedures, tool notes, and memory rules crammed together. When something doesn’t work, there’s no way to tell which instruction is causing the problem. Changing one behavior means scrolling through pages of unrelated content to find the right line.


### Wrong File, Wrong Purpose


Detailed task instructions in SOUL.md (those belong in a skill). Personality rules in AGENTS.md (move them to SOUL.md). Running context in AGENTS.md (use MEMORY.md or daily files instead, covered in our[memory configuration guide](https://klausai.com/blog/openclaw-memory-how-to-make-your-agent-actually-remember/) ).


### Exceeding Character Limits Silently


Bootstrap files[truncate at 20,000 characters per file](https://docs.openclaw.ai/concepts/context) without warning by default. If your SOUL.md is 25,000 characters, the last 5,000 disappear. If the most important rules are at the bottom, the agent never sees them. Run` /context list` to check.


### File Path Errors


Bootstrap files must be in the workspace directory. A SOUL.md in the wrong folder[gets silently ignored](https://github.com/openclaw/openclaw/issues/53547) . The agent starts without it and you might not notice for days. If` /context list` doesn’t show SOUL.md, check that the file is in the right location.


## Frequently Asked Questions


### Do I need all seven bootstrap files?


No.[Blank files are skipped automatically](https://docs.openclaw.ai/concepts/agent) . Start with SOUL.md and AGENTS.md. Add USER.md when you want the agent to know your preferences. Add TOOLS.md when the agent keeps reaching for the wrong tool. You can always add files later.


### Can I reset my agent’s personality?


Yes. Edit or replace SOUL.md. The change takes effect on the next session. The agent has no memory of its previous personality unless you saved it elsewhere. It’s just a Markdown file.


### What’s the difference between SOUL.md and a system prompt?


SOUL.md becomes part of the system prompt.[OpenClaw assembles the full prompt](https://docs.openclaw.ai/concepts/context) from all bootstrap files, tool schemas, and skills metadata at the start of each session. SOUL.md is one input, not the entire prompt. Other bootstrap files, your installed skills, and the tools available to the agent all contribute to what the model receives.


### How do I test whether my bootstrap files are loading?


Run` /context list` in any session. It shows every injected file, its character count, and whether anything was truncated. If a file doesn’t appear, check that it exists in your workspace directory and isn’t blank.


## Key Takeaways


- Bootstrap files define your agent’s identity and behavior before a conversation starts. SOUL.md handles personality; AGENTS.md handles procedures. Keep them separate.
- Seven files load by default, but start with two: SOUL.md and AGENTS.md. Add USER.md, TOOLS.md, and others when you have a specific need for them.
- The per-file character limit is 20,000; the total limit across all bootstrap files is 150,000. Exceeding these causes silent truncation with no warning.
- Specific instructions beat vague ones. “Reply in 2-3 sentences unless asked for detail” changes behavior. “Be concise” doesn’t.
- Run` /context list` to verify what your agent actually sees. If a file isn’t showing up, check the path. If content is getting cut, trim the file.


---


Klaus ships with bootstrap files pre-configured for your use case: a SOUL.md tuned for professional communication, AGENTS.md with memory and safety policies, and TOOLS.md that matches your integrations. If you’d rather skip the configuration and start with something that works, sign up at[klausai.com](https://klausai.com/) .


## Sources


- OpenClaw. “Agent Bootstrapping.”[docs.openclaw.ai/start/bootstrapping](https://docs.openclaw.ai/start/bootstrapping)
- OpenClaw. “SOUL.md Template.”[docs.openclaw.ai/reference/templates/SOUL](https://docs.openclaw.ai/reference/templates/SOUL)
- OpenClaw. “Context Management.”[docs.openclaw.ai/concepts/context](https://docs.openclaw.ai/concepts/context)
- OpenClaw. “Agent Runtime.”[docs.openclaw.ai/concepts/agent](https://docs.openclaw.ai/concepts/agent)
- OpenClaw. “Agent Workspace.”[docs.openclaw.ai/concepts/agent-workspace](https://docs.openclaw.ai/concepts/agent-workspace)
- aaronjmars. “soul.md: Build a personality for your agent.”[github.com/aaronjmars/soul.md](https://github.com/aaronjmars/soul.md)
- OpenClaw. “Bug: Bootstrap files not loaded from workspace.”[github.com/openclaw/openclaw/issues/53547](https://github.com/openclaw/openclaw/issues/53547)
- The New Stack. “How To Define an AI Agent Persona by Tweaking LLM Prompts.”[thenewstack.io](https://thenewstack.io/how-to-define-an-ai-agent-persona-by-tweaking-llm-prompts/)
