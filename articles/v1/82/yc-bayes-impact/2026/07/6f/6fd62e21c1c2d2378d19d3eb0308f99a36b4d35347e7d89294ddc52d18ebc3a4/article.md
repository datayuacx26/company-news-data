---
schema_version: "1.0.0"
document_id: "6fd62e21c1c2d2378d19d3eb0308f99a36b4d35347e7d89294ddc52d18ebc3a4"
company_key: "yc-bayes-impact"
company: "Bayes Impact"
source_id: "yc-bayes-impact-news-import-46ce02fd9463"
canonical_url: "https://www.bayesimpact.org/blog/tool-calling-open-models/"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-24T19:10:44.510846+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:b6b3f5181aec5a8c1d7aabc30672790ffeaaaebabfc8f281bf9d97170b5d525d"
---

# Why we test every tool with a mid-range model first

We ship the[Bayes Platform](https://github.com/bayesimpact/bayes-platform) to governments and nonprofits across Europe. One of its core features is a conversational agent that can answer questions grounded in an organization’s own documents: employment policy handbooks, medical protocols, legal references.


The retrieval piece is not a pipeline stage or a prompt-engineering trick. It’s a tool call. The LLM decides when to search, formulates a query, and gets chunks back from pgvector. This design keeps the agent modular: RAG is one tool among many (form filling, sub-agent delegation, source citation, resource surfacing), and the orchestration is the same for all of them.


We tested it with Gemini, it worked immediately, and we moved on. Then we added Gemma 4 and realized the tool worked fine, we’d just designed it badly.


## RAG as a tool, not a stage


Most RAG tutorials show a fixed pipeline: embed the query, retrieve chunks, stuff them into the prompt, generate. We do something different. Our agent has a` retrieveProjectDocumentChunks` tool registered alongside its other capabilities. The system prompt tells the model when to use it, but the model decides whether and how to call it, including what query to send and how many chunks to request.


```text
retrieveProjectDocumentChunks  :
description  :   >
Retrieve the most relevant project document chunks
for the current conversation context and latest user question.
parameters  :
conversationSummary  :   string
latestUserQuestion  :   string (required)
topK  :   number (default 20, max 20)
```


The model calls it, gets back chunk content with document metadata and similarity scores, and generates a grounded answer. The Vercel AI SDK’s tool loop handles the call/response cycle automatically.


This matters because the agent isn’t a chatbot with search bolted on. It’s a tool-using agent where retrieval is one capability. The same architecture handles a form-filling agent that calls` fillForm` to progressively populate fields, or a parent agent that delegates to specialized sub-agents via tool calls.


## What happens under the hood


The offline side is standard: documents are uploaded, text is extracted with Docling (PDF, DOCX, PPTX, images), chunked, embedded with Vertex AI, and stored in PostgreSQL with pgvector. For plain text files we use a sentence splitter (512 tokens, 50 overlap), but for structured documents (PDF, DOCX, PPTX) Docling’s hybrid chunker does the splitting. It follows the document’s own structure (headings, sections, paragraphs) rather than cutting at a fixed token count, so there’s no overlap and chunks are usually smaller than 512 tokens. That’s a strength for well-structured documents, though it can over-fragment some files where the source structure is too granular.


The online side is where it gets interesting. When the model calls the retrieval tool, the service concatenates the conversation summary and user question, embeds the combined text, and runs a cosine similarity query against the chunk embeddings. The results come back ranked, with parent-chunk deduplication so the model doesn’t see five overlapping passages from the same section.


No re-ranker (for now 😇), no hybrid search, no ANN index. Brute-force pgvector cosine distance. It’s simple, and for the document volumes our partners have (hundreds, not millions), it works well.


## What we got wrong


The Bayes Platform supports multiple models. Each organization picks the one that fits its constraints: Gemini 2.5 Flash and Pro for teams that can use Google Cloud, but also self-hosted open models for organizations with strict data sovereignty requirements. We run Gemma 4 27B and Mistral Small 3.1 24B on vLLM hosted on our servers.


When we added Gemma 4, our retrieval tool didn’t work as well. Our first reflex was to blame the model, Gemma must be worse at tool calling. But looking at the actual failures, the pattern was clear: we’d built a tool schema that was needlessly hard for any model to use. Gemini is forgiving enough to paper over bad design. Gemma 4 is more literal, and that’s actually useful feedback.


The tool name` retrieveProjectDocumentChunks` is a mouthful that leaks implementation details. The model doesn’t need to know it’s retrieving “chunks” from “project documents.” It needs to know it can search the organization’s knowledge base. A name like` searchKnowledgeBase` would have been clearer for any model, and especially for smaller ones where every token in the schema counts.


The parameters had the same issue.` conversationSummary` is optional and nullable,` topK` defaults to 20 but caps at 20, that’s a confusing spec that invites the model to pass` "null"` as a string or pick arbitrary numbers. Simpler defaults, fewer optional fields, and a clearer description would have helped from the start.


## Designing tools for the model you’ll actually run


We initially tested with Gemini 2.5 Flash. Everything worked on the first try: clean tool calls, proper parameter handling. We shipped it and moved on. The issue is that Gemini is tolerant of ambiguous schemas, it’ll figure out what you meant. That’s great until you add a model that takes your schema at face value.


Our rule now: **test every new tool with a mid-range open model first.** If the schema is clear enough for Gemma 4 27B, it’ll work with anything. The reverse is not true.


What this looks like in practice:


- Name tools for what they do from the user’s perspective, not how they’re implemented.` searchKnowledgeBase` over` retrieveProjectDocumentChunks` .
- Keep schemas flat. Fewer optional fields, explicit defaults, short clear descriptions.
- If a parameter can have a sensible default, make it required with that default, don’t make it nullable.
- Test the full loop (call, execute, feed result back, generate) with every model you support before shipping.


## What’s next


We’re actively reworking the retrieval tool. Beyond the rename, the bigger change is making the tool schema dynamic: instead of one static tool definition, the agent gets a tool shaped to its specific configuration, its document tags, its retrieval scope, its knowledge domain. A healthcare agent shouldn’t see the same retrieval tool as an employment agent. This is the direction we’re heading, and Gemma 4’s literalness is a good forcing function to get the design right.


The self-hosted path matters to us. Some of our government partners cannot send citizen data to a cloud API, full stop. Running Gemma 4 27B on dedicated infrastructure is what makes the platform viable for them. We’re enthusiastic about where open models are going, and building our tools to work well with them is an investment, not a workaround.
