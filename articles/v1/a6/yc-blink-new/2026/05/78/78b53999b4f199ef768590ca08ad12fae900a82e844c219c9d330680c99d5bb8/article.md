---
schema_version: "1.0.0"
document_id: "78b53999b4f199ef768590ca08ad12fae900a82e844c219c9d330680c99d5bb8"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-github-actions"
published_at: "2026-05-09T02:56:08+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:12.047673+00:00"
content_hash: "sha256:e683b9643b4107622fc95107d6b69b31fd94ffee2077e39dd6e0a9cb1c44de2a"
---

# Claude Code in GitHub Actions: Automate PR Reviews and Bug Fixes

## Three Workflows That Return Real Value


### Workflow 1: Automatic PR review with custom standards


Add a` CLAUDE.md` file at your repository root to guide what Claude flags:


```text
# Review standards


-   Flag any async function without explicit error handling
-   Flag any SQL query without parameterization
-   Flag functions over 40 lines without a docstring
-   Skip node_modules, lock files, and generated migration files
```


Claude reads this file before every review. The more specific your rules, the less noise. This is the same` CLAUDE.md` your[power user Claude Code workflows](https://blink.new/blog/claude-code-tips-power-users) already use for local context — the GitHub Action reads it the same way.


A tighter prompt for the automated review step:


```text
prompt  :   "Review this pull request against the standards in CLAUDE.md. Post inline review comments on specific diff lines. Skip findings that are already flagged as known issues in existing PR comments."
claude_args  :   "--max-turns 5"
```


### Workflow 2: Auto-fix failing tests


```text
name  :   Fix Failing Tests
on  :
check_run  :
types  : [  completed  ]
jobs  :
fix  :
if  :   github.event.check_run.conclusion == 'failure'
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
-   uses  :   anthropics/claude-code-action@v1
with  :
anthropic_api_key  :   ${{ secrets.ANTHROPIC_API_KEY }}
prompt  :   "The test suite just failed. Read the failing output, identify the root cause across all relevant files, fix the implementation, and open a pull request with your changes. Do not modify the tests — only fix the implementation."
claude_args  :   "--max-turns 10"
```


This triggers on any` check_run` marked as failed.` --max-turns 10` gives Claude enough rounds to trace multi-file failures. The` Do not modify the tests` instruction prevents the common failure mode where the model deletes assertions to make the suite pass.


### Workflow 3: Release notes on merge to main


```text
name  :   Release Notes
on  :
push  :
branches  : [  main  ]
jobs  :
notes  :
runs-on  :   ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4
with  :
fetch-depth  :   30
-   uses  :   anthropics/claude-code-action@v1
with  :
anthropic_api_key  :   ${{ secrets.ANTHROPIC_API_KEY }}
prompt  :   "Read the last 30 commits. Generate structured release notes grouped as: Features, Bug Fixes, Breaking Changes. Skip merge commits and chore/refactor commits unless they affect public API. Post as a GitHub issue titled 'Release Notes — [today's date]'."
claude_args  :   "--max-turns 5"
```


` fetch-depth: 30` tells` actions/checkout` to pull the last 30 commits. Without it, Claude sees only the latest commit and can't read the history.


For a deeper look at which tasks are worth automating vs. doing by hand, the[10 repetitive developer tasks to automate with Claude Code](https://blink.new/blog/claude-code-workflow-automation) guide covers the same patterns applied to local workflows.


## What to Watch Out For


Real limitations of Claude Code GitHub Actions — cost per run, token limits, and when not to automate


Blink


**Cost per run adds up.** Small PRs on Sonnet cost roughly $0.50–$2 per review. Large PRs on complex codebases can reach $5–$15. At high PR volume, monitor your API spend the same way you monitor Actions minutes. Use` --max-turns` conservatively and` paths:` filters to run reviews only on directories that matter:


```text
on  :
pull_request  :
paths  :
-   'src/**'
-   'lib/**'
```


**Token limits hit large monorepos.** Claude reads the surrounding codebase, not just the diff. On very large repositories, context can overflow and the analysis will cut off mid-review. Add explicit scope to your prompt:` "Focus only on files changed in this PR, not the full codebase."`


**It's wrong sometimes.** The 80/100 confidence threshold filters most false positives. It doesn't filter all of them. In real-world testing, the pipeline flagged pre-existing bugs on PRs that didn't introduce them — technically correct, but noise. Treat Claude's output as a first pass, not a merge gate.


**Security review still needs humans.** Automated review caught a removed auth guard in the LogRocket real-world tests. It missed a silent authentication token error that scored 75 — one point below the confidence threshold. For auth code and payment flows, automated review supplements engineer review; it doesn't replace it.


**GitHub Actions minutes.** Each review spins up a runner. Free-tier private repos have a monthly minute limit. Set` timeout-minutes` at the workflow level to prevent runaway jobs from consuming your budget.


## Build GitHub Actions Automation Into Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a GitHub Actions dashboard that shows PR review status, CI pipeline health, and automated comment activity across all my repos, and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


## FAQ


No. GitHub Actions uses the Claude API directly — you need an API key from[console.anthropic.com](https://console.anthropic.com/) , not a Pro or Max subscription. Those subscriptions cover the Claude.ai chat interface and the Claude Code desktop application. For CI use, pay-as-you-go API billing is the right setup. Rate limits apply on the free tier, so teams with high PR volume typically use a paid API tier.


It depends on diff size and how much codebase context Claude reads. Small PRs (under 100 lines changed) typically cost $0.50–$2 per review on Sonnet. Larger PRs on complex codebases can reach $5–$15. Claude Opus 4.7 is available for more complex reviews but costs more per token. Set` --max-turns 5` and use` paths:` triggers to keep per-review costs predictable. The[GitHub Actions billing docs](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/about-billing-for-github-actions) cover runner costs separately.


Yes. When triggered via` @claude` on an issue or PR, Claude can create commits, push branches, and open pull requests. The GitHub App needs Contents: Read & Write permission. For automated workflows, Claude runs on your runner and commits via the GitHub token. You control what it's allowed to do via` --allowedTools` in` claude_args` . The[official security documentation](https://docs.anthropic.com/en/docs/claude-code/github-actions) covers permission scoping in detail.


Automatic PR review uses the` prompt` parameter in your workflow — Claude posts comments on every push without waiting for a human trigger.` @claude` mention mode listens for comments containing` @claude` and responds to whatever the commenter asked. Both use the same` anthropics/claude-code-action@v1` action; the mode is auto-detected based on whether a` prompt` is provided. Most teams run both: automatic review on every push for the first-pass mechanical check, plus` @claude` for interactive follow-up.


Yes. Enterprise environments can route Claude API calls through Amazon Bedrock or Google Vertex AI for data residency control and unified cloud billing. Bedrock setup requires GitHub OIDC configured in AWS and an IAM role with Bedrock permissions. Vertex AI setup requires Workload Identity Federation in GCP. Both avoid storing static credentials in GitHub Secrets. The complete setup for each provider is documented in the[Claude Code GitHub Actions docs](https://docs.anthropic.com/en/docs/claude-code/github-actions) .
