---
schema_version: "1.0.0"
document_id: "3d953314173757e6afc641ea2673c1dc29c73068c928369c3b2d101c7e7a28c0"
company_key: "yc-extend"
company: "Extend"
source_id: "yc-extend-news-import-054f4f06cd55"
canonical_url: "https://www.extend.ai/resources/checkr-case-study"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T18:04:33.983860+00:00"
fetched_at: "2026-07-29T18:04:35.041286+00:00"
content_hash: "sha256:1f999c9e6fb7a07f0d1b55373743217503d7f87773a5158ad1032723fb515551"
---

# How Checkr cut manual review time by 60% to near 100% across millions of documents

[Checkr](https://checkr.com/) is the AI verification platform that decodes messy, fragmented, hard-to-verify information about people, and delivers clear answers about a person's identity, background, and credentials. Trusted by more than 140,000 customers globally across employment screening, income verification, and tenant screening, Checkr helps businesses and individuals make high-stakes decisions with confidence.


Checkr uses Extend to process millions of documents across its screening workflows accurately, quickly, and scalably.


> Many of our document workflows are 95%+ accurate, with some near 100%. We've seen significant improvements in the accuracy of our document workflows since we started using Extend, and accuracy we get from the platform is on par with or better than our human Ops accuracy at scale.


## **Challenge: processing global document variations without sacrificing accuracy or speed**


Court extracts, police certificates, education transcripts, and identity documents directly influence how quickly a screening moves forward and what its outcome is.


> Documents are how trust gets translated into evidence. Every document touched in a screening either advances a candidate or holds them up.


Global document examples


Sample documents that illustrate the variation Checkr encounters across international screening workflows.


Built with[Extend UI](https://www.extend.ai/ui) — an open source UI kit for document components.


Speed and accuracy matter equally. A delay can keep a candidate from starting work. An incorrect result can wrongly flag or clear someone.


> Background screening is one of the few domains where both false positives and false negatives carry severe real-world consequences. That tension is what sets the engineering bar.


Maintaining the quality, scale, and speed of document processing is particularly challenging when it comes to international screenings, which involve a long tail of documents from every jurisdiction in the world. For example, Canadian driver's abstracts, proof of address documents in the UK, or identity documents from hundreds of countries.


Formats, languages, layouts, and authenticity signals vary widely:


- Unfamiliar scripts
- Stamped or handwritten certifications
- Low-quality scans
- Important details buried in free text
- Government templates that change
- Missing fields in candidate uploads


> Historically, processing these meant a combination of vendor-specific integrations, human operations review, and bespoke OCR pipelines that broke whenever a country changed its document template.


At Checkr's scale, each new format could create another engineering project. Even a small accuracy regression on a common document type could affect many screenings.


## **Solution: a complete document platform for production screening workflows**


Checkr considered general OCR providers, cloud document AI services, specialized vendors, and building internally on foundation models. However, Checkr needed more than a performant model. It needed a platform the engineering team could "live in for years, across an expanding set of document types and schemas."


The key criteria for Checkr's ideal solution included:


- **Built-in evaluation tooling.** Checkr needed to continuously measure workflow performance, run regression tests as schemas changed, and quantify accuracy on held-out evaluation sets so engineers could ship faster without sacrificing performance.
- **Long-tail document coverage.** The platform needed to handle uncommon formats, languages, layouts, scans, and changing templates without turning every document type into a new engineering project.
- **Confidence-calibrated review routing.** High-confidence extractions clear automatically, while ambiguous cases enter a first-class human-review state that provides the meaningful oversight required by Checkr's AI policy.
- **Iteration speed on new document workflows.** Onboarding a new document workflow needed to be a configuration and evaluation exercise, not an engineering project.
- **Security and data handling.** Any platform processing sensitive screening documents needed to meet Checkr's exceptionally high standards for data protection and privacy.


The Extend team also worked directly with Checkr as its requirements grew.


> Extend stood out as the right choice for Checkr because it met our key criteria, but also because of the partnership we felt with them. The Extend team was willing to engage deeply with us: jumping on calls at short notice, rapidly iterating their platform based on our feedback, even joining in on our hackathons. Their responsiveness and collaborative spirit gave us confidence we'd be able to evolve together as our requirements grew.


## **Implementation: how Checkr automates document routing from extraction to decision**


Extend sits inside Checkr's screening orchestration layer:


1. A candidate or partner submits a document.
2. Extend classifies, extracts, and validates it.
3. Extend returns structured outputs and confidence scores.
4. High-confidence results proceed automatically.
5. Low-confidence or ambiguous results move to human review.
6. Checkr uses evaluation sets to monitor performance and catch regressions.


Checkr's first Extend workflow supported document review for Employment Verification. The company has since expanded Extend across more document types and screening workflows.


> Any time a document is involved, we're considering how we can improve the way we process it with Extend.


## **Impact: millions of pages processed, with review time down 60–100%**


Metric Outcome


**Volume** Millions of pages processed and growing


**Accuracy** Average 95–100% accuracy across document workflows


**Human review** Average reduction of 60–100% in human document review time


**Deployment** New document workflows operationalized in hours or days rather than weeks


Checkr reports that Extend’s accuracy is comparable to and accelerates human operations accuracy at scale.


> We can onboard a new document type as a configuration and evaluation exercise rather than as an engineering project. That decouples document coverage from engineering capacity and helps democratize document intelligence capabilities in our platform across our entire organization, empowering subject matter experts to build and iterate on our document workflows. The end result is a faster, more accurate product for our customers and candidates, more efficient Ops processes, and more engineering capacity to build new features and capabilities into our products.


## **Four principles of building mission-critical document workflows**


Based on Checkr's experience, four recurring principles can be generalized to other engineering teams building document workflows.


1. **Start with an eval set.** Before you build the workflow, build the way you'll measure it. An eval set forces you to be concrete about what "correct" means for each document type, which fields actually matter, and what the edge cases look like. It's also what lets you tell whether a model change, schema change, or vendor change actually made things better. Without one, every iteration is a vibes-based exercise. Invest more in your eval set than feels comfortable.
2. **Design for human-in-the-loop from day one.** Human-in-the-loop is not a fallback for when the model fails. In a regulated industry, it's a permanent, first-class state in every workflow you'd want to operate. Confidence-based routing, reviewer tooling that surfaces the relevant fields rather than the whole document, and clean feedback paths from reviewer decisions back into the system are all dramatically more expensive to retrofit than to build in.
3. **Treat latency and accuracy as a real trade-off and design accordingly.** If low latency is a requirement, your workflow probably can't be a single linear extract-then-validate pipeline. You'll end up parallelizing across pages or fields, optimizing for *time to first insight* rather than *time to fully verified result* , and treating non-determinism as a feature rather than a bug, especially in validation, where running an extraction multiple times and looking at the spread can be more informative than a single high-confidence answer. Don't pretend you can have low latency, high accuracy, and full determinism all at once.
4. **Partner with your SMEs.** The people on your ops team who look at these documents every day know more about them than anyone on your engineering team will. They know which fields are reliably present, which ones get fabricated by candidates, which jurisdiction-specific quirks trip up the obvious extractors, and which "edge cases" are actually the modal case for certain document types. The fastest way to build a bad document workflow is to design it from a spec without sitting next to the people who currently do the work manually.
