---
schema_version: "1.0.0"
document_id: "c09369861aa3f584dcd6afffa5a42ad43640c0876c877de291cbe6d044f66cfd"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/cookbook-how-to-set-up-langchain-tracing-on-athina-in-2-minutes"
published_at: "2024-04-22T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:677648cf06aaa87bdd27572e3f307633c58160d1aa153507ee788f2515b9e522"
---

# Cookbook: How to set up Langchain tracing on Athina in 2 minutes

Do not index


Original Paper


Blog URL


Athina Tracing works with Langchain through Langchain Callbacks, a feature of the[Athina Logger](https://pypi.org/project/athina-logger/) SDK.


This SDK automatically generates detailed, nested traces for each Langchain application execution, which can then be used to set up monitoring, debugging, alerting and evaluations.


###


Importance of Tracing


Here is what a Langchain trace looks like once it is logged to Athina. Try out our interactive demo below.


For developers working with Language Learning Models (LLMs), tracing and logging are crucial for:


- Observability


- Debugging


- Performance monitoring


- Ensuring applications run smoothly


Athina's tracing capabilities enable automatic capture of detailed execution flows and data interactions, vital for optimizing and scaling applications.


###


How It Works


Athina's tracing mechanism involves attaching a callback handler to your Langchain application. This handler gathers detailed run information and transmits it to Athina's platform for viewing and analysis, with minimal codebase modifications required.


###


Step-by-Step Tutorial


Follow these steps to set up Langchain tracing with Athina:


####


Step 1: Install Necessary Packages


Install Athina Logger SDK, Langchain, and Langchain OpenAI using pip:


```text
pip install athina-logger langchain --upgrade
```


####


Step 2: Set Up Athina API Key


Enable logging with your Athina API Key:


```text
from athina_logger.api_key import AthinaApiKey
import os


AthinaApiKey.set_api_key(os.getenv('ATHINA_API_KEY'))
```


####


Step 3: Configure Langchain Callback Handler


Set up the Langchain Callback Handler to trace every execution:


```text
from athina_logger.tracing.callback.langchain import LangchainCallbackHandler
athina_handler = LangchainCallbackHandler()
```


####


Step 4: Add the Athina callback handler to start logging Langchain traces


```text
chain = LLMChain(llm=ChatOpenAI(openai_api_key=OPENAI_KEY), prompt=chat_prompt)


# THIS IS ALL YOU NEED TO ADD
response = chain.invoke('AI', {'callbacks': [athina_handler]})
```


That’s it! You’re all done 🎉


###


Need Help?


For any inquiries or further assistance with tracing integration for your Langchain applications, contact us athello@athina.ai or visit[https://athina.ai](https://athina.ai/) . We’re here to support you!
