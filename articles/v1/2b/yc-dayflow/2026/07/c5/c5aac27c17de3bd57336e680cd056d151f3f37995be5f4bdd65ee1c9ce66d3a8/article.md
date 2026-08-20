---
schema_version: "1.0.0"
document_id: "c5aac27c17de3bd57336e680cd056d151f3f37995be5f4bdd65ee1c9ce66d3a8"
company_key: "yc-dayflow"
company: "Dayflow"
source_id: "yc-dayflow-rss-659ae9f20797"
canonical_url: "https://www.dayflow.so/blog/what-is-an-automatic-time-tracker/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.227507+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:6049d66b81904d293abf3c6a9c7865bd4d5c0f28a38a1475242259bd5cfe1e1f"
---

# What is an automatic time tracker?

An automatic time tracker is software that records where your time goes without you starting timers, filling timesheets, or tagging anything. You work; it watches; at the end of the day there is a record. The category exists because the manual version fails the same way for everyone: you forget to start the timer, then forget to stop it, then stop using the tool.


## The three kinds


**1. App and window loggers.** These record which application and window title is in the foreground, and build charts from it.[ActivityWatch](https://activitywatch.net/) (free, open source, cross-platform) is the reference implementation; RescueTime and Rize add cloud dashboards, productivity scores, and coaching on top. Strength: extremely lightweight, works everywhere. Limit: the output is app names - “Chrome, 5h 12m” - which tells you where your windows were, not what you did.


**2. Screen-based journals.** These look at the screen itself and reconstruct the work.[Dayflow](https://www.dayflow.so/) (free, open source, Mac) records one frame every 10 seconds and has AI write a readable timeline - “Implementing OAuth token refresh, 9:04-9:48” - because the same Chrome window can be code review or cat videos, and only the screen contents know which. Strength: the output is an actual account of your day in sentences. Limit: Mac-only, and it sees only what happens on that screen.


**3. AI memory reconstructors.** Tools like Timely’s “Memory” record app activity to a cloud timeline, then help you (or an AI) assemble billable time entries from it afterward. Aimed at teams and client billing rather than personal awareness.


## What separates the good ones


- **Where the data lives.** Your complete working history is sensitive. Some tools keep it on your machine (Dayflow, ActivityWatch, Qbserve, Memtime); most cloud tools process it on their servers. If the record is intimate, local should be the default.
- **What the output is.** Charts answer “how much”; a journal answers “what happened.” Pick by the question you actually ask at 6pm - for most individuals it is[where did my day go](https://www.dayflow.so/where-did-my-day-go/) , which charts cannot answer.
- **Idle cost.** An all-day tool must be invisible: think ~100MB of RAM and under 1% CPU ([the battery math is here](https://www.dayflow.so/blog/screen-recording-battery-impact/) ).
- **Price and openness.** The field splits into subscriptions ($7-25/month for RescueTime, Rize, Timing, Timely) and free open source (ActivityWatch, Dayflow). Open source also means the privacy claims are auditable rather than promised.


## The short version


If you want cross-platform app statistics, ActivityWatch is the free classic. If you bill clients by the hour, look at Timing or Memtime. If the question is what you actually did all day, answered in plain sentences with everything staying on your Mac - that is exactly what Dayflow is: the only Mac option that is fully automatic, AI-powered, 100% local, and free at the same time. The[full comparison across nine trackers is here](https://www.dayflow.so/best-time-trackers-mac/) .
