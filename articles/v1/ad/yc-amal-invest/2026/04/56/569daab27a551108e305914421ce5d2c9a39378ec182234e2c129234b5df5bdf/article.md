---
schema_version: "1.0.0"
document_id: "569daab27a551108e305914421ce5d2c9a39378ec182234e2c129234b5df5bdf"
company_key: "yc-amal-invest"
company: "Amal Invest"
source_id: "yc-amal-invest-news-import-cb58b37e0a38"
canonical_url: "https://amalinvest.com/changelog/2026-04"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-21T06:14:08.584317+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:b17186345c2cb8aeb9f3bdceed35c12ddfb76823bf9852239e75da6bbcafe747"
---

# Phantom positions and stuck orders

# Phantom positions and stuck orders


April 30, 2026


---


### Fixed


- Return of Capital distributions (a dividend flavor used by some ETFs and REITs) no longer break the activity feed. Dividends of every flavor now show up where they should
- Cancelled or already-filled scheduled orders no longer linger in the UI, and neither do dust positions worth less than a cent
- Fully liquidated positions no longer reappear as phantom rows with zero shares. We rewrote the duplicate asset-resolution path behind it. If a position is closed, it's gone
