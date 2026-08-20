---
schema_version: "1.0.0"
document_id: "c06bd4f75e8aac2d167f8ab0568af4d6f65d86ff972d0235d608f655938df31f"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-news-import-2df9936a897a"
canonical_url: "https://www.tweeks.io/changelog/2025-10-monthly-changelog"
published_at: "2025-11-03T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:29.125136+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:5adf61cbe68afdff87201d201eca6a91a0a8f9461fb4980cadfb3644d5f81b36"
---

# Changelog: September & October 2025

Welcome to the first Tweeks changelog. The extension launched in early October and a lot has shipped since. Here's what went out in the first six weeks.


## New


- **Create tweeks with AI** — Open the popup on any page, describe what you want changed in plain English, and Tweeks writes a script that does it. You can build something new or modify an existing tweek, and generation keeps running if you close the popup. Status indicators and a timer show how your generation is going.


- **Visual element selector** — Instead of describing where something is on the page, you can point at it. A crosshair button in the popup turns on an interactive picker with live highlighting. Click the element you want changed and Tweeks targets it directly. The selection sticks around even if the popup closes.


- **A full script manager** — Every tweek can be toggled, edited, searched, filtered, and sorted from the extension. The popup separates what's active on your current tab from the rest of your collection.
- **Built-in library** — The extension ships with a set of pre-built tweeks you can install in one click. If you edit one and regret it, you can revert to the library version.
- **Script sharing** — Share a tweek with a link. Anyone who opens it can see what it does and install it. Early favorites include[Hacker News Filter & Sort](https://www.tweeks.io/t/97e72c6de5c14906a1351abd) and a set of[UI cleanup controls for X.com](https://www.tweeks.io/t/1eed748ffbe74fce93c677ed) .
- **Interactive onboarding** — A guided setup walks you from install to your first working tweek. It detects the extension automatically, so you know right away whether setup worked.
- **Script guard** — If a script misbehaves and starts flooding a page with changes, Tweeks disables it automatically and tells you.
- **Feedback built in** — Rate generations and send us feedback from the popup.
- **Tweeks on mobile** — The website now works properly on phones.


## Improved


- **Generation input** — The create flow was redesigned. The prompt field is focused when the popup opens and remembers what you typed.
- **Notifications** — On-page toasts were redesigned to be less intrusive and easier to read.
- **Settings and library** — The settings page was rebuilt, and the popup library got a matching retheme with sorting, filter animations, and remembered state.
- **Update flow** — When modifying a tweek, a clear button lets you start over, and a hint points you back to your most recent modification.


## Fixed


- **Notifications on modern web apps** — Toasts now render correctly on React-based sites that previously swallowed them.
- **Popup state when switching tabs** — The popup now shows the right scripts for the tab you're actually on.
- **Your prompt survives errors** — If a generation fails, your input is preserved so you can retry without retyping.
- **Library tab reliability** — Fixed the library tab occasionally opening blank when clicking the extension badge.


## Extension Version Coverage


This changelog covers extension versions 0.0.1.0 through 0.0.2.2 on Chrome.
