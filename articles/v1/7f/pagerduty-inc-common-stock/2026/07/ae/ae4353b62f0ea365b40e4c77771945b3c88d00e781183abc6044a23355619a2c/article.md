---
schema_version: "1.0.0"
document_id: "ae4353b62f0ea365b40e4c77771945b3c88d00e781183abc6044a23355619a2c"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-b3e2d0380f72"
canonical_url: "https://www.pagerduty.com/eng/pagerduty-for-ai-how-the-sre-agent-triages-ai-incidents/"
published_at: "2026-07-21T13:56:31+00:00"
first_seen_at: "2026-07-21T16:30:24.397731+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:206a19fc67f56373e30b46e01e86fe37fdcee858997e02eb45d99841ddd7a04b"
---

# PagerDuty for AI: How the SRE Agent Triages AI Incidents by PagerDuty Engineering

## Triaging AI Incidents


An AI agent that has quietly gone off the rails does not turn a dashboard red. Last year[we wrote about](https://www.pagerduty.com/eng/pagerduty-arize-building-end-to-end-observability-for-ai-agents-in-production/) defining AI incidents and how we instrument[our agents with Arize](https://fast.wistia.net/embed/iframe/31ew4aiox8) . In that post, we tackled detection: flagging failures that traditional monitoring never catches. In this post, we cover **what happens after the AI incident fires** .


Monitors based on LLM-as-a-judge evals reframe what an alert means. The signal is not “something is broken”; it is “something might be broken” according to the LLM. That changes what the responder is supposed to do at 2 a.m. and raises the question of whether these incidents should wake a human at all. For example, a drop in a relevance metric is usually not something a human can fix in the next fifteen minutes, unlike a bad deployment. The fix is a prompt change, a retrieval tweak, a model swap, or a re-tune of the eval itself. However, a low-relevance alert could also be due to an upstream service failure that causes errors in the AI feature’s responses, without implying a drop in its internal performance that the responder can adjust for. With PagerDuty as the source that combines signals across our observability stack, responders can remediate true AI output quality drift separately from traditional application failures.


Given the relative novelty of AI incidents, our main goal is to help responders and reduce on-call load by adding context and data about the issue, predicting whether it is a real problem, and suggesting potential resolution paths to help resolve incidents faster. We leverage our platform primitives together with the SRE Agent to be able to:


- classify whether the incident requires escalation to a human
- run pre-approved remediation processes
- file follow-up tickets for the team that owns the AI feature


The first step in that direction was to connect the SRE Agent to AI observability platforms. These platforms capture every agent interaction as a trace, which contains the full record of LLM calls, tool calls, retrievals, and outputs for that request. With this connection, the SRE Agent takes the first pass on the incident automatically: pulling monitor state, surfacing relevant traces, and handing the responder a diagnosis and a ranked set of next steps.


## The SRE Agent responder


PagerDuty responders currently use the SRE Agent to help them with incident triage and diagnostics. The SRE Agent’s capabilities have evolved to a point where teams now encode runbooks and processes into the agent via skills and connect it to their observability, code, and documentation data sources. Below we outline an example of how we work through an AI incident together with the SRE Agent.


Our Insights Agent is a PagerDuty chat assistant that answers operational questions like “how many SEV-1s did we have last week?” by calling analytics tools behind the scenes. It has multiple monitors running in Arize that trigger PagerDuty incidents when breached. In this example, we examine an incident in which the agent’s tool-selection performance drops below a designated threshold. The responder engages with the SRE Agent on the incident page, which then starts working through the standard steps that the human responder would normally take: check the current monitor status to see if it recovered; filter the Arize project for the offending traces; review the eval explanations and summarize patterns; and potentially check for recent code changes that might have caused degradation.


The SRE Agent knows about this process and its underlying details through a PagerDuty skill, built from existing runbooks using the[create-pagerduty-skill](https://github.com/PagerDuty/claude-code-plugins/blob/main/commands/create-pagerduty-skill.md) , while the connections with Arize, GitHub, and other data sources are established with the SRE Agent’s[connectors](https://support.pagerduty.com/main/docs/connectors-tools-and-skills) .


After sifting through traces and code, the SRE Agent comes back with a rundown of what happened. In this example, there was a genuine but minor degradation of tool selection performance, and the SRE Agent recommends that the incident can be closed with some follow-up actions to improve the agent’s tool calling behavior.


[https://cdn.pagerduty.com/wp-content/uploads/2026/07/sre-agent-triage-rundown.mp4](https://cdn.pagerduty.com/wp-content/uploads/2026/07/sre-agent-triage-rundown.mp4)


Since the SRE Agent has access to the underlying code via the GitHub[code connector](https://support.pagerduty.com/main/docs/connectors-tools-and-skills) , we can dive deeper into its suggestions and ask it to pinpoint exactly what changes should be made. The responder uses a customized code-suggestion skill to keep the agent’s suggestions within the boundaries defined by the team. The SRE Agent proposes a few adjustments to the agent’s system prompt to address the issues found in the failing traces.


[https://cdn.pagerduty.com/wp-content/uploads/2026/07/sre-agent-code-suggestions.mp4](https://cdn.pagerduty.com/wp-content/uploads/2026/07/sre-agent-code-suggestions.mp4)


These suggestions provide an additional data point of potential adjustments to the agent code that can be replayed against failing traces and regression tested following our agent[self-improvement strategy](https://www.youtube.com/watch?v=12e0pSOBBQk&t=1s) .


## What this unlocks next


The SRE Agent takes the first pass on triage today. Where this is heading is a loop that gets progressively smarter, so that fewer incidents need human attention and the ones that do arrive with more of the work already done.


The next step is to make the signal itself smarter. LLM-as-a-judge evals can be noisy on a small sample, and so we want PagerDuty to automatically trigger a re-check of the score. The idea is that real failures still page while noise gets suppressed on the way in.


Furthermore, we want to classify incidents by failure type, such as hallucinations, tool-call errors, and model latency. Each one can be routed to a different fix, and classification lets triage and remediation pick the right path. With a cleaner, classified signal, more of the response can be automated. A model latency incident, for example, can be handled by temporarily switching to a pre-approved fallback model, and additional async changes can be done during business hours.


We are actively working to extend support to other AI observability tools as part of our SRE Agent ecosystem and have designed this integration to be flexible, allowing us to enable data ingestion from whichever tools our on-call responders rely on. Additionally, we are building a lighter version of the same pattern that pulls failure-pattern context into the incident through incident workflows, supporting other means of automation beyond the SRE Agent scope. Different platforms, different customer footprints, same head start when the incident occurs.


Put together, these pieces point to where we believe AI incident response is headed: Autonomous Operations. A standard pattern for AI observability in which PagerDuty can diagnose incidents and, over time, resolve them on the customer’s behalf, regardless of which tool a team uses to trace and evaluate their agents.


AI failures do not announce themselves the way a server fire does. Detection tells you something might be wrong. Connecting the SRE Agent to the observability data is how we start answering what to do about it automatically and in a way we can extend to the rest of the ecosystem, rather than rebuilding for every tool.


## About the authors


Irena Grabovitch-Zuyev is a Staff Applied Scientist at PagerDuty, where she leads the development of AI agents behind PagerDuty Advance, the company’s generative AI capabilities. With over 15 years of experience in machine learning, her work focuses on applying generative AI, data mining, and information retrieval to transform how customers interact with PagerDuty and advance incident response and automation. Irena holds a graduate degree from the Technion – Israel Institute of Technology, where she specialized in information retrieval in social networks.


Antonio Correia is a Senior Applied Scientist in PagerDuty’s Data Science Core team, specializing in AI-driven customer recommendations on operational maturity, event orchestration rules, and incident response analytics. He is currently developing evaluation frameworks and benchmarks to measure and improve the performance of AI products.


Everaldo Aguiar is a Director of Applied AI and Insights at PagerDuty. With a background in predictive analytics and AI, his work focuses on deploying machine learning and Generative AI to enhance automation, noise reduction, and operational efficiency. Everaldo holds a PhD in Computer Science and Engineering from the University of Notre Dame, where he specialized in education data and student attrition models.


Murilo Venturin is a Senior Machine Learning Engineer at PagerDuty, where he builds and deploys production-scale AI systems, including intelligent agents and LLM-based workflows. With 7 years of experience in machine learning, his work centers on applying Generative AI and scalable ML systems to drive automation and operational efficiency. Murilo holds a degree in Electronic Engineering and an MSc in Biomedical Engineering, both from the University of Brasília.


Derek Barnes is a Senior Product Manager at PagerDuty who is responsible for AI & Automation features that accelerate the diagnosis and remediation of incidents for our customers. His current focus is building out capabilities to directly support LLM and AI-based incidents as a first-class problem space. Derek holds a B.Eng in Computer Engineering from McMaster University and an MBA from the University of Toronto.
