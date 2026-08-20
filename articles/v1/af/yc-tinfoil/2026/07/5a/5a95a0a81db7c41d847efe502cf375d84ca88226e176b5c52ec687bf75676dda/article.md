---
schema_version: "1.0.0"
document_id: "5a95a0a81db7c41d847efe502cf375d84ca88226e176b5c52ec687bf75676dda"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2026-07-14-secure-prompt-caching"
published_at: "2026-07-14T12:00:00+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:04863c0a9da5cdb092aa835cdf20b92a041df43175e74d807a5f177e4b3c8b98"
---

# Architecting secure prompt caching

[← Back to Posts](https://tinfoil.sh/blog)


# Architecting secure prompt caching


Jul 14, 2026


•


10 min read


Tanya Verma and Sacha Servan-Schreiber


We're excited to announce cached prompt[pricing](https://tinfoil.sh/pricing/#api) in the Tinfoil Inference API. This has been one of the most frequently requested features because it makes eligible inference requests require less compute, which is great for us because we can serve more customers and great for customers because they pay less. It works by caching inputs that the model has recently processed, which has an outsized benefit on long-context tasks such as coding agents. We've taken our time implementing prompt caching because caching is privacy's natural enemy, and a hasty implementation could weaken the privacy guarantees that Tinfoil provides. In this post, we explain these pitfalls, and how we avoid it.


The problem is that caching inherently requires keeping state around, and that state introduces timing side channels that attackers can exploit to learn information they shouldn't. Specifically, an attacker can observe if something is processed faster or slower and infer whether it was cached. While this may seem innocuous at first, timing side channels can leak[secret keys](https://cr.yp.to/antiforgery/cachetiming-20050414.pdf) and other[private information](https://www.cs.jhu.edu/~rubin/courses/fall03/papers/timing.pdf) , and have underpinned many high-profile attacks, including[Spectre](https://spectreattack.com/spectre.pdf) and[Meltdown](https://meltdownattack.com/meltdown.pdf) . Unfortunately, caching is also extremely necessary as it enables efficiency across nearly all layers of modern computing infrastructure.


## Prefix caching makes inference go brrrr


Inference consists of two phases:


**Prefill** : reading your prompt. The model ingests all tokens from the entire prompt at once, in parallel. This work scales with the length of the prompt, and it has to finish before the model produces the first token of the response.


**Decode** : generating the full response. The model then generates the answer one token at a time, and each token depends on the previous one, so output generation cannot be parallelized.


The idea is that the prefill computation can be performed once, then saved, and then reused if part of a future prompt is identical. Inference libraries/frameworks like vLLM and SGLang use different approaches to achieve this central idea.


Or in other words, prefix caching stores the preprocessed conversation history up to the latest user message (i.e., the "prefix") for a given session. Functionally, this means that for each additional user message, the full context up to that message is already preprocessed and cached, which speeds up inference and reduces API usage costs for end users. As a result, all model providers take great steps to ensure that they can optimize the prompt structure to maximize caching. This is especially important when the input context is large, for example with many agentic workloads such as coding. See for instance these write-ups from the[Codex team](https://openai.com/index/unrolling-the-codex-agent-loop/) and the[Claude Code team](https://code.claude.com/docs/en/how-claude-code-works) on how they architected the agent loop. Figure 1 illustrates how prefix caching reuses prefill computation across requests.


**Figure 1:** Prefix caching: the prefill computation for a shared prompt prefix is performed once and reused for subsequent requests (conversation turns), reducing latency and cost.


## Cache timing attacks and how salting stops them


A cache shared across users introduces a timing side channel that can be exploited by any user of the inference service.


To see this, consider a user, Alice, who makes a request for` hello` , which gets processed and stored in the prefix cache. Another user, Bob, can make a request for` hello` and observe that the inference server responded a few milliseconds faster than usual, leading Bob to conclude that Alice's prompt also had the prefix` hello` .


The solution to prevent this type of cross-user attack is to salt the cache. Recall that a request hits the cache only if its tokens match a cached prefix exactly, starting from the very first token. A cache salt is a per-user value that acts like an invisible first word on every prompt the user sends: Alice's request is effectively` ⟨alice's salt⟩ hello` and Bob's is` ⟨bob's salt⟩ hello` . Because they differ from the first token onward, so to the cache the two prompts look nothing alike, and Bob can never hit (or time) an entry that Alice created.


In short, salting partitions the cache to ensure every user only has access to their own cache. Partitioning a cache key by security or privacy context is a well-established mitigation for cache timing attacks, for instance,[HTTP caching](https://datatracker.ietf.org/doc/html/rfc9111#name-timing-attacks) guidance refers to the analogous technique as "double keying". The same pattern is used in production inference systems:[vLLM](https://github.com/vllm-project/vllm/issues/16016) and[NVIDIA Dynamo](https://docs.nvidia.com/dynamo/dev/additional-resources/nvidia-request-extensions-nvext#cache-salt-and-tenant-isolation) support per-request` cache_salt` and hosted providers including[OpenAI](https://developers.openai.com/api/docs/guides/prompt-caching#frequently-asked-questions) and[Anthropic](https://platform.claude.com/docs/en/build-with-claude/prompt-caching#faq) isolate prompt caches at organization or workspace boundaries. Figure 2 illustrates how standard cache salting partitions the cache per user.


**Figure 2:** Standard cache salting: the cache is partitioned using a per-user salt, ensuring each user only accesses their own cache and preventing cross-user side-channel attacks.


## Server-side cache salting is not enough for private inference


Conventional cache isolation assumes that the cache operator is trusted. For example, a model provider may partition its prompt cache by organization ID or user ID. This prevents ordinary users in different namespaces from reusing or probing one another’s cache entries.


Tinfoil has a stronger threat model: the infrastructure operator itself is not trusted with the contents of inference requests. Tinfoil itself must not be able to probe a customer's cache namespace.


Suppose Alice’s namespace is determined only by:


This means Bob, who doesn't know Alice's API key, cannot authenticate as Alice. But Tinfoil *does* know Alice's API key, we have to bill her for tokens! If an operator can cause a request to be evaluated under Alice's API key, it can submit guessed prefixes and use hit-vs-miss timing to test whether those prefixes are present in Alice's cache.


The weakness here is that every input selecting the cache partition is known to or controlled by the operator.


A client-held secret changes this:


The tenant ID anchors the namespace to the correct customer and the client-held secret prevents the operator from probing Alice's cache.


## Client-keyed cache salting


What makes for a good client-held secret? We have two goals:


1. Unknown to Tinfoil
2. Allow cache sharing for end-users across sessions.


Here, it helps to lay out the possible namespaces for a prefix cache, what each is derived from, and whether it protects against the operator:


Cache namespace Namespace input Intended sharing Protected from operator?


Global None All users No


Organization-scoped Provider-known organization ID Everyone in one organization No


Provider-keyed user scope Provider-known user ID One user across sessions No


**Client-keyed user scope** Tenant ID + client-held secret One user across sessions **Yes (enforced in the enclave)**


Client-keyed session scope Tenant ID + session secret One session Yes (enforced in the enclave), but with less cache reuse


A session ID in principle could satisfy the "unknown to Tinfoil" requirement. The downside, however, is that session scoping severely limits the effectiveness of caching. For instance, a single user who is running a coding agent and has an AGENTS.md file would not be able to benefit from prompt caching speedups of their AGENTS.md file across two coding sessions.


To this end, we upgraded our SDKs to generate a client-side secret. The enclave combines it with the authenticated tenant identity to derive the cache namespace:


The secret is encrypted directly to the secure enclave where the inference request is processed and sent along with the request. This ensures that when you use[one of the SDKs](https://docs.tinfoil.sh/sdk/prompt-caching) or the Tinfoil proxy, caching is enabled and remains secure from side-channel attacks, even from Tinfoil.


## Cache-aware routing


Salted caching using client-side secrets solves the privacy requirement. But on the server side, another challenge is dealing with model replicas. Each model replica keeps its own prefix cache in GPU memory, so a request only benefits from an earlier one if both land on the same replica. If it's routed anywhere else, it will have to pay full prefill again.


The router (also running in the enclave) therefore needs to be cache-aware and send repeat prefixes from the same namespace back to the same replica. The routing key should therefore take into account the cache salt along with the prefix. Additionally, the user should not be able to influence which replica a request gets routed to, so as to prevent any denial-of-service through targeted overloading. Based on that, the routing key is derived as follows:


To design secure cache-aware routing, we derive the route deterministically based on the` user_cache_secret` and a` tinfoil_routing_secret` . The` tinfoil_routing_secret` is known only to Tinfoil, which prevents replica targeting. By having the two secrets involved in determining routing choice, we can guarantee that neither the user nor Tinfoil can force a replica to be selected for a particular request (i.e., the replica selection cannot be computed offline or maliciously influenced by anyone).


To actually determine which model replica sees the request, we use rendezvous hashing to rank the healthy replicas for each routing key. Under normal load, requests prefer the key's highest-ranked replica. For hot or frequently used keys, the router expands the warm replica set and selects the least-loaded member of that set. This helps preserve cache locality without creating a single-replica bottleneck for hot keys.


## What if a user doesn't set their user_cache_secret?


By default, the SDKs and the Tinfoil proxy will either use a user-specified` user_cache_secret` , or mint one automatically if it isn't provided. But if a user doesn't provide one, such as if they're not using Tinfoil via an official client, the cache salt will fall back to a tenant-wide salt (derived from the API key). So these customers will still get cached pricing, isolated per API key, protected from other customers but not from Tinfoil.


## How long is the cache present for?


All cached requests are kept in the GPU VRAM (in the future we may support[KV cache offloading](https://vllm.ai/blog/2026-01-08-kv-offloading-connector) restricted to CPU RAM, which is also volatile). This is the default model exposed by vLLM, which is the inference server that we use. The cache is not exported or persisted to disk. This simplifies the security model since it ensures that there is no state between enclave restarts. If the enclave is shut down, all volatile memory goes with it. We do not set an explicit expiration time for the cache (this might change in the future). A cached prefix will stay around until it is evicted due to resource constraints.


## Getting started with secure caching


For details on cached input token pricing, see our[pricing page](https://tinfoil.sh/pricing/#api) and check out the[docs](https://docs.tinfoil.sh/sdk/prompt-caching#prompt-caching) . To take advantage of secure caching, you'll need to update to the latest SDK, which will include the necessary client-side changes to enable secure caching on API requests. We hope this will enable a new class of applications that use private inference with Tinfoil, such as coding agents and other high-context workloads.


As always, please reach out with questions or feedback!


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2026-06-23-confidential-computing-overhead)
