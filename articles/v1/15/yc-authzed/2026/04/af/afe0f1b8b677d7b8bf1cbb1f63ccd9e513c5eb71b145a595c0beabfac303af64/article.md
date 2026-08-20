---
schema_version: "1.0.0"
document_id: "afe0f1b8b677d7b8bf1cbb1f63ccd9e513c5eb71b145a595c0beabfac303af64"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/decomposing-iam"
published_at: "2026-04-04T07:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:d1b2554b238163ad0fdc2a0823a5dd88a18f01f36ee8a070d946f7310dabdf97"
---

# It's Time to Decompose IAM

[As predicted](https://authzed.com/blog/agentic-security-and-governance-rsac-and-kubecon) , securing Agentic AI was the talk of the town at last week’s RSAC conference. Everyone understood the *why* very clearly. It's literally a world-changing opportunity, with world-ending risks, but no one seemed to know quite what the *how* looked like.


Part of the problem is that we're looking at the wrong puzzle pieces. There was plenty of talk about Identity and Access Management (IAM) as the core of agentic security, but IAM has never been a single thing—and treating it like one is holding us back.


So what is IAM? It's a convenient shorthand for “all the infrastructure to put the right resources in the right hands at the right time,” but authentication (the “I”) and authorization (the “AM”) are[fundamentally different disciplines](https://authzed.com/blog/authentication-vs-authorization) with fundamentally different challenges. Conflating them may simplify slide decks, but it's holding us back, and Agentic AI might be the thing that finally decouples the two so we can move on.


## Authentication is complicated


Identity—authentication—is very far from solved, and as long as bad actors want in, it never will be. Every year brings new challenges and standards, but the entire authorization lifecycle ends at the door when you verify that the requestor is who they claim to be.


Because authorization patterns are so diverse, organizations will almost certainly need to engage with several solutions. New patterns, partners, risks, and regulations might all mandate additional systems. The question isn't whether you can get by with one authentication system, but how you can manage multiple systems without disrupting your business or the applications that run it.


## Authorization has its own problems


Authorization, on the other hand, doesn't care how you authenticated. It cares what you can do now that we know who you are, and that's a completely different set of challenges.


Latency at scale is one. A second-long login might be acceptable, but if you're performing dozens of authorization checks for every request that follows, milliseconds matter.


There's a human impact, too. Authorization is an “everybody” issue. HR, FinOps, Sharepoint admins—everyone who manages knowledge resources, employees, or systems has an interest in authorization. Stakeholders need to be able to work in their preferred systems and trust that updates take effect immediately. Since most authorization logic resides in application code, that group also includes stressed out developers struggling to keep internal customers happy while still trying to build new applications and value.


## Agents are not an "NHI problem"


The industry keeps framing agentic computing as an identity problem. It's not. Scaling authentication is difficult, but straightforward—and not new. The real risk with agents isn't that they won't be able to log into our systems. It's that we won't be able to govern them once they do.


Agents also challenge the assumptions on which we've built existing authorization logic.[Role-Based Access Control](https://authzed.com/blog/permissions-management) (RBAC) systems may have worked fine for established business processes, but enabling a non-deterministic agent that makes real-time decisions to solve problems (without controls around its tools) requires expressive and scalable access controls. It's a complicated problem requiring a sophisticated solution, and authorization is its core.


On this page


- Authentication is complicated
- Authorization has its own problems
- Agents are not an "NHI problem"


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Agentic Security and Governance at RSAC and Kubecon Next week, RSAC and KubeCon happen at the same time. While the two shows have very different core audiences, the overlap in topic this year is striking—especially around authorization as a key driver of AI and agentic security. Mar 20, 2026 · 3 min](https://authzed.com/blog/agentic-security-and-governance-rsac-and-kubecon)[Company Agentic Security and Governance at RSAC and Kubecon Next week, RSAC and KubeCon happen at the same time. While the two shows have very different core audiences, the overlap in topic this year is striking—especially around authorization as a key driver of AI and agentic security. Cormac Foster · Mar 20, 2026 · 3 min](https://authzed.com/blog/agentic-security-and-governance-rsac-and-kubecon)


[AI Policy Engines Don't Work for AI Authorization. Here's Why Policy engines promise flexible authorization, but they struggle with the ambient context and relationship-driven decisions that AI systems require. Learn why Relationship-Based Access Control (ReBAC) is the better approach for securing AI agents and RAG pipelines. Feb 18, 2026 · 8 min](https://authzed.com/blog/policy-engines-dont-work-for-ai-authorization-heres-why)[AI Policy Engines Don't Work for AI Authorization. Here's Why Policy engines promise flexible authorization, but they struggle with the ambient context and relationship-driven decisions that AI systems require. Learn why Relationship-Based Access Control (ReBAC) is the better approach for securing AI agents and RAG pipelines. Sohan Maheshwar · Feb 18, 2026 · 8 min](https://authzed.com/blog/policy-engines-dont-work-for-ai-authorization-heres-why)
