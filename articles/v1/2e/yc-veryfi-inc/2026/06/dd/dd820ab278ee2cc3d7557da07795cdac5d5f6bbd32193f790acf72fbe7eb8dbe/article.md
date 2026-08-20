---
schema_version: "1.0.0"
document_id: "dd820ab278ee2cc3d7557da07795cdac5d5f6bbd32193f790acf72fbe7eb8dbe"
company_key: "yc-veryfi-inc"
company: "Veryfi, Inc."
source_id: "yc-veryfi-inc-news-import-05be82a7e37e"
canonical_url: "https://www.veryfi.com/technology/veryfi-lens-vs-doxis-scanner-sdk/"
published_at: "2026-06-26T22:40:49+00:00"
first_seen_at: "2026-07-26T04:33:53.637378+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:899af73382d4c1d27b52ce725fc5e4d4f22e845513926c548be10f4e5b5e4760"
---

# Veryfi Lens vs Doxis (formerly Klippa) Scanner SDK: Which is Better for Mobile Document Capture?

If you’re planning to capture and extract data from financial documents: receipts, invoices, bank statements, expense reports you’ve probably run into two names:[Veryfi Lens](https://www.veryfi.com/lens/mobile-document-scanner/) and Doxis Scanner SDK (formerly Klippa).


Both are well developed SDKs. Both have solid developer tooling and enterprise customers. But they’re built for fundamentally different problems, and choosing the wrong one will cost you time and money.


## **Competitive breakdown** :


### **Veryfi Lens**


Veryfi Lens is a mobile document capture SDK purpose-built for financial documents (receipts, invoices, bank statements) as well as any document types including[insurance cards](https://www.veryfi.com/health-insurance-cards-ocr-api/) ,[driver’s licenses](https://www.veryfi.com/drivers-license-ocr-api/) , IDs, and more. It runs on-device using the Neural Engine on iOS and dedicated AI chips on Android, feeds data directly into Veryfi’s 110+ field extraction engine, and works entirely offline. It was built because the founders couldn’t find anything that handled a white receipt on a white tablecloth in the real world.


### **Doxis Scanner**


Doxis Scanner SDK, previously Klippa Scanner SDK before the rebrand, is a general-purpose document scanning SDK for enterprise workflows. It handles 100+ document types: invoices, IDs, passports, HR forms, contracts, logistics docs. It’s cloud-dependent for extraction, and has strong traction with European enterprises.


## **Side-by-Side: The Key Differences**


Doxis SDK tends to struggle when documents are captured against white or low-contrast backgrounds. Because its document detection relies primarily on traditional image processing techniques, it can have difficulty distinguishing the boundaries of the document from the surrounding background, sometimes confusing the two.


Veryfi Lens takes a different approach. It leverages **AI models trained on a wide variety of real-world scenarios** to understand what constitutes a document, even in challenging conditions such as white backgrounds, varying lighting environments, and complex scenes. This makes the Lens SDK significantly smarter and more robust, enabling more accurate document detection and a better user experience across a broader range of capture conditions.


- **Architecture:** Veryfi Lens runs entirely on-device. Capture, quality assessment, and initial processing happen locally using the device’s AI hardware – no network call, no cloud round-trip. Doxis sends data to their servers for extraction. That’s not necessarily a problem, but it’s a structural difference that affects latency, offline capability, and privacy posture.


- **Offline Mode:** Veryfi Lens is full offline capture and processing. Works in basements, warehouses, areas with no connectivity. Doxis: requires connectivity for extraction.


- **Accuracy:** Veryfi achieved 99.5%+ accuracy, backed by models trained on[NVIDIA DGX H100](https://www.nvidia.com/en-eu/data-center/dgx-h100/) and[B200](https://www.nvidia.com/en-eu/data-center/dgx-b200/) hardwares. Doxis claims “up to 99%.” The delta sounds small but compounds across high volumes.


- **Document Types:** Veryfi Lens covers any document types while being specialized in financial documents: receipts, invoices, bank statements, credit/business/insurance cards, W-2s, W-9s, bills of lading, hotel folios, healthcare EOBs, and long CPG receipts. Doxis covers 100+ general document types with broader range but less financial depth.


- **Data Extraction:** Veryfi Lens connects directly to Veryfi’s extraction APIs. Doxis extraction is cloud-based and general-purpose.


- **Fraud Detection:** Veryfi includes a built-in[Fraud Detection Suite](https://www.veryfi.com/fraud-detection/) and[GenAI Detector](https://www.veryfi.com/ai-generated-documents/) , running both on-device and via API. Doxis offers server-side analysis including duplicate hashing and copy-move detection.


- **Integration Time:** Veryfi Lens takes minutes, five lines of code to a working camera. Doxis claims “within 1 day.” Both are fast; Veryfi is faster.


- **Pricing** :[Veryfi’s pricing](https://www.veryfi.com/pricing/) is transparent and public. There’s a free tier (100 documents/month, no credit card required), a pay-as-you-go Starter plan from $500/month, and volume discounts for high usage. Per-document rates run $0.08 for receipts, $0.16 for invoices, and $0.25 for bank statements. Lens is available as an add-on via custom pricing. Doxis doesn’t publish pricing. Their model is an annual license fee per SDK with volume caps and overage packages on top, you need to contact their sales team for a quote. If you want to know what you’ll pay before talking to a sales rep, Veryfi is the only one of the two that lets you do that.


## **Where Veryfi Lens Wins**


Veryfi Lens includes an on-device blur detection model that automatically identifies when an image is too blurry or unclear. When poor image quality is detected, users are prompted to retake the photo before submission. The model is continuously learning and improving over time, helping ensure consistently high-quality image capture. This significantly enhances data extraction accuracy and leads to more reliable OCR results. Let’s look into details one by one:


1. Built for financial documents, not documents in general Doxis is a generalist: solid, capable, well-maintained. Veryfi Lens was engineered specifically for real-world financial document capture: crumpled receipts, thermal paper, bad lighting, long grocery receipts, embossed credit cards. If your app lives in that space, the specialization pays off in accuracy and reliability that a generalist can’t match.


1. On-device AI, which is entirely offline. No network dependency means no latency, no connectivity requirement, and no round-trip failure mode. For field workers, warehouse environments, or any app where connectivity is unreliable, this isn’t a nice-to-have, it’s a requirement. Doxis doesn’t offer a similar feature.


1. Privacy-by-design, not compliance-by-policy There’s a meaningful difference between “we process your data compliantly” and “your data never left the user’s device.” For fintech, banking, healthcare, and any regulated environment where document data is sensitive, Veryfi’s on-device architecture is structurally stronger.


1. Long receipt and CPG capture. CPG receipts are notoriously difficult: thermal, long, often folded or crumpled. Veryfi’s CPG glide mode handles them natively. If your use case includes cashback programs, grocery loyalty, or any CPG workflow, this capability is non-negotiable. Doxis doesn’t have it.


1. A full extraction engine at the end of the pipe Lens isn’t just a camera, it’s the front end of Veryfi’s entire document intelligence stack: purpose-built models NVIDIA H100 and B200-trained accuracy. When you need structured financial data out the other end, that pipeline depth matters.


## **Where Doxis Has the Edge**


General-purpose document range Doxis handles passports, contracts, HR documents, logistics paperwork, medical records. Veryfi is focused on financial documents. If your app needs to scan employee IDs and invoices with equal priority, Doxis may serve you better out of the box.


European enterprise ecosystem Doxis has strong BENELUX and broader EU enterprise traction. If your customer base is a European enterprise and already embedded in some of the platforms they support, that ecosystem integration has real value.


## **The Bottom Line**


The wrong SDK costs you weeks of debugging edge cases that a better-fit tool would have handled out of the box. For financial document capture, especially offline, high-accuracy, or privacy-sensitive use cases, Veryfi Lens is the one built for exactly that problem. Doxis covers more ground. Veryfi covers your ground better.


*Interested in learning more about Veryfi Lens,*[talk to one of our partnership experts today!](https://www.veryfi.com/talk-to-expert/?ref=footer)
