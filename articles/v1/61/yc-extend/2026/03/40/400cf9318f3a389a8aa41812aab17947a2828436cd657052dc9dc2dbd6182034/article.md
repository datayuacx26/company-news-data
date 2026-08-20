---
schema_version: "1.0.0"
document_id: "400cf9318f3a389a8aa41812aab17947a2828436cd657052dc9dc2dbd6182034"
company_key: "yc-extend"
company: "Extend"
source_id: "yc-extend-news-import-054f4f06cd55"
canonical_url: "https://www.extend.ai/resources/mercury-case-study"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-21T19:22:10.257736+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:7e2293ba82de18ddee5157d04f5194140fdceee39c5b5092353893092bc00473"
---

# How Mercury shipped low-latency document processing for 300K+ users

> We tested AWS, foundation models, and newer vendors, and evaluated options on latency, accuracy, and the developer experience. Extend was the only one that could hit our performance bar, with the tooling we needed to scale.
>
>
> **Paul-Arthur Asselin, Senior Software Engineer**


[Mercury](https://mercury.com/) is radically different banking*: it merges premium software with banking to help 300K+ entrepreneurs accomplish everything they want with their money. In 2025, the company hit $650M in annualized revenue and more than $20 billion in monthly transaction volume.


Every customer starts by onboarding onto Mercury's platform, which requires formation docs, identity verification, and more. These documents are extremely varied and have infinite formats, and the bar for accuracy is high. At the same time, low latency is critical during user-facing flows where customers have a risk of dropping off if the document validation takes too long.


Here's how Mercury's engineering team shipped real-time document upload in their onboarding flow, delighting their users and saving weeks of engineering effort.


## **TLDR: What Mercury's journey shows**


If you're building real-time document validation into a user-facing flow, three things stand out:


- **Latency is hard at scale.** For low p95 latency at scale, you need a dedicated engine built for low latency, multiple fallbacks to protect against model instability, and evals to measure whether smaller and faster models give you acceptable performance.
- **Build vs. buy is really about maintenance.** Your schemas will evolve, new document types will come in, and edge cases will keep growing. Mercury has the engineering talent to build anything, but they chose to buy because the ongoing cost of maintaining document infrastructure takes away engineering hours that make their core product better.
- **Tooling matters as much as performance.**[Composer](https://docs.extend.ai/product/optimization/composer) (Extend's schema optimization tool) helped Mercury optimize their schemas to get better performance when switching to faster models. Built-in eval tooling gave them a way to catch regressions. The dashboard let non-technical domain experts debug and fix prompts.


## **The problem**


Mercury already had legacy document vendors in place, but those vendors couldn't handle the challenges of the onboarding use case.


1. The variety of documents is huge, and they come in dozens of languages: non-Latin alphabets, right-to-left text, and wildly different formats across hundreds of countries. Any approach that relied solely on traditional OCR wouldn't work on the long tail of edge cases in Mercury's global document set.
2. p90 latency had to be fast enough for a real-time product experience. Onboarding is the first thing a user experiences on Mercury's platform, and it shapes how they see the entire product. If document verification is slow, the user is stuck staring at a spinner and unable to move forward.


## **Evaluating every option on the market**


Mercury's team tested cloud providers, foundation models, newer startups, and Extend. Here's what they found:


**Cloud providers.** Mercury already uses AWS for most of their stack, so Textract was the obvious first try. Unfortunately, the performance didn't work for the infinite diversity of document types.


**Direct to foundation models.** Using foundation model APIs worked at first, but performance issues, high latency, and the lack of bounding boxes eventually made it unsuitable for production use.


Bounding boxes matter a lot to Mercury because they're processing sensitive financial data and need source citations for every extracted field. Mercury uses them to power efficient review workflows for downstream human review.


**Other startups.** Mercury tested other startups. Some didn't clear the accuracy threshold required, while others were API-only and lacked the tooling necessary to be production-ready.


**Extend.** Then Mercury tested Extend, and it stood out across the board:


- Advanced vision-language models showed state-of-the-art accuracy on varied document layouts
- Mercury's team could toggle between fast mode to enable a < 7 second p90, while performance mode was used for async jobs
- Bounding box coordinates and citations enabled an intuitive user experience embedded within the product
- Composer optimized their schema and removed the need for manual prompt engineering, while built-in eval tooling prevented regressions as they iterated on new edge cases


> Integration was smooth and the docs were great. We even generated our own bindings and published an open-source Haskell SDK for integrating with Extend.
>
>
> **Paul-Arthur Asselin, Senior Software Engineer**


## **What happened in production**


Mercury shipped document validation into the onboarding flow.


> Our approval rate went up. Users get instant feedback and can fix issues live instead of waiting days for a followup. The onboarding funnel got healthier.
>
>
> **Paul-Arthur Asselin, Senior Software Engineer**


The work didn't stop there. Extend made further optimizations that reduced latency to 5-7 seconds on average. Composer ran on one of their hardest document types and improved performance on edge cases to 98%. When Mercury asked for HEIC file support, Extend shipped it within a week.


> Extend's team was hands-on from day one, from setup to schema tuning to speed improvements. Anytime we flag something, it gets fixed fast. That kind of support is rare.
>
>
> **Paul-Arthur Asselin, Senior Software Engineer**


## **Looking forward**


Mercury recently launched personal banking, and they continue to grow their product suite to cover more financial tools and offerings. Many of these products will require document processing, and Extend will be there to help handle any document type, edge case, or layout that comes their way.


---


* *Mercury is a financial technology company, not a bank. Banking services provided through Choice Financial Group and Column N.A., Members FDIC.*
