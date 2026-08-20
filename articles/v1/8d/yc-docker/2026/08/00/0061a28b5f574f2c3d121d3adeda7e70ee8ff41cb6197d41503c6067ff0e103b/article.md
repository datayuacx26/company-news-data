---
schema_version: "1.0.0"
document_id: "0061a28b5f574f2c3d121d3adeda7e70ee8ff41cb6197d41503c6067ff0e103b"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-rss-8dbda93e62b5"
canonical_url: "https://www.docker.com/blog/governance-is-a-developer-experience-problem/"
published_at: "2026-08-05T13:00:00+00:00"
first_seen_at: "2026-08-05T13:43:26.768760+00:00"
fetched_at: "2026-08-05T13:43:28.101171+00:00"
content_hash: "sha256:b8319ff45045ef7503c3973cbc30adeb79bc65e40e68e8c2692d8b1d24f36cb5"
---

# Governance Is a Developer Experience Problem

This is the third post of a 3-part series by Docker Captain Karan Verma. Catch up on[Part 1: Your Laptop Is the New Production Environment](https://www.docker.com/blog/your-laptop-is-the-new-production-environment/) and[Part 2: Runtime Enforcement, Not Runtime Advice](https://www.docker.com/blog/runtime-enforcement-not-runtime-advice/) .


The conversation around AI governance often starts with security. That’s understandable. When autonomous systems can execute commands, access tools, and interact with production-adjacent environments, organizations naturally focus on risk. But after spending time thinking about agent workflows, I’ve become convinced that governance is about more than security. It’s also a developer experience problem.


## The Trust Bottleneck


Most organizations don’t struggle to adopt new tools because the tools are incapable. They struggle because the organization doesn’t trust them yet. The history of software development is full of examples. Cloud adoption accelerated when organizations became comfortable with cloud governance. Containers accelerated when teams gained confidence in isolation and operational controls. CI/CD accelerated when organizations trusted automated deployment pipelines. The pattern repeats. Capability arrives first. Trust arrives later. Adoption follows trust. AI agents are no different.


*Caption: Capability alone does not drive adoption. Trust enables organizations to delegate work, expand usage, and realize productivity gains.*


## The Wrong Tradeoff


Governance is often framed as a choice between speed and control. Move fast and accept risk. Or add controls and slow everyone down. In practice, the most successful developer platforms rarely make this tradeoff. Instead, they create environments where developers can move quickly because boundaries already exist. A developer deploying through a mature platform doesn’t need to think about every networking rule, access policy, or infrastructure safeguard every time they ship code. The platform already provides those guarantees. The same principle applies to agent systems. The goal isn’t to force developers to manually approve every action. The goal is to create environments where useful actions can happen safely by default.


## A Tale of Two Teams


Imagine two engineering teams using the same coding agent. The first team allows agent usage only in limited experiments because nobody is completely certain what the agent can access, execute, or modify. Every new workflow requires additional review. Every new capability triggers a discussion about risk.


The second team operates within clearly defined boundaries around execution, tools, and credentials. Developers understand where agents run, what systems they can access, and how activity is observed.


The underlying model is identical. The difference is trust. Over time, that difference may matter more than the model itself. Organizations rarely scale technology they do not trust.


## Why Boundaries Create Freedom


This idea sounds counterintuitive at first. Boundaries feel restrictive. But in software systems, boundaries often enable autonomy rather than limiting it.


When organizations know:


- where agents run,
- what agents can access,
- which tools agents can use,
- how activity is observed,


They become more comfortable delegating work. Without those boundaries, every workflow becomes an exception process. Every deployment requires discussion. Every new capability triggers concern. Every new tool requires negotiation. Governance reduces uncertainty. Reducing uncertainty increases trust. And trust enables adoption.


## The Platform Shift


One thing that stands out in recent discussions around agent infrastructure is that governance is increasingly moving into the platform itself. Developers shouldn’t need to become security experts every time they use an agent. Just as developers rely on platforms to handle identity, networking, deployment, and observability concerns, governance increasingly becomes part of the environment where agents operate. When governance is embedded into the platform, developers spend less time worrying about boundaries and more time focusing on outcomes. That’s a developer experience improvement as much as a security improvement.


## Governance as an Enabler


The organizations that adopt agents most successfully may not be the organizations with the fewest controls. They may be the organizations with the clearest controls. Clear boundaries create confidence. Confidence enables delegation. Delegation unlocks productivity. Viewed through that lens, governance is not the thing slowing agent adoption. It is one of the things that makes large-scale adoption possible.


## Looking Ahead


The conversation around AI agents often focuses on what models can do. Increasingly, I think the more interesting question is what organizations are willing to trust them to do. That trust won’t come from capability alone. It will come from visibility, accountability, and well-defined boundaries because the future of agentic software is unlikely to be determined solely by the most capable agents. It will also be shaped by the environments that make those agents trustworthy enough to use at scale.


### Learn more


- Read about why[AI Agents need isolation](https://www.docker.com/blog/why-ai-agents-need-isolation/)
- Read[Part](https://www.docker.com/blog/your-laptop-is-the-new-production-environment/) and[Part 2](https://www.docker.com/blog/runtime-enforcement-not-runtime-advice/) of this AI Governance series
- Find out how[Docker’s AI Governance solutions](https://www.docker.com/products/ai-governance/) work across all tools
