---
schema_version: "1.0.0"
document_id: "b9eacc7f428b7e2173b3974dd7d53d10205a376a31922cfc218b056946cb2402"
company_key: "rocket-companies-inc-class-a-common-stock"
company: "Rocket Companies Inc."
source_id: "rocket-companies-inc-class-a-common-stock-news-import-a92ea4897bd8"
canonical_url: "https://careers.rocket.com/blog/technology-and-product/autonomous-ml-experiments-model-tuning"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-24T00:08:45.498983+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:2f6eefb0afb5f1c438a5907c7ce23a6624c35f0d8362b0bd78bd2dc5edf5093b"
---

# What 200+ autonomous machine learning experiments taught us about the future of model tuning

# What 200+ autonomous machine learning experiments taught us about the future of model tuning


Author:


Abu Mahmud


Apr 15, 2026


•


8-minute read


Share:


What if you could go home on a Wednesday evening, and by the time you opened your laptop Thursday morning, an AI Agent had run over two hundred experiments on your existing ML model? Testing ideas you hadn't thought of, discarding failures, building on successes, and leaving you with a cleaner, better-performing model.


We tried it. And it worked.


At Rocket’s Data Science org, we gave a frontier AI agent a carefully written protocol and our model tuning framework, with instructions to keep experimenting and trying to beat our best model until we tell it to stop. Over the next twelve hours, the AI Agent autonomously ran 220+ experiments, improving our model's primary performance metric by roughly 3%. That's a meaningful gain for a system that had already been tuned with automated hyperparameter optimization.


This post is the story of how we set it up, what the AI Agent did overnight, and the surprising lessons we took away from the experience.


## The inspiration


At Rocket, one of our core ISMs is that we're obsessed with finding a better way. That obsession runs deep in our Data Science org, where we are always looking for new approaches to push our models further.


The idea of autonomous ML experiments was not new. Several teams and researchers in the AI community have been exploring autonomous experimentation loops and protocol-driven agent workflows. In early 2026, Andrej Karpathy open sourced a project called AutoResearch. It is a simple framework that lets an AI agent autonomously modify and train an LLM, evaluate the results, and keep iterating. As he explained, *“The core idea is that you are not touching any of the Python files like you normally would as a researcher. Instead, you are programming the program.md Markdown files that provide context to the AI agents and set up your autonomous research org.”* The AI agent acts as the researcher. It reads a protocol, designs experiments, runs them, logs results, and loops, all without human intervention.


It was compelling. However, it was built for a specific setup. The framework focuses on experimenting with a small but real LLM training setup on a single GPU. We asked a different question. Could we adapt this idea for a real-world ML model, such as the classification models that support our business processes?


The core insight was that Andrej Karpathy's framework was not really about LLMs. It was about the protocol. A protocol is a structured document that tells an AI agent exactly how to experiment, what constraints to respect, and how to evaluate success. The domain does not matter. The discipline of the protocol does.


So, we wrote our own.


## How we set it up


Our system had five components, and the design philosophy was simple.


**1. The Protocol Document:** A detailed markdown file that served as the AI Agent’s operating manual. It specified the experimentation loop (modify code → commit → train → evaluate → keep or revert), the primary metric to optimize, and a clear priority order for what to try (feature engineering before hyperparameter tuning, simplification before complexity). Crucially, it included one non-negotiable rule, which was “never stop experimenting until a human tells you to.”


**2. A Fixed evaluation harness:** The AI Agent could change almost anything (preprocessing, feature engineering, model architecture, hyperparameters), but the evaluation code and train/test split logic were read-only. This ensured that every experiment was measured on the same terms. The AI Agent couldn't game the metric. It could only genuinely improve the model.


**3. A results log:** Every experiment’s outcome was appended to a structured log. It included the metric score, a description of what changed, whether it was kept or discarded, and a link to the exact code through a git commit. This gave us a complete audit trail to review in the morning.


**4. Git-based version control:** Each experiment was a git commit. If an experiment improved the metric, the change was kept. If it regressed or crashed, the AI Agent reverted to the last known-good state. This made the process fully reversible. No experiment could corrupt the codebase.


**5. Hard time budgets:** Each training run had a strict wall-clock timeout. If a run exceeded the budget, it was killed and discarded. This prevented the AI Agent from getting stuck on expensive approaches and kept throughput high.


That was it. No custom infrastructure, no special frameworks. Just a well-written protocol, version control, and clear metric. We kicked it off in the evening and went home.


## What happened overnight


The AI Agent worked through the night on the ML model in roughly three phases.


### Phase 1: The obvious moves.


It started with what any experienced Data Scientist would try first. It swapped in alternative model architectures, adjusted core hyperparameters, and tested different encoding strategies for categorical features. Some of these worked: deeper trees and a slower learning rate produced early gains. Others, like switching to a different gradient boosting library, didn't transfer well and were quickly discarded.


By the end of this phase, the model was already meaningfully better. The low-hanging fruit had been picked.


### Phase 2: The creative (and mostly failed) middle.


This is where things got interesting. With the obvious ideas exhausted, the AI Agent started getting creative. It tried synthetic oversampling techniques for the minority class. It implemented a custom loss function designed for imbalanced data. It built ensemble methods that combined multiple models. It explored exotic feature encodings.


Most of these experiments failed to improve results, some of them spectacularly. Synthetic oversampling generated artificial data points that didn't reflect reality, cutting performance nearly in half. The custom loss function had subtle numerical issues that broke the model entirely. An ensemble approach where a simpler meta-model combined predictions from multiple base models collapsed under the class imbalance.


But the AI Agent didn't get frustrated. It logged each failure, reverted the code, and moved on. Every failed experiment narrowed the search space.


### Phase 3: An unexpected pattern.


Around midnight, with over a hundred experiments logged, the AI Agent surfaced a pattern that might have taken a human researcher weeks to notice.


It noticed that several individually unsuccessful changes (removing a preprocessing step here, changing an encoding strategy there, adding a specific interaction feature) suddenly worked when applied together. We came to call these synergistic changes. They are modifications that are neutral or even harmful in isolation but produce meaningful improvements when combined.


The winning model required seven specific changes applied simultaneously. No subset of six would match its performance. The AI Agent arrived at this by methodically rebuilding from a clean baseline, testing each change individually (where most appeared to fail), and then combining them based on a hypothesis about why they might interact.


This is, in our view, the most significant finding of the experiment on our model training setup. It suggests that the ML optimization landscape has hidden pockets of performance that can only be reached by coordinated changes, and that in our case, the brute-force throughput of an autonomous AI researcher is uniquely suited to finding them.


## Making sense of all the experiments


Once the experiments were done, we asked the AI Agent one more thing: write up what you did.


The agent produced a complete and detailed article of roughly 7,000 words that covered every phase of its experimentation. It walked through what it tried, why it tried it, what failed and why, what succeeded and why, and how the final winning configuration came together. It explained its reasoning at each decision point, documented the dead ends, and summarized the key takeaways.


The result was a research report that any data scientist on the team could pick up and immediately understand the full story of what happened overnight. No one had to dig through two hundred git commits or parse a results log. The agent had already done the analysis, connected the dots, and laid everything out in plain language. This turned out to be just as valuable as the model improvement itself.


## What we learned


After reviewing the full results log (220+ experiments, a 14% success rate, and a roughly 3% improvement to our primary metric) three takeaways stood out.


### Most experiments fail, and that's the point.


Eighty-six percent of the AI Agent’s experiments made the model worse or had no effect. That sounds like a terrible hit rate, but it's exactly how ML research works. The difference is throughput. A human researcher might run five to ten well-designed experiments in a day. The AI Agent ran over two hundred in twelve hours. At a 14% success rate, that's still thirty useful findings. More than most teams produce in a week.


And unlike a human, the AI Agents don’t carry the emotional weight of failure. There's no sunk-cost fallacy, no reluctance to abandon an idea you spent hours designing. Failed experiments are just data points.


### Synergies are invisible until you find them.


The best-performing model required seven changes working in sync, and no proper subset matched its results. When the AI Agent tried to apply these changes one at a time from the baseline, most appeared to hurt performance. A perfectly misleading result that would have caused a human researcher to abandon them.


The changes formed a coherent philosophy. Let the model see raw data with minimal preprocessing, encode categorical features efficiently, and provide a small number of domain-informed interaction features. Each piece supported the others. Removing the preprocessing step only helped because the encoding strategy had also changed. The interaction features only helped because the raw data was flowing through unscaled.


This kind of path-dependent, synergistic optimization is very difficult to uncover through manual experimentation. You'd have to test a combinatorial space of changes. Impractical for a human, but exactly what a tireless AI researcher can do.


### The protocol Is everything.


The quality of the AI Agent’s experimentation was directly shaped by the quality of the protocol document. The protocol didn't just tell the AI Agent what to optimize. It provided parameters for how to approach the problem. It established a priority order (feature engineering before hyperparameter tuning), a philosophy (simplicity over complexity), and guardrails (never modify the evaluation harness, always commit before running, always revert on failure).


When we reviewed the overnight results, the experiments that produced the best improvements were the ones most aligned with the protocol's guidance. The worst failures occurred when the AI agent strayed furthest from the protocol’s principles. It attempted complex multi-model ensembles when the protocol advised simplicity and implemented custom loss functions when the protocol suggested trusting the default objective.


Writing a good protocol is, we believe, becoming one of the more important skills in AI-assisted Data Science.


## What this means for data scientists


The goal was never to take humans out of the process. It was to change what our Data Scientists at Rocket spend their time on. Instead of manually writing codes and running experiments, our team focused on:


- **Designing the protocol:** deciding what the AI Agent should try, what constraints to enforce, and what "better" means
- **Interpreting results:** understanding why certain changes worked, not just that they worked
- **Making strategic decisions:** knowing when to stop exploring and start exploiting, when to pivot direction, and which combinations to investigate


The AI Agent handled the grind. It ran hundreds of train-evaluate-log cycles that constitute the bulk of ML experimentation time. It ran all night without fatigue, maintained perfect records, and surfaced patterns and improvements that our team could then reason about.


We see this as an example of a broader shift in the data science space, driven by teams and researchers around the world. Data science tools are evolving from notebooks and experiment trackers to protocols and autonomous agents. In many ways, this begins to resemble the next evolution of hyperparameter tuning, moving faster through the search space while expanding what can be explored.


Previously, much of a data scientist’s intuition went into designing the grid, selecting candidate models, and tuning setups within fixed constraints. Now, that intuition shifts toward thoughtfully designing protocols, defining how an agent explores, what it is allowed to change, and how it learns from prior experiments. The teams that learn to write effective protocols, design good sandboxes, and interpret AI-generated experiment logs will have a meaningful advantage.


We're continuing to develop this approach at Rocket, and we're excited about where it leads.


### Abu Mahmud


Abu Mahmud is an Associate Data Scientist at Rocket Innovation Studio, where he helps build intelligent systems. His work spans predictive modeling, simulations, and designing systematic experiments. As a core principle, Abu believes that doing good science requires strong engineering foundations that make meaningful experimentation possible.


## Related Resources


[3-minute read Speed of development using AI Discover how Rocket® leverages AI to transform every meeting into actionable insights, driving business value, productivity, and knowledge management. Read more](https://careers.rocket.com/blog/technology-and-product/speed-of-development-using-ai)[10-minute read How we made AI a first-class engineering citizen — and sustained 3.5x productivity How Rocket sustained 3.5x productivity by treating AI as a full engineering contributor — with the architecture, testing, and guardrails our clients deser... Read more](https://careers.rocket.com/blog/technology-and-product/ai-first-class-engineering-citizen)[7-minute read Sustainable AI: Balancing innovation and eco-responsibility Explore the environmental impact of generative AI and discover practical strategies for using technology responsibly while minimizing carbon footprint and resou... Read more](https://careers.rocket.com/blog/technology-and-product/sustainable-ai)
