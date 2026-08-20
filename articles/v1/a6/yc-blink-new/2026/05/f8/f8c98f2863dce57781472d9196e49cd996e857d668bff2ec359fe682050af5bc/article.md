---
schema_version: "1.0.0"
document_id: "f8c98f2863dce57781472d9196e49cd996e857d668bff2ec359fe682050af5bc"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-autonomous-coding"
published_at: "2026-05-18T13:31:49+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:13:05.421454+00:00"
content_hash: "sha256:ec166efc8c8b13a394b5294bbe863a14697fcbc484e04ca7fd8f9c27413eece0"
---

# OpenClaw Autonomous Coding: Let Your Agent Fix Bugs and Open PRs While You Sleep

## Step 1: Install the GitHub Skill


From your OpenClaw CLI:


```text
skill   add   github-issue-resolver
skill   add   github-pr-reviewer
```


Or from Blink Claw's skill dashboard — one-click install, no terminal required.


These skills install the tool definitions: PR reading, comment posting, issue labeling, commit creation, PR opening. Your agent calls these tools during every automation cycle.


## Step 2: Authenticate and Give Repo Access


Go to GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens. Create a token with:


- **Contents** : Read and write
- **Pull requests** : Read and write
- **Issues** : Read and write


Scope it to the specific repositories you want the agent to work in — not your entire account.


Store the token in your agent's` SOUL.md` secrets section:


```text
## Secrets
GITHUB_TOKEN: ghp_your_token_here
GITHUB_REPO: yourorg/yourrepo


```


OpenClaw loads secrets at startup and injects them as environment variables. The GitHub skills pick them up automatically.


## Step 3: Configure Your SOUL.md for Developer Tasks


The` SOUL.md` file controls your agent's identity, standards, and behavior. Add a developer-specific section:


```text
## Role
I am a developer agent for [  yourrepo  ]. I fix bugs, review PRs, and keep
the codebase clean — autonomously.


## Code Standards
-   Every new endpoint must have authentication middleware
-   Every database query must have a LIMIT clause
-   Test coverage must stay above 80%
-   No direct pushes to main — all changes go through PRs


## When to Escalate (do not attempt autonomously)
-   Changes touching /src/auth/ or /src/payments/
-   Database schema migrations
-   Changes affecting more than 5 files
-   Issues labeled 'architecture' or 'security'


## Overnight Task Queue
-   Check for failing tests every morning at 6am
-   Review open PRs every 2 hours
-   Generate deprecation report every Sunday at 10pm
```


The escalation section is critical. An agent with clear stop conditions is safer than one with unconstrained access.


Developer arriving Monday morning to find OpenClaw agent completed overnight work — fixed tests, opened PRs, triaged issues


Blink


## Step 4: Set Up Your First Automation


The simplest starting automation: automatic triage of new GitHub issues labeled` bug` .


In your` HEARTBEAT.md` , add:


```text
every 10 minutes:
- Check for new GitHub issues labeled 'bug'
- For each new issue: read the issue body, locate the relevant code,
attempt a fix, run tests, open a PR if tests pass, comment with
progress summary


```


That's it. Label a bug` bug` . The agent picks it up within 10 minutes, writes a fix, and either opens a PR or reports back what it tried.


Blink Claw runs your agent 24/7 — your coding agent keeps working even when you're offline. If you're self-hosting, the same automation runs only when your machine runs — which means the Friday evening bug doesn't get a PR until Monday morning when you open the laptop.


## Step 5: Test With a Known Bug


Before you trust the agent with real bugs, test it on one you understand.


Create a GitHub issue with a clear bug description: "Function calculateTotal() in src/utils/pricing.ts returns NaN when the discount field is null. Expected: 0." Label it` bug` .


Watch the agent cycle. Within one run interval, it should:


1. Read the issue
2. Open` src/utils/pricing.ts`
3. Identify the null check gap
4. Write the fix
5. Run` npm test` (or your test command from SOUL.md)
6. Open a PR with the fix and test output


If it fails, check the SOUL.md for missing repo context. The agent performs better when it knows your test command, your code structure, and your conventions.


Developer reviewing pull request opened autonomously by OpenClaw coding agent — clear description, correct fix


Blink


## The Limits


OpenClaw handles repetitive coding tasks well. It does not replace senior engineering judgment.


**Where it performs well:**


- Null check bugs and defensive coding gaps
- Missing test coverage for existing functions
- PR standards enforcement (style, auth patterns, query limits)
- Documentation updates after feature merges
- Dependency version bumps for non-breaking upgrades


**Where you stay hands-on:**


- Architectural decisions involving new data models
- Security-sensitive code paths
- Performance work requiring profiling data
- Database migrations
- Any change that affects the deployment configuration


The clearer your` SOUL.md` escalation rules, the more you can trust the agent's autonomous decisions. Teams that define clear scope in week one report far fewer "wait, why did the agent do that?" moments by week two.


## Self-Hosting vs Blink Claw


### Self-hosted OpenClaw


Runs on your machine or a VPS. Requires Docker setup, manual security patches, and goes offline when your machine does. Works for developers comfortable with infrastructure.


[Blink Claw Managed cloud. No Docker. No VPS. Your agent runs 24/7 — not just when your laptop is on. Message it from Telegram, Discord, or Slack. Security patches applied automatically. $22/mo all-in, LLM costs included via 200+ model router.](https://blink.new/claw)


Self-hosted Blink Claw


Setup time 2–4 hours (Docker, VPS config) Under 5 minutes


Uptime Tied to your machine 24/7 cloud


LLM costs Separate API keys + bills Included ($22/mo all-in)


Model access One API key at a time 200+ models, no switching


Security patches Manual, your responsibility Automatic


Messaging interface CLI only Telegram, Discord, Slack


Monthly cost $0 infrastructure + $20–80 LLM bills $22/mo flat


At $22/mo all-in, Blink Claw costs less than 30 minutes of developer time per month. For teams that want coding automation without the infrastructure management, it's the obvious path. Run OpenClaw without the hassle — Blink Claw handles everything from $22/mo →[blink.new/claw](https://blink.new/claw)


For deeper GitHub configuration and skill setup, see[OpenClaw GitHub automation](https://blink.new/blog/openclaw-github-automation) ,[best OpenClaw skills in 2026](https://blink.new/blog/best-openclaw-skills-2026) , and the[OpenClaw getting started guide](https://blink.new/blog/openclaw-getting-started) . For reference, the[official OpenClaw documentation](https://docs.openclaw.ai/) covers the full task flow model.


---


Yes. OpenClaw only works on bugs when it's running. If your agent is on a laptop that sleeps, it can only process issues when the laptop is open. For overnight autonomous work, you need a persistent runtime — either a VPS you manage yourself, or managed hosting like Blink Claw which keeps your agent running 24/7 without any infrastructure work.


Read and write access to Contents, Pull requests, and Issues on the target repositories. Use a fine-grained personal access token scoped to the specific repos you want the agent to work in — never a classic token with account-wide access. Rotate the token every 90 days as a habit.


Yes. Configure your SOUL.md to require tests alongside every fix. The agent generates tests as part of the bug-fix loop, runs them, adjusts if they fail, and only opens the PR once both the fix and new tests pass. The test output appears in the PR description for your review.


It goes through your normal review process. You review and approve before anything merges. You can also configure the agent to open draft PRs only, which keeps everything in review state until you explicitly move it forward. No auto-merge unless you configure it yourself.


Simple bugs — null checks, wrong variable, off-by-one errors — typically produce a PR within 5–15 minutes. Complex bugs requiring multi-file changes can take 30–60 minutes. If the agent hits your configured iteration limit without a passing solution, it opens a draft issue summarizing what it tried and where it got stuck.


Yes. Configure one agent per repository in your SOUL.md, or configure a single agent with access to multiple repos and routing rules that determine which skills apply to which repo. Most teams start with one agent per active repo and expand once they're comfortable with the workflow.
