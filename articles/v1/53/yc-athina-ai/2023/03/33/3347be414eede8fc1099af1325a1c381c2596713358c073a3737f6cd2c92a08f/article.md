---
schema_version: "1.0.0"
document_id: "3347be414eede8fc1099af1325a1c381c2596713358c073a3737f6cd2c92a08f"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/react-synergizing-reasoning-and-acting-in-language-models"
published_at: "2023-03-10T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:07b611b5f15d303c9bc368a7eb52c591103f14e43cc963273f4ac8de3e6d24f7"
---

# ReAct: Synergizing Reasoning and Acting in Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2210.03629](https://arxiv.org/abs/2210.03629)


Blog URL


[blog.athina.ai /react-s...e-models](https://blog.athina.ai/react-synergizing-reasoning-and-acting-in-language-models)


**Original Paper:**[https://arxiv.org/abs/2210.03629](https://arxiv.org/abs/2210.03629)


**By:**[Shunyu Yao](https://arxiv.org/search/cs?searchtype=author&query=Yao%2C%20S) ,[Jeffrey Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20J) ,[Dian Yu](https://arxiv.org/search/cs?searchtype=author&query=Yu%2C%20D) ,[Nan Du](https://arxiv.org/search/cs?searchtype=author&query=Du%2C%20N) ,[Izhak Shafran](https://arxiv.org/search/cs?searchtype=author&query=Shafran%2C%20I) ,[Karthik Narasimhan](https://arxiv.org/search/cs?searchtype=author&query=Narasimhan%2C%20K) ,[Yuan Cao](https://arxiv.org/search/cs?searchtype=author&query=Cao%2C%20Y)


**Abstract:**


> While large language models (LLMs) have demonstrated impressive capabilities across tasks in language understanding and interactive decision making, their abilities for reasoning (e.g. chain-of-thought prompting) and acting (e.g. action plan generation) have primarily been studied as separate topics. In this paper, we explore the use of LLMs to generate both reasoning traces and task-specific actions in an interleaved manner, allowing for greater synergy between the two: reasoning traces help the model induce, track, and update action plans as well as handle exceptions, while actions allow it to interface with external sources, such as knowledge bases or environments, to gather additional information. We apply our approach, named ReAct, to a diverse set of language and decision making tasks and demonstrate its effectiveness over state-of-the-art baselines, as well as improved human interpretability and trustworthiness over methods without reasoning or acting components. Concretely, on question answering (HotpotQA) and fact verification (Fever), ReAct overcomes issues of hallucination and error propagation prevalent in chain-of-thought reasoning by interacting with a simple Wikipedia API, and generates human-like task-solving trajectories that are more interpretable than baselines without reasoning traces. On two interactive decision making benchmarks (ALFWorld and WebShop), ReAct outperforms imitation and reinforcement learning methods by an absolute success rate of 34% and 10% respectively, while being prompted with only one or two in-context examples. Project site with code:
>
>
> [this https URL](https://react-lm.github.io/)


---


###


Summary Notes


####


Simplified Blog Post: Enhancing AI with REACT: Merging Thought and Action


In the fast-paced world of artificial intelligence (AI), combining reasoning with action is key to creating more intelligent systems. This approach, much like how humans think and act, leads to better decision-making and learning in AI.


Large language models (LLMs) have greatly advanced AI, but their true power is realized when they can both think and act effectively. REACT is a cutting-edge method designed to improve how LLMs solve tasks by blending thought processes with dynamic actions.


####


What is REACT?


REACT stands for Reasoning and Acting, a method that significantly enhances AI capabilities. It enables LLMs to generate thought processes and actions at the same time, allowing for adjustments as tasks evolve. The strengths of REACT include:


- Allowing AI to interact with external data, enriching its thought process with up-to-date information.


- Making AI's action plans more adaptable to changes, ensuring swift responses to new tasks or environments.


####


Testing REACT's Effectiveness


REACT's performance was tested across various benchmarks like question answering, fact verification, text-based gaming, and webpage navigation. The results showed:


- A decrease in errors in the model's reasoning, helped by using external data sources like Wikipedia.


- Better performance in tasks requiring action, even with fewer training examples, compared to other learning models.


- Enhanced model interpretability and reliability, making it easier to distinguish between internal and external information.


####


Contributions of REACT


REACT's introduction is a milestone for AI, offering:


- A novel method that combines reasoning with action in LLMs, opening doors to more complex AI applications.


- Proven effectiveness across different benchmarks, showing REACT's versatility and efficiency.


- New insights into blending thought and action in tasks, suggesting areas for future innovation.


####


Evaluating REACT


The evaluation involved testing models in various scenarios, from simple reasoning or action tasks to combinations of both, using different LLMs like PaLM-540B for a thorough performance assessment.


####


Results and Insights


REACT has significantly boosted task performance, showing better success rates and efficient use of training data. This efficiency reduces the need for large datasets, highlighting REACT's potential to make AI training more streamlined and adaptable.


####


Future Directions


The possibilities for REACT are vast, with future research aiming to:


- Scale REACT for more complex tasks.


- Combine REACT with other machine learning approaches for improved performance.


- Test REACT in real-world scenarios to assess its adaptability and robustness.


####


Conclusion


REACT marks a significant step forward for AI, especially in using LLMs for practical tasks. By emulating human reasoning and action, REACT not only boosts the capabilities of LLMs but also sets the stage for creating more sophisticated, understandable, and efficient AI systems. As AI continues to evolve, REACT provides a promising framework for future advancements, promising to unlock new AI possibilities.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
