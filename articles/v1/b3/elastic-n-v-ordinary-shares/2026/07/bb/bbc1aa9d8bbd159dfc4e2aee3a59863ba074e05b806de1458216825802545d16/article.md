---
schema_version: "1.0.0"
document_id: "bbc1aa9d8bbd159dfc4e2aee3a59863ba074e05b806de1458216825802545d16"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/elastic-openai-partnership"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-30T14:40:06.528090+00:00"
fetched_at: "2026-07-30T14:40:08.854828+00:00"
content_hash: "sha256:b7a956058e77012e4b7bff61d9d08983c3b6d3c99647902b7da8078a7dc87f1c"
---

# Elastic and OpenAI collaborate to bring frontier intelligence to unstructured enterprise data

# Elastic and OpenAI collaborate to bring frontier intelligence to unstructured enterprise data


By


[Gregory Tademoto](https://www.elastic.co/blog/author/gregory-tademoto)[Hemant Malik](https://www.elastic.co/blog/author/hemant-malik)


July 30, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


AI agents are only as useful as the enterprise context they reason over. Much of what organizations know and need for AI agents to deliver is spread across unstructured data, such as documents, tickets, logs, metrics, traces, and security alerts that change continuously and are governed by different permissions.


Today, we announced an expanded collaboration to close that gap, helping organizations build production-ready AI applications and agents using OpenAI models with Elasticsearch. OpenAI brings advanced reasoning; Elastic is purpose-built for unstructured data and brings the retrieval, governance, and operational data layer that helps models find relevant, real-time context and use it in production.


The collaboration will focus on three customer outcomes:


- **Context-aware AI agents**


that retrieve accurate, permission-aware enterprise knowledge at scale with fewer tokens


-


**Agentic observability**


that correlates telemetry and accelerates root cause investigation for SRE teams


- **Agentic security operations**


that accelerate detections, help triage alerts, enable faster response, and proactively recommend detection rules to close security gaps


Elastic has supported OpenAI models through its AI assistants and connectors since 2023. This announcement expands that foundation across product collaboration and joint customer value.


## Elasticsearch: The context layer for agentic AI


In the agentic AI era, an enterprise’s value lies in its context — its data, history, decisions, and institutional knowledge spread across unstructured data. OpenAI’s


[in-house data agent](https://openai.com/index/inside-our-in-house-data-agent/) shows that reliable outcomes depend on bringing together multiple layers of context. Elasticsearch as a


[context layer](https://www.elastic.co/search-labs/blog/agent-memory-elasticsearch) gives agents persistent, governed memory to retrieve what matters as well as the ability to retain useful knowledge across interactions and ground every response and action in the enterprise.


The practical challenge in context is a retrieval problem: selecting the right information at the right time with the right permissions. Elasticsearch combines lexical and vector search, semantic reranking, filters, and access controls in one platform. In recent


[Elastic internal retrieval tests](https://www.elastic.co/search-labs/blog/agent-memory-elasticsearch) ,


Elastic was used for organizational memory and delivered 0.89 recall and complete protection of data across users.


[Economics](https://www.elastic.co/search-labs/blog/elastic-agent-builder-ai-agents-context-management) matters as much as accuracy. In a constrained experiment using the BrowseComp-Plus dataset, our


[precomputed Knowledge Indicators](https://www.elastic.co/search-labs/blog/pre-computed-context-llm-agent-costs) reduced input-token use by up to 75% compared with the experiment's standard retrieval augmented generation (RAG) baseline while answer accuracy increased from 60% to 92%. Results will vary by workload, but the direction is clear: Better retrieval can improve both reliability and cost.


Elastic Agent Builder can be used with OpenAI models and agents for improving context and delivering higher relevance and greater efficiency. Agent Builder tools can be exposed to OpenAI agents over MCP, Skills are available to Codex, and Agent Builder supports GPT models for reasoning and tools.


As models advance, context is what enables enterprises to derive real ROI and maintain unique knowledge and advantage in the world of agentic AI. Together, OpenAI and Elasticsearch give developers a practical foundation for agents that can create, manage, and retrieve context over governed enterprise data.


## Elastic Observability: Agentic investigations powered by unstructured telemetry


Operational context lives in unstructured telemetry data. Agentic applications introduce additional data like prompts, tool calls, model responses, token usage, traces, and retrieval steps. Elastic Observability brings those AI application signals together with logs, metrics, traces, and digital experience data in a single observability platform. With Elastic Workflows, Agent Builder, and Skills, teams automate investigations and remediation using telemetry data, past incidents, design docs, runbooks, and guidelines — applying repeatable practices.


Elastic’s agentic investigation experiences begin when an alert fires, correlating evidence across signals, identifying a likely root cause, and surfacing recommended next steps as the SRE investigation begins. The outcome is less time gathering evidence, less tool switching, and faster resolution.


The same platform can help teams operating OpenAI-powered applications. Elastic’s


[OpenAI integration](https://www.elastic.co/docs/reference/integrations/openai) collects API usage metrics and organization audit logs. Teams can correlate token consumption, model activity, API calls, and administrative changes with application latency, errors, deployments, and infrastructure health, using one data platform and one query language from the application layer through the model layer.


## Elastic Security: Agentic security operations platform


Security teams face a similar context problem with too many alerts spread across a steadily increasing number of endpoints, networks, cloud providers, identities, and application data. Elastic Security is an agentic security operations platform that unifies SIEM, XDR, and native automation around an agentic operating model. The platform investigates, correlates evidence, and prepares a response plan, while analysts retain judgment, verification, and approval. These capabilities are available in Elastic Security today, and this new operational model is already helping our customers streamline investigations and improve outcomes.


Attack Discovery works with OpenAI's latest frontier models to correlate disparate alerts into coherent attack chains, mapping activity to MITRE ATT&CK, identifying affected users and hosts, and attaching findings to cases and response workflows, so analysts triage a handful of evidence-backed attacks instead of thousands of raw alerts.


Agent Builder ships with a native agent that comes ready-made with skills, such as threat hunting and alert analysis, controlled tool access, and full reasoning traces for auditability. Teams have the option of building custom agents and workflows on top of Agent Builder. Meanwhile, Automatic Import builds custom data integrations in minutes, Automatic Migration translates Splunk and QRadar detection rules into Elastic Security, and the Elastic Security MCP App brings alert triage, threat hunting, and case management into the AI tools analysts already use.


With these capabilities, our customers are already seeing measurable value.


[Visa](https://www.elastic.co/customers/visa) , as part of its migration from a legacy SIEM to Elastic Security, built its first agentic SOC workflow with a constrained, human-on-the-loop AI validation step. This cut triage on a high-stakes mainframe detection from 10–20 minutes to seconds while keeping every AI decision auditable.


[Airtel](https://www.elastic.co/blog/ai-driven-analytics) 's managed security team reports up to 40% faster triage and analysis and 30% faster investigations using Attack Discovery, Agent Builder, and Automatic Import.


Elastic also helps security and compliance teams monitor the OpenAI platform itself. The existing integration ingests organization audit events, such as login attempts, API key lifecycle changes, role assignments, and configuration updates. These events can be analyzed alongside endpoint and network activity to identify suspicious behavior across the broader AI environment. Elastic’s upcoming support for OpenAI's Compliance Logs Platform will give enterprise teams a more complete, auditable view of activity across OpenAI workspaces and services.


## What comes next


For developers, Elastic


[agent-skills](https://github.com/elastic/agent-skills) give OpenAI Codex repeatable, Elastic-specific guidance, while Elasticsearch and connected tools provide governed, real-time access to the unstructured enterprise data agents need to reason accurately and act with precision. We will continue working on deeper integration points to help serve as the context layer.


For security defenders, as an


[OpenAI Daybreak Cyber Partner Program](https://openai.com/index/daybreak-securing-the-world/) member, we will work to bring GPT-5.5 Cyber specific models reasoning capabilities into Elastic Security’s agentic workflows, helping teams correlate alerts, recommend response actions, and generate detection rules to close confirmed gaps. Combined with Elastic Security Labs threat research, this can turn adversary behavior into faster, more precise protection with responsible deployment and abuse-prevention controls.


For SRE teams, we plan to deepen agentic investigation capabilities with frontier AI. As Elastic


[Knowledge Indicators](https://www.elastic.co/observability-labs/blog/elastic-knowledge-indicators-log-extraction) become generally available, they can help precompute operational context from raw telemetry and unstructured operational knowledge, giving agents more precise context for dashboards, topology maps, rules, investigations, and remediation workflows.


Whether you are looking for answers stored in documents, operational telemetry, or security events, OpenAI and Elastic are partnering to bring frontier intelligence to your unstructured data.


## Ready to get started?


Connect OpenAI models and Codex to Elastic to build grounded agents with


[Agent Builder](https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/get-started) and


[Agent Skills](https://www.elastic.co/docs/explore-analyze/ai-features/agent-skills) , observe OpenAI usage with


[Elastic LLM observability](https://www.elastic.co/docs/reference/integrations/openai) , and investigate production issues and threats with Elastic AI Assistant


[with OpenAI models](https://www.elastic.co/docs/explore-analyze/ai-features/llm-guides/connect-to-openai) . Start building today.


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, and associated marks are trademarks, logos or registered trademarks of elasticsearch B.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
