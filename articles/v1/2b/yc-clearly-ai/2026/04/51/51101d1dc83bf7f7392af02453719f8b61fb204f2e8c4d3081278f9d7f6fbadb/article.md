---
schema_version: "1.0.0"
document_id: "51101d1dc83bf7f7392af02453719f8b61fb204f2e8c4d3081278f9d7f6fbadb"
company_key: "yc-clearly-ai"
company: "Clearly AI"
source_id: "yc-clearly-ai-news-import-066e8c806377"
canonical_url: "https://clearly-ai.com/blog/clearly-ai-technical-explanation"
published_at: "2026-04-03T00:00:00+00:00"
first_seen_at: "2026-07-23T05:49:00.156409+00:00"
fetched_at: "2026-07-28T21:25:41.488077+00:00"
content_hash: "sha256:106a3c587c646b229a38055105516b6de80f98e985af9b4260cac66307b218a7"
---

# Clearly AI for technical people

A fellow security engineer told me after RSAC:


> I wish there was a button on vendor websites to say “I’m a technical person - just tell me how it works.”


So, today I’m doing just that! Here’s a technical explanation of Clearly AI, as well as how we compare to our competitors or building in-house.


### What is Clearly AI?


Clearly AI is software for security teams who need to complete security (and related) reviews faster.


**The problem:** Security teams at enterprises spend significant time gathering scattered information (design docs, code, tickets, wikis) and performing repetitive baseline security reviews across many systems.


**The solution:** Clearly AI helps by gathering that information faster (via Integrations and uploads) and generating structured assessments with AI. You bring the context (Projects, Knowledge Base, Integrations); Clearly AI runs customizable security and privacy evaluation against that context and produces answers with citations and, where applicable, compliance status.


**Intended audience:** Security engineers, compliance analysts, and risk owners at mid-size to large organizations who run threat models, control assessments, privacy impact assessments, or similar reviews. Clearly AI acts as a force multiplier: it handles triage and data gathering so experts can focus on judgment and high-risk items.


What Clearly AI is built for:


1.


**Triage:** Check many systems for baseline controls, flag what needs human attention, and approve low-risk systems quickly.


2.


**Deep analysis:** For complex systems, synthesize context and produce thorough security analysis that would take hours to gather manually.


### How Clearly AI works


#### 1) Kickoff


Clearly AI can be run:


-


Automatically when a new artifact shows up (for example, a Jira ticket, Confluence design doc, or a PR) via customizable workflows


-


Adhoc via #ask-security Slack integration / chat-based input


This trigger model is designed to match how security and privacy reviews actually enter a queue, rather than forcing teams into a new workflow.


#### 2) Build context


When a review triggers, Clearly AI pulls in context from the same places a human reviewer would start:


-


The primary artifacts (ticket, doc, PR)


-


Supporting artifacts and “adjacent context” (linked docs, relevant repository context, prior decisions, standards)


From there, it runs a structured analysis that breaks the work into multiple steps (not “one big prompt”), which improves consistency and reduces hallucinations compared to DIY.


#### 3) Run the appropriate reviews


Clearly AI is effectively a “first-draft security architect” workflow:


-


Builds an architecture / system view from what exists in the artifacts (diagrams from code, etc)


-


Produces data flows, identifies exposure points, and extracts what matters about AuthN/AuthZ, third parties/SBOM impact, data types/logging, etc.


-


Runs a threat analysis (STRIDE, MITRE ATT&CK, MAESTRO, etc) and returns prioritized, actionable mitigations rather than generic checklists.


-


Can escalate only higher-risk items to a security reviewer, while leaving lower-risk fixes as dev-ready guidance.


#### 4) Outputs & outcomes


Outputs are designed to be “review artifacts” you can actually drop into an existing program:


-


Architecture diagram + key system assumptions


-


Structured findings, threats, and mitigations (often STRIDE-based)


-


A clear list of follow-ups when the source artifacts are missing key info


-


Structured findings that can be posted back into Jira / a findings DB


### Clearly AI vs. Clover Security / Prime Security


#### Dashboard vs. Outcome


Clover & Prime were built by security vendors for what they think security teams want: dashboards. Both tools are very dashboard-driven (let me scan all of your Jira tickets and give you a dashboard).


Clearly AI is outcome-focused. We produce a security review the way a security engineer would do it by looking at a system holistically, mapping data flows and controls against your frameworks, and creating a prioritized plan of what to do next.


#### AI explainability and transparency


Clover & Prime hide their AI functionality behind dashboards and reports - with minimal explainability or ability to decompose risk scores. Because of this, outputs are often very generic and can vary greatly over each run.


With Clearly AI you can:


-


See the exact prompts being run, modify them, version them, and re-run them consistently across systems


-


Run near-deterministic outputs for repeatability.


#### Clearly AI is completely customizable:


-


**Trigger routing:** which artifacts trigger which workflows (Jira vs Confluence vs PR vs ad hoc)


-


**Policy + standards encoding:** ingest your standards, plus structured questionnaires (including custom YAML-style questionnaires for policy compliance workflows)


-


**Company knowledge:** add secure defaults, provide historical tickets / threat models, and Clearly AI will base data off of your workflows


-


**Workflow tuning:** when outputs are off, it routes back for adjustment so behavior becomes reliable over time


-


**Enterprise controls:** single-tenant or private cloud options, BYO LLM API key, etc.


Clover and Prime make you adapt to their way of working, whereas we adapt to yours.


### Instead of DIY: Build on Clearly AI


Claude Code might make building easier, but there’s a major difference between cobbling together a POC and building a reliable production system. We create a harness for security engineers to build a variety of workflows on top of our baseline guardrails, context engineering, and platform.


-


**Consistency and repeatability:** Clearly AI explicitly decomposes the work into multiple structured LLM calls per component to improve reliability over “single prompt, single output.” If you run the same review 10 times, you get the same outputs.


-


**Operationalization:** it is built to generate outputs that become tickets and routed decisions, including escalating only the higher-risk cases.


-


**Continuous improvement loop:** instead of every engineer rewriting prompts, the system is tuned so it gets more trustworthy over time.


We provide all of the key AI engineering components to your team - from guardrails like hallucination prevention to context building via MCPs and integrations. Your team can still "build" on Clearly AI - we just take care of a lot of the difficult fundamentals for you.


Claude Code is a harness for developers to write code, Clearly AI is a harness for security teams to build complex evaluation workflows.
