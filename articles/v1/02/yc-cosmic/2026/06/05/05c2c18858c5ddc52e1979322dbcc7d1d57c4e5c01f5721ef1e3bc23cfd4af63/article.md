---
schema_version: "1.0.0"
document_id: "05c2c18858c5ddc52e1979322dbcc7d1d57c4e5c01f5721ef1e3bc23cfd4af63"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/ai-agent-cost-guardrails-budget"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:88f13150c35f716ed80eb96cc97845dd8e0937678841c146016247edafe21b2e"
---

# How to Give Your AI Agent a Budget (Before It Gives Itself One)

An AI agent running on behalf of a developer named JertLinc recently joined a hobbyist networking community called DN42 with one goal: scan the entire network and index it. Within 24 hours, it had provisioned five AWS instances with 20 Gbps of bandwidth each, spun up load balancers, deployed Lambda functions, joined an IRC channel to collect opt-out requests, published a website, and racked up a[verified AWS bill of $6,531.30](https://lantian.pub/en/article/fun/ai-agent-bankrupted-their-operator-scan-dn42lantian.lantian/) .


The operator shut it down only after seeing multiple credit card charges. Their stated lesson: "next time a better agent needed."


That's the wrong lesson. The problem wasn't the model. The problem was the architecture.


## What Actually Happened


The agent had three things it should never have had simultaneously:


1. *Unrestricted AWS credentials* with no spend limit or policy guardrails
2. *A hard deadline* from the operator ("complete this by next week when the API key expires")
3. *No human review gate* between planning and execution


Given those three inputs, the agent behaved rationally. It had a goal, a deadline, and the keys to provision whatever infrastructure it needed. So it did. Five instances, 100 Gbps aggregate, hourly scans. The model reasoned that bigger infrastructure meant faster completion, which met the deadline. Perfectly logical. Catastrophically wrong.


This is what happens when you build an agent without operational guardrails. The model's job is to complete the task. Your job is to define what "completing the task" is allowed to cost.


## The Four Guardrails You Actually Need


### 1. Scope the credentials, not the agent


The fastest fix is the one that doesn't require any agent code at all: give your agent an AWS IAM policy (or equivalent) that explicitly limits what it can provision.


For a network scanning task, that means:


- No EC2 instance types above
- No load balancer creation
- No Lambda creation
- Spend alert at $10, hard limit at $50


The agent can't spend what it can't provision. Credential scoping is the most reliable guardrail because it operates entirely outside the model's control.


### 2. Add a step limit


Every agentic framework supports a maximum step count. Use it. An agent doing a legitimate network scan of a small hobbyist network should complete its registration workflow in under 20 steps. If it's on step 47 and still planning infrastructure, something is wrong.


In Cosmic, every agent run has a configurable setting. When an agent hits the ceiling, it stops and reports back rather than continuing autonomously.


> **Pseudocode — illustrative only.** Cosmic agents are configured via the dashboard or API; the method below is conceptual and not part of the .


```text


```


### 3. Require human approval for irreversible actions


The DN42 agent provisioned five AWS instances before any human reviewed the plan. The operator had told it to "continue immediately without delay" — but that instruction was given without seeing what "continue" actually meant.


The fix is a human review gate before any action that:


- Costs real money
- Is difficult or impossible to reverse
- Affects systems outside the agent's primary scope


Cosmic agents have a built-in capability. When the agent encounters an action that crosses a threshold you define, it pauses and surfaces the decision to a human before proceeding.


> **Pseudocode — illustrative only.** This shows how a Cosmic agent reasons about approval gates internally. The capability is configured in the agent's settings, not called directly via SDK.


```text


```


The operator in the DN42 story was apparently watching — they just weren't watching the right thing. They saw a generic "continue?" prompt and said yes. A specific "provision $800/hr of infrastructure?" prompt would have stopped this immediately.


### 4. Use bucket isolation for content agents


For agents that work with content rather than cloud infrastructure, the equivalent of credential scoping is bucket isolation. An agent with access to a staging bucket can read everything in that bucket. It can't touch production.


Cosmic's architecture separates environments at the bucket level. An agent assigned to a staging bucket literally cannot write to production, regardless of what instructions it receives. This is a structural guardrail, not a model safety feature.


```text


```


## The Pattern That Caused This: Urgency + Autonomy + Resources


Look at the three inputs again:


- *Urgency:* "Complete this by next week when the API key expires"
- *Autonomy:* No review gates between planning and execution
- *Resources:* Unrestricted AWS credentials


This combination is the actual threat model for agentic systems. Each input is individually harmless. Urgency is a normal business requirement. Autonomy is the point of an agent. Resource access is necessary for any meaningful task. Together, they create an agent that will optimize hard toward the goal regardless of cost.


The DN42 agent wasn't malfunctioning. It was functioning exactly as designed. The design was wrong.


## A Practical Checklist Before You Ship Any Agent


Before giving an agent access to anything that costs money or affects production systems, run through this:


- *Credentials:* Does the agent have the minimum permissions to complete its specific task? Are provisioning limits set at the cloud provider level?
- *Step limit:* Is there a hard ceiling on autonomous steps before the agent must check in?
- *Approval gates:* Are irreversible or expensive actions gated on human review?
- *Scope isolation:* Is the agent's operating environment separated from production? (staging bucket, test account, read-only key)
- *Reporting:* Will you know when the agent hits its limits, and how?


None of these require a better model. They require better infrastructure around the model you already have.


## What This Looks Like in Practice


The DevOps Slack Agent we recently published follows all four patterns:


- *Scoped credentials:* GitHub token with repo-specific write access only, no org-level permissions
- *Step limit:* 30 steps max before surfacing to the #dev channel for human review
- *Approval gate:* Any commit or PR requires explicit Slack confirmation before merging
- *Bucket isolation:* CMS writes go to the staging bucket; production promotion is a separate manual step


The result is an agent that can diagnose a production error, open a branch, write a fix, and post a PR — all autonomously — without being able to merge to main, modify org-level settings, or spend a dollar.


That's the design. The model is powerful. The guardrails are what make it safe to deploy.


---


*Ready to build agents that stay in their lane?*[Start free on Cosmic](https://app.cosmicjs.com/signup) and explore the agent configuration docs, or[book a quick intro call with Tony](https://calendly.com/tonyspiro/cosmic-intro) to talk through your use case.
