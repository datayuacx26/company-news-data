---
schema_version: "1.0.0"
document_id: "bdd6c612249ab14f4c1646f3d7253f0a6bd4201b315021cf89d5bd3feb5471a0"
company_key: "yc-strand-ai"
company: "Strand AI"
source_id: "yc-strand-ai-news-import-ee8f1008ced1"
canonical_url: "https://strandai.com/blog/tcga-coad-open-predictions"
published_at: "2026-08-15T00:00:00+00:00"
first_seen_at: "2026-08-20T00:58:37.799173+00:00"
fetched_at: "2026-08-20T00:58:39.767914+00:00"
content_hash: "sha256:fba635f0c164419fdc0b21576aa9898961f52adaa0d0e2e7269b6ec8fa8e2b08"
---

# Open Lattice predictions on TCGA-COAD

# Open Lattice predictions on TCGA-COAD


August 2026 Oded Falik


Virtual staining is usually shown as a figure. A cropped patch, one marker, a chosen field of view. You cannot pan away from it.


So we published the slides instead.


Below is a live Lattice prediction on a public TCGA colorectal (COAD) H&E slide. It is model output, not a physical stain. Drag to pan, scroll to zoom from whole-slide down to individual cells, and use the marker panel to toggle predicted protein channels over the same tissue. No sign-in, nothing staged ahead of you.


Loading interactive slide...


A live Lattice prediction on a public TCGA-COAD H&E. Split view shows H&E on the left and predicted marker channels on the right, with synced pan and zoom.


## What you are looking at


The input is one standard H&E image. No serial section, no additional tissue, no stain.


The output is a set of spatially resolved protein maps at the resolution of the underlying image, which is why you can zoom to individual cells instead of to a coarse heatmap. Toggling a marker changes which predicted channel is composited over the tissue.


Split view is the best way to read it. H&E on the left, prediction on the right, same coordinates and same zoom. Put a structure under the cursor and watch what the model does with it.


## Why colorectal, and why public


TCGA is public, so anyone can pull the same input slide we did. Colorectal is our largest tissue category, and it is one of the indications partners ask about most.


Lattice is pan-cancer. Our training data spans a wide range of tissue types, with stomach, breast, lung, reproductive and pancreatic tissue all well represented alongside colon and rectum. This slide shows you the resolution and the behaviour. The breadth is the part you get on your own cohort.


## Run it on your own slides


Send us H&E you already have. We return predicted spatial proteomics, the analysis, and a written readout.


[Run Lattice on your own H&E](https://app.strandai.com/sign-in)


Research use only (RUO). Not for use in clinical or diagnostic procedures.
