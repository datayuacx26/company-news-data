---
schema_version: "1.0.0"
document_id: "94ff9f7afc9bb4c1dca2e561ea7aa13dd5bb35a7eb924c94ad21d7b9d3213c0c"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompttts-2-describing-and-generating-voices-with-text-prompt"
published_at: "2023-10-12T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:81a5e8eade7d1847c541f9b41d0c554aee2e79ff1813a13bf9a3b7a48ab13ef4"
---

# PromptTTS 2: Describing and Generating Voices with Text Prompt

Do not index


Original Paper


[https://arxiv.org/abs/2309.02285](https://arxiv.org/abs/2309.02285)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2309.02285](https://arxiv.org/abs/2309.02285)


**By:**[Yichong Leng](https://arxiv.org/search/eess?searchtype=author&query=Leng%2C%20Y) ,[Zhifang Guo](https://arxiv.org/search/eess?searchtype=author&query=Guo%2C%20Z) ,[Kai Shen](https://arxiv.org/search/eess?searchtype=author&query=Shen%2C%20K) ,[Xu Tan](https://arxiv.org/search/eess?searchtype=author&query=Tan%2C%20X) ,[Zeqian Ju](https://arxiv.org/search/eess?searchtype=author&query=Ju%2C%20Z) ,[Yanqing Liu](https://arxiv.org/search/eess?searchtype=author&query=Liu%2C%20Y) ,[Yufei Liu](https://arxiv.org/search/eess?searchtype=author&query=Liu%2C%20Y) ,[Dongchao Yang](https://arxiv.org/search/eess?searchtype=author&query=Yang%2C%20D) ,[Leying Zhang](https://arxiv.org/search/eess?searchtype=author&query=Zhang%2C%20L) ,[Kaitao Song](https://arxiv.org/search/eess?searchtype=author&query=Song%2C%20K) ,[Lei He](https://arxiv.org/search/eess?searchtype=author&query=He%2C%20L) ,[Xiang-Yang Li](https://arxiv.org/search/eess?searchtype=author&query=Li%2C%20X) ,[Sheng Zhao](https://arxiv.org/search/eess?searchtype=author&query=Zhao%2C%20S) ,[Tao Qin](https://arxiv.org/search/eess?searchtype=author&query=Qin%2C%20T) ,[Jiang Bian](https://arxiv.org/search/eess?searchtype=author&query=Bian%2C%20J)


**Abstract:**


> Speech conveys more information than text, as the same word can be uttered in various voices to convey diverse information. Compared to traditional text-to-speech (TTS) methods relying on speech prompts (reference speech) for voice variability, using text prompts (descriptions) is more user-friendly since speech prompts can be hard to find or may not exist at all. TTS approaches based on the text prompt face two main challenges: 1) the one-to-many problem, where not all details about voice variability can be described in the text prompt, and 2) the limited availability of text prompt datasets, where vendors and large cost of data labeling are required to write text prompts for speech. In this work, we introduce PromptTTS 2 to address these challenges with a variation network to provide variability information of voice not captured by text prompts, and a prompt generation pipeline to utilize the large language models (LLM) to compose high quality text prompts. Specifically, the variation network predicts the representation extracted from the reference speech (which contains full information about voice variability) based on the text prompt representation. For the prompt generation pipeline, it generates text prompts for speech with a speech language understanding model to recognize voice attributes (e.g., gender, speed) from speech and a large language model to formulate text prompts based on the recognition results. Experiments on a large-scale (44K hours) speech dataset demonstrate that compared to the previous works, PromptTTS 2 generates voices more consistent with text prompts and supports the sampling of diverse voice variability, thereby offering users more choices on voice generation. Additionally, the prompt generation pipeline produces high-quality text prompts, eliminating the large labeling cost. The demo page of PromptTTS 2 is available online.


---


###


Summary Notes


##


Text-to-Speech Systems: Introducing Voice Variability with Text Prompts


Artificial Intelligence (AI) and machine learning have significantly improved text-to-speech (TTS) systems, making them sound more natural.


Yet, achieving a variety of voices easily remains a challenge. Microsoft Research introduces an innovative solution, PromptTTS 2, which uses text prompts to describe and generate different voice characteristics effortlessly.


###


Key Challenges with Voice Variability in TTS


Creating varied voices in TTS systems faces two main obstacles:


1. **One-to-Many Issue** : A single text prompt might need to represent various voices, complicating the capture of every voice nuance.


1. **Data Gathering Difficulty** : Compiling detailed text prompts that describe voice traits is expensive and time-consuming.


###


Introducing PromptTTS 2


PromptTTS 2 brings new strategies to overcome these challenges:


- **Variation Network** : It fills in the missing information about voice variability from text prompts, using a reference speech and a diffusion model during training. This tackles the one-to-many issue and allows for more flexible voice generation.


- **Prompt Generation Pipeline** : This automated system generates quality text prompts by understanding speech attributes and using a large language model (LLM) to write prompts. It reduces the need for manual prompt creation and enables the development of diverse voice datasets more easily.


###


How PromptTTS 2 Works


PromptTTS 2's system architecture includes:


- **TTS Module** : Responsible for synthesizing speech.


- **Style Module** : Extracts prompt and reference representations, allowing voice synthesis with just text prompts, thanks to the variation network.


###


The Role of LLM in Generating Prompts


Using a large language model for prompt generation is crucial. It detects speech attributes like gender and accent and creates descriptive sentences.


This ensures a wide range of detailed text prompts, leading to more varied voice synthesis.


###


Achievements and Future Possibilities


PromptTTS 2 marks a significant advancement by:


- Introducing a **Diffusion-Based Variation Network** to manage voice variability.


- Creating an **Automated Text Prompt Generation** system, reducing reliance on manual prompts.


- Showing excellent results in **Voice Consistency and Variability** with text prompts.


Looking ahead, there's potential to extract more attributes and apply this technique to other voice generation areas, opening up exciting opportunities for AI developers.


###


Conclusion


PromptTTS 2 represents a major step forward in solving voice variability challenges in TTS systems. By combining variation networks with LLM for prompt generation, it provides a flexible and quality solution for voice generation.


This innovation reduces the need for extensive manual work and paves the way for creating more natural and diverse speech outputs in TTS systems.


####


Further Reading


For those interested in exploring more, reviewing literature on TTS advancements, neural networks, and machine learning applications in speech synthesis is advisable.


This will deepen your understanding of the technologies and innovations driving the future of TTS systems.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
