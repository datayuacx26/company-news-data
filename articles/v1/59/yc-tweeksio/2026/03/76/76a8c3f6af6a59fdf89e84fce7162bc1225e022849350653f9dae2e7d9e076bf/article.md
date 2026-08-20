---
schema_version: "1.0.0"
document_id: "76a8c3f6af6a59fdf89e84fce7162bc1225e022849350653f9dae2e7d9e076bf"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-news-import-2df9936a897a"
canonical_url: "https://www.tweeks.io/changelog/2026-02-monthly-changelog"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:29.125136+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:55569105862f687a6c8fe052aa2131d511bd6abccf01c6422d9108f401d80998"
---

# Changelog: February 2026

February was a big month for Tweeks. The extension launched on Firefox, bringing the full userscript experience to a second browser. Alongside that, many new features shipped across the extension, library, and web app including bundles, Google OAuth, a profile dashboard, and a redesigned onboarding flow.


## New


- **Firefox support** — Tweeks is[now available on Firefox](https://addons.mozilla.org/en-US/firefox/addon/tweeks/) . Installation, script management, and AI generation all work across Chrome and Firefox.
- **Tweeks panel** — A persistent, edge-docked panel replaces the old discovery toast. It stays accessible as you browse, displays your active tweeks sorted by priority, and can be collapsed or expanded on demand.


- **Bundles** — Tweeks can now be grouped into collections and bundles can be shared or installed as a set.
- **Profile Dashboard** — A comprehensive dashboard for reviewing and managing your tweeks.


- **Google OAuth** — You can now sign in to the Tweeks website with Google in addition to email.
- **Library search and filter** — The Library tab in the extension popup now has a search bar and filters, making it faster to find a specific installed script.
- **Claim local scripts for web editing** — Scripts installed locally through the extension can now be claimed and opened directly in the web editor without leaving your current page.
- **Right-click menu editing** — Tweeks can now add items to the right click menu on webpages.


## Improved


- **Onboarding** — The[onboarding flow](https://tweeks.io/onboarding) has been redesigned with a tweek selection step, making it easier to get started with scripts relevant to the sites you already use.
- **Discover page** — Site filtering is more precise, scripts are sorted by install count, and tweeks that run on all sites are now visible in results. Popular picks like[Remove Shorts on YouTube](https://www.tweeks.io/t/bcd8bc32b8034b79a78a8564) now surface right at the top.
- **Popup tab memory** — The popup remembers whether you last had the Create or Library tab open and returns to it on the next open.
- **Blog** — Tweeks[now has a blog](https://tweeks.io/blog) .
- **Browser-aware install links** — The website detects your browser and links directly to the Chrome Web Store or Firefox Add-ons depending on what you're using.
- **Shared tweek previews** — Shared scripts now consistently include images when links are previewed in social apps and messaging.
- **Discover navigation state** — Discover now preserves your active filters, search terms, and page position when you visit a tweek detail page and navigate back, so you pick up right where you left off.
- **Script version labels in the editor** — The version history and version selector dropdowns in the web editor now display the script's` @version` tag (e.g., v1.2.3) alongside timestamps, making it easier to identify specific revisions.


## Fixed


- **Firefox: AI generation reliability** *(Firefox only)* — Firefox could suspend the extension's background process during generation, causing silent failures. Generation now stays active through completion.
- **Popup status sequencing** — The popup could briefly show a completed state before generation had started. The status display now follows the correct order.
- **Discover page loading** — An error that could prevent the discover page from loading under high query volume has been resolved.
- **Repeated discovery notifications on same-domain navigation** — The Tweeks panel no longer re-shows the discovery notification when navigating between pages on the same domain within a tab. It reappears if the set of available scripts changes or you move to a different site.


## Reliability


Database queries serving shared scripts and tweek detail pages have been optimized, reducing load times.


## Extension Version Coverage


This changelog covers extension versions 0.0.6.0 - 0.0.6.3. All changes are cross-browser unless noted otherwise.
