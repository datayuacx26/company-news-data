---
schema_version: "1.0.0"
document_id: "b9ce5127b9763267cd21a959bb003cc305bd8d0aba4f2ff8c43ce6e1058ea211"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/2026-07-20-imbue-catalyst-nanochat"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:33fb6bd654b42e9519a9249590fc2e10962104eee31dc7466158e3ed26d8ddeb"
---

# Automating AI model research with evolution

## **Key takeaways**


- We’re[open-sourcing Imbue Catalyst](https://github.com/imbue-ai/catalyst) , an AI tool for computational research and scientific discovery
- Our evolution-based optimizer improves nanochat LLM performance 3x further than a regular AutoResearch agent


## Introduction


Recent model and harness improvements have enabled practical use cases of “self-improvement”: AI systems that iteratively improve their own foundations.


Approaches such as[Darwin Godel Machine](https://arxiv.org/abs/2505.22954) focus on improving a system’s harness, while others optimize the training recipes, architecture choices and algorithms behind the models themselves ([Andrej Karpathy’s AutoResearch](https://github.com/karpathy/autoresearch/) ,[Recursive’s “First Steps Toward Automated AI Research”](https://www.recursive.com/articles/first-steps-toward-automated-ai-research) ). Even though many of these results have been on small “toy” models, it is becoming clear that the next generation of LLMs is already being built with the help of today’s AI systems (e.g.[Anthropic “When AI builds itself”](https://www.anthropic.com/institute/recursive-self-improvement) ).


Today, we’re announcing[Imbue Catalyst](https://github.com/imbue-ai/catalyst/) . Catalyst is a research tool that uses evolution-inspired methods to help with the following research tasks:


- Optimize a piece of code, algorithm, or model with respect to a given metric
- Find solutions that satisfy certain programmatic verification criteria
- Discover explanations for computationally reproducible phenomena
- Assist with reviewing, formalizing, and editing theories in computational research fields


We believe that AI research works best as an open, collaborative effort. Hence, we’re releasing Imbue Catalyst under an AGPL-3.0 license. We’re looking forward to seeing what you’ll discover with it!


In this post, we’ll use Catalyst to optimize[nanochat](https://github.com/karpathy/nanochat) , a small transformer language model trained from scratch, and dive into the mechanisms that allow Catalyst to continue discovering improvements well after regular coding agents level off.


## Nanochat auto optimization


Andrej Karpathy first proposed the[AutoResearch](https://github.com/karpathy/autoresearch/) setup, in which a coding agent is prompted to improve the training setup for a small transformer language model ([nanochat](https://github.com/karpathy/nanochat) ). It is asked to do so by iteratively running experiments, and using git to keep track of its progress.


The setup starts out with a training script that runs for exactly 5 minutes on a single H100 GPU. The agent is free to change the model architecture, training recipe and hyperparameters in order to minimize the resulting BPB (bits per byte) metric on a validation dataset, as long as it doesn’t exceed the 5-minute training time budget.


Nanochat training optimization is an example of what we call a *verifiable goal* in Catalyst. Verifiable in this context means that you can use a program to measure and/or verify how well a candidate solution achieves the goal at hand. In this case, the verifier is measuring the BPB metric of the resulting model.


We compare Imbue Catalyst’s verifiable goal solver, ”Catalyst (Evolution)”, against the following reference points:


- Plain AutoResearch. We show the “mean of 3 runs” data reported by[Weco AI](https://www.weco.ai/blog/autoresearch-vs-classical-hpo) .
- Recursive Superintelligence’s[recently reported results](https://www.recursive.com/articles/first-steps-toward-automated-ai-research) . With two caveats:


- Recursive switched to a different GPU (B200) part-way through their run. We only include the first part of their run that ran on the original H100 GPU.
- Recursive did not report the number of experiments in their blog post, but rather reported cumulative wall-clock time. It is unclear from their post whether their system executed multiple experiments in parallel, and what the typical duration of each experiment was. Thus, our x-axis scaling of their results is estimated and might not correspond to the true experiment efficiency of their system.


- For ablation purposes, we also compare against a simplified “linear” configuration of Catalyst’s solver. In this configuration, the evolution-based mechanisms are disabled. All other factors (base LLM, hardware, prompts, etc.) are kept identical to our main result to allow for an apples-to-apples comparison.


** Number of experiments (x-axis) is estimated for Recursive’s run*


Catalyst’s evolution-based solver significantly outperforms both the original AutoResearch and Catalyst’s own linear ablation baseline.


In the measured run, Imbue Catalyst achieved a val_bpb score of 0.9361. Notably, when we stopped the run after 340 experiments, val_bpb had not plateaued yet, suggesting that further improvements could be possible given enough time and compute budget.


Our solution also surpassed the best result achieved by Recursive’s reported H100 run, but it is uncertain whether their solution would have continued improving if they had kept running on the H100 for longer. We would also like to re-emphasize that the number of experiments in their run (x-axis) is unknown, and, for all we know, their solution could be more efficient than ours in reaching a given val_bpb.


## Why linear agents get stuck


How does our evolution-based solver outperform a single agent by such a big margin?


A smoking gun can be found in the traces from the agents involved in our linear run. Here is one agent’s comment:


> “
>
>
> Given the program state — all mechanistic axes closed\[…\] — the highest-value next step is \[…\] a record-capable standalone seed-97 db128 re-roll of the byte-identical record graph. This is \[…\] the only record-advancing action. ”


In other words: The agent got into a state where it concluded that all optimization ideas had been exhausted. At that point, it resorted to “re-rolling” the same setup repeatedly with varying random seeds. Once it reached this state, it seemed unable to “zoom back out” and reconsider whether any other angles for attacking the problem were available.


In our research, we have observed this phenomenon quite frequently: Today’s frontier LLMs can struggle with revising earlier conclusions and “going back to the drawing board.” Rather, they suffer from something akin to tunnel vision, accompanied by a “hypothesis collapse” that prevents them from considering alternative explanations of the data.


When agents have access to persistent memory, this behavior can be observed even when CoT reasoning traces and the overall agent context are cleared in between turns, as is the case for our harness.


The problem is further exacerbated by another weakness in current models: Models often show confirmation bias and perception errors when interpreting experiment results. We’ve seen models conclude that an experiment’s result plot was conclusively in support of their prior hypothesis, even when a human review made clear that the plot either didn’t support the hypothesis at all, or contained clear indicators pointing to a faulty experiment setup (e.g. insufficient number of training steps to allow for a particular conclusion).


## Escaping dead ends by evolving interpretations


To combat these issues, Imbue Catalyst implements ideas inspired by Darwinian evolution. In our previous work, we’ve investigated how[LLM-based evolution can be used for code optimization](https://imbue.com/blog/2026-02-27-darwinian-evolver) . Catalyst builds on these ideas, but makes two enhancements:


- It removes the need for hand-written mutation and scoring functions. The optimization problem can be specified as natural language, and Catalyst uses coding agents to translate the specification into an appropriate verifier.
- We add the concept of “interpretation strands”, paired with experiment sharing. This combination allows us to significantly increase experiment efficiency in the evolution process, making it possible to apply evolution to problems that require expensive and/or long-running experiments to gather data.


An interpretation strand represents an agent’s current interpretation of its observed experiments and literature searches. Each strand additionally summarizes the agent’s current research roadmap - a list of open questions, bets and ideas for how the research task can be advanced further.


In Imbue Catalyst, the interpretation strand is the primary object of evolution: We maintain not just one, but an entire population of interpretation strands at any given point in time.


Initially, we start with a small number of empty strands, all with the same initial fitness scores.


From this starting point, we proceed through a sequence of iterations. Iterations alternate between two modes: (1) **data gathering** and (2) **integration & scoring** . Typically, most iterations are data gathering iterations. For nanochat, we use a ratio of 9:1, i.e. every 10th iteration is of the integration & scoring type.


Data gathering iterations work as depicted above: First, we sample


strands from the population. Sampling is weighted by each strand’s current fitness score, using the approach from our[Darwinian Evolver framework](https://imbue.com/blog/2026-02-27-darwinian-evolver) . We then ask separate agents to each propose a single next experiment, one per sampled strand. The proposals are ranked based on their expected research impact, and we select the top


for execution in the current iteration.


Once the experiments have been completed, their results get presented to a superset of the initially sampled strands, extended by an additional fitness-weighted sampling from the population. Separate agents are asked to update each strand’s interpretations given the new experiment results.


Every few iterations, we perform the additional knowledge integration & scoring steps:


1. **Knowledge integration:** A subset of strands is asked to integrate their recent observations back into a “theory” - a consolidated snapshot of their current understanding of all data seen so far. As part of this process, the agents are also asked to revise their respective research roadmaps.
2. **Branching:** For some of the integration targets, we specifically request the agents to branch off a second alternative continuation of their current theory. The agents are encouraged to consider alternative interpretations of the data and to come up with a distinct, often bolder, research roadmap. This branched theory forms the beginning of a new interpretation strand, which gets added back to the population.
3. **Fitness scoring:** All newly integrated and/or branched strands get evaluated along two dimensions:


1. An agent for each strand is asked to propose a solution candidate for the research problem at hand, using its best understanding to that point. Those solutions are evaluated, e.g. by executing them and observing their val_bpb performance. A separate review agent also checks for any indications of reward hacking or adherence issues with the user’s research specifications. Intuitively, we expect that an interpretation strand that has a “more correct” understanding of the data will also be able to produce a better solution candidate, and conversely, a better performing solution candidate is taken as a proxy to indicate a more accurate, complete and hence more desirable interpretation strand.
2. Additionally, we ask an agent to review the research roadmaps of each strand and rank the novelty of each.
3. The overall fitness score for a strand is a combination of the performance of its solution candidate and the novelty of its research roadmap.


## The experiment efficiency vs. diversity tradeoff


Our approach introduces diversity between strands through three factors:


1. The interpretation strand branching described above. This is the most direct, and typically strongest driver of diversity, but it only happens during integration & scoring iterations.
2. Experiment visibility sampling: Early in the research process, when the population is small, each result is observed by every interpretation strand. However, as the population grows, our process begins presenting a given result to only *a random subset* of interpretation strands. This means that over time, each strand will have observed an overlapping, but slightly different subset of experiments.
3. LLM token sampling stochasticity: Last but not least, even with everything else being equal, variations between strands would arise over time simply due to the random token sampling in the underlying LLMs.


The experiment visibility sampling comes with a notable tradeoff: in many research scenarios, experiments are the main driver of runtime and resource cost. While nanochat experiments are *relatively* fast (on the order of minutes), time and compute requirements to run a single experiment can be substantial in other deep learning settings.


Thus, we should maximize the utility we get out of any given experiment. This would suggest that experiment results should always be presented to *all* active interpretation strands, not just to a subset of them. However, a single experiment can also be misleading! The experiment might be flawed and lead the observing strands to draw incorrect conclusions that hamper their future progress.


In Catalyst, we expose the number of additional strands that observe each given experiment as a hyperparameter, called “Extra Interpretations.” This allows you to trade off between experiment efficiency and research robustness/diversity based on your needs. For the nanochat run, we used the default value of 3.


## What’s next?


When building Imbue Catalyst, we did not stop at verifiable goals. Many scientific questions are *not* verifiable through a simple measurement or piece of code. In an[upcoming post](https://imbue.com/blog/2026-07-20-imbue-catalyst-theory-discovery) , we’ll look at how we extended the evolution approach to discover *explanations* : developing scientific theories that tell us *why* a given phenomenon occurs.


**In the meantime, we invite you to try out Imbue Catalyst for your optimization problem:[https://github.com/imbue-ai/catalyst#getting-started](https://github.com/imbue-ai/catalyst#getting-started)**


Please keep in mind that Catalyst is an experimental tool. AI-assisted research is in its early stages. Its results may not always be what you have in mind - don’t expect miracles just yet!


A few tips for getting the most out of Imbue Catalyst:


- The default workflows perform a certain number of solver iterations. If the workflow completes but hasn’t produced the desired result yet, you can always add additional loops by using the “Add step” button at the bottom.
- If you have any existing context or experiment code, consider[setting up a template](https://github.com/imbue-ai/catalyst/blob/main/src/docs/quickstart.md#environment-templates) . And if you find a template that works well for your area of research: why not open a pull request at[https://github.com/imbue-ai/catalyst-templates](https://github.com/imbue-ai/catalyst-templates) to share it with the community?
- Catalyst imposes a default time limit of 30 minutes on any experiment. Set the` CATALYST_EXPERIMENT_TIMEOUT_SECS` environment variable if your research task requires longer experiments.
- Always verify research results and check for reward hacking and falsification attempts.
- Last but not least, Catalyst can be quite token-hungry! Luckily, many model subscription plans can be used. Read this[model/cost overview](https://github.com/imbue-ai/catalyst/#supported-models--estimated-costs) for details.


We’d love to hear about your research ideas and experiences with Imbue Catalyst! Please find our[discussion board on GitHub](https://github.com/imbue-ai/catalyst/discussions) to post questions, ideas and feedback. Or follow us on[X](https://x.com/imbue_ai) or[Bluesky](https://bsky.app/profile/imbue-ai.bsky.social) for updates.


—


## Appendix: Reproduction details


**Nanochat auto optimization**


- Catalyst commit hash:` a53221e82c4209fb3c91f7c5d0d87e4dc2cca4ff`
- “Solve Verifiable Goal (Evolution)” workflow


- Verifiable goal: “Come up with improvements to the[train.py](http://train.py/) setup ([train.py](http://train.py/) provided as a starting point) to get the lowest possible val_bpb value.”
- Verification instructions: “Running the training script will output its val_bpb value at the end.”
- Guidance:


```text
text
```


- Evolve iterations: 140
- Scoring interval: 10


- Template:[autoresearch](https://github.com/imbue-ai/catalyst-templates/tree/main/autoresearch)
- Claude Code harness with Opus 4.8 (Xhigh)
- All other settings at their defaults


The linear ablation run used identical settings, except for:


- “Solve Verifiable Goal (Multi Strand)” workflow


- Number of Strands: 1
- Max iterations: 260
- Integration interval: 20
