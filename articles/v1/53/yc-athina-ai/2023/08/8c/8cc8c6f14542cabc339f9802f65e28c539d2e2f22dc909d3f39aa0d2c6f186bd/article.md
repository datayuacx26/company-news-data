---
schema_version: "1.0.0"
document_id: "8cc8c6f14542cabc339f9802f65e28c539d2e2f22dc909d3f39aa0d2c6f186bd"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/you-only-prompt-once-on-the-capabilities-of-prompt-learning-on-large-language-models-to-tackle-toxic-content"
published_at: "2023-08-10T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:0b4bc51389dc7cfc77ae13cf4d7465632ec7a5d8d3be2cdb11a474938fcea31f"
---

# You Only Prompt Once: On the Capabilities of Prompt Learning on Large Language Models to Tackle Toxic Content

Do not index


Original Paper


[https://arxiv.org/abs/2308.05596](https://arxiv.org/abs/2308.05596)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2308.05596](https://arxiv.org/abs/2308.05596)


**By:**[Xinlei He](https://arxiv.org/search/cs?searchtype=author&query=He%2C%20X) ,[Savvas Zannettou](https://arxiv.org/search/cs?searchtype=author&query=Zannettou%2C%20S) ,[Yun Shen](https://arxiv.org/search/cs?searchtype=author&query=Shen%2C%20Y) ,[Yang Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20Y)


**Abstract:**


> The spread of toxic content online is an important problem that has adverse effects on user experience online and in our society at large. Motivated by the importance and impact of the problem, research focuses on developing solutions to detect toxic content, usually leveraging machine learning (ML) models trained on human-annotated datasets. While these efforts are important, these models usually do not generalize well and they can not cope with new trends (e.g., the emergence of new toxic terms). Currently, we are witnessing a shift in the approach to tackling societal issues online, particularly leveraging large language models (LLMs) like GPT-3 or T5 that are trained on vast corpora and have strong generalizability. In this work, we investigate how we can use LLMs and prompt learning to tackle the problem of toxic content, particularly focusing on three tasks; 1) Toxicity Classification, 2) Toxic Span Detection, and 3) Detoxification. We perform an extensive evaluation over five model architectures and eight datasets demonstrating that LLMs with prompt learning can achieve similar or even better performance compared to models trained on these specific tasks. We find that prompt learning achieves around 10\\% improvement in the toxicity classification task compared to the baselines, while for the toxic span detection task we find better performance to the best baseline (0.643 vs. 0.640 in terms of F1-score). Finally, for the detoxification task, we find that prompt learning can successfully reduce the average toxicity score (from 0.775 to 0.213) while preserving semantic meaning.


---


###


Summary Notes


####


Leveraging Large Language Models for Toxic Content Moderation


The internet is a vast space that, unfortunately, includes toxic content. This poses a significant challenge for platforms aiming to foster healthy online communities. Traditional methods of detecting and moderating toxic content often fall short due to the evolving nature of internet slang and the subtleties within harmful content.


However, the emergence of Large Language Models (LLMs) like GPT-3 and T5, equipped with prompt learning, is revolutionizing how we approach content moderation.


This blog post explores how LLMs are making online spaces safer and more inclusive through their ability to effectively identify and mitigate toxic content.


####


The Toxic Content Challenge


Toxic content encompasses bullying, harassment, hate speech, and more, threatening the integrity and inclusiveness of online platforms.


Traditional detection methods, which rely on datasets annotated by humans, struggle to keep up with the dynamic nature of language, often missing new phrases or subtle toxic nuances.


####


The Role of Prompt Learning in LLMs


Prompt learning simplifies the task of instructing LLMs to perform specific tasks, such as detecting toxicity, by using well-crafted prompts.


This method taps into the vast knowledge LLMs acquire during training, enabling them to understand and generate human-like text, making them efficient moderators.


####


Transforming Toxicity Moderation


LLMs with prompt learning are revolutionizing content moderation by:


- **Toxicity Classification:** Asking LLMs if content is toxic helps accurately identify and filter out harmful material.


- **Toxic Span Detection:** LLMs can locate the specific toxic elements within a text, helping moderators address the issue more effectively.


- **Detoxification:** LLMs can rewrite toxic statements into non-harmful ones, maintaining the original message but in a respectful manner.


####


Impact and Performance


A study comparing LLMs to traditional models using eight diverse datasets from various online platforms found that LLMs excelled in toxicity classification, toxic span detection, and content detoxification.


This highlights the effectiveness of prompt learning and its potential to significantly ease the burden on human moderators.


####


Future Implications and Ethical Considerations


The success of LLMs in content moderation opens up possibilities for creating safer online communities.


However, ethical issues such as potential misuse, privacy concerns, and preventing the models from generating toxic content themselves must be addressed.


The study also stresses the need for Green AI, promoting environmentally sustainable AI research practices.


####


Conclusion: A Step Towards a Safer Internet


This research points to a future where digital platforms are safer and more inclusive, thanks to the capabilities of LLMs and prompt learning.


As these models are refined and integrated into real-time moderation systems, the vision of a safer internet becomes closer to reality.


For AI engineers and content moderators, this represents a shift towards more advanced moderation tools and underscores the importance of ethical AI development.


The potential of prompt learning in combating toxic content is a significant step forward in making the internet a place for positive and productive interactions.


To explore the code and datasets used in the study, visit[here](https://github.com/xinleihe/toxic-prompt) .


The journey towards a more respectful and inclusive online environment is challenging, but advancements in AI, especially through the use of LLMs and prompt learning, are equipping us to meet this challenge head-on. Let's continue to advance and ensure the internet remains a space for positive interactions.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
