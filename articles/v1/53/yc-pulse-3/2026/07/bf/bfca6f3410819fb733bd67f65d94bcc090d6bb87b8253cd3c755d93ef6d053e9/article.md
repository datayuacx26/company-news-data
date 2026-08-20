---
schema_version: "1.0.0"
document_id: "bfca6f3410819fb733bd67f65d94bcc090d6bb87b8253cd3c755d93ef6d053e9"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/the-geometry-problem-why-tables-are-the-hardest-problem-in-document-ai"
published_at: null
first_seen_at: "2026-07-23T21:33:19.979240+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:bc652da38baab2f5a4273f936191b4ab63f217e1774853425684b373bdae8d95"
---

# The Geometry Problem: Why Tables Are the Hardest Problem in Document AI

When people talk about document AI, they usually focus on text. OCR has largely solved character recognition, and modern language models can summarize or classify paragraphs with ease. But the hardest, most valuable part of documents is not paragraphs of text. It is tables.


To humans, tables are obvious. We see headers, rows, and totals. We understand the relationships immediately. To machines, a table is ambiguous geometry. Flattened into tokens or lines, its structure disappears. And once structure is gone, meaning is gone.


## **How models “see” tables (and why they fail)**


Modern OCR and vision-language models break pages into tokens or small image patches. This is effective for raw text but fails for structured layouts.


- **Patch encoders** like vision transformers divide the page into 16x16 image chunks. Characters split across patches are misaligned.


- **Flattened sequences** lose row and column coordinates. A two-dimensional grid becomes a one-dimensional string.


- **Merged headers** and spanning cells have no obvious representation in a flattened view.


The result is that even if every number is extracted, the relationships between them are scrambled.


## **The illusion of correctness**


The most dangerous failure is not missing text. It is extracting the text correctly but losing the structure.


Imagine a P&L where EBITDA is aligned to the wrong geography, or a rent roll where a tenant’s revenue is shifted under another column. The raw characters look accurate. The outputs pass surface-level checks. But the structure that made the numbers meaningful has collapsed.


This is worse than obvious OCR failure. An error that is visible can be corrected. An error that looks correct but is misaligned propagates quietly through models and decisions.


## **Tables are mathematical objects**


Tables are not text boxes on a page. They are structured mathematical objects.


- **Headers** define variables and hierarchies.


- **Cells** inherit meaning from their header stack.


- **Subtotals** impose constraints. If numbers do not add up, something is wrong.


- **Cross-page continuity** links rows that span multiple pages into one logical unit.


A table can be expressed as a graph or a system of equations. Remove the geometry, and the graph becomes a bag of numbers.


## **Failure modes across industries**


The geometry problem is not confined to finance. It is everywhere:


- **Finance** : P&Ls, credit agreements, regulatory filings.


- **Healthcare** : clinical trial results, lab reports, billing tables.


- **Scientific publishing** : experimental data, statistical tables.


- **Legal** : contracts with schedules of obligations or payment structures.


Every industry where accuracy matters is bottlenecked by table extraction.


## **What good table extraction requires**


Solving the geometry problem means treating tables as first-class objects, not formatting artifacts. A serious system must deliver:


- **Granular bounding boxes** for every cell, header, and merged span.


- **Header stack reconstruction** so cells inherit the full hierarchy above them.


- **Cross-page stitching** to preserve continuity across long tables.


- **Constraint validation** to enforce that totals reconcile with subtotals. **‍**
- **Deterministic outputs** so the same document produces the same structure every time.


## **Why benchmarks understate the problem**


Most public document AI benchmarks sidestep table complexity. Datasets like FUNSD or DocVQA feature small, toy tables with clean formatting.


Real-world enterprise tables look nothing like this. They have deeply nested headers, footnotes, rotated text, multi-page continuations, and inconsistent units. Achieving 90 percent accuracy on FUNSD does not mean a system can handle a 200-line rent roll or a multi-page financial statement.


## **The path forward: geometry-first document AI**


The solution is not more generic text modeling. It is geometry-first design.


- **Layout analysis** must reconstruct grids and spanning cells before semantic labeling.


- **Bounding boxes and lineage** must anchor every value to its coordinates.


- **Determinism** must ensure reproducibility across versions and runs.


- **Validation layers** must enforce constraints and reject outputs that break basic arithmetic rules.


Document AI will not be production-ready until it treats tables as structured geometry with mathematical constraints, not as flattened tokens.


## **Closing: The last frontier**


Tables are the hardest problem in document AI. They are also the most important. Without geometry, document AI is a demo. With geometry, documents become structured, auditable, machine-usable data.
