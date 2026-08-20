---
schema_version: "1.0.0"
document_id: "1eac91be8fb23297e48b303753714ff2d49639b5ce712db4112cdbeb7259138c"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-news-import-4f455b4bb3a3"
canonical_url: "https://jfrog.com/blog/ai-agents-jfrog-skills-mcp-tools/"
published_at: "2026-04-21T15:45:19+00:00"
first_seen_at: "2026-07-27T12:11:59.089588+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:81950187d10e1a39d20361985b5bf7d55957a075bf0da542a1826fc7a89b1bf4"
---

# Unlock the Power of Agents with JFrog’s Skills and MCP Tools

Agents are writing code, suggesting dependencies, and reviewing PRs, without any knowledge about your trusted package sources, security posture, or governance policies. When agents operate without supply chain context, they introduce risk, create rework, and weaken the guardrails DevSecOps teams rely on to ship with confidence.


JFrog is changing that. Today, we’re launching an official set of[JFrog Platform Skills](https://github.com/jfrog/jfrog-skills) and expanding our suite of[MCP tools](https://docs.jfrog.com/integrations/docs/jfrog-mcp-server-tools) and plugins to give autonomous agents full visibility into your JFrog-managed software supply chain. Every action your agent takes is now grounded in enterprise context, ensuring smarter, safer, and more sustainable software delivery.


## What Agents Can Do with Supply Chain Context


With JFrog’s Skills, MCP tools, and plugins, your agents can actively participate in DevSecOps workflows analyzing, validating, and acting across your software supply chain. You ask in natural language and the agents use JFrog’s artifact, security, and governance data to take secure and compliant actions automatically.


Here are some of the high-impact workflows that become possible:


### Secure and Manage Your Software Supply Chain


Enable agents to proactively secure your pipelines by querying JFrog’s security and curation data before vulnerable or tampered artifacts enter builds or production. Agents gain full visibility into CVEs, provenance, checksums, and build origins, shifting security left without slowing delivery. You get faster, safer releases with security built in at every step, not bolted at the end.


**Ask your agent:**


- *Tell me about CVE-2021-44228*
- *Which packages in libs-release-local have critical CVEs? Check curation status for the top 3 and whether they have been downloaded despite the vulnerability.*


### Ensure Governance and Compliance


Let your agents keep you in check. Surface curation status, flag license risks, pull audit events, and identify packages that were downloaded or used despite being flagged. Every action is automatically aligned with your organization’s policies and governance stays intact even as automation scales.


**Ask your agent:**


- *Is commons-compress 1.21 approved?*
- *Curation audit events from the last 7 days*


### Trace Builds and Verify Provenance


Agents can trace exactly where every artifact originated, verify its integrity, and surface the full chain of custody when something looks wrong. They can instantly retrieve build origins, VCS commits, and checksum verification, accelerating root cause analysis without manual log digging, so developers can stay focused on building and shipping.


**Ask your agent:**


- *Which build produced payment-service-1.4.2.jar? Show me build info and VCS commit.*
- *Verify that ./lib/my-artifact.jar has not been tampered with. Check it against Artifactory and show me its build provenance.*


### Optimize Storage and Supply Chain Costs


Need to understand what team, project, or artifacts are driving usage and storage costs? Agents can keep your artifact ecosystem lean by identifying stale or oversized artifacts, detecting SNAPSHOT buildup, and highlighting cleanup opportunities before storage costs grow out of control.


**Ask your agent:**


- *Show me the largest files across all local repositories*
- *Find artifacts in libs-release-local not downloaded in the last 3 months, larger than 1MB. Flag any SNAPSHOT buildup.*


## BYOA – Bring Your Own Agent: Your Agents, Your Way


JFrog enables a BYOA approach by providing multiple secure and flexible ways to bring supply chain intelligence to your agents. Use our skills or MCP tools individually or combine them, depending on what your team needs.


### JFrog’s Platform Skills Integrate into Your AI Eco-System


We’ve created detailed skills that provide your coding agents deep, domain-specific knowledge of the JFrog Platform so you can simply ask what you need in natural language and the right action happens.


Skills are open source and can be installed into your preferred coding agents, including Cursor, Claude Code, or others with a single[command](https://github.com/jfrog/jfrog-skills) .


Every request is routed through the JFrog CLI or the expanding suite of MCP tools, ensuring that responses remain accurate, contextual, and fast.


### JFrog’s MCP Server


If you prefer to access JFrog Platform capabilities via MCP, our[MCP Server](https://jfrog.com/blog/introducing-jfrog-mcp-server/) is already available. Additional tools and capabilities are added to the JFrog MCP Server on an ongoing basis. Check our[docs](https://docs.jfrog.com/integrations/docs/jfrog-mcp-server-tools) for the latest.


Enable in the JFrog Platform UI under: **Platform** → **Integrations** → **Tools & Integrations** → **MCP Server** and follow the[docs](https://docs.jfrog.com/integrations/docs/enable-the-jfrog-mcp-server) to get started.


### JFrog’s Plugin for Agents


For teams that want plug-and-play simplicity, JFrog’s plugin for coding agents bundles JFrog’s Platform Skills and MCP tools into a zero-configuration package. Authenticate once via OAuth, and your agent is immediately aware of your supply chain, no CLI setup or token management required.


Coming soon for Claude Code, Cursor, VSCode with Copilot, and more coding agents. The plugin will also include native support for the[JFrog MCP Registry](https://jfrog.com/ai-catalog/mcp-registry) , giving organizations the ability to discover, govern, and control which MCP servers are approved for use across their teams.


## What’s Coming Next


Agentic DevSecOps is here. Agents are not just writing code, they are actively managing builds, enforcing governance, strengthening security, and optimizing your supply chain with full awareness of your environment.


New[Skills](https://docs.jfrog.com/integrations/docs/jfrog-skills) ,[MCP tools](https://docs.jfrog.com/integrations/docs/jfrog-mcp-server-tools) and plugins ship every week, bringing more capabilities to your agents as the platform evolves.


Ready to see it in action? Join our upcoming webinar:[Agentic DevSecOps Workflows with JFrog](https://leap.jfrog.com/WN-MoFu-Bin-26-05-Agentify-Your-DevSecOps-CS-AM-lp.html)
