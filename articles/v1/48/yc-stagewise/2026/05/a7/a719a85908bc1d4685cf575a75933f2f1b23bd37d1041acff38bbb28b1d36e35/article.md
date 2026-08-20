---
schema_version: "1.0.0"
document_id: "a719a85908bc1d4685cf575a75933f2f1b23bd37d1041acff38bbb28b1d36e35"
company_key: "yc-stagewise"
company: "stagewise"
source_id: "yc-stagewise-news-import-0e327623c986"
canonical_url: "https://stagewise.io/news/release-week-may-18-24"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-22T14:44:14.475585+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:8c504811783d23e79cb10613cd678cd373ef2453120fb09dd6f9afd16d3a1fc8"
---

# Release Week: May 18–24

## What Shipped Last Week


Last week focused on making stagewise easier to use as an orchestration platform for multiple agents.


We re-aligned browsing capabilities around agents instead of treating browser tabs as a standalone feature. We also focused on better keyboard navigation and other small quality-of-life improvements.


### Browser tabs now belong to agents by default


Browser tabs got a redesigned UI, and the ownership model changed with it. Tabs are now **per-agent by default** , so tab state follows the agent that opened it.


The pin icon makes a tab global. Use it when a page should stay available outside one agent.


### stagewise now recovers your tabs and selected agent after restart


stagewise now stores which tabs were open before the last restart, plus which agent was selected.


On launch, both come back. The app state is closer to where you left it.


### UI zoom, more hotkeys, and less input clutter


You can now zoom the stagewise UI in and out with the usual app shortcuts: **CMD/CTRL+-** to zoom out, **CMD/CTRL++** to zoom in, and **CMD/CTRL+0** to reset.


We also added more hotkeys across the app, so common actions are easier to reach from the keyboard.


We removed the list of agent-owned shell sections from the chat input area. It added noise in a place that should stay focused on the prompt.


### Gemini 3.5 Flash support


Gemini 3.5 Flash is now available in the model picker.


This adds another current Google model option for agentic coding work inside stagewise.


### CMD+K opens the new control center


We added a new **CMD+K control center** for navigation and app actions.


It gives you one searchable place for agents, tabs, settings, and app actions.


## What the Week Adds Up To


This week was about state recovery, cleaner tab ownership, more keyboard entry points, and broader model support.


Less reset between tabs, agents, and restarts.
