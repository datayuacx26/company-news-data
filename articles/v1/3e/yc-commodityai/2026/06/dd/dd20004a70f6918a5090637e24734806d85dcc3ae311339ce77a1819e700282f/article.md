---
schema_version: "1.0.0"
document_id: "dd20004a70f6918a5090637e24734806d85dcc3ae311339ce77a1819e700282f"
company_key: "yc-commodityai"
company: "CommodityAI"
source_id: "yc-commodityai-news-import-2bca2e2104a7"
canonical_url: "https://www.commodityai.com/posts/reconciling-shipping-documents"
published_at: "2026-06-05T09:00:00+00:00"
first_seen_at: "2026-07-24T06:14:02.324494+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:6d0efb91158c8f73922866c2afa8a6aedf17aef18e34134ab6dc1b8b24a25b6f"
---

# A practical guide to reconciling shipping documents with AI

Reconciliation is the unglamorous heart of physical commodity operations. A single grain cargo can generate dozens of documents — bills of lading, weight certificates, quality certificates, statements of fact, invoices — and every pair of them is an opportunity for figures to disagree.


## The three-way check


In practice almost every reconciliation reduces to a three-way comparison: what the contract says, what the documents say, and what the systems say. Agents make this tractable by extracting all three into one structure and diffing them field by field — quantities against tolerance, dates against laycan, parties against the contract, prices against the price basis.


## Designing for exceptions, not perfection


The goal is not a system that never raises a hand. It is a system that raises its hand precisely. A useful reconciliation agent separates three situations:


- Clean matches, which post straight through with the evidence attached.
- Known variances — within tolerance, expected at this stage — which post with a note.
- True breaks, which become tasks for the operator with both versions side by side.


Operators stop being proofreaders and become adjudicators. The work that remains is the work that genuinely needs judgment.


## What to measure


Teams that succeed with this track two numbers from day one: the straight-through rate (documents that needed no human touch) and the time-to-break (how quickly a true discrepancy reached a human). The first measures efficiency; the second measures risk.
