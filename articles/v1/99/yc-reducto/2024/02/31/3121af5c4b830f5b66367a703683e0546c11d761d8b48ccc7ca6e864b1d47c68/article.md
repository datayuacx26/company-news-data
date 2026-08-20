---
schema_version: "1.0.0"
document_id: "3121af5c4b830f5b66367a703683e0546c11d761d8b48ccc7ca6e864b1d47c68"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/document-api"
published_at: "2024-02-27T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:8e1c27bf20a09dc49d77dc27aefe2e6f871f760c1f9e4c36e1f8c95742f4c4e3"
---

# Introducing Reducto's Document API

We've spent the last few months building a powerful document ingestion for LLM workflows. We're excited to share more about what we've built below.[To get started, you can try our demo here](https://app.reducto.ai/share/16a76358-a2cd-41ba-9f21-4e3755c98211) .


### High quality RAG is hard. High quality document processing shouldn't be.


Reducto started when we were consulting for teams building with LLMs. We went into our projects expecting to help teams with fun ML problems, but very quickly learned that one of the biggest bottlenecks across most pipelines was actually well before retrieval or generation.


Simply put, processing PDFs accurately is really hard.


Almost everything on the market worked when we were given simple layouts with perfect file metadata, but those same solutions consistently were slow and inaccurate when we tried using them with complex documents. Multi column layouts get jumbled together, figures are ignored, and tables are a consistent nightmare. Our peers told us that they had to spend dozens of hours building in house processing pipelines because off the shelf solutions weren't enough.


Our goal since then has been to build something so accurate that ingestion becomes a solved problem.


### Our vision focused approach


PDFs are designed for humans to understand visually, so we decided to take a similar approach with our processing.


Our process starts with a layout segmenting model to understand *where* everything is and *what* it is. By classifying every text block, table, image, and figure, we're able to use a very specific approach for each and then recompose the document structure in a way that captures the original content. There's a lot that goes into each pipeline, but in short we:


- Accurately extract text and tables even with nonstandard layouts
- Automatically convert graphs to tabular data and summarize images in documents
- Intelligently chunk information using the document’s layout data
- Process long documents in seconds


We've tested this approach with really challenging examples and found that it's able to perform in cases where traditional document processing fails (like the table below!). You can test our API outputs with your own docs[here](https://app.reducto.ai/share/10962a9e-daba-4ba6-b13c-d2a7520b0855?fbclid=IwAR2lu6L5tbsd68MUm_rKhZv591WUqIQ2Yq-URDi_7uNnf3Ne6ya2IGI2K8g) .


### Quality in, quality out


Bad ingestion leads to low response quality and hallucinations, but we also really think that high quality parsing can meaningfully improve the overall performance of RAG pipelines. We put that to the test by benchmarking overall RAG performance using Reducto's parsing and a few other solutions in the space.


This benchmark uses a[scanned 10-K filing](https://utfs.io/f/c2d22275-9c66-4a0b-bbc5-dad333043d76-c2t3e8.pdf) and 823[question/answer pairs created by LlamaIndex](https://github.com/run-llama/llama-datasets/blob/8fcdc64c19d9d61981c92e7d66982bc57bdc42b8/llama_datasets/10k/uber_2021/rag_dataset.json) to evaluate RAG. We graded each response using GPT-4 alongside manual verification, and tracked latency when processing the document. In order to make the comparison fair we kept all parts of the RAG process besides ingestion fixed for each arm.


You can view our evaluation code and outputs here:[https://github.com/reductoai/benchmark](https://github.com/reductoai/benchmark)


### Work With Us


We've been fortunate to be able to build Reducto alongside the most supportive customers we could ask for, and are ready to onboard more. If you're building with LLMs and would like to improve your document ingestion pipeline, please **reach out to us at**contact@reducto.ai .
