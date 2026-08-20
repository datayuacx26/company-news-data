---
schema_version: "1.0.0"
document_id: "3084213271a9c9e165c275be0a1daff715ce3768c85407c1af6448347ab7ac06"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/chain-of-verification-reduces-hallucination-in-large-language-models"
published_at: "2023-09-20T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:c9445a1ba9d362db282e9dd29ffa84678c048189ace49b2c4bc12d39ea7dba18"
---

# Chain-of-Verification Reduces Hallucination in Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2309.11495](https://arxiv.org/abs/2309.11495)


Blog URL


[blog.athina.ai /chain-o...e-models](https://blog.athina.ai/chain-of-verification-reduces-hallucination-in-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2309.11495](https://arxiv.org/abs/2309.11495)


**By:**[Shehzaad Dhuliawala](https://arxiv.org/search/cs?searchtype=author&query=Dhuliawala%2C%20S) ,[Mojtaba Komeili](https://arxiv.org/search/cs?searchtype=author&query=Komeili%2C%20M) ,[Jing Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20J) ,[Roberta Raileanu](https://arxiv.org/search/cs?searchtype=author&query=Raileanu%2C%20R) ,[Xian Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20X) ,[Asli Celikyilmaz](https://arxiv.org/search/cs?searchtype=author&query=Celikyilmaz%2C%20A) ,[Jason Weston](https://arxiv.org/search/cs?searchtype=author&query=Weston%2C%20J)


**Abstract:**


> Generation of plausible yet incorrect factual information, termed hallucination, is an unsolved issue in large language models. We study the ability of language models to deliberate on the responses they give in order to correct their mistakes. We develop the Chain-of-Verification (CoVe) method whereby the model first (i) drafts an initial response; then (ii) plans verification questions to fact-check its draft; (iii) answers those questions independently so the answers are not biased by other responses; and (iv) generates its final verified response. In experiments, we show CoVe decreases hallucinations across a variety of tasks, from list-based questions from Wikidata, closed book MultiSpanQA and longform text generation.


---


###


Summary Notes


##


Boosting AI Accuracy with the Chain-of-Verification Method


###


Introduction


In the fast-paced world of artificial intelligence (AI), ensuring that large language models (LLMs) are accurate and reliable is crucial, especially for AI engineers in enterprise settings.


A key challenge is dealing with "hallucination" - when models produce believable but incorrect information.


This post introduces an effective solution: the Chain-of-Verification (CoVe) method. CoVe improves the accuracy of LLM outputs by making the model verify its own answers.


###


The Issue of Hallucination


Hallucination in LLMs is a problem that affects the trustworthiness of AI-generated content. Despite bigger models and better training, hallucination remains an issue. Traditional solutions, like corrections during or after training and external fact-checking tools, often fail to consistently ensure accuracy.


###


The CoVe Approach


CoVe tackles this issue with a four-step process:


- **Generate Baseline Response:** The model first creates an initial answer.


- **Plan Verifications:** It then develops questions to check for inaccuracies in its first answer.


- **Execute Verifications:** The model answers these questions itself.


- **Generate Final Verified Response:** Using what it learned, the model updates its initial response to be factually accurate.


This method helps the model to critically evaluate and refine its outputs, making the information it generates more reliable.


###


Testing and Outcomes


CoVe was tested on tasks like question answering and longform text generation, showing a marked improvement in accuracy and reduction in hallucinations compared to existing methods. This indicates that CoVe can make LLM outputs more trustworthy.


###


Why CoVe Works


The success of CoVe lies in its independent verification step, which helps the model better distinguish between correct and incorrect information. Different ways of implementing this step were tested, with a factored approach showing particular promise by preventing reliance on inaccurate initial answers.


###


Conclusion


The Chain-of-Verification method represents a major step forward in reducing hallucination in LLMs. For enterprise AI engineers, using CoVe can lead to AI-generated content that is not only innovative but also highly accurate, enhancing the credibility and usefulness of AI applications.


###


Looking Ahead


The importance of methods like CoVe, which enable structured self-verification, is immense as we move deeper into the AI era.


These methods will be key to leveraging the full potential of AI technologies while ensuring the integrity and trustworthiness of their outputs.


AI engineers in enterprise environments are encouraged to explore the benefits of the Chain-of-Verification method, setting the stage for a future where AI-generated content is both groundbreaking and impeccably accurate.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
