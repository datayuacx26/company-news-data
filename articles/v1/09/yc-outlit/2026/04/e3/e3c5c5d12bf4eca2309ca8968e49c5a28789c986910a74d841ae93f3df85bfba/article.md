---
schema_version: "1.0.0"
document_id: "e3c5c5d12bf4eca2309ca8968e49c5a28789c986910a74d841ae93f3df85bfba"
company_key: "yc-outlit"
company: "Outlit"
source_id: "yc-outlit-news-import-30f1306359d9"
canonical_url: "https://www.outlit.ai/blog/how-to-run-your-renewal-process-with-outlit"
published_at: "2026-04-17T00:00:00+00:00"
first_seen_at: "2026-07-23T19:28:50.452707+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:0839758a9699e6ade4df5b552ad7a707126b23c7c3a00cf6d16f54b3b778c901"
---

# Running Your Renewal Process with an AI Agent

You can run your entire SaaS renewal process through an AI agent by connecting your billing, product usage, and support data to[Outlit](https://outlit.ai/) , then using[Claude Code](https://docs.anthropic.com/en/docs/claude-code) to query that context before every renewal touchpoint. This guide covers five workflows you can set up today: renewal risk scoring, pre-renewal briefs, expansion opportunity flagging, at-risk escalation, and post-renewal health checks. Each workflow includes a ready-to-use prompt template and a complete CLAUDE.md you can drop into a project.


## Renewals break down because the context is scattered


The CSM running your renewals is supposed to know everything about the account: whether they're using the product, whether support has been clean, whether billing is healthy, whether they've grown into a bigger plan, and whether anything went wrong in the last quarter. That context exists. It's in Stripe (billing and subscription data), PostHog or SDK events (feature adoption and usage trends), Pylon (support history), Slack (internal account discussions), and Fireflies or Granola (call records from the last QBR).


Nobody pulls all of it before a renewal conversation. The CSM checks Stripe for the renewal date, maybe skims the last support ticket, and goes in with whatever they remember. If the account has been healthy, that's usually fine. If the account has been quietly disengaging for three months, the CSM finds out during the renewal call, not before it.


The problem gets worse at scale. A CS team managing 200 accounts can't do deep renewal prep for every one. Prep happens for the enterprise accounts and the rest get a templated email. The accounts that churn are usually the ones nobody looked at closely enough.


An agent connected to Outlit changes the economics of renewal prep. Outlit's[Customer Context Graph](https://www.outlit.ai/products/customer-context-graph) unifies billing, usage, support, and conversation data into a single account profile. That[customer context infrastructure](https://www.outlit.ai/blog/what-is-customer-context-infrastructure) lets the agent query the profile 90 days before renewal and every week after, score risk, flag expansion signals, and produce a brief before every renewal conversation. The same depth of prep that used to be reserved for top accounts now happens for every account on the book.


Renewal prep today


- CSM checks Stripe for the renewal date and current plan
- Skims the last support ticket, tries to remember the QBR
- Writes a quick agenda based on what they can recall
- Sends a templated renewal email for smaller accounts
- ~45 min per account. Inconsistent. Only done for top accounts.


Renewal prep with an agent


- Agent queries Outlit 90 days out, scores risk across every data source
- Produces a prep brief before every renewal conversation
- Flags expansion opportunities based on actual usage growth
- Escalates at-risk accounts before the renewal window closes
- ~3 min to review. Every account gets the same depth.


By the end of this guide, you'll have a CLAUDE.md and five prompt templates you can copy into a Claude Code project and run today.


## What does a renewal agent do?


The agent is not replacing your CS team's judgment. It is pulling together context that already exists across your tools and presenting it in a form that makes renewal conversations more informed. Outlit connects to Stripe for billing and subscription data, PostHog or SDK events for product usage, Pylon for support history, and Slack for internal account context. Where call records are connected (Fireflies or Granola), the agent can also reference what was discussed in the last QBR or check-in.


When the agent queries Outlit for an account approaching renewal, it receives a unified profile: billing status, usage trends, support ticket history, and conversation context. It reasons over that profile to produce the specific output each workflow requires, whether that's a risk score, a prep brief, or an escalation alert.


**How Outlit works.** Connect your data sources once (Stripe, PostHog, Pylon, Slack, and optionally Fireflies or Granola). Outlit resolves every record to an account-level profile and keeps it current. Coding agents query that profile through the Outlit CLI and installed skill; remote MCP clients use a workspace MCP URL with OAuth.


## How the renewal agent runs


Every workflow follows the same pattern. The agent reads its instructions from the CLAUDE.md, queries Outlit for the relevant accounts, reasons over the unified profile, and produces a structured output for the CS team to review.


Execution flow


sequence


```text
# Every workflow follows this pattern
1.   Agent runs  (manual, scheduled via cron, or triggered by a wrapper)
2.   Reads CLAUDE.md  (role, scope, workflow instructions)
3.   Queries Outlit  (account ID, renewal window, or full book)
4.   Receives unified profile  (billing, usage, support, conversations)
5.   Reasons over context  (what's relevant to this renewal?)
6.   Produces output  (risk score, brief, escalation alert)
7.   Surfaces to human for review and action
```


## Five renewal workflows


01


Renewal risk scoring


Weekly


Every week, the renewal risk scoring workflow queries Outlit for all accounts with renewals in the next 90 days. For each account, it pulls billing status from Stripe, usage trends from PostHog or SDK events, open support tickets from Pylon, and any internal Slack threads. It scores each account as high, medium, or low risk and produces a prioritized list for the CS team. The output is not a generic health score. It cites specific signals: usage dropped 40% in the last 30 days, two unresolved P1 tickets, billing failed last month.


- 01


Query Outlit for all accounts renewing in the next 90 days
- 02


Pull billing, usage, support, and Slack context per account
- 03


Score each account as high, medium, or low risk with cited signals
- 04


Flag accounts with missing data sources as "needs manual review"
- 05


Produce prioritized list for team review, sorted by risk level


Example output · Weekly renewal risk report, 12 accounts in window


**High risk.** Meridian Analytics (renews May 14). Daily active users dropped from 34 to 11 over the past 6 weeks. Two P1 support tickets opened in March, both marked resolved, but the customer replied "this keeps happening" on the second one. Last Slack mention was your CSM asking if anyone knew why their API usage flatlined. Nobody replied.


**Medium risk.** Lark Systems (renews May 28). Usage is flat but their champion (Dana, VP Product) hasn't logged in for 22 days. Billing is clean. No open tickets, but their last support interaction was a feature request for SSO that was closed as "not planned." Worth checking whether SSO is still a blocker.


**Low risk.** Brightline Labs (renews June 3). Usage grew 18% quarter over quarter. Zero open tickets. Recent support history is clean and positive. They upgraded their plan in January. Only flag: their contract includes a 15% first-year discount that expires at renewal.


02


Pre-renewal brief


On demand


Before any renewal conversation, the pre-renewal brief workflow gives the CSM a single-page summary covering where the account stands across every dimension. Billing: current plan, payment history, any failed charges. Usage: trend over the last 90 days, feature adoption, who the key users are. Support: open and recently resolved tickets, recent support history. Relationship: last call notes, any concerns raised, stakeholder changes. The brief is written for someone walking into a conversation, not for a dashboard.


- 01


Query Outlit for the account: billing, usage, support, Slack, call recordings
- 02


Produce account snapshot: plan, tenure, billing health
- 03


Summarize usage trend, feature adoption, and key user activity
- 04


Surface open tickets, recent support history, and relationship context
- 05


Identify what to watch for in the renewal conversation


Example output · Lark Systems, renewal call tomorrow


**Account snapshot.** On Growth plan ($2,400/mo) since January 2025. Contract renews May 28. No failed payments. 16 months as a customer.


**Usage.** 23 active users, down from 28 in January. Core feature adoption is strong but they haven't touched the analytics module since onboarding. Their top user (Jamie, Ops Lead) logs in daily. Their champion (Dana, VP Product) last logged in 22 days ago.


**Support.** One ticket in the last 90 days: SSO feature request, closed as "not planned." Recent support sentiment is trending negative. Dana's tone on the SSO ticket was notably frustrated.


**Relationship.** Last call was 6 weeks ago (QBR). Dana said "we're happy with the core product but my team keeps asking about SSO." Your CSM said it was "on the roadmap." Based on the call recording, Dana's tone shifted after that exchange. No follow-up call was scheduled.


**Watch for.** Dana's declining login frequency plus the SSO closure could combine into a real risk at renewal. If she raises SSO again and the answer hasn't changed, the conversation may turn. Worth having a concrete alternative or timeline before the call.


03


Expansion opportunity at renewal


Triggered


When an account enters the 90-day renewal window, the expansion opportunity workflow checks for upsell signals: usage growth exceeding current plan limits, new teams or users adopting the product, feature usage that suggests they would benefit from a higher tier. The output is a brief for the CSM or AE that quantifies the opportunity and provides a talking point grounded in actual usage data, not a generic upsell prompt.


- 01


Run this when an account enters the 90-day renewal window
- 02


Query Outlit for usage data, current plan, and feature adoption
- 03


Identify signals: usage growth, new team adoption, higher-tier feature usage
- 04


Produce expansion brief with signal, usage detail, opportunity sizing, and talking point
- 05


Produce the brief for the CSM and AE to review


Example output · Brightline Labs, expansion signal at renewal


**Signal.** Brightline is on the Growth plan (50 user seats). They currently have 47 active users, up from 31 at last renewal. At current growth rate, they will hit the seat cap within 6 weeks of renewal.


**Usage detail.** Their engineering team (12 users) started using the API integration in February. API call volume has grown 3x since then. This is a Scale plan feature they are accessing through a legacy allowance not included in their current contract.


**Opportunity.** Upgrade to Scale plan ($4,200/mo, up from $2,400/mo). The seat cap increase and API access formalize what they are already using. Their 15% first-year discount expires at renewal, so the pricing conversation is happening regardless.


**Talking point.** "Your engineering team has been getting a lot of value from the API integration. The Scale plan makes that access permanent and gives your team room to grow without hitting seat limits."


04


At-risk renewal escalation


Triggered


Run the at-risk renewal escalation workflow when an account within 60 days of renewal shows specific danger signals, instead of waiting for the weekly risk report. Danger signals include: usage drop of more than 30% in the last 30 days, multiple unresolved support tickets, champion going inactive for more than 14 days, billing failures, or an explicit cancellation mention in a support ticket or call recording. The escalation alert is produced for the CS manager with enough context to act the same day.


- 01


Run this when an account within 60 days of renewal crosses a danger threshold
- 02


Query Outlit for full account context across all sources
- 03


Identify trigger signal and supporting signals from other sources
- 04


Pull call context if available (last QBR, recent check-in)
- 05


Produce escalation alert for the CS manager to review, not just the CSM


Example output · ESCALATION: Meridian Analytics, renews in 47 days


**Trigger.** Usage dropped 62% in the last 30 days (34 daily active users to 11). This crosses the 30% threshold for automatic escalation.


**Supporting signals.** Two P1 tickets opened in March about data sync failures. Both marked "resolved" by support, but the customer's last reply on ticket #4218 said "this keeps happening, we're evaluating other options." That reply was 8 days ago and has no internal follow-up.


**Call context.** Last recorded call was a QBR on February 12. Their VP Eng (Marcus) asked about self-hosted deployment. Your CSM said it was not available. Marcus said "that might be a problem for us going forward." No follow-up call was scheduled.


**Recommended action.** This account needs a call this week, not at renewal. The combination of usage collapse, unresolved support issues, and an explicit "evaluating other options" signal means the renewal conversation may already be lost if you wait. Suggest a direct outreach from the CS manager to Marcus, not the CSM who handled the last QBR.


05


Post-renewal health check


30 days after


Thirty days after an account renews, the post-renewal health check compares usage from the 30 days before renewal to the 30 days after. It checks whether any commitments made during the renewal conversation have been followed up on. It flags accounts where usage dipped post-renewal, which can indicate a team that renewed reluctantly and may not renew again next cycle.


- 01


Run this 30 days after an account renews
- 02


Query Outlit for usage comparison: 30 days pre vs 30 days post
- 03


Check whether commitments from the renewal call were followed through
- 04


Flag accounts where usage dropped more than 15% post-renewal
- 05


Produce the health check for review; escalate to CS manager if usage decline is significant


Example output · Lark Systems, 30-day post-renewal check


**Renewal outcome.** Renewed May 28 on Growth plan. No plan change. 12-month term.


**Usage comparison.** Pre-renewal 30 days: 23 avg daily active users. Post-renewal 30 days: 19 avg daily active users. Down 17%. The drop is concentrated in their product team (Dana's group), which went from 8 active users to 3.


**Commitments check.** During the renewal call, your CSM committed to "a dedicated session on the analytics module with Dana's team within two weeks." Based on email and Slack, no session was scheduled. No follow-up was sent.


**Flag.** This account renewed but the underlying risk has not resolved. Dana's team is disengaging. The commitment made to close the renewal was not followed through. If this pattern holds through Q3, next year's renewal will be harder. Recommend scheduling the analytics session this week and re-engaging Dana directly.


## How to set up a renewal agent with Outlit


You need Outlit with your billing and usage sources connected, the Outlit CLI authenticated, and Claude Code with the Outlit skill installed. Authenticate the CLI with` outlit auth login` ; if it asks for a key, create one in[Settings > API Keys](https://app.outlit.ai/settings/workspace/api-keys) . Remote MCP clients use your workspace MCP URL from[Settings > CLI & MCP](https://app.outlit.ai/settings/workspace/mcp) and complete OAuth in the client.


**Step 0: Install the Outlit CLI and authenticate.** Run the CLI login flow and let the CLI store the credential.


Terminal


bash


```text
curl   -fsSL https://outlit.ai/install.sh | bash
outlit   --version
outlit   auth login
```


**Step 1: Connect your data sources in Outlit.** Log into Outlit, go to Integrations, and connect Stripe (required for renewal dates and billing data). Then add PostHog or SDK events for product usage, and Pylon for support history. If your team records calls, connect Fireflies or Granola for QBR and check-in context. Each source you add makes the renewal workflows more accurate.


**Step 2: Install Claude Code.** You'll need Node.js 18 or higher.


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


**Step 4: Create your project and add the CLAUDE.md below.** Make a new directory for your renewal agent, drop in the CLAUDE.md, and you're ready to run.


**Step 5: Run the renewal risk scoring first.** Pick the weekly risk scoring workflow as your starting point. It covers all accounts in the renewal window at once, so you'll see immediately whether the data sources are connected correctly and the output quality matches your expectations.


**Step 6: Schedule recurring workflows.** Use cron or your preferred scheduler to run the risk scoring weekly. The pre-renewal brief runs on demand before each conversation. The escalation and post-renewal workflows can be triggered by a wrapper script that monitors account status changes in Outlit.


**Start with risk scoring.** It is the fastest to validate because it covers multiple accounts in a single run. If the output cites specific signals from Stripe, PostHog, and Pylon, your setup is working. Roll out the remaining workflows from there.


## What changes when this is running


Every account approaching renewal gets the same depth of analysis regardless of size. The risk scoring catches accounts that are quietly disengaging weeks before the renewal conversation, not during it. Your CS team stops being surprised by churn they could have seen coming.


Pre-renewal briefs become a habit because the output is immediately useful and saves more time than it takes to read. The CSM walks into the conversation knowing what was said on the last call, what the support history looks like, and what the usage trend has been. The customer notices the difference.


Expansion opportunities stop being left on the table at renewal. The agent identifies accounts that have grown into a bigger plan before the CSM has to guess. Renewal conversations that could have been flat become upgrade conversations, backed by data the customer already generated.


At-risk escalation means the CS manager hears about problems before the renewal window closes, not after. The account that went dark three weeks before renewal gets attention from a senior person with enough context to act. The escalation is specific enough to be useful: not "this account is at risk" but "their VP Eng asked about self-hosted, was told no, and usage dropped 62% since."


Post-renewal health checks catch the accounts that renewed reluctantly. A team that renewed but immediately disengaged is telling you something. The agent catches commitments that were made during the renewal conversation and never followed through, giving the CSM a concrete action before the next quarter.


## Get started with the CLAUDE.md


Copy the CLAUDE.md below into your project to get started. It contains the agent's role, instructions for querying Outlit, and all five workflow definitions. Below it are individual prompt templates you can run as-is, with variables to swap out per account.


CLAUDE.md


```text
# Renewal Process Agent


## Role
You are a renewal operations agent. Your job is to monitor accounts approaching renewal, assess risk, identify expansion opportunities, and produce briefs and alerts that help CS teams run informed renewal conversations. You do not contact customers directly. All outputs are internal. The CSM or CS manager decides what to share externally.


## How to use Outlit
Before every response, use the installed Outlit skill and CLI to fetch context for the relevant account or account list. Do not rely on prior conversation context or general knowledge about a company. Fetch current data on every request.


Outlit returns unified account profiles including: billing data (Stripe), product usage (PostHog or SDK events), support history (Pylon), internal context (Slack), and call records (Fireflies or Granola) where connected.


If a specific data source is missing or not connected for an account, say so explicitly. Do not invent context or assume health based on incomplete data.


## Output principles
- Be specific. Reference actual usage numbers, actual ticket IDs, actual billing states. Generic summaries waste time.
- Be brief. A risk score should fit in a Slack message. A renewal brief should fit on one page.
- Distinguish signals from noise. A single missed login is not a signal. A champion's login frequency dropping from daily to zero over three weeks is.
- Cite your sources. Every claim should reference whether it came from Stripe, PostHog, Pylon, Slack, or a call record.
- Never contact a customer or prospect directly in any form.


## Workflow instructions


### Renewal risk scoring (weekly)
1. Query Outlit for all accounts with renewals in the next 90 days
2. For each account, pull: billing status and payment history (Stripe), usage trend for the last 90 days (PostHog or SDK events), open and recently resolved support tickets (Pylon), any internal Slack context
3. Score each account as high, medium, or low risk
4. For high and medium risk, cite the specific signals driving the score
5. Produce a prioritized list sorted by risk level for team review
6. Flag any account where data sources are missing as "unable to score, needs manual review"


### Pre-renewal brief (on demand)
1. Query Outlit for the account: billing, usage, support, Slack, call recordings
2. Produce a brief with five sections: account snapshot (plan, tenure, billing health), usage (trend, feature adoption, key users), support (open tickets, recent support history), relationship (last call notes, stakeholder changes, concerns raised), watch for (what could come up in the renewal conversation)
3. Keep it under 300 words. Produce the brief for the CSM to review.


### Expansion opportunity at renewal (triggered)
1. Run this when an account enters the 90-day renewal window
2. Query Outlit for usage data, current plan limits, and feature adoption
3. Identify signals: usage growth exceeding plan limits, new team adoption, feature usage suggesting a higher tier
4. Produce an expansion brief with: signal summary, usage detail, opportunity sizing, one suggested talking point grounded in their actual usage
5. Produce the brief for the CSM and AE to review


### At-risk renewal escalation (triggered)
1. Run this when an account within 60 days of renewal shows: usage drop >30% in 30 days, multiple unresolved support tickets, champion inactive >14 days, billing failure, or explicit cancellation mention in support or call recording
2. Query Outlit for full account context
3. Produce an escalation alert with: trigger signal, supporting signals from other sources, call context if available, recommended action
4. Produce the escalation alert for the CS manager to review, not just the CSM. This bypasses the normal weekly report.


### Post-renewal health check (30 days after)
1. Run this 30 days after an account renews
2. Query Outlit for usage comparison: 30 days pre-renewal vs 30 days post-renewal
3. Check whether any commitments made during the renewal conversation (from call recordings or Slack) have been followed up on
4. Produce a health check with: renewal outcome, usage comparison, commitments check, flag if anything needs attention
5. Produce the health check for review. If usage dropped >15% post-renewal, escalate to the CS manager.


## What you should never do
- Contact a customer or prospect directly in any form
- Score an account as low risk when data sources are missing or incomplete
- Share escalation alerts with the CSM before the manager has reviewed them
- Skip querying Outlit and rely on your own knowledge of a company
- Invent usage numbers, ticket counts, or billing details
```


### Prompt templates


Variables in gold


are yours to swap out. Everything else runs as written.


Workflow 01 · Renewal risk scoring


Weekly


```text
Run the weekly renewal risk scoring.


Query Outlit for all accounts renewing in the next  [90 / 60 / 30]   days.


For each account, pull billing status, usage trend, open support tickets, and any Slack context. Score each as high, medium, or low risk. For high and medium, cite the specific signals.


Output a prioritized list sorted by risk level. Produce the report for  [CS manager name]   to review.


Do not action anything without my confirmation.
```


Workflow 02 · Pre-renewal brief


On demand


```text
Prepare a pre-renewal brief for  [account name]  .


Renewal date is  [date]  . CSM on the account:  [CSM name]  .


Query Outlit for everything we have: billing, usage, support, Slack, call recordings. Produce a brief covering: account snapshot, usage trend, support history, relationship context, and what to watch for in the renewal conversation.


Produce the brief for  [CSM name]   to review. Keep it under 300 words.


Do not action anything without my confirmation.
```


Workflow 03 · Expansion opportunity


Triggered


```text
[Account name]   is entering their renewal window (renews  [date]  ).


Query Outlit for their usage data, current plan, and feature adoption. Check whether usage has grown beyond their current plan limits or if new teams are adopting the product.


If there is an expansion opportunity, produce a brief with: the signal, usage detail, opportunity sizing, and one talking point for the renewal conversation.


Produce the brief for  [CSM name]   and  [AE name]   to review if applicable.


Do not action anything without my confirmation.
```


Workflow 04 · At-risk escalation


Triggered


```text
Run an at-risk check on  [account name]  . They renew in  [X]   days.


Query Outlit for: usage trend (last 30 days vs prior 30 days), open support tickets, champion activity, billing status, and any mentions of cancellation or alternatives in call recordings or support tickets.


If any escalation signals are present (usage drop >30%, unresolved tickets, champion inactive, cancellation mentions), produce an escalation alert with the trigger, supporting signals, and recommended action.


Produce the alert for  [CS manager name]   to review. Do not share with the CSM until the manager reviews.


Do not action anything without my confirmation.
```


Workflow 05 · Post-renewal health check


30 days after


```text
[Account name]   renewed  [X]   days ago on  [date]  .


Query Outlit for usage comparison: 30 days before renewal vs 30 days after. Check whether any commitments made during the renewal conversation have been followed up on.


Produce a health check with: renewal outcome, usage comparison, commitments status, and a flag if anything needs attention.


Produce the health check for  [CSM name]   to review. If usage dropped more than 15%, escalate to  [CS manager name]  .


Do not action anything without my confirmation.
```
