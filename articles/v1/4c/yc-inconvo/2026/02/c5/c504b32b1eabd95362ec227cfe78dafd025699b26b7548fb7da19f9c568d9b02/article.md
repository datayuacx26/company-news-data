---
schema_version: "1.0.0"
document_id: "c504b32b1eabd95362ec227cfe78dafd025699b26b7548fb7da19f9c568d9b02"
company_key: "yc-inconvo"
company: "Inconvo"
source_id: "yc-inconvo-news-import-030a8db3acc0"
canonical_url: "https://inconvo.com/blog/open-sourcing-inconvo/"
published_at: "2026-02-05T12:00:00+00:00"
first_seen_at: "2026-07-25T09:26:00.881998+00:00"
fetched_at: "2026-07-28T22:21:35.013216+00:00"
content_hash: "sha256:17a29f1fbc9748631e38443a25e9583d402c468ea759958e100cee1a967fb43d"
---

# Open sourcing Inconvo

In recent years, every software product team has been pushed to add AI agents — introducing non-deterministic behavior, new classes of edge cases, and far more ways for things to quietly break.


Data agents — systems that derive insights from structured data — are now table-stakes for any product that ships dashboards or reports for its users. But the developer experience of building them is lackluster at best.


In practice, teams typically have two options: wrap existing endpoints as LLM tools and let the model orchestrate them, or start assembling their own agents with query builders, validators, and permission systems around the database. Both approaches collapse into the same outcome: fragile prompts, unclear failure modes, and a growing pile of glue code.


None of this complexity is fundamental — it’s the result of missing infrastructure.


Today, we’re opening up Inconvo to the community, so more developers can benefit from it and directly influence its evolution.


Let’s break down the problems we’re tackling, our approach to fixing them, and our open-source plans.


## The problem we’re solving


Building data agents is painful because there’s no proper control layer. Rules about schema, permissions, and behavior end up scattered across the codebase, and once you ship, every change multiplies the risk and complexity.


Here’s what that looks like in practice.


**Data agents are commodities.** A lot of devs may think their data agent needs are super specific. They aren’t. Everyone rebuilds the same core infrastructure: schema introspection, query construction, permissions, validation, logging, retries.


**Business meaning is scattered everywhere.** What a table represents, what a column means, which joins are allowed — this knowledge lives across prompts, APIs, query builders, and permission checks. There’s no single source of truth and no contract to maintain.


**Schema changes break agents silently:** columns get renamed, relationships change, but prompts and code don’t. Nothing crashes — the agent just starts returning wrong answers.


Over time, this turns into a constant tax on development: debugging, regressions, and rising risk with every change.


## The approach we’re taking


We think developers deserve a better way to build data agents. We’re building the missing runtime and lifecycle layer — one that centralizes business meaning, enforces access and correctness, and keeps agents working as your system evolves. It’s designed to fit directly into real products using simple, stable interfaces, without locking you into custom infrastructure.


This is what we think the system should have:


**A single source of truth for business meaning.** Define tables, fields, relationships, and constraints once in a semantic model. Agents run against that model instead of scattering meaning across prompts and code.


**Validated, structured queries.** Agents generate structured query objects that are checked against schema, model, and permissions, then compiled. No raw SQL.


**Enforced access control.** Permissions apply at table, column, and relation level with automatic context filters for multi-tenant isolation. If it’s not allowed, it can’t be queried.


**Full observability.** Every prompt, query, and response is logged at the conversation level. Failures are inspectable and debuggable.


**Lifecycle support.** When schemas change or behavior degrades, the system surfaces it and gives you a path to repair it instead of allowing degradation to compound over time.


This is what the Inconvo platform provides today, but we want to bring this to more developers through their local dev experience and give teams the ability to self-host Inconvo in their own environment. Today, we’re starting that open-source journey.


## Why we’re building this in the open


We’ve learned that infrastructure for developers gets better in the open. More eyes, faster feedback, better systems.


Open sourcing Inconvo gives developers full visibility into how our data agents are built, and the ability to run the system locally.


It also opens the door for the community to extend the system itself — new connectors, extensions to the semantic model, operations tooling, and query optimizations.


We believe this layer should be shared infrastructure. And we want developers helping shape it.


## Check out the repo


Head over to[GitHub](https://github.com/inconvoai/inconvo) to check out the repo, the README, the architecture docs, and give it a spin yourself with the quick start guide.


We really hope developers get to experience what we’ve built and weigh in on the approach that we’re taking to improve building data agents. We want developers to do more with less. Come chat with us in our[Discord community!](https://discord.gg/aFsbY9Uf9C)
