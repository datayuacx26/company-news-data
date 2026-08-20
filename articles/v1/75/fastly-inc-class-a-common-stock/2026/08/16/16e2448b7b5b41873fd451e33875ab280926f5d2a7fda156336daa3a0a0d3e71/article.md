---
schema_version: "1.0.0"
document_id: "16e2448b7b5b41873fd451e33875ab280926f5d2a7fda156336daa3a0a0d3e71"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/ai-content-provenance-c2pa-support-with-image-optimizer/"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T15:34:29.692401+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:e8b5de695c3d46edf73777c78943c99d5977b2b645378be715091fce0a4d10c2"
---

# AI Content Provenance is Gaining Momentum: Get C2PA Support Now with Image Optimizer

On August 2, 2026, content provenance stopped being a best practice and became the law. That's the date California's[SB 942 (the California AI Transparency Act)](https://leginfo.legislature.ca.gov/faces/billTextClient.xhtml?bill_id=202320240SB942) became enforceable, with[Article 50(2) of the EU AI Act](https://artificialintelligenceact.eu/article/50/) following after a short 3-month grace period. These laws carry real penalties for providers of AI that generates or alters media without provenance metadata identifying the content as AI-generated. While not all organizations are subject to these specific laws, transparency and related regulations are getting a lot of attention and increasing adoption globally. Consumers are also pushing for better transparency. According to a recent[report](https://emplifi.io/press/emplifi-survey-digital-authenticity-ai-report/) , more than 90% of consumers expect brands to disclose AI usage in marketing.


This may seem daunting, but there’s good news! Fastly Image Optimizer can help support your ability to preserve and pass through content provenance data through your delivery pipeline. C2PA support for the latest spec (2.4) is now available to all[Image Optimizer](https://www.fastly.com/products/image-optimizer) customers, across all image formats (except JPEG XL), and still at no additional cost.


## Why this matters now


Regulators didn't invent these rules in a vacuum. Generative AI made it trivial to produce a photorealistic image in seconds, and just as easy to pass one off as real. When anyone can generate a convincing photo of an event that never happened, "seeing is believing" stops being a safe assumption for your readers, your customers, and your brand. Brand trust is at a premium.


## Where C2PA fits


Content provenance methods, such as C2PA, are designed to address this problem. Instead of trying to detect fakes after the fact, C2PA attaches a verifiable record to the file itself: what made it, when, and how it changed along the way. That shifts the burden from "prove this is fake" to "here's exactly where this came from."


The[C2PA (Coalition for Content Provenance and Authenticity)](https://c2pa.org/membership/) is an authority on content provenance, and its metadata spec has effectively become an industry baseline. When people say "provenance metadata," they usually mean C2PA. Fastly and many of our customers are members, because a trustworthy internet is a group project.


## Provenance that survives transformation


Here's the hard part most teams run into - with C2PA anything that touches an image, such as resizing, reformatting, or optimizing, can strip or invalidate metadata. That's a problem when your delivery pipeline exists to transform images.


Image Optimizer solves this with C2PA pass-through built directly into the transformation path. Your provenance metadata is preserved through the image optimizations you already run, which means:


-


**Provenance that persists through transformations.** Resize, reformat, and optimize while the C2PA manifest travels with the image, along with a new attestation added by Image Optimizer.


-


**Broad format coverage.** It works across the formats you actually use; all image formats (except JPEG XL) supported by Image Optimizer are also supported for C2PA.


-


**No rework or enablement required.** Keep your current setup and follow[the docs](https://www.fastly.com/documentation/reference/io/metadata/) to use pass-through C2PA.


-


**Use the tools you already have.** Any standard C2PA metadata, up to version 2.4, in your origin files can be passed through Fastly Image Optimizer and re-signed with additional provenance data for the transformations made by our systems.


## Available for all, starting now


Last year, we released[C2PA support in Image Optimizer](https://www.fastly.com/blog/trust-at-scale-with-fastly-image-optimizer-and-c2pa) with support for JPEG. What's changed is that C2PA support is now more easily available for everyone, we now support the latest C2PA spec, and we’ve expanded the supported image formats. Now, every Image Optimizer customer can use C2PA without requesting enablement; it's simply part of the product. If you're on IO, you're ready to get started.


## How it works


To make images as small as possible, Image Optimizer’s default behavior is to strip the embedded metadata from processed images. The[metadata](https://www.fastly.com/documentation/reference/io/metadata/) parameter is used to select which type of metadata to pass through the optimization. New` c2pa` and` copyright,c2pa` values for the` metadata` parameter instruct Image Optimizer to preserve the C2PA manifest.


Keep in mind that the C2PA manifest will result in the optimized image being slightly larger than if the metadata was stripped out. The exact size will depend on the C2PA manifest in the source image and how many attestations it already includes. Image Optimizer’s attestation will add about 12.5 KB plus the size of an embedded thumbnail, depending on the changes that are made to the image.


## What's next


This is a foundation, not a finish line. We continuously evolve our products to adapt quickly to regulatory requirements and C2PA standards. We're already exploring external C2PA manifests so you get full provenance without metadata increasing file sizes, and latency.


## Ready to get started?


If you're already using Image Optimizer, C2PA support is available. Check the[metadata documentation](https://www.fastly.com/documentation/reference/io/metadata/) to put it to work. If you're not,[talk to an expert](https://www.fastly.com/contact-sales) today.
