---
schema_version: "1.0.0"
document_id: "364856c7b22cb78ea06d8566efe1338acdec34f8a8a618e128b2ed6e97cf40df"
company_key: "yc-phospho"
company: "phospho"
source_id: "yc-phospho-rss-5d0953ae6f5a"
canonical_url: "https://blog.phospho.ai/ai-chat-bubble-how-to-create-a-free-custom-ai-assistant/"
published_at: "2024-11-14T16:13:31+00:00"
first_seen_at: "2026-07-25T18:58:01.685792+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:d8c2a4c7e00633025f0843b593fe283f509635f32c506bc1b572a90e722d6a81"
---

# Ai chat bubble: how to create a free custom AI assistant

Exciting news, phospho is now bringing brains to robots!


With phosphobot, you can control robots, collect data, fine-tune robotics AI models, and deploy them in real-time.


Check it out here:[robots.phospho.ai](https://robots.phospho.ai/?ref=blog.phospho.ai) .


Deploy easily you custom AI assistant following this Github repository:[AI Chat Bubble](https://github.com/phospho-app/ai-chat-bubble?ref=blog.phospho.ai) .


0:00


/ 0:10


This AI assistant crawls a specific website, processes its content, and answers questions based on it. Let's break down the core concepts behind this repository.


### 1. Extracting Website Content


The first step is to gather the text content from a website. To achieve this, the repository uses **Scrapy** , a Python framework designed for web scraping. Scrapy is used to create a “spider”: a program that visits a given URL, extracts the text content from that website, and breaks it down into smaller chunks.


The crawling process in the repository is set up with a depth of **3** . The spider collects information from the main page and also follows links from the main page. It gathers additional content up to three levels deep. This depth ensures that the assistant can understand a wide range of content related to the website.


### 2. Loading Data into a Vector Database


Once the website content is extracted, it is embedded using a **hugging face embedding model** called **BAAI/bge-small-en-v1.5** . Then the embeddings vectors are then stored in the **Qdrant** database, making it easier for the AI to retrieve relevant information when asked a question.


### 3. Asking Questions


After the content is stored and indexed, the AI assistant is ready to answer questions. This functionality is built around an API endpoint that allows users to submit questions. When a user asks a question, the system queries the vector database for the most relevant content. It then processes this content and generates an answer based on the data stored in the database.


Now you understand the code behind[AI Chat Bubble](https://github.com/phospho-app/ai-chat-bubble?ref=blog.phospho.ai) . Follow the quickstart to launch your own assistant !


## Want to take AI to the next level?


At Phospho, **we give brains to robots** . We let you power any robot with advanced AI – control, collect data, fine-tune, and deploy seamlessly.


New to robotics? Start with our **dev kit** .


👉 Explore at[robots.phospho.ai](https://robots.phospho.ai/?ref=blog.phospho.ai) .
