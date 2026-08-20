---
schema_version: "1.0.0"
document_id: "6dc5547889fb314ff984d1a5f27d9f10576799de68d1cf8c90ae47d438e08cef"
company_key: "yc-dayflow"
company: "Dayflow"
source_id: "yc-dayflow-rss-659ae9f20797"
canonical_url: "https://www.dayflow.so/blog/see-where-your-time-goes-mac/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.227507+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:a490a1b4c13b56945846d6f9f798f64fb998cf1883e9e0529ffc8690d3dac598"
---

# How to see where your time goes on a Mac (no timers required)

There are three real ways to see where your time goes on a Mac, and none of them involve starting a timer. They differ in how much truth they give you.


## Option 1: Screen Time (built in, free, app-level)


Apple’s Screen Time (System Settings → Screen Time) already counts your app usage. Turn on “App & Website Activity” and you get daily and weekly charts of hours per app, pickups, and notifications.


Worth enabling because it is free and already there. Its ceiling: it answers in app names. “Safari: 4h 51m” contains your research, your writing breaks, and your rabbit holes in one undifferentiated bar. It also has no export to speak of and no idea what you were doing. (Full comparison:[Screen Time vs a real time tracker](https://www.dayflow.so/compare/screen-time-vs-dayflow/) .)


## Option 2: ActivityWatch (free, open source, window-level)


[ActivityWatch](https://activitywatch.net/) is the open-source standard for app logging: it records active apps and window titles locally, with browser extensions for per-site detail. You get honest, exportable data at the window-title level - one step more specific than Screen Time, still in the “which windows were open” universe.


Best if you want raw data to slice yourself and cross-platform coverage (Mac, Windows, Linux).


## Option 3: Dayflow (free, open source, task-level)


[Dayflow](https://www.dayflow.so/) answers a different question: not which apps were open, but what you were doing. It records the screen at one frame every 10 seconds and has AI write your day into a readable timeline - “Implementing OAuth token refresh, 9:04-9:48,” “Researching flight prices, 2:10-2:25” - because the useful unit of “where did my time go” is the task, not the app. Everything stays on your Mac (open source, MIT, local AI supported), and it runs on ~100MB of RAM.


Best if the output you want is an actual account of your day you can read - the[where-did-my-day-go question](https://www.dayflow.so/where-did-my-day-go/) , answered nightly.


## The honest recommendation


Enable Screen Time regardless; it is free context. Then pick by output: raw window data → ActivityWatch; a readable journal of what you actually did → Dayflow. Both are free and local, so trying each for a few days costs nothing but the comparison. If you need billable-hours features instead - client projects, invoicing rates - that is a[different category](https://www.dayflow.so/best-time-trackers-mac/) (Timing, Memtime) and worth paying for only if someone else pays for the hours.
