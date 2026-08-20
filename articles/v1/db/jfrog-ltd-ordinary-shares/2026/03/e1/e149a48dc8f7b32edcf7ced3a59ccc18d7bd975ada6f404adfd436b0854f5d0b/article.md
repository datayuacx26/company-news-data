---
schema_version: "1.0.0"
document_id: "e149a48dc8f7b32edcf7ced3a59ccc18d7bd975ada6f404adfd436b0854f5d0b"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-news-import-4f455b4bb3a3"
canonical_url: "https://jfrog.com/blog/agent-skills-new-ai-packages/"
published_at: "2026-03-16T20:26:07+00:00"
first_seen_at: "2026-07-27T12:11:59.089588+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:9aee935ecb86ba625e955b544e1aa14297eb66bcc537d799b12701bcda22da3a"
---

# Agent Skills are the New Packages of AI: It’s Time to Manage Them Securely

Let’s talk about **agent skills** . As the AI agent ecosystem matures, we’re seeing a major shift in how users equip agents to run automated workflows. While robust protocols such as MCP exist to handle complex system integrations and authentication, skills have emerged as the go-to, low-friction way to shape an agent’s day-to-day behavior.


Skills are extremely easy to adopt. In many cases, they are simply lightweight files that orchestrate scripts and commands. This simplicity makes them the fastest way to get an agent up and running, and delivering value.


## From Organizational Know-How to Agent Skills


At their core, skills are reusable, file-system-based units of knowledge that equip an agent with domain-specific expertise. They provide the workflows, deep context, and best practices needed to turn an agent from a generalist into a specialist.


This is what makes skills especially valuable for enterprises. They allow organizations to equip agents with institutional know-how by codifying how tasks should be handled across different domains. In doing so, they embed the company’s operational DNA directly into the instructions that guide the agent’s behavior.


As a result, we’re seeing rapid adoption. Enterprises are increasingly taking community open-source skills, adapting them to their own workflows, and building proprietary skills in-house to capture their unique expertise.


Unlike chat-level prompts, skills are loaded on-demand, eliminating the need to repeatedly include the same context and instructions across multiple conversations and waste unnecessary tokens. To achieve this and manage context efficiently, skills use progressive disclosure, keeping agents fast while allowing them to follow the skill’s instructions and optionally load referenced files or execute bundled code as needed.


## The Wild West of Agent Skills


But here’s the catch: skills can run scripts and execute instructions that may cause damage, or at the very least, violate an organization’s compliance standards. In many cases, a skill runs with the same privileges as the user or process controlling the invoking agent. That means access to sensitive data and the ability to perform potentially harmful operations is part of the reality.


Trust in skills isn’t a nice-to-have; it’s a necessity.


On top of that, most organizations today lack a **single source of truth (SSoT)** for these skills. As a result, we are starting to face the same challenges we encounter with open-source software (OSS) packages. Questions that used to apply to libraries and dependencies now apply to skills as well:


- Who actually wrote this skill?
- What version is this, and is there a newer, better, or safer one available?
- Are there malicious prompts or vulnerable instructions hidden inside?
- Does the skill require excessive access permissions?


As skills become a core building block for enterprise agents, these questions move from being theoretical to operational. Organizations need clear visibility into what skills are being used, where they come from, and whether they can be trusted. Without that, scaling agents safely becomes exceedingly difficult, bordering on impossible.


## Where are We Getting Our Skills from Today?


Today, the tools and processes for hosting and distributing skills are highly fragmented. Pulling skills directly from[GitHub](https://github.com/) works well for quick experimentation, but it becomes nearly impossible to govern at scale.[Vercel](https://skills.sh/) offers a frictionless experience for individuals, yet it lacks enterprise capabilities such as strict version control and clear ownership.


[ClawHub](https://clawhub.ai/) represents a significant step forward. As a superset of[Agent Skills](https://agentskills.io/home) , it adopts proven open-source repository standards and brings them into the agent ecosystem. It provides built-in versioning, rich metadata, and a comprehensive API that makes skills easier to manage and integrate.


The broader reality is straightforward. **Open-source skills are becoming the community packages of the emerging agent ecosystem. As AI systems grow more autonomous and complex, this creates a new layer of risk.** Unpredictable behavior, weak governance, limited version control, and the absence of scanning for malicious instructions can quickly turn into serious security concerns.


## The Solution: A Dedicated Organizational Skill Registry for the Enterprise


Once you recognize that skills experience the same growing pains as open-source packages, it becomes clear that they require the same level of control, visibility, and security. This is exactly why JFrog created the **[Agent Skills Registry](https://docs.jfrog.com/artifactory/docs/skills-repositories)** .


Fully compatible with[Agent Skills](https://agentskills.io/) ,[ClawHub](https://clawhub.ai/) , and[OpenShell](https://build.nvidia.com/openshell/) the registry allows organizations to centrally manage and govern the skills used across their environments. It brings enterprise-grade security and control while preserving the frictionless experience developers expect. Further validating that secure skill management is non-negotiable for the enterprise, NVIDIA now features our Agent Skills Registry within their AI-Q reference architecture.


### How JFrog’s Agent Skills Registry is different


At this point, you might be wondering: *Don’t coding agents like Claude Code or Cursor already provide private marketplaces to manage skills?* While these platforms do offer some built-in capabilities, they come with an important limitation: Their skill management is tightly coupled to their own development environments.


This means that to benefit from their features, teams must consume and manage skills through proprietary marketplaces that operate in isolation from the rest of the organization’s tooling. If you decide to switch coding agent vendors, you’re also forced to rebuild your entire skills system of record from scratch, recreating skills and re-establishing governance in the new ecosystem.


For engineering teams operating at scale, this sparks several challenges:


- You cannot easily enforce that **Project A uses v1 of a specific deployment skill while Project B uses v2** .
- Skills cannot be shared seamlessly across different coding agents, automation tools, or CI/CD pipelines.
- Governance becomes fragmented across multiple developer environments, creating invisible security gaps that leave the organization vulnerable.


The result is a classic walled garden. It may solve a short-term convenience problem for individual developers, but it introduces a long-term governance challenge for the organization.


Unlike proprietary marketplaces that isolate your workflows, the Agent Skills Registry from JFrog provides a unified repository designed to scale across every tool and environment in your stack. It delivers the freedom to innovate with AI without sacrificing the centralized control the enterprise demands.


## A Closer Look at How Skills are Published and Installed


Let’s dive into the core workflow to see how you can easily and securely publish and install skills. You can do this via the JFrog CLI, or by using a dedicated JFrog skill that teaches your agent how to publish and install skills the safe way!


**Or just use our pre-built skill for skills publishing and let the agent do the work for you.**


### Phase 1: Publish – scan, sign, and secure


The publishing process starts with building your Skill the standard way. Once you’re ready to distribute it, all you have to do is run:


```text


# Using environment variables (optional for provencance evidence)
export EVD_SIGNING_KEY_PATH=./private.key
export EVD_KEY_ALIAS=my-evd-key


jf skills publish org-coding-skill/
jf skills publish teamA_commit_style_skill/ --repo teamA-skills-repo


```


*If no signing key is provided, the upload succeeds but evidence creation is skipped.*


**Under the Hood:** This command does more than simply upload the skill. It acts as the gateway to your secure supply chain. When triggered, it initiates several key security and integrity steps:


#### **1. Advanced Scanning**


Our two-stage scanning engine first pre-processes the skill’s dependencies and binaries. It then escalates from rapid behavioral triage to deep, multi-tool verification. This layered approach enables accurate detection of malicious code, compromised packages, command and control behavior, and prompt poisoning attempts, while minimizing false positives.


Once the scan completes, JFrog automatically generates an **evidence entity** , an *in-toto compliant attestation* within the JFrog Platform that captures the scan results. This record guarantees that the skill was genuinely scanned by our system and that the results have not been tampered with.


#### **2. Provenance Generation (Optional)**


If the scan passes, the skill is added to the[skill repository](https://jfrog.com/learn/ai-security/agent-skills-repository/) . However, scanning alone is not enough. You also need to prove the skill’s origin.


To accomplish this, a cryptographically signed **provenance evidence entity** is generated and stored alongside the skill in the repository. This attestation verifies the integrity and origin of the skill’s content and enables validation at install time.


All metadata is being taken from the frontmatter:


Example:


```text
---
name: pdf-processing
description: Extract text and tables from PDF files, fill forms, merge documents.
license: Apache-2.0
metadata:
author: example-org
version: "1.0"
---
```


### Phase 2: Install – zero-trust consumption


Now that the skill is safely scanned, signed, and stored in the repository, it’s ready for the team to use. Just like publishing, consumption is incredibly simple—either via our dedicated JFrog Agent Skill or directly through the CLI.


#### **1. Discovering Skills** (` jf skill search` )


Before you can install a skill, you need to find the right one. No more pinging your team in Slack, ***“Hey, does anyone have an agent script for deployment?”***


**Under the Hood:** When you search for a relevant word, the CLI queries the central repository behind the scenes. It scans through metadata, tags, and descriptions of all approved skills in your organization and instantly returns a list of relevant results. You see exactly what’s available, who owns it, and what the latest version is.


#### **2. Secure Consumption** (` jf skill install <SKILL_NAME>` )


Once you’ve found what you need, pulling it into your environment is just one command away:` jf skill install frogs-best-skill --repo skills-rnd-local
`


## Autonomy Requires Control: Get Started Today


Agent-driven workflows are rapidly becoming part of how organizations build and deliver software. Skills unlock powerful new capabilities, but without proper management they risk creating a new class of unmanaged dependencies. Left unchecked, this can evolve into a new form of “Shadow AI” that quickly turns into a significant security and governance liability.


Enterprises need to treat skills as governed, **first-class software assets** , with clear visibility, version control, and security oversight. The JFrog Agent Skills Registry, part of JFrog AI Catalog, provides the foundation for managing this new layer of the software stack, enabling organizations to scale autonomous systems without sacrificing control, compliance, or security.


### How to Get Started


1. **Install the JFrog CLI:** Get the[latest version](https://jfrog.com/getcli/) to access the new` skill` commands.
2. **Explore the Docs:**[Check out our documentation](https://docs.jfrog.com/artifactory/docs/skills-repositories) to see how to configure your first skill repository.
3. **Collaborate:** Start sharing and collaborating on verified skills with your colleagues safely.


The future of software is autonomous, but it shouldn’t be unmanaged. With the JFrog Agent Skills Registry, you aren’t just building faster; you’re building a foundation of trust that allows your AI agents to scale as fast as your ambition.
