---
schema_version: "1.0.0"
document_id: "947f0caec2111719805f10d05225a143dfd976701a38ff52cfa5cbab71247cbe"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/anydoc-and-pdf-inspector"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T09:56:10.567029+00:00"
fetched_at: "2026-08-06T09:56:12.501743+00:00"
content_hash: "sha256:9cf34e256e9b2a24eb79c1e9b55117321067a181448641fa83a26d520c4eae05"
---

# Introducing AnyDoc and pdf-inspector: Firecrawl's open-source document parsing stack

No single library reliably converts every common document format to clean markdown. You end up stitching four or five tools together, each with its own dependencies, output shape, and failure modes.


**So we built two[Firecrawl](https://www.firecrawl.dev/) projects to solve it:[pdf-inspector](https://github.com/firecrawl/pdf-inspector) for PDFs, and[AnyDoc](https://github.com/firecrawl/anydoc) for everything else.** Two Firecrawl open-source Rust libraries, one lineage, deliberately separate repos. Between them they cover the document formats a real AI pipeline actually sees, with no API key and no system dependencies.


*P.S. Both libraries already power document processing in Firecrawl's[/parse](https://docs.firecrawl.dev/features/parse) and[/scrape](https://docs.firecrawl.dev/features/scrape) endpoints. If you use those, you're already using them.*


## Firecrawl pdf-inspector


### What is Firecrawl pdf-inspector


[pdf-inspector](https://github.com/firecrawl/pdf-inspector) is a from-scratch Rust library for PDFs, with **1.9M views and 8.1k GitHub stars** on the repo at time of writing. It reads a PDF's internal structure (font encodings, text operators, image coverage) in milliseconds, without rendering anything, and decides per page whether the content is text-based or needs OCR.


- **Text-based pages** get native extraction directly, with reading order preserved.
- **Scanned or image-heavy pages** are flagged with the reason, so a vision pipeline can pick them up.


For a fully text-based PDF, pdf-inspector alone is the entire pipeline. For mixed documents, it's the smart router that decides what actually needs a GPU.


### Why it matters on its own


Most PDF pipelines make the same wrong bet: treat every page as if it might be scanned, so route everything through OCR. That's slow, expensive, and often less accurate than the native text that was sitting in the PDF the whole time.


pdf-inspector fixes the routing layer:


- **Per-page classification.** Analyzes internal structure only. No rendering, no GPU, milliseconds per page.
- **Native text extraction.** Pulls text directly from text-based pages with reading order intact.
- **Clean handoff for the rest.** For scanned pages, it hands back the page reference and the reason, so an OCR pipeline can do the heavy lifting only where it's needed.


That routing layer is what makes[Fire-PDF](https://www.firecrawl.dev/blog/fire-pdf-launch) , Firecrawl's hosted PDF parsing engine, 3.5x to 5x faster than the previous pipeline: for a 200-page report where 150 pages are pure text, 150 pages skip GPU entirely.


### How to use it


Add pdf-inspector to your Rust project directly from the repo. The README covers the classifier and native-extraction APIs, plus the reproducible-results branch if you want to benchmark it against your own PDFs.


Or don't integrate it directly at all: send PDFs to Firecrawl's[/parse](https://docs.firecrawl.dev/features/parse) or[/scrape](https://docs.firecrawl.dev/features/scrape) and they go through pdf-inspector (and Fire-PDF for the scanned pages) automatically.


---


## Firecrawl AnyDoc


### What is Firecrawl AnyDoc


AnyDoc is the other half of the stack, also a from-scratch Rust library, also markdown out. It converts documents with a single call:


```text
anydoc  ::  to_markdown  (  "file.docx"  )
```


It supports 14 formats in one binary:


- docx
- doc
- docm
- xlsx
- xls
- xlsm
- pptx
- ppt
- rtf
- odt
- ods
- odp
- epub
- csv


No API key. No system dependencies. Nothing to install alongside it.


### Why it matters


No single existing library reliably covers every common document format. Each one handles a subset, and the formats it doesn't cover become someone else's dependency, with different output, different failure modes, and often much slower conversions. AnyDoc handles all 14 in one dependency-free library.


Who feels this most: the engineer whose users upload "whatever they have": a .docx contract, an .xls export from 2009, a pitch deck, an .epub. Today, getting usable text out of all of it is a plumbing project. AnyDoc is the part that stops being plumbing.


It's also quickly become a dev favorite. Here's[Garry Tan](https://x.com/garrytan) on it:


Here's[Nick](https://x.com/nickscamara_) , Firecrawl's Co-founder and CTO, on how AnyDoc is different from pdf-inspector:


### How it compares


We benchmarked AnyDoc against the common alternatives on 94 documents spanning all 14 formats.


- **Coverage: 14 of 14.** AnyDoc is the only library in the benchmark that parses every format. The nearest alternative, LibreOffice, covers 12 of 14. Every other option covers a subset, which is exactly why teams end up stitching multiple libraries together.
- **Speed: 4.6ms median per document.** Across all 14 formats, AnyDoc's median conversion time is 4.6ms. The alternatives run between 52ms and 1,130ms, roughly 20x to 245x slower depending on the tool. LibreOffice sits at the slow end of that range.
- **Quality: highest overall, with honest caveats.** Quality is LLM-judged on completeness, structure, formatting, and cleanliness. AnyDoc scored highest on every format we tested, overall score 80 vs 68 for the next-best option. Two notes worth being upfront about: the corpus is ours, and mammoth scores higher than AnyDoc on completeness alone (95 vs 87) on the single format it supports (docx). If completeness on docx is the only thing you care about, that gap matters. If you care about coverage across every format your users actually upload, AnyDoc wins the aggregate.


### How to use it


Add AnyDoc to your Rust project, then call it on any supported document:


```text
use   anydoc;


let   markdown   =   anydoc  ::  to_markdown  (  "contract.docx"  )  ?  ;
```


That's the whole integration. No API key, no external service, no separate binary. JavaScript bindings are in progress; we'll share those when they land.


Or don't integrate anything at all: Firecrawl's[/parse](https://docs.firecrawl.dev/features/parse) and[/scrape](https://docs.firecrawl.dev/features/scrape) endpoints already use AnyDoc automatically when they encounter a non-PDF document.


---


## Two libraries, one shape


Same principles across the pair: from-scratch Rust, local execution, no API key, no system dependencies, markdown out. That's not a coincidence. It's the shape developers kept asking for after pdf-inspector shipped, and it's the shape that makes both libraries safe to drop into any pipeline without dragging in a heavy runtime.


The two are deliberately separate repos and deliberately separate products. AnyDoc is not "pdf-inspector grown up." pdf-inspector handles PDFs. AnyDoc handles everything else. One lineage, two Firecrawl products, and together they cover the document formats a real AI pipeline actually sees.


## Try the stack


- **pdf-inspector** (PDFs):[github.com/firecrawl/pdf-inspector](https://github.com/firecrawl/pdf-inspector)
- **AnyDoc** (14 non-PDF formats):[github.com/firecrawl/anydoc](https://github.com/firecrawl/anydoc)
- **Use both via API** : send any document to[/parse](https://docs.firecrawl.dev/features/parse) or[/scrape](https://docs.firecrawl.dev/features/scrape) . PDFs go through pdf-inspector (and[Fire-PDF](https://www.firecrawl.dev/blog/fire-pdf-launch) ); everything else goes through AnyDoc. No configuration.


If you're already stitching four libraries together to cover the document formats your users upload, we'd love to hear how the two of these hold up as a replacement.
