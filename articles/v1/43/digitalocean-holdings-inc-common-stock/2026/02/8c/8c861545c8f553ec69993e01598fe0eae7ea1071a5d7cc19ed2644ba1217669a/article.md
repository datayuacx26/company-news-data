---
schema_version: "1.0.0"
document_id: "8c861545c8f553ec69993e01598fe0eae7ea1071a5d7cc19ed2644ba1217669a"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/gradient-ai-platform-llamaindex-integration"
published_at: "2026-02-18T20:23:52.293+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:19:40.776398+00:00"
content_hash: "sha256:f21e2041e55d62b29985a1958e2cbfc0aa755564c92d55900dbf131ccce08b33"
---

# DigitalOcean Gradient™ AI Platform Now Integrates with LlamaIndex

We’re excited to announce that DigitalOcean Gradient™ AI Platform now integrates natively with LlamaIndex - one of the most popular frameworks for building RAG applications.


This means you can now connect your Gradient AI Platform Knowledge Base and LLMs directly to LlamaIndex workflows, using the abstractions you already know. No additional infrastructure. No complex setup. Just install two packages and start building.


## Why This Matters


If you’ve built RAG applications before, you know the drill: provision a vector database, set up an embedding pipeline, manage credentials across services, and stitch everything together. It’s a lot of overhead before you write a single line of application logic.


With these new integrations, we’ve done the heavy lifting. Your Knowledge Base handles document ingestion, chunking, and embeddings. The LlamaIndex retriever connects directly to it. Add our LLM integration, and you have a complete RAG pipeline running on managed DigitalOcean infrastructure.


## What’s New


Two packages are now available on PyPI:


[llama-index-retrievers-digitalocean-gradientai](https://pypi.org/project/llama-index-retrievers-digitalocean-gradientai/) Connect to your Knowledge Base as a LlamaIndex retriever. Supports hybrid search (keyword + semantic), metadata filtering, and async operations.


[llama-index-llms-digitalocean-gradientai](https://pypi.org/project/llama-index-llms-digitalocean-gradientai/) Use Gradient AI Platform-hosted LLMs in your LlamaIndex workflows. Supports streaming responses and async for high-throughput applications.


Both packages work with LlamaIndex query engines, chat engines, callbacks, and the broader ecosystem.


## Get Started in Minutes


Install the packages:


```text
pip install llama-index-retrievers-digitalocean-gradientai llama-index-llms-digitalocean-gradientai


```


From there, configure your Gradient AI Platform credentials and drop the retriever and LLM into your existing LlamaIndex code. Check out ourdocumentation for a complete walkthrough and code examples.


## What You Can Build


These integrations open up a range of possibilities:


- **Support assistants** grounded in your product documentation
- **Internal tools** that query company wikis and runbooks
- **Code assistants** with context from private repositories
- **Research tools** for document-based Q&A


If you’re already using LlamaIndex, you can integrate the Gradient AI Platform into your existing application. If you’re starting fresh, you now have a fully managed path from Knowledge Base to production RAG app.


## What’s Next


This is just the beginning. We’re continuing to expand Gradient AI Platform integrations with popular AI frameworks, and we’d love to hear what you’re building.


Get started with LlamaIndex - or explore the[Gradient AI Platform documentation](https://docs.digitalocean.com/products/gradient/) to learn more about Knowledge Bases and hosted LLMs.


Happy building!
