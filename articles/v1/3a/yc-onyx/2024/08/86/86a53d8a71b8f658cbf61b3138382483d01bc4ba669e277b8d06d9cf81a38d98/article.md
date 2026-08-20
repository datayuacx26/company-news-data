---
schema_version: "1.0.0"
document_id: "86a53d8a71b8f658cbf61b3138382483d01bc4ba669e277b8d06d9cf81a38d98"
company_key: "yc-onyx"
company: "Onyx"
source_id: "yc-onyx-news-import-2a3a6e22cda1"
canonical_url: "https://onyx.app/blog/empower-your-data-local-document-chat-with-llama-3-1"
published_at: "2024-08-22T00:00:00+00:00"
first_seen_at: "2026-07-25T17:58:53.878122+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:e7021f2b01dd130eea8340ffa281024b0efe2bd48480791665a7ba0f1cf129f7"
---

# Empower Your Data: Local Document Chat with Llama 3.1

Many companies cannot send their internal data, documents, and conversations with an external third party such as OpenAI. In order to benefit from AI without sharing their data, these companies can turn to self-hosting open source models. In this post we'll share how you can connect a state-of-the-art LLM to your own knowledge sources/documents and run everything locally without connecting to the internet.


Llama 3.1 405B is the first opely available model that rivals the top AI models when it comes to state-of-the-art capabilities in general knowledge, steerability, math, tool use, and multilingual translation. At the time of writing, the 405B model is ranked fifth on[LMSYS Chatbot Arena Leaderboard](https://arena.lmsys.org/) . The upgraded versions of 8B and 70B models are multilingual and have a significantly longer context length of 128K, state-of-the-art tool use, and overall stronger reasoning capabilities.


With the increased context length, we can start making better use of these open source models by adding our private knowledge sources. To set up the Llama model locally and connect it to your company knowledge base follow these steps:


## Step 1: Install and Configure Ollama


**Install Ollama:** Ollama allows you to host LLMs locally and provides a REST API for the model. Visit[Ollama's Github](https://github.com/ollama/ollama) for installation instructions.


**Run Llama Model** : Start the Llama model with the following command:


**Verify API:** Ensure the API is running with a curl request:


## Step 2: Configure Danswer to Use Ollama


**Run Danswer Locally:** Follow[this quickstart guide](https://docs.danswer.dev/quickstart) to setup and run Danswer on localhost.


**Add A Custom LLM:** Navigate to the LLM tab in the Admin panel and add a Custom LLM. Enter any display name, and use “ollama” for the **Provider Name** .


Ollama configuration page in Danswer


**Configure Llama3.1:** For the API Base use[http://localhost:11434](http://localhost:11434/) and use llama3.1 for the Model Name. Additionally add llama3.1 as your Default Model. After clicking save/update the model should be live in Danswer.


Model configuration page for Ollama in Danswer


## Step 3: Integrate Your Knowledge Sources


**Setup Connectors:** Danswer allows you to connect over 35 different knowledge sources such as Salesforce, Dropbox, and Microsoft Teams. Navigate to the Add Connector page and follow the steps for each connecter you'd like to integrate.


Available connectors page in Danswer


**Chat with Your Docs:** With everything set up you can now query your company data using Llama3.1 and keep all of your data local!


Chatting with Llama 3.1 and your connected docs


If you need any help setting up something similar, feel free to[join our Slack](https://join.slack.com/t/danswer/shared_invite/zt-2lcmqw703-071hBuZBfNEOGUsLa5PXvQ) .
