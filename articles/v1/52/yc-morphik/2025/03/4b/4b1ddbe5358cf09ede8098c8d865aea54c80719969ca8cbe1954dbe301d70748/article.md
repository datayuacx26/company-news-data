---
schema_version: "1.0.0"
document_id: "4b1ddbe5358cf09ede8098c8d865aea54c80719969ca8cbe1954dbe301d70748"
company_key: "yc-morphik"
company: "Morphik"
source_id: "yc-morphik-news-import-d199aadc49bb"
canonical_url: "https://dev.morphik.ai/blog/gpt-vs-morphik-multimodal"
published_at: "2025-03-15T00:00:00+00:00"
first_seen_at: "2026-07-22T04:54:38.041510+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:55e354341258365a0d9abf3baa03bc80d354021877845d3fb7a747fbae45c6eb"
---

# When Multimodal Models Go Blind

## Here's the sequence for o4-mini-high:


You hand o4‑mini‑high a technical patent with an embedded IRR vs Frequency graph and ask:


"At what frequency does IRR peak?"


It thinks for 30 seconds and instead of just reading the chart, it hits you with:


"Which page is that on?"


Cue dramatic facepalm. 🤦


Even after I grumbled "Page 6," it pulled out the Python tool use gun (my favorite as well) proclaimed the peak was "the highest point on the line." Technically wrong and hilariously sure of itself.


## Here's the sequence for Morphik:


We treat each page like one giant image+text puzzle:


1. Snap the whole page as an image (diagrams, tables, doodles included)
2. Extract text blocks with their exact positions (headings, captions, footnotes)
3. Blend vision & text embeddings into a multi-vector cocktail 🍹
4. Retrieve the full region (text+diagram) as a unit—no more orphaned charts


Result? The same question returns:


"IRR peaks at 0 MHz." Boom. 🎯
