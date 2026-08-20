---
schema_version: "1.0.0"
document_id: "8bfbd1165412e168c9663c0d73fbb6a3de13d3fa7f5b52301d58ef3475cb63c9"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/file-explorer"
published_at: "2025-06-06T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:dbda418bb88646cc6171eb52076b52e9422a78953f991c2713e350fad57eba40"
---

# File explorer

We often heard from users that they wanted a file explorer built into Sourcebot - so we built one! Checkout the release[here](https://github.com/sourcebot-dev/sourcebot/releases/tag/v4.2.0) . Here are some highlights:


### Fast 🏎️


We optimized our implementation for responsiveness so you can jump between files as fast as you would in an IDE (or faster!).


### Collapsible ⬅️


When you don't need it, you can collapse it using the button or with` crtl` /` cmd` +` b` .


### Breadcrumbs 🍞


We added breadcrumbs to file paths, allowing you to click into folders and view their contents. This works everywhere a file path is surfaced in Sourcebot (e.g., explore menu, search results, etc.).


### Icons ✨


We use the same icon set as[vscode-icons](https://github.com/vscode-icons/vscode-icons) to make things sparkle ✨.


If you are interested in learning more, take a look at the[release notes](https://github.com/sourcebot-dev/sourcebot/releases/tag/v4.2.0) .


## Other changes & fixes


-


Add separate login / signup screens[#311](https://github.com/sourcebot-dev/sourcebot/pull/331)


-


Adds support for encrypted license keys[#355](https://github.com/sourcebot-dev/sourcebot/pull/335)


-


Fix repo images in authed instance case[#322](https://github.com/sourcebot-dev/sourcebot/pull/332)


-


Added manifest.json in and add manifest json[#322](https://github.com/sourcebot-dev/sourcebot/pull/332)


-


Fixed bug with repos being in index state after clearing the Sourcebot cache[#339](https://github.com/sourcebot-dev/sourcebot/pull/339)


-


Added hover tooltip for long repo names in filter panel[#338](https://github.com/sourcebot-dev/sourcebot/pull/338)


-


Fix symbol hover popover clipping issue[#326](https://github.com/sourcebot-dev/sourcebot/pull/326)


-


Improve symbol reference/definition list perf[#327](https://github.com/sourcebot-dev/sourcebot/pull/327)


-


Add copy file button by @drew-u410[#328](https://github.com/sourcebot-dev/sourcebot/pull/328)


-


Add support for GCP IAP JIT account provisioning[#330](https://github.com/sourcebot-dev/sourcebot/pull/330)


-


Add keyboard shortcuts for goto def & find all refs[#329](https://github.com/sourcebot-dev/sourcebot/pull/329)


-


Refactored docs for readability[#322](https://github.com/sourcebot-dev/sourcebot/pull/322)


-


Add support for structured logs[#323](https://github.com/sourcebot-dev/sourcebot/pull/323)


-


Fix "Mark decorations may not be empty" issue[#325](https://github.com/sourcebot-dev/sourcebot/pull/325)


-


Fixed issue with how entitlements are resolved for cloud.[#319](https://github.com/sourcebot-dev/sourcebot/pull/319)


## Community shoutouts


- [@drew-u410](https://github.com/drew-u410) for adding a copy file button in[#328](https://github.com/sourcebot-dev/sourcebot/pull/328)
