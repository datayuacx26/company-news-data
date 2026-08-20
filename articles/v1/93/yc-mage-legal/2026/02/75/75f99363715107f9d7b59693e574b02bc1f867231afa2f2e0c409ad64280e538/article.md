---
schema_version: "1.0.0"
document_id: "75f99363715107f9d7b59693e574b02bc1f867231afa2f2e0c409ad64280e538"
company_key: "yc-mage-legal"
company: "Mage Legal"
source_id: "yc-mage-legal-news-import-389f7de0f02c"
canonical_url: "https://magelegal.com/blog/harvey-vs-kira-vs-infrastructure-legal-ai"
published_at: "2026-02-17T00:00:00+00:00"
first_seen_at: "2026-07-22T03:05:30.029696+00:00"
fetched_at: "2026-07-28T22:20:29.370610+00:00"
content_hash: "sha256:17d8a8d58c062cffe9fb39191549fa7e6460b081b06fded59e94504fdf2d5b92"
---

# Harvey vs. Kira vs. Infrastructure: Three Approaches to Legal AI

The legal AI market has three distinct paradigms, each designed for a different problem. Understanding which paradigm fits your workflow is more important than comparing feature lists. A research tool optimized for legal questions will not perform well as a contract extraction system, and an extraction system will not replace your legal research workflow. They are fundamentally different products solving fundamentally different problems.


The three paradigms are: research assistants (exemplified by Harvey), legacy extraction platforms (exemplified by Kira Systems), and purpose-built extraction infrastructure (the approach Mage takes). Here is how they differ and when each one is the right choice.


## The Research Paradigm: Harvey


Harvey is built on the research assistant model: you ask a question, the system searches relevant sources, and it generates a natural language answer. This paradigm is powerful for legal research, knowledge queries, and document-specific questions.


**Where Harvey excels:**


- Legal research questions ("What are the Delaware Chancery Court standards for fiduciary duty in a squeeze-out merger?")
- Drafting assistance (generating first drafts of memos, briefs, and correspondence)
- Document-specific queries ("Does this agreement contain a non-compete?" for a single document)
- Knowledge synthesis across legal precedent and commentary


**Where the research paradigm struggles for M&A:**


- Systematic extraction across hundreds of documents. Asking 50 questions about each of 300 contracts requires 15,000 individual queries, each with potential for[hallucination](https://magelegal.com/blog/llm-hallucination-in-contract-analysis) and missed context
- Structured output. Research tools produce narrative answers, not the structured data that populates disclosure schedules and diligence memos
- Comprehensive coverage. A research tool finds what you ask about. It does not surface provisions you did not think to query. The value of diligence is finding what you do not know to look for
- Cross-contract comparison. Comparing a specific provision type across 200 agreements requires structured data, not 200 individual narrative answers


Harvey is an excellent tool for the problems it was designed to solve. It is not designed for the structured extraction workflow that defines[M&A contract review](https://magelegal.com/contract-review) .


## The Legacy Extraction Paradigm: Kira Systems


Kira Systems pioneered machine learning for legal contract review over a decade ago, well before the current wave of LLM-based tools. Its approach is fundamentally different from both research assistants and modern extraction tools.


**Kira's approach:** Kira uses supervised machine learning models trained on labeled contract data. For each provision type (indemnification caps, change of control clauses, termination rights), Kira has a trained model that recognizes that provision in new documents. The system includes hundreds of pre-built "smart fields" for common provisions and allows organizations to train custom models for their specific needs.


**Where Kira excels:**


- Organizations with large, recurring document sets where the upfront training investment pays off over time
- Provision types well-covered by Kira's pre-built models
- Teams with dedicated resources to manage and maintain the system
- Use cases where the document types and provision categories are stable and predictable


**Where the legacy paradigm struggles:**


- New or unusual document types that fall outside Kira's pre-built models require custom training with labeled data, which takes time and expertise
- Adding new provision categories requires new training data and model retraining, creating a lag between identifying a need and having the capability
- Flexibility for diverse data rooms where the document types and relevant provisions vary significantly across transactions
- Accuracy on provision types with limited training data, since supervised ML models are only as good as their training sets


Kira remains a capable tool for organizations that have invested in its ecosystem. But the training-dependent model creates friction for M&A deal teams that encounter different document types and provision requirements on every transaction.


## The Infrastructure Paradigm: Purpose-Built Extraction


The third paradigm, which Mage represents, combines modern large language models with structured extraction schemas designed specifically for M&A workflows. Instead of answering questions (research) or applying trained models (legacy ML), purpose-built extraction processes documents through a type-aware pipeline that understands legal document structure.


**How it works:**


1. Documents are classified by type (customer agreement, employment agreement, lease, etc.)
2. Each document is segmented into[individual clauses](https://magelegal.com/blog/clause-level-segmentation-precision)
3. Clauses are extracted against type-specific schemas (what to extract from an indemnification clause in an asset purchase agreement differs from an employment agreement)
4. Extracted data is organized into structured output with source citations
5. Output feeds directly into deal deliverables


**Where purpose-built extraction excels:**


- Systematic review across entire data rooms (hundreds of contracts processed automatically)
- Structured output that maps to diligence deliverables without reformatting
- Flexibility across document types without per-provision training
- Cross-contract analysis (compare any provision type across the full data room)
- Source verification on every finding ([preventing hallucination](https://magelegal.com/blog/llm-hallucination-in-contract-analysis) through mandatory citation)


**Trade-offs:**


- Not designed for general legal research (use a research tool for that)
- Optimized for transactional work, not litigation review or regulatory analysis
- Structured interfaces mean less freeform flexibility than a research assistant ([by design](https://magelegal.com/blog/why-we-dont-let-users-write-prompts) )


## Choosing the Right Tool


The decision framework is straightforward once you separate the problems.


**Use a research tool (Harvey, CoCounsel) when:**


- You need answers to legal research questions
- You are drafting memos, briefs, or correspondence
- You need to analyze a small number of specific documents with specific questions
- Your primary need is legal knowledge, not document extraction


**Use a legacy extraction tool (Kira) when:**


- You have a large, recurring document set with stable provision types
- You have already invested in training Kira's models for your use case
- Your team has dedicated resources to maintain the system
- Speed and flexibility on new document types are not primary concerns


**Use purpose-built extraction infrastructure (Mage) when:**


- You are doing[M&A due diligence](https://magelegal.com/ma-diligence) across hundreds of contracts
- You need structured output that feeds directly into deal deliverables
- Every data room contains different document types requiring flexible extraction
- You need[100% contract coverage](https://magelegal.com/blog/pe-diligence-coverage-sampling-contracts-risk) without the cost of manual review
- Your team needs to be productive from the first upload without weeks of setup


The most common mistake in the market is using a research tool for an extraction problem. Harvey is excellent for asking questions. But M&A diligence is not a question-asking exercise. It is a structured extraction exercise across hundreds of documents, producing deliverables, not answers.


Similarly, using an extraction tool when you need a research assistant will disappoint. If your question is "What does Delaware law say about material adverse effect clauses?" you want Harvey, not Mage.


The legal AI market is large enough to support specialized tools for specialized problems. The[evaluation framework](https://magelegal.com/blog/legal-ai-tools-for-manda-evaluation-framework) starts with correctly identifying which problem you are solving.


---
