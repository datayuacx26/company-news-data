---
schema_version: "1.0.0"
document_id: "9d51f0577591145d7ea32ca5722db16f03d2c578a5b2e8c7edd26dedade43b61"
company_key: "yc-layerup"
company: "Layerup"
source_id: "yc-layerup-news-import-60fa3a01cf26"
canonical_url: "https://uselayerup.com/blog/how-ai-agents-improve-as-enterprises-use-them"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-24T02:01:56.334076+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:f1a20daa1f3c4178f6e9fe527f0fdf4266dbe314e2b6380becf01300b488ad05"
---

# Agents that compound: how Layerup's AI improves the more your enterprise uses it

[Back to blog](https://www.uselayerup.com/blog) Claims · CIO / CTO


# Agents that compound: how Layerup's AI improves the more your enterprise uses it


Static models plateau on day one. Layerup's agents are built so that every file worked, every correction made, and every outcome observed becomes signal that raises accuracy and lowers cost over time.


Layerup


•


June 4, 2026


•


11 min read


Touchless rate over 6 months


~30% to ~70%


Most enterprise AI is bought as a fixed asset. You evaluate a model, you measure its accuracy on a pilot, and whatever number you see in the proof of concept is roughly the number you live with for the life of the contract. The model does not know your claims, your guidelines, your adjusters, or your edge cases any better in month twelve than it did in week one. That is a reasonable description of how a static large language model behaves, and it is the wrong way to think about an agent system.


Layerup's agents are designed around a different assumption: the first version you deploy is the worst version you will ever run. Every file an agent works, every correction an adjuster makes, and every downstream outcome the business observes is captured as signal. That signal flows back into the system and raises accuracy, expands automatable scope, and lowers cost per file. The agent that handled your claims in month six is meaningfully better than the one you turned on in month one — and it got there on your data, not on a vendor's release schedule.


This piece is about the engineering that makes that true, and what it looks like when you measure it on core claims metrics.


## Why a static model plateaus on day one


A base language model is frozen at training time. It carries broad world knowledge but no knowledge of your reserving philosophy, your state-specific endorsements, the three carriers whose loss runs always arrive as scanned faxes, or the fact that your SIU team treats a particular injury pattern as a fraud indicator. Prompt engineering papers over some of this, but a prompt is a fixed instruction set. It cannot observe that it was wrong last Tuesday and adjust.


The result is a system whose accuracy is set on the day of deployment and erodes from there as your business, your forms, and your fraud patterns drift away from whatever the prompt assumed. To improve a static system you have to manually rewrite prompts and re-run evaluations — a human-paced loop that almost never keeps up with production reality.


An agent that compounds inverts this. Improvement is a property of the system, captured automatically from production, and the human-paced loop is reserved for reviewing and approving changes rather than discovering them.


## The feedback substrate: what every file teaches the system


The foundation is instrumentation. Layerup agents are built so that a unit of work is never just an output — it is a fully traced record of how the output was produced. For a single claims file the system retains the inputs it read, the documents it extracted from and the confidence per field, the tools and core systems it called, the intermediate reasoning steps, the decision it proposed, the human action taken on that decision, and the eventual outcome once the file resolves.


That record is the raw material for improvement. Three kinds of signal come out of it.


- Explicit corrections. When an adjuster overrides a coverage determination, edits an extracted field, or rejects a recommended reserve, the delta between proposed and accepted is captured with full context. This is the highest-value signal in the system because it is a labeled error on your exact data.
- Implicit signal. Approve-without-edit, time-to-decision, how often a file is escalated, and whether a downstream step had to be reworked all indicate quality without anyone explicitly grading the agent.
- Outcome signal. Reserve accuracy against ultimate paid, reopen rate, subrogation actually recovered, litigation rate, and leakage findings close the loop weeks or months later and tell the system whether a decision that looked right at the time actually was.


## How that signal turns into a better agent


Captured signal does nothing on its own. The engineering that matters is the loop that converts it into behavior changes, and the controls that keep those changes safe. Layerup's loop operates at several layers, fastest and lowest-risk first.


1. Retrieval and memory. Corrected examples and resolved files become retrievable context. The next time the agent sees a similar submission, it retrieves the prior correction and the guideline it maps to. This layer improves within hours and requires no model change — the agent simply has better, more relevant context grounded in your own resolved work.
2. Guideline and policy compilation. Recurring corrections of the same type are clustered and surfaced as proposed rule changes — a new appetite check, a tightened extraction validation, an added fraud indicator. A human reviewer approves or rejects. Approved changes become deterministic guardrails, so the same mistake is structurally prevented rather than statistically discouraged.
3. Evaluation-gated tuning. Accumulated labeled examples build a growing, domain-specific evaluation set. Prompt changes, tool changes, and model updates are scored against this golden set before any of them reach production. Nothing ships unless it beats the incumbent on your metrics.
4. Model adaptation. Where volume justifies it, the labeled corpus supports fine-tuning or preference optimization of the underlying models on your domain — terminology, document layouts, and decision patterns specific to your book.


The first two layers deliver most of the early gains and they move fast. The deeper layers compound over months as data volume grows. Crucially, every layer is gated by evaluation: the system proposes improvements continuously, but a change only takes effect if it measurably outperforms what it replaces on a held-out set drawn from your files.


## Learning that is safe by construction


A system that changes its own behavior is only an asset if the changes are controlled. The instinct of a CIO hearing 'the agent learns' should be to ask how it is prevented from learning the wrong thing. Layerup's controls are explicit.


- Evaluation gating. No learned change reaches production without beating the current behavior on a held-out, versioned evaluation set. Regressions are blocked before deployment, not discovered after.
- Human-approved promotion. Guideline and rule changes are reviewed by a designated owner. The system proposes; people promote. The audit trail records who approved what and on what evidence.
- Versioning and rollback. Every agent configuration, prompt, ruleset, and model version is versioned. Any change can be rolled back, and any decision can be replayed against the exact configuration that produced it.
- Confidence-based routing. The agent acts autonomously only where calibrated confidence and policy allow. Low-confidence or high-severity files route to a human, and that human's action becomes the next training signal.
- Full auditability. Because every decision is traced to its inputs, tools, and configuration version, regulators and internal audit can reconstruct exactly why any given claim was handled the way it was.


The combination matters: the system explores improvements aggressively but promotes them conservatively. You get the compounding upside of a learning system with the change-control discipline of a regulated environment.


## What this looks like on core claims metrics


Take a representative deployment in auto physical-damage and low-complexity bodily-injury claims. The agent's job is first-notice-of-loss intake, document extraction, coverage verification, severity and reserve recommendation, and routing — with adjusters reviewing and approving. The figures below are illustrative of the trajectory these metrics tend to follow as the system accumulates worked files over the first six months. The absolute numbers vary by line, data quality, and starting point; the shape of the curve is the consistent part.


Month one is the cold-start baseline. The agent works from guidelines and general capability but has seen none of the book's history yet. It is useful but cautious: it extracts well, proposes reserves, and routes a large share of files to humans because its calibrated confidence is still low on this domain.


Touchless straight-through rate


~30%


Extraction field accuracy


~92%


Reserve within ±10% of ultimate


~60%


Avg. FNOL-to-decision


~8 hrs


Over the following weeks adjusters correct the things the agent gets wrong on this specific book — unusually formatted repair estimates, coverage edge cases, severity patterns that are reserved more conservatively than the general baseline. Each correction is captured, clustered, and — after review — compiled into retrieval context and into deterministic validation rules. By month three the same agent is materially stronger because it now carries those corrections.


Touchless straight-through rate


~50%


Extraction field accuracy


~97%


Reserve within ±10% of ultimate


~72%


Avg. FNOL-to-decision


~3 hrs


By month six the loop has closed on outcomes, not just corrections. Files opened in months one and two have resolved, so the system can now compare its early reserve recommendations against ultimate paid and tune severity scoring against what actually happened. Reopen rate becomes a training signal against premature closure. The evaluation set is large enough that model-level adaptation has been promoted after clearing the golden-set gate.


Touchless straight-through rate


~70%


Extraction field accuracy


~99%


Reserve within ±10% of ultimate


~84%


Avg. FNOL-to-decision


~1 hr


The headline metric — touchless straight-through rate — roughly doubles, but the more important story is underneath it. Touchless rate rises because confidence gets better calibrated on the book, which happens because extraction accuracy rises and reserve accuracy rises, which happens because corrections and outcomes flow back into the system. Each metric is a lever on the others, and the loop pulls all of them at once. The same number of adjusters end up handling the genuinely ambiguous files while the routine majority resolves in about an hour without a human touch.


## Why this becomes a durable advantage


There is a strategic point buried in the metrics. Because the improvement is driven by your corrections and your outcomes on your book, the resulting agent is tuned to your business specifically. The system that handles your claims at month six is not a configuration any competitor can buy off a shelf — it is the compounded product of your own operational data. The more files you run through it, the larger that gap grows.


This is the difference between buying a tool and building an asset. A static model depreciates as the world drifts away from its training data. A compounding agent appreciates as it accumulates more of your work. The flywheel — more files, more signal, better accuracy, more files handled autonomously, more signal — is the entire point.


## Deployment pattern


1. Start in shadow mode on one line. Let the agent propose decisions on live files without acting, so the first weeks of correction signal accumulate against real outcomes with zero production risk.
2. Promote to assisted mode with confidence-based routing. The agent acts on high-confidence files, humans handle the rest, and every human action feeds the loop.
3. Watch the evaluation set, not just the dashboard. Track touchless rate, extraction accuracy, reserve accuracy, and cycle time together — they move as a system.
4. Let outcomes close the loop. Reserve and reopen signal arrive on a delay; the deepest accuracy gains land once early files resolve and feed back.
5. Expand scope as confidence calibrates. The automatable share of the book grows as the agent earns it, file by file.


## The headline


The question to ask any AI vendor is not 'how accurate is it today.' It is 'how much better will it be after my team has run a year of files through it, and what in the architecture makes that true.' For a static model the honest answer is 'about the same.' For Layerup's agents the answer is engineered into the system: every file worked is signal, every correction is captured, every change is evaluation-gated and human-approved, and the agent that handles your claims keeps getting better the more your enterprise uses it.


Tags


AI agents


Claims


Continuous learning


Evaluation


Architecture


Authored by


Layerup


The agentic AI operating system for insurance. We deploy AI agents inside the systems carriers, MGAs, MGUs, TPAs, and health plans already run.


[Book a demo](https://www.uselayerup.com/contact) •


[Explore the platform](https://www.uselayerup.com/platform)


—


Related


## Keep reading.


More pieces from the same category, or the same audience.


[Claims Compressing FNOL-to-payment cycle time from 14 days to 36 hours The industry talks about cycle time as if it were a property of the claim. It is not. It is a property of the queue. Here is how to drain the queue. May 22, 2026 10 min read](https://www.uselayerup.com/blog/compressing-fnol-to-payment-cycle-time)


[Claims Estimate QA is the highest-leverage AI deployment in auto and property claims Estimate QA decides what you pay. Reviewing every estimate, line by line, is the single workflow with the largest dollar impact per agent hour. May 8, 2026 9 min read](https://www.uselayerup.com/blog/estimate-qa-highest-leverage-claims-deployment)


[Claims Closing the subrogation gap: turning recoverable exposure into actual recoveries Subrogation is not a detection problem. It is a workflow problem. Files with recovery potential get identified, then quietly drop off the radar because the next step is too expensive to take. April 24, 2026 9 min read](https://www.uselayerup.com/blog/closing-the-subrogation-gap)


Get started


## Move from reading to deploying.


Pick one workflow inside one line of business. Talk to us about where the highest-leverage starting point is in your operation.


[Book a demo](https://www.uselayerup.com/contact)[All posts](https://www.uselayerup.com/blog)
