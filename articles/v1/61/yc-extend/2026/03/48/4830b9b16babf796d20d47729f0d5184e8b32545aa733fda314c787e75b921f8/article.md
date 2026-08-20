---
schema_version: "1.0.0"
document_id: "4830b9b16babf796d20d47729f0d5184e8b32545aa733fda314c787e75b921f8"
company_key: "yc-extend"
company: "Extend"
source_id: "yc-extend-news-import-054f4f06cd55"
canonical_url: "https://www.extend.ai/resources/nuvocargo-case-study"
published_at: "2026-03-16T00:00:00+00:00"
first_seen_at: "2026-07-24T07:25:34.796069+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:e00f1f9ec32ec105aad340a9d2e65d12980acceeae658c9b682b64f6fd27e2a7"
---

# How Nuvocargo hit 99% Document Accuracy for US-Mexico Cross-Border Freight

> Extend has been pushing the frontier on AI for messy, real-world docs. By pairing Extend’s AI with our orchestration layer in NuvoOS, we achieved highly impactful results and near perfect accuracy much faster than if we had built it ourselves.
>
>
> **Deepak Chhugani, CEO, Nuvocargo**


[Nuvocargo](https://www.nuvocargo.com/) builds software for North American freight. Their platform, NuvoOS, is an AI-native operating system that manages the full lifecycle of a shipment, from document intake to customs clearance. With access to 30,000+ trucks and a 24/7 in-house monitoring team, they help shippers achieve up to 40% faster border crossing times.


Here's how Nuvocargo used Extend to hit 97-99% accuracy across document intake, classification, extraction, and shipment attribution, with near zero human involvement.


---


**The problem**


US-Mexico cross-border freight is one of the most complex supply chains in the world. The challenges of cross-border freight are immense:


- Dozens of document types (invoices, pedimentos, packing lists, DODAs, certificates of origin)
- Messy inputs (scanned PDFs, email attachments, even WhatsApp screenshots)
- High stakes (every missed SKU or HTS code delaying shipments and upsetting customers)


One missing document, incorrect HTS code, or mismatched consignee can hold up a truck at the US-Mexico border for weeks on end. At Nuvocargo's volume, operations teams were manually re-entering fields across hundreds of invoices, packing lists, and customs forms every day. It didn't scale, and every error meant upset customers and missed delivery windows.


---


**Why Extend**


Nuvocargo’s team is world-class. They built their own OS for freight and have one of the most sophisticated AI / ML teams we've worked with. However, they recognized that building document processing in-house would slow down their ability to ship core product features.


Instead of spending weeks of engineering hours to build it in-house and bearing the ongoing maintenance costs, they selected Extend to handle near perfect accuracy, classify and extract across dozens of documents types, and scale to hundreds of thousands of documents even when formats change and trade lanes expand.


> We build our own software and often build on top of LLMs. But document processing across this many document types, at this volume, is its own hard problem. Extend got us to 99% accuracy fast, without us owning the maintenance burden forever.
>
>
> **Deepak Chhugani, CEO, Nuvocargo**


---


**How it works in production**


Nuvocargo’s R&D team rapidly designed and shipped a robust document pipeline built on top of Extend's APIs. When a document comes in through email, Zendesk, or WhatsApp:


- Extend’s **classification API** identifies the document type
- The **extraction API** pulls the structured data: consignee, SKU, invoice number, HTS codes, quantities, unit values, and currency
- That data flows into Nuvocargo's custom-built orchestration layer within NuvoOS, gets intelligently linked to the correct shipment, and is queued for customs clearance


---


**Results**


- **97-99% accuracy** across classification, extraction, and shipment attribution
- **4-5 hours saved per week per operations rep** on document processing
- Tens of thousands of documents processed
- Faster customs filings, fewer border delays, near zero manual touch


When an operations rep or customer logs into NuvoOS, shipments come in with documents already classified, extracted, and linked. The bottleneck that used to hold trucks for days or weeks is now handled automatically.


---


**Looking forward**


As volume grows and Nuvocargo expands into more of the North American freight market, the document problem only gets bigger. New shippers bring new document formats, and more volume means more edge cases. Extend scales with them, without requiring Nuvocargo to rebuild pipelines every time a new format shows up.


Deepak wrote about his own experience building this with Extend. Read it[here](https://www.deepakchhugani.com/p/4-how-we-used-ai-to-hit-99-accuracy) .
