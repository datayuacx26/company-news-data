---
schema_version: "1.0.0"
document_id: "afe2c89605e651c6b866ec69130ed3c5b426efe927ed011f12537e5e172ed4a6"
company_key: "yc-whalesync"
company: "Whalesync"
source_id: "yc-whalesync-news-import-4fcd9b7a082a"
canonical_url: "https://www.whalesync.com/blog/how-to-build-read-only-revops-dashboards"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-30T07:26:03.249213+00:00"
fetched_at: "2026-07-30T07:26:04.334699+00:00"
content_hash: "sha256:3218539c7ab6d4967212de181da5411ba6b9641f0c97a01f73e5bb0e869a5f0e"
---

# How to Build Read-Only RevOps Dashboards in Airtable, Notion, or Google Sheets

Every revenue team has the same recurring meeting. Leadership wants to see pipeline coverage, stage conversion, and forecast numbers. The data lives in Salesforce, HubSpot, or Affinity. And the person who owns RevOps spends Sunday night exporting CSVs, massaging them into a spreadsheet, and screenshotting charts into a slide deck that is stale before anyone opens it.


The obvious fix, giving everyone access to the CRM, usually makes things worse. CRM seats are expensive, the reporting UI intimidates casual viewers, and every extra person with edit rights is another way for a stage to get dragged into the wrong column. What most teams actually need is a **read-only RevOps dashboard** : a live, viewer-friendly picture of revenue data, built in a tool people already use, that nobody can break.


This guide covers how to build one: where the data should come from, how to move it, and how to set up the read-only layer in Airtable, Notion, or Google Sheets.


## Why RevOps dashboards should be read-only


A dashboard has exactly one job: let people see the state of the business. The moment viewers can also edit the underlying records, you have created a second system of record, and the two will drift apart. Making the dashboard read-only solves several problems at once:


- **Your CRM stays the source of truth.** Data flows in one direction: out of Salesforce, HubSpot, or Affinity and into the dashboard. Nothing a viewer does in Airtable, Notion, or Sheets can overwrite a clean CRM record.
- **You stop paying for viewer seats.** Executives, board members, founders, and cross-functional partners who only need to *look* at pipeline don't need CRM licenses. Viewer access in Airtable, Notion, and Google Sheets is far cheaper, and often free.
- **You control the surface area.** A dashboard shows the ten fields that matter, not the two hundred that exist. That also makes it easy to leave sensitive fields, like personal contact details or compensation-linked amounts, out of the dashboard entirely.
- **Viewers get a tool they already understand.** Nobody needs training to read a Google Sheet or scroll a Notion page.


## Step 1: Decide what the dashboard answers


Before touching any tool, write down the three to five questions the dashboard must answer. For most RevOps teams the list looks something like: How much qualified pipeline do we have against target? How are deals moving between stages this quarter? Which deals are stuck, and with whom? What closed recently, and what is forecast to close?


Each question maps to a small set of CRM objects and fields: usually opportunities or deals, plus owner, stage, amount, close date, and last activity. Resist the urge to sync everything. A dashboard built on a dozen fields is faster to build, faster to load, and far easier for viewers to trust.


## Step 2: Get the data out of your CRM


There are three common approaches, in ascending order of usefulness.


### Manual CSV exports


Every CRM can export a report to CSV. It works, but it is a snapshot: accurate for about an hour, then quietly wrong. Manual exports also tend to fail in annoying ways. HubSpot, for example, caps export size, so large property sets have to be split across multiple files. If your dashboard cadence is truly monthly, exports can be fine. For anything approaching real time, they are the Sunday-night problem in disguise.


### Trigger-based automation


Tools like Zapier or Make can append a row to a sheet when a deal changes. This handles simple cases, but CRM data is update-heavy: stages change, amounts get revised, owners rotate. Modeling constant updates as one-way triggers means maintaining a web of automations that still only keeps the copies loosely consistent, and a single failed run silently drops edits.


### Live one-way sync


The cleanest option is a sync tool that continuously mirrors selected CRM tables and fields into your dashboard tool.[Whalesync](https://www.whalesync.com/) is built for exactly this: it connects[Salesforce](https://www.whalesync.com/connector/salesforce) ,[HubSpot](https://www.whalesync.com/connector/hubspot) , and[Affinity](https://www.whalesync.com/connector/affinity) to Airtable, Notion, and Google Sheets, and lets you[set the sync direction per field](https://docs.whalesync.com/features/two-way-sync) . For a read-only dashboard, point every arrow one way, CRM to dashboard, so records stay continuously up to date while nothing ever flows back. You choose exactly which tables and fields sync, which is how you keep sensitive data out of the viewer layer.


## Step 3: Build the read-only layer


Each destination tool has a different way of showing data without letting viewers touch it.


### Airtable: Interface Designer


Airtable is the strongest option when you want something that feels like a real BI dashboard. Land your synced CRM records in a base, then build on top of it with[Interface Designer](https://support.airtable.com/docs/interface-designer-permissions) : charts for pipeline by stage, number elements for coverage metrics, and filtered record lists for deal reviews. The key feature is permissions: you can share an interface with read-only access, and interface-only collaborators see the dashboard without ever seeing the underlying base. Grouped and filtered views let you give sales leadership one page and the board another, all from the same synced data. For what else can plug into the same base, see our guide to[Airtable integrations](https://www.whalesync.com/blog/airtable-integrations) .


### Notion: linked views in a living document


Notion is the right choice when the dashboard should live next to context: board memos, QBR notes, account plans.[Sync your deals into a Notion database](https://www.whalesync.com/blog/how-to-connect-and-sync-notion-to-salesforce-in-5-minutes-with-whalesync) , then create linked views of it anywhere: a board grouped by stage on the pipeline page, a table filtered to closing-this-month in the forecast doc. Set page permissions to *Can view* for your audience and the numbers stay live while the records stay locked. Notion won't give you rich charting, but for narrative reporting, like a weekly pipeline summary with the actual pipeline embedded, it is unmatched.


### Google Sheets: pivot tables and protected ranges


Sheets is the pragmatic pick, especially for finance-adjacent stakeholders who live in spreadsheets. The pattern that works: sync raw CRM data into one tab, treat it as an untouchable data layer, and build pivot tables and charts in separate tabs. Protect the data tab so only the sync (and you) can modify it, then share the file as view-only. Or keep the raw data in a separate spreadsheet entirely and pull it into the dashboard file with IMPORTRANGE. Viewers get familiar charts and filters; the data layer stays pristine. It is the same pattern as[using Google Sheets as a database](https://www.whalesync.com/blog/how-to-use-google-sheets-as-a-database) , applied to CRM reporting.


## Salesforce, HubSpot, and Affinity: notes on each source


**Salesforce** has powerful native reporting, but it lives behind expensive seats, and cross-object reports have a learning curve that casual viewers never climb. Syncing opportunity data out to a friendlier surface is often less about capability and more about access and cost.


**HubSpot** dashboards are solid for marketing and sales users who are already in the tool daily. The friction shows up when data needs to leave: native exports are manual, capped, and disconnected from anything else you want to analyze alongside them, such as product usage or billing data sitting in a spreadsheet.


**Affinity** is the interesting case for venture and private capital teams. Its relationship intelligence is excellent, and Affinity Analytics offers native dashboards, but that sits on higher-tier plans, and LPs, venture partners, and operating advisors rarely warrant seats at all. Syncing deal flow into a read-only Airtable interface or a Notion page is a common pattern for Monday partner meetings and LP updates alike.


## Keep it trustworthy


A few habits separate dashboards people rely on from dashboards people ignore:


- **Keep the flow strictly one-way.** The moment someone edits a synced copy, trust erodes. Read-only permissions plus a one-directional sync makes drift structurally impossible.
- **Show freshness.** Include a last-updated timestamp so viewers know they are looking at live data, not last quarter's export.
- **Sync less than you think you need.** Fewer fields means faster loads, cleaner views, and less sensitive data outside the CRM.
- **Assign an owner.** Someone in RevOps should own the dashboard's definitions, like what counts as qualified pipeline and which stages roll into the forecast, and keep them aligned with the CRM.


## The payoff


A read-only RevOps dashboard is one of the highest-leverage things a revenue operations team can ship. It kills the Sunday-night export ritual, gets leadership answering their own questions, and does it all without adding a single risky edit path into your CRM. Pick the surface your stakeholders already live in: Airtable for BI-style interfaces, Notion for narrative context, Sheets for spreadsheet natives. Then put a live one-way sync underneath it and lock the viewer layer down.


If you want the sync part to take minutes instead of a sprint,[Whalesync](https://www.whalesync.com/) connects Salesforce, HubSpot, and Affinity to Airtable, Notion, and Google Sheets with field-level control over what syncs and in which direction. Set the arrows one way, share the dashboard read-only, and let the data flow.
