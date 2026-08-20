---
schema_version: "1.0.0"
document_id: "b3b9ff45dd812619ba4126ac72ae8ced86c9ece046bbe692309040cca9ba7cd3"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-news-import-4d91977bfa54"
canonical_url: "https://www.gendigital.com/blog/news/company-news/ai-agent-trust-hub-standards"
published_at: "2026-03-05T18:37:46+00:00"
first_seen_at: "2026-07-23T22:48:21.730721+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:d02574ea6ef7962ed29c772999bebf2f0e3cb279930a0aada2daca23d9b0eb62"
---

# Leading the Way for AI Agent Safety

Gen has a family of trusted consumer brands that for decades have helped people stay safe in the digital world. We pride ourselves in the trust we’ve built with people around the world to navigate the latest technology safely and with confidence. And that’s exactly why we recently launched the Gen Agent Trust Hub – our trust layer for the agentic AI ecosystem.


AI agents are no longer theoretical. They are here. They are editing files, installing packages, calling APIs, connecting to MCP servers, spawning sub-agents, persisting memory, and executing real workflows across environments. This shift from passive AI to autonomous execution fundamentally expands the attack surface.


The Agent Trust Hub (ATH) exists to meet this moment. It is designed to provide continuous protection across the full lifecycle of an AI agent – from identity and verification to runtime enforcement. But security at this scale cannot rely on proprietary guardrails alone. It requires shared foundations the industry can build on.


To help establish that foundation, we created the **AI Agent Safety Standards** **by Gen** to serve as a unified framework designed to bring consistency, portability, and accountability to agentic systems. This framework is currently built on two complementary pillars:


- **AI Agent Runtime Safety Standard (AARTS)** – an open standard for runtime decision enforcement across agent hosts
- **Skill IDs** – a deterministic, content-addressable fingerprinting system for AI agent skills


Together, they enable the ATH and the broader agentic AI community to enforce trust not just within one environment but across the broader agentic ecosystem.


Let’s dig into the details.


## **Pillar One: Standardizing Runtime Safety for AI Agents**


### **AARTS**


AI agents operate across a fragmented ecosystem of IDEs, orchestrators, frameworks, and standalone applications. Each host defines its own lifecycle events, security hooks, and enforcement logic.


This fragmentation creates blind spots. A security engine integrated into one host may lack context or control in another, even when the risks are identical.


**AARTS** addresses this gap. It is not a product. It is a vendor-neutral contract that defines:


- Where security decisions can be made (hook points)
- What data is available for evaluation (data model)
- How decisions are enforced (verdict semantics)


Simply put: AARTS standardizes runtime security decision-making across agent hosts.


### **The Architecture**


AARTS defines three cleanly separated roles:


1. **Host**
Runs the agent, emits lifecycle events, and enforces decisions.
2. **Adapter**
Maps host-native events into a standardized AARTS schema.
3. **Security Engine**
Evaluates events and returns structured verdicts.


This separation prevents superficial “checkbox security” while preserving portability. Hosts provide the right context at the right time. Engines make decisions without depending on host internals.


In ATH, this architecture allows enforcement logic to remain portable while enabling hosts to integrate in a predictable way.


### **Hook Points: Where Safety Happens**


AARTS v0.1 defines 19 hook points across the agent lifecycle, including:


- **PreToolUse** — evaluate shell commands, file writes, web requests, package installs
- **PreLLMRequest** — protect prompt integrity and instruction layering
- **PreSkillLoad / PrePluginLoad** — enforce supply chain controls
- **PreMCPConnect** — treat MCP connections as trust boundaries
- **PreMemoryRead / PreMemoryWrite** — protect persistent memory


These hooks enable consistent policy enforcement regardless of whether an agent runs in an IDE, CLI, or orchestrator.


**Verdict Semantics: Allow, Deny, Ask**


AARTS engines return structured verdicts:


- **Allow**
- **Deny**
- **Ask**


Each verdict includes severity, threat category, optional confidence scoring, and human-readable reasoning.


Importantly, AARTS enforces deterministic behavior:


- If a host cannot support interactive “Ask,” it defaults safely to Deny
- If an engine fails, hosts fail-open and log explicitly


These operational tradeoffs are intentional. Predictability is essential in security infrastructure.


### **Why AARTS Matters**


Without a shared runtime contract:


- Security integrations are bespoke and brittle
- Tool taxonomy is inconsistent
- Prompt injection controls vary by host
- Supply chain risk is unevenly enforced


With AARTS:


- One engine can operate across multiple hosts
- Policies remain portable
- Audit logs become comparable
- Trust boundaries are explicit


AARTS standardizes enforcement in the agentic world.


To read the full technical breakdown, see the full AART[technical blog post](https://www.gendigital.com/blog/news/company-news/ai-agent-runtime-security) . To comment on the standards, visit our page on[GitHub](https://github.com/gendigitalinc/aarts) .


## **Pillar Two: Skill IDs**


### **Fingerprinting AI Agent Skills**


AI agents are extended through **skills** – directory bundles containing a SKILL.md prompt file alongside scripts, templates, configuration, and documentation.


These skills function like plugins. Install one, and the agent gains new capabilities.


But skills are not single files. They are directory trees.


And directory trees are notoriously difficult to fingerprint reliably.


### **The Identity Problem**


A naive SHA-256 hash of a ZIP file does not work:


- ZIP files are non-deterministic
- Timestamps vary
- Entry order differs
- Unicode normalization differs across OS platforms
- Wrapper directory names vary


Two identical skills can produce different ZIP hashes.


That is unacceptable for security workflows.


### **Designing the Skill ID**


Skill IDs solve this problem by creating a deterministic, content-addressable identifier for a skill’s logical directory tree.


The algorithm:


1. Extract all files and directories
2. Normalize paths (slashes, Unicode NFC, dot components)
3. Strip wrapper directories
4. Hash each file individually (SHA-256)
5. Build sorted tree entries using null-byte delimiters
6. Hash the tree to produce the final Skill ID


The result is a 64-character SHA-256 digest that changes **if and only if meaningful content changes.**


Same content, different ZIP packaging? Same ID.
One byte changed? Different ID.


This mirrors git’s tree hashing model and decades of security best practices.


### **What Skill IDs Enable**


A stable identity primitive unlocks established security workflows:


**Allowlisting**
Approved skills can bypass repeated analysis.


**Content-addressable storage**
Deduplicate identical submissions automatically.


**Per-file caching**
Reuse expensive AI analysis on unchanged files.


**Detection flagging**
Connect file hashes to reputation systems.


**Version tracking**
Content becomes the version boundary.


Skill IDs provide identity before verdict.


### **From Identity to Authenticity: Signing Skills**


Identity answers “what is this?”


Authenticity answers “who published this?”


We are exploring a signing model inspired by Authenticode:


- A skill.sig file embedded inside the directory
- Excluded from the Skill ID calculation
- Contains a signature over the Skill ID
- Verifiable against the author’s public key


This enables:


- Author signing
- Marketplace attestation
- Certificate chains
- Revocation workflows


Self-verifiable skills create accountability without external lookups.


To read the full technical breakdown, see the full[SkillsID technical blog post](https://www.gendigital.com/blog/insights/research/ai-agent-skill-fingerprinting) . To comment on the standards, visit our page on[GitHub](https://github.com/gendigitalinc/skill-id-standard) .


## **The AI Agent Safety Standards by Gen: Identity + Enforcement**


AARTS governs runtime enforcement.
Skill IDs govern artifact identity.


Together, they form the foundation of the **AI Agent Safety Standards.**


- Skills can be uniquely identified, versioned, and signed.
- Runtime actions can be evaluated consistently across hosts.
- Supply chain trust and runtime enforcement operate under shared semantics.


This is the same model that matured endpoint security over decades:


- File hashes
- Content signatures
- Runtime enforcement
- Portable policy engines


We are applying those proven patterns to a new artifact type and a new execution model.


## **Looking Ahead**


These standards are early. AARTS v0.1 is a draft. Skill ID signing is an evolving proposal. Adapter conformance testing, enterprise fail-open profiles, and cross-marketplace reputation sharing remain active areas of collaboration.


But the direction is clear.


AI agents are moving from experimentation to infrastructure. Security must do the same.


The Gen Agent Trust Hub is our contribution to building a predictable, interoperable trust layer for the agentic ecosystem.


We invite host builders, marketplace operators, enterprise security teams, and fellow security vendors to review, adopt, and challenge.


Because in the agentic era, safety cannot be bespoke. It must be foundational.
