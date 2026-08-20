---
schema_version: "1.0.0"
document_id: "4d3d9449fedc67646e95f5192fb3917fb0a5f4900c440d6ca94d1359c7d74346"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/post-hoc-explanations-of-language-models-can-improve-language-models"
published_at: "2023-05-19T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:2e3fe6e322a0fff5b77d11730a91f9e31ada2b7878f4557e91173aaee39a4cca"
---

# Post Hoc Explanations of Language Models Can Improve Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2305.11426](https://arxiv.org/abs/2305.11426)


Blog URL


[blog.athina.ai /post-ho...e-models](https://blog.athina.ai/post-hoc-explanations-of-language-models-can-improve-language-models)


**Original Paper:**[https://arxiv.org/abs/2305.11426](https://arxiv.org/abs/2305.11426)


**By:**[Satyapriya Krishna](https://arxiv.org/search/cs?searchtype=author&query=Krishna%2C%20S) ,[Jiaqi Ma](https://arxiv.org/search/cs?searchtype=author&query=Ma%2C%20J) ,[Dylan Slack](https://arxiv.org/search/cs?searchtype=author&query=Slack%2C%20D) ,[Asma Ghandeharioun](https://arxiv.org/search/cs?searchtype=author&query=Ghandeharioun%2C%20A) ,[Sameer Singh](https://arxiv.org/search/cs?searchtype=author&query=Singh%2C%20S) ,[Himabindu Lakkaraju](https://arxiv.org/search/cs?searchtype=author&query=Lakkaraju%2C%20H)


**Abstract:**


> Large Language Models (LLMs) have demonstrated remarkable capabilities in performing complex tasks. Moreover, recent research has shown that incorporating human-annotated rationales (e.g., Chain-of-Thought prompting) during in-context learning can significantly enhance the performance of these models, particularly on tasks that require reasoning capabilities. However, incorporating such rationales poses challenges in terms of scalability as this requires a high degree of human involvement. In this work, we present a novel framework, Amplifying Model Performance by Leveraging In-Context Learning with Post Hoc Explanations (AMPLIFY), which addresses the aforementioned challenges by automating the process of rationale generation. To this end, we leverage post hoc explanation methods which output attribution scores (explanations) capturing the influence of each of the input features on model predictions. More specifically, we construct automated natural language rationales that embed insights from post hoc explanations to provide corrective signals to LLMs. Extensive experimentation with real-world datasets demonstrates that our framework, AMPLIFY, leads to prediction accuracy improvements of about 10-25% over a wide range of tasks, including those where prior approaches which rely on human-annotated rationales such as Chain-of-Thought prompting fall short. Our work makes one of the first attempts at highlighting the potential of post hoc explanations as valuable tools for enhancing the effectiveness of LLMs. Furthermore, we conduct additional empirical analyses and ablation studies to demonstrate the impact of each of the components of AMPLIFY, which, in turn, leads to critical insights for refining in-context learning.


---


###


Summary Notes


####


Improving Language Models with Automated Rationales: The AMPLIFY Framework


In the ever-evolving world of artificial intelligence, Large Language Models (LLMs) stand out for their ability to perform complex tasks such as generating text, answering questions, and coding. Despite their advanced capabilities, further improving these models presents challenges. The introduction of human-annotated rationales has shown promise but struggles with scalability. Here's where the AMPLIFY framework comes in, automating rationale generation to significantly enhance LLM performance.


####


Overview of AMPLIFY


The AMPLIFY framework builds on several key steps to improve LLMs:


- **Choosing a Proxy Model** : It starts with a smaller model like GPT-2 or BERT to make generating explanations feasible.


- **Identifying Misclassified Samples** : It then focuses on samples that LLMs misclassify, especially those the proxy model is confident it has misclassified.


- **Automating Rationale Generation** : By applying explainability techniques, AMPLIFY creates natural language rationales.


- **Creating Few-shot Prompts** : These automated rationales are then used to construct prompts that significantly boost the LLM’s task performance.


####


Testing AMPLIFY


The framework was tested using the Big-Bench-Hard benchmark, comparing it to traditional prompting methods. AMPLIFY showed a 10-25% improvement across various tasks. Key factors in its success include:


- **Proxy Model Choice** : The selection is crucial, though fine-tuning the proxy model offered minimal benefits.


- **Strategy for Sample Selection** : Selecting samples based on a High Misclassification Confidence Score was most effective.


- **Methods for Generating Explanations** : Techniques like Gradient x Input and its contrastive version were most useful.


- **Tailoring Rationale Templates** : Customizing templates for specific tasks slightly improved outcomes.


####


Key Takeaways


- AMPLIFY outperforms traditional prompting methods, underscoring the value of automated rationales.


- The default capabilities of proxy models are generally adequate, with fine-tuning offering little extra benefit.


- Focusing on samples where the proxy model is highly confident in its misclassification is a winning strategy.


- The choice of explanation method and the customization of rationale templates are important for maximizing performance.


####


Conclusion


The AMPLIFY framework marks a significant advance in improving LLM performance by automating the generation of rationales. This approach not only enhances efficiency and effectiveness but also opens up new possibilities for AI development. As we move forward, the role of post hoc explanations in advancing LLMs is clear, with AMPLIFY leading the way.


####


Acknowledgments and Funding


This work was supported by NSF awards, Google, JP Morgan, Amazon, the Harvard Data Science Initiative, the Digital, Data, and Design Institute at Harvard, and the Hasso Plattner Institute through the UCI-HPI fellowship.


####


Appendices


- **Appendix A** : Offers a deep dive into proxy model performance and the effects of fine-tuning.


- **Appendix B** : Discusses the limitations and broader impacts of LLMs and post hoc explanations.


- **Appendix C** : Explores further experiments with larger proxy models and additional tasks.


####


Reporting Errors


We encourage feedback on any errors found, helping us improve the quality of our research. Your input is greatly appreciated.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
