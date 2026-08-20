---
schema_version: "1.0.0"
document_id: "13e7412a661b01f3d92b3b9cd27a34e95db551681f19d7fc2ab23e0def0b793a"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/exporting-marketplace-data-to-your-warehouse/"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-15T09:12:25.375763+00:00"
fetched_at: "2026-08-15T09:12:26.807099+00:00"
content_hash: "sha256:dbe7b011c88f73cb4eb677962c9b62410f926f861bf59af0592c5028c2b1e03a"
---

# Exporting Marketplace Data to Your Warehouse

*Marketplace data feeds are structured exports of your listing, agreement and billing records, delivered on the marketplace’s schedule rather than fetched on yours. Getting them into a warehouse is mostly a modelling problem, not a pipeline problem.*


---


Every marketplace reporting project starts the same way. Someone asks a question the console cannot answer — revenue by product by region by month, say — and the answer is “we’ll export it.” Three weeks later there is a pipeline, a table, and a number that does not match the console.


The pipeline was never the hard part. The hard part is that marketplace data has a shape most warehouses are not modelled for, and if you flatten that shape you lose the ability to answer the question you built the warehouse for.


---


## **What actually gets delivered**


AWS Marketplace[delivers data feeds as CSV files to an encrypted Amazon S3 bucket you provide](https://docs.aws.amazon.com/marketplace/latest/userguide/data-feed-service.html) . They are generated within a day and contain 24 hours of data from the previous day, delivered daily at midnight UTC.


Three consequences follow immediately, and each one breaks a common assumption:


- **It is push, not pull.** You do not call an endpoint when you want yesterday’s numbers. Files appear in your bucket, and your pipeline is a consumer of an event you do not control.
- **The grain is daily.** Anything you want to know intraday, you cannot get here. That is a different integration — see the events section below.
- **An “update” is not just a sale.** The documentation defines an update as a customer subscribing, a customer being invoiced, or AWS disbursing payment. Three different business events land in the same feed on the same cadence.


The other two surfaces are worth knowing so you do not build the wrong one. AWS Marketplace also offers[supplementary reports and seller dashboards](https://docs.aws.amazon.com/marketplace/latest/userguide/reports-and-data-feed.html) powered by Amazon Quick. Dashboards answer questions somebody has already anticipated. Feeds are what you use when they have not.


---


## **The bi-temporal problem, which is the whole problem**


This is the part that determines whether your warehouse is useful in a year.


Marketplace transactional data is delivered in a **bi-temporal** structure. Every fact carries two timelines:


Timeline What it records The question it answers


**Valid time** When the fact was true in the real world ”What did the customer owe in March?”


**System time** When the fact was recorded in the database ”What did we *think* the customer owed, when we closed March?”


Those are different questions and they have different answers, and a finance team needs both. Valid time tells you the truth. System time tells you what you reported at the time — which is what an auditor asks about, and what you need in order to explain why last quarter’s number moved.


The standard mistake is to` UPSERT` on a primary key. It produces a table that always shows current truth and can never reproduce a past statement. The month-end report you filed in April becomes unreproducible in May, and nobody notices until somebody asks why.


**Append, do not overwrite.** Keep both timestamps as columns. Build your current-state view as a query over the history — typically the latest row per key by system time — rather than as the storage model. Storage is cheap; a lost audit trail is not.


---


## **Feeds are not the whole picture**


A daily feed tells you what happened. It does not tell you in time to do anything about it.


If your use case is provisioning, entitlement changes, or anything a customer is waiting on, the feed is the wrong integration. Those arrive as events — AWS Marketplace publishes subscription and entitlement changes through Amazon EventBridge, and the practical differences are covered in[Marketplace Events and Webhooks: A Practical Guide](https://www.suger.io/resources/blog/marketplace-events-and-webhooks/) .


A useful split:


- **Events** drive product behaviour. Grant access, revoke access, start metering.
- **Feeds** drive reporting. Revenue, disbursement, invoice reconciliation.


Teams that try to run reporting off events end up rebuilding the feed badly, with gaps wherever a handler failed. Teams that try to run provisioning off feeds keep customers waiting up to a day for access. The two integrations look similar and are not interchangeable.


---


## **Modelling it so the numbers reconcile**


The number in your warehouse will eventually be compared to the number in the console, and to the number in your general ledger. Three modelling decisions determine whether that comparison is quick or a week of work.


**Keep the marketplace’s own identifiers as first-class columns.** Agreement ID, offer ID, product code, customer identifier. Resist the urge to map straight onto your internal account ID at load time — do it in a downstream model where the join is visible and fixable. When a reconciliation fails, the first question is always “which agreement?”, and if that column was mapped away at ingest you cannot answer it.


**Do not net anything at ingest.** Refunds, credits and adjustments arrive as their own rows. Netting them into a single balance during load destroys the ability to explain a movement. Land them as they arrive; net in the model.


**Store currency as it was delivered.** Converting at load time bakes in whatever rate your pipeline used that day, and it will not match finance’s rate. Keep the original amount and currency; convert downstream against a rate table you control.


The general shape of why these numbers disagree — and the four places the disagreement usually starts — is covered in[Why Marketplace Numbers Never Match](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) .


---


## **The multi-marketplace complication**


Everything above describes one marketplace. The moment you sell through more than one, the modelling problem changes character: each marketplace has its own delivery mechanism, its own cadence, its own identifiers and its own idea of what an agreement is.


The instinct is to build a table per marketplace and union them in a view. That works until the first question that spans them — total committed revenue by customer, say — and then you discover that “customer” means a different thing in each source, and none of them is your CRM’s account ID.


The alternative is to decide the shared model first: what an agreement, an entitlement, a usage record and a disbursement mean in *your* business, then map each marketplace onto it at ingest. It is more work up front and it is the only version that survives adding the fourth source. The vocabulary that model needs is the one the[Cloud GTM glossary](https://www.suger.io/resources/blog/cloud-gtm-glossary/) defines.


---


## **Frequently asked questions**


**How often does AWS Marketplace deliver seller data feeds?** Daily. Data feeds are delivered at midnight UTC and contain 24 hours of data from the previous day, as CSV files placed in an encrypted Amazon S3 bucket the seller provides.


**What format do marketplace data feeds arrive in?** Comma-separated value files, delivered to a seller-provided encrypted Amazon S3 bucket for ETL into the seller’s own business intelligence tools.


**What is bi-temporal data and why does it matter here?** Every fact carries valid time, when it was true in the real world, and system time, when it was recorded. You need both to reproduce a report as it was filed, which is what auditors and finance teams ask for.


**Should I use data feeds for provisioning customers?** No. Feeds are daily, so a customer could wait up to a day for access. Use subscription and entitlement events for anything a customer is waiting on, and reserve feeds for reporting.


**Should I convert currency when loading marketplace data?** No. Store the original amount and currency as delivered, and convert downstream against a rate table you control. Converting at load bakes in a rate that will not match finance’s.


---


## **Takeaways**


- Feeds are pushed to your bucket daily at midnight UTC, covering the previous 24 hours. Your pipeline is a consumer, not a caller.
- Marketplace data is bi-temporal. Keep valid time and system time as columns, append rather than overwrite, and derive current state as a query.
- Use events for anything a customer is waiting on, and feeds for anything finance is waiting on. They are not interchangeable.
- Keep marketplace identifiers, un-netted adjustments and original currency at ingest. Every one of them is something a failed reconciliation will ask for.
- Decide the shared model before the second marketplace, not after the fourth.


Suger reads agreement, entitlement, usage and disbursement records from every marketplace it supports into one model, and exports it to the warehouse and BI tools your team already uses. If your current export is a per-marketplace table and a union view,[see how Suger handles reporting](https://www.suger.io/platform/reporting/) or[talk to our team](https://www.suger.io/contact-us/) .
