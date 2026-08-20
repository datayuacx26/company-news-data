---
schema_version: "1.0.0"
document_id: "c0fc37632ff9cbbf0d3deedea7b3fc401e26ad0227124b78f25b42aae5b8e831"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/why-financial-spreading-is-still-a-manual-job"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-28T15:43:29.933178+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:9dda5e358c28f7bb236c91508e34b31e0e6d0d954e1d8a60cc65bdb5cc67d9e6"
---

# Why Financial Spreading Is Still a Manual Job

Ask a commercial lender where their analysts lose the most time and the answer is rarely the credit decision itself. It is the work that comes before it. Spreading, the process of taking a borrower's financial statements and translating them into the bank's standardized template, remains one of the most stubbornly manual workflows in banking. An analyst opens a PDF of audited financials, a tax return, or a management-prepared statement, and begins rekeying numbers line by line into spreading software that has looked more or less the same for twenty years.


The strange part is that this workflow has survived wave after wave of automation. OCR vendors promised to solve it a decade ago. More recently, general-purpose language models were supposed to make it trivial. And yet at most institutions, spreading still takes hours per borrower, still requires a second reviewer to catch keying errors, and still creates a bottleneck that slows every downstream decision from origination to annual review.


## **The document is the problem**


Spreading resists automation because the input is not standardized and never will be. Every accounting firm formats statements differently. A balance sheet from one preparer nests its asset categories three levels deep, while another flattens everything into a single column with indentation carrying the meaning. Subtotals appear and disappear. A line called "other liabilities" on one statement corresponds to four separate lines on another. The statements themselves arrive as born-digital PDFs, as scans of printouts, and occasionally as photographs of paper documents taken at an odd angle.


Traditional OCR reads these documents as undifferentiated text. It can usually tell you that the number 4,250,000 appears somewhere on the page. What it cannot tell you is that the number belongs to the row labeled "total current assets," that the row is a subtotal rather than a line item, that the column represents the prior fiscal year rather than the current one, and that a footnote marker next to the label points to a disclosure that materially changes how a credit analyst should read the figure. Spreading depends on exactly this kind of structural understanding, which is why the analyst never left the loop.


## **What indentation actually encodes**


Financial statements communicate hierarchy visually. An indented line belongs to the category above it. A bolded row with a rule underneath it is a subtotal. A value wrapped in parentheses is negative. None of this is marked up in the file in any machine-readable way, and all of it changes the meaning of the numbers. When an extraction system flattens the visual hierarchy, the output can be technically accurate at the character level and completely wrong at the accounting level, because a subtotal gets treated as a line item and the spread double counts an entire category.


This is where extraction built on vision-language models diverges from legacy tooling. Pulse reads the page the way an analyst does, mapping the layout first, identifying tables and their internal structure, resolving indentation into explicit hierarchy, and linking footnote markers to the disclosures they reference. The output preserves the relationships that make the numbers mean something, and every extracted value carries a bounding box pointing back to its exact location on the source page, so a reviewer can verify any figure in seconds rather than hunting through the PDF.


## **From hours to minutes, with an audit trail**


The practical payoff shows up in two places. The first is speed. When statements arrive as structured data with hierarchy intact, mapping them into a spreading template becomes a tractable problem rather than a data entry job, and the analyst's role shifts from rekeying to review. The second is auditability. Because every value is grounded to coordinates on the source document, the spread carries its own evidence. When an examiner or an internal reviewer asks where a number came from, the answer is a highlighted region on a page rather than an analyst's memory.


Below is a page from a publicly available audited financial statement, shown first exactly as it appears in the source document and then again after processing through Pulse. Setting the two side by side makes the segmentation pass visible: each table, each row structure, and each block of text is identified and located before a single value is read.


*A statement of net position from a publicly available audited financial statement, shown twice side by side. On the left is the original page exactly as it appears in the source document. On the right is the same page with the Pulse extraction overlay applied, where a purple box outlines the statement title, a dark green border marks the detected table region, and green boxes trace every individual cell boundary, including indented line items, subtotal rows, and both fiscal year columns. The comparison shows how Pulse maps the full structure of the statement before extracting a single value.*


Spreading will always require judgment. Deciding how a preparer's "other income" maps to the bank's chart of accounts is credit work, not extraction work. But the part of the job that is pure transcription, the hours spent moving numbers from a PDF into a template, is exactly the part that structure-preserving extraction eliminates. The institutions that treat spreading as a document understanding problem rather than an OCR problem are the ones getting their analysts back to analysis.
