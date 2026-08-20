---
schema_version: "1.0.0"
document_id: "7b4c5d273e91c22231dd0fd075e5e6ede43c4970b35413430aea1737b5c7a970"
company_key: "yc-raycaster"
company: "Raycaster"
source_id: "yc-raycaster-news-import-a1693d0ab1b3"
canonical_url: "https://www.raycaster.ai/blog/benchmark-answers-completed-work"
published_at: "2026-03-24T00:00:00+00:00"
first_seen_at: "2026-07-24T10:29:25.527103+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:eec7744b16a526684c6e86ad8427d9127099a1a92be095d748b12e7d5ada7cd4"
---

# The Illusion of the Correct Answer: Why High-Stakes Work Demands Document Engineering

Every week, a new model claims the throne on a leaderboard, and the industry rushes to ask the exact same question: **can the model answer correctly?**


It is a question that made perfect sense for the era of chat. But as we transition to autonomous agents, treating an LLM like a magic 8-ball is a mistake. When you place an agent in a labyrinth of constraints—regulatory packages, clinical trial submissions, intricate legal exhibits—asking if a model is "smart" is the wrong question. Organizations do not dispense capital for plausible text strings; they pay for completed documents that survive meticulous deliberation, clear compliance audits, and rigorous human scrutiny.


> **The era of treating an LLM like a magic 8-ball is over. Passing a benchmark proves baseline competence, but high-stakes work requires verifiable trust. The operative question is no longer whether a model can answer, but how much work a system can complete correctly under real review constraints.**


The rest of this post dismantles the illusion that Pass@1 is the finish line. We unpack what a rigorous benchmark like APEX actually demands of agents; why the evolution of coding agents serves as our most prescient blueprint; how a document-native runtime maximizes baseline competence (allowing smaller models to match frontier scores); what a successful, verifiable trajectory looks like under the hood; and why a high benchmark score is merely the starting point for deployment.


## What benchmarks measure now


The most rigorous public evaluations of "agentic" professional work have evolved far beyond isolated prompts. They now manifest as structured worlds—labyrinths of files, applications, and tools governed by automated graders. It is less a trivia night and more a synthetic stint on the job.


The recent[APEX-Agents benchmark](https://arxiv.org/abs/2601.14242) (Vidgen et al.) is the most visible example in this class today. Consider the scale it demands of an agent:


- **~166 files per environment** on average
- **9 apps and 63 tools** to navigate and execute the work
- **~1.82 hours** of estimated human labor required to complete a single task


This is hard in a way that mirrors real work: any one task usually needs a small subset of files, which means most of the environment is plausible-but-irrelevant context. The agent has to locate the right source, in the right version, then use it correctly.


The benchmark was also stress-tested during creation with frontier agents, so weak prompt-first strategies were exposed early.


Public scores are still low: top systems remain around 30% on Pass@1 and around 40% on Pass@8.


So the benchmark is already asking for navigation, retrieval, tool use, and an auditable final answer. That is a big upgrade from chat-only tests. It still is not the full burden of production: a trajectory can pass while producing a document no one should sign if review, traceability, and change control are treated as afterthoughts.


## What real work still requires


> The real bottleneck is the system around the model. We are building the equivalent of Git, CI/CD, and code review for messy, real-world documents.


While model intelligence remains a prerequisite, once a competence floor is cleared, the chasm between a flashy demo and a deliverable that legal or compliance will actually sign off on is bridged by system design: how evidence is meticulously retrieved, how edits are bounded, and how seamlessly a human can verify or roll back changes.


To be precise: domains that are bound by raw reasoning or pure mathematical calculation will naturally benefit most from ongoing improvements at the model layer. But in document-heavy domains, outcomes are dominated by the messiness of the real world. Here, large swings come from system choices like:


- retrieval routing policy
- multimodal parsing quality
- deterministic workflow design
- version-controlled edit management
- reviewer-facing change control


Put plainly: changing architecture can move benchmark outcomes more than swapping models within the same class—and it is the same lever that decides whether outputs are fit for a real review queue.


That cuts two ways. Economically, runtime design that lets smaller models reach frontier-adjacent scores shifts the price-performance curve instead of defaulting to the largest checkpoint. For governance, raw capability is never sufficient: in regulated or high-stakes work, deliverables have to stay **verifiable, reviewable, and reversible** under explicit standards, or the work does not ship.


## What coding evals already proved


Software engineering pioneered this trajectory. As coding evaluation matured from toy completions to complex, issue-level workflows, three lessons became impossible to ignore:


1. benchmark construction determines signal quality
2. harness design can materially change outcomes for the same base model
3. verification loops (test/review/retry) are part of capability, not implementation detail


In coding, the stack evolved from raw generation to harnessed workflows: retrieval, test execution, code review, and merge gates. Treat those as inspiration, not metaphor only—document systems need the same progression: retrieval plus evidence checks, standards and checklists, reviewer-visible diffs, and approval gates before anything becomes the record.


Document agents inherit all three lessons and add harder surface area: scans, dense tables, diagrams, handwriting, and long revision histories across files that were never designed for code-style tracking.


Anthropic’s recent research on[harness design for long-running applications](http://anthropic.com/engineering/harness-design-long-running-apps) drives this home: they found that even frontier models fail at long-horizon tasks, drifting out of scope or hallucinatively approving their own bad work, unless constrained by a strict architecture. Their solution was to separate the agent doing the work from an independent "evaluator" agent forced to grade against explicit criteria. The model's raw capability was unlocked only by the orchestration built around it.


A similar parallel exists for context and retrieval. Cursor's write-up on semantic search in the agent harness: on their offline **Cursor Context Bench** retrieval eval, the same models with semantic search in the toolset answer questions about codebases more accurately—about **12.5%** on average, with a roughly **6.5%–23.5%** spread by model. Online A/B tests on real agent traffic show the same idea in product metrics (e.g. code retention and fewer dissatisfied follow-ups when search is on; smaller effect sizes because not every task needs retrieval). They emphasize that **grep plus semantic search together** beats either alone—exactly the split we rely on in document workspaces ([Improving agent with semantic search](https://cursor.com/blog/semsearch) ).


The transfer to document work is straightforward but harder: retrieval must handle prose, tables, scans, figures, and version drift across many file formats. That means benchmark uplifts are informative, but likely still a lower bound on the architecture effects you see in real enterprise work.


## Where a document runtime moves scores


Against that backdrop—realistic worlds, but still grader-centric—we asked a narrower empirical question: if you keep the tasks and models fixed and swap in a document-native runtime (retrieval routing, workspace navigation, and change-aware tooling), how much moves?


Across consulting, investment banking, and law with four models, our runtime improved baseline on **20/24** model-domain-metric cells (Pass@1 + Mean).


For` GPT 5.4 mini` and` GPT 5.4 nano` , improvements were **12/12** cells, with average deltas of **+4.60 Pass@1** and **+7.48 Mean** .


### Where lift appears—and where it doesn't


Domain Mean Δ (Doc Agent − APEX baseline) Pass@1 Δ Improved cells


Management Consulting +6.28 +4.55 8/8


Investment Banking +0.08 −0.68 4/8


Law +8.38 +4.80 8/8


The shape is instructive.


Consulting and law improve most, where retrieval quality, cross-document grounding, and disciplined edit workflows dominate. Investment banking is flatter. This is largely because the IB domain is overwhelmingly focused on financial modeling and analysis—tasks bounded by post-training capabilities, numerical precision, and strict calculation chains inside Excel.


That is entirely consistent with the story so far: better architecture lifts retrieval and grounding reliability where those dominate the task. However, for workflows that are purely analytical or mathematical, performance will scale naturally on the model layer as foundation models improve their reasoning capabilities. But for document-heavy work—the messy reality of synthesizing contracts, reconciling scattered PDF evidence, and auditing compliance rules—model improvements alone will never be enough.


There is a collateral benefit hidden in these numbers. We originally built our document engine for the grueling realities of biopharma and energy. We did not write custom optimizations for APEX's legal or management consulting tasks. Yet, the system performed materially better in those new domains simply because the fundamental physics of document work—finding the right page, anchoring on the exact clause, managing version drift—are universal. When you solve for the document, the domain follows.


The bar chart below summarizes lift across every model we ran in one glance. The image at the top of the article keeps the nano Pass@1 and list-pricing comparison against the APEX GPT-5.2 (High) reference (at approximately 11x lower cost) so we do not repeat that same figure in the body.


Uplift is broad: Gemini 3 Flash + Doc Agent posts **+11.0 pp Mean / +6.0 pp Pass@1** , GPT 5.4 nano + Doc Agent posts **+10.4 pp Mean / +5.9 pp Pass@1** , and GPT 5.4 mini + Doc Agent posts **+5.9 pp Mean / +2.85 pp Pass@1** .


For the same runs, nano on Pass@1 (see chart above the title):` GPT 5.4 nano + Doc Agent` reaches **20.95** , near APEX` GPT 5.2 (High)` at **20.8** , with **+5.9 pp** over same-model nano baseline—at roughly **10.9×** lower list token pricing than GPT-5.2 at a 1:1 input/output mix.


That nano result is the clearest proof of the claim. When a sub-tier model (` GPT 5.4 nano` ) in a document-native harness matches the bare baseline of a frontier model (` GPT 5.2 High` ), it breaks the assumption that complex knowledge work is just a function of scale.


Model capabilities will inevitably continue to rise, but raw intelligence alone cannot dexterously untangle the messy realities of document-heavy work: broken file references, scanned PDFs, handwritten notes in clinical trial batch records, or legal exhibits with missing pages. A massive model will hallucinate if it reads the wrong version of a contract; a tiny model will succeed if the architecture reliably hands it the exact governing clause. That is why trust is always built *around* the models, not just by them.


## Inside one completed trajectory


Yet, aggregate averages often obscure the underlying mechanics. Take one representative APEX-style task in management consulting: the prompt asks for a market cost-efficiency analysis using a defined methodology (Cost Structure Score) for a fictional brand (PureLife) across several international markets.


Completing it means threading a dense file tree of statements, market-sizing spreadsheets, and competitor benchmarks. The agent leans on **semantic vector search** and **grep** to locate the methodology and exact columns, then writes Python to load the master sheet, clean fields, and compute weighted scores that match the grader.


Semantic retrieval is the same primitive across domains: the agent routes a natural-language need to the right folder, file, and page before it can safely reason or compute.


### Sample Task: Cost Structure Score Analysis


Domain: Management Consulting • Exported on 3/25/2026


Analyse market cost efficiency using the Cost Structure Score methodology for PureLife's Vitamins strategy:


- Calculate the Total Cost Structure Score for each market under the weighted methodology using the following components: Import Cost Score (35%), Distribution Cost Score (35%), and Total Cost Load Score (30%).
- Show the country with the highest cumulative cost structure score and the country with the lowest cumulative cost structure score, with each entry showing the market name and corresponding score side by side (e.g., China – 3.81).


Format percentages to 0.01%. Round all numbers and intermediate calculations to two decimals. Output the answer in here.


I’ll first locate the source dataset and any existing “Cost Structure Score” tables for PureLife’s Vitamins strategy in your workspace, then I’ll compute the weighted Total Cost Structure Score per market and identify the highest and lowest markets.


Next I’ll extract the underlying “Import Cost Score”, “Distribution Cost Score”, and “Total Cost Load Score” values by market from the relevant PureLife analysis workbook(s), then I’ll apply the 35%/35%/30% weighting and round as requested.


Matches found for "Import Duties":


### Buyer_Profile_Analysis_Data/1_CRM_PnL_Aligned_Data_Extract_Master.xlsx | Market | Market Code | COGS % | Marketing % | Distribution % | Regulatory & Compliance % | Import Duties % | SG&A % | Total OpEx % | Operating Margin % |


I found README files alongside the dataset that likely explain the Cost Structure scoring inputs (COGS/Distribution/Import Duties etc.). Next I’ll read those READMEs to locate the exact “Cost Structure Score” methodology and how to transform the raw cost components into the three requested score components.


// Applying min-max scaling to cost data and computing weighted sum


highest \\{Market: Germany, Total: 4.72} lowest {Market: Brazil, Total: 1.28}


#### Analysis Results


Highest Total Cost Structure Score Lowest Total Cost Structure Score


Germany – 4.72 Brazil – 1.28


A successful trajectory like the one above is a tightly routed pipeline:


1. **Semantic retrieval (Vector Search)** to surface README files and presentations that explain the methodology and point to the correct master spreadsheet.
2. **Exact search (Grep)** to scan across dozens of Excel files and text documents to verify column names and variables like` Import_Duty_%` and` Distribution_Cost_%` .
3. **Environment Navigation & Data Processing** writing Python code to load the master P&L, clean the data, apply inverse min-max scaling, and calculate the weighted scores.
4. **Deterministic computation** recovering from missing files and syntax errors to arrive at the exact final values.
5. **Reviewer-visible output** displaying the exact formatted answer requested by the user.


Miss any step and the task fails, even if the prose sounds plausible. This is the hidden workload inside a single Pass@1 success bit: not one model call, but a routed pipeline. With the right architecture and context engineering, even small models can run it end to end.


Crucially, because the harness forces agents to show their work via explicit diffs and evidence links rather than just outputting a final string, it inherently doubles as a high-signal environment for evaluation and Reinforcement Learning (RL) on real-world tasks.


## Why higher scores still aren’t enough


A grader's verdict is binary; the reality of production is anything but. A benchmark might mark a run as successful while inadvertently leaving you with silent file deletions, unreviewed redlines, or claims that feign authority but lack a tether to a versioned source.


This becomes exponentially more critical as we anticipate the immediate future: the rise of cloud agents. Soon, organizations will not just have one model answering a prompt sequentially; they will have dozens of autonomous agents working in parallel across the same enterprise knowledge base. When multiple agents and multiple humans, each with their own organizational permissions and context, are collaborating on a live filing, raw intelligence is not enough to resolve merge conflicts or share context safely. The HCI (Human-Computer Interaction) implications of this are staggering. If we do not have rigorous control surfaces—redlines, rollback, deterministic review—parallel agent work will devolve into chaos.


Our thesis is that reliable document agents require document-native infrastructure analogous to what Git enabled for software. Deployment quality is decided by these control surfaces, not by the score alone:


- **Verifiable (The Audit Trail)** : a clear, step-by-step record showing exactly how the agent arrived at a change (which files it opened, what it searched for, and how it calculated the numbers). A legal claim or regulatory submission must cite the exact clause, table cell, and document version, not just a plausible summary.
- **Reviewable (Tracked Changes)** : a proposed set of edits presented clearly, much like redlines or suggested edits in Word. Reviewers need to see exactly what was added or removed before approving.
- **Reversible** : if the system edits the wrong version or applies a bad change, teams need one-step rollback with full history preserved.
- **Standards-driven (Automated Checkers)** : built-in rules that enforce domain standards (like a playbook, policy, or formatting guideline) consistently across every run.


Verification is not a vibe check: the reviewer (human or tool) can jump to the exact clause, table cell, or inspection finding the output claims—same instinct as jumping to a failing test line in code.


This is not hypothetical. In their failure analysis, the APEX-Agents authors noted that "deletions are never requested in the task prompts"—yet "GPT-5.2 \[deleted\] the most files (n=21)" ([APEX-Agents paper](https://arxiv.org/pdf/2601.14242) ). This is exactly why review and rollback are first-class requirements, not UX polish. We must design for *human-as-reviewer* by default, treating it as the core operating model rather than a post-hoc safety layer.


That is the same handoff coding already institutionalized: tests and CI for correctness, pull requests for review, merges for consent. Document workflows need the analogue—explicit evidence, reviewer-facing diffs, and publish gates—whether or not the benchmark run happened to pass. Evaluation protocols must grade not only the final outcome, but the entire process: the proposed edits plus the audit trail of how they were made.


Diffs and publish gates on decks, filings, and contracts mirror what coding agents already treat as normal: bounded edits, explicit review, and rollback—so knowledge work can inherit the same safety properties.


## Checks to pair with Pass@1


None of these requirements negate the need for headline task accuracy; rather, they constrain what that accuracy is allowed to mean in a professional context. Pass@1 (or Pass@k) should sit beside a small set of companion checks, much like unit tests sit beside integration tests in engineering. For document-heavy workflows, the minimum useful surface is:


1. **Task Completion@1** (the benchmark bit you already report)
2. **Consistency@k** (rerun stability under fixed seeds and inputs)
3. **Edit Integrity** (unrelated sections and files unchanged)
4. **Grounding Coverage** (material claims traceable to sources and versions)
5. **Approval@1** (first-pass human accept rate on real queues)
6. **Severity-weighted Error Cost** (not all mistakes weigh the same)
7. **Cost and latency per successful run** (economics of the full pipeline)


A single score optimizes for demos and leaderboard screenshots. The bundle optimizes for work you can defend in front of counsel or a quality unit—and it is the bundle coding orgs already expect before they call a release safe.


The thread running through this post echoes the premise we began with: benchmarks grade whether a run cleared the bar, but the real world grades whether the artifact is safe to ship. Software engineering learned decades ago to pair raw code generation with tests, review, and merge discipline. Document-heavy work is due the exact same reckoning—just with messier files, stricter traceability, and higher stakes.


The paradigm shift can be summarized in one line:


- from **"can the model answer?"**
- to **"can the system complete work humans trust?"**


The frontier of AI is no longer about proving a model can think. It is about proving a system can work. When you solve the architecture, even smaller, cheaper models perform like frontier models.


## References and prior work


- [APEX-Agents paper (Vidgen et al., 2026)](https://arxiv.org/abs/2601.14242)
- [APEX-Agents leaderboard](https://www.mercor.com/apex/apex-agents-leaderboard/)
- [OfficeQA Pro: An Enterprise Benchmark for End-to-End Grounded Reasoning](https://arxiv.org/abs/2603.08655)
- [Introducing OfficeQA (Databricks)](https://www.databricks.com/blog/introducing-officeqa-benchmark-end-to-end-grounded-reasoning)
- [Cursor: Improving agent with semantic search](https://cursor.com/blog/semsearch)
- [Anthropic: Harness design for long-running application development](http://anthropic.com/engineering/harness-design-long-running-apps)
- [Git book: About Version Control](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)


## What we are building next


Raycaster is an applied research team building this stack in production and turning it into publishable, reusable research artifacts. We are not just another company focused on evaluation; we are actively designing the interfaces, runtimes, and version-controlled environments where humans and agents will actually collaborate.


This is an invitation to build that future alongside us:


- **Vertical AI companies:** Reach out to deploy our harness to enhance your own domain-specific agents. As agents do more complex tasks, the platform running them effectively becomes a Reinforcement Learning (RL) environment—we capture the full trajectory of edits, review, and human approval or rejection. Collaborate with us to build new, rigorous benchmarks that reflect this end-to-end reality. Right now, code is the easiest domain to evaluate. Let's build the equivalent for real-world documents.
- **Academic Researchers:** We are actively looking for paper coauthors and long-term collaborators to partner with us on the nascent science of document engineering, NLP/IR, HCI/CSCW for agentic workspaces, and knowledge-work evaluation environments where the trajectory of edits is the unit of measurement.
- **Direct Customers:** Work with us to bridge the agonizing gap between frontier AI capabilities and actual production adoption for your high-stakes, document-heavy workflows.
- **Top Talent:** Join our team to research and build the bleeding edge of the future of work.


Emailteam@raycaster.ai to start the conversation.


## See document work the way we evaluate it


Book time to walk through retrieval, review surfaces, and change control for your team's artifacts.


[Schedule a demo](https://app.cal.com/team/raycaster/meeting)[Explore solutions](https://www.raycaster.ai/biopharma)
