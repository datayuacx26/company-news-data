---
schema_version: "1.0.0"
document_id: "0fd8f692cab607da48bd59bcc4966cb1fed2aaa9df80ef867868ca98cb59ed25"
company_key: "yc-litellm"
company: "LiteLLM"
source_id: "yc-litellm-news-import-8bfeaefbc2ad"
canonical_url: "https://docs.litellm.ai/blog/valkey_semantic_caching"
published_at: "2026-06-17T10:00:00+00:00"
first_seen_at: "2026-07-22T02:33:29.495129+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:17060ce229ec2188933fd1b440541161e1a2bc0a85c2ab5a09f2b469598053ce"
---

# Semantic Caching on Valkey and AWS ElastiCache

LiteLLM now supports semantic prompt caching on Valkey. If you run a Valkey cluster with the[valkey-search](https://github.com/valkey-io/valkey-search) module, including AWS ElastiCache for Valkey, you can point LiteLLM at it with` type: valkey-semantic` and get embedding-based cache hits without standing up Redis Stack or a separate vector database.


## Why this matters​


Semantic caching stores responses by the meaning of a prompt rather than an exact string match, so a reworded request can still hit the cache and skip a paid model call. Until now LiteLLM's semantic cache was built on RedisVL, which depends on RediSearch's` FT.*` vector API. RediSearch is not available on Redis OSS or on ElastiCache for Redis OSS, which left teams standing up Redis Stack or Qdrant just to get semantic caching. With Redis moving to a source-available license, more teams are standing up Valkey instead, and ElastiCache for Valkey is a common managed target.


Valkey ships vector search through the valkey-search module, and ElastiCache for Valkey exposes it. LiteLLM's new backend talks to valkey-search directly over the Redis protocol, so semantic caching on ElastiCache for Valkey works without RediSearch, Redis Stack, or Qdrant in the path.


## How it works​


The` valkey-semantic` backend builds its own vector index from the field types valkey-search supports, a tag field that isolates each cache key's scope and an HNSW vector field for the prompt embedding, then runs a KNN query at lookup time and returns the cached response when the cosine similarity clears your threshold. Prompt extraction, embedding generation, and response handling are shared with the existing Redis semantic cache, so behavior matches the Redis path including per-request scope isolation. Connections resolve from` VALKEY_HOST` ,` VALKEY_PORT` , and` VALKEY_PASSWORD` , falling back to the` REDIS_*` equivalents, and passwordless clusters are supported for IAM or no-auth setups.


## Get started​


Add the cache to your` config.yaml` :


```text
litellm_settings  :         cache  :     True         cache_params  :           type  :   valkey  -  semantic          host  :   os.environ/VALKEY_HOST          port  :   os.environ/VALKEY_PORT          valkey_semantic_cache_embedding_model  :   openai  -  embedding          similarity_threshold  :     0.8
```


For ElastiCache with encryption in transit, pass a` rediss://` URL through` cache_params.redis_url` instead of host and port. To try valkey-search locally, the bundled image has the module ready:


```text
docker run -d -p 6379:6379 valkey/valkey-bundle:8.1
```


See the[caching docs](https://docs.litellm.ai/docs/proxy/caching) for the full setup, including the SDK usage and the parameter reference.
