---
schema_version: "1.0.0"
document_id: "26fda5c13d1516ac358f1edc70bfb5c15138a87929938876690435ef11b2bbef"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/introducing-classify"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-23T21:33:19.979240+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:e2c723b17877c1d710ad6f48cb2cbb58b06f9bf6c97fde5f5bfc18531520dd83"
---

# Introducing Classify

Most document pipelines start with a question nobody has answered yet: what is this file?


A mixed intake folder might contain bank statements, invoices, contracts, and tax forms, all waiting on the same generic extraction flow. Running one broad extraction over everything means paying for work you don't need, applying the wrong schema to half your documents, and cleaning up the results downstream. The pipeline isn't failing at extraction. It's failing one step earlier, at understanding what it was handed in the first place.


Classify fixes this by adding a lightweight decision step before extraction. It answers "what is this?" so the rest of your pipeline can do "extract everything from it" properly.


## **How It Works**


You define the categories that match how your business thinks about documents, such as bank_statement, invoice, contract, tax_form, or other, along with descriptions of each. Classify inspects the document and returns the best match.


Each category can map to a pipeline_id, so classification doubles as routing. A bank statement flows into your bank-statement pipeline, an invoice into your invoice pipeline, and a contract into your contract pipeline, with no manual sorting in between. What used to require a human triage step, or a brittle set of filename rules, becomes a single API call at the front of your intake flow.


Because the categories and descriptions are entirely yours to define, Classify matches the way your business actually talks about documents rather than forcing you into a fixed taxonomy. If your team distinguishes between a credit memo and an investment document, or between a vendor bill and a receipt, Classify can make the same distinction.


## **Why It Runs Before Extraction**


Classify sits in front of /extract, and that ordering matters more than it might seem. Extraction settings often depend heavily on document type: the schema you apply, how you chunk the content, how you handle tables, and what downstream processing or review the output needs can all vary by category. When every document runs through the same generic configuration, you're forced to build for the lowest common denominator and accept the noise that comes with it.


With Classify as the first step, everything downstream can adapt. The pipeline identifies the document, then continues with the right extraction, the right schema, the right table handling, and the right review workflow for that specific type. You stop compromising on configuration because you finally know what you're configuring for.


## **Built for Speed and Cost**


Classification only makes sense as a first step if it's fast and cheap, so we designed it that way. For PDFs and images, Classify can inspect a small page range instead of OCRing the whole document, which means you learn what a file is without paying to process all of it. Teams no longer need to run expensive or overly broad extraction logic just to find out what they're working with, and the savings compound quickly at volume.


## **What Teams Are Routing**


The use cases we see most often include bank statements versus invoices versus contracts, tax forms versus payroll documents, insurance claim packets versus financial statements, credit memos versus investment documents, resumes versus onboarding forms, and vendor bills versus receipts.


These are exactly the distinctions that matter in real intake workflows, where documents arrive mixed together from clients, vendors, and internal teams. In every case the pattern is the same: messy, unpredictable input goes in, and structured, predictable flows come out.


## **A Smarter First Step**


Classify makes automated document intake cleaner end to end. Upload a folder of mixed files, classify each one, and route them into the right extraction, schema, table, or review workflow without anyone touching the documents by hand. It's a small step at the front of the pipeline that removes wasted work everywhere behind it, and it turns document intake from something teams babysit into something that simply runs.


## **Available now in the Pulse platform.**


*Want to see Classify work on your documents?*[Chat with our team here](https://www.runpulse.com/contact-sales) .
