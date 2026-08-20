---
schema_version: "1.0.0"
document_id: "c465982cc6edc20043b3440f1859b6d383a9dfe873e14b01b7cd07f3b693b84e"
company_key: "yc-tesora"
company: "Tesora"
source_id: "yc-tesora-rss-4d7d570f276c"
canonical_url: "https://tesoraai.substack.com/p/when-excel-broke-we-rebuilt-it-with"
published_at: "2026-06-26T18:48:55+00:00"
first_seen_at: "2026-08-10T03:58:35.856576+00:00"
fetched_at: "2026-08-10T03:58:36.939902+00:00"
content_hash: "sha256:e82a28d23e358c7e6c0c6b7fcf7d3e6569cf9f47fe9ad9da17ada51f62a2d14d"
---

# When Excel broke, we rebuilt it with a billion rows

# When Excel broke, we rebuilt it with a billion rows


### How Tesora rebuilt Excel to overcome its biggest shortcomings.


[AI for Actuaries @Tesora.ai](https://substack.com/@aiforactuariestesoraai)


,[Federico Reyes Gomez](https://substack.com/@federicoreyesgomez)


, and[Philo Bishay](https://substack.com/@philobishay)


Jun 26, 2026


Excel is the de-facto standard “language” for finance and insurance. Despite this, actuaries leave Excel for two main reasons. (1) It can only store about a million rows (1,048,576 to be exact), so it breaks with real insurer datasets. And (2) It sloppily stores data and logic in the same place, making auditability difficult. Tesora engineered a solution to fix both of these problems.


## **The abstractions that break everything**


Actuarial spreadsheets have a hard ceiling. Excel starts degrading past roughly 100k rows of model computation and caps out at about 1M. The moment actuarial spreadsheets hit real world data; five million policies, six hundred million payment records, they stop being a tool and become a liability. The industry’s standard answer is to rewrite in code: A data engineer re-expresses the actuary’s logic in SQL, R, or Python. That rewrite introduces what we call a reimplementation boundary. The actuary owns the spreadsheet, IT owns the code, and the two artifacts can now diverge silently. The most troublesome bugs are those where plausible-but-wrong numbers occur without raising an exception.


Keep the spreadsheet as the language and replace the calculator underneath it.


We took the opposite approach. Keep the spreadsheet as the language and replace the calculator underneath it. This is a technical account of how we accomplished this.


## **A spreadsheet is a relational query plan in disguise**


Excel is really four things welded together: formula semantics, an authoring surface of cells, ranges, and named expressions, a reactive incremental dependency graph, and a plugin host for domain-specific libraries. The scalability problem lives in the first two, the memory model behind cells and functions. A columnar engine solves that part well. But a columnar engine gives you nothing for the dependency graph, and the dependency graph is exactly what makes a spreadsheet feel alive.


So the engine keeps the authoring surface and the dependency graph intact and replaces the executor. What makes this tractable is that every Excel formula is already a relational node in disguise:


Once the grid is lowered to a logical IR where a scalar cell, a 12×12 development triangle, and a 100M-row policy table are all just Relations, scale stops being a special case. It becomes a routing decision. The same formula runs in-process for 200 cells or against a database for a billion rows.


The architecture has three layers, and the seam between them is structural, not cultural:


Layer 1 is the promise to the actuary. Layer 3 is an implementation detail we reserve the right to change forever. Layer 2 is the thing nobody hands you off the shelf, because it has to be two contradictory things at once. It has to stay incremental, good for a human nudging a tail factor, and capable of pushing a large node to a columnar kernel. No existing engine is good at both, and that difficulty is the reason the layer is the work.


## **Measuring what the engine actually buys**


We ran the real production rater, unchanged, at one risk, 10,000 risks, and the full book.


Batch rating, 10,000 LHS-sampled risks, real production rater:


The number that matters in that table is not the throughput. It is that the checksums are identical across all three paths. For natively-lowered ops like SUM, SUMIF, and VLOOKUP, the engine runs 6.5× to 124× faster than the baseline across the 10 to 10k row range, and strictly faster past 100k, because the baseline cannot run there at all. The engine handles 100k to 500k in-process and a billion rows through columnar streaming, with ungrouped sum benchmarked at 36M rows/s. For a formula the engine cannot lower, it falls through to the baseline, equal by gating.


One routing intuition the measurements disproved is worth naming. SQL pushdown loses on in-memory data. Table ingest costs roughly 1,823 ns/row for the SQL path versus 26 ns/row for the columnar path, so pushdown only pays off on warehouse-resident data where ingest is free. The routing model treats hard limits, Excel’s row ceiling and OOM, as capability, so canRun returns false and the planner never routes there. It treats cost cliffs like cache-to-disk spill as cost the model prices explicitly. Keeping those two categories separate is what lets routing be both safe and economical.


## **Right-sizing: only climb as far as the workload forces**


The most expensive machinery available, true differential dataflow with sub-second edits directly over 100M raw rows, is also the one actuarial models almost never need. The shape of an actuarial model explains why. The enormous node, scan the book and build a triangle or factor table, is upstream and stable. The interactive knobs, trend, tail factor, rate change, are small and downstream, feeding off a 200-cell aggregate rather than the raw rows.


We climb a deliberate complexity ladder and stop at the rung the workload forces:


The benchmark that settles the question: across a 300-edit parameter sweep over a 2M-row book, sourceScans equals 1 in every mode. The actuary drags a trend assumption back and forth thirty times, the five-million-row scan happens exactly once, and each edit recomputes only the small downstream cone. The book is never re-read.


## **From model to system: how declarative computation compounds**


Speed and scale unlock a question the spreadsheet-as-file paradigm never had to answer. What can you build on top of a model once it runs fast enough to be called?


When the computation is a rewrite, Python code someone hand-translated from a spreadsheet, the answer is limited. The model is a closed artifact. Calling it from another analysis means importing it, trusting the translation stayed correct, and accepting that future changes to the spreadsheet have to be manually re-propagated to the code. When the computation is a compiled declaration, the model becomes a node in a larger graph, and a few things follow from that.


**The same model serves three cardinalities without branching.** A rating model that prices one risk interactively is, structurally, identical to the model that prices 10,000 risks in a parameter sweep or the full in-force book in a portfolio roll-up. Cardinality is an input to the router, not a property of the model. An actuary can author a model at the level of a single risk, where the logic is easiest to inspect and validate, and have that exact logic be what runs at book scale. There is no production version.


**Versioned models become replayable.** Because the model is a serializable DAG rather than a mutable file, every version of it can be stored and re-executed against any data vintage. The question “what would last year’s rate plan have produced on this year’s book?” is a routing query, not a forensic exercise. This is the foundation of regulatory reproducibility, and more practically, of understanding rate change attribution without reconstructing the model from memory.


**Analyses compose into pipelines.** Once one model is a callable node, a second analysis can depend on it explicitly rather than copy-pasting its outputs. A GLM relativity model feeds a rating model feeds a portfolio aggregation feeds a reserve adequacy check. Each step is authored in the same declarative surface the actuary already uses, and the engine handles the data flow between them. The alternative, separate spreadsheets connected by manual exports, produces a system with no explicit dependency graph and no way to propagate a change from step one through to step four automatically.


**What-if analysis becomes systematic rather than ad hoc.** A parameter sweep over trend assumptions, loss development factors, or territorial relativities is just a batch execution with a structured input space. The same logical plan runs against a grid of inputs, and the results are comparable because they came from the same compiled model, not from someone re-running a file by hand and hoping they got the right version. Over time, the sweep history is itself a dataset, a record of how the model’s outputs respond to its inputs, and that dataset supports the kind of sensitivity analysis regulators ask for and pricing committees actually need.


The compounding effect is the point. Individual analyses built on a shared, callable, versioned model accumulate into an institutional knowledge base where each new piece of work is additive rather than redundant. A team that has been doing this for two years has a library of validated models, a traceable history of every rate change, and a test suite that catches regressions automatically. A team still passing spreadsheets by email has to reconstruct all of that from scratch each time someone asks a new question.


## **Turning computations into compounding systems**


The performance numbers matter. But the more important consequence is the reimplementation boundary described at the start. When a rate filing has to be defended to a regulator, “this is the model, and the production numbers came from this exact computation” is a categorically stronger claim than “the spreadsheet was reimplemented in Spark by another team, and we tested that the two agree on a sample.”


An actuary can run ad-hoc analysis to gain insights today, but leading actuaries add strategic value to the companies they run by building systems that compound value. They do this by building pipelines that codify their expertise, tribal knowledge, and gut instincts into routines that can be run deterministically, continually tweaked, and glued together. Imagine your loss runs feeding into your GLM which feeds into your rater which generates quotes that you tie back to claims that you use to tweak next year’s model, so on and so forth. The north star is a fully connected data feedback loop that continuously updates all reports and analyses in realtime, allowing the system to start spotting anomalies and proactively alerting you when there are problematic focus areas.


If you want to talk about how to build scalable actuarial pipelines without depending on IT or having to configure coding agents, please reach out to us! We’re always happy to help.


[Share](https://tesoraai.substack.com/p/when-excel-broke-we-rebuilt-it-with?utm_source=substack&utm_medium=email&utm_content=share&action=share)


A guest post by


[Federico Reyes Gomez](https://substack.com/@federicoreyesgomez?utm_campaign=guest_post_bio&utm_medium=web)


CTO and Founder of Tesora


[Subscribe to Federico](https://federicoreyesgomez.substack.com/subscribe?)


A guest post by


[Philo Bishay](https://substack.com/@philobishay?utm_campaign=guest_post_bio&utm_medium=web)


Head of Product, former chief actuary, MBA, lover of learning


[Subscribe to Philo](https://philobishay.substack.com/subscribe?)
