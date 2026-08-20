---
schema_version: "1.0.0"
document_id: "a97e611d7c3bcbfb51f313d0f86c4ce1e9aa4aab296806ed20e8c6f666ea0c1c"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-32a3c94cba04"
canonical_url: "https://building.nubank.com/building-ai-agents-in-practice-with-clojure/"
published_at: "2026-02-02T10:00:00+00:00"
first_seen_at: "2026-07-20T04:35:52.604812+00:00"
fetched_at: "2026-07-28T22:22:42.278869+00:00"
content_hash: "sha256:35e6dacc78c2383a17280e017ad206be5b252c45e0fe9f3b50ae963e0fe44862"
---

# Building AI agents in practice with Clojure

During Clojure South, Marlon Silva, Senior Software Engineer at Nubank, shared his perspective on a recurring challenge for software engineers working with AI today: how to move beyond using AI assistants and start engineering reliable, task-oriented AI agents.


According to Marlon, the industry has made it increasingly easy to consume AI — APIs, copilots, and assistants are everywhere — but building AI-powered systems still requires engineers to make a series of low-level, often uncomfortable decisions. In his talk, he focused on demystifying those decisions, framing AI not as a black box, but as infrastructure, and integrations that need to be reasoned about explicitly.


Rather than presenting a new framework or abstraction layer, Marlon walked through the architectural choices he believes matter most when building agents in practice, and explained why Clojure offers a particularly strong foundation for exploring this space.


## Infrastructure first: where your models live matters


Marlon started by arguing that any serious AI initiative begins with infrastructure — specifically, how teams access models. While this decision is often treated as an implementation detail, he emphasized that it directly impacts scalability, experimentation, security, and integration with existing systems.


From his perspective, engineers typically face two options:


- **Direct AI vendors:** Marlon noted that these providers are an excellent entry point for individual developers. Signing up is straightforward, APIs are well-documented, and it is possible to start experimenting almost immediately. For learning and early exploration, this path minimizes friction.
- **Cloud providers** : For organizations, however, Marlon argued that leveraging existing cloud relationships is usually the better long-term decision. Most companies already have accounts, billing, security controls, and observability in place. Cloud Providers like AWS and GCP make it possible to access models from multiple AI Labs without introducing new suppliers into the stack.


According to Marlon, when teams are operating inside an organization, the most pragmatic default is to use the models already available through their cloud providers. This removes procurement overhead and allows engineers to focus on building systems instead of managing vendors.


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## The fragmentation problem: too many APIs


Once access to models is established, Marlon pointed out a second, inevitable problem: API fragmentation. Each provider exposes different request formats, parameters, and SDKs, which quickly complicates development and makes experimentation costly.


In his talk, Marlon described this fragmentation as one of the first scaling pain points teams encounter when AI moves beyond a single script or proof of concept.


To address this, he introduced LiteLLM as a practical unification layer. LiteLLM is a proxy that standardizes access to multiple models and providers behind a single API, regardless of where the model is hosted.


Marlon highlighted three concrete benefits of this approach:


- **A unified interface** , allowing teams to switch models without rewriting integration code.
- **Centralized observability** , creating a single point for logging, debugging, and auditing model interactions.
- **Cost control** , which he emphasized as critical. Token usage scales quickly in production, and LiteLLM enables organizations to track and limit usage per team, service, or key.


From Marlon’s perspective, this kind of proxy is not an optimization: it becomes foundational infrastructure as soon as AI is part of a real system.


## Why smaller models often work better for agents


Marlon then challenged a common assumption in the AI space: that larger models are always better.


For task-oriented AI agents, he argued, this is rarely true. Citing recent research from Nvidia, Marlon explained that Small Language Models (SLMs) are often a better fit for agents designed to execute specific, well-defined actions.


According to him, the broad generalization capabilities of large LLMs, models capable of writing essays or long-form prose, are unnecessary for most agent workloads. Using them in these contexts leads to wasted capacity, higher costs, and increased complexity.


He outlined several practical advantages of SLMs:


- **Cost efficiency** : models like Llama 4 Scout on AWS Bedrock cost orders of magnitude less per token than large proprietary models.
- **Lower energy consumption** : making them a more responsible choice at scale and more eco-friendly.
- **Feasible fine-tuning:** adapting a 7–10B parameter model to a specific domain is realistic, whereas doing the same with very large models often is not real for most companies.


For Marlon, choosing SLMs is not a compromise, as it is an engineering decision aligned with the actual requirements of agent-based systems.


## Why Clojure fits this problem space


From there, Marlon shifted focus to tooling. He explained that AI development is inherently experimental: prompts change, parameters are adjusted, models are swapped, and assumptions are constantly tested. In that context, developer feedback loops matter. This is where Clojure stands out.


Marlon described Clojure as an ergonomic language — not because of syntax alone, but because of its **REPL-driven development model** . The ability to evaluate functions incrementally, inspect results immediately, and iterate without restarting an application fundamentally changes how engineers explore problem spaces.


In his experience, this interactive workflow aligns closely with how AI systems are built and refined.Beyond the REPL, Marlon highlighted two interoperability advantages:


- **Java interoperability** : Because Clojure runs on the JVM, it has seamless access to the Java ecosystem. Cloud SDKs, HTTP clients, observability tools, and mature libraries are immediately available.
- **Python interoperability** : With libraries such as libpython-clj, Clojure can import and execute Python code directly. While not as seamless as Java interop, this capability allows engineers to reuse Python-based AI tooling without abandoning Clojure’s interactive workflow.


For Marlon, this combination makes Clojure a strong orchestration layer for AI systems that need to integrate with multiple ecosystems.


## From theory to practice: the live demonstration


To make these ideas concrete, Marlon walked through a live demonstration.


He started with simple Python scripts that sent text, images, and PDFs to Bedrock-hosted models via a local LiteLLM proxy. These examples established a baseline using familiar tooling.


Next, he reproduced the same workflows in Clojure by importing the Python LiteLLM package directly into the Clojure runtime. Python functions were called from Clojure code, with inputs and outputs handled interactively.


According to Marlon, the most important part of the demo was not that this approach works, but how it changes the development experience. Python-based workflows often require frequent context switching — editing files, running scripts, and restarting processes. With Clojure and the REPL, the entire feedback loop stays inside the editor.


For exploratory domains like AI, Marlon argued, this difference directly translates into faster iteration and deeper focus.


## Conclusion


Marlon closed by emphasizing that AI agents remain a young and rapidly evolving field. Libraries, architectures, and best practices are still in flux, which makes flexibility a key requirement.


He also offered a note of caution: granting models excessive autonomy without clear boundaries and controls can lead to fragile systems. In his view, frameworks that favour obscure control flow and mostly relies on LLM itself encourages a “ship and pray” approach to AI development. At the same time, Marlon pointed out that new agent architectures are actively emerging from research groups at organizations like Nvidia and DeepMind, signaling that significant changes are still ahead.


His conclusion was pragmatic: combining well-chosen infrastructure, models sized to the problem, and tools that favor exploration creates a solid foundation for building AI systems grounded in engineering discipline. The repository shared during the talk serves as a starting point for engineers interested in continuing that exploration.


## References


[Small Language Models are the Future of Agentic AI](https://arxiv.org/pdf/2506.02153)


[AlphaEvolve: A coding agent for scientific and algorithmic discovery](https://arxiv.org/pdf/2506.13131)


[GitHub –](https://github.com/marlonjsilva)[marlonjsilva/clj-agents](https://github.com/marlonjsilva/clj-agents)


[GitHub – clj-python/libpython-clj](https://github.com/clj-python/libpython-clj)
