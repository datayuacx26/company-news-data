---
schema_version: "1.0.0"
document_id: "106f656f389607bc1ae40abe8ec8c09266d1dbff31d96102daf5fe9211372026"
company_key: "yc-model-ml"
company: "Model ML"
source_id: "yc-model-ml-news-import-691cd1d4942e"
canonical_url: "https://modelml.com/articles/introducing-the-model-ml-composite"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-31T23:42:22.239551+00:00"
fetched_at: "2026-07-31T23:42:24.116382+00:00"
content_hash: "sha256:bc25139c6f5a68a7b50f75d47cb924fe3dd13418b438e93596c116f130d9f798"
---

# Introducing the Model ML Composite

#### *The Model ML Composite is our evaluation benchmark for AI in Financial Services. It turns model selection from a standing bet into a measured, per-task decision, and keeps that decision current as the frontier moves.*


### Overview


Benchmarks have historically measured whether a model gets the answer right and how quickly. As inference spend grew materially, token consumption became a third dimension. But none of these tell a financial institution what a piece of work will cost.


Accuracy and latency are the properties of a model and the infrastructure under it. Cost is a property of the whole system: the model, the harness it runs inside, how many times it retries, how many tools it calls, how much context it re-sends, and how much of that context arrives cached. Hold the model constant, change the harness, and the number changes.


The distinction had little practical weight while a task meant one call to one API. The model and the system around it were close enough to the same thing that pricing them separately would have been pedantry. Agentic work pulled them apart. A task now runs as a loop of model calls, tool calls and re-sent context, continuing until the agent is done, and the bill covers every step. How long that loop runs depends on the model's judgment and on the harness in roughly equal measure. The harness decides whether context is re-sent or compacted, what gets cached, and how many retries a failed tool call is allowed. A model that reasons efficiently but calls tools carelessly can cost more than a slower, nominally pricier one that gets there in three steps instead of eleven.


Price also stopped tracking capability. When every candidate was API-priced by one of four labs, the two moved together closely enough that a ranking served as a procurement decision. The strongest open-weight models now trail the frontier by roughly six months on raw performance while pricing at a fraction of the cost.


As a result, model selection is now evaluated as a frontier: the set of models for which no alternative is both better and cheaper, and therefore the only set worth routing to.


Token efficiency was a move in this direction, but it still measures the wrong quantity. Two models with near-identical token usage can differ several-fold in cost once unit pricing, the input/output split, and cache eligibility are applied. For example, Fable 5 and Gemini 3.6 Flash invert on precisely this point in document intelligence. Flash uses more tokens and still costs less, because Fable's premium sits in unit pricing rather than volume.


Finance compounds the problem. Pulling a figure from a filing and building a valuation model require significantly different capabilities. Across the tasks we tested, no single model led on all of them. Performance between the highest and lowest-scoring frontier models on a single task varied by more than 30 percentage points. A single-model commitment therefore guarantees you are wrong somewhere, overpaying on the work a leaner model handles or underperforming on the work that needs a frontier one.


Rather than tying the platform to one provider, Model ML routes every task to the model best suited to it. The Model ML Composite is the evaluation behind that routing. It tells us which models perform best on specific kinds of financial work, where a cheaper model is sufficient, and when a new release actually changes the answer.


### Our approach


Every model in the Composite runs inside the Model ML agent harness. Holding the system fixed and varying only the model is what makes a cost difference attributable to the model at all. It also means the tasks are scored the way they would actually be run, with the same tool access, retry behavior and context handling in every case.


For each run we record the full trajectory: every model call, every tool call, and every token of context re-sent. Cost is computed from published unit pricing, applied separately to input and output tokens, with cached input priced at the cached rate. That produces a figure in dollars per task rather than a token count, which is the only form in which the number is comparable across models.


For our analysis, we evaluated 13 models against hundreds of real-world finance tasks, spanning major closed-source models from OpenAI, Anthropic, Google, Meta, and xAI, alongside the strongest open-weight models from labs such as Mistral, DeepSeek, Moonshot, and Z.ai.


Unlike many existing finance-related benchmarks, which focus primarily on common financial tasks such as filing review or earnings analysis, the Model ML Composite also evaluates the quality of final deliverables, including pitch books, financial models, and end-to-end workflows.


Our approach reflects the work finance professionals ask agents to produce: investor presentations, board materials, and similar high-stakes outputs that must not only be accurate but also professionally formatted and grounded in judgment.


Tasks were created by domain experts in financial services and evaluate model performance against defined criteria covering PowerPoint creation, Excel creation, analytical finance, multi-step financial workflows, and document retrieval. We identified several task sub-types, each best served by a different category of model at the Pareto frontier of performance and cost. These are the models for which no alternative is both better and cheaper, and therefore the only candidates worth routing to.


An overview of these tasks can be seen below:


-


**PowerPoint creation:** Slide quality, measured through geometric checks that assess slide harmony alongside styling checks performed by LLM judges against our index of materials.


-


**Excel creation:** The correctness of the financial model produced with specified inputs.


-


**Analytical finance:** The ability to perform complex, multi-step quantitative analysis.


-


**Financial workflows** : The ability to decompose a complex analysis into a multi-step workflow and execute it the way a finance professional would: right sources, consistent bases, bridges that foot.


-


**Document retrieval:** The system's ability to perform tasks using context drawn from a vast array of financial documents.


### Scoring


*Figure 1. Model accuracy per task. Use the task dropdown to navigate results against tasks and the models dropdown to filter the view.*


Every model runs each task inside the Model ML agent harness, against curated datasets and rubrics built by ex-finance professionals, so results reflect performance on real finance work. Each task is graded against its criteria in two ways:


-


**Deterministic checks:** These are objective, rule-based measures. For example, whether an agent's calculated IRR and MOIC on an LBO match expected values within a defined tolerance of ±0.5%, and whether structural tests pass, such as the debt schedule amortizing correctly, or the sources and uses table balancing to zero.


-


**LLM-as-a-judge:** Each criterion is graded by an AI model acting as a grader, using a rubric written specifically for that criterion with clear definitions of what each score means. Every criterion is graded independently and orthogonally rather than collapsed into a single generic "quality" score.


Each criterion is normalized to a scale from 0 to 1. A model's score for a dataset is the average across all items, shown as a percentage, so results are comparable across datasets of different sizes.


### Key takeaways


*Figure 2. Model performance per cost across tasks. Use the task dropdown to navigate results against tasks and the Models dropdown to filter the view.*


**The strongest, most cost-efficient results come from matching the model to the task.** Finance doesn’t involve a single task, and no single model performs best at all of them.


Here’s what we found across the categories we tested:


-


**Analytical finance justifies the frontier premium.** Opus 5 led, with GPT-5.6 Sol close behind and Fable 5 next. These were the most accurate models tested on multi-step, multi-company tasks such as peer-group margin and growth screens, VWAP work, sector-specific analysis, cash-conversion and leverage analysis. The edge came from definitional discipline, working from company-reported rather than vendor-standardized figures, on the right basis, using the appropriate statistical convention. These are the distinctions that decide whether a multi-step quantitative analysis actually ties out. Cheaper models did not come close enough for the saving to be worth taking.


-


**On the other hand, single-filing extraction is where the frontier premium is hardest to defend.** For extracting figures from filings and large document sets, Fable 5 was the most accurate diligence engine. On single filings, it returned the exact figure and cited the correct source with a score of 83.7%, ahead of Gemini 3.6 Flash and Grok 4.5. However, GPT-5.6 Terra reached 93% of Fable 5's score at approximately 2% of the cost.


-


**On multi-document retrieval, open-weight models keeps pace with closed-source ones.** Gemini 3.6 Flash led on multi-document work, with Opus 5 and Fable 5 just behind in the tightest race in the benchmark. GLM 5.2 scored within a few points of Gemini 3.6 Flash at a small fraction of its cost. On a diligence workload of thousands of documents, the accuracy you give up costs less than the price you save..


-


**Financial workflows favor the GPT-5.6 family of models at both ends of its price range.** Scoring used a cross-family judge panel so that no model's own family could inflate its result. GPT-5.6 Sol scored highest, with Terra and Gemini 3.6 Flash just behind, and Opus 5 a fraction back. Fable 5 and Sonnet 5 trailed the frontier on the sourcing-and-basis discipline the score rewards. Terra's position matters most commercially, since it sits within a point of the category leader at a small fraction of Sol's cost.


-


**PowerPoint creation was a three-way race, and the right pick depends on what you optimize for.** GPT-5.6 Sol, Fable 5 and Opus 5 were effectively tied on overall deck quality, and each earned its score on a different component. Sol produced the highest share of client-ready slides, Fable 5 followed the brief most faithfully, and Opus 5 produced the best-looking output. Sol also slightly outscored Fable 5 at roughly a third of the cost, which gives it the strongest position in the category. GPT-5.6 Luna is the cheap end of the frontier at about 5% of Fable 5's cost, with a visible quality trade-off.


-


**On Excel model creation, a quarter of the price bought the same result. GPT-5.6 Sol leads,** with Opus 5 and GPT-5.6 Terra close behind. Scores were lower across the board than in other task categories, reflecting how demanding correct end-to-end model construction is. Terra matched Opus 5 here at roughly a quarter of the cost, the clearest instance in the Composite of a cost-optimal model and a frontier model producing the same answer.


-


**The strongest open-weight models trail the frontier by roughly six months on raw performance, and frequently sit on the cost-performance frontier regardless.** Where a task runs at volume and the accuracy gap is narrow, the open-weight option is the correct choice on the merits. The gap widens in analytical finance and Excel modelling, and narrows to near nothing in retrieval.


### Conclusion


Across thirteen models and five categories, no model led on all of them, and no two categories produced the same cost-optimal choice. The highest score and the best value were usually different models.


The current results of our Composite have a shelf life. Every finding here describes the frontier as it stands today. But frontiers move. A new release can reset a category, and a price change can reset one without any model getting better. The models on the frontier today are on it because of where the curve sits today, which means a measured decision decays into a standing bet the moment measurement stops.


The Composite is built to be re-run rather than published. We update it as models ship, as harnesses change, and as pricing moves, so that routing reflects the current frontier instead of the one that held when the evaluation was written.


For a financial institution, the practical consequence is that an intelligence strategy is a measurement capability before it is a vendor choice. Firms that can tell which model is cost-optimal for which task, and can tell when that answer changes, will spend less and produce better work than firms that picked well once.
