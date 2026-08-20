---
schema_version: "1.0.0"
document_id: "939664c4854f2ae3d8b3446be5ccec77e7e04597a04b29c27bf7aeeb4981e6a5"
company_key: "yc-sunsama"
company: "Sunsama"
source_id: "yc-sunsama-rss-77cf29bd998b"
canonical_url: "https://www.sunsama.com/blog/why-we-built-it-slack-and-teams-status-sync"
published_at: "2024-11-21T22:34:41+00:00"
first_seen_at: "2026-07-20T23:20:39.474964+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:2d1c0b2c6ee36937cd3c197fcedf9c82985123358023989ef054c16e4aa9f86b"
---

# Why we built it: Slack and Teams Status Sync - Sunsama Blog

When you enter[Focus Mode](https://www.sunsama.com/features/focus-mode) , Sunsama automatically updates Microsoft Teams or Slack to pause notifications, update your status message, and change your associated emoji to protect your focus and let your team know you're focused. Exiting focus mode reverts your status and settings. This is probably one of my favorite "hidden gem" features in Sunsama.


This feature grew out of my own pre-focus rituals. When I sit down to focus and start working, I go through a cleanup/setup ritual to create a clean desktop with only essential windows open and notifications muted:


- Closing unnecessary tabs and windows.
- Opening the necessary tabs and windows.
- Rearranging my windows, desktops, and split-screen setup to avoid bouncing between apps and windows.
- Putting away my phone and switching to do not disturb mode.


It’s all minor stuff, but it adds up to a big impact for me. When a notification pops up or a pile of papers sits in the corner of my desk, my eyes dart away from my work every few seconds. When there’s stuff everywhere, I get this feeling I can best describe as a mental “itch”. There’s something bothering me that I need to look at or look into and I can’t focus on the task at hand knowing that thing is in the way. Even when I cook dinner, I tidy the kitchen first and lay out my tools so that mental “itch” doesn’t bother me. For years, I didn’t realize this mental “itch” was constantly breaking my focus and keeping from doing deep work.


The big breakthrough for me came when I first started writing software. I discovered that I did my best work late at night, working until 2 or 3 AM with just the computer screen in front of me. It wasn’t just that this was the “romanticized ideal” of software development—my fingers flew across the keyboard faster, my code had fewer bugs, and I’d lose track of time for hours. The darkness effectively neutralized all distractions in my office or apartment with a simple flip of a switch. Once I realized this, I began finding ways to control my visual environment, which led me to tidy up and prepare my digital workspace before starting a new task. In fact, this is the motivating principle behind many interfaces in Sunsama. “Focus mode” dims the lights on the rest of your tasks for the day, “Daily planning” dims the lights on the rest of the week, and so on.


In a perfect world, transitioning from one task to another would be seamless. Before I even arrive, both the digital and physical environments perfectly arrange themselves, allowing me to dive right into work. Like a surgeon in an operating theater, I have my code editor open full-screen in its own window. The browser tab with the local build runs on the desktop to the left of the code editor. Another window opens with Sunsama and the Linear ticket I plan to address. My phone stays in do not disturb mode, and I close Slack. Instead, I spend five minutes clicking through windows, arranging screens, and hunting down the right tabs before I can start coding. However, when I enter “Focus mode” in Sunsama, I know my notifications will automatically pause, eliminating one source of distraction. More importantly, because it’s automated, I’m less likely to forget this step when I’m in a rush to get started.


When I first sat down to build this feature, my goal was to automate syncing my notification status in my chat tool with my actual “focus status” in Sunsama. As I started working on it, I realized we could also set a plain-text status and emoji, allowing colleagues to know what you were up to. Although I knew these features existed in Slack, I never used them because it’s unrealistic to open Slack multiple times a day just to update your status. In an actual office, coworkers see my headphones and either back away quietly or leave a note on Slack. Now, we can replicate that behavior in Slack or Teams without needing to remember to update your status. All you have to do is enter Focus Mode in Sunsama (with one keystroke) and we’re able to put digital headphones over your ears so your colleagues know to respect your focus.


As I was building this feature, I started going deeper into automating my digital decluttering and attempted to automatically set my iOS and OS X status to “Do not Disturb” whenever I entered “Focus mode” in Sunsama. Unfortunately, as of late 2024, the OS X and iOS APIs do not expose a safe and standard way for updating a user’s “Focus status” e.g. “Work”, “Sleep”, “Do not disturb” etc. If that ever changes, I’ll be stoked to automate one more little bit of my get focused ritual for myself and for everyone else ☺️


Until then, if you want to automatically turn off notifications from distracting chat apps while you are deep in focused work, you can do that with Sunsama. Here’s a guide on how to set this up[for Slack](https://help.sunsama.com/docs/slack-update-slack-when-focused-in-sunsama) and for[Microsoft Teams](https://help.sunsama.com/docs/teams-update-teams-when-focused-in-sunsama) .
