---
schema_version: "1.0.0"
document_id: "bcd0e2d56db73f73c278f2e7911543c4e026eccc9bce267844d5c4cf95b78373"
company_key: "yc-amal-invest"
company: "Amal Invest"
source_id: "yc-amal-invest-news-import-cb58b37e0a38"
canonical_url: "https://amalinvest.com/changelog/2026-05"
published_at: "2026-05-23T00:00:00+00:00"
first_seen_at: "2026-07-21T06:14:08.584317+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:b89e5f2ef5d672e5d7e8d1be5b2aaf17e1a65d2ca3572cfda9115aa059a0d8a8"
---

# A new trading engine

# A new trading engine


May 23, 2026


---


### New


#### Chart timeframes


Portfolio and per-fund charts now have 3M, 6M, 1Y, and 2Y selectors. Previously everything was locked to 3 months. Ranges are cached in your session, so switching is instant, and the default view got slightly faster too.


#### Export fund history as CSV


From any fund detail dropdown: invested amount vs portfolio value, sampled twice a month from inception, with gain and loss per row. Uses the same engine that handles splits, mergers, and dividends, so the numbers match the app. Requested by Atif.


#### Dashboard access with a disconnected broker


If you disconnect your broker, you can still see your historical positions and realized P&L (useful for Zakat) instead of getting bounced to onboarding. A banner explains what you're looking at and how to reconnect.


### Improved


#### Trading lifecycle v2


"What does this user own and what did they pay for it" used to be answered by replaying every closed broker order on every page load. Fragile: one missed fill and the numbers could drift. That's now four separate records: what you asked for, what we sent to the broker, what actually filled, and what you hold. The dashboard reads positions directly, portfolio operations are locked so background jobs can't fight over the same fund, and every dollar is auditable end to end.


Also under the hood: the dashboard moved to a new UI stack, Node 24, and Turbopack production builds.


### Fixed


A rewrite this size caused some real bumps after the May 9 deploy. All fixed and reconciled:


- Portfolio order fills weren't being recorded for about 65 users between May 9 and 11, so dashboards showed $0 while the broker had actually filled the orders. We paused the queue, shipped a fallback for the fill id missing from Alpaca's orders response, and backfilled everyone affected
- Pre-v2 individual stock buys weren't migrated forward and disappeared from the Stocks list. Backfilled
- A reconciliation worker could hang for over 24 hours on a single Alpaca call. Now bounded by a 60 second timeout
- Automatic liquidations now require a database-backed authorization record before any sell goes out, so stale queue jobs can't re-trigger them
- The withdraw keyboard now shows up on mobile Safari, and the amount step no longer steals focus from the dialog title
- Dust rows with near-zero quantity no longer pollute filtered holdings, warrants no longer create duplicate preview orders, and deleted slices are hidden
