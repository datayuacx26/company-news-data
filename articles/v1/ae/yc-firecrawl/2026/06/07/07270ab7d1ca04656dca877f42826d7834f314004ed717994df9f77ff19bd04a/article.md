---
schema_version: "1.0.0"
document_id: "07270ab7d1ca04656dca877f42826d7834f314004ed717994df9f77ff19bd04a"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/best-hermes-plugins"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:35.258497+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:69239cb672bc25bc2496aa8810ab37f9b81cdb692640430d345fae561929768e"
---

# 9 Best Hermes Tools and Plugins to Install in 2026

## TL;DR: Best Hermes Plugins


Plugin What it does


Firecrawl Live web context: search, scrape, crawl, browser rendering


Composio Connect Connects Hermes to 1,000+ SaaS apps via OAuth


Langfuse Observability Traces every turn, tool call, latency, and cost


Honcho Memory Builds a model of how you think over time


Hindsight Memory Knowledge-graph memory with precise entity recall


Kanban Dashboard Multi-agent task board that survives restarts


Google Meet Joins calls, transcribes, and writes the notes


Disk Cleanup Clears agent-generated temp files safely


Self-Evolution Optimization loops for improving skills and prompts


---


Hermes Agent from Nous Research is a capable harness on its own, but a harness is only half the story. Out of the box it remembers across sessions, runs on a schedule, and works on your real files. What it cannot do by default is reach the apps where your work lives, give you a clear view of what it did, or remember you the way a good assistant should. That gap is what plugins fill.


The Hermes ecosystem matured fast through the first half of 2026. There are now built-in plugins that ship with the agent, memory providers you can swap in, skills in the shared[AgentSkills format](https://agentskills.io/specification) , and third-party MCP integrations that connect Hermes to the outside world. If you came from OpenClaw, most of this will feel familiar: the[OpenClaw vs Hermes comparison](https://www.firecrawl.dev/blog/openclaw-vs-hermes) covers how the two agents line up on architecture, memory, and security.


Nous Research keeps pushing the harness itself in parallel. In late June it shipped Mixture-of-Agents presets as virtual models, which it benchmarks above the gated frontier models, a signal that Hermes is worth the time you put into setting it up well:


I have been running Hermes daily, and the plugins below are the ones that earned a permanent spot. These are the best Hermes plugins I would install first on a fresh setup, ordered roughly by how often I reach for them.


## What are Hermes plugins?


"Plugin" is a loose word in the Hermes world because the agent accepts capabilities in a few different shapes:


- **Built-in plugins** ship with Hermes and turn on with one command (` hermes plugins enable <name>` ). Disk Cleanup, Langfuse, and Google Meet are examples.
- **Skills** are` SKILL.md` files in the AgentSkills format. They teach Hermes one task and install from sources like[skills.sh](https://skills.sh/) or the official Nous collections.
- **Memory providers** replace the default file-based` MEMORY.md` and` USER.md` with something more capable. Honcho and Hindsight are the two strong options.
- **Third-party MCP integrations** register a whole set of tools at once. Composio is the headline example, exposing over a thousand apps through a single endpoint.
- **External frameworks** run alongside Hermes rather than inside it, like the Self-Evolution engine that optimizes your skills.


The list below mixes all five. Inside a real session you stop noticing the distinction, which is the point, but it helps to know the install path differs by type.


## How to install Hermes plugins


There is no single install command because the types differ. The four patterns cover almost everything:


```text
# Built-in plugins
hermes   plugins   enable   <  plugin-nam  e  >


# Skills (AgentSkills SKILL.md format)
hermes   skills   install   official/  <  categor  y  >  /  <  skill-nam  e  >


# Memory providers
hermes   memory   setup


# Remove a skill
hermes   skills   uninstall   <  skill-nam  e  >
```


Third-party MCP integrations like Composio install through their own path, usually by pasting a URL into a Hermes chat session and letting the agent wire it up. Each section below lists the exact command. The full command reference lives at[hermes-agent.nousresearch.com/docs/reference/cli-commands](https://hermes-agent.nousresearch.com/docs/reference/cli-commands) .


## 1. Firecrawl


**An agent is only as smart as the context it has. Firecrawl gives Hermes live web context.**


Hermes ships with` web_search` and` web_extract` , but without a real backend` web_extract` falls back to plain HTTP and returns empty content on any JavaScript-rendered page. Firecrawl is the default backend that fixes this. Set one environment variable and both tools route through Firecrawl: search returns clean markdown with page content instead of bare snippets, and extract uses real browser rendering so JS-heavy and protected pages actually come back with content.


The reach goes past single pages. Firecrawl covers crawling, so Hermes can pull an entire docs site or a competitor's whole blog in one job, and the same API key drives a cloud browser backend for interactive sessions. On a server-resident agent running research on a schedule, this is the difference between an agent that reliably reads the web and one that quietly returns nothing.


- ` web_search` : Live search that returns markdown page content, not just titles and descriptions
- ` web_extract` : Full-page markdown with real browser rendering, summarized above 5,000 characters
- ` crawl` : Pull every page under a domain or path in a single natural-language request
- ` browser` : Firecrawl cloud browsers for login flows, forms, and dynamic pages, no local Chromium needed
- ` include_domains` /` exclude_domains` : Scope research to the sources you trust


**Install:**


```text
# ~/.hermes/.env
FIRECRAWL_API_KEY  =  fc-YOUR-API-KEY
```


Hermes auto-detects the key and routes` web_search` and` web_extract` through Firecrawl with no further config. Run` hermes tools` to confirm the active backend or pin it explicitly. Grab a free key (1,000 credits per month) at[firecrawl.dev/app/api-keys](https://firecrawl.dev/app/api-keys) .


**Example:**


```text
"Every Monday, search for new LLM agent papers from the past week and send a summary to my Telegram"
"Crawl the Stripe API docs and list every endpoint that supports idempotency keys"
"Extract the pricing page at example.com and alert me when the Pro tier changes"
```


**Honest take:** This is the first thing I set up on any Hermes install, because unattended research is the whole reason to run a resident agent and it falls apart without reliable web access. The full breakdown of how the tools differ is in the[Hermes web search guide](https://www.firecrawl.dev/blog/hermes-web-search) . The one caveat: heavy crawling burns credits, so scope your jobs rather than crawling whole domains when you only need a section.


**Cons:** You can try the endpoints without a key, but ongoing use needs one and consumes credits on large crawls. The free tier covers substantial testing.


When Firecrawl is the web layer under an agent, builders tend to stick with it.[Peter Steinberger](https://x.com/steipete) , founder of[OpenClaw](https://openclaw.ai/) , put it plainly:


Full reference at[docs.firecrawl.dev](https://docs.firecrawl.dev/) .


## 2. Composio Connect


**Composio connects Hermes to 1,000+ SaaS apps through a single MCP endpoint with managed OAuth.**


Out of the box, Hermes can only reach what the core system natively supports. Composio Connect opens the door to Slack, Gmail, Notion, Linear, Jira, HubSpot, GitHub, Google Calendar, Salesforce, and hundreds more. Authentication is OAuth, so you authorize each app once and never paste an API key. The tool router exposes everything through one MCP endpoint, which keeps your Hermes config clean instead of accumulating a server entry per app.


The practical payoff is workflows that span apps Hermes has no native connector for: turn a Slack message into a Linear ticket, pull calendar context before drafting an email, update HubSpot from agent output without opening the CRM.


- ` 1,000+ toolkits` : Slack, Gmail, Notion, Linear, HubSpot, GitHub, and the rest, through one integration
- ` OAuth` : Authorize once per app, no API key management
- ` single MCP endpoint` : One config entry instead of dozens of individual servers
- ` cross-app chains` : Compose actions across services that have no native Hermes connector


**Install:**


Paste the Composio Hermes URL directly into a Hermes chat session:


```text
https://composio.dev/hermes


```


Hermes installs it and walks you through connecting apps over OAuth. No config file editing, no credential copying.


**Example:**


```text
"Turn the last message in #bugs into a Linear ticket with a P1 label"
"Pull today's calendar and draft a prep doc for each meeting in Notion"
"Create a HubSpot deal for every Stripe invoice over 5,000 dollars this week"
```


**Honest take:** This is the fastest way to make Hermes useful at work rather than just smart in a terminal. One install replaces weeks of one-off connector building, which is why Composio recurs across the[best MCP servers for developers](https://www.firecrawl.dev/blog/best-mcp-servers-for-developers) . The thing to know is that your OAuth tokens flow through Composio's infrastructure, so for personal productivity it is a non-issue, but a team handling sensitive data should read the security docs first.


**Cons:** Hosted service, so you depend on Composio's uptime and rate limits. The free tier covers development; production usage moves to paid plans. Some niche apps expose only a subset of their full API through Composio.


Full reference at[composio.dev/hermes](https://composio.dev/hermes) and the toolkit list at[composio.dev/toolkits](https://composio.dev/toolkits) .


## 3. Langfuse Observability


**Langfuse turns Hermes from a black box into a traced, inspectable system.**


The moment you use Hermes for anything serious, you want to know what it actually did. The Langfuse plugin streams Hermes activity into Langfuse's tracing dashboard: turns, LLM calls, tool invocations, latency, token usage, and cost per session. That makes the otherwise-hard questions answerable. Which tool calls really happened, where did the agent spend time, which model calls were expensive, and why did a long autonomous run fail halfway through.


- ` traces` : Full turn-by-turn view of every LLM and tool call
- ` cost tracking` : Token and dollar cost per session, which alone justifies the install for frequent users
- ` latency` : See where time goes across a multi-step run
- ` self-hostable` : Run Langfuse yourself if you do not want traces in a cloud service


**Install:**


```text
pip   install   langfuse
hermes   plugins   enable   observability/langfuse
```


Then add your keys to` ~/.hermes/.env` :


```text
LANGFUSE_PUBLIC_KEY  =  your-public-key
LANGFUSE_SECRET_KEY  =  your-secret-key
LANGFUSE_HOST  =  https://cloud.langfuse.com    # or your self-hosted URL
```


**Honest take:** I would enable this early if you are building workflows or spending real money on model calls. The cost view changes how you think about which tasks to automate. For casual chat-only use it is useful but not urgent.


**Cons:** Adds a small per-turn latency as traces ship to Langfuse. The free tier has trace retention limits. Not worth it for single-user casual setups, but for anything production-facing it is the first plugin I enable.


Full reference at[langfuse.com](https://langfuse.com/) and the Hermes feature docs at[hermes-agent.nousresearch.com/docs/user-guide/features/overview](https://hermes-agent.nousresearch.com/docs/user-guide/features/overview) .


## 4. Honcho Memory Provider


**Honcho replaces Hermes's file-based memory with a system that builds a model of how you think.**


Hermes's default memory is file-based:` MEMORY.md` and` USER.md` that the agent maintains across sessions. It works. Honcho is what you install when "works" is not enough. Instead of storing raw text, it reasons about each conversation turn through a dialectic process, deriving insights about your preferences, habits, communication style, and goals. Those accumulate, so the agent's understanding of you deepens the more you use it, not just because it stores more but because it reasons about what it stores.


It also handles multi-agent isolation properly. Run a coding assistant and a personal assistant as separate Hermes profiles and Honcho keeps their user models apart, so they do not contaminate each other.


- ` dialectic reasoning` : Derives insights from each turn instead of storing raw transcripts
- ` semantic search` : Full search across the accumulated memory history
- ` profile isolation` : Separate user models per Hermes profile
- ` tunable cadence` : Independent knobs for context refresh, dialectic frequency, and reasoning depth


**Install:**


```text
hermes   memory   setup
# select "honcho" from the provider list
```


Add your key, then install the optional skill for full configuration access:


```text
echo   'HONCHO_API_KEY=your-key'   >>   ~/.hermes/.env
hermes   skills   install   official/autonomous-ai-agents/honcho
```


**Honest take:** This is the memory provider for an agent you want to keep for months. The personalization is real, but it is also slow to show value: day-one Honcho knows nothing, and it takes a few sessions of dialectic reasoning before the difference is noticeable.


**Cons:** Requires an API key from[honcho.dev](https://honcho.dev/) . The dialectic reasoning adds per-turn latency, tunable via` dialecticCadence` . More to configure than the default memory, and low immediate payoff.


Full reference at[github.com/plastic-labs/honcho](https://github.com/plastic-labs/honcho) and the Hermes docs at[hermes-agent.nousresearch.com/docs/user-guide/features/honcho](https://hermes-agent.nousresearch.com/docs/user-guide/features/honcho) .


## 5. Hindsight Memory Provider


**Hindsight stores facts and entities as a knowledge graph for precise, structured recall.**


Most memory providers store semantic chunks and retrieve them by similarity. Hindsight does something structurally different: it extracts discrete facts, named entities, and the relationships between them, then stores those as a knowledge graph. Ask about a specific service and Hindsight surfaces facts about that service, not chunks that happened to mention it nearby.


Its` reflect` operation is the unusual part. Periodically it reads across all stored memories, derives higher-level insights, and consolidates related facts, so the knowledge improves over time rather than just growing. And it runs as a local daemon, so nothing has to leave your machine.


- ` knowledge graph` : Entities and relationships, not just chunks
- ` entity queries` : Highest retrieval precision when you ask about a specific thing
- ` reflect` : Periodic synthesis that consolidates and refines stored facts
- ` local daemon` : Runs on-device with no API key for privacy-sensitive setups


**Install:**


```text
hermes   memory   setup
# select "hindsight" from the provider list
# leave the API key blank to run the local daemon
```


**Honest take:** Reach for Hindsight over Honcho when you need exact recall of structured facts, or when the data genuinely cannot leave your machine. For general "remember me over time" personalization, Honcho is the easier starting point.


**Cons:** More complex to configure than Honcho or the default memory. Knowledge-graph extraction adds overhead versus raw semantic storage. Better suited to real privacy or precision needs than casual use.


Full reference at[hermes-agent.nousresearch.com/docs/user-guide/features/memory-providers](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory-providers) .


### A counterpoint worth hearing before you install either


Not everyone keeps a memory provider. A fair number of long-time users try every option and end up back on the built-in` USER` /` MEMORY` /` SOUL` markdown files Hermes ships with. One daily driver, three months in, summed it up on[r/hermesagent](https://www.reddit.com/r/hermesagent/comments/1u8fm0t/three_months_with_hermes_agent_what_i_wish_i_had/) :


> I tried every memory provider connected to Hermes, plus a few open-source ones from GitHub. In the end, I turned them all off. The markdown USER / MEMORY / SOUL system Hermes comes with is probably the best setup. \[...\] Agents don't remember. Agents read.


The built-in system is short, editable by the agent, and easy to inspect and clean by hand, and for a lot of setups that is enough. The honest way to read items 4 and 5: reach for Honcho or Hindsight when you have a specific need the markdown files do not cover (deep personalization, precise entity recall, local-only storage), not by default on day one.


## 6. Kanban Dashboard


**The Kanban Dashboard gives multi-agent work a visual board that survives crashes and restarts.**


Once you are coordinating more than one Hermes worker, a chat transcript stops being a useful way to track what happened. The Kanban Dashboard adds a SQLite-backed task board shared across all profiles on a host. Tasks carry an assignee, optional dependency links, a workspace kind, and a tenant namespace. The board itself has drag-and-drop columns, run history, worker logs, live WebSocket updates, and per-task event attribution.


Because it is SQLite-backed and runs entirely on localhost, tasks survive crashes, restarts, and reboots, and nothing leaves your machine.


- ` SQLite task board` : Persistent tasks shared across profiles on the host
- ` dependencies` : Link tasks so workers run in the right order
- ` live updates` : WebSocket progress without refreshing
- ` orchestrator + worker skills` : Pair with the dashboard for a complete delegation setup


**Install:**


```text
pip   install   'hermes-agent[web,pty]'
hermes   dashboard
```


This starts a local server at` http://127.0.0.1:9119` with the Kanban tab inside. Then load the supporting skills:


```text
hermes   skills   load   devops/kanban-orchestrator
hermes   skills   load   devops/kanban-worker
```


**Honest take:** This earns its place the moment you have agents fanning work out in parallel. For single-task casual use it is overkill, and the value only shows up once a transcript is genuinely too messy to follow.


**Cons:** Overkill for one-off tasks. The standalone daemon mode is deprecated, so run it through the gateway. Worth installing only when you are actually coordinating multiple workers.


Full reference at[hermes-agent.nousresearch.com/docs/user-guide/features/web-dashboard](https://hermes-agent.nousresearch.com/docs/user-guide/features/web-dashboard) .


## 7. Google Meet


**The Google Meet plugin lets Hermes join calls, transcribe them, and write the notes.**


This is one of the most immediately practical plugins for teams. Hermes joins a meeting as a headless participant, transcribes live audio through your chosen speech-to-text provider, and generates artifacts from the session: transcripts, structured notes, and action items. It shows up to the standup, takes notes, and has the action items ready before the call is over.


- ` headless participant` : Joins the call without anyone driving it
- ` six STT providers` : faster-whisper (local, free), Groq, OpenAI Whisper, Mistral, xAI, and a local CLI wrapper
- ` artifacts` : Transcripts, structured notes, and action items from each session
- ` chains to project tools` : Pushes output into Notion or Linear via bundled skills


**Install:**


```text
hermes   plugins   enable   google-meet
```


Configure your speech-to-text provider in` ~/.hermes/config.yaml` :


```text
voice  :
stt_provider  :   whisper   # or groq, openai, faster-whisper
```


**Honest take:** Running local faster-whisper means transcription stays on your machine, which I prefer for internal calls. Just be explicit with your team that an agent is in the meeting, because transcription needs consent.


**Cons:** It is headless and Google Meet-focused, so no native Zoom or Teams support. Transcription quality on fast speakers or heavy accents depends on which STT provider you run.


Full reference at[hermes-agent.nousresearch.com/docs/user-guide/features/overview](https://hermes-agent.nousresearch.com/docs/user-guide/features/overview) .


## 8. Disk Cleanup


**Disk Cleanup clears the temp files Hermes generates, safely, before they pile up.**


This is the boring plugin you will still be running in three months. Agents create a lot of throwaway files: test scripts, scratch outputs, browser profiles, logs, one-off experiments. Left alone they accumulate fast, and you end up spending an afternoon deciding what is safe to delete. Disk Cleanup tracks the temporary files created during agent sessions and gives you slash commands to clear them without guessing.


- ` /disk-cleanup status` : See what has accumulated
- ` /disk-cleanup dry-run` : Preview exactly what would be removed before anything is touched
- ` /disk-cleanup quick` : Remove obvious temp files
- ` /disk-cleanup deep` : Thorough cleanup pass


**Install:**


It ships with Hermes. Enable it with:


```text
hermes   plugins   enable   disk-cleanup
```


**Honest take:** The dry-run mode is the reason to trust it: you always see the full list before anything is removed. Any agent that touches files should have this on from day one.


**Cons:** It only flags files Hermes itself created, so anything you made manually in the same directory is not tracked. It is a cleanup layer for agent artifacts, not a general disk manager.


## 9. Hermes Self-Evolution


**Self-Evolution runs optimization loops to improve your Hermes skills, prompts, and tool descriptions.**


This is the advanced entry, and it earns its spot once you have skills you rely on that fail in repeatable ways. Self-Evolution is an external framework that runs alongside Hermes rather than inside it. Instead of hand-editing a skill every time it underperforms, you run an evaluation loop and let the framework generate candidate improvements, then you inspect what changed and decide whether the new version is actually better.


- ` evolve_skill` : Run an optimization loop against a specific skill
- ` synthetic eval` : Generate test cases to score candidate versions
- ` prompt + tool description tuning` : Improve more than just skill bodies
- ` iteration control` : Set how many candidate rounds to run


**Install:**


Clone and install it as a separate package that points at your Hermes repo:


```text
git   clone   https://github.com/NousResearch/hermes-agent-self-evolution.git
cd   hermes-agent-self-evolution
pip   install   -e   ".[dev]"
export   HERMES_AGENT_REPO  =~  /.hermes/hermes-agent
```


Then run an optimization loop against a skill:


```text
python   -m   evolution.skills.evolve_skill   --skill   github-code-review   --iterations   10   --eval-source   synthetic
```


**Honest take:** This is not a first-week install. It belongs after you have a skill that fails predictably and you can tell whether a new version is genuinely better. Used well, it is a more systematic alternative to guessing at prompt edits.


**Cons:** Not plug-and-play. A candidate can score better on the eval while performing worse in real use, so you have to verify every result. Each run is a small experiment with its own model cost depending on iterations and eval setup.


Full reference at[github.com/NousResearch/hermes-agent-self-evolution](https://github.com/NousResearch/hermes-agent-self-evolution) .


## Building the top Hermes plugins into your workflow


It helps to anchor the stack to what Hermes is actually built for. Asked on[r/hermesagent](https://www.reddit.com/r/hermesagent/comments/1ug4amk/how_are_you_actually_using_hermes_im_struggling/) how people really use it, NousResearch was direct:


> Hermes shines at persistent agents with automation, not raw coding. How most people use it: 24/7 background tasks (social media posting, lead gen, monitoring, research), memory and long-term projects, run on a VPS via Telegram or Discord for always-on. \[...\] Start simple: one profile, run on VPS with Telegram.


That is the lens for picking plugins. You are equipping an always-on agent that works while you are away, not a coding copilot, which is why web context, app connectors, and memory matter more here than they would in an editor.


Nine plugins is more than most setups need on day one. The combination I would actually start with is small: **Firecrawl** for live web context, **Composio Connect** for the apps where your work happens, and **Langfuse** so you can see what the agent is doing and what it costs. Those three turn Hermes from a clever terminal companion into something that reaches your real tools and that you can trust to run unattended. The same trio anchors my[best Codex plugins](https://www.firecrawl.dev/blog/best-codex-plugins) list, which is the Codex version of this roundup if you run more than one agent.


From there, add by use case rather than collecting everything. If you run Hermes daily and want it to feel personal, add a memory provider: Honcho for personalization that deepens over months, Hindsight for precise structured recall or local-only privacy. If you coordinate multiple agents, add the Kanban Dashboard. If meetings are part of the job, Google Meet writes the notes. Disk Cleanup is the quiet one that pays off the longer you run the agent, and Self-Evolution is worth exploring once you are maintaining skills seriously.


Two things make this stack coherent. First, Hermes and OpenClaw share the same AgentSkills` SKILL.md` format, so skills are largely portable between them (the[OpenClaw skills](https://www.firecrawl.dev/blog/openclaw-skills) roundup covers the same format from the OpenClaw side), and[skills.sh](https://skills.sh/) is the best place to browse the top Hermes plugins and skills the community is publishing. Second, most of these are free to install, with the paid surface limited to the services they connect to. For the wider agent picture, the[Hermes Agent guide](https://www.firecrawl.dev/blog/hermes-agent) covers installation, memory, scheduling, and how the pieces fit together end to end. Start with the three-plugin core, then let your own workflows tell you what to add next.
