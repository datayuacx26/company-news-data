---
schema_version: "1.0.0"
document_id: "06f98e5921303b6acc9b8f9cd7def18ac34f045267e52bad0960050a0514ecad"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/sota-table-parsing"
published_at: "2024-11-04T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:e70a404fd86e5a978ccbc4f59bbde9ef511343d886a2a9ddc6794a87219d293f"
---

# State-of-the-art table parsing

## Overview


Tables in PDFs present some of the most challenging scenarios for parsing PDFs effectively. Although many parsers can effectively parse simple tables with embedded metadata, nearly all pipelines struggle to effectively extract data in complex tables with merged cells, dense text, or gap based structures.


[RD-TableBench](https://reducto.ai/blog/rd-tablebench) is an open source benchmark with 1000 hand-labeled examples from a diverse dataset. This blog uses the scoring framework from RD-TableBench to evaluate multiple table processing models to define the current state of the art.


## Results


The benchmark was run using RD-TableBench and a single pass with all listed vendors. For any vendors with multiple parsing options (ie: "Hi-Res"), we used their premium/accurate parsing pipeline to capture ideal performance as of Nov 4, 2024.


Reducto's models offer state of the art accuracy, with an average table similarity score of 90.2%.


We find that in the market today, cloud providers like Azure and AWS tend to outperform most new entrants, and are followed by 'gpt4o' prompted to extract table content directly.


### Results Viewer


All results are publicly viewable and reproducible. You can view outputs for each datapoint in our[interactive results viewer](https://huggingface.co/spaces/reducto/rd_table_bench) , and explore the open source dataset[on Hugging Face](https://huggingface.co/datasets/reducto/rd-tablebench) .


### Parsing Complex Structures


Reducto outperforms other PDF parsing solutions because our vision models were trained with an emphasis on decomposing table structure.


For a table like the example above, it's insufficient to simply identify basic columns and rows. There can be multiple hierarchies within a complex table, including cells that span the width of the table (projected row headers), nested tabular structures, and multi row/col merged cells. We capture these nuances and output the table in HTML to preserve structure while ensuring strong performance for LLM inference.


We also rely on traditional computer vision techniques for extraction instead of resorting to VLMs for full page parsing. This approach is more difficult to start, but offers meaningful benefits for real world use cases such as deterministic parsing results and the ability to reliably preserve important metadata like bounding boxes that can be helpful with citations.


### Vision Language Models for PDF Parsing


Although gpt4o offers strong performance in the benchmark, we found that the loss cases for vision language models can be severe and challenging to mitigate. For example, in a test for a chemical document with a 4o-mini based solution, the model confidently hallucinates multiple rows of fake data and hallucinates values within cells such as adding element column headers that do not exist in the document.


The frequency of these loss cases increases with more dense tables. VLMs do offer exceptional performance in specific scenarios, such as with handwriting, but we believe that they should be used with strict guardrails for any use case that relies on accurate data.


## Conclusion


Reducto is the most accurate PDF parsing solution, particularly for LLM use cases that involve extracting from tables. If you're building LLM applications and struggling with ingestion accuracy, you can[try our playground](https://app.reducto.ai/share/16a76358-a2cd-41ba-9f21-4e3755c98211) or reach out to us for a demo.
