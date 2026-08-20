---
schema_version: "1.0.0"
document_id: "7522df69243bdc026ca038beca3d82f54c4158a3c84f197f7bba2f5516f6750a"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/pbnr-prompt-based-news-recommender-system"
published_at: "2023-04-16T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:d1c1fa30e94082ddf00323b586ac8a5ca57e6ac28c2065fa899ef53039f5a8c8"
---

# PBNR: Prompt-based News Recommender System

Do not index


Original Paper


[https://arxiv.org/abs/2304.07862](https://arxiv.org/abs/2304.07862)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2304.07862](https://arxiv.org/abs/2304.07862)


**By:**[Xinyi Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20X) ,[Yongfeng Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20Y) ,[Edward C. Malthouse](https://arxiv.org/search/cs?searchtype=author&query=Malthouse%2C%20E%20C)


**Abstract:**


> Online news platforms often use personalized news recommendation methods to help users discover articles that align with their interests. These methods typically predict a matching score between a user and a candidate article to reflect the user's preference for the article. Some previous works have used language model techniques, such as the attention mechanism, to capture users' interests based on their past behaviors, and to understand the content of articles. However, these existing model architectures require adjustments if additional information is taken into account. Pre-trained large language models, which can better capture word relationships and comprehend contexts, have seen a significant development in recent years, and these pre-trained models have the advantages of transfer learning and reducing the training time for downstream tasks. Meanwhile, prompt learning is a newly developed technique that leverages pre-trained language models by building task-specific guidance for output generations. To leverage textual information in news articles, this paper introduces the pre-trained large language model and prompt-learning to the community of news recommendation. The proposed model "prompt-based news recommendation" (PBNR) treats the personalized news recommendation as a text-to-text language task and designs personalized prompts to adapt to the pre-trained language model -- text-to-text transfer transformer (T5). Experimental studies using the Microsoft News dataset show that PBNR is capable of making accurate recommendations by taking into account various lengths of past behaviors of different users. PBNR can also easily adapt to new information without changing the model architecture and the training objective. Additionally, PBNR can make recommendations based on users' specific requirements, allowing human-computer interaction in the news recommendation field.


---


###


Summary Notes


##


Simplifying News Consumption: Meet PBNR, Your Personalized News Guide


In today's digital world, finding news that matches our interests can be like looking for a needle in a haystack. With local newspapers dwindling and "news deserts" expanding, we're turning to online platforms more than ever. But as the flood of information grows, it's getting harder to filter out what matters to us. Enter the realm of news recommendation systems, designed to ease this information overload.


###


The Journey of News Recommendation


Initially, news recommendation systems were all about analyzing user behavior to suggest relevant content.


They've employed everything from Convolutional Neural Networks (CNNs) and Long Short-Term Memory (LSTM) networks to complex attention mechanisms, aiming to guess what users might like next. Recently, pre-trained language models have entered the scene, promising to use existing knowledge to improve recommendations without needing massive datasets.


Yet, applying these language models straight to news recommendation has its hurdles, as they're not built for this out of the box. That's where the Prompt-based News Recommender (PBNR) system comes into play, offering a fresh perspective on tailoring news to your taste.


###


Understanding PBNR: A Fresh Take on News


PBNR takes advantage of the T5 pre-trained language model in a unique way, using an encoder-decoder structure.


It introduces personalized prompts to guide the model, making it versatile for recommending news. Here's how it works:


####


Key Features of PBNr


- **Model Design** : Utilizes the T5 model in an encoder-decoder fashion, turning user and article info into a sequence that the model can understand.


- **Prompt Learning** : Creates personalized prompts to steer the model's output, making recommendations more relevant to individual users.


- **Training Method** : Employs a dual approach, focusing on language generation and ranking accuracy to improve article suggestions.


- **Versatility** : The model can easily adapt to new information or different recommendation scenarios with minor prompt adjustments.


###


PBNR in Action


Testing on the Microsoft News Dataset (MIND) has shown that PBNR can hold its own or even outperform existing models like LSTUR, TANR, NRMS, and NAML.


It excels in providing varied recommendations and adjusting to users' changing needs, thanks to its dynamic prompt-based approach.


###


What's Next for PBNR?


PBNR's innovative approach opens up new possibilities for more personalized and flexible news recommendations.


Looking forward, refining prompt design and reducing reliance on large datasets could further enhance its performance. And the potential doesn't stop at news; PBNR could revolutionize content delivery across various domains.


###


Wrap-Up


The Prompt-based News Recommender (PBNR) system represents a significant advancement in news recommendation technology.


By leveraging advanced language models and custom prompts, PBNR delivers a personalized news-reading experience that stands out in today's crowded digital landscape.


As we continue to evolve with the digital age, PBNR shines as a beacon of innovation, promising a more tailored and engaging way to stay informed.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
