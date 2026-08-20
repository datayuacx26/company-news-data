---
schema_version: "1.0.0"
document_id: "0ad699033996c713bc8538274f14dc78256a69e196c8c08d4db8175edef1a8ce"
company_key: "yc-sieve"
company: "Sieve"
source_id: "yc-sieve-news-import-7bd448ce2404"
canonical_url: "https://sieve.ai/blog/reintro"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-22T13:25:09.501437+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:6b58f9ab0373f0dd4f44f73e578877944af94c02d0cd6dba4d99a9830794b54a"
---

# Reintroducing Sieve

Sieve supplies multimodal data to many of the frontier AI labs, but that is not how we started. I wanted to share the path that got us here.


Abhi and I started the company in 2022 building developer tools for computer vision. After several pivots, we landed on an API platform for video understanding and editing, with capabilities like auto-resizing, translation, scene detection, object tracking, speaker tracking, and more. By early 2025, we had real customers that included top creative tools, social platforms, and media companies using our APIs in production. Around that same time, we noticed a different pattern emerging: a smaller group of customers was using those APIs to annotate large video datasets.


As that use case repeated across teams, we came to two conclusions:


1. Our APIs were functioning as highly effective annotation systems for research teams
2. Model progress would likely make many of our API products obsolete within 1–2 years


We considered going deeper on a standalone annotation offering, but ultimately chose to supply datasets end-to-end instead. Our view was that video data operates at a level of granularity and scale where collection, QA, and labeling have to be built tech-first. We also believed sourcing, filtering, and annotation were too tightly coupled to be treated as separate workflows.


Within weeks of making that shift, we started working with top-tier research teams. Today, we work with many leading AI labs, Fortune 100 companies, and fast-growing startups building multimodal models.


## The world’s largest searchable video corpus


Most research teams evaluating a data partner care about the same four things: quality, scale, speed, and diversity. Most data vendors struggle to deliver all four at once because their workflow is largely reactive, and their sourcing process can vary dramatically from one request to the next. As a result, quality is hard to predict, timelines slip, and scale depends too heavily on the specifics of each project.


Sieve takes a different approach. We build datasets proactively around two core capabilities:


1. Building one of the world’s largest catalogs of raw video (which also gives us a massive base layer of aligned *images* and *audio* at scale)
2. Building highly precise search and understanding systems over that corpus


We continuously acquire data through our contributor platform and data partnerships, absorbing the upfront cost so we can deliver quickly when demand arises. Today, we collect millions of hours of new content each month. Our infrastructure ingests, indexes, and makes this corpus queryable across hundreds of granular attributes that matter for frontier model training.


That combination is what lets us move fast without sacrificing precision. We can deliver datasets that are diverse, tightly matched to a research objective, and consistent in quality at scale.


## Our technical advantage


Video, and multimodal data more broadly, is fundamentally different from most data categories. Doing it well requires a deeply technical company. To give a sense of scale, Sieve operates on hundreds of petabytes of video; every 100 petabytes is roughly 75 million hours of 1080p content, which is not feasible to review manually. At that scale, collection, indexing, filtering, QA, and annotation all have to be software systems, not service workflows. The systems we’ve built to operate at this scale are a large part of Sieve’s advantage, and they come directly from how we got our start.


This software-first foundation lets us solve data operations problems in ways most vendors cannot, and work directly with research teams on datasets that meaningfully improve model outcomes. Every person on our team has an engineering degree, and roughly 75% of the company is in research and engineering.


A few examples of what that looks like in practice:


- **Layered QA from source to shipment:** Quality control starts well before final review. We qualify contributors before onboarding, ramp new sources in a controlled way, run automated validation throughout capture, upload, and processing, apply dataset-level checks on deliverables, and then route every asset through final human QA. By the time content reaches a reviewer, it has already passed through multiple layers of filtering, which improves yield, reduces rework, and keeps quality consistent at scale.
- **Human QA that improves the system over time:** Human review remains a constant, but it does not become a bottleneck. We use outputs from human QA across slices of each dataset to refine internal processes, improve acceptance criteria, and train our QA/QC models. That means our filters, whether pre-existing or built for a new customer requirement, get better over time as more data is collected and reviewed.
- **Precise filtering and curation at scale:** Our infrastructure can automatically watch, index, and classify massive volumes of video, which lets us rapidly slice large inventories down to narrow research requirements. This makes it possible to deliver datasets that are both broad in supply and tightly aligned to a specific training objective, even under short timelines.
- **Rich multimodal signal design across collection and post-processing:** Our differentiation is not just in post-processing annotation. We often design collection itself to produce richer multimodal signals, then augment, structure, and annotate that data afterward across temporal, spatial, and audio dimensions, including higher-complexity signals where needed. This makes the resulting datasets substantially more useful for frontier training workloads.


These are deeply technical systems that give us a differentiated way to scale data supply while maintaining tight control over quality, consistency, and delivery speed.


## What we’re building towards


Our goal is to push the frontier of multimodal AI by improving one of its key constraints: access to high-quality, high-scale training data. Today, we contribute to that frontier through work spanning media generation and editing, visual understanding, robotics, world models, and computer use.


Our team combines deep technical understanding of model-facing data requirements with real operational experience building datasets at scale, with backgrounds from Scale AI, Mercor, and FAANG. We have delivered hundreds of petabytes of video, run collection programs globally, and paid millions of dollars to content partners.


We’re excited to continue working with teams building at the frontier. If you’re interested in our work,[reach out](https://sieve.ai/contact?ref=sieve.ghost.io) .
