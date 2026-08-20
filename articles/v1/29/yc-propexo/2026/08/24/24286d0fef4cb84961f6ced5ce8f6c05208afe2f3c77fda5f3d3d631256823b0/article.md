---
schema_version: "1.0.0"
document_id: "24286d0fef4cb84961f6ced5ce8f6c05208afe2f3c77fda5f3d3d631256823b0"
company_key: "yc-propexo"
company: "Propexo"
source_id: "yc-propexo-news-import-1c9cf0eb2f62"
canonical_url: "https://propexo.com/blog/pms-integrations-data-strategy-gap/"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-12T03:27:59.234779+00:00"
fetched_at: "2026-08-12T03:28:02.539824+00:00"
content_hash: "sha256:b7d79138a2011533a10477b74cecfdc6a3fea3735c2a0cd995607ba02ffdd629"
---

# Why PMS Integrations Don't Fix Your Data Strategy

This piece was originally published by Thesis Driven as[Your PMS Integrations Are Working. Your Data Strategy Isn’t.](https://www.thesisdriven.com/letters/your-pms-integrations-are-working-your-data-strategy-isnt/) It’s one of the clearest statements we’ve seen of a problem we work on every day, so rather than reproduce it, we wanted to build on it from where we sit: inside the pipes connecting property management systems to the tools that need their data.


The core claim holds up from where we sit: multifamily operators have spent years and real budget getting their systems talking to each other, and a surprising number of them still can’t answer a simple portfolio-wide question without a manual export and a weekend in a spreadsheet. That’s not a failure of effort. It’s a sign that “integrated” and “usable” are being treated as the same milestone when they’re two different ones.


## Why does a “connected” PMS still leave you data-blind?


Because connecting to a system and structuring what comes out of it are separate jobs, and most integration work only does the first one. Yardi, RealPage, Entrata, and AppFolio have each made real improvements to their APIs over the past few years — better documentation, more endpoints, faster rate limits. None of that changes what shape the data arrives in, or whether it agrees with the other five tools an operator also runs.


Recent survey work on PMS platform openness (the kind Thesis Driven has been running) tends to land every major vendor in a similar middling band — genuine progress, but nowhere near “solved.” That’s consistent with what we see in the field: the ceiling on any single vendor’s API quality is lower than operators assume, and it was never the part of the stack that determines whether analytics or AI actually works.


## The four ways data leaves a PMS, and why each one stops short


Every export path out of a property management system falls into one of four buckets, and all four hand the problem back to you at the same point — the moment the data lands.


- **API calls.** Built for developers integrating one workflow at a time, not for an analytics team trying to pull a full portfolio history.
- **Scheduled reports.** Useful, but the vendor defines the columns, the grain, and the refresh schedule. You get what they decided to ship.
- **SFTP file drops.** Same vendor-controlled shape as scheduled reports, delivered as a file instead of a report, on the vendor’s timeline rather than yours.
- **Read-only database access.** The closest thing to raw data, and the least commonly offered — when it exists at all, it often carries a six-figure price tag.


All four get data *out* . None of them normalize it, reconcile it against your other systems, or guarantee it’ll still look the same after the vendor’s next schema change. That work happens after the export, or it doesn’t happen at all.


## Why fragmentation is the real problem, not integration quality


Because every operational tool in a multifamily stack keeps its own definitions, and nothing forces them to reconcile. A PMS, a maintenance platform, and a leasing CRM can each use the word “vacant” to mean something slightly different — pending move-out, notice given, or physically empty — and each one is internally consistent while disagreeing with the other two.


Stack a screening tool, an AI leasing assistant, and a resident engagement platform on top of that same PMS, and you don’t get a richer picture of a resident or a unit. You get several partial pictures, each accurate within its own tool and none of them reconciled with the others. That’s the fragmentation Thesis Driven’s piece names directly, and it’s the part that a faster API or a better export format doesn’t touch.


## Why AI pilots are the moment this gap stops being theoretical


Because an AI tool doesn’t know its inputs are fragmented — it just answers anyway. Thesis Driven cites industry figures (MLQ’s research among them) putting AI pilot failure rates well above half, and separate research from JLL finding that most commercial real estate firms piloting AI hit only a fraction of their program goals. The pattern in both: the model wasn’t the constraint. The data feeding it was incomplete, and the tool produced a confident-sounding answer anyway.


That’s a worse failure mode than a broken dashboard, because a broken dashboard is obviously broken. A leasing recommendation or a maintenance-prioritization score built on a fragmented resident profile looks like a normal answer. It just isn’t reliably a correct one, and nothing in the AI layer will tell you that.


## What a real data strategy actually requires


An operator-controlled data warehouse — Snowflake, Databricks, BigQuery, or similar — that you own regardless of which PMS or point tools you’re running this year or next. On top of that: automated pipelines that pull from every operational tool daily, not on whatever cadence the vendor’s report scheduler allows, feeding into one normalized schema where the same entity means the same thing everywhere it shows up.


That combination — owned warehouse, daily automated pipelines, one schema — is what turns “our systems are integrated” into “our data is usable.” Thesis Driven’s reporting names operators like AvalonBay and Bozzuto who built this layer deliberately, before layering AI on top rather than after discovering AI didn’t work without it. The order matters more than the tooling choice.


## Where Propexo fits in that stack


[Propexo Connect](https://propexo.com/connect/) is the extract-and-load layer: it pulls raw data out of Yardi, RealPage, Entrata, AppFolio, and the rest of the multifamily PMS landscape and lands it in your warehouse, as-is. It does not transform or normalize anything in flight — that’s a deliberate boundary, because normalization decisions belong in a layer you can see and audit, not buried inside a connector.


That normalization happens in the[Unified API](https://propexo.com/unified-api/) , which sits on top of the raw, warehoused data and returns a consistent schema across every PMS it covers — so a query for “vacant units” behaves the same way whether the underlying system is Yardi or Entrata. Connect gets the data there without touching it; the Unified API is what makes it comparable once it has arrived. For a walkthrough of what that looks like across three systems at once, see[building a single source of truth across Yardi, RealPage, and Entrata](https://propexo.com/how-to/single-source-of-truth-across-yardi-realpage-entrata/) .


None of this requires waiting for a PMS vendor to open up further. The data your operational tools already produce is enough to build on, once it’s landed somewhere you control and structured one way instead of five.
