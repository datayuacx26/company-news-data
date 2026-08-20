---
schema_version: "1.0.0"
document_id: "f536b748f8f47d044ded4750c31712ca43eb7513a92d7d1dac7e48c4b5fae5af"
company_key: "yc-moda"
company: "Moda"
source_id: "yc-moda-news-import-ace8c1d7597a"
canonical_url: "https://moda.dev/blog/cohorts"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-24T04:44:21.515931+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:ef82f4ba676bee71371fdbed507089a42be2739b53901bdf3c4d6916d7ff70e0"
---

# Cohorts: see which AI users are about to churn, and why

Most analytics tools tell you that conversation volume dropped 18% last week. Few tell you which users are about to leave. Almost none tell you why.


Today we're shipping **Cohorts** in Moda: behavioral groups built from the signals already in your conversation data. The cohort that matters most on most days is "at risk of churn", and we built it so you can answer the question every PM and founder eventually asks themselves at 11pm: *who's about to leave, and what do I do about it?*


Behavioral cohort assignment


0 / 32 users routed


signal pool · per-user evidence


Each user is joined to their frustration markers, tool failures, and session deltas, then routed to the cohort whose signature they match.


At risk of churn


0


frustration up · failures unresolved


Power users


0


consistent return · broad use


Stuck early


0


abandoned in first 3 sessions


Quiet decliners


0


fell off without complaining


01


## What we shipped


Open Moda, click into Users. Your users are now grouped not by plan or signup date, but by behavior:


- **At risk of churn.** Frustration trending up, tool failures unresolved, sessions declining.
- **Power users.** Consistent return, broad feature use, low friction.
- **Stuck early.** Abandoned a workflow within the first three sessions.
- **Quiet decliners.** Once-active users who fell off without complaining.


These aren't heuristics on top of usage frequency. Each cohort is derived from the signals Moda already produces per conversation: per-message embeddings


, topic segmentation, frustration detections, tool failure events. We just join those signals at the user level and surface the groups that matter.


Click any cohort to see the individual users. Click any user to see their evidence: which conversations, which tool failures, which frustration markers fired and when. Every claim Moda makes about a user is traceable back to the underlying conversation.


user_482 · trailing 6 weeks


sessions / week


−6w


−5w


−4w


−3w


−2w


−1w


now


frustration markers


tool failure rate


cohort signature


sessions


−73% trailing 4w


frustration


0 → 7 markers


tool failures


auth-tool 4xx, 6 in 7d


The trajectory above is what "at risk" actually looks like under the hood. Three independent signals (session decline, frustration markers, tool failures) all firing in the same window on the same user. Any one of them in isolation is noise. All three together is a cohort.


02


## Ask it in plain English


The part we are most excited about isn't in the dashboard.


Moda has a CLI (` moda` ) that exposes the same data the dashboard does. We packaged it as a skill that drops into Claude Code, Cursor, Codex, or any agent that can run shell commands. Which means you can open your terminal and ask:


agent · ~/work/moda


>


Your agent loads the skill, runs` moda frustrations` ,` moda tool-failures` , and` moda conversations` against your tenant, joins the results, and gives you a ranked list with the reasoning attached. The last line of the response is a *pattern* : the thread that connects the users in the cohort, e.g., *"7 of 10 share the same auth-tool failure path since Tuesday."* That pattern is the part you can actually ship a fix for. The dashboard tells you *who* ; the agent finds the thread that connects them. (And yes, those users are fabricated; we are not naming names in a launch post.)


You don't need an agent to use the CLI.` moda overview` ,` moda frustrations` , and the rest work directly in your shell. But if you already drive your work through Claude Code, Cursor, or Codex, this is one less context switch.


03


## Why behavioral cohorts beat demographic ones


Demographic cohorts ("paid plan", "signed up Q1", "team plan") are useful for marketing. They are not useful for keeping users from leaving. The signal that someone is about to churn isn't who they are; it's what they're doing.


Three rephrase loops on the same intent. A retry on every payment-tool call. Sessions falling off a cliff after a tool started returning empty results. These signals are already sitting in your conversation logs. Most teams just don't have a way to surface them at a cohort level.


That's the gap Cohorts closes. Same data you already send us, no new instrumentation, no new dashboards to wire up.


04


## Available now


Cohorts is live for every Moda tenant.


- **Dashboard:**[moda.dev/users](https://moda.dev/users)
- **CLI:**` npm install -g @moda-ai/cli && moda init`
- **Claude Code, Cursor, Codex, or any agent:**` npx -p @moda-ai/cli moda init` . The skill registers automatically.


If you don't have a Moda tenant yet, request one at[moda.dev](https://moda.dev/) . New teams onboard in under an hour and you can have real cohorts on real data the same day.


05


## What's next


The next two things on the Cohorts roadmap:


1. **Cohort alerts.** Notify me when the at-risk cohort grows by more than X% week-over-week, or when a new behavioral group emerges that doesn't fit an existing one.
2. **Cross-cohort comparison.** What do power users do that at-risk users don't? Surface the diff automatically so the team building the product can act on it.


If there's a question your current dashboard can't answer today, tell us. That is, almost without exception, how the Moda roadmap gets built.


See which of your users are about to leave, before they tell you.


[Talk to the team](https://cal.com/team/moda/demo-meeting)
