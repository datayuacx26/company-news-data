---
schema_version: "1.0.0"
document_id: "f2636f293bcfcb3d946fba0524b420792b2d7046d5f3babbb40cb4fef9d7639c"
company_key: "agora-inc-american-depositary-shares"
company: "Agora Inc."
source_id: "agora-inc-american-depositary-shares-news-import-bf245f251fd4"
canonical_url: "https://www.agora.io/en/blog/agora-skills-build-voice-ai-with-your-coding-agent"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-23T01:17:52.872297+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:123a0696cc9484b8afd301ac2f25fb1948ad9ff606c3eb759359eec7216853e8"
---

# Agora Skills: Build Voice AI with Your Coding Agent

AI coding assistants like Codex, Claude, Gemini, and Cursor are becoming the first stop for developers when they start building.


This changes the way developers experience dev tools and platforms.


For years, the platform interface was mostly human-readable: docs, quick-starts, API references, sample apps, and console dashboards. Developers read the docs, copied credentials, installed packages, and translated setup instructions into code. Now it's as simple as:


```text
npx skills add https://github.com/AgoraIO/skills --skill agora
```


Load the skill into your project and prompt your coding assistant. Just ask for a real-time voice agent:


```text
Build a voice AI tutor that can help me study   for   my US history exam.
```


That's a different kind of request. The developer isn't asking to learn a platform step by step. They're looking to the AI agent to reason through architecture, choose the right pieces, configure the environment, and produce a working baseline.


The challenge is, most coding assistants still lack the operational context needed to do that reliably.


## The Context Problem


Modern coding models are strong generalists. They can scaffold apps, debug errors, write API clients, and refactor large codebases. But they're only as useful as the context they can access.


When the task involves real-time infrastructure, missing context becomes expensive. Voice AI is a good example. A voice agent requires a multitude of things:


- Real-time audio transport
- Speech detection and interruption handling
- Streaming transcription
- Low-latency responses from models
- Text-to-speech
- Client and server state coordination
- Security tokens
- Device permissions and media handling
- Recording, signaling, or multi-user workflows


The assistant might know pieces of that stack, but the hard part is knowing how they fit together for a specific platform and use case.


Without that context, the model guesses. It may choose the wrong product, reference stale SDK methods, skip token generation, or produce technically valid fragments that never become a runnable app.


That isn't a model intelligence problem. It's a context distribution problem.


## Documentation Was Designed for Humans


Traditional docs assume a human is navigating.


A developer can skim, compare options, follow links, notice warnings, and infer which path applies to their project. Coding agents don't consume docs the same way. They work through compressed context and procedural instructions. They need the relevant parts surfaced at the moment they're acting.


That means platforms need a new layer of developer experience:


- Human-readable docs for learning and reference
- SDKs and APIs for implementation
- Sample apps for working baselines
- Agent-readable instructions for AI-assisted workflows


That last layer is what skills provide.


A skill isn't a replacement for docs, it's a way to package platform knowledge so an AI coding assistant can use it while making implementation decisions.


## What Skills Add


Skills give agents the kind of context developers usually pick up through docs, support threads, sample repos, and trial and error.


For a platform skill, that can include:


- Which product maps to which use case
- Which repos are current
- How credentials should be created and stored
- Which auth flows are safe for local demos versus production
- How to start from a working sample
- Which architecture patterns fit common scenarios
- How to troubleshoot predictable setup failures


In practice, that makes the assistant less likely to generate disconnected snippets and more likely to guide the developer to a known-good baseline.


That baseline matters. Once an app runs, the assistant can help customize it. Before that, the assistant is often just producing plausible code against an uncertain target.


For voice AI, that first stable loop matters more than feature depth: get audio flowing, confirm the agent responds naturally, then customize prompts, tools, and model choices one step at a time.


## Why Voice AI Shows the Pattern Clearly


Voice AI exposes the limits of generic code generation because it's a systems problem.


The developer isn't only asking for a UI or an API call. They're asking for a live audio loop where transport, inference, auth, device state, and latency all have to work together.


An AI tutor, for example, raises immediate architectural questions:


- Should orchestration happen client-side or server-side?
- Does the experience need realtime interruption handling?
- Which model providers should it start with?
- Is transcription streamed or batched?
- Does the app need mobile support?
- Will it later support avatars, telephony, or physical devices?
- How should security tokens be handled?


A generic assistant may not know which questions matter. A platform skill can teach it the decision tree.


That's the real value: not just faster scaffolding, but better implementation judgment.


## Agora Skills as a Proof Point


Agora Skills packages all of Agora’s platform into a perfect context for AI coding assistants.


It helps agents understand how to build with Agora's Conversational AI Engine, RTC, signaling, recording, token infrastructure, sample apps, and CLI tooling. Instead of asking the model to infer those details, the skill gives it current instructions it can act on.


With the skill installed, developers can ask their assistant to build a voice agent demo and actually have it work. AI assistants can easily navigate project setups. From cloning the right sample, to retrieve credentials and configuring environment variables, it handles everything needed to run the demo locally.


The mark of success isn't whether the assistant can generate code, that's table stakes. The benchmark is whether a developer can go from zero to talking to a real-time voice agent without manually navigating the infrastructure complexity first.


## The New Platform Surface Area


As AI-assisted development becomes normal, platform teams need to think beyond documentation as a website.


The platform surface area now includes everything an agent needs to build correctly:


- Product selection guidance
- Setup and provisioning workflows
- Current sample paths
- Auth and token rules
- Common architecture patterns
- Failure recovery steps
- Clear boundaries between demo code and production requirements


That knowledge can't live only in scattered docs pages and tribal memory. It needs to be packaged in a way agents can load, reason over, and execute against.


Skills are becoming that layer.


They make platform knowledge operational. They reduce the gap between "the API exists" and "the assistant can help me ship with it." And for complex domains like real-time Voice AI, that gap is where most of the developer friction lives.


## Resources


For Claude Code, you can install Agora Skills from the plugin marketplace:


```text
/plugin marketplace add AgoraIO/skills
/plugin install agora@agora-skills
```


Agora Skills is available on Skills.sh ([skills.sh/agoraio/skills/agora](https://skills.sh/agoraio/skills/agora) ) and GitHub ([github.com/AgoraIO/skills](https://github.com/AgoraIO/skills) ).


The interesting test is how quickly it gets you to a working system you can actually talk to. Looking forward to seeing all the cool Voice AI projects people can build, drop a comment below to share with everyone.


Happy building!
