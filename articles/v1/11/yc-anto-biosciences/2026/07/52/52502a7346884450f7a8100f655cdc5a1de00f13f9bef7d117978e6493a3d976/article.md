---
schema_version: "1.0.0"
document_id: "52502a7346884450f7a8100f655cdc5a1de00f13f9bef7d117978e6493a3d976"
company_key: "yc-anto-biosciences"
company: "Anto Biosciences"
source_id: "yc-anto-biosciences-news-import-c516c9aa21d4"
canonical_url: "https://anto.bio/publications/metatrinity"
published_at: null
first_seen_at: "2026-07-24T23:08:26.325303+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:4ff5c72fe43fef5ebfff99d56f89c358e557ea95f44943256a81305e687d5bbf"
---

# MetaTrinity

## Enabling Fast Metagenomic Classification via Seed Counting and Edit Distance Approximation


### Abstract


Metagenomics, the study of genome sequences of diverse organisms cohabiting in a shared environment, has experienced significant advancements across various medical and biological fields. Metagenomic analysis is crucial, for instance, in clinical applications such as infectious disease screening and the diagnosis and early detection of diseases such as cancer. A key task in metagenomics is to determine the species present in a sample and their relative abundances. Currently, the field is dominated by either alignment-based tools, which offer high accuracy but are computationally expensive, or alignment-free tools, which are fast but lack the needed accuracy for many applications. In response to this dichotomy, we introduce MetaTrinity, a tool based on heuristics, to achieve a fundamental improvement in accuracy-runtime tradeoff over existing methods. We benchmark MetaTrinity against two leading metagenomic classifiers, each representing different ends of the performance-accuracy spectrum. On one end, Kraken2, a tool optimized for performance, shows modest accuracy yet a rapid runtime. The other end of the spectrum is governed by Metalign, a tool optimized for accuracy. Our evaluations show that MetaTrinity achieves an accuracy comparable to Metalign while gaining a 4x speedup without any loss in accuracy. This directly equates to a fourfold improvement in runtime-accuracy tradeoff. Compared to Kraken2, MetaTrinity requires a 5x longer runtime yet delivers a 17x improvement in accuracy. This demonstrates a 3.4x enhancement in the accuracy-runtime tradeoff for MetaTrinity. This dual comparison positions MetaTrinity as a broadly applicable solution for metagenomic classification, combining advantages of both ends of the spectrum: speed and accuracy. MetaTrinity is publicly available.


### Key Results


MetaTrinity is enabling applications that weren't previously possible due to computational constraints:


- 4x faster than the most accurate existing tool
- 17x more accurate than the fastest existing tool
- 3.4x better overall at balancing speed and accuracy than current solutions


MetaTrinity is open-source and available on GitHub.
