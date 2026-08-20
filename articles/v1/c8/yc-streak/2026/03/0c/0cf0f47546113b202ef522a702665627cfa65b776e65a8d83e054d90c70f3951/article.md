---
schema_version: "1.0.0"
document_id: "0cf0f47546113b202ef522a702665627cfa65b776e65a8d83e054d90c70f3951"
company_key: "yc-streak"
company: "Streak"
source_id: "yc-streak-news-import-5582781ca789"
canonical_url: "https://www.streak.com/post/prioritize-outreach-ai-autofill"
published_at: "2026-03-26T00:00:00+00:00"
first_seen_at: "2026-07-24T03:06:01.355147+00:00"
fetched_at: "2026-07-28T22:17:35.086279+00:00"
content_hash: "sha256:2d2fadaef1f26ee8d938c8e9918dc8ee2bc0fdf47d41050ae27ed32f20011111"
---

# How to prioritize outreach in Streak using AI autofill

Streak's customer success team manages thousands of accounts. When it's time to do proactive outreach — a feature rollout, a re-engagement push, a renewal cycle — the question isn't what to say. It's who to contact first.


Treating every account the same wastes time on contacts who won't respond and underinvests in the ones who will.


So we built a prompt using Streak's[AI autofill](https://www.streak.com/features/ai-autofill) feature that scores every account and helps us decide where to invest our time.


The AI reads each contact's engagement history — emails, call logs, meeting notes, support interactions — and assigns a priority score directly in your pipeline.


If you manage existing customer relationships in customer success, account management, or sales, you likely face the same triage. In the past, it's usually done manually: scanning recent activity, checking when you last heard from someone, making a judgment call.


This post walks through how to set up a prompt to score accounts in your own pipelines. Scroll down to find the exact prompt available to copy and paste into your AI autofill custom instructions.


## What is outreach priority scoring?


Outreach priority scoring is a method for ranking existing contacts by their likelihood to respond to outreach, using behavioral signals already logged in your CRM — emails, calls, meetings, and support history.


It's distinct from[lead scoring](https://www.streak.com/post/8-lead-scoring-examples-for-better-lead-prioritization?utm_source=blog&utm_medium=hyperlink&utm_campaign=outreach-priority-scoring) , which evaluates new prospects using external data like firmographics, company size, or website behavior.


Outreach priority scoring works on contacts you already have a relationship with. The question it answers isn't "is this a good prospect?" — it's "is this person engaged and likely to respond if I reach out today?"


## Who this is for


This approach is useful for anyone[managing a pipeline](https://www.streak.com/features/ai-pipeline-creator?utm_source=blog&utm_medium=hyperlink&utm_campaign=outreach-priority-scoring) of existing contacts where outreach volume is high enough that manual prioritization becomes a bottleneck. That includes:


- **Customer success teams** doing proactive check-ins, renewals outreach, or re-engagement campaigns
- **Account managers** managing a large book of accounts with varying activity levels
- **Sales teams** working a mix of active and dormant deals, where knowing who's still warm matters


The common thread: you have engagement history in your CRM, and you need a faster way to decide who's worth contacting first.


## When to use outreach priority scoring


This isn't something you'd run on every contact every day. It's most useful at specific moments in your workflow:


- **Before a proactive outreach push** — score your pipeline first so you're reaching out to the most likely responders, not working alphabetically or by deal size
- **After a period of inactivity** — if a segment of your pipeline has gone quiet, scoring helps you identify who's actually gone cold versus who just hasn't been contacted recently
- **At the start of a new period** — quarterly business reviews, renewal cycles, re-engagement campaigns — any time you're about to do a sweep of your contacts and want to sequence it intelligently
- **When you've inherited a pipeline** — if you're picking up someone else's accounts, scoring gives you a fast read on who has an active relationship versus who you're essentially starting from scratch with


## What signals to evaluate before reaching out


The most reliable signals for predicting responsiveness come from behavior, not attributes. Here's what to look for:


- **Past engagement strength** — Have they[opened or replied to emails](https://www.streak.com/features#email-tracking) ? Attended calls? Asked follow-up questions or taken action after conversations?
- **Inbound help-seeking behavior** — Have they reached out proactively? Contacted support? Raised issues or requested guidance?
- **Ongoing relationship indicators** — How recent was their last engagement? Are there multiple touchpoints over time? Is the tone of past interactions warm?
- **Responsiveness pattern** — Do they typically reply? Have recent threads stalled? Did they stop responding after several attempts?


These four signals form the backbone of the scoring prompt below.


## How to set up outreach priority scoring using Streak AI Autofill


1. Open your pipeline in Streak and create a new column (e.g., "Outreach Priority").
2. In the column settings menu, open the **AI Autofill** section.
3. Paste in the prompt below and click **Save** .
4. Select the contacts you want to score.
5. Click the **AI Autofill** menu at the top of your pipeline and select the fields you want to autofill. In this case, the “Outreach Priority” field.
6. Streak reads each box's history and fills the column with a score.


You can run this on individual records or in batch across your entire pipeline.


## The outreach priority prompt (copy + paste)


Copy the prompt below directly into your AI autofill column.


```text
You are analyzing an email inbox, call logs, meeting notes, and support history to determine outreach priority.


Your goal is to assign an outreach priority score   from     1   to   5  ,   where  :
5   = Highest priority (Very likely to respond; strong past engagement)
4   = High priority (Good engagement history; likely to respond)
3   = Medium priority (Some engagement; responsiveness unclear)
2   = Low priority (Minimal engagement or weak responsiveness)
1   = Very low priority (No meaningful engagement or historically unresponsive)


Evaluation CriteriaAssess the contact using the following signals:
1.   Past Engagement Strength
Have they responded to emails   in   the past?
Have they attended meetings or calls?
Have they asked follow-up questions?
Have they taken action after conversations?
2.   Inbound Help-Seeking Behavior
Have they reached out asking   for   help?
Have they contacted support?
Do call logs or meeting notes show they engaged   with   support?
Have they escalated issues or requested guidance?
3.   Ongoing Relationship Indicators
Recency   of   engagement (more recent = higher priority)
Multiple touchpoints over time
Evidence   of   collaboration or reliance on our team
Positive or warm tone   in   past interactions
4.   Responsiveness Pattern
Do they typically reply?
Do they ignore outreach?
Do threads show stalled communication?
Did they stop responding after multiple attempts?


Scoring Guidelines
Frequent help-seeking or repeated engagement →   4   or   5
Recent inbound request → minimum   4
Consistent replies and active participation →   4   or   5
Some past engagement but inconsistent replies →   3
Minimal interaction history →   2
No engagement or historically non-responsive →   1
If they have reached out   for   help multiple times   in   the last   90   days, strongly favor a   5.
If they have not responded to the last   3   outbound attempts, cap at   2.


Output Format (Follow Exactly)
Priority Score: [  1  –  5  ]
Engagement Summary:(Brief summary   of   relevant history)
Signals Identified:
- Bullet   1
- Bullet   2
- Bullet   3
Reasoning  :(  2  –  4   concise sentences explaining why   this   score was assigned)
The response should always be formatted   as   Priority Score: after the colon, insert the score   1  –  5.
```


` ‍
`


A few notes on the prompt:


- The 1–5 scale is calibrated so that a score of 5 reflects strong, recent, multi-touchpoint engagement, and a score of 1 reflects no meaningful history.
- The output format (Priority Score, Engagement Summary, Signals Identified, Reasoning) is explicitly defined so the AI returns structured, consistent results across every record.
- If a contact has reached out for help multiple times in the last 90 days, the prompt instructs the AI to strongly favor a 5. If they haven't responded to the last three outbound attempts, the score is capped at 2.


## What the output looks like


Each scored record returns a structured result in this format:


**Priority Score:** \[1–5\]


**Engagement Summary:** A brief summary of relevant history.


**Signals Identified:**


- Signal 1
- Signal 2
- Signal 3


**Reasoning:** 2–4 sentences explaining why this score was assigned.


This makes it easy to scan a pipeline view and prioritize accordingly — without opening each box individually.


## How Streak's success team uses outreach priority scoring


Before any proactive outreach push — a new feature rollout, a re-engagement sweep, a renewal cycle — run the autofill across the relevant segment to score contacts before you start writing.


Where the score changes behavior is in how much effort goes into each outreach.


A 4 or 5 signals recent engagement: a support ticket, a sales conversation, active threads. Those contacts are worth more personalized, higher-effort emails. A 2 or 1 might still get contacted if there's a product reason to reach out, but a templated message makes more sense than a carefully crafted one.


The score doesn't decide who to contact — it decides where to invest the time.


It's also flexible by design. Different team members run it at different cadences depending on what they're working on. The prompt stays the same; when and how you act on the output depends on your goal.


## FAQ


### **What is outreach priority scoring?**


Outreach priority scoring ranks existing contacts by how likely they are to respond to outreach, based on behavioral signals already in your CRM — email replies, call logs, meeting notes, and support history. It helps you sequence outreach by actual engagement rather than guessing or working alphabetically.


### **How is outreach priority scoring different from lead scoring?**


Lead scoring evaluates new prospects using external signals — company size, industry, website behavior. Outreach priority scoring evaluates existing contacts using internal signals already in your CRM. The goal is different too: lead scoring identifies who to pursue; outreach priority scoring identifies who is most likely to respond when you reach out.


### **How does AI autofill work in Streak to score contacts?**


AI autofill reads the timeline of each box in your Streak pipeline — emails, call logs, meeting notes, and any activity logged to that record — and applies a prompt you define. For outreach priority scoring, the prompt instructs the AI to evaluate engagement signals and return a 1–5 score with a summary and reasoning. You can run it on individual records or in batch across your entire pipeline.


### **Can I use this prompt for a sales pipeline, not just customer success?**


Yes. The prompt evaluates engagement signals that apply across pipeline types. For a sales context, you may want to adjust the evaluation criteria to reflect what responsiveness looks like earlier in a relationship — for example, weighting demo attendance or proposal follow-through more heavily.


### **What plan is AI autofill available on?**


AI autofill is available on all Streak plans. Each autofill operation uses 1 AI credit. The number of credits included varies by plan — see[streak.com/pricing](http://streak.com/pricing) for a full breakdown.


### **How do AI credits work with autofill?**


Each time AI autofill fills a field, it uses 1 AI credit from your plan's monthly allowance. Credits are only deducted when an operation completes successfully — if autofill doesn't return a result, no credits are used. If an action is about to use a significant number of credits, Streak will show a confirmation prompt before running. Additional credits can be purchased as a recurring add-on if you need more than your plan includes.
