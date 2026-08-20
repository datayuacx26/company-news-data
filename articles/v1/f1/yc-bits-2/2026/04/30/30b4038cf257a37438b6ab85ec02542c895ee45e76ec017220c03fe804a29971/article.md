---
schema_version: "1.0.0"
document_id: "30b4038cf257a37438b6ab85ec02542c895ee45e76ec017220c03fe804a29971"
company_key: "yc-bits-2"
company: "Bits"
source_id: "yc-bits-2-news-import-8ae56e6aae1c"
canonical_url: "https://klausai.com/blog/how-openclaw-actually-works-architecture-for-non-engineers/"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-24T21:07:46.493176+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:04a83f75c482e52e5b85d8f3463acf7e41c55c7dd54b46e5fdd6e5820858a360"
---

# How OpenClaw Actually Works: Architecture for Non-Engineers

There are a lot of “OpenClaw architecture” posts out there. Most of them are written by people who read the source code once and drew a diagram. This one is written by someone who deals with the parts that don’t show up in those diagrams.


This post is about how OpenClaw works, not why you should use us. If you’re evaluating whether OpenClaw is right for your business, or you’re already using it and want to understand what’s happening behind the chat window, this should help.


## What Is OpenClaw, in One Paragraph?


OpenClaw is a self-hosted AI agent that runs on your computer or a cloud server, connects to your messaging apps (WhatsApp, Slack, Telegram, Discord), and executes real tasks using a large language model for reasoning and a set of tools for action.


It was created by Peter Steinberger in November 2025 ([Wikipedia](https://en.wikipedia.org/wiki/OpenClaw) ) and has grown to over 335,000 GitHub stars as of March 2026 ([GitHub](https://github.com/openclaw/openclaw) ).


If you want the full “what is it and what can it do” overview, see[What Is OpenClaw? A Complete Guide for Business Owners](https://klausai.com/blog/what-is-openclaw-guide-for-business-owners) . This article is about what’s happening under the hood.


## The Five Pieces That Make It Work


OpenClaw is built from five core components that work together. Each one handles a different job.


Component What it does Analogy


**Gateway** Routes messages between your apps and the agent The mailroom


**Brain** Uses an LLM to reason about tasks and decide what to do The decision-maker


**Hands** Executes actions: runs commands, calls APIs, browses the web The workers


**Memory** Stores preferences, contacts, and context in files on disk The notebook


**Heartbeat** Triggers the agent on a schedule to check for pending tasks The alarm clock


The Gateway receives your message. The Brain figures out what to do. The Hands do it. Memory makes sure the agent remembers what happened. And the Heartbeat keeps the agent working even when you’re not talking to it.


That’s the whole system. Everything else is implementation detail.


## How the Gateway Routes Your Messages


The Gateway is a long-running process that stays on all the time. Technically, it’s a WebSocket server (default port 18789) that manages connections to every messaging platform simultaneously ([OpenClaw Docs](https://docs.openclaw.ai/concepts/architecture) ).


It works as a hub-and-spoke model. One Gateway connects to WhatsApp, Slack, Telegram, Discord, email, and more. Each platform has its own channel adapter that handles authentication and message formatting. When a message arrives on any channel, the adapter converts it into a standard format and passes it to the Gateway, which routes it to the right agent session.


This means you can talk to your agent on WhatsApp while your coworker talks to it on Slack. Same agent, same memory, different apps. The Gateway keeps each conversation in its own session so nothing bleeds across.


For business owners, the Gateway is the reason OpenClaw feels like “one assistant across all your apps” instead of separate chatbots in each platform. Your assistant checks your calendar from a Slack message, then sends you the answer on WhatsApp because that’s where you asked the question.


The tradeoff is that the Gateway needs to be running all the time. On a hosting service like Klaus, that’s handled for you. Self-hosted, it means keeping a process alive on your server. If it goes down, your agent goes dark until you restart it (see[OpenClaw Hosting: Managed vs Self-Hosted](https://klausai.com/blog/openclaw-hosting-managed-vs-self-hosted) for more on that decision).


## How Your Agent Thinks: The Brain and the Agent Loop


The Brain is OpenClaw’s reasoning engine, the component that takes your message and decides what actions to take, in what order, using which tools. When the Gateway delivers your message, the Brain processes it.


It uses a pattern from AI research called ReAct (Reasoning + Acting): the agent thinks about the task, takes an action, observes the result, and repeats until the task is done ([Yao et al., ICLR 2023](https://arxiv.org/abs/2210.03629) ). Your agent is basically a while loop with opinions.


Here’s what that looks like in practice. Say you send: “Schedule a meeting with Sarah tomorrow at 2pm.”


1. The agent reasons: “I need to check the calendar for availability, find Sarah’s email, and create an event.”
2. The agent acts: calls the calendar tool.
3. The agent observes: “Tomorrow at 2pm is free.”
4. The agent acts again: creates the calendar event and sends an invite to Sarah.
5. The agent responds: “Done. Meeting with Sarah scheduled for tomorrow at 2pm.”


That’s four separate steps, each requiring a decision. The LLM handles the reasoning; OpenClaw handles the execution.


### Context Assembly: Why Your Agent Feels Consistent


Before the LLM sees your message, OpenClaw assembles a context window. This is the information the agent has access to for this conversation. It includes:


- **SOUL.md** : the agent’s personality and tone instructions
- **AGENTS.md** : operational rules and behaviors
- **TOOLS.md** : what tools are available and how to use them
- **Relevant memories** : pulled from the memory system (more on this below)


These files are loaded at the start of every conversation. That’s why your agent feels like the same person each time you talk to it, even across different messaging apps. The personality isn’t in the LLM. It’s in the configuration files that OpenClaw feeds to the LLM.


### Tool Calling: The Agent Doesn’t Run Anything Directly


The LLM never runs a command itself. It outputs structured instructions (“call the calendar tool with these parameters”), and the Hands execute them. The LLM provides the intelligence. OpenClaw provides the operating system.


This separation is important for security. The LLM can only use tools that OpenClaw makes available. It can’t install software, access files outside its workspace, or call APIs that haven’t been configured. The “Hands” have the same permissions as the user running OpenClaw, but they only act when the Brain tells them to.


## Why Your Agent Remembers: OpenClaw’s Memory System


OpenClaw’s memory system is how your agent retains context, preferences, and learned behavior across conversations, despite the LLM starting from zero every time. If you close a ChatGPT thread and open a new one, it has no idea who you are.


OpenClaw solves this by writing important information to plain Markdown files on disk ([OpenClaw Docs](https://docs.openclaw.ai/concepts/memory) ). The files are the source of truth. There is no hidden state.


It works in two tiers:


- **MEMORY.md** : long-term storage for durable facts. Your name, your preferences (“I prefer meetings before noon”), your team members, key decisions. This file loads automatically at the start of every session.
- **Daily files (memory/YYYY-MM-DD.md)** : running context and observations. What happened in today’s conversations, notes from research tasks, follow-ups. Today’s and yesterday’s files load automatically.


When the agent needs to recall something from weeks ago, it uses semantic search combined with keyword matching to find relevant notes across all memory files.


There’s also a “memory flush” built into the system. Before a conversation gets too long and context needs to be compressed, OpenClaw reminds the agent to save anything important to disk. This prevents context loss when conversations run long.


For business, this means your agent actually learns over time. Tell it your client’s name, your preferred meeting times, or your email signature style, and it remembers. Not just for this conversation. Permanently.


The files are plain Markdown, which means you can read them yourself. Open the file, see exactly what your agent “knows,” and edit it if something is wrong. No black box. At Klaus, every customer’s agent starts with a pre-configured memory structure so it’s useful from day one.


## What Skills and Tools Actually Are


Tools are the actions the agent can take. Run a shell command, read a file, browse a website, call an API, send an email. The Hands execute these actions when the Brain decides they’re needed.


Skills are different. A skill is a set of task-specific instructions (stored as a Markdown file with YAML metadata) that teaches the agent how to do something complex. “Lead enrichment” is a skill. “Daily standup summary” is a skill. “Draft a cold outreach email” is a skill.


ClawHub, the community marketplace, has over 13,700 skills available as of March 2026 ([OpenClaw Statistics](https://openclawvps.io/blog/openclaw-statistics) ). But here’s the important part: skills aren’t all loaded into the agent’s context at once. OpenClaw shows the agent a compact list of available skills (names and descriptions), and the agent reads the full instructions only when it decides one is relevant. This keeps the context window clean and prevents the agent from getting confused by irrelevant instructions.


Think of skills like apps on your phone. You don’t run all of them at once. You open the one you need when you need it.


At Klaus, we pre-install the skills our customers use most and include paid tool credits via[Orthogonal](https://klausai.com/blog/klaus-orthogonal-hundreds-apis-zero-configuration) so you’re not configuring API keys and tool access from scratch. Install a “lead enrichment” skill and your agent knows how to use Apollo and Hunter.io without you explaining the workflow.


## The Heartbeat: Why Your Agent Does Things Without Being Asked


The Heartbeat is OpenClaw’s scheduling engine, the component that triggers your agent to act on its own without waiting for a message. It fires every N minutes (default is 30), prompting the agent to check a file called HEARTBEAT.md for defined tasks and pending work.


This is proactive behavior. The agent isn’t waiting for you to ask a question. It’s checking on things: new emails, calendar changes, stock prices, overdue tasks.


For example, you can configure a heartbeat task that says: “Every morning at 8am, check my calendar and send me a Slack message with today’s meetings and any scheduling conflicts.” The agent will do this whether you’re awake or not.


This is also how OpenClaw handles follow-ups. Tell the agent “remind me to follow up with the investor on Wednesday” and it writes a task to the heartbeat file. Wednesday morning, it follows up.


Most AI tools wait for you to show up and type something. An agent with a heartbeat works in the background, like a human assistant who checks in regularly and flags things that need your attention.


## Frequently Asked Questions


### How is OpenClaw different from ChatGPT or Claude?


ChatGPT and Claude answer questions in a browser. OpenClaw connects to your messaging apps, executes multi-step tasks (checking calendars, sending emails, browsing websites), and persists memory across sessions. It’s an agent that does things, not a chatbot that responds to prompts.


### Does OpenClaw need to be running all the time?


Yes. The Gateway runs as a long-lived process (daemon) on your machine or server. If it stops, your agent can’t receive or send messages. On a hosting service like Klaus, this is handled for you. Self-hosted means keeping that process alive yourself. See[OpenClaw Hosting: Managed vs Self-Hosted](https://klausai.com/blog/openclaw-hosting-managed-vs-self-hosted) for the tradeoffs.


### Can OpenClaw use any AI model?


Yes. OpenClaw is model-agnostic. It works with Claude, the GPT-5 family, DeepSeek, and others. You can connect via OpenRouter (which gives you access to dozens of models) or use direct API keys from each provider.


### Is my data safe with OpenClaw?


Your conversation history, memory files, and tool configurations stay on your machine (or your hosting provider’s machine). Model API calls go to the LLM provider (Anthropic, OpenAI, etc.), which means your prompts pass through their servers. The Gateway binds to localhost by default and uses a device pairing system for access control ([OpenClaw Docs](https://docs.openclaw.ai/concepts/architecture) ).


### What does OpenClaw cost to run?


The software is free and open source (MIT license). You pay for two things: AI model API calls (which vary by model and usage) and hosting infrastructure (if you don’t run it on your own machine). For a full breakdown, see[How Much Does OpenClaw Cost? A Real Pricing Breakdown](https://klausai.com/blog/how-much-does-openclaw-cost-pricing-breakdown) .


## Key Takeaways


- OpenClaw has five core components: Gateway (message routing), Brain (LLM reasoning), Hands (action execution), Memory (persistent context), and Heartbeat (proactive scheduling).
- The Gateway connects all your messaging apps to one agent, so you can talk to it wherever you already communicate.
- The agent thinks using a reasoning-and-acting loop (ReAct), chaining multiple tool calls to complete multi-step tasks.
- Memory is plain Markdown on disk, not hidden state. Your agent remembers preferences and context across sessions, and you can read and edit the files yourself.
- The Heartbeat makes the agent proactive. It can monitor your inbox, check your calendar, and follow up on tasks without being asked.


---


Want to skip the setup and start with an agent that’s already configured? Sign up at[klausai.com](https://klausai.com/) .


## Sources


- [OpenClaw Docs: Gateway Architecture](https://docs.openclaw.ai/concepts/architecture) . WebSocket server, hub-and-spoke model, device pairing, session management.
- [OpenClaw Docs: Memory](https://docs.openclaw.ai/concepts/memory) . Memory tiers, MEMORY.md, daily files, semantic search, memory flush.
- [OpenClaw GitHub Repository](https://github.com/openclaw/openclaw) . 335,000+ stars, MIT license, TypeScript/Swift.
- [Star History](https://www.star-history.com/blog/openclaw-surpasses-react-most-starred-software) . “OpenClaw Surpasses React as GitHub’s Most-Starred Software Project.” 2026.
- [Wikipedia: OpenClaw](https://en.wikipedia.org/wiki/OpenClaw) . Creation history, key milestones.
- Yao, S. et al. “[ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629) .” ICLR 2023. Reasoning + acting pattern used in OpenClaw’s agent loop.
- [OpenClaw Statistics 2026](https://openclawvps.io/blog/openclaw-statistics) . 2M monthly active users, 13,729 ClawHub skills, ecosystem metrics.
