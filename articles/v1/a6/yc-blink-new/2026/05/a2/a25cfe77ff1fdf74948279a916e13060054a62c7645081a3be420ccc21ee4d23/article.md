---
schema_version: "1.0.0"
document_id: "a25cfe77ff1fdf74948279a916e13060054a62c7645081a3be420ccc21ee4d23"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-agent-view"
published_at: "2026-05-13T00:58:29+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:59.865099+00:00"
content_hash: "sha256:38fd531e537332ca0794fb5389feb6d146ff70093841990f390edff0894672be"
---

# Claude Code Agent View: How the New Agentic Workflow Changes Development

## How to Enable Agent View


Agent view is a **Research Preview** , available on Pro, Max, Team, Enterprise, and Claude API plans.


1


#### Update Claude Code


Agent view requires v2.1.139 or later. Check your version:` claude --version` . Update with` claude update` .


2


#### Open agent view


From any directory, run:


```text
claude   agents
```


The table opens. An empty table just means no sessions are running yet.


3


#### Dispatch your first background session


Type a task prompt in the input at the bottom and press Enter. A row appears immediately and fills in as Claude starts working.


4


#### Background an existing session


Inside any Claude Code session, run` /bg` . Or press ← on an empty prompt — it backgrounds the current session and opens agent view in one step.


5


#### Start directly in the background from your shell


Skip the interactive session entirely:


```text
claude   --bg   "investigate the flaky SettingsChangeDetector test"
```


Claude prints the session ID and keeps working after you close the terminal.


## How Parallel Sessions Stay Isolated


The first question most developers ask: what happens when two sessions edit the same file?


Each background session gets its own git worktree under` .claude/worktrees/` . The sessions share the repository checkout but write to isolated branches. Two sessions can work against the same codebase without touching each other's changes.


When you delete a session, its worktree is removed. Push or merge changes before deleting.


Outside a git repository, sessions write directly to the working directory. Avoid running parallel sessions that edit the same files without git isolation in place.


A **supervisor process** hosts all background sessions separately from your terminal. Sessions persist through agent view closing, shell closing, and Claude Code updates. Session state lives in` ~/.claude/jobs/<id>/state.json` . The one thing that stops a background session: machine sleep or shutdown. Run` claude respawn --all` to restart everything from where it left off.


## Agent View in Real Multi-Agent Workflows


Multi-agent Claude Code workflows — parallel sessions coordinated through Agent View with git worktree isolation


Blink


Agent view changes what[parallel agentic workflows](https://blink.new/blog/claude-code-subagents-parallel) actually look like in practice.


A few patterns Anthropic documented from early users:


**Dispatch and review.** Kick off five feature branches at once, each optionally paired with a custom skill. Return to a list of PRs ready for review instead of managing five separate terminals.


**Long-running jobs.** PR babysitters, dashboard updaters, and loop sessions that run on a schedule all show their next run time in the row. You check the list instead of tailing logs.


**Context switching without losing flow.** Mid-session on one task, press ←, start a quick codebase question in a new session, peek when the answer lands, arrow right back into the original task.


This pairs directly with the[agentic coding practices](https://blink.new/blog/agentic-coding-best-practices) you're already using. If you haven't set up parallel sessions before, agent view is the lowest-friction entry point — the coordination layer is built in.


One limitation to know upfront: rate limits apply per session. Ten parallel sessions consume quota roughly ten times as fast as one. The[Claude Code docs](https://code.claude.com/docs/en/agent-view) flag this explicitly — plan your concurrency accordingly.


## Build Agent View Workflows Into Your App With Claude Code or Cursor


Agent View shows you every step your AI agent takes. Pair it with Blink for full-stack infrastructure.


Install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a full-stack app and host it on Blink — I want to watch every step in Agent View."


Your agent provisions database, auth, backend, and hosting — you see every tool call in real time. No Vercel config, no Supabase account.


[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Agent view is a CLI dashboard that shows every Claude Code session in one table — their state (working, waiting, done), a one-line Haiku-generated summary, and PR status. Open it with` claude agents` . Launched May 11, 2026 as a Research Preview on Pro, Max, Team, Enterprise, and API plans. Requires v2.1.139 or later.


Agent view is specific to Claude Code's CLI — it's not available inside Cursor. Cursor has its own background agent feature for parallel sessions. Agent view is Claude Code's answer to the same problem: managing concurrent AI coding sessions without a tmux grid or manual tab switching.


Subagents are spawned by a parent session — one Claude Code conversation creates and manages child agents internally. Agent view manages independent top-level sessions that you dispatch yourself from the CLI. Subagents are parallel workers inside one job; agent view is the dashboard for multiple independent jobs running side by side. Both approaches are covered in the[parallel subagents guide](https://blink.new/blog/claude-code-subagents-parallel) .


They keep running. Background sessions are hosted by a supervisor process independent from your terminal. Close the window, close your shell, start a new session — dispatched work continues. The exception is machine sleep or shutdown, which stops background sessions. Run` claude respawn --all` to restart them all from where they left off.
