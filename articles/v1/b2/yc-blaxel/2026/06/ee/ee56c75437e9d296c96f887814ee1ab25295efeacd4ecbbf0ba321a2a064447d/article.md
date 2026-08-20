---
schema_version: "1.0.0"
document_id: "ee56c75437e9d296c96f887814ee1ab25295efeacd4ecbbf0ba321a2a064447d"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/blaxel-supports-claude-managed-agents-with-self-hosted-sandbox-execution"
published_at: "2026-06-11T12:17:26+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:aea4f0d34b4bd158a094e807d316db312ea277f6fe645c099289eaaae34d3c1d"
---

# Blaxel now supports Claude Managed Agents with self-hosted sandbox execution

Blaxel integrates with Claude Managed Agents (CMA) as a self-hosted sandbox provider. Anthropic manages the agent loop. Blaxel provides the execution substrate. Developers get full control over where and how their Claude agents run code, access files, and execute commands.


## What this means


Claude Managed Agents is[Anthropic's agent harness](https://platform.claude.com/docs/en/managed-agents/overview) for running Claude as an autonomous agent. It handles orchestration, context management, prompt caching, and error recovery. By default, agents execute tools inside Anthropic-managed cloud sandboxes.


With self-hosted sandboxes,[announced by Anthropic in May 2026](https://claude.com/blog/claude-managed-agents-updates) , developers can move tool execution to infrastructure they control. The agent loop stays on Anthropic's side. The compute moves to yours.


Blaxel is now available as a self-hosted sandbox provider for CMA. When a Claude Managed Agent session starts, Blaxel microVMs handle the actual work: running shell commands, reading and writing files, executing code, and performing computations. All in microVM-isolated environments, with persistent memory and 25-millisecond resume from auto-suspend.


## How it works


The integration follows an orchestrator/worker architecture.


An orchestrator sandbox runs on Blaxel and connects to the CMA environment's work queue. When Anthropic's control plane assigns a session, the orchestrator claims the work item and spawns a dedicated worker microVM (a Blaxel sandbox) for that session.


The worker sandbox receives tool execution requests from Anthropic, runs them locally, and posts results back. File operations, bash commands, code execution, and any other tool calls happen inside the Blaxel sandbox. Tool inputs and outputs flow back to Anthropic's control plane so Claude can see results and decide what to do next.


Each worker sandbox persists for the duration of its session. The agent can install packages, write files, build projects, and accumulate state over minutes or hours of work. Nothing resets between tool calls. When the session ends, the sandbox can be cleaned up or kept in standby for reuse.


This maps directly to how CMA's self-hosted sandbox protocol works. As described in[Anthropic's self-hosted documentation](https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes) :


> Self-hosted sandboxes keep the orchestration on Anthropic's side but move tool execution into infrastructure you control, so the agent's code, filesystem, and network egress never leave your environment.


## Why the split matters


The separation between agent orchestration and tool execution is an architectural choice with practical consequences.


Anthropic handles the parts they're good at: the agent loop, model inference, prompt caching, compaction, and session history. Developers don't need to build or maintain any of that.


Blaxel handles runtime infrastructure, where the actual compute happens. This means developers control:


-


Where code runs (Blaxel's multi-region infrastructure)


-


How environments are isolated (microVMs with VM-level security boundaries)


-


What the network can reach (Blaxel's programmable networking controls)


-


How state is managed (persistent memory that survives auto-suspend)


-


Cost behavior (transitions to standby after approximately 15 seconds of no active connection, resume in 25 milliseconds)


For teams building on Claude that need to keep data within their own infrastructure boundary, reach internal services, or operate under specific compliance controls, this separation gives them the CMA agent harness without giving up control over execution.


## What this enables


With Blaxel as the execution layer for Claude Managed Agents, several types of workloads become straightforward to run:


**Long-running autonomous tasks.** CMA sessions can run for hours. A worker sandbox on Blaxel persists for the entire session, maintaining full state. The agent can work through multi-step workflows (code generation, testing, iteration) without losing context between tool calls.


**Code generation and execution.** Claude agents can write code, run it, inspect the output, and iterate. The sandbox provides a full Linux environment with filesystem, process management, and network access. Blaxel's MCP server integration means agents interact with the sandbox through tool calls for file operations, process management, and code execution.


**Data processing in controlled environments.** Agents processing sensitive data can do so inside Blaxel micro-VMs, where file operations and network egress stay within your configured boundaries. Data never passes through Anthropic's sandbox infrastructure.


**Multi-agent architectures.** Teams running multiple Claude agents can provision separate Blaxel sandboxes per session, each isolated and independently managed. The orchestrator handles the mapping between CMA sessions and Blaxel compute.


## Blaxel's infrastructure under the hood


The integration builds on the same runtime infrastructure Blaxel provides across all its products.


Blaxel sandboxes are microVMs, the same virtualization technology that powers AWS Lambda. Each sandbox is a full isolated environment with its own filesystem, processes, and network stack. Transition to standby kicks in after 15 seconds of no active connection, and resume from standby takes 25 milliseconds with full memory state intact.


For CMA workloads, this means worker sandboxes spin up fast when sessions arrive and don't waste compute when the agent is waiting for Claude's next instruction. Persistent memory means the sandbox doesn't need to rebuild state if there's a pause in the agent loop.


Every Blaxel sandbox also exposes a[Model Context Protocol (MCP) server](https://docs.blaxel.ai/Sandboxes/MCP) , giving agents a structured interface for file operations, process management, and code execution. This same MCP integration powers Blaxel's existing support for the Claude Agent SDK.


## Getting started


If you're already building with Claude Managed Agents and want to run sessions on Blaxel infrastructure, check our[integration guide for Claude Managed Agents](https://docs.blaxel.ai/Tutorials/Claude-Managed-Agents) . The setup involves creating a self-hosted environment in the Anthropic Console, deploying the Blaxel orchestrator, and pointing CMA sessions at your environment.


To get started, clone the[sample repository](https://github.com/blaxel-ai/cma-blaxel-sandbox) :


bash


` git


clone https://github.com/blaxel-ai/cma-blaxel-sandbox.git


`


## How this fits


Blaxel is the infrastructure foundation for autonomous agents. That means providing the execution layer wherever autonomous agents need to run, regardless of which orchestration system manages the agent loop.


Claude Managed Agents is one of several approaches developers are using to build autonomous systems. In each case, the agent needs somewhere to execute: a place to run code, store files, manage state, and access the network.


Blaxel provides that execution layer with the reliability and control that production workloads require. Persistent memory, instant resume, isolation, and programmable networking, available as the runtime for whatever orchestration layer you choose.


The CMA integration is the latest step in making Blaxel available wherever autonomous agents need compute. We'll continue building integrations that give developers more options for how they deploy and run their agents.


Also, if you're using the Claude Agent SDK (rather than CMA), Blaxel already supports that workflow. Our[Claude Agent SDK tutorial](https://docs.blaxel.ai/Tutorials/Claude-Agent-SDK-MCP) walks through connecting Claude agents to Blaxel sandboxes via MCP.
