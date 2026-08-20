---
schema_version: "1.0.0"
document_id: "8904b3abef42b38b0e6f831d3fa55ce98ec6d40c1c5c07cc393f2836b159e368"
company_key: "yc-dynamo-ai"
company: "Dynamo AI"
source_id: "yc-dynamo-ai-news-import-ae057c02df24"
canonical_url: "https://www.dynamo.ai/blog/introducing-the-first-of-dynamo-agentwardens-capabilities-for-agentic-ai-security-automated-risk-detection-and-evaluation-for-ai-agents"
published_at: "2025-11-07T00:00:00+00:00"
first_seen_at: "2026-07-25T02:05:58.090330+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:d528ebaabcd1ee81fdbc71d88490e1472bfe6d6e0fa18dcd43121732429d642c"
---

# Introducing the First of Dynamo AgentWarden’s Capabilities for Agentic AI Security: Automated Risk Detection and Evaluation for AI Agents

Today, Dynamo introduces the first capability of **AgentWarden** , **a breakthrough line of evaluation and guardrail products to comprehensively secure AI agents.** AgentWarden’s **automated risk detection and evaluation capabilities** continuously evaluate the safety of agentic systems through holistic analysis of MCP tools accessible to agents. Generate automated reporting within 5 minutes to quickly diagnose and remediate key agentic AI risks. By automatically detecting data exfiltration and prompt injection risks, AgentWarden’s evaluation capabilities enable teams to scale AI agents with confidence, maximizing adoption while preserving security and compliance.


## Realities of the Agentic AI Frontier: Accelerated Productivity, Expanded Risks, Strained Oversight


At Dynamo, we empower organizations to harness AI confidently—by identifying, measuring, and mitigating emerging risks, bridging the gap between innovation and governance with real-time security and compliance controls.


While AI agents are actively accelerating enterprise productivity, they also expand the surface area of an organization’s security and privacy risks from **(a) external prompt injection and exfiltration attacks, (b) internal compliance and governance standards,** and **(c) unintended, misaligned autonomous behaviors.**


Existing tools, controls, and procedures can’t fully monitor and protect against these novel vulnerabilities. Compliance and security leaders face new, fast-moving risks as agents interact with multiple MCPs, accessing data and systems across shifting boundaries, often without a human in the loop.


The problem is both visibility and actionability. Security teams need a clear view of agent behavior and guidance on how to control the risks they find. But today, even the most capable teams still can’t answer questions such as:


- *Can we safely approve this MCP?*
- *What are the potential pathways for data exfiltration, prompt injection, or misaligned agent actions?*
- *How do we maintain confidence in our controls and systems as MCP tools evolve?*


These concerns are justified. Real-world attacks have shown how tool-using agents can exfiltrate data accidentally or be manipulated via prompt injection. Each new integration compounds complexity, creating dynamic trust boundaries that simple evaluations don’t capture.


When agents combine **(1) access to private data, (2) exposure to untrusted content,** and **(3) the ability to communicate externally** , to create a “Lethal Trifecta” of AI risk, the potential for harm compounds. This pattern has been shown across Microsoft 365 Copilot, GitHub's official MCP server, GitLab Duo, Slack AI, and dozens of other production systems.


## How AgentWarden Detects and Maps Risks


Enter **AgentWarden's automated risk detection capabilities: evaluate the safety and security of agentic AI systems through holistic analysis of MCP tools accessible to agents.** Dynamo’s AgentWarden maps agent trajectories and MCP tool use to concrete attack paths, continuously evaluates them against risk scenarios, and recommends runtime safeguards. In turn, **organizations gain a comprehensive view into the key privacy, security, and compliance risks presented by enterprise agentic systems** , turning opaque agent behavior into transparent, traceable risk intelligence.


**AgentWarden provides an x-ray scan across enterprise agentic use of MCPs, diagnosing otherwise undetectable vulnerabilities and providing recommendations for remediation so that agentic use cases drive value rather than risk.**


In the example above, we pry open the black box of one of the most frequently cited enterprise use cases of AI agents: helping software engineers resolve customer tickets.


1. The first step in the agentic trajectory is for the agent (e.g., Cursor, GitHub Copilot, Claude) to access the details of the customer issue. To do this, the AI agent requests metadata from the GitHub MCP server.


1. It’s here that AgentWarden detects the first of element of the Lethal Trifecta: an **Untrusted Input.**
2. In this example, the external customer ticket contains a malicious prompt injection that is seeking to utilize the AI agent to leak sensitive data. GitHub’s MCP server does not understand this malicious threat and passes the customer ticket information (including the prompt injection) to the AI coding agent.


2. The AI coding agent proceeds with faithfully carrying out the task given the details. The smarter the agent, the more effective it is at carrying out tasks. **Unfortunately, agent intelligence is not correlated with understanding of cybersecurity tasks. Oftentimes, the smarter the agent, the more dangerous the risk, as the agent is capable of doing more (harm).**


1. Here, AgentWarden detects the second component of the Lethal Trifecta: **Access to Private Data** . The coding agent decides it needs to access private data in order to resolve the customer ticket. It accesses a private database and secret API keys from the local environment.
2. Finally, AgentWarden detects the third Legal Trifecta factor: **Ability to Communicate Externally** . The coding agent completes the task by using the GitHub MCP server to expose the sensitive API keys to malicious actors, following the directions of the malicious prompt injection.


For each agent and MCP combination, AgentWarden’s evaluations answer the following, **to paint an accurate picture of the amplified and new risks brought on by AI agents:**


- Measure the severity of risk from this agent using this combination of MCP tools?
- How likely do these vulnerabilities or attacks occur in practice? This consists of an analysis of agent trajectories and logs.
- Exactly how would it happen? This includes clearly illustrated attack-path scenarios.


Importantly, Dynamo’s AgentWarden team successfully conducted this attack using GitHub’s MCP and Cursor to leak secret API keys. See on overview of the attack[here](https://screen.studio/share/vWCF6kh7) , as well as how AgentWarden detected and mitigated this risk[here](https://screen.studio/share/l6mRGf7r) .


While GitHub’s MCP server and Cursor agents claim that they already have protections in place to detect and stop secret leakage, the AgentWarden team conducted these sample attacks and evaluations in September 2025, *after* GitHub and Cursor claimed they already fixed the issue. It took fewer than 2 hours for Dynamo’s AgentWarden team to conduct this successful attack.


**With the AgentWarden product and platform, enterprises can conduct automated risk evaluations and generate comprehensive reports within 5 minutes.**


## AgentWarden's Risk Evaluation Suite


AgentWarden's evaluation capabilities produce comprehensive qualitative and quantitative results so organizations can diagnose key risks and vulnerabilities of agents and MCP tools.


Key Qualitative Results Uncovered include:


- Tagging of each MCP tool and its risks: access to private data, exposure to untrusted content, and ability to communicate externally
- List of prioritized trajectories (combinations of agent MCP tool use) that lead to highest severity risks
- Recommendations for remediation, for each MCP tool


Key Quantitative Results Analyzed include:


- Frequency that regular usage of AI agents invokes a high-risk lethal trifecta: Histogram of frequency of tool combinations, depicting the severity of risk and the frequency of use from analyzing agentic trajectories
- Key Data, include the number of tools from MCP servers analyzed, the amount of high-risk combinations overall, how many high-risk combinations are present in agentic trajectories
- The percentage of high-risk tools within an MCP: a heatmap of where the vulnerabilities reside across MCP servers


## Securing the Future of Agentic AI


Unlocking agentic AI at enterprise scale requires that organizations gain a full picture of their agent’s capabilities and risks. **AgentWarden’s evaluation capabilities provide that exact window into an otherwise murky and hard-to-monitor ecosystem of increasingly independent, intelligent tools.**


AgentWarden’s evaluation capabilities launch with full support for the following agents and MCP servers:


- AI Agents: Cursor, Github Copilot, Codex, Claude Code, and in-house agents.
- MCP servers: GitHub, ServiceNow, Microsoft Outlook, Atlassian Jira, Confluence, Slack, Salesforce, Databricks, Snowflake, HubSpot, Figma, in-house MCP servers.


As AI agents reshape enterprise workflows, AgentWarden helps organizations innovate with confidence and security —anchored in visibility, control, and trust.


**And AgentWarden won’t stop here: stay tuned for upcoming announcements about AgentWarden’s custom guardrail and real-time observability capabilities.**


To learn more,[schedule a demo](https://share-na2.hsforms.com/1zzN5S83LRKuYWPM6T6097gqol04) today!
