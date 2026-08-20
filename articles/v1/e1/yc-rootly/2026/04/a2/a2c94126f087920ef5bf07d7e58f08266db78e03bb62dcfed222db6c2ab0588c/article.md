---
schema_version: "1.0.0"
document_id: "a2c94126f087920ef5bf07d7e58f08266db78e03bb62dcfed222db6c2ab0588c"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/rootly-mcp-goes-ga-up-to-95-less-tokens-used-daily-by-enterprises"
published_at: "2026-04-02T07:49:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:d960c82618af71f916ba6a48cd1d59c5d8433556f49cd4cde8cdee053a610f03"
---

# Rootly MCP goes GA: up to 95% less tokens

Our[MCP server](https://github.com/Rootly-AI-Labs/Rootly-MCP-server) just hit GA. It’s downloaded ~7,000 times a month, it’s running in production at companies you’ve definitely heard of, and, spoiler alert, we’ve been building something else alongside it that we’re pretty excited about. But let’s rewind a bit first.


Back in early 2025, almost nobody was talking about the Model Context Protocol. Most engineering teams hadn’t heard of it, and the many who had were skeptical; yet another integration standard, right?


But over at Rootly AI Labs, we had a hunch that this was how AI agents would eventually talk to the tools that keep production systems running. So we shipped an[MCP server](https://rootly.com/blog/introducing-the-rootly-mcp-server) as fast as we could.


## Early Believers


Literally the next day, companies like Brex and Canva were running our MCP server in production. Not in a sandbox, not as a proof of concept. In production, wired into their actual incident management workflows.


That caught us off guard, honestly. But it made sense in retrospect. These teams are always on the cutting edge, and they were already looking to integrate more AI in their workflow. Our MCP server gave them a way to plug incident management directly into what they were assembling.


## What’s Changed Since Then


The download numbers tell the story: over 7,000 times a month on PyPI and growing.


The server has come a long way from that first release. A few highlights:


- Incident correlation was probably the most requested feature. During an active incident, you’re almost never dealing with one isolated thing. The server can now surface related incidents, enabling teams (and their AI assistants) to spot patterns, recurring issues, and upstream dependencies as events unfold.
- We also added multiple ways to consume the server: SSE, self-hosted, and HTTP streaming. Production environments are messy and varied, so we wanted teams to pick whatever fits their architecture and security requirements.
- Then there’s Code mode, instead of the LLM calling MCP tools directly (and burning tokens on full tool schemas every time), the agent writes code that calls the tools. It's dramatically more token-efficient and more reliable, because the agent is doing what it's already great at: writing code.


## The token efficiency numbers


We've been thinking a lot about how to make agents faster and cheaper for our customers. Token efficiency is a big part of that: fewer tokens means lower costs and quicker responses, which matters a lot when an agent is helping you in the middle of an incident. Code mode came out of that thinking, and the benchmarks against HTTP/SSE show it's working.


For single-request queries, HTTP/SSE wins by a hair. Fair enough:


‍


Task Code Mode HTTP/SSE Winner


Get current user 341 tokens 329 tokens HTTP/SSE by 4%


Latest incident 1,319 tokens 1,301 tokens HTTP/SSE by 1%


List 5 services 1,067 tokens 771 tokens HTTP/SSE by 28%


List 5 teams 1,579 tokens 1,286 tokens HTTP/SSE by 19%


‍


But nobody runs a single MCP call during an incident. The real work is multi-step, and that's where Code mode pulls away:


‍


Workflow Code Mode HTTP/SSE Savings


Root cause analysis (4-call chain) 232 tokens 1,924 tokens (3 requests) Code Mode uses 83% fewer


Service dependencies (6-call chain) 358 tokens 3,175 tokens (6 requests) Code Mode uses 89% fewer


Team workload analysis (5-call chain) 444 tokens 9,373 tokens (5 requests) Code Mode uses 95% fewer


‍


Every additional step compounds the savings. Code mode batches the logic into a single execution instead of paying the schema-and-response tax on each tool call. Correlate alerts, check dependencies, find the on-call, pull runbooks, the kind of chain an agent actually runs during an incident, and you're looking at 83–95% fewer tokens than the same workflow over HTTP/SSE.


That's not just a cost thing. Fewer tokens means faster responses, and during an active incident, the difference between a sub-second answer and a multi-second one is the difference between an agent that's useful and one your team ignores.


‍


## MCP Is Great for Agents. But What About You?


Look, I’m a believer. I even built my own[MCP server on the side](https://www.npmjs.com/package/mcp-sylvain-kalache) , if you’re curious. MCP is fantastic when the consumer is an AI agent. But we’re not the only ones noticing that it’s not always the right tool for every job.


The conversation has been heating up. Our own CTO Quentin Rousseau dug into this in his latest[blog post](https://blog.quent.in/blog/2026/02/24/your-ai-agent-doesnt-need-an-mcp-server-it-needs-a-good-cli/) , pointing out that a typical MCP setup burns ~15,000 tokens on tool schemas before you’ve even asked a question, while the CLI equivalent costs ~300. He also noted that[OpenClaw](https://github.com/nichochar/open-claw) built its entire skills architecture on standalone CLIs, not MCP servers.


And the community is echoing this. Eric Holmes’ post[MCP is dead. Long live the CLI](https://ejholmes.github.io/2026/02/28/mcp-is-dead-long-live-the-cli.html) trended on HN with hundreds of comments making a similar argument. Just last week, Perplexity CTO Denis Yarats[announced at Ask 2026](https://awesomeagents.ai/news/perplexity-agent-api-mcp-shift/) that the company is moving away from MCP internally. And Y Combinator CEO Garry Tan[put it bluntly](https://x.com/garrytan/status/2031910564344262988) : he got frustrated with MCP, vibe coded a CLI wrapper in 30 minutes, and it worked better.


## Introducing the Rootly CLI


This is the “something else” we teased at the top.


We just released the[Rootly CLI](https://github.com/rootlyhq/rootly-cli) : a lightweight, Go-based command-line tool built from the ground up to be AI-agent-native. It’s designed so that agents can interact with Rootly’s full incident management surface at a fraction of the token cost of MCP, with none of the protocol overhead. This solves a bunch of the issues mentioned above.


We built it with agents as the primary consumer. The output is TTY-aware; it auto-switches to JSON when it detects it’s being piped from an agent rather than a human terminal. There’s a markdown output mode (because agents love markdown), server-side filtering so you’re not shipping entire datasets back just to extract what you need, and every flag supports environment variables, so there’s no credential hardcoding in automation pipelines.


The way we think about it: MCP and the CLI are complementary tools for different contexts. MCP shines when you need rich, bidirectional agent-to-tool communication with structured schemas. The CLI is for when an agent (or a human, of course) needs to get something done fast; check who’s on call, pull the last few incidents for a service, trigger an alert from a CI/CD pipeline, without spinning up a full protocol layer.


## What's Next


We're not treating the GA as a finish line. Rootly AI Labs is still heads-down exploring what's possible at the intersection of AI and reliability engineering, and we'll keep shipping tools that help our customers manage incidents better, whether those tools are used by humans, agents, or both.


One area we're actively working on: authentication. Right now, connecting an AI agent to Rootly means generating an API token, copying it into your config, and managing its lifecycle yourself. That works, but it's friction that shouldn't exist. We're building OAuth 2.0 support into the MCP server so you can authenticate through a standard browser flow; log in, approve access, done. No tokens to rotate, no secrets in dotfiles. It also opens the door to scoped permissions, so you can give an agent access to specific teams or services instead of handing it the keys to everything.


The future of incident management is agentic, and we plan to stay right at its center.


‍
