---
schema_version: "1.0.0"
document_id: "282d2f2b2ef0834b5397f6d23dc4672509dd07ba62f92fadbb52e4560c5e9572"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/speechprompt-v2-prompt-tuning-for-speech-classification-tasks"
published_at: "2023-03-01T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:1d9d905e82e4ee120939de0edddaab34b666f0aade1f923b26fc243e70f054ba"
---

# SpeechPrompt v2: Prompt Tuning for Speech Classification Tasks

Do not index


Original Paper


[https://arxiv.org/abs/2303.00733](https://arxiv.org/abs/2303.00733)


Blog URL


[blog.athina.ai /speechp...on-tasks](https://blog.athina.ai/speechprompt-v2-prompt-tuning-for-speech-classification-tasks)


**Original Paper:**[https://arxiv.org/abs/2303.00733](https://arxiv.org/abs/2303.00733)


**By:**[Kai-Wei Chang](https://arxiv.org/search/eess?searchtype=author&query=Chang%2C%20K) ,[Yu-Kai Wang](https://arxiv.org/search/eess?searchtype=author&query=Wang%2C%20Y) ,[Hua Shen](https://arxiv.org/search/eess?searchtype=author&query=Shen%2C%20H) ,[Iu-thing Kang](https://arxiv.org/search/eess?searchtype=author&query=Kang%2C%20I) ,[Wei-Cheng Tseng](https://arxiv.org/search/eess?searchtype=author&query=Tseng%2C%20W) ,[Shang-Wen Li](https://arxiv.org/search/eess?searchtype=author&query=Li%2C%20S) ,[Hung-yi Lee](https://arxiv.org/search/eess?searchtype=author&query=Lee%2C%20H)


**Abstract:**


> Prompt tuning is a technology that tunes a small set of parameters to steer a pre-trained language model (LM) to directly generate the output for downstream tasks. Recently, prompt tuning has demonstrated its storage and computation efficiency in both natural language processing (NLP) and speech processing fields. These advantages have also revealed prompt tuning as a candidate approach to serving pre-trained LM for multiple tasks in a unified manner. For speech processing, SpeechPrompt shows its high parameter efficiency and competitive performance on a few speech classification tasks. However, whether SpeechPrompt is capable of serving a large number of tasks is unanswered. In this work, we propose SpeechPrompt v2, a prompt tuning framework capable of performing a wide variety of speech classification tasks, covering multiple languages and prosody-related tasks. The experiment result shows that SpeechPrompt v2 achieves performance on par with prior works with less than 0.15M trainable parameters in a unified framework.


---


###


Summary Notes


##


SpeechPrompt v2: Simplifying Speech Classification with Prompt Tuning


The world of speech processing is rapidly evolving, with pre-trained models at the forefront of this transformation. These models have greatly benefited from using large amounts of unlabeled data, leading to more versatile and powerful applications. However, as the variety of speech processing tasks grows, the traditional method of fine-tuning these models is becoming less feasible due to high computational and storage costs.


Prompt tuning offers a solution by tweaking a pre-trained language model with task-specific prompts, making it a resource-efficient alternative. This blog post explores SpeechPrompt v2, a leading approach in prompt tuning for speech classification tasks.


####


Understanding Prompt Tuning


Prompt tuning is a method that fine-tunes a small number of parameters in a pre-trained language model to adapt it for specific tasks.


This approach is gaining popularity for its ability to conserve computational resources and storage space across various tasks in natural language processing (NLP) and speech processing.


####


Background and Related Work


Prompting techniques were first embraced in NLP and have since been adapted for speech processing.


Innovations like WAVPROMPT and the original SpeechPrompt have shown promise in applying prompt tuning to speech classification and generation tasks.


However, the capability of SpeechPrompt to handle a wide range of speech processing tasks required further investigation.


####


SpeechPrompt v2 Methodology


SpeechPrompt v2 is designed to efficiently apply prompt tuning to various speech classification challenges. It uses:


- A pre-trained spoken language model with fixed parameters, except for the prompt vectors, which are trainable.


- A novel, learnable verbalizer that improves classification performance compared to the earlier version's frequency-based verbalizer.


####


Testing SpeechPrompt v2


The evaluation covered diverse speech classification tasks, including speech command recognition, intent classification, and emotion recognition, across multiple languages.


The key datasets used were Google Speech Commands and Voxforge. Despite the variety of tasks, SpeechPrompt v2 maintained a consistent architecture with a minimal number of trainable parameters.


####


Results


With less than 0.15M trainable parameters, SpeechPrompt v2 achieved superior performance in tasks like Lithuanian and Arabic speech command recognition and sarcasm detection. These results highlight the efficiency and effectiveness of prompt tuning in speech classification.


####


Conclusion


SpeechPrompt v2 marks a significant advancement in speech classification, offering an efficient and scalable framework that incorporates a learnable verbalizer for improved performance. Future work will focus on enhancing the stability of prompt tuning and expanding its applications to a wider range of tasks and languages.


####


Acknowledgements


This project was supported by generous contributions from Amazon, Microsoft, and Google during the 2022 Jelinek Memorial Summer Workshop on Speech and Language Technologies at Johns Hopkins University.


####


Final Thoughts


SpeechPrompt v2 represents a promising development in speech processing, providing a scalable and efficient alternative to traditional model fine-tuning.


Its ability to perform across various speech classification tasks with minimal computational requirements positions it as a valuable tool for AI engineers looking to advance speech technology capabilities.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
