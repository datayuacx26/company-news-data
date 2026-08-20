---
schema_version: "1.0.0"
document_id: "f9ab621336ef44df3f21e6596adf0768159a9dd7f7ce0be794d19b2ff847d6df"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/quantifying-language-models-sensitivity-to-spurious-features-in-prompt-design-or-how-i-learned-to-start-worrying-about-prompt-formatting"
published_at: "2023-10-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:c780a806047870731de8ddb5237f1ae455db44d4fbfe9094918bced27e453c82"
---

# Quantifying Language Models' Sensitivity to Spurious Features in Prompt Design or: How I learned to start worrying about prompt formatting

Do not index


Original Paper


[https://arxiv.org/abs/2310.11324](https://arxiv.org/abs/2310.11324)


Blog URL


[blog.athina.ai /quantif...rmatting](https://blog.athina.ai/quantifying-language-models-sensitivity-to-spurious-features-in-prompt-design-or-how-i-learned-to-start-worrying-about-prompt-formatting)


**Original Paper:**[https://arxiv.org/abs/2310.11324](https://arxiv.org/abs/2310.11324)


**By:**[Melanie Sclar](https://arxiv.org/search/cs?searchtype=author&query=Sclar%2C%20M) ,[Yejin Choi](https://arxiv.org/search/cs?searchtype=author&query=Choi%2C%20Y) ,[Yulia Tsvetkov](https://arxiv.org/search/cs?searchtype=author&query=Tsvetkov%2C%20Y) ,[Alane Suhr](https://arxiv.org/search/cs?searchtype=author&query=Suhr%2C%20A)


**Abstract:**


> As large language models (LLMs) are adopted as a fundamental component of language technologies, it is crucial to accurately characterize their performance. Because choices in prompt design can strongly influence model behavior, this design process is critical in effectively using any modern pre-trained generative language model. In this work, we focus on LLM sensitivity to a quintessential class of meaning-preserving design choices: prompt formatting. We find that several widely used open-source LLMs are extremely sensitive to subtle changes in prompt formatting in few-shot settings, with performance differences of up to 76 accuracy points when evaluated using LLaMA-2-13B. Sensitivity remains even when increasing model size, the number of few-shot examples, or performing instruction tuning. Our analysis suggests that work evaluating LLMs with prompting-based methods would benefit from reporting a range of performance across plausible prompt formats, instead of the currently-standard practice of reporting performance on a single format. We also show that format performance only weakly correlates between models, which puts into question the methodological validity of comparing models with an arbitrarily chosen, fixed prompt format. To facilitate systematic analysis we propose FormatSpread, an algorithm that rapidly evaluates a sampled set of plausible prompt formats for a given task, and reports the interval of expected performance without accessing model weights. Furthermore, we present a suite of analyses that characterize the nature of this sensitivity, including exploring the influence of particular atomic perturbations and the internal representation of particular formats.


---


###


Summary Notes


##


How Prompt Design Influences Language Model Performance: An In-Depth Look


In the dynamic field of artificial intelligence, language models are key players, enhancing natural language processing, machine learning, and AI overall.


One critical, yet often overlooked aspect that affects these models' performance is prompt design. This post explores the significant impact of prompt design on language models, highlighting its importance and offering practical solutions for AI engineers in the corporate sector.


###


Introduction


The success of a language model is not solely dependent on its architecture or the data it was trained on. The design of the prompts it receives plays a vital role.


The format, wording, and even minor details like capitalization and punctuation can greatly influence the model's output.


The effect of prompt design on model performance has not received enough attention, creating a gap in our understanding and optimization of AI systems.


###


Key Findings on Prompt Design


Recent studies have brought to light how sensitive language models are to the design of prompts. Key findings include:


- **Performance Variability** : Slight changes in how prompts are formatted can cause big differences in model performance. For example, changing the capitalization of instructions can unpredictably affect the results.


- **Format Sensitivity** : This effect is consistent across various model sizes and configurations, suggesting that prompt design choices add an unpredictable factor that could skew how models are evaluated.


- **Weak Performance Correlation** : The common practice of comparing models using a standard prompt format might be flawed, given the weak correlation between prompt format and model performance.


###


Solutions


To tackle these issues, we propose two approaches:


####


FORMAT SPREAD Algorithm


- Analyze how performance varies with different, yet semantically equivalent, prompt formats.


- Estimate the range of performance users might experience, offering a more accurate basis for evaluating models.


####


Bayesian Optimization


- Explore different prompt formats efficiently to find the best configurations.


- Aim to get the most performance insight for the least computational cost.


###


Methodology


Our study suggests a new way to evaluate prompt design:


####


Grammar of Plausible Formats


- Create a comprehensive set of rules to generate many semantically equivalent prompt formats.


- Make sure these different formats are meaningful and comparable.


####


Performance Measurement


- Assess how different formatting choices impact model performance by testing a variety of generated prompts.


- Use this data to improve and fine-tune prompt design for better model performance.


###


Analysis Tools


This research introduces two important tools:


####


FORMAT SPREAD


- Test a wide range of formats to understand how sensitive models are to formatting changes.


- Works well even for models accessed only via API, making it versatile for various evaluation scenarios.


####


Bayesian Optimization


- Predict how performance changes with different formats.


- Streamline prompt design for better efficiency and results.


###


Implications and Future Directions


This research highlights the need for:


- **Reporting Standards** : Future studies should test models with multiple prompt formats to draw stronger conclusions.


- **User Experience** : Understanding how prompt design affects interaction can greatly improve how users engage with language models.


###


Conclusion


The complex relationship between prompt design and language model performance calls for a more sophisticated approach to model testing and evaluation.


Introducing strategies like FORMAT SPREAD and tools such as Bayesian Optimization marks a step towards more reliable and thorough evaluations.


For AI engineers in the business world, adopting these insights and methods can enhance the efficiency and effectiveness of AI model deployment and use, ensuring language models reach their full potential in real-world applications.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
