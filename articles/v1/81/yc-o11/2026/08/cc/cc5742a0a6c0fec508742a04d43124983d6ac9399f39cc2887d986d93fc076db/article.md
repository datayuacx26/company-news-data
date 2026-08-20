---
schema_version: "1.0.0"
document_id: "cc5742a0a6c0fec508742a04d43124983d6ac9399f39cc2887d986d93fc076db"
company_key: "yc-o11"
company: "o11"
source_id: "yc-o11-news-import-e61ea6fbe134"
canonical_url: "https://o11.ai/blog/why-o11-moved-from-deck-and-spreadsheet-automation-to-ai-data-warehouses"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-17T12:10:05.697433+00:00"
fetched_at: "2026-08-17T12:10:07.459125+00:00"
content_hash: "sha256:0e38983cdfb37a1084b178bb2ed3d7eb17b9d4081b7e1d63a854702fe30707a9"
---

# Why o11 Moved from Deck and Spreadsheet Automation to AI Data Warehouses

The short answer is that decks and spreadsheets were the visible output of a deeper problem: important enterprise work starts with fragmented data, not with a blank PowerPoint slide or an empty workbook. o11 began by helping people produce those artifacts faster. The product direction has evolved toward the layer underneath them: an AI data warehouse that connects approved systems, builds a usable model of enterprise context, and keeps that context current.


That change does not make the original workflows irrelevant. A pitchbook, investment memo, operating review, or financial model still matters. It changes where the automation starts. Instead of treating a deck or workbook as the whole problem, o11 treats it as one destination for data and judgment that already exist across the organization.


This distinction matters for any company with complex information. It is especially clear in private equity and investment banking, where a single deliverable can depend on email, data-room files, CRM records, market data, prior work, and a model. But the same pattern appears in consulting, insurance, legal services, operations, and other enterprise environments.


## The original problem looked like document production


For years, the most visible bottleneck was production. A banker needed to turn source material into a pitchbook. An analyst needed to update a workbook. A consultant needed to turn research into a client-ready document. The last mile was slow because the work involved many repetitive actions:


- finding the latest source file;
- copying information into a working document;
- reconciling numbers across versions;
- applying an established template;
- checking whether an old claim still matched current evidence; and
- sending the output through review.


An AI assistant that worked directly in Excel, PowerPoint, Word, Google Sheets, Google Slides, or Google Docs could remove much of that friction. Native workflow automation remains useful because the final artifact has real formatting, formulas, comments, permissions, and review conventions.


But a document-first approach has a limit. It usually begins after someone has already decided which source to open. It can improve the artifact while leaving the source landscape fragmented. If a relationship is recorded in an email, the latest financials are in a data room, the company name varies across systems, and the decision history lives in a calendar note, the hardest work is still context assembly.


## The deeper problem is enterprise context


The same question often appears in many forms:


> What do we know about this company, transaction, customer, or operating issue, and where did each fact come from?


Answering it reliably requires more than generating prose. It requires locating relevant records, understanding which entities refer to the same thing, respecting access controls, preserving provenance, and presenting the result in a workflow where a person can review it.


That is the job traditionally associated with data platforms and data engineering. Traditional systems are powerful, but teams commonly have to design pipelines, map schemas, define models, maintain connectors, and decide how structured records relate to documents and messages. Smaller teams may have the data but not the capacity to build and continuously maintain every layer.


o11’s product direction is a response to this gap. The goal is not to replace judgment or make every enterprise decision automatic. The goal is to make the organization’s approved information more findable, connected, and usable so people and AI workflows can work from the same context.


## What changed in the product thesis


The shift can be summarized as a move from “make this artifact” to “make the information behind the artifact continuously usable.”


Earlier center of gravity New center of gravity Why it matters


A deck, workbook, or document An enterprise context layer that can feed many outputs The same evidence can support research, analysis, drafting, and review


A single open file Approved sources across systems The answer is less dependent on one person knowing where to look


Prompt-to-output generation Connect, index, retrieve, and act The workflow begins with evidence and relationships


Manual refreshes Continuously updated context New files, messages, and records can become part of the working record


Formatting as the finish line Traceability and permission-aware use A polished output is only useful if its facts can be checked and safely shared


This is why “AI data warehouse” is a better description of the current direction than “AI deck generator” or “AI spreadsheet tool.” The former describes the foundation. The latter describes individual surfaces that can benefit from it.


## What an AI data warehouse means in this context


The phrase should not be read as a claim that every company can eliminate its existing warehouse or database. o11 is better understood as a product layer for assembling and applying enterprise context across approved sources.


The current[o11 Memory](https://o11.ai/solutions/atlas) description names sources such as DealCloud, Salesforce, Outlook, calendars, email, files, data rooms, market data, ERP, notes, templates, and systems of record. It describes continuous indexing, permission-aware retrieval, and a continuously updated institutional record at terabyte scale.


In practical terms, the system is intended to help with four connected jobs:


1. **Connect:** reach the sources an organization has approved.
2. **Index:** turn source material into searchable, structured context.
3. **Relate:** connect people, companies, deals, records, and work product where evidence supports the relationship.
4. **Apply:** use that context in research, decisions, drafts, analysis, and supervised actions.


The product does not remove the need for source owners, governance, review, or good data practices. “Self-maintaining” means reducing the repetitive platform work required to keep an information layer useful. It does not mean that an organization can ignore policy, source quality, or human accountability.


## Why financial services made the direction obvious


Private equity and investment banking are demanding environments for a data product because the data is both fragmented and consequential. A deal team may need to combine:


- a CRM record and relationship history;
- emails and meeting notes;
- a confidential information memorandum;
- financial statements and operating metrics;
- market and transaction data;
- diligence requests and answers;
- prior analyses and templates; and
- a model or presentation under review.


The workflow is not simply “summarize the PDF.” It is “reconcile what is known, identify what is missing, show what changed, and prepare a reviewable output.” That is why o11 retains specific pages for[private equity](https://o11.ai/industry/private-equity) and[investment banking](https://o11.ai/industry/investment-banking) . Financial services is a deep specialization and proof point, not a limit on the product’s underlying architecture.


For example, a private-equity team may use connected context to find the latest portfolio-company update, compare it with the prior quarter, identify which metric changed, and prepare a supervised review package. An investment-banking team may use it to gather source material for a pitchbook or market update while keeping the analyst responsible for positioning and client-facing judgment. The data foundation makes those workflows more consistent; it does not make the recommendation itself.


## What this means for decks and spreadsheets


The new positioning is not “we no longer care about decks and spreadsheets.” It is “decks and spreadsheets are downstream surfaces in a larger system.”


Consider a quarterly portfolio review. A document-first tool might help write commentary into a deck after an analyst has collected the numbers. A data-foundation approach starts by making the relevant records easier to locate and compare: operating updates, financial statements, prior review materials, and source notes. The deck can still be the final meeting artifact. The difference is that the work can be repeated with more consistent context and a clearer evidence trail.


The same logic applies to models. A workbook remains a place for formulas, assumptions, scenarios, and review. But the inputs should not depend on a chain of copy-paste steps that no one can reconstruct. Connecting source context to the working model can reduce avoidable reconciliation work while keeping the analyst in control of assumptions and outputs.


## What o11 is not claiming


Responsible product positioning needs boundaries. o11 is not saying:


- that every source is automatically correct;
- that every relationship can be inferred without review;
- that a data warehouse removes the need for engineering in every enterprise;
- that AI-generated analysis should be accepted without validation;
- that a generated deck or workbook is investment advice; or
- that o11 replaces a firm’s systems of record.


The more accurate claim is narrower and more useful: o11 connects approved enterprise sources and applies their context to work, with source permissions and reviewability as core requirements. It is designed to reduce the distance between information, analysis, and action.


## A practical migration path for teams


Teams do not need to replace every workflow on day one. A sensible adoption path is incremental:


Stage Starting question Useful first workflow


Discover Where do people spend time finding context? Search across approved files, email, CRM, and records


Ground Which recurring output needs the clearest evidence? Source-backed review brief or diligence packet


Connect Which systems should contribute to that workflow? Link data-room, CRM, calendar, and document context


Repeat What changes every week or quarter? Portfolio, pipeline, or operating review


Govern What must remain permissioned and reviewable? Access rules, citations, approvals, and audit checks


This sequence keeps the implementation tied to a real business outcome. It also creates a way to test source quality and user trust before expanding coverage.


## How to evaluate the new direction


When comparing an AI data warehouse with a document-only assistant, ask:


1. Can it reach the systems where the organization’s context actually lives?
2. Does it preserve source permissions during retrieval?
3. Can a user inspect the evidence behind an answer or generated passage?
4. How are new and changed records incorporated?
5. Does it support the organization’s existing review surfaces?
6. What remains the responsibility of data, security, and domain teams?


The last question is as important as the first. A credible enterprise product explains its boundaries. o11’s current[enterprise page](https://o11.ai/enterprise) and[security messaging](https://o11.ai/solutions/atlas) frame the product around controlled sources, permission-aware context, and supervised work.


## Frequently asked questions


### Did o11 stop supporting decks and spreadsheets?


No. Those surfaces remain important for professional work. The strategic shift is to make the connected data and context underneath them more useful, so outputs are not isolated from the evidence that supports them.


### Is o11 a replacement for Snowflake, Databricks, or a company’s database?


Not by default. Existing warehouses and databases can remain part of the stack. o11’s value is in connecting approved enterprise context and making it available to AI-assisted workflows, including information that may live outside a conventional warehouse.


### Why call it an AI data warehouse?


Because the product direction includes source connection, indexing, modeling of enterprise context, permission-aware retrieval, and ongoing maintenance—not only generation inside one application.


### Is the product only for financial services?


No. Private equity and investment banking are important specializations because their workflows expose the problem clearly. The broader use case is any enterprise with fragmented, sensitive, and context-heavy data.


### Does “self-maintaining” mean no humans are needed?


No. People still define access, review important outputs, correct source issues, and own decisions. The goal is to reduce repetitive maintenance and context assembly, not to remove accountability.


### What should a team do first?


Start with one recurring workflow where people repeatedly search across sources and produce a reviewable output. Define the sources, permissions, evidence standard, and human approval step before expanding.


## Sources and further reading


- [o11 Memory: connected, permission-aware enterprise context](https://o11.ai/solutions/atlas)
- [o11 for private equity](https://o11.ai/industry/private-equity)
- [o11 for investment banking](https://o11.ai/industry/investment-banking)
- [o11 enterprise overview](https://o11.ai/enterprise)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) , for the governance principle that AI systems should be monitored, evaluated, and managed across their lifecycle.


The product description and claims in this article were reviewed against o11’s public product pages on 2026-08-14.
