---
schema_version: "1.0.0"
document_id: "58458af913f1c5b72619c328d59998df40f8afabd821336a118c459bad9845e7"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-workflow-templates"
published_at: "2026-05-15T01:11:40+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:13:38.923429+00:00"
content_hash: "sha256:7fc6d3c2afa3a9d47c42cdaad7b78c2ba407dca66ea08045ac9ca7177b39cdf9"
---

# Best OpenClaw Workflow Templates: 12 Ready-to-Use Automation Recipes

## Developer Workflows


Developer using OpenClaw workflow templates to automate PR reviews and code quality checks across multiple repositories


Blink


### 5. PR Review Bot — Saves 2-4 hours/week


**What it does:** Reviews new pull requests within 30 seconds of opening. Comments on logic errors, security issues, missing tests, and style violations.


**Triggers:** Webhook on PR opened (or scheduled every 30 min)


**Skills needed:**` github-issue-triage` (v2.4.1)


**Starter config:**


```text
## PR Review — On New PR Opened
When triggered by a new pull request:
1.   Read the full diff and surrounding context
2.   Check for security issues (auth bypasses, SQL injection, XSS)
3.   Check for logic errors and edge cases
4.   Check that tests cover new code paths
5.   Post a structured review comment


Format:
## AI Review Notes
**Critical (must fix):**   [list, or "None"]
**Suggested (worth considering):**   [list, or "None"]
**Tests:**   [coverage assessment]


Label the PR "ai-reviewed" when done.
Keep tone constructive. Flag issues, don't block on style nitpicks.
```


---


### 6. Issue Triage and Auto-Labeling — Saves 2 hours/week


**What it does:** Labels and assigns every new GitHub issue within 30 seconds. Routes to CODEOWNERS. Closes stale issues after 30 days.


**Triggers:** Webhook on issue created; scheduled weekly for stale cleanup


**Skills needed:**` github-issue-triage`


**Starter config:**


```text
# ~/.openclaw/config.yaml
github  :
autoLabel  :   true
autoAssign  :   true
useCodeowners  :   true
staleDays  :   30
labelRules  :
-   keywords  : [  "auth"  ,   "login"  ,   "oauth"  ]
label  :   "area:auth"
-   keywords  : [  "database"  ,   "sql"  ,   "migration"  ]
label  :   "area:database"
-   keywords  : [  "crash"  ,   "exception"  ,   "error"  ]
label  :   "type:bug"
-   keywords  : [  "security"  ,   "vulnerability"  ]
label  :   "priority:critical"
alsoLabel  :   "security"
```


Teams using this workflow report processing[80+ hours per quarter](https://getclawdbot.com/blog/openclaw-github-issue-triage-guide/) of triage overhead automatically.


---


### 7. Deployment Notification Watcher — Saves 30 min/week


**What it does:** Monitors CI/CD pipelines. Sends a Telegram notification when a deployment succeeds or fails. Includes a link to the run and a one-line status.


**Triggers:** Webhook on CI completion


**Skills needed:** GitHub integration (built-in)


**Starter config:**


```text
## Deployment Watcher — On CI Completion
When a GitHub Actions workflow completes:
-   If success: "✅ Deployed to [  environment  ]: [  branch  ] — [commit message] ([  link  ])"
-   If failure: "❌ Deployment failed: [  branch  ] — [error summary] ([  link  ])"


Send to Telegram immediately.
For failures, also check the logs for the most likely cause and include it in 1 sentence.
```


---


### 8. Weekly Code Quality Report — Saves 1 hour/week


**What it does:** Runs weekly static analysis, checks for TODO comments, reviews test coverage trends, and flags files that have grown over 500 lines.


**Triggers:** Mondays at 8 AM


**Skills needed:** None (uses shell execution)


**Starter config:**


```text
## Weekly Code Quality Report — 8 AM Mondays
Run on the main codebase:
1.   Count TODO/FIXME comments and list the top 5 by age
2.   List any files over 500 lines (these usually need decomposition)
3.   Summarize test coverage change since last Monday (if available)
4.   Flag any dependencies with known vulnerabilities (npm audit / pip check)


Format as a Markdown report. Post to Slack in #engineering.
Keep it under 300 words — highlight the top 3 things to address.
```


---


## Business Operations Workflows


### 9. Lead Follow-Up Automation — Saves 3+ hours/week


**What it does:** Checks your CRM for leads with no activity in 3+ days. Drafts a follow-up message for each. Presents for review before sending.


**Triggers:** Weekday mornings at 9 AM


**Skills needed:** CRM connector (varies by CRM)


**Starter config:**


```text
## Lead Follow-Up Check — 9 AM Weekdays
Check CRM for leads:
-   Stage: "Contacted" or "Demo Scheduled"
-   Last activity: 3+ days ago
-   No meeting booked


For each qualifying lead:
1.   Read the conversation history
2.   Draft a short follow-up (under 100 words)
3.   Personalize based on what was discussed


Present all drafts as a Telegram message formatted like:
---
Lead: [  Name  ] at [  Company  ]
Last contact: [X days ago]
Draft: "[  message  ]"
Approve? (send "approve [  name  ]" to send)
---
```


---


### 10. Meeting Notes Processor — Saves 30 min/meeting


**What it does:** After each meeting, takes your raw notes or transcript and produces: action items with owners, key decisions, and a 3-bullet summary ready to email to attendees.


**Triggers:** Manual (run after each meeting)


**Skills needed:** None


**Starter config:**


```text
## Meeting Notes Processor — On Demand
When given raw meeting notes or a transcript:
1.   Extract action items (format: [  Action  ] — [  Owner  ] — [Due date])
2.   List key decisions made
3.   Write a 3-bullet summary


Output format:
## [Meeting Name] — [  Date  ]
**Summary:**
-   [bullet 1]
-   [bullet 2]
-   [bullet 3]


**Decisions:**
-   [decision 1]
-   [decision 2]


**Action Items:**
-   [ ] [  action  ] — @[  owner  ] — due [  date  ]


Save to ~/meetings/YYYY-MM-DD-[  name  ].md
```


---


### 11. Content Calendar Manager — Saves 2 hours/week


**What it does:** Tracks your content pipeline. Reminds you when drafts are due, when content is scheduled to publish, and flags anything that's falling behind.


**Triggers:** Weekday mornings at 8 AM


**Skills needed:** Airtable or Notion connector (optional)


**Starter config:**


```text
## Content Calendar Check — 8 AM Weekdays
Review content pipeline for the week:
-   What's due for drafting in the next 3 days?
-   What's scheduled to publish this week?
-   What's overdue (past due date with no draft)?


Send a brief to Telegram:
-   "Due this week: [list with dates]"
-   "Publishing: [  list  ]"
-   "Behind: [list — flag these]"


Keep it short. I need to know what needs attention today.
```


---


### 12. Weekly Business Digest — Saves 1 hour/week


**What it does:** Every Monday morning, pulls revenue metrics, key support tickets from the past week, and top acquisition channels. Summarizes into a 5-minute read for leadership.


**Triggers:** Mondays at 7 AM


**Skills needed:** Stripe connector, analytics connector (varies)


**Starter config:**


```text
## Weekly Business Digest — 7 AM Mondays
Compile a weekly digest:


Revenue (Stripe):
-   New MRR this week vs last week
-   Churned MRR
-   Top 3 new customers


Support:
-   Number of tickets opened/closed this week
-   Top 3 recurring issues by volume
-   Any unresolved P1 tickets


Growth:
-   Top 3 acquisition channels (from analytics)
-   Any significant traffic spikes or drops


Format as a clean weekly report (under 400 words).
Send to Slack in #leadership.
```


---


## How to Customize and Combine Templates


These templates are starting points. The most powerful automations combine multiple workflows.


**Example: Full Developer Operations Stack**


Combine templates 5, 6, 7, and 8 to run a near-zero-overhead engineering team:


- PRs reviewed automatically on open
- Issues triaged on creation
- Deployments announced in real-time
- Code quality reviewed every Monday


**Example: Solo Founder Stack**


Combine templates 1, 2, 9, 10, and 12 for a personal chief of staff:


- Morning briefing before you open Slack
- Email triaged every 2 hours
- Lead follow-ups drafted automatically
- Meeting notes processed post-call
- Weekly business digest every Monday


**Customization tips:**


- Replace generic outputs ("send to Telegram") with your actual channels and formats
- Add your specific tools ("check Notion", "check Linear") instead of generic ones
- Start with 2-3 templates, verify they work, then expand
- Keep instructions specific — vague instructions produce vague results


## Setting Up Always-On Execution with Blink Claw


These workflows only deliver value if they run when scheduled.


An agent on your laptop misses every trigger that fires while you're away. A morning briefing that never reaches you. A PR review that waits until you open your machine. An issue triage that falls silent over the weekend.


Blink Claw runs your OpenClaw agent 24/7 for $22/mo all-in — LLM costs included via 200+ model router. No Docker, no VPS, no server maintenance. Your agent receives messages from Telegram, Discord, or Slack around the clock. 30+ data center regions keep it fast globally.


Security patches apply automatically. You never track CVEs. You write the workflows — Blink Claw makes sure they run.


→[Run these workflows 24/7 with Blink Claw → blink.new/claw](https://blink.new/claw)


More workflow resources:


- [OpenClaw SOUL.md and HEARTBEAT.md Setup Guide](https://blink.new/blog/openclaw-soul-heartbeat-setup) — configure the files that power every workflow
- [OpenClaw GitHub Automation](https://blink.new/blog/openclaw-github-automation) — deep dive on developer workflows 5-8
- [OpenClaw Morning Briefing via Telegram](https://blink.new/blog/openclaw-morning-briefing-telegram) — detailed walkthrough for workflow #1


Personal productivity dashboard showing automated workflow results from an AI agent handling daily tasks


Blink


No. Pick one template in a category that costs you real time today. Set it up, run it for a week, and verify it actually saves you effort. Most users start with the Morning Briefing (template 1) or PR Review Bot (template 5) because the ROI is immediately visible. Add templates incrementally.


Store credentials in your` TOOLS.md` as environment variable references — never paste keys directly. Example:` Stripe: $STRIPE_SECRET_KEY (in .env)` . Your agent reads` TOOLS.md` every session and knows where to find credentials without exposing them in plain text.


Add format specifics to your HEARTBEAT.md instruction. Instead of "send a summary," write: "Send a summary formatted as bullet points, max 200 words, via Telegram to @username." The more specific the instruction, the more consistent the output.


Yes. Multiple cron jobs can trigger different HEARTBEAT.md sections. Each section runs independently. For complex chains (where step B depends on step A's output), use OpenClaw's Task Flow to manage state across steps.


Trigger it manually:` openclaw cron trigger \[name\]` . Watch the output in real-time. Verify the format, check that tool calls succeeded, confirm the right channels received output. Fix any issues before leaving it to run on schedule.


Most will. Templates that rely on semantic understanding (duplicate detection, code review, meeting notes) work significantly better with larger models. Templates with simple structured tasks (email triage, habit tracking) work fine with smaller local models. Test your specific setup —[OpenClaw supports Ollama and other local model backends](https://getclawdbot.com/blog/openclaw-github-issue-triage-guide/) .
