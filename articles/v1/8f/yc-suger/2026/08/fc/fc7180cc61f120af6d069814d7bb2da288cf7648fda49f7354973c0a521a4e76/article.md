---
schema_version: "1.0.0"
document_id: "fc7180cc61f120af6d069814d7bb2da288cf7648fda49f7354973c0a521a4e76"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/what-moves-when-you-switch-platforms/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-20T00:55:49.760493+00:00"
fetched_at: "2026-08-20T00:55:51.412607+00:00"
content_hash: "sha256:76b3f5a61dcdf69b9205d700a030751e168fdeca15e68fba8007183d6b70639d"
---

# What Moves When You Switch Marketplace Platforms

*When you switch cloud marketplace platforms, almost nothing you are afraid of losing actually moves. Your seller account, your live agreements, your entitlements and your transaction history stay with the cloud provider — they are the marketplace’s records, not the old tool’s. What moves is the software that operates them. This is an artifact-by-artifact checklist of which is which.*


---


The person scoping a mid-year platform change is rarely worried about features. They have already decided the incoming tool is better. What keeps the project in a spreadsheet instead of in motion is a single unanswered question: *if we unplug the current tool, what breaks?*


The honest answer is “less than you think,” but “trust me” is not a migration plan. So this post does the inventory the way a careful ops owner would — one marketplace artifact at a time — and puts each into one of two columns: **stays with the cloud provider** (the marketplace is the system of record, so switching tools cannot touch it) or **moves to the new platform** (it lives in the tool, so you carry it across). Once every artifact is sorted, the migration stops being a leap of faith and becomes a checklist.


This is the companion to the[cloud marketplace platform migration checklist](https://www.suger.io/resources/blog/cloud-marketplace-platform-migration/) , which sequences the switch phase by phase. That post is *when* to do each step; this one is *what* each step is actually moving.


---


## What actually stays with the cloud provider when you switch?


**Your seller account, your published listings, your live agreements, your entitlements and your full transaction history stay with the cloud provider. They are marketplace records, held by AWS, Microsoft, Google Cloud or wherever you sell — a platform reads and writes to them, but never owns them.** This is the fact that de-risks the whole decision, so it is worth stating without hedging.


Take the two that scare people most: the seller account and the signed agreements.


Your **seller account** is the identity you registered with the cloud provider — the entity that is the seller of record, the one the marketplace pays out to. You hold it directly with AWS, Microsoft or Google Cloud. A marketplace-management platform connects *to* that account to operate on your behalf; it is not the account. Disconnecting a platform does not close, suspend or transfer your seller registration, any more than revoking a BI tool’s database credentials deletes the database.


Your **live agreements** are contracts between you and your buyer, formed on the marketplace. AWS is explicit that “AWS Marketplace seller agreements are contracts between sellers and buyers,” that “an agreement is formed when a buyer accepts an offer, public or private,” and — critically — that “AWS is not a party to any agreements.” Read that carefully: the agreement is a record the marketplace holds and governs, between two parties, neither of which is your platform vendor. A signed private offer that is active today stays active, keeps billing, and keeps entitling the buyer no matter what tool you use to look at it. The same shape holds on Microsoft and Google Cloud, where the executed contract is likewise the marketplace’s record between seller and buyer.


If those two stay, the rest of the “revenue” column stays with them, because they are the parts that carry the money.


---


## The stays-put column, artifact by artifact


Everything in this column is a marketplace record. Switching platforms points a new tool at it; it does not move it. You do not export it, and you cannot lose it by unplugging the old system.


Artifact Why it stays with the provider


**Seller account** Your registered seller-of-record identity with AWS, Microsoft or Google Cloud. A platform connects to it; it is not held by the platform.


**Published listings** Your products sit in the marketplace catalog, published under your seller account. Changing the tool that manages them does not delist or republish them.


**Live agreements / private offers** Contracts between you and your buyer, formed on acceptance and held by the marketplace. Active offers keep billing and entitling throughout a switch.


**Entitlements** The buyer’s right to use what they bought — a marketplace-side record tied to the agreement. Buyers keep their access; nothing re-provisions.


**Transaction & disbursement history** Past orders, invoices and payouts are the marketplace’s ledger. A new platform reads this history in on connection rather than importing it from the old tool.


**Usage-metering records already reported** Metering you have already submitted became invoice data the marketplace holds. Past reported usage is not stranded in the old platform.


The single sentence to take from this table: **you are not moving your revenue, because your revenue never lived in the platform.** It lives in the marketplace, and the marketplace is not the thing you are switching.


For the deeper distinction between a private offer and the entitlement it creates — the two records people most often assume are the platform’s property — see[offer versus entitlement](https://www.suger.io/resources/blog/offer-vs-entitlement/) .


---


## The moves-with-you column, artifact by artifact


**What actually moves is the operating layer: your integrations, your automation, your team’s workflows, and any data you created that the marketplace never saw.** This is real migration work, but it is bounded, and none of it is your live revenue.


- **Marketplace connections.** The new platform authenticates to each seller account you hold — you set up the same connection you did the first time, once per marketplace. This is additive: a new tool can connect alongside the old one, because reading marketplace data is non-destructive.
- **CRM and finance integrations.** The pipe that syncs marketplace data into Salesforce or HubSpot, and out to your billing and ERP systems, is a property of the platform. You re-point it at the new tool. How that pipe is built is the subject of[connecting your CRM to cloud marketplace data](https://www.suger.io/resources/blog/connect-crm-to-cloud-marketplace-data/) .
- **Metering pipeline configuration.** The *records* you already reported stay with the marketplace, but the *code path* that sends new usage — where your app posts metering to — has to move. This is the one cutover to treat with care, covered below.
- **Automation and workflows.** Approval flows, alerts, notification rules and internal routing were built in the old tool. They are re-created in the new one. This is configuration effort, not data at risk.
- **Derived data you added.** Custom fields, internal notes, tags and any annotations you layered on top of marketplace data are genuinely yours to export, because the marketplace never held them. Everything sourced *from* the marketplace comes across on its own; only the data you invented needs a hand-off.
- **API credentials and auth.** Any internal systems that called the old platform’s API need new credentials for the new one. The pattern for connecting programmatically is in[authentication for marketplace APIs](https://www.suger.io/resources/blog/authentication-for-marketplace-apis/) .


Notice what is on this list and what is not. The list is tooling — connectors, config, credentials, and the handful of fields you created. Your listings, agreements, entitlements and history are all in the *other* column, untouched.


---


## The one artifact to move carefully: usage metering


**Usage metering is the only artifact whose cutover can touch a live invoice, so it moves last and in parallel.** If your pricing is contract- or seat-based, this section does not apply and your migration is simpler.


The nuance is the split described above: the metering *records* you have already reported are the marketplace’s, and they stay. What moves is the *reporting path* — the endpoint your product posts new usage to. Because that new usage becomes buyer invoices, an error here is a billing error, not a cosmetic one.


The safe pattern is sequencing, not courage. Start reporting usage to the new platform’s metering endpoint, confirm the records match what you have been sending, and only then stop reporting through the old path. Never dual-report the same usage to the same marketplace from two systems at once, and never cut the old path until the new one has produced correct records across a full billing cycle. Metering earns its place at the end of the sequence for exactly this reason. The mechanics of accurate metering across pricing models are on the[marketplace billing and metering platform](https://www.suger.io/platform/billing-metering/) page.


---


## Turning the two columns into a migration checklist


**Run the switch so that every stays-put artifact is verified before any moves-with-you artifact is cut over, and so the only irreversible step comes last.** The two columns above map directly onto an order of operations:


1. **Connect the new platform read-only and confirm the stays-put column.** Point the new tool at each seller account and check that it sees your listings, live agreements, entitlements and history correctly. This is where you prove, for your own account, that the “it all stays with the provider” claim is true — before you change anything.
2. **Re-point the moves-with-you integrations one at a time.** Rebuild CRM and finance syncs, re-create workflows, and re-issue API credentials against the new platform, reconciling each against the old tool as you go.
3. **Cut over new private offers, then co-sell.** Start creating *new* offers in the new platform. This is fully reversible — a new offer created in the wrong tool is just an offer you re-create — so it is safe to do early once integrations are stable.
4. **Cut over metering, in parallel, if usage-based.** Follow the parallel-reporting pattern above, and only stop the old path after a clean billing cycle.
5. **Sunset the old tool.** Decommission the previous in-house or third-party system. This is the sole irreversible step, and it comes last — after you have transacted real deals on the new platform.


Every step before the last is reversible, because the marketplace records never moved. That is what “low-risk migration” actually means: not that you are careful, but that the assets you cannot afford to lose were never in play.


For the reasons a team reaches this point in the first place — and how to choose the tool you switch *to* — see[how to choose a cloud marketplace platform](https://www.suger.io/resources/blog/how-to-choose-a-cloud-marketplace-platform/) .


---


## Frequently asked questions


**Does switching marketplace platforms move my seller account?** No. Your seller account is the seller-of-record identity you hold directly with the cloud provider. A platform connects to it to operate on your behalf, but it does not own it. Disconnecting a platform does not close, suspend or transfer your seller registration.


**Do live agreements and private offers survive a platform switch?** Yes. On AWS Marketplace an agreement is a contract between seller and buyer, and AWS is not a party to it. Active agreements and private offers keep billing and entitling buyers regardless of which tool you use to view them. Microsoft and Google Cloud work the same way.


**Will I lose my transaction and revenue history?** No. Past orders, invoices, disbursements and reported usage are the marketplace’s records. A new platform reads your history in on connection rather than importing it from the old tool, so you are not carrying an export across.


**What actually has to move when I switch platforms?** The operating layer: marketplace connections, CRM and finance integrations, the metering reporting path, automation and workflows, API credentials, and any custom fields or notes you created. None of that is your live revenue — it is tooling and configuration.


**Which artifact is riskiest to migrate?** Usage metering, because new reported usage becomes buyer invoices. The records you already sent stay with the marketplace; only the reporting path moves. Report to both systems in parallel first, confirm the records match across a full billing cycle, and cut the old path last.


**Do I have to delist products to switch?** No. Delisting is never required. Your listings stay published under your seller account throughout the switch. A new platform connects alongside the old one, and nothing is removed until you sunset the old tool at the very end.


## Takeaways


- The assets you are afraid of losing stay with the cloud provider: your seller account, published listings, live agreements, entitlements and full history are marketplace records, not the old tool’s property.
- On AWS Marketplace an agreement is a contract between seller and buyer, and AWS is not a party to it — so a live offer keeps billing and entitling regardless of which platform you use.
- What moves is the operating layer: marketplace connections, CRM and finance integrations, workflows, API credentials, and the handful of custom fields you created. That is bounded work, not live revenue.
- Move usage metering last and in parallel — the records you already reported stay put; only the reporting path changes, and it is the one path that feeds live invoices.
- Sort every artifact into “stays” or “moves” first, and the migration becomes a checklist whose only irreversible step is sunsetting the old tool, at the very end.


---


Suger is a Cloud GTM platform for selling and billing through cloud marketplaces — AWS, Microsoft, Google Cloud, Snowflake, Alibaba Cloud, and Oracle. Adopting it follows exactly this pattern: it[connects to the seller accounts you already hold](https://doc.suger.io/get-started/connect-your-marketplace/) so you can operate listings and offers from one place, while your agreements and history stay where they always were — on the marketplace. See how the operations you would move fit together on[the Suger platform](https://www.suger.io/platform/) , and how signed contracts are managed once you switch in[marketplace agreement management](https://www.suger.io/platform/agreements/) .
