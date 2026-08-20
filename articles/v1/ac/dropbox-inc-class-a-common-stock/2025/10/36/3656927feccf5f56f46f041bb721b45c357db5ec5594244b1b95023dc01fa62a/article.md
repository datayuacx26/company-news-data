---
schema_version: "1.0.0"
document_id: "3656927feccf5f56f46f041bb721b45c357db5ec5594244b1b95023dc01fa62a"
company_key: "dropbox-inc-class-a-common-stock"
company: "Dropbox Inc."
source_id: "dropbox-inc-class-a-common-stock-news-import-0470a1f405f8"
canonical_url: "https://dropbox.tech/machine-learning/practical-blueprint-evaluating-conversational-ai-at-scale-dash"
published_at: "2025-10-02T00:00:00+00:00"
first_seen_at: "2026-07-25T01:59:28.029954+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:37df68d414314d97b11754365c90730dd06de5d394f9e2044b1c336c778b274d"
---

# A practical blueprint for evaluating conversational AI at scale

LLM applications present a deceptively simple interface: a single text box. But behind that minimalism runs a chain of probabilistic stages, including intent classification, document retrieval, ranking, prompt construction, model inference, and safety filtering. A tweak to any link in this chain can ripple unpredictably through the pipeline, turning yesterday’s perfect answer into today’s hallucination. Building[Dropbox Dash](https://www.dash.dropbox.com/) taught us that in the foundation-model era, AI evaluation—the set of structured tests that ensure accuracy and reliability—matters just as much as model training.


In the beginning, our evaluations were somewhat unstructured—more ad-hoc testing than a systematic approach. Over time, as we kept experimenting, we noticed that the real progress came from how we shaped the processes: refining how models retrieved info, tweaking prompts, and striking the right balance between consistency and variety in answers. So we decided to make our approach more rigorous. We designed and built a standardized evaluation process that treated every experiment like production code. Our rule was simple: Handle every change with the same care as shipping new code. Every update had to pass testing before it could be merged. In other words, evaluation wasn’t something we simply tacked on at the end. It was baked into every step of our process.


We captured these lessons in a playbook that covers the full arc of datasets, metrics, tooling, and workflows. And because people don’t just work in text, evaluation must ultimately extend to images, video, and audio to reflect how work really happens. We’re sharing those findings here so that anyone working with LLMs today can replicate our evaluation-first approach for themselves.


Dropbox Dash: AI that understands your work


Dash knows your context, your team, and your work, so your team can stay organized, easily find and share knowledge, and keep projects secure, all from one place. And soon, Dash is coming to Dropbox.


[Learn more →](https://dash.dropbox.com/?utm=blogs)


### Step 1: Curate the right datasets


To kick off our evaluations, we started with publicly available datasets to establish a baseline for retrieval and question answering performance. For question answering, we drew on[Google’s Natural Questions](https://ai.google.com/research/NaturalQuestions) ,[Microsoft Machine Reading Comprehension](https://microsoft.github.io/msmarco/) (MS MARCO), and[MuSiQue](https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00475/110996/MuSiQue-Multihop-Questions-via-Single-hop-Question) . Each brought something different to the table: Natural Questions tested retrieval from very large documents, MS MARCO emphasized handling multiple document hits for a single query, and MuSiQue challenged models with multi-hop question answering. Choosing the right mix of datasets gave us early, useful signals on how our system and parameter choices would hold up.


But public datasets alone aren’t enough. To capture the long tail of real-world phrasing, we turned to internal datasets by collecting production logs from Dropbox employees dogfooding Dash. We built two kinds of evaluation sets from this data. **Representative query datasets** mirrored actual user behavior by anonymizing and ranking top internal queries, with annotations provided through proxy labels or internal annotators. And **representative content datasets** focused on the types of material our users rely on most: widely shared files, documentation, and connected data sources. From this content, we used LLMs to generate synthetic questions and answers spanning diverse cases: tables, images, tutorials, and factual lookups.


Together, these public and internal datasets gave us a stack of carefully curated queries and answers that mirrored real-world chaos. Great! But datasets alone are just an inert mass until you wrap them in scoring logic. The next step was to turn these examples into a live alarm system, where each run clearly signals success or failure, with success defined through metrics, budget limits, and automated checks before the first experiment even begins.


### Step 2: Define actionable metrics and rubrics


When evaluating outputs from conversational AI systems, it’s tempting to reach for the usual suspects like BLEU, ROUGE, METEOR, BERTScore, and embedding cosine similarity. These offline metrics are well understood, quick to compute, and have been the backbone of benchmarking for natural language processing for years. But when applied to real-world tasks—for example, retrieving a source-cited answer, summarizing an internal wiki, or parsing tabular data—they quickly run out of steam.


Here’s what traditional metrics can (and can’t) tell you:


**Metric** **Does well on** **Fails on**


BLEU Exact word overlap Paraphrasing, fluency, factuality


ROUGE Recall-heavy matching Source attribution, hallucination


BERTScore Semantic similarity Granularity of errors, citation gaps


Embedding sim Vector-space proximity Faithfulness, formatting, tone


We used these metrics early on for quick checks, and they were useful for catching egregious cases where the model drifted wildly. But they couldn’t enforce deployment-ready correctness. We’d see high ROUGE scores even when an answer skipped citing its source, strong BERTScore results alongside hallucinated file names, and fluent Markdown outputs that still buried factual errors in the middle of a paragraph. These failures aren’t rare; they’re the norm when deploying AI in production. So we asked a better question: What if we used LLMs themselves to grade the outputs?


**Enter the LLM as a judge** Using one LLM to evaluate another may sound recursive, but it unlocks real flexibility. A judge model can check for factual correctness against ground truth or context, assess whether every claim is properly cited, enforce formatting and tone requirements, and scale across dimensions that traditional metrics simply ignore. The key insight is that LLMs are often better at scoring natural language when you frame the evaluation problem clearly.


Just as important, we learned that rubrics and judge models themselves need evaluation and iteration. Prompts, instructions, and even the choice of judge model can change outcomes. In some cases, like evaluating specific languages or technical domains, we rely on specialized models to keep scoring fair and accurate. In other words, evaluating the evaluators became part of our own quality loop.


**How we structure LLM-based evaluation** We approached our LLM judges as if they were software modules: designed, calibrated, tested, and versioned. At the core sits a reusable template. Each evaluation run takes in the query, the model’s answer, the source context (when available), and occasionally a hidden reference answer. The judge prompt then guides the process through a structured set of questions, such as:


- Does the answer directly address the query?
- Are all factual claims supported by the provided context?
- Is the answer clear, well-formatted, and consistent in voice?


The judge responds with both a justification and a score that’s either scalar or categorical, depending on the metric. For example, a rubric output might look like this:


```text
{
"factual_accuracy": 4,
"citation_correctness": 1,
"clarity": 5,
"formatting": 4,
"explanation": "The answer was mostly accurate but referenced
a source not present in context."
}
```


Every few weeks, we ran spot-checks on sampled outputs and labeled them manually. These calibration sets gave us a way to tune the judge prompts, benchmark agreement rates between humans and models, and track drift over time. Whenever a judge’s behavior diverged from the gold standard, we updated either the prompt or the underlying model.


While LLM judges automated most of the coverage, human spot-audits remained essential. For each release, human engineers manually reviewed 5–10% of the regression suite. Any discrepancies were logged and traced back to either prompt bugs or model hallucinations, and recurring issues triggered prompt rewrites or more fine-grained scoring.


To make this system enforceable, we defined three types of metrics, each with a clear role in the development pipeline:


**Metric type** **Examples** **Enforcement logic**


Boolean gates “Citations present?”, “Source present?” Hard fail changes can’t move forward


Scalar budgets Source F1 ≥ 0.85, p95 latency ≤ 5s Block deploying any changes that affect the test


Rubric scores Tone, formatting, narrative quality Logged in dashboards; monitored over time


Every new model version, retriever setting, or prompt change was checked against these dimensions. If performance slipped below the thresholds, the change didn’t move forward. And because metrics only matter when they’re built into the workflow, we wired them into every stage of development. Fast regression tests ran automatically on every pull request, the full suite of curated datasets ran in staging, and live traffic was continuously sampled and scored in production. Dashboards consolidated the results, and that made it easy to see key metrics, pass/fail rates, and shifts over time.


With this setup, the same evaluation logic gated every prompt tweak and retriever update. The result is consistency, traceability, and reliable quality control.


### Step 3: Set up an evaluation platform


Once we had datasets and metrics in place and had gone through a few cycles of building, testing, and shipping, it became clear we needed more structure. Managing scattered artifacts and experiments wasn’t sustainable. That’s when we adopted Braintrust, an evaluation platform we’ll dive into shortly. It brought structure to our workflows by helping us manage datasets, scorers, experiments, automation, tracing, and monitoring.


At its core, the platform gave us four key capabilities. First, it gave us a **central store** , meaning a unified, versioned repository for datasets and experiment outputs. Second, it provided us with an **experiment API** where each run was defined by its dataset, endpoint, parameters, and scorers, producing an immutable run ID. (We built lightweight wrappers to make managing these runs simple.) Third, it offered **dashboards with side-by-side comparisons** that highlighted regressions instantly and quantified trade-offs across latency, quality, and cost. And finally, it gave us **trace-level debugging** , where one click revealed retrieval hits, prompt payloads, generated answers, and judge critiques.


Spreadsheets were fine for quick demos, but they broke down fast once real experimentation began. Results were scattered across files, hard to reproduce, and nearly impossible to compare side by side. If two people ran the same test with slightly different prompts or model versions, there was no reliable way to track what changed or why. We needed something more structured, and we needed a shared place where every run was versioned, every result could be reproduced, and regressions surfaced automatically. That’s what an evaluation platform gave us: the ability to reproduce, compare, and debug together without slowing down.


### Step 4: Automate evaluation in the dev‑to‑prod pipeline


We treated prompts, context selection settings, and model choices just like any other piece of application code, meaning they had to pass the same automated checks. Every pull request kicked off about 150 canonical queries, which were judged automatically and returned results in under 10 minutes. Once a pull request was merged, the system reran the full suite along with quick smoke checks for latency and cost. If anything crossed a red line, the merge was blocked.


**Dev event** **Trigger** **What runs** **SLA**


Pull request opened GitHub Action ~150 canonical queries, judged by scorers Results return in under ten minutes


Pull request merged GitHub Action Canonical suite plus smoke checks for latency and cost Merge blocked on any red‑line miss


These canonical queries were small in number but carefully chosen to cover critical scenarios: multiple document connectors, “no-answer” cases, and non-English queries. Each test recorded the exact retriever version, prompt hash, and model choice to guarantee reproducibility. If scores dropped below a threshold—for example, if too many answers were missing citations—the build stopped. Thanks to this setup, regressions that once slipped into staging were caught at the pull-request level.


**On-demand synthetic sweeps** Large refactors or engine updates could hide subtle regressions, so we ran end-to-end evaluation sweeps to catch them early. These sweeps began with a golden dataset and could be dispatched as a Kubeflow DAG, running hundreds of requests in parallel. (A Kubeflow DAG is a workflow built in Kubeflow Pipelines, an open-source ML platform, where the steps are organized as a directed acyclic graph.) Each run was logged under a unique run_id, making it easy to compare results against the last accepted baseline.


We focused on RAG-specific metrics such as binary answer correctness, completeness, source F1—an F1 score applied to retrieved sources, measuring how well the system balances precision (retrieving only relevant documents) and recall (retrieving all relevant ones)—and source recall. Any drift beyond predefined thresholds was flagged automatically. From there, LLMOps tools let us slice traces by retrieval quality, prompt version, or model settings, helping pinpoint the exact stage that shifted so we could fix it before the change ever reached staging.


**Live-traffic scoring** Offline evaluation is critical, but real user queries are the ultimate test. To catch silent degradations as soon as they happened, we continuously sampled live production traffic and scored it with the same metrics and logic as our offline suites. (All of our work at Dropbox is guided by our[AI principles](https://www.dropbox.com/ai-principles) .) Each response, along with its context and retrieval trace, was logged and routed through automated judgment, measuring accuracy, completeness, citation fidelity, and latency in near real time.


Dashboards visible to both engineering and product teams tracked rolling quality and performance medians; for example, over one-hour, six-hour, and 24-hour intervals. If metrics drifted beyond a set threshold, such as for a sudden drop in source F1 or a spike in latency, alerts were triggered immediately so the team could respond before end users were affected. Because scoring ran asynchronously in parallel with user requests, production traffic saw no added latency. This real-time loop let us catch subtle issues quickly, close the gap between code and user experiences, and maintain reliability as the system evolved.


**Layered gates** To control risk as changes moved through the pipeline, we used layered gates that gradually tightened requirements and brought the evaluation environment closer to real-world usage. The **merge gate** ran curated regression tests on every change, and only those meeting baseline quality and performance passed. The **stage gate** expanded coverage to larger, more diverse datasets and applied stricter thresholds, checking for rare edge cases. Finally, the **production gate** continuously sampled real traffic and scored it to catch issues that only emerged at scale. If metrics dipped below thresholds, automated alerts were fired and rollbacks could be triggered immediately.


By progressively scaling dataset size and realism at each gate, we blocked regressions early while ensuring that staging and production evaluations stayed closely aligned with real-world behavior.


### Step 5: Close the loop with continuous improvement


Evaluation isn’t a phase; it’s a feedback loop. A system that learns from its own mistakes evolves faster than any roadmap allows. Gates and live-traffic scoring provide safeguards, but to build resilient AI systems, evaluation also has to drive continuous learning. Every low-scoring output, flaky regression, or drifted metric isn’t just a red flag. Rather, it’s a chance to improve the system end to end. This is where the loop closes and the next cycle begins.


Every poorly scored query carries a lesson. By mining low-rated traces from live traffic, we uncovered failure patterns that synthetic datasets often missed: retrieval gaps on rare file formats, prompts cut off by context windows, inconsistent tone in multilingual inputs, or hallucinations triggered by underspecified queries. These hard negatives flowed directly into the next dataset iteration. Some became labeled examples in the regression suite, while others spawned new variants in synthetic sweeps. Over time, this built a virtuous cycle where the system was stress-tested on exactly the edge cases where it once failed.


Not every change was ready for production gates, especially riskier experiments like a new chunking policy, a reranking model, or a tool-calling approach. To explore these safely, we built a structured A/B playground where teams could run controlled experiments against consistent baselines. Inputs included golden datasets, user cohorts, or synthetic clusters. Variants covered different retrieval methods, prompt styles, or model configurations. Outputs spanned trace comparisons, judge scores, and latency and cost budgets. This safe space let tweaks prove their value, or fail fast, without consuming production bandwidth.


LLM pipelines are multi-stage systems, and when an answer failed, guessing was costly. To speed up debugging, we invested in playbooks that guided engineers straight to the likely cause. Was the document never retrieved? Check the retrieval logs. Was context included but ignored? Review the prompt structure and truncation risk. Did the answer fail because the judge mis-scored it? Re-run against the calibration set and human labels. These playbooks became part of triage, ensuring regressions were traced systematically rather than debated.


Finally, the cultural piece: Evaluation wasn’t owned by a single team. Instead, it was embedded into everyday engineering practice. Every feature pull request linked to evaluation runs. Every on-call rotation had dashboards and alert thresholds. Every piece of negative feedback was triaged and reviewed. And every engineer owned the impact of their changes on quality, not just correctness. Speed mattered when shipping new products, but the cost of mistakes could be high. Predictability came from guardrails, and those guardrails were evaluation.


### What we learned


When we first set out, our prototypes were stitched together with whatever evaluation data we had available. That was fine for quick demos, but once real users started asking real questions, the cracks showed.


Tiny prompt tweaks led to surprise regressions. Product managers and engineers debated whether an answer was good enough, each using their own mental scoreboard. And the worst part? Problems slipped past staging and into production because nothing was catching them.


The solution wasn’t more heroics; it was structure. We created a central repository for datasets and ran every change through the same Braintrust-powered evaluation flows. Automated checks became our first line of defense, catching missing citations or broken formatting before code could merge. Shared dashboards replaced hallway debates with real numbers, visible to both engineering and product teams.


One of the biggest surprises was how many regressions came not from swapping models but from editing prompts. A single word change in an instruction could tank citation accuracy or formatting quality. Formal gates, not human eyeballs, became the only reliable safety net. We also learned that judge models and rubrics aren’t set-and-forget assets. Their own prompts need versioning, testing, and recalibration. In some cases, like evaluating responses in other languages or niche technical domains, we found that a specialized judge was the only way to keep scoring fair and accurate.


The takeaway is that evaluation isn’t a sidecar to development. Treat your evaluation stack with the same rigor you give production code, and you’ll ship faster, safer, and with far fewer "how’d this get through?” moments.


Our current stack catches regressions and keeps quality steady, but the next frontier is making evaluation proactive rather than purely protective. That means moving beyond accuracy to measure things like user delight, task success, and confidence in answers. It means building self-healing pipelines that can suggest fixes when metrics drop, shortening the debug loop. And it means extending coverage beyond text to images, audio, and low-resource languages, so evaluation reflects the way people actually work.


The goal is simple: Keep raising the bar so evaluation doesn’t just guard the product but pushes it forward. By treating evaluation as a first-class discipline—anchored in rigorous datasets, actionable metrics, and automated gates—we can turn probabilistic LLMs into dependable products.


*Acknowledgments: Dhruvil Gala, Venkata Prathap Reddy Sudha, April Liu, and Dongjie Chen*


~ ~ ~


*If building innovative products, experiences, and infrastructure excites you, come build the future with us! Visit*[jobs.dropbox.com](https://jobs.dropbox.com/) *to see our open roles and learn about creating a more enlightened way of working.*
