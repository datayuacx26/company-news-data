---
schema_version: "1.0.0"
document_id: "937394278063b00e887f03a2f78d7d8170ac851c7a26df21b0cc98a908e3b0e4"
company_key: "yc-quicknode"
company: "Quicknode"
source_id: "yc-quicknode-news-import-d0b2f3935987"
canonical_url: "https://www.quicknode.com/blog/blockbook-rest-is-live-migrate-from-self-hosted-blockbook-with-zero-code-changes"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-25T20:23:53.195520+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:7959c470f9b5aa7087f860b225f0feab63b44e31f65a90b4d6a55436d463c116"
---

# Blockbook REST is Live on Quicknode | Quicknode

#


Blockbook REST is live. Migrate from self-hosted Blockbook with zero code changes.


[Quicknode](https://www.quicknode.com/blog/author/quicknode)


June 23, 2026 — 3 min read


You have a Quicknode endpoint. For most workloads, that is enough.


But address-level data is different. Transaction history by address, balances across many addresses, deposit confirmation, UTXO lookups. None of that is what raw RPC was built to serve.


Wallet and payment teams run into this consistently. Balance queries are not possible through native RPC methods. The workaround works. Repeated calls, block scanning, a custom database, UTXO tracking logic. But it compounds. Engineering cost that grows with your product and never returns.


Blockbook indexes addresses so your app does not have to reconstruct them from raw blocks.


---


## **Skip the custom indexer**


Blockbook is a blockchain indexer originally built by Trezor for wallet infrastructure. Quicknode runs it as a managed add-on across six chains: Bitcoin, Bitcoin Cash, Dogecoin, Litecoin, Zcash, and Ethereum.


One API call returns full transaction history, current balance, totals received and sent, and unconfirmed activity. Supports addresses and xpub, so you can query an entire wallet from a single descriptor. It attaches to the Quicknode endpoint you already use. No custom indexer, no extra infrastructure, nothing to maintain.


New to Blockbook?
