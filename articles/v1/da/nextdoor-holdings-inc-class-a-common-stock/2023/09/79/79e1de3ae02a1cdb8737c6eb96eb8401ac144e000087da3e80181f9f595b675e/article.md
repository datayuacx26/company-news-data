---
schema_version: "1.0.0"
document_id: "79e1de3ae02a1cdb8737c6eb96eb8401ac144e000087da3e80181f9f595b675e"
company_key: "nextdoor-holdings-inc-class-a-common-stock"
company: "Nextdoor Holdings Inc."
source_id: "nextdoor-holdings-inc-class-a-common-stock-rss-10247875dc01"
canonical_url: "https://engblog.nextdoor.com/from-pre-trained-to-fine-tuned-nextdoors-path-to-effective-embedding-applications-3a13b56d91aa"
published_at: "2023-09-07T11:31:32+00:00"
first_seen_at: "2026-07-20T23:17:36.074163+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:339f5827ee4a030e94b0b288e52defc84c670c32499a2524e9a0b2462268968d"
---

# From Pre-trained to Fine-tuned: Nextdoor’s Path to Effective Embedding Applications

# From Pre-trained to Fine-tuned: Nextdoor’s Path to Effective Embedding Applications


[Karthik Jayasurya](https://medium.com/@CarthickJ?source=post_page---byline--3a13b56d91aa---------------------------------------)


11 min read


·


Sep 7, 2023


--


## Background


The majority of ML models at Nextdoor are typically driven by a large number of features that are primarily either continuous or discrete in nature. The personalized features usually stem from historical aggregations or real-time summarization of interaction features, typically captured through logged tracking events. However, representing content through deep understanding using information behind it (text/image) is crucial for modeling nuanced user signals and better personalizing complex user behavior across many of our products. In the rapidly evolving field of NLP, utilizing transformer models to perform representation learning effectively and efficiently has become increasingly important for user understanding and improving their product experience.


Towards that, we have built a lot of entity embedding models spanning entities such as posts, comments, users, search queries & classifieds. We first leveraged deep understanding of content and used that to derive embeddings for meta entities like users based on their past interacted content. These powerful representations are found to be very crucial towards extracting meaningful features for some of the biggest ML ranking systems at Nextdoor such as notifications scoring and feed ranking. By making them readily available and building to scale, we can drive adoption of state-of-the-art reliably and put them in the hands of ML Engineers for rapidly building performant models across the company.


This blog primarily focuses on how we iterated on the development of embedding models, how they are featurized and served at large scale into various product applications as well as some of the challenges encountered during this process. We summarize the evolution of work across three sections. In section 1, the focus is to leverage state-of-the-art pre-trained models to rapidly evaluate the value of embeddings models as feature extractors. Section 2 describes how to fine-tune embeddings using unlabelled data for certain products, whereas Section 3 demonstrates the use of labeled data to fine-tune embeddings for better task prediction. This work is driven by the Knowledge Graph Team at Nextdoor, a horizontal team that works in close collaboration with product ML teams as well as the ML Platform team who owns the ML training and serving platform and the FeatureStore service powering ML models at Nextdoor.


## 1. Leveraging Pre-trained models


The first generation of embeddings are built from pre-trained language models using the Sentence-BERT paradigm ([https://www.sbert.net/](https://www.sbert.net/) ). SBERT is well-known to produce better embedding representations compared to original BERT models \[1\]. The main goal here is to rapidly experiment with embeddings as features and realize their value in the product as quickly as possible. The text from content entities, viz. Nextdoor posts & comments, is extracted from post’s subject and body and comment text respectively, which is then fed into a multilingual text embedding model to derive respective entity embeddings for all countries Nextdoor operates in. For a given user, their historical interacted posts’ embeddings are weighted aggregated based on interaction type to inform user (interaction) embedding. Ex: Active interaction such as post creation/comment/click would have higher weight compared to a more passive interaction like impression. These signals are aggregated across both online (feed) and offline (emails) product surfaces to represent user embedding holistically and are updated daily for all users in the platform.


These features were found to be among the most important features for multiple ranking models and delivered significant performance lifts in key product OKR metrics across both notifications and feed when shipped in early 2022. The pre-trained models also served as a good proof-of-concept to build out reliable feature ingestion pipelines and monitoring systems identifying any potential feature drifts and disruptions. This helped form a robust playbook for deploying several next generation embedding features.


Press enter or click to view image in full size


## 2. Fine-tuned embeddings from unlabeled data


The next generation of embeddings describes training custom models which are improvements over pre-trained versions by leveraging techniques of fine-tuning. The signals used to generate embeddings earlier come from user interactions across notifications and home feed products either directly or indirectly. In contrast, this section details a use case that makes use of unlabelled data to perform representation learning to improve user search experience.


Our neighbors use Nextdoor search to find useful local information by expressing intent explicitly. We tried to capture both long and short term intent to determine and serve user perennial (e.g.: home maintenance) as well as ephemeral needs (e.g.: lost & found). Search queries — while being high intent in nature, are inherently short and noisy. A searcher might try multiple variations of a query successively in order to get their intent fulfilled as much as possible. Additionally, due to the nature of local search, relying on labeled feedback from search results may not fully capture user intent due to limited liquidity.


To fully capture user intent signals, we rely on a self-supervised training strategy to learn fine-tuned representations for any given query. Specifically, we first built an SBERT backed query embedding model that learns to embed search queries in lower dimensional space. Then, we aggregate embeddings from user queries across different time windows (weekly/monthly/quarterly periods) to generate multiple user (intent) embeddings. The same model also extracts the intent of a post to generate the corresponding post embeddings. The resulting user, post and query embeddings are transformed and featurized as described in the later section to improve the performance of the ranking models.


The query embedding model is originally built to drive contextual query expansion in Nextdoor search pipeline \[2\]. This sentence transformer model is trained on historical search queries in order to best learn query representations. We first collected search logs that consisted of sequences of search queries within a session across all searchers over a period of time. Then, they are pre-processed **** using traditional NLP methods like lemmatization, spell checking, deduplication etc. to form a clean corpus of tokens, which is composed of n-grams (n=1,2,3) and whole queries. To generate a training dataset, we created positive pairs of tokens occurring within a user search session and negative pairs randomly occurring across sessions. Contrastive learning with cosine similarity loss is used to train the underlying model.


For the query expansion use-case, this model drove better contextual search results by identifying related candidates improving recall. This helped not only improve key search metrics across content search and product search in For Sale & Free but also reduced the rate of null queries significantly compared to prior word embedding models. We also leveraged **HSNWlib** \[3\] **,** an approximate nearest neighbors library to implement this deep learning based query expansion further improving expansion latencies by more than 10x. For notification & feed use cases, intent features generated from transformations of post & user embeddings helped achieve significant positive impact on our top line engagement metrics. Although features can only be computed for searchers and are of low coverage overall, this explicit signal is found to be very useful in improving the overall search experience.


Press enter or click to view image in full size


## 3. Fine-tuned embeddings from labeled feedback


In the next evolution of embeddings, we additionally leverage user feedback to fine-tune models further. The pre-trained entity embeddings have served us well over a year, but they are off-the-shelf models trained using public benchmark datasets. As such, their semantics are quite different in nature from the Nextdoor domain. Moreover, their high but fixed model dimensionality contributes to significant storage and serving costs, especially when user embeddings are updated for all Nextdoor neighbors daily. To address these, we built a two-tower framework to fine-tune embeddings with user feedback collected across Nextdoor surfaces while reducing **** dimensionality, customizing to our domain, and being cost effective.


The fine-tuned models are developed and trained in phases, incrementally adding complexity. In the first phase, the inputs to post and user towers are pre-trained embeddings, which are then transformed using multiple FC layers, reducing dimensionality at each step. The standard cross-entropy is used as a loss function to predict the task of notification clicks for a given user and post. To generate a training dataset, we sampled from random explore logs to reduce selection bias, the same process as that of the downstream ranking model. Once the model is fully trained, the last layer generates fine-tuned user and post representations.


These pytorch models are trained on millions of records using SageMaker GPU instances with varying hyperparameters, and the model with the best offline performance is chosen to generate & store fine-tuned embeddings into FeatureStore. The earlier described playbook is followed to build and monitor offline and online feature pipelines. Serving these cached features to downstream models has shown promising lifts in all engagement metrics (CTR/sessions/contributions/DAU/WAU) while keeping guardrail metrics that measure harmful/hurtful content distribution across the platform neutral.


In the next phase, we fed the post tower directly with text extracted from the post entity, allowing us to fine-tune parameters of the SBERT model. The test AUC score is used as a benchmark to determine how many layers and transformer blocks to unfreeze for trying out different training schemes along with optimization of hyperparameters of typical DNN models. The best model also improved the user — post cosine similarity of fine-tuned embeddings by up to 16% when compared to respective pre-trained versions — an additional evaluation criteria of intrinsic quality of improvement in representations. It is also noteworthy that this quality improvement is achieved while reducing dimensionality by more than 10x!


## Get Karthik Jayasurya’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


In the most recent phase, we extended into multi-task learning (MTL) setup modeling both notification clicks and feed actions to jointly optimize learning of fine-tuned embeddings. Again, these objectives mimic downstream rankers exactly to make sure learnt embeddings directly optimize downstream tasks. MTL models have the added advantage of learning a single model across multiple product surfaces thereby reducing operational burden and maintenance costs, while leveraging knowledge transfer across shared tasks for better representations. The feed and notifications surfaces are highly related as clicking on email notifications lands directly into the pinned view of the post in the newsfeed. Additionally, most actions on home feed are used as features in notification ranker making these tasks very related **.**


Press enter or click to view image in full size


## Using embeddings in ML models


As most of our downstream production models are tree based models, they don’t directly integrate with vector features like embeddings, like that of Deep neural networks. Therefore, we primarily use outputs from embedding models as feature extractors into downstream models. Specifically, we rely on transformations like cosine similarity & dot products across these entity embeddings in order to generate meaningful affinity features. While transitioning into neural network systems is currently underway — these vector transformations provide a neat way to integrate embedding based features into existing models and enable rapid experimentation for assessing performance lift of new deep features.


We first create schema and declare feature groups corresponding to each embedding to host within our in-house FeatureStore. Then, content based embedding features are ingested into our Featurestore in near real-time using task worker jobs as they get created/updated. For users, the daily scheduled jobs in Airflow compute embedding aggregations based on pre-specified lookback windows, weighted across various interaction types, and are batch ingested into Featurestore. Once systems are set up to ingest all relevant embeddings with appropriate TTL, we then write logging code to compute and log the derived features such as cosine similarity and dot product between user & post, user & user. Specifically, in feed, these features would represent affinities between post vs viewer, viewer vs author and analogously between post vs recipient, sender vs recipient in notifications world. Similarly, we also compute affinities across comment entities to inform activity based ranking in newsfeed. The data obtained from feature logging is used to train downstream ML ranking models, to avoid online-offline skew, and the model with best offline performance lift with new features is promoted for online AB test evaluation and ramping further towards majority member experience.


## Challenges & Future


Multiple entity embeddings i.e user, post, comment, query etc have been successfully integrated into various product surfaces at Nextdoor at large scale. In the past, models based on comment embeddings helped foster and cultivate kinder conversations to improve platform vitality metrics \[4\]. More recently, contextual topic embeddings are also developed using BERTopic \[5\] to achieve coarser level personalization of content to neighbors, while informing us about prevalence of content categories and types across the platform. We are also experimenting with image embeddings using CLIP \[6\] to leverage image/video information behind content.


In addition, as an extension to labeled fine-tuning, we plan to further improve representations along two dimensions. One is by concatenating representations with additional features such as image embeddings and existing interaction features to leverage multimodal and dense signals. The other is to extend tasks to other surfaces such as ads, For Sale & Free (marketplace) etc to make representations more holistic across products. Once downstream models are fully modernized to DNN-based methods, the embeddings can be integrated into the model directly without losing any information from computation of transformations.


As we build more and more embeddings capturing different signals, we also need to be mindful of additional incurred costs from new features. Some of the initial challenges of serving high dimensional vectors during inference are mitigated by performing embedding transformations directly within FeatureStore rather than passing embeddings across microservices minimizing network bandwidth and scaling costs. This worked well with tree based models, however in the future, serving embeddings directly with DNN models can add up costs. Caching and serving fine-tuned embeddings can help control dimensionality while incorporating domain specific knowledge. This allowed us to rapidly experiment and quickly evaluate ROI at a smaller scale, justifying overall costs. From an infra standpoint, we found that optimizing the payload format of embedding features as well as sequencing of calls to efficiently read/write from FeatureStore greatly reduces overall costs at full scale.


## Acknowledgments


This work would not have been possible without close cooperation and collaboration with various ML product partners (Notifications/Feed/Search/Vitality) as well as significant support for ML platform and FeatureStore service from ML Platform team. I would like to take this opportunity to give a huge shoutout to all the dedicated Nextdoor folks from these teams behind this endeavor.


Nextdoor is building the largest Local Knowledge Graph (LKG) in the world. The local knowledge graph inherited in our neighborhoods is Nextdoor’s unique proprietary data that can be used to enable personalized neighborhood and neighbor experiences. The Knowledge Graph team is focused on understanding neighbors and content by creating standardized neighbor/content data using state-of-the-art ML methods.


Third-party large language models (LLMs) such as GPT and the corresponding dialogue applications like ChatGPT, which are built upon these language models, lack access to the specific local knowledge of Nextdoor. As a result, they are unable to offer location-based services to our users as we desire. It is crucial for us to develop in-house custom LLMs that leverage our unique local knowledge graphs. We are building our own large language models (LLMs), that are based on top of Nextdoor’s raw content and the structured knowledge graph to power multiple products.


Please reach out to us if you are interested to learn more — we are hiring!


## References


1. Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks
2. [https://engblog.nextdoor.com/modernizing-our-search-stack-6a56ab87db4e](https://engblog.nextdoor.com/modernizing-our-search-stack-6a56ab87db4e)
3. [https://github.com/nmslib/hnswlib](https://github.com/nmslib/hnswlib)
4. [https://engblog.nextdoor.com/using-predictive-technology-to-foster-constructive-conversations-4af437942bd4](https://engblog.nextdoor.com/using-predictive-technology-to-foster-constructive-conversations-4af437942bd4)
5. [https://maartengr.github.io/BERTopic/api/bertopic.html](https://maartengr.github.io/BERTopic/api/bertopic.html)
6. [https://openai.com/research/clip](https://openai.com/research/clip)
