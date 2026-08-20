---
schema_version: "1.0.0"
document_id: "b97442c55442676a4cd2b575c3ea305742b78ddaf2982a07f48b8a51a09dbad6"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/how-subsurface-maps-actually-encode-information"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-24T09:39:59.698897+00:00"
fetched_at: "2026-07-28T22:36:38.456755+00:00"
content_hash: "sha256:54f3d64b012b50916b77bc6a0273c3dc00520079f97348bab833b9f1233a214e"
---

# How Subsurface Maps Actually Encode Information

Oil and gas is an industry that runs on documents that were never meant for machines, and the capital allocation decisions that matter at an integrated operator are made on the basis of a document stack that spans every era of the business. Within that stack, the subset that presents the highest barrier to automation is the visual one, where the well logs and the structure maps contain domain-specific symbolic conventions that almost never appear in the training distributions of general-purpose extraction systems.


This is the central reason upstream digital transformation programs have stalled out at the document layer even after every other system in the value chain has been modernized, and there is a specific structural reason for that breakdown, with the same shape on every visual document in the upstream stack. The most consequential element on a geological map is not the map itself, but the small block of text and symbols in the lower corner that explains what the rest of the page means, and that small block is exactly what generic document AI consistently fails to read.


This is the core asymmetry that defines a large class of subsurface documents. The page presents a visual field of symbols, colors, fill patterns, line styles, and proportional shapes, and almost none of those marks carry intrinsic meaning. They are placeholders that have been bound to specific quantities, formations, rock types, or interpretive categories by a separate piece of the page that the rest of the document indexes against. A circle on a basin map is not a circle. It is a value, but only because the legend says so. A hatched fill on a cross-section is not decoration. It is a lithology, but only because the legend says so. A dashed contour on a structure map is not just a line of a different style. It is a data-reliability indicator, but only because the legend says so. The information density of the page is high, but the page is unreadable without the decoder ring in the corner.


Consider a regional petroleum occurrences map of the kind a basin-level geologist or a new-ventures team would use to compare the productive endowment of a sedimentary basin against its neighbors.


A generic document AI system asked to read this page produces a list of latitude tick numbers down one side, a sequence of basin names in roughly geographic order, and a scattered set of numerical values with no association to anything. The output looks superficially complete because every word and number on the page has been transcribed somewhere in the output. The substance of the document, the basin-by-basin reserves estimate that is the entire reason the page exists, has been silently discarded. The right way to handle this document is to invert the order in which the page is consumed. The output that matters from this kind of page is not a transcription of every label, it is a per-basin record with the symbol type, the symbol size, the numerical value associated with the symbol, and the unit derived from the legend. The numerical values exist in the source document. They are simply not retrievable without the legend mediating between the symbol on the map and the quantity it represents.


‍


A regional petroleum occurrences map of Indonesia, parsed by Pulse. The legend defines four symbol types for oil and gas reserves and for shows and discoveries. Bounding boxes preserve the location of every symbol, and the page resolves into a per-basin record where every value is decoded against the legend.


The same asymmetry repeats on different document types with completely different legend conventions. A geological cross-section is the cleanest illustration. The page presents a side view of a subsurface transect, with a continuous band of rock units stacked vertically and patterned to indicate their identity. A parser that fails to read the legend mapping cannot tell sandstone from claystone in the structured output, which means it cannot tell reservoir from seal, which means the cross-section, the most fundamental document in any subsurface evaluation, becomes operationally useless even when every label and axis value has been correctly transcribed.


A geological cross-section near Logan Butte, parsed by Pulse. The legend in the upper right binds each fill pattern to a rock type. Bounding boxes preserve the position of every element, and the section resolves into a stratigraphic column with the fault, the water table, and the marker layer separated from the lithology layers.


This is what general-purpose document AI is structurally bad at. The dominant approach in commodity OCR and document-AI systems is to read the page in approximate left-to-right, top-to-bottom order, recovering tokens and emitting them in a sequence. That approach treats the legend as one more block of text on a page full of other text, and it treats the symbols on the map or section as either invisible or as isolated graphical elements with no semantic relationship to the legend. The system has no representation of the fact that the small block in the corner is supposed to govern the interpretation of every other mark on the page. The system has no way to express in its output that the value 13.7 next to a basin name on the map should be tagged with the unit "billion barrels of oil" because of a sentence three hundred pixels away in the legend. The connection exists in the document's design and in the reader's head. It does not exist in the parser's data model, and as a result it does not exist in the parser's output.


Recovering that connection is what document intelligence for subsurface workflows has to do. The parser has to recognize that a region of the page is a legend before it has fully parsed the rest of the page, so that subsequent decoding of symbols can be conditioned on it. The parser has to bind each entry in the legend to a structured key that captures the symbol's geometry, its fill state, and the meaning the legend assigns to it. A reserves estimate on the petroleum occurrences map is not just a number, it is a number anchored to a basin, and the basin is anchored to a location on a coastline.


The output of this kind of system, on the documents discussed above, is not a transcription. It is a structured representation that captures the legend as a named key, captures every map symbol or section pattern as a typed entry against that key, and preserves the bounding-box geometry of every element so that the output can be audited back to the source page. What is new is the realization that automation cannot be applied to this class of document until the parser learns to read the legend the way the domain reader does, which is first.


[Pulse](https://www.runpulse.com/) was built for documents whose meaning lives in their structure rather than in their text, and the subsurface map is the canonical instance of that pattern. Neither one is useful without the other, and neither one is recoverable by a system that does not recognize the relationship between them.


‍[Reach out to our team here](https://www.runpulse.com/contact-sales) if you are interested in learning more.
