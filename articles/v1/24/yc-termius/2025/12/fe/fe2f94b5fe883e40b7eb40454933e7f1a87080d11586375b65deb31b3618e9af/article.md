---
schema_version: "1.0.0"
document_id: "fe2f94b5fe883e40b7eb40454933e7f1a87080d11586375b65deb31b3618e9af"
company_key: "yc-termius"
company: "Termius"
source_id: "yc-termius-news-import-2bcf9b9228b4"
canonical_url: "https://termius.com/blog/remember-what-was-done-with-bookmarks-for-session-logs"
published_at: "2025-12-10T00:00:00+00:00"
first_seen_at: "2026-07-24T03:42:18.942730+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:b36b7a132cc034ef29196d86a6eb1a253cb00e88cf01aa847384374f7a351358"
---

# Long-term memory for Session Logs - Termius Blog

There's a moment every engineer knows too well. You connect to a host to fix something, wondering what happened last time you touched this server. Sometimes you remember. Most of the time, you don't.


Working in a team, the situation gets even trickier. Everyone has pieces of the puzzle, but they're scattered across people and their different devices. Your teammate changed something last night, but their laptop is now asleep at home — along with the only record of what they did.


Reconstructing what happened becomes a small investigation. Context takes too long to retrieve. Handover relies on Slack messages, screenshots, or whatever someone wrote down in a hurry, if written at all. That's where the idea behind Session Logs in Termius began.The First Step: Recent Session Logs


Earlier this year, we rolled out the recent Session Logs, which immediately solved one of the most common frustrations: picking up where you left off, regardless of the device you were using. Fix something quickly on your phone at 2 AM, and the next morning, the same session log appears on your laptop, ready for you to revise and write a post-mortem.


## But what if a log needs to live longer?


That's where we discovered the limitation. Having the latest session recorded is great when you're bouncing between devices, but not every session is meant to disappear the moment a new one begins.


Some sessions carry important decisions or discoveries: the time you finally fixed a recurring issue and want to document it, the on-call workaround at 2 AM that absolutely needs a proper fix later. These aren't disposable fragments of history, but the context. Losing it means losing an essential part of the picture.


There was a need to preserve the sessions that mattered without relying on screenshots, copy-paste, or the hope that someone stored them in a document somewhere. That's how Bookmarks were born.


## Introducing Bookmarks for Session Logs


Bookmarks let you save selected session logs to your vault for as long as you need. When a session contains something important — a tricky Nginx issue, a chain of errors you want to revisit later, or a workaround your teammates should be aware of — you can preserve it with one action.


The notes section lets you capture the reasoning behind your decisions: *What were you trying to fix? Why did you roll back? What did you discover along the way?*


Write it down, and it stays with the session forever.


The real change happens when you're working in a team.


Bookmarks stored in team vaults instantly become shared context. A teammate can open your bookmarked session, scroll through every command you ran, read exactly why the session mattered, and continue the work without repeating your steps. No Slack archaeology. No guesswork.


With Bookmarks, troubleshooting stops being an isolated activity trapped on a single device. It becomes a shared, traceable workflow that survives handovers, shifts, and device switching. For the first time, your team has something the terminal never had: long-term, shared, end-to-end encrypted memory of what actually happened.
