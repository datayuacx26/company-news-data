---
schema_version: "1.0.0"
document_id: "8bcb0bebd0762bd6db8ce3d709c7accf5c12c500737e23ba826a2dd042e4e435"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/rd-tablebench"
published_at: "2024-11-04T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:bbfec4c127880cd6cae83f11efa1985fbbf5d151bc88942af117c9b04629f72c"
---

# Announcing RD-TableBench: An Open-Source Table Benchmark

RD-TableBench is an open benchmark to help teams evaluate extraction performance for complex tables. The benchmark includes a variety of challenging scenarios including scanned tables, handwriting, language detection, merged cells, and more.


The full dataset including labels is available[here](https://huggingface.co/datasets/reducto/rd-tablebench) .


We also benchmarked the extraction performance of various models with RD-TableBench[here](https://reducto.ai/blog/sota-table-parsing) . All result data points are available in the[RD-TableBench Demo](https://huggingface.co/spaces/reducto/rd_table_bench) .


## The Data


Reducto employed a team of PhD-level human labelers who manually annotated 1000 complex table images from a diverse set of publicly available documents. While other approaches benchmarked may have been trained on some of this data, it was unseen to Reducto's models during both training and validation.


The dataset was selected to include examples with different structures, text density, and language. The following graphs show a breakdown of table size by number of cells and language for each table.


## Evaluation Methodology


For the initial release, we evaluated the following tools/methods: Reducto, Azure Document Intelligence, AWS Textract Tables, GPT4o, Google Cloud Document AI, Unstructured, Chunkr, and LlamaParse.


All of the tools, except AWS Textract Tables, are general document parsing solutions which first detect the presence of tables and then provide parsed outputs in a specific format. We used the highest quality settings for each tool where applicable (e.g., Unstructured IO and Chunkr were both run in the more expensive High Res modes, Llamaparse was invoked with Pro mode). Invocation and grading code is available[here](https://github.com/reductoai/rd-tablebench) .


All tools were invoked by passing the table centered in a PDF with whitespace padding. For tools without direct PDF processing ability (GPT4o), we converted the PDFs to images using the` poppler` library.


## Measuring Table Similarity


To benchmark effectively, we needed a metric to determine the distance between two tables. A table can be represented as a 2D string array, with each cell corresponding to a table cell. Merged cells are represented by repeating their values across every cell they occupy. For example:


Hello this is


a test


becomes:


Hello this is


a a test


A simple check for an exact match would penalize minor deviations heavily, so we adopted a more flexible approach using the[Needleman-Wunsch algorithm](https://en.wikipedia.org/wiki/Needleman%E2%80%93Wunsch_algorithm) .


### Hierarchical Alignment Approach


We treat table comparison as a hierarchical alignment problem, similar to DNA sequence alignment, employing a two-level strategy:


1. **Cell-Level Alignment** : Align individual cells within rows using a modified version of Needleman-Wunsch that accounts for partial text matches.
2. **Row-Level Alignment** : Align entire rows using another instance of Needleman-Wunsch, with cell alignment scores informing row-level similarity.


This hierarchical approach captures both structural similarity (how rows align) and content similarity (within corresponding rows).


### Cell-Level Comparison


We use Levenshtein distance to capture partial matches between individual cells, rather than relying on binary exact matches. This metric counts the minimum number of edits required to transform one string into another.


The distance is normalized to a score between 0 and 1:


- 0 represents completely different strings.
- 1 represents identical strings.
- Values in between represent partial matches.


### Row-Level Alignment


At the row level, Needleman-Wunsch is adapted to use the cell-level similarity scores. Missing rows at the beginning or end of tables are not penalized (free end gaps), accommodating natural subtable cropping.


This approach effectively handles:


- Cropped subtables.
- Inserted or deleted rows.
- Split or merged rows.


### Final Scoring


The final similarity score, normalized between 0 and 1, indicates:


- 1.0 for perfectly matching tables.
- 0.0 for completely different tables.
- Intermediate values representing partial matches.


You can explore the full implementation and scoring system[here](https://github.com/reductoai/rd-tablebench) .


## Prior Work


PubTabNet and FinTabNet offer large datasets for table evaluation. However, these datasets are collected from a homogeneous corpus and use programmatically generated labels from file metadata.


Our goal with RD-TableBench is to provide a more diverse set of real-world examples, ensuring accuracy with manual annotations.


## Current SOTA and Future Updates to RD-TableBench


RD-TableBench is intended solely for evaluation and testing purposes, but we recognize that releasing the benchmark risks the use of this data in future model training. To maintain scoring integrity, only a subset of our evaluation framework is being released.


Grading results for various parsing options are available[here](https://reducto.ai/blog/sota-table-parsing) :
