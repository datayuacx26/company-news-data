---
schema_version: "1.0.0"
document_id: "af4ddb757df66825aaeb41f1123120ace91c49035ffcf9cabf56a7bca13c83c3"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-with-github"
published_at: "2026-06-04T01:53:09+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:3c1b2aa0a359120e53e0c4d87eb43cb26c56fbe04e4520b23329adca5e0b38e8"
---

# Claude Code With GitHub: Create PRs, Review Code, and Push Changes Automatically

## Stage and Commit With Intent


The worst Claude Code commits are` git add .` followed by` "fix stuff"` . The best ones are staged by logical unit.


Tell Claude Code to use interactive staging:


```text
git   add   -p
```


This walks through each diff hunk individually. Claude Code evaluates each change and decides: stage it, skip it, or split it. The result is a commit that matches one logical change — not a dump of everything that changed in the last hour.


Commit with a conventional message:


```text
git   commit   -m   "feat: add JWT refresh token rotation on 401 response"
```


Add this to` CLAUDE.md` to get consistent commit messages across every session:


```text
## Commit Style
Use conventional commits: feat:, fix:, chore:, refactor:, docs:
Subject line under 72 characters.
Add a body paragraph for non-obvious changes.
Reference issue numbers with "Fixes #N" or "Closes #N".
```


Claude Code follows this precisely. It generates the subject, decides whether the change warrants a body, and adds the issue reference if one is mentioned in the task.


## Open a Pull Request From the Terminal


Once the branch has commits, open a PR without leaving the terminal:


```text
gh   pr   create   \
--title   "feat: add JWT refresh token rotation"   \
--body   "## What this does
Rotates the refresh token on every 401 response to prevent session fixation.


## Testing
- All unit tests passing
- Manually tested with expired access token
- No changes to auth API surface


Fixes #142"   \
--base   main   \
--head   feat/add-user-auth
```


Claude Code can generate this entire command — including the PR body — from the diff. Prompt it:


```text
Create a PR for the changes on this branch. Include what was changed and why in the body. Link to the relevant issue if one is open.


```


For PRs not ready for review:


```text
gh   pr   create   --draft   --title   "WIP: user auth refactor"
```


Draft PRs signal to teammates that the work is in progress. Claude Code converts them to ready-for-review once the implementation is complete:


```text
gh   pr   ready   [PR-number]
```


PR creation and CI automation from Claude Code — green checks, review approvals, and merge all handled from the terminal


Blink


## Handle Review Comments Without Leaving the Terminal


A reviewer leaves comments. You want to address them without switching to the browser.


Fetch the comments:


```text
gh   pr   view   47   --comments
```


Paste the output into your Claude Code session:


```text
Here are the review comments on PR #47. Address all of them and push the updates to the same branch.


```


Claude Code reads each comment, applies the changes, and when done, you push:


```text
git   push   origin   feat/add-user-auth
```


The PR updates automatically. Then respond in the thread:


```text
gh   pr   review   47   --comment   --body   "All review comments addressed in latest commits."
```


Request re-review from a specific person:


```text
gh   pr   edit   47   --add-reviewer   username
```


This loop — fetch comments, fix, push, respond — is the core of interactive code review with Claude Code. It works for any PR, any reviewer, any codebase.


## Check CI Status and Merge


You don't need to open GitHub to see if CI passed.


```text
gh   pr   checks   47
```


Output shows every check, its status, and a link to the failing job. Claude Code reads this output. If a test is failing, it goes back to fix the code and push again — without you needing to click through to the Actions tab.


When checks pass and the PR is approved, merge:


```text
gh   pr   merge   47   --squash   --delete-branch
```


` --squash` collapses all commits into one clean merge commit.` --delete-branch` removes the feature branch after merge. For teams that require linear history:


```text
gh   pr   merge   47   --rebase   --delete-branch
```


## Create Issues for Future Work


Claude Code surfaces improvement opportunities constantly. Capture them immediately instead of letting them disappear into conversation history.


```text
gh   issue   create   \
--title   "Refactor: extract token validation into shared middleware"   \
--body   "Token validation is duplicated across 4 route handlers. Should be extracted to Express middleware."   \
--label   "refactor,good-first-issue"
```


Then list open issues when starting a new session:


```text
gh   issue   list   --assignee   @me   --state   open
```


Claude Code reads this list and picks up the highest-priority task. This turns conversation notes into trackable, searchable work items — one of the best practices for teams using Claude Code at scale.


## Automate PR Review With GitHub Actions


The` gh` CLI handles the interactive half of the GitHub workflow.[Claude Code's GitHub Actions integration](https://github.com/anthropics/claude-code-action) handles the automated half — running Claude directly in CI on every PR.


Add` .github/workflows/claude-review.yml` :


```text
name  :   Claude Code Review


on  :
pull_request  :
types  : [  opened  ,   synchronize  ]


jobs  :
review  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   uses  :   anthropics/claude-code-action@beta
with  :
anthropic_api_key  :   ${{ secrets.ANTHROPIC_API_KEY }}
```


This triggers a Claude Code session on every PR push. It can leave review comments, flag issues, and suggest fixes without manual review time. If you're looking for the broader CI/CD picture, see our[Claude Code GitHub Actions guide](https://blink.new/blog/claude-code-github-actions) for the full pipeline setup.


For developers newer to Claude Code's setup, the[Claude Code tutorial for beginners](https://blink.new/blog/claude-code-tutorial-for-beginners) covers` CLAUDE.md` configuration, memory, and project initialization from scratch — useful context before adding GitHub automation on top.


The complete Claude Code GitHub workflow — branch, PR, review, CI, merge — all automated from the terminal


Blink


## Build and Deploy Your GitHub Project With Blink


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build this feature, create a PR, and deploy it on Blink Cloud."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## Frequently Asked Questions


You install` gh` separately — it does not come bundled with Claude Code. Run` brew install gh` (macOS),` sudo apt install gh` (Linux), or` winget install GitHub.cli` (Windows), then run` gh auth login` to connect your GitHub account. Once authenticated, Claude Code can run all` gh` commands in your terminal sessions without additional configuration.


Yes — if you give Claude Code permission to run` git push` and` gh pr create` , it executes them without prompting. You can add a rule to` CLAUDE.md` like "Always confirm before pushing to remote branches" if you want a manual checkpoint. For fully automated workflows, the GitHub Actions integration (` claude-code-action` ) runs Claude entirely in CI with no manual approval step required.


The` gh` CLI works inside your Claude Code terminal sessions — it's the right tool for the interactive daily workflow: create branch, commit, open PR, respond to review comments. The GitHub MCP server gives Claude Code direct API access to GitHub from outside a terminal session, which is useful for automated pipelines, reading files across repos, or multi-repo orchestration. For most developers, the` gh` CLI is sufficient. See our[best MCP servers for developers](https://blink.new/blog/best-mcp-servers-developers) guide for GitHub MCP setup details.


Add commit style guidelines to your` CLAUDE.md` file in the repo root. Specify the format (conventional commits, a Jira ticket prefix, etc.), a subject line length limit, and whether to include a commit body. Claude Code reads` CLAUDE.md` at session start and applies those rules throughout. Pairing this with` git add -p` for interactive staging also helps — it forces Claude Code to evaluate each change hunk, which produces more focused, precise commit messages.


Yes. Run` gh pr view \[PR-number\] --comments` in your Claude Code session to fetch all review comments, then paste the output into the conversation and ask Claude Code to address the feedback. It reads each comment, applies the changes, and pushes the updated branch. You can also configure the GitHub Actions integration to trigger Claude Code automatically when a reviewer requests changes — no manual` gh` commands needed for that path.


Fetch the latest changes with` git fetch origin` , then rebase:` git rebase origin/main` . Claude Code reads the conflict markers in each file and resolves them. After resolving, run` git add .` and` git rebase --continue` . If the rebase gets complex,` git rebase --abort` resets to the pre-rebase state cleanly. The best prevention: add a rule to` CLAUDE.md` to rebase against main at the start of each session — resolving one day of drift is far easier than resolving two weeks.
