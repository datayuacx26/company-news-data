---
schema_version: "1.0.0"
document_id: "d8cddec2856cf226449343800bf1de80e5053bc97c3e4bd1334f3bcd3a984dba"
company_key: "yc-extend"
company: "Extend"
source_id: "yc-extend-news-import-054f4f06cd55"
canonical_url: "https://www.extend.ai/resources/long-array-extraction-benchmark"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-24T07:25:34.796069+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:49e8b017764b892cc833d31a4f7b9bc4e4d54afca96c230c72ba680da547fc9e"
---

# LongArray-Extract: A Benchmark for Complete Structured Extraction at Scale

At Extend, we are committed to delivering a reliable, scalable, and performant document infrastructure that leading AI teams can trust to ship agents in production. This is not a theoretical commitment. We build our products and benchmarks around real-world use cases and systems of record documents because production document processing is judged by how the system performs on the hardest documents, not the cleanest examples. Long array extraction is one such example of a difficult problem in production.


Extraction is central to document processing because it captures the critical inputs in unstructured documents and transforms them into structured data. Long array extraction is the ability to capture every repeated item in a document, preserving the cardinality of the underlying record set, the attributes associated with each record, and the relationships between those attributes across pages, sections, tables, and document layouts.


For example, a clinical adverse-event listing might contain 1,283 individual events spread over 234 pages. A Wells Fargo combined statement covers two accounts and 1,100 transactions over 60 pages. A motion for summary judgment contains 705 numbered factual paragraphs over 96 pages.


LongArray-Extract measures whether a system can accurately reconstruct large sets of repeated records from benchmark documents modeled on production systems of record and the long-array use cases we see in customer pipelines. Similar to[RealDoc-Bench](https://www.extend.ai/resources/realdocbench) , it is designed around production-style documents and customer-relevant workloads. Both benchmarks are open-sourced to provide transparency and reproducibility.


## TLDR


LongArray-Extract tests whether extraction systems can return complete, schema-faithful arrays when the output grows from a dozen of rows to thousands.


Extend MAX completed every document at 99.2% accuracy, 2.8x faster than the closest peer. The long tail exposed tradeoffs in completion rate, latency, and partial-output behavior.


TLDR benchmark summary


LongArray-Extract


### Extend MAX completed every document at 99.2% accuracy, 2.8x faster than the closest peer.


Mean per-document accuracy99.2%Failures count as 0%


Completion45/45Extend MAX scored every document


Speed vs closest peer2.8xFaster than Reducto Deep Extract


Largest array2.2kTransactions in one PDF


Speed x accuracy


#### 2.8x faster than the closest peer


Accuracy is aggregate mean per-document accuracy across all 45 PDFs. Latency is mean wall-clock time per document.


Operational overview


Extend MAX reached 99.2% aggregate accuracy at 301s mean latency.


The closest peer in accuracy reached 97.4%, but ran in 846s mean latency. That makes Extend MAX 2.8x faster while still leading by 1.7accuracy points.


#### Latency and accuracy data


Latency is mean wall-clock time per document; accuracy includes failed runs as 0%.


LongArray-Extract aggregate accuracy and mean wall-clock latency by extraction system System Latency Accuracy


Extend MAX


#1


301s 99.2%


Reducto Deep Extract


846s 97.4%


Claude Opus 4.7


532s 83.1%


Reducto Standard


201s 80.9%


Pulse Effort


324s 68.8%


Pulse Auto


219s 64.5%


Gemini 3.1 Pro


332s 47.3%


LlamaParse Standard


88s 47.2%


LlamaParse Agentic


149s 34.7%


GPT-5.5


128s 31.6%


Gemini 3.5 Flash


186s 31.1%


All 45 documents


#### Mean per-document accuracy


Each PDF contributes one score. Failed or timed-out runs count as 0%, so completion is reflected in the accuracy.


Extend MAX


99.2%


01


Extend MAX


#1


99.2%


02


Reducto Deep Extract


97.4%


03


Opus 4.7


83.1%


04


Reducto Standard


80.9%


05


Pulse Effort


68.8%


06


Pulse Auto


64.5%


07


Gemini Pro


47.3%


08


LlamaParse Standard


47.2%


09


LlamaParse Agentic


34.7%


10


GPT-5.5


31.6%


11


Gemini Flash


31.1%


#### Accuracy results


Every PDF contributes one score. Failed or timed-out runs count as 0%.


LongArray-Extract mean per-document accuracy by extraction system Rank System Mean accuracy Completed PDFs


01 Extend MAX


#1


99.2%


45/45


02 Reducto Deep Extract


97.4%


45/45


03 Claude Opus 4.7


83.1%


45/45


04 Reducto Standard


80.9%


45/45


05 Pulse Effort


68.8%


45/45


06 Pulse Auto


64.5%


45/45


07 Gemini 3.1 Pro


47.3%


45/45


08 LlamaParse Standard


47.2%


26/45


09 LlamaParse Agentic


34.7%


35/45


10 GPT-5.5


31.6%


45/45


11 Gemini 3.5 Flash


31.1%


45/45


## How we evaluated


Most extraction tests stop at short documents or fixed field sets. This benchmark focuses on the production failure mode we see when the answer is itself large: the model or extraction system has to keep emitting rows, preserve row-level context, avoid duplicates, and return every expected item.


### The dataset


LongArray-Extract covers use cases we see in production across customer pipelines. For example, dense financial statements with detailed transaction rows, clinical reports with extensive event rows, and enumerated paragraphs for court documents. Each document has generated ground truth, so missing rows and incorrect fields can be scored directly instead of inferred from spot checks.


[Hugging Face dataset](https://huggingface.co/datasets/Extend-AI/LongArray-Extract)


Dataset examples


Source PDF


wf_08_n2200_combined-0VqBcuDi.pdf


### What we measured


The primary metric is per-document extraction accuracy. Each system returns structured JSON for the full document. We compare the extracted output to the expected output field by field, then aggregate those matches into a document score.


The benchmark also tracks completion and latency. A response that fails, times out, or returns an unscoreable output is not the same as a low-quality completed extraction. For reporting, we show both quality and completion behavior because production systems need both.


### Methodology


We evaluated Extend MAX, raw frontier models, and document-AI platforms on the same benchmark documents. The raw frontier-model baselines were run without custom chunking or retry logic, matching the "send this document to a model and ask for JSON" experience. Extend MAX and competitor platforms were evaluated through their extraction systems and API modes, including LlamaParse Standard and LlamaParse Agentic.


All outputs are normalized into a common scoring format. Bank statement descriptions use fuzzy matching to avoid over-penalizing punctuation or whitespace differences. Legal pleading court names also use fuzzy matching, and vendor-specific response shape differences are normalized before scoring to ensure fairness. Failed runs are tracked separately; when we report mean per-document accuracy, failed documents contribute zero.


## What we found


Mean per-document accuracy weights every PDF equally, so a small document and a large document each contribute one score. Failed or timed-out runs count as 0%, which makes the metric reflect whether a system can complete the whole extraction, not just how accurate it is on the rows it returns.


Extend MAX led the aggregate benchmark with 99.2% mean per-document accuracy and 100% completion across all 45 documents.


The closest peer reached 97.4%, followed by Claude Opus 4.7 at 83.1% and Reducto Standard at 80.9%.


All 45 documents


### Mean per-document accuracy


Each PDF contributes one score. Failed or timed-out runs count as 0%.


01


Extend MAX


#1


99.2%


02


Reducto Deep Extract


97.4%


03


Opus 4.7


83.1%


04


Reducto Standard


80.9%


05


Pulse Effort


68.8%


06


Pulse Auto


64.5%


07


Gemini Pro


47.3%


08


LlamaParse Standard


47.2%


09


LlamaParse Agentic


34.7%


10


GPT-5.5


31.6%


11


Gemini Flash


31.1%


#### Complete aggregate results


The complete aggregate matrix for accuracy, completion behavior, and mean wall-clock latency.


Complete LongArray-Extract aggregate benchmark results by extraction system Rank System Mean accuracy Completed PDFs Completion rate Completed-only accuracy Mean latency


01 Extend MAX


#1


99.2% 45/45 100.0% 99.2% 301s


02 Reducto Deep Extract


97.4% 45/45 100.0% 97.4% 846s


03 Claude Opus 4.7


83.1% 45/45 100.0% 83.1% 532s


04 Reducto Standard


80.9% 45/45 100.0% 80.9% 201s


05 Pulse Effort


68.8% 45/45 100.0% 68.8% 324s


06 Pulse Auto


64.5% 45/45 100.0% 64.5% 219s


07 Gemini 3.1 Pro


47.3% 45/45 100.0% 47.3% 332s


08 LlamaParse Standard


47.2% 26/45 57.8% 81.7% 88s


09 LlamaParse Agentic


34.7% 35/45 77.8% 44.6% 149s


10 GPT-5.5


31.6% 45/45 100.0% 31.6% 128s


11 Gemini 3.5 Flash


31.1% 45/45 100.0% 31.1% 186s


### 1. The gap opens in the long tail


Below roughly 200 rows, many systems look close to production-ready. Past that point, the benchmark separates systems that keep enumerating from systems that collapse into partial arrays, sparse samples, or terminal failures.


Long-tail divergence


### System spread widens with document size


#### Long-tail benchmark coverage


Exact document counts and published array-size ranges behind the long-tail comparison.


LongArray-Extract long-tail chart coverage by industry and array size Industry Document type Documents Published array-size range Example


Financial services Bank statements 25 ~150-2,200 transactions 2,200 transactions


Healthcare Clinical adverse event listings 12 31-1,283 events 1,283 adverse events


Legal Legal pleadings 8 27-1,139 facts 27 numbered facts


Each point summarizes the spread between the best and worst system at that document size. The full benchmark includes small, medium, and long-tail documents because production pipelines are judged by the hardest records.


### 2. Completion changes how accuracy should be read


Completion rate illustrates why reliability has to sit next to accuracy. Some systems scored well on documents they completed. But across the full benchmark, incomplete or failed runs changed the ranking once every document was counted.


For production pipelines, "accurate when it returns" is a different property from "reliably returns the whole output." The benchmark reports both so teams can see whether a system is failing loudly, failing silently, or completing with degraded quality.


Completion behavior


### Completeness x Accuracy


#### Completion and accuracy data


Completed-output accuracy is separated from aggregate accuracy so partial completion remains visible.


LongArray-Extract completion and accuracy results by extraction system System Completed PDFs Completion rate Completed-only accuracy Aggregate accuracy


Extend MAX


#1


45/45 100.0% 99.2% 99.2%


Reducto Deep Extract


45/45 100.0% 97.4% 97.4%


Claude Opus 4.7


45/45 100.0% 83.1% 83.1%


Reducto Standard


45/45 100.0% 80.9% 80.9%


Pulse Effort


45/45 100.0% 68.8% 68.8%


Pulse Auto


45/45 100.0% 64.5% 64.5%


Gemini 3.1 Pro


45/45 100.0% 47.3% 47.3%


LlamaParse Standard


26/45 57.8% 81.7% 47.2%


LlamaParse Agentic


35/45 77.8% 44.6% 34.7%


GPT-5.5


45/45 100.0% 31.6% 31.6%


Gemini 3.5 Flash


45/45 100.0% 31.1% 31.1%


Operational overview


Extend MAX completed all 45 PDFs with 99.2% aggregate accuracy.


LlamaParse Agentic completed 35 of 45 documents, but returned lower-accuracy outputs, landing at 34.7% aggregate accuracy.


X-axis is the fraction of all 45 benchmark PDFs completed. Y-axis is mean accuracy on documents where the system returned a scoreable output. Aggregate mean per-document accuracy still counts failed documents as 0%.


### 3. Production tradeoffs include speed


Latency matters only after a system can return the right output. LongArray-Extract therefore compares speed against aggregate accuracy instead of treating runtime as a standalone win.


Extend MAX sits on the aggregate Pareto frontier: no evaluated system is both faster and more accurate. The closest peer in accuracy is 2.8x slower while trailing Extend MAX by 1.7 points.


Latency x accuracy


### Production tradeoffs include speed


#### Latency and accuracy data


Extend MAX


#1


301s 99.2%


Reducto Deep Extract


846s 97.4%


Claude Opus 4.7


532s 83.1%


Reducto Standard


201s 80.9%


Pulse Effort


324s 68.8%


Pulse Auto


219s 64.5%


Gemini 3.1 Pro


332s 47.3%


LlamaParse Standard


88s 47.2%


LlamaParse Agentic


149s 34.7%


GPT-5.5


128s 31.6%


Gemini 3.5 Flash


186s 31.1%


Operational overview


Extend MAX sits on the aggregate Pareto frontier: no evaluated system is both faster and more accurate.


The closest peer in accuracy is 2.8x slower than Extend MAX while trailing by 1.7 points.


Latency is mean per-document wall-clock time, shown on a log scale. Accuracy is aggregate mean per-document accuracy across all 45 PDFs, with failed or timed-out runs counted as 0%.


### 4. Raw frontier models need a harness to close the completion gap


Raw frontier models can understand the document and produce accurate rows, especially early in an extraction. Comprehension is not the problem. It is sustained recall and cardinality as the required output grows.


As arrays get longer, raw model outputs become incomplete: tail rows are omitted, records are sampled from the middle of the document, or valid JSON is returned before every expected item has been captured.


Extend MAX closes that gap by wrapping the frontier model into part of a dedicated extraction harness. The system controls extraction, manages context across smaller units of work, reconciles row boundaries across the full document, and verifies the merged array before downstream systems receive it. The model focuses on understanding the document; Extend MAX handles the orchestration required to return complete arrays at scale.


Completion simulation


### 90 records across 12 pages


Speed, completion, and error rate are derived from aggregate benchmark latency, completion, and accuracy. Fast lanes can still finish with fewer correct rows.


Extend MAX


ref 99.2% / 301s


ok 0


merged 0


dropped 0


Reducto Deep Extract


ref 97.4% / 846s


ok 0


merged 0


dropped 0


Claude Opus 4.7


ref 83.1% / 532s


ok 0


merged 0


dropped 0


Reducto Standard


ref 80.9% / 201s


ok 0


merged 0


dropped 0


LlamaParse Standard


ref 47.2% / 88s


completes 57.8%


ok 0


merged 0


dropped 0


LlamaParse Agentic


ref 34.7% / 149s


completes 77.8%


ok 0


merged 0


dropped 0


GPT-5.5


ref 31.6% / 128s


ok 0


merged 0


dropped 0


Simulation inputs and assumptions


#### Measured benchmark inputs


Measured aggregate values used to scale the animation. The simulation is illustrative, not a raw benchmark run.


LongArray-Extract aggregate benchmark inputs used by the completion simulation System Reference accuracy Completion Mean latency Simulated maximum


Extend MAX


#1


99.2% 100.0% 301s 90/90


Reducto Deep Extract


97.4% 100.0% 846s 90/90


Claude Opus 4.7


83.1% 100.0% 532s 90/90


Reducto Standard


80.9% 100.0% 201s 90/90


LlamaParse Standard


47.2% 57.8% 88s 52/90


LlamaParse Agentic


34.7% 77.8% 149s 70/90


GPT-5.5


31.6% 100.0% 128s 90/90


captured


merged / duplicated


dropped / not returned


page boundary


### 5. Production pipelines need more than long context


LongArray-Extract shows that this problem is not solved by context length alone. The document can fit in the model window and still fail because the required answer is too large to emit reliably in one turn.


For production document workflows, that difference shows up as missing transactions, adverse events, facts, or citations that downstream agents treat as complete records. The stronger approach is orchestration around the model: choose document representations, route by expected output load, split work deliberately, preserve global context, repair boundary issues, and verify the merged array before downstream systems receive it.


## How Extend MAX extraction works


Long array extraction requires Extend MAX.


Extend MAX extraction uses dynamic chunking for large documents based on table size, table density, and schema complexity. The chunks are chosen to preserve semantic context as much as possible, so related rows, headers, sections, and field definitions stay connected during extraction.


The extraction then makes multiple passes through the full document. Those passes persist split context across the long-document extraction, reconcile rows that cross chunk boundaries, and verify that the merged array is complete before the output is returned.


Smaller models run alongside the main extraction to detect and fix mechanical issues around page boundaries, section transitions, repeated headers, continuation rows, and other boundary conditions that can create dropped, duplicated, or merged records.


## Try it for yourself


- [Hugging Face dataset](https://huggingface.co/datasets/Extend-AI/LongArray-Extract)
- [Extend demo playground](https://dashboard.extend.ai/demo)
- [See all Extend benchmarks](https://www.extend.ai/benchmarks)
