---
schema_version: "1.0.0"
document_id: "20bef22e60babf592ddd65f0db2fefedcc37a6991735a9d38c355a046a26e42d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-goal-mode"
published_at: "2026-05-24T12:24:37+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:f2461dcc60142e27aa74759e3955fc410c1c2300f3123363ec99961efdcbddcc"
---

# Claude Code /goal Mode: Set a Target and Let Claude Work Until It's Done

## When to Use /goal (and When Not To)


**Good fits:** database migrations with a verifiable end state, making a failing test suite pass, refactoring code to satisfy strict type checks, cleaning lint violations across a large codebase.


All of these share one trait: you can write a one-sentence pass/fail condition. Haiku evaluates that condition after each turn and produces a clear yes or no.


**Bad fits:** open-ended exploration, architecture decisions, creative tasks, or anything where "done" means different things in different contexts.


If you can't articulate the end state in a condition Haiku can check,` /goal` won't exit cleanly. You'll either hit the turn limit or end up with a condition that always returns true without the underlying problem being solved.


The sweet spot is substantial work with a verifiable end state. A migration that must touch 30 files, a refactor that must pass strict TypeScript, a test suite that must go from 12 failures to 0.


The /goal command loop: after every turn, a fast model evaluates the condition and either continues or stops when the goal is achieved


Blink


## The claude agents Dashboard: Manage Multiple Sessions at Once


Run` claude agents` in your terminal. One screen opens for every active Claude Code session.


Each row shows the session status at a glance: running, blocked waiting for input, or done. You dispatch three separate tasks — a bug fix, a PR review, and a flaky test investigation — and watch all three as rows without switching windows.


Background sessions keep running without a terminal attached. Attach to any row to read the full conversation history. Press` ←` to return to the session list.


New per-session dispatch flags let you configure each session independently:


- ` --add-dir` — include an additional directory in context
- ` --settings` — use a project-specific settings file
- ` --mcp-config` — point to a different MCP config per session
- ` --model` — run different sessions on different models
- ` --effort` — set effort level per task


That last flag matters when you're running a low-stakes cleanup task alongside a high-complexity refactor. Set effort appropriately for each session rather than globally.


For more on multi-session coordination strategies, see the[multi-agent coding workflow guide](https://blink.new/blog/multi-agent-coding-workflow) .


## /fast Mode on Opus 4.7: 2.5× Faster Live Debugging


` /fast` now defaults to Opus 4.7. That's approximately 2.5× faster than the previous default for the same quality output.


Per-token cost is higher, but the pricing tier stays at 30/30/30/150 per MTok — unchanged from Opus 4.6 fast mode. The speed gain comes from Opus 4.7's architectural improvements, not a quality tradeoff.


Turn` /fast` on during live debugging sessions where you need rapid iteration and depth is secondary. A quick config change, a one-line fix, a failing assertion — these all benefit from faster response times.


Leave` /fast` off for complex multi-step reasoning. Opus 4.7 base mode is better when Claude needs to work through several layers of a problem before answering.


To pin back to Opus 4.6, set` CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1` in your environment.


## Three New Hook Features Worth Knowing


**String array exec form** eliminates shell quoting problems:


```text
"exec"  : [  "node"  ,   "scripts/check.js"  ,   "--strict"  ,   "--reporter=json"  ]
```


Spawn commands directly without a shell. Arguments pass as array elements — no escaping, no injection risk. Safer than string-form exec for any hook that runs untrusted input or complex paths.


**` continueOnBlock` in PostToolUse** keeps turns alive when tools are blocked:


```text
{
"type"  :   "PostToolUse"  ,
"continueOnBlock"  :   true
}
```


When Claude's tool call is rejected, the rejection reason feeds back as context and the turn continues. Claude adapts its next action instead of stopping cold. Useful when you want hooks to enforce rules while letting Claude course-correct without your intervention.


**` terminalSequence` in hook output** enables desktop notifications and bells:


```text
{
"terminalSequence"  :   "  \a  "
}
```


Emit a terminal bell when a long-running task completes. Set a window title to show the current goal status. Works cleanly alongside` claude agents` multi-session view — you get audio or visual signals without writing a separate notification script.


You can find the complete list of changes at[Claude Code releases on GitHub](https://github.com/anthropics/claude-code/releases) .


## Build Autonomous Apps With Claude Code and Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app with an auth-gated API and deploy it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Setting a /goal in Claude Code creates a chain of autonomous turns with each one moving closer to the verifiable end condition


Blink


` /plan` structures a task before Claude acts on it.` /goal` runs the task repeatedly until a condition holds. For a detailed breakdown, see the[Claude Code plan mode guide](https://blink.new/blog/claude-code-plan-mode) . They solve different problems: planning up front vs. iterating until done.


Not effectively.` /goal` needs a verifiable end condition — something Haiku can evaluate as pass or fail after each turn. Open-ended tasks lack that exit signal, so the loop has no reliable termination point. Stick to tasks with a clear, checkable finish line.


Use the bounding syntax to cap turns:` /goal all tests pass or stop after 10 turns` . Claude halts after the limit and reports back with its progress. Without a bound,` /goal` runs until the condition holds or the session ends manually.


No setup required. Run` claude agents` in any terminal where Claude Code v2.1.139 or later is installed. It discovers active sessions automatically — no config file, no additional flags needed to get started.


Attach to the session via` claude agents` and send an interrupt with` Ctrl+C` . Alternatively, kill the terminal session running Claude Code. The goal cancels cleanly and Claude reports its progress before exiting.
