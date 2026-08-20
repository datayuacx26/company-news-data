---
schema_version: "1.0.0"
document_id: "1e03e1d70b75837e6d1e0ee5d8dcc38198d143af7cd1cb8212424cb65f4e2dea"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/set-goals-for-your-agents"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:080aee11aa42226d69f8ee9da028df52e1013db800f5a7cf85a589119ed8066f"
---

# Set Goals for Your Agents

Agents can now chase **Goals** . Instead of prompting an Agent step by step, you describe the outcome you want in plain language and it works toward that goal on its own in a loop: each pass runs a full turn with all of its tools, checks whether the goal is met, and loops again until it's done or it reaches the step limit you set.


Goals can run once on demand or on a schedule, like "every day at 9am", so an Agent can keep a task handled without you in the loop. Every iteration runs as a normal Agent turn against its own session, so the whole thing is auditable in run history.


## What's New


- **Goals you describe in plain language.** Tell an Agent what "done" looks like and it pursues the outcome in a self-checking loop, verifying its own progress along the way instead of waiting for the next prompt.
- **You set the step limit.** Choose how many work steps a goal can take before it stops (each step uses real tokens, so keep it modest). A token safety cap also stops any run that goes long, so a goal always ends on its own: when it's met, or when it reaches its limit.
- **Schedulable.** Add a cadence like "every day at 9am" and the goal runs on repeat.
- **The command.** Type in any Agent chat to describe and run a goal on the spot, or send alone to browse the goals saved on that Agent.
- **Every run lands in Runs.** Goal runs show up in the Agent's new **Runs** view alongside chat runs and Computer Use sessions, each tagged with the goal name, how it was triggered (manual or scheduled), the outcome (goal met or "Budget reached"), and the steps and tokens it used.


## Why This Matters


Some work isn't one question and one answer; it's an objective that takes several steps and a bit of judgment about when it's finished. Goals let an Agent own that loop end to end: it decides what to do next, checks its own work, and stops when the outcome is reached, all within limits you control.


## How It Works


1. Add a goal from an Agent's settings, or type followed by your objective in chat.
2. Optionally give it a schedule and set how many steps it can take.
3. The Agent loops toward the goal, running a turn and checking progress after each pass, until it's complete or it reaches its step limit. Track every run in the Agent's run history.


Notes:


- Send with no text to browse the goals already saved on an Agent.
- Each goal iteration is a normal Agent turn with the Agent's full toolset, recorded in run history for auditing.
