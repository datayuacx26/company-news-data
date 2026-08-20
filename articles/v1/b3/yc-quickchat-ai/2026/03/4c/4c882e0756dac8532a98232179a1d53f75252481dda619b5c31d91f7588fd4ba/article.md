---
schema_version: "1.0.0"
document_id: "4c882e0756dac8532a98232179a1d53f75252481dda619b5c31d91f7588fd4ba"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/automate-bug-triage-with-claude-code-and-datadog"
published_at: "2026-03-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:ca186a489597b4f4c3eba00b1c9eda5cbb05d8b705f7227ca5bcb46dd425e8cb"
---

# I'm Too Lazy to Check Datadog Every Morning, So I Made AI Do It

## Confession Time


I don’t want to check Datadog every morning. There, I said it.


Don’t get me wrong — I love that we have monitoring. I love that alerts exist. I love that somewhere, a dashboard is faithfully tracking every 5xx error our platform produces. It’s just a tedious job begging to be automated.


At Quickchat, we handle thousands of conversations daily across Slack, Telegram, WhatsApp, Intercom, and more. Our Datadog is… busy. Every morning the ritual is the same: scroll through Datadog alerts on Slack, squint at error spikes, mentally classify each one as “real problem” or “meh, transient,” and then finally start actually writing code around 11am.


I figured there had to be a lazier way.


## The Laziest Possible Solution


As any self-respecting programmer knows, the best kind of work is the kind you automate away. So I asked myself: what if I never had to open Datadog again? What if an AI could check it for me, figure out what’s actually broken, dig through the codebase, fix it, and open a PR — all before I finish my first coffee?


Here’s what I built in about 30 minutes (because spending more than that would defeat the purpose of being lazy):


1. **Datadog MCP Server** gives Claude Code access to live monitoring data
2. **A Claude Code skill** tells the AI how to triage alerts like a responsible engineer (something I aspire to be)
3. **A cron job** kicks it off every weekday at 8am
4. **Parallel AI agents** each grab an issue, spin up isolated worktrees, and open PRs


Let me walk you through it — slowly, because I’m in no rush.


## Step 1: Plug Datadog Into Claude Code (2 minutes)


The[Model Context Protocol (MCP)](https://modelcontextprotocol.io/) lets AI tools talk to external services. Datadog has a remote MCP server with OAuth, so there are zero API keys to manage. My favorite kind of setup: the kind where I barely have to do anything.


One file in the repo root:


```text
// .mcp.json
{
"mcpServers"  : {
"datadog"  : {
"type"  :   "http"  ,
"url"  :   "https://mcp.datadoghq.eu/api/unstable/mcp-server/mcp"
}
}
}
```


Done. Every developer on the team gets it automatically. First launch asks you to click a button in the browser to authenticate. Maximum effort: one click.


(Swap` datadoghq.eu` for` datadoghq.com` if you’re on the US1 region.)


## Step 2: Teach the AI to Do My Job (10 minutes)


Claude Code has this concept of **skills** — markdown files that live in` .claude/skills/` and act as reusable prompt templates. If you’re new to Claude Code workflows,[our AI coding tips](https://quickchat.ai/post/my-ai-coding-tips) cover the fundamentals. I created` /triage-datadog` , which is essentially a document explaining to an AI how to do the morning triage I’ve been avoiding.


The skill has four phases:


**Gather** — “Hey Claude, go check Datadog for anything that blew up in the last 24 hours. Monitors, error logs, incidents, the works.”


**Classify** — sort findings into three piles:


- **Actionable** — actual code bugs. The good stuff
- **Infrastructure** — server problems. Not my department (just kidding, it’s also my department, but let’s pretend)
- **Noise** — transient blips that resolved themselves. The universe’s way of testing our alert fatigue


**Fix** — for each real bug, spin up an AI agent in an isolated git worktree. It reads the codebase, finds the root cause, writes a fix with tests, and opens a PR. All by itself. While I’m doing literally anything else.


**Report** — summarize everything in a neat table so I can glance at it and feel informed.


The agents run in parallel because waiting for them sequentially would be… well, a waste of my time not doing anything.


## Step 3: The Cron Job That Changed My Mornings (1 minute)


The skill works great when invoked manually. But manually invoking things every morning is exactly the kind of responsibility I’m trying to escape.


One line in the crontab:


```text
3   8   *   *   1-5   claude   -p   --dangerously-skip-permissions   '/triage-datadog'
```


That’s` claude -p` for “just print the output and exit, don’t try to have a conversation with me.” The` --dangerously-skip-permissions` flag sounds scary, but it just means the agent won’t pause and wait for a human to click “approve” on every file read. In practice, each agent runs in a dedicated, isolated environment — a[sandboxed macbox session](https://quickchat.ai/post/tmux-session-summaries-for-parallel-ai-agents) with scoped git worktrees and no access to production infrastructure, secrets, or deployment pipelines. The agent can read code, write fixes, and open PRs. That’s it. And` 1-5` means weekdays only — even AI deserves weekends.


Want to sleep better at night? You can lock down what tools it can use:


```text
claude   -p   --dangerously-skip-permissions   --allowedTools   "Bash(git:*) Bash(gh:*) Edit Read Grep Glob Agent"   '/triage-datadog'
```


This explicit tool allowlist is the final layer — on top of the isolated environment, scoped filesystem access, and git worktree sandboxing. Belt, suspenders, and a parachute.


## My Morning Now vs. Before


**Before:** Wake up. Coffee. Open Datadog. Scroll. Squint. Sigh. Investigate. Maybe fix something. Start real work at 11.


**After:** Wake up. Coffee. Check Slack. See PRs already waiting for review. Approve the good ones. Start real work at 9:15.


Here’s what the triage report looks like:


```text
## Daily Datadog Triage Report — 2026-03-12


### Overview
- Total alerts/errors found: 7
- Actionable: 2
- Infrastructure (manual review): 1
- Noise (skipped): 4


### PRs Created
| Issue                          | Severity | PR    |
|--------------------------------|----------|-------|
| Unhandled TypeError in webhook | error    | #1842 |
| Missing rate limit on /export  | warning  | #1843 |


### System Health
Degraded — 1 infrastructure issue needs manual review
```


Two bugs found, two PRs created, four noisy alerts ignored, one infra issue flagged for a human. All before I even opened my laptop.


## Why Lazy Engineering Is Good Engineering


**The 30-minute investment keeps paying off.** Every merged fix means one fewer alert tomorrow. The dashboard gets quieter over time. Laziness compounds.


**Context is preserved.** Each PR explains which alert triggered it, what the root cause is, and how the fix works. No more Slack threads asking “does anyone know what this alert is about?” (That was usually me asking, by the way.)


**The whole setup is a markdown file and a cron line.** No infrastructure to maintain. No Docker containers to babysit. No Kubernetes YAML to debug. If I need to change the triage logic, I edit a text file. It’s almost too simple to write a blog post about, and yet here we are.


## The Fine Print (Because I’m Lazy, Not Reckless)


A few honest caveats:


- **This won’t save you during an outage.** If production is on fire, you still need a human. This handles the long tail of “eh, that’s probably a bug” errors that pile up in the backlog
- **OAuth tokens expire.** When they do, the cron job fails quietly. Check your logs sometimes. Or set up an alert for when the alert-checker fails. Yes, I see the irony
- **Your laptop needs to be awake.** Crontab doesn’t work when your Mac is sleeping. I run mine on a server that’s always on, but GitHub Actions with a cron trigger works just as well if you don’t have one
- **Review the PRs and inspect the alerts.** The AI is good, but it’s not infallible. It’s more like a very eager junior developer who never sleeps and never complains — you still want to review the code before merging and sanity-check that the triage classifications make sense


## Try It Yourself (It’s 30 Minutes, Tops)


If you use Claude Code and Datadog, here’s the full recipe:


1. Drop the` .mcp.json` config in your repo root
2. Create a skill under` .claude/skills/` with your triage logic
3. Add a crontab entry (or GitHub Actions cron) to run it weekdays
4. Run` /mcp` in Claude Code to authenticate with Datadog


That’s it. The[Datadog MCP Server docs](https://docs.datadoghq.com/bits_ai/mcp_server/setup/) have setup guides for other editors too.


## What’s Next (For When I Get Even Lazier)


The same pattern — MCP for data, skill for logic, cron for scheduling — works for basically anything repetitive. Security scans. Dependency updates. Performance regression checks. The building blocks are generic; you just swap the skill definition.


My ultimate goal? A Monday morning where I open my laptop, see a clean dashboard, a stack of pre-reviewed PRs, and absolutely nothing that requires my attention before lunch.


A programmer can dream.
