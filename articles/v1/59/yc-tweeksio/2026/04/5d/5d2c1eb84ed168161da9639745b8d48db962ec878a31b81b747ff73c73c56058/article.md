---
schema_version: "1.0.0"
document_id: "5d2c1eb84ed168161da9639745b8d48db962ec878a31b81b747ff73c73c56058"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-news-import-2df9936a897a"
canonical_url: "https://www.tweeks.io/changelog/2026-04-15-changelog"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:29.125136+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:9d88d0cc431509deb4af17b66987ce1d2af291f986866058cd08cea9824060c8"
---

# Changelog: April 1-15, 2026

This release adds Tweeks MCP, a guided way to connect supported AI tools to the Tweeks extension for browser automation and tweek management. It also launches a[dedicated docs hub](https://www.tweeks.io/docs) for Tweeks Engine with API and permissions reference, while tightening popup search, theme persistence, generation view behavior, and panel mounting across the extension. If AI-generated comments bug you, the community has you covered twice over: an[AI comment highlighter for YouTube](https://www.tweeks.io/t/H0cEkxBL) and an[AI slop comment finder for Reddit](https://www.tweeks.io/t/EbL7NBYO) .


## New


- **Tweeks MCP with guided setup** — Tweeks now includes MCP integration through` @tweeks/mcp` , a local bridge that connects supported AI tools to the Tweeks extension for browser automation and tweek management. Setup can detect installed tools, write MCP config, install the local bridge, and open the activation step automatically, with manual activation links as a fallback.


- Supported tools: Claude Desktop, Claude Code, Cursor, VS Code, Windsurf, Codex, Zed, and OpenCode.
- Permissions model: MCP starts in read-only mode so agents can inspect tabs and page content before you explicitly allow full control for navigation, clicks, form fills, JavaScript, and persistent script changes.


- **Docs and Tweeks Engine API reference** — The website now includes a[dedicated docs hub](https://www.tweeks.io/docs) for Tweeks Engine with browsable reference for APIs like` TW_inference` ,` TW_email` , context menus, and Tweeks notifications. The docs also include a permissions tab, anchor-linked sections, and downloadable` .md` and` .json` exports for people building around Tweeks.


## Improved


- **Clearer onboarding create and modify guidance** — The onboarding flow now does a better job explaining when to create a tweek from scratch and when to modify an existing one.
- **Redesigned uninstall page** — The uninstall flow now makes it easier to leave feedback and understand what to do next.


## Fixed


- **Popup search, OTP, and theme persistence** — Fixed search layout inconsistencies in the popup, improved OTP persistence during sign-in, and made theme choices stay more consistent between the popup and panel settings.
- **Tweeks panel mounting on hydration-heavy sites** — The panel now mounts more reliably on hydration-heavy sites.
- **Generation view open state** — The generations view now opens and stays in the expected state more consistently.


## Extension Version Coverage


This changelog covers extension versions 0.0.6.14 through 0.0.6.15. All changes are cross-browser unless noted otherwise.
