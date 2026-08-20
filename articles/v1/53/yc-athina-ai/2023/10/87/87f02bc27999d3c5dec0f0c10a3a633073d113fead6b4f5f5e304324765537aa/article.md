---
schema_version: "1.0.0"
document_id: "87f02bc27999d3c5dec0f0c10a3a633073d113fead6b4f5f5e304324765537aa"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/autonomous-tree-search-ability-of-large-language-models"
published_at: "2023-10-14T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:ec604adc5b5d4528ea5ef8339ee791a06622a6488fdf056cf82aba248aaa9d95"
---

# Autonomous Tree-search Ability of Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2310.10686](https://arxiv.org/abs/2310.10686)


Blog URL


[blog.athina.ai /autonom...e-models](https://blog.athina.ai/autonomous-tree-search-ability-of-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2310.10686](https://arxiv.org/abs/2310.10686)


**By:**[Zheyu Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20Z) ,[Zhuorui Ye](https://arxiv.org/search/cs?searchtype=author&query=Ye%2C%20Z) ,[Yikang Shen](https://arxiv.org/search/cs?searchtype=author&query=Shen%2C%20Y) ,[Chuang Gan](https://arxiv.org/search/cs?searchtype=author&query=Gan%2C%20C)


**Abstract:**


> Large Language Models have excelled in remarkable reasoning capabilities with advanced prompting techniques, but they fall short on tasks that require exploration, strategic foresight, and sequential decision-making. Recent works propose to utilize external programs to define search logic, such that LLMs can perform passive tree search to solve more challenging reasoning tasks. Though impressive results have been achieved, there are several fundamental limitations of these approaches. First, passive tree searches are not efficient as they usually require multiple rounds of LLM API calls to solve one single problem. Moreover, passive search methods are not flexible since they need task-specific program designs. Then a natural question arises: can we maintain the tree-search capability of LLMs without the aid of external programs, and can still generate responses that clearly demonstrate the process of a tree-structure search? To this end, we propose a new concept called autonomous tree-search ability of LLM, which can automatically generate a response containing search trajectories for the correct answer. Concretely, we perform search trajectories using capable LLM API via a fixed system prompt, allowing them to perform autonomous tree-search (ATS) right out of the box. Experiments on 4 puzzle games demonstrate our method can achieve huge improvements. The ATS-BFS method outperforms the Chain of Thought approach by achieving an average accuracy improvement of 33%. Compared to Tree of Thoughts, it requires 65.6% or 47.7% less GPT-api cost to attain a comparable level of accuracy. Moreover, we have collected data using the ATS prompt method and fine-tuned LLaMA. This approach yield a greater improvement compared to the ones fine-tuned on CoT data. Specifically, it outperforms CoT-tuned LLaMAs by an average of 40.6% and 38.5% for LLaMA2-7B and LLaMA2-13B, respectively.


---


###


Summary Notes


##


Boosting Problem-Solving in AI with Autonomous Tree-Search


As the world becomes more complex, so do the challenges we ask artificial intelligence (AI) to solve. This is particularly true for Large Language Models (LLMs) like GPT-4, which are spearheading AI advancements.


However, these models often hit a wall with tasks that require strategic thinking and step-by-step decision-making.


The study "Autonomous Tree-Search Ability of Large Language Models" introduces an exciting development aimed at enhancing AI's problem-solving skills without human help.


###


The Challenge with Current LLMs


LLMs traditionally use linear reasoning, like the Chain-of-Thought (CoT), for solving problems. This works for simple tasks but struggles with complex, multi-step challenges.


Alternatives like the Tree of Thoughts (ToT) method offer more flexibility but still face issues like inefficiency and limited adaptability.


###


What is Autonomous Tree-Search (ATS)?


The study introduces a groundbreaking method called Autonomous Tree-Search (ATS), enabling LLMs to independently create tree-structured search paths.


This method significantly improves LLMs' problem-solving abilities without external code, marking a big step in AI autonomy.


####


How ATS Works


ATS utilizes two strategies:


- **ATS-BFS (Breadth-First Search):** Simultaneously explores various solution paths, ensuring a thorough search.


- **ATS-DFS (Depth-First Search):** Focuses on deeply exploring one solution path before moving to the next, prioritizing depth.


Implemented in cutting-edge models like GPT-4 and LLaMA, ATS offers a fresh approach to tackling complex AI problems.


###


Performance and Impact


The effectiveness of ATS was tested using new puzzle datasets, requiring advanced reasoning and sequential decisions. Key findings include:


- **Higher Accuracy:** ATS outshone both CoT and ToT in solving complex puzzles, with ATS-BFS improving solution accuracy by up to 33%.


- **Lower Costs:** ATS reduced the need for API calls, cutting computational costs by more than 65% compared to traditional methods.


###


Future Possibilities and Directions


ATS's success opens up new opportunities for AI development. Its efficiency and flexibility could lead to LLMs handling more complex tasks independently, transforming AI development and deployment into a more autonomous and cost-effective process.


Future research aims to expand ATS for larger models and integrate it with other reasoning techniques, further boosting LLMs' problem-solving prowess.


###


Conclusion


The study on the "Autonomous Tree-Search Ability of Large Language Models" marks a significant advancement in AI, enabling LLMs to independently tackle complex problems.


This breakthrough could lead to more efficient, flexible, and autonomous AI systems. For AI Engineers in enterprise settings, leveraging ATS could greatly enhance your AI systems' problem-solving abilities, fostering innovation and efficiency.


Keeping up with advancements like ATS is crucial for staying competitive in the ever-evolving AI landscape.


####


References


For more details on the research and methodologies behind ATS, the following references offer comprehensive information on LLMs, Chain-of-Thought reasoning, and Autonomous Tree-Search development:


- Zhang, Z., Ye, Z., Shen, Y. (2023). "Autonomous Tree-Search Ability of Large Language Models." Institute for Interdisciplinary Information Sciences, Tsinghua University; IT-IBM Watson AI Lab, IBM Research.


- A wide range of literature on GPT models and their capabilities, including key papers and studies on computational efficiency and environmental impacts.


By embracing Autonomous Tree-Search principles, AI engineers can achieve new heights in problem-solving efficiency and innovation in their AI models.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
