---
schema_version: "1.0.0"
document_id: "28123af57402fc023663e2ec33f187765ad55ede2028956c9acc44573b79eb28"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/automatic-root-cause-analysis-via-large-language-models-for-cloud-incidents"
published_at: "2023-11-13T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:7a4c05c5df6b9aecb44969a38a54ccd9986c2d6bb06ce67b14f97882db73570f"
---

# Automatic Root Cause Analysis via Large Language Models for Cloud Incidents

Do not index


Original Paper


[https://arxiv.org/abs/2305.15778](https://arxiv.org/abs/2305.15778)


Blog URL


[blog.athina.ai /automat...ncidents](https://blog.athina.ai/automatic-root-cause-analysis-via-large-language-models-for-cloud-incidents)


**Original Paper:**[https://arxiv.org/abs/2305.15778](https://arxiv.org/abs/2305.15778)


**By:**[Yinfang Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20Y) ,[Huaibing Xie](https://arxiv.org/search/cs?searchtype=author&query=Xie%2C%20H) ,[Minghua Ma](https://arxiv.org/search/cs?searchtype=author&query=Ma%2C%20M) ,[Yu Kang](https://arxiv.org/search/cs?searchtype=author&query=Kang%2C%20Y) ,[Xin Gao](https://arxiv.org/search/cs?searchtype=author&query=Gao%2C%20X) ,[Liu Shi](https://arxiv.org/search/cs?searchtype=author&query=Shi%2C%20L) ,[Yunjie Cao](https://arxiv.org/search/cs?searchtype=author&query=Cao%2C%20Y) ,[Xuedong Gao](https://arxiv.org/search/cs?searchtype=author&query=Gao%2C%20X) ,[Hao Fan](https://arxiv.org/search/cs?searchtype=author&query=Fan%2C%20H) ,[Ming Wen](https://arxiv.org/search/cs?searchtype=author&query=Wen%2C%20M) ,[Jun Zeng](https://arxiv.org/search/cs?searchtype=author&query=Zeng%2C%20J) ,[Supriyo Ghosh](https://arxiv.org/search/cs?searchtype=author&query=Ghosh%2C%20S) ,[Xuchao Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20X) ,[Chaoyun Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20C) ,[Qingwei Lin](https://arxiv.org/search/cs?searchtype=author&query=Lin%2C%20Q) ,[Saravan Rajmohan](https://arxiv.org/search/cs?searchtype=author&query=Rajmohan%2C%20S) ,[Dongmei Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20D) ,[Tianyin Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20T)


**Abstract:**


> Ensuring the reliability and availability of cloud services necessitates efficient root cause analysis (RCA) for cloud incidents. Traditional RCA methods, which rely on manual investigations of data sources such as logs and traces, are often laborious, error-prone, and challenging for on-call engineers. In this paper, we introduce RCACopilot, an innovative on-call system empowered by the large language model for automating RCA of cloud incidents. RCACopilot matches incoming incidents to corresponding incident handlers based on their alert types, aggregates the critical runtime diagnostic information, predicts the incident's root cause category, and provides an explanatory narrative. We evaluate RCACopilot using a real-world dataset consisting of a year's worth of incidents from Microsoft. Our evaluation demonstrates that RCACopilot achieves RCA accuracy up to 0.766. Furthermore, the diagnostic information collection component of RCACopilot has been successfully in use at Microsoft for over four years.


---


###


Summary Notes


##


Revolutionizing Cloud Incident Analysis with RCACopilot


Cloud computing is essential in today’s digital landscape, but incidents are inevitable. Quick and accurate root cause analysis (RCA) is vital for maintaining cloud service reliability.


Traditional RCA methods, though, are slow and often error-prone. This is where **RCACopilot** comes in, using large language models (LLMs) to automate RCA, making the process faster and more efficient.


###


Traditional RCA: A Slow Process


Managing cloud services means resolving incidents fast. Traditionally, this requires manually going through tons of data—logs, metrics, and more—which is slow and susceptible to mistakes, leading to delays and potential service outages.


###


The Power of Large Language Models


Large language models have changed the game by automating complex tasks. For RCA, they can quickly analyze data and use past incidents to make better future predictions. This is a big improvement over the old way of manually analyzing data and relying on outdated guides.


###


Meet RCACopilot


**RCACopilot** is here to change how companies manage cloud incidents by automating RCA in two main steps:


- **Gathering Data** : It efficiently collects relevant data for each incident.


- **Identifying the Root Cause** : By using an LLM, it predicts the root cause and explains it in an easily understandable way.


Microsoft has already implemented this system, proving its effectiveness in a large enterprise environment.


####


Benefits of RCACopilot


- **Speed** : Automates the slow process of collecting and analyzing data.


- **Precision** : Uses historical data and LLMs to make accurate root cause predictions.


- **Scalability** : Proven to work on a large scale at Microsoft, showing its adaptability.


###


How It’s Built


RCACopilot uses C# and Python, with a user-friendly web interface for managing incident handlers. This setup allows for flexibility and ease of use for AI engineers in various development settings.


###


Impact and Evaluation


When tested with Microsoft’s cloud service incidents, RCACopilot outperformed traditional methods and other LLM-based systems in accuracy, showcasing its ability to reduce manual work and enhance resolution precision in the cloud computing industry.


###


The Future of Cloud Computing


RCACopilot represents a significant step forward, offering a scalable and efficient way to automate RCA.


Its success at Microsoft suggests wide applicability and benefits for cloud services. As cloud infrastructure becomes more complex, tools like RCACopilot will be crucial for ensuring service reliability and availability.


For AI engineers in enterprise settings, adopting RCACopilot and similar technologies is a stride towards more resilient, efficient, and manageable cloud infrastructures. The future of cloud computing is automated, accurate, and adaptable.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
