---
schema_version: "1.0.0"
document_id: "6244b63ebe31badb77ff967822745db36c0e208a0daa9ef0f01752346e33a342"
company_key: "yc-furtherai"
company: "FurtherAI"
source_id: "yc-furtherai-news-import-96169723635d"
canonical_url: "https://www.furtherai.com/blog/introducing-mentions-human-in-the-loop-ai-agents"
published_at: null
first_seen_at: "2026-07-25T06:00:31.379140+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:f01423ec9766cb0c88c5cf1425910c2f057c36bf390a786f1e5ec619ce4cbed4"
---

# Mentions: Human-in-the-Loop AI Agents at FurtherAI

Today we're launching Mentions in FurtherAI. When an AI agent reaches a step that needs human judgment, it pauses the workflow and tags the right person, the same way you'd @mention a coworker in Slack. That person opens the mention, sees the exact step and its full context, makes the call, and the agent picks the work back up. No one has to sit and watch the run.


Micromanaging is bad for humans. It turns out it's not great for AI either. Most insurance workflows aren't fully autonomous, and forcing a person to monitor every run just to catch the one moment their input matters wastes the time the automation was supposed to give back.


## Why insurance workflows still need a human in the loop


An agent can handle most of an[underwriting](https://www.furtherai.com/blog/ai-for-underwriting) or[claims workflow](https://www.furtherai.com/blog/best-ai-tools-insurance-claims-processing) on its own, from classifying documents and extracting data to running checks, standardizing loss runs, and drafting a summary. But most workflows aren't end to end, and certain steps still need human judgment or a sign-off.


Before Mentions, catching those moments meant reviewing everything. Someone had to stay close to a run just to be there when their input mattered, which is a poor use of an expert's time and undercuts the point of automating the rest.


## How Mentions works


Mentions gives the agent a way to ask for help without stopping everything or handing the whole task back to you. Here's the flow:


1. The agent runs the workflow until it hits a step flagged for human review.
2. It pauses at that step and creates a mention for the specific person who owns that decision.
3. That person gets the mention in a dedicated Mentions tab, with an open count so nothing slips through.
4. They open it, see the exact step, the account, and the full context the agent has gathered so far.
5. They make the decision and resume the workflow with one click.
6. The agent continues through the remaining steps and marks the mention resolved.


Each mention carries the details you need to act fast: the account name, the workflow it belongs to, the step that's paused, who it's assigned to, and how long it's been waiting. Open mentions and resolved ones live in separate views, so your queue always reflects what still needs a human.


‍


‍


## What this looks like in a real workflow


Take a property submissions workflow, one of the more involved ones an underwriting team runs. The agent moves through 17 steps on its own, from receiving the submission and extracting account and[ACORD data](https://www.furtherai.com/blog/best-ai-acord-form-data-extraction) to running clearance and OFAC checks, aggregating loss runs, calculating a triage score, completing adverse media research, and generating the[underwriting summary](https://www.furtherai.com/blog/automate-underwriting-summary-audit-capabilities-mga) .


Then it reaches a step that needs a sign-off, reviewing the Salesforce data, and pauses. The agent mentions the underwriter who owns the account. They click into the open item, land on that specific workflow with the full context for the decision, and approve the step to unblock the agent. It creates the opportunity in Salesforce, finishes the run, and the mention moves to resolved.


## Why this matters for underwriting and claims teams


Mentions changes what "automation" has to mean for your team. You no longer have to choose between a workflow that runs fully on its own and one a person supervises end to end. The agent does the work, and, in the words of FurtherAI's Insurance Product Lead Danny O'Lenic, "instead of reviewing everything, you're pulled in when judgment is necessary."


That keeps accountability where it belongs, with a clear record of who made which decision and when, and it keeps your underwriters and adjusters focused on high-value work instead of watching progress bars.


**Mentions are live in FurtherAI today.**


‍


**DISCLAIMER**


*This article is for general informational purposes only and does not constitute legal, regulatory, compliance, underwriting, or other professional advice. The content reflects information available as of the date of publication, and FurtherAI undertakes no obligation to update it as laws, regulations, or AI technologies evolve.*


‍
