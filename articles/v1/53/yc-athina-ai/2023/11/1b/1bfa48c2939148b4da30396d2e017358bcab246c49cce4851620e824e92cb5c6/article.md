---
schema_version: "1.0.0"
document_id: "1bfa48c2939148b4da30396d2e017358bcab246c49cce4851620e824e92cb5c6"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/chain-of-thought-reasoning-is-a-policy-improvement-operator"
published_at: "2023-11-08T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:0acbbbb5ac1d5f7d43ce343beaba46144b6cda6cbcc87206233f1917f0b7d223"
---

# Chain-of-Thought Reasoning is a Policy Improvement Operator

Do not index


Original Paper


[https://arxiv.org/abs/2309.08589](https://arxiv.org/abs/2309.08589)


Blog URL


[blog.athina.ai /chain-o...operator](https://blog.athina.ai/chain-of-thought-reasoning-is-a-policy-improvement-operator)


**Original Paper:**[https://arxiv.org/abs/2309.08589](https://arxiv.org/abs/2309.08589)


**By:**[Hugh Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20H) ,[David C. Parkes](https://arxiv.org/search/cs?searchtype=author&query=Parkes%2C%20D%20C)


**Abstract:**


> Large language models have astounded the world with fascinating new capabilities. However, they currently lack the ability to teach themselves new skills, relying instead on large amounts of human-generated training data. We introduce SECToR (Self-Education via Chain-of-Thought Reasoning), a proof-of-concept demonstration that language models can teach themselves new skills using chain-of-thought reasoning. During the self-learning loop, SECToR asks models to solve addition problems using chain-of-thought reasoning before training the next version of the model to solve those same problems directly without using such reasoning. This process often results in an improved model which can, when again augmented with chain-of-thought reasoning, solve even harder problems than the original model, allowing the self-learning loop to continue. Language models trained via SECToR autonomously learn to add up to the longest-length-digit numbers without access to any ground truth examples beyond an initial supervised fine-tuning phase consisting only of numbers with 6 or fewer digits. Our central hypothesis is that chain-of-thought reasoning can act as a policy improvement operator, similarly to how Monte-Carlo Tree Search is used in AlphaZero (Silver et al., 2017). We hope that this research can lead to new directions in which language models can learn to teach themselves without the need for human demonstrations.


---


###


Summary Notes


##


Revolutionizing AI with Chain-of-Thought Reasoning


The advancements in large language models (LLMs) have been groundbreaking, showcasing their ability to grasp complex concepts and solve challenging problems.


Despite their strides, a key question persists: Can these models learn new things on their own, beyond their initial training?


Enter SECToR (Self-Education via Chain-of-Thought Reasoning), an innovative approach that aims to push LLMs into the realm of self-learning.


###


Tackling the Issue of Data Exhaustion


In the world of AI, data exhaustion is a major hurdle. Traditionally, models rely on the data they're trained with, which limits their ability to learn new tasks.


This becomes a significant issue for self-learning, where models are expected to pick up new skills independently.


Past attempts at self-learning have faced challenges, such as error avalanching, where minor mistakes snowball into major inaccuracies.


###


SECToR's Innovative Approach


SECToR introduces a two-phase training strategy to overcome these challenges. It starts with supervised learning on simple tasks, like basic addition, before moving to self-training on more complex problems.


This approach is built around chain-of-thought reasoning, which allows the model to break down problems into manageable steps.


####


Highlights of SECToR:


- **Two-Phase Training** : Combines supervised and self-learning.


- **Starts with Simple Addition** : Trains on adding numbers with up to 6 digits first.


- **Moves to Self-Training** : Employs chain-of-thought reasoning for more complex tasks.


###


Results and Impact


The study used a 582M parameter ByT5 model, initially trained on 6-digit addition problems. A curriculum learning strategy was used to ensure proficiency before advancing. In the self-training phase, the model worked on both simple and complex addition, detailing its reasoning for the latter. This enabled the model to autonomously learn to accurately add numbers up to 29 digits.


####


Achievements:


- **High Accuracy** : Surpassed 98% accuracy on up to 29-digit additions.


- **Better Generalization** : Showed marked improvement in tackling new problem types.


- **Enhanced Problem-Solving** : Benefited from the detailed reasoning process.


###


Looking Ahead: Applications Beyond Simple Tasks


SECToR's potential goes beyond basic arithmetic or games. Its chain-of-thought reasoning could significantly benefit various fields, including mathematics and computer programming.


However, issues like computational demands and the limits of self-improvement remain. Future research could focus on more efficient learning techniques and ensuring model reliability and safety.


###


Conclusion: Pioneering Self-Learning in AI


SECToR exemplifies the possibility of LLMs teaching themselves new tasks, marking a significant milestone in AI research.


This breakthrough could lead to AI systems capable of autonomous learning, opening new research avenues and applications across different domains.


For AI engineers, this represents a move towards models that learn and grow independently, signaling a new era in artificial intelligence.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
