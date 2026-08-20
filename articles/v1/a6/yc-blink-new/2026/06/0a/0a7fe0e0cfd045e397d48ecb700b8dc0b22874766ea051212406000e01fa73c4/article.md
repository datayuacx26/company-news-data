---
schema_version: "1.0.0"
document_id: "0a7fe0e0cfd045e397d48ecb700b8dc0b22874766ea051212406000e01fa73c4"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-tips-tricks-power-users"
published_at: "2026-06-12T01:16:16+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:b925f2b6f4125f22b020b5d7d913a44477d505c0b98ed951348994c4578cebab"
---

# Claude Code Tips and Tricks: 20 Things Power Users Do Differently

## Section 3: Session Management


Context pollution is the silent killer of Claude Code productivity. Most users don't manage it. Power users treat context like RAM — allocate carefully, free what you don't need.


### Tip 10: /compact at 50% Context, Not 100%


Don't wait for forced compaction. By the time Claude is forced to compact your context, you've already degraded the quality of its responses for the last 20% of the session.


Run` /compact` when you hit roughly half capacity. You see the context percentage in the status bar. Build a habit: if it's over 50%, compact before starting the next task.


The compacted summary preserves the important decisions and current state. Starting fresh from that summary is better than pushing a swollen context to its limit.


### Tip 11: New Feature = New Session


This is counterintuitive but correct. When you finish one feature and start the next, don't continue the session. Close it and start fresh.


Why: the previous feature's context — its specific files, decisions, and error messages — becomes noise for the next task. Claude will occasionally reference old patterns that don't apply, or avoid solutions because something similar failed earlier in the session.


Fresh context, fresh start. Each feature gets a clean slate.


### Tip 12: Use Git Worktrees for Parallel Claude Code Sessions


This is the most underused pattern in the power user toolkit. Git worktrees let you check out the same repo at multiple paths simultaneously. Each path gets its own Claude Code session:


```text
# Create a worktree for a new feature branch
git   worktree   add   ../project-feature-auth   feature/auth-refactor


# Now you can run Claude Code in both directories simultaneously
# Terminal 1: claude (in project/)
# Terminal 2: claude (in project-feature-auth/)
```


Two features in parallel, no branch switching, no context bleeding. Each session stays focused on its own task. This scales to as many features as your machine can handle.


### Tip 13: Save Session State at the End of Each Session


Before closing a long Claude Code session, ask it to write a brief summary:


> "Summarize what we built today, what decisions we made, and what's left to do. Write it as 5-6 bullet points I can paste into the next session's CLAUDE.md."


Paste that into the` ## Recent Context` section of your CLAUDE.md. The next session starts with full context about where you left off, without you having to explain it from scratch.


## Section 4: MCP and Tool Extension


Connecting multiple MCP servers gives Claude Code direct access to your entire development ecosystem


Blink


*Connecting multiple MCP servers gives Claude Code direct access to your entire development ecosystem*


Claude Code without MCP servers is like a browser without plugins. Functional, but limited. The right MCP stack extends what Claude can do — reading issues, running queries, checking CI — without leaving the conversation.


### Tip 14: Add GitHub MCP First


GitHub MCP gives Claude direct access to your PRs, issues, CI runs, and code search. It's the highest-leverage first MCP for any team:


```text
{
"mcpServers"  : {
"github"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-github"  ],
"env"  : {
"GITHUB_PERSONAL_ACCESS_TOKEN"  :   "your-token"
}
}
}
}
```


Once connected, you can ask: "Look at the open PRs and find which ones have failing CI." Or: "Read issue #234 and implement the feature it describes." The agent can read your actual project context, not just what you've told it.


The[GitHub MCP server](https://github.com/modelcontextprotocol/servers/tree/main/src/github) is maintained by Anthropic and covers the full GitHub API surface — repos, issues, PRs, actions, code search.


### Tip 15: Add Filesystem MCP for Multi-Repo Work


The Filesystem MCP lets Claude read and write files outside the current directory. Essential for monorepos and multi-repo setups:


```text
{
"mcpServers"  : {
"filesystem"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-filesystem"  ,   "/path/to/allowed/directories"  ]
}
}
}
```


With this, Claude can read a shared types package while editing your frontend, or check a sibling service's schema without you copy-pasting it.


### Tip 16: Add Blink MCP for Full-Stack Deployment From Within Claude Code


The Blink MCP gives Claude Code 62 tools for database, auth, backend, and hosting — all provisioned from the conversation:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask: "Set up a Postgres database for this project and deploy it on Blink Cloud." The agent provisions infrastructure, runs migrations, and gives you a live URL — without leaving Claude Code, without opening a Supabase dashboard, without configuring Vercel. See[Anthropic's Claude Code docs](https://docs.anthropic.com/en/docs/claude-code/overview) for integrating MCPs in general.


### Tip 17: Verify Your MCP Stack Before Every Session


Before starting any major task, run:


```text
claude   mcp   list
```


You'll see every connected server and whether it's active. It takes 10 seconds and prevents "why isn't Claude looking at my issues?" confusion halfway through a complex session. If a server is disconnected, reconnect it before you start — not after you've already invested 30 minutes of context.


## Section 5: Cost Control


Agentic coding sessions cost real money if you're not thoughtful. Three patterns keep costs reasonable without sacrificing output quality.


### Tip 18: Route Codebase Exploration to Haiku


Not every task needs Sonnet. When you want Claude to explore a codebase, understand a file, or answer "what does this module do?" — that's a Haiku task. Switch models explicitly:


```text
# Via /model command in the conversation
/model   claude-haiku-4-5
```


Haiku processes codebase exploration at a fraction of the cost of Sonnet. The output for understanding and summarization is nearly identical. Reserve Sonnet for actual code generation and multi-file edits.


### Tip 19: Set Auto-Compact at 70% Context


The default is 100% (forced compaction when context is full). Power users set it at 70%:


```text
"autoCompact"  :   true  ,
"autoCompactThreshold"  :   0.7
```


This keeps responses sharp throughout the session. Context quality degrades as it fills — setting the threshold at 70% means Claude always has headroom. It also prevents the jarring forced compaction that interrupts your flow when you're in the middle of a complex edit.


### Tip 20: Batch Similar Tasks in One Session, Not Multiple


Starting a new Claude Code session has overhead — context loading, MCP reconnections, project re-indexing. If you have five similar tasks (add logging to 5 API endpoints), batch them into one session:


```text
"Add structured logging to these 5 endpoints:
1. /api/users
2. /api/orders
3. /api/products
4. /api/auth/login
5. /api/auth/logout


Use the same format for all: {timestamp, endpoint, userId, statusCode, durationMs}"


```


One session, one context load, consistent output. Five separate sessions would cost you reconnection overhead plus context inconsistency. The batched version also produces more consistent code because Claude maintains the pattern it established in the first endpoint.


**Pick one tip from Section 1 and implement it in the next 10 minutes.** Start with the` ## What NOT to Do` section — add 5 lines to your CLAUDE.md right now. The improvement in your next session will be noticeable.


## Build This Into Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up my project with these Claude Code conventions and deploy it on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


Start with CLAUDE.md before anything else. A well-structured CLAUDE.md with explicit build commands and a` ## What NOT to Do` section will eliminate the most common mistakes and reduce back-and-forth with the agent. New Claude Code users who write a solid CLAUDE.md on day one get to productive sessions dramatically faster than those who skip it.


Plan Mode uses more tokens than direct execution because it generates a full plan before acting. For tasks touching one or two files, the overhead isn't worth it. For multi-file refactors, adding a new feature, or anything that could affect production behavior, Plan Mode pays for itself by catching mistakes before they happen. The cost of one caught mistake usually exceeds the cost of dozens of Plan Mode sessions.


Run` /compact` proactively at 50% context fill, not reactively at 100%. The last 30% of a swollen context produces noticeably lower-quality output because Claude is working harder to maintain coherence across a large, noisy context window. Compact early and often — the summary preserves everything important, and you'll notice sharper responses in the next section of your session.


GitHub MCP is the highest-leverage first addition — it lets Claude read your actual issues, PRs, and CI status instead of relying on what you've described. Filesystem MCP is essential for multi-repo or monorepo work. After those two, the right MCPs depend on your stack: Postgres MCP for direct database access, Blink MCP if you're deploying full-stack apps, Slack MCP for teams. Use` claude mcp list` before every session to verify your stack is connected. See[Claude Code's MCP documentation](https://docs.anthropic.com/en/docs/claude-code/mcp) for setup details.
