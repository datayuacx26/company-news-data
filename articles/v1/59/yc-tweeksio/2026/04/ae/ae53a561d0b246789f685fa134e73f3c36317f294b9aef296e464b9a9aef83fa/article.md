---
schema_version: "1.0.0"
document_id: "ae53a561d0b246789f685fa134e73f3c36317f294b9aef296e464b9a9aef83fa"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-news-import-2df9936a897a"
canonical_url: "https://www.tweeks.io/changelog/2026-04-01-changelog"
published_at: "2026-04-02T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:29.125136+00:00"
fetched_at: "2026-07-28T22:16:03.530620+00:00"
content_hash: "sha256:6dd3cbb1679fe94d4f9ea5068c7a6e2693bbb074bd36ebefc93904ecdd9226ea"
---

# Changelog: March 16-31, 2026

The extension popup has been redesigned to streamline the interface and make create and modify flows clearer and easier to use. This update also adds protected-host controls, usage milestone surfaces in the Tweeks panel, and more reliable generation status updates across the extension. New in the catalog: a[chat history search for Arena AI](https://www.tweeks.io/t/EnQgrZRX) and a[1970s CRT terminal skin for Google Search](https://www.tweeks.io/t/8c8c0953f6984163922c4da7) .


## New


- **Redesigned popup and onboarding** — The popup now uses a more streamlined layout that feels less overwhelming and makes create and modify flows easier to distinguish, while onboarding has been updated to match the new experience.


- **Protected hosts with user overrides** — Tweeks now blocks protected sites by default and lets you explicitly allow them when you want Tweeks to run there.
- **Usage milestones and stats surfaces** — The Tweeks panel now shows usage milestone callouts and shareable stats surfaces so you can track how much you have customized the web.


## Improved


- **Faster popup startup** — The popup now opens more quickly without regressing state restoration or loading behavior.
- **Clearer generation progress** — The popup now stays on the active generation tab, and generation status messages are easier to follow while a tweek is being built.


## Fixed


- **More reliable generation toasts** — Completion, failure, and apply toasts now appear more consistently, stay aligned correctly, and no longer go stale after a page reload or disappear too early.
- **Element selector and settings stability** — The element selector no longer freezes when Tweeks settings are open and is less likely to get suppressed by other Tweeks surfaces.
- **Protected-host rules and panel behavior** — Protected-host registration is enforced more consistently, Tweeks settings clicks work again in the options page, and the panel behaves more reliably on video-heavy pages.
- **Installed tweek edit sync** — Opening an installed tweek in the web editor now stays in sync more reliably with the version you have locally.


## Reliability


- **More durable generation handling** — Background generation jobs and status updates are more resilient, reducing cases where longer-running generations could fall out of sync with the popup or panel.


## Extension Version Coverage


This changelog covers extension versions 0.0.6.10 through 0.0.6.13. All changes are cross-browser unless noted otherwise.
