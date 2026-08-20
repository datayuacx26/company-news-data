---
schema_version: "1.0.0"
document_id: "5f75a52b7aa7571e867c3bea2702fef8326cb6ffd2f3997fe7c6f2207dd17d2b"
company_key: "yc-mage-legal"
company: "Mage Legal"
source_id: "yc-mage-legal-news-import-389f7de0f02c"
canonical_url: "https://magelegal.com/blog/contract-review-software-comparison-research-vs-extraction"
published_at: "2026-02-17T00:00:00+00:00"
first_seen_at: "2026-07-22T03:05:30.029696+00:00"
fetched_at: "2026-07-28T22:20:29.370610+00:00"
content_hash: "sha256:0ef438bc24e2404644b0e789a963d375a4f5dfc7aafd7dea75e632e6fad5dc1e"
---

# Research vs. Extraction: Two Paradigms for Contract Review Software

Contract review software is technology that assists attorneys in analyzing legal agreements, extracting relevant provisions, and identifying risks. The market has grown rapidly, but beneath the feature comparisons and vendor claims, there are two fundamentally different paradigms at work. Understanding which paradigm a tool uses tells you more about its real-world utility than any feature list.


The research paradigm treats document analysis as a question-answering problem: you ask questions, the tool searches your documents, and it generates answers. The extraction paradigm treats document analysis as a data structuring problem: the tool processes every document, extracts relevant provisions into a structured schema, and presents a complete analytical layer across your document set.


## The Research Paradigm


Research-paradigm tools are built on a conversational model. You upload documents and ask questions in natural language. The tool retrieves relevant content from your documents and generates an answer.


**Example interaction:**


- "Does Contract #47 contain a change of control provision?"
- "What is the indemnification cap in the ABC Corp. MSA?"
- "Are there any non-compete restrictions in the employment agreements?"


Each question produces a narrative answer, typically a paragraph summarizing the relevant finding with varying degrees of specificity.


**Strengths of the research paradigm:**


- Flexible. You can ask any question about any document.
- Intuitive. The conversational interface requires no training.
- Fast for single questions. Getting one answer about one document is quick.
- Good for ad hoc analysis. When you need to check a specific provision in a specific document, research tools are efficient.


**Limitations for systematic contract review:**


-


**Scale.** Systematic diligence requires extracting dozens of data points from each of hundreds of contracts. Running 50 queries per contract across 300 contracts means 15,000 individual query-response cycles. Each cycle has a small probability of[hallucination or missed context](https://magelegal.com/blog/llm-hallucination-in-contract-analysis) . At 15,000 cycles, even a 2% error rate produces 300 errors.


-


**Coverage gaps.** Research tools find what you ask about. They do not surface provisions you did not think to query. If you do not ask about audit rights in vendor agreements, you will not learn that three vendors have unusual audit provisions. The value of diligence is comprehensiveness, finding what you do not know to look for.


-


**Narrative output.** Research tools produce paragraphs. Diligence deliverables require structured data: specific provision types with specific values and specific sources. Converting 15,000 narrative answers into structured memo entries requires significant manual work.


-


**No cross-contract analysis.** Each query-response cycle is independent. The tool answers one question about one (or several) documents at a time. Comparing indemnification caps across 200 customer agreements requires either a carefully crafted comparative query (which[RAG pipelines handle poorly](https://magelegal.com/blog/why-rag-fails-for-legal-contract-review) ) or manual compilation from individual answers.


## The Extraction Paradigm


Extraction-paradigm tools are built on a data structuring model. You upload documents, and the tool processes every document through a structured extraction pipeline, producing a complete dataset of provisions across the entire document set.


**Example output:** A structured table showing every customer agreement with columns for: contract name, counterparty, effective date, term, auto-renewal, change of control, assignment restrictions, indemnification cap, basket type, survival period, termination triggers, and non-standard terms. Each cell contains the extracted value with a link to the source clause.


**Strengths of the extraction paradigm:**


-


**Comprehensive.** Every document is processed against the full extraction schema. No provisions are missed because no one asked about them. If the schema includes audit rights, every audit right across every agreement is extracted, whether or not the attorney thought to look for it.


-


**Structured output.** The output is a dataset, not a collection of paragraphs. Provision types, extracted values, and source citations are organized in a structured format that maps directly to diligence deliverables.


-


**Cross-contract analysis.** Because every provision is extracted into a structured schema, comparing any provision type across the entire document set is a table operation, not a research project. How many customer agreements have[change of control clauses](https://magelegal.com/blog/what-300-ndas-taught-me-about-change-of-control-clauses) ? Which employment agreements have non-competes exceeding 12 months? Where do indemnification caps deviate from the standard form? These questions are answered by the existing dataset.


-


**Deliverable-ready.** Structured extraction output maps directly to diligence memos, disclosure schedules, and exception lists. The data populates the deliverable template without manual reformatting.


**Limitations:**


- Less flexible for ad hoc questions. If you need to know something outside the extraction schema, the tool may not answer it.
- Requires domain-specific design. The extraction schema must be built for the specific use case (e.g., M&A diligence). A generic extraction schema produces generic output.
- Structured interfaces may feel less intuitive than a conversational interface for users accustomed to search-and-ask workflows.


## Why the Paradigm Matters


The paradigm choice determines three downstream consequences:


### 1. Deliverable Quality


Research output requires manual assembly. An attorney using a research tool collects individual answers, organizes them by document and provision type, extracts specific data points from narrative paragraphs, and formats everything into the memo template. This assembly work takes[significant time](https://magelegal.com/blog/why-associates-spend-60-hours-on-material-contracts) and introduces transcription errors.


Extraction output is pre-assembled. The structured data maps to deliverable fields directly. The attorney reviews and edits, not compiles and reformats.


### 2. Error Patterns


Research tools produce errors through hallucination and missed retrieval across individual queries. Each error is independent and unpredictable. Quality control requires checking every answer against the source document.


Extraction tools produce errors through boundary detection and parameter extraction failures. These errors are systematic and predictable: if the system struggles with a particular document format, the pattern is visible across multiple documents. Systematic errors are easier to identify and correct than random ones.


### 3. Total Time to Deliverable


For a[300-contract data room](https://magelegal.com/blog/real-cost-of-manual-contract-review-in-manda) , the total time from upload to deliverable-quality output differs significantly:


**Research paradigm:** Processing time (1-2 hours of querying) + manual compilation (40-60 hours of organizing narrative answers into structured findings) + formatting (10-20 hours of populating deliverables). Total: 50-80 hours of attorney time.


**Extraction paradigm:** Processing time (minutes) + review time (20-30 hours of reviewing structured findings against sources) + deliverable generation (2-3 hours of configuring and exporting). Total: 25-35 hours of attorney time.


The extraction paradigm is not faster because it processes documents faster. It is faster because it eliminates the compilation and formatting steps that dominate the research-paradigm workflow.


## When to Use Which


The paradigms are complementary, not competitive.


**Use research for:**


- Legal research questions during deal strategy and negotiation
- Ad hoc analysis of specific documents or provisions
- Drafting assistance for memos and correspondence
- Quick checks on individual document terms


**Use extraction for:**


- Systematic[contract review](https://magelegal.com/contract-review) across full data rooms
- Generating structured[diligence deliverables](https://magelegal.com/ma-diligence)
- Cross-contract comparison and pattern detection
- Achieving[100% contract coverage](https://magelegal.com/blog/pe-diligence-coverage-sampling-contracts-risk) within deal timelines


The[evaluation framework](https://magelegal.com/blog/legal-ai-tools-for-manda-evaluation-framework) for choosing between specific tools within each paradigm involves testing on real deal data, measuring output quality against existing work product, and assessing total time to deliverable. But the first decision, before evaluating any specific tool, is identifying which paradigm matches the problem you are solving.


For M&A deal teams, the contract review phase is an extraction problem. The legal research phase is a research problem. Using the right tool for each produces better results than forcing either paradigm to serve both purposes.


---
