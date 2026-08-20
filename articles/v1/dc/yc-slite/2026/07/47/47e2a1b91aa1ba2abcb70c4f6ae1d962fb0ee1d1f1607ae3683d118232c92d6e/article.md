---
schema_version: "1.0.0"
document_id: "47e2a1b91aa1ba2abcb70c4f6ae1d962fb0ee1d1f1607ae3683d118232c92d6e"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/desktop-tabs-side-by-side-windows"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:16fe02fa3a517ddb9656c01c54533961c1d4c2579af3e15d7c245563bda3a99a"
---

# Desktop tabs & side-by-side windows

Full tab support has landed in the desktop app, matching the browser experience. You can open docs in new tabs and reorder them like in Chrome, command-click a doc in search or the sidebar to open it in a new tab, or drag tabs out to work side by side in a focused, sidebar-free view. On Windows, native window controls replace the old custom ones, and the app now supports Windows Snap layouts.


### Open the Slite Agent instantly (` Alt+Space` )


A global shortcut (Alt+Space by default, customizable) opens the Agent from anywhere on your desktop and pre-fills your query in a new tab.


### Custom Desktop App Icons


On desktop, choose from 6 animated custom app icons (Classic, Dust, Sunset, Agent, Agent Dust, Agent Sunset) in your profile settings, they work for macOS and Windows.


### Auto-Sync for Asana


The Slite Agent now checks Asana on a regular cadence and automatically folds new or updated projects straight into your knowledge base, so your docs stay current without manual upkeep.[Learn more →](https://slite.slite.page/p/8keWplcR5kcO5z/Asana)


### New Slite onboarding


The onboarding experience is now shown to every new team member joining, not just the workspace creator, so new joiners get a guided experience from day one featuring brand-aligned styling, animations, and graphics introducing Agent features.


### Improvements


- Sketches and diagrams rendered in Excalidraw now inherit and adapt nicely to dark and light mode themes.
- Enabled tax_id_collection inside both subscription and credit top-up Stripe checkout windows.
- Standard, Basic, and Premium workspaces trialing Pro will fall back to their paid plan upon trial expiration rather than hard-downgrading to Free.
- Convert raw Block Kit Slack messages to markdown, resolving user IDs to display names in digests.
- Allow public API note updates with compact and hybrid sliteml format.


### Fixes


- Fixed memory caching mapping bugs on protected/read-only tables where comments would save but render orphaned in the sidebar.
- Fixed rendering exceptions on approval cards that contained list elements.
- Synchronized streaming deserialize to prevent empty panels during generation and stripped code fences in the Improve with AI panel.
- Fixed 404/not-found bugs when exporting extremely large notes to PDF.
- Escaped note titles and linked metadata in crawler responses to block potential injection.
- Fixed the top-screen dropdown history loader to consistently fetch and reload recent conversational threads.
- Resolved composed-key character overrides (⌥S → 'ß') breaking hotkey detection on macOS shortcut inputs.
- Fixed editor bug where deleting a line failed to reset sequence numbers.
- Cleaned up the legacy business vs individual billing pop-ups from app settings.
- Cleaned up outdated product descriptions across the help center.
