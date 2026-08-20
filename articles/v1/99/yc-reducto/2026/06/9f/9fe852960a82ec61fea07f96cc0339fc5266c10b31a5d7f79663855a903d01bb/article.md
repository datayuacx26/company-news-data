---
schema_version: "1.0.0"
document_id: "9fe852960a82ec61fea07f96cc0339fc5266c10b31a5d7f79663855a903d01bb"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/reducto-deep-split-agent"
published_at: "2026-06-03T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:8450a6c63796df487b098143d40c1267c26da5dcc4a6f9112043ea60a84853be"
---

# Deep Split: Utilizing Agent Harnesses for Accuracy at Scale

Companies today often manually sift through documents to categorize data. Long statements that have pages that relate to separate accounts, disclosures, or supporting schedules all in one file need to be separated. Other times, they’ll look through mixed packets that are one file, but contain different types of documents entirely.


This is costly and time-consuming, but necessary: before you can extract the right data, you need to separate it into the right sections.


That’s what Split does, automatically. We identify which page ranges belong to which categories based on natural-language descriptions, returning page numbers for each one.


And today, we’re announcing new improvements that make split even better.


## A new way to split


We’re re-introducing Split with a new implementation built on the same agent harness architecture behind[Deep Extract](https://reducto.ai/blog/reducto-deep-extract-agent) . We use an agentic loop with sub-agents to break down page categorizations, utilizing our knowledge of the source document.


This agent harness method allows us to both handle longer documents and support workflows with large numbers of categories. In early testing, customers have uploaded documents containing thousands of pages and 150+ categories that existing solutions would previously struggle to handle.


And much like extract citations, we now also provide contextual evidence for why a page was assigned to a certain category, making the result easier to trust, debug, and iterate on.


## Why it matters


When the right pages are grouped together, downstream extraction runs on cleaner, more relevant context. You’re not passing in irrelevant pages or conflicting information, or sending your pages to the wrong downstream pipeline entirely. That means faster processing, more accurate outputs, and less effort spent recovering from pages that were routed incorrectly.


A major financial firm goes one step further: they use our new Split for data organizational purposes, scanning entire packets, each hundreds of pages or more all in one file, across hundreds of potential categories. They’ve unlocked entire sets of data and product features that were previously inaccessible.


It also matters in document sets with many similar-looking sections. Think of compliance packets, loan files, or insurance documents, where adjacent pages may contain data across repeated headers and formats with duplicate groups of accounts. In those workflows, better separation helps ensure each page is handled with the right schema and the right context from the start, regardless of input. You won’t always know how many named parties are in a document like this, or chapters in a book. Split takes care of it for you.


## Get Started with Split


You can try it today through our API along with its corresponding[documentation](https://docs.reducto.ai/configs/split/deep-split) , or in Studio at[studio.reducto.ai](http://studio.reducto.ai/) . Deep split is priced at 4 credits per page; you can find more information[here](https://docs.reducto.ai/reference/credit-usage) .


Want to see how Split can work for you? Reach out to[reducto.ai/contact](http://reducto.ai/contact) to request a demo or ask any questions, and we’d be happy to help with your use case if Split sounds right for you.
