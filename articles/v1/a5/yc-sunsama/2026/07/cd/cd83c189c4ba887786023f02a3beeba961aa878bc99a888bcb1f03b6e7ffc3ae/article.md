---
schema_version: "1.0.0"
document_id: "cd83c189c4ba887786023f02a3beeba961aa878bc99a888bcb1f03b6e7ffc3ae"
company_key: "yc-sunsama"
company: "Sunsama"
source_id: "yc-sunsama-news-import-3d5f9384ecf1"
canonical_url: "https://roadmap.sunsama.com/changelog/task-priority-auto-sort"
published_at: null
first_seen_at: "2026-07-24T02:45:30.397865+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:6464a6d44882452076c288d1accef36f699f80b6cd18a70491afc02b9b519642"
---

# Task Priority & Auto-Sort

[Sunsama](https://roadmap.sunsama.com/)


[Home](https://roadmap.sunsama.com/)[Feedback](https://roadmap.sunsama.com/improvements)


Feedback


[Changelog](https://roadmap.sunsama.com/changelog)


[← Back to changelog](https://roadmap.sunsama.com/changelog)[Powered by Canny](https://canny.io/powered-by-canny?utm_source=changelog_subdomain&utm_medium=powered&utm_campaign=sunsama&company=Sunsama)


Task Priority


Mark which tasks are important.


There are now two priority systems: one for your daily task list, and one for your backlog. They serve different purposes, so they have different levels and different behavior.


Daily priority


Daily priorities reset each day by default so they stay intentional.


Backlog priority


Backlog priorities persist.


Adjusting daily priority decay


By default, daily priority resets to "normal" the next day. The intent: a task you flagged as urgent for today shouldn't quietly stay urgent forever. If it rolls over and is still important tomorrow, you can re-flag it.


You can opt into rollover for specific levels in Settings → General → Priority rollover


. Selected levels persist across days; unselected ones reset.


Read more about task priority from the[help center](https://help.sunsama.com/docs/usage-guides/tasks/task-priority/) .


Auto-Sort


Auto-sort keeps your daily task list in a sensible order without you having to drag tasks around. When you create or change a task, Sunsama figures out where it should sit in the day's list and puts it there.


When auto-sort runs


Auto-sort repositions a task when:


- The task is created
- Its priority changes
- Its planned time changes
- A timer is started on it


It tries to move only the task that changed. In some situations, it might reposition other tasks as well to keep everything in chronological order.


What auto-sort honors


- Priority


— higher-priority tasks land above lower-priority ones. See[Task Priority](https://roadmap.sunsama.com/docs/usage-guides/tasks/task-priority) .
- Scheduled times


— tasks with a fixed calendar time, and recurring tasks with rigid start times, stay in their chronological position
- Your manual pins


— if you've manually dragged a task against priority order (e.g. a no-priority task above an urgent one), Sunsama assumes that's intentional and won't move it
- Running timer


— if a timer is running on a task, that task stays pinned at the top


Turning auto-sort on or off


Go to Settings → General → Auto-sort tasks


to enable or disable auto-sort entirely.


Read more about auto-sort from the[help center](https://help.sunsama.com/docs/usage-guides/tasks/auto-sort/) .
