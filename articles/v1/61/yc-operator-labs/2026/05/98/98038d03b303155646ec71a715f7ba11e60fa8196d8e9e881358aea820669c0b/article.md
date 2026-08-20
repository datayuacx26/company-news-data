---
schema_version: "1.0.0"
document_id: "98038d03b303155646ec71a715f7ba11e60fa8196d8e9e881358aea820669c0b"
company_key: "yc-operator-labs"
company: "Operator Labs"
source_id: "yc-operator-labs-news-import-4e5219b8eb88"
canonical_url: "https://operator.io/blog/what-is-openclaw"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-22T07:38:43.480760+00:00"
fetched_at: "2026-07-28T21:44:23.277081+00:00"
content_hash: "sha256:789dcf4ce9835849969c84b4fba53da085fd4bb7ce25aa698b83544d83bf3dd1"
---

# What is OpenClaw? The open source AI agent, explained

[OpenClaw](https://en.wikipedia.org/wiki/OpenClaw) is an open source framework for running your own AI agent.


The simplest way to picture it: take the kind of model that powers ChatGPT, Claude, or Gemini, then wire it to a messaging app you already use and give it the ability to actually do things, run tools, search the web, keep files, remember what happened last week, and run jobs on a timer.


That bundle, the model plus the channel plus the ability to act and persist, is what OpenClaw is, and you run it on a machine you control rather than logging into someone else's website.


## Where it came from


Peter Steinberger, the Austrian developer behind[PSPDFKit](https://pspdfkit.com/) , started the project in late 2025 as a small bridge between a model and Telegram. It spread fast under the name Clawdbot, was renamed Moltbot in January 2026 after Anthropic raised trademark concerns about similarity to Claude, and settled on OpenClaw days later as a model agnostic, open source name.


A readable timeline of those two renames is in[this rebrand writeup](https://lumadock.com/blog/clawdbot-moltbot-openclaw-rebrand) . Steinberger's own[February 2026 post](https://steipete.me/posts/2026/openclaw) explains the next chapter: he joined OpenAI to work on personal agents, while OpenClaw moves toward an independent foundation with OpenAI as a sponsor.


The[MIT license](https://github.com/openclaw/openclaw/blob/main/LICENSE) and the public[GitHub repository](https://github.com/openclaw/openclaw) stayed in place through all of that, which is why third party hosts like[Operator.io](https://operator.io/) can run the same codebase without forking it.


## What it is


OpenClaw is software you install. On a server or your own computer you run` npm install -g openclaw@latest` (or follow the[install guide](https://docs.openclaw.ai/install) ), point it at a model provider with an API key, and connect a channel.


From then on a single long lived process called the[gateway](https://docs.openclaw.ai/) keeps the agent alive and reachable, and you talk to it through[Telegram](https://operator.io/blog/talk-to-your-agent-on-telegram) , WhatsApp, Slack, Signal, or one of the other channels documented on[openclaw.ai](https://openclaw.ai/) . The project targets a current[Node](https://nodejs.org/) runtime (Node 24 recommended, Node 22.19+ supported per the docs).


It is model agnostic on purpose. You can run it on a frontier model from OpenAI, Anthropic, or Google, or on a local model through[Ollama](https://ollama.com/) or a router like[OpenRouter](https://openrouter.ai/) , and switch between them in config. The model does the thinking; OpenClaw supplies the tools, the channel, and the memory around it.


## The parts of OpenClaw


OpenClaw is a handful of parts working together, and it helps to know what each one does.


Part What it does


Gateway The always running process that holds the agent open and keeps your channel connected, which is why so much of the troubleshooting people search for is about the gateway being down


Workspace A directory of plain files where your data and the agent's memory live, so what it learned last week is a file it can open this week


Skills Packaged abilities you add for specific jobs, like working with documents, media, or a particular service


[MCP](https://modelcontextprotocol.io/) servers Reach external tools and APIs through one standard protocol, so a single connection exposes your email, your calendar, and other apps you authorize


Web search Current information rather than only what the model was trained on


Scheduler Runs jobs on a timer, which turns a one off request into a standing automation


None of these is exotic alone; the combination is what lets it do work and remember having done it.


## What makes it different from a chat app


ChatGPT, Claude, and Gemini can all do a lot now, but the work happens inside a conversation that is built to end. OpenClaw is built around two things a chat window is weak at: acting and remembering.


It keeps a real workspace, a directory of plain files that stays put between conversations, so a budget is a CSV it appends to and your notes are text files it reads back. It uses tools through MCP and a library of skills, so it can reach your email, your calendar, and other connected apps, plus the shell when you allow it, and the safety section below covers how to keep that reach in bounds.


And it runs on a schedule, so a morning briefing or a price check happens on its own clock. The longer argument for why that combination matters is in[why choose a personal agent](https://operator.io/blog/operator-personal-agent) .


## What people use it for


Most of what people hand it are small standing jobs rather than one off questions.


A reading list that summarizes links you send and resurfaces them later. A budget that logs a purchase from a forwarded receipt. A watcher that pings you when a price drops or a web page changes. A daily digest of news, your calendar, and yesterday's activity, delivered to your chat at seven in the morning.


None of these is impressive on its own. The agent keeps the file current for months without you thinking about it. The[prompts library](https://operator.io/prompts) is full of these.


## Setup


Standing up OpenClaw yourself is four steps once you have a machine for it. You install it with` npm install -g openclaw@latest` , or pull the Docker image, which needs a current Node runtime and, on a minimal server, the git binary, the two things behind most[install errors](https://operator.io/blog/openclaw-npm-install-failed) .


You give it a model by adding an API key for whichever provider you picked. You connect a channel by pairing it with a Telegram bot, which is the[step](https://operator.io/blog/talk-to-your-agent-on-telegram) people most often get stuck on. Then you send it a first instruction and answer the questions it asks back.


From there it runs as a background service and you talk to it from your phone. When it goes smoothly the whole thing takes a short evening; when a dependency or a pairing code fights you it can eat a real afternoon, which is the gap a managed host exists to close.


## How you run it


There are two paths. You can self host: rent a[VPS](https://operator.io/blog/where-to-host-openclaw) or use a spare machine, install Node and OpenClaw, supply your own model API key, connect your channel, and keep the whole thing patched and running.


That gives you full control and costs as little as a few dollars a month for the box plus whatever your model usage runs. It also makes you the operations team, and a good share of the most searched OpenClaw problems are[install errors](https://operator.io/blog/openclaw-npm-install-failed) and gateway failures from exactly that work.


The other path is a managed host.[Operator](https://operator.io/) is managed OpenClaw: it runs the framework for you with a model, web search, channels, and connectors already wired up, so you sign in and the agent is already running. You give up running it on your own hardware in exchange for skipping the setup and the upkeep. Which one fits depends on whether owning the box matters more to you than owning your afternoon, and the full breakdown of the choices and their prices is in[where to host OpenClaw](https://operator.io/blog/where-to-host-openclaw) .


## How it compares to other tools


OpenClaw sits next to a few things you may already use, and the differences are worth being clear about. Against automation tools like[Zapier](https://operator.io/blog/operator-vs-zapier) , Make, or n8n, which run fixed trigger and action workflows you build and map step by step, OpenClaw takes an instruction in plain language and works out the steps itself, which suits jobs that need a judgment call more than a rigid pipeline.


Against a coding agent such as[OpenAI Codex](https://operator.io/blog/openai-codex-vs-personal-agent) or[Claude Cowork](https://operator.io/blog/claude-cowork-vs-personal-agent) , which run on your own computer to edit files and write code, OpenClaw runs as a service that keeps going when your laptop is closed and is built for everyday work rather than changing a codebase. And against a plain chat assistant, the gap is the one above: it acts and it remembers.


Each of those tools has a place, and OpenClaw is the one aimed at being a personal agent that runs continuously.


## Safety and cost


Because the code is open source, anyone can read exactly what it does, and you can run it on infrastructure you control with no third party in the loop.


The safety questions are about reach rather than the code. An agent that can run shell commands, write files, and act in your connected accounts is powerful in proportion to what you let it touch, so the permissions you grant and the scopes you set on each connected app are where the real care goes. The project documents allowlists, tokens, and sandboxing in its[security guide](https://docs.openclaw.ai/gateway/security) .


Early 2026 also saw real incidents around exposed gateways and malicious skills in community marketplaces, which is why running it on a separate machine or in a managed container, rather than on your daily laptop, is the common advice for limiting how much it can touch.


The software itself is free; the recurring costs are the model behind it and the machine it runs on, which is why[where you host it](https://operator.io/blog/where-to-host-openclaw) is the question that actually sets your bill. The short version of who OpenClaw is for: people who want an assistant that does the work and keeps the results, and who are willing either to run a small server or to pay someone to run it. You can[try the managed version free](https://operator.io/pricing) , or clone the[repo](https://github.com/openclaw/openclaw) and stand up your own.
