---
schema_version: "1.0.0"
document_id: "ee1f32f4cdc159667d258b1035aed2b167beaff1f9f1a9313fdaa467e3c03f34"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/fire-pdf-launch"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-21T20:06:32.273956+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:9acd0f3f810e5ac59fa225d4906858dd5afa9435443374d1872cc7b49846bcda"
---

# Introducing Fire-PDF: Firecrawl's New PDF Parsing Engine

PDF parsing has always been one of the hardest parts of web scraping. Most PDFs aren't simple text — they contain scanned pages, multi-column layouts, tables, formulas, and mixed content. Until now, every solution forced a tradeoff: fast but inaccurate, or accurate but too slow to run at scale. **Today we're shipping Fire-PDF, a PDF parsing engine built to eliminate that tradeoff.**


*P.S. Every PDF sent through our API now goes through Fire-PDF automatically. No configuration needed.*


## What is Fire-PDF?


Fire-PDF is our new Rust-based PDF parsing engine.


It converts any PDF — scanned, text-based, or mixed — into structured markdown. Our open-source Rust library[pdf-inspector](https://github.com/firecrawl/pdf-inspector) classifies each page in milliseconds. Text-based pages go straight to native extraction, skipping GPU entirely. Only scanned or image-heavy content hits the neural layout model and OCR.


The result is clean markdown with correct reading order, preserved tables, formulas in LaTeX, and proper multi-column structure.


## How Fire-PDF makes web data extraction 5x faster


Compared to our[previous PDF parser](https://www.firecrawl.dev/blog/pdf-parser-v2) , Fire-PDF is 3.5-5.7x faster — averaging under 400ms per page.


Speed comes from two places:


- First, text-based pages never touch GPU — they get native extraction through pdf-inspector in milliseconds.
- Second, the GPU fleet uses lane-based routing to isolate requests by document size, so a 200-page report never impacts latency for a single-page invoice.


### Smarter about what hits GPU


Most documents are not fully scanned. A financial report might have 150 text-based pages and 60 scanned ones. With our previous pipeline, all of it went through OCR.


With Fire-PDF, only the pages that *need* it do.


[pdf-inspector](https://github.com/firecrawl/pdf-inspector) is our open-source Rust library that classifies every page by analyzing PDF internals — font encodings, text operators, and image coverage — in milliseconds, without rendering.


- Text-based pages get instant native extraction.
- Only scanned or image-heavy pages go through GPU.


For mixed documents, this can eliminate GPU processing for the majority of pages, which translates directly into **faster processing** and **lower cost** .


### Layout-aware accuracy for complex documents


Speed alone isn't enough if tables come out garbled or multi-column text comes out of order. Fire-PDF uses a neural document layout model to detect text blocks, tables, formulas, images, headers, and footers individually — then handles each region type correctly.


- **Tables** get higher token limits and up to 25 seconds to generate accurate markdown table output
- **Formulas** get formula-specific prompts and are preserved in LaTeX
- **Text regions** get tight 12-second, 256-token budgets for efficiency
- **Reading order** is predicted neurally, with XY-cut projection as a fallback for multi-column layouts


Each region gets tuned parameters rather than one-size-fits-all OCR. The difference shows on documents that previously came through as jumbled text. Think: financial tables, academic papers with equations, legal filings with dense columns.


## How the pipeline works


Fire-PDF runs every PDF through five stages:


1. **Classify** — pdf-inspector scans the PDF's internal structure in milliseconds, classifying each page as text-based or needing OCR
2. **Render** — Pages needing OCR are rendered to images at 200 DPI. Oversized pages are automatically capped or sliced
3. **Layout Detection** — Rendered images go through a neural document layout model on GPU, returning bounding boxes, element types, and reading order
4. **Extraction** — Text-based pages use native extraction (no GPU). Scanned regions are sent to a vision-language model (GLM-OCR) with task-specific prompts and parameters per region type
5. **Assembly** — Results are sorted by reading order and assembled into markdown. Tables become markdown tables. Formulas are preserved in LaTeX. Geometric deduplication removes overlapping detections


## Try it today


Fire-PDF is live for all Firecrawl users. Every PDF you send through the API uses it automatically - no configuration required.


[Get started with Fire-PDF](https://www.firecrawl.dev/) ·[Read the docs](https://docs.firecrawl.dev/features/document-parsing)
