---
schema_version: "1.0.0"
document_id: "14c9cc6d80539c80fd1d83fcd4edceb223800e090d656e97b72e171a5df2b664"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/model-tuning-via-prompts-makes-nlp-models-adversarially-robust"
published_at: "2023-03-13T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:e6be5bcc86a148dc609968dfda0ca69fabff7a955608f4087c5010e5b260d2ea"
---

# Model-tuning Via Prompts Makes NLP Models Adversarially Robust

Do not index


Original Paper


[https://arxiv.org/abs/2303.07320](https://arxiv.org/abs/2303.07320)


Blog URL


[blog.athina.ai /model-t...y-robust](https://blog.athina.ai/model-tuning-via-prompts-makes-nlp-models-adversarially-robust)


**Original Paper:**[https://arxiv.org/abs/2303.07320](https://arxiv.org/abs/2303.07320)


**By:**[Mrigank Raman](https://arxiv.org/search/cs?searchtype=author&query=Raman%2C%20M) ,[Pratyush Maini](https://arxiv.org/search/cs?searchtype=author&query=Maini%2C%20P) ,[J. Zico Kolter](https://arxiv.org/search/cs?searchtype=author&query=Kolter%2C%20J%20Z) ,[Zachary C. Lipton](https://arxiv.org/search/cs?searchtype=author&query=Lipton%2C%20Z%20C) ,[Danish Pruthi](https://arxiv.org/search/cs?searchtype=author&query=Pruthi%2C%20D)


**Abstract:**


> In recent years, NLP practitioners have converged on the following practice: (i) import an off-the-shelf pretrained (masked) language model; (ii) append a multilayer perceptron atop the CLS token's hidden representation (with randomly initialized weights); and (iii) fine-tune the entire model on a downstream task (MLP-FT). This procedure has produced massive gains on standard NLP benchmarks, but these models remain brittle, even to mild adversarial perturbations. In this work, we demonstrate surprising gains in adversarial robustness enjoyed by Model-tuning Via Prompts (MVP), an alternative method of adapting to downstream tasks. Rather than appending an MLP head to make output prediction, MVP appends a prompt template to the input, and makes prediction via text infilling/completion. Across 5 NLP datasets, 4 adversarial attacks, and 3 different models, MVP improves performance against adversarial substitutions by an average of 8% over standard methods and even outperforms adversarial training-based state-of-art defenses by 3.5%. By combining MVP with adversarial training, we achieve further improvements in adversarial robustness while maintaining performance on unperturbed examples. Finally, we conduct ablations to investigate the mechanism underlying these gains. Notably, we find that the main causes of vulnerability of MLP-FT can be attributed to the misalignment between pre-training and fine-tuning tasks, and the randomly initialized MLP parameters.


---


###


Summary Notes


####


Enhancing NLP Model Robustness with Model-tuning Via Prompts (MVP)


In the fast-changing digital world, making Natural Language Processing (NLP) models strong against adversarial attacks and unexpected data (out-of-distribution or OOD) is crucial for AI engineers in large companies.


A new method called Model-tuning Via Prompts (MVP) promises better results than traditional fine-tuning techniques (MLP-FT) in both effectiveness and efficiency.


####


Traditional Fine-tuning vs. MVP: A Detailed Comparison


Traditionally, fine-tuning pre-trained language models for specific tasks involved adding a multi-layer perceptron (MLP) and adjusting this larger model.


This method works but makes models more open to attacks due to new, untrained parameters.


MVP, on the other hand, adds a prompt template to inputs and predicts by filling in the blanks. This method, which more closely mimics how models were initially trained, has several advantages:


- **Better at Fighting Adversarial Attacks** : MVP outperforms traditional methods across different tests, models, and types of attacks.


- **Great Results without Special Training** : Without needing extra defense training, MVP still beats top methods, showing better performance against adversarial tricks.


- **Even Stronger with Special Defense Training** : Using adversarial training with MVP further boosts its defense capabilities without losing accuracy.


####


Why MVP Works So Well


MVP's success comes from its closer match to the original training tasks of models, avoiding the introduction of untrained parameters that weaken MLP-FT. Using various prompts and potential answers for each category also helps improve security.


####


Efficiency and Robustness


MVP shines in more areas than just defense:


- **Needs Fewer Examples** : MVP achieves good accuracy with fewer training examples than MLP-FT, making it more efficient.


- **Better Defense for the Accuracy** : At similar levels of accuracy, MVP offers better protection, showing it's more effectively robust.


####


Out-of-Distribution (OOD) Defense


MVP also does better at handling OOD scenarios in sentiment analysis tasks, important for the real-world application of NLP models where data can greatly differ from training sets.


####


Insights from Human Testing


A study with people reviewing adversarial examples showed that humans could often spot the manipulations and performed worse on these trick examples than on normal ones. This underlines the need for robust NLP models.


####


Limitations and Impact


While MVP is promising for improving NLP model robustness, the study focused on models smaller than 1B parameters and tasks commonly fine-tuned with an MLP head. The findings might not apply to larger models or different tasks. Also, using multiple templates with MVP can slow down response times, but the trade-off for better model security and reliability might be worth it for AI engineers.


####


Conclusion


Model-tuning Via Prompts (MVP) presents a powerful and efficient way to make NLP models more robust against adversarial and OOD scenarios.


By sticking closer to the original training methods and avoiding new, untrained parameters, MVP significantly improves over traditional fine-tuning. Its efficiency, defense capabilities, and better handling of OOD scenarios make MVP an appealing choice for enhancing model reliability. AI engineers at large companies should consider using MVP for fine-tuning to ensure their NLP systems are secure and reliable in various situations.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
