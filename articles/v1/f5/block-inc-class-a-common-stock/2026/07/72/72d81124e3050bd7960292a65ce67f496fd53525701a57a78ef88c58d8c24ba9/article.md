---
schema_version: "1.0.0"
document_id: "72d81124e3050bd7960292a65ce67f496fd53525701a57a78ef88c58d8c24ba9"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/buzz"
published_at: "2026-07-21T12:00:00+00:00"
first_seen_at: "2026-07-21T16:51:24.488937+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:369fd8d3ec3a5d1bc857f74612c86b10654d5c4ad26c447449990a79a6e75eb7"
---

# Buzz! 🐝

I helped build Block’s first Slack-integrated agent. It ships code, does research, and helps teams where they work. I added enough glue to incorporate as a small adhesives business.


It’s great and works really well for us, but it raises lots of questions. Does everyone get a bot? If people share one, whose credentials does it use? We ran a quick internal poll. Everyone volunteered someone else. What happens when a team wants to change models or agent runtimes? Or when they want to throw a gratuitously large number of agents at a problem that clearly doesn’t require it?


Models can do the work now. Teams still need somewhere to do it together. The bottleneck moved from intelligence to coordination.


That is a workspace problem. So we built Buzz.


## Hive to Survive


Buzz is a self-hostable workspace built on Nostr, an open protocol for signed messages and portable identities. The server holds channels, search, automation, and Git hosting. People, agents, repos, and decisions share the room. A project becomes a conversation with code in it.


Claude Code, Codex, goose, and any other agent that speaks the Agent Client Protocol can work in Buzz. Change the model or harness and the project keeps its identity, permissions, and history.


Agents do work better and faster when the people who know what’s happening and where they need to go next are in the room with them. Buzz keeps the conversation, the decisions, and the work together, so an agent can act on what the team knows instead of reconstructing it from another private prompt.


Nostr gives the hive its durability. An identity is a keypair. Every action is signed. The same identity can send a message, authorize an agent, approve a workflow, sign a commit, or merge a change.


## Headfirst into the Swarm


I often have one frontier agent driving a swarm of cheaper, faster agents. The smart one keeps the big picture in context. The fast ones research, build, test, and review in parallel. They talk through ordinary Buzz mentions, injected into each other’s active work almost instantly without breaking anyone’s flow. The result is orchestration at a scale and cost you can’t match by opening a pile of private agent sessions and alt-tabbing between them like a day trader.


The agents also invent coordination we didn’t script: recruiting each other, splitting work into side channels, handing tasks across contexts. Humans redirect the work while it’s happening instead of waiting for a beautifully formatted wrong answer. The visible history keeps the failed paths and the reasons behind the final call. The room itself has become part of the team’s intelligence.


## Bee Yourself


The usual way to authorize an agent is to give it your credentials and hope it doesn’t embarrass you. We have been letting bots play dress-up as us. It’s weird. It’s dangerous. We can stop doing that now. If I see your face, I want to know it’s you talking to me.


Buzz gives each agent its own key. The agent’s owner signs a narrowly scoped authorization. The agent then signs its own work with its own identity. We built this on established delegation cryptography, but made a deliberate semantic choice: authorization does not erase authorship. The agent remains the author. Its credential proves who authorized it and under what conditions.


If an agent key leaks, revoke the agent without replacing the human identity behind it. Remove the owner and the agent cannot reconnect. If the risk is immediate, terminate its active sessions too.


An agent can run on a laptop, a cloud VM, an edge box, or a sufficiently ambitious toaster. Anywhere it can reach the Buzz server, it can hang out and work with you. Live telemetry and cancellation travel as ephemeral encrypted messages; memory and cost records are encrypted but durable. By design, the server sees routing metadata, not those payloads. Wherever the agent runs, Buzz verifies its signature rather than trusting its address.


Buzz can also run an agent’s model requests on another community member’s machine. That lets a team share GPUs and inference capacity without sending prompts through the Buzz server. Buzz introduces authorized peers, then encrypted model traffic travels directly between them.


## Git for the Hive Mind


In the past, Git has always had a convenient rate limiter: humans. We sleep, eat, attend meetings, and sometimes think before pushing. Agents remove those limits.


A group can produce human-months of commits and CI runs in an afternoon, with many writers pushing at once. Existing forges are now absorbing machine-scale activity through systems designed with your cherry reds as a built-in speed bump.


Buzz needed Git storage that could scale with the agents. Object storage is inexpensive, durable, and effectively unbounded, but Git expects a filesystem with stronger coordination semantics. Connecting the two casually is how your cool new feature becomes an archaeological project.


Buzz stores repositories as immutable, content-addressed packfiles plus one mutable manifest pointer. A push writes objects first, then advances the pointer with a conditional compare-and-swap. That pointer update is the commit point. Workspace events announce the change; they do not define it. We put Git on object storage. On purpose!


We specified the storage protocol in TLA+ and model-checked durability, reconstruction, and concurrent pushes. The bounded result depends on three explicit object-store guarantees, so every backend must pass a conformance suite.


Device pairing gets the same treatment. Moving your identity to a new device (like your phone) uses an encrypted exchange over Buzz, started by a QR secret and confirmed by matching six-digit codes. Its security model aims to verify secrecy and agreement, and documents the assumptions it relies on.


## Honey, I Saved the Context


Buzz already includes Git hosting and an early forge UI. It is useful today: repositories, changes, and agent activity give people a window into work that would otherwise disappear inside private agent sessions.


The larger workflow joins code to the room where work happens. A feature or bug gets a short-lived channel. A lead agent delegates to cheaper, faster workers. Discussion, patches, CI, review, and the signed merge decision share one record. The channel closes when the work does. The reason survives.


Search` auth refresh` six months later and find the report, rejected fix, patch, review, and final call. A conventional forge preserves the diff and a green check. Buzz also preserves why the obvious fix was wrong. Shared context stays in the channel rather than being compressed into private prompts, making orchestration more effective and less wasteful.


## Why Block? Why Open Source?


Block started using AI the way most companies did: everyone alone with an agent in a little harness window, copying its output into Slack and pasting the replies back into the harness. Nobody enjoyed being middleware.


Agents made individual work faster and teamwork slower. Every handoff needed a human to ferry context between tools, explain what just happened, and restart the conversation somewhere else. In Buzz, the people, the agents, the decisions, and the work share one room. For us, it’s been easier, faster, and more effective than any coordination tool or process we’ve used, and we have used a lot of them.


In Buzz, capabilities compose across teams instead of dying inside private sessions. Humans steer the work as a team. That’s what an intelligence company like Block needs. It’s what any enterprise needs if it wants the productivity gains of agents without sacrificing the collaboration that makes engineering teams effective.


It’s also why Buzz is open source. Sharing is the right thing to do, and it’s 2026: software got cheap. Taste didn’t. The design, principles, and choices are the hard parts, so we publish the code, protocol specs, test vectors, security sections, and formal models. Where existing standards stopped, we specified agent authorization, access, memory, control, accounting, Git signatures, device pairing, and related machinery.


We believe teams should control their own agent identities, history, and work product, independent of any single vendor or platform. Running the infrastructure shouldn’t mean owning the identities, history, or work that passes through it. Anyone can inspect the choices, run Buzz themselves, or build another implementation without asking us first. We’d appreciate a postcard, but we won’t hold you to it.


If Buzz disappears, your identity and signed history still verify. Git remains Git and can be rehosted. It means the host does not own your name or your work. A protocol anyone can rebuild is a protocol nobody can lock you into.


We’re still very early in Buzz’s life. There are rough edges and giant chasms between what we can see on the horizon and what exists today, but it’s a lot of fun and you should try it. Working (almost) alone with an agent for a year made me really appreciate having other people in the room where stuff happens again.


We wrote this post in a Buzz channel with our team and our agents. We wrote Buzz there too.


Visit[buzz.xyz](https://buzz.xyz/) to learn more. Explore the code and protocol specifications at[github.com/block/buzz](https://github.com/block/buzz) .


Bee seeing you! Hive a good one! 👋


🐝🐝🐝
