---
schema_version: "1.0.0"
document_id: "6ecfcc51d4a15122fde7d8e7d929d983167fafda8ac4fd528b71965bf463ad4a"
company_key: "yc-kapa-ai"
company: "kapa.ai"
source_id: "yc-kapa-ai-news-import-d4974854d4ea"
canonical_url: "https://www.kapa.ai/blog/the-best-pdf-converter-for-rag"
published_at: "2026-04-29T00:00:00+00:00"
first_seen_at: "2026-07-23T13:19:19.793244+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:2b4554484643293e9c12da91d9a64cf36a2d205c802c9c7a16579fe93a776d74"
---

# How to choose a PDF-to-markdown converter - kapa.ai - Instant AI answers to technical questions

This is how well six common PDF-to-markdown converters perform in our domain:


Each converter is scored across four dimensions (headers, tables, figures, text) that we outline below. The scores vary wildly across converters and dimensions, and no single converter dominates all of them.


This post explains how we built the evaluation that produced these results, so you can adopt the same approach for your domain. Your results will likely look different because they depend on what kinds of documents you process and what your pipeline needs from them.


## Why this problem matters to Kapa


Kapa builds AI assistants that answer technical questions using a company's own knowledge base. We ingest data from many sources, but PDFs are particularly challenging.


Our customers in hardware-related fields like semiconductors and industrial automation have the majority of their technical knowledge locked in PDFs. These are often hundreds of pages long with complex tables and many diagrams that carry critical information.


To make this knowledge retrievable, we need to convert these PDFs into clean, structured markdown that our RAG pipeline can index effectively. The quality of a RAG system is bounded by the quality of its input data. If the converter loses table structure, misses figures, or breaks heading hierarchy, no amount of downstream optimization can recover what was lost.


## Why build an evaluation framework first


You cannot improve what you cannot measure. Comparing PDF converters by hand does not scale, and the quality of your evaluation determines the ceiling of how well you can solve the problem. Once a converter is in production, iterating without a reliable evaluation becomes risky. You do not know whether a change actually improves things or breaks something, so you stop making changes altogether. Systems without an effective evaluation framework usually do not progress at all once in production.


The evaluation also needs to be specific to your domain, both in the metrics it uses and the data it runs on. General academic benchmarks or the ones published by converter providers often do not capture exactly what matters to your application, and their test documents may look nothing like yours. You need to build your own. For us, what matters is:


-


**Document structure** , because our downstream pipeline relies on heading hierarchy


-


**Tables** , because they carry a large share of the important information in technical documents


-


**Figures** , because diagrams and charts are often essential to understanding the content


-


**Text** , because the body text needs to be extracted faithfully


Things like visual layout, page numbers, footers, and formatting details do not matter. We need to represent the information accurately, not reproduce the original document.


## Our evaluation approach


The idea is straightforward: design metrics that capture what you care about, then compute them by comparing converter output against hand-annotated ground truth. You take a set of representative PDFs, create a correct reference conversion for each, run every converter on the same documents, and score the results. This gives you a repeatable, quantitative way to compare converters and track improvements over time.


### Data set


Our data set has two parts.


#### Real-world PDFs


These are full documents representative of what our customers actually upload. Each one is fully annotated by hand to create a ground truth conversion. They test the combined challenges you see in production: complex tables nested under deep heading hierarchies, figures mixed with body text, multi-column layouts. The challenge here is data set size, because labeling PDFs by hand is time-consuming and you will struggle to build a really large set.


#### Synthetic unit tests


These are not full documents but small, focused PDFs that contain just a single element like one specific table with cells spanning multiple columns or a page with only nested headers. Some are snippets isolated from real-world PDFs, while most are generated programmatically from HTML or LaTeX templates with automatically derived ground truth. Each one is designed to test a single aspect of conversion like multi-column cell handling, header nesting, or figure detection in ambiguous layouts. When a synthetic test fails, you know immediately what went wrong because there is only one thing being tested. This is harder to diagnose in a full end-to-end PDF where many things interact. The difficulty with synthetic tests is representativeness: because you are creating them yourself, you have to be careful not to overweight certain types of scenarios or create tests that would never occur in real documents.


### Data format


Pure markdown is not expressive enough to evaluate against. Markdown tables cannot represent cells that span multiple rows or columns, and a markdown image link loses almost all information about where the figure appeared in the original PDF: its page, bounding box, type, and whether it is useful content or decorative.


Our evaluation format is based on markdown but extended where markdown falls short. Tables use HTML, which preserves the full structural information including` colspan` ,` rowspan` , captions, and cell formatting. Figures are wrapped in` <figure>` tags with metadata for type, page number, bounding box, and whether the figure is useful or decorative. Both ground truth and converter output conform to this format, so any difference the metrics detect is a real content or structure error, not a formatting choice. The ground truth only needs to be created once and can be transformed into other representations later if we want to evaluate which downstream format works best for retrieval.


### Metrics


For each document in the data set, we compute the following metrics against the ground truth. They are organized into four groups (headers, tables, figures, text) because those are the dimensions we care about.


Before any metric can be computed, each group needs to solve a matching problem: which detected element corresponds to which ground truth element? If the matching is wrong, the metrics are meaningless. Each group handles this differently because the elements have different properties.


#### Headers


Headers are matched by text similarity. We compute the normalised edit distance between every detected header and every ground truth header, then use the Hungarian algorithm to find the optimal global assignment. Pairs that are too dissimilar after matching are rejected as unmatched. Matching uses text only. Header level is deliberately excluded to avoid making level accuracy tautological.


Metric


What the score means


` header_recall`


Of all the headers that exist in the document, how many did the converter find? Low recall means the converter is missing headers.


` header_precision`


Of all the headers the converter output, how many are real? Low precision means the converter is inventing headers that do not exist.


` level_accuracy`


When the converter finds a header, does it get the depth right? Is an h2 actually an h2 and not an h3? Errors on top-level headers are penalized more because they affect more of the document.


` position_accuracy`


When the converter finds a header, is it under the right parent? If a subsection ends up under the wrong section, all the content beneath it is misplaced. Top-level errors are penalized more.


#### Tables


Tables are matched the same way as headers, but on a flat string representation of each table's cell contents. The cell-level metrics below only run on matched pairs. Unmatched tables affect only recall and precision.


Metric


What the score means


` table_recall`


Of all the tables in the document, how many did the converter find? Low recall means the converter is missing tables.


` table_precision`


Of all the tables the converter output, how many are real? Low precision means the converter is inventing tables that do not exist.


` cell_text_similarity`


When the converter finds a table, how accurately is the text inside each cell reproduced?


` span_accuracy`


When the converter finds a table, are merged cells (cells that span multiple rows or columns) preserved correctly?


` dimension_overlap`


When the converter finds a table, does it have the right number of rows and columns? If the ground truth is 5x3 and the converter outputs 7x3, this score drops.


#### Figures


Figures cannot be matched by text since many contain no text at all. Instead, figures are grouped by page and matched by spatial overlap: we compute the intersection-over-union (IoU) between all bounding boxes on the same page and use the Hungarian algorithm to find the best assignment. Pairs with insufficient overlap are rejected as unmatched.


Metric


What the score means


` figure_recall`


Of all the figures in the document, how many did the converter detect? Low recall means the converter is missing figures.


` figure_precision`


Of all the figures the converter output, how many are real? Low precision means the converter is detecting figures that do not exist.


` iou_accuracy`


When the converter finds a figure, how closely do the detected boundaries match the actual figure boundaries?


` localization_accuracy`


When the converter finds a figure, is it placed under the correct section header in the output?


#### Text


Unlike the previous groups, text does not require a matching step. Tables and figures are stripped out, and the remaining body text is compared directly between converter output and ground truth on a per-page basis.


Metric


What the score means


` flow_text_similarity`


How accurately is the running body text of the document reproduced? Tables and figures are excluded since they have their own metrics.


### Weighting and scoring


With many individual metrics per document-converter pair, you need a way to aggregate scores into something you can actually compare. The individual metrics are rolled up into group scores (headers, tables, figures, text), which are then combined into an overall score.


Not all groups matter equally. We weight headers at 1.5x because document structure determines how everything downstream is organized. Tables, figures, and text are weighted equally at 1.0. You should overweight the dimensions that matter most to your use case.


At the same time, do not collapse everything into a single number and call it done. Keep the group-level scores visible so you can see where a converter is strong and where it falls short. The goal is to make comparison manageable without hiding the detail that matters.


## Common failure modes


As part of this comparison, we found the following common errors that the evaluation framework surfaced.


### Wrong heading level assignment


When a converter assigns the wrong markdown depth to a heading, the document structure below it breaks. A single wrong level means every child heading ends up under the wrong parent, and the error cascades through the subtree. In a set of SDK release notes, one converter correctly output the first release as` ### 1. Release 1.2.0` but demoted all subsequent releases to` #####` , making them children of` #### 1.1 New features` instead of siblings of the first release:


The same kind of damage happens when a converter misses a header entirely and promotes one of its children to fill the gap, and all the other siblings end up under the promoted header instead of being peers.


This was the largest source of score variation across converters we tested. Interestingly, some converters produce a *flatter* tree than the ground truth, assigning everything one level too shallow, but preserve sibling relationships correctly. That is a much less damaging error for downstream chunking than the inconsistent demotion shown above.


### Table extraction errors


Tables are where we saw the most varied failure modes across converters. The hardest problems are getting the structure of complex tables with cells spanning multiple rows or columns right, and handling very large sparse tables (like checkbox matrices) with multi-column headers.


-


**Multiline cell splitting.** Some converters treat multiline cells as multiple rows. A single cell containing a paragraph of text becomes three or four rows, changing the table dimensions entirely. On one document, a 3x5 revision-history table became 7x5.


-


**Wide table truncation.** Some converters truncate tables with many columns. On a document with 16-column register tables, one converter split them into narrower fragments, outputting 128 tables for what should have been 32.


-


**Non-tabular content converted to tables.** Table-of-contents pages and product labels were converted into` <table>` elements, creating tables that do not exist in the original document.


-


**Colspan loss.** Some converters drop merged cells and split their content across multiple cells, losing the structural information that a single cell was meant to span several columns.


Our synthetic unit tests confirmed that no converter dominates every table subtype. Some are strongest on sparse checkbox matrices, others on dense numeric tables.


## Summary


The hard part of PDF conversion for RAG is not picking a converter. It is building the evaluation that lets you pick with confidence and keep improving over time.


Your evaluation needs to be specific to your domain, both in the data it runs on and the metrics it computes. Generic benchmarks will not surface the failures that matter to your pipeline. No converter we tested dominates across all dimensions, so choosing one is always a tradeoff. The evaluation framework is what lets you make that tradeoff with data instead of guesswork, and revisit it whenever converters improve or your requirements change.
