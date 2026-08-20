---
schema_version: "1.0.0"
document_id: "911c1ba07ac7e44ed3875ec9b37e8b857516e35c1ed3e470fd007c71266e2738"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/reducto-deep-extract-agent"
published_at: "2026-04-06T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:73d2a8d096552016a369b0a2a63bc8b496862caa501c66264c85870d5a3592c8"
---

# Introducing Deep Extract

Today we’re launching our most powerful update yet for structured extraction: Deep Extract.


Deep Extract is a new agent harness approach to extraction that verifies and corrects its own output until the results are accurate. Much like human-in-the-loop, Deep extract has an agent-in-the-loop, offloading the human reviewer’s burden with an autonomous verification cycle that holds itself accountable for accuracy.


This is particularly powerful when you're dealing with a long list of items to extract — think invoice line items, brokerage statement transactions, equipment manifests, and more. Deep Extract has already extracted over 28 million fields on documents up to 2,500 pages long in our production beta, and we're continuing to expand what's possible.


For the documents that matter most, it gets to 99–100% field accuracy, even out-performing expert human labelers on extraction tasks.


## The challenge with long extraction solutions today


Over the past year, we kept hearing the same thing from customers. Their existing extraction pipelines were breaking down on long, complex documents — invoices running dozens of pages, financial statements spanning hundreds. However, totals didn't reconcile, and it flagged to teams that line items were dropped completely.When we asked how they were handling it, the answer was almost always the same: they'd hired people to have a human-in-the-loop (HITL) manually check the output.


The issue isn't that models are bad at reading documents. It's that single-pass extraction has no mechanism to catch its own mistakes, and models get lazy. Models are prone to shortcuts on long, repetitive tasks. Given a thousand line items to extract, they'll often stop short, consolidate, or skip entries rather than working through every last row.


This is amplified even more when citations are needed. For many of our customers, citations are not just a nice to have, but a need in order to prove their outputs.


## Reducto’s agent harness approach


The rise of long-horizon agents and agent harness architectures pointed to a better way. If agents could reliably tackle complex, multi-step tasks in other domains, the same approach should work for extraction: break the problem down, verify the work, and iterate until it's right.


Deep Extract brings that same discipline to automated extraction. Instead of a single pass, it runs an agentic loop: extract, verify the results against the source document, identify what's missing or inconsistent, and re-extract until the output meets a defined quality threshold.


Rather than treating a complex document as a single monolithic task, Deep Extract deploys sub-agents to break it down and conquer each piece, which is what allows it to remain accurate even on documents with thousands of rows across hundreds of pages.


The key is that you can define what correct looks like, directly in your system prompt. Without one, Deep Extract can still intelligently determine one that could suit the task the best.


For an invoice, that might be: "ensure all line items sum to the stated total." For a financial statement: "verify that assets equal liabilities plus equity." Without this, the alternative is a person manually checking every field — a process that could take hours or even days depending on the length of the document.


With the citations flag enabled, the output also contains granular bounding boxes for all the fields extracted. This can be incredibly powerful for audit trails, human review workflows, and any application where you need to trace an extracted value back to its exact location in the original document.


## What Deep Extract unlocks in real production cases


Through our beta testing period, we worked closely with Reducto design partners to make sure Deep Extract was effective with real-world documents and use cases. Many of their engineering teams had tried all the other solutions on the market, but to no avail.


Some other use cases included extraction from:


- A county’s payment report with transmittal number, check number, price, description, pay date and more
- Active exchange positions reports with symbol, cost basis, and unrealized gain/loss
- Agricultural invoices with payment details like invoice number, CHQ number/date, bill amount, deduction, net, and more
- Cattle sales invoices, county payment approval reports, residential permit applications, and job detail reports


Each line item could have 10+ columns to account for, with thousands of pages per document. We've seen customers go from 10-20% field accuracy with a frontier model to 99-100% just by switching to using Reducto’s Deep Extract.


Because Deep Extract is doing more work, it takes longer than a standard extraction call. That said, measured against the real alternative of someone manually reviewing a 500-page fund statement field by field, it's faster, cheaper, and consistent at scale.


## Get started today


Deep Extract is available now as a configuration for our Extract endpoint. Enable it by setting deep_extract: true in your extract settings and optionally adding verification criteria to your system prompt.


**For Developers:** Full documentation at[docs.reducto.ai](https://docs.reducto.ai/configs/extract/deep-extract) .


**For Enterprise teams** : If you're processing high-stakes documents at scale and want to talk through whether Deep Extract is the right fit, reach out to us directly.


We’re excited to continue pushing on the frontiers of our interactions with documents.
