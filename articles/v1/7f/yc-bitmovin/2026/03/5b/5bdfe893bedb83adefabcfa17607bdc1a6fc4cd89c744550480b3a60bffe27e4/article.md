---
schema_version: "1.0.0"
document_id: "5bdfe893bedb83adefabcfa17607bdc1a6fc4cd89c744550480b3a60bffe27e4"
company_key: "yc-bitmovin"
company: "Bitmovin"
source_id: "yc-bitmovin-news-import-596f48821b52"
canonical_url: "https://bitmovin.com/blog/hackathon-ai-question-to-action-streaming-workflows/"
published_at: "2026-03-30T14:48:00+00:00"
first_seen_at: "2026-07-27T08:05:57.697851+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:288afb32d4a359a270b7e108f4ee34163dae74b1b322eedbe9342cf3d15cfa28"
---

# Hackathon Spotlight: Rethinking Workflow Navigation with AI Agents like B.O.S.S

## TL;DR


- Engineers working with Bitmovin’s platform already have access to powerful tools for encoding, playback, and observability, but navigating between multiple dashboards, APIs, and documentation sources can create friction that slows teams down.
- Bitmovin built B.O.S.S. (Bitmovin One-Stop Shop) to solve this during an internal hackathon. B.O.S.S. is an AI agent prototype that unifies documentation, platform data, and workflow guidance into a single conversational interface, so engineers can go from question to action without switching context.
- Rather than a single monolithic AI, B.O.S.S. routes requests to specialized agents orchestrated by a central assistant for cohesive, contextual answers.
- B.O.S.S. can explain why an encoding job failed, surface relevant documentation, pull observability data, and suggest next steps, all within one interaction. This reduces the need to escalate to teammates or support.


---


## Table of Contents


Working with video workflows today means dealing with a lot of moving parts. Encoding, playback, and observability each come with their own APIs, documentation, and ways of working. The capability is there, but getting to the right answer still takes time, whether that’s searching docs, checking specs, or figuring out where something lives in the dashboard. Even for teams that know the platform well, there’s still friction in connecting everything together, especially when something needs to be understood or fixed quickly. In many cases, getting unblocked still depends on asking someone else or going through support, rather than having everything you need directly accessible.


It’s not really a capability problem, it’s an access problem.


During a recent hackathon, we explored what a more direct way of interacting with Bitmovin’s platform could look like. The result is B.O.S.S. (Bitmovin One-Stop Shop), an internal prototype that brings documentation, platform data, and guidance into a single interface. It gives users the context and guidance they need to understand and move through workflows more efficiently.


In this blog, we walk through the problem we set out to solve, how B.O.S.S. works, and what it shows about the future of interacting with streaming workflows.


## **The problem: where workflows slow down**


An encoding fails. You have the error, but not enough context to know what actually caused it, so you start digging. You check the logs, look at the configuration, then open documentation to validate what you’re seeing. At some point, you’re comparing it against an example or a spec just to be sure. You’re not blocked, you’re just piecing things together, and that’s where most of the time goes.


That pattern shows up in a few consistent ways:


- Understanding why something failed or behaved unexpectedly
- Mapping what you’re seeing back to how something was configured
- Switching between encoding, player, and observability to get the full picture
- Double-checking specs or examples before making a change
- Knowing what you need, but not exactly where to find it


None of this is difficult on its own, it just doesn’t live in one place. So the workflow becomes indirect, you check a few sources, validate what you can, and if it’s still unclear, you ask someone else or try again. There’s no direct path from question to answer while staying in context, and that’s where things slow down.


## **What we built: introducing B.O.S.S.**


To explore a more direct way of moving through workflows, we built B.O.S.S. (Bitmovin One-Stop Shop) during a recent hackathon. It’s an internal prototype that brings documentation, platform data, and guidance into a single interface, allowing users to ask questions and get the context they need without switching between tools.


Instead of searching across dashboards, docs, and APIs, B.O.S.S. acts as a single entry point. A user can ask about an encoding job, understand why something failed, find relevant documentation, or get guidance on what to do next, all within the same interaction.


It doesn’t replace workflows or automate them. B.O.S.S. gives users the context and guidance they need to understand and move through workflows more efficiently.


## **How it works: from question to action**


B.O.S.S. starts with a simple interaction, a user asks a question. From there, it interprets the intent and routes the request to the right source, whether that’s platform data, documentation, or examples. The goal isn’t just to return information, but to provide enough context to understand what’s happening and what to do next.


Under the hood, this is handled through a set of specialized agents that focus on different parts of the platform. Instead of relying on a single system to do everything, each agent is designed to handle a specific type of request, such as documentation, encoding workflows, or SDK examples. These are then orchestrated through a central assistant that connects everything together.


In practice, this means a single interaction can:


- Retrieve details about an encoding job and explain what happened
- Surface relevant documentation or examples based on the question
- Pull observability data to provide additional context
- Guide users toward the next step, including where to go in the dashboard


These agents are connected to underlying tools such as documentation search, SDK repositories, and platform data sources, allowing B.O.S.S. to respond with real, actionable information rather than generic answers. The result is a more direct path from question to action, where users stay in context instead of navigating between multiple systems.


## **What we learned**


Building B.O.S.S. quickly exposed how much of the challenge isn’t in accessing data, but in connecting it in a way that’s actually useful. The underlying information, whether platform data, documentation, or examples, already exists. The value comes from bringing it together in context and making it immediately actionable.


It also highlighted how important the right tooling is. Early on, we experimented with newer SDK options, but limitations in stability and feature support meant we had to shift to a more mature approach. That decision made a significant difference in how quickly we could move and iterate.


Even within a short timeframe, the system processed a large volume of interactions, requiring hundreds of millions of tokens to build and test the experience. This reinforced both the potential and the cost considerations of agent-driven systems, especially when they are connected to multiple data sources.


## **Conclusion**


The complexity of streaming workflows isn’t going away, but how developers interact with that complexity can change. The challenge is no longer just building the right capabilities, it’s making them easier to access, understand, and act on in the moment.


B.O.S.S. is an early step in that direction. It shows how bringing context, guidance, and platform data into a single interface can reduce friction and make workflows easier to navigate, without changing the workflows themselves.


---
