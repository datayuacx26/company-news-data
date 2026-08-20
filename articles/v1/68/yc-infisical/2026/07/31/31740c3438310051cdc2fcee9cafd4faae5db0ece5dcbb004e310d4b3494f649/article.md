---
schema_version: "1.0.0"
document_id: "31740c3438310051cdc2fcee9cafd4faae5db0ece5dcbb004e310d4b3494f649"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/agent-proxy"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T19:10:17.136731+00:00"
fetched_at: "2026-07-31T19:10:18.826077+00:00"
content_hash: "sha256:8d7385377facc8fb151880a4a2c32b211acf32bd6844c8fb0efcbab033b71de0"
---

# Agent Proxy: Secure Credential Injection for Agents

In April, we launched[Agent Vault](https://github.com/Infisical/agent-vault) , an open-source HTTP credential proxy and vault, which was one of the first purpose-built, standalone MITM transparent proxy implementations for brokering credentials to agents. Today, we’re announcing its commercial grade successor:[Infisical Agent Proxy](https://infisical.com/docs/documentation/platform/agent-proxy/overview) .


At Infisical, we process billions of secrets per month including application configuration, database credentials, API keys, and more for all kinds of workloads. This includes AI agents, which consume secrets, but in a different format. In the past year, we’ve observed[credential brokering](https://x.com/dangtony98/status/2057940043826954463?s=20) become ubiquitous for combatting credential exfiltration against agents. Credential brokering lets agents use credentials to access services without reading any underlying values. This works by passing requests through a dedicated proxy which attaches credentials onto requests at the network boundary before forwarding them outbound to a destination.


The concept of an agent proxy has become so common that I’d argue it represents a new kind of infrastructure primitive, alongside sandboxes, designed specifically to address the security guarantees needed for secure agent deployments; I believe it is one of many components that will constitute the AI cloud in the nascent future.


Today, we dive into secrets brokering and how Agent Proxy delivers the commercial grade proxy needed for any secure agent infrastructure deployment. Hopefully, this article makes for an interesting read and is useful for anyone who needs to securely provision agents access to services; you might even consider taking[Infisical Agent Proxy](https://infisical.com/docs/documentation/platform/agent-proxy/overview) for a spin.


## How We Got Here


If you’re not familiar with the domain space of credential exfiltration and agent proxies, our original[Agent Vault launch post](https://x.com/dangtony98/status/2046982854710857762?s=20) provides a fuller read. We dive into why traditional secrets management designed for deterministic callers breaks down for non-deterministic actors (i.e. agents).


In short, my claim was that most of today’s computing paradigms weren’t designed for agents but for traditional workloads with fixed execution paths. After all, for most of computing history, autonomous software agents were more science fiction than engineering reality. Now that they’re real, we're discovering that many of the assumptions our software infrastructure was built on no longer hold.


Agents, being non-deterministic actors, present a new kind of challenge, where the underlying process is probabilistic, which makes it vulnerable to leaking secrets through attack vectors like prompt injection. This is the machine equivalent of social engineering, manipulating an agent into disclosing information or taking actions it otherwise wouldn’t. But the challenge doesn't stop there. Prompt injection can originate from almost anywhere, from explicit user input to data the agent ingests as part of its normal workflow. Even without any attackers, most organizations don’t want agents sending credentials to LLM providers. This gets pretty gnarly fast.


Agents, at least in their current state, cannot be trusted with holding secrets directly. There has to be a dedicated forward HTTP proxy beside each agent (be it through a dedicated service, sidecar, or egress layer) to securely broker credentials for it to access the external world.


Many sophisticated agent deployments have now converged on this pattern. It’s the same principle described across Anthropic’s[Managed Agents architecture blog](https://www.anthropic.com/engineering/managed-agents) , Vercel’s[credential brokering](https://vercel.com/changelog/safely-inject-credentials-in-http-headers-with-vercel-sandbox) , Cloudflare’s[outbound workers](https://developers.cloudflare.com/cloudflare-for-platforms/workers-for-platforms/configuration/outbound-workers/) , and most recently in[Claude Tag’s architecture](https://claude.com/docs/claude-tag/concepts/agent-identity) . While each variation of secrets brokering has its own architectural nuances, the general principle is that the agent has no access to real credential values; if anything, it might have fake credentials the broker swaps for real ones at the edge.


This is precisely what led us to build and open-source[Agent Vault](https://github.com/Infisical/agent-vault) , one of the first dedicated secrets brokering implementations for agents.


## What We Learned


It turns out that when you launch something that addresses a real "hair on fire" problem like credential exfiltration with the right ergonomics, it gets adopted pretty fast. Despite minimal promotion, Agent Vault crossed 2,000 stars on GitHub in just a few months with usage spanning tens of thousands of installations, millions of agent runs, and dozens of agents including Claude Code, OpenClaw, and even non-agentic untrusted code.


Looking back, Agent Vault’s success came down to a handful of product engineering decisions that, together, made this particular implementation of secrets brokering practical:


- Architecture: Agent Vault took a non-invasive transparent MITM proxy approach to brokering credentials at the network boundary. Regardless of whether an agent used a CLI, SDK, MCP, or raw API calls, requests would ultimately bottom out at HTTP and be brokered transparently by the proxy.
- Security: Agent Vault recommends deploying the proxy on a separate host from the agent, establishing a stronger trust boundary between the component that brokers credentials and the untrusted code consuming them.
- Proximity: Agent Vault was designed as a forward proxy that lives as close to the agent as possible, within the same private network, minimizing latency without sacrificing the security boundary.


As adoption grew, however, one thing became increasingly clear: Agent Vault needed a tighter integration with a secrets store, and preferably a more advanced secrets management platform like Infisical to avoid fragmented secret stores and extend the capabilities of brokered secrets with rich features like dynamic secrets, secrets rotation, versioning, auditing, and much more. In retrospect, we had bundled two distinct concepts into one: the secrets store and the secrets broker.


While credential brokering solved credential exfiltration, it’s a last-mile delivery method for how secrets should be made available to agents, not a standalone platform. Secrets brokering should follow the same pattern as Kubernetes Operators like[External Secrets Operator (ESO)](https://external-secrets.io/latest/) , which fetch secrets from an external secrets store and make them available to workloads at runtime, except with secrets being made available to agents through proxied traffic credential injection. The broker shouldn't become the source of truth for secrets; it should simply retrieve them on demand and securely deliver them to agents.


That realization led to Agent Proxy.


## Introducing Infisical Agent Proxy


[Infisical Agent Proxy](https://infisical.com/docs/documentation/platform/agent-proxy/overview) is a secrets broker, that fetches secrets back from Infisical and makes them available to agents, inspired by everything we’ve learned from the success of Agent Vault. Eaxcept instead of storing any secrets, it offloads secrets management to Infisical and focuses exclusively on brokering secrets to your agents, making the broker itself lightweight, stateless, and highly scalable.


With Agent Proxy, we’re providing 30+ presets for common services your agents need to access including LLM providers (Anthropic, OpenAI, etc.) and services like GitHub, Slack, and Datadog. If we don't have a preset readily available, you can always request one and register a custom service with a host pattern and brokering rules.


We're continuing to rethink how secrets should be consumed by agents: We believe that vaults and/or secret stores are here to stay, but secrets delivery will change drastically to fit the ergonomics of how agents operate. Part of that change will be an HTTP proxy to secure agent implementations, and Infisical Agent Proxy is that solution, now tightly integrated with the secrets manager you’re already familiar with: Infisical.


As part of Infisical Secrets Management, Agent Proxy inherits all secrets, integrations, automated rotations, RBAC, audit logs, access policies, and workflows you may already have in Infisical.


## Give it a Spin


From a deployment perspective, Agent Proxy is intentionally lightweight; it ships as part of the[Infisical CLI](https://infisical.com/docs/cli/overview) and can also be deployed as a Docker container. It’s part of any Infisical Secrets Management and available on any plan.


In production, we recommend deploying Agent Proxy on a separate host machine within close proximity to your agent(s). Agent Proxy should run as close to your agents as possible, whether that's on the same private network, alongside sandbox workers, or as a dedicated sidecar responsible for brokering outbound traffic. Keeping the proxy physically close to the agent minimizes latency while still preserving the trust boundary between the untrusted execution environment and the component responsible for handling credentials.


Conceptually, there are only four steps:


1. Define a[proxied service](https://infisical.com/docs/documentation/platform/agent-proxy/proxied-services) inside Infisical that specifies which hostnames Agent Proxy should broker, how authentication works, and which secrets should be attached. You can choose from one of 30+ presets or configure your own.
2. Create separate machine identities for both the proxy and the agent, allowing each to authenticate independently and receive only the permissions they require.
3. Deploy Agent Proxy alongside your agent infrastructure.
4. Configure your agents to send outbound HTTPS traffic through the proxy.


You can find the complete setup guide for Agent Proxy in the documentation for it[here](https://infisical.com/docs/documentation/platform/agent-proxy/overview) .


If you’re already managing secrets with Infisical, you can try Agent Proxy today. If not, start a[free trial today](https://infisical.com/pricing) or[talk to one of our experts](https://infisical.com/talk-to-us) to see how Agent Proxy could secure your agents.
