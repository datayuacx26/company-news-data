---
schema_version: "1.0.0"
document_id: "8dc93924b41c3fd208c86e22f20d904fcaf951e66f90009515ef6d60b16fa977"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/competitive/formance-vs-fragment"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T12:01:32.170369+00:00"
fetched_at: "2026-08-13T12:01:33.630161+00:00"
content_hash: "sha256:30180bf613e8e2d3498483fbefdbe276f95f8ffbfdd206f365c83ab64f5125fb"
---

# Formance vs Fragment: Built for What Comes After the MVP

Teams comparing Formance vs Fragment need to know whether the system of record built for the MVP still holds as transaction volume rises and regulatory obligations spread across more payment rails.


Your first chart of accounts took an afternoon to model at MVP stage. Eighteen months later, the same ledger sits under a dozen payment providers. It also supports a stablecoin leg the product team added last quarter and a safeguarding audit on the calendar. The MVP-era ledger is now the post-MVP bottleneck, and two systems answer that question differently.


Formance is an open-source, programmable core ledger that unifies fiat and digital assets with regulatory-grade traceability, built for what comes after the MVP.


Fragment is a managed ledger API often evaluated by teams trying to reach a correct double-entry model quickly at the MVP stage. They are built for different phases of the same company.


### What is an MVP ledger?


An MVP (Minimum Viable Product) is the earliest shippable version of a product. It is the smallest feature set that lets a team validate the core value proposition with real users.


In ledger terms, an MVP ledger is the first pass at a financial system of record, typically a single-currency, single-rail, double-entry model that supports one or two payment providers and a handful of account types. It's optimized for speed to first transaction, but it breaks down against the multi-rail, multi-asset, multi-jurisdiction reality that emerges once the product gains traction.


Post-MVP is the phase where that initial model meets production volume, new payment rails, additional asset classes, and regulatory obligations it was never designed to carry.


### Why Formance works post-MVP


Formance puts the logic inside the ledger, keeps it programmable, and makes correctness a property of the ledger itself. At the post-MVP scale, financial logic that lives outside the ledger becomes middleware that must be written, tested, and maintained forever.


Formance offers teams the financial logic, correctness guarantees, and an audit trail within the ledger itself, so multi-party postings, fee splits, and reserve movements remain programmable and provable without a layer of middleware to maintain them.


#### Formance's programmable logic inside the ledger


The[Formance Ledger](https://www.formance.com/modules/ledger) executes multi-party postings, fee splits, reserve movements, and fund-attribution logic as first-class ledger operations.


The ledger becomes the single source of financial truth. The core is MIT-licensed with[full source](https://github.com/formancehq/ledger) on GitHub, and it's available as Open Source / Community Edition, Enterprise Self-Hosted, or Enterprise Private Cloud. Financial figures can be described in Numscript, Formance's purpose-built language for describing financial transactions.


A multi-party settlement that would otherwise be backend code wrapped in transaction handling and retry logic becomes a handful of send statements that both engineers and finance teams can read without translation.


When the fee structure changes, the change is shipped as a script edit, and the intent remains auditable in one place.


#### Formance's structural correctness


The Formance Ledger enforces correctness at write time. It records[ledger postings](https://www.formance.com/glossary/posting) as append-only entries in a hash-chained transaction history that is never rewritten. It tracks both recorded and effective time for each transaction, so engineers can backdate corrections and reconstruct any balance at any historical point. When a transaction affects multiple accounts, it either commits in full or not at all.


Formance blocks unbalanced writes and commits multi-account transactions atomically. An unbalanced transaction cannot commit, so orphan value is structurally impossible. Atomic multi-posting means that a transaction affecting many accounts succeeds or fails as a single unit. Complex split-and-fee logic and reserve movements stay consistent inside the ledger. At low volume, these properties read like bonuses. Under regulatory scrutiny, they are prerequisites.


#### A Numscript example in practice


A merchant pays a recipient $5,000 from its[omnibus account](https://www.formance.com/glossary/omnibus-account) , the platform captures a 2% fee, and a parallel 2,500 USDC leg settles to the same recipient. The merchant's omnibus account holds both fiat and USDC balances. In Numscript, the entire flow is a single atomic transaction.


The accounts are:


**Real-world party** **Ledger account** **Holds**


Merchant @merchants:acme:omnibus the merchant's USD and USDC balance


Recipient @users:alice:wallet the recipient's received USD and USDC


Platform fee income @platform:revenue:fees platform fee revenue


The merchant disburses $5,000.00. Of the USD leg, 2% goes to platform fee revenue, and the remainder goes to the recipient. The 2,500 USDC leg goes to the same recipient.


```text
// MERCHANT_DISBURSEMENT
// Event: merchant disburses USD and USDC; platform keeps a 2% USD fee
send   [USD/2 500000]   (
source   =   @merchants:acme:omnibus
destination   = {
2  %   to   @platform:revenue:fees
remaining   to   @users:alice:wallet
}
)
send   [USDC/6 2500000000]   (
source   =   @merchants:acme:omnibus
destination   =   @users:alice:wallet
)
set_tx_meta  (  "event_type"  ,   "merchant_disbursement"  )
set_tx_meta  (  "disbursement_id"  ,   "dsb001"  )


```


Multiple send statements in one script commit as a single transaction. Execution is atomic, ensuring that either all modeled transactions are committed, or none are. The fiat leg and the stablecoin leg cannot diverge.


Around a prescriptive ledger API, teams typically open a database transaction, lock the omnibus balance, compute the fee, insert postings for the fiat leg, call a separate path for the stablecoin leg, handle the case where the first succeeds and the second times out, attach a deduplication key so the webhook retry doesn't run everything twice, and emit audit records.


Every step is code that your team writes, tests, and maintains, and the transaction's intent is scattered across services rather than stated in a single script.


#### Formance's auditable, point-in-time history


Every posting is auditable and reconstructable to any past moment. The hash chain makes the history tamper-evident, and the transaction remains readable by a finance analyst without an engineering ticket to interpret it.


Adding another ledger leg or asset movement is another send statement inside the same atomic transaction. Formance Ledger supports point-in-time views and backdated corrections without rewriting history, thanks to its bi-temporal model. Middleware-based logic often reaches the same outcome only by reprocessing history and reconciling by hand.


### When post-MVP ledger complexity catches up


MVP-era financial logic degrades quietly, then expensively. Each new provider adds a data format and a settlement-timing quirk, while each reconciliation failure gets patched with a script. Then, each patch adds hours to month-end close.


The[Liberis situation](https://www.formance.com/blog/customer-story/liberis) shows the pattern in real life. Bundled transactions and split mechanisms among multiple partners made reconciliation difficult to manage as their program expanded, and their engineering team spent time building basic ledger functionality instead of shipping product. Formance reports Liberis achieved zero reconciliation errors across 14 countries over 1.5 years in production.


Four trigger events tend to force the architecture conversation:


1


**A regulation deadline:** For example, the FCA's PS25/12 rules, in force since 7 May 2026, require[daily reconciliation](https://www.fca.org.uk/news/press-releases/payment-safeguarding-rules-changes) of safeguarded funds, annual safeguarding audits, and monthly regulatory returns for UK payment firms. Similar pressure is landing from[MiCA](https://www.esma.europa.eu/esmas-activities/digital-finance-and-innovation/markets-crypto-assets-regulation-mica) ,[DORA](https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng) , and the[GENIUS Act](https://www.congress.gov/bill/119th-congress/senate-bill/1582/text) , each of which pushes data integrity, reconciliation, and reserve disclosure requirements directly into ledger architecture.


2


**Payment rail splits:** Providers multiply past what the original model assumed, and every added statement format and cut-off behavior lands on a schema designed for two.


3


**Automated reconciliation as an engineering tax:** Manual matching scripts break whenever a provider changes formats and consume hours that were budgeted for features.


4


**A poorly constructed balance execution:** A balance nobody can explain, or a duplicate posting after a network retry.


Any of these triggers on their own is manageable. Stacked together, they force a choice about where financial logic lives: either inside a ledger built to carry it, or spread across middleware that must be maintained forever.


That choice is where Formance and Fragment diverge, and it's easier to see by starting with the case Fragment was built for.


### Why Fragment works at MVP stage


Fragment is built for teams that want to reach a correct double-entry ledger fast without owning ledger infrastructure at the MVP stage.


The trade-offs (closed source, cloud-only deployment, and customization limits as flows grow) are acceptable when the product is single-asset, single-rail, and not yet under regulatory pressure.


#### Fragment's schema-first managed platform


Accounts and entry patterns are declared through a schema, and Fragment generates the tooling around them. The visual ledger simulator is a genuine evaluation-stage advantage for teams that want to model accounts and simulate entries quickly, and the declared structure can be attractive before production buildout.


For a narrow, well-understood use case, onboarding may be fast, and the developer experience can be strong. The fit becomes shakier when a new payment rail or market requires an asset class or logic the initial model does not comfortably express.


If custom transaction logic is not expressed within the ledger core, multi-step coordination must live around the ledger. As flows grow into multi-party settlements or non-standard business logic, customization tends to shift from inside the platform to code written around it.


#### Fragment's evaluation-stage advantage


Fragment's main appeal is getting to a financially correct balance infrastructure quickly. The simulator-led workflow is useful for teams that want to model accounts and simulate entries before writing production code, and the guided schema-first modeling approach is helpful.


[Stripe's backing](https://techcrunch.com/2024/07/22/digital-ledger-fragment-9m-banks-balance-sheets) gives it credibility, and its standard debit/credit model is immediately familiar to finance teams. Commercial fit is a separate question, and teams should confirm current pricing directly and whether the managed model fits their operating requirements.


#### Fragment's fit for early-stage teams


Without a dedicated infrastructure engineer, a tightly managed, bounded cloud ledger is a rational choice. Fragment fits a narrow single-asset, single-rail flow with minimal regulatory requirements and no multi-rail, multi-entity, or multi-market planned expansion.


#### Fragment's trade-offs at scale


Vendor-controlled architecture makes deployment flexibility and structural changes open questions once new rails or asset classes arrive under regulator scrutiny. Migrating a core ledger is open-heart surgery.


### Formance vs Fragment feature comparison


Formance and Fragment differ on source access, deployment, customization, reconciliation, and compliance posture as payment rails, assets, and compliance requirements grow:


**Property** **Formance** **Fragment**


License and source access MIT-licensed core ledger with full source available Closed source, and no code inspection


Multi-asset support Fiat and digital assets tracked natively, with balances and precision maintained per asset Single-asset model per entry; multi-asset flows require coordination in application code


Transaction language Numscript DSL and logic executes inside the ledger Schema-driven entry model, but customization narrows as flows grow more complex


API surface REST API with atomic multi-posting support. Numscript is executed server-side Managed API, but integration and debugging depth vary by use case


Connectivity Pre-built connectors (Stripe, Adyen, Wise, Fireblocks, and others) plus a generic connector framework Custom ingestion and provider workflow requirements may require code around the ledger


Reconciliation Drift detection and balance verification against external provider records No deep audit or traceability tooling


Orchestration Flows offers event-driven multi-step workflows No orchestration engine because custom multi-step logic lives around the ledger


Deployment Open Source / Community Edition, Enterprise Self-Hosted, or Enterprise Private Cloud Cloud-hosted only with no self-hosting or data residency control


Certifications SOC 2 Type II, ISO 27001 certified, DORA-assessed, and MiCA and GENIUS alignment Early-stage product with limited enterprise track record and unclear compliance posture


### Formance vs Fragment: which ledger fits your MVP stage?


The choice between Formance and Fragment comes down to where your product sits on the MVP-to-post-MVP curve, including your stage, your regulatory surface area, and how much of your product's financial logic you're willing to maintain as middleware around the ledger.


#### Formance's Ledger when you're past MVP and the ledger has to carry real weight


Choose Formance if you are Series A or later with multi-rail or multi-asset flows across fiat and[digital assets](https://www.formance.com/glossary/digital-asset) , a regulated entity carrying EMI, MTL, MiCA, or DORA obligations, a company that needs self-hosted deployment for data sovereignty, or a team whose product logic is complex enough that encoding it inside the ledger beats maintaining it as middleware.


Formance is built for what comes after the MVP: programmable, auditable, and yours to run wherever your compliance posture demands.


#### Fragment is ideal when you're at the MVP stage and speed is the only thing that matters


Choose Fragment if you are early-stage with a single-asset, single-rail product, no near-term regulatory exposure, no multi-market plans in the early stages, and no engineering bandwidth to run infrastructure at the component level.


Fragment's guided modeling workflow and managed developer experience can get you to a correct double-entry ledger quickly. Confirm current pricing and evaluation terms directly before committing.
