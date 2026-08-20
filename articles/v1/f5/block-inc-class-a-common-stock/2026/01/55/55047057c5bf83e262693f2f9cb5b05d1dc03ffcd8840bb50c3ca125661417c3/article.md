---
schema_version: "1.0.0"
document_id: "55047057c5bf83e262693f2f9cb5b05d1dc03ffcd8840bb50c3ca125661417c3"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/the-missing-piece-for-local-ai-development"
published_at: "2026-01-14T12:00:00+00:00"
first_seen_at: "2026-07-20T03:32:06.876214+00:00"
fetched_at: "2026-07-28T20:54:51.009577+00:00"
content_hash: "sha256:5bd17540bb75bbedc3b3dcc5ff35c603a145ddad48a4c1086ad76b41dc519264"
---

# The Missing Piece for Local AI Development

Engineers at Block are running multiple AI agents locally. The feedback loop is faster, especially for mobile. You're constantly deploying to emulators, and remote agents add latency you don't need.


A typical setup is one agent on a larger task like a feature or refactor. One or two others handle smaller things like UI tweaks or bug investigations. Some engineers are even using agents to coordinate other agents!


All of this works until you run into an expensive build. I'm primarily on Android and my laptop doesn't survive multiple Gradle tasks and emulators running at once.


## The Problem


When multiple agents decide to run a Gradle build, your machine grinds to a halt. One instance of Gradle is already resource-intensive, and running multiple builds simultaneously makes them compete for CPU, memory, and disk I/O. A build that normally takes under 3 minutes stretches to 15. The fans spin up. Everything freezes. And if they're both trying to deploy to the same emulator, you get task clashing on top of it.


There's also a correctness problem. When test output from multiple builds interleaves, agents can't tell which results are theirs. They lose track of what they're even debugging.


The agents don't know about each other and they can't coordinate.


## The Workarounds


There are a few options here, and none of them are great.


You could run multiple emulators so each agent has its own target. Now you're burning even more resources on a machine that's already struggling. And you still haven't solved the multiple expensive task problem.


You could manually sequence the tasks yourself. Wait for Agent A to finish its build before letting Agent B run. But now you're babysitting the agents instead of doing your own work. The whole point of running multiple agents is to get more done in parallel, not to become a human task scheduler.


Either way, you're giving up something: time, resources, or attention.


## What We Tried First


We built a CLI wrapper. The idea was simple. Wrap the command and the calling context, put them into a First In First Out (FIFO) queue. One runs at a time, the rest wait.


```text
bash    1  queue ./gradlew build
```


Technically it worked, but there was a problem we didn't anticipate.


**AI coding tools have shell timeouts.**


[Claude Code](https://docs.anthropic.com/en/docs/claude-code) gives you about 2 minutes by default.[Cursor](https://cursor.com/) hard-codes 30 seconds. If your command is waiting in queue when the timeout hits, it gets killed.


Then the agents try to "be smart" and run the command without the queue wrapper, defeating the whole purpose.


We tried extending the timeouts with environment variables. That helped for Claude, but Cursor's limit isn't configurable. Even with longer timeouts, the issue of waiting compounds the more agents you're using. You realistically only want timeouts for **execution time** , not **waiting time** .


## Why Model Context Protocol Works Better


[MCP](https://modelcontextprotocol.io/) tool calls don't go through the shell. The agent connects directly to an MCP server, and that connection stays alive until the tool returns. There's no external timeout to worry about.


With a CLI, the agent spawns a shell process, the shell runs your command, and if the shell process takes too long, the agent kills it. With MCP, the agent calls a tool and waits for the response. No shell, no timeout.


So we rewrote the queue as an MCP server. Same FIFO concept. One build at a time. But the timeout problem disappears.


## How It Works


Agent A calls` run_task` with a Gradle command. The MCP server queues it, runs it, returns the result. If Agent B calls the same tool while A is running, B waits in the queue until A finishes. Both agents block on their tool calls, but neither times out.


Here's what that looks like in practice:


Time Agent A Agent B


0


Started build


0


Building... Entered queue, waiting


3


Completed (192.6s) Started build


3


Completed (32.6s)


Agent B's build only took 32 seconds because it didn't have to compete with Agent A. Gradle's daemon was warm, caches were populated, the machine was free.


Total time: 3


. Without the queue, both builds fighting each other would've taken 10+ minutes, and your laptop would've been unusable.


## Implementation Details


The whole thing is about 600 lines of Python. SQLite with[Write-Ahead Logging (WAL)](https://www.sqlite.org/wal.html) mode for queue state. Process groups to clean up orphaned builds if an agent crashes. Output goes to log files instead of returning inline, so you don't burn context window tokens on build spam.


One gotcha: if you're using Claude Code, add instructions to your` CLAUDE.md` telling it to prefer the MCP tool over Bash for build commands. Otherwise it'll run Gradle directly and skip the queue. The[README](https://github.com/block/agent-task-queue#readme) has the snippet we use.


## Try It Out


We've open sourced[Agent Task Queue](https://github.com/block/agent-task-queue) so anyone running multiple local agents can benefit! If you're triggering expensive operations like builds or tests give it a try and let us know how if it helps or how we can improve!


It's available under Apache 2.0 and you can install it with[uvx](https://docs.astral.sh/uv/) :


```text
bash    1  uvx agent-task-queue@latest
```


Works with most AI coding tools that support MCP: Claude Code, Cursor,[Windsurf](https://codeium.com/windsurf) ,[GitHub Copilot](https://github.com/features/copilot) , and more. Check out the[repo](https://github.com/block/agent-task-queue) for setup instructions.


---


We've had a lot of success with this internally. Engineers run two or three agents at once and their machines stay responsive. Builds that used to thrash for 15 minutes now finish as expected. If you're running multiple agents locally, give them this queue and let them coordinate.
