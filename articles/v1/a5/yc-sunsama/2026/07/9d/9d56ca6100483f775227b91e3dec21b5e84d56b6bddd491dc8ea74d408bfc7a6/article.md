---
schema_version: "1.0.0"
document_id: "9d56ca6100483f775227b91e3dec21b5e84d56b6bddd491dc8ea74d408bfc7a6"
company_key: "yc-sunsama"
company: "Sunsama"
source_id: "yc-sunsama-news-import-3d5f9384ecf1"
canonical_url: "https://roadmap.sunsama.com/changelog/weekly-product-changelog-june-5-2026"
published_at: null
first_seen_at: "2026-07-24T02:45:30.397865+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:f92fe92a8c97c76e8bd46807103aa619f1b34a4a78682eac5403d726f01876b8"
---

# Weekly Product Changelog: June 5, 2026

[Sunsama](https://roadmap.sunsama.com/)


[Home](https://roadmap.sunsama.com/)[Feedback](https://roadmap.sunsama.com/improvements)


Feedback


[Changelog](https://roadmap.sunsama.com/changelog)


[← Back to changelog](https://roadmap.sunsama.com/changelog)[Powered by Canny](https://canny.io/powered-by-canny?utm_source=changelog_subdomain&utm_medium=powered&utm_campaign=sunsama&company=Sunsama)


Added:


- Sunny reasoning: Sunny now shows its thinking process as a collapsible summary while it works.
- Toggl task mapping: You can now map Sunsama tasks to specific Toggl tasks within a project from your channel settings, for more precise time tracking
- MCP backlog sorting: The GET_BACKLOG_TASKS MCP tool now returns tasks in your backlog order instead of random order


Improved:


- MCP timeboxing: The timebox_a_task_to_calendar tool now respects your per-channel calendar settings, matching how timeboxing works in the UI
- Sunny chat: The chat window now stays pinned to the bottom while Sunny is streaming a response


Fixed:


- iCloud calendar events: Bookings synced via iCloud (e.g. from[cal.com](http://cal.com/) ) were incorrectly treated as free time, causing tasks to be auto-scheduled on top of them
- Monthly recurrence: Selecting recurring days in a different order no longer picks the wrong first occurrence; "last day of month" combined with specific days now works correctly
- Backlog pagination: Tasks at position 0 in the backlog were silently dropped when scrolling past page boundaries
- Subtask timer: Pausing a subtask timer now immediately reflects in the UI; no page reload needed
- Trello import: Pasting a Trello card URL into the task dialog now correctly carries over the due date
- Asana My Tasks: Custom sections now load correctly when sorted by due date (affects paid Asana plans with custom sections)
- Outlook links: Email backlinks in the desktop app now use the correct URL format and open reliably in your browser
- Calendar visibility toggle: The toggle to mark a calendar event as non-blocking is now reliably responsive
- Task merging (macOS): Option-dragging a task onto another to merge it as a subtask no longer crashes and freezes the UI
