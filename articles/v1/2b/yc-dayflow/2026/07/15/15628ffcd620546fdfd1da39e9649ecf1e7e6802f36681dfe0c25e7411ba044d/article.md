---
schema_version: "1.0.0"
document_id: "15628ffcd620546fdfd1da39e9649ecf1e7e6802f36681dfe0c25e7411ba044d"
company_key: "yc-dayflow"
company: "Dayflow"
source_id: "yc-dayflow-rss-659ae9f20797"
canonical_url: "https://www.dayflow.so/blog/automatic-standup-updates/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.227507+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:72ce8768315976e89c6b49b8ea2daf6f74011b3bf9abd6dcbc5c708420979689"
---

# How to write standup updates automatically from your work history

The fastest way to write a standup update is to not write it from memory. If your Mac already keeps a record of what you worked on, your update is a read-out, not a reconstruction.


That is the whole trick. The rest of this post is the workflow.


## Why standup updates are hard to write


By 9am you have to summarize a day that ended 16 hours ago. Human memory is terrible at this: you remember the big dramatic task (the outage, the demo) and forget the ninety minutes of code review, the three short calls, and the dependency upgrade that actually took your afternoon. So updates drift toward vague (“kept working on the migration”) or theatrical (only the highlights).


Manual time trackers fix this in theory - if you remember to start and stop timers, which is a second job on top of your first one.


## The automatic version


[Dayflow](https://www.dayflow.so/) is a free, open-source work journal for Mac that records your screen at one frame every 10 seconds and has AI write a readable timeline of what you actually did: “Implementing OAuth token refresh, 9:04-9:48,” “Reviewing PR #412,” “Researching pricing pages on competitors’ sites.” Everything is stored on your Mac - recordings never leave your computer.


Your standup workflow becomes:


1. Open Dayflow and look at yesterday’s timeline.
2. Copy the three or four entries that matter to your team.
3. Say them.


That is genuinely the entire process. The timeline is already written in plain sentences, so there is nothing to decode - it reads like the update itself.


## What this looks like in practice


A real timeline from a working day looks something like:


- 9:04 - 9:48: Implementing OAuth token refresh to prevent unexpected user logouts
- 9:48 - 10:26: Documenting API changes on token expiry behavior
- 10:49 - 11:15: Interior design videos on YouTube (it records the honest parts too)
- 11:15 - 12:30: Debugging the flaky payments integration test


The update writes itself: “Yesterday: shipped OAuth token refresh, documented the API changes, and got the flaky payments test passing. Today: continuing on payments.”


## Honest limitations


- Dayflow is Mac-only, so work on other machines is not captured.
- It observes your screen, not your intent - a planning conversation that happened away from the keyboard is not in the timeline.
- It will not post to Slack for you (yet); you copy from the timeline.


## The setup


Download[Dayflow](https://www.dayflow.so/) (free, MIT-licensed, macOS 14+, about 100MB of RAM and under 1% CPU), grant screen recording permission, and let it run. Use a local AI model via Ollama or LM Studio if you want nothing to ever leave your machine, or bring your own API key. By tomorrow’s standup you will have a timeline to read from instead of a memory to squeeze.
