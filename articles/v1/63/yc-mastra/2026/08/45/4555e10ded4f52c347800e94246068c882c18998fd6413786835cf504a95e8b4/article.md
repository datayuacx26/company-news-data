---
schema_version: "1.0.0"
document_id: "4555e10ded4f52c347800e94246068c882c18998fd6413786835cf504a95e8b4"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/llm-as-a-judge"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-13T12:39:25.041167+00:00"
fetched_at: "2026-08-13T12:39:27.012997+00:00"
content_hash: "sha256:c2624c9b20f2f15bb67fa9ce08049027b93fdaeb5bc4af5aed2a9f13c70ef0fd"
---

# LLM as a judge: how to evaluate LLM and agent outputs at scale

You shipped an agent that returns a 200 OK on every request, and your dashboards are green. Yet users report answers that drift off-topic, invent facts, or ignore retrieved context. Traditional metrics fall apart here, because qualities like tone or faithfulness resist rigid measurement.


Manual review catches these problems, but it does not scale past a few hundred outputs. That gap is where LLM as a judge earns its place. You use one language model, guided by an evaluation prompt, to score the output of another against criteria you define. Research on GPT-4 as a judge shows[up to 85% agreement with human annotators](https://www.confident-ai.com/blog/why-llm-as-a-judge-is-the-best-llm-evaluation-method) , slightly above the agreement two humans reach on the same task.


This guide covers what the method is, the scoring and prompting techniques that make judges reliable, and how to extend LLM as a judge evaluation to multi-step agents in production.


## What is LLM as a judge?


LLM as a judge is an evaluation technique rooted in natural language processing that uses a large language model to score the text outputs of an AI system against criteria you specify in a rubric. The judge receives the input, the generated output, any supporting context, and a set of instructions, then returns a score, a label, or a preference between two candidates.


It is worth being precise about what this is not. It is not a fixed metric like BLEU or ROUGE, which compare surface-level n-gram overlap against a reference. Those methods are transparent and cheap, but they miss semantic nuance and demand a golden answer. An LLM judge instead approximates human labeling, relying on the semantic relationships the model learned during training.


The technique is flexible by design. You can grade helpfulness, faithfulness, tone, or brand voice by rewriting the prompt, with no model retraining involved. That flexibility is the core reason this approach matters: it turns subjective quality assessment into a repeatable, automated signal.


### Key characteristics of an LLM judge


Your judge is more than a scoring wrapper. A few properties define how it behaves and where it fits in an evaluation pipeline.


-


**Prompt-driven scope:** the evaluation prompt alone determines what the judge measures, so one model can grade many criteria.


-


**Referenceless or reference-based:** it can score an output on its own merits or against a golden answer when you have one.


-


**Explainable output:** unlike n-gram metrics, a judge can return the reasoning behind its verdict, which helps you debug failures.


-


**Non-deterministic by nature:** the same input can yield slightly different scores, so calibration and structured constraints matter.


## Why LLM as a judge works, and when to trust it


You might reasonably wonder why a language model would be any better at judging text than at generating it. The answer is separation of tasks. When your product generates a response, it juggles context, user input, and a long instruction set at once, and that complexity invites mistakes.


The judge does something narrower. It receives a focused prompt and performs what amounts to a classification task: is this output relevant, faithful, or biased? Critiquing is easier than creating, and detecting a mistake is easier than avoiding it in the first place. A relevance judge only checks whether an answer addresses the question. It does not have to produce a better answer.


Independence is the other reason this holds up. Because the judge runs as a separate call, often with a different model, it can catch bias or hallucination even when the primary model was manipulated into producing them. This does not make the judge smarter than your production model. It is simply doing a simpler, more focused job.


Trust has limits, though. You should treat LLM as a judge scores as a proxy that needs validation against human labels, and you should stay alert to the biases covered later in this guide.


## LLM as a judge use cases


Your judge fits three moments in the lifecycle of an AI product, and recognizing which one you are in shapes how you design the evaluation.


During development, you compare models and prompts to confirm you are actually improving. After changes, you run regression testing to make sure nothing broke. Once the system is live, you monitor real interactions for quality and safety. Production is where LLM as a judge evaluation shines, because there is rarely a reference answer to compare against.


Concrete applications span most LLM product types. The table below maps common product types to the criteria an LLM judge typically scores.


**Product type** **Typical judge criteria** **Reference needed**


Customer support chatbot User frustration, resolution, tone No


RAG system Context relevance, faithfulness No


Code generation tool Correctness, conciseness Yes (known implementation)


Marketing copy generator Brand voice, format adherence No


Data extraction pipeline Field accuracy, completeness Yes (labeled records)


For a customer support chatbot, you can detect user frustration, unresolved conversations, or repeated questions across a full multi-turn transcript. Tracking user frustration signals early through tone and escalation patterns helps you intervene before satisfaction drops.


For a retrieval-augmented generation system, you can score context relevance and check faithfulness to retrieved documents. For a code generation tool, you can grade correctness against a known implementation.


Teams also use judges to enforce style. You can score whether a marketing draft matches brand voice or whether a response leaks personally identifiable information. The pattern stays constant: define the criterion, write the prompt, run it at scale.


## Types of LLM judges


You have several judge structures to choose from, and the right one depends on whether you have a reference answer and whether you are scoring one output or comparing many. The three canonical types were formalized in the[MT-Bench and Chatbot Arena paper](https://arxiv.org/abs/2306.05685) , and they remain the foundation of most LLM as a judge work today.


*An evals preview showing how scored outputs appear in an observability interface.*


Before the details, one clarification: none of these types are limited to single-turn prompts. You can apply each to multi-turn conversations by feeding the full transcript into the rubric, as long as it fits the context window.


### Referenceless single-output scoring


You reach for a referenceless judge when there is no single correct answer, which is common for open-ended or creative tasks. The judge sees only the input, the generated output, and optionally supporting context such as retrieved documents or the tools an agent called. It then scores against a rubric covering qualities like accuracy, coherence, or helpfulness.


This is the most common form of direct scoring in production, precisely because it needs no golden answer. You might score the helpfulness of support replies or the readability of generated copy. The judge interprets your criteria independently, which is powerful but also why a clear rubric matters so much.


### Reference-based scoring


You use reference-based scoring when a correct answer exists and you want more consistent, reproducible verdicts. Here the judge receives an expected output alongside the generated one. That reference acts as an anchor, calibrating the judge and reducing score variability, especially on nuanced criteria like factual correctness.


This structure suits tasks with clear right answers: checking a generated math solution against a labeled one, or verifying code correctness against a known implementation. It is also the backbone of regression testing, where you compare a new response to a previously approved answer for the same input.


### Pairwise comparison


You choose a pairwise comparison when you want to know which of two outputs is better rather than an absolute score. The judge sees both candidates for the same input, in randomized order, and picks a winner based on your criteria. This mirrors the human voting in Chatbot Arena, with an LLM standing in for the crowd.


Pairwise comparisons excel at A/B testing prompts, models, or fine-tunes, and they work even when a rubric is subjective. The tradeoff is that they run offline, since you need to generate multiple responses to contrast. They also carry position bias, which you address by swapping order, covered later.


### Evaluation by criteria


You apply evaluation by criteria when you want to grade one specific dimension at a time rather than a general preference. Instead of asking whether an output is good overall, you ask whether it is concise, whether it stays in format, or whether it contains bias.


This decomposition keeps each judgment focused, which improves accuracy and makes results easier for reviewers to verify. You can run these criteria as separate evaluators and combine their verdicts deterministically afterward, a technique the scoring section returns to.


## Building LLM judges in TypeScript with Mastra


You can build the techniques in this guide inside a TypeScript codebase with Mastra, an open-source framework for AI agents, workflows, and evals. Its eval system supports[LLM as a judge](https://mastra.ai/docs/evals/custom-scorers) scoring with custom rubrics, plus classification, tool-calling, and multi-turn evals that grade entire agent trajectories.


*Mastra records each agent run as a trace of spans, so you can attach judges at the trace or span level.*


Every run produces a trace showing which model was called, the tokens in and out, and where a step failed. You inspect these in Studio during local development and export them to OpenTelemetry-compatible backends in production. Because the framework routes to **90+ model providers** through one interface, you can pick a strong judge model independently of the model your agent runs on.


The main tradeoffs are honest ones. Mastra is TypeScript-only, so it does not fit teams working primarily in Python, and it is a younger project than some established evaluation libraries.


[Build your first traced agent evaluation with Mastra.](https://mastra.ai/docs)


## LLM judge scoring methods and metrics


Your scoring design has more impact on reliability than almost any other choice, because LLMs generate text and are not naturally calibrated for fine-grained numeric scales. The goal is to extract a stable signal from a probabilistic model, and several methods help you do that.


### Binary versus multi-categorical scoring


You get the most reliable results from binary or low-precision scoring. Asking a judge to decide “polite” versus “impolite” is far more consistent than asking it to distinguish a 73 from an 82 on a politeness scale. As the interval gets finer, the model produces increasingly arbitrary scores.


When binary is too coarse, a three-option scheme works well, such as “relevant,” “irrelevant,” and “partially relevant.” Adding an “unknown” option prevents the judge from forcing a verdict when it lacks information, which reduces noise in your results.


### G-Eval, GPTScore, and DAG-based scoring


You can improve reliability with[G-Eval](https://galileo.ai/blog/g-eval-metric) , a framework that uses chain-of-thought prompting to stabilize judgments. The judge first generates detailed evaluation steps from your criterion, then applies those steps to produce a score through a form-filling process.


A related approach,[GPTScore](https://github.com/jinlanfu/GPTScore) , scores outputs by computing the conditional probability of a target text given the rubric prompt, which avoids explicit rubric steps but depends heavily on how you frame the prompt. Both suit subjective criteria like coherence and answer relevancy where structured reasoning helps. Early benchmarks using GPT-4 as the judge model showed that G-Eval aligned more closely with human rankings than traditional metrics on summarization tasks.


The catch is that G-Eval is not deterministic, so a benchmark built on it can drift between runs. When you have a clear-cut criterion, such as format correctness in a summarization task, you can structure the evaluation as a directed acyclic graph. Each node is a judge handling one atomic decision, and edges define the logical flow. Breaking judgment into binary sub-decisions removes ambiguity and enforces alignment with your expectations.


### Using probabilities of output tokens


You can smooth an otherwise jumpy score by using the probabilities of output tokens. Rather than asking for a single fine-grained number, you ask the model to consider a range of scores and compute a weighted sum from the token probabilities. This produces a continuous, more stable score without asking the judge to make artificially precise distinctions.


The technique pairs naturally with chain-of-thought scoring and is one way to counteract the tendency of judges to cluster around round numbers seen frequently in training data.


### Semantic versus operational metrics


You will work with two broad families of metrics when running LLM as a judge evaluations. Semantic metrics assess meaning: faithfulness, relevance, correctness, and tone. These are where judges add the most value, because n-gram methods like ROUGE and BLEU cannot capture them. Operational metrics track measurable behaviors, such as format adherence, PII presence, or the number of turns to resolution.


The table below contrasts the main approaches so you can match a method to a task before writing any prompts.


**Approach** **Reference needed** **Best for** **Main limitation**


Binary scoring Optional Clear yes or no criteria Too coarse for graded quality


G-Eval Optional Subjective criteria like coherence Non-deterministic across runs


DAG decision tree Optional Deterministic, rule-like checks More effort to build


ROUGE and BLEU Required Structured tasks with a golden answer Misses semantic nuance


## How to build an LLM judge step by step


You should treat building a judge as a small machine learning project rather than a one-off prompt. The judge needs its own evaluation, because a scorer you cannot trust is worse than no scorer at all. The following steps take you from raw data to a validated evaluator.


1.


**Look at your data and define failure modes.** You start by reading your actual outputs. Before writing any criteria, review a sample of real outputs and synthetic data, and note where the system goes wrong: contradictions, omissions, off-topic answers, or leaked context. These observed failure modes become the concrete behaviors your judge must catch. Keep the scope tight and split distinct dimensions into separate evaluators from the start.


2.


**Define the metrics that matter.** You then translate failure modes into specific, measurable criteria. Decide which changes actually matter for your use case. In one correctness judge, adding a greeting might be acceptable while suggesting an unrequested action counts as wrong. That decision belongs to you, not the model. Favor clear, binary distinctions wherever possible, and write down exactly what each label means.


3.


**Write the judge prompt and pick the judge model.** You now craft the core prompt, the heart of the whole system. Write it like instructions for an intern doing the task for the first time: define every term, explain what each label means, and include the reasoning steps you want followed. Good prompt engineering here means being explicit rather than clever. Start with a more capable judge model to establish a strong baseline, then test whether a smaller one meets your needs.


4.


**Tune the model settings for consistency.** Prompts behave differently across models, so instructions tuned for one may not transfer. Set a low temperature so the same input yields consistent verdicts, and request structured outputs like JSON to make parsing trivial. This is where prompt engineering pays off a second time: small wording changes can shift agreement rates by several points.


5.


**Validate the evaluator against human labels.** You finish by measuring the judge against a manually labeled golden dataset that serves as your ground truth. Run the judge over that set and compare its verdicts to your labels using precision and recall, weighting whichever error type hurts most. If you care most about catching incorrect responses, prioritize recall. Hold out part of your labeled data and only test the final prompt against it, the way you would guard a test set in any ML workflow.


## Prompting techniques for reliable judges


Your prompt is where an unreliable judge becomes a trustworthy one, and the same prompt engineering tricks that steer product outputs steer judges just as well. A handful of techniques deliver most of the gains.


### Chain-of-thought prompting and requiring explanations


You get more accurate verdicts when you ask the judge to reason before it scores. Including detailed evaluation steps rather than a vague instruction pushes the model to work through the criteria, an approach known as chain-of-thought prompting. It reliably improves alignment with human expectations.


The reasoning also leaves a trail you can inspect. When an LLM as a judge evaluator flags an output as unfaithful, the explanation shows which sentence triggered it, which turns debugging from guesswork into review.


### Few-shot prompting


You can anchor a judge’s behavior with few-shot prompting, adding a few labeled examples to the prompt. This few-shot learning approach reportedly raised one model’s consistency from 65% to 77.5%, and it remains one of the highest-leverage changes you can make. Include few-shot examples that cover both clear and borderline cases.


Order and balance matter. If every negative example sits at the end, or negatives outnumber positives, the arrangement can skew verdicts, so test how your examples affect results before trusting them.


### Constrain to structured outputs


You reduce parsing errors and downstream noise by requiring structured outputs. Ask for JSON with named fields for the verdict, the score, and the reasoning. A predictable schema lets you aggregate thousands of evaluations without brittle string handling.


Structured output also nudges the model toward discipline. When the format enforces a single label from a fixed set, the judge is less likely to hedge or produce something you cannot score.


### Score smoothing and criteria decomposition


You handle complex rubrics best by decomposing them. Split completeness, accuracy, and relevance into separate evaluators, let each judge one quality, then combine results deterministically. You might flag an answer if any criterion fails, or sum positive labels into a weighted score.


Score smoothing complements this. Techniques like question-answer generation break an output into sentence-level checks and compute a score from the proportion that pass, which turns an arbitrary judgment into a defensible number.


## Reliability, bias, and calibration


You cannot trust a judge you have not stress-tested, because these evaluators carry systematic biases that quietly distort scores. Knowing the common ones and their countermeasures is the difference between a signal you can act on and one that misleads you.


### Position bias and swapping positions


You will hit position bias whenever you run pairwise comparisons, because judges tend to favor the response shown first. The fix is straightforward: run each comparison twice with the order swapped, and only declare a winner when the same output wins both times. Anything else counts as a tie.


This doubles your evaluation cost for pairwise judging, which is one more reason many teams prefer direct scoring for high-volume production monitoring.


### Calibration and reproducibility


You improve reproducibility by controlling the sources of variance. Set a low temperature, use a capable model, and constrain the output format. Because scores are still non-deterministic, you can poll the judge multiple times and aggregate through voting or averaging, an approach sometimes described as replacing a single judge with a jury.


Verbosity bias and self-preference, sometimes called LLM narcissism, are worth watching too. Judges tend to reward longer answers and, in some cases, favor text from the same model family. Direct scoring against explicit criteria dampens both.


### Agreement with human judgment


You measure a judge’s quality by how well it agrees with people. Build a labeled dataset, compute agreement, and treat that number as your calibration baseline. When a capable model reaches around 85% agreement, comparable to inter-annotator agreement, you have reasonable grounds to trust it for that task.


Agreement drifts over time as your product and expectations change. Recheck it periodically, and be ready to revise the prompt or add criteria when observed outputs no longer match what your LLM as a judge system rewards.


## Pitfalls of LLM as a judge and how to avoid them


You will get burned if you treat a judge as plug-and-play, so it helps to name the failure modes directly. The most common one is over-trusting non-deterministic scores. A judge can return different numbers for the same output, so build in structured constraints or aggregation before you rely on a benchmark.


Vague criteria are the second trap. A prompt that says “rate professionalism” with no definition lets the model fall back on training-data assumptions, which imports bias you never intended. Define every term and prefer binary choices.


Cost and latency are practical pitfalls. Powerful judges are slower and pricier than regex or small classifiers, which makes them poorly suited to real-time guardrails. Mixing methods helps: filter obvious cases with cheap rules, then reserve the judge for nuanced calls.


Data privacy is a related concern, since third-party judge APIs mean sending outputs to an external service. Factor that into your architecture decisions early.


The final pitfall is skipping validation. An LLM as a judge system that has never been checked against human labels is a guess wearing a lab coat. Always calibrate first, then monitor the judge itself over time.


## Evaluating multi-step agent behavior


You face a harder problem when the system under test is an agent rather than a single completion. An agent might call a model, invoke three tools, retry a failed call, and branch into a sub-workflow, all inside one user request. Scoring only the final answer hides where and why things went wrong.


*An agent run produces a tree of spans, which lets you evaluate individual steps as well as the final output.*


The depth here draws on patterns documented in[Principles of Building AI Agents](https://mastra.ai/books/principles-of-building-ai-agents) , which frames agent evaluation as a way to manage both accuracy and token cost in production.


### Trace-level versus span-level evaluation


You gain the most insight by evaluating at two levels. Trace-level evaluation judges the whole run: did the agent accomplish the user’s goal end to end? Span-level evaluation zooms into individual steps, scoring whether a specific tool call used the right arguments or whether a retrieval step returned relevant context.


Combining both tells you not just that a run failed but where. A trace can succeed overall while a wasteful retry inflated token cost, and only span-level scoring surfaces that.


### Task completion and tool-call adherence


You should measure two things agents get wrong that single completions cannot. Task completion asks whether the agent actually finished what the user requested, judged across the full trajectory rather than any one turn. Tool-call adherence asks whether the agent selected the right tool and passed valid arguments.


A referenceless judge handles both well, reading the trace and scoring against a rubric you define. For repeatable checks, a DAG structure lets you enforce that required tools were called before grading the outcome.


## Observability, LLM evals, and guardrails in production


You do not get to stop evaluating once you ship, because real users exercise your system in ways no test suite anticipated. Production evaluation rests on three pillars: tracing to capture what happened, scheduled LLM evals to score it, and guardrails to block harmful outputs in real time.


Tracing comes first. You instrument the application to capture inputs, model and tool calls, and completions, then store them for analysis. With traces in hand, you schedule LLM as a judge runs over samples of live traffic and push results to a dashboard where trends like a spike in hallucinations become visible. The LLM as a judge framework you choose for this step should support both referenceless and reference-based scoring so you can handle production traffic and regression suites with the same tooling.


### Online versus offline evaluation


You run offline evaluation during development against a fixed dataset, where you can use reference answers and pairwise comparisons. It answers whether a change improves quality before it reaches users, which makes it the right home for regression testing.


Online evaluation happens on live production traffic, where no reference exists. You sample recent interactions, score them referencelessly, and track the distribution over time. Sampling a subset, such as 10% of conversations, keeps cost manageable at scale.


### Evaluations versus monitors and alert conditions


You should separate two concepts that are easy to conflate. An evaluation produces a score for an output against a criterion. A monitor watches those scores over time and fires an alert when a condition trips, such as the share of unfaithful answers crossing a threshold.


Evals tell you how good an output is. Monitors tell you when your aggregate quality is slipping, so you can intervene before too many users are affected. You need both, and they operate on different cadences.


### Guardrails for prompt injection and output sanitization


You defend against prompt injection and unsafe outputs with guardrails, which run inline as generation happens rather than on a schedule. Because a heavyweight judge is too slow for that path,[guardrails](https://mastra.ai/blog/building-fast-reliable-input-processors) usually combine fast rule-based checks with lightweight classifiers. Frameworks like Mastra let you attach input and output processors to an agent so you can sanitize responses and block injection attempts before they reach the user.


Guardrails and evals are complementary, not interchangeable. Guardrails stop a bad output at the moment. Evals and monitors tell you how often bad outputs occur and whether your defenses are holding.


## Wrapping up


You now have the full arc: pick a judge type, constrain its scoring, tune the prompt, and validate it against human labels before trusting a single number. Start simple with binary criteria and direct scoring, then add DAG structures only when a task demands them. If your system includes agents, extend the same trace-first evaluation to individual tool calls so you catch failures the final answer would hide.
