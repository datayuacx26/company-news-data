---
schema_version: "1.0.0"
document_id: "7474e5e3f541c1fc5cd1bbc01307b64cd030158c418e59e0c100e0cfd884cf06"
company_key: "yc-rutter"
company: "Rutter"
source_id: "yc-rutter-news-import-c12c34cd87f8"
canonical_url: "https://www.rutter.com/blog/bank-feeds-embedded-reconciliation"
published_at: "2026-05-26T14:03:39.401+00:00"
first_seen_at: "2026-07-22T12:26:40.375919+00:00"
fetched_at: "2026-07-28T22:07:11.290939+00:00"
content_hash: "sha256:b912909bfc8df2fdda4f593cf4810bfa4b8ef740c60c50bea75c73a2cfe53eb2"
---

# Why bank feeds became baseline infrastructure for embedded reconciliation

Bank feeds, ERP transaction sync, embedded reconciliation. Three names for the same infrastructure modern financial products now depend on. Rutter Bank Feeds reaches 30+ ERP and accounting platforms today. Direct integrations cover NetSuite, Xero, Sage, and Sage Intacct. Aggregator partnerships with Plaid and Yodlee extend coverage into QuickBooks Online and the long-tail ERPs. Mercury uses it to land Mercury transactions inside the customer’s QuickBooks, Sage, Xero, or NetSuite, alongside 100+ other fintechs and banks running on Rutter today.


Every bank and fintech eventually arrives at the same requirement. Their customers’ bank transactions need to land inside the ERP where the finance team already works, so the team can reconcile without leaving the bank or fintech’s product. That experience is embedded reconciliation, and bank feeds is the infrastructure underneath.


What changed is the timeline. Building bank feeds coverage used to take years. Rutter recently helped a chartered bank deploy across QuickBooks Online, NetSuite, Xero, and Sage in 2.5 weeks. That is the bar now.


Bank feeds is no longer a differentiator. It is the floor.


## What are bank feeds?


**Bank feeds** are the infrastructure that delivers a bank’s or fintech’s transaction data into the customer’s ERP or accounting system, so the customer can reconcile inside the ERP. Commercial banks call it bank feeds (the source). Fintech product teams call it **ERP transaction sync** (the destination). Controllers call it **embedded reconciliation** (the use case) when the experience lives inside the bank or fintech’s own product.


**Rutter Bank Feeds** is the product. Embedded reconciliation is what it powers.


## Why is bank feeds infrastructure now table stakes?


Five years ago, building bank feeds in-house was a defensible choice. The ecosystem was simpler, customer expectations were lower, and the build cost was something a serious bank or fintech could absorb.


That calculus has changed. Mid-market and SMB finance teams expect their bank or fintech to deliver clean, real-time transaction data into the ERP they work in. If the bank or fintech cannot do that, the customer reconciles by exporting CSVs or switches to one that can.


Embedded reconciliation has moved from a feature on the roadmap to a parity requirement on the buyer’s evaluation matrix.


## Why is building bank feeds an ecosystem problem, not an API problem?


Most teams initially frame bank feeds as an integration problem. Connect to the bank, push to the ERP, done. The reality is different. Delivering bank feeds at scale means delivering data through two different paths.


Both paths require their own approvals, technical work, and ongoing maintenance. Direct integrations require getting listed and approved as a data source by each ERP. Plaid and Yodlee paths require being a recognized data source inside those networks.


Neither is a one-time API project. Both are multi-year ecosystem efforts that compound across direct integrations, aggregator partnerships, and ongoing certification as each ERP updates its requirements.


Teams that try to build this internally usually cover one or two ERPs and stall. As soon as a customer shows up on a different system, the bank or fintech has to either turn the customer away or start another integration project. Rutter Bank Feeds ships with the ecosystem work already in place across both paths.


## How fast can bank feeds be deployed?


Historically, building bank feed coverage across direct integrations and aggregator partnerships required years. That timeline is no longer what buyers will accept.


Rutter recently helped a chartered bank deploy bank feeds across QuickBooks Online, NetSuite, Xero, and Sage in 2.5 weeks. That speed reflects the fact that the ecosystem work, the listings, the partnerships, and the FDX support, was already done. The bank built against Rutter’s bank feed API and inherited the coverage.


## Should you build or buy bank feeds?


The case for building bank feeds in-house in 2026 is hard to make. Embedded reconciliation is now an expected feature in the buyer’s evaluation matrix. Spending engineering years on infrastructure the buyer already assumes you have is a hard case to bring to a CEO or a board.


The banks and fintechs winning embedded reconciliation deals are the ones who treated bank feeds as a buy-versus-build decision and chose buy. That frees engineering capacity for the parts of the product that differentiate them.


Rutter Bank Feeds delivers transaction data into NetSuite, Xero, Sage Intacct, QuickBooks Online, and 30+ ERP and accounting platforms through both direct integrations and Plaid and Yodlee partnerships.[See the full platform coverage list](https://www.rutter.com/platform-coverage) . The ecosystem work is done. Build against the API and ship.


[→ See how it works](https://www.rutter.com/bank-feeds)


## Frequently asked questions


### What are bank feeds?


Bank feeds are the infrastructure that delivers a bank’s or fintech’s transaction data into the customer’s ERP or accounting system, so the customer can reconcile inside that ERP. They are the foundation of embedded reconciliation.


### What is embedded reconciliation?


Embedded reconciliation is the experience of a finance team reconciling their bank or fintech transactions inside the bank or fintech’s own product, without leaving the workflow to log into a separate system.


### What is the difference between bank feeds and ERP transaction sync?


They describe the same capability from different angles. Bank feeds is the source-side term used by commercial banks. ERP transaction sync is the destination-side term used by fintech product teams. Both refer to transaction data flowing from a bank or fintech into the customer’s ERP.


### How do banks deliver bank feeds to ERPs?


Through two paths. The first is a direct integration with the ERP, where the bank pushes data or the ERP pulls via FDX. NetSuite, Xero, Sage Intacct, and Sage Business Cloud work this way. The second is via Plaid or Yodlee, where the bank delivers data to one of those aggregators and the ERP’s bank feed module ingests it from there. QuickBooks Online and most long-tail ERPs work this way.


### Should banks and fintechs build bank feeds in-house or buy?


Most are choosing buy. Bank feeds infrastructure does not differentiate the product. It enables embedded reconciliation. Building in-house takes years and requires direct ERP partnerships, aggregator certifications, and FDX support. Buying frees engineering capacity for the differentiating parts of the product.


### Who uses Rutter Bank Feeds?


Mercury uses Rutter Bank Feeds today, alongside 100+ other fintechs and banks running on Rutter. Mercury uses it to land Mercury transactions inside the customer’s QuickBooks, Sage, Xero, or NetSuite. A chartered bank recently deployed bank feeds across QuickBooks Online, NetSuite, Xero, and Sage in 2.5 weeks.
