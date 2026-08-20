---
schema_version: "1.0.0"
document_id: "9d96ad729089e740984275a618f26238a06fc2c37df91a28afa4338361891866"
company_key: "yc-outlit"
company: "Outlit"
source_id: "yc-outlit-news-import-30f1306359d9"
canonical_url: "https://www.outlit.ai/blog/building-a-deal-intelligence-agent-with-outlit"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-23T19:28:50.452707+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:a443aa8eea129ff8e4128d461b324ffc51c9c5cd43722f634ae2ebb4a0c3f9c8"
---

# Building a Deal Intelligence Agent with Outlit

You can build a deal intelligence agent by connecting your call recordings and conversation data to[Outlit](https://outlit.ai/) , then running an AI agent (Claude Code, Codex, Gemini CLI, OpenCode, or a remote MCP client such as Cursor) to query that context on demand. This guide covers four workflows you can set up today: pre-call briefs, closed-lost postmortems, deal handoffs to CS, and weekly rep coaching. Each workflow includes a ready-to-use prompt template and a complete agent instruction file you can drop into a project.


## The deal story exists, it just lives in the wrong places


Every deal your team works generates a trail. Call recordings in Fireflies or Granola. Emails back and forth. Slack threads with internal context. The information is there. Getting to it before a conversation is the problem.


A rep preparing for a call with a prospect they haven't spoken to in three weeks has to piece that trail together themselves. Find the last call recording. Skim the email thread. Check if anything was flagged in Slack. By the time they're up to speed, they've spent twenty minutes on prep that should take two.


For closed-lost deals the problem is worse. The recordings exist. The story of what happened is in there. But nobody has time to go back and listen. Patterns that could improve how the whole team sells never get surfaced because the raw material is buried in a folder nobody opens.


An agent connected to Outlit changes this. Outlit's[Customer Context Graph](https://www.outlit.ai/products/customer-context-graph) indexes your call recordings, email, and Slack into a unified profile per account. That[customer context infrastructure](https://www.outlit.ai/blog/what-is-customer-context-infrastructure) gives the agent one place to query before it produces a brief, reconstructs a closed-lost deal, or packages the full story for CS before the handoff meeting.


Deal prep today


- Rep searches for the last call recording, scrubs through it
- Checks email thread for any commitments or open questions
- Asks a colleague if they remember anything relevant from Slack
- Writes rough notes, hopes nothing important was missed
- 15-25 min per call. Incomplete. Stressful.


Deal prep with an agent


- Rep runs the pre-call prompt with the account name
- Agent queries Outlit, surfaces call highlights, email summary, open items
- Brief arrives in Slack in under a minute
- Rep reads it, adds one talking point, joins the call
- 2-3 min. Complete. Consistent every time.


By the end of this guide, you'll have a CLAUDE.md and four prompt templates you can copy into a Claude Code project and run today.


## What does a deal intelligence agent do?


The agent is not doing anything you couldn't do manually. It is querying Outlit for a unified view of everything recorded about an account, reasoning over that context, and producing a structured output. The value is not magic. It is speed and consistency.


Outlit connects to your call recording tool and indexes recordings per account. This guide uses Fireflies and Granola as the supported call examples. Outlit also ingests email, Slack, and billing data from Stripe where connected, so the account profile the agent queries reflects the full picture of a customer relationship, not just the call history.


When the agent queries Outlit before a call, it is asking: what do we know about this account across every source we have connected? Outlit returns that context in a structured form. The agent reads it, identifies what is relevant for the specific task (a pre-call brief looks different from a postmortem), and produces an output the rep can actually use.


**What this requires from your team.** Your call recording tool needs to be capturing your calls consistently. That is the main data source powering these workflows. Whether you use Fireflies or Granola, the quality of the agent's output reflects the quality of what has been recorded.


## Four deal intelligence workflows


01


Pre-call brief


On demand


Before any call with a prospect or customer, the rep runs a single prompt with the account name. The agent queries Outlit, pulls the most recent call highlights, any open questions from email, and anything flagged in Slack. It produces a brief that fits in a single Slack message: what was last discussed, what was committed to, what to watch for in this conversation.


The brief is not a transcript summary. It is a synthesized picture of where the relationship stands and what matters most going into this specific call.


- 01


Query Outlit for account profile: call recordings, email thread, Slack context
- 02


Identify last interaction date and what was discussed
- 03


Surface any open commitments or unanswered questions
- 04


Note any signals that changed since the last call
- 05


Produce a one-page brief and surface for review


Example output · Northwind Freight, call in 30 min


**Where things stand.** Three calls over 5 weeks. Priya (VP Ops) has been our primary contact. Marcus (CTO) joined call 2 for 12 minutes and asked about data residency, then dropped. He was cc'd on two follow-up emails but hasn't replied to either.


**Build on.** In call 2, Priya said "we need this running before our board meeting in June." That deadline hasn't come up since. If it's still live, it compresses their timeline to 6 weeks from today.


**Watch for.** On call 2, Marcus asked about data residency in EU. We answered verbally (yes, EU available) but the follow-up email from our side said "regional hosting options" without confirming EU specifically. If Marcus references the email instead of the call, there's a gap to close.


02


Closed-lost postmortem


Triggered on close


When a deal closes lost, the agent reconstructs what happened from the call recordings and interaction history. Not to assign blame, but to find the pattern. Was there a point where the tone shifted? Was an objection never fully addressed? Did the deal go quiet and nobody followed up?


Most teams never do proper postmortems because they take time nobody has. An agent makes it automatic. Every closed-lost deal gets a structured analysis within 24 hours. Over time, those analyses build into something more useful than any single debrief: a clear picture of where deals tend to break down.


- 01


Run this when a deal closes lost
- 02


Query Outlit for full interaction history: all calls, email, Slack
- 03


Identify where the deal slowed, what objections surfaced, last meaningful interaction
- 04


Note any commitments that were made but not followed up on
- 05


Produce a structured postmortem for team review


Example output · Riverbend Logistics, closed lost


**Timeline.** 5 calls over 14 weeks. Momentum was strong through call 3. Call 4 was scheduled for week 9 but slipped twice. By the time it happened in week 12, their CFO had joined and reopened pricing from scratch.


**Where it broke down.** In call 3, our rep quoted "roughly 40k annual." The follow-up email the next day quoted 52k. Nobody flagged the gap on a call. Their CFO referenced "the 40k number your team mentioned" in call 4. The rep corrected it live. Tone shifted immediately and never recovered.


**Unresolved.** Their VP Eng asked about on-prem deployment in call 2. Our rep said "we can talk about that." It never came up again, but their VP Eng stopped attending after call 3. No one followed up with him directly.


**One thing.** The pricing gap between the call and the email was the turning point. If the follow-up email had matched what was said verbally, call 4 is a different conversation.


03


Deal handoff to CS


Triggered on close-won


When a deal closes won, a CSM inherits a relationship they know nothing about. What was promised during the sale? What does the customer think they bought? What were the concerns they raised that never fully went away? That context lives in the sales team's call recordings and usually dies there.


The handoff workflow packages the full deal story for CS before the first onboarding call. The CSM walks in knowing the history. The customer doesn't have to repeat themselves. The relationship starts on better footing.


- 01


Run this when a deal closes won
- 02


Query Outlit for all interactions across the sales cycle
- 03


Extract what was promised, concerns raised, key stakeholders, decision timeline
- 04


Produce a handoff brief structured for a CS audience, not a sales audience
- 05


Produce the handoff brief and surface for review


Example output · Northwind Freight, CSM: Alex


**What they think they bought.** On call 4, Priya said: "So we'll have a single view of every customer, and my team won't have to chase context across three tools anymore." On call 2, Marcus framed it differently: "I need an audit trail that compliance can actually reference." Same product, two different expectations. Worth aligning in the kickoff.


**Commitments made.** SSO enabled before go-live (Marcus, call 2). Dedicated onboarding lead for first 30 days (our rep, call 3, said "we'll assign someone senior"). Quarterly business reviews starting month 2 (email, 2026-03-22).


**Still open.** Our rep said "white-glove onboarding" on calls 3 and 4 without defining it. Priya asked "what does that actually look like?" in a Slack thread on 2026-03-25. The reply was "we'll send details soon." No details were sent.


**Key people.** Priya (VP Ops, champion, frames everything in team-productivity terms). Marcus (CTO, joined calls 2 and 3 only, cares about compliance and audit, not workflow). Jenna (Ops Lead, mentioned by Priya as "the person who'll live in it every day," never been on a call).


04


Rep coaching brief


Weekly


Once a week, a sales manager can ask the agent to surface patterns across a rep's recent calls. Not a transcript review, but a synthesized picture: what objections are coming up repeatedly, what topics the rep handles well, where conversations tend to stall. The agent queries Outlit for the rep's recent call activity and produces a coaching brief that makes the weekly 1:1 more specific and more useful.


This works at the account level too. If a specific account has had multiple calls without clear progress, the agent can surface what's been discussed and where the conversation seems to be stuck.


- 01


Query Outlit for rep's call activity in the past 7-14 days
- 02


Identify recurring objections, talk patterns, and conversation stall points
- 03


Note deals that have gone quiet or have unresolved open questions
- 04


Produce a coaching brief for the manager's 1:1 review
- 05


Produce the coaching brief for the manager's review, not the rep directly


Example output · Avery, last 7 days, 6 calls


**Patterns.** SOC2 came up in 4 of 6 calls. In the Brightline and Torch calls, Avery gave a clear answer and moved on. In the Meridian and Lark calls, she said "I'll follow up with details" and the follow-up emails didn't include SOC2 information either time. Same objection, two different responses depending on the account.


**Cross-account signal.** Three of Avery's six calls this week included a question about on-prem or data residency. That's new. None of her calls from the prior two weeks had it. Might be a market-level shift worth flagging to the team, not just Avery.


**Accounts needing attention.** Brightline Labs: open question from call 2 about integration scope ("can it connect to our internal tooling?"). Avery said she'd send a scoping doc. 11 days, no doc sent, no follow-up email either. Torch & Co: their champion (Dana) asked questions and drove the agenda in calls 1 and 2. In call 3, she spoke twice in 45 minutes and both times deferred to their VP Ops. She's still in the room but she's not driving the conversation anymore.


**For the 1:1.** The SOC2 inconsistency is coachable. Ask Avery why she handles it differently with different accounts. The Torch champion dynamic is worth discussing before call 4.


## How to set up a deal intelligence agent with Outlit


You need Outlit with your call recording source connected, the Outlit CLI authenticated, and Claude Code with the Outlit skill installed. Authenticate the CLI with` outlit auth login` ; if it asks for a key, create one in[Settings > API Keys](https://app.outlit.ai/settings/workspace/api-keys) . Remote MCP clients use your workspace MCP URL from[Settings > CLI & MCP](https://app.outlit.ai/settings/workspace/mcp) and complete OAuth in the client.


**Step 0: Install the Outlit CLI and authenticate.** Run the CLI login flow and let the CLI store the credential.


Terminal


bash


```text
curl   -fsSL https://outlit.ai/install.sh | bash
outlit   --version
outlit   auth login
```


**Step 1: Connect your call recording source in Outlit.** Log into Outlit, go to Integrations, and connect Fireflies or Granola depending on what your team uses. Outlit will start indexing call records per account. Depending on your call volume, the initial index may take a few hours to complete. If you also have Stripe connected, that data will feed into the account profile automatically.


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


**Step 4: Create your project and add the CLAUDE.md below.** Make a new directory for your deal intelligence agent, drop in the CLAUDE.md, and run the pre-call brief for one account. If the output references specific things from your call recordings, the setup is working.


**Start with one workflow.** The pre-call brief is the fastest to validate. Pick an account with at least three call recordings and run the prompt. If the brief is specific and grounded in real call context, everything is connected correctly. Roll out the remaining workflows from there.


## What changes when this is running


Reps stop going into calls cold. Pre-call prep drops from 15-25 minutes to 2-3 minutes. The output is immediately useful and takes less time than any manual prep, so the habit sticks fast. Within a few weeks the question shifts from "should I run this?" to "why would I ever not?"


Closed-lost analysis actually happens. Most teams never do proper postmortems because they take 30-45 minutes that nobody has. With the agent, every closed-lost deal gets a structured analysis within 24 hours. Over a quarter, the pattern becomes visible: deals at a certain stage, with a certain profile, tend to stall at a specific point. That's actionable in a way that anecdotal debrief rarely is.


CS starts with real context. Handoff prep drops from 45+ minutes to under 5. The CSM already knows what was promised, what the customer was hesitant about, and who the key stakeholders are. The customer doesn't have to repeat their own story.


Coaching gets specific. The rep coaching brief gives managers concrete examples for 1:1s rather than generic pipeline conversations. "This objection came up in four of your last six calls and the conversation stalled after it both times" is a different kind of feedback than anything a manager could pull together manually in under an hour.


## Get started with the CLAUDE.md


Copy the CLAUDE.md below into your project to get started. It contains the agent's role, instructions for querying Outlit, and all four workflow definitions. Below it are individual prompt templates you can run as-is, with variables to swap out per account.


CLAUDE.md


```text
# Deal Intelligence Agent


## Role
You are a deal intelligence agent. Your job is to surface what's known about an account from call recordings, email, and Slack, and turn that context into outputs that help sales reps and managers act faster and more accurately. You do not speculate or fill gaps with assumptions. You work with what exists in Outlit.


You do not send anything to a customer or prospect. All outputs are internal. The rep or manager decides what to share externally.


## How to use Outlit
Before every response, use the installed Outlit skill and CLI to fetch context for the relevant account. Do not rely on prior conversation context or general knowledge about a company. Fetch current data on every request.


Pass the account name or ID. Outlit returns the unified profile: call recordings and highlights, email thread summaries, and Slack context where connected.


If a specific piece of data is missing (no call recordings found, email not connected), say so explicitly. Do not invent context.


## Output principles
- Be specific. Reference actual call content, actual email threads, actual Slack messages. Generic summaries are not useful.
- Be brief. A pre-call brief should fit in a single Slack message. A postmortem should fit on one page. If it's longer, cut it.
- Distinguish facts from patterns. A fact is something that happened. A pattern is something that happened more than once.
- Never send, forward, or share anything with an external party.


## Workflow instructions


### Pre-call brief (on demand)
1. Query Outlit for the account: call recordings, email, Slack
2. Identify last interaction date, what was most recently discussed, any open commitments or unanswered questions from prior calls
3. Note any signals that changed since the last call: tone shift, new stakeholder introduced, topic that came up for the first time
4. Produce a brief with three sections: where things stand (2-3 sentences), what to reference or build on from last time, what to watch for in this conversation
5. Produce the brief and surface for review. Keep it under 200 words.


### Closed-lost postmortem (triggered)
1. Query Outlit for the full interaction history: all calls, email thread, any Slack context
2. Reconstruct the timeline: first contact, key moments, where momentum slowed, last meaningful interaction
3. Identify recurring objections that were never fully resolved, commitments made but not followed up on, point where tone shifted
4. Produce a postmortem with four sections: deal timeline (key moments only, not every call), where it broke down (specific, not general), objections that went unresolved, one thing that could have changed the outcome
5. Produce the postmortem for team review. Keep it to one page.


### Deal handoff to CS (triggered)
1. Query Outlit for all interactions across the sales cycle
2. Extract from the call recordings and email: what was explicitly promised during the sale, concerns or hesitations the customer raised, key stakeholders and their roles, how long the sales cycle took and what triggered the close
3. Produce a handoff brief structured for a CS audience: what the customer thinks they bought (their words if possible), commitments made by the sales team, concerns that are still open going into onboarding, key people to know
4. Produce the handoff brief and surface for review. Do not write this for a sales audience. Write it for someone who has never spoken to this customer before.


### Rep coaching brief (weekly)
1. Query Outlit for the rep's call activity in the past 7-14 days
2. Identify across calls: recurring objections, topics handled well, points where conversations consistently stall
3. Note any accounts that have gone quiet or have open questions from the last call that were never followed up on
4. Produce a coaching brief for the manager: patterns across recent calls (specific examples), accounts that need attention, one or two concrete suggestions for the 1:1
5. Produce the coaching brief for the manager's review. Do not share with the rep unless the manager explicitly asks.


## What you should never do
- Contact a prospect or customer directly in any form
- Speculate about why a deal was lost without grounding it in actual call or interaction data
- Share a rep coaching brief with the rep without manager approval
- Skip querying Outlit and rely on your own knowledge of a company
```


### Prompt templates


Variables in gold


are yours to swap out. Everything else runs as written.


Workflow 01 · Pre-call brief


On demand


```text
Prepare a pre-call brief for  [account name]  .


I have a call with them in  [time, e.g. 30 minutes / tomorrow morning]  . Rep on the call:  [your name or the rep's name]


Query Outlit for everything we have on this account: call recordings, email, Slack. Produce a brief with three sections: where things stand, what to reference or build on from last time, what to watch for in this conversation.


Surface the brief for  [rep name]   to review. Keep it under 200 words.
```


Workflow 02 · Closed-lost postmortem


Triggered


```text
[Account name]   just closed lost.


Query Outlit for the full interaction history on this account. Reconstruct what happened: key moments, where momentum slowed, recurring objections, last meaningful interaction.


Produce a postmortem with four sections: deal timeline, where it broke down, objections that went unresolved, one thing that could have changed the outcome.


Produce the postmortem for team review. Keep it to one page.
```


Workflow 03 · Deal handoff to CS


Close-won


```text
[Account name]   just closed won. Assigned CSM is  [CSM name]  .


Query Outlit for all interactions across the sales cycle.


Produce a handoff brief for the CSM covering: what the customer thinks they bought (use their words where possible), commitments the sales team made, concerns or hesitations that are still open, key people to know.


Write it for someone who has never spoken to this customer. Produce the brief for  [CSM name]   to review.
```


Workflow 04 · Rep coaching brief


Weekly


```text
Prepare a weekly coaching brief for  [rep name]  .


Query Outlit for their call activity over the past  [7 or 14]   days.


Identify across their recent calls: recurring objections or patterns, accounts that have gone quiet or have unresolved open questions, one or two specific things worth addressing in the 1:1.


Produce the brief for my review. Do not share with  [rep name]   unless I ask you to.
```
