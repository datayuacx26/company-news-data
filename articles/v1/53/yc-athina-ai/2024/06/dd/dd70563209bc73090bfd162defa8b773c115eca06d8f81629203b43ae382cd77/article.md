---
schema_version: "1.0.0"
document_id: "dd70563209bc73090bfd162defa8b773c115eca06d8f81629203b43ae382cd77"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/a-practical-guide-to-measure-and-improve-retrieval-in-a-rag-based-llm-application"
published_at: "2024-06-11T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:c121a724eda8e61c5d9db96e257b3481a58d2a94a5fc6bd23ed898fc0e31c769"
---

# A Practical Guide to Measure and Improve Retrieval in a RAG-based LLM Application

Do not index


Original Paper


Blog URL


####


Introduction


Efficiently extracting relevant information from massive datasets is essential.


This guide details a Python notebook designed to show developers how to experiment with and improve retrievals.


By adjusting parameters and using specific evaluation metrics, one can enhance the performance and accuracy of retrieval, which is one of the best ways to improve RAG pipelines.


####


How the Notebook Functions


The notebook encompasses several critical steps:


1. **Environment preparation** and library installation.


1. **Function definition** for dataset management (creation, modification, deletion).


1. **Experiment execution** with varied retrieval parameters.


1. **Document handling** using embedding and vector storage solutions.


1. **Result evaluation** with custom metrics to finalize the optimal configuration.


####


Step-by-Step Tutorial


####


Step 1: Environment Setup


Begin by installing all necessary packages:


```text
pip install athina chromadb langchain langchain-openai langchain-community langchain-chroma
```


####


Step 2: Import Libraries and Set API Keys


Load required libraries and set up API keys:


```text
import os
import json
import requests
from dotenv import load_dotenv
from athina.keys import OpenAiApiKey, AthinaApiKey


load_dotenv()
OpenAiApiKey.set_key(os.getenv('OPENAI_API_KEY'))
AthinaApiKey.set_key(os.getenv('ATHINA_API_KEY'))


# Initialize the headers
headers = {
"athina-api-key": os.getenv('ATHINA_API_KEY'),
"Content-Type": "application/json"
}


```


####


Step 3: Manage Datasets


Develop functions to handle datasets effectively:


```text
from athina.helpers.constants import API_BASE_URL


def create_dataset(name, description):
payload = json.dumps({"name": name, "description": description, "source": "api"})
response = requests.post(f"{API_BASE_URL}/api/v1/dataset_v2", headers=headers, data=payload)
return response.json()


def add_rows_to_dataset(dataset_rows, dataset_id):
response = requests.post(f"{API_BASE_URL}/api/v1/dataset_v2/{dataset_id}/add-rows", headers=headers, json={"dataset_rows": dataset_rows})
return response.json()


def not_empty(text):
return text is not None and text.strip() != ""


```


####


Step 4: Conduct the Retrieval Experiment


Configure and run the experiment with different settings:


```text
chunk_sizes = [128, 512, 1024]
chunk_overlap = [0, 10]
dataset_config_to_id_map = {}
for chunk_size in chunk_sizes:
for overlap in chunk_overlap:
print(f"Chunk size: {chunk_size}, Overlap: {overlap}")
dataset_name = "Dataset_{}_{}_{}".format(chunk_size, overlap, int(time.time() * 1000) )
dataset_description = "Dataset with chunk size {} and overlap {}".format(chunk_size, overlap)
dataset = create_dataset(dataset_name, dataset_description)['data']['dataset']
dataset_id = dataset["id"]
dataset_config_to_id_map[(chunk_size, overlap)] = dataset_id
```


####


Step 5: Evaluation


Measure performance using the evaluation suite:


```text
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma
from langchain.document_loaders import TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma
import athina
from athina.evals import ContextContainsEnoughInformation, RagasContextRelevancy


loader = TextLoader("State_of_AI_Report_2023.txt")
data = loader.load()


with open("queries.json") as f:
queries = f.readlines()
queries = "".join(queries)
queries = json.loads(queries)['queries']


for dataset_config, dataset_id in dataset_config_to_id_map.items():
chunk_size, overlap = dataset_config
print(f"Chunk size: {chunk_size}, Overlap: {overlap}")
# Split
text_splitter = RecursiveCharacterTextSplitter(chunk_size=chunk_size, chunk_overlap=overlap)
all_splits = text_splitter.split_documents(data)
valid_documents = [doc for doc in all_splits if not_empty(doc.page_content)]
# Store splits
vectorstore = Chroma.from_documents(documents=valid_documents, embedding=OpenAIEmbeddings())
dataset_rows = []
for query in queries:
relevant_documents = vectorstore.similarity_search(query)
dataset_rows.append({
"query": query,
"context": [ page.page_content for page in relevant_documents ]
})
break
response = add_rows_to_dataset(dataset_rows, dataset_id)
print(response)
print("Added rows to dataset")


# Configure your evaluation suite
eval_model = "gpt-4o"
eval_suite = [
ContextContainsEnoughInformation(model=eval_model),
RagasContextRelevancy(model=eval_model)
]


print("Running evaluation suite")
# Run the evaluation suite
athina.run(
evals=eval_suite,
data=None,
dataset_id=dataset_id,
max_parallel_evals=10
)


```


####


Results and Analysis


The notebook generates detailed results for each retrieval experiment, allowing developers to compare and analyze the performance of different configurations.


By evaluating key metrics such as context recall and relevancy, one can identify the most effective retrieval settings for their specific use case.


Metrics for chunk size 1024:


Metrics for chunk size 512:


Metrics for chunk size 128:


We can clearly observe the impact of varying chunk sizes and overlaps on retrieval performance, enabling developers to optimize their data retrieval strategies for enhanced results.


In this example, the best results were achieved with a chunk size of 1024.


####


Contact Us


For more assistance or to conduct your own retrieval experiments, contact us athello@athina.ai or visit[https://athina.ai](https://athina.ai/) . We are ready to help you refine your data retrieval strategies for optimal results!
