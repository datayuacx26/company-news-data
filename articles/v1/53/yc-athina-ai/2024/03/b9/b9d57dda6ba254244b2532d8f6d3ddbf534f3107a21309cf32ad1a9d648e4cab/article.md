---
schema_version: "1.0.0"
document_id: "b9d57dda6ba254244b2532d8f6d3ddbf534f3107a21309cf32ad1a9d648e4cab"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/how-to-evaluate-your-llama-index-query-engine-using-ragas-evals-athina-ai"
published_at: "2024-03-13T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:8dc43625c20a4b3363006e209e0d23e62e94b20a4e410a614c66b784a1536243"
---

# How to evaluate your Llama Index query engine using Ragas evals + Athina AI

Do not index


Original Paper


Blog URL


If you're using Llama Index to work with advanced retrieval strategies, you're going to need a great evaluation setup.


Ragas is a library of state-of-the-art evaluation metrics for RAG pipelines.


Here's how you can use[Athina's SDK](https://github.com/athina-ai/athina-evals) to run Ragas evals on your Llama Index RAG pipeline.


####


Why use Athina?


- Suite of over 35 state-of-the-art evals


- UI to log and view results for easy comparison


- Quick setup - run a suite of evals in a few lines of code, or set up automatic evals in a few clicks using the UI


- Granular analytics on model performance, segmented at every level.


You can try the[sandbox](https://bit.ly/athina-sandbox) account here.


###


How to run Ragas evals on your Llama Index query engine using Athina


1. **Install the dependencies**


```text
pip install athina
```


1. **Import dependencies and set your API keys**


You'll need to set the` OPENAI_API_KEY` and the` ATHINA_API_KEY` in your .env file.


*(This will still run without the*` *ATHINA_API_KEY*` *but you won't be able to view the results in the nice dashboard UI, or track your experiments)*


```text
import os
from athina.evals import (
RagasContextRelevancy,
RagasAnswerRelevancy,
RagasFaithfulness,
RagasAnswerCorrectness,
)
from athina.runner.run import EvalRunner
from athina.loaders import RagasLoader
from athina.keys import AthinaApiKey, OpenAiApiKey
from llama_index import VectorStoreIndex, ServiceContext
from llama_index import download_loader
import pandas as pd
from dotenv import load_dotenv


load_dotenv()


OpenAiApiKey.set_key(os.getenv('OPENAI_API_KEY'))
AthinaApiKey.set_key(os.getenv('ATHINA_API_KEY'))
```


1. **Create a Llama Index query engine**


Here's some boilerplate code to do this, but if you have a real project using llama index, you're probably already doing something like this.


```text
WikipediaReader = download_loader("WikipediaReader")


loader = WikipediaReader()


documents = loader.load_data(pages=['Y Combinator'])


vector_index = VectorStoreIndex.from_documents(
documents, service_context=ServiceContext.from_defaults(chunk_size=512)
)


query_engine = vector_index.as_query_engine()
```


1. **Load your evaluation dataset**


Here's a very basic example with 2 datapoints.


```text
raw_data_llama_index = [
{
"query": "How much equity does YC take?",
"expected_response": "YC invests $500k in exchange for 7% equity."
},
{
"query": "Who founded YC?",
"expected_response": "YC was founded by Paul Graham, Jessica Livingston, Robert Tappan Morris, and Trevor Blackwell."
},
]


llama_index_dataset = RagasLoader(query_engine=query_engine).load_dict(raw_data_llama_index)


# Optional - print as a pandas DataFrame for nicer visibility
pd.DataFrame(llama_index_dataset)
```


All datapoints must contain a` query` .


For certain Ragas evaluators, you will also require an` expected_response` . This is a reference response that the evaluator can compare to the LLM generated response.


Note that all evaluators do NOT need the expected response field.[See the docs](https://docs.athina.ai/evals/preset_evals/ragas_evals) for more info.


1. **Run the eval!**


Now all that's left is to run the evaluator!


```text
eval_suite = [
RagasAnswerCorrectness(),
RagasFaithfulness(),
RagasContextRelevancy(),
RagasAnswerRelevancy(),
]


# Run the evaluation suite
EvalRunner.run_suite(
evals=eval_suite,
data=llama_index_dataset,
max_parallel_evals=1,   # If you increase this, you may run into rate limits
)


```


Evaluation metrics are on a scale of 0 to 1, with 0.0 being the worst score possible, and 1.0 being the best score possible.


You'll see the results as a DataFrame like this:


The evaluation request will also be logged and saved to Athina, and you can view the results from your Athina dashboard anytime!


These evaluations can help you quickly diagnose issues in your RAG pipeline. Eval-driven development can help you iterate 10x more rapidly than by simply eyeballing retrievals and responses.


The best teams will run a similar suite of evaluations in production as well.


If you want to run such evals automatically on your production logs, try our[sandbox](https://bit.ly/athina-sandbox) , or[contact us](https://cal.com/shiv-athina/30min) to book a demo.
