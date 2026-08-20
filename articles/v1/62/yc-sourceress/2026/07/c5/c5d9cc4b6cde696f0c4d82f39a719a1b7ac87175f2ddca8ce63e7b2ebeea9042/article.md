---
schema_version: "1.0.0"
document_id: "c5d9cc4b6cde696f0c4d82f39a719a1b7ac87175f2ddca8ce63e7b2ebeea9042"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/2026-07-20-imbue-catalyst-theory-discovery"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:e2e903b071568818b6ae86d275f626c6ad7e82c8d38e1a0bde6b45f6a41b5ddb"
---

# Autonomous theory discovery

## **Key takeaways**


- Our AI research tool[Imbue Catalyst](https://github.com/imbue-ai/catalyst) can find mechanistic explanations for deep-learning phenomena
- Combining adversarial feedback with an evolution-inspired optimizer loop enables autonomous hypothesis exploration, validation and selection
- Try it on your research questions today!


## Introduction


There has been a lot of recent interest in using LLMs to help in the discovery of new scientific insights. From systems that extract new insights from existing data ([Kosmos](https://arxiv.org/abs/2511.02824) ), to multi-agent setups for hypothesis generation and evaluation ([DeepMind’s Co-Scientist](https://www.nature.com/articles/s41586-026-10644-y) ), to agentic systems that automate the entire ideation, research, and paper writing cycle ([Sakana’s AI Scientist](https://arxiv.org/abs/2408.06292) and[AI Scientist II](https://arxiv.org/abs/2504.08066) ), a lot of progress has been made in the last year alone.


We just open-sourced[Imbue Catalyst](https://github.com/imbue-ai/catalyst/) , an AI-driven research tool that uses evolution-inspired methods to help with computational research tasks.


In a[previous post](https://imbue.com/blog/2026-07-20-imbue-catalyst-nanochat) , we showed how Imbue Catalyst can discover and autonomously combine optimizations for training nanochat, a small transformer language model. In this post, we move beyond the realm of metric-driven optimization, and see if we can apply evolutionary search to an even more ambitious target: discovering novel, mechanistic explanations for a given phenomenon from the field of deep learning.


## Case study: Neuron clustering during MLP bifurcation


A common phenomenon when training neural networks is bifurcation: distinct transition points at which a lower-rank weight distribution suddenly “splits” into two separate directions (see e.g.[Yang 2026](https://arxiv.org/abs/2605.24057) ).


This phenomenon can be observed even in very shallow networks and with simple data distributions. You can head over to this[Shallow MLP Gym](https://jamiesimon.io/shallow-mlps/) and observe it right now in your browser. Just use the following settings and click “start” to run the training:


*Target function:` x\[1\] + x\[2\] + abs(x\[1\]) + abs(x\[2\])` , input dimensions: 2, width: 500, init scale: 10^-6, learning rate: 10^-3, batch size: 10, nonlinearity: ReLU. Check the “scatterplot” visualization.*


You should see the inner layer’s weights behave similarly to this during training:


You might notice that the split is not instantaneous. Rather, you can observe discrete lines, or “angular clusters”, of neurons moving off from the initial diagonal direction. It is not surprising that different neurons split off at different speeds, given their random initializations. However, we weren’t sure why the *clusters* of neurons were forming - why are neurons moving in groups, rather than individually?


### Probing the cause of the phenomenon


We used Imbue Catalyst to investigate this phenomenon. The Catalyst agents autonomously set up experiments to probe the phenomenon, and surfaced key insights about its behavior. One of the agents’ early observations was that the clustering phenomenon was sensitive to hyperparameter choices, most importantly, the initialization scale, learning rate, and batch size.


Here you can see what happens when we increase the batch size:


The clusters have largely disappeared! Most neurons are moving independently now.


Based on this insight, we could sketch out a rough idea of what might be going on here. However, our explanation was still incomplete and in need of validation.


In this video, you can see how we used Imbue Catalyst to fill in the missing pieces, rewrite our draft into something much more rigorous, and test the validity of the resulting theory by attempting to falsify it.


### Towards autonomous theory discovery


The next question was whether we could build an AI system to replicate this result *without* the need to draft an explanation sketch first. In other words: Can we build a system that goes directly from a description of the phenomenon to a mechanistic explanation?


If you describe the clustering phenomenon to the current generation of frontier models, you get a large variance of explanation attempts. Some models, such as Gemini 3.5 Flash, get on the right track some of the time, while others (e.g. Claude Opus 4.8) practically never even discover the connection to batching on their first attempt. None of them appear able to one-shot a complete explanation in one go.


To address this lack of robustness, we developed a *theory evolver* to iteratively explore different hypotheses and build up increasingly powerful and correct explanations.


At a high level, our evolver works by maintaining a population of competing theory candidates. Each theory candidate consists of an explanation hypothesis and the associated mathematical and/or empirical evidence to support it. Theory candidates in the population get reviewed and then ranked against each other to assign each one a fitness score. During the evolver’s main loop, we repeatedly sample


theory candidates from the current population, make an attempt at improving them, and repeat the review and scoring process before adding the new theory candidates back to the population. The sampling is weighted by each candidate’s current fitness score, causing improvement effort to be directed to the most promising candidates.


Our theory evolver combines three key ideas into a single, integrated loop:


**1. Adversarial refinement**


LLMs can suffer from confirmation bias and hallucination. We counteract this by deploying separate, independent review and refinement agents.


1. Each theory candidate is subjected to a team of dedicated review agents. Those agents operate in their own isolated contexts and have a single purpose: finding flaws in the theory candidate. They do so by (1) attempting to actively falsify any statement made in the theory and (2) looking for gaps in its explanations.
2. Subsequently, a separate team of refinement agents may try to improve & strengthen the theory candidate by addressing the flaws and gaps found by the review agents.


**2. Encouraging hypothesis diversity**


To avoid getting stuck with the first explanation attempt, we encourage the parallel exploration of several diverse hypotheses.


Initially, we ask an agent to perform a broad exploration of the phenomenon and then select a number of distinct explanation ideas. Each idea is sent to a dedicated theory writing agent, which tries to flesh the idea out into a complete theory candidate. All of the resulting theories are included in the initial theory candidate population, thus starting out with a diverse range of “seed” candidates.


When a theory candidate is sampled for improvement, we randomly apply one of the following three improvement operations:


1. **Refine:** Incrementally refine the current theory by addressing flaws and gaps identified by the review agents, as described above (most common)
2. **Write-different:** Develop a new theory that is *fundamentally different* from *all existing candidates sampled in the current iteration* . This encourages the exploration of completely new ideas that aren’t already present in the population.
3. **Streamline:** Shorten the existing theory candidate to remove clutter and trim it down to its key insights. Such a streamlined theory offers a less constrained starting point for subsequent refinement.


Last but not least, experiment results are never shared horizontally between different theory lineages. This increases diversity, but comes at the cost of some experiments having to be repeated in different lineages. We may provide an option to adjust this behavior in a future version of Catalyst to accommodate different efficiency/diversity targets.


**3. Evolution-inspired resource allocation**


Theory candidates compete with each other through a sophisticated ranking & scoring mechanism (see Appendix 2 for details). A fitness-based sampling approach is used to balance efficiency with maintaining diversity, based on our previous[Darwinian Evolver](https://imbue.com/blog/2026-02-27-darwinian-evolver) framework.


The theory evolver is available in Imbue Catalyst through the “Develop Theory (Evolution)” workflow.


### Evolving explanations for the phenomenon


We ran 5 iterations of the theory evolver to see if it would move towards a correct explanation for the bifurcation clustering phenomenon.


To verify the robustness of our solver, we repeated this experiment with different models. Gemini 3.5 Flash, Claude Opus 4.8, and Claude Sonnet 4.6 all converged towards the right explanation at the end of this process. This worked despite many of the initial theory candidates not even directionally capturing the correct mechanism. To test even weaker models, we also performed a run using Claude Haiku 4.5. However, this run did not find a valid explanation within the tested 5 iterations.


The following graph shows the theory candidate lineage in the Claude Sonnet 4.6 run:


Each node corresponds to a theory candidate generated in this run. Nodes are colored based on their final fitness scores, and edges indicate the inputs to the improvement operation that generated the theory candidate. We manually labeled all theory candidates for whether they *directionally* represent the right mechanism (regardless of specifics), and marked those with a plus in this graph. The highest-scoring theory candidate of this run is marked with a star symbol.


Here you can find the[highest-scoring theory candidate](https://cdn.sanity.io/files/dg1xe82e/production/0fdecf04428222bfabbf099e76c590ea8f1531fc.pdf) from the Claude 4.6 run. This process was completed with zero human guidance besides the initial phenomenon description.


For reference, we also provide the[theory resulting from the manual theory draft completion workflow](https://cdn.sanity.io/files/dg1xe82e/production/316edab7f1e6797213c6321494de331448082397.pdf) shown in the video above.


The automatically generated theory is still rather rough in its presentation and contains a handful of remaining errors and gaps. However, it captures the correct mechanism. By manually applying additional theory refinement and streamlining steps, the theories could easily be improved further.


## Limitations


—


*⚠️ Imbue Catalyst is **not** a fully autonomous research scientist. Despite the self-review steps built into Catalyst, hallucination, data falsification, and reward hacking remain unsolved challenges.*


*Hallucinations can be hard to distinguish from legitimate results.*


*Catalyst may present results that already exist in the literature as its own. Always cross-check results against the existing literature.*


—


AI-based research agents still face many shortcomings today. Some limitations of Imbue Catalyst in particular:


- Current agents struggle with establishing new frameworks, formalisms or abstractions that could otherwise help with structuring a problem space and enabling broader generalization. This lack of abstraction may limit the degree to which the system can achieve truly novel insights that aren’t already within reach of existing frameworks.
- A lot hinges on “asking the right question”: Getting good results requires a sufficient degree of specificity from the user when describing the research prompt. Imbue Catalyst comes with interactive steering support to assist in refining an ambiguous prompt: Throughout the research process, the system suggests prompt clarifications that the user can accept with a click. However, the user still needs to have a good understanding about which research direction(s) should be followed.
- Similarly, very open-ended or broad research questions are still unlikely to yield useful results.
- Imbue Catalyst’s workflows can be *very* token-hungry. We have not made any investments into optimizing token efficiency or improving context prefix caching hit rates.
- Hallucination, falsified results, and reward hacking remain unsolved problems. While Catalyst uses specialized agents and prompts to check for such issues, those checks are by no means complete.
- Catalyst does not consistently cite prior work. It may present results that already exist in the literature as its own.
- Last but not least, we strongly advise against submitting AI-generated research artifacts for publication. Catalyst’s outputs are intended as a starting point that requires further investigation and reproduction, and never as a final research result.


## What are you going to discover?


Do you have a research question you’d like to probe? Maybe a deep-learning phenomenon whose mechanism isn’t fully understood? A hypothesis that you’d like to validate?


**You can try out Imbue Catalyst today:[https://github.com/imbue-ai/catalyst#getting-started](https://github.com/imbue-ai/catalyst#getting-started)**


Please keep in mind that Catalyst is an experimental tool. AI-assisted research is in its early stages. Its results may not always be what you have in mind - don’t expect miracles just yet!


A few tips for getting the most out of Imbue Catalyst:


- Start with a narrow, well-defined problem. Only then try your luck with more open-ended questions.
- Keep an eye on the Research Summary Reports to catch misalignment early. You can steer the remainder of the ongoing workflow at any time by adding guidance instructions.
- The default workflows perform a certain number of solver iterations. If the workflow completes but hasn’t produced the desired result yet, you can always add additional loops by using the “Add step” button at the bottom.
- If you have any existing context or experiment code, consider[setting up a template](https://github.com/imbue-ai/catalyst/blob/main/src/docs/quickstart.md#environment-templates) . And if you find a template that works well for your area of research: why not open a pull request at[https://github.com/imbue-ai/catalyst-templates](https://github.com/imbue-ai/catalyst-templates) to share it with the community?
- Catalyst imposes a default time limit of 30 minutes on any experiment. Set the` CATALYST_EXPERIMENT_TIMEOUT_SECS` environment variable if your research task requires longer experiments.
- Always verify research results and check for missing citations.
- Last but not least, Catalyst can be quite token-hungry! Luckily, many model subscription plans can be used. Read this[model/cost overview](https://github.com/imbue-ai/catalyst/#supported-models--estimated-costs) for details.


We’d love to hear about your research ideas and experiences with Imbue Catalyst! Please find our[discussion board on GitHub](https://github.com/imbue-ai/catalyst/discussions) to post questions, ideas, and feedback. Or follow us on[X](https://x.com/imbue_ai) or[Bluesky](https://bsky.app/profile/imbue-ai.bsky.social) for updates.


To close with a quote from[Anthropic’s article](https://www.anthropic.com/institute/recursive-self-improvement) :


> “
>
>
> *The comparative advantage of humans as of right now is still in seeing the bigger picture and thinking beyond the confines of the immediate task.* ”


—


## Appendix


### Appendix 1: Selecting *good* theories


For research that is looking for a solution satisfying some verification criteria, defining what a “good” result looks like is fairly straightforward: Any solution that passes the verification criteria could be considered a good result. But what if there isn’t a single, formally defined criterion for evaluating solutions? What if what we’re looking for is a “good” explanation for a given phenomenon?


It’s not clear to us whether there is a single answer to the question of what makes a “good” theory. However, we can consider some heuristics, depending on the specific field of research at hand.


For mathematics, Barkeshli et al. investigate this question as part of their 2026 paper “[Artificial Intelligence and the Structure of Mathematics](https://arxiv.org/abs/2604.06107) ” in section “3.4 Universal importance measures: Theorems and willow trees”. A key realization is that it’s quite easy to come up with new mathematical theorems. However, the vast majority of those theorems are not going to be “important”. An important theorem in mathematics appears to be one that “unlocks” a large number of further (important) discoveries. We’d therefore be looking for heuristics that *predict* whether a given theorem will yield further insights down the road.


When it comes to deep-learning theory, similar considerations apply: strictly speaking, any phenomenon that has been observed in a given neural network can be “explained” trivially: the explanation of the phenomenon is simply the full training and/or inference program that produced the phenomenon, together with any model weights, input data, and/or initialization seeds. Applying the model means running the program through a computer and observing its outputs. Such a “model” of the phenomenon makes perfect predictions within its scope. However, it is not a *useful* model in any meaningful way: it doesn’t allow any generalization or transfer to different settings. To a human, there’s no deeper insight or understanding to be had from such a model.


### Appendix 2: Theory scoring dimensions


For the theory evolver in Imbue Catalyst, we developed a combination of scores to estimate the quality and usefulness of a given theory. The combination of scores serves as the fitness score: Theory candidates with higher scores are sampled more frequently, and more resources are dedicated to further improving them. We know that our scoring methodology is incomplete, but we see it as a meaningful starting point. Overall, our scores cover the following high-level attributes of a theory:


- Correctness: Is the theory mathematically sound, and does it make correct, empirically confirmed predictions?
- Explanatory power: How completely does the theory explain the phenomenon at hand, and how well does it generalize?
- Alignment: Is the theory short enough to be comprehensible? Does it follow all user instructions? For example, a user might specifically request a mathematically derived theory, rather than one that is only supported by empirical observation.


Our scoring method assesses different dimensions of a theory candidate on a scale between 0-1. Some dimensions are measured on an absolute scale on a per-candidate basis. For other dimensions, we use a ranking-based scoring methodology, where a candidate’s score represents how well it performs *relative to* the other theory candidates that are being scored at the same time.


For each dimension, the theory evolver has a corresponding improvement mechanism that actively drives future theory candidates towards better performance.


**Dimension 1: Prediction accuracy & coverage**


**Type:** Correctness / Explanatory power | Ranking


**Motivation:** A good theory should make accurate predictions.


**How it’s being measured:** We source past experiments across the theory candidates that participate in the current scoring round, including from any falsification attempts made over those theories. An LLM selects up to ten experiments from this pool that are most relevant to the phenomenon. Next, dedicated agents are each given one of the theory candidates and asked to make predictions for the ten experiments. The prediction agents see the experiment setups, but do not have access to the *results* of the experiments. They can also output NO_PREDICTION if the given experiment is out of scope for the given theory candidate. Finally, a new agent receives the predictions of all participating theories, together with the *actual observed* experiment results, and is asked to rank the predictions from least to most accurate. We compute two scores from this result: The *prediction accuracy score* for a theory candidate is computed as a weighted average over the accuracy rankings of its predictions, and the *prediction coverage score* is derived from the fraction of experiments for which the theory made any prediction at all.


**Mechanism of improvement:** The theory review step generates falsification and expansion reviews: A falsification review attempts to falsify each statement in a given theory, and addressing it will typically improve the *accuracy* of predictions made by that theory. The expansion review on the other hand actively suggests ways for generalizing or extending the coverage of a theory. A subsequent theory refinement step tries to improve a theory candidate by addressing both types of reviews.


**Dimension 2: Soundness**


**Type:** Correctness | Absolute


**Motivation:** Besides making accurate predictions, we also want to make sure that the way that a theory *derives* those predictions is sound. This is especially critical for theories that use mathematical proofs.


**How it’s being measured:** During theory review, agents attempt to find flaws in the theory candidate and emit falsification reviews. A scoring agent reads the falsification reviews to check whether any mathematical or logical flaws were raised. It then assesses the severity of those flaws, and produces an overall soundness score.


**Mechanism of improvement:** A theory refinement step will try to address any flaws raised in a falsification review, including any soundness issues.


**Dimension 3: Completeness of explanation**


**Type:** Explanatory power | Ranking


**Motivation:** A fully correct theory is only useful to us if it also *sufficiently* *explains* the specific phenomenon that we’re researching.


**How it’s being measured:** An agent is asked to review all theories participating in the current scoring round, and ranks them based on how complete their explanations for the target phenomenon are. This ranking gets converted into a score.


**Mechanism of improvement:** An agent within the theory review step is tasked with raising gaps in a given theory candidate’s explanation of the target phenomenon. It outputs any gaps in the form of an “adherence review”. A subsequent theory refinement step will attempt to address those gaps.


**Dimension 4: Length**


**Type:** Alignment | Absolute


**Motivation:** All else being equal, we prefer shorter theories over longer ones. Furthermore, redundancy and irrelevant additions that aren’t core to the research question at hand can make a theory hard to read, understand, and review - both for the human user, but also for the other agents within Catalyst.


**How it’s being measured:** An agent is prompted to determine the line number range of the “main part” of the given theory, ignoring any introduction and appendix sections. Then, we count the number of words


in the theory’s main part and calculate the length score as


.


**Mechanism of improvement:** We may randomly select a “streamlining” step for improving a sampled theory candidate. This step attempts to shorten the given theory down to its core essence. The probability for selecting the streamlining operation in the evolver loop increases in proportion to 1 - length_score, causing longer theories to get streamlined more frequently.


**Dimension 5: Instruction adherence**


**Type:** Alignment | Absolute


**Motivation:** Make sure that any guidance and criteria provided by the user are followed. (e.g. “make sure to use method X”, or “focus on a mathematical proof, rather than empirical data fitting”)


**How it’s being measured:** An agent is asked to review the user’s research guidance, and check whether a given theory candidate adheres to each requirement.


**Mechanism of improvement:** The adherence review generated during theory review includes a list of adherence gaps. A subsequent theory refinement step will attempt to address those gaps.


### Appendix 3: Reproduction details


**Explaining neuron clusters during MLP bifurcation**


- Catalyst commit hash:` a53221e82c4209fb3c91f7c5d0d87e4dc2cca4ff`
- “Develop a Theory (Evolution)” workflow


- Phenomenon to explain:


```text
text
```


- Template:[bifurcation](https://github.com/imbue-ai/catalyst-templates/tree/main/bifurcation)
- Claude Code harness with Sonnet 4.6 (High)
- All other settings at their defaults
