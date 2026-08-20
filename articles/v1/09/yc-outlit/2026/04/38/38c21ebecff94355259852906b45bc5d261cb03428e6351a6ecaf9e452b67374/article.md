---
schema_version: "1.0.0"
document_id: "38c21ebecff94355259852906b45bc5d261cb03428e6351a6ecaf9e452b67374"
company_key: "yc-outlit"
company: "Outlit"
source_id: "yc-outlit-news-import-30f1306359d9"
canonical_url: "https://www.outlit.ai/blog/tracking-customer-health-scores-with-outlit"
published_at: "2026-04-22T00:00:00+00:00"
first_seen_at: "2026-07-23T19:28:50.452707+00:00"
fetched_at: "2026-07-28T22:15:44.247179+00:00"
content_hash: "sha256:1ad8ee7610ba29e07bedaef24660f8d77b1b3ff4545ff3645ebe263bba7d6c4f"
---

# Tracking Customer Health Scores with Outlit

You can track customer health scores with[Outlit](https://outlit.ai/) by building them from queryable signals instead of a single number in a dashboard, then wiring[Claude Code](https://docs.anthropic.com/en/docs/claude-code) or any agent to read those signals before it takes action. This guide covers five workflows you can set up today: nightly health scoring, degradation response, recovery watch, expansion trigger, and weekly health drift reporting. Each workflow includes a ready-to-use prompt template and a complete CLAUDE.md you can drop into a project.


## Why traditional customer health scores don't work for AI agents


Most health scores are built for a dashboard. A CSM opens the view, sees red, yellow, or green, clicks in, reads a few notes, and makes a call. The score was designed so a human could scan it. The signals feeding the score were compressed into a number because a human cannot keep 12 variables in their head at once.


An agent has the opposite constraint. It does not need the number. It needs the evidence. If the agent is going to draft a recovery email, schedule a call, or flag an account for leadership, it needs to know which specific signals degraded, when, and by how much. "Account is red" is not enough. "API volume dropped 62% over 14 days, two P1 tickets opened this week, primary contact has not logged in for 21 days" is actionable.


A health score for agents is a set of signals any agent can query, cite, and reason over. The score itself is a derived reading, useful for sorting and filtering. The signals are what drive action. Build the signals first, and the score becomes a view on top of them rather than the source of truth.


Health score today


- One number per account in a dashboard view
- Human opens the dashboard and clicks in
- Context is buried in tabs and tooltips
- Action depends on whoever happens to look
- ~15 min per account to decide. Coverage is whoever logs in.


Health score for agents


- Signals any agent can query
- Agent returns a reading with evidence
- Drivers surface with a recommended action
- Same depth, every account, every day
- ~2 min per flagged account. Full-book coverage.


By the end of this guide, you'll have a CLAUDE.md, a signal set, and five prompt templates you can copy into a project and run today.


## What Outlit does for a health score


[Outlit](https://outlit.ai/) connects the sources your signals come from (your payment tool, product analytics, helpdesk, email, and team chat) and resolves every record through the[Customer Context Graph](https://www.outlit.ai/products/customer-context-graph) . That[customer context infrastructure](https://www.outlit.ai/blog/what-is-customer-context-infrastructure) gives an agent the unified profile: usage counts, support history, billing status, recent interaction tone, and timeline activity. Instead of asking "what is the health score for Canopy Analytics," the agent asks "what has happened on Canopy Analytics in the last 30 days" and computes the reading itself from signals it can cite. The same profile powers adjacent workflows like[automating customer success](https://www.outlit.ai/blog/how-to-automate-customer-success-with-claude-code) and[running your renewal process](https://www.outlit.ai/blog/how-to-run-your-renewal-process-with-outlit) ; a health score is a view on top of that context layer.


**How Outlit works.** Connect your data sources once (billing, product analytics, support, email, and team chat). Outlit resolves every record to an account-level profile and keeps it current. Coding agents query that profile through the Outlit CLI and installed skill; remote MCP clients use a workspace MCP URL with OAuth.


## The signal set


The signal set is the contract between your agents and your health score. Every workflow in this guide reads from the same signals, so defining them first makes everything downstream consistent. Start with five to eight signals. More than that and nobody will understand what drives a reading.


A reasonable starting set looks like this:


Signal set


definition


```text
# Usage signals
usage_trend_30d          # daily active users, last 30 vs prior 30
core_feature_use         # count of key events in last 14 days


# Relationship signals
primary_contact_activity     # days since last login by most active user
qbr_recency              # days since last QBR, scheduled status


# Support signals
ticket_volume_14d        # count and priority distribution
unresolved_p1_count      # open high-priority tickets


# Commercial signals
plan_utilization         # usage as % of plan limits
billing_status           # current, past due, or in dunning
```


Each signal should have a clear source in Outlit, a defined window, and a threshold that flips it from healthy to degraded. The agent computes a score by counting degraded signals and weighting them, but it always returns the underlying signals alongside the score. A "red" reading with five degraded signals is a different action than a "red" reading with one severely degraded signal.


## How an agent uses the signals


Every workflow follows the same pattern. The agent queries Outlit for an account or a cohort, computes the signals, derives a reading, and produces an output that cites the signals driving it.


Execution flow


sequence


```text
# Every workflow follows this pattern
1.   Agent runs  (scheduled, triggered, or on demand)
2.   Reads CLAUDE.md  (signal set, thresholds, workflow instructions)
3.   Queries Outlit  (single account or cohort)
4.   Computes each signal  (from the unified profile)
5.   Derives the reading  (count of degraded signals + weight)
6.   Produces output  (reading + evidence + recommended action)
7.   Surfaces to human for review and action
```


## Where the state lives


The agent does not have a hidden backend. It reads customer data from Outlit and writes its outputs to files in the project directory. Two folders carry all the state this system needs between runs:


Project layout


files


```text
project/
CLAUDE.md
readings/
2026-04-21.json          # yesterday's nightly snapshot
2026-04-22.json          # tonight's nightly snapshot
drill-downs/
2026-04-22/
canopy-analytics.md    # degradation drill-down produced tonight
trellis-health.md
```


Each file in` readings/` is one snapshot. It contains every account's current reading, signal values, degraded list, and a` degraded_since` timestamp that tracks how long the account has been at watch-or-worse:


readings/2026-04-22.json


json


```text
[
{
"account_id"  :  "canopy-analytics"  ,
"reading"  :  "at-risk"  ,
"degraded_since"  :  "2026-04-19"  ,
"score"  : 41,
"signals"  : {
"usage_trend_30d"  : -48,
"primary_contact_activity"  : 22,
"unresolved_p1_count"  : 2,
"qbr_recency"  : 145
},
"degraded"  : [ "usage_trend_30d"  ,  "primary_contact_activity"  ,  "unresolved_p1_count"  ,  "qbr_recency"  ]
}
]
```


**Degradation detection is a diff.** After writing tonight's snapshot, the agent compares it against yesterday's, account by account. Reading levels have an explicit ordering (healthy = 0, watch = 1, at-risk = 2, critical = 3), so a drop is just` curr_level > prev_level` :


Degradation check (pseudocode)


logic


```text
# Run after tonight's snapshot is written
for   account  in   readings[today]:
prev_level = level(readings[yesterday][account].reading)
curr_level = level(readings[today][account].reading)


if   curr_level > prev_level:
if    not   exists(drill-downs/<today>/<account>.md):
produce_drill_down(account, prev, curr)
```


The` exists()` check is the idempotency guard. If the nightly job retries or is run twice in a day, the agent sees the drill-down file already exists and skips. One drill-down per account per day, maximum. Delete the file if you want the agent to regenerate it.


**Recovery detection is the same diff, in reverse.** The` degraded_since` field tracks how long each account has been at watch-or-worse. If an account's reading improves, the agent clears` degraded_since` in tonight's snapshot (the account recovered; the system is agnostic to whether a CSM intervened or it self-corrected). If an account has` degraded_since` set to a date 14+ days ago and the reading has not improved, the agent produces an escalation drill-down for the CS manager. The readings themselves are the register; there is no separate intervention log to maintain.


Claude Code has native file read and write, so there is no extra infrastructure. Put the project in a git repo for an audit trail of every nightly run. If your CS team has a data warehouse or an Airtable base already, point the agent at that instead; the pattern does not change, only the store does.


## Five health score workflows


01


Nightly health scoring


Scheduled


Every night, the nightly health scoring workflow runs across your full book. For each account, the agent queries Outlit, computes every signal in the set, derives a reading, and writes the reading plus the signal detail back to a location your CS team can query. The score is always fresh and always comes with the evidence behind it. No dashboard polling, no stale numbers.


- 01


Query Outlit for every active account
- 02


Compute each signal per account using the defined thresholds
- 03


Derive a reading (healthy, watch, at-risk, critical) from signal counts and weights
- 04


Compare the new reading against last night's reading to flag movements
- 05


Write today's reading plus signal evidence to` readings/YYYY-MM-DD.json`


Example output · Nightly run, 3 readings changed


**Canopy Analytics. Watch → At-risk.** Three signals degraded tonight. Usage trend dropped to -48% (crossed the -40% threshold). Primary contact activity now 22 days (threshold: 14). Unresolved P1 count is 2 (threshold: 1). QBR was cancelled and not rescheduled (already flagged). Recommend prioritizing on the Monday triage with a direct outreach from the CSM.


**Trellis Health. Healthy → Watch.** One signal degraded: a new contact (Derek, IT Lead) has opened three infrastructure tickets in 14 days, while the primary champion (Laura) has gone quiet (12 days). No usage drop yet. Pattern suggests a potential champion transition. Recommend a low-touch check-in with Laura before the signals cascade.


**Brightline Labs. Healthy → Healthy (score up).** Plan utilization crossed 85% and core feature use is up 40% month over month. No degraded signals. Flagging as an expansion candidate for Workflow 04 tomorrow morning.


02


Degradation response


Triggered


When an account's reading drops a level (healthy to watch, watch to at-risk, at-risk to critical), the degradation response workflow produces a drill-down for the assigned CSM. The drill-down names the specific signals that degraded, the timeline of the drop, relevant context from Outlit (recent tickets, conversation tone, billing status), and a recommended action calibrated to which signal actually moved. A drop driven by a support spike is a different action than a drop driven by a champion going silent.


- 01


Trigger when an account's reading level drops
- 02


Query Outlit for the underlying signal detail and recent timeline
- 03


Identify the dominant driver (which signal moved most)
- 04


Pull any recent tickets, email threads, or internal chat context for color
- 05


Produce a drill-down with the specific signal evidence and a recommended action


Example output · Canopy Analytics, degradation drill-down


**Reading change.** Watch (score 68) last night → At-risk (score 41) tonight.


**Dominant driver.** Usage trend. Daily active users dropped from 41 to 14 over 4 weeks. The drop accelerated in the last 10 days. Their data team (6 users) stopped using the analytics module entirely last Wednesday.


**Supporting signals.** Unresolved P1 count at 2 (both related to the data export issue first reported March 8). Primary contact activity now 22 days. Last QBR was cancelled 3 weeks ago with no reschedule.


**Relevant context.** On the second P1 ticket, the customer replied "this keeps happening." The internal #cs-escalations channel discussed this on April 9 but no action was assigned.


**Recommended action.** The drop is dominated by usage, and the usage drop is concentrated on the feature flagged in the P1s. This is not a check-in call. This is a direct engineering response. Recommend CSM loops in the engineering lead on the data export fix and shares a root-cause summary with Canopy within 48 hours.


03


Recovery watch


Ongoing


Recovery watch runs as the third step of the nightly job, right after scoring and degradation detection. It uses the` degraded_since` field already in the snapshot to close the loop on accounts that were previously flagged: if an account has recovered since yesterday, the agent clears its` degraded_since` and logs the recovery; if an account has been at watch-or-worse for 14+ days without improvement, the agent produces an escalation drill-down for the CS manager. No manual intervention log is required — the readings themselves are the register.


- 01


Read today's and yesterday's snapshots after tonight's scoring
- 02


For each account whose reading level improved, clear` degraded_since` and log a recovery
- 03


For each account with` degraded_since` 14+ days ago and no improvement, produce an escalation drill-down
- 04


Skip any account that already has an escalation file for today (idempotency)
- 05


Produce a daily summary of recoveries and escalations for the CS manager


Example output · Tuesday nightly summary


**Recoveries (1).** Canopy Analytics recovered: reading improved from at-risk to watch after 9 days. Usage trend recovered from -48% to -12%. Unresolved P1 count dropped from 2 to 0.` degraded_since` cleared.


**Escalations (1).** Ridgeline Software escalated: at-risk for 17 days with no improvement. Primary driver has been` qbr_recency` the whole time (QBR cancelled March 29, not rescheduled). Secondary:` primary_contact_activity` now 24 days. Escalation drill-down written to` drill-downs/2026-04-22/ridgeline-software-escalation.md` .


**Still being watched (4).** Trellis Health (day 6, watch), Brightline Labs (day 3, watch), Meridian Analytics (day 11, at-risk), Lark Systems (day 2, watch). None exceed the 14-day threshold yet.


04


Expansion trigger


Weekly


A healthy account is not automatically an expansion candidate. An expansion candidate is healthy and showing capacity pressure. The expansion trigger workflow looks for the specific combination: reading is healthy across all signals, and plan utilization or core feature use is pushing against the boundaries of the current plan. When those signals coincide, the agent drafts a grounded expansion outreach citing the customer's actual usage pattern, ready for the CSM to review and send.


- 01


Filter accounts to those with a healthy reading and no active risk signals
- 02


Identify accounts with plan utilization over 80% or core feature use growing over 40% month over month
- 03


Exclude any account in an active renewal risk conversation
- 04


Query Outlit for usage detail to ground the outreach in something specific
- 05


Draft a personalized outreach message referencing the actual usage pattern


Example output · Brightline Labs, expansion trigger fired


**Signal.** Reading healthy (no degraded signals). Plan utilization at 94% (Growth plan, 47 of 50 seats). Core feature use up 62% month over month, driven by the engineering team adopting the API integration in February.


**Context.** Last QBR positive. VP Engineering mentioned wanting "deeper integration with our internal tools." Support sentiment positive in last 60 days. No renewal risk signals.


**Draft outreach.** "Hi Jamie, your engineering team has been getting real use out of the API integration since February, and you're now at 47 of your 50 seats with API call volume up around 60% month over month. The Scale plan formalizes the API access your team is already using and gives you room to grow past the seat cap. Worth a 20-minute call this week to walk through what the move would look like?"


05


Weekly health drift report


Friday


Every Friday, the weekly health drift report summarizes movement across the book. It covers accounts that moved between reading levels, the signals that drove the movement, and cohort-level trends (is support volume spiking across mid-market accounts, are enterprise accounts utilizing less of their plan). The report is for the CS manager and leadership, not for individual CSM action. It is the layer above the nightly score that makes patterns visible.


- 01


Aggregate the last 7 nightly readings across every account
- 02


Group accounts by movement (improved, flat, degraded)
- 03


Identify the signals most responsible for movement this week
- 04


Cut the data by cohort (segment, plan tier, tenure) to surface pattern-level trends
- 05


Produce a scannable report for the CS manager's Friday review


## How to set this up


You need an Outlit account with at least a few data sources connected, the Outlit CLI authenticated, and an agent (Claude Code, Codex, or any setup you prefer) with the Outlit skill installed. The same workflows run through any capable coding agent; this guide uses Claude Code as the concrete example.


Authenticate the CLI with` outlit auth login` ; if it asks for a key, create one in[Settings > API Keys](https://app.outlit.ai/settings/workspace/api-keys) . Remote MCP clients use your workspace MCP URL from[Settings > CLI & MCP](https://app.outlit.ai/settings/workspace/mcp) and complete OAuth in the client.


**Step 0: Install the Outlit CLI and authenticate.** Run the CLI login flow and let the CLI store the credential.


Terminal


bash


```text
curl   -fsSL https://outlit.ai/install.sh | bash
outlit   --version
outlit   auth login
```


**Step 1: Connect your data sources.** Log into Outlit, go to Integrations, and connect the sources that feed your signal set. At minimum connect your payment tool (e.g., Stripe), your product analytics (e.g., PostHog or the Outlit SDK), and your helpdesk (e.g., Pylon). Add your team chat and email as they apply. The signals in this guide assume these categories exist; if a source is missing, the relevant signal will simply report as unavailable rather than guessing.


**Step 2: Install Claude Code.** You'll need Node.js 18 or higher.


Terminal


bash


```text
npm   install -g @anthropic-ai/claude-code
# Verify install
claude   --version
```


**Step 3: Install the Outlit skill for Claude Code.** This gives the agent the workflow guidance and CLI commands it needs.


Terminal


bash


```text
outlit   setup claude-code
# Verify auth and agent setup
outlit   doctor
```


**Step 4: Define your signal set.** Before the agent can score anything, you need to decide which signals matter and what thresholds flip them from healthy to degraded. Start with the five to eight signals in the section above, calibrate the thresholds against your last two quarters of churn data, and write them into your CLAUDE.md. Revisit the signal set quarterly.


**Step 5: Create your project and add the CLAUDE.md below.** Make a new directory for your health score agent, drop in the CLAUDE.md, and you're ready to run.


**Step 6: Run the nightly scoring workflow first.** It exercises every signal against every account in one run, so you will see whether the data sources are connected and whether the readings roughly match your gut sense. Expect 2 to 4 weeks of threshold adjustments before the output is trustworthy. Calibration is not a one-time event. Have CSMs flag readings that feel wrong, retune that signal's threshold, re-run, and repeat. Most teams go through 5 to 10 rounds before the readings are reliable enough to act on without second-guessing.


**Step 7: Schedule and wire the workflows.** Use cron or your preferred scheduler to run one nightly job that chains three steps in order: nightly scoring, degradation response, and recovery watch. All three read and write the same` readings/` and` drill-downs/` folders, so one invocation handles everything that needs to happen every night. Schedule the expansion trigger weekly and the drift report on Friday as separate jobs. None of the workflows need their own webhook or event trigger — the nightly diff is the trigger.


**Start with signal quality, not score quality.** The score is a derived view. If individual signals are wrong (usage counted from the wrong event, support volume missing a source, billing status stale), the score will be wrong too. Spend the first week auditing signal values against a handful of accounts you know well. Once the signals are right, the score will follow.


## What changes when this is running


Your CS team stops debating what "red" means. Every reading comes with the specific signals that drove it, so conversations move from "this account looks bad" to "usage dropped 48% and the champion has been silent for three weeks, which action do we take." The agent does not need to reach a human consensus on a number; it just reports the evidence.


Interventions get closed out. Recovery watch catches the silent failure case where a CSM talks to a customer, the account disappears from the top of the red list, and nobody checks whether anything actually changed. A two-week watch forces the loop to close.


Expansion stops leaning on gut feel. The trigger only fires when health is clean and capacity pressure is real. CSMs stop pitching upgrades to accounts that are secretly unhappy and stop missing the ones that are quietly outgrowing their plan. Combine this with the expansion workflow in the[customer success guide](https://www.outlit.ai/blog/how-to-automate-customer-success-with-claude-code) if you want the agent to draft outreach directly from the trigger.


Leadership sees pattern-level movement. The Friday report surfaces trends that individual account reviews miss. If support volume is spiking across a specific segment or a plan tier is showing utilization drops, it shows up in the weekly view before it shows up in the pipeline.


## Get started with the CLAUDE.md


Copy the CLAUDE.md below into your project to get started. It contains the agent's role, the signal set, instructions for querying Outlit, and all five workflow definitions. Below it are individual prompt templates you can run as-is, with variables to swap out per account.


CLAUDE.md


```text
# Customer Health Score Agent


## Role
You are a customer health score agent. Your job is to compute account-level health signals from Outlit, derive readings, respond to degradations, watch recoveries, trigger expansion outreach, and produce weekly drift reports. You do not contact customers directly. Every output is internal. A CSM or CS manager decides what to do with your recommendations.


## How to use Outlit
Before every response, use the installed Outlit skill and CLI to fetch current context for the relevant account or cohort. Do not rely on prior conversation context or general knowledge about a company. Fetch current data on every request.


Outlit returns unified account profiles including billing, product usage, support history, email threads, team chat, and timeline activity. If a specific data source is missing for an account, report the affected signal as unavailable. Do not guess. Do not fill gaps with assumptions.


## Signal set
Compute every signal from the unified Outlit profile. Each signal has a source, a window, and a threshold that flips it to degraded.


### Usage signals
- usage_trend_30d: daily active users last 30 days vs prior 30. Degraded at -40% or worse.
- core_feature_use: count of key feature events in last 14 days. Degraded when below the per-plan floor.


### Relationship signals
- primary_contact_activity: days since the account's most active user logged in (infer as the user with the most product sessions in the last 90 days). If your team maintains an explicit champion field per account, use that instead. Degraded at 14+ days.
- qbr_recency: days since last QBR. Degraded at 120+ days or when a QBR was cancelled and not rescheduled.


### Support signals
- ticket_volume_14d: count of tickets in last 14 days. Degraded when volume is 2x the account's trailing 60-day average.
- unresolved_p1_count: count of open P1 tickets. Degraded at 1 or more.


### Commercial signals
- plan_utilization: usage as a percentage of plan limits. Degraded below 20% (under-use risk) or above 95% (throttling risk).
- billing_status: current, past due, or in dunning. Degraded when past due or in dunning.


## Reading derivation
Count the degraded signals per account. Map to reading levels:
- 0 degraded: healthy
- 1 degraded: watch
- 2 to 3 degraded: at-risk
- 4 or more degraded: critical


Override: any unresolved_p1_count degraded with a billing_status degraded is automatically critical regardless of count.


Always return the reading alongside the full signal detail. Never return just the reading.


## Output principles
- Be specific. Every claim should cite a signal value and a source window.
- Be brief. A CSM reading a drill-down should get the situation and the recommended action in under 60 seconds.
- Distinguish dominant drivers from supporting signals. The action depends on which signal moved most.
- If data is incomplete, name what is missing. Do not invent.
- Never contact a customer in any form.


## Workflow instructions


### Nightly health scoring (scheduled)
1. Query Outlit for every active account
2. Compute every signal per account using the thresholds above
3. Derive a reading from the signal counts and the critical override rule
4. Carry forward `degraded_since` from yesterday's snapshot. Set it to today's date for any account that dropped out of healthy tonight; clear it for any account that improved to healthy.
5. Write the full snapshot (reading, signals, degraded, degraded_since) to `readings/YYYY-MM-DD.json`


### Degradation response (triggered by the nightly diff)
1. After the nightly snapshot is written, diff today against yesterday's snapshot using level ordering (healthy=0, watch=1, at-risk=2, critical=3)
2. For each account whose level increased, skip if `drill-downs/YYYY-MM-DD/.md` already exists (idempotency)
3. Otherwise, query Outlit for the last 30 days of timeline and identify the dominant driver (the signal with the largest adverse movement)
4. Pull any recent tickets, email threads, or internal chat context for color
5. Write the drill-down to `drill-downs/YYYY-MM-DD/.md` with signal evidence, dominant driver, and a recommended action calibrated to the driver


### Recovery watch (runs after nightly scoring)
1. For each account whose reading level improved versus yesterday's snapshot, log the recovery in tonight's summary. The `degraded_since` clear is handled by the scoring step above.
2. For each account with `degraded_since` set to a date 14+ days ago and no improvement tonight, write an escalation drill-down to `drill-downs/YYYY-MM-DD/-escalation.md`
3. Skip escalations that already have a file for today (idempotency)
4. Produce a daily summary: recoveries, escalations, and accounts still being watched
5. Do not maintain a separate interventions log. The readings/ snapshots are the register.


### Expansion trigger (weekly)
1. Filter the latest nightly readings to accounts with 0 degraded signals
2. Identify accounts with plan_utilization above 80% or core_feature_use growth above 40% month over month
3. Exclude any account in an active renewal risk conversation (confirm in Outlit timeline)
4. Query Outlit for the specific usage pattern driving the capacity pressure
5. Draft a short outreach message grounded in the actual usage pattern. No generic upsell copy.


### Weekly health drift report (Friday)
1. Aggregate the last 7 nightly readings across every account
2. Group accounts by movement: improved, flat, degraded
3. Identify the signals most responsible for movement this week
4. Cut the data by segment, plan tier, and tenure to surface cohort-level trends
5. Produce a scannable report for the CS manager. Headers and bullets only. No wall of text.


## What you should never do
- Contact a customer in any form
- Return a reading without the signal detail behind it
- Invent a signal value when the source is unavailable
- Override the signal set or thresholds without explicit instruction
- Mark an account as recovered without confirming every degraded signal has recovered
```


### Prompt templates


Variables in gold


are yours to swap out. Everything else runs as written.


Workflow 01 · Nightly health scoring


Scheduled


```text
Run the nightly health scoring across every active account.


Query Outlit for each account. Compute every signal in the signal set. Derive the reading using the thresholds and the critical override rule from CLAUDE.md.


Compare each account's reading against last night's stored reading. For any account whose reading level changed, write a one-paragraph summary naming the signals that moved.


Write the reading, the signal detail, and the diff to  readings/YYYY-MM-DD.json   in the project directory.


Do not action anything without my confirmation.
```


Workflow 02 · Degradation response


Triggered


```text
[Account name]  's reading dropped from  [prior level]   to  [new level]   on last night's run.


Query Outlit for the underlying signal detail and the last 30 days of timeline activity. Identify the dominant driver and any supporting signals.


Pull any recent tickets, email threads, or internal chat context relevant to the driver.


Produce a drill-down covering: reading change, dominant driver, supporting signals, relevant context, and a recommended action calibrated to which signal actually moved.


Keep it under 250 words. Produce it for  [CSM name]   to review.


Do not action anything without my confirmation.
```


Workflow 03 · Recovery watch


Ongoing


```text
Run recovery watch as the follow-up to tonight's scoring.


Open  readings/<today>.json   and  readings/<yesterday>.json  .


For each account whose reading improved versus yesterday, log the recovery. For each account with degraded_since set to a date 14+ days ago and no improvement tonight, write an escalation drill-down to  drill-downs/<today>/<account>-escalation.md  . Skip any escalation that already has a file for today.


Produce a daily summary for  [CS manager name]  : recoveries, escalations, and accounts still being watched (with days at current level).


Do not action anything without my confirmation.
```


Workflow 04 · Expansion trigger


Weekly


```text
Run the weekly expansion trigger against the latest nightly readings.


Filter to accounts with 0 degraded signals. From that set, identify accounts with plan_utilization above 80% or core_feature_use growth above 40% month over month.


Exclude any account currently in a renewal risk conversation (confirm by scanning the Outlit timeline for the last 30 days).


For each remaining candidate, query Outlit for the specific usage pattern driving the capacity pressure. Draft a short outreach message grounded in that pattern. No generic copy. Reference the actual signals.


Produce the candidate list with draft messages for  [CSM name]   to review.


Do not action anything without my confirmation.
```


Workflow 05 · Weekly health drift report


Friday


```text
Compile the weekly health drift report for the week ending  [date]  .


Aggregate the last 7 nightly readings across every account. Group accounts by movement: improved, flat, degraded.


Identify the signals most responsible for movement this week. Cut the data by segment, plan tier, and tenure to surface cohort-level trends.


Write a scannable report for  [CS manager name]   with these sections:
- Headline movements (3 to 5 accounts worth calling out)
- Signal-level drivers this week
- Cohort trends
- Accounts entering recovery watch this week


Headers and bullets only. No wall of text.


Do not action anything without my confirmation.
```
