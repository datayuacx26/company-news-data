---
schema_version: "1.0.0"
document_id: "bc8fce1274cb589dfa7dc4cba531274896424cbd12ec8d06c742bd40a087bf3f"
company_key: "yc-litellm"
company: "LiteLLM"
source_id: "yc-litellm-news-import-8bfeaefbc2ad"
canonical_url: "https://docs.litellm.ai/blog/version-support"
published_at: "2026-06-20T00:00:00+00:00"
first_seen_at: "2026-07-22T02:33:29.495129+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:3a7eca60082e6f4cf31042c31719af65b790bbca6add8a32b1f877246fbbe908"
---

# LiteLLM version support: focusing on the four most recent stable lines

*Starting Monday, June 29, 2026, LiteLLM will only actively support the four most recent stable minor lines. Here's what's changing and what it means for you.*


## Why we're doing this​


Maintaining older lines means carrying every fix back to keep them all in parity. That overhead grows with the number of lines we keep alive, not the number of fixes we make. Our focus is ensuring the most up-to-date product offerings are stable and working for you. Because of this, LiteLLM is focusing on the four most recent stable minor lines going forward.


## How the rolling window works​


This shift in focus takes effect Monday, June 29, 2026.


A minor line is a release series written as 1.89.x, covering every patch in it: 1.89.0, 1.89.1, 1.89.2, and any later ones. We support the four most recent lines and every patch inside each of them.


Today the four supported lines are **1.89.x, 1.88.x, 1.87.x, and 1.86.x** . Everything **1.85.x and earlier** has reached end of life and will no longer actively receive updates. The window rolls forward: when 1.90.x ships, 1.86.x rolls out and the supported set becomes 1.90.x, 1.89.x, 1.88.x, and 1.87.x. With a new line about every week, that works out to roughly a month of coverage per line.


## What this means for you​


To stay supported, pin to a line and take its patches, then move up before it ages out. Patching within a line is a drop-in; moving up a line is where you'd check the release notes for changes. Enterprise customers who need longer coverage can reach out, and for rare high-severity issues we'll use our judgment and may patch outside the window.


## How to stay current​


The best way to stay up to date on these changes is to bookmark our[release notes](https://docs.litellm.ai/release_notes) . We update it as new versions ship, so you can see the latest stable line and the three behind it that are still supported.
