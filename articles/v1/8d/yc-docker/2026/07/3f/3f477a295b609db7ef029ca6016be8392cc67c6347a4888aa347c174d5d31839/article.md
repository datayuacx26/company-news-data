---
schema_version: "1.0.0"
document_id: "3f477a295b609db7ef029ca6016be8392cc67c6347a4888aa347c174d5d31839"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-rss-8dbda93e62b5"
canonical_url: "https://www.docker.com/blog/ai-engineer-worlds-fair-2026-the-runtime-is-where-agent-trust-is-won/"
published_at: "2026-07-14T13:00:00+00:00"
first_seen_at: "2026-07-20T23:20:22.390152+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:f7b73300d23467fea5ec441549a9eee75d3bbb2538c54c21ea1df9e68c3b9156"
---

# AI Engineer World’s Fair 2026: The Runtime Is Where Agent Trust Is Won

We spent the week at AI Engineer World’s Fair in San Francisco, on stage and on the floor. Here’s what we heard, and where we think it lands for anyone building with agents.


## The SDLC is being rebuilt in public


This week at AIE felt like a synthesis of what’s been playing out in developer tools for the last few years, for anyone who’s been watching. The software development lifecycle is reshaping itself into an AI-native SDLC, and the industry is naming the new jobs and developer concerns that come with this rapid transformation.


The proof was in the track list: Evals, Context Engineering, Harness Engineering, Memory, Sandbox & Platform Engineering, Inference, plus a whole thread on “software factories.” Two years ago most of these phrases were far from being thought of as categories. Now each one is a discipline with its own sessions, its own vocabulary, and its own crop of companies on the expo floor built to solve that single problem.


So what were most talks about? A little bit less of “can agents and AI do this,” and more of “given this way of building, what decisions and trade offs do we need to think about?” Evals, loops, harnesses, context, memory, isolation, cost. None of this is brand-new, but it’s all getting a whole new level of mindshare, as developers work out the new shape of creating software with AI. Even the model labs spent much of their stage time on how you build *with* the model: the integration API, the harnesses, the ergonomics, rather than the model itself.


## The job we care most about: securing where agents run


Of all those emerging disciplines, sandboxing is the one that hit critical mass this year. There was a full track dedicated to sandbox and platform engineering, and the sessions inside it were still working out what a sandbox should even be: full VM, lightweight runtime, Kubernetes, something purpose-built. Talks focused on concerns such as running agentic sandboxes at scale and comparing isolation technologies head to head.


This is the job Docker showed up to talk about, across three sessions.


### Give agents more freedom by giving them less surface


Our EVP of engineering, Tushar Jain, gave the mainstage talk: “Unlock Agent Autonomy: The Runtime for AI-Native Systems.” The actors have changed – agents read and write whole codebases, spawn subagents, install dependencies, and call APIs across laptops, CI, cloud, and org boundaries, often unsupervised. Teams leaning into this shift are moving fast, but most organizations still won’t let agents run autonomously, not because the model isn’t capable, but because trust isn’t there yet. This thinking draws on a concept security researcher Simon Willison has written about, the “lethal trifecta”: any useful agent tends to end up with access to private data, exposure to untrusted content, and the ability to act in the outside world, all three, by design. No prompt or policy doc gets rid of that. The durable fix lives one layer down, at the runtime, which is where we spent the last decade: isolation, network policy, trusted images, credentials. Agents are just the next workload.


### An agent doesn’t have to be malicious to be dangerous


Rowan Christmas, a staff product manager at Docker, made the risk concrete. In “YOLO Mode, Safely: microVM Sandboxes for Any Agent,” he ran a coding agent on his own laptop with nothing but read access, and no sandbox or unusual permissions. Within a few minutes it had pieced together a surprising amount about his online banking activity from what it could passively see. A destructive command like rm -rf is the obvious fear, but the mundane can bring risk: read access, plus untrusted content, plus the ability to act, is already enough to do damage. An agent doesn’t have to be malicious to expose you. It just has to be able to see. The alternative Rowan showed puts each session in its own Docker sandbox based on a microVM, with a boundary you define across filesystem, network, and tools. It can run Claude Code, Cursor, Codex, or whatever you’re driving.


Once an agent can install packages, run Docker, and reach the network, which describes most genuinely useful agents, a hardware boundary buys you something you can’t easily bolt on later. And where much of the scale conversation is cloud-first, built for fleets of agents running server-side, Docker’s approach starts first on the laptop the developer already uses, because that’s where most people actually run agents today. (We go deeper on the reasoning in[“Why microVMs”](https://www.docker.com/blog/why-microvms-the-architecture-behind-docker-sandboxes/) and our[comparison of sandboxing approaches](https://www.docker.com/blog/comparing-sandboxing-approaches-ai-agents/) , including what the isolation costs you, because it isn’t free.)


### Nobody’s reviewing what your agents just installed


The third talk covered the tool layer. Jim Clark, a principal software engineer on our MCP team, spoke about “Who Approved That MCP Server? Governing the Tool Layer,” and opened with a line that got knowing laughs: “shadow MCP”. Developers install MCP servers faster than security can review them, and an unvetted server is a direct line to your data. That worry was all over the event, not just our session. Jim’s demo put every server behind one org-managed catalog, vetted, signed, default-deny on anything unapproved, with the policy enforced live on stage.


## Where this leaves us


So how does it come together? An agent is only as trustworthy as the boundaries around it, and those boundaries live in three places: what it builds on, where it runs, and what it can reach. Miss any one of them and the other two won’t cover for you. A hardened image dependency is no help if the agent can still read your whole filesystem unsandboxed, and a locked-down sandbox is no help if the agent can call an unvetted MCP server straight out of it.


That was the case Docker made all week: harden what agents build on, isolate where they run, control what they can reach, and govern all three from one place. We think this is the part that has to be solved first, because it’s where AI-native developers will start building the apps of the future.


Further reading:


- [Docker Sandboxes](https://docs.docker.com/ai/sandboxes/) run standalone (brew install docker/tap/sbx)
- [Docker AI Governance](https://www.docker.com/blog/introducing-docker-ai-governance/) ties sandbox and MCP policy into one console.
- [MCP Catalog, Toolkit, and Gateway](https://www.docker.com/products/mcp-catalog-and-toolkit/) are in Docker Desktop today
- [Docker Hardened Images](https://www.docker.com/products/hardened-images/) are a drop-in change to your FROM line
