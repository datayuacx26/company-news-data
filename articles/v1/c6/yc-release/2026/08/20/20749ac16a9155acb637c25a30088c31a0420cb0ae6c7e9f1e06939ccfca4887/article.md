---
schema_version: "1.0.0"
document_id: "20749ac16a9155acb637c25a30088c31a0420cb0ae6c7e9f1e06939ccfca4887"
company_key: "yc-release"
company: "Release"
source_id: "yc-release-news-import-be48e1d74beb"
canonical_url: "https://release.com/blog/hardening-ai-agents-with-release-managed-environments"
published_at: null
first_seen_at: "2026-08-17T10:00:40.724357+00:00"
fetched_at: "2026-08-17T10:00:43.357848+00:00"
content_hash: "sha256:a382dcb92c94e5a4d029fcbb3d7d031266b0b15235376e3e07a3d43e9158b0a4"
---

# Hardening Your AI Agents: Run Them in Your Own Kubernetes Cluster, Not on a Laptop or in a Vendor Black Box

## The Problem No One Wants to Admit


At Release, we've been building environments for every PR since day one. Lately, more of those PRs aren't coming from human developers. They're coming from AI coding agents: Cursor, Claude Code, and similar tools that write code, run tests, commit, and open pull requests on their own. The productivity wins are real. So are some risks that rarely come up in the demos.


Here's what the typical AI agent setup actually looks like from a security standpoint:


- The agent runs on a developer's laptop, inheriting their full shell environment: AWS credentials, database connection strings, GitHub tokens, and whatever else lives in` .env` files or` ~/.aws/credentials` .
- The agent can call arbitrary shell commands, reach any service the developer can reach, and accumulate a session-long memory of everything it has touched.
- When it's done, there's no audit trail beyond a git log and whatever the LLM chose to surface in the conversation.


The alternative most teams consider is a SaaS AI platform that runs agents for you. That solves the laptop problem, but trades it for a different one. Your code, your environment variables, and your proprietary data flow into someone else's infrastructure. You trust their security posture on faith. You can't inspect the runtime or verify what happens to your data after the session ends.


Neither option gives a platform or security team real control over what's running.


## Two Failure Modes, One Root Cause


The laptop and the vendor black box are surface-level different but share the same root cause: **agents running without real isolation** .


Real isolation means:


- The agent's runtime is separate from the engineer's identity and credentials.
- The blast radius of any mistake or compromise is bounded to a single, short-lived environment.
- Every environment gets only the secrets it needs, not everything the developer has access to.
- The runtime disappears when the work is done, not when someone remembers to clean it up.
- Human review happens before any agent-generated change reaches a shared or production system.


Without those properties, "AI-assisted development" is just "development with an additional attack surface."


## How Release Answers It


Release is an environments-as-a-service platform built on Kubernetes, designed to give every PR, every feature branch, and every AI agent its own full-stack, production-like environment. The security properties that make it useful for human developers are the same ones you need to harden an agent workflow.


### 1. Your Kubernetes Cluster, Not Ours


Release provisions and manages Kubernetes clusters inside **your own AWS or GCP account** . When an agent's environment spins up, it runs in a namespace inside your VPC. Your code never leaves your cloud. Your data never leaves your cloud. There's no "trust us" involved: you can inspect the cluster, configure your own IAM policies, and apply whatever network controls your security team requires.


This is the foundational difference from a vendor black box. You own the compute. Release manages the orchestration.


For teams already operating under compliance frameworks like SOC 2, HIPAA, and internal data residency requirements, this matters. The agent needs to run where your data is allowed to be. With Release, that's the default.


### 2. An Isolated Environment for Every Agent Run


Every branch, every pull request, and every agent session gets its own isolated, full-stack ephemeral environment: a complete application stack including your backend services, databases, background workers, and any dependent microservices.


Agent A's run cannot interfere with Agent B's run. A prompt injection that causes one agent to do something unexpected stays contained to that environment. A misconfiguration, a bad tool call, a runaway process. The blast radius is bounded to a single namespace that will be torn down automatically when the work is done.


This is the difference between an agent that can wreck your shared staging database and one that can only wreck its own throwaway copy of it.


### 3. Secrets Scoped to Each Environment


The agent on a developer's laptop runs with whatever that developer happens to have in their credential chain. It sees everything: production database URIs, AWS keys with broad permissions, third-party API tokens.


With Release, you inject secrets per environment. Through[native Kubernetes secrets management](https://docs.release.com/how-to/how-to-set-environment-variables) or integrations with tools like Doppler, each environment gets exactly the credentials it needs for its scope: staging credentials for a staging environment, read-only database access for a test environment, nothing more.


An agent running a test suite doesn't need write access to your production S3 bucket. With per-environment secrets, it simply doesn't have it. The credential surface shrinks to match what the work actually requires.


### 4. Ephemeral Runtimes That Disappear When the Work Is Done


The agent's runtime isn't a long-lived server. It's an ephemeral environment that spins up when the PR opens and tears down automatically when it closes or when you define the teardown condition.


There's no persistent, always-on agent process accumulating session state, building up permissions over time, or sitting idle with your credentials loaded. The attack surface exists only while the agent is actively doing work. Then it's gone.


Release environments[scale to zero when inactive](https://docs.release.com/guides-and-examples/advanced-guides/pausing-instant-datasets/pausing-resuming-environments) and tear down automatically, which cuts pre-production cloud costs by 30 to 70 percent. From a security standpoint, an idle environment holding credentials isn't a persistent target if it no longer exists.


### 5. PR-Gated Workflow: No Agent Change Reaches Production Unreviewed


The workflow Release enables is GitOps by design. An agent makes changes, commits them to a branch, and opens a pull request. Release automatically provisions a full-stack preview environment for that branch. Humans can review the running application directly, stepping through a live stack rather than reading a code diff. Automated end-to-end tests run against that real stack before any review is approved.


Nothing an agent writes merges to main until it has passed through that review gate. The agent is fast and autonomous for the work that can be automated: building, deploying, running tests against its own environment. The path to production still runs through a PR where a human can say no.


This is the operational safety layer that pure-automation approaches miss. Agents make mistakes. Agents can be manipulated via prompt injection. The PR-gated model means those mistakes get caught at review time, against a real running preview, before they compound.


### 6. Structured Agent Access via the MCP Server


Rather than giving an agent raw shell access to your infrastructure, Release provides a[Model Context Protocol (MCP) server](https://release.com/product/mcp-server) that exposes structured, auditable operations: create an environment, deploy a service, fetch logs, check environment status.


The agent calls Release operations through the MCP interface, not through arbitrary shell commands. That's a dramatically smaller API surface than open access to the terminal. Every action is a defined operation with known semantics, not a free-form shell command that could reach anything the agent's process can reach.


From a security standpoint, that matters: you know what the agent can do because you control what operations the MCP server exposes.


## What This Looks Like in Practice


A team running AI agents with Release ends up with a workflow that looks something like this:


1. A developer or automated system triggers an agent with a task: "implement the new rate-limiting middleware described in this issue."
2. The agent works locally or in its own provisioned environment, writing code and running initial tests.
3. The agent opens a pull request. Release automatically provisions a full-stack preview environment for that branch inside the team's own Kubernetes cluster.
4. The agent (or a separate review agent) can interact with the running preview environment through the Release MCP server: check deployment status, fetch logs, run additional tests.
5. The preview environment uses secrets scoped to the staging configuration, not production credentials.
6. A human reviewer looks at the diff and the running preview application before approving.
7. The PR merges. The preview environment tears down automatically.


At no point does the agent have access to production credentials. At no point does the agent's runtime persist beyond its task. At no point does agent-generated code reach production without human review of a real, running application.


## The Containment Question


The question for platform and security teams isn't whether AI agents will write your code. They already do. Resisting that at this point is a hiring and velocity problem, not a security decision.


The actual question is whether those agents run with real containment. And the answer depends on your infrastructure, not your vendor's promises.


Release provides the Kubernetes-backed foundation: your cluster, per-environment secrets, ephemeral runtimes that disappear when the PR closes, and a review step where humans interact with a running application before anything merges. Those properties were built for human developer workflows. They apply just as well when the PR author is an AI agent.


[Try Release](https://release.com/signup) and see how your agent workflow looks when it runs in a hardened environment inside your own cloud.
