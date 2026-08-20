---
schema_version: "1.0.0"
document_id: "4f5448d2984fa9c853726232dc2078aa28e212f719585dfe0a0ab18687d34f6a"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/chatgpt4pcg-2-competition-prompt-engineering-for-science-birds-level-generation"
published_at: "2024-03-05T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:13.235024+00:00"
content_hash: "sha256:fe495a3ce08ee8258f3d1f1baabf0d4984757ced59b70ed5f67e2b6ba1e8504b"
---

# ChatGPT4PCG 2 Competition: Prompt Engineering for Science Birds Level Generation

Do not index


Original Paper


[https://arxiv.org/abs/2403.02610](https://arxiv.org/abs/2403.02610)


Blog URL


[blog.athina.ai /chatgpt...neration](https://blog.athina.ai/chatgpt4pcg-2-competition-prompt-engineering-for-science-birds-level-generation)


**Original Paper:**[https://arxiv.org/abs/2403.02610](https://arxiv.org/abs/2403.02610)


**By:**[Pittawat Taveekitworachai](https://arxiv.org/search/cs?searchtype=author&query=Taveekitworachai%2C%20P) ,[Febri Abdullah](https://arxiv.org/search/cs?searchtype=author&query=Abdullah%2C%20F) ,[Mury F. Dewantoro](https://arxiv.org/search/cs?searchtype=author&query=Dewantoro%2C%20M%20F) ,[Yi Xia](https://arxiv.org/search/cs?searchtype=author&query=Xia%2C%20Y) ,[Pratch Suntichaikul](https://arxiv.org/search/cs?searchtype=author&query=Suntichaikul%2C%20P) ,[Ruck Thawonmas](https://arxiv.org/search/cs?searchtype=author&query=Thawonmas%2C%20R) ,[Julian Togelius](https://arxiv.org/search/cs?searchtype=author&query=Togelius%2C%20J) ,[Jochen Renz](https://arxiv.org/search/cs?searchtype=author&query=Renz%2C%20J)


**Abstract:**


> This paper presents the second ChatGPT4PCG competition at the 2024 IEEE Conference on Games. In this edition of the competition, we follow the first edition, but make several improvements and changes. We introduce a new evaluation metric along with allowing a more flexible format for participants' submissions and making several improvements to the evaluation pipeline. Continuing from the first edition, we aim to foster and explore the realm of prompt engineering (PE) for procedural content generation (PCG). While the first competition saw success, it was hindered by various limitations; we aim to mitigate these limitations in this edition. We introduce diversity as a new metric to discourage submissions aimed at producing repetitive structures. Furthermore, we allow submission of a Python program instead of a prompt text file for greater flexibility in implementing advanced PE approaches, which may require control flow, including conditions and iterations. We also make several improvements to the evaluation pipeline with a better classifier for similarity evaluation and better-performing function signatures. We thoroughly evaluate the effectiveness of the new metric and the improved classifier. Additionally, we perform an ablation study to select a function signature to instruct ChatGPT for level generation. Finally, we provide implementation examples of various PE techniques in Python and evaluate their preliminary performance. We hope this competition serves as a resource and platform for learning about PE and PCG in general.


---


###


Summary Notes


##


ChatGPT4PCG 2: Enhancing Procedural Content Generation with Advanced Prompt Engineering


The second edition of the ChatGPT4PCG competition marks a significant leap in procedural content generation (PCG) by leveraging advanced artificial intelligence (AI) techniques.


This competition focuses on utilizing prompt engineering (PE) to generate diverse and innovative game levels for Science Birds, offering valuable strategies for AI engineers in enterprise companies.


###


Introduction


The inaugural ChatGPT4PCG competition showcased the potential and identified limitations of using ChatGPT for generating Science Birds levels. It faced challenges such as metric exploitation and classifier inefficiency.


To overcome these, ChatGPT4PCG 2 has introduced enhancements like a diversity metric, acceptance of Python program submissions, and an improved evaluation pipeline, aiming for richer, more complex level designs.


###


Key Improvements


####


Diversity Metric


- A new metric to encourage a wide range of creative designs, addressing the issue of repetitive structures.


####


Python Program Submissions


- Participants can now submit Python programs, enabling the use of advanced PE techniques for more detailed and nuanced designs.


####


Enhanced Classifier and Function Signature


- Deploying a new classifier better suited for Science Birds and selecting an optimal function signature have both led to more accurate content generation.


###


Evaluation Pipeline


The revamped evaluation process includes manual submission checks, response analysis through submitted programs, and diversity assessment. A specialized Python package has been developed to streamline this process, ensuring fairness and efficiency.


###


Experimental Insights


Experiments were conducted to evaluate the new classifier, diversity metric, and various PE techniques. These experiments highlighted the importance of precise prompt engineering for achieving the desired results.


###


PE Techniques Evaluation


The competition tested a variety of PE strategies, such as:


- **Zero-shot and few-shot prompting:** Basic strategies to gauge effectiveness with minimal examples.


- **Chain of Thought (CoT) prompting:** Detailed reasoning to guide the model towards generating sophisticated content.


- **Advanced control flow techniques:** Exploring innovative designs through complex prompting strategies.


###


Conclusion


ChatGPT4PCG 2 pushes the boundaries of using LLMs in game design by introducing Python program submissions and a diversity metric. The competition offers insights that could shape future PCG initiatives and the broader application of AI in creative processes. It stands as a key milestone in the exploration of LLM capabilities, steering the development of more dynamic and engaging digital content.


###


References


The work discussed is based on extensive research and experimentation, drawing from foundational PCG papers, evaluation methodologies, and studies on LLMs in creative content generation.


These references solidify our understanding of PCG's current state and the innovative steps taken in the ChatGPT4PCG 2 competition.


In summary, ChatGPT4PCG 2 advances the use of AI in game design and procedural content generation by addressing previous limitations and introducing new metrics and formats for submission.


It paves the way for creativity and innovation in PCG, offering valuable insights for AI engineers and gaming industry professionals.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
