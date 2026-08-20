---
schema_version: "1.0.0"
document_id: "150a67437d7847e3d282de5cb5960ef055c4e44d573782961ba584bb4315d2f8"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-99d40c54e3ad"
canonical_url: "https://deepnote.com/changelog/2026-04-13"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:08.376400+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:e6d587416116e92d7925e98d7f6c3de773f3703fa263126403c0c455851a64d1"
---

# Polars support, PDF export & a cleaner notebook

## [April 13, 2026](https://deepnote.com/changelog/2026-04-13)


###


[Polars support, PDF export & a cleaner notebook](https://deepnote.com/changelog/2026-04-13#polars-support-pdf-export--a-cleaner-notebook)


####


[TL;DR](https://deepnote.com/changelog/2026-04-13#tldr)


- **Polars support** - Polars DataFrames are first-class citizens in data tables and charts.
- **Export**` .ipynb` **or**` .deepnote` **files to PDF** - turn any notebook into a PDF in a click, straight from the notebook menu.
- **A cleaner notebook & workspace** - better table of contents, nested bullets in text blocks, tidier project sidebar, friendlier empty state, and simpler folder defaults.


##


[Polars support](https://deepnote.com/changelog/2026-04-13#polars-support)


Deepnote now treats Polars DataFrames as first-class citizens:


- They render in the **interactive data table** , with the same filtering and sorting you already get with pandas.
- They work in **charts** , including server-side aggregation for large datasets.


SQL blocks still return pandas DataFrames for now. If you'd like that to be configurable, let us know.


##


[Export your .ipynb or .deepnote file to PDF](https://deepnote.com/changelog/2026-04-13#export-youripynbordeepnotefile-to-pdf)


You can now export any notebook, whether it’s as a` .ipynb` or a native` .deepnote` file, straight to PDF. Just pick **Export as →**choose **PDF with code** or **PDF without code** .


You'll find the full set of export options in the same place:` .deepnote` ,` PDF` ,` .ipynb` , and **Project as**` .zip` **,** in the project topbar menu and in the left sidebar.


Full details in[the docs](https://deepnote.com/docs/export-pdf) .


##


[Cleaner notebook & workspace](https://deepnote.com/changelog/2026-04-13#cleaner-notebook--workspace)


- **Better table of contents**


The table of contents now behaves the same way in notebooks and apps, and it lives on the notebook itself instead of being tucked into the left sidebar. Small change, but the inconsistency had been bugging us for a while.


- **Nested bullet points**


You no longer need a markdown block to create nested bullet lists. Just hit` Tab` to indent and` Shift + Tab` to outdent, right in a text block.


- **A simpler project sidebar**


We removed the separate **Terminals** section from the project sidebar. Terminals are still there when you need them, just without the dedicated sidebar slot.
