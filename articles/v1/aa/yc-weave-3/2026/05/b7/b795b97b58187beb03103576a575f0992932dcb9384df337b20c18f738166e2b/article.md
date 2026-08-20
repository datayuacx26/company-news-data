---
schema_version: "1.0.0"
document_id: "b795b97b58187beb03103576a575f0992932dcb9384df337b20c18f738166e2b"
company_key: "yc-weave-3"
company: "Weave"
source_id: "yc-weave-3-news-import-b8160165addf"
canonical_url: "https://weaveos.com/blog/introducing-weave-router-right-sizing-inference-for-production-agentic-workloads"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-22T19:33:03.286750+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:a0e1755d73872248aacdbc7183dcf8a820f14206566b875d27426091bbbba0d5"
---

# Introducing Weave Router: Right-Sizing Inference for Production Agentic Workloads

Most teams running agentic workloads pick one frontier model and send every request to it. That means short completions, simple lints, and trivial autocompletes get billed at the same rate as long-context reasoning. The routine majority of traffic ends up subsidizing the small minority of requests that actually need a frontier model.


This isn't a model problem. It's a routing problem. When the call site only knows about one tier, every request pays frontier prices. In the Claude Code workloads we've analyzed, 60 to 70 percent of requests are short, structurally simple completions that an open-source model handles at parity, at roughly one-fortieth of the cost.


The fix is per-request routing, not a cheaper default. Each call goes to the lowest-cost model that can produce an equivalent answer, and Weave Router makes that decision automatically on every request.


## A proxy that picks the right model per request


Weave Router is a standalone Go service that sits in front of every model provider. It speaks whatever wire format your client already uses (Anthropic Messages, OpenAI Chat Completions, or Google Gemini` generateContent` ), decides whether the request warrants a frontier model, and dispatches to the appropriate upstream. Clients update one base URL and nothing else.


Routing isn't a heuristic over prompt length or a hard-coded keyword list. Each request is embedded with a small ONNX model, scored against a frozen set of intent clusters, and matched to the lowest-cost candidate that has historically performed at parity with the frontier on that cluster. The scorer is derived from the AvengersPro research line, retrained on production traffic, and shipped as a versioned artifact bundle. Routing changes go through the same release process as any other code change. On the public[Router Arena](https://github.com/RouteWorks/RouterArena) leaderboard, Weave Router currently holds the top position.


You can run it two ways:


-


**Managed (recommended).** Sign up at[router.workweave.ai](http://router.workweave.ai/) , add your provider keys, and point your client at our base URL. Routing is active within minutes, using the artifact bundle we tune against production traffic on an ongoing basis.


-


**Self-hosted.** For teams with data-residency requirements or air-gapped environments, the same binary runs in your VPC. Source at[github.com/workweave/router](http://github.com/workweave/router) .


Both shapes expose the same three surfaces:


-


**The proxy.** A drop-in replacement for the Anthropic Messages, OpenAI Chat Completions, and Gemini APIs. Existing clients need no changes beyond the base URL.


-


**The decision endpoint.** A read-only routing call that returns the model, provider, and rationale for any request body without proxying upstream. Useful for shadow runs, eval pipelines, and dashboards that need to inspect routing decisions before committing traffic.


-


**The dashboard.** API key issuance, BYOK credential rotation, and routing-decision telemetry with per-request cost, latency, and decision-reason drill-downs.


## Inside a routing decision


When a request arrives, the router parses it into a format-agnostic envelope. The envelope is lazy: it doesn't deserialize the full body unless a cross-format emit is required, so a request that arrives as Anthropic and dispatches to an Anthropic upstream skips the JSON round-trip entirely.


The envelope's prompt is then embedded with a small in-process ONNX model. The embedding gets scored against a frozen set of cluster centroids, top-p selection narrows the field, and the candidate model with the highest score in the surviving clusters wins. The routing path adds low single-digit milliseconds of overhead on top of the upstream call.


## Design choices


Two decisions inside the routing layer do most of the work that separates this from a generic LLM router.


**Selection is multi-objective, not quality-maximizing.** Within each surviving cluster, the scorer doesn't pick the highest-scoring candidate. It picks the candidate that wins on a cost / latency / verbosity composite, conditional on quality being at parity with the top option on that cluster. Cost is the obvious axis, and it's where the 80%+ savings come from. Latency matters because two models can be quality-equivalent and still differ by seconds on time-to-first-token, which is the difference between an autocomplete feeling instant and feeling broken. Verbosity matters because two models can be quality-equivalent on the answer and still differ 3–5x in output tokens, which is both a cost multiplier and a UX multiplier in agentic loops. Most routers in the literature collapse this into a single quality objective and treat the rest as downstream concerns; we treat all three as first-class signals inside the decision itself.


**Decisions are sticky per session, not per request.** A naive router re-picks the optimal model on every turn, which sounds good in isolation and is catastrophic in practice for agentic workloads. The cost model of every major provider is built around prompt caching: cached input tokens are roughly an order of magnitude cheaper than fresh ones, and an agent loop that re-sends a multi-thousand-token system prompt on every turn relies on those cache hits compounding. Turn one warms a cache on provider A; if turn two routes to provider B, the agent pays full freight to re-prime an identical context, *and* the cache on A goes cold by the time turn three wants it back. The bill goes up, not down. Weave Router pins a session to its first routing decision, so cache hits compound the way the upstream provider intended. The scorer only revisits the decision at boundaries where the swap is actually cheaper end to end, after accounting for the cache-warmup cost on the new provider.


## Why most routers break agentic workloads, and why this one doesn't


A general-purpose router optimized for one-shot chat completions can get away with picking a fresh model per request. An agentic router can't. In a Claude Code session, a coding agent loop, or any long-running tool-use conversation, the same multi-thousand-token system prompt and conversation history get re-sent on every turn. Prompt caching is what makes that economically survivable. Cached input tokens are roughly an order of magnitude cheaper than fresh ones, and on long contexts that gap is the entire business case.


A naive router that re-picks the optimal model on every turn destroys this. Turn one warms a cache on provider A; turn two routes to provider B and pays full freight to re-prime an identical context; turn three routes back to A and finds the cache cold because the TTL elapsed during the detour. The bill goes *up* , not down, and latency degrades on every turn. The second failure mode is format translation: a client that depends on Anthropic's` cache_control` markers can't tolerate a router that strips them on the open-source path, because the open-source path then bills as if caching didn't exist.


Session pinning (covered above) addresses the first failure mode. The second one is in the format-translation layer. Cache-control markers, extended-thinking blocks, tool-use payloads, and Gemini` thoughtSignature` round-trips are preserved across format translation rather than collapsed to a lowest-common-denominator shape. The three wire formats (Anthropic, OpenAI, Gemini) are treated as peers. Same-format paths stay close to the inbound bytes; cross-format paths share a canonical intermediate representation. Capabilities added to one ingress surface consistently across the others.


The result: your agent keeps speaking its existing API, caches stay warm, and the router only swaps models at boundaries where the swap is actually cheaper end to end.


## Where the money actually goes


A frontier model serving one hard request isn't expensive. A frontier model serving thousands of trivial requests is. That's the cost structure most teams discover only after their bill compounds.


On production Claude Code traffic, we're measuring 80 to 85 percent cost reductions with no observable quality regression, calculated by comparing what each request actually cost against what it would have cost if dispatched to the frontier model the client originally requested. The savings come from routing the routine majority of requests to open-source models via OpenRouter and reserving frontier models for the requests that warrant them. Semantic caching, scoped per cluster with per-cluster similarity thresholds, drives cost further down.


None of this is a research breakthrough. It's infrastructure work with a clear payback period. The reason most teams haven't built it themselves is that the format translation layer is non-trivial, and getting it wrong silently degrades your traffic.


## One bill, no provider sprawl


The managed service handles the upstream provider relationships so you don't have to. We hold the Anthropic, OpenAI, OpenRouter, and Gemini accounts, negotiate the rate limits, eat the noisy-neighbor problems, and bill you a single unified line item in credits. There's no Anthropic invoice plus an OpenAI invoice plus an OpenRouter invoice plus a Gemini invoice arriving on different days with different cost structures. Just one usage-based balance against your Weave account.


This matters more than it sounds like it should. The reason most teams haven't already routed across providers isn't that the technical problem is hard; it's that managing four upstream billing relationships, four sets of rate limits, and four key rotation cadences is its own ongoing tax. Centralizing the provider layer is what makes the cost win actually accessible.


For teams with data-residency requirements or air-gapped environments, the self-hosted binary takes your own provider keys directly. Same routing logic, same evals; you keep the upstream relationships and the billing stays between you and each provider.


## Features that weren't economical at frontier rates


The interesting consequence of right-sizing each request isn't the line item on next month's invoice. It's the floor it removes from the price of building AI features. Workloads that didn't pencil out at frontier rates suddenly do: ambient agents firing on every keystroke, long-context summarization across full codebases, per-user retrieval re-evaluated on each page load.


Frontier models stay available for requests that actually need them. They just stop getting charged for the ones that don't.


## The model is no longer the product


The router is the first piece of a larger thesis: the model isn't the product anymore. The system around it (routing, caching, evaluation, cost telemetry, credential management) determines whether an AI feature is good, economical, and shippable. Weave's job is to ship that system as infrastructure.


What's in progress: per-customer routing artifacts that adapt to a workload's distribution over time, a learned cache that goes beyond cosine similarity, and tighter feedback between live quality signals and routing decisions, so the cost-efficient frontier moves automatically as the open-source field advances.


For now, the router is live and the source is available.


## Getting started


The recommended path is the managed service. Sign up at[router.workweave.ai](http://router.workweave.ai/) , add your provider keys (OpenRouter alone is sufficient), and run:


```text


```


```text


```


```text


```


The installer prompts for scope, retrieves a key from the dashboard, and configures the client base URL.


For self-hosted deployments, the[README](https://github.com/workweave/router) walks through the rest.
