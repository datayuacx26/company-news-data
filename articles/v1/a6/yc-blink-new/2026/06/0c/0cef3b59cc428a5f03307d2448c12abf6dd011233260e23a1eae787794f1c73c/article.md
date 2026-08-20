---
schema_version: "1.0.0"
document_id: "0cef3b59cc428a5f03307d2448c12abf6dd011233260e23a1eae787794f1c73c"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-github-workflow"
published_at: "2026-06-02T12:28:58+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:fbf3c3d900adca2a0789623c58fffc67ca1d387305281d3f69f64ce0d550dcb5"
---

# Claude Code With GitHub: Commits, PRs, and Branch Workflows

## Creating Pull Requests With Claude Code


Once a branch is pushed, Claude can open the PR:


```text
"Open a PR from feat/add-user-authentication to main.
Title: Add JWT authentication middleware
Include what changed and flag any breaking changes."


```


Claude generates a structured description:


```text
## What changed
-   Add JWT token validation middleware
-   Token expiry: 24h (configurable via JWT_EXPIRY env var)
-   Returns structured error responses (RFC 7807)


## Breaking changes
None — middleware is opt-in via route configuration


## Testing
Run: npm test -- --testPathPattern=auth
4 new test cases, all passing
```


Claude also reads existing PRs when you share a URL — paste` https://github.com/org/repo/pull/123` and Claude understands the diff, the conversation, and what reviewers have asked for:


```text
"Review this PR: https://github.com/org/repo/pull/123
Flag any security issues and suggest improvements"


```


Research from GitHub's engineering team shows that 78% of review round-trips on typical PRs are mechanical comments — variable naming, null checks, missing tests. Claude catches these before the PR is opened, reducing review time to the decisions that actually require a human.


GitHub pull request created by Claude Code — automated commit messages and PR descriptions


Blink


## Advanced Patterns


### Plan mode before large refactors


Before letting Claude modify 10+ files, switch to plan mode. Claude maps every file it intends to change before touching anything:


```text
"Enter plan mode. I want to refactor authentication from session
cookies to refresh tokens. Show me every file you would change
and explain why."


```


Plan mode prevents the common failure: Claude starts editing, gets 6 files in, then realizes it needs to change something that breaks the earlier edits. You get a complete change map before execution. This pairs well with the[dynamic workflows covered in our sub-agents guide](https://blink.new/blog/claude-code-dynamic-workflows) .


### Hooks for automated pre-commit checks


Claude Code hooks run commands automatically at specific points in the tool lifecycle. Add a` PreToolUse` hook on` Bash` to enforce lint and tests before every commit:


```text
// .claude/settings.json
{
"hooks"  : {
"PreToolUse"  : [
{
"matcher"  :   "Bash"  ,
"hooks"  : [
{
"type"  :   "command"  ,
"command"  :   "npm run lint && npm run test:unit"
}
]
}
]
}
}
```


With this in place, Claude runs lint and tests before executing shell commands. If tests fail, it stops and surfaces the errors. Your CI pipeline stops being the first line of defense against broken commits.


### Parallel branch work with sub-agents


Claude Code supports parallel sub-agents for work across multiple branches simultaneously. Instead of three sequential tasks, run them concurrently:


```text
"Start three parallel tasks:
- Task 1: implement payment processing on feat/payments
- Task 2: fix the 3 failing auth tests on fix/auth-tests
- Task 3: update dependencies on chore/deps-update"


```


Each sub-agent works on its branch independently. You get three PRs ready for review in the time it previously took to complete one. This is the workflow pattern behind the 40–60% cycle time reduction that teams report with AI-assisted development — the bottleneck shifts from implementation to review, where it belongs.


For the full sub-agent playbook, see[our guide to agentic coding best practices](https://blink.new/blog/agentic-coding-best-practices) .


### Reading your repository before acting


Claude Code reads the full git log, not just the current diff. When you ask it to fix a bug, it can check when the bug was introduced, which branch it came from, and who last touched the affected code:


```text
"Check the git log for src/auth/middleware.ts.
When was validateToken() last changed and what did that commit do?"


```


This makes root-cause analysis faster. Claude cross-references commit history with the current codebase state without you running` git log --follow -p` manually.


Claude Code PR workflow complete — branch merged, CI passed, changes deployed


Blink


## Build GitHub Workflows Into Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build a full-stack app with Claude Code and push it to GitHub — then deploy on Blink with a single command."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


No. Claude Code uses the system git binary and your existing credentials — the` gh` CLI auth, macOS Keychain, or Git Credential Manager. If` git push` works in your terminal, Claude Code can push. No additional token setup is required.


Yes. Paste a GitHub PR URL and Claude reads the full diff, inline review comments, and conversation thread. It can summarize what reviewers asked for, suggest changes that address open comments, or draft a reply explaining a decision. This works for both your own PRs and PRs from contributors you're reviewing.


Two layers: first, add` Never commit directly to main` to your` CLAUDE.md` — Claude will follow this instruction. Second, enforce it at the GitHub level with branch protection rules: require a pull request before merging and block force-pushes. Branch protection is the hard stop; CLAUDE.md is the soft convention that prevents Claude from trying in the first place.


Without instructions, Claude writes descriptive single-line messages based on what changed in the diff. With a` CLAUDE.md` that specifies conventional commits, it will use` feat:` ,` fix:` ,` refactor:` ,` chore:` , or` docs:` prefixes with scope in parentheses. Claude also writes a body paragraph listing key changes when the diff is large enough to warrant it.


Yes, with some caveats. Claude can read conflict markers (` <<<<<<<` ,` =======` ,` >>>>>>>` ) and resolve them based on context — it understands what both sides of the conflict are trying to do. For simple conflicts (formatting, import ordering, minor additions), it resolves correctly. For conflicts involving competing feature logic, use plan mode first: have Claude explain both sides before it picks one. Never let Claude auto-resolve conflicts in critical business logic without reviewing the resolution.


Commit` CLAUDE.md` to the repository root and version-control it like any other configuration file. Every developer who uses Claude Code on the project gets the same conventions automatically — branch naming, commit format, PR expectations. When conventions change, update` CLAUDE.md` in a PR like any other change. There is no per-developer configuration required; the file is the shared contract.
