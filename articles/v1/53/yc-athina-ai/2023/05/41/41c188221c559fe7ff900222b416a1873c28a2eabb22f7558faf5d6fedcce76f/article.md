---
schema_version: "1.0.0"
document_id: "41c188221c559fe7ff900222b416a1873c28a2eabb22f7558faf5d6fedcce76f"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/less-likely-brainstorming-using-language-models-to-generate-alternative-hypotheses"
published_at: "2023-05-30T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:fecdbbccb6f7d0e8d94022763e6fc5c28d096a5ef628387900198ef2b28f1fc4"
---

# Less Likely Brainstorming: Using Language Models to Generate Alternative Hypotheses

Do not index


Original Paper


h[ttps://arxiv.org/abs/2305.19339](https://arxiv.org/abs/2305.19339)


Blog URL


[blog.athina.ai /less-li...potheses](https://blog.athina.ai/less-likely-brainstorming-using-language-models-to-generate-alternative-hypotheses)


**Original Paper:**[https://arxiv.org/abs/2305.19339](https://arxiv.org/abs/2305.19339)


**By:**[Liyan Tang](https://arxiv.org/search/cs?searchtype=author&query=Tang%2C%20L) ,[Yifan Peng](https://arxiv.org/search/cs?searchtype=author&query=Peng%2C%20Y) ,[Yanshan Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Y) ,[Ying Ding](https://arxiv.org/search/cs?searchtype=author&query=Ding%2C%20Y) ,[Greg Durrett](https://arxiv.org/search/cs?searchtype=author&query=Durrett%2C%20G) ,[Justin F. Rousseau](https://arxiv.org/search/cs?searchtype=author&query=Rousseau%2C%20J%20F)


**Abstract:**


> A human decision-maker benefits the most from an AI assistant that corrects for their biases. For problems such as generating interpretation of a radiology report given findings, a system predicting only highly likely outcomes may be less useful, where such outcomes are already obvious to the user. To alleviate biases in human decision-making, it is worth considering a broad differential diagnosis, going beyond the most likely options. We introduce a new task, "less likely brainstorming," that asks a model to generate outputs that humans think are relevant but less likely to happen. We explore the task in two settings: a brain MRI interpretation generation setting and an everyday commonsense reasoning setting. We found that a baseline approach of training with less likely hypotheses as targets generates outputs that humans evaluate as either likely or irrelevant nearly half of the time; standard MLE training is not effective. To tackle this problem, we propose a controlled text generation method that uses a novel contrastive learning strategy to encourage models to differentiate between generating likely and less likely outputs according to humans. We compare our method with several state-of-the-art controlled text generation models via automatic and human evaluations and show that our models' capability of generating less likely outputs is improved.


---


###


Summary Notes


##


Enhancing AI Decision-Making with "Less Likely Brainstorming"


In the fast-paced world of artificial intelligence (AI), finding ways to minimize cognitive biases in decision-making is increasingly important.


These biases, such as the tendency to favor information that confirms pre-existing beliefs, can affect decision quality, especially in critical areas like healthcare.


This post introduces "less likely brainstorming," a method that uses language models to create alternative ideas or interpretations to counteract these biases.


This technique is particularly useful for AI engineers in large organizations and has significant implications for improving decision-making in clinical environments.


###


Understanding Cognitive Biases


Cognitive biases in clinical settings can lead to errors, as clinicians might ignore evidence contradicting their initial diagnosis.


Generating a wider range of diagnoses can help reduce these biases. "Less likely brainstorming" addresses this by creating less obvious but possible interpretations or hypotheses.


###


How It Works: Using Language Models


This method relies on language models to generate less obvious but still relevant outcomes from given data. It has been tested in two scenarios: interpreting brain MRI results and applying everyday commonsense reasoning. A key part of this approach is a new contrastive learning strategy called BRAIN STORM, which distinguishes between more and less likely outcomes. This involves extra learning objectives to understand the relationship between the context, an indicator of likelihood, and the outcomes.


####


Implementation Details


- **Evaluation Methods** : The effectiveness is assessed using automated metrics and human evaluations.


- **Data Used** : Tests were conducted using datasets for brain MRI interpretations and commonsense reasoning tasks.


- **Benchmarking** : The models were benchmarked against standard text generation models and new methods introduced in this research.


###


Results: Broadening Perspectives


The findings show that the model successfully generates less common diagnoses from brain MRI data without compromising relevance. For everyday reasoning, it produces unexpected yet plausible outcomes. These results suggest that this method can foster creative thinking and lessen bias in decision-making.


###


Implications for AI in Decision-Making


This research underlines the value of generating unusual interpretations to broaden the range of diagnoses considered by clinicians, potentially improving patient care.


It shows how AI can help combat human cognitive biases in critical decision-making, offering a positive direction in using technology to enhance healthcare decisions and beyond.


###


Looking Ahead: A Bright Future for AI


Introducing "less likely brainstorming" for AI models marks a step forward in tackling human cognitive biases.


This technique could significantly improve decision-making across various sectors, notably in healthcare where the consequences of bias are particularly severe.


###


Future Directions


Further studies are recommended to refine the models to avoid irrelevant outcomes and to apply this method in more clinical and reasoning scenarios. Ongoing improvements will be key to ensuring the method's real-world efficacy and applicability.


###


Code Sharing


For those interested in further exploration or development, the study's code is available on GitHub. Sharing resources like this is essential for encouraging innovation and collaboration in the AI field.


In summary, "less likely brainstorming" showcases the potential of AI to aid in decision-making and reduce biases, particularly in critical areas such as healthcare.


By offering different perspectives and challenging common thought patterns, AI can significantly improve decision-making processes and outcomes.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
