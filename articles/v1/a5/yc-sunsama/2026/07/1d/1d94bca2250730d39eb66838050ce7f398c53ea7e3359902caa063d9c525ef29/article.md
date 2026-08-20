---
schema_version: "1.0.0"
document_id: "1d94bca2250730d39eb66838050ce7f398c53ea7e3359902caa063d9c525ef29"
company_key: "yc-sunsama"
company: "Sunsama"
source_id: "yc-sunsama-news-import-3d5f9384ecf1"
canonical_url: "https://roadmap.sunsama.com/changelog/weekly-product-changelog-may-15-2026"
published_at: null
first_seen_at: "2026-07-24T02:45:30.397865+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:5e95ab10f7a136514fefdbb093516d5fe9d53927317d689eed320b4d48d7e2d1"
---

# Weekly Product Changelog: May 15, 2026

[Sunsama](https://roadmap.sunsama.com/)


[Home](https://roadmap.sunsama.com/)[Feedback](https://roadmap.sunsama.com/improvements)


Feedback


[Changelog](https://roadmap.sunsama.com/changelog)


[← Back to changelog](https://roadmap.sunsama.com/changelog)[Powered by Canny](https://canny.io/powered-by-canny?utm_source=changelog_subdomain&utm_medium=powered&utm_campaign=sunsama&company=Sunsama)


Added:


- Task Priority: Set priorities with a single keystroke and filter your daily view to focus on what matters most. Read more about how it works[here](https://roadmap.sunsama.com/changelog/task-priority-auto-sort) .
- Linear: Hide done tasks: New "Hide done" toggle in the Linear integration panel filters out completed issues from your list.
- MCP improvements: Sunny can now see integration source details on imported tasks (GitHub repo, Linear issue number, Jira key, etc.), and a new get_task_by_id tool lets you fetch any task directly by its Sunsama ID.
- Disable spellcheck: New user toggle in Settings > Display to turn off spell-check across all text editors.


Improved:


- Import priority from integrations: Tasks imported from Linear, Todoist, Jira, Asana, and other sources now carry their priority signal into Sunsama.
- Auto-sort on integration import: Tasks added via the integration panel now respect your auto-sort preference and land in their correct sorted position.
- Auto-sort on day change: Moving a task to a different day via keyboard shortcuts (D / S), right-click reschedule, the date picker, PlanDay, or the mobile long-press menu now triggers auto-sort on the destination day.


Fixed:


- Various auto-sort bug fixes: Several gaps where auto-sort didn't run when it should have been addressed across import and day-change workflows.
- Fixed a tablet log in issue
- Menu bar event order: Upcoming events in the Mac menu bar are now sorted chronologically.
- Toggl: full channel name visible on hover.
- Workload counter: snoozed tasks with subtasks handled correctly in today's workload counter for time remaining
- Fixed an issue with recurring tasks start time changing to midnight.
- Desktop: Dock icon raises main window
- Desktop: Focus bar respects "focus mode disabled" setting on break start
- Desktop: Pomodoro sounds restored: Session-end and break-end sounds were silently dropped during a recent focus bar refactor and are now back.
- Desktop: Focus bar z-order on Windows: Focus bar no longer loses its always-on-top position after running for a while.
