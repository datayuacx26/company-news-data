---
schema_version: "1.0.0"
document_id: "ee45cecf5a7936102c74ee73966bcfb17846a80535ef2bc6a64a27fbbc41f9c9"
company_key: "yc-nunu-ai"
company: "nunu.ai"
source_id: "yc-nunu-ai-news-import-9ab4e6dc8961"
canonical_url: "https://nunu.ai/blog/pr-bot"
published_at: "2025-12-09T16:17:27.043+00:00"
first_seen_at: "2026-07-22T22:59:24.636969+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:6b23afe931e57fa676aeb40b6189d683d74d09198c301df8c857a227a5cdfa27"
---

# Introducing PR Testing Agents

Hi everyone!


AI coding tools like Cursor and Claude Code let you ship features faster than ever. Timelines that used to take weeks now take days. At this pace, it feels like you should be able to ship something new every day.


But in reality, that rarely happens. Manual QA can’t keep up, and plenty of bugs still slip into production.


That’s why we’re introducing **PR Testing Agents** . Directly inside your GitHub Pull Request, you can tag the nunu Testing Agent to validate your new feature. It performs true end-to-end testing on the real build. Unlike Codex or other PR bots, this isn’t code analysis - it interacts with your actual product.


## How does it work?


We deploy your build to a real device (Android or iPhone for apps, Windows VMs for desktop and web). The Agent connects on the OS level, reads the screen, and controls the device using primitive actions like taps, swipes, and clicks. Because it is fully vision-based, it experiences your app exactly like a human user would, ensuring real bugs don’t go unnoticed.


The Agent creates its own testing strategy. Your GitHub comment becomes a high-level instruction and it handles the rest on the fly.


If you want to give it more context about your app, you can attach extra materials like design docs, feature specs, or full documentation pages.


# Real Example


1. You open a Pull Request that adds support for profile picture uploads.


2. Your teammate drops a simple “LGTM,” and Codex gives it a thumbs-up.


3. To confirm it actually works on the build, you tag the nunu Testing Agent and let it run an E2E test right inside the PR.


4. The nunu testing agent performs the test and finds that the Picture is actually not displaying! BUG!


5. Yey, you have just prevented a buggy build from shipping!


This is a real example! See for yourself by checking out the[open-source demo repo](https://github.com/nunu-ai/minishop/pull/2) !


# Next Steps


Try it yourself! Get access[here](https://form.typeform.com/to/pDvj3Oxg) !


[<< back to blog](https://nunu.ai/blog)
