---
schema_version: "1.0.0"
document_id: "aa379428fde076c374c78e64e17c7a46cde0924fb6f94e734aff826e23655722"
company_key: "yc-outlit"
company: "Outlit"
source_id: "yc-outlit-news-import-30f1306359d9"
canonical_url: "https://www.outlit.ai/blog/how-to-automate-customer-success-with-claude-code"
published_at: "2026-04-12T00:00:00+00:00"
first_seen_at: "2026-07-23T19:28:50.452707+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:ded943d2e402ebccddea2096dbe388a6204a67b792932fc24eb9acbccdf55cb1"
---

# How to Automate Customer Success with Claude Code

You can automate customer success by connecting your CS data sources to a context layer like[Outlit](https://outlit.ai/) , then running AI agents through[Claude Code](https://docs.anthropic.com/en/docs/claude-code) that query customer context before taking action. This guide covers five workflows you can set up today: churn risk triage, expansion identification, QBR prep, escalation response, and weekly CS reporting. Each workflow includes a ready-to-use prompt template and a complete CLAUDE.md you can drop into a project.


## The CS platform is the wrong layer to fix


Your CS team doesn't have a tooling problem. They have a context problem.


Before a CSM can decide what to do with an account, they have to assemble a picture of that account. Last customer email in Gmail. Billing status in Stripe. Usage drop in PostHog. Open support issue in Pylon. Internal notes in Slack. That assembly work, pulling context from six places and synthesizing it, is most of what a CS job actually is. The decision itself takes thirty seconds. The context work takes an hour.


Every CS platform sold over the last decade has tried to solve this by centralizing the data into a dashboard. That helps, but it doesn't remove the work. A human still has to log in, navigate, read, and synthesize. The platform made it faster to gather context. It didn't eliminate the need to gather it.


This guide takes a different approach. Instead of making context easier for humans to gather, you make[customer context infrastructure](https://www.outlit.ai/blog/what-is-customer-context-infrastructure) available to agents before they act. The agent does the synthesis. The human gets the output and makes the call.


Monday morning, today


- Open Pylon, filter by last active date, scan for quiet accounts
- Cross-reference your sales tool to check health scores
- Pull usage data from your product analytics dashboard
- Check support tickets per account
- Write up a triage doc and share it in Slack
- ~90 minutes before the first decision gets made


Monday morning, with this setup


- Open Slack
- The triage is already there, compiled and ranked
- Each flagged account has a context summary and a recommended action
- The CSM reviews, picks up the phone, or delegates
- ~5 minutes from Slack open to first action taken


By the end of this guide, you'll have a CLAUDE.md and five prompt templates you can copy into a Claude Code project and run today.


## What Outlit does


[Outlit](https://outlit.ai/) connects to your existing stack (Stripe, PostHog, Pylon, Slack, Gmail, and product events from the SDK) and resolves those records through the[Customer Context Graph](https://www.outlit.ai/products/customer-context-graph) . One customer, one profile, regardless of how fragmented your data sources are. When an agent needs context, it queries Outlit instead of calling five APIs and assembling the picture itself.


**How Outlit works.** Connect your data sources once (support, billing, product analytics, email, and Slack). Outlit resolves every record to an account-level profile and keeps it current. Coding agents query that profile through the Outlit CLI and installed skill; remote MCP clients use a workspace MCP URL with OAuth.


## How it works with Claude Code


The CLAUDE.md file tells Claude Code what to do: when to query Outlit, what to produce, and what to surface to a human before taking action. Write it once, and the agent follows it every time it runs.


The pattern is always the same: query context first, then act.


Execution flow


sequence


```text
# Every workflow follows this pattern
1.   Claude Code runs  (manual, scheduled, or triggered by webhook)
2.   Reads CLAUDE.md  (role, scope, workflow instructions)
3.   Queries Outlit through the CLI and skill  (customer ID or account list)
4.   Receives unified customer profile  (interactions, signals, history)
5.   Reasons over context  (risk? expansion? action needed?)
6.   Produces output  (Slack message, draft email, recommended action)
7.   Surfaces to human if decision required
```


## Five CS workflows you can run through Claude Code today


Each of these workflows follows the same pattern: Claude Code queries Outlit for context, reasons over it, and surfaces an output. No dashboard. No manual triage. A human only gets involved at the decision point.


01


Churn risk triage


Monday morning


Schedule Claude Code to run every Monday using a cron job. It pulls your full account list from Outlit, flags anything that looks like a churn signal (usage drop, no activity in 14+ days, support volume spike, missed QBR), and sends a Slack notification via Outlit with a prioritized list and recommended actions.


- 01


Query Outlit for accounts active in last 90 days
- 02


Score each account against churn signal criteria
- 03


Rank by risk level, flag top 5 for immediate action
- 04


Draft recommended next action per flagged account
- 05


Send a Slack notification via Outlit with context summary per account


Example output · Monday triage, 3 accounts flagged


**High risk: Canopy Analytics.** Daily active users dropped from 41 to 14 over the last 4 weeks. Two P1 tickets opened in March about data export failures. Both resolved, but the customer replied "we're looking at alternatives" on the second one. Last CSM call was 5 weeks ago. No QBR scheduled.


**Medium risk: Trellis Health.** Usage is stable but their champion (Laura, VP Ops) hasn't logged in for 19 days. She was the primary driver of their expansion last quarter. Meanwhile, a new user (Derek, IT Lead) has opened three tickets about SSO configuration in the past two weeks. Possible internal ownership change.


**Medium risk: Ridgeline Software.** No support tickets, usage is flat. The flag is what's missing: they were scheduled for a QBR 3 weeks ago and it was cancelled by the customer. No reschedule. Their last Gmail thread said "we'll circle back after our board meeting." That was 18 days ago.


02


Expansion identification


Weekly


Claude Code queries Outlit for accounts showing expansion signals: usage at 80%+ of plan limits, team growth, new use cases appearing in interaction history, positive sentiment in recent conversations. It surfaces the top candidates with a draft outreach message per account, ready to send or modify.


- 01


Query Outlit for usage and interaction signals
- 02


Identify accounts hitting plan thresholds or showing growth
- 03


Cross-reference against recent CSM conversation tone
- 04


Draft personalized expansion outreach per account
- 05


Produce the output for team review with draft copy attached


Example output · Brightline Labs, expansion signal


**Signal.** Brightline is on the Growth plan (50 seats). They have 47 active users, up from 31 six months ago. Their engineering team (12 users) started using the API integration in February without requesting it. API call volume tripled since then. They are using a Scale plan feature on a Growth plan contract.


**Context.** Last QBR was positive. Their VP Eng mentioned wanting "deeper integration with our internal tools." No renewal risk signals. Recent support interactions have been positive.


**Draft outreach.** "Hi Jamie, I noticed your engineering team has been getting a lot of value from the API integration over the past few months. I'd love to walk through the Scale plan, it formalizes the API access your team is already using and gives you room to grow past the 50-seat cap. Worth a quick call this week?"


03


QBR prep briefing


On demand


Before any QBR, a CSM prompts Claude Code with the account name. It queries Outlit for the full account history, including interactions since last QBR, usage trends, open issues, milestones hit, and any risk signals, then produces a structured briefing document. The CSM walks in already knowing the story. The QBR prep that used to take two hours takes five minutes.


- 01


Query Outlit for account history since last QBR
- 02


Summarize usage trends, milestones, and open issues
- 03


Flag any risks or topics that need direct address
- 04


Produce structured briefing: wins, risks, agenda suggestions
- 05


Produce the briefing and surface for review


Example output · Canopy Analytics, QBR next Tuesday


**Wins.** Successfully migrated to v3 of the API in January. Their data team (6 users) adopted the analytics module in February and usage has been consistent. Support ticket volume dropped 40% since last QBR.


**Risks.** Two data export failures in March triggered P1 tickets. Both were resolved but the customer's reply on the second ticket said "this keeps happening." Their CSM acknowledged the issue in Slack but no root cause was shared with the customer. This will come up.


**Open items from last QBR.** Your CSM committed to "monthly check-in calls." One was held in January. None since. The customer hasn't raised this, but the gap is visible in the interaction timeline.


**Suggested agenda.** Lead with the data export issue and share the root cause fix. Address the check-in cadence gap before they do. The wins are real but the trust signal from the P1 response is fragile.


04


Escalation response


Triggered


When a high-priority ticket comes in, a webhook handler invokes Claude Code. It queries Outlit for the full account context: who this customer is, their history, similar past issues, and what was promised in the last interaction. It drafts a response grounded in real account data and surfaces it for review before anything gets sent.


- 01


Trigger on ticket creation above priority threshold
- 02


Query Outlit for account context and interaction history
- 03


Draft response grounded in actual account history
- 04


Surface draft + context summary for review
- 05


Human approves, edits, or overrides before anything sends


Example output · Trellis Health, P1 ticket: API rate limiting


**Account context.** Trellis Health, Growth plan, customer since March 2025. Primary contact is Laura (VP Ops). They process high-volume API calls for patient scheduling. Last interaction 3 days ago was positive (a billing question resolved quickly).


**This ticket.** Their engineering lead (Derek) reports API requests are being rate-limited during peak hours (9-11am EST). He says scheduling data is arriving late for clinical staff. This is the third infrastructure-related ticket from Derek in two weeks. The prior two were about SSO configuration.


**Relevant history.** Laura mentioned in the last QBR that "reliability during morning hours is non-negotiable for us." Your CSM noted internally that their API usage has been growing faster than their plan tier supports. No plan upgrade was discussed.


**Draft response.** "Hi Derek, I can see your API volume during peak hours is exceeding the rate limits on your current plan. I've flagged this with our infrastructure team for an immediate review. In the meantime, I want to make sure this isn't impacting patient scheduling. Can we get on a call today to walk through a short-term fix and discuss the plan options that match your current volume? I know morning reliability is critical for your team."


05


Weekly CS standup report


Friday


Schedule a Friday afternoon run. Claude Code queries Outlit across your entire account base and compiles the week in CS: accounts that moved positively, accounts that deteriorated, support volume, key wins, and risks going into the following week. It produces the report for team review.


- 01


Query Outlit for all account activity in the past 7 days
- 02


Identify accounts with meaningful movement in either direction
- 03


Summarize support volume, recent support history, and open escalations
- 04


Call out top wins and top risks for the following week
- 05


Produce formatted report every Friday at 4pm for team review


Example output · Week of April 7, CS standup


**Accounts to watch.** Canopy Analytics: usage down 66% over 4 weeks, two unresolved P1s, cancelled QBR not rescheduled. Trellis Health: three infrastructure tickets in two weeks from a new contact (Derek, IT Lead), possible champion transition from Laura. Ridgeline Software: went dark after cancelling QBR 3 weeks ago, said they would "circle back after board meeting," no follow-up.


**Wins.** Brightline Labs upgraded to Scale plan on Wednesday. Meridian Analytics completed onboarding and hit their first usage milestone (10k API calls). Lark Systems renewed for 12 months with no negotiation.


**Support summary.** 23 tickets this week (down from 31 last week). Two P1s, both related to Canopy's data export issue. Average first response time: 2.4 hours. Overall support sentiment this week was positive.


**Open escalations.** Canopy's data export root cause is identified but not communicated to customer. Trellis rate limiting ticket from Thursday is awaiting engineering review. Both need action Monday.


## How to set this up


You need an Outlit account with at least one integration active, the Outlit CLI authenticated, and Claude Code with the Outlit skill installed. Authenticate the CLI with` outlit auth login` ; if it asks for a key, create one in[Settings > API Keys](https://app.outlit.ai/settings/workspace/api-keys) . Remote MCP clients use your workspace MCP URL from[Settings > CLI & MCP](https://app.outlit.ai/settings/workspace/mcp) and complete OAuth in the client.


**Step 0: Install the Outlit CLI and authenticate.** Run the CLI login flow and let the CLI store the credential.


Terminal


bash


```text
curl   -fsSL https://outlit.ai/install.sh | bash
outlit   --version
outlit   auth login
```


**Step 1: Connect your first data source.** Log into Outlit, go to Integrations, and connect the source that best matches your first workflow. For CS workflows, start with Pylon for support, Stripe for billing, PostHog for product usage, or Slack for internal customer context. One integration is enough to run the first workflow.


**Step 2: Install Claude Code.** If you don't have it yet, run this in your terminal. You'll need Node.js 18 or higher.


Terminal


bash


```text
npm   install -g @anthropic-ai/claude-code
# Verify install
claude   --version
```


**Step 3: Install the Outlit skill for Claude Code.** This gives Claude Code the Outlit workflow guidance and CLI commands it needs.


Terminal


bash


```text
outlit   setup claude-code
# Verify auth and agent setup
outlit   doctor
```


**Step 4: Create your project and write a CLAUDE.md.** Create a new directory for your CS agent project. The CLAUDE.md is the file that tells Claude Code what it's supposed to do. Part 2 of this guide gives you a complete one you can use as a starting point.


**Step 5: Connect Slack.** If you want agents to send Slack notifications, enable Outlit's Slack integration in the console. This lets agents send notifications to your organization's configured notification channel when triage results, QBR briefings, and weekly reports are ready for review.


**Step 6: Run your first workflow.** Start with the churn risk triage. It's the clearest signal of whether context is flowing correctly. If the output is specific and grounded in real account data, the setup is working. If it's generic, check integration sync status and` outlit doctor` .


**Step 7: Schedule recurring workflows.** Use a cron job to automate the Monday triage and Friday standup report. For ticket-triggered workflows like escalation response, set up a webhook handler that invokes Claude Code when a high-priority ticket is created in your support tool.


**What to expect on first run.** The first output will be imperfect. The CLAUDE.md will need tuning: the output format, the signal criteria, the way it handles edge cases. That's normal. Treat the first week as calibration. Most teams get to a workflow they trust within three to five runs.


### What to watch out for


The more connectors you have active in Outlit, the richer the agent's context. Start with one connector and add more as you see gaps in the output.


Every workflow in this guide ends with a human in the loop. An agent can draft an escalation response, but a human should send it. An agent can flag a churn risk, but a human should make the call. Be explicit in your CLAUDE.md about where the agent stops and where the human picks up.


You'll know it's working when your CS team stops spending time gathering context and starts spending time on the decision itself. If QBR prep drops from two hours to five minutes, it's working.


### Get started with the CLAUDE.md


Copy the CLAUDE.md below into your project to get started. It contains the agent's role, instructions for querying Outlit, and all five workflow definitions. Below it are individual prompt templates you can run as-is, with variables to swap out per account.


CLAUDE.md


```text
# CS Operations Agent


## Role
You are a CS operations agent for this company. Your job is to surface customer context, identify risks and opportunities, and produce outputs that help CSMs take action faster. You do not make consequential decisions autonomously. You produce recommendations, drafts, and summaries. Humans approve before anything gets sent or logged.


## How to use Outlit
Before responding to any customer-related request, use the installed Outlit skill and CLI to fetch current customer context. Do not reason from memory or general knowledge about a customer. Always fetch the current context first.


When querying, pass the customer ID or company name. Outlit will return the unified profile: interaction history, usage signals, account health, open tickets, and CSM notes.


If Outlit returns incomplete data, note what's missing in your output. Do not fabricate context to fill gaps.


## Output principles
- Be specific. Every recommendation should reference something real from the customer's context.
- Be brief. A CSM reading your output in Slack should understand the situation and the recommended action in under 60 seconds.
- Be clear about confidence. If the signal is weak, say so.
- Never send, post, or log anything without explicit human approval.


## What you should never do autonomously
- Send an email or message to a customer
- Update a customer record without human confirmation
- Escalate a ticket on behalf of a CSM
- Mark an account as churned or at-risk in any system of record
- Share customer data outside of the designated Slack channels


## Workflow instructions


### Churn risk triage (Monday)
1. Query Outlit for all accounts active in the last 90 days
2. For each account, evaluate: last interaction date, usage trend (last 30 days vs prior 30), support ticket volume, QBR recency
3. Flag any account where two or more signals are negative
4. Rank flagged accounts by severity
5. For each flagged account: write a 2-sentence context summary and a single recommended next action
6. Send a Slack notification via Outlit. Format: account name, risk level, context summary, recommended action.


### Expansion identification (Weekly)
1. Query Outlit for accounts showing: usage at 80%+ of plan limits, team member growth, new use cases in interaction history, positive support sentiment in last 60 days
2. Cross-reference: is the account in an active renewal or renewal risk conversation? If yes, exclude from expansion outreach.
3. For each candidate: draft a short, specific outreach message grounded in their actual usage context. No generic upsell copy.
4. Produce the output for team review. Format: account name, signal summary, draft outreach message.


### QBR prep briefing (On demand)
1. Query Outlit for all account activity since the last QBR date
2. Summarize: usage trends, milestones hit, open issues, support volume, any commitments made in previous interactions
3. Flag: any topic that needs direct address in the QBR, any risk that the CSM should be prepared to discuss
4. Produce: structured briefing with sections for Wins, Risks, Open items, and Suggested agenda points
5. Produce the briefing and surface for review. Keep it under one page.


### Escalation response (Triggered)
1. When triggered by a high-priority ticket: query Outlit immediately for the customer's full context
2. Note: account tier, relationship history, tone of recent interactions, any prior escalations and how they were resolved
3. Draft a response that acknowledges the specific issue, references any relevant history, and outlines next steps
4. Do not generalize. The draft should read like it was written by someone who knows this customer well.
5. Surface the draft + a 3-bullet context summary for review. Human sends or modifies.


### Weekly CS standup report (Friday)
1. Query Outlit for all account activity in the past 7 days
2. Identify accounts that moved positively (usage up, positive interaction, issue resolved) and negatively (usage drop, negative interaction, new risk signal)
3. Compile: support volume summary, recent support history, open escalations, notable wins
4. Write top 3 risks going into the following week with one-sentence context each
5. Produce the report every Friday at 4pm for team review. Keep it scannable. No wall of text. Bullets and headers only.
```


### Prompt templates


Variables in gold


are yours to swap out. Everything else runs as written.


Workflow 01 · Churn risk triage


Monday


```text
Run the weekly churn risk triage.


Query Outlit for all accounts that have been active in the last 90 days.
Evaluate each account against the criteria in CLAUDE.md. Flag any account
where two or more signals are negative.


Send a Slack notification via Outlit with the results. For each flagged
account, include: account name, risk level (high / medium), a 2-sentence
context summary, and one recommended next action.


Today is  [date]  . Do not action anything without my confirmation.
```


Workflow 02 · Expansion identification


Weekly


```text
Run the expansion identification workflow.


Query Outlit for accounts showing expansion signals as defined in
CLAUDE.md. Exclude any account currently in renewal risk conversations.


For each candidate, draft a short outreach message grounded in their
actual usage context. Not generic copy. Reference something specific
from their Outlit profile.


Produce the output for my review. Include: account name, signal summary,
draft message. Flag me before anything gets sent.
```


Workflow 03 · QBR prep briefing


On demand


```text
Prepare a QBR briefing for  [account name]  .


Their last QBR was on  [date]  . Query Outlit for all activity since then.


Produce a structured briefing with four sections:
- Wins since last QBR
- Open risks or issues to address
- Open items or commitments from last time
- Suggested agenda points


Keep it under one page. I want to be able to read it in five minutes
and walk into the QBR fully prepared.
```


Workflow 04 · Escalation response


Triggered


```text
A high-priority ticket just came in from  [account name]  .


Ticket summary:  [paste ticket text here]


Query Outlit for their full context before drafting anything. Look at:
interaction history, prior escalations and resolution, account tier,
and tone of recent conversations.


Draft a response that reads like it was written by someone who knows
this customer. Reference their history where relevant. Don't be generic.


Surface the draft for my review. I'll review and send.
```


Workflow 05 · Weekly CS standup report


Friday


```text
Compile the weekly CS standup report for the week ending  [date]  .


Query Outlit for all account activity in the past 7 days. Identify
accounts that moved positively and negatively. Summarize support
volume and recent support history.


Write a report with these sections:
- Accounts to watch (top 3 risks going into next week)
- Wins this week
- Support summary
- Open escalations


Produce the report for my review. Keep it scannable. Bullets and headers only.
No wall of text.
```
