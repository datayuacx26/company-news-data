---
schema_version: "1.0.0"
document_id: "2a03655e31146e90abe469aba884e048d104bc54c5c6e48078d4f6c6941925ac"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-3b2f958954ed"
canonical_url: "https://www.mezmo.com/blog/builder-in-the-loop-henry-andrews-on-building-aura-like-production-software"
published_at: null
first_seen_at: "2026-07-22T04:13:38.440386+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:6864119e78b80388cf0a8eb36afb6dcf09a344a9b6bd7469bd107140fc34a1a5"
---

# Builder in the loop: Henry Andrews on building AURA like production software

**An interview series with the people building Mezmo’s open-source agentic harness for production operations.**


Builder in the loop is a Mezmo interview series focused on the engineers, product leaders, and operators shaping[AURA](https://www.mezmo.com/aura) , our open-source, MCP-native agentic harness for production operations.


The goal is to get past the polished product layer and talk through the decisions that matter when AI starts interacting with real systems. What should agents be allowed to do? How should their actions be traced? Where should humans stay in the loop? And what does it take to make agentic workflows reliable enough for SRE and platform engineering teams to trust?


In this installment, we sat down with **Henry Andrews** , Principal Product Manager at Mezmo, to talk about MCP, Rust, open source, and why AURA is being built more like production infrastructure than an AI demo. Henry’s background across test engineering, engineering management, and product shows up clearly in how he talks about agents: less “look what it can do,” and more “what happens when this becomes part of the system teams depend on?”


## **The short version**


Henry’s view is simple: if agents are going to participate in production operations, the harness around them needs to behave like production software.


That means AURA needs to be:


- **Composable** , so teams can connect agents to existing tools through MCP
- **Inspectable** , so every action can be traced and reviewed
- **Governable** , so autonomy can be introduced gradually with clear control points
- **Reliable** , so the system around the agent is not the weakest part of the workflow


AURA is not being built as a shortcut around SRE judgment. It is being built to help agents gather context, follow defined workflows, and produce evidence humans can verify.


## **Icebreaker: If AURA had a theme song**


We started with a low-stakes question: if AURA had a theme song, what would it be?


Henry’s answer: **“Mr. Roboto” by Styx.**


The line that stood out was, “thank you very much, Mr. Roboto, for doing the jobs that nobody wants to do.”


For anyone who has been pulled into an incident without enough context, the reference lands. A lot of incident work is repetitive before it becomes strategic: gather logs, check recent changes, inspect metrics, look for known failure patterns, compare against runbooks, and build a timeline.


That is the kind of work AURA is designed to support. Not by replacing the engineer, but by helping agents take a structured first pass and leave behind a trail the team can inspect.


## **From tool connectivity to an agent harness**


Henry’s first major contribution to AURA was practical: helping build the initial MCP server in early 2025.


MCP made it easier to connect agents to tools, but tool access alone was not enough. Teams still needed a way to define workflows, control behavior, inspect execution, and test how agents would operate against production-like systems.


That gap became Henry’s entry point into AURA.


MCP created the connectivity layer. AURA became the harness for making that connectivity usable in operational workflows.


## **The hardest decision: Building the harness in rust**


When we asked Henry about the hardest technical decision, the answer was not a model choice or a prompting strategy.


It was the decision to build a custom agent harness in **Rust** instead of taking the more common Python and LangChain path.


For Henry, the reasoning came back to reliability.


If an agent is going to access production context, call tools, and participate in incident workflows, the harness around it cannot be fragile glue code. It needs to behave like a service: stable, observable, testable, and maintainable.


That decision says a lot about AURA’s design philosophy. The agent layer can be flexible, but the system coordinating agent behavior needs to be dependable.


## **Configuration SREs can actually work with**


One design choice Henry emphasized was AURA’s use of **TOML-based agent definitions** .


That may sound like an implementation detail, but it has real operational implications.


Agent behavior should be defined in a way teams can commit to Git, review, version, roll back, and standardize across environments. Henry compared the model to Kubernetes manifests: declarative configuration that fits how engineering teams already manage infrastructure.


That matters because agent workflows should not live only inside prompts or opaque interfaces. They should be reviewable, repeatable, and controlled like the rest of the production environment.


## **Why open source matters in production**


Henry’s view on open source is direct: production systems should not depend on black boxes.


For SRE teams, that is less about ideology and more about operational responsibility.


If an agent takes an action, calls a tool, or recommends a path during an incident, the team needs to know:


- what context it used
- what tools it called
- what decisions it made
- where a human approved or rejected the next step
- how to modify the workflow when the environment changes


That is also why AURA is designed to emit **OpenTelemetry traces** for agent activity. “The agent said so” is not an incident review. Teams need an audit trail they can inspect.


The Apache 2.0 license supports the same goal: make the harness practical to adopt, extend, and govern in real environments.


‍


## **What’s next**


Henry will be giving a virtual PlatformCon talk titled **“Stop Duct Taping AI,”** focused on the principles behind AURA and what it takes to treat agents like production software.


That is the core takeaway for SREs and platform engineers: agents will not become useful in production because they are impressive in a demo. They will become useful when their workflows are inspectable, their context is reliable, their actions are governed, and their autonomy is earned.


That is the work behind AURA, and it is why we started Builder in the loop: to show the real design decisions behind the system, directly from the people building it.


AURA is Apache 2.0. Come build with us:


[github.com/mezmo/aura](https://github.com/mezmo/aura)


‍


## **About the interviewee**


[Henry Andrews](https://www.linkedin.com/in/henryandrews/) is a Principal Product Manager at Mezmo. His background spans test engineering, engineering management, and product, which informs his approach to building agentic systems that can be inspected, governed, and trusted in production workflows.
