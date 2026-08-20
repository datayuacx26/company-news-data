---
schema_version: "1.0.0"
document_id: "aa7a6ce4014e0786df13c58bf41b5afb49add88d590bbf2212aa736b6e3d1d8b"
company_key: "yc-finta"
company: "Finta"
source_id: "yc-finta-news-import-8be56c12e8e6"
canonical_url: "https://www.finta.io/changelog/introducing-storage-mode"
published_at: "2026-06-05T12:00:00+00:00"
first_seen_at: "2026-07-25T04:54:26.845641+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:f930207ca18ae33a3ee631214b9a625fef243f8b68d75859c61d5ca995fb2a4f"
---

# Introducing Storage Mode

**Finta can now keep a durable copy of your financial data — so your syncs stay resilient and your data is ready for the API and AI tools.**
By default, Finta runs in pass-through mode: when a sync runs, we fetch your data from your bank or payment provider and push it straight to your destination, without holding onto anything in between. Storage mode changes that. Turn it on and Finta keeps a synced copy of your accounts, transactions, balances, and investments on our end.


## What you can do


- **Keep syncing through bank errors** — when a connection has an issue or a provider has an outage, your last-known data is already stored, so your destinations stay up to date.
- **Unlock programmatic and AI access** — storage mode is the foundation for the Finta API and the new[Finta MCP](https://www.finta.io/changelog/finta-mcp-is-in-beta) , which let tools and AI assistants read your data instantly.
- **Watch your backfill in real time** — flip it on and see each bank connection fill in, connection by connection.
- **Stay in control** — turn it off whenever you want. When you do, we hard-delete every stored record. No soft deletes, no leftovers.


## Get started


Turn on storage mode from **Settings → Data** . Finta starts pulling in your history right away, and once the backfill finishes you're fully set up.
Read the[storage mode guide](https://docs.finta.io/getting-started/storage-mode) for exactly what Finta stores and answers to common questions.


[Finta MCP is in Beta June 12, 2026](https://www.finta.io/changelog/finta-mcp-is-in-beta)[Restore Archived Rules February 23, 2026](https://www.finta.io/changelog/restore-archived-rules)
