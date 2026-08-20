---
schema_version: "1.0.0"
document_id: "b1bddaeca660aecc4ffa730a2dfd8f39833e7a2683faf99a005963ede7b161f7"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-news-import-4d91977bfa54"
canonical_url: "https://www.gendigital.com/blog/news/company-news/ai-agent-runtime-security"
published_at: "2026-03-05T16:21:33+00:00"
first_seen_at: "2026-07-23T22:48:21.730721+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:157073b3824e8406c6270d26ea52e0d20cf81f155f5e77c917f4ab45fe96b67f"
---

# Introducing AARTS: An Open Standard for AI Agent Runtime Safety

We built a security engine that protects AI agents across several major platforms. The hardest part wasn't building the protection. It was discovering that each platform exposes different security-relevant events, provides different context about what the agent is doing and why, and enforces security decisions differently. The same agent action we can catch and block in one platform is invisible in another, not because our detection failed, but because the platform never told us it happened. There are now over a dozen agent platforms, from IDE assistants like Claude Code, Cursor, and GitHub Copilot, to agentic frameworks like LangChain, CrewAI, and OpenAI Agents SDK, to orchestrators like n8n, and each one handles this differently, if at all.


Today, Gen is releasing AI Agent Runtime Safety Standard (AARTS) as part of its Agent Trust Hub initiative, which aimsto address this fragmentation. This is not a security product, but rather a set of vendor-neutral standards that connects agent hosts (IDEs, orchestrators, frameworks, standalone agent apps), security engines (components that evaluate agent actions), and adapters (thin mappings that translate host-native events into a common schema). For those building or operating agents, AARTS enables portable, consistent security decisions across hosts, eliminating the need to rebuild security integrations from scratch. The standard is open and vendor-neutral, and any host or security vendor can implement or contribute to it.


## The Problem AARTS Addresses


Agentic systems fail in ways that are familiar but occur more rapidly and with greater reach. Examples include sequences evolving from helpful actions to dangerous activities like "download and execute," prompt injection steering tool use, supply chain issues affecting skills and plugins, sensitive data exposure by agents, and permission confusion resulting from sub-agents and external tool providers creating additional trust boundaries. Although these risks are not new, they now manifest within a rapidly diversifying set of hosts and tool APIs.


Without a shared contract, security becomes a collection of one-off integrations, each missing context and offering inconsistent enforcement. This leads either to weak coverage, due to incomplete integrations and context, or to overly blunt blocking when trust is undermined by limited interpretation. AARTS targets the root cause: the lack of shared semantics and hook points.


In our previous[post on Agent Detection and Response](https://www.gendigital.com/blog/news/company-news/ai-agent-detection-and-response) , we described how attackers register AI-hallucinated package names on npm and PyPI, preloaded with infostealers. Catching that requires seeing the package install action with enough context — the package name, the source, whether the agent hallucinated it or the user requested it. One platform gives us all of that. Another gives us the command string with no structured metadata. A third doesn't emit the event at all. Same attack, three different levels of visibility.


Simply put, AARTS standardizes where security decisions can be made (hook points), what data is available for decision-making (data model), and how decisions are enforced (verdict semantics), ensuring consistent protections across different agent hosts.


## Architecture and Roles


AARTS defines three distinct roles:


-


Host: The environment that runs the agent. The host emits lifecycle events at defined hook points and enforces decisions.


-


Adapter: A minimal translation layer that maps host-specific events into AARTS events, normalizing tool names, lifecycle phases, and context.


-


Security Engine: The evaluator that receives AARTS events, applies detection strategies, and returns structured verdicts.


This separation matters. Embedding security logic in hosts results in superficial security ("checkbox security"), while engines depending on host internals compromise portability. AARTS keeps the boundary clean: hosts provide the right moments and context, and engines make decisions. AARTS also supports various integration models (in-process, subprocess, remote), accommodating different latency, isolation, and operational requirements.


## How the Standard Works


AARTS defines hook points, moments in an agent's lifecycle where a security engine can intercept, evaluate, and act. Version 0.1 specifies 19 of them across session management, prompt lifecycle, tool execution, plugin and skill loading, sub-agent behavior, memory operations, and output delivery.


Not all hooks carry equal weight. Three illustrate the design logic:


-


PreToolUse is the primary checkpoint. Before an agent runs a shell command, writes a file, makes a network request, or installs a package, the security engine sees the action, its arguments, and the context that led to it, then returns a verdict: allow, deny, or ask the user. This is where most real-world threats get caught.


-


PreLLMRequest operates earlier in the loop, before the prompt reaches the model. It lets engines inspect prompt integrity: whether safety instructions survived context compaction, whether untrusted content was injected into the conversation, whether the instruction layering is intact. If PreToolUse is the checkpoint at the door, PreLLMRequest watches for problems before the agent even decides what to do.


-


PreSkillLoad addresses supply chain risk. When an agent installs a plugin or skill from a marketplace, the engine can evaluate the trust source (where it came from, whether it's signed, whether it was modified) before it gets loaded. AARTS can also direct the host to strip hooks from skills, preventing plugins from disabling the security layer that's supposed to govern them.


The remaining 16 hooks follow the same pattern: they sit at the moment where risk becomes real and provide the engine with enough context to make a decision.


For those decisions to be portable across hosts, the inputs have to be consistent. AARTS standardizes the event structure: every event carries a mandatory envelope (hook point, session ID, timestamp, host identity) plus standard extensions, reusable field groups for conversation context, tool details, trust source metadata, modification tracking, and provenance. A canonical tool taxonomy maps host-specific tool names (Cursor's "terminal" vs. Claude Code's "bash") to common categories (shell, file_write, web_request, package_install), so detection logic written once works everywhere.


Verdicts are structured: a decision (allow, deny, or ask), a threat category, a severity level, confidence, and a human-readable explanation for the user. Two design choices are worth noting. If the host can't prompt the user, ask falls back to deny, safe but a real constraint for headless orchestrators. If the engine crashes or times out, the host treats it as allow and logs the failure. Fail-open is the right default for developer tools where latency matters, but enterprise deployments will want to revisit this per environment.


## Conformance Levels: Gradual Adoption


AARTS defines three conformance levels:


-


Level 1 (Basic): Requires SessionStart and PreToolUse hooks, minimum tool coverage (shell, file_write, web_request), enforcement of allow and deny (ask may be treated as deny).


-


Level 2 (Standard): Adds plugin and skill load hooks, prompt lifecycle protection, broader tool coverage, additional hooks (PrePluginLoad, PreSkillLoad, PostInputProcessing, PreLLMRequest, PostToolUse), and requires provenance on PreToolUse.


-


Level 3 (Comprehensive): Provides full lifecycle coverage including user input, prompt response cycle, tool modification, sub-agents, MCP connect, memory read/write, output delivery, and session end. Clarifies sub-agent inheritance and memory protection.


This tiered approach offers a realistic adoption path: Level 1 addresses basic failures, Level 2 tackles prompt integrity and supply chain issues, and Level 3 supports persistent and autonomous agents.


## Open Considerations


As we move AARTS beyond a draft specification, the following are being considered:


-


Enterprise fail-open story: The spec mandates fail-open behavior. Deployment profiles may be needed to reflect the realities of different environments.


-


Adapter integrity: Ensuring adapters correctly map tool types and extract artifacts is vital. A conformance test suite for adapters is as important as the specification itself is a next-step priority.


-


Consent for privacy: Explicit consent is required for external transmission of prompts and tool data. Operational guidance or recommended patterns are needed to avoid incompatible implementations.


-


Ask semantics in headless environments: Treating ask as deny by default is safe but may discourage engines from using ask. A standard way for deployment policy to handle ask in these scenarios is needed.


AARTS aims to translate these considerations into predictable operational behavior.


## Call for Feedback


AARTS v0.1 is a draft intended for early review by host builders, security vendors, and enterprise operators. The full specification is available[here](https://github.com/gendigitalinc/aarts) . If you build agent hosts, consider whether you want every security integration to remain one-off. If you develop security engines, ask yourself whether you want to rewrite the same logic for each host and still miss critical context. AARTS aims to make the answer “no”. We welcome contributions — open an issue or submit a pull request.
