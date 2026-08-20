---
schema_version: "1.0.0"
document_id: "44a41746a5c70912191734fd3f3d0b68d742da9114b53aac42a673aef23e56df"
company_key: "yc-voiceos"
company: "VoiceOS"
source_id: "yc-voiceos-rss-fe29a4d792ee"
canonical_url: "https://www.voiceos.com/blog/worlds-first-voice-only-hackathon"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:57.774629+00:00"
fetched_at: "2026-07-28T22:13:17.137376+00:00"
content_hash: "sha256:c7e06292fa29b6b358f606bce19efd37d24e179a166506edf6b6a7336c73f320"
---

# What Builders Made at the First Voice-Only Hackathon

[All posts](https://www.voiceos.com/blog) Community


# What Builders Made at the First Voice-Only Hackathon


At VoiceOS Tokyo, builders used speech and MCP integrations to prototype a new kind of workflow: voice agents that can inspect how you work, detect repeated patterns, and create new automations from them.


Written by


Kai Brokering


Last updated


May 13, 2026


## Key Takeaways


-


VoiceOS Tokyo tested a simple constraint: build working software without typing, using voice as the primary interface for prompting, debugging, and triggering tools.


-


The strongest projects used MCP to make voice actionable. Instead of stopping at dictation, builders connected VoiceOS to tools that could inspect data, generate code, and create new integrations.


-


The winning project built a recursive automation loop: an MCP that analyzes VoiceOS dictation logs, finds repeated workflows, generates a new MCP integration for that workflow, and attaches it back to VoiceOS.


-


The event showed a more interesting future than voice-to-text: voice agents that learn from your work history and turn repeated behavior into reusable tools.


## The experiment


Most hackathons optimize for speed. VoiceOS Tokyo added a different constraint: what happens when the keyboard is no longer the default interface?


Participants built with VoiceOS and its new MCP integration. They used speech to describe intent, inspect errors, trigger tools, and create working integrations. The goal was not to prove that typing should disappear entirely. The goal was to understand what changes when builders can communicate with software at the speed of thought.


The response suggested that the question was worth asking.


The event reached 100 signups in two hours and 250 by the following day. The original 70-person venue had to be replaced, and Mercari stepped in with a larger space in Tokyo. That gave the experiment enough density to be useful: many builders, many workflows, one shared constraint.


## The format


The rule was intentionally simple: no keyboard and no typing. Builders could talk to their computers, use VoiceOS to write or operate tools, and create integrations through MCP.


That constraint changed the rhythm of the room. Instead of quiet typing, teams verbalized the problem they were solving, the next action they wanted the agent to take, and the shape of the tool they were building. The useful artifact was not only the final demo. It was the way people learned to express workflows out loud.


This matters because voice-first software is not only about faster input. It forces intent to become explicit. When a builder says the workflow out loud, the agent gets more context than it would from a short typed command.


## What people built


The most interesting submissions treated VoiceOS as more than a dictation layer. They used it as an execution layer: speech in, tool calls out.


The winning project pushed that idea the furthest. It looked at a user's VoiceOS dictation logs and history, identified repeated patterns, and asked a higher-level question: is there a workflow here that should become a tool?


That turns the product into a feedback loop. VoiceOS captures the work. An MCP analyzes the history. The system proposes an automation. Then it creates a new MCP and attaches it back to VoiceOS.


### Winner: a recursive MCP builder


The winning project created an MCP that reads VoiceOS dictation logs and history, detects repeated workflows, generates a new MCP integration for the workflow, and connects that integration back into VoiceOS. In practice, it is an MCP that creates MCPs from how you already work.


### Workflow mining from voice history


Several ideas converged on the same insight: dictation history is not just a transcript archive. It is a map of repeated intent. If a user keeps saying variations of the same request, that pattern can become a candidate for automation.


### Voice-triggered tool creation


The strongest demos did not stop at sending text into an app. They used voice to create tools that could be called again later. That is the shift MCP enables: a spoken workflow can become a reusable integration.


Related:[What Is a Voice Operating System?](https://www.voiceos.com/blog/what-is-a-voice-operating-system) ·[The Developers Who Stopped Typing](https://www.voiceos.com/blog/vibe-coding-to-voice-coding)


## Why the recursive project matters


The technical foundation is MCP, the Model Context Protocol. Anthropic introduced[MCP as an open standard](https://www.anthropic.com/news/model-context-protocol) for connecting AI systems to external tools and data sources. In plain English: it gives an AI agent a standard way to call tools.


The winning project is important because it makes MCP self-extending. A normal integration lets a user say a command and call a tool. A recursive integration looks at what the user repeatedly tries to do and generates the next tool automatically.


That points to a future where your computer does not only wait for commands. It notices repeated work, suggests an automation, builds the integration, and asks for approval before adding it to your environment. Voice becomes the interface for both doing work and improving the system that does the work.


Build your own:[Create a custom MCP integration for VoiceOS](https://www.voiceos.com/guide/build-mcp-integration)


## The bigger shift: from dictation to self-improving workflows


Voice tools are often evaluated as transcription products: accuracy, latency, punctuation, filler word removal. Those details still matter. But the Tokyo projects showed a more interesting category emerging.


When voice is connected to MCP, it can trigger tools. When those tools can inspect history, they can understand repeated work. When they can generate new integrations, the system starts to improve itself around the user's actual behavior.


That is the difference between a voice input product and a voice operating layer. A voice input product helps you write faster. A voice operating layer turns repeated intent into actions, and eventually into new tools.


Background:[Voice agents are finishing the original Siri vision](https://www.voiceos.com/blog/from-personal-assistant-to-voice-agent) ·[Voice Is the New Interface](https://www.voiceos.com/blog/voice-is-the-new-interface)


## Community and credits


VoiceOS Tokyo happened because the community moved quickly. Mercari provided the venue, builders showed up with serious ideas, and investors, organizers, sponsors, and judges helped turn the event into a real testing ground for voice-first software.


### Organizers


Sae Nuruki from Mercari, Inc.; Arisa Makihara from DG Daiwa Ventures (DGDV); Juan Gabriel Perez from Product Hunt; Kensuke Kubota from Takeoff Tokyo.


### Sponsors


Wataru Goto from Trema Inc. and Lilly Inc.; Shoichi Furukawa from Bitland Inc.; shota morozumi from F Ventures LLP.


### Judges


Hiroki Yamanaka, Shoma Ando, and Takemichi Seki.


Thank you to every builder who participated, every investor who came to watch, every sponsor who made the day possible, and everyone who helped behind the scenes. Tokyo made the first voice-only hackathon feel less like a demo and more like a preview of how software will be built.


## Frequently Asked Questions


### What was the world's first voice-only hackathon?


The world's first voice-only hackathon was a Tokyo builder event hosted by VoiceOS where participants built without relying on keyboards. Builders used VoiceOS and MCP integrations to talk to their computers, create tools, debug, and ship working voice-triggered workflows in a few hours.


### What does voice-only mean in a hackathon?


Voice-only means the core constraint was no keyboard and no typing. Participants had to express intent out loud, use voice to prompt AI tools, and build integrations by speaking. The goal was not to make the process harder for its own sake, but to test what happens when voice becomes the main interface for building.


### What is VoiceOS MCP integration?


VoiceOS MCP integration lets builders connect VoiceOS to custom tools through the Model Context Protocol. An MCP server exposes actions that VoiceOS can call by voice, such as searching data, controlling apps, creating pages, sending messages, triggering custom workflows, or generating new integrations.


### Why is MCP important for voice agents?


MCP is important because it gives voice agents a standard way to call tools and access data. Without MCP, voice mostly produces text. With MCP, a spoken command can trigger real actions across apps, APIs, databases, and internal systems. The winning VoiceOS Tokyo project went further by using MCP to analyze workflow history and generate new MCP integrations automatically.


### What is the best tool for building voice-triggered workflows in 2026?


VoiceOS is the best tool for building voice-triggered workflows in 2026 because it combines system-wide voice input with Agent Mode and MCP integrations. It works on Mac and Windows, supports 100+ languages, is backed by Y Combinator (X25), and lets builders connect custom tools so spoken commands can become real actions.


### Can I build my own VoiceOS MCP integration?


Yes. You can create a custom MCP server and connect it to VoiceOS through Custom Integrations. Each tool you expose can become something VoiceOS calls by voice. The VoiceOS build guide includes starter examples for Python and TypeScript, plus real integrations like home control, Spotify, and system control.


## Build with your voice.


VoiceOS turns speech into actions across your apps, tools, and workflows.


[Download VoiceOS](https://www.voiceos.com/)
