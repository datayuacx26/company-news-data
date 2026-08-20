---
schema_version: "1.0.0"
document_id: "4c463fa3ce7fba7751193ec0f1059e179d3284dc56b30d0f7e618cb3d3b9677e"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-news-import-2df9936a897a"
canonical_url: "https://www.tweeks.io/changelog/2025-12-monthly-changelog"
published_at: "2026-01-06T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:29.125136+00:00"
fetched_at: "2026-07-28T22:24:12.939989+00:00"
content_hash: "sha256:002576fa00d1c71c9f56d112ebd6a52e3b99cf07b47fec2b13d8924d03dff810"
---

# Changelog: December 2025

Public profiles got real usernames this month, updating a tweek now shows you its full prompt history, and scripts can be disabled for individual sites. Under the hood, we overhauled the extension's storage, which you'll notice as fewer hiccups on long browsing sessions.


## New


- **Usernames** — Public profiles now use a friendly username instead of a random code, so your profile link is something you can actually say out loud.
- **Prompt history viewer** — When updating a tweek, a View History option shows every prompt used to create and refine it. Useful when you can't remember what you asked for last time.


- **Per-site disabling** — You can now turn a script off for a specific site without disabling it everywhere. Good for tweeks that run on all sites but get in the way on one.
- **Profile sorting** — Visitors to your public profile can now sort your tweeks.


## Improved


- **Faster generation starts** — Page capture now transfers in parallel and retries on its own, so generations begin sooner and fail less often on large pages.
- **Library targeting** — The Library tab makes it clearer which tweeks run where, which helps once you've collected site tweeks like[X.com Focus Mode Sidebar Hider](https://www.tweeks.io/t/11cc7c3dad484bc899ca9c67) and the[X Feed Personalization Panel](https://www.tweeks.io/t/619f4b7f8989448c88c7b32c) .
- **Discover knows when you're set up** — The Discover page hides install prompts once it detects the extension, and dismissed suggestions stay quiet for longer.
- **Smoother sign-in** — The sign-in page redirects you automatically when you're already signed in.
- **Strict sites** — Generation quality improved on sites with strict security policies, like LinkedIn.


## Fixed


- **Clipboard actions** — Tweeks that copy to the clipboard now work reliably across sites.
- **Rare crashes on certain sites** — Fixed crashes that could occur on a small number of sites.
- **Large page timeouts** — Generations on very large pages no longer time out during page capture.


## Reliability


We rebuilt the extension's storage layer this month. Scripts and settings now hold up better through browser restarts and long idle periods, and userscripts are better isolated from the pages they run on.


## Extension Version Coverage


This changelog covers extension versions 0.0.3.2 through 0.0.4.3. All changes are cross-browser unless noted otherwise.
