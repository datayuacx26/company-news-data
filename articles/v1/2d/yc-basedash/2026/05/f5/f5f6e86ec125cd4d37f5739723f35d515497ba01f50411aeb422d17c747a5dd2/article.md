---
schema_version: "1.0.0"
document_id: "f5f6e86ec125cd4d37f5739723f35d515497ba01f50411aeb422d17c747a5dd2"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/introducing-basedash-skills/"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:a9ba25102d95f57d5f477a9d67c4e8e8c1a2afac1bad23a15013919524d6f3dd"
---

# Introducing Basedash Skills

Today we’re launching **Skills** — reusable bundles of instructions that every Basedash AI surface can read on demand.


You define a concept once — how your team calculates activation, which fields matter for support triage, what makes a clean revenue breakdown — and every agent in Basedash picks it up automatically. No more pasting the same caveats into every prompt. No more drift between teams about what a metric actually means.


Teach Basedash once. It learns everywhere.


## Why we built it


AI is great at producing answers and terrible at remembering rules. Anyone who has used a chat-based BI tool for more than a few days has run into the same problem: you can describe what activation means once, but the next person asking the same question gets a different definition. The system has no memory of how your team thinks about its data.


We’ve watched our own team work around this for the past few weeks. Whenever someone asked the chat agent about “MRR” or “activation rate”, they’d quietly include the same definition in their prompt — “trials excluded, GAAP only, calendar weeks in UTC”. The definition was implicitly the team’s, but the AI had no way to know that.


Skills are how we move those definitions out of one-off prompts and into shared, durable context. They sit somewhere between a system prompt and a semantic layer: more structured than free-form context, lighter than dbt models.


## How Skills work


A skill is a name and a few lines of plain-language instructions. There’s no schema to follow, no SQL to write — you describe the concept the way you’d brief a new analyst.


1. **Write a skill.** An admin opens **Settings → AI context** and creates a skill with a name and a few lines of instructions — how a metric is defined, when to use a specific chart type, or which fields matter.
2. **AI reads it on demand.** Every agent in Basedash gets a lightweight catalogue of skill names in its system prompt. When a skill looks relevant to the current request, the agent loads its full instructions through a tool call you can see right in the thinking trace.
3. **Every surface uses it.** The same catalogue feeds chat, the chart builder, dashboards, automations, insights, and background tasks. One definition, every agent.


Crucially, there’s no semantic search or black-box retrieval here. The agent decides which skill is relevant based on the name + the context of the request, and you can see exactly which skills it loaded for any given answer. Auditable by design.


## What a good skill looks like


Skills work best when each one focuses on a single concept or workflow. A few patterns we’ve seen work well internally:


- **Activation rate.** “Activation = signed up and completed onboarding within 7 days. Exclude trial-only accounts and internal email domains. Use calendar weeks in UTC; weeks start on Monday. Always compare cohorts by signup_month.”
- **Revenue analysis.** “MRR is GAAP recurring revenue, excluding trial accounts. Always segment revenue by plan tier in any breakdown. When comparing periods, use calendar months in UTC.”
- **Support triage.** “Group tickets by category and priority before summarizing. Tickets open longer than 48 hours are SLA risk. Use human-readable status names, not raw enum values.”
- **Experiments.** “A/B reports use exposure_first events, 7-day windows, and always include p-values. Treatment and control are denoted by the variant column.”


A skill can be anywhere from a single line to a few paragraphs — up to 50,000 characters, so even your most nuanced playbooks fit. Skills are the natural home for the kinds of conventions that used to live in a Notion page nobody reads.


## Skills as a lightweight semantic layer


The use case we get most excited about: skills as a lightweight semantic layer for metrics. Every B2B SaaS company has its own definition of MRR, activation, churn, and retention — and “lightweight” is the operative word here. You don’t have to model your entire warehouse in dbt to start codifying the metrics that matter.


You write a skill called “Activation rate” once. It explains how to calculate it, which accounts to exclude, what time window to use, and what chart you prefer. From then on, anyone in your organization who asks the AI about activation gets a consistent answer — whether they’re chatting with the agent, building a dashboard, or scheduling a weekly report. The chart builder lays it out the way you specified. The automation runs use the same rules. The insights agent surfaces trends in the right units.


This is the part where the multi-surface story matters. A semantic layer that only powers one feature is just an opinionated function. A semantic layer that powers every AI surface in your BI tool is something else: a shared brain that gets smarter every time someone adds a skill.


## How we use Skills at Basedash


We’ve been running on Skills internally for the past several weeks, and they’ve quietly become some of the most reached-for context in our workspace.


Our growth team wrote an “Activation rate” skill on day one — the definition was already on a Notion page, so it took five minutes to port over. Now nobody has to remember the trial exclusion rule when asking the agent about weekly activation. Our finance team’s “Revenue analysis” skill is the reason every chart in our exec dashboards uses GAAP and excludes trials, even when the person building it didn’t know to specify. Our support team’s triage skill means automation-generated weekly summaries categorize tickets the way humans would.


The compounding effect is real. We started with three skills, and now we’re at fifteen. Each new one removes another thing the team had to remember to type into a prompt — and every existing dashboard and automation gets a little smarter without anyone touching them.


## Getting started


Skills are available today for all Basedash users.


1. [Sign up for Basedash](https://charts.basedash.com/signup) (or log in)
2. As an admin, go to **Settings → Organization → AI context**
3. Click **New skill** and write your first one — try a metric you’ve explained more than once
4. Ask the chat agent about it and watch the **Reading {name} skill** step appear in the trace


For setup tips and best practices, see the[Skills feature page](https://www.basedash.com/features/skills) or the[AI context docs](https://www.basedash.com/docs/features/custom-context) .


## What’s next


Skills round out our story for shared, durable AI context. Combined with[global context](https://www.basedash.com/docs/features/custom-context) for org-wide facts,[Insights](https://www.basedash.com/features/insights) for proactive findings,[Dashboards](https://www.basedash.com/features/dashboards) for AI-built reports, and[Automations](https://www.basedash.com/features/automations) for scheduled work, Basedash now has a place for every layer of how teams encode their business in AI.


We’re going to keep investing here — letting anyone in your org propose a skill, surfacing which skills were loaded for any given answer, and helping agents author new skills as they learn what your team cares about. The shape we’re chasing: a BI tool that gets sharper the longer you use it.


**Write your first skill today and stop re-explaining what your metrics mean.**
