---
schema_version: "1.0.0"
document_id: "c89b52a4e73b019b9ccf4b1c9839e40106231e0cd0924fd938fb975869b68b52"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/shared-filesystems-for-agents-at-mcp-dev-summit-mumbai-2026"
published_at: "2026-07-09T13:19:44+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:43:05.676547+00:00"
content_hash: "sha256:1ef37a497a714093315fe3ee0881cf6da64069fc09d9db70dd583006b9ce0684"
---

# Shared filesystems for agents at MCP Dev Summit Mumbai 2026

A few weeks ago, I spoke at the inaugural[MCP Dev Summit Mumbai](https://events.linuxfoundation.org/mcp-dev-summit-mumbai/) , part of a global series of events by the[Agentic AI Foundation](https://aaif.io/) . The talk covered three challenges that come up when building and running multi-agent systems at scale: integration complexity, code isolation, and state preservation. This post focuses on the third challenge, and why we at Blaxel think the answer is a shared filesystem.


The common approach to coordinating agents working the same task is to pass serialized JSON between them: state bundled into messages and transferred from one agent to the next. This works fine for simple workflows, but both durability and complexity become harder to manage as swarms grow.


In my talk, I examined the feasibility of replacing this JSON messaging approach with filesystem-based storage. Under this approach, agents read and write their results as simple Markdown files to a shared filesystem.


This lets the state persist beyond any single message and also removes size constraints when dealing with large or binary artifacts. An additional - and often underrated - benefit is that it significantly simplifies debugging, because you can always go back and look at the files an agent left behind to figure out where things went wrong.


Blaxel's[Agent Drive](https://docs.blaxel.ai/Agent-drive/Overview) is designed to support these types of agentic workflows:


- It's a distributed filesystem that can be mounted to multiple sandboxes or agents simultaneously, giving every agent a common workspace.
- It supports concurrent read-write access, so agents can write outputs and pick up inputs without hitting conflicts.
- Replication is built in, so generated artifacts can be persisted, reused, and audited.
- It has[granular access control](https://docs.blaxel.ai/Agent-drive/Permissions) , enabling the same Agent Drive to be mounted read-write for one agent and read-only for others


After walking through this filesystem-based approach (plus discussing solutions to the other challenges), I demonstrated it live: a small swarm of agents working on a task, coordinating with each other through files written to an Agent Drive.


The demo went well and once I was off-stage, numerous developers came up to me with follow-up questions, both about[Agent Drive](https://blaxel.ai/agent-drive) and Blaxel's[other platform features](https://blaxel.ai/) more broadly. We talked about scalability, data protection, performance, and applicability to different use cases.


Beyond the talk itself, this was a great event. It was extremely well organized, it was packed with excellent technical talks, and I made connections with many people building in the agentic AI space in India. Here’s to the next one!


> If you're building multi-agent workflows and Agent Drive sounds like it might be a good fit for you, it's currently in private preview.[Join the waitlist](https://blaxel.fillout.com/t/pbTXmanx3Sus) to get access.
