---
schema_version: "1.0.0"
document_id: "236869ca0d8b6ed828e6a6cb4d350acb26a4007ac56678c5cc354bff00bb45b4"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/sgl-pt-a-strong-graph-learner-with-graph-prompt-tuning"
published_at: "2023-08-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:e9e16aabb079c759d1a1aa92654ea6994f4c836282e26c8cac5322245c65271c"
---

# SGL-PT: A Strong Graph Learner with Graph Prompt Tuning

Do not index


Original Paper


[https://arxiv.org/abs/2302.12449](https://arxiv.org/abs/2302.12449)


Blog URL


[blog.athina.ai /sgl-pt-...t-tuning](https://blog.athina.ai/sgl-pt-a-strong-graph-learner-with-graph-prompt-tuning)


**Original Paper:**[https://arxiv.org/abs/2302.12449](https://arxiv.org/abs/2302.12449)


**By:**[Yun Zhu](https://arxiv.org/search/cs?searchtype=author&query=Zhu%2C%20Y) ,[Jianhao Guo](https://arxiv.org/search/cs?searchtype=author&query=Guo%2C%20J) ,[Siliang Tang](https://arxiv.org/search/cs?searchtype=author&query=Tang%2C%20S)


**Abstract:**


> Recently, much exertion has been paid to design graph self-supervised methods to obtain generalized pre-trained models, and adapt pre-trained models onto downstream tasks through fine-tuning. However, there exists an inherent gap between pretext and downstream graph tasks, which insufficiently exerts the ability of pre-trained models and even leads to negative transfer. Meanwhile, prompt tuning has seen emerging success in natural language processing by aligning pre-training and fine-tuning with consistent training objectives. In this paper, we identify the challenges for graph prompt tuning: The first is the lack of a strong and universal pre-training task across sundry pre-training methods in graph domain. The second challenge lies in the difficulty of designing a consistent training objective for both pre-training and downstream tasks. To overcome above obstacles, we propose a novel framework named SGL-PT which follows the learning strategy \`\`Pre-train, Prompt, and Predict''. Specifically, we raise a strong and universal pre-training task coined as SGL that acquires the complementary merits of generative and contrastive self-supervised graph learning. And aiming for graph classification task, we unify pre-training and fine-tuning by designing a novel verbalizer-free prompting function, which reformulates the downstream task in a similar format as pretext task. Empirical results show that our method surpasses other baselines under unsupervised setting, and our prompt tuning method can greatly facilitate models on biological datasets over fine-tuning methods.


---


###


Summary Notes


####


Revolutionizing Graph Neural Networks: An Introduction to SGL-PT


Graph Neural Networks (GNNs) are pivotal in advancing numerous machine learning applications, from analyzing social networks to discovering new drugs.


Yet, the traditional method of adapting these pre-trained models for specific tasks—known as fine-tuning—has its limitations.


This is where SGL-PT (Strong Graph Learner with Graph Prompt Tuning) comes in, offering a groundbreaking framework that improves the integration of pre-training and fine-tuning processes, thereby enhancing the application of GNNs in addressing complex issues.


Let's explore the key elements and benefits of SGL-PT, showcasing its superiority over traditional fine-tuning methods.


###


Identifying the Challenges


- **Need for Universal Pre-training Tasks** : Traditional graph self-supervised learning methods focus more on intra-data (local) relations, leaving inter-data (global) relations between graphs less explored. SGL-PT addresses this by integrating generative and contrastive models to capture a broader spectrum of graph relations.


- **Reformulating Downstream Tasks** : Adapting pre-trained models to downstream tasks is often challenging due to format discrepancies. SGL-PT simplifies this process, ensuring a smoother transition between pre-training and application phases.


###


Core Components of SGL-PT


####


Local and Global Branch Integration


- **Local Branch** : Focuses on enhancing intra-data relations through a graph masked autoencoder approach, improving relational reasoning and robustness.


- **Global Branch** : Aims at better understanding inter-data relations by employing graph contrastive learning, furthering the model's discriminatory capabilities.


- **Integration** : Combines the strengths of both branches, leveraging robustness and discriminative features for a comprehensive learning approach.


####


Advancements in Prompt-based Learning


- **Task Reformulation** : Introduces a masked super node that simplifies adapting the pre-trained model to downstream tasks, effectively summarizing the graph's information.


- **Class Mapping Without Verbalizers** : Utilizes supervised graph prototypical contrastive learning to streamline class mapping, eliminating the need for explicit verbalizer design.


###


Empirical Success


SGL-PT has shown superior performance, especially in settings without supervision, outdoing existing methods.


Its application to biological datasets, in particular, has demonstrated significant improvements, validating its advantages over traditional fine-tuning and other graph prompt methods.


###


Building on Previous Works


SGL-PT extends the legacy of graph self-supervised learning by addressing the shortcomings of previous methods that either focused too narrowly on specific graph relations or struggled to align pre-training with downstream tasks effectively.


###


Conclusion: The Future of GNNs with SGL-PT


SGL-PT marks a significant advancement in GNN applications, overcoming key challenges in graph self-supervised learning. By combining a robust pre-training method with an innovative prompt tuning approach, SGL-PT not only boosts model performance on downstream tasks but also sets a new benchmark for GNN development.


Its success exemplifies the potential of prompt-based learning in graph neural networks, ushering in a new era of more efficient and adaptable solutions in the field.


The "Pre-train, Prompt, and Predict" strategy of SGL-PT reflects the evolving dynamics of machine learning, emphasizing the importance of adaptability and efficiency.


As we continue to explore the capabilities of GNNs, frameworks like SGL-PT provide crucial insights and tools for unlocking the full potential of these powerful models, promising a future of innovative and impactful applications.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
