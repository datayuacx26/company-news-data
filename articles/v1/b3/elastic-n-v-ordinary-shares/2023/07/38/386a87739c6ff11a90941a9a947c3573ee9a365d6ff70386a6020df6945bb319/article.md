---
schema_version: "1.0.0"
document_id: "386a87739c6ff11a90941a9a947c3573ee9a365d6ff70386a6020df6945bb319"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/generative-ai-using-elastic-amazon-sagemaker-jumpstart"
published_at: "2023-07-25T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:f0869901585970b9f03cd2bd13f89f867cfd31e76d751371b5ff2d4a1356fe79"
---

# Generative AI using Elastic and Amazon SageMaker JumpStart

Elasticsearch has native integrations with the industry-leading Gen AI tools and providers. Check out our webinars on going[Beyond RAG Basics](https://www.elastic.co/virtual-events/beyond-rag-basics) , or[building prod-ready apps](https://www.elastic.co/virtual-events/vector-databases-fast-track-production) with the Elastic vector database.


To build the best search solutions for your use case, start a[free cloud trial](https://cloud.elastic.co/registration) or try Elastic on your[local machine](https://github.com/elastic/start-local) now.


In the rapidly advancing world of artificial intelligence, one of the most intriguing and transformative developments is[generative artificial intelligence (GAI)](https://www.elastic.co/what-is/generative-ai) . GAI represents a significant leap forward in AI capabilities, enabling machines to generate original and creative content across various domains including conversations, stories, images, videos, and music. Enterprises seek not only top-performing infrastructure, but also a secure platform to harness the power of GAI without compromising their sensitive data and intellectual property.[Large language models (LLMs)](https://www.elastic.co/what-is/large-language-models) strive to understand and produce text that resembles human language, utilizing the structure, meaning, and context of natural language.


Elastic and Amazon Web Services (AWS) understand this pressing need and have taken the lead in offering cutting-edge solutions to meet these demands. Using Amazon SageMaker JumpStart combined with[Elasticsearch](https://elastic.co/elasticsearch) ’s capabilities, businesses can now confidently explore and adopt the most suitable AI models for their specific use cases while maintaining cost-effectiveness, security, and[privacy](https://www.elastic.co/blog/privacy-first-ai-search-langchain-elasticsearch) .


Elasticsearch’s integration with advanced AI models further enhances its capabilities. By leveraging Elasticsearch’s[retrieval prowess](https://www.elastic.co/enterprise-search/generative-ai) ,[LLMs](https://www.elastic.co/what-is/large-language-models) can access the most relevant documents to provide accurate and up-to-date responses. This synergy between Elasticsearch and LLMs ensures that users receive contextually relevant and factual answers to their queries, setting a new standard for information retrieval and AI-powered assistance.


Elasticsearch is a scalable data store and[vector database](https://www.elastic.co/elasticsearch/vector-database) that offers a range of features to ensure exceptional search performance. It supports traditional keyword and text-based search using the[BM25](https://www.elastic.co/search-labs/blog/lexical-and-semantic-search-with-elasticsearch) algorithm, as well as AI-ready[vector search](https://www.elastic.co/elasticsearch/vector-database) with exact match and approximate kNN (k-Nearest Neighbor) search capabilities. These advanced features allow Elasticsearch to retrieve highly relevant results for queries expressed in natural language. By combining traditional, vector, or[hybrid search](https://www.elastic.co/search-labs/blog/hybrid-search-elasticsearch) approaches, Elasticsearch delivers precise results, making it effortless for users to find the information customers need.


The Elasticsearch platform seamlessly incorporates robust machine learning and artificial intelligence capabilities directly into its solutions, empowering you to create highly sought-after applications and accomplish tasks with remarkable efficiency. By leveraging these advanced technologies, you can harness the full potential of Elasticsearch to deliver exceptional user experiences and expedite your workflow.


## Implementing RAG using Elasticsearch and open source LLM available in Amazon SageMaker JumpStart


The solution below explains how to use[Retrieval Augmented Generation](https://www.elastic.co/search-labs/blog/retrieval-augmented-generation-rag) (RAG) to enable GAI capabilities on domain-specific business data using Elasticsearch, Amazon SageMaker JumpStart, and your choice of open source LLMs.


### Solution overview


We will start by reviewing the architecture diagram below. It explains how to get domain-specific responses from an LLM hosted in Amazon SageMaker JumpStart using enterprise data hosted in Elasticsearch using RAG.


We used the following AWS and third-party services:


1. [Amazon SageMaker](https://aws.amazon.com/pm/sagemaker) and[Amazon SageMaker JumpStart](https://aws.amazon.com/sagemaker/jumpstart/) for hosting the open source LLMs from Hugging Face
2. Falcon 40B Instruct and Flan-T5 XL LLM from Hugging Face
3. Elasticsearch for storing[embeddings](https://www.elastic.co/what-is/vector-embedding) of the enterprise knowledgecorpus and doing similarity search with user questions
4. Python,[LangChain](https://python.langchain.com/v0.1/docs/get_started/introduction/) , and[Streamlit](https://streamlit.io/) for building the RAG application
5. Amazon EC2 for hosting the Streamlit application
6. [AWS Identity and Access Management](https://aws.amazon.com/iam/) roles and policies for access management


### Step-by-step explanation


**Offline data ingestion:**


We ingest data from an enterprise knowledge corpus – for example this could be internal web pages, documents describing a company’s process, or corporate financial data.


1. The documents are ingested using a web crawler or any other ingestion mechanism.
2. The textual content is converted into vectors and stored in a dense_vector field by a sentencetransformer type ML model.


**Real-time flow on userquery :**


1. The user provides a question via the Retrieval Augmented Generation (RAG) web application.
2. The RAG application generates a hybrid search request for Elasticsearch based on the user's question and sends it to Elasticsearch. The hybrid search request does a BM25 match on the text field and kNN search on the dense_vector field.
3. Elasticsearch returns thedocument body and source URL (if applicable) to the RAG application. The RAG application accepts only the top scored document.
4. The RAG application passes the top scored document body (context) along with user question (prompt) to the LLM hosted as Amazon SageMakerendpoint .
5. The Amazon SageMaker endpoint generates a textual response and sends it back to the RAG application.
6. The RAG application performs any required post processing tasks. For example, it adds a source url to the response generated from the LLM. The user views the response in the web application.


Let’s now look at a few setup steps and a few implementation steps to create a working search solution:


**Setup steps:**


1. **Sign up** for a free trial of an Elasticsearch cluster with[Elastic Cloud](https://cloud.elastic.co/registration?onboarding_token=vectorsearch&cta=cloud-registration&tech=trial&plcmt=article%20content&pg=search-labs) .
2. **Create a new deployment** on AWS following[these steps](https://www.elastic.co/guide/en/cloud/current/ec-create-deployment.html) .
3. **Add a new machine learning node** following the steps below. This will enable you to run machine learning models in your deployment.
4. Click on **Edit** under Deployment Name in the left navigation bar.
5. Scroll down to the Machine Learning instances box.
6. Click **+Add Capacity** .
7. Under Size per zone, click and select **2GB RAM** .
8. Click on **Save** and then **Confirm** .
9. **Reset and download the elastic user password** following these[steps](https://www.elastic.co/guide/en/cloud/current/ec-password-reset.html) .
10. **Copy the deployment ID** from the Overview page under Deployment name.
11. **Load anembedding model into Elasticsearch.** Here, we have used[all-distilroberta-v1](https://huggingface.co/sentence-transformers/all-distilroberta-v1) model hosted in the Hugging Face model hub. You can choose other sentence transformer types based on your case. Import this Python **notebook**[here](https://github.com/Udayel/RAGElastic-LLM) in Amazon SageMaker and run it. Provide the **Cloud Id** , **Elasticsearch username** , and **Elasticsearch password** when prompted. This will download the model from Hugging Face, chunk it up, load it into Elasticsearch, and deploy the model onto the machine learning node of the Elasticsearch cluster.
12. **Create an Elasticsearch index** by opening[Kibana](https://elastic.co/kibana) from the Elastic Cloud console and navigating to Enterprise Search - Overview. Click on **Create an Elasticsearch Index** . Choose **Web Crawler** as the Ingestion method. Enter a suitable Index name and click **Create Index** .
13. **Add an[Inference](https://www.elastic.co/docs/explore-analyze/elastic-inference) Pipeline** by clicking on **Pipelines tab > Copy** and customizing it in the Ingest Pipeline Box. Click **Add Inference Pipeline** in the Machine Learning Inference Pipelines box. Enter the Name for the new pipeline. Select the trained Model loaded in step 6 and Select title as source field. Click **Continue** in two subsequent screens and click **Create Pipeline** at the Review stage.


1. **Update the mapping fordense vector** by clicking on **Dev Tools** and running the following code. This will enable you to run kNN search on the title field vectors. From Elasticsearch version 8.8+, this step will be handled automatically.


1. **Configure web crawler** to crawl Elastic Docs (you can replace this with your Enterprise Domain corpus). Click on the relevant index under Available indices. Click on the **Manage Domains** tab. Click **Add domain** . Enter[https://www.elastic.co/guide/en](https://www.elastic.co/guide/en) and click **Validate Domain** . Click **Add domain** and then **Add Crawl rules** . Add the following rules. Click **Crawl** and then **Crawl all domains on this index** . This will start Elasticsearch’s web crawler and it will crawl the targeted documents, generate vectors for the title field, and index the document and vector.


The **implementation steps** for instantiating the solution presented in this post are as follows:


1. **Choose your LLM.** Amazon SageMaker JumpStart offers a wide selection of proprietary and publicly available foundation models from various model providers. Log in to Amazon SageMaker Studio, open Amazon SageMaker JumpStart, and search for your preferred Foundation model. Please find the list of models available for each task[here](https://aws.amazon.com/sagemaker/jumpstart/?sagemaker-data-wrangler-whats-new.sort-by=item.additionalFields.postDateTime&sagemaker-data-wrangler-whats-new.sort-order=desc) .


1. **Deploy your LLM.** Amazon SageMaker JumpStart studio also provides a no-code interface to deploy the model. You can easily deploy a model with few clicks. After the deployment is successful, copy the Endpoint Name.


1. **Download and set up the RAG Application.** Launch an EC2 instance and clone the code from this[GitHub link](https://github.com/Udayel/RAGElastic-LLM) . Set up a virtual environment following[these steps](https://docs.python.org/3/library/venv.html) . Install the required Python libraries by running the command pip install -r requirements.txt. Update the config.sh file with the following:
2. ES_CLOUD_ID: Elastic Cloud Deployment ID
3. ES_USERNAME: Elasticsearch Cluster User
4. ES_PASSWORD: Elasticsearch User password
5. FLAN_T5_ENDPOINT: Amazon SageMaker Endpoint Name pointing to Flan T5
6. FALCON_40B_ENDPOINT: Amazon SageMaker Endpoint Name pointing to Falcon 40B
7. AWS_REGION: AWS Region
8. **Run the application** using the command streamlit run rag_elastic_aws.py. This will start a web browser and the url will be printed to the command line.
9. **Response of LLM without context.**


1. **Response of LLM with context** derived from Elasticsearch.


## Conclusion


In this post, we showed you how to create a Retrieval Augmented Generation-based search application using a combination of Elasticsearch, Amazon SageMaker JumpStart, open-source LLMs from Hugging Face, and open source Python packages like LangChain and Streamlit.


Learn more by exploring[JumpStart](https://aws.amazon.com/sagemaker/jumpstart/) ,[Amazon Titan](https://aws.amazon.com/bedrock/titan/) models,[Amazon Bedrock](https://aws.amazon.com/bedrock/) , and[Elastic](https://www.elastic.co/) to build a solution using the sample implementation provided in this post and a data set relevant to your business.


Or, start your own[7-day free trial](https://aws.amazon.com/marketplace/pp/prodview-voru33wi6xs7k?trk=5fbc596b-6d2a-433a-8333-0bd1f28e84da&sc_channel=el) by signing up via[AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-voru33wi6xs7k?trk=d54b31eb-671c-49ba-88bb-7a1106421dfa%E2%89%BBchannel=el) and quickly spin up a deployment in minutes on any of the[Elastic Cloud regions on AWS](https://www.elastic.co/guide/en/cloud/current/ec-reference-regions.html#ec_amazon_web_services_aws_regions) around the world. Your AWS Marketplace purchase of Elastic will be included in your monthly consolidated billing statement and will draw against your committed spend with AWS.


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party[generative AI](https://www.elastic.co/generative-ai) tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, ESRE, Elasticsearch Relevance Engine and associated marks are trademarks, logos or registered trademarks of Elasticsearch N.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


#### Frequently Asked Questions


##### What is generative artificial intelligence (GAI)?


GAI represents a significant leap forward in AI capabilities, enabling machines to generate original and creative content across various domains including conversations, stories, images, videos, and music.


#### How helpful was this content?


Not helpful


Somewhat helpful


Very helpful


[Report an issue](https://discuss.elastic.co/c/elastic-community-ecosystem/elasticsearch-labs/101)


## Related Content


[AI](https://www.elastic.co/search-labs/blog/category/ai)[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)


July 7, 2026


#### [Short queries, formal documents: how HyDE improved semantic search precision by 50% in Elasticsearch](https://www.elastic.co/search-labs/blog/hyde-semantic-search-elasticsearch)


HyDE boosts semantic search precision and recall by 50% on short queries. Here's how to implement it in Elasticsearch with the Inference API and semantic_text.


JR


By:[Jeffrey Rengifo](https://www.elastic.co/search-labs/author/jeffrey-rengifo)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[AI](https://www.elastic.co/search-labs/blog/category/ai) +1


June 24, 2026


#### [Elasticsearch DiskBBQ delivers 7x faster vector search than Qdrant on network-attached storage](https://www.elastic.co/search-labs/blog/vector-search-benchmark-elasticsearch-vs-qdrant)


Elasticsearch DiskBBQ achieves up to 7x higher vector search throughput than Qdrant at comparable recall on network-attached storage. Explore the benchmark methodology and full results.


SF


By:[Sachin Frayne](https://www.elastic.co/search-labs/author/sachin-frayne)


[Agentic AI](https://www.elastic.co/search-labs/blog/category/agentic-ai)[AI](https://www.elastic.co/search-labs/blog/category/ai) +1


July 6, 2026


#### [Who grades the grader? LLM-as-a-Judge inside Elasticsearch Workflows](https://www.elastic.co/search-labs/blog/rag-evaluation-elasticsearch-workflows)


Find out if your RAG agent is ready to ship. Score it on correctness, faithfulness and retrieval quality using only Elasticsearch Workflows and two Claude models.


JR


By:[Jeffrey Rengifo](https://www.elastic.co/search-labs/author/jeffrey-rengifo)


[AI](https://www.elastic.co/search-labs/blog/category/ai)


June 15, 2026


#### [Your search index is already an agent memory system: Persistent agent memory for Claude Code with Elasticsearch](https://www.elastic.co/search-labs/blog/persistent-memory-agents-elasticsearch-claude-code)


Give your AI agent persistent cross-session memory using Elasticsearch: Hybrid recall, a knowledge graph, and cross-device handoffs. Three commands to install.


JV


By:[Jeff Vestal](https://www.elastic.co/search-labs/author/jeff-vestal)


[AI](https://www.elastic.co/search-labs/blog/category/ai)[Integrations](https://www.elastic.co/search-labs/blog/category/integrations)


June 15, 2026


#### [Your FAQ bot doesn't need a PhD: LLM query routing with Elastic Workflows](https://www.elastic.co/search-labs/blog/llm-query-routing-elastic-workflows)


Route LLM queries by complexity using Elasticsearch search metadata: Mistral Small for FAQ questions, Claude Sonnet for multi-source synthesis.


JR


By:[Jeffrey Rengifo](https://www.elastic.co/search-labs/author/jeffrey-rengifo)
