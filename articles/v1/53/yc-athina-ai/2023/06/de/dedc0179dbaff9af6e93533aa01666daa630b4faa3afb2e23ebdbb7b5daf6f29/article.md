---
schema_version: "1.0.0"
document_id: "dedc0179dbaff9af6e93533aa01666daa630b4faa3afb2e23ebdbb7b5daf6f29"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-algebra-for-task-composition"
published_at: "2023-06-01T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:ce1eccd019d494f9780c7214a18ad946887b86018eefbccf8e530560da391674"
---

# Prompt Algebra for Task Composition

Do not index


Original Paper


[https://arxiv.org/abs/2306.00310](https://arxiv.org/abs/2306.00310)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2306.00310](https://arxiv.org/abs/2306.00310)


**By:**[Pramuditha Perera](https://arxiv.org/search/cs?searchtype=author&query=Perera%2C%20P) ,[Matthew Trager](https://arxiv.org/search/cs?searchtype=author&query=Trager%2C%20M) ,[Luca Zancato](https://arxiv.org/search/cs?searchtype=author&query=Zancato%2C%20L) ,[Alessandro Achille](https://arxiv.org/search/cs?searchtype=author&query=Achille%2C%20A) ,[Stefano Soatto](https://arxiv.org/search/cs?searchtype=author&query=Soatto%2C%20S)


**Abstract:**


> We investigate whether prompts learned independently for different tasks can be later combined through prompt algebra to obtain a model that supports composition of tasks. We consider Visual Language Models (VLM) with prompt tuning as our base classifier and formally define the notion of prompt algebra. We propose constrained prompt tuning to improve performance of the composite classifier. In the proposed scheme, prompts are constrained to appear in the lower dimensional subspace spanned by the basis vectors of the pre-trained vocabulary. Further regularization is added to ensure that the learned prompt is grounded correctly to the existing pre-trained vocabulary. We demonstrate the effectiveness of our method on object classification and object-attribute classification datasets. On average, our composite model obtains classification accuracy within 2.5% of the best base model. On UTZappos it improves classification accuracy over the best base model by 8.45% on average.


---


###


Summary Notes


####


Enhancing AI with Prompt Algebra for Better Task Handling


###


Introduction


The world of artificial intelligence (AI) is witnessing a new era where pre-trained models are becoming more flexible and efficient in tackling complex tasks.


The introduction of prompt algebra is a game-changer, allowing the combination of soft-prompt tokens to improve how AI models handle multiple tasks at once.


###


Exploring the Background


####


The Rise of Vision Language Models (VLMs) and Prompt Tuning


- VLMs like VL-BERT and VILBERT have made significant strides in handling tasks that involve both visual and textual inputs.


- The introduction of models like CLIP and ALBEF has further enhanced the ability of AI to perform well in tasks with limited examples (zero-shot and few-shot learning).


- Prompt tuning is a technique used in natural language processing to direct AI models towards specific tasks by optimizing certain vectors, improving their performance.


####


Understanding the Basics


- **VLMs Classification:** These models work by aligning text features with visual features to classify images.


- **Prompt Tuning in VLMs:** This involves adding a special token to the text input that the model learns to optimize during training, helping it focus better on the desired task.


###


Improving Task Composition with Prompt Algebra


Prompt algebra introduces a method to linearly combine prompts, maintaining their compositional properties. This approach aims to:


- Increase the flexibility of VLMs in handling complex tasks.


- Ensure that prompts are correctly grounded, reducing interference between them.


###


Testing the Approach: Experiments


The research tested the method on datasets involving object and object-attribute classifications. The findings revealed:


- The composite classifier maintained its performance on original tasks while improving in cross-task performance.


- Regularization techniques helped keep learned prompts grounded, enhancing the classifier's robustness and composition skills.


###


The Takeaway


The study shows that prompt algebra is effective in merging independently trained models to handle multiple tasks better.


The use of constrained prompt tuning further boosts the performance of these composite classifiers. This breakthrough suggests a promising path forward for making AI models more adaptable and efficient.


###


Key Highlights


- **Prompt Algebra's Success:** Proof that combining prompts through prompt algebra enhances task handling in AI.


- **The Constrained Prompt Tuning Method:** A new approach that secures prompts in the pre-trained vocabulary, refining the model's efficiency.


###


Looking Ahead


Future directions include:


- Extending the method to work with multiple prompts.


- Creating an automated system for balancing different prompts during combination for optimized task performance.


The integration of prompt algebra and constrained prompt tuning marks a significant advancement in AI, paving the way for more versatile and capable models.


As AI evolves, these methodologies are set to play a pivotal role in the development of technology and its applications.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
