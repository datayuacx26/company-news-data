---
schema_version: "1.0.0"
document_id: "49119625ca256450f3abee93428df87979c511f9f8bbb1ea023949de169778d4"
company_key: "yc-cua"
company: "Cua"
source_id: "yc-cua-news-import-c869e0198521"
canonical_url: "https://cua.ai/blog/clawdbot-computer-use-history"
published_at: "2026-01-28T00:00:00+00:00"
first_seen_at: "2026-07-21T15:30:37.690309+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:0d05e0bfdc6545f0d470b80d385676866a60af03e2e6381f52f1e93fee1ebf81"
---

# A Story of Computer-Use: Where We Started, Where We're Headed

*Published on Jan 28, 2026 by Francesco Bonacci. Originally posted on[X](https://x.com/francedot/status/2016627257310384554) .*


***TLDR** : Since Clawdbot went viral, I've gotten a lot of questions: Where did this all come from? What's next? Here's my take.*


Clawdbot just hit the mainstream. The open-source AI assistant — now rebranded to Moltbot after trademark issues — has captured the imagination of developers and mainstream users alike. An AI that runs on your own machine, controlled through WhatsApp, extensible through plugins. It feels like the future arriving all at once.


But this moment didn't emerge from nowhere. It's the culmination of over two years of evolution - from hacky projects posted on Reddit to benchmark environments to philosophical reframings of what "computer use" even means.


I've had a front-row seat to this story. I co-authored Windows Agent Arena at Microsoft, and now I'm building Cua ([@trycua](https://x.com/@trycua) ) — open-source infrastructure for computer-use agents — alongside Dillon Dupont ([@ddupont808](https://x.com/@ddupont808) ), who built one of the very first browser agents back in 2023.


This is the story of how we got here.


## The Idea That Came Too Early


The concept of AI using computers isn't new. It dates back to December 2016.


That's when OpenAI released[Universe](https://openai.com/index/universe/) — a platform for training AI agents to use games, websites, and applications. The approach was remarkably similar to what we have today: AI controls programs via virtual keyboard and mouse, perceiving screen pixels through a VNC remote desktop. No source code access, no special APIs. Just pixels in, actions out. Universe shipped with over a thousand environments — Flash games, browser tasks, even GTA V and Portal. The goal was ambitious: train AI with "general problem solving ability—something akin to human common sense."


It didn't work. The vision was right, but the timing was wrong.


Years later, Karpathy ([@karpathy](https://x.com/@karpathy) ) reflected: "We actually worked on this idea in very early OpenAI (see Universe and World of Bits projects), but it was incorrectly sequenced — **LLMs had to happen first.** "


The missing piece wasn't better RL algorithms or more compute. It was language. AI needed to understand instructions, reason about goals, and plan multi-step actions. That required LLMs.


## The Prerequisite: AI That Can See


Before AI could use a computer, it needed to see one.


On September 25, 2023, OpenAI published the GPT-4V System Card, documenting their vision-enabled model. For the first time, a frontier LLM could process images alongside text. Six weeks later, at DevDay on November 6, 2023, OpenAI opened API access to GPT-4 with Vision.


The implications were immediate: if an AI could see images, it could see screenshots. If it could see screenshots, it could understand what was on a computer screen. And if it could understand the screen, maybe it could interact with it.


The prerequisite was in place.


## The Vision: LLM as Operating System


Days after DevDay, Andrej Karpathy posted a diagram that would prove prescient.


On November 10, 2023, he[tweeted](https://x.com/karpathy/status/1723140519554105733) what he called the "LLM OS" — a conceptual architecture where the LLM serves as the CPU of an operating system. The context window is RAM. Embeddings are the filesystem. And crucially, the browser, terminal, and other applications are peripherals connected to this central intelligence.


The framework was elegant: computer use isn't a separate capability — it's just adding peripherals to the LLM OS. Video input (screenshots), keyboard output, mouse control. The LLM doesn't need to understand "computer use" as a special skill. It just needs the right I/O.


## The First Builders


While Karpathy was sketching architectures, others were already building.


On October 21, 2023 — days after GPT-4V became available and three weeks before Karpathy's tweet — Dillon Dupont (now CTO of[Cua](https://github.com/trycua) ) created[GPT-4V-Act](https://github.com/ddupont808/GPT-4V-Act) . It was one of the first browser UI agents: a system that could take a screenshot, label interactive elements with numerical IDs ([Set-Of-Mark](https://arxiv.org/abs/2310.11441) ), let the AI decide what to click, and execute the action.


The pattern that would define the field emerged: **screenshot → understand → decide → act → repeat.**


In February 2024, Microsoft released UFO, a GUI agent for Windows - quiete a glimpse on the future of computer-use (back then we were calling this UI-Focused or Desktop Agents). The approach was similar: visual understanding of the screen, identification of UI elements + interpolation with the OS Accessibility Tree, and execution of actions. The research community was converging on a shared architecture.


But there was a problem. These systems worked in demos, but how well did they actually perform? Without rigorous evaluation, progress was hard to measure.


## Measuring Progress: The Benchmark Era


In April 2024, researchers from XLang Labs ([@TianbaoX](https://x.com/@TianbaoX) et al.) released[OSWorld](https://os-world.github.io/) — the first comprehensive benchmark for computer-using agents. It provided 369 tasks across Ubuntu, Windows, with real applications and execution-based evaluation.


The results were sobering.


The gap between human and machine performance was massive. Tasks that humans completed effortlessly — opening applications, filling forms, managing files — challenged even the most capable models. Scrolling, dragging, zooming: actions we perform without thinking presented fundamental difficulties. The results were sobering.


In September 2024,[Windows Agent Arena](https://github.com/microsoft/WindowsAgentArena) arrived — a project I co-authored with Dillon ([@ddupont808](https://x.com/@ddupont808) ) while at Microsoft. It offered scalable infrastructure for testing agents on Windows: with Azure ML integration, researchers could run hundreds of evaluation tasks in minutes instead of days. We learned firsthand how difficult it was to build reliable, reproducible environments for agent evaluation.


The benchmark era had begun. **You can't improve what you can't measure.**


Just before the major labs entered, Microsoft Research released[OmniParser](https://www.microsoft.com/en-us/research/articles/omniparser-for-pure-vision-based-gui-agent/) ([@MSFTResearch](https://x.com/@MSFTResearch) ) (October 8, 2024) — a screen parsing module that converts UI screenshots into structured elements using only vision. No HTML, no accessibility trees, just pixels. It paired a detection model (to find clickable regions) with a captioning model (to describe what each element does). OmniParser became a key building block for VLMs that were not trained on GUIs to use pixels as the action space — hitting #1 trending model on Hugging Face, achieving top results on Windows Agent Arena, and enabling pure vision-based agents.


## The Giants Enter


On October 22, 2024, Anthropic made computer use official.


With the release of[Claude 3.5 Sonnet's computer use capability](https://www.anthropic.com/news/3-5-models-and-computer-use) , Anthropic became the first major AI lab to offer a production API for AI-driven computer interaction. Their framing was ambitious: rather than building task-specific tools, they taught Claude "general computer skills — allowing it to use a wide range of standard tools and software programs designed for people."


The results on OSWorld jumped from the previous best of ~12% to 14.9% (screenshot-only) and 22% (with extended steps). Still far from human performance, but a significant leap.


Ethan Mollick ([@emollick](https://x.com/@emollick) ), writing in his newsletter[One Useful Thing](https://www.oneusefulthing.org/p/when-you-give-a-claude-a-mouse) , observed Claude executing over 100 independent moves without requesting permission. His conclusion: "Agents are going to be a very big deal indeed."


Nine days later, the open-source community responded. Browser-use ([@gregpr07](https://x.com/@gregpr07) ) launched on November 5, 2024 — a Python library for AI browser automation that would eventually accumulate over 77,000 GitHub stars. The infrastructure was becoming accessible.


Then, on January 23, 2025, OpenAI entered with[Operator](https://openai.com/index/introducing-operator/) , powered by their Computer-Using Agent (CUA) model.


The benchmark results set a new state-of-the-art:


The benchmark wars had begun.


## The Mainstream Moment


In March 2025, computer-use agents escaped the research lab.


[Manus](https://x.com/francedot/status/2016627257310384554#:~:text=Manus%20launched%20%E2%80%94%20and,things%22%20for%20them.) launched — and two million users joined the waitlist in a single week. Developed by the team behind[Monica.im](https://monica.im/) , Manus promised autonomous task completion: responding to emails, managing calendars, booking reservations. The shift was seismic. Computer use was no longer a developer tool or research benchmark. Regular users wanted AI that could "do things" for them.


That same month[Cua](https://www.ycombinator.com/companies/cua) ([@trycua](https://x.com/@trycua) ) joins YC X25. After building benchmarks at Microsoft, we saw what was missing: robust, open-source infrastructure for running computer-use agents safely. Sandboxed environments, SDKs, evaluation tools — the plumbing that would let developers build on top of this wave without reinventing the wheel.


The era of consumer computer-use agents had arrived. And the infrastructure race was on.


## The Plot Twist: Code as Action


Then came a reframe that changed everything.


In August 2025, researchers from[@SFResearch](https://x.com/@SFResearch) published[CoAct-1](https://arxiv.org/abs/2508.03923) : "Computer-using Agents with Coding as Actions." The core idea: let agents write and execute code instead of clicking through GUIs.


The architecture was hybrid. An Orchestrator analyzed each task and routed it to either a GUI Operator (for visual interactions) or a Programmer (for code execution). File management, data processing, system configuration — tasks that were tedious via GUI became trivial via code.


The results were striking:


## The Reframe: CLI as Computer Use


In December 2025, George Hotz (geohot) wrote a[blog post](https://geohot.github.io//blog/jekyll/update/2025/12/18/computer-use-models.html) that shifted how many thought about computer-use agents:


> "Turns out the idea wasn't a desktop emulator with a keyboard and mouse, it was just a command line."


He was talking about Claude Code — Anthropic's CLI tool for agentic coding. It didn't manipulate pixels or click buttons. It ran commands, read files, wrote code, and executed programs. For developer workflows, it was remarkably effective.


On January 1, 2026, Vercel CEO Guillermo Rauch ([@rauchg](https://x.com/@rauchg) )[expanded the argument](https://www.linkedin.com/feed/update/urn:li:share:7412636605791932416/) :


> "The fundamental coding agent abstraction is the CLI. It's not a UI or form factor preference, it's rooted in the fact that agents need access to the OS layer." "Coding agents are, at their core, computer-use agents. They run programs, create new ones, install missing ones, scan the filesystem, read logs. More than text editors, they're automating your computer at a low level."


For certain domains — software development, system administration, data processing — the CLI framing makes sense. These tasks are text-native. The terminal is their natural interface. CLI-based agents have OS-level access, work on any machine you can SSH into, and can scale horizontally. You can run a thousand of them in parallel.


But this framing has limits.


## The Space for GUI Agents


Not everything is text. Not everything should be. The CLI works when tasks can be expressed as commands and code. But large categories of computer use are fundamentally visual: design tools, video editing, CAD software, spreadsheets with complex layouts, applications with novel interfaces.


Consider Photoshop. A designer exploring color palettes, adjusting compositions, experimenting with filters. The feedback loop is visual and iterative. You try something, see the result, adjust. The "happy accidents" — the unexpected results that spark new directions — emerge from this interplay. Reduce it to code, and you lose the exploration.


The same applies to Premiere, Figma, Blender, Excel with charts and pivot tables. These tools were built for visual interaction. Their power comes from direct manipulation, not abstraction.


There's also the matter of reach. The CLI assumes technical users on systems they control. But most computer use happens in browsers, in apps, on devices where the terminal isn't an option. GUI agents can meet users where they are — any application, any interface, any platform.


And then there's the end-to-end loop. For tasks that span multiple applications — pulling data from a website, processing it in a spreadsheet, generating a report, emailing it — GUI agents can navigate the full workflow without requiring every tool to have an API or CLI.


The future isn't CLI versus GUI. It's knowing when to use each. Code-as-action for automation at scale. Visual agents for creative work and unfamiliar interfaces. The best systems will blend both — using code when efficient, falling back to vision when necessary.


This is why we built[Cua-Bench](https://github.com/trycua/cua) . GUI agents need rigorous evaluation — not just on fixed screenshots, but on the messy reality of changing interfaces, novel applications, and creative workflows. If we want agents that can handle Photoshop and Figma, we need benchmarks that test for robustness, not just pattern matching.


Your browser does not support the video tag.


## The Composable Future


Which brings us to today — and to Clawdbot. Created by Austrian developer Peter Steinberger,[Clawdbot](https://github.com/clawdbot) (now Moltbot after a trademark-driven rebrand) represents the next paradigm: composable, self-hosted, extensible agents.


What makes it different:


- **Self-hosted** : Runs entirely on your machine. Your data stays local.
- **Multi-model** : Works with Claude, GPT, or local models via Ollama.
- **Extensible** : Skills and plugins from ClawdHub — a public registry where anyone can contribute capabilities.
- **Accessible** : Control it through WhatsApp, Telegram, or any messaging app you already use.


The agent is no longer a monolithic product. It's a composition of parts. Need Fitbit integration? There's a skill for that. Want AI-powered web search via Kagi? Install the plugin. The user builds their own agent from components.


This isn't without tradeoffs. Security researchers have already demonstrated supply-chain vulnerabilities in ClawdHub — a poisoned skill could access months of private messages and credentials. The openness that makes the system powerful also makes it a target.


But the paradigm is clear: the future of computer-use agents is modular, and user-controlled.


## The Evolution in One Frame


Looking back, the progression follows a clear arc:


- 2023: "Can AI see a screen?" (GPT-4V)
- 2024: "Can AI click buttons?" (Claude Computer Use, Operator)
- 2025: "Can AI write code instead?" (CoAct-1)
- 2026: "Can AI be MY agent, built MY way?" (Moltbot)


Each step expanded what "computer use" meant. From visual perception to GUI automation to code execution to composable personal agents.


The through-line is agency. Each evolution gave AI more ways to act on the world — and gave users more control over how that agency is deployed.


## What Comes Next?


The pieces are in place. Vision-capable models. Benchmarks to measure progress. Hybrid architectures that combine GUI and code. CLI-based agents with OS-level access. Composable skill systems.


The question is no longer whether AI can use computers. It's how we want that capability to be packaged, controlled, and distributed.


Will the future be cloud-hosted agents from major labs, or local-first tools like Moltbot? Will capabilities be gated behind subscriptions, or open-sourced and community-maintained? Will agents operate autonomously, or remain tightly controlled by users?


These aren't just technical questions. They're questions about how we want to live with increasingly capable AI.


For my part, I'm betting on commoditization — of both the sandbox layer and the agents themselves. The infrastructure for running computer-use agents should be self-hostable, open, and owned by the developers who build on it. The agents should be composable, swappable, not locked to a single vendor.


That's why we built Cua as open-source from day one. The best outcomes happen when developers can experiment, iterate, and build on each other's work.


The story of computer-use is still being written. But looking at where we started — a failed experiment in 2016 — and where we've arrived, one thing is clear: the pace isn't slowing down.


*Francesco Bonacci is the CEO of Cua, building open-source infrastructure for computer-use agents. Previously at Microsoft, where he co-authored Windows Agent Arena.*


*Dillon Dupont is the CTO of Cua. He built one of the first browser UI agents (GPT-4V-Act) in October 2023 and co-authored Windows Agent Arena at Microsoft.*
