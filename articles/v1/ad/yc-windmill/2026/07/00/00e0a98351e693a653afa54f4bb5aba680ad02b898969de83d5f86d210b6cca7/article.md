---
schema_version: "1.0.0"
document_id: "00e0a98351e693a653afa54f4bb5aba680ad02b898969de83d5f86d210b6cca7"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-12b6d71fe86e"
canonical_url: "https://www.windmill.dev/blog/how-webmux-shaped-ai-sessions"
published_at: "2026-07-26T00:00:00+00:00"
first_seen_at: "2026-08-03T14:00:49.178645+00:00"
fetched_at: "2026-08-05T03:48:37.411401+00:00"
content_hash: "sha256:c6c40fedc0fc0e48e76a623dfdc65793331f9976d2818f259c1a7f4f0f575e04"
---

# How building AI Sessions was shaped by our own experience with Webmux

Before we shipped a single line of AI Sessions, we had already spent months supervising AI coding agents on our own codebase, through a tool we built for ourselves:[Webmux](https://github.com/windmill-labs/webmux) .


Webmux is our open-source dashboard for running parallel AI agents across git worktrees. We built it in early 2026 because no existing tool fully solved the problem of keeping five or more agents going at once, then we lived in it every day. Between mid-January and late March, our weekly[merged PRs](https://github.com/windmill-labs/windmill/pulls?q=is%3Apr+is%3Aclosed) went from 36 to 96, an all-time high for the team.


That daily use was the design spec for AI Sessions. Every friction we hit supervising agents in Webmux turned into a decision about how AI Sessions should work. This post traces those decisions from the tool we used internally to the product we ship on July 27.


## Webmux is workmux plus a web interface​


Credit where it is due: Webmux is very inspired by[workmux](https://github.com/raine/workmux) . The gist of Webmux is workmux's worktree and tmux automation, plus a web interface, plus a Linear integration that polls for issues.


workmux nails the core abstraction: one worktree, one tmux session, one agent per task, built on tools you already use. Its philosophy is to tie together git, your terminal multiplexer, and your agent instead of replacing them.


We added a browser on top. Webmux creates the worktree, allocates ports, spins up the tmux session, and starts Claude or Codex with your prompt, then streams the live terminals to a single tab alongside PR status, CI results, and service health. A Linear poller watches assigned issues: label one, and Webmux spins up a worktree and starts the agent on it.


Your browser does not support the video tag.[Download the video](https://www.windmill.dev/assets/medias/demo-70e6479e1cd41749dbbe5042d255b08b.mp4) .


## Two ways to put a UI on a coding agent​


The first real decision came out of building Webmux, and it applies to anyone putting an interface around a coding agent. You have two options.


One: wrap the agent. Drive it through its SDK and custom plumbing, then build your own rendering layer on top. This integrates tighter into your own UI, because you control every pixel.


Two: forward the terminal. Display Claude Code or Codex as they are, and let the agent's own output be the interface. The payoff is that as Anthropic and OpenAI improve their tools, you benefit immediately, and nothing on your end breaks.


Webmux forwards the terminal. That is why plugging in another CLI is a line of configuration, and why upstream updates just show up. The lesson we carried forward: reinventing a layer that someone else maintains better is a tax you pay on every release.


Your browser does not support the video tag.[Download the video](https://www.windmill.dev/assets/medias/linear-73a76660e87224dff8951c72e5284c0d.mp4) .


## What actually makes these agents good​


Here is the part that shaped AI Sessions the most.


What makes Claude Code and Codex powerful is not the model alone. It is that they act on a full, complete filesystem through tools. They can list, read, grep, and edit any file in the project, so they operate on the whole codebase at once instead of being limited to whatever fits in a context window slice.


A Windmill workspace is not literally a filesystem. But you can treat it as a virtualized one backed by a database: scripts, flows, apps, resources, variables, and data tables, all as objects the agent can address. AI Sessions gives the agent that same whole-workspace tool access. It uses` search_workspace` to find any script or flow by keyword and` get_runnable_details` to inspect their schema and content, so it can act globally across the workspace rather than one item at a time.


## Where we wrapped, and where we refused to​


Being honest about the two approaches above: for AI Sessions, the agent layer itself had to be wrapped. There is no terminal to forward when the agent edits structured workspace objects, so we drive it through the SDK. There was no other choice there.


What we refused to do was extend that same wrapping into the workspace layer. The agent's authoring operations do not reimplement Windmill. They go through the same[draft tools](https://www.windmill.dev/docs/core_concepts/draft_and_deploy) you use by hand, which preserve the draft lifecycle and conflict detection. The agent edits the same drafts you edit, produces the same diffs, and trips the same conflict detection when someone else deploys mid-session. Read-only API calls the session can make on its own; anything that mutates state asks for confirmation first.


## Iterate on a draft, deploy when it is ready​


The last lesson also came from worktrees. A git worktree lets workmux and Webmux keep iterating on an uncommitted branch without ever touching main. You experiment, you throw work away, and nothing reaches production until you decide to merge.


AI Sessions maps that model onto the workspace. The agent's changes land as[drafts](https://www.windmill.dev/docs/core_concepts/draft_and_deploy) , user-scoped and autosaved, running in parallel to the deployed version without affecting it. You keep iterating on a drafted but not deployed version of the workspace until it is ready, review the diff, then deploy to make it live.


For larger changes,[workspace forks](https://www.windmill.dev/docs/advanced/workspace_forks) take it further: a complete copy of the workspace where triggers and schedules are cloned but disabled, so nothing fires against production while the agent works. When you are happy, merge back from the UI or open a git pull request for review.


## See it running​


AI Sessions launches July 27. Webmux is open source and MIT licensed today: read the code on[GitHub](https://github.com/windmill-labs/webmux) , and take a look at[workmux](https://github.com/raine/workmux) , the project that inspired it.


[Windmill](https://www.windmill.dev/) is an[open-source](https://github.com/windmill-labs/windmill) and[self-hostable](https://www.windmill.dev/docs/advanced/self_host/) developer platform to build, orchestrate, and monitor internal tools and data pipelines, combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.


You can[self-host](https://www.windmill.dev/docs/advanced/self_host/) Windmill using a` docker compose up` , or go with the[cloud app](https://app.windmill.dev/user/login) .
