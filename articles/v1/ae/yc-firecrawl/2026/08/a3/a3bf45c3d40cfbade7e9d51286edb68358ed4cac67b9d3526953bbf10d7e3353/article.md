---
schema_version: "1.0.0"
document_id: "a3bf45c3d40cfbade7e9d51286edb68358ed4cac67b9d3526953bbf10d7e3353"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/best-chatgpt-plugins"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-10T17:07:06.704938+00:00"
fetched_at: "2026-08-10T17:07:07.841770+00:00"
content_hash: "sha256:f0f9b30e2327b9ac002e0665de1bae8ccf77cf04a15455578df9a4a7d4b0bafa"
---

# What Are the Best ChatGPT Plugins to Install in 2026?

**TL;DR:** Best ChatGPT plugins


- **Firecrawl** : Live web context for ChatGPT: search, scrape, interact, monitor, YouTube transcripts, document parsing
- **Slack** : Search channels and DMs, summarize threads, draft replies without leaving chat
- **Asana** : Turn briefs into projects, tasks, and follow-ups; check portfolio status
- **Canva** : Create and edit real Canva designs, presentations, and social posts inside ChatGPT
- **ElevenLabs (MCP)** : Text-to-speech, voice cloning, sound effects, and voice design over MCP
- **Runway (MCP)** : Image and video generation with Gen-4.5, Aleph, Kling, and Veo models
- **Hex** : Query your data warehouse and run Threads agents directly from ChatGPT


Some of these are official ChatGPT apps you install from` chatgpt.com/plugins` in one click. Others are MCP connectors you add through Developer Mode by pasting an MCP server URL. Both surfaces sit next to each other in the same ChatGPT session, and once installed you stop noticing the difference.


---


I've been using ChatGPT with plugins connected for most of 2026, and the shift is real. A plugged-in ChatGPT stops being a smart chat window and starts behaving like a workspace: it can pull live web data, act on my Slack, generate finished designs, hit my data warehouse, and produce voice and video on demand.


That shift isn't a personal quirk, it's the mainstream now:


- ChatGPT nearly doubled in eight months, going from 400M weekly active users in February 2025 to 800M by October, per Sam Altman at DevDay ([Business Insider](https://www.businessinsider.com/chatgpt-users-openai-sam-altman-devday-llm-artificial-intelligence-2025-10) ).
- People send more than 2.5 billion prompts per day, around 29,000 per second, according to a September 2025[OpenAI and Harvard working paper](https://www.nber.org/papers/w34255) .
- 92% of Fortune 500 companies now use ChatGPT, workplace seats crossed 7 million, and Enterprise seats grew roughly 9x year over year, per[OpenAI's State of Enterprise AI 2025](https://openai.com/index/the-state-of-enterprise-ai-2025-report/) .


The catalog exploded after OpenAI merged the older "app directory" into a proper Plugin surface and opened up custom MCP connectors on paid plans. Between OpenAI's curated apps and the growing set of MCP servers you can point ChatGPT at yourself, the number of workflows that used to require five tabs now happens in one conversation.


These are the best ChatGPT plugins I'd hand someone starting today. Seven picks that cover live web data, team communication, project management, design, audio, video, and data analysis. Some are official OpenAI-listed apps. Others are MCP connectors you snap in through Developer Mode.


## What are ChatGPT plugins?


A ChatGPT plugin is any integration that lets ChatGPT read from or act on an outside service during a conversation. In 2026 that catalog splits into two surfaces:


- **Official ChatGPT apps and connectors.** Vendor-built, listed in the plugin directory at[chatgpt.com/plugins](https://chatgpt.com/plugins) , installed in one click. OAuth handled for you, admin governance in the workspace settings on Business and Enterprise plans. Slack, Asana, Canva, Hex, HubSpot, Google Drive, GitHub, and dozens of others live here.
- **Custom MCP connectors.** Any service that ships an[MCP server](https://www.firecrawl.dev/blog/mcp-servers) endpoint can be added through Developer Mode: Settings, Connectors, add a custom connector, paste the URL, sign in. This is how Runway, ElevenLabs, self-hosted tools, and regional endpoints connect to ChatGPT.


Both surfaces work in the same conversation. Plugins activate implicitly when your prompt matches, or explicitly when you mention the plugin name. Inside a session you stop caring which one is an app and which one is an MCP connector, which is exactly how it should feel.


The[Firecrawl MCP](https://www.firecrawl.dev/blog/firecrawl-mcp-chatgpt) is a good example of the crossover: Firecrawl ships both as an official ChatGPT plugin and as an MCP server, so you get the same web toolkit whether you install it as a one-click app or as a custom connector.


## How to install ChatGPT plugins


Installing an official plugin takes about a minute.


**1. Open Settings.** Click your profile in ChatGPT, then Settings, then **Apps and Connectors** .


**2. Find the plugin.** Browse the directory at[chatgpt.com/plugins](https://chatgpt.com/plugins) or search by name (Firecrawl, Slack, Canva, Asana, Hex).


**3. Connect.** Click Connect on the plugin card. ChatGPT walks you through OAuth or asks for the credentials the plugin needs. On Business and Enterprise workspaces, admins may need to enable the plugin first from workspace settings.


**4. Use it.** Once connected the plugin activates implicitly when your prompt matches, or you can invoke it by mentioning the plugin by name ("Canva, design an Instagram post about..."). The plugin picker also lives in the` +` / Tools menu inside any chat.


For MCP connectors the flow is slightly different:


**1. Turn on Developer Mode.** Settings, Advanced, Developer Mode. Available on paid plans.


**2. Add a custom connector.** Settings, Connectors, add a custom connector, name it (for example "Runway"), paste the MCP server URL (for example` https://mcp.runwayml.com/mcp` ), then Connect and sign in.


**3. Use it.** MCP connectors show up in the same tool picker as official apps. Business, Enterprise, and Edu plans get full MCP with write actions; Plus and Pro get read-only MCP by default.


Admin governance for both types lives under workspace settings for Business and Enterprise, and any plugin can be disabled per-chat from the tool picker if you don't want it routing to your current task.


## The best ChatGPT plugins to try in 2026


Here are the seven I keep connected and reach for daily.


## 1. Firecrawl


**The[Firecrawl ChatGPT plugin](https://chatgpt.com/plugins/plugin_asdk_app_6a314a73f8ac819195b0d55e36b9c609?q=firecrawl) gives ChatGPT direct access to live, clean web data across search, scraping, interaction, monitoring, YouTube, and documents.**


ChatGPT is only as fresh as the web it can reach. Built-in browsing gives snippets and summaries, but the moment you need real answers grounded in real sources, you hit walls: shallow search results with no page content, noisy JavaScript-rendered pages, PDFs and DOCX that don't parse, and stale snippets that lead nowhere.


The Firecrawl plugin fixes both sides of that problem in one install. It ships both as an official ChatGPT plugin and as an MCP server, so you can install it as a one-click app or connect it as a custom MCP connector inside Developer Mode.


[Firecrawl](https://www.firecrawl.dev/) powers over 5 billion requests across 1.25M+ developers and 150,000+ companies, and is one of the top 50 most-starred repos on[GitHub](https://github.com/firecrawl/firecrawl) . It's designed for AI agents: JavaScript rendering, anti-bot handling, and file-based output all happen automatically, so ChatGPT's context window stays clean even on big jobs.


**What ChatGPT gets access to:**


- [search](https://www.firecrawl.dev/search) : Web search that returns the excerpts that best answer the query, with full page content included alongside the links
- [scrape](https://www.firecrawl.dev/scrape) : Clean markdown or structured JSON from any URL, including JavaScript-heavy sites, PDFs, and DOCX files
- [interact](https://www.firecrawl.dev/interact) : Scrape a page then act on it with natural language: click, fill forms, paginate, log in, extract dynamic content
- [crawl](https://www.firecrawl.dev/crawl) : Recursively follow links across a full site to build a knowledge base or dataset
- [monitor](https://www.firecrawl.dev/monitor) : Watch pages for changes and get alerts (pricing pages, job boards, release notes), with Slack delivery built in
- [parse](https://www.firecrawl.dev/parse) : Turn PDFs, DOCX, XLSX, and PPTX into clean markdown or structured data
- **YouTube summaries and transcripts** : Pull transcripts and generate summaries of any YouTube video without a separate tool


**Install:**


Open ChatGPT, go to Settings, Apps and Connectors, search for Firecrawl, and click Connect. Or install directly from the plugin page at[chatgpt.com/plugins/firecrawl](https://chatgpt.com/plugins/plugin_asdk_app_6a314a73f8ac819195b0d55e36b9c609?q=firecrawl) .


**Example prompts:**


```text
"Search the web for this week's biggest AI announcements and give me a briefing with sources"


"Scrape https://news.ycombinator.com and list the top 10 stories with links and points"


"Summarize this YouTube video: https://youtube.com/watch?v=... and pull the transcript so I can quote it"


"Monitor https://openai.com/blog for new posts and post updates to my #product-updates Slack channel"


"Interact with Campspot.com to find campsites near Yosemite available Aug 15 to 17"


"Parse this PDF and turn the pricing table into structured JSON I can paste into a doc"
```


**Honest take:** This is the one plugin I would install first on any ChatGPT account. Every other plugin on this list solves a specific job; Firecrawl solves the underlying "ChatGPT can't see the live web properly" problem that limits every other workflow.


Once it's connected, ChatGPT stops guessing at current documentation, pricing, or company details, and starts pulling the real page in real time. For the launch details and end-to-end walkthrough, see[Firecrawl is Now an Official ChatGPT Plugin](https://www.firecrawl.dev/blog/firecrawl-official-chatgpt-plugin) .


Full plugin page at[chatgpt.com/plugins/firecrawl](https://chatgpt.com/plugins/plugin_asdk_app_6a314a73f8ac819195b0d55e36b9c609?q=firecrawl) . Docs at[docs.firecrawl.dev](https://docs.firecrawl.dev/) . Get a free API key at[firecrawl.dev/app/api-keys](https://firecrawl.dev/app/api-keys) .


## 2. Slack


**The official Slack app for ChatGPT puts your workspace inside the chat: search channels and DMs, summarize threads, and draft replies without switching tabs.**


Slack is where most work conversations happen, and pulling context out of it into ChatGPT used to mean copy-pasting threads. The official ChatGPT app for Slack (and the paired Slack app for ChatGPT) closes that loop.


You can search channels and DMs by keyword or semantically, ask ChatGPT to summarize a long thread into action items, and draft replies inside Slack that go through ChatGPT first.


Semantic search across Slack requires Slack AI on a Business+ or Enterprise+ plan; keyword search works on any paid Slack tier. Enterprise admins have to enable the ChatGPT app under admin settings, and workspace admins may need to approve the install before end users can connect.


**What you get:**


- Keyword and semantic search across channels and DMs
- Thread summaries: paste a link, get action items, decisions, and open questions
- Draft replies inside Slack that are ChatGPT-composed and preserve tone
- Slack chats appear in the ChatGPT sidebar once connected, so you can start a new conversation from a Slack message


**Install:**


Open ChatGPT, go to Settings, Apps and Connectors, search for Slack, click Connect, and authorize the workspace. For the reverse direction (using ChatGPT inside Slack), install the[ChatGPT app for Slack](https://slack.com/marketplace/A097V82EGG2-chatgpt) from the Slack marketplace and follow the workspace OAuth flow.


**Example:**


```text
"Summarize the last 24 hours in #product-launches and pull out anything I need to reply to"


"Draft a reply to this Slack thread that acknowledges the concern and proposes a fix by Friday"


"Search Slack for every mention of the auth migration in the last two weeks"
```


**Honest take:** Genuinely useful once you accept the plan tier requirements. The semantic search on top of Slack AI is the feature that changes the workflow: you stop having to remember which channel a decision happened in.


Thread summaries are strong too, especially for catching up after a long OOO. It falls short on drafting nuanced replies where team-specific context matters, so I still read what it drafts before sending.


**Cons:** Requires a paid Slack plan for anything meaningful, and semantic search requires Slack AI on Business+ or Enterprise+. Not available for Plus and Pro users in the EEA, UK, and Switzerland; Business and Enterprise plans are unaffected. Admin approval can slow the rollout on larger workspaces.


Full plugin page:[chatgpt.com/plugins/slack](https://chatgpt.com/plugins/plugin_asdk_app_69a1d78e929881919bba0dbda1f6436d) . Reference:[help.openai.com/chatgpt-app-in-slack](https://help.openai.com/en/articles/12462158-chatgpt-app-in-slack) .


## 3. Asana


**Asana's ChatGPT plugin turns brainstorms and briefs into real projects, tasks, and follow-ups without leaving the chat.**


Asana was one of the first task managers to ship a proper sync-based ChatGPT integration, and it recently got upgraded with MCP-based write actions. What that means in practice: ChatGPT can now create and update Asana tasks from inside a conversation, not just read your projects.


You can dump a call recording or a Slack thread into ChatGPT, ask it to turn the outcomes into an Asana project, and it will structure the sections, tasks, subtasks, and assignees for you.


The sync indexes projects, sections, tasks, subtasks, comments, and activity notes, so ChatGPT can also answer status questions across your entire portfolio. It works inside regular chat and inside Deep Research.


**What you get:**


- Turn briefs, docs, and chat brainstorms into complete Asana projects with tasks and subtasks
- Create, update, and complete tasks with MCP-based write actions
- Check status across projects and portfolios from a ChatGPT conversation
- Sync indexes projects, sections, tasks, subtasks, comments, and activity notes for search


**Install:**


Open ChatGPT, go to Settings, Apps and Connectors, search for Asana, and click Connect. On the Asana side, the connector requires Full Permissions to the workspace you're syncing. Initial sync can take minutes to hours depending on workspace size. Admin approval may be required to enable the newer MCP-based write actions.


**Example:**


```text
"Turn this launch brief into an Asana project with milestones for design, engineering, and marketing"


"What are the top three tasks I own across all my projects this week?"


"Create tasks in the Q4 Marketing project for each recommendation in this doc, assign them to Sam, and set due dates for next Friday"
```


**Honest take:** The write actions are the real unlock. Sync-only integrations are useful for reading status, but the friction was always in the "turn this into tasks" step, which used to happen manually.


With write actions, ChatGPT can go from a call transcript to a fully structured Asana project in one prompt. The output still needs a quick human review before you assign real work, but it's the closest I've seen to a chat-to-task-tracker flow that works.


**Cons:** Initial sync can be slow on large workspaces. Upgrade to MCP write actions may require admin approval. The generated task breakdowns are sometimes over-eager: I usually delete a few tasks per project before assigning.


Full plugin page:[chatgpt.com/plugins/asana](https://chatgpt.com/plugins/plugin_asdk_app_69616780bd208191b4fb44ba44f72b61) . Reference:[asana.com/apps/chatgpt](https://asana.com/apps/chatgpt) .


## 4. Canva


**Canva's ChatGPT plugin lets you generate, preview, and edit real Canva designs from inside the chat: presentations, social posts, invites, and more.**


Canva was one of the first design tools to ship a genuinely useful ChatGPT integration, and it has quietly become one of the plugins I use most.


You describe what you want in plain language ("Canva, design an Instagram post announcing our launch, with the Firecrawl brand palette"), and Canva returns an editable design you can revise through chat: change the copy, adjust the tone, translate to another language, resize for a different aspect ratio, or rework the layout while keeping the original structure intact.


**What you get:**


- Create presentations, social posts, invites, and other design types from a prompt
- Revise text, tone, and layout through natural-language edits in chat
- Translate designs to another language while preserving layout
- Resize a design to a different aspect ratio without redoing the layout
- Access to Canva's template library and brand kit for on-brand output


**Install:**


Open ChatGPT, go to Settings, Apps and Connectors, search for Canva, click Connect, and authorize your Canva account. To use it in a chat, either mention "Canva" in your prompt or enable it from the Tools menu inside the chat.


**Example:**


```text
"Canva, design a LinkedIn banner for our Series B announcement using the Firecrawl brand colors"


"Canva, take the deck from yesterday and translate it into Spanish, keeping all the graphics intact"


"Canva, resize this Instagram post as a story and adjust the layout so the headline stays legible"
```


**Honest take:** For marketers who don't need to touch Figma, this plugin covers 80% of daily design work directly in ChatGPT. The best moment is asking it to iterate: instead of clicking through Canva's UI, you say "make the CTA more prominent" or "swap the hero image for something that feels more editorial" and it does.


It's still Canva, so if you want brutalist typography or a really distinctive editorial style, you'll hit the ceiling of the template system.


**Cons:** You're bounded by Canva's aesthetic conventions. If your brand needs anything highly custom, the output starts feeling generic. Complex multi-page designs sometimes need manual cleanup in the Canva editor after the chat pass.


Full plugin page:[chatgpt.com/plugins/canva](https://chatgpt.com/plugins/plugin_connector_68df33b1a2d081918778431a9cfca8ba?q=canva) . Reference:[canva.com/help/chatgpt-templates](https://www.canva.com/help/chatgpt-templates/) .


## 5. ElevenLabs (MCP)


**The official ElevenLabs MCP server gives ChatGPT text-to-speech, voice cloning, sound design, and voice generation over a custom connector.**


ElevenLabs is the reference for AI voice generation, and their MCP server is how you plug that capability into ChatGPT. Unlike the vendor apps in this list, this one connects through Developer Mode as a custom MCP connector rather than the official app directory.


Once connected, ChatGPT can generate voiceovers, clone voices from a sample, transcribe audio, isolate voice from noisy recordings, generate sound effects and soundscapes, produce music from video, and design new voices from a text description.


The repo has 1.5k GitHub stars and 249 forks, is maintained by ElevenLabs directly, and works with any MCP client (Claude Desktop, Cursor, Windsurf, ChatGPT Developer Mode).


**What you get:**


- ` text-to-speech` : Generate speech from text in dozens of languages and voices
- ` voice-cloning` : Clone a voice from a short audio sample
- ` speech-to-text` : High-quality transcription
- ` speech-to-speech` : Convert speech from one voice to another while preserving inflection
- ` audio-isolation` : Strip background noise from voice recordings
- ` sound-effects` : Generate short sound effects and ambient soundscapes from a text prompt
- ` video-to-music` : Generate a matching soundtrack for a given video
- ` voice-design` : Create entirely new voices from a text description


**Install:**


In ChatGPT, turn on Developer Mode (Settings, Advanced), then go to Settings, Connectors, and add a custom connector. Point it at the ElevenLabs MCP server. On the ElevenLabs side, follow the[MCP setup guide](https://github.com/elevenlabs/elevenlabs-mcp) to run the server locally or via` uvx` , and set your` ELEVENLABS_API_KEY` .


```text
# Install via uv (recommended)
uvx   elevenlabs-mcp


# Or run from source
git   clone   https://github.com/elevenlabs/elevenlabs-mcp
cd   elevenlabs-mcp   &&   uv   sync
```


**Example:**


```text
"Generate a 30-second voiceover in a warm, mid-tempo British accent for this ad copy"


"Clone this voice sample and read the attached script in the same voice"


"Isolate the speaker's voice from this noisy recording and give me a clean audio file"


"Design a new voice: young, sardonic, slightly gravelly, and use it to read this dialogue"
```


**Honest take:** ElevenLabs is one of the few AI audio tools that produces genuinely usable voice output, and running it through MCP means ChatGPT can chain it into other workflows: scrape a script, generate a voiceover, save it to a file, all in one conversation.


The MCP surface is more capable than most people realize: voice cloning and voice design in particular open up production workflows that used to need dedicated software.


**Cons:** File operations are sandboxed to` ELEVENLABS_MCP_BASE_PATH` (defaults to` ~/Desktop` ), which is a good security boundary but can trip up automated pipelines. Voice cloning uses ElevenLabs credits that add up quickly on longer content. Requires local setup rather than a one-click install.


Repo:[github.com/elevenlabs/elevenlabs-mcp](https://github.com/elevenlabs/elevenlabs-mcp) .


## 6. Runway (MCP)


**Runway's official MCP connector lets ChatGPT generate images and video with Gen-4.5, Aleph, Kling, Veo, and other frontier models.**


Runway ships an official MCP server that plugs directly into ChatGPT through a custom connector. Once connected, ChatGPT can call Runway's full model catalog to generate video from text or image, animate stills, and produce sequences without opening the Runway app.


It supports the whole current lineup: Gen-4.5, Aleph, Kling, Veo, Seedance, and Runway's own GPT-based image models. ChatGPT picks the right model based on your prompt, or you can name a specific one.


This is the most impressive "you just added a superpower" moment I've had with an MCP connector. Prompts that used to require jumping between three tools now happen in one message.


**What you get:**


- Image and video generation across every model in Runway's catalog
- Image-to-video: animate a still image with motion prompts
- Text-to-video: describe a scene and get a video clip back
- Model auto-selection based on the prompt, or explicit model choice
- Works with the same credits as your Runway account


**Install:**


In ChatGPT, turn on Developer Mode. Then go to Settings, Connectors, click **Add custom connector** , name it` Runway` , and paste the MCP URL:


```text
https://mcp.runwayml.com/mcp


```


Click Connect and sign in with your Runway account. The full setup guide is at[help.runwayml.com Connecting to Runway MCP](https://help.runwayml.com/hc/en-us/articles/51931843164691-Connecting-to-Runway-MCP) .


**Example:**


```text
"Generate a 5-second video of a paper plane flying through a rainstorm, cinematic style, using Gen-4.5"


"Take this product photo and animate it: slow camera pan, subtle background motion, keep the product still"


"Create three variations of a 3-second social ad clip: text-first, product-first, and lifestyle"
```


**Honest take:** Video generation inside ChatGPT feels different from generating in a dedicated app because ChatGPT holds the full context of the project. You can research a reference (via Firecrawl), draft a script, generate voiceover (via ElevenLabs MCP), and generate matching video (via Runway MCP) inside a single conversation.


That composability is what makes MCP connectors more powerful than they look on paper.


**Cons:** Explore Mode generations aren't supported through the MCP, and costs are billed to your Runway account rather than being free. Video generation still takes real time (30s to a few minutes depending on model and resolution). The output quality tracks the Runway model you're using, so cheaper models look cheaper.


Reference:[help.runwayml.com Connecting to Runway MCP](https://help.runwayml.com/hc/en-us/articles/51931843164691-Connecting-to-Runway-MCP) .


## 7. Hex


**Hex's ChatGPT plugin connects ChatGPT to your data workspace: search projects, ask open-ended data questions, and run the Threads agent from chat.**


Hex is a collaborative data workspace where analysts build notebooks, apps, and reports over a SQL warehouse.


The Hex ChatGPT plugin lets you talk to that workspace: search for existing projects by name or content, ask analytical questions in natural language, and kick off a Hex Threads session (Hex's AI data agent) that queries your warehouse and returns a full analysis with charts and reasoning.


For teams already running on Hex, this is a huge quality-of-life win. Instead of context-switching to the Hex app to run a query, you ask ChatGPT and get a full Threads notebook back with the answer.


**What you get:**


- Search across Hex projects by name and content
- Start and continue Hex Threads (Hex's data agent) from ChatGPT
- Run SQL over your data warehouse and get charts and analysis back
- Available on Hex Team and Enterprise plans


**Install:**


Open ChatGPT, go to[chatgpt.com/plugins](https://chatgpt.com/plugins) , search for Hex, and click Connect. Authorize the OAuth flow into your Hex workspace. On Business and Enterprise ChatGPT, an admin may need to enable the Hex app under workspace settings first. For regional or single-tenant deployments, use Developer Mode to add the Hex MCP server as a custom connector instead.


**Example:**


```text
"Search Hex for any projects covering churn analysis and open the most recent one"


"Ask Hex Threads: what was our weekly active user count over the last six months, broken down by pricing tier?"


"Run a cohort analysis on users who signed up in April and stayed active past 30 days"
```


**Honest take:** If your team already lives in Hex, this is the plugin that fits it into your daily ChatGPT flow. The Threads agent is genuinely good at running queries and returning analysis, and having that available from any ChatGPT window means you use it more.


For teams that don't have Hex, this obviously doesn't apply, but for the ones who do, it's a workflow multiplier.


**Cons:** Requires a Hex Team or Enterprise plan; the free tier doesn't get the Threads agent. Regional endpoints need the MCP path rather than the official app. Threads agent runtime can be several minutes on complex queries, which is more than most ChatGPT users expect from a plugin.


Full plugin page:[chatgpt.com/plugins/hex](https://chatgpt.com/plugins/plugin_connector_690a9430a270819196671dcb4c95898e?q=hex) . Reference:[learn.hex.tech/docs/api-integrations/mcp-server](https://learn.hex.tech/docs/api-integrations/mcp-server) .


## Building the top ChatGPT plugins into your workflow


The most useful ChatGPT setup isn't any one plugin, it's the combination.


The stack I actually use every day looks like this: Firecrawl for live web context and monitoring, Slack and Asana for team communication and follow-through, Canva for finished design output, and Hex when I need to ask a real question of the warehouse. Runway and ElevenLabs slot in for the video and audio parts of a project.


The split between official apps and MCP connectors matters less than it sounds. Some of the best plugins (Firecrawl, Slack, Asana, Canva, Hex) ship as official ChatGPT apps with one-click OAuth. Others (Runway, ElevenLabs) live behind an MCP server URL you add through Developer Mode.


Once installed, both sit in the same tool picker and ChatGPT chains them the same way. The MCP standard is what unlocks the second half of this ecosystem: anything with an MCP endpoint plugs into ChatGPT with a URL and a signin.


For discovery,[chatgpt.com/plugins](https://chatgpt.com/plugins) is the official plugin directory. For MCP servers, the[MCP servers roundup](https://www.firecrawl.dev/blog/best-mcp-servers-for-developers) and the[best web search MCP servers](https://www.firecrawl.dev/blog/best-web-search-mcp-servers) guide are good starting points to see what's out there.


If you're working across coding agents, the sibling roundups on[best Codex plugins](https://www.firecrawl.dev/blog/best-codex-plugins) and[best Grok plugins](https://www.firecrawl.dev/blog/best-grok-plugins) cover the same territory for OpenAI Codex and xAI's Grok Build.


If you're evaluating how the Firecrawl plugin sits alongside the MCP version, the Firecrawl MCP for ChatGPT walkthrough covers both surfaces and when to use each.


If you install one plugin from this list, make it Firecrawl. It changes what ChatGPT can see, which changes what every other plugin in your setup can act on.
