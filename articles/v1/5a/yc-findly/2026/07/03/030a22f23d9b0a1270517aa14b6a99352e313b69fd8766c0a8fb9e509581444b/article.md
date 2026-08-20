---
schema_version: "1.0.0"
document_id: "030a22f23d9b0a1270517aa14b6a99352e313b69fd8766c0a8fb9e509581444b"
company_key: "yc-findly"
company: "Findly"
source_id: "yc-findly-news-import-b6e251978cf0"
canonical_url: "https://www.findly.ai/blog/the-quest-to-talk-to-databases"
published_at: null
first_seen_at: "2026-07-23T09:28:25.885439+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:2a05e40c00845d3cb2ccc72cb4195c22fdff413c3153ce4aedcb7384583fdf9b"
---

# The quest to talk to databases - Pedro Nascimento and Julia Togashi

We will start by giving an overview of the current academic research, talking about the limitations for practical and production use cases and how we overcome them.


‍


## ‍ **Text-to-SQL using language models**


‍


### **1 - Evaluation datasets**


The current state of the art of text-to-SQL generation models has been brought forward by the recent advancements in language models. Models like GPT4 and Gemini can generate high-quality results. The recent BIRD\[1\] dataset aims to close the gap between the older WIKISQL\[2\] and SPIDER\[3\] datasets.


‍


#### **WIKISQL**


Introduced by Salesforce in 2017, WikiSQL was the first big dataset built for text-to-SQL evaluation. The dataset contains over 80k hand-annotated examples of questions and SQL queries distributed across 24241 tables from Wikipedia. The main problem of the dataset is its simplicity.


**Year:** 2017


**Leaderboard:**[https://github.com/salesforce/WikiSQL](https://github.com/salesforce/WikiSQL)


‍


#### **SPIDER**


Spider is a large-scale[complex and cross-domain](https://medium.com/@tao.yu/spider-one-more-step-towards-natural-language-interfaces-to-databases-62298dc6df3c) semantic parsing and text-to-SQL dataset annotated by 11 Yale students. The goal of the Spider challenge is to develop natural language interfaces to cross-domain databases. It consists of 10,181 questions and 5,693 unique complex SQL queries on 200 databases with multiple tables covering 138 different domains. In Spider 1.0, different complex SQL queries and databases appear in train and test sets. To do well on it, systems must *generalize well to not only new SQL queries but also new database schemas* .


**Year:** 2018


**Leaderboard:**[https://yale-lily.github.io/spider](https://yale-lily.github.io/spider)


‍


Spider chart of some text-to-SQL datasets in 2018.[Source](https://medium.com/@tao.yu/spider-one-more-step-towards-natural-language-interfaces-to-databases-62298dc6df3c)


‍


#### **BIRD**


BIRD is a large-scale cross-domain, text-to-SQL benchmark with a particular focus on large database values. BIRD goal is to mitigate the gap between text-to-SQL research and real-world applications by exploring three additional challenges:


1. Handling large and dirty database values


2. External knowledge evidence


3. Optimizing SQL execution efficiency


**Year:** 2023


**Leaderboard:**[https://bird-bench.github.io/](https://bird-bench.github.io/)


‍


Image showcasing BIRD's performance on NLG models.[Source](https://arxiv.org/pdf/2305.03111.pdf)


‍


The BIRD dataset has a limitation of only supporting SQLite.\`


You can see that from the BIRD dataset, at the moment this article was written the top score submitted is only **65.45%** , which makes it infeasible as a solution for self-service analytics.


‍


### **2 - The state of art academic approaches to solving it**


Text-to-SQL approaches have improved significantly in the past 6 - 7 years. These were due to innovations across different model architectures and learning strategies. A few examples are described below:


‍


#### **Seq2SQL**


> Seq2Seq models, particularly those that leverage deep neural networks. Innovations such as Seq2SQL\[2\] have enhanced the domain by utilizing rewards from query executions to guide the learning process, demonstrating a notable increase in execution and logical form accuracy by employing policy-based reinforcement learning. Seq2SQL leverages the structure of SQL to prune the space of generated queries and simplify the generation problem. Seq2SQL outperformed the state-of-the-art semantic parsing approach at that time, improving execution accuracy from 35.9% to 59.4% and logical form accuracy from 23.4% to 48.3% on WikiSQL.


‍


#### **IRNet**


> Advances have been made by schema-aware models like RAT-SQL\[6\] and IRNet\[7\] , which tackle the inherent challenge of adapting to new and unseen database schemas. IRNet introduces a three-phase approach to overcome the disconnect between natural language intents and SQL query specifics, as well as the difficulty in predicting columns due to the prevalence of out-of-domain words. Initially, IRNet conducts schema linking between a question and a database schema, followed by the generation of a SemQL query, an intermediate representation devised to bridge the gap between natural language and SQL. This phase is critical as it simplifies the natural language complexities into a more structured form that closely aligns with SQL's syntax. The final step involves deterministically converting the SemQL query into a SQL query, leveraging domain knowledge. On the Spider benchmark, IRNet showcases its efficacy by achieving 46.7% accuracy, marking a 19.5% absolute improvement over prior state-of-the-art approaches and securing the top position on the leaderboard at its time of introduction. These advancements underscore the importance of incorporating database schema knowledge directly into models, enabling them to adeptly handle complex queries that involve joins, conditions, and cross-domain data retrieval, thereby bridging the gap between natural language and SQL more effectively and setting new benchmarks in performance on rigorous datasets.


‍


#### **RAT-SQL**


> RAT-SQL, on the other hand, presents a novel approach by employing a relation-aware self-attention mechanism within its framework. This allows the model to effectively encode database schema relations, establish links between the schema and the query, and enhance feature representation, leading to a substantial increase in accuracy. Specifically, on the Spider dataset, RAT-SQL outperformed its predecessors by achieving an exact match accuracy of 57.2%, and with the augmentation of BERT, it further elevates its performance to a state-of-the-art 65.6% accuracy. This marked a significant 8.7% absolute improvement over previous models, demonstrating qualitative enhancements in the model's comprehension of schema linking and alignment.


‍


#### **TAPEX**


> TAPEX\[8\] introduces a groundbreaking method for improving language models trained on structured tabular data, a domain where data scarcity has historically been a challenge. By synthesizing executable SQL queries alongside their results to generate a high-quality, diverse corpus, TAPEX effectively teaches language models to mimic SQL executors, thus overcoming the limitations of available tabular datasets. This novel approach has demonstrated superior performance across four benchmark datasets, achieving significant improvements: an increase to 89.5% (+2.3%) in WikiSQL denotation accuracy, 57.5% (+4.8%) in WikiTableQuestions, 74.5% (+3.5%) in SQA, and 84.2% (+3.2%) in TabFact accuracy. These results not only establish TAPEX as a pioneering solution that outperforms previous table pre-training approaches but also mark the first successful application of synthetic executable programs to set new state-of-the-art records in multiple downstream tasks.


‍


#### **DIN-SQL**


> Further breakthroughs have been achieved through the integration of Large Language Models (LLMs) and schema-aware approaches. DIN-SQL\[5\] shows that decomposing complex queries into sub-tasks before employing LLMs can significantly boost performance. Experiments with three LLMs show that this approach consistently improves their simple few-shot performance by roughly 10%, pushing the accuracy of LLMs towards SOTA or surpassing it. On the holdout test set of Spider, the SOTA, in terms of execution accuracy, was 79.9%, and the new SOTA at the time of this writing using our approach was 85.3%. Our approach to in-context learning beats many heavily fine-tuned models by at least 5%. Additionally, when evaluated on the BIRD benchmark, our approach achieved an execution accuracy of 55.9%, setting a new SOTA on its holdout test set.


‍


These developments show how fast the field is evolving and underscore the importance of sophisticated model architectures, the strategic decomposition of tasks, and the inclusion of database schema understanding in elevating the efficacy of text-to-SQL technologies. This shows we are getting closer to self-service BI, but also as shown in datasets like BIRD there are many challenges to be addressed.


‍


### **3 - The problem**


Most of the recent research on text-to-SQL focuses on the language model alone and not on the full system that solves it. It is great to continue advancing the capabilities of the models to generate SQL from a specification. However, this alone is not enough to solve the self-service BI problem and also puts too much of the responsibility on the language model alone.


‍


Having the LLM to do all the work is very hard and bad for many reasons, to include a few:


‍


1. Tribal knowledge and ambiguity on questions - internal business knowledge needs to be understood by the model. “Raw models” lack internal business context.
2. The LLM doesn't know the precise definition of metrics and dimensions. This needs to be taught to the model.
3. LLM makes mistakes in the SQL generation e.g. syntax errors, joining wrong columns, etc. This is a deterministic problem that doesn't need an LLM, for example, a well-configured semantic layer can generate SQL and do the joins for you.
4. The context window of LLMs is limited, so handling hundreds and thousands of tables/columns is hard for the LLM alone.
5. LLM doesn't provide access control.
6. Latency is a problem, giving too much information at once to the LLM makes the inference slow.
7. Relying on a statistical model alone to solve a hard problem that requires a lot of precision might lead to bad results.


### **Our approach at Findly**


At Findly we split the problem into multiple stages as shown in the picture below


‍


Findly's product architecture in 2024


‍


#### **1 - Findly language parsing layer**


This step of our system is responsible for translating the natural language question into metadata that will be consumed by the semantic layer, that then will be transformed into SQL to answer the question\[4\] . For this, we use not only language models but other fine-tuned machine-learning models. This topic is complex enough to deserve its own blog post in the future :)


‍


We decided to tackle the problem using two concepts: trying to break down a big complex problem into smaller ones and trying to solve problems incrementally, from less complex to more complex. Besides having the clear advantage of better and more reliable results, this approach also helps us prevent previously mentioned problems while dealing with LLMs, such as the token limit and the slow inference as the number of tokens grows.


‍


#### **2 - Semantic Layer**


A semantic layer is an abstraction layer that sits between the user and the database. It provides a simplified, business-focused view of the data, enabling users to interact with the database without needing to understand the underlying complexities, such as SQL syntax, table relationships, or the physical schema of the database. The semantic layer translates user queries, often made through graphical interfaces or natural language, into technical queries that the database understands.


‍


**Pros:**


‍


1. **Interoperability** : the semantic layer is deterministically fluent in many different dialects and databases.
2. **Complex SQL generation** : Handle complex SQL generation i.e., Generates the SQL query for the desired dialect/database, handles joins between tables, handles complex business logic that leads to complex SQL, etc. This all eases the work needed from the language models.
3. **Computational Constraints** : The context window of LLMs is limited, so handling hundreds and thousands of tables/columns is hard for the LLM alone - simplifying the modeling by making it talk to the semantic layer instead, reduces the amount of information needed
4. **Consistency** : Ensures that business terms and metrics are defined consistently across the organization, reducing confusion and errors in reporting and analysis. **‍**
5. **Security** : Can enforce data access controls, ensuring users can only access the data they are authorized to see.


‍


The main problem with the current semantic layers is the complexity of maintenance. The need for human experts to model and write all the modeling in the SL modeling language, as the company grows you also need to keep it updated as the original data sources change and grow.


By using a semantic layer we can solve many of the problems with generating text-to-SQL directly. We reduce the problem of talking to data, to talking to metadata, and then from the metadata we generate SQL.


‍


#### **3 - *Semantica* - Co-pilot for semantic layer generation**


Using *Semantica* , our co-pilot for the semantic layer generation we reduce the time of creating and maintaining the semantic layer by more than 10x. *Semantica* uses a mix of machine learning with deterministic inference approaches. It works by inferring and automatically creating important metrics and dimensions for the user's business, helping the team extract valuable information from its data warehouse.


‍


*Semantica* also helps to reduce the amount of data duplication across the company. Instead of having a lot of data cleansing and duplication happening across multiple layers, *Semantica* allows all the business logic and modifications of the same to happen in the semantic layer instead of having to change the underlying data, which immensely speeds up the development and reduces maintenance costs.


‍


Screenshot of *Semantica* generating the semantic layer in Findly, March 2024


‍


‍


#### **4 - Dashboard**


The dashboard must ensure a fluent human-AI interaction that provides reliability for the user. This is where the magic happens, where users can visualize complex information, uncover hidden patterns, and transform data into insights. Some key features to consider:


‍


- **Transparent metadata:** Displaying the selected metadata, lets users peek under the hood to understand how the AI models interpreted your queries and what data and sources were used to generate the insights.
- **Error detection and handling:** Handling errors and incomplete data, alerting users when the information for a given query is insufficient or potentially inaccurate.
- **Interactive visualizations:** Complex data is best understood through visuals. It’s important to provide the relevant visualizations for each query. **‍**
- **Security and compliance:** Having robust security measures and access controls, so users can only see the information they’re authorized to access, keeping sensitive data safe and compliant.


‍


Screenshot of[Findly's dashboard](https://dashboard.findly.ai/) , March 2024


‍


‍


#### 5 - Above and beyond text-to-SQL


At Findly, we believe in empowering non-technical users to access and manipulate data simply and efficiently. Our goal is to maximize the value and insights extracted from this data, ensuring users can achieve the greatest impact from their analyses.


‍


We go beyond basic text-to-SQL capabilities by offering advanced insights in both textual and code formats. Our platform provides summaries that highlight the most relevant points, enabling quick reporting and guiding users in their analysis. Additionally, we deliver deeper insights by automatically generating and executing Python code, addressing complex questions such as future predictions, trend analysis, and outlier detection.


‍


#### 6 - Evaluation


We evaluate our models using our internal eval framework. We decided to create our own benchmarks as they are more grounded in real use cases e.g. Google Analytics data, Facebook ads, etc. We believe solving those cases is more impactful for real-world problems, thus creating more value for our users.


‍


Findly approach beats GPT-4 level model by **more than 50%,** and this margin increases further in highly complex scenarios involving numerous tables and joins. Some of the improvements noticed over GPT4- alone:


- Significantly reduced level of hallucinations, ensuring that the metrics and dimensions queried are always in the user's database, even for complex and open questions
- No syntax errors while generating SQL
- Ability to handle complex where clauses e.g., no problems with case sensitivity, support to more complex regular expressions, etc
- Models are much more factually grounded, it is able to understand internal business terminology easier
- We can handle a large number of tables (dozens, hundreds, …) and do complex joins without problems


We plan to keep improving our evals and make it open soon!


‍


### **Conclusion**


Having a semantic layer to reduce the amount of work the LLM needs to do, and also curating the precise business information helps to bridge the gap to make a system that actually can talk to your databases rather than just a nice prototype.


‍


In the next posts, we will be diving more into the specific challenges of each component described above and how to address them.


‍


### References


\[1\] Li, J., Hui, B., Qu, G., Yang, J., Li, B., Li, B., Wang, B., Qin, B., Cao, R., Geng, R., Huo, N., Zhou, X., Ma, C., Li, G., Chang, K. C. C., Huang, F., Cheng, R., & Li, Y. (2023). Can LLM Already Serve as A Database Interface? A BIg Bench for Large-Scale Database Grounded Text-to-SQLs. arXiv.[https://arxiv.org/abs/2305.03111](https://arxiv.org/abs/2305.03111)


*‍*


\[2\] Zhong, V., Xiong, C., & Socher, R. (2017). Seq2SQL: Generating Structured Queries from Natural Language using Reinforcement Learning. arXiv.[https://arxiv.org/abs/1709.00103](https://arxiv.org/abs/1709.00103)


*‍*


\[3\] Yu, T., Zhang, R., Yang, K., Yasunaga, M., Wang, D., Li, Z., Ma, J., Li, I., Yao, Q., Roman, S., Zhang, Z., & Radev, D. (2018). Spider: A Large-Scale Human-Labeled Dataset for Complex and Cross-Domain Semantic Parsing and Text-to-SQL Task. arXiv.[https://arxiv.org/abs/1809.08887](https://arxiv.org/abs/1809.08887)


*‍*


\[4\] Wikipedia contributors. (n.d.). Semantic parsing. Wikipedia. Retrieved March 7, 2024, from[https://en.wikipedia.org/wiki/Semantic_parsing](https://en.wikipedia.org/wiki/Semantic_parsing)


*‍*


\[5\] Pourreza, M., & Rafiei, D. (2023). DIN-SQL: Decomposed In-Context Learning of Text-to-SQL with Self-Correction. arXiv.[https://arxiv.org/abs/2304.11015](https://arxiv.org/abs/2304.11015)


*‍*


\[6\] Wang, B., Shin, R., Liu, X., Polozov, O., & Richardson, M. (2019). RAT-SQL: Relation-Aware Schema Encoding and Linking for Text-to-SQL Parsers. arXiv.[https://arxiv.org/abs/1911.04942](https://arxiv.org/abs/1911.04942)


*‍*


\[7\] Guo, J., Zhan, Z., Gao, Y., Xiao, Y., Lou, J.-G., Liu, T., & Zhang, D. (2019). Towards Complex Text-to-SQL in Cross-Domain Database with Intermediate Representation. arXiv.[https://arxiv.org/abs/1905.08205](https://arxiv.org/abs/1905.08205)


*‍*


\[8\] Liu, Q., Chen, B., Guo, J., Ziyadi, M., Lin, Z., Chen, W., & Lou, J.-G. (2022). TAPEX: Table Pre-training via Learning a Neural SQL Executor. OpenReview.[https://openreview.net/forum?id=O50443AsCP](https://openreview.net/forum?id=O50443AsCP)
