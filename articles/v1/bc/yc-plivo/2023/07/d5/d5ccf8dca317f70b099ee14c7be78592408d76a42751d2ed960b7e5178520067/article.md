---
schema_version: "1.0.0"
document_id: "d5ccf8dca317f70b099ee14c7be78592408d76a42751d2ed960b7e5178520067"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/askplivo-chatbot/"
published_at: "2023-07-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:04085079ad12795bf8fd40f0ac5d5c6ee24268358a8447113181a7ce390bb96b"
---

# How I Built Our Internal /askplivo AI-Powered Chatbot for Slack

*Plivo’s CTO Michael Ricordeau writes …*


I’ve been exploring AI and wanted to test a few things on my own because that’s what I enjoy doing as[CTO at Plivo](https://www.linkedin.com/in/michaelricordeau/) . I wanted to learn about[OpenAI](https://openai.com/) tools and build something useful for the company at the same time. The result was our new /askplivo chatbot for Slack.


We have a lot of resources on our website, including API documentation, blog posts, and use case guides. It can be challenging for our employees to navigate through them and find what they want.


Occasionally, I would see questions popping up on our Slack channels about a feature, or telecom regulations (I’m looking at you,[10DLC](https://www.plivo.com/docs/sms/a2p-10dlc/quickstart/) ), or country coverage. If we could have a Q&A chatbot assistant capable of answering questions instead of waiting (desperately, sometimes) for a co-worker to reply we could be more productive.


For instance, imagine you’re a Plivo support engineer and a customer is asking whether Plivo supports[SMS](https://plivo-webflow.webflow.io/sms) in France. (I use France as an example because that’s where I’m from, but you can use any other country;[we support more than 190](https://plivo-webflow.webflow.io/sms-coverage/rs) .) With a chatbot, you could get an answer in less than a minute.


Because my goal was to learn more about AI and OpenAI, I started exploring how I could build a chatbot that used Plivo’s content enhanced with the power of AI. I wanted something that would be smart enough to answer with relevant information and not[hallucinate](https://en.wikipedia.org/wiki/Hallucination_(artificial_intelligence)) , which is a well-known problem.


## Designing the AI chatbot


I like Python for prototyping and[MVP](https://en.wikipedia.org/wiki/Minimum_viable_product) because the syntax is simple, and I’ve been using it for 20 years. Granted, it’s not super performant, but I’m not planning to build the next Google here. I wanted a lightweight web framework, so I used[Flask](https://flask.palletsprojects.com/) to expose the API to interact with Slack and[RQ](https://python-rq.org/) to process jobs in the background.


For AI, I found LangChain, a Python (and TypeScript) framework that abstracts many of the[LLM](https://en.wikipedia.org/wiki/Large_language_model#:~:text=A) and indexing parts. It’s well-integrated with the OpenAI API.


To store, index, and query (search) the data, I wanted to use a[vector database](https://learn.microsoft.com/en-us/semantic-kernel/memories/vector-db) . I had many challenges choosing one: I tried[RedisSearch](https://redis.io/docs/stack/search/) and[FAISS](https://github.com/facebookresearch/faiss) but ended up using[Qdrant](https://qdrant.tech/) ; the cloud version offers a free tier that was good enough for my use case, and it’s written in Rust, so it’s memory-efficient.


Finally, I picked[Fly](https://fly.io/) to host the software. I wanted to test Fly because it’s cheap and easy to deploy and scale with a Dockerfile, and I didn’t want to use Heroku anymore.


Here’s how everything fits together:


## Technical details and pain points


Building the application was both fun and a learning experience.


I had high hopes for using FAISS, but it’s not compatible between x86_64 and arm64 architectures, so if, like me, you’re developing on an arm64-based Apple M1/M2, you cannot build your FAISS index file and upload it to your x86_64 server — and the Fly hosting platform doesn’t support arm64. I turned instead to Qdrant.


To feed the data in the Qdrant vector database, I took the[Plivo](https://www.plivo.com/) website sitemap.xml file, scraped all of the URLs, extracted the content, and transformed it into LLM embeddings, then inserted it in Qdrant. If you do this, be warned that OpenAI will bill you for the embeddings.


Next, I loaded the documents. LangChain, by default, tries to load all documents in memory, so I got quite a lot of OOM killer in my Fly instance. I didn’t want to triple the memory usage (and my bill), so I built a data importer to process documents in chunks instead of loading everything in one shot. Also, LangChain has no GitHub data importer, so I wrote one.


At this point, I had the resources I needed for the chatbot hosted on Fly. I should note that while most of the tools I used are lightweight and fast enough for our use case, OpenAI can be a bottleneck. If you use the GPT-4 model, response time can take up to two minutes, depending on the complexity of your question. I had to change the API timeout for the OpenAI Python SDK to 180 seconds.


Once I had the back end set up, the next step was to create the Slack bot app from the Slack admin console. I’ve documented the process of configuring and deploying the application in the README page of the project’s GitHub repository.


## Why don’t we consult the oracle?


After a few hours of development time, I had a working chatbot. Here’s what using it looks like.


## Et voilà!


Want to develop something like the /askplivo bot? Check out[my code](https://github.com/plivo/plivoaskme) — it’s open source, so feel free to use it and fork it. You’ll find there’s room for improvement and a few challenges to overcome:


- The search query is optimized for text. Most of the time, it won’t use the GitHub indexed code; I need to dig into that.
- It occasionally hallucinates responses — making up a 10DLC API resource, for instance. I’m planning to experiment with[Anchor GPT](https://github.com/dioptra-ai/anchor-gpt) to fix that.
- If you add new documents, you have to re-index all documents from scratch, which is slow and costly.
- There is no chat session concept — each question/response is always unrelated to the previous ones. I’m not sure we need this feature.


If you use the code, and especially if you enhance it in a way it can benefit others, feel free to contribute and send a PR.
