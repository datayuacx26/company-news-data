---
schema_version: "1.0.0"
document_id: "f12f4a04ffdf95c621201d168900f263ffa971308a286eb53d2fe8606bc3660d"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/reducto-harvey-legal-ai-customer-story"
published_at: "2026-04-07T12:00:00+00:00"
first_seen_at: "2026-07-23T23:11:50.013054+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:57a97e7d94362ad8b623444978eecf677a85915cc8e94ad1aeb2962bad9773b3"
---

# How Harvey Turned OCR Quality Into Customer Confidence with Reducto

*“When we launched Reducto, the difference was huge. OCR-related customer concerns dropped sharply, and customers dramatically increased their use of OCR on more challenging documents.”*


*— Jin Zhang, Tech Lead at Harvey*


[Harvey](https://www.harvey.ai/) is the operating system for legal and professional services. Trusted by more than 1,300 organizations across 60 countries, Harvey helps practitioners work faster and with greater confidence by streamlining workflows in areas like contract analysis, due diligence, compliance, and litigation.


A key part of that experience is a secure document and knowledge layer that allows customers to upload, organize, and query large volumes of legal materials as a unified source of truth. As adoption of this capability accelerated, the team decided to invest in document intelligence as a first-class platform capability by partnering with Reducto.


*Harvey's Vault Product*


## Listening to the signals


Before[Reducto](https://reducto.ai/) , Harvey already had document ingestion in place piecing together popular PDF parsing libraries. These systems supported many early use cases, but as customers began uploading more complex legal documents with unpredictable file types and resolutions, quality quickly became a roadmap priority that could have an outsized impact on better user experience. Customer tickets referenced things like handwriting, redlines, and image-based documents where misread or skipped content led to incomplete results or confusing outputs.


> “At the end of the day, customers care about whether the system truly understands their documents. They upload a file, ask a question, and expect a clear, context-aware answer, so when that breaks down, it becomes immediately painful” said Jin.


Document processing was always foundational. As the range of documents expanded and usage continued to scale, the team wanted to evaluate partners that would help it accelerate improvements to its document processing capabilities.


## Building a Dedicated Document Platform


The team evaluated three paths: building in-house, using general-purpose model APIs, or partnering with a specialized vendor. Jin led the effort, comparing solutions across accuracy, reliability, cost, and—most critically—time to market.


Speed mattered. Harvey aimed to ship a production-ready solution within a few months. Building internally would have meant allotting time to accumulate years of OCR expertise, so the focus shifted to finding a partner that was already operating at production scale.


Jin ran extensive evaluations using multiple public datasets alongside internal datasets built from real customer edge cases. The team didn’t just compare headline accuracy—they built a detailed scoring matrix across dozens of axes, including extraction quality, robustness to layout and scan issues, consistency across document types, latency, and failure modes. They tested a broad range of approaches, from frontier lab-made reasoning models (e.g., Gemini, GPT-class models) to open-source OCR stacks, alongside many specialized vendors.


The goal was to find a solution that performed reliably across the board and cleared every customer edge case, not just one that looked good on a single metric. Reducto was a clear winner on the technical front, while also providing fast, hands-on support during evaluation and rollout. Reducto also offered built-in citations, so every answer could be traced back to the original document—a critical requirement for legal teams that need verifiable, auditable sources.


> “During onboarding, I had a great experience working closely with Raunak and the engineering team. Every time I had a question, they would respond immediately within minutes” said Jin.


Real-world reliability ultimately sealed the decision. Harvey needed a solution that would scale to billions of pages. Reducto met the demand. Combined with a highly responsive engineering team and a smooth on-prem Kubernetes deployment, the partnership allowed Harvey to reach full production in **roughly six weeks** , without compromising on quality or stability.


## Testing What’s Possible Next


With the foundation now in place, Harvey is beginning to explore where else Reducto may be a good fit.


> “It’s a mutually beneficial relationship,” said Jin. “Our customers want fast, error-free, high-quality document understanding. So anywhere Reducto can replace an existing system to improve one of those dimensions while maintaining the others ultimately leads to a better user experience.”


More broadly, as AI becomes increasingly embedded in the legal workforce and agentic systems take on more responsibility, document intelligence becomes more than an optimization—it becomes the prerequisite. Harvey’s exponential growth is a testament to the increasing appetite to explore AI in traditional industries like legal. In a document-first industry, getting that layer right is what allows everything above it to move forward with confidence.


---


***If you want to try Reducto on your own documents, either visit***[Studio](https://studio.reducto.ai/) ***to sign up for free, or***[request a demo](https://reducto.ai/contact?utm_source=website&utm_medium=blog) ***.***
