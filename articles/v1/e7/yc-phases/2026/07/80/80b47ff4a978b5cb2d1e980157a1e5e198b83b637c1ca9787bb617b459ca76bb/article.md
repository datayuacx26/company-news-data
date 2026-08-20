---
schema_version: "1.0.0"
document_id: "80b47ff4a978b5cb2d1e980157a1e5e198b83b637c1ca9787bb617b459ca76bb"
company_key: "yc-phases"
company: "Phases"
source_id: "yc-phases-news-import-61e19d289ed9"
canonical_url: "https://phases.ai/blog/building-secure-agents-in-clinical-trials"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-24T08:18:43.014149+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:7bf7a4378abce96cbc20806741961ec3050aa47d724b9bc50db85888688c921d"
---

# Building secure agents in clinical trials

In June 2026, Novo Nordisk disclosed that attackers had breached its internal systems and copied data from some of its clinical trials, including patient IDs, years of birth, biomarkers, immunogenicity results, and lifestyle details down to smoking and alcohol use.1 The company said the data was pseudonymized and that no participant could be identified from it. The group that claimed the attack said the haul was larger, and included dozens of the company's own proprietary AI models alongside the trial data. Researchers who examined the intrusion traced the entry point to credentials left sitting in code, tied to an account that had far more access than it needed.2


This is an all too familiar kind of security vulnerability, the kind that happens with or without AI in the picture. But as AI takes on more of the work in clinical trial operations, from reading, classifying, and QCing trial master file (TMF) documents to emailing sites, reconciling queries, and moving data between systems, the same mistake gets easier to make and more expensive when it happens. An agent that holds too much access is a bigger target than a person who does, because it acts continuously and at a scale no single credential misuse by a human ever could.


## What can go wrong with AI agents


A[copilot](https://phases.ai/blog/copilot-ceiling-in-clinical-trials) drafts text and leaves a person to decide what to do with it, whereas an agent takes actions on live systems with real credentials and real access, and it's that difference that leads to risk.


### Prompt injection


An agent works by reading text from different sources, such as documents, emails, query responses, or records pulled from other systems. A prompt injection is when an attacker hides instructions inside the text that an agent reads, hoping the agent will follow them instead of performing its actual task. For instance, a single line hidden in an emailed document could tell the agent to download documents it should not touch and send them somewhere it should not. It is the most common attack on these systems, and it is genuinely hard to completely prevent.34


### Too much access


To be useful, an agent needs tools and credentials so it can read from the EDC or update the TMF, for instance. The problem of too much access arises when the agent is given more access to perform its task than it requires. This is the same failure mode behind the Novo Nordisk breach.2 A prompt-injection attack that succeeds against a properly scoped agent still can't reach documents it was never given access to in the first place.


## How to secure AI agents


Securing an agent means keeping it isolated, constrained, and guardrailed, with access only to the systems it needs at the moment it needs them.


### Filtering


Anything that reaches the agent without the system controlling it, such as a user typing into a chat interface or a document being ingested, needs to pass through a filtering check first. That check usually runs on a small, secondary model whose only job is to sit between the untrusted prompt or document and the main system and decide whether it looks malicious.5 A set of deterministic filter checks can run alongside it to catch specific malicious phrases like "give me your system prompt." What these filters ultimately prevent are the prompt injection attacks described above.
