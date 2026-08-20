---
schema_version: "1.0.0"
document_id: "294205ef43cde2780e662d97c5e3287a3d93858acf5364d55260e639c20b6401"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/product/fireblocks"
published_at: "2026-05-14T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:f6a2bc968e018a43c027ec4a42261bf0d56b9776ba3aff213fccd5a9e9fd8f97"
---

# New Connectivity with Fireblocks

If you're running a fintech that uses Formance as its core ledger, every fiat movement is already tracked: bank transfers, payouts, internal flows. But if you also custody digital assets on Fireblocks, that part of your treasury lives outside the ledger, reconciled manually, invisible until someone exports a CSV.


The Fireblocks Connector closes that gap. It pulls vault accounts, balances, and transactions from your Fireblocks workspace into Formance, so your digital asset positions feed into your ledger and reconcile the same way your fiat flows already do.


## What it Does


The connector syncs three things from your Fireblocks workspace into Formance on a configurable interval.


- **Accounts.** Every vault account becomes a Formance account, keyed by the vault account ID and carrying the original name. The per-asset positions inside a vault account aren't separate accounts. They show up through the balance layer.
- **Balances.** Vault-level balances for every asset Fireblocks supports: 1,500+ tokens across 80+ blockchains, plus fiat positions. The connector pulls the full asset catalog on install, refreshes it daily, and resolves the correct decimal precision automatically. Add a new asset to a vault tomorrow and it appears on the next sync.
- **Transactions.** Pulled from the Fireblocks transactions endpoint and classified by direction: external→vault as a payin, vault→external as a payout, vault→vault as an internal transfer. Fireblocks' richer states (pending signature, broadcasting, confirming, completed, blocked, rejected) collapse into Formance's status model, with the original sub-status preserved as metadata. Each transaction carries its Fireblocks ID, the on-chain hash, network fee, and note where present.


This is a read-only connector. It syncs data from Fireblocks into Formance and does not initiate transfers or sign transactions on the Fireblocks side.


## Why This Matters for Reconciliation


Take a concrete scenario. A company holds a USDC reserve on Fireblocks for cross-border settlements, while processing bank transfers through its banking partners and open banking connections. At the end of each day, the treasury team needs to reconcile:


- Stablecoins deposited to Fireblocks against fiat debited from their bank accounts
- USDC sent to partners against the corresponding invoices in their system
- Internal transfers between vault accounts against the operational movements they were meant to record


Without the connector, that means exporting CSVs from Fireblocks, reformatting them, and manually cross-referencing against the ledger. Every day.


With the connector, Fireblocks data flows into Formance automatically. From there it feeds into your ledger and reconciliation happens in a single pass. Discrepancies surface on their own. The CSV ritual disappears.


## Vault Accounts as Operational Units


Institutional crypto custody isn't one bucket. Fireblocks structures assets through vault accounts: logical groupings that hold balances across multiple assets and networks. Teams typically separate operational concerns this way, with a Treasury vault, an Operations vault, and a Customer Custody vault.


The connector preserves this separation. Each vault account becomes its own Formance account, with per-asset positions as balance lines underneath. Internal transfers carry both source and destination references. If you manage multiple workspaces (one per legal entity or region, say), you create one connector instance per workspace.


## Self-custody and your ledger


Fireblocks is built around MPC-based self-custody. Private keys are split across multiple parties so no single point of compromise can sign a transaction. Banks, regulated fintechs, and asset managers choose this model because they need to demonstrate key control to auditors and regulators. When the auditor asks for the link between on-chain positions and the books, the answer should come from one system, not a manual stitch-job across vault reports and ledger exports.


## Digital assets as financial infrastructure


Stablecoins are now used as a settlement and treasury rail, and more institutions hold ETH, BTC, and SOL as part of their treasury strategy rather than as speculative positions. The operational infrastructure around digital assets has lagged, though. Most teams still track these flows in separate tools and struggle to produce a unified view across fiat and crypto.


Formance Ledger handles this natively. Tracking crypto alongside fiat isn't a special mode. It's the same model the ledger uses for everything. Assets are typed identifiers (` USD/2` ,` USDC/6` ,` ETH/18` ,` BTC/8` ), and double-entry rules apply uniformly across them. The ledger refuses to cross asset boundaries without an explicit conversion. USDC arrives as USDC, ETH as ETH, USD as USD. Each lives in its own namespace, with no base-currency flattening and no separate ledger for crypto. The Fireblocks Connector extends that to one of the most widely used institutional custody platforms.


## Where it fits


The Fireblocks Connector joins 18 other connectors in Formance's Connectivity module, which spans banking, digital asset custody, and payment processor connectivity. It sits alongside our existing crypto-side connectors, broadening the set of custody platforms you can sync into Formance. Many institutional teams already run on Fireblocks.


Every connector uses the same account model and transaction format, feeding into your ledger through the same flow whether the data comes from a bank, a payment processor, or a custody platform. That's how you end up matching USDC settlements against bank debits, tracking ETH staking rewards alongside interest income, or consolidating a treasury that spans several providers, all in one place.


## Availability


The Fireblocks Connector is available now and requires Connectivity module v3.2.0 or later. Setup takes a few minutes. You'll need an API user and signing key from your Fireblocks console with read permissions, scoped to the workspace you want to sync.


Read the[Fireblocks Connector docs](https://docs.formance.com/modules/connectivity/psp/connectors/fireblocks) to learn more.


## Learn more about Fireblocks


Fireblocks is a digital asset infrastructure platform built around MPC-based self-custody, used by banks, fintechs, exchanges, and asset managers to securely move, store, and tokenize digital assets. For more details visit the[Fireblocks website](https://www.fireblocks.com/) .
