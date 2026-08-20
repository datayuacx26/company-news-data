---
schema_version: "1.0.0"
document_id: "8d989de9e3a32b257881132c55021928ee1e48150fda3347d74bd47b428535f9"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/reasoning-with-language-model-is-planning-with-world-model"
published_at: "2023-05-24T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:55a13dadddd9727d691f3362def8405f0324264de3fa913cd7449e181f9b4db2"
---

# Reasoning with Language Model is Planning with World Model

Do not index


Original Paper


[https://arxiv.org/abs/2305.14992v1](https://arxiv.org/abs/2305.14992v1)


Blog URL


[blog.athina.ai /reasoni...ld-model](https://blog.athina.ai/reasoning-with-language-model-is-planning-with-world-model)


**Original Paper:**[https://arxiv.org/abs/2305.14992v1](https://arxiv.org/abs/2305.14992v1)


**By:**[Shibo Hao](https://arxiv.org/search/cs?searchtype=author&query=Hao%2C%20S) ,[Yi Gu](https://arxiv.org/search/cs?searchtype=author&query=Gu%2C%20Y) ,[Haodi Ma](https://arxiv.org/search/cs?searchtype=author&query=Ma%2C%20H) ,[Joshua Jiahua Hong](https://arxiv.org/search/cs?searchtype=author&query=Hong%2C%20J%20J) ,[Zhen Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Z) ,[Daisy Zhe Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20D%20Z) ,[Zhiting Hu](https://arxiv.org/search/cs?searchtype=author&query=Hu%2C%20Z)


**Abstract:**


> Large language models (LLMs) have shown remarkable reasoning capabilities, especially when prompted to generate intermediate reasoning steps (e.g., Chain-of-Thought, CoT). However, LLMs can still struggle with problems that are easy for humans, such as generating action plans for executing tasks in a given environment, or performing complex math, logical, and commonsense reasoning. The deficiency stems from the key fact that LLMs lack an internal world model to predict the world state (e.g., environment status, intermediate variable values) and simulate long-term outcomes of actions. This prevents LLMs from performing deliberate planning akin to human brains, which involves exploring alternative reasoning paths, anticipating future states and rewards, and iteratively refining existing reasoning steps. To overcome the limitations, we propose a new LLM reasoning framework, R––easoning via––P––lanning (RAP). RAP repurposes the LLM as both a world model and a reasoning agent, and incorporates a principled planning algorithm (based on Monto Carlo Tree Search) for strategic exploration in the vast reasoning space. During reasoning, the LLM (as agent) incrementally builds a reasoning tree under the guidance of the LLM (as world model) and task-specific rewards, and obtains a high-reward reasoning path efficiently with a proper balance between exploration vs. exploitation. We apply RAP to a variety of challenging reasoning problems including plan generation, math reasoning, and logical inference. Empirical results on these tasks demonstrate the superiority of RAP over various strong baselines, including CoT and least-to-most prompting with self-consistency. RAP on LLAMA-33B surpasses CoT on GPT-4 with 33% relative improvement in a plan generation setting.


---


###


Summary Notes


####


Blog Post: Enhancing AI with Advanced Planning Techniques


The field of artificial intelligence (AI) is evolving rapidly, with Large Language Models (LLMs) at the forefront, demonstrating impressive reasoning capabilities.


Yet, these models often struggle with complex tasks that require multi-step reasoning or adapting to changing environments.


This is mainly because LLMs, unlike humans, lack the ability to predict future outcomes based on current actions.


The introduction of the Reasoning via Planning (RAP) framework aims to overcome this limitation by enhancing LLMs with advanced planning and predictive abilities.


###


Framework Introduction


RAP is a groundbreaking framework that equips LLMs with the dual functions of a world model and a reasoning agent.


It incorporates planning algorithms like Monte Carlo Tree Search (MCTS) to navigate through reasoning steps efficiently.


RAP's key strength is its balanced approach to exploring new reasoning paths while also focusing on paths that promise high rewards.


###


How It Works


- **Model Setup** : RAP prompts LLMs to forecast the results of actions in a given situation, thus acting as a world model. It uses MCTS to build a tree structure for reasoning, with each node representing a possible world state and edges representing actions.


- **Planning and Exploration** : RAP focuses on selecting actions that lead to high-reward outcomes, using a process of exploration and reward-based refinement. This process improves decision-making and guides the model towards more effective strategies.


###


Implementing RAP


- **Customizable Rewards** : RAP features a versatile reward system designed for various tasks, allowing the LLM to focus on actions that best meet the task's goals, whether it's solving math problems or understanding common sense.


- **World Model as a Simulator** : By forecasting future world states, RAP allows LLMs to simulate the outcomes of different actions, aiding in tasks that require foresight and strategic planning.


###


Performance and Results


RAP has shown to outperform traditional reasoning methods across multiple domains, including plan generation, logical reasoning, and especially in mathematical reasoning. Compared to GPT-4 using the Chain-of-Thought approach, RAP, when tested on LLaMA-33B models, demonstrated a 33% relative improvement in planning tasks. This highlights RAP's structured and adaptable reasoning strategy.


###


Conclusions and Future Directions


RAP represents a major step forward in giving LLMs human-like planning and reasoning capabilities.


By enabling simulation and planning within task environments, RAP improves the models' effectiveness and extends their applicability to more complex tasks.


Looking ahead, the focus will be on making RAP more adaptable to different tasks, incorporating dynamic planning strategies, and testing the framework in real-world scenarios.


These efforts aim to unlock new potentials for LLMs, setting the stage for a new era in AI reasoning.


RAP not only points towards the future of AI but also guides engineers towards unlocking the full capabilities of LLMs. As we refine this framework, the possibilities for LLM achievements seem limitless.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
