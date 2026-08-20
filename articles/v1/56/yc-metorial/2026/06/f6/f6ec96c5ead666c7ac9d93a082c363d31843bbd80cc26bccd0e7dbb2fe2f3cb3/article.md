---
schema_version: "1.0.0"
document_id: "f6ec96c5ead666c7ac9d93a082c363d31843bbd80cc26bccd0e7dbb2fe2f3cb3"
company_key: "yc-metorial"
company: "Metorial"
source_id: "yc-metorial-news-import-9c457a3d81d4"
canonical_url: "https://metorial.com/blog/security-foundations"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-24T04:14:45.918392+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:18f5eb213c731aea0b01377547183b6d48193c01bb09bc05024d8e54e7f77165"
---

# Security has to come first

AI agents are no longer just answering questions. They are being connected to company systems: customer data, internal tools, production credentials, support workflows, billing systems, and everything around them. That changes the security problem.


Software usually starts with solving the most basic problems, the things we can put on a landing page and demo on a sales call. Then you add permissions, logging, secrets management, and compliance features once customers start asking for them. That is understandable from a product perspective, but it leaves seams. You can usually feel where security was wrapped around a product rather than built into how it works.


For agent infrastructure, that is not good enough. If an agent can reach real systems, the security model has to be part of the system from the beginning. That is the way we have tried to build Metorial. Not because it sounds better in a security document, sadly they usually don't care about that, but because the alternative makes us uncomfortable. There are three parts that matter most and that's what we've built Metorial around: isolation, data control, and the protocol layer.


## Isolate every interaction


The code that connects an agent to your systems is often not code you wrote yourself. It might be a first-party integration, a[custom MCP server](https://metorial.com/features/custom-mcp) , or a[Docker image](https://metorial.com/features/docker-mcp) from a registry. That code needs to run somewhere. If it runs too freely, one bad integration can become much more than a bug.


On Metorial, integration code runs inside an[enclave](https://metorial.com/features/enclaves) . An enclave is a sandbox that keeps the integration contained and monitored. Sandboxing itself is not a new idea, it's well understood and proven.


The important part is the level where we apply it. We do not use one sandbox per customer, or one per integration. We create isolation per user, per connection, per tool call. That means each interaction has its own boundary. If something misbehaves, the blast radius is that one interaction. And the[firewall](https://metorial.com/features/firewalls) around it can block traffic, the call can be traced, and the problem does not quietly spread into the rest of the account.


This kind of isolation can be slow if it is done poorly. We spent a lot of engineering time making the enclaves lightweight enough that we would not have to choose between safety and speed on every call.


## You have the keys to your data


There is a real difference between being told your data is encrypted and being able to control the keys yourself.


For many teams, platform-managed encryption is fine. For teams with strict compliance requirements, it often is not enough. They need to know who controls the keys, when they are used, and how that use can be audited. Secrets in Metorial live in[Vault](https://metorial.com/features/vault) . They are encrypted at rest and in transit, and access is logged. Underneath that is[KMS](https://metorial.com/features/kms) . Customers can bring their own AWS KMS keys to encrypt the credentials Metorial stores for them. We can still handle the operational work of storing and using those credentials, but the keys that protect them can stay under the customer's control.


That matters because trust should not depend only on our word. If a customer needs to prove how encryption is being used, they should be able to do that. And when even that is not the right boundary, Metorial can run[on-prem](https://metorial.com/features/on-prem) , in the customer's own cloud or data center.


## Treat the protocol as part of security


Infrastructure is only part of this. The protocol matters too.


MCP makes it easier to connect agents to tools, but a convenient protocol does not automatically create a safe deployment. If MCP servers are deployed with broad privileges, plaintext credentials, or no clear user identity, the risk is not theoretical. It is exactly the kind of thing that shows up once teams move quickly and connect too much too fast. So we treat the protocol layer as a place where security should be enforced, not just a way to pass messages around.


Requests flow through[Protoguard](https://metorial.com/protoguard) before they reach tools. Protoguard checks requests, applies policy, blocks prompt-injection attempts, and keeps the call tied to the identity of the real user behind it.


Access is connected to identity through[SSO and SAML](https://metorial.com/access-control) . Actions are[traced](https://metorial.com/tracing) , searchable, and attributed to people rather than hidden behind a shared API key. The goal is not to give teams a long checklist to configure later. The goal is for the secure path to be the normal path.


## Why we care about this


Building this way takes longer. There are easier ways to ship agent infrastructure.


But the more we work with AI agents, the clearer this feels: once agents can act inside real systems, security is not a feature on the side. It is the thing everything else depends on. That is why Metorial we care deeply.


You can find the formal details on our[security page](https://metorial.com/security) , including SOC 2 Type 2 reports and the material security teams usually need during review.
