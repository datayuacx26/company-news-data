---
schema_version: "1.0.0"
document_id: "82347bb857753c0179d437f11544572bd1adab7d657fbe619aec5662bdfceb33"
company_key: "yc-outlit"
company: "Outlit"
source_id: "yc-outlit-news-import-30f1306359d9"
canonical_url: "https://www.outlit.ai/blog/how-to-automate-onboarding-with-outlit"
published_at: "2026-04-20T00:00:00+00:00"
first_seen_at: "2026-07-23T19:28:50.452707+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:2b409a90db17c8afe9428bb4688c7abed644528e575b708b18593acd67b75640"
---

# How to Automate Onboarding with an AI Agent

You can onboard new customers faster by connecting your signup, billing, product usage, and support data to[Outlit](https://outlit.ai/) , then using[Claude Code](https://docs.anthropic.com/en/docs/claude-code) to query that unified context before every onboarding touchpoint. This guide covers five workflows you can set up today: new signup brief, activation stall detection, first-week check-in prep, onboarding milestone tracking, and sales-to-CS handoff. Each workflow includes a ready-to-use prompt template and a complete CLAUDE.md you can drop into a project.


## Onboarding breaks down because the context is scattered


A new customer signs up on Tuesday. Stripe logs the subscription. Your database provisions the user. Product events start flowing through PostHog or your SDK. If sales was involved, the discovery call sits in your call recording tool and the email thread lives in another inbox entirely. A support ticket lands in your helpdesk within the first week. Internal conversation about the account happens in Slack.


The CSM assigned to the account is supposed to pull that together into a coherent picture before the first check-in. In practice, they check the signup email, maybe skim the last sales call if they were CC'd, and walk in knowing the customer's name and plan. Everything the customer said during the sales process, every action they took in the product, every early friction point they hit: the CSM only learns about it if they happen to look in the right tool at the right time.


This gets worse as the CS team grows. The first few accounts get deep onboarding attention because someone has time to dig. As volume scales, onboarding becomes templated. New customers get a welcome email, a canned check-in invite, and generic enablement content. The accounts that stall in the first 30 days are usually the ones nobody looked at closely enough to notice the stall happening.


An agent connected to Outlit changes the economics of onboarding. Outlit's[Customer Context Graph](https://www.outlit.ai/products/customer-context-graph) unifies signup, billing, usage, support, and conversation data into a single account profile. That[customer context infrastructure](https://www.outlit.ai/blog/what-is-customer-context-infrastructure) lets the agent query the profile the moment a customer signs up and every day after, then produce briefs, detect activation stalls, prep check-ins, track milestones, and carry sales context into CS without anyone assembling it by hand.


Onboarding today


- CSM reads the signup email and guesses at context
- Skims a sales call if they remember it exists
- Sends a templated welcome and a canned check-in
- Finds out about stalls at the 30-day review
- ~20 min per account. Inconsistent coverage.


Onboarding with an agent


- Signup brief produced the moment the account is created
- Stalls flagged daily with the specific missing milestone
- Check-ins prepped with actual product and support history
- Sales context carried into CS without a handoff meeting
- ~3 min to review. Every account gets the same depth.


By the end of this guide, you'll have a CLAUDE.md and five prompt templates you can copy into a Claude Code project and run today.


## What the onboarding agent actually does


The agent is not replacing your CS team's judgment. It is pulling together context that already exists across your tools and presenting it in a form that makes onboarding conversations more informed. Outlit connects your billing, signup, product usage, support, and conversation data into a single account profile, so every new customer shows up with the full context behind them from the moment they sign up.


When the agent queries Outlit for a new or onboarding account, it receives a unified profile: who signed up, what plan they chose, what they said on the sales call, what they've done in the product since signup, and what they've asked support about. It reasons over that profile to produce the specific output each workflow requires, whether that's a welcome brief, a stall alert, or a check-in prep doc. The same profile carries forward into[ongoing customer success workflows](https://www.outlit.ai/blog/how-to-automate-customer-success-with-claude-code) and[customer health scoring](https://www.outlit.ai/blog/tracking-customer-health-scores-with-outlit) once onboarding is complete.


**How Outlit works.** Connect your data sources once (billing, signup, product analytics, support, team chat, email, and call recordings). Outlit resolves every record to an account-level profile and keeps it current. Coding agents query that profile through the Outlit CLI and installed skill; remote MCP clients use a workspace MCP URL with OAuth.


## How the onboarding agent runs


Every workflow follows the same pattern. The agent reads its instructions from the CLAUDE.md, queries Outlit for the relevant accounts, reasons over the unified profile, and produces a structured output for the CS team to review.


Execution flow


sequence


```text
# Every workflow follows this pattern
1.   Agent runs  (manual, scheduled via cron, or triggered by a wrapper)
2.   Reads CLAUDE.md  (role, scope, workflow instructions)
3.   Queries Outlit  (account ID, signup window, or activation cohort)
4.   Receives unified profile  (signup, billing, usage, support, conversations)
5.   Reasons over context  (what's relevant to this stage of onboarding?)
6.   Produces output  (signup brief, stall alert, check-in prep, handoff doc)
7.   Surfaces to human for review and action
```


## Five onboarding workflows


01


New signup brief


Triggered


The moment a new account is created in your signup or billing system, the new signup brief workflow queries Outlit for everything available about that account and produces a one-page brief for the assigned CSM. The brief covers who signed up, what plan they chose, anything the agent can learn from the email domain or referral source, and any prior sales context if the account went through a sales cycle. The output is the first thing the CSM sees before they send a welcome message.


- 01


Trigger on account creation in your signup or billing system
- 02


Query Outlit for the new account profile and any linked pre-signup context
- 03


Pull sales call records if the account came through sales
- 04


Pull email threads for sales or marketing touchpoints linked to the account
- 05


Produce the signup brief for the assigned CSM


Example output · Harbor Analytics, new signup 4 hours ago


**Account.** Harbor Analytics signed up on the Growth plan ($2,400/mo). 5-user seat plan. Signup source tagged as "outbound" in signup metadata. Billing active, no trial period.


**Who signed up.** Priya Venkat (priya@harboranalytics.com). Based on the domain, Harbor Analytics is a mid-market analytics consulting firm. Priya is listed as Head of Data on her LinkedIn per the enrichment field on signup.


**Sales context.** Two discovery calls recorded in Fireflies (Feb 14 and Feb 28) with your AE. Priya raised three use cases: client reporting automation, internal dashboarding for partners, and replacing an aging internal tool. The primary driver on the second call was "we've been duct-taping this for two years and it's costing us billable hours."


**Email thread.** A 14-email thread with your AE covering pricing, security review, and a security questionnaire completed March 3. Priya confirmed on March 5 she had internal sign-off.


**What to say in the welcome.** Reference the billable-hours framing from the second discovery call, acknowledge the security review they completed, and offer to prioritize client reporting as the first use case since it was the most concrete. Avoid generic "let us know if you have questions" language for an account that went through this much sales cycle.


02


Activation stall detection


Daily


Every day, the activation stall detection workflow queries Outlit for all accounts in their first 14 days that have not hit your defined activation milestones. Milestones vary by product (first data imported, first team member invited, first integration connected, first report generated) but the pattern is the same: the agent compares each new account against the activation checklist and flags the specific milestone each stalled account has not reached. The CSM gets a daily list of accounts that need a nudge before the stall becomes disengagement.


- 01


Query Outlit for all accounts signed up in the last 14 days
- 02


For each account, check activation milestones against product usage events
- 03


Flag accounts missing one or more milestones, noting the specific gap
- 04


Prioritize by how far behind the expected activation timeline they are
- 05


Produce a daily stall list for the CS team


Example output · Daily activation stall report, 4 accounts flagged


**Harbor Analytics.** Day 6 post-signup. Priya logged in twice but has not invited any team members and has not imported any data. Her AE flagged client reporting as the primary use case during sales, which requires data import to be meaningful. Recommend a direct email from the CSM referencing the reporting use case and offering a 20-minute setup call.


**Meridian Tech.** Day 11 post-signup. Account logged in once on day 1, not since. No milestones hit. No support tickets. Email domain suggests a 40-person engineering org but only the signup user has access. Recommend checking whether the signup user is the right champion or whether someone else on the team needs access.


**Lark Systems.** Day 9 post-signup. Invited 3 team members, imported sample data, but has not created any production-relevant content yet. May be a typical exploration pattern but worth a low-touch nudge to offer a template or prebuilt example for their industry.


03


First-week check-in prep


On demand


Before any first-week check-in call, the first-week check-in prep workflow gives the CSM a single-page brief covering what the customer has done in the product, what they have not, what they have asked support about, and any signals from their team's activity. The brief is written for someone walking into a conversation, not for a dashboard. The goal is for the CSM to reference specific things the customer has done, not ask generic "how's it going" questions.


- 01


Query Outlit for the account: signup context, billing, usage, support, internal chat
- 02


Summarize product activity since signup (feature use, team invites, data imported)
- 03


Surface any support tickets and their resolution status
- 04


Note any internal chat mentions of the account
- 05


Identify what to reference and what to watch for on the call


Example output · Harbor Analytics, first-week check-in tomorrow


**Account snapshot.** On Growth plan since March 8. Primary contact Priya Venkat. 3 of 5 seats used. Billing healthy.


**Product activity.** Priya and two team members (Jordan and Kai) have logged in. They imported a client dataset on day 2 and generated 4 reports on day 4. They have not yet connected the partner dashboard workflow, which was the second use case from discovery.


**Support.** One ticket opened day 3 in Pylon: "Can we schedule reports to send to external emails?" Resolved same day with a documentation link. Priya replied "got it, thanks" and no further tickets since.


**Internal context.** Your AE posted in the #new-accounts Slack channel on day 1 noting Priya "is going to be a case study customer if we treat her well." No other internal mentions.


**Reference on the call.** The 4 reports they generated (ask which ones felt useful). The scheduled reports question (their actual need). Partner dashboard as the next milestone (unblocks their second use case from discovery).


**Watch for.** They are on track but have not yet touched the partner dashboard workflow. If Priya does not mention it, ask directly. The sales conversation made it clear this was priority two, so silence on it is worth noticing.


04


Onboarding milestone tracking


Weekly


Every week, the onboarding milestone tracking workflow produces a cohort view of all customers in their first 30 days. Accounts are grouped by milestone completion so the CS team can see at a glance which customers are on track, which are partially activated, and which are stalling. The output is designed for a CS standup, not a dashboard. It calls out specific accounts that need attention this week.


- 01


Query Outlit for all customers within 30 days of signup
- 02


Bucket each account by milestones completed (0, 1-2, 3-4, all)
- 03


Cross-reference support activity and recent usage trends
- 04


Identify accounts that moved between buckets this week (progress or regression)
- 05


Produce a weekly cohort report with specific action items per bucket


05


Sales-to-CS handoff brief


Triggered


When a new account signs up, the sales-to-CS handoff brief workflow checks whether the account has prior sales activity in Outlit (recorded calls, sales email threads, internal Slack discussion before signup). If it does, the workflow pulls that context together and packages it into a brief for the assigned CSM. The CSM inherits the full sales context without needing to chase down the AE.


- 01


Trigger on account creation for accounts with prior sales activity
- 02


Query Outlit for all linked conversations, emails, and internal discussion
- 03


Extract stated use cases, stakeholders mentioned, and timeline commitments from sales calls
- 04


Note any commitments the sales team made that CS needs to honor
- 05


Produce the handoff brief for the assigned CSM


Example output · Harbor Analytics, sales-to-CS handoff


**Deal summary.** Harbor Analytics closed on the Growth plan ($2,400/mo, annual). 3-week sales cycle. Primary AE: Marcus. Primary contact: Priya Venkat (Head of Data).


**Stated use cases.** Three use cases raised across two discovery calls: (1) client reporting automation, stated as the primary driver, (2) internal partner dashboarding as a secondary use case, (3) replacing an internal tool they described as "duct-taped for two years."


**Stakeholders.** Priya is the buyer and primary user. She mentioned Jordan (analyst) and Kai (data engineer) as daily users. Her manager Elena is the budget approver but not an expected user.


**Commitments from sales.** Marcus committed to "a dedicated onboarding session within the first two weeks" on the second call. He also told Priya the scheduled reports feature was on the roadmap for Q2. On security, Marcus said the completed questionnaire was sufficient and did not promise any custom security work.


**Watch for.** Priya is being positioned internally as a potential case study customer. She has expectations from the sales conversation around priority one being client reporting, and she expects the promised onboarding session within the first two weeks.


## How to set this up


You need Outlit with your signup and usage sources connected, the Outlit CLI authenticated, and Claude Code with the Outlit skill installed. Authenticate the CLI with` outlit auth login` ; if it asks for a key, create one in[Settings > API Keys](https://app.outlit.ai/settings/workspace/api-keys) . Remote MCP clients use your workspace MCP URL from[Settings > CLI & MCP](https://app.outlit.ai/settings/workspace/mcp) and complete OAuth in the client.


**Step 0: Install the Outlit CLI and authenticate.** Run the CLI login flow and let the CLI store the credential.


Terminal


bash


```text
curl   -fsSL https://outlit.ai/install.sh | bash
outlit   --version
outlit   auth login
```


**Step 1: Connect your data sources in Outlit.** Log into Outlit, go to Integrations, and connect the sources you use for signup and activation. At minimum, connect your payment tool (e.g., Stripe), your signup or auth layer (e.g., Clerk or Supabase), and your product analytics (e.g., PostHog or SDK events). Add your helpdesk, email, and team chat as they apply to your stack. If your team records sales calls in Fireflies or Granola, connect that too for discovery call context. Each source you add makes the onboarding workflows richer.


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


**Step 4: Create your project and add the CLAUDE.md below.** Make a new directory for your onboarding agent, drop in the CLAUDE.md, and you're ready to run.


**Step 5: Run the activation stall detection first.** Pick the daily stall detection workflow as your starting point. It covers every account in the first 14 days at once, so you'll see immediately whether the signup and usage data sources are connected correctly and the output quality matches your expectations.


**Step 6: Schedule recurring workflows.** Use cron or your preferred scheduler to run stall detection daily and milestone tracking weekly. The signup brief and handoff brief can be triggered by a wrapper script that listens for account creation events. The first-week check-in prep runs on demand before each call.


**Start with stall detection.** It is the fastest to validate because it runs across every new account at once. If the output names specific missing milestones and cites activity from your connected tools, your setup is working. Roll out the remaining workflows from there.


## What changes when this is running


Every new customer gets a signup brief within minutes of signing up, not days. The CSM walks into the welcome message already knowing what the customer said during sales, what plan they picked, and what use case they cared about. The generic "let us know if you have questions" welcome becomes something specific and grounded in the customer's actual situation.


Activation stalls get caught in days instead of weeks. The CS team stops being surprised at the 30-day review by customers who never activated. The daily stall list makes it obvious which accounts need a nudge this week and what specifically is missing, so the nudge can be targeted instead of generic.


First-week check-ins stop being awkward. The CSM references the 4 reports the customer generated, the support question they asked, the feature they have not yet tried. The customer notices that the CSM actually knows their account. Check-ins that used to feel like interrogations become working sessions.


Cohort tracking gives the CS manager a weekly view of onboarding health without anyone assembling it by hand. The manager sees which accounts moved forward this week, which regressed, and which need direct intervention. Staffing decisions, playbook changes, and coaching conversations become data-driven instead of anecdotal.


Sales-to-CS handoff stops being a dropped ball. Every piece of context the sales team generated lands in front of the CSM the moment the account shows up in your signup system. Commitments made during sales get honored because CS knows about them. Customers stop having to repeat themselves about their use cases, stakeholders, and timelines.


## Get started with the CLAUDE.md


Copy the CLAUDE.md below into your project to get started. It contains the agent's role, instructions for querying Outlit, and all five workflow definitions. Below it are individual prompt templates you can run as-is, with variables to swap out per account.


CLAUDE.md


```text
# Customer Onboarding Agent


## Role
You are a customer onboarding operations agent. Your job is to monitor new accounts from signup through their first 30 days, detect activation stalls, prep first-week check-ins, track onboarding milestones, and hand off sales context to CS. You do not contact customers directly. All outputs are internal. The CSM or CS manager decides what to share externally.


## How to use Outlit
Before every response, use the installed Outlit skill and CLI to fetch context for the relevant account or account list. Do not rely on prior conversation context or general knowledge about a company. Fetch current data on every request.


Outlit returns unified account profiles including billing, signup, product usage, support history, internal chat, email threads, and sales call records (e.g., Fireflies or Granola) where connected. If a specific data source is missing or not connected for an account, say so explicitly. Do not invent context or assume activation progress based on incomplete data.


## Output principles
- Be specific. Reference actual activation events, actual ticket IDs, actual usage counts. Generic summaries waste time.
- Be brief. A signup brief should fit on one page. A stall alert should fit in a short message.
- Distinguish signals from noise. One missed login on day 2 is not a stall. Zero product activity for 7 days after signup is.
- Cite your sources. Every claim should reference which data source it came from (billing, product usage, support, internal chat, email, call records).
- Never contact a customer or prospect directly in any form.


## Workflow instructions


### New signup brief (triggered)
1. Trigger on account creation in your signup or billing system
2. Query Outlit for the new account profile and any linked pre-signup context
3. Pull sales call records if the account came through sales
4. Pull email threads for any sales or marketing touchpoints linked to the account
5. Produce a one-page brief for the assigned CSM covering account details, stakeholders, stated use cases, sales commitments, and specific recommendations for the welcome message


### Activation stall detection (daily)
1. Query Outlit for all accounts within 14 days of signup
2. For each account, compare product usage events against your activation milestones (first data import, first team invite, first integration connected, first core feature use)
3. Flag accounts missing one or more milestones with the specific gap named
4. Prioritize by how far behind the expected activation timeline they are
5. Produce a daily stall list with specific recommended actions per account


### First-week check-in prep (on demand)
1. Query Outlit for the account: signup context, billing, usage, support, internal chat, email
2. Summarize product activity since signup with specific counts and feature use
3. Surface any support tickets and their resolution
4. Note any internal chat mentions of the account
5. Identify what to reference on the call and what to watch for
6. Keep the brief under 300 words. Produce it for the CSM to review.


### Onboarding milestone tracking (weekly)
1. Query Outlit for all customers within 30 days of signup
2. Bucket each account by activation milestones completed
3. Cross-reference support activity and recent usage trends
4. Identify accounts that moved between buckets this week
5. Produce a weekly cohort report with specific action items per bucket for the CS standup


### Sales-to-CS handoff brief (triggered)
1. Trigger on account creation for accounts with prior sales activity (calls or emails)
2. Query Outlit for all linked conversations, emails, and internal discussion
3. Extract stated use cases, stakeholders mentioned, and timeline commitments from sales calls
4. Note any commitments the sales team made that CS needs to honor
5. Produce the handoff brief for the assigned CSM


## What you should never do
- Contact a customer or prospect directly in any form
- Mark an account as activated when usage data is missing or incomplete
- Assume sales context based on the email domain without verifying against email or call records
- Skip querying Outlit and rely on your own knowledge of a company
- Invent activation events, support ticket counts, or user names
```


### Prompt templates


Variables in gold


are yours to swap out. Everything else runs as written.


Workflow 01 · New signup brief


Triggered


```text
Produce a new signup brief for  [account name]  .


Query Outlit for everything we have: account details, plan, signup context, any linked sales call records, email threads, and internal chat mentions.


Produce a one-page brief covering: account snapshot (plan, source, contact), stakeholders (who signed up, who else is mentioned), sales context (stated use cases, commitments made), and specific recommendations for the welcome message.


Produce the brief for  [CSM name]   to review. Keep it under 300 words.


Do not action anything without my confirmation.
```


Workflow 02 · Activation stall detection


Daily


```text
Run the daily activation stall detection.


Query Outlit for all accounts signed up in the last  [14 / 7]   days.


For each account, check these activation milestones:  [milestone 1]  ,  [milestone 2]  ,  [milestone 3]  ,  [milestone 4]  .


Flag any account missing one or more milestones with the specific gap named. Prioritize by how far behind the expected timeline they are.


Produce a daily stall list for  [CS team name]   with specific recommended actions per account.


Do not action anything without my confirmation.
```


Workflow 03 · First-week check-in prep


On demand


```text
Prepare a first-week check-in brief for  [account name]  .


Check-in call is scheduled for  [date]  . CSM on the account:  [CSM name]  .


Query Outlit for the full account picture: signup context, plan, product activity since signup, support tickets, internal chat mentions, and any relevant email threads.


Produce a brief covering: account snapshot, product activity, support history, internal context, what to reference on the call, and what to watch for.


Produce the brief for  [CSM name]   to review. Keep it under 300 words.


Do not action anything without my confirmation.
```


Workflow 04 · Onboarding milestone tracking


Weekly


```text
Run the weekly onboarding milestone tracking report.


Query Outlit for all customers within 30 days of signup. Bucket each account by milestones completed: 0, 1-2, 3-4, all completed.


Identify accounts that moved between buckets this week and any that regressed.


Produce a cohort report for  [CS manager name]  's standup with specific action items per bucket.


Do not action anything without my confirmation.
```


Workflow 05 · Sales-to-CS handoff


Triggered


```text
Produce a sales-to-CS handoff brief for  [account name]  , a new signup with prior sales activity.


AE who ran the cycle:  [AE name]  . Incoming CSM:  [CSM name]  .


Query Outlit for all sales-side context: discovery call records (e.g., Fireflies), email threads with the sales team, internal Slack discussion, and the plan details from signup.


Produce a handoff brief covering: deal summary, stated use cases, stakeholders identified, commitments made during sales, and what the CSM should watch for in the first 30 days.


Produce the brief for  [CSM name]   to review.


Do not action anything without my confirmation.
```
