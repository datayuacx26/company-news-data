---
schema_version: "1.0.0"
document_id: "509c3cdf515802a3ee011ede4d28deb439d19bdc85fc2129144f3490f9c7f68b"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/everyone-wants-to-be-the-ai-gateway"
published_at: "2026-08-17T20:04:27+00:00"
first_seen_at: "2026-08-18T00:50:24.007165+00:00"
fetched_at: "2026-08-18T00:50:25.316073+00:00"
content_hash: "sha256:b21b582329381c2cecaf2b547604d25c7092346e0bcbe3526d63d8e3bbb7f7f7"
---

# Everyone wants to be the AI gateway

[Stripe is reportedly nearing a deal to buy OpenRouter for more than $7 billion](https://www.bloomberg.com/news/articles/2026-08-16/stripe-nears-deal-to-buy-ai-firm-openrouter-for-over-7-billion) . Three months ago, OpenRouter had just[raised a $113 million Series B](https://openrouter.ai/blog/series-b/) .


Meanwhile, Palo Alto Networks paid[$140 million](https://www.sec.gov/Archives/edgar/data/1327567/000132756726000015/panw-20260430.htm) for[Portkey and made it the AI gateway inside Prisma AIRS](https://investors.paloaltonetworks.com/news-releases/news-release-details/palo-alto-networks-completes-acquisition-portkey-secure-ai) . Stripe wants the layer that brokers model demand. Palo Alto Networks wants the layer that governs agent traffic. Both are buying the middle.


The deals expose the category’s identity problem: products called AI gateways often do different jobs.


A gateway is a place in the request path, not a product category. That explains why Ramp, Merge, Vercel, Cloudflare and AWS can all launch an “AI gateway” without building the same thing.


An AI gateway sits between a caller and whatever executes the request:


` Application or agent → Gateway → Model, tool, API or resource`


Ollama can expose an OpenAI-compatible API, but it usually executes the model itself. It is a runtime. A gateway sits between the caller and the system executing the request.


Own that middle box and you get to make a decision before the request runs. You also see what happened afterward: cost, latency, usage, errors and demand.


What changes from product to product is the decision made in that box.


Primary placement only. Capabilities overlap.


## “Gateway” names the place, not the job


Model choice used to be an implementation detail. It is now a moving target. Providers change prices, models leapfrog each other, capacity disappears and new serving options show up every week. A team using several models needs somewhere to encode its preferences and fallbacks.


The same request now carries more policy than a normal API call. It may contain sensitive data. It may need to stay in a region, fit within a budget or use an approved provider. A cached answer should not be treated like a fresh model call.


Agents make the boundary even more useful. A request can now lead to an API call, a database write or a purchase. Someone needs to decide whether the agent may act, whose credentials it may use and whether the resource has been paid for.


Those are different jobs that happen to fit in the same place.


The commercial incentives are different too. A router can charge on model usage or make a developer platform stickier. A cloud gateway can pull more traffic into the cloud. An agent gateway can drive adoption of the surrounding runtime and identity stack. A payment gateway can take a fee or expand a payment network.


Much of the proliferation comes from existing companies extending their strongest asset into a newly valuable control point.


---


## Four jobs are hiding under the same label


### 01. Model routing


**Which model should handle this request?**


[OpenRouter](https://openrouter.ai/docs/guides/routing/provider-selection) started with unified model access.[Vercel AI Gateway](https://vercel.com/docs/ai-gateway/models-and-providers/provider-options) brings routing and fallback into Vercel’s developer workflow.[Ramp Router](https://ramp.com/router/) applies Ramp’s cost-optimization instinct to model choice.[Merge Gateway](https://docs.merge.dev/merge-gateway/get-started) looks like a natural extension of Merge’s original product: one normalized API in front of a fragmented supplier base.[LiteLLM](https://docs.litellm.ai/) and[Bifrost](https://docs.getbifrost.ai/overview) attack the same problem from open-source proxies: one interface, many providers, with routing and spend controls attached.


They overlap, but their distribution is different. That matters as much as the routing algorithm.


### 02. Traffic control


**Can this request run, and under which rules?**


[Cloudflare AI Gateway](https://developers.cloudflare.com/ai-gateway/usage/rest-api/) logs, caches and controls model traffic.[Kong](https://developer.konghq.com/ai-gateway/) and[Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/genai-gateway-capabilities) are extending control planes that enterprises already use for APIs.[Envoy AI Gateway](https://aigateway.envoyproxy.io/docs/api/) ,[kgateway](https://github.com/kgateway-dev/kgateway) and[Traefik Hub AI Gateway](https://doc.traefik.io/traefik-hub/ai-gateway/overview) bring the cloud-native gateway stack into AI traffic.[Portkey](https://portkey.ai/docs/product/ai-gateway) , now part of Palo Alto Networks, and[TrueFoundry](https://www.truefoundry.com/docs/platform/deploy-control-plane-and-gateway-plane) come from the AI observability and governance side. Their feature lists overlap; their routes into the customer do not.


### 03. Agent access


**Which tools can this agent use, and on whose behalf?**


Agent access is about tools rather than tokens. The gateway decides whether an agent can call Salesforce, whose identity it carries and which credentials it receives.


[Amazon Bedrock AgentCore Gateway](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway.html) fronts tools, other agents and models.[Google Agent Gateway](https://cloud.google.com/blog/products/identity-security/introducing-agent-gateway-isv-ecosystem-for-security-and-governance) is building an ecosystem around identity, security and governance.[agentgateway](https://agentgateway.dev/docs/standalone/main/about/introduction/) provides an open-source data plane for LLM, MCP and agent-to-agent traffic.[Gravitee Agent Gateway](https://documentation.gravitee.io/apim/agent-mesh) extends an established API-management stack to A2A and MCP. Some also route models. Their harder job is controlling which actions an agent may take, and on whose behalf.


### 04. Commercial control


**What may this customer consume, and on which terms?**


[Cloudflare’s Monetization Gateway](https://blog.cloudflare.com/monetization-gateway/) is one early expression of it. The gateway checks payment before returning a page, dataset, API or MCP tool. Its initial flow uses HTTP 402 and x402: the resource returns payment instructions, the caller pays and retries with proof.


This makes sense for machine-to-machine purchases. It does not replace subscriptions, negotiated prices, credits, commitments, taxes or invoicing. “Has this request been paid for?” is narrower than “what did this customer buy?”


We wouldn't rank these four groups on a single up-and-to-the-right chart. A gateway that owns more decisions also owns more latency, failure modes and switching cost. More surface area can be an advantage, but it is not a neutral measure of product quality.


---


## The categories will blur


Features are already converging. Routers add policy and budgets. API gateways add model selection. Agent gateways front models as well as tools. Cloudflare now has both an AI Gateway and a Monetization Gateway.


Features will probably converge faster than the businesses behind them.


A product sold to a platform engineering team through an existing cloud contract behaves differently from one adopted by a developer with an API key. A router earning margin on model spend has a different incentive from an API platform using the gateway to retain traffic. The interface may look similar while the buyer, distribution and economics remain different.


If, a few years from now, the successful products have the same buyer, make money the same way and replace each other cleanly, then this map was describing a temporary phase. We expect similar feature lists wrapped around different control points and distribution advantages.


When we compare gateways, we ask five questions:


**1. What traffic passes through it?**


**2. Which decision does it make before the request runs?**


**3. What advantage did the company bring from its existing business?**


**4. How does the gateway make money, directly or indirectly?**


**5. Where does the customer contract live?**


---


## Commercial control is the missing gateway decision


At Lago, we are building toward this commercial gateway layer. Runtime infrastructure is starting to absorb commercial logic without the customer agreement needed to interpret it.


A gateway needs to decide quickly how a request should run. The commercial system has to remember what the customer was promised.


Consider a customer with an annual commitment, a monthly allowance, prepaid credits and a negotiated price for one model family. A token count does not tell you what to charge. Neither does a successful x402 payment. You need the contract, the running balances and the history of adjustments.


A useful architecture separates three responsibilities:


**Execution gateway.** Routes the request and applies technical policy.


**Commercial gateway.** Checks the customer’s entitlements, balance, credits, commitments and spending rules before or during consumption.


**Commercial system of record.** Settles what happened into balances, invoices and revenue records.


These layers should exchange decisions and usage. They do not need to collapse into one universal gateway. Lago’s opportunity is to make commercial policy available in the request path while preserving the contract and ledger behind it.


The[Lago Agent SDK](https://getlago.com/docs/guide/ai-agents/agent-sdk/overview) is one test of this boundary. It normalizes usage from OpenAI, Anthropic, AWS Bedrock, Google Gemini and Mistral while leaving the application’s model client in place.


With Cloudflare AI Gateway, Lago can read the logs and use the cost Cloudflare actually metered. Cache hits carry zero tokens and zero cost, so they settle at zero. Cloudflare’s metered cost does not include reasoning tokens, so thinking-heavy models should be billed from token usage instead.[The implementation is documented here](https://getlago.com/docs/changelog/product#cloudflare-ai-gateway-bill-what-your-gateway-actually-metered) .


Cloudflare stays responsible for execution truth. Lago applies the customer contract. We are extending the same adapter model to other execution layers, including Vercel AI Gateway, while building toward synchronous commercial decisions for use cases that need authorization before consumption.


When evaluating a gateway, ask which decision you are delegating and what becomes hard to move later.
