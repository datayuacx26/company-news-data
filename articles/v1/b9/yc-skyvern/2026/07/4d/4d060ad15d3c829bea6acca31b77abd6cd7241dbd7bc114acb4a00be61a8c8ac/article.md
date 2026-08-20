---
schema_version: "1.0.0"
document_id: "4d060ad15d3c829bea6acca31b77abd6cd7241dbd7bc114acb4a00be61a8c8ac"
company_key: "yc-skyvern"
company: "Skyvern"
source_id: "yc-skyvern-rss-4bc1426a1548"
canonical_url: "https://www.skyvern.com/blog/rustwright-playwright-rewritten-in-rust-that-occupies-70-less-memory-and-is-2-55x-faster/"
published_at: "2026-07-15T15:15:11+00:00"
first_seen_at: "2026-07-20T23:24:10.914365+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:40dcd6cf05ffefa74347ab666b067cd83c7e8387662820074d1bd5133e7ea71a"
---

# Rustwright: Playwright rewritten in rust that occupies 70% less memory (and is 2.55x faster)

Hey everyone, Suchintan from Skyvern (YC S23). We run browser automation for AI agents at scale, and we're launching[Rustwright](https://github.com/Skyvern-AI/rustwright?ref=skyvern.com) :[Playwright on an in-process Rust CDP engine.](https://github.com/Skyvern-AI/rustwright?ref=skyvern.com) The engine consumes 70% less memory than Playwright, and is 2.55x faster to boot.


**We built this to improve Skyvern’s memory footprint** , and thought this would be independently valuable to the broader community


The whole idea fits in two lines:


```text
playwright-python:
your code ──pipe──► Node driver ──CDP──► Chromium


rustwright:
your code ───────── raw CDP ───────────► Chromium


```


Removing the driver changes three things:


1. **Less abstraction.** The driver subprocess and its pipe speaks CDP straight to Chromium. In our runs, the new engine is 2.55× and **consumes 70% less memory** than the equivalent playwright process
2. **No Playwright driver fingerprint.** The driver never loads, so its signatures never appear: no` __playwright__binding__` globals, no` Runtime.enable` on the default path (the well-known console-serialization leak). The claim is deliberately narrow: no Playwright-specific automation fingerprint, not "undetectable"
3. **A single rust core** : a single Rust core backs both language bindings (Python with sync and async; Typescript) with plans to expand to many popular languages (Ruby / Go / Java to name a few). Within the supported surface, migration is often a one-line import change —` from rustwright.sync_api import sync_playwright.` Clicks and typing go through real CDP input events, not synthetic DOM calls, and cross-origin iframes auto-attach with` frame_locator()` routing across origins.


[playwright-python](https://github.com/microsoft/playwright-python?ref=skyvern.com) pipes every call through a bundled Node driver process.[Rustwright](https://github.com/Skyvern-AI/rustwright?ref=skyvern.com) drives Chromium over raw Chrome DevTools Protocol from a Rust core, an async CDP client on Tokio (WebSocket, plus opt-in Unix-pipe transport), exposed in-process through thin PyO3 bindings for Python and napi-rs bindings for Node.


Status: alpha, Chromium-only


**Want to give it a try? Change one line of code!**


```text
pip install rustwright


- from playwright.sync_api import sync_playwright
+ from rustwright.sync_api import sync_playwright


```


⭐ Let us know what you think:[https://github.com/Skyvern-AI/rustwright](https://github.com/Skyvern-AI/rustwright?ref=skyvern.com)
