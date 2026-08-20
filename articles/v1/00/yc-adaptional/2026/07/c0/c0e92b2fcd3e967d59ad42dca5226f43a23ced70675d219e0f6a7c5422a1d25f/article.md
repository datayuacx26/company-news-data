---
schema_version: "1.0.0"
document_id: "c0e92b2fcd3e967d59ad42dca5226f43a23ced70675d219e0f6a7c5422a1d25f"
company_key: "yc-adaptional"
company: "Adaptional"
source_id: "yc-adaptional-news-import-c2112bd5ddd4"
canonical_url: "https://www.adaptional.com/blog/foundational-models-hit-94-on-insurance-docs"
published_at: null
first_seen_at: "2026-07-21T04:38:31.952963+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:d15295ea0f6bac577f39c49651b6fa571226eeb4ac89c52faa9b656521b398a0"
---

# Foundation Models Hit 94% on Insurance Docs. So What’s Left?

Something fundamental changed in the last six months, and many people haven’t noticed it yet.


AI research labs have made a convincing argument: *as foundation models improve, the products built on top of them improve too.* This has sparked a wave of AI document-processing systems built on top of foundation models. Rather than competing with the models, these companies sit downstream of them.


Naturally, this raises an important question. As foundation models improve, *what happens to the gap between the model and the product built on top of it?* Does that gap widen, shrink, or eventually disappear?


Until recently, the industry operated on a comfortable assumption:


Off-the-shelf models such as GPT, Claude, or Gemini struggled with the complexity of insurance documents. Six months ago, when we evaluated early models on complex loss runs, average accuracy hovered around 50%. The limitation wasn’t their ability to read text. It was their ability to maintain consistent reasoning across long, inconsistent files.


Today, that assumption no longer holds.


### Introducing LossBench


To understand how well AI systems handle insurance document processing, we introduce[LossBench,](https://github.com/Adaptional/LossBench) a benchmark designed to evaluate foundation models on realistic insurance documents. We focus on loss runs because they are both widely used across insurance workflows and among the most *challenging* documents to process automatically.


Loss runs contain many of the structural complexities underwriters encounter in practice: multi-page PDFs, stitched layouts, wrapped rows, repeated summaries, and inconsistent formatting. LossBench includes over 1,000 real-world extraction tasks, with documents ranging from fewer than 10 claims to more than 400 individual rows.


With LossBench, we can measure *off-the-shelf performance* from publicly available models. In practice, accuracy on arbitrarily long loss runs can exceed 95% when system-level scaffolding techniques such as chunking, deduplication, and, more recently,[recursive language models](https://arxiv.org/pdf/2512.24601) are applied. **


*‍* To be clear, LossBench is designed to measure *baseline model capability* , not the *maximum achievable performance with external scaffolding.*


The top-line results were unexpected. AD8 ( *Adaptional's* *model* ) achieved an average F1 score of **94.9%** , while Gemini Flash achieved **94.6%** , a difference of **just 0.3%** . Claude Opus 4.5 and GPT-5.2 followed at **88.8%** and **88.7%** , respectively.


On *clean* documents, the gap has effectively disappeared. In baseline test sets with fewer than 35 rows, frontier models routinely achieved 99–100% accuracy, matching or exceeding specialized systems.


In one case, both Gemini Flash and GPT-5 Mini extracted *every* claim correctly.


What does this mean for us?


If a system’s only responsibility is extracting fields from relatively clean documents, **foundation models are already at parity** . Extraction is no longer the primary bottleneck.


However, if we probe further, the results indicate a more nuanced answer.


### Diving Deeper into the Numbers


Insurance documents do not become difficult gradually. They become difficult suddenly once they exceed a certain level of structural complexity.


We observed this inflection point consistently in documents exceeding roughly 100 rows. Below this threshold, most models maintain accuracy above 90%. Beyond it, performance begins to diverge significantly:


When segmented by document size, Claude Opus 4.5 *drops* from over **99%** on small documents to **36.4%** on large ones. GPT-5.2 *falls* to **51.6%** . Gemini Flash performs significantly better at **84.9%** , but still shows measurable degradation. AD8 maintains **88.8%** , with a much smaller decline relative to its baseline.


At first glance, this looks like a simple context window problem. As inputs grow larger, models struggle to maintain consistent reasoning across the entire document — a phenomenon often described as[context rot](https://research.trychroma.com/context-rot) *.*


But the aggregate number hides something more specific. When we break performance down by failure mode, a clearer pattern emerges.


On the largest document in the benchmark, containing 443 individual claims ( **lr-12-3** ), GPT-5.2 achieved **82% precision but only 22% recall** . This asymmetry is telling. The model produced *very few false positives, but a large number of false negatives.* In other words, the claims it extracted were almost always correct, but it missed most of them.


‍


We observed the same pattern across multiple formats. In one loss run with **seven coverage types per claim** , GPT-5.2 achieved **96% precision but only 24% recall** , extracting the first coverage type and omitting the remaining six. This points to the model understanding the schema, but lacking in the traversal.


This pattern reflects a *structural limitation* rather than a capability limitation. Even when the full document fits inside the model’s context window, accuracy declines as the document grows. Models begin to lose track of entity continuity, treat related rows as independent records, or stop extraction prematurely.


This raises a natural question: is the limitation in reasoning, or in how the document is *represented* ?


### What if we use OCRs instead of passing raw PDFs?


Structured preprocessing directly addresses this failure mode. We compare performance between passing OCR-extracted text and passing raw PDFs, and find that F1 improves consistently, with *gains that scale with document complexity:*


*GPT-5.2's smaller gain in the 70+ bucket likely reflects stronger native PDF performance in that range, not a degradation from preprocessing on the largest documents.*


### The Reliability Gap


Structured preprocessing improves performance, but it does not resolve the underlying inconsistency. This gap in performance becomes more important when considering the criticality of the documents themselves. Loss runs are not tolerant of omission. Missing a single large claim can materially affect underwriting decisions, pricing, and regulatory compliance. As a result, reliability is not measured only by average accuracy, but by consistency across all documents.


We evaluated consistency using standard deviation across test sets. AD8 showed a standard deviation of **6.2%** , while GPT-5 Mini showed **28.2%** , including worst-case failures as low as **7.3%** accuracy. Even frontier multimodal systems exhibited brittleness. Gemini Flash achieved near-perfect results on some loss runs but complete extraction failure on others depending on input format.


### Closing Thoughts


Foundation models have rapidly commoditized baseline extraction. Tasks that once required specialized systems can now be handled reasonably well by general models.


At a purely numerical level, this convergence is logical. Vertical products improve alongside foundation models, but the metrics themselves have a fixed ceiling. There is only 100% precision, 100% recall, and 100% accuracy to achieve. The gap cannot widen indefinitely. Mathematically, the gap must close.


*But products are not evaluated in a vacuum.* They are evaluated in business contexts. Many workflows require more than raw extraction. They require guarantees such as **completeness, consistency, and explainability.**


These guarantees do not come from the model alone. They must be enforced at the system level. This shifts where value accumulates.


As models improve, access to intelligence becomes less differentiating. *What remains scarce is applying that intelligence exhaustively and reliably across messy, real-world inputs.*


### Data


We’ve shown the results from our tests here, but you can also run these experiments yourself or use the dataset for your own research with[LossBench](https://github.com/Adaptional/LossBench) .


### Acknowledgements


Article written by Jeffrey Xie
