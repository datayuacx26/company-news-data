---
schema_version: "1.0.0"
document_id: "eaa757943af63be8c837083e80ef301dc1c7252cc95c9ed6f08b83ad7cff35d1"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-tips-and-tricks"
published_at: "2026-05-31T00:44:31+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:94d369007d512220f68674b6b8f280694db377d7161b092bd59453db1f60adcd"
---

# Claude Code Tips and Tricks: 20 Things Power Users Do Differently

## Bucket 2: Plan better, waste less (Tips 5–8)


### Tip 5: Press Shift+Tab to enter plan mode (always)


Plan mode forces Claude to write a full implementation plan before touching any code. This one habit eliminates most wasted Claude Code sessions.


Press **Shift+Tab** before any non-trivial task. Read the plan. Correct misunderstandings in the plan stage — not in the implementation stage. The Anthropic team's guidance: "Pour effort into the plan."


### Tip 6: Use Opus 4.5 with thinking for hard problems


For architecture decisions, complex debugging, or multi-system designs:


```text
claude   --model   claude-opus-4-5
```


Opus is slower and costs more tokens. For hard problems, the trade-off is worth it. The Anthropic team's own reasoning: "Less steering plus better tool use equals faster overall even with a larger model." Use Sonnet for fast iteration. Switch to Opus when you're stuck.


### Tip 7: Run /effort max for hard debugging


```text
/effort max


```


This burns usage faster. Reserve it for the sessions where you need maximum reasoning depth — architecture decisions, hard bugs, security reviews. The default effort level on Team and Enterprise plans is` high` . On Pro, it's` standard` .


### Tip 8: Have one Claude write the plan, a second Claude review it


Before any complex implementation, run this two-agent review pattern:


**Session 1:** Write the implementation plan.


**Session 2:** Feed the plan to a fresh Claude session with this prompt:


```text
Act as a staff engineer reviewing this implementation plan.
Find every assumption, edge case, and risk. Be adversarial.
[paste plan here]


```


This surfaces problems the planning agent missed. It takes 5 minutes and saves hours of cleanup after a misdirected implementation.


---


CLAUDE.md as a compounding institutional knowledge base that improves with every correction


Blink


## Bucket 3: CLAUDE.md as compounding capital (Tips 9–12)


### Tip 9: Update CLAUDE.md after every correction


After any correction, end with:


```text
Update your CLAUDE.md so you don't make that mistake again.


```


Claude adds the lesson to` CLAUDE.md` . This is what Anthropic calls "Compounding Engineering" — every correction improves every future session on that project. After 50 corrections, your CLAUDE.md is a precision instrument. Most developers never do this and keep re-explaining the same things.


### Tip 10: Install the GitHub Action with /install-github-app


```text
/install-github-app


```


This installs Claude Code as a GitHub Action in your repository. You can then tag` @claude` in PR comments to add learnings directly to CLAUDE.md during code review:


```text
@claude add to CLAUDE.md: never use enum types in TypeScript — use const objects with `as const` instead


```


Code review becomes a CLAUDE.md maintenance workflow. The whole team contributes to the institutional knowledge base.


### Tip 11: Configure auto-memory


```text
/memory


```


Auto-memory saves your preferences and corrections between sessions to` ~/.claude/<workspace>/<project>/memory/` . This means Claude remembers your style preferences even when starting a fresh session without reading CLAUDE.md explicitly. Configure what gets auto-saved in the memory settings menu.


### Tip 12: Invest in a team CLAUDE.md checked into git


A CLAUDE.md committed to your repository is a shared asset. The whole team can add conventions, anti-patterns, and project context. The practices that survive PR review become institutional knowledge that every new team member and every Claude session inherits automatically.


Start your team CLAUDE.md with a standard format: build commands, test commands, architecture overview, code conventions, known gotchas. Treat it like a living document, not a setup artifact. Deeper setup guidance is in the[Claude Code Getting Started guide](https://blink.new/blog/claude-code-getting-started) .


---


## Bucket 4: Verification — the tip that changes everything (Tips 13–16)


[Agents complete approximately 20 autonomous actions](https://agentmarketcap.ai/blog/2026/04/05/anthropic-agentic-coding-trends-report-claude-code-eight-shifts) before requiring human input — double the capability from six months prior. That autonomy is only safe with verification habits. These four prompts are the difference between shipping clean code and shipping confident-but-wrong code.


### Tip 13: "Prove to me this works"


```text
Prove to me this works.
Show me the behavior difference between main and this branch.


```


Claude diffs old behavior against new behavior, finds edge cases, and surfaces regressions. This is the single most underused power-user prompt in Claude Code. Run it every time Claude says it's done.


### Tip 14: "Grill me on these changes"


```text
Grill me on these changes and don't make a PR until I pass your test.


```


Claude becomes your adversarial reviewer. It challenges the implementation, asks hard questions, and forces you to defend every decision. PRs that pass this review rarely get nit-picked by human reviewers.


### Tip 15: "Scrap this and implement the elegant solution"


After a mediocre first attempt:


```text
Knowing everything you know now, scrap this and implement the elegant solution.


```


The first attempt surfaces requirements and constraints. The second attempt, with all that context, is far cleaner. This prompt saves you from polishing a structurally weak implementation.


### Tip 16: Append /simplify to any prompt after making changes


```text
/simplify


```


Runs parallel review agents that check for code reuse, quality, efficiency, and CLAUDE.md compliance. Think of it as a fast, automated code review that runs before your human reviewer sees anything. Make it a habit after every significant change.


---


Claude Code dynamic workflows orchestrating hundreds of parallel subagents for codebase-wide tasks


Blink


## Bucket 5: Automation, scale, and dynamic workflows (Tips 17–20)


### Tip 17: Use hooks for auto-formatting


Hooks run shell commands automatically on Claude Code events. The` PostToolUse` hook on` Write|Edit` is the most useful: it runs your formatter after every file edit, so formatting failures never reach CI.


Configure in` ~/.claude/settings.json` :


```text
{
"hooks"  : {
"PostToolUse"  : [
{
"matcher"  :   "Write|Edit"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  :   "prettier --write $FILE_PATH"
}
]
}
]
}
}
```


Replace` prettier` with your formatter. Works with` eslint --fix` ,` gofmt` ,` rustfmt` , any command-line tool.


### Tip 18: /loop for recurring tasks


```text
/loop 5m /babysit


```


This schedules Claude to run the same prompt every 5 minutes. Use it to auto-address PR review comments as they come in, or to monitor a long-running process and take action when something needs attention.


```text
/loop 30m Check for new Slack feedback and open a PR for any actionable suggestions


```


The[Claude Code power user tips from Anthropic's Help Center](https://support.claude.com/en/articles/14554000-claude-code-power-user-tips) covers more` /loop` patterns for CI/CD integration.


### Tip 19: --bare flag for SDK/CI


```text
claude   --bare   "Analyze this codebase and output a dependency report as JSON"
```


The` --bare` flag skips CLAUDE.md discovery, interactive UI overhead, and session setup. It starts approximately 10x faster than a standard Claude Code session. Use it for non-interactive pipeline use cases where your prompt is explicit and you don't need session persistence.


This flag is expected to become the default behavior for non-interactive use in a future release.


### Tip 20: Dynamic workflows for massive parallel tasks (New: May 28, 2026)


**New — May 28, 2026, research preview.** Dynamic workflows are available on Max (5x and 20x), Team, Enterprise plans, and the Claude API. Not available on Claude Pro. They consume significantly more tokens than standard sessions — start with a scoped task to calibrate usage.


Dynamic workflows let Claude write orchestration scripts that spawn **tens to hundreds of parallel subagents** in a single session. Each subagent attacks a piece of the problem independently. Results are checked before they're folded in. You come back to a single, coordinated answer.


Real-world scale: Jarred Sumner used dynamic workflows to port the Bun runtime from Zig to Rust — 750,000 lines of code — in 11 days with 99.8% of tests passing. Hundreds of agents worked in parallel, each porting a single file. A fix loop ran until the build was clean. ([Source: Anthropic](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code) )


Trigger a workflow:


```text
Create a workflow to hunt for N+1 query patterns across all database calls in this codebase.


```


Or switch on **ultracode mode** via the effort menu. Claude decides when a workflow is appropriate.


Best use cases: codebase-wide security audits, large framework migrations, stress-testing architecture from multiple independent angles, any task that spans hundreds of files.


---


## Build Your Projects With Claude Code or Cursor


These 20 tips make Claude Code more powerful. Blink makes what you build with Claude Code production-ready — database, authentication, and hosting included.


Install the Blink plugin:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then give Claude Code this prompt:


```text
I've got a Node.js app built with Claude Code.
Set up a production PostgreSQL database, authentication, and deploy it on Blink Cloud.


```


Blink Cloud handles infrastructure automatically. What used to require a DevOps engineer now runs in a single prompt.


Explore at[blink.new/cloud](https://blink.new/cloud) . Documentation for the Blink plugin is at[blink.new/docs/cloud/tools/skills](https://blink.new/docs/cloud/tools/skills) .


Want to know how Claude Code compares to other tools? See[Claude Code vs GitHub Copilot](https://blink.new/blog/claude-code-vs-github-copilot) for a direct comparison.


Try Blink free — ship your first app today


Describe what you want to build. Get a working app with database, auth, and hosting in minutes.


[Start free](https://blink.new/)


## FAQ


Verification. Specifically, the prompt "Prove to me this works." Claude can be confidently wrong. Running "Prove to me this works. Show me the behavior difference between main and this branch." after every task surfaces regressions before they reach production. It's the tip Anthropic's own team lists first in their power user guide, and the one most developers never use.


CLAUDE.md is a project-specific context file Claude reads at the start of every session. When you correct Claude on something — a wrong convention, a missed edge case, a bad pattern — ending with "Update your CLAUDE.md so you don't make that mistake again" permanently captures that lesson. Anthropic calls this "Compounding Engineering": every correction improves every future session. Most developers re-explain the same things session after session instead of writing them down once.


No. Dynamic workflows require Max plan (5x at $100/month or 20x at $200/month), Team, Enterprise, or the Claude API. They were launched May 28, 2026 as a research preview. Standard Claude Code features — plan mode, parallel worktrees, hooks, CLAUDE.md, all 20 tips in this guide — work on Claude Pro ($20/month). Dynamic workflows are for large-scale parallel tasks that require spawning hundreds of subagents.


Use git worktrees. Each worktree checks out a different branch independently, letting Claude Code run a separate session without branch conflicts. Run` git worktree add ../my-project-feature feature/my-feature` to create a worktree, then` claude` inside that directory. Use` --name` to label each session. Color-code your terminal tabs by task.


The` --bare` flag skips CLAUDE.md discovery and interactive session setup, starting Claude Code approximately 10x faster than a standard session. It's designed for non-interactive use in CI/CD pipelines, scripts, and SDK integrations where your prompt is explicit and you don't need persistent session state. Expected to become the default for non-interactive use in a future release.
