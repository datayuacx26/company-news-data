---
schema_version: "1.0.0"
document_id: "1c3cb382427c62adaa9ccb477224cbb5d47b21238ebdc4c4c255a3374a1f54b6"
company_key: "yc-finta"
company: "Finta"
source_id: "yc-finta-news-import-8be56c12e8e6"
canonical_url: "https://www.finta.io/changelog/mercury-now-syncs-in-real-time"
published_at: "2026-02-23T12:00:00+00:00"
first_seen_at: "2026-07-25T04:54:26.845641+00:00"
fetched_at: "2026-07-28T22:19:22.533448+00:00"
content_hash: "sha256:5b33d4c23e0940d40b94dced2376c40c05e06d33a6643b2ced8c2abac8df0cd9"
---

# Mercury Now Syncs in Real Time

## Mercury Now Syncs in Real Time


Your Mercury data no longer waits for the next scheduled refresh. When Mercury processes a transaction or updates an account balance, Finta picks it up automatically and syncs it to your destinations right away.


## What's New


### Real-Time Transaction Sync


Every time Mercury creates or updates a transaction, Finta receives a webhook and syncs the change to your destinations automatically. No more waiting hours for new transactions to appear.


### Category Syncing


Mercury's transaction categories (like Software, Payroll, and Advertising) now sync to your destinations alongside your transactions. If Mercury recategorizes a transaction, Finta picks up the change on the next sync.


### Balance Updates in Real Time


Checking, savings, treasury, and credit account balances now update the moment Mercury sends a balance event, not just during daily syncs.


### Accurate Transaction Removal


Transactions that are cancelled, failed, reversed, or blocked are now properly removed from your destinations instead of staying behind.


## Existing Mercury Users


If you connected Mercury before this update, you'll see an **Enable Real-Time Sync** button on your Mercury connection card. Click it to re-authorize with Mercury and turn on webhooks. It only takes a moment, and your connection will start syncing in real time right away.


[Restore Archived Rules February 23, 2026](https://www.finta.io/changelog/restore-archived-rules)[Control Pending Transactions in Your Coda Pack January 25, 2026](https://www.finta.io/changelog/control-pending-transactions-in-your-coda-pack)
