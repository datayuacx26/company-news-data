---
schema_version: "1.0.0"
document_id: "df9e5f4039e73213f5e453739cb76ef08940299070b78da5c2bbf5097a65e016"
company_key: "yc-mage-legal"
company: "Mage Legal"
source_id: "yc-mage-legal-news-import-389f7de0f02c"
canonical_url: "https://magelegal.com/blog/how-to-architect-a-document-ai-pipeline-for-legal"
published_at: "2026-04-26T00:00:00+00:00"
first_seen_at: "2026-07-24T03:07:51.367861+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:b6c8737266aea18c2b0388e2c058525d53a16262471e16f9e1f3f49c107e87cc"
---

# How to Architect a Document AI Pipeline for Legal

This is the architectural piece I get asked about by engineers building legal AI internally and by attorneys curious about what's under the hood. Six layers, what each one does, where each one fails, and why each one matters.


## Layer 1: Ingestion


What it does: pulls documents from where they live (data rooms, document management systems, raw uploads) into the system in a format the rest of the pipeline can use.


What's hard about it: data rooms are messy. File names are often unhelpful ("Final v3 (Updated) FINAL.pdf"). Documents are nested. PDFs may be scanned with bad OCR. Excel files have meaningful structure that flat-text extraction destroys. Word documents have track changes that need to be resolved. ZIP files of ZIP files happen.


The architectural choice: don't trust the file extension. Run content-based classification at ingestion. Preserve structure (tables, lists, headings) in a way the downstream layers can use. Handle OCR explicitly with quality scoring; reject low-confidence OCR for human re-scanning rather than passing it downstream.


Common failure mode: shipping with whatever the cloud provider's default ingestion gives you. The work happens here or it happens nowhere.


## Layer 2: Classification


What it does: assigns each document a type (NDA, MSA, employment agreement, lease, IP assignment, financing instrument, organizational document, etc.) and routes it to the right downstream extraction pipeline.


What's hard about it: classification has long-tail. Most documents are common types; the long tail (joint venture agreements, settlement deeds, specific industry instruments) is where the misclassifications happen. Misclassification cascades: a wrong type means wrong extractors, wrong findings, wrong memo language.


The architectural choice: ensemble classification (LLM + structural features + historical patterns), with confidence scoring per document. Documents below a confidence threshold go to a human-review queue rather than to the wrong downstream extractor. Cheap to implement, expensive to skip.


## Layer 3: Structured extraction


What it does: pulls the partner-defined risk-relevant provisions out of each document, with citations to the source clause.


What's hard about it: provisions vary across drafting traditions, jurisdictions, and counterparties. The same concept (e.g., "limitation of liability") can be drafted twenty different ways and still mean roughly the same thing. The extractor needs to find the concept regardless of phrasing, and it needs to flag genuine variants where the concept's effect differs.


The architectural choice: domain-specific extraction prompts, run per provision type, with citation requirements baked into the output schema. The model is asked not just for the answer but for the source clause range; the citation gets verified at the validation layer. We have written about why this matters in[LLM Hallucination in Contract Analysis](https://magelegal.com/blog/llm-hallucination-in-contract-analysis) .


## Layer 4: Multi-document reasoning


What it does: handles questions whose answers depend on more than one document — amendment chains, cross-referenced provisions, schedule-to-agreement linkage, top-of-the-stack issue prioritization.


What's hard about it: this is where naive RAG fails. Standard retrieval-augmented generation embeds chunks of text, retrieves the most semantically-similar chunks for a given query, and synthesizes. It has no native concept of order. When the amended-out provision and the operative provision are both in the chunk store, RAG cannot tell which is current.


The architectural choice: sequential structured extraction with explicit add/modify/delete tracking. For amendment chains specifically: process the original agreement and each amendment in order, track which provisions have been added, modified, or deleted at each step, and produce a resolved view of the current operative terms. The resolved view is what downstream layers see. We expand on this in[Amendment Chain Resolution: The Hardest Problem in Legal AI](https://magelegal.com/blog/amendment-chain-resolution-hardest-problem-legal-ai) .


This is the layer most teams get wrong. RAG is easy and demos well. It also produces confidently-wrong output the moment a real data room hits the system.


## Layer 5: Validation


What it does: catches LLM errors before they reach the user.


What's hard about it: LLM errors are by default plausible. The output reads like a correct answer; verifying takes work the user doesn't have time for.


The architectural choice (multi-pronged):


- **Citation verification.** Every finding has a source clause range. The system checks that the cited text exists in the source document and that it actually says what the finding claims. Citations that don't verify are flagged or dropped.
- **Schema conformance.** The output of each extraction is required to fit a typed schema. Outputs that don't conform are re-prompted or sent to a human review queue.
- **Cross-extraction consistency.** When two extractions on the same provision disagree (e.g., the indemnity-cap extractor and the limitation-of-liability extractor have different views), surface the disagreement instead of picking one.
- **Confidence floors.** Findings below a confidence threshold are marked and reviewed; they don't go silently into the memo.


Validation is the layer most teams skip because it doesn't show up in demos. It also catches the cases that ship hallucinations to clients. Skip it at your peril.


## Layer 6: Output


What it does: produces the user-facing artifacts (issues lists, memos, schedules, redlines) in the firm's voice and structure.


What's hard about it: partner-grade output is partner-grade. The bar is "edits the language, not the substance." Hitting that bar consistently is engineering work — templating, voice tuning, structural conventions — not a property of the model.


The architectural choice: firm-branded templates per deliverable, voice-tuned per firm, with the underlying findings rendered into the template via a deterministic pass rather than a generative one. Generation produces the substance; templating produces the voice. Mixing them yields output that's neither structurally consistent nor stylistically partner-grade.


## Why six layers and not three


A common shortcut is to collapse the pipeline: ingest → ask the model a question → display the answer. Three layers, fast to build, demos well.


Three layers fail in production for the reasons we've named:


- No classification: extraction runs on the wrong document types
- No structured extraction: extractor output is unstructured prose, not citable findings
- No multi-document reasoning: amendment chains break
- No validation: hallucinations ship
- No output layer: partner rewrites everything anyway


Each layer is an engineering investment that catches a specific failure mode. Skip the layer; ship the failure mode. There are no shortcuts that aren't visible in the output the first time a real deal hits the platform.


## The architectural takeaway


Modern legal AI is not "use the model." It is "build the chassis around the model that handles the parts the model alone can't." Six layers of chassis. Each one earns its place by catching a specific failure mode that the absence of that layer would let through.


For the implementation-time view of the same picture, see[The F1 Engine Problem](https://magelegal.com/blog/f1-engine-problem) . For the multi-document reasoning deep-dive, see[Amendment Chain Resolution](https://magelegal.com/blog/amendment-chain-resolution-hardest-problem-legal-ai) . For how this maps to the broader category, see[Legal AI for M&A: The Practitioner's Guide](https://magelegal.com/blog/topics/due-diligence) .


— Raffi
