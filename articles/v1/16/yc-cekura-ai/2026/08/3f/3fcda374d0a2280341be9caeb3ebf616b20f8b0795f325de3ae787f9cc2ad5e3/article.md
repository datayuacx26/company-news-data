---
schema_version: "1.0.0"
document_id: "3fcda374d0a2280341be9caeb3ebf616b20f8b0795f325de3ae787f9cc2ad5e3"
company_key: "yc-cekura-ai"
company: "Cekura"
source_id: "yc-cekura-ai-news-import-2109cda3ccd2"
canonical_url: "https://www.cekura.ai/blogs/metric-optimizer-trustworthy-evals"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T08:47:33.696615+00:00"
fetched_at: "2026-08-12T08:47:35.593125+00:00"
content_hash: "sha256:a759c63739356f7ef4e0f4b11ab471c946b6e951c1ecaa764ea63be23e125ab6"
---

# Beyond 100%: How Cekura Makes Metric Optimization Trustworthy

Cekura's Metric Optimizer fits an evaluation metric to labeled call data, then checks its own result. It asks a reviewer when the evidence does not resolve a rule, reruns the dataset before accepting a perfect score, and reports the cases it could not settle instead of hiding them.


Last updated: August 2026 By Rishabh Sanjay, Founding Engineer, Cekura


TL;DR


- Cekura's Metric Optimizer does more than fit an evaluator to past labels. It asks for human judgment when a rule is unclear, verifies apparent 100% scores against evaluator noise, and surfaces unresolved cases instead of hiding uncertainty.
- A single clean pass is not evidence. In a 2026 study of a production safety-evaluation harness, borderline items flipped pass/fail on up to ~50% of identical reruns at the provider default temperature ([Tamba, 2026](https://arxiv.org/abs/2606.26185) ). That study measured a safety grader, not Cekura's optimizer, so the transferable finding is the run-to-run instability rather than a rate.
- Labels alone do not carry the rule behind a verdict. A call marked wrong because the agent barged in, because a policy has an exception, or because the reviewer read "successful" differently produces the same label and three different rules.
- A metric that only fits the calls used to improve it has been fitted, not tested. A regression check runs the proposal against prior calls it never saw and shows only the verdicts that changed.
- Cekura tests, monitors and self-improves voice and chat agents. This post covers one layer of that: the evaluation metrics those agents get scored against. It does not remove the need for a human to decide what a correct verdict is.


## Why does metric optimization need more than a higher score?


Evaluation metrics rarely fail because teams do not have a rubric. They fail because real conversations expose the parts of the rubric that were never fully specified: an edge case that should not count, a label that needs context, or an evaluator that reaches a different conclusion on the same call twice.


A few months ago, we wrote about[Cekura's Metric Optimizer](https://www.cekura.ai/blogs/voice-evals-auto-improve-human-feedback) as a way to fit an evaluation metric to annotated call data. The core idea still holds: start with examples a human has reviewed, inspect the evidence behind every miss, and produce a metric that better reflects the team's intent.


The difference now is that the optimizer is designed less like a system that simply chases a higher score. It behaves more like a collaborator: one that knows when to ask for human judgment, when to distrust a perfect result, and when to explain what it could not resolve.


## How does the optimizer clarify a rule instead of just fixing failures?


Labels alone do not always contain the rule behind a verdict. A call may be marked wrong for reasons the label never records. Perhaps the agent barged in while the caller was still speaking, or the endpointer cut the caller off mid-sentence and the agent answered the wrong question. Perhaps a policy has an exception, or the reviewer intended a different interpretation of 'successful.'


The optimizer now reads the feedback attached to each sample before it acts. When a note already explains the decision, it uses that context. When the evidence is genuinely ambiguous or conflicts with the label, it pauses and asks a focused question in the product.


Reviewers see the relevant transcript, audio when available, the metric inputs, the current label, the metric's verdict, and a link back to the source call. They can choose from suggested answers, provide their own context, or approve a corrected label. The run resumes with that decision rather than guessing.


What changes for the reviewer:


- Focused questions only when the data does not resolve the rule.
- Relevant evidence in one place: transcript, audio, metric inputs, label, and result.
- Grounded notes saved back to the sample, so the next run starts smarter.


## Why does a 100% score have to earn trust?


LLM-based evaluators can be non-deterministic, even with conservative settings. A candidate that gets every example right once may still flip on the next run. Treating that single pass as a finished metric is how an optimizer can look successful in a demo and disappoint in production.


*The same evaluator, the same call, three runs. A perfect score on one pass says nothing about the next.*


The size of that effect is measurable. In a 2026 study of a production safety-evaluation harness, borderline items flipped pass/fail on up to ~50% of identical reruns when temperature was left at the provider default. Across 690 API calls, 1 to 2 of 7 borderline items stayed non-reproducible even under forced greedy decoding ([Tamba, 2026](https://arxiv.org/abs/2606.26185) ).


That harness graded safety evaluations rather than Cekura metrics, so the transferable finding is the instability itself, not the rate. Non-determinism is not unique to graders; it changes measured LLM performance across runs generally ([Song et al., The Good, The Bad, and The Greedy](https://arxiv.org/abs/2407.10457) ).


## How is a perfect score verified?


The updated optimizer treats a perfect score as a claim to verify. It reruns the full dataset before allowing a candidate to win. For samples that have shown instability, it also performs repeated stability checks. If a case flips, the optimizer does not present the result as a clean, verified 100%.


That verification is not free. Rerunning the whole dataset and then repeating checks on unstable samples multiplies evaluator calls, so you are buying confidence with compute and wall-clock time. The tradeoff is deliberate: a metric that ships on one clean pass costs less to produce and more to trust.


This is why Cekura's own benchmarks report pass^1 and pass^3 side by side rather than a single number: each scenario runs three times through 59 evaluators, and a result only counts as reliable when it holds across all three ([benchmark methodology](https://benchmarks.cekura.ai/) ). That benchmark tests voice orchestration rather than metric optimization, so what carries across is the method, not the figures. Cekura's optimizer applies the same standard to a candidate metric, whichever[voice agent evaluation metric](https://www.cekura.ai/blogs/voice-ai-evaluation-metrics) it targets.


*Reporting pass^1 and pass^3 separately keeps a result that held once from being read as a result that holds.*


## What happens to the cases that stay unresolved?


The optimizer preserves the uncertainty. The review experience calls out unresolved samples and classifies the reason: evaluator noise, an ambiguous label, or a limitation that cannot be fixed without overfitting. You can refine the definition, correct the data, or accept that a case has occasional variance. Reporting guidance for human annotation makes the same point: agreement work should include the analysis of disagreement patterns, not just a headline agreement score ([James, 2026](https://arxiv.org/abs/2603.06865) ).


*Unresolved cases stay visible and labeled by reason, rather than being averaged into a clean score.*


## What does the optimizer explain about its recommendation?


The output is still an editable metric that you review before applying. What has improved is the explanation around it. The optimizer now leads with what changed from the original metric in plain language, rather than only describing the final rule. It also keeps the relevance gate separate from the evaluator body, so a proposed change is easier to understand and safer to adopt.


Here is what the loop looks like when you run it:


1. Review real calls and leave a short note when the verdict needs context.
2. Start Auto Improve.
3. Answer any focused questions the optimizer raises.
4. Review the proposed changes, the verified score, and any unresolved cases.
5. Apply, edit, or reject the recommendation.


## How do you test a metric beyond its training set?


A metric should not only fit the calls used to improve it. Once a proposal is ready, Cekura gives you a post-optimization review chat and a regression check to test the proposed metric on a broader set of prior calls and runs.


The regression check highlights the samples where the proposed metric produces a different label than the previous version. Instead of treating every difference as a win, you can inspect those changed verdicts, decide which ones are correct, and explain the edge cases in plain language in the review chat.


*A regression check runs the proposed metric against calls it was never optimized on, and surfaces only the verdicts that changed.*


That feedback closes a second loop. The chat proposes the relevant reviewed cases for inclusion; after the reviewer confirms, those cases are added to the evaluation dataset and a new Auto Improve run can optimize against them. This turns regression review from a final approval step into a way to continually broaden the metric's coverage.


The post-optimization loop:


1. Run a regression check across broader historical calls and runs.
2. Review only the cases whose verdict changed between the old and proposed metric.
3. Use chat to explain incorrect changes; confirm the cases that should become new training examples.
4. Re-run Auto Improve with the expanded dataset.


## FAQ


### Why does an eval metric score 100% and still fail in production?


Because a single pass is not evidence of stability. LLM-based evaluators can return different verdicts on the same call across identical runs, so a candidate metric that clears every example once may flip on the next run. Cekura reruns the full dataset before a candidate can win, and adds repeated stability checks on samples that have already shown instability. A score that cannot survive those reruns is not reported as a verified 100%.


### How many times should you rerun an LLM evaluator before trusting a score?


At least enough to catch the borderline cases, because those are where verdicts flip. Cekura's benchmarks run each scenario three times and report pass^1 and pass^3 separately, so a single-run success cannot be mistaken for a reliable one. Those benchmarks measure voice orchestration rather than metric optimization, so the method transfers and the numbers do not. For metric optimization, the optimizer reruns the whole dataset and then repeats checks on the samples with a history of instability.


### What is a regression check on an eval metric?


A regression check compares a proposed metric against the previous version across a broader set of prior calls and runs, then surfaces only the samples whose verdict changed. Cekura shows those changed verdicts for review instead of counting every difference as an improvement. Reviewers decide which changes are correct, and the cases they confirm can be added to the evaluation dataset for the next optimization run.


### When should a human review an evaluator's verdict?


When the evidence does not resolve the rule. Cekura's optimizer reads the feedback attached to each sample first and proceeds on its own when a note already explains the decision. It pauses only when the evidence is genuinely ambiguous or conflicts with the label, then asks one focused question with the transcript, audio where available, the metric inputs, the current label and the metric's verdict in one place.


### Can you optimize an eval metric without overfitting to the training set?


Not by fitting alone. A metric tuned only on the calls used to improve it can encode those calls rather than the rule behind them. Cekura separates the two by testing a proposal against prior calls and runs it was not optimized on, and by classifying unresolved cases as evaluator noise, an ambiguous label, or a limitation that cannot be fixed without overfitting. Naming that third category keeps it from being optimized away.


Use the Metric Optimizer in Cekura's[Metric Lab](https://docs.cekura.ai/documentation/guides/metric-lab) to turn reviewed calls into evaluation logic your team can inspect, trust, and keep improving.
