---
schema_version: "1.0.0"
document_id: "2c10d2d39a1c3659d428249ebd4d63d15c5d48e39249e261ac04df91c12f22ae"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-news-import-2df9936a897a"
canonical_url: "https://www.tweeks.io/changelog/2026-01-monthly-changelog"
published_at: "2026-02-02T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:29.125136+00:00"
fetched_at: "2026-07-28T22:22:47.419354+00:00"
content_hash: "sha256:9dc8e9eaf9cb2f9828d1465654d27027048322fe14d3db913e74eac56be7104c"
---

# Changelog: January 2026

January was the biggest month for Tweeks yet. There's a real code editor on the web now, with version history and diffs. Tweeks themselves can call AI models and send email. And when a request isn't possible, generation says so instead of handing you something broken.


## New


- **Web code editor** — Editing a tweek now happens on a dedicated editor page instead of a modal. It has full version history, a diff viewer for comparing versions, and script import. Saving keeps you where you are, and basic editing works on phones and tablets too.


- **Tweeks can use AI** — The new` TW_inference` API lets tweeks call AI models. A tweek can now summarize the article you're reading or rewrite text on the page. Available on Pro.
- **Tweeks can email you** —` TW_email` does the same for email: tweeks can now send you messages. Price alerts and daily digests are the obvious uses, but anything worth leaving the browser for works.
- **Generation that says no** — Tweeks now checks whether your request is actually feasible on the page before handing you a result. You get a success, a success with a warning about limitations, or a clear explanation of why it isn't possible. No more scripts that quietly do nothing.
- **Select multiple elements** — The visual element selector now supports up to five elements at once, so you can point at everything you want changed in one request.


- **Short share links** — Shared tweeks and profiles now use short` /t/` and` /u/` links that are easy to paste anywhere.
- **One-click web sign-in** — Already signed in to the extension? The website can now sign you in with one click, no email round-trip.
- **Rebuilt onboarding** — Onboarding now asks one question at a time, works with the keyboard, and remembers your progress if you step away.
- **Redesigned tweek menus** — Tweeks that add on-page menu commands, like the[X/Twitter article export button](https://www.tweeks.io/t/bc2926d27d5a4c6885dfd49c) that saves articles as Markdown or PDF, get a redesigned launcher you can drag anywhere, with theming that adapts to the site.
- **Report and related tweeks on share pages** — Shared tweek pages now show related tweeks and include a report button.


## Improved


- **Real-time generation progress** — The popup now streams live progress while your tweek is being generated.
- **Expandable prompts** — Long prompts on pending generations can be expanded to read in full.
- **Better link previews** — Shared tweeks and profiles get redesigned dark previews when posted in social and messaging apps.
- **Quieter suggestions** — Discovery suggestions now include a "Don't show this again" option.
- **Share pages and navigation** — Cleaner share page layout, a refined header, and clearer install counts across the site.


## Fixed


- **Mobile code editing** — Fixed the keyboard not appearing in the code editor on mobile devices.
- **Dismissed notifications reappearing** — Dismissed suggestions no longer come back after a page reload.
- **Back/forward navigation** — Tweeks now behave consistently with the browser's back/forward cache.


## Reliability


Sessions recover automatically when the browser suspends the extension in the background, so you stay signed in. And if your browser doesn't support the APIs Tweeks needs, the extension now tells you what's wrong up front.


## Extension Version Coverage


This changelog covers extension versions 0.0.4.5 through 0.0.6.0. All changes are cross-browser unless noted otherwise.
