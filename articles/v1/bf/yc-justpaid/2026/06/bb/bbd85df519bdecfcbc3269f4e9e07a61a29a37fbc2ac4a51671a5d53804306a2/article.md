---
schema_version: "1.0.0"
document_id: "bbd85df519bdecfcbc3269f4e9e07a61a29a37fbc2ac4a51671a5d53804306a2"
company_key: "yc-justpaid"
company: "JustPaid"
source_id: "yc-justpaid-news-import-e87a819620a9"
canonical_url: "https://www.justpaid.ai/blog/why-ai-native-erp-is-rewiring-finance"
published_at: "2026-06-01T12:00:00+00:00"
first_seen_at: "2026-07-25T10:25:13.983315+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:fe3f4a3981478a18f3e778c85dad8ec2263dbc3285b326710bfa6e448b356b40"
---

# Why AI-Native ERP Is Rewiring Finance

## You Can’t Bolt AI Onto a 1998 General Ledger. Finance Is Finding That Out the Hard Way.


Every legacy finance tool now has an “AI” badge on its pricing page.


Almost none of them changed the database underneath it.


That gap — between AI on the marketing site and AI in the architecture — is one of the most important stories in finance right now. Stephen Hedlund, Head of Finance at Rillet, recently put his finger on exactly why it matters.


In a conversation about why AI-native ERP is changing finance, Hedlund made the case that you cannot retrofit intelligence onto a system that was never designed to hold it.


Rillet, the AI-native ERP that raised over $100M from Sequoia and Andreessen Horowitz, rebuilt the general ledger from scratch rather than wrapping AI around someone else’s infrastructure.


At JustPaid, we built around the same conviction — one layer down the stack in billing and AR.


So this one hits home.


Here’s why the AI-native rebuild is the real shift, and why “AI-powered” legacy software mostly isn’t.


---


## The ERP Most Companies Run Was Built for a Different Decade


Start with an uncomfortable number: Gartner estimates that through 2027, roughly 70% of ERP implementations will fail to fully meet their original business objectives.


Large transformation projects routinely overrun budgets by triple digits.


These are not edge cases. They are the base rate for the software running most finance teams.


Hedlund’s framing of the problem is sharp: legacy ERPs treat data the way 1998 treated data.


Something you key in.


Store in rigid tables.


Pull out in static reports.


AI needs the opposite.


It needs rich metadata on the way in, deterministic logic in the middle, and a system that can read its own context on the way out.


You cannot get there by adding a chatbot to a 20-year-old schema.


The schema is the problem.


This is the same kind of platform shift NetSuite made with cloud, and the same bet Rillet is now making with AI: when the shift is big enough, you do not patch the old thing.


You rebuild.


---


## “AI-Powered” and “AI-Native” Are Not the Same Product


The distinction Hedlund draws maps cleanly onto how AI actually has to live inside a finance system.


Think of it in three layers.


### 1. Ingestion: How Data Enters


AI-native systems capture deep metadata at the source — the contract, the CRM, the payment — instead of flattening everything into a few generic fields.


That matters because AI is only as useful as the context it can access.


If the system loses the nuance at ingestion, no chatbot can recreate it later.


### 2. Core Processing: What Happens in the Middle


This layer has to be deterministic.


Your close cannot hallucinate.


Math is math.


An AI-native architecture keeps the language model away from ledger arithmetic while still allowing AI to assist with interpretation, explanation, classification, and workflow.


That separation matters.


AI should help finance teams move faster, but it should not compromise accuracy.


### 3. Extraction and Action: How Data Leaves


This is where agents belong.


They can surface anomalies, draft reconciliations, explain variances, recommend next steps, and help finance teams act faster.


But this only works when the system underneath has clean, contextual, structured data.


Bolt-on AI usually only touches this third layer: a copilot stapled to the front of a system that is still rigid underneath.


AI-native means all three layers were designed together.


That is the difference between a feature and a foundation.


---


## The Results Show Up Where Finance Feels Pain: The Close


The proof is not in the architecture diagram.


It is in the calendar.


Hedlund points to a company that went from a 15-day close to a 3-day close in its first month on an AI-native platform.


That is not a tuning improvement.


That is a different category of system.


When ingestion, processing, and extraction are built for AI, the manual reconciliation and chasing that bloat the close can start to disappear.


We see the same physics in billing and AR.


JustPaid customers run a 3x faster month-end close and collect payments 17 days faster because the contract-to-cash flow was built AI-native from day one — not assembled from a CRM, a billing tool, and three spreadsheets held together by a controller’s heroics.


---


## The Buyer Changed, So the Software Has To


There is also a generational point underneath all of this.


The CFO buying finance software today often grew up on consumer-grade tools.


They expect fast onboarding.


Clean interfaces.


Automation that works.


Implementation in weeks, not 12-to-18-month death marches.


The data backs the urgency. In recent surveys, the large majority of CFOs plan to increase AI spend, yet most still have no generative AI inside their finance function — even though nearly all of them believe it would help.


That gap between intent and reality is exactly what AI-native vendors are built to close.


Rillet is closing it at the ERP layer.


JustPaid closes it at the billing and AR layer with a 3-to-7 day implementation instead of weeks — because there is no legacy migration to fight.


---


## What This Means for Your Finance Stack


If you are evaluating finance tools in 2026, Hedlund’s lens is the right one to borrow.


### Ask Where the AI Lives


Is it in the demo, or is it in the data model?


If the vendor cannot explain how AI changes ingestion and processing — not just the chat box — it is probably a retrofit.


### Judge by the Close, Not the Feature List


The honest benchmark for any AI claim in finance is whether it shortens the close and the cash cycle.


A long feature list does not matter if the finance team is still exporting CSVs, chasing approvals, and reconciling manually.


### Weight Implementation Time Heavily


A 12-month rollout is often a signal that you are buying yesterday’s architecture.


Weeks, not months, is a signal that the system was built for this era.


### Stack AI-Native With AI-Native


An AI-native ERP like Rillet paired with AI-native billing like JustPaid beats a legacy core with a dozen bolt-ons because the data stays rich end to end.


It does not flatten at every handoff.


That is where the real leverage comes from.


---


## The Takeaway


The takeaway from Stephen Hedlund’s argument is bigger than ERP.


The entire finance stack — ledger, billing, AR, FP&A — is getting rebuilt from the database up.


The teams that win the next decade will not be the ones that added AI to old systems.


They will be the ones running on systems where AI was never optional.


---


## Frequently Asked Questions


## What does “AI-native ERP” actually mean?


An AI-native ERP is built with AI embedded in its core architecture — from how data is ingested and processed to how insights are surfaced — rather than having AI features added to a legacy system after the fact.


As Rillet’s Stephen Hedlund argues, this requires rebuilding the general ledger and data model, not bolting a copilot onto an old database.


## Why can’t legacy ERPs just add AI?


Legacy ERPs store data in rigid schemas designed decades ago for manual entry and static reporting.


AI needs rich metadata on ingestion and flexible, deterministic processing — capabilities the old data model often cannot provide.


That is why retrofitted “AI-powered” tools typically only add surface-level features like chatbots.


## How is AI-native billing different from AI-native ERP?


They operate at different layers of the finance stack.


An AI-native ERP like Rillet rebuilds the general ledger and core accounting layer.


AI-native billing like JustPaid rebuilds the contract-to-cash flow — extraction, invoicing, collections, and reconciliation.


Both share the same principle: AI belongs in the architecture, not stapled on top.


## What results does AI-native finance software deliver?


The clearest signal is a faster close.


Hedlund cites a company moving from a 15-day close to a 3-day close in its first month on an AI-native platform.


JustPaid customers see a 3x faster month-end close and collect payments 17 days faster.


## Get Started with JustPaid


Automate invoicing, streamline accounts receivable, and accelerate revenue with JustPaid.[Compare JustPaid pricing plans](https://www.justpaid.ai/pricing) or book a walkthrough.


[Get started • Schedule demo](https://www.justpaid.ai/demo)
