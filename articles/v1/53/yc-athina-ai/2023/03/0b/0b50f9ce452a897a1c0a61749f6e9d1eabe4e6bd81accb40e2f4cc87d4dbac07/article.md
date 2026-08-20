---
schema_version: "1.0.0"
document_id: "0b50f9ce452a897a1c0a61749f6e9d1eabe4e6bd81accb40e2f4cc87d4dbac07"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/context-faithful-prompting-for-large-language-models"
published_at: "2023-03-20T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:62acf684b4d6bc20d00c46326ff5b51b19a794a0bf07c9a0ce0d6fa1c0e0e6f2"
---

# Context-faithful Prompting for Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2303.11315](https://arxiv.org/abs/2303.11315)


Blog URL


[blog.athina.ai /context...e-models](https://blog.athina.ai/context-faithful-prompting-for-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2303.11315](https://arxiv.org/abs/2303.11315)


**By:**[Wenxuan Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20W) ,[Sheng Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20S) ,[Hoifung Poon](https://arxiv.org/search/cs?searchtype=author&query=Poon%2C%20H) ,[Muhao Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20M)


**Abstract:**


> Large language models (LLMs) encode parametric knowledge about world facts and have shown remarkable performance in knowledge-driven NLP tasks. However, their reliance on parametric knowledge may cause them to overlook contextual cues, leading to incorrect predictions in context-sensitive NLP tasks (e.g., knowledge acquisition tasks). In this paper, we seek to assess and enhance LLMs' contextual faithfulness in two aspects: knowledge conflict and prediction with abstention. We demonstrate that LLMs' faithfulness can be significantly improved using carefully designed prompting strategies. In particular, we identify opinion-based prompts and counterfactual demonstrations as the most effective methods. Opinion-based prompts reframe the context as a narrator's statement and inquire about the narrator's opinions, while counterfactual demonstrations use instances containing false facts to improve faithfulness in knowledge conflict situations. Neither technique requires additional training. We conduct experiments on three datasets of two standard NLP tasks, machine reading comprehension and relation extraction, and the results demonstrate significant improvement in faithfulness to contexts. Code and data are released at
>
>
> [this https URL](https://github.com/wzhouad/context-faithful-llm)


---


###


Summary Notes


####


Making Large Language Models More Context-Aware


In the exciting world of artificial intelligence (AI), Large Language Models (LLMs) like GPT-3 stand out for their ability to mimic human-like text.


They excel in many tasks, from essay writing to programming.


However, they struggle with staying up-to-date or correcting false information.


This is a big issue, especially when accuracy and current knowledge are crucial.


####


The Challenge with Context


LLMs learn from huge datasets, capturing knowledge until a certain date.


This means they have a vast understanding of language and facts, but this knowledge is frozen in time. When new situations arise or information changes, LLMs often can't keep up.


They either repeat what they've learned or miss the mark on adapting to new contexts.


####


Core Issues


LLMs face two main problems due to their static knowledge:


- **Knowledge Conflicts** : Their reliance on old information can clash with new facts.


- **Prediction Abstention** : LLMs find it hard to know when they lack enough information, leading to possible inaccuracies.


####


Innovative Approaches


Recent studies have looked into making LLMs better at handling new contexts and knowing when to hold back on predictions. Two promising methods are:


- **Opinion-based Prompting** : This technique asks LLMs for opinions, pushing them to weigh new information instead of just what they know.


- **Counterfactual Demonstrations** : This involves giving LLMs examples with false facts to teach them to focus on the prompt's context over their stored knowledge.


Testing these methods has shown they help LLMs respond better to new situations.


####


Discoveries


The research found:


- LLMs can rely less on outdated information with the right prompting techniques.


- Improved models are better at knowing when not to make a prediction, reducing misinformation.


- Bigger models adapt better but also tend to memorize more, showing a balance between adaptability and knowledge retention.


####


Looking Ahead


These findings are a big step in making LLMs more adaptable to context.


Techniques like opinion-based prompting and counterfactual demonstrations can make LLMs more accurate and reliable. Future research might explore applying these methods to more complex tasks, different languages, and standard training processes.


This could make LLMs more useful and sophisticated.


####


Ethical Considerations


Improving LLMs also brings ethical concerns, especially about biases in training data. Moving forward, it's vital to develop and use these technologies responsibly.


This research has been backed by the NSF Grant IIS-2105329 and the DARPA MCS program, showing the collaborative effort behind these advancements.


####


Conclusion


The quest to make LLMs more context-aware and adaptable is progressing.


By tackling the challenges of static knowledge and prediction abstention, we're getting closer to LLMs that are not just smarter but also more in tune with the ever-changing context of human language and knowledge.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
