---
schema_version: "1.0.0"
document_id: "f3923ce8730112919446f99f397f7095d41c4a4d72e73ec3d9a6bfdd0fba194e"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-news-import-4d91977bfa54"
canonical_url: "https://www.gendigital.com/blog/news/company-news/ai-agent-detection-and-response"
published_at: "2026-03-04T19:40:43+00:00"
first_seen_at: "2026-07-23T22:48:21.730721+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:771b0cfbb810d4ac4b4593683c2649f79cd1628349ff4e651ea943db064a3989"
---

# Gen Agent Trust Hub Introduces Agent Detection and Response: The Missing Security Layer for Agentic AI

You gave an AI agent access to your computer. Maybe it's writing code for you, managing your emails, or handling your finances. It's working with your credentials, acting on your behalf, and making decisions you used to make yourself. Fast, confidently, and largely unsupervised. That's the deal: you trade safety and oversight for productivity. That changes the security model.


But who's watching what it actually does?


That is why we built the Gen Agent Trust Hub (ATH), our trust layer for the agentic AI ecosystem, designed to secure agents across their lifecycle from skill to runtime to enforcement.


Agent Detection and Response (ADR) is the runtime pillar — the layer that governs what agents actually do.


## What is ADR?


ADR intercepts agent actions at the moment of execution, before they land, and evaluates whether they're safe. It allows, asks the user, or denies. That's the core.


Verification tells you what an agent **is** . ADR tells you what it's **doing** . These are complementary. Verification is necessary, but it can't govern what happens at runtime. A verified skill can still hallucinate a malicious package name. A signed plugin can still be exploited through prompt injection. A trusted agent can still lose its safety instructions mid-session and go off the rails. The moment of action is where risk becomes real.


The principle isn't new. Every major computing paradigm has developed its own detection and response layer. Endpoints got EDR. Distributed infrastructure got XDR. Not replacements for what came before, but purpose-built layers for new execution surfaces with new threat models. AI agents are the next such surface, and ADR is the layer built for it.


Within the Agent Trust Hub, ADR is the enforcement engine that turns trust signals into real-world protection.


## Why Now?


The threats targeting AI agents are already here. What follows is a small sample of the agentic threats we’re seeing. The actual attack surface spans everything an agent can do, and agents can do a lot.


**Your agent installs malware for you.** AI agents hallucinate. When they hallucinate a package name during installation, that's not just an error, it's an opportunity. Attackers monitor AI-suggested package names and register them on npm and PyPI, pre-loaded with infostealers and backdoors. The technique is called slopsquatting, and it's already being exploited at scale. In February 2026, researchers uncovered a campaign of[19 typosquatted packages specifically](https://www.securityweek.com/new-sandworm_mode-supply-chain-attack-hits-npm/) targeting users of AI coding tools, designed to exfiltrate SSH keys, AWS credentials, and API tokens.


**Your agent ignores your instructions.** Summer Yue, Meta's Director of Alignment, told her OpenClaw agent to[confirm before taking any action](https://techcrunch.com/2026/02/23/a-meta-ai-security-researcher-said-an-openclaw-agent-ran-amok-on-her-inbox/) on her email. It worked, until the session ran long enough to trigger context compaction, a mechanism where the agent compresses its memory to keep running. The safety instructions were among the first things dropped. The agent deleted 847 emails. She had to physically run to her machine to stop it.


**Your agent is a new delivery channel for malware.** Attackers are already using multiple paths to get malicious code running through agents. Weaponized skills on public marketplaces have delivered the[AMOS infostealer to macOS users](https://x.com/GenThreatLabs/status/2018323584834318419) . Malicious project configurations have achieved[remote code execution in Claude Code](https://research.checkpoint.com/2026/rce-and-api-token-exfiltration-through-claude-code-project-files-cve-2025-59536/) the moment a developer opens a repository. Prompt injection through fetched content can turn a trusted agent into an attacker's proxy. The delivery mechanisms vary. The result is the same: your agent executes something it shouldn't, and it does so with your permissions.


These are just three examples. The pattern is consistent: agents operate with real authority, and that authority is being exploited. Through supply chains, through the agent's own fragility, and through the same malware ecosystem that has targeted every other computing platform before them.


## Where ADR Must Operate


To be effective, ADR must sit as close to the agent's execution loop as possible. Not as an external service watching from the outside. Inside, where the actions happen.


Agent sessions contain source code, credentials, business logic, and personal data. ADR evaluates locally, on the device. No prompts, tool inputs, or code leave your machine for analysis.


Agents are not robust systems. Their own internal mechanisms can silently drop safety instructions or get poisoned across sessions. A security layer that only sees inputs and outputs misses the moment things go wrong inside. ADR watches the execution loop itself: every action the agent attempts, in context, in sequence.


Only a client-side layer sees the complete picture: the prompt history, what files were already modified, the sequence of prior actions. A remote service sees isolated events. ADR sees the session. That difference is what makes it possible to distinguish a legitimate file deletion from a compromised agent cleaning up its tracks.


## Sage: ADR in Practice


In February, we[open-sourced Sage](https://www.gendigital.com/blog/news/company-news/sage-ai-agent-security) as the first implementation of ADR. It runs client-side, directly inside the agent's execution loop, across Claude Code, Cursor, and OpenClaw. Over 200 detection rules cover supply chain attacks, credential exposure, dangerous commands, persistence mechanisms, and more, backed by Gen's threat intelligence.


Since launch,[Sage](https://github.com/avast/sage/) has quickly crossed 1,000 installs and is available across the major agentic tool marketplaces. The open-source community is already contributing, porting Sage to new platforms like OpenCode and hardening detection rules. As one independent security researcher concluded after[testing Sage](https://www.linkedin.com/pulse/paranoid-sage-testing-security-tool-agents-radek-hendrych-capff/) : "Real-time monitoring and management of AI agents based on deterministic measures is, in my view, the right way to go."


All these responses confirm what we suspected: people want this layer.


## What's Next


ADR is the runtime layer. But runtime protection alone isn't enough.


The Agent Trust Hub is built on the idea that trust in the agentic era must be layered: verification before installation, ecosystem trust signals across marketplaces, and enforcement at execution. ADR closes the gap at the moment risk becomes real.


The industry needs a common interface for how agents expose their actions to security tools. A shared standard, so that protection works consistently regardless of which agent you use or which platform you're on. We've been working on that too. More soon.
