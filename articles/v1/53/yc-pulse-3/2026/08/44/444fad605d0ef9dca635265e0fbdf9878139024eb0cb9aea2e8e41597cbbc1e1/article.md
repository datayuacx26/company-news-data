---
schema_version: "1.0.0"
document_id: "444fad605d0ef9dca635265e0fbdf9878139024eb0cb9aea2e8e41597cbbc1e1"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/document-metadata-is-now-available-in-pulse"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-08-08T04:52:09.111881+00:00"
fetched_at: "2026-08-08T04:52:11.321414+00:00"
content_hash: "sha256:1f1077aaa78d41c7905750e0898e81b5a13a3ccdf7fc71d4b71e84acd95fc066"
---

# Document Metadata Is Now Available in Pulse

Document extraction usually starts with what is visible on the page: text, tables, figures, handwriting, and layout. The original file often carries another layer of information that never appears in the rendered document. A PDF can contain authorship and revision history, a workbook can identify hidden sheets, and a photograph can preserve capture time, camera details, and GPS coordinates.


Teams that need both document content and file provenance often maintain separate parsers for every format, then reconcile those results after extraction.


Today, Pulse is making document metadata available as an optional part of the standard Extract pipeline. Pulse reads the original file before conversion or OCR and returns its native properties and deterministic structure alongside the extracted content.


## **How it works**


Enable the feature with extensions.document_metadata set to true. Pulse reads the original bytes, detects the file format, and returns the maximum safely recoverable metadata under extensions.document_metadata. The normal extraction continues unchanged, and fields that do not exist in the source are omitted rather than filled with null values.


The response includes file identity, byte size, media type, and a SHA-256 hash; common properties such as title, authors, keywords, timestamps, application, and producer; custom properties; deterministic structure; and format-specific details.


For example, a ten-page test PDF returned its title and two authors, creation and modification timestamps, PDF producer, custom case ID, page labels, ten outline entries, one attachment, five annotations, and three form fields. The visible pages alone do not expose most of that information, but it now arrives with the extraction result in one response.


## **Across file types**


Pulse recovers metadata from PDFs, DOCX files, XLSX and XLSM workbooks, PPTX presentations, JPEG, PNG, TIFF, and WebP images, HTML documents, and CSV files. PDF results can include XMP, outlines, forms, annotations, and attachments. Workbooks expose sheet names, visibility states, and the active sheet. Images return dimensions, DPI, frame information, and common EXIF fields. CSV files return deterministic characteristics such as encoding, row count, and column count.


## **Available across Pulse**


Document Metadata uses the same Extract configuration across the Pulse API, Platform, CLI, and MCP server. In the Platform, enable Document Metadata under Extract extensions. API requests use extensions.document_metadata, the CLI exposes --document-metadata, and the MCP extract tool accepts document_metadata as a boolean argument.


Enable it on your next extraction to recover the information stored around the document, not only the content rendered inside it!
