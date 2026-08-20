---
schema_version: "1.0.0"
document_id: "6c9a5917f4e4896d9a4c0a953b50a8c579b769347f5601967e7e6cfa43493d59"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/product/coinbase-prime"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:eaefd33fd248b3c928f92c8a59a5fad80d9e7d8ecdeac80fd67abbb790211e26"
---

# New Connectivity with Coinbase Prime

The Coinbase Prime Connector closes the gap between your digital assets and the rest of your treasury. It pulls wallets, balances, and transactions from your Coinbase Prime portfolios into Formance so your digital asset positions feed into your ledger and reconcile the same as your fiat flows.


## What it Does


The connector syncs three things from your Coinbase Prime portfolio into Formance on a configurable interval:


- **Accounts.** Every wallet in your portfolio becomes an account in Formance Ledger. Coinbase Prime distinguishes between vault wallets (cold storage) and trading wallets (active positions), and that distinction is preserved so you can query and filter on it downstream. The portfolio itself also appears as an account, carrying the aggregate balances.
- **Balances.** Portfolio-level balances across all asset types, including stablecoins like USDC and USDT, digital assets like ETH, BTC, and SOL, and fiat currencies like USD. Amounts are normalized into the same monetary notation used for your bank balances, making them directly comparable.
- **Transactions.** Withdrawals, deposits, and conversions are imported with their direction and status. Each transaction carries its original Coinbase Prime reference so you can always trace back to the source.


This is a read-only connector. It syncs data from Coinbase Prime into Formance, it does not initiate transfers or payouts on the Coinbase side.


## Why This Matters for Reconciliation


Consider a concrete scenario. A company holds a USDC reserve on Coinbase Prime for cross-border settlements, while processing bank transfers through its banking partners and open banking connections. At the end of each day, the treasury team needs to reconcile:


- Stablecoins purchased on Coinbase Prime against fiat debited from their bank accounts
- USDC sent to partners against the corresponding invoices in their system
- Conversions between USDC and local currencies against the expected rates


Without the connector they must export CSVs from Coinbase Prime, reformat them, and manually cross-reference against the ledger — daily for active positions, or at month-end when Coinbase Prime issues its portfolio statement. Either way, the reconciliation work falls on a person.


With the connector, Coinbase Prime data flows directly into Formance automatically. From there it feeds into your ledger and reconciliation between the two happens in a single pass. Discrepancies surface automatically. The CSV ritual disappears.


## Vault and Trading Wallets


Coinbase Prime separates digital assets into multiple buckets. Vault wallets are long-term cold storage, air-gapped, and require multiple approvals to move. Trading wallets are active storage where positions are actively managed.


The connector preserves this separation. When Formance syncs a portfolio, vault wallets and trading wallets are tagged distinctly, so your internal reporting can reflect the same operational boundaries your custody setup enforces. You can track your USDC cold reserves independently from your active ETH and BTC trading positions, while still seeing the consolidated portfolio view when you need it.


If you manage multiple portfolios on Coinbase Prime, say one for client custody and another for your operating reserves, you create one connector instance per portfolio. Each syncs independently, each maintains its own account hierarchy.


## Digital assets as financial infrastructure


The timing of this connector reflects a broader shift. Stablecoins like USDC are increasingly used as a settlement and treasury rail. In an increasing number of corridors, stablecoins are being used to settle cross-border transfers faster than correspondent banking allows — though the operational overhead around them has often lagged. At the same time, more institutions hold ETH, BTC, and SOL as part of their treasury strategy, not just as speculative positions.


But the operational infrastructure around digital assets has lagged. Most teams still track these flows in separate tools, reconcile them manually, and struggle to produce a unified view of their position across fiat and crypto. Whether it’s a USDC reserve used for settlements, an ETH position held for staking yield, or a BTC allocation as part of a treasury diversification strategy, they all need to be visible to your ledger.


When digital asset data lives in a separate system, reconciling it against fiat flows requires manual work at every step. The connector brings that data into the same infrastructure so your ledger can treat all asset classes consistently. The Coinbase Prime Connector makes that possible for one of the largest institutional crypto platforms in the world.


## Where it fits


The Coinbase Prime Connector joins 18 connectors in Formance’s Connectivity module, which covers three areas: banking connectivity, digital asset custody, and payment processor connectivity. Coinbase Prime strengthens the digital asset side, but the value is the same across all three.


Every connector uses the same account model and transaction format, feeding into your ledger through the same flow regardless whether the external data comes from a bank, a payment processor, or now Coinbase Prime. That’s how you end up matching USDC settlements against bank debits, tracking ETH staking rewards alongside interest income, or consolidating a treasury that spans several providers, all from one place.


## Availability


The Coinbase Prime Connector is available now and requires Connectivity module v3.2.0 or later. Setup takes a few minutes. You’ll need an API key from your Coinbase Prime console with read permissions, scoped to the portfolio you want to sync.


Read the[Coinbase Prime Connector docs](https://docs.formance.com/modules/connectivity/exchange/connectors/coinbaseprime) to learn more.


## Learn more about Coinbase Prime


Prime is Coinbase's full-service prime brokerage offering, combining trading, financing, custody and staking into one platform. For more details visit the[Coinbase Prime website](https://www.coinbase.com/prime) .
