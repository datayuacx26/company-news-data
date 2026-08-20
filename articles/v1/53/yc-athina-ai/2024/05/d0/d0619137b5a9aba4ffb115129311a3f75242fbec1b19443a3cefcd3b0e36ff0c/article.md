---
schema_version: "1.0.0"
document_id: "d0619137b5a9aba4ffb115129311a3f75242fbec1b19443a3cefcd3b0e36ff0c"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/evaluating-llm-chatbot-conversations-with-athina-ai"
published_at: "2024-05-04T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:e8dfa6971c251e914abf50d3add17d3894c91f6666bea2a79f491aacd4a777f6"
---

# Evaluating LLM Chatbot Conversations with Athina AI

Do not index


Original Paper


Blog URL


Evaluating LLM applications in production comes with a lot of challenges.


Let’s take an evaluation system for a chatbot, as an example.


---


###


Message-level evaluation WITH chat history


Most evaluations will measure the quality or accuracy of ***individual messages.***


For example, some common evaluation metrics for RAG apps are **Answer Relevancy** and **Answer Completeness.**


💡


**Answer Relevancy (**[Github](https://github.com/athina-ai/athina-evals/blob/main/athina/evals/ragas/ragas_evaluator.py) **) (**[Docs](https://docs.athina.ai/evals/preset-evals/ragas#answer-relevancy) **)** Measures how relevant the` response` is to the` user_query`


💡


**Answer Completeness (**[Github](https://github.com/athina-ai/athina-evals/blob/main/athina/evals/llm/does_response_answer_query/evaluator.py) **) (**[Docs](https://docs.athina.ai/evals/preset-evals/does-response-answer-query) **)** Does the` response` completely address the` user_query`


But consider a chat that looks like this:


```text
Conversation


#1
User: I want to buy a smartphone
AI: Great, do you have a brand in mind?


#2
User: Apple
AI: Good choice - would you like to buy an iPhone 15?
```


When message #2 is evaluated, the user_query is just “Apple”, which isn’t a complete query. This means the **Answer Relevancy** evaluator will be useless to run here.


####


So how do we solve this?


Athina’s evaluation orchestration will automatically include the previous chat history as context in the prompt for LLM-based evaluators.


So our Answer Completeness evaluators


💡


**Answer Relevancy** Measures how relevant the` response` is to the` user_query` given the previous` chat_history` as context.


This is done by including the actual chat history in the evaluation prompt ([https://github.com/athina-ai/athina-evals/blob/main/athina/evals/llm/context_contains_enough_information/evaluator.py](https://github.com/athina-ai/athina-evals/blob/main/athina/evals/llm/context_contains_enough_information/evaluator.py) ).


---


###


Conversation-level evaluation


Sometimes, you might want evaluations that measure the entire conversation as a whole.


For example, Athina’s Conversation Coherence evaluator returns a` ConversationCoherence` score that represents **the % of messages that were coherent with the chat** .


- A high score indicates that the AI agent is coherent across the entire conversation.


- A low score indicates that many of the AI responses were incoherent given the previous chat history.


Here’s a post explaining how this works and how you can use it:


####


[How to Evaluate AI Chats Using Conversation Coherence Evaluator](https://blog.athina.ai/how-to-evaluate-ai-chats-using-conversation-coherence-evaluator)
