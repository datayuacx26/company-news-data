---
schema_version: "1.0.0"
document_id: "d358d8792fd316bd1a78018f6d5350923146889f9ddd316b6cb28eca78772d32"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/codspeed-uses-claude-managed-agents"
published_at: "2026-06-11T06:16:36+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:e1f047690f41ebd0f500730244f247674e6cb77b4d0633b42c4350565bf41170"
---

# CodSpeed uses Claude Managed Agents and Blaxel to optimize your code performance

[CodSpeed](https://codspeed.io/) is the software performance platform that autonomously suggests optimizations and catches performance regressions before they land. It offers an AI agent that takes customer code, measures it using CodSpeed's CPU simulation approach — which delivers consistent, noise-free performance measurements even inside virtualized environments — and iterates: modifying code, re-running benchmarks, and comparing results until it can propose a measured set of performance improvements, complete with diffs and a pull request.


To achieve this goal, a CodSpeed agent needs to call tools, execute commands, work with files and directories, write code, and perform computations. This is best achieved with a managed agent harness. At the same time, considering the sensitive nature of the customer code it works with, CodSpeed also needs complete control over the execution environment in which their agent runs.


With the launch of Anthropic's[Claude Managed Agents](https://platform.claude.com/docs/en/managed-agents/overview) , CodSpeed explored a new approach:[self-hosting](https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes) the[sandbox layer with Blaxel](https://docs.blaxel.ai/Sandboxes/Overview) while keeping the agent loop entirely within Anthropic's platform.


> “Claude Managed Agents + Blaxel moves two pieces off our plate: scaling the agent harness processes (and their observability) and the handling of secure sandboxes, since we now don’t need to provision them directly. This reduces the overall friction significantly for us.” - Arthur Pastel, CodSpeed


## How it works


When a CodSpeed session starts, Claude Managed Agents loads the system prompt and the CodSpeed MCP server, then requests a Blaxel sandbox provisioned from CodSpeed's custom image.


The agent clones the repository once, then iterates in place: edit code, run benchmarks, compare results, repeat. Because benchmarks run under CodSpeed's CPU simulation, measurements stay consistent and reproducible even though the code executes in a virtualized environment where wall-clock benchmarking would be far too noisy to trust.


On top of each measurement, CodSpeed automatically generates profiling data of the benchmarked code. The agent retrieves this data through the CodSpeed MCP server to pinpoint bottlenecks precisely, letting it target the right code paths and iterate much faster. The agent stops when something measurable and impactful is found.


When the agent stops, results are submitted back via CodSpeed's internal MCP server as structured data: the performance impact, the code diff, and before/after benchmarks. CodSpeed surfaces this data in its UI and, if the user approves, a second agent opens a pull request with the recommended changes.


Here's a sequence diagram of the architecture:


## Choosing Blaxel as the sandbox provider


For CodSpeed, selecting Blaxel as its sandbox provider made sense for multiple reasons:


-


High-performance, scalable infrastructure: Blaxel sandboxes are designed for AI-native workflows, allowing agents to run code in persistent environments that wait on standby indefinitely when not used. Sandboxes automatically scale to zero after 15s inactivity and resume in 25ms, eliminating cold starts and ensuring that CodSpeed only pays for compute that its agent actually uses. Blaxel also provides feature-rich storage options to support stateful, long-running agent sessions without compromising security.


-


Custom worker images with specialized tooling: CodSpeed's agent doesn't just run code; it measures it and tries to improve its performance. For this, the agent needs access to specific tools, such as the CodSpeed CLI, CPU simulation tools, and other specialized profiling and benchmarking utilities. With Blaxel, CodSpeed is able to[create and use custom sandbox images](https://docs.blaxel.ai/Sandboxes/Templates) containing all the necessary toolchains and build dependencies needed by its worker agents.


-


Proxy and controllable networking: Blaxel supports[proxy routing with secrets injection](https://docs.blaxel.ai/Sandboxes/Proxy) , enabling CodSpeed to give its agent secure access to third-party credentials. Blaxel’s proxy injects the credentials into the worker's outbound requests, so the secret never enters the worker sandbox or the CMA agent config. For even greater security, the proxy can also be used to control which external domains the worker sandbox can reach.


-


Existing commercial relationship: CodSpeed was already using Blaxel's perpetual sandboxes before Claude Managed Agents launched. As a result, the CodSpeed engineering team knew the platform and the APIs, and already had very positive experiences with Blaxel's support team. So, when it was time to choose a sandbox provider, Blaxel was the natural fit.


## The best of both: managed agents + self-hosted execution


This approach demonstrated that CodSpeed could have the best of both worlds: the agent harness, authentication, and lifecycle is handled by the Claude Platform, while the execution environment for customer code remains within their sphere of control and observability via Blaxel. The combination removes two concerns for CodSpeed: scaling the agent harness processes and their observability, and direct provisioning of sandboxes.


The combination of Claude Managed Agents and self-hosted Blaxel sandboxes creates an agentic solution that's faster to operate and easier to maintain, while still allowing full configuration and control over the sandbox execution layer.


For any team running agents on sensitive code with specialized execution requirements, this split - managed agent harness, self-hosted sandbox execution layer - is worth considering. To learn more, check out our tutorial on[using Claude Managed Agents with Blaxel](https://docs.blaxel.ai/Tutorials/Claude-Managed-Agents) .
