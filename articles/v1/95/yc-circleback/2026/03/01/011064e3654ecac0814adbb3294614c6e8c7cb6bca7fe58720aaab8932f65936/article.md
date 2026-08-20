---
schema_version: "1.0.0"
document_id: "011064e3654ecac0814adbb3294614c6e8c7cb6bca7fe58720aaab8932f65936"
company_key: "yc-circleback"
company: "Circleback"
source_id: "yc-circleback-news-import-b22aa3eada49"
canonical_url: "https://circleback.ai/blog/circleback-automation-setup-guide"
published_at: "2026-03-11T00:00:00+00:00"
first_seen_at: "2026-07-21T13:31:38.263145+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:b1bddc6ea24f363a47ceccf6b2db110ee69f27411e50bd14ebe83971793d7cbe"
---

# How to Build 13 Circleback Automations: Complete Setup Guide

This guide provides step-by-step setup instructions for every automation in our[16 Circleback Automations That Save Hours Every Week](https://circleback.ai/blog/circleback-automations-you-should-build) blog post. Each walkthrough was built by asking the Circleback Assistant how to set it up.


Before you start, make sure automations are[enabled for your workspace](https://support.circleback.ai/en/articles/11049112-enable-automations-for-your-workspace) . All automations follow the same core pattern: set a trigger → add an action → choose a destination → save.


For a general overview, see[Getting started with automations](https://support.circleback.ai/en/articles/10460573-getting-started-with-automations) .


## 1. Deal summary to CRM after every sales call


**What it does:** Automatically pushes a structured deal summary to Salesforce or HubSpot after every tagged sales call.


### Step 1: Set up the trigger


1. Go to **Automations** → **Create automation**
2. Add a trigger condition:
3. **Option A:** Filter by your **Sales** tag
4. **Option B:** Filter by meeting name pattern (e.g., "contains 'Discovery'" or "contains 'Demo'")
5. You can combine conditions with "and"/"or" logic


### Step 2: Add an AI insights action


1. Select[Generate insights with AI](https://support.circleback.ai/en/articles/10505795-generate-insights-with-ai) from the actions menu
2. Create a custom insight with structured fields:
3. **Key Discussion Points**
4. **Objections Raised**
5. **Next Steps**
6. **Deal Stage Signals**
7. Write a prompt describing what you want extracted — be specific about the format


### Step 3: Push to your CRM


**If using Salesforce:** 1. Add[Salesforce](https://support.circleback.ai/en/articles/10641585-integrate-with-salesforce) as a second action 2. Connect your Salesforce account 3. Choose to update **opportunities** (matches by invitee email → contact) 4. Select which meeting outcomes to include (notes, action items, insights)


**If using HubSpot:** 1. Add[HubSpot](https://support.circleback.ai/en/articles/10542439-integrate-with-hubspot) as a second action 2. Choose to update **deals** 3. Select your meeting outcomes


**If using another CRM:** - Use[Zapier](https://support.circleback.ai/en/articles/10508886-integrate-with-apps-using-zapier) or **Make** to connect to Pipedrive, Affinity, or other apps - Or use the **Webhook** action for custom integrations


### Step 4: Save and enable


1. Name your automation (e.g., "Sales Call → CRM Summary")
2. Click **Create**


The automation runs automatically after each matching call — your CRM gets updated before you've left Zoom.


## 2. Competitive mentions to a dedicated Slack channel


**What it does:** Detects competitor mentions in sales calls and pushes a digest to your #competitive-intel Slack channel.


### Step 1: Set up the trigger


1. Go to **Automations** → **Create automation**
2. Filter by your **Sales** tag, or by meeting name pattern (e.g., "contains 'Discovery'" or "contains 'Demo'")


### Step 2: Add an AI insights action


1. Select[Generate insights with AI](https://support.circleback.ai/en/articles/10505795-generate-insights-with-ai)
2. Create a custom insight called "Competitive Mentions"
3. Write a prompt like:


> *Extract any mentions of competitors discussed in this call. For each mention, include: the competitor name, what the prospect said about them, and any context about how they're evaluating alternatives. If no competitors were mentioned, return "None."*


1. Add structured fields: **Competitor Name** , **What Was Said** , **Context / Evaluation Status**


### Step 3: Send to Slack


1. Add[Slack](https://support.circleback.ai/en/articles/10672225-integrate-with-slack) as a second action
2. Select your **#competitive-intel** channel
3. For private channels, add the Circleback app to the channel first (Channel details → Integrations → Apps)
4. Include: insights, meeting name, attendees, and a link to full notes


### Step 4: Save and enable


1. Name it (e.g., "Competitor Intel → Slack")
2. Click **Create**


**Pro tip:**[Share this automation with your whole sales team](https://support.circleback.ai/en/articles/11049112-enable-automations-for-your-workspace) so it runs on everyone's calls — go to the automation's **…** menu → **Share** → enable for your workspace.


## 3. Discovery call scorecard to Slack


**What it does:** Generates a structured scorecard after every discovery call with questions asked, pain points, budget signals, and more.


### Step 1: Set up the trigger


1. Go to **Automations** → **Create automation**
2. Filter by meeting name pattern (e.g., "contains 'Discovery'"), or by your Sales tag + name pattern for tighter targeting, or create a dedicated "Discovery" tag


### Step 2: Add an AI insights action


2. Create a custom insight called "Discovery Call Scorecard"
3. Add structured fields:
4. **Questions Asked** — key discovery questions the rep asked
5. **Pain Points Uncovered** — prospect's challenges and frustrations
6. **Budget Signals** — mentions of budget, pricing sensitivity, or spend
7. **Timeline Indicators** — urgency, decision timeframe, implementation deadlines
8. **Decision-Maker Involvement** — who's involved, who has final say
9. **Overall Score** (optional) — have AI rate the call quality 1–5
10. Write a prompt like:


> *Analyze this discovery call and score it across the following dimensions. Be specific and quote the prospect where relevant. If a category wasn't covered, note it as "Not discussed."*


### Step 3: Send to Slack


2. Select your sales team channel (e.g., **#sales-team** or **#discovery-calls** )
3. Include: insights, meeting name, attendees, and link to full notes


### Step 4: Save and enable


1. Name it (e.g., "Discovery Scorecard → Slack")
2. Click **Create**


**Pro tip:** Share this workspace-wide so it runs for your entire sales team. Managers can review scorecards async without joining every call.


## 4. Sales meeting feed to Slack with pipeline context


**What it does:** Pushes structured deal intelligence to a #sales-pipeline Slack channel after each sales call, creating a running feed you can review anytime.


### Step 1: Set up the trigger


1. Go to **Automations** → **Create automation**
2. Filter by your **Sales** tag


### Step 2: Add an AI insights action


2. Create fields:
3. **Deal Stage Signals** — where this deal sits in the pipeline
4. **Objections Raised** — concerns or pushback from the prospect
5. **Commitments Made** — next steps, promises, follow-ups


### Step 3: Send to Slack


2. Push to **#sales-pipeline** or similar channel
3. Include: meeting name, attendees, and insights


### Step 4: Save and enable


1. Name it (e.g., "Sales Pipeline Feed → Slack")
2. Click **Create**


This gives you a running feed throughout the week — every sales call drops structured intel into Slack as it happens.


**For weekly digests:** On Fridays, use Circleback Ask: "What objections came up in sales calls this week?" or "Summarize all sales meetings this week, grouped by company." For a fully automated weekly rollup, route meeting data to a Google Sheet via[Zapier](https://support.circleback.ai/en/articles/10508886-integrate-with-apps-using-zapier) and set up a scheduled Zap to compile and send the digest.


## 5. Meeting summary to the right Slack channel


**What it does:** Routes meeting summaries to the right Slack channel based on team or project tags. The most popular Circleback automation.


This automation is covered in full detail in the[blog post](https://circleback.ai/blog/circleback-automations-you-should-build) , since it's the one most people build first. The short version:


### Step 1: Create one automation per channel pairing


1. Go to **Automations** → **Create automation**
2. Add a trigger (meeting name pattern like "contains 'standup'" or a team tag like "Engineering")
3. Add[Slack](https://support.circleback.ai/en/articles/10672225-integrate-with-slack) as the action
4. Select the destination channel (#engineering, #marketing, etc.)
5. Include: summary/notes, action items, attendees, link to full meeting


### Step 2: Repeat for each team


Meeting type Trigger condition Slack channel


Engineering standup Name contains "standup" #engineering


Marketing sync Tag = "Marketing" #marketing


Leadership review Name contains "leadership" #leadership


Product planning Tag = "Product" #product


Design reviews Tag = "Design" #design


### Step 3: Share with your workspace


Go to each automation's **…** menu → **Share** →[enable for your workspace](https://support.circleback.ai/en/articles/11049112-enable-automations-for-your-workspace) . For private channels, add the Circleback app first.


**Tips:** Be specific with conditions — if "standup" appears in multiple teams' meetings, combine conditions (e.g., name contains "standup" AND attendee includes "@engineering.com"). Start with 2–3 core channels, then expand.


## 6. Action items to your project management tool


**What it does:** Pushes extracted action items with owners and deadlines to Linear, Notion, or any PM tool via Zapier.


### Step 1: Set up the trigger


1. Go to **Automations** → **Create automation**
2. Choose which meetings should push action items:
3. **All meetings:** Leave conditions blank
4. **Specific teams:** Filter by tag (e.g., "Engineering," "Product")
5. **Specific meeting types:** Filter by name pattern


### Step 2: Connect your project management tool


**If using Linear:** 1. Add[Linear](https://support.circleback.ai/en/articles/10508846-integrate-with-linear) as the action 2. Connect your Linear account 3. Select the team/project where tasks should land 4. Action items automatically become Linear issues with owners assigned


**If using Notion:** 1. Add[Notion](https://support.circleback.ai/en/articles/10508865-integrate-with-notion) as the action 2. Connect your Notion workspace 3. Select the database where tasks should go 4. Map action items to your database properties


**If using Asana, Jira, Monday, or others:** 1. Add[Zapier](https://support.circleback.ai/en/articles/10508886-integrate-with-apps-using-zapier) or **Make** as the action 2. Set up a Zap that creates tasks in your tool with owner + deadline fields mapped


### Step 3: Save and enable


1. Name it (e.g., "Action Items → Linear")
2. Click **Create**
3. [Share with your workspace](https://support.circleback.ai/en/articles/11049112-enable-automations-for-your-workspace) so it runs for everyone


**Pro tip:** Create multiple automations to route action items to different places based on meeting type — engineering standups to Linear, marketing syncs to Asana, product planning to Notion.


## 7. Meeting notes to Notion or Google Docs


**What it does:** Automatically saves full meeting notes — summary, action items, transcript link — to a Notion database or Google Doc after every meeting.


### Step 1: Set up the trigger


1. Go to **Automations** → **Create automation**
2. Choose which meetings should save notes:
3. **All meetings:** Leave conditions blank
4. **Specific types:** Filter by tag or name pattern


### Step 2: Connect your destination


**If using Notion:** 1. Add[Notion](https://support.circleback.ai/en/articles/10508865-integrate-with-notion) as the action 2. Connect your Notion workspace 3. Select the database where notes should go 4. Map meeting data to your database properties (title, date, attendees, summary, action items)


**If using Google Docs:** 1. Add[Google Docs](https://support.circleback.ai/en/articles/10508827-integrate-with-google-docs) as the action 2. Connect your Google account 3. Select the shared folder where docs should be saved 4. Each meeting creates a new doc with the full notes


### Step 3: Save and enable


1. Name it (e.g., "Meeting Notes → Notion" or "Meeting Notes → Google Docs")
2. Click **Create**


**Pro tip:** Create multiple automations to route different meeting types to different destinations:


Meeting type Destination


Customer calls Notion CRM database


Internal meetings Google Docs shared folder


Interviews Private Notion hiring database


## 8. Customer feedback to a product channel


**What it does:** Extracts feature requests, complaints, and praise from customer calls and pushes them to a #product-feedback Slack channel.


### Step 1: Set up the trigger


1. Go to **Automations** → **Create automation**
2. Filter by your **Customer check-in** or **Customer interview** tag, or by meeting name pattern (e.g., "contains 'customer'" or "contains 'check-in'"), or combine tags with OR logic


### Step 2: Add an AI insights action


2. Create a custom insight called "Customer Feedback"
3. Add structured fields:
4. **Feature Requests** — what they wish the product did
5. **Complaints / Pain Points** — frustrations, bugs, friction
6. **Praise** — what they love, what's working well
7. **Verbatim Quotes** (optional) — exact words for extra impact
8. Write a prompt like:


> *Extract customer feedback from this call. Categorize into: feature requests (what they want built), complaints (what's frustrating them), and praise (what they love). Include direct quotes where possible. If a category has nothing, mark it "None mentioned."*


### Step 3: Send to Slack


2. Select your **#product-feedback** channel
3. Include: insights, meeting name, attendees, and link to full notes


### Step 4: Save and enable


1. Name it (e.g., "Customer Feedback → Product")
2. Click **Create**
3. [Share with your workspace](https://support.circleback.ai/en/articles/11049112-enable-automations-for-your-workspace) so it captures calls from your whole CS/Sales team


**Pro tip:** After a month, ask Circleback Ask: "What feature requests came up most often in customer calls this month?" Patterns will surface fast.


## 9. Customer health signals to your CS team


**What it does:** Detects sentiment signals (churn risk, frustration, enthusiasm) in customer meetings and routes alerts to your CS channel.


### Step 1: Set up the trigger


1. Go to **Automations** → **Create automation**
2. Filter by **Customer check-in** or **Customer interview** tag, or combine tags with OR logic


### Step 2: Add an AI insights action


2. Create a custom insight called "Customer Health Signals"
3. Add structured fields:
4. **Churn Risk Phrases** — "evaluating other options," "not sure we'll renew," "too expensive," "looking at alternatives"
5. **Frustration Signals** — complaints, repeated issues, negative tone
6. **Enthusiasm Signals** — praise, expansion interest, referral mentions
7. **Overall Sentiment** — Red / Yellow / Green
8. **Recommended Action** (optional) — AI-suggested next step
9. Write a prompt like:


> *Analyze this customer meeting for health signals. Flag any churn risk language, frustration signals, and enthusiasm signals. Rate overall sentiment as Red (at risk), Yellow (some concerns), or Green (healthy). Include direct quotes where possible. If no concerning signals, note "No red flags detected."*


### Step 3: Send to Slack


2. Select your **#customer-success** or **#cs-alerts** channel
3. Include: insights, meeting name, attendees, and link to full notes


### Step 4: Save and enable


1. Name it (e.g., "Customer Health → CS Team")
2. Click **Create**
3. [Share with your workspace](https://support.circleback.ai/en/articles/11049112-enable-automations-for-your-workspace) so it runs on all customer-facing calls


**Pro tip:** Create a second automation filtered to fire only when sentiment = Red, routed to a dedicated **#churn-alerts** channel or DM to the CS lead.


## 10. Send meeting recap to attendees automatically


**What it does:** Sends a clean meeting recap to everyone on the call within minutes of the meeting ending.


### Quick setup: Use Settings (simplest)


Go to **Settings → Emails** and configure:


1. **Who receives the email:**
2. **Myself** — get a copy of notes after every meeting
3. **Everyone invited** — automatically send notes to all calendar invitees
4. **What's included:**
5. Notes are always included
6. **Recording and transcript** — toggle to include a link to the full meeting record


This works for a simple "send to all attendees on every meeting" setup. See[Email sending preferences](https://support.circleback.ai/en/articles/10460632-email-sending-preferences) for details.


## 11. Executive summary for leadership meetings


**What it does:** Generates a concise executive summary — decisions, owners, open questions, strategic context — after leadership or board meetings.


### Step 1: Set up the trigger


1. Go to **Automations** → **Create automation**
2. Filter by meeting name pattern (e.g., "contains 'leadership'" or "contains 'board'" or "contains 'exec'"), or create a dedicated "Leadership" tag


### Step 2: Add an AI insights action


2. Create a custom insight called "Executive Summary"
3. Add structured fields:
4. **Decisions Made** — what was agreed, approved, or finalized
5. **Owners Assigned** — who's responsible for what
6. **Open Questions** — unresolved items needing follow-up
7. **Strategic Context** — key themes, priorities, or shifts discussed
8. **Timeline/Deadlines** (optional) — dates or milestones mentioned
9. Write a prompt like:


> *Generate a concise executive summary of this leadership meeting. Include: key decisions made (with context), owners assigned, open questions still unresolved, and strategic context. Keep it scannable — this is for busy executives who need the essentials fast.*


### Step 3: Route to restricted channel


Add[Slack](https://support.circleback.ai/en/articles/10672225-integrate-with-slack) → select your restricted channel (e.g., #leadership) 2. Add Circleback app to the private channel first


### Step 4: Save and enable


1. Name it (e.g., "Leadership Summary → Exec Channel")
2. Click **Create**


**Pro tip:** Keep this automation private (don't share workspace-wide) so only designated people can trigger exec summaries. Sensitive meeting content stays controlled.


## 12. Cross-meeting themes with Circleback Ask


**What it does:** Surfaces recurring topics and patterns across all your meetings using Circleback Ask.


This automation works differently from the others — instead of triggering after a single meeting, it uses Circleback Ask to analyze patterns across many meetings at once.


### On-demand approach (no setup needed)


Open Circleback Ask and try prompts like:


- "What topics came up most frequently across all meetings this week?"
- "What themes keep recurring in customer calls this month?"
- "What strategic topics have been discussed across leadership meetings in the last 30 days?"
- "What issues were mentioned in multiple engineering standups this week?"


Run it every Friday and drop the answer into your leadership Slack channel. Takes two minutes.


### Automated approach (with Zapier)


For a fully automated weekly digest:


1. **Build a per-meeting automation:** Trigger on all meetings (or specific tags) →[Generate insights with AI](https://support.circleback.ai/en/articles/10505795-generate-insights-with-ai) (key topics, recurring issues, strategic mentions) → Send to[Zapier](https://support.circleback.ai/en/articles/10508886-integrate-with-apps-using-zapier) → Google Sheet
2. **Create a weekly Zap:** Schedule for every Friday → pull the week's rows → use an AI step to analyze themes → send the digest to Slack or email


## 13. Interview debrief to hiring channel


**What it does:** Compiles structured interview debriefs and pushes them to a private hiring channel.


### Step 1: Set up the trigger


1. Go to **Automations** → **Create automation**
2. Filter by your **Candidate interview** tag, or by meeting name pattern (e.g., "contains 'interview'" or "contains 'panel'")


### Step 2: Add an AI insights action


2. Create a custom insight called "Interview Debrief"
3. Add structured fields:
4. **Candidate Name** — who was interviewed
5. **Role** — position they're interviewing for (if mentioned)
6. **Strengths** — what impressed the interviewer(s)
7. **Concerns** — red flags, gaps, hesitations raised
8. **Specific Examples** — concrete moments or answers referenced
9. **Recommendation** — hire / no hire / next round (if stated)
10. **Interviewer** — who conducted this interview
11. Write a prompt like:


> *Extract a structured interview debrief from this conversation. Include: candidate strengths (what impressed), concerns or red flags, specific examples or answers mentioned, and the interviewer's recommendation if stated. Use direct quotes where possible. Keep it concise and factual.*


### Step 3: Send to private hiring channel


1. Add[Slack](https://support.circleback.ai/en/articles/10672225-integrate-with-slack) as the action
2. Select your private hiring channel (e.g., **#hiring** or **#recruiting-team** )
3. Add the Circleback app to the private channel first
4. Include: insights, meeting name, attendees, and link to full notes


### Step 4: Save and enable


1. Name it (e.g., "Interview Debrief → Hiring")
2. Click **Create**
3. [Share with your workspace](https://support.circleback.ai/en/articles/11049112-enable-automations-for-your-workspace) so it runs for all interviewers


**Pro tip:** For panel interviews with multiple interviewers in separate sessions, each interview triggers its own debrief. The hiring manager sees all assessments land in #hiring back-to-back — no chasing required.


## Related resources


- [Getting started with automations](https://support.circleback.ai/en/articles/10460573-getting-started-with-automations)
- [Generate insights with AI](https://support.circleback.ai/en/articles/10505795-generate-insights-with-ai)
- [Auto-tagging meetings](https://support.circleback.ai/en/articles/9890336-auto-tagging)
- [Integrate with Slack](https://support.circleback.ai/en/articles/10672225-integrate-with-slack)
- [Integrate with Salesforce](https://support.circleback.ai/en/articles/10641585-integrate-with-salesforce)
- [Integrate with HubSpot](https://support.circleback.ai/en/articles/10542439-integrate-with-hubspot)
- [Integrate with Linear](https://support.circleback.ai/en/articles/10508846-integrate-with-linear)
- [Integrate with Notion](https://support.circleback.ai/en/articles/10508865-integrate-with-notion)
- [Integrate with Google Docs](https://support.circleback.ai/en/articles/10508827-integrate-with-google-docs)
- [Integrate with apps using Zapier](https://support.circleback.ai/en/articles/10508886-integrate-with-apps-using-zapier)
- [Draft email action](https://support.circleback.ai/en/articles/10508787-draft-email)
- [Email sending preferences](https://support.circleback.ai/en/articles/10460632-email-sending-preferences)
- [Enable automations for your workspace](https://support.circleback.ai/en/articles/11049112-enable-automations-for-your-workspace)
