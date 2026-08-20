---
schema_version: "1.0.0"
document_id: "1581e84eb04f75a2797a98035b5ffc0c28d690d81080ee8267e116267856fa7e"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-news-import-2df9936a897a"
canonical_url: "https://www.tweeks.io/changelog/2026-03-15-changelog"
published_at: "2026-03-17T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:29.125136+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:1c0884fe24104c8b4487a7ed772d88e82afb0b87061b2bf014ee3a04ce7d8317"
---

# Changelog: March 1-15, 2026

The Tweeks panel now has a full in-page settings experience with host access controls, the extension gains dark mode, and generation toasts now show you what was built. This update also adds translation support in the web editor and a round of reliability and UX fixes across installs, navigation, and onboarding. Meanwhile two community tweeks are taking off: the[Universal AI Slop Finder](https://www.tweeks.io/t/PUjj1S2F) and an exporter that[turns ChatGPT, Claude, and Gemini chats into Markdown](https://www.tweeks.io/t/ab46eaf4412c4575a692d791) .


## New


- **Tweeks panel settings** — The Tweeks panel now includes a full in-page settings experience with appearance modes, host access controls, and a configurable grabber shortcut.


- **Dark mode for the extension popup** — The popup now supports dark mode, matching your system preference. A theme override setting lets you lock it to light or dark regardless of your OS setting.


- **Expandable generation details** — After a tweek is generated, you can expand its completion message to see a summary of what was built and jump directly to the code.


- **Tweek translation workflow** — The web editor now includes a translation workflow for tweeks, with a button to automatically generate translations for supported languages.


## Improved


- **Smoother panel navigation** — The Tweeks panel no longer bounces or re-animates when navigating between pages on the same site.
- **Smarter discovery dismissals** — Dismissed discovery scripts are now remembered per-script, so you won't see the same suggestions again.
- **Panel handle hides during fullscreen video** — The panel handle automatically hides when you enter fullscreen video so it doesn't obstruct content.
- **Profile actions open in the dashboard** — "Open Profile" and edit actions in the extension now take you directly to the web dashboard.
- **Better menu command generation** — Tweeks now gives the generator clearer guidance for stateful script menu commands, reducing cases where labels fall out of sync after state changes.
- **Localized Chrome Web Store listing** — The Chrome Web Store listing is now localized across 11 languages, including screenshots, promo tiles, and extension metadata.


## Fixed


- **Extension detection during installs** — Fixed extension readiness and install-state races that could make "Install" buttons appear stuck or unavailable across share pages, onboarding, and the dashboard.
- **Installing shared tweeks with drafts** — Shared tweeks that have in-progress edits now install the published version correctly.
- **Dashboard tab loading** — Fixed an issue where switching tabs in the dashboard could show empty content on first render.
- **Post-login redirect** — Signing in no longer occasionally lands you on the wrong page.
- **Grabber side preference** — The grabber now correctly remembers which side you placed it on after restarting the browser.
- **Toast readability** — Fixed contrast issues on toast notifications that could make text hard to read on certain sites.
- **Panel animation glitch** — Fixed a visual glitch where the panel could briefly bounce when badge animations played.


## Extension Version Coverage


This changelog covers extension versions 0.0.6.4 through 0.0.6.9. All changes are cross-browser unless noted otherwise.
