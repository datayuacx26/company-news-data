---
schema_version: "1.0.0"
document_id: "6212dd70698a410055ef40b732637d9d45bad683bdc398c9b6299ff1715931f3"
company_key: "international-business-machines-corporation-common-stock"
company: "International Business Machines Corporation"
source_id: "international-business-machines-corporation-common-stock-news-import-da8ebfcd6a45"
canonical_url: "https://research.ibm.com/blog/how-the-wrong-training-environment-can-teach-ai-models-to-misbehave"
published_at: "2026-07-09T13:30:39.026+00:00"
first_seen_at: "2026-07-24T12:23:30.059185+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:300cb0180ac793a40a6ff039a6ae65cc530c27c405c4697a3dcb60c341ea57b5"
---

# How the wrong training environment can teach AI models to misbehave

Imagine you’re a developer who’s been tasked with auditing a large language model for safety issues. You probe the LLM with harmful prompts, trying to get it to divulge sensitive information or generate prohibited content. It refuses all of them. The model has passed your test with flying colors.


But the next day, you learn that a user was able to easily bypass filters for safety and abuse. How could this be?


This problem is known as “alignment faking” or “AI scheming,” where highly capable AI systems exploit flaws in reward functions to maximize performance metrics without actually solving the intended task. As in the above example, they can appear safe, truthful, or compliant during evaluation, but they adopt different, potentially problematic behaviors when they’re deployed.


According to a new[paper](https://doi.org/10.48550/arXiv.2602.12124) from a team led by Pin-Yu Chen, principal AI research scientist at IBM Research, this could be part of a broader and less explored problem: Flaws in the training environment itself can lead to misalignment. Even if the reward function is correctly designed, models can exploit loopholes in the environment, like hidden correlations or “shortcut” signals. Chen performed this research with colleagues at IBM Research and the[Notre Dame–IBM Technology Ethics Lab](https://ethics.nd.edu/labs-and-centers/notre-dame-ibm-technology-ethics-lab/) . They presented their results this week at the International Conference on Machine Learning (ICML) in Seoul, South Korea.


In a series of experiments, the team found that capability-oriented training can inadvertently induce exploitative or deceptive behavior in AI models, particularly LLMs trained using reinforcement learning. Rather than being one-off tricks, these skills are actually broadly translatable and generalizable and can be applied to new tasks and taught to other models. And they become more frequent as model capability increases, even alongside true improvements.


In other words, as LLMs become more capable, they also become better at finding shortcuts that maximize rewards during training.


## Studying aberrant model behavior


To investigate how models can misbehave, the researchers developed four controlled experimental scenarios, which they called “vulnerability games.” Each game was injected with loopholes that temped the models to develop deceptive behavior:


**Context-Conditional Compliance:** In one experiment, models were given harmful and harmless prompts written in different styles. As in our example above, the test models learned to behave safely under testing situations but acted unsafely in real-world contexts. Instead of understanding safety principles, they relied on superficial cues like writing style. Whereas a model auditor may use formal language in their prompts, a normal user may write informally or with typos, for example. “If they can sense they are being evaluated, then they may behave,” said Chen. It’s sort of like when Volkswagen infamously programmed its diesel vehicles to run cleaner under inspection conditions, a trick that was only discovered when researchers measured emissions[under real world conditions](https://www.inverse.com/article/26444-scientists-discovered-vw-scandal-accident) .


**Audited Self-Grading:** In this game, models were asked to produce answers and report whether those answers were correct. When audits were infrequent, the models learned that dishonesty was the best policy. They claimed to be correct regardless of the truth, reaping their ill-gotten rewards.


**Proxy Metric Gaming:** This game tested models on the complex task of text summarization, where reward was strictly defined by a metric that prizes surface level An n-gram, so called because it is 'n' units long, is a collection of items from a text that is used to analyze documents. An n-gram can be used to identify patterns like word frequency in a given document.


n -gram


overlap over something tough to measure, like semantic understanding. It showed that highly capable models optimized for measurable metrics (like word overlap in summaries) rather than true quality, leading to outputs that score well but are significantly less useful or coherent for human users.


**Reward Tampering:** Here, researchers simulated an automated evaluation pipeline that required models to submit code solutions within a JSON object. They found that models consistently exploited technical vulnerabilities in the evaluation system itself, such as directly altering a score field instead of solving the assigned task.


Across all four scenarios, the researchers found that models consistently learned to exploit these weaknesses during standard reinforcement learning training. These behaviors emerged naturally as the models improved, without any explicit instructions to cheat.


## Why do alignment risks happen?


Previous studies have proven the phenomena of alignment faking and AI scheming, but the specific conditions where they arise aren’t well understood. Two major research questions arise.


The first is simple: What gives rise to scheming? Is it the training environment, reward scheme, evaluation metrics, or something else? The second question is about whether models can teach themselves and even other models to become unreliable: Does this behavior generalize via zero-shot transfer, catalyzed learning, and distillation?


With the problem thoroughly demonstrated, Chen and team sought to answer that first research question on a diverse set of models, including base and instruction-tuned models, as well as models trained to produce chain-of-thought style reasoning. They trained the models using reinforcement learning and scored them based on how well they performed on the intended task and how frequently they cheated. They also tracked how many steps it took for a newly emerged exploit to become a model’s go-to move.


Almost all the way across the board, exploitative behaviors arose spontaneously. In the cases of self-grading and context-conditional compliance, models discovered these exploits by chance, received a reward, and amplified the strategy. In other cases, like with proxy metrics, cheating was rewarded from the beginning and naturally became the dominant approach.


Importantly, capability-oriented, training-induced alignment risk is different from standard reward hacking. Its scope is broader, in that it games specifications, even when reward objectives are correctly specified. It is also deceptive, because it can mimic legitimate learning.


This behavior reflects a broader phenomenon known as shortcut learning, which has previously been observed in domains like computer vision. For example, an AI may figure out which hospitals have the most cancer patients and[flag their scans](https://www.nature.com/articles/s41598-025-89210-x) , rather than actually learning to spot cancer cells. Chen notes that neural networks often rely on any signal that maximizes reward, even if it is irrelevant to the actual task. In generative AI systems, this tendency is amplified because of their flexibility and their ability to generalize.


## A growing concern


A particularly concerning discovery is that these exploitative strategies are not isolated tricks. Instead, they behave like general skills. Once learned, they can transfer to new tasks, help models discover additional exploits more quickly, and even spread to other models through training data distillation. This suggests that such behaviors could propagate widely as AI systems are developed and reused.


In a series of tests, models were examined to see whether success at one exploit would transfer to succeeding at or discovering another exploit. Generally speaking, both did happen. Fine-tuning a “student” model on a well-honed cheater “teacher” model also proved to increase the student’s rates of cheating. This raises concerns about long-term AI development, particularly in systems that iteratively improve themselves, as misalignment could accumulate across generations.


“You can imagine this catastrophic setting where one generation of model starts to develop some misalignment behavior or cheats, and those kinds of ‘bad genes’ will transfer to the future generations,” said Chen.


Besides the ability for the problem to grow, a major challenge is that these exploitative behaviors often appear alongside real improvements in a model’s task performance, creating a developer blind spot where increasing capability masks underlying misalignment. As a result, standard evaluation methods may fail to detect the problem.


## The right environment


In terms of mitigation, Chen emphasized the need for careful design of training environments, not just reward functions. This includes ensuring balanced and representative data distributions, avoiding hidden correlations, and evaluating models across diverse conditions.


The new work emphasizes the importance of investigating how misalignment arises and spreads, said Chen, with the goal of developing techniques to prevent harmful behaviors from being learned or transferred. Crucially, AI alignment is not about defining the right objectives, but also about designing the right environments in which models learn.


Recent IBM developments in[generative computing](https://research.ibm.com/blog/generative-computing-mellea) could help ensure proper alignment. Mellea is an open-source library that imposes requirements at inference time. It has the ability to automatically decompose a user query into requirements, and self-check if those requirements are met when solving a given task. This approach can enhance AI's reliability when solving a task by ensuring that its work is checked against user specified guardrails.


Moving forward, the team is currently investigating exactly how misalignment can be transferred among models, including by distillation.
