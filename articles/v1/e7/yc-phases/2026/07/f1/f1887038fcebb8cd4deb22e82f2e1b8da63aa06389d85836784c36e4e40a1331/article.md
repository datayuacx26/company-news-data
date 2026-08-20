---
schema_version: "1.0.0"
document_id: "f1887038fcebb8cd4deb22e82f2e1b8da63aa06389d85836784c36e4e40a1331"
company_key: "yc-phases"
company: "Phases"
source_id: "yc-phases-news-import-61e19d289ed9"
canonical_url: "https://phases.ai/blog/you-cant-trust-chatgpt-or-claude-to-run-your-clinical-trials"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-23T20:18:02.061944+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:883ffce948564c9b7af18de57b2d9f756ea1e94a4a522b3049b12d4d44ef2262"
---

# You can't trust ChatGPT or Claude to run your clinical trials

In June 2025, the FDA introduced Elsa, an AI tool designed to help staff read, write, and summarize information faster, with the longer-term goal of accelerating drug reviews. FDA Commissioner Marty Makary said the rollout came in ahead of schedule and under budget. Weeks later, however, Marcel Botha, CEO of the design firm 10XBeta, told Pharmaceutical Executive that Elsa "was making stuff up."1


That was in mid-2025, and the underlying models have since improved significantly. But perhaps more importantly, the industry has become much better at building the systems around those models.


A model on its own is only one component of an AI system. What matters just as much is its so-called harness: the software that controls what context the model receives, which tools it can use, what actions it can take, how its outputs must be structured and cited, and when it must escalate to a human.


It is not enough to take an off-the-shelf harness from Anthropic or OpenAI and call it a day. The systems and constraints around an agent need to be designed for the specific task it is performing.


The same is true of evaluations, or evals. These are internal benchmarks built around representative use cases that allow you to measure how the complete system performs across many runs and over time.


It's the combination of models, harnesses and evals that result in trusted outputs from AI systems, something ChatGPT and Claude alone cannot be depended on.


## Even frontier models hallucinate


Artificial Analysis, an independent AI benchmarking group, runs a test called AA-Omniscience to measure models' hallucination rates, specifically measuring how models handle factual questions, including whether they answer incorrectly when they should instead admit that they do not know.2 On that benchmark, the most capable of all AI models, Anthropic's Claude Fable 5, recorded a hallucination rate of 55%.2 This measures performance on a factual-recall benchmark, not a clinical-workflow deployment.


For users of these models in high stakes environments, that should be a wake up call. The results demonstrate an important limitation: highly capable models can still answer confidently when they should abstain. That helps explain some of the criticism Elsa faced at launch. Access to internal documents and a capable model are not, by themselves, enough to make a system dependable.


Smarter models do not necessarily hallucinate less, but they have become much better at instruction-following and long-running agentic work. In September 2025, about three months after Elsa launched, Claude Sonnet 4.5 was the first model that showed substantial improvements in its ability to follow instructions across long, multi-step tasks.3


Put a model with that level of reliability inside the right harness, and it can perform useful work over a long time horizon without drifting away from the task, and with very low hallucination rates. But the surrounding system remains critical. Even a model that follows instructions well still needs task-specific guardrails, scoped context, verification steps, citation requirements, and clearly defined escalation paths.


By May 2026, the FDA had released Elsa 4.0 with substantially expanded capabilities. These included access to the agency's consolidated data, secure web search, optical character recognition for scanned documents and images, document-generation tools, and data-analysis and visualization capabilities.4


In short, the harness around the model had become much stronger.


What the FDA announcement does not describe is how the agency evaluates Elsa internally. For a system like this, representative internal evals would be essential for determining whether new models, retrieval methods, and tools actually improve accuracy and consistency rather than merely adding more capabilities, and it's likely that they did not have that many when they first launched.


## Harnesses and evals in clinical trial operations


What does this all mean in the context of clinical operations?


Let's consider two examples: TMF document management and monitoring.


### TMF


TMF document management is one of the cleaner use cases because many outputs can be evaluated against defined labels, document requirements, metadata, and quality criteria.


In this case, the harness needs to provide the model with the relevant document and its metadata, the applicable classification framework, and a clear set of work instructions for assessing document quality. The model should also be required to support each decision with citations linking back to the underlying document.5


A separate verification step using a secondary judge model can then assess whether the classification or quality finding is supported by the cited evidence. Depending on the confidence level and consequence of the decision, that verification may involve a second model, deterministic checks, or human review.6


You can then build a representative dataset of synthetic documents that have already been classified and quality-checked by qualified reviewers. Whenever the system changes, whether because of a new model, prompt, retrieval method, classification rule, or workflow, the complete evaluation suite can be rerun. That allows you to measure whether accuracy remains high and whether the system behaves consistently across releases.


The goal is a system that classifies, files, and quality-checks every document in your TMF, helping you remain **continuously inspection-ready** throughout the trial. Instead of scrambling to remediate the TMF before a database lock, study closeout, or unexpected inspection, your team can identify and resolve gaps as they arise.


### Monitoring


In monitoring, the harness needs to provide the model with access to the appropriate source systems and documents. That may include the CTMS, the study protocol, the clinical monitoring plan, prior monitoring reports, and the annotated monitoring visit report. The system also needs a way to search across those sources as well as a checklist of how it should perform the review.


A separate verification step using a judge model can then assess whether each finding is genuinely supported by its citation and whether the assigned severity and recommended action are appropriate.


The evals should be built from a representative set of synthetic reports that have been independently reviewed by qualified human reviewers, with disagreements adjudicated where necessary.


With these evals, the goal is not only to match all the findings the human reviewer found, but more so to spot the findings the human misses because they can't possibly have the level of oversight an AI can across all the possible document sources. Then, whenever a change is made to the monitoring agent or its harness, the evals should be rerun to ensure performance remains within an agreed tolerance.


The goal is **100% monitoring oversight** across all your reports, with every finding supported by cited evidence. The intended result is review at a **higher quality** , **lower cost** , and **far greater speed** than human review alone.


## Conclusion


One of the most important lessons from the recent development of AI agents is that the model is not the product. An agent is only as reliable as the system around it: the context it receives, the tools and permissions it is given, the workflow it follows, the checks applied to its output, the evals used to measure it, and the humans available to handle exceptions.


The FDA was early with Elsa, and the system has clearly become more capable as both the underlying models and the surrounding infrastructure have improved.


At Phases, we treat the model as only one component of the system. Our agents across TMF, data management, and monitoring are built around[controlled access to source data](https://phases.ai/blog/building-secure-agents-in-clinical-trials) , task-specific workflows, cited outputs, independent verification, human escalation, and critically regression evaluations that are rerun whenever the system changes. That is how you build trust in clinical AI.


If you are interested in learning more about how our agents can help improve clinical operations, you can book a demo using the link below.


---


## References


1.


Mazzolini, Chris and Hollan, Mike. "FDA's Elsa AI Tool Raises Accuracy and Oversight Concerns." Applied Clinical Trials Online. July 23, 2025.[https://www.appliedclinicaltrialsonline.com/view/fda-elsa-ai-tool-raises-accuracy-and-oversight-concerns](https://www.appliedclinicaltrialsonline.com/view/fda-elsa-ai-tool-raises-accuracy-and-oversight-concerns)↩


2.


Artificial Analysis. "AA-Omniscience: Knowledge and Hallucination Benchmark." Accessed July 12, 2026.[https://artificialanalysis.ai/evaluations/omniscience](https://artificialanalysis.ai/evaluations/omniscience)↩↩2


3.


Edwards, Benj. "Anthropic says its new AI model 'maintained focus' for 30 hours on multistep tasks." Ars Technica. September 2025.[https://arstechnica.com/ai/2025/09/anthropic-says-its-new-ai-model-maintained-focus-for-30-hours-on-multistep-tasks/](https://arstechnica.com/ai/2025/09/anthropic-says-its-new-ai-model-maintained-focus-for-30-hours-on-multistep-tasks/)↩


4.


FDA. "FDA Expands AI Capabilities and Completes Data Platform Consolidation." May 6, 2026.[https://www.fda.gov/news-events/press-announcements/fda-expands-ai-capabilities-and-completes-data-platform-consolidation](https://www.fda.gov/news-events/press-announcements/fda-expands-ai-capabilities-and-completes-data-platform-consolidation)↩


5.


Xia, Sirui, et al. "Ground Every Sentence: Improving Retrieval-Augmented LLMs with Interleaved Reference-Claim Generation." Findings of NAACL 2025 / arXiv:2407.01796.[https://arxiv.org/abs/2407.01796](https://arxiv.org/abs/2407.01796)↩


6.


"LLM-as-Judge." Encyclopedia of Agentic Coding Patterns, aipatternbook.com. Accessed July 2026.[https://aipatternbook.com/llm-as-judge](https://aipatternbook.com/llm-as-judge)↩
