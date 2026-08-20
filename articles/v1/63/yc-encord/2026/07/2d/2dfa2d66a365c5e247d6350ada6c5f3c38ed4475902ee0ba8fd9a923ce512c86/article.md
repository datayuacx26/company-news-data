---
schema_version: "1.0.0"
document_id: "2dfa2d66a365c5e247d6350ada6c5f3c38ed4475902ee0ba8fd9a923ce512c86"
company_key: "yc-encord"
company: "Encord"
source_id: "yc-encord-news-import-59af355da1b0"
canonical_url: "https://encord.com/blog/eval-stack-webinar-recap/"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-21T18:06:14.355617+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:bfb4f7660c669a655509660921224691f7a0fa0795d7466d13795ba958286f7f"
---

# The Eval Stack the Top AI Teams Are Building Right Now [Webinar Recap]

# The Eval Stack the Top AI Teams Are Building Right Now \[Webinar Recap\]


[Eric Landau](https://encord.com/author/eric-landau/)


Co-Founder & CEO at Encord


July 17, 2026


|


5 min read


Summarize with AI


-
-
-


*A panel with Martin Fisch (Encord), Jesse Willman (Cohere), and Wei-Yin Ko (Adaptation Labs)*


Benchmark scores look clean. Model quality doesn't. That was the thread running through our recent panel with two of the most experienced practitioners working at the frontier of model evaluation and human feedback infrastructure today.


Here's what we took away from the conversation.


## LLM-as-judge has a precision ceiling and most teams are hitting it


The session opened with a simple question: where exactly are automated eval approaches falling short?


Wei-Yin Ko was direct. LLMs are trained on broad swaths of knowledge, which makes them good at averages, but not specificities. The domains where you most need precise evaluation are often the domains where a general-purpose model has the least reliable judgment. And critically, if your LLM judge is already capable enough to label your data reliably, you have to ask why you're training a new model at all.


Jesse Willman added an important nuance: it's not that LLM-as-judge is useless, it's that teams are getting overoptimistic about its ceiling. The real discipline is in systematic testing. Finding where automated evals genuinely hold up and where human judgment is still irreplaceable, rather than assuming one approach covers everything.


The practical threshold Martin flagged from Encord's experience: 60-70% accuracy from an automated judge might be acceptable for some use cases. For anything where quality really matters, you need to push much higher and that's where human feedback becomes non-negotiable.


## Tooling has improved, but task complexity is outrunning it


Jesse made a sharp observation when asked whether tooling is currently a bottleneck or an accelerator: it's been reactive, and that's the problem.


A new type of model task emerges, tooling providers scramble to be first to support it, and the cycle repeats. The businesses that have fallen behind are the ones that optimised for today's problems while ignoring tomorrow's. The ones pulling ahead are building for where the frontier is heading, not where it currently sits.


The scale of the shift is stark. Years ago, the benchmark for a cutting-edge applied ML task was scanning invoices with 95% precision. Today teams are evaluating multi-hop reasoning across hundreds of pages of mixed-modality documents, asking complex inferential questions that go far beyond retrieval. Every modality (video, audio, LiDAR, autonomous agents) has followed the same pattern: model capability grows, task complexity follows, and the tooling layer scrambles to catch up.


Wei-Yin added a practical dimension: as the audience doing annotation changes, the tools need to reflect that too. Tracking not just the final output but the iterative steps along the way, such as how an annotation evolved and where decisions were made, becomes increasingly valuable as models learn from intermediate signals, not just end states.


## Rigorous human feedback is a design problem, not a headcount problem


When Martin asked what defines rigorous human feedback both panelists pointed to the work upstream of annotation.


Wei-Yin framed it as a rubric design problem. A good rubric isn't just about right and wrong. Rather, it encodes what you actually want the model to produce, including company-specific guidelines, tone, format, and domain conventions. And because language is inherently ambiguous in a way that code isn't, even the most carefully designed rubric will be interpreted differently by different annotators. That variance has to be tracked, measured, and used to calibrate annotators over time.


Jesse's take was more operational. The biggest failure mode he's seen isn't a bad rubric, it's teams that design a rubric, hand it off, and disengage. The guidelines given to human evaluators are often genuinely ambiguous, and if you're not willing to iterate on them based on what you hear from the people doing the work, you're setting yourself up to waste a significant budget. The most successful pipelines he's seen involve deep, ongoing partnership with the execution layer.


On the question of multiple teams with different ontologies contributing to the same data collection effort, both panelists acknowledged it's one of the harder operational problems in the field. Jesse's pragmatic advice: work with fewer vendors but build deeper partnerships, and give those partners enough context about your design principles that they can proactively flag inconsistencies before they compound.


## Expert evaluators are now the bottleneck


Five years ago, a $1M data budget covered simple labeling at cents per task, done by non-specialists in minutes. Today, a single task might require hours from a subject matter expert (a lawyer, a senior coder, a clinician) at hundreds of dollars a pop. The economics have shifted fundamentally, and many teams are still working with budget expectations that haven't caught up.


Jesse's advice: stop trying to optimise cost by compromising on evaluator quality for the tasks where quality is genuinely critical. Instead, get more principled about where you actually need SMEs versus where you can use lighter-touch approaches, add human-in-the-loop validation to catch errors before they propagate, and find a partner who is invested enough in your success to help you work through those trade-offs honestly rather than just taking the budget.


Wei-Yin's addition was practical: before you commit a significant budget to a data collection exercise, test whether the data you're collecting actually moves the metrics you care about. Run a small slice first. If it's not shifting the needle, the problem might be in the rubric design or the translation from human feedback to model signal.


## Process reward models are promising


The panel's most technically rich exchange came when Martin asked about process reward models and the "superhuman problem". The point at which models become capable enough that humans can no longer reliably evaluate their outputs.


Wei-Yin gave a clear explanation of the distinction: outcome reward models give a single score at the end of a task; process reward models score each intermediate reasoning step, steering the model toward correct logic before it reaches a conclusion. For complex tasks, multi-step coding, medical diagnosis, long-horizon reasoning, this granularity matters.


But he was candid about the limitations. Reasoning traces are messy. Models reflect, backtrack, and revise mid-thought in ways that make clean step boundaries hard to define. The more complex the reward shaping, the more opportunities for the model to find unintended ways to maximise reward without producing what you actually want a phenomenon the field knows as reward hacking.


His proposed direction: rather than trying to score every step of an opaque reasoning trace, train models to produce reasoning that is inherently human-readable, structured, interpretable, and verifiable at each step. Make interpretability a training objective.


The human data problem is outpacing compute


Asked to choose between a shortage of compute and a shortage of high-quality human feedback data as their bigger concern for the next 18 months, both panelists chose human data, though Jesse noted compute scaling has been more predictable than it gets credit for.


The core of Jesse's concern: the cognitive and domain demands placed on human evaluators are increasing faster than the infrastructure, talent pipelines, and tooling designed to support them. Two years ago you were evaluating single-modality outputs of a few thousand tokens. Today you might be evaluating multi-hop agentic workflows across mixed modalities, with tens of thousands of tokens and deep subject matter expertise required throughout.


Wei-Yin's framing was about timing and foresight: the harder challenge isn't just finding the right data, it's anticipating what the right data will look like before the frontier moves there. By the time a new model capability is well understood, the window to get ahead of the data problem has often already closed.


## The earliest warning sign your feedback pipeline is breaking


The session closed with a question for the practitioners in the audience: what's the first sign a human feedback pipeline is about to break as it scales?


Jesse's answer was simple and experience-hard: unexplained divergence. When your eval results plateau in a way that doesn't match your expectations, or start shifting in a direction that doesn't make intuitive sense, that's the signal. His advice, which he acknowledged is easier said than done, is to maintain a healthy obsession with the numbers.


Interested in how Encord supports human feedback and eval workflows at scale?[Speak to our team →](https://encord.com/book-demo/)


[< Previous FDA's New Predetermined Change Control Plan Rule for Radiological AI Software, Explained](https://encord.com/blog/fda-pccp-rule/)[Next > How to annotate video for ADAS and autonomous driving: A Technical Guide](https://encord.com/blog/adas-video-annotation-guide/)


## Get the data right.


300+ of the best AI teams in the world use Encord.


[Take a tour](https://encord.com/explore-product/)[Book a demo](https://encord.com/book-demo/)
