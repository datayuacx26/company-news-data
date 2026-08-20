---
schema_version: "1.0.0"
document_id: "c939ad3fcdf8d6bbca620711b30a1d0eaebce65f03925c3229242aee3ee27a0f"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/reducto-leads-benchmark-complex-document-extraction"
published_at: "2026-06-30T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:4f8b71df11e13c4e8f0f852afb5fb4215c9c057fd90a78dc490092abd53708d7"
---

# Reducto Deep Extract Leads Benchmark on Complex Document Extraction

*TL;DR:*


- *We commissioned micro1 to perform and publish the first complex structured extraction benchmark on 225 sourced documents with an average of 88,700+ fields each, comparing results from six major extraction products (Reducto Deep Extract, Extend MAX, LlamaExtract-Agentic, GPT-5.5, Opus 4.8, and Gemini 3.1 Pro).*
- *Reducto Deep Extract ranked 1st on all four grading dimensions:* ***100% completeness, 99.6% precision, 99.6% recall, and 99.3% leaf accuracy.***
- *Frontier LLMs collapse on long documents. Gemini 3.1 Pro and Claude Opus 4.8 completed only 112 and 116 of 225 documents.*


*Engineering by Arth Bohra, Yifei Hu, Jean Billa, and Abhi Arya* . *Written by Donald Wu.*


## Context


Existing structured extraction benchmarks generally fall short where it counts in real world scenarios. Often, they focus on a small subset of examples, emphasizing short documents and extractions with a low number of fields. Real cases customers care about look a lot different: ten thousand field extractions from 300 page invoices, financial statements, and claims, with densities impossible for standard extraction systems and frontier models to handle.


Benchmarks often are created and published by the vendor releasing them, meaning documents get chosen by these individual companies and grading methodologies are tweaked until the best result is achieved. Unfavorable documents get removed from the test set, or are synthetically generated that incorrectly simulate real-life use cases.


That’s why we commissioned micro1 to create an *independent* benchmark focused specifically on difficult document extraction tasks, on real-life documents:


- **An independent data lab was commissioned.** micro1 was responsible for sourcing real documents, performing human validation on the data, and publishing results. Documents were not synthetic, contained diversity across industries, and were not modified in any way by the Reducto team.
- **No extraction tool helps build the answer key.** The "correct" answer-key came from frontier models reading the original documents directly, then were finalized by human reviewers. No document-extraction product was used at any point in the ground-truth building.
- **Failures are counted, not ignored.** Every document is run through every system. Failures, refusals, and null outputs are logged as such rather than dropped.


## Results


Reducto Deep Extract ranked #1 overall as well as within each category, with:


- **100.0% coverage** , completing the full extraction 225 out of 225 documents.
- **99.6%** **precision** . Precision is the fraction of what a system returned that's actually real and exists in ground truth, penalizing made-up, duplicated, or extra rows.
- **99.6% recall.** Recall is the fraction of what should have been returned that the system actually found, and it penalizes missed rows and fields left blank.
- **99.3% leaf accuracy.** Leaf accuracy is the fraction of individual cell values that match the answer key on the rows a system got right
- **0 failures** , such as timeouts, schema incompatibilities, or context window overload.


We breakdown below where each alternative solution fell in the outcome.


Coverage is the first test of an extraction system. Before accuracy matters, the system has to return a usable result. Reducto Deep Extract completed every document in the benchmark, while every other system failed, rejected, or otherwise did not complete part of the corpus. In production, those missing completions become manual reviews or systems that don’t work at all.


But completion alone is not enough. A system also has to extract the data that was actually requested.


Recall is where the benchmark separates systems most clearly. Even when other systems completed a document, several still missed large portions of the required data. GPT-5.5 returned just 52.7% of ground-truth fields, and Gemini returned 48.6%. Reducto Deep Extract reached 99.6% recall while also completing the entire corpus.


That combination matters. A model can appear accurate on the fields it returns, while still leaving too much of the document behind.


Leaf accuracy measures correctness on matched rows:` correct_leaves / total_leaves` . In other words, once a system has returned the right row, did it get the individual values right? Reducto led here as well, with 99.3% leaf accuracy.


Taken together, Reducto Deep Extract was the only system to lead across the full extraction workflow: it completed every document, returned nearly all expected fields, and preserved high per-cell accuracy on the data it extracted.


## Conclusion


The key takeaway: difficult extraction is not just about getting some fields right. It is about reliably extracting all required fields, across the entire document, at production scale. For teams building workflows around document data, recall and failure rate are often the difference between a useful system and one that doesn’t extract enough data to make it worthwhile at all.


Read the[full benchmark](https://www.micro1.ai/benchmark/long-extraction) from micro1, or[contact us](https://reducto.ai/try-deep-extract) to evaluate Reducto Deep Extract on your own documents.
