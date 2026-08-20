---
schema_version: "1.0.0"
document_id: "5676383fa47a8d5cf231cc835245288eec978cb8f33425d28f89deb5db13d141"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/what-in-context-learning-learns-in-context-disentangling-task-recognition-and-task-learning"
published_at: "2023-05-16T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:6a7e4ff712f2da0859304a8ccae99fbc7a3e852f172958f355e592847e1e4512"
---

# What In-Context Learning "Learns" In-Context: Disentangling Task Recognition and Task Learning

Do not index


Original Paper


[https://arxiv.org/abs/2305.09731](https://arxiv.org/abs/2305.09731)


Blog URL


[blog.athina.ai /what-in...learning](https://blog.athina.ai/what-in-context-learning-learns-in-context-disentangling-task-recognition-and-task-learning)


**Original Paper:**[https://arxiv.org/abs/2305.09731](https://arxiv.org/abs/2305.09731)


**By:**[Jane Pan](https://arxiv.org/search/cs?searchtype=author&query=Pan%2C%20J) ,[Tianyu Gao](https://arxiv.org/search/cs?searchtype=author&query=Gao%2C%20T) ,[Howard Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20H) ,[Danqi Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20D)


**Abstract:**


> Large language models (LLMs) exploit in-context learning (ICL) to solve tasks with only a few demonstrations, but its mechanisms are not yet well-understood. Some works suggest that LLMs only recall already learned concepts from pre-training, while others hint that ICL performs implicit learning over demonstrations. We characterize two ways through which ICL leverages demonstrations. Task recognition (TR) captures the extent to which LLMs can recognize a task through demonstrations -- even without ground-truth labels -- and apply their pre-trained priors, whereas task learning (TL) is the ability to capture new input-label mappings unseen in pre-training. Using a wide range of classification datasets and three LLM families (GPT-3, LLaMA and OPT), we design controlled experiments to disentangle the roles of TR and TL in ICL. We show that (1) models can achieve non-trivial performance with only TR, and TR does not further improve with larger models or more demonstrations; (2) LLMs acquire TL as the model scales, and TL's performance consistently improves with more demonstrations in context. Our findings unravel two different forces behind ICL and we advocate for discriminating them in future ICL research due to their distinct nature.


---


###


Summary Notes


##


Demystifying In-Context Learning in Large Language Models


###


Introduction


The world of Artificial Intelligence (AI) is being transformed by Large Language Models (LLMs) like GPT-3, LLaMA, and OPT, which excel at a variety of tasks through in-context learning (ICL).


This ability enables them to undertake tasks based on examples given in their prompts. Although in-context learning is widely recognized, the specific mechanisms behind it remain largely unexplored.


This blog post delves into a significant study that identifies two key mechanisms: Task Recognition (TR) and Task Learning (TL), offering insights for AI Engineers looking to better utilize LLMs.


###


Exploring the Mechanisms


####


Task Recognition (TR)


- **What It Is** : TR is when LLMs identify what task they're facing using their pre-trained knowledge, without needing exact labels.


- **Key Insight** : The efficiency of TR is mostly constant, regardless of the model size or the number of examples provided. This indicates that LLMs can recognize tasks using their existing knowledge, even with inaccurate labels.


####


Task Learning (TL)


- **What It Is** : TL is the process of learning new patterns from input-label examples in the prompt. It shows how models adapt their responses based on the context.


- **Key Insight** : Contrary to TR, TL improves with larger models and more examples, suggesting that bigger LLMs are better at adapting to new information, emphasizing the role of model size in in-context learning.


###


Practical Advice for AI Engineers


####


Choosing the Right Model Size


- For tasks that lean heavily on task learning (TL), larger LLMs tend to be more effective. Their ability to absorb and learn from new information makes them ideal for complex or unfamiliar tasks.


####


Crafting Better Prompts


- **For TR-focused tasks** : Make sure the prompt clearly defines the task, tapping into the model's existing knowledge.


- **For TL-focused tasks** : Use detailed and diverse examples in the prompt to aid learning. More examples can enhance the model's understanding and adaptation skills.


####


Task-Specific Approaches


- Understand that tasks vary in nature. Tasks relying on pre-trained knowledge (e.g., general knowledge queries) might not benefit much from larger models or extra examples. However, tasks requiring the learning of new patterns (e.g., specific data analysis for a company) could see significant gains with bigger models and more comprehensive demonstration sets.


###


Conclusion


Investigating in-context learning in LLMs highlights the distinct roles of Task Recognition (TR) and Task Learning (TL). These insights enable AI Engineers to use LLMs more strategically, selecting appropriate model sizes and designing prompts that enhance in-context learning capabilities.


As AI evolves, recognizing and leveraging these nuances in in-context learning will be key to maximizing the potential of LLMs for enterprise applications.


###


Acknowledgements


This exploration is based on pioneering research by Jane Pan, Tianyu Gao, Howard Chen, and Danqi Chen from Princeton University's Department of Computer Science.


Their work lays the groundwork for further research into LLM capabilities and limitations, guiding AI Engineers in effectively applying these models in practical scenarios.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
