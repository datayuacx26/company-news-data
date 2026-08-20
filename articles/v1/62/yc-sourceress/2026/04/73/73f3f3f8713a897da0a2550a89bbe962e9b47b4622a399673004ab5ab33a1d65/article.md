---
schema_version: "1.0.0"
document_id: "73f3f3f8713a897da0a2550a89bbe962e9b47b4622a399673004ab5ab33a1d65"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/blueprint"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:7e59e67059dd956dadaaa527076c8ea0d6e4a3abb712bc647dc1bc2e45fa46ee"
---

# Blueprint: One-shot bigger coding tasks

A few days ago, I tried getting an agent to port a rendering pipeline from Rust to the Godot game engine. Instead of it integrating the renderer with the engine’s existing asset management system, it decided to port over my code literally and wrote its own sprite loader. *Sigh…*


Without explicit guidance, coding agents make a lot of assumptions. They don’t ask questions when they don’t understand what you need, so they build features that don’t align with your vision and leave you with messy code you’ll need to clean up. This makes it much harder to use agents for complex tasks, since you’re often stuck throwing away code, re-prompting, and babysitting them to make sure they build what you actually want.


Existing planning tools, including Claude Code Plan Mode, don’t feel like they solve this problem. In practice, they function as a way for AI agents to think before writing code, not as a system for the agent to discover what the programmer wants. I’ve found that even if I use something like Claude Code Plan Mode, the agent still ends up railroading me into following its ideas rather than my own.


To address this, we built Blueprint: a planning copilot that elicits your intent and helps you tackle ambitious tasks efficiently.


## How it works


Blueprint is available as both a[set of skills](https://github.com/imbue-ai/blueprint/tree/main#install) compatible with most coding agents and as an[extension for VSCode, Cursor and Windsurf](https://github.com/imbue-ai/blueprint/tree/main#also-available) . When you give Blueprint an initial prompt, it reads your codebase, then asks questions to learn about your intent. Most questions have reasonable multiple-choice options, though you can always answer with free-form text. You can answer as few or as many as you like, skipping any question you have no opinion on. Once you’re done answering, Blueprint writes a detailed plan based on your responses.


Because Blueprint researches your codebase first, its questions are grounded in real code rather than abstract assumptions. They often raise both technical and UX concerns programmers didn’t initially think about. Beyond informing the agent, this process helps you think through what you want to build. The questions are thorough enough that most of our users trust the agent has a clear grasp of their intention, and using Blueprint often becomes as simple as giving an initial prompt, answering some questions, and handing the plan off to an implementer agent without even reading it.


## What early users say


To make sure that Blueprint is actually useful for real-world tasks, we ran beta tests frequently. Most of our early beta testers now use Blueprint regularly, and find it immensely useful for their day-to-day software development.


When asked to compare Blueprint to Claude Code Plan Mode, one user said:


> “
>
>
> “\[Blueprint\] is strictly better. The problem with Claude Code Plan Mode is that it asks you questions to check the box, not thinking super hard about it. This is like a smarter, better, deeper way of actually making the plan, which matters when the plan is large.” ”


Our users also consistently find Blueprint’s questions very useful for developing the plan and thinking through what they want to build:


> “
>
>
> “It got me to think of a bunch of questions before doing the thing.” ”


> “
>
>
> “If you tell a human to implement something, they ask you questions. It’s not that you haven’t thought of the answers, you just wouldn’t tell them all at once.” ”


> “
>
>
> “Blueprint is a much more guided experience. It makes it almost impossible to ‘miss’ an important detail, because it will proactively raise questions along the way until there’s enough detail to get to a concrete implementation plan.” ”


Even the most skeptical of our beta testers were eventually won over. Cat put it briefly:


> “
>
>
> “Using Blueprint… I like it a lot actually.” ”


## How we built it


Although Blueprint’s final workflow is quite simple, it took a lot of effort to incrementally develop it through rounds of internal testing and user feedback. We found that to create a truly useful planning copilot, the developer experience often matters a lot more than the final plan quality, and many of our initial assumptions turned out to be wrong.


For instance, when I first started working on Blueprint, I implicitly assumed that the main value it would provide was the plan. I thought that users would want to comb over comprehensive plans to ensure the agent fully understood what they wanted. In our testing, however, we quickly discovered that early users found the questions to be the most useful part of Blueprint, and we’ve leaned the workflow heavily in this direction.


## Beyond coding


We think Blueprint and tools like it will be incredibly powerful, especially as AI tools expand into the wider world beyond coding. While AI agents have become very competent at executing tasks, they often risk becoming the proverbial genie that grants your wishes in the worst possible way. Because of this, we think it’s important to develop AI systems that proactively ask questions to understand the user’s intention, rather than working from possibly mistaken assumptions.


As these systems become more and more powerful and we become increasingly reliant on them in our daily lives, we think it’s important to ensure that the AI ecosystem remains open. As such, we are open-sourcing Blueprint, and we invite the community to use it, extend it, and build alongside us.


## Getting started


Blueprint is available as a set of skills that can be installed by running:


```text
sh
```


If you prefer using VSCode, Cursor or Windsurf, you can install the extension[here](https://github.com/imbue-ai/blueprint/tree/main#also-available) .


Blueprint is open source — check it out and share feedback at[GitHub](https://github.com/imbue-ai/blueprint) ! Come chat with the team on[Discord](https://discord.gg/GvK8MsCVgk) .
