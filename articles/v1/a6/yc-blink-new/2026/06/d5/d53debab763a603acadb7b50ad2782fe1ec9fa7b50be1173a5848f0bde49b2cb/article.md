---
schema_version: "1.0.0"
document_id: "d53debab763a603acadb7b50ad2782fe1ec9fa7b50be1173a5848f0bde49b2cb"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-tips-power-users"
published_at: "2026-06-10T12:39:18+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:8a57154c017315808ccf030d2c15257cfc66b0d0c4fdba5a16d6a42150a6780e"
---

# Claude Code Tips and Tricks: 20 Things Power Users Do Differently

## Session Management


**11. Use /compact before long refactors, not after.**


Context fills up fast during complex tasks. Run` /compact` before starting a large refactor to compress earlier conversation history and give Claude more headroom for the work ahead.


Running` /compact` mid-refactor — after context is already full — risks losing track of files Claude already read.


**12. Use /clear between unrelated tasks.**


Every prompt adds to the context window. Debug the auth module, then switch to building a payment endpoint without` /clear` , and Claude carries all the auth noise into the payment task.


` /clear` between unrelated tasks is the highest-leverage habit for consistent Claude Code output.


**13. Checkpoint before risky operations.**


Every prompt creates a checkpoint. Run` /rewind` to open the rewind menu and restore to any previous state. But the most reliable safety net is git:


```text
git   add   -A   &&   git   commit   -m   "pre-refactor checkpoint"
```


Then rewind in Claude if needed, or` git reset --hard` if Claude already committed something wrong.


**14. Use verbose mode when output looks wrong.**


```text
claude   -v
```


Verbose mode shows every file Claude reads, every command it runs, and its reasoning at each step. Use it when a session produces unexpected results — it shows exactly where the reasoning went off track.


**15. Name sessions so you can resume them.**


For complex tasks spanning multiple sessions:


```text
/rename   oauth-migration
```


Pick up where you left off:


```text
claude   --resume
```


Lists your named sessions. This makes it safe to stop mid-task, close the terminal, and continue the next day with full context restored.


## Advanced Workflows


**16. Use git worktrees to let Claude work on a branch.**


```text
git   worktree   add   ../project-feature-branch   feature/new-payments
cd   ../project-feature-branch
claude
```


Claude works in the isolated worktree without touching your main checkout. Your main branch stays clean. This is the right pattern for large features — Claude runs the full task, you merge when satisfied.


**17. Run parallel Claude sessions on different features.**


Claude Code is single-threaded per session, but you can run multiple terminal sessions simultaneously. Open two terminals. Session A handles the API. Session B handles the frontend. Each session has its own context.


Teams using this pattern routinely run 4–6 parallel Claude sessions on different tickets. The developer orchestrates and reviews. Claude executes.


**18. Add Claude Code to GitHub Actions for autonomous PR reviews.**


```text
name  :   Claude Code Review
on  : [  pull_request  ]
jobs  :
review  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   run  :   |
claude -p "Review the diff for bugs and security issues in src/api/.
Report findings with file references and line numbers." \
--allowedTools "Read,Bash(git diff HEAD~1)" \
--output-format json
```


Claude reads the diff and surfaces findings automatically. Routine PRs get reviewed without a human reviewer. Escalate only when Claude flags real issues.


**19. Use --output-format json for programmatic use.**


```text
claude   -p   "list all API endpoints with their HTTP methods"   --output-format   json
```


Returns structured JSON instead of prose. Pipe it into` jq` , feed it into scripts, or use it in CI workflows that need machine-readable output. Combine with` --allowedTools` to restrict what Claude accesses during automated runs.


**20. Use Claude Code and Cursor together by role.**


Claude Code and Cursor complement each other. They're not competing for the same workflow.


Use Claude Code for: large refactors, autonomous debugging, codebase-wide migrations, CI/CD automation, and tasks where you walk away and review results.


Use Cursor for: active coding sessions, line-level editing with inline suggestions, interactive exploration, and work where you want to stay in the editor loop.


The most effective setup: orchestrate large tasks with Claude Code, do daily coding in Cursor. Let each tool handle the work it's built for.


## Build Your Workflow on Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Set up a full-stack app with auth and database using Claude Code and deploy it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Power user workflow: Claude Code handles development, Blink handles infrastructure — database, auth, and hosting automated


Blink


CLAUDE.md is a markdown file at your project root that Claude Code reads at the start of every session. It's not optional — it's the difference between Claude guessing your conventions and Claude knowing them. Power users maintain CLAUDE.md like code: they prune it regularly, treat bloat as a bug, and use imports (` @path/to/file` ) to keep it modular. The most effective CLAUDE.md files are under 200 words at the root, with extended context in imported files.


Use` /clear` between unrelated tasks to reset the entire context. Use` /compact` proactively before large tasks, not reactively when context is already full. Use subagents for research: "use a subagent to investigate how our auth system handles token refresh" — they read files in their own context window and report back summaries, keeping your main session clean for implementation.


Yes. Headless mode:` claude -p "your prompt"` . Combine with` --allowedTools` to restrict permissions —` --allowedTools "Read,Bash(git diff HEAD~1)"` for read-only review tasks. Use` --output-format json` for machine-readable output. Use` --permission-mode auto` for autonomous runs where you want a safety classifier without requiring interactive approvals. Common patterns: automated PR reviews, dependency update PRs, test failure summaries on new commits.


Three approaches. Simple: open multiple terminal windows and run` claude` in each — each session has its own context. Isolated: use git worktrees (` git worktree add ../project-feature feature/branch` ) and run Claude in each worktree so edits don't collide. Visual: the Claude Code desktop app manages multiple sessions visually, each in its own worktree. Teams use all three depending on task scope and coordination needs.
