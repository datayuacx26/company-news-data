---
schema_version: "1.0.0"
document_id: "5392345dce7d85a607c0e875b023e4baaa4c73c3097bc860f187c5cf66a425b0"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/agent-development-kit"
published_at: "2025-12-17T17:32:25.927+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:92fd86be647a4c61e88a1239ad05928d616e9053738cc5df13b47633eab90e81"
---

# Introducing DigitalOcean Gradient™ AI Agent Development Kit: Deploy agent code as a real application

AI agents are everywhere right now. You’ve seen the demos: agents that plan trips, write code, or manage inboxes. But once you try to run one outside a demo, reality sets in.


Taking an agent from “it works locally” to “this runs reliably in production” usually means stitching together infrastructure, deployment pipelines, logging, endpoints, and monitoring, often without changing the agent logic itself. The gap between prototype and production is where most agent projects stall.


The[DigitalOcean Gradient™ AI Agent Development Kit (ADK)](https://docs.digitalocean.com/products/gradient-ai-platform/how-to/build-agents-using-adk/) , now available in[public preview](https://cloud.digitalocean.com/account/feature-preview?i=e0fda3&feature=agentKitPublicPreview) , is designed to close that gap. The ADK is a Python SDK and CLI that lets you deploy agent code as a hosted, production-ready service. Build your agent however you want. The ADK is how you deploy it cleanly onto real infrastructure, get started quickly using our[GitHub repo](https://github.com/digitalocean/gradient-adk) .


## Why Choose ADK?


There are plenty of tools out there, but the ADK offers a distinct edge for developers.


-


**Easiest path to production:** Take the agent code you already have and deploy it as a hosted service without re-architecting your workflow. You don’t have to rebuild your agent just to make it production-ready.


-


**Framework-agnostic execution:** You can run any Python-based agent implementation that conforms to its entry point and runtime requirements. Whether you use LangGraph, LangChain, PydanticAI, agents, or custom orchestration, you keep your framework and abstractions.


-


**Framework-aware observability:** The ADK provides built-in logs and execution traces so agents can be operated and debugged like real services. Deeper, node-level tracing is available for supported frameworks, while other agents still benefit from standardized input, outputs, and runtime visibility.


-


**DigitalOcean advantage:** Most platforms focus narrowly on agent execution and leave data and application infrastructure as external dependencies. With the ADK, agents run alongside inference workloads, knowledge bases, and supporting application services on DigitalOcean, under a single platform, network, and security boundary.


-


**Operational simplicity:** We removed the need to manually assemble infrastructure, deployment pipelines, logging, and endpoints just to run an agent in production. You focus on agent behavior, the DigitalOcean platform handles execution.


## Key Components of the Public Preview


The Gradient AI ADK provides everything you need to move from local development to a hosted environment, without changing how you write agent code.


-


**Python package (` gradient-adk` :** Easily installable via` pip` to integrate with your existing code. Since the ADK runs standard Python, your agent can use any Python library or external service, databases, APIs, SDKs, or internal services, without special adapters or plugins.


-


**Unified CLI:** A single tool to initialize projects, develop locally, and deploy to managed infrastructure with one command:` gradient agent deploy` .


-


**Hosted runtime:** Exposes your deployed agents via a standardized HTTP` /run` endpoint. Support for multiple deployments of the same agent (for example, dev, staging, and prod). Agent hosting is free during the public preview.


-


**Traces & insights for any workflow:** Add full tracing (including LLM calls, tool calls, knowledge base calls, and model specific metadata) to your agent logic using custom decorators across popular Python-based agent frameworks like **LangGraph, LangChain, CrewAI, PydanticAI, or even fully custom workflows.** If you’re using LangGraph or PydanticAI, traces are captured automatically with zero additional configuration.


-


**Knowledge Base (KB) support:** Seamlessly query DigitalOcean Knowledge Bases directly in your agents for reliable context.


-


**Evaluations for multi-step agents:** Ensure your multi-step agents behave reliably in production: build test cases from datasets and apply metrics to validate correctness, security, and tone for every step of an ADK Agent, not just the final output.


- **A2A (Agent-to-Agent) communication *(Coming Soon)*** : Expose agents as A2A-compliant endpoints, auto-generate AgentCards, and trace multi-agent interactions. Internal multi-agent calls are supported in this preview, while external calls will arrive in.


## Get Started Quickly


We’ve made it incredibly easy to go from a prototype to a real service in minutes. Agent hosting is free during the public preview, so you can deploy, test, and iterate without worrying about hosting costs while you evaluate the ADK.


1. **Enable the Preview:** To get started with the ADK, enable the public preview on your[Feature Preview page](https://cloud.digitalocean.com/account/feature-preview?i=e0fda3&feature=agentKitPublicPreview) in the DigitalOcean Cloud Console. Once you’ve opted in, access will be granted within a few minutes.
2. **Install ADK** ([GitHub repo for instructions](https://github.com/digitalocean/gradient-adk) )


Bash


```text
pip install gradient-adk


```


1. **Use a template:** Check out the[ADK repository on GitHub](https://github.com/digitalocean/gradient-adk-templates) for starting points using LangGraph, CrewAI, and more, including examples like a multi-agent RAG workflow, a news agent, or a web researcher agent etc.
2. **Deploy:** Once your agent logic is ready, ship it to production with a single command.


The gap between “cool AI tech” and “valuable AI products” is shrinking. Stop living in prototypes and start building production-grade AI workforces with Gradient AI ADK.


[Try the Agent Development Kit Today ->](https://cloud.digitalocean.com/account/feature-preview?i=e0fda3&feature=agentKitPublicPreview)


*Note: We expect but do not guarantee that public previews perform for production-level workloads. We recommend using simulated test data and not running sensitive workloads on public preview products.*
