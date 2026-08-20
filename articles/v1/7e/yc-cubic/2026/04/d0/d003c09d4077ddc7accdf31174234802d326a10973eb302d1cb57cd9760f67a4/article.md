---
schema_version: "1.0.0"
document_id: "d003c09d4077ddc7accdf31174234802d326a10973eb302d1cb57cd9760f67a4"
company_key: "yc-cubic"
company: "cubic"
source_id: "yc-cubic-news-import-612274238019"
canonical_url: "https://www.cubic.dev/blog/ultrareview-launch-week-03-day-3"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-23T06:53:38.364034+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:4aa9cf2d1723b8b6167a62c870e370d1517ef2f16eebd5478f34fdf9c8ab6183"
---

# cubic blog: Ultrareview | Launch week 03, Day 3

95% of PRs need fast, accurate feedback. The other 5% are complex, large or critical, and require the kind of deeper review you'd ask your CTO for.


Our review agents rank #1 on[Martian's Code Review benchmark](https://codereview.withmartian.com/?model=anthropic_claude-opus-4-5-20251101&mode=offline) : they're accurate and built for speed, with a cap on time and how many files can be reviewed per run. You get feedback in minutes, and that's the right tradeoff for most changes. But not for all of them.


That's why we came up with **ultrareview** . An ultrareview is a deeper review mode that can take twice as much time, and three times as many tokens, giving our agents time and resources to research, trace, and find more complex bugs, while still catching every issue a regular review would find. They're the same reviews you already know, on steroids.


We only use the best available models in the market for ultrareviews, and every Pro account gets 2 ultrareview runs included per seat per month.


**How to use**


After opening a PR, mention` @cubic-dev-ai ultrareview` in a new comment in the PR:


This will trigger the new ultrareview and stop any regular review already running. Ultrareviews can take up to 45 minutes to run, even though they typically finish in under 30 minutes.


**How is this different from codebase scans?**


A[codebase scan](https://docs.cubic.dev/codebase-scan/codebase-scans) is a deep analysis of your entire codebase, running for 12+ hours.


Ultrareviews sit in between standard PR reviews and codebase scans. They're deeper than a regular review, but less open-ended than a codebase scan (they're bound to the changes in your PR), with explicit execution limits (~45 minutes), and still fast enough to keep you shipping.


Available today on the cubic Pro plan.
