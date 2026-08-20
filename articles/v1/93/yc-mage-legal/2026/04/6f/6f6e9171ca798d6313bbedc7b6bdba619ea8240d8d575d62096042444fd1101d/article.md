---
schema_version: "1.0.0"
document_id: "6f6e9171ca798d6313bbedc7b6bdba619ea8240d8d575d62096042444fd1101d"
company_key: "yc-mage-legal"
company: "Mage Legal"
source_id: "yc-mage-legal-news-import-389f7de0f02c"
canonical_url: "https://magelegal.com/blog/mage-product-walkthrough"
published_at: "2026-04-26T00:00:00+00:00"
first_seen_at: "2026-07-22T03:05:30.029696+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:a5481e11fd3c19f93aef40119f07a0e06f39258b2490ee6a4c1687e856f7fbe6"
---

# Mage Product Walkthrough: What the Tool Actually Does

This is a walkthrough of what Mage actually does on a real deal. Written as a sequence — Day 1 to closing — not as a feature list. The point is to give an attorney evaluating the tool a concrete picture of the work day-by-day.


## Day 1, morning: access and configuration


You log in. The first thing the system asks is what deal this is and what kind: buy-side, sell-side, financing, PE add-on. The answer drives the default risk checklist and the suggested workflow.


You point Mage at the data room. Box and Dropbox connect directly via OAuth. For everything else (custom secure data rooms, raw zip files, folder uploads), there's a direct upload path. Credentials don't live in the tool.


Ingestion starts. For a mid-market data room with 1,000-3,000 documents, ingestion completes in 30-60 minutes. You configure the risk checklist while it runs. The default for an M&A buy-side review covers about 30 standard risks; you adjust based on what this target needs (industry-specific carve-outs, unusual issue priorities, jurisdiction-aware filters).


By the time ingestion is done, every document has a category, a deal-relevance score, and a position in the priority queue. Documents the system isn't sure how to classify go to a human-review pile rather than getting routed to the wrong extraction.


## Day 1, afternoon: triage


You see the classified data room. Folders are organized the way M&A teams actually organize them: by document type and by deal-relevance, not by the data room's original folder structure. The top-tier folder has the material commercial contracts, the financing documents, the founder IP assignments, the corporate organizational documents, the senior employment agreements.


You spot-check classifications. Click through ten documents in each top-tier folder, confirm the system has them right. If something looks miscategorized, fix it; the correction propagates to similar documents.


You start the first-pass risk review. The system runs the configured risk checklist against every document overnight. There's nothing for you to do during this part except go home.


## Day 2, morning: partner-reviewable findings


You come in to a sortable, filterable findings view. Every contract has its findings, with severity (high, medium, low), confidence scoring, the source-clause snippet, and a suggested human-review target.


Common pattern: 1,500 documents, 150 high-severity findings, 400 medium, 800 low. The associate filters to high-severity, walks through each one, and either accepts (it's a real issue) or rejects (false positive, system was wrong). The work is reviewing rather than reading from scratch.


Amendment chains have already been resolved. A multi-amendment MSA shows up with a single composite view: current operative termination provision, current operative limitation of liability, current operative anti-assignment language. Each finding cites which amendment introduced the operative term. The associate doesn't reconstruct the chain; the system did.


The partner walks in around mid-morning. They see the same findings view, filter to high-severity, push back on three or four findings the system flagged that they consider boilerplate. The associate updates. Within an hour the team has a triaged issues list.


## Day 2, afternoon: gap analysis and request list


The system also identifies what's missing from the data room. Top-customer contracts that weren't uploaded, IP assignments for senior engineers that aren't there, lease agreements for known sites that aren't documented. This becomes the first information request to seller's counsel.


Gap analysis sounds straightforward but it's the kind of thing that gets missed in manual review because the team is busy reading what is there. Surfaced systematically, it saves real time downstream.


## Day 3-4: memo and schedule


The system drafts the deal memo from the findings, in the firm's voice. Structure is partner-grade by default: executive summary at the top (one page), material findings by category in the middle (financial, IP, employee, regulatory, real property, commercial), outstanding diligence requests at the bottom.


The associate edits. The bar is "edits the language, not the substance." For a mid-market deal, the memo edit pass takes 8-12 hours rather than the 25-40 hours of from-scratch drafting. The partner reviews. By end of day 4, the memo goes to the client.


Sell-side: the system also drafts the disclosure schedules from the underlying source agreements. Section 3 schedules — material contracts, IP, employees, real property, debt, litigation — get drafted with citations to source. The associate verifies. Disclosure schedule prep drops from 80-120 hours to 20-30.


## Through closing: redline and covenant tracking


Once the deal moves into negotiation, the system compares each round of counterparty markups against the firm's preferred positions. Material deviations surface; cosmetic changes get filtered. The associate reviews, the partner approves the negotiation positions, the response goes back.


The closing checklist runs in the system from signing through closing. Every condition, every consent, every delivery, every interim covenant gets tracked. The standard "closing checklist as a spreadsheet" gets replaced with a live view that updates as items move.


Interim covenants are the place deals come unglued. We wrote about this in[Signing-to-Closing Interim Covenants](https://magelegal.com/blog/signing-to-closing-interim-covenants) . The system tracks them.


## The honest demo


Vendor demos are designed to win. We run demos on real deals — yours, not ours. The standard pattern: you give us access to a recent or current deal, we ingest it, run the full workflow, and walk you through the result. You compare against your manual workproduct on that deal. The decision after that is usually obvious in either direction.


To see this on a real deal:[request a demo](https://magelegal.com/?demo=1) .


For the broader workflow context:[AI Due Diligence: An Operational Playbook](https://magelegal.com/blog/topics/due-diligence) . For how Mage compares to other tools:[Legal AI vs. Harvey vs. Generic AI](https://magelegal.com/blog/harvey-vs-kira-vs-infrastructure-legal-ai) .
