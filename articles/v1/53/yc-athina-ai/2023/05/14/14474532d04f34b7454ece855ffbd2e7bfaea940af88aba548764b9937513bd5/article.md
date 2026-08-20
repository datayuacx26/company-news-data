---
schema_version: "1.0.0"
document_id: "14474532d04f34b7454ece855ffbd2e7bfaea940af88aba548764b9937513bd5"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/better-zero-shot-reasoning-with-self-adaptive-prompting"
published_at: "2023-05-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:62b63dbae5dcbf0b86ef92b4a6b867b89b809fe5170d24b0ff2442b3192f1806"
---

# Better Zero-Shot Reasoning with Self-Adaptive Prompting

Do not index


Original Paper


[https://arxiv.org/abs/2305.14106](https://arxiv.org/abs/2305.14106)


Blog URL


[blog.athina.ai /better-...rompting](https://blog.athina.ai/better-zero-shot-reasoning-with-self-adaptive-prompting)


**Original Paper:**[https://arxiv.org/abs/2305.14106](https://arxiv.org/abs/2305.14106)


**By:**[Xingchen Wan](https://arxiv.org/search/cs?searchtype=author&query=Wan%2C%20X) ,[Ruoxi Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20R) ,[Hanjun Dai](https://arxiv.org/search/cs?searchtype=author&query=Dai%2C%20H) ,[Sercan O. Arik](https://arxiv.org/search/cs?searchtype=author&query=Arik%2C%20S%20O) ,[Tomas Pfister](https://arxiv.org/search/cs?searchtype=author&query=Pfister%2C%20T)


**Abstract:**


> Modern large language models (LLMs) have demonstrated impressive capabilities at sophisticated tasks, often through step-by-step reasoning similar to humans. This is made possible by their strong few and zero-shot abilities -- they can effectively learn from a handful of handcrafted, completed responses ("in-context examples"), or are prompted to reason spontaneously through specially designed triggers. Nonetheless, some limitations have been observed. First, performance in the few-shot setting is sensitive to the choice of examples, whose design requires significant human effort. Moreover, given the diverse downstream tasks of LLMs, it may be difficult or laborious to handcraft per-task labels. Second, while the zero-shot setting does not require handcrafting, its performance is limited due to the lack of guidance to the LLMs. To address these limitations, we propose Consistency-based Self-adaptive Prompting (COSP), a novel prompt design method for LLMs. Requiring neither handcrafted responses nor ground-truth labels, COSP selects and builds the set of examples from the LLM zero-shot outputs via carefully designed criteria that combine consistency, diversity and repetition. In the zero-shot setting for three different LLMs, we show that using only LLM predictions, COSP improves performance up to 15% compared to zero-shot baselines and matches or exceeds few-shot baselines for a range of reasoning tasks.


---


###


Summary Notes


##


Elevating Zero-Shot Reasoning with Self-Adaptive Prompting


The field of artificial intelligence (AI) is advancing rapidly, with large language models (LLMs) leading the charge in solving complex reasoning challenges.


Despite their capabilities, creating effective prompts and choosing the right examples for these models is a daunting task that requires a lot of human input and expertise.


This blog post introduces an innovative method called Consistency-based Self-adaptive Prompting (COSP), which significantly improves the zero-shot reasoning abilities of LLMs without the need for manually selecting responses or labels.


###


The Prompting Challenge


Creating effective prompts for LLMs is tricky. Traditional techniques like few-shot and zero-shot Chain of Thought (CoT) prompting guide LLMs to generate reasoned outputs.


However, these methods have their drawbacks: few-shot learning is resource-heavy because it needs carefully chosen examples, while zero-shot learning often underperforms without human-crafted context.


###


What is COSP?


COSP offers a solution by refining LLM reasoning through a two-step approach:


- **Stage 1:** Generate multiple reasoning paths for an input query using zero-shot CoT, creating a pool of potential responses.


- **Stage 2:** Select the best candidates from this pool based on their consistency, diversity, and lack of repetition, and use them to craft a better prompt.


This method uses the model's outputs to dynamically create prompts, facilitating a self-learning environment that boosts reasoning capabilities.


###


COSP's Mechanism


COSP stands out for its simplicity, requiring no labeled data. It improves reasoning by evaluating the self-consistency of LLM outputs, making it computationally efficient and easily adaptable to new tasks without manual tweaking.


####


Implementation Steps


For AI engineers, implementing COSP involves:


- **Generating Reasoning Paths:** Let the LLM produce multiple paths for a query.


- **Evaluating Candidates:** Use a scoring system to assess these paths for consistency, diversity, and non-repetition.


- **Refining Prompts:** Use the best candidates to prompt the LLM again, aiming for higher reasoning quality.


COSP is adaptable and enhances LLM reasoning performance across various tasks.


###


COSP Performance


Testing COSP on arithmetic and logical reasoning tasks shows it consistently outperforms zero-shot baselines and competes well with resource-heavy few-shot methods, demonstrating its efficacy and efficiency.


###


Looking Ahead


COSP's potential is vast, with possibilities for broader application in language processing tasks, improved self-consistency measures, and adaptation to other models. This research direction promises significant advancements in AI and new opportunities for applying LLMs in natural language understanding.


###


Conclusion


COSP marks a major advancement in developing autonomous and adaptable LLMs for complex reasoning tasks. By leveraging models' outputs in a self-adaptive manner, it boosts performance and reduces the need for intensive human oversight.


COSP offers AI engineers a practical and efficient way to overcome the hurdles of zero-shot reasoning, leading to more reliable, scalable, and versatile AI solutions.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
