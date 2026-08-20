---
schema_version: "1.0.0"
document_id: "4ec9767681f490172f44aa611272e88fbce5d58c153af02ae558f6f114fb8706"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-subagents-parallel"
published_at: "2026-05-09T12:39:13+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:07.653203+00:00"
content_hash: "sha256:9fb0cbe593607dd1106fb4d803a3dd9ad266ad32435830f0b64362f6e9b81233"
---

# Claude Code Sub-Agents: Run Multiple Tasks in Parallel (2026 Guide)

## How to Invoke Sub-Agents in Claude Code


Three patterns, escalating from suggestion to guarantee.


**1. Natural language** — Claude decides whether to delegate:


```text
Research the authentication, database, and API modules in parallel
using separate sub-agents.


```


**2. @-mention** — guarantees the named sub-agent runs:


```text
@code-reviewer look at the auth changes in src/auth/


```


Type` @` in Claude Code to see available sub-agents in the typeahead. The @-mention controls *which* sub-agent runs, not what prompt it receives.


**3. Create a custom sub-agent**


Run` /agents` in Claude Code → Library tab → Create new agent. Or write the file directly:


```text
# .claude/agents/test-runner.md
---
name  :   test-runner
description  :   Runs tests and reports failures. Use proactively after code changes.
tools  :   Bash, Read
model  :   haiku
---
You are a test specialist. Run the test suite and return only failing
tests with error messages. Omit passing test output entirely.
```


Once defined, invoke it in parallel with another agent:


```text
Use the test-runner sub-agent to run tests, and simultaneously use
the code-reviewer sub-agent to check the implementation quality.


```


**CLI flag for sessions and CI:**


```text
# Run your entire session as a specific sub-agent
claude   --agent   code-reviewer


# Inject multiple sub-agents for a single session
claude   --agents   '{
"test-runner": {
"description": "Runs tests. Use proactively after code changes.",
"prompt": "Run the test suite and return only failing tests.",
"tools": ["Bash"],
"model": "haiku"
},
"doc-writer": {
"description": "Updates API documentation after code changes.",
"prompt": "You update API docs to match code changes. Be concise.",
"tools": ["Read", "Write", "Edit"]
}
}'
```


**Foreground vs. background:**


Foreground sub-agents block until complete — permission prompts and questions come through to you. Background sub-agents run concurrently while you keep working. Claude decides based on the task, but you can ask explicitly:


```text
Run the test suite in the background while I keep working.


```


Or press` Ctrl+B` to background a running task.


## A Real Workflow Example: Shipping a Feature With 3 Sub-Agents


Goal: add OAuth login to an existing Express app.


**Step 1: Launch parallel research**


```text
Research the authentication, database, and routing modules in parallel
using separate sub-agents. I need to understand how they connect before
I implement OAuth.


```


Three Explore sub-agents fire simultaneously. Each returns a focused summary. No context bloat.


**Step 2: Implement with a parallel reviewer**


```text
@code-reviewer check the existing auth code for patterns we must preserve.
Meanwhile, start implementing the OAuth flow in src/auth/oauth.ts.


```


The reviewer runs in the background. You keep building.


**Step 3: Parallel tests + documentation**


```text
Use the test-runner sub-agent to write and run unit tests for the new
OAuth module. Simultaneously, update the API documentation in docs/auth.md.


```


Tests and docs complete concurrently. What takes 45 minutes sequentially takes 15 minutes here.


**Step 4: Final security review**


```text
Use the code-reviewer sub-agent to do a final security review of
src/auth/oauth.ts — focus on token handling and session management.


```


Clean summary. Main conversation never got cluttered with test output or documentation churn.


The pattern works because each step is self-contained. The sub-agents don't need to coordinate with each other in real time — they report their summaries back to the main conversation, and you route information forward manually when needed.


## Sub-Agent Limitations: When NOT to Use Them


Sub-agents are easy to misuse. Here's where they break down.


**Cost multiplies.** Each sub-agent has its own context window and burns tokens independently. Three parallel sub-agents running` sonnet` can cost 3× a sequential approach. Route simpler tasks to` haiku` to offset this.


**Sub-agents cannot spawn sub-agents.** There's no nested delegation. If you need multi-level orchestration, use the Agent Teams feature (experimental, requires` CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` ) or chain sub-agents from your main conversation.


**Context isn't shared in real time.** Sub-agents don't see each other's work as it happens. If Agent A's output changes what Agent B should do, you need to wait for A to finish and route the result manually.


**File conflicts will happen** if two sub-agents edit the same file. Give each agent ownership of distinct files or directories. For stricter isolation, set` isolation: worktree` in the sub-agent frontmatter — the agent gets its own git worktree and can't touch your working directory.


**Debugging is harder.** When three things run simultaneously, tracing failures takes more effort. For exploratory sessions or anything requiring frequent back-and-forth, a single focused conversation beats a fleet of agents.


**Stay in the main conversation when:** you're making a quick targeted change, you need iterative refinement, multiple phases share significant context (planning → implementation → testing), or latency matters (sub-agents start fresh and may need time to gather context).


## Build Your Multi-Agent App on Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your main Claude Code agent to run sub-agents in parallel:


> "Use sub-agents to simultaneously: write tests for the auth module, refactor the database layer, and update the API documentation. Then deploy the result on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.


[Learn more about Blink Cloud →](https://blink.new/cloud)


## FAQ


Sub-agents run inside a single Claude Code session and report results back to your main conversation. Agent teams are fully independent Claude Code instances that communicate directly with each other — teammates can message each other, share a task list, and coordinate autonomously without routing everything through you. Use sub-agents for focused, self-contained tasks. Use agent teams (experimental) when workers need to share findings and challenge each other's conclusions. Enable teams with` CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` .


Two options. First, restrict tools in the sub-agent's frontmatter: a read-only agent should have` tools: Read, Grep, Glob` — exclude` Write` and` Edit` entirely. Second, set` isolation: worktree` in the frontmatter to give the sub-agent an isolated git worktree. Its changes are written there and cleaned up automatically if it makes no edits, or you can review and merge them when it finishes.


Yes. Pass` --agents` with a JSON object when launching Claude Code. Each sub-agent definition supports the same fields as file-based agents:` description` ,` prompt` ,` tools` ,` model` ,` permissionMode` ,` maxTurns` ,` disallowedTools` , and more. This makes it straightforward to inject specialized agents into automated pipelines without committing agent definition files to your repository.


Set the` model` field in the sub-agent's frontmatter. Options are` sonnet` ,` opus` ,` haiku` , a full model ID like` claude-sonnet-4-6` , or` inherit` (the default — uses the same model as your main conversation). Route codebase exploration and search tasks to` haiku` for speed and lower cost. Reserve` sonnet` or` opus` for implementation and security review agents where quality matters most.


If you're using Claude Code for anything non-trivial, sub-agents are the fastest path to more output without increasing your main session's complexity. Start simple — create a` code-reviewer` or` test-runner` , ship one feature using parallel agents, see how the context isolation changes your workflow. Then build from there.


For more on building with Claude Code, read the[Claude Code tutorial for beginners](https://blink.new/blog/claude-code-tutorial-for-beginners) , the[agentic coding guide](https://blink.new/blog/agentic-coding-guide) ,[spec-driven development with AI](https://blink.new/blog/spec-driven-development-ai) , and the[Claude Code plan mode guide](https://blink.new/blog/claude-code-plan-mode-guide) .
