---
schema_version: "1.0.0"
document_id: "c1062ada320d1400044e0a7e8824e809dfa1cc960afe79cd9063f4cd46f1064f"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/product-innovation/agent-gateway-runtime-governance/"
published_at: "2026-07-23T07:00:00+00:00"
first_seen_at: "2026-07-23T19:07:43.019079+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:b557147ce26c49a7fe8bc107899729f30fa692367871c001a718bbdf6f5e3752"
---

# Introducing Agent Gateway: Runtime AI agent governance

### Topics


---


AI Agents


,


Non-Human Identities


,


AI


### Table of Contents


---


---


### Share


-
-
-


---


Ready to make Identity a business advantage? Sign up today.


[Get started](https://www.okta.com/free-trial/)


## The AI security blind spot in your enterprise


The ability to answer three questions determines whether your AI agents are under control:


1. *Where are my agents?*
2. *What can they connect to?*
3. *What can they do?*


For most security leaders, there is no clear answer to the second question.


The reason it's so difficult to answer is that agents executing business-critical work aren't often built internally. Up until now, there’s been no standard way for Claude Code, GitHub Copilot, or Salesforce Agentforce to call Okta for credentials, stay within scope, or log what they do.


The common fallback is long-lived API keys: broad access, no user attribution, no audit trail, one leaked credential away from a breach. And when something goes wrong, you most likely can't give your auditor the key information they need, such as which agent accessed the data, on whose behalf the agent acted, and under what policy it operated.


The result: an increased likelihood of a breach. And when a breach happens, you could face regulatory exposure, failed audits, and the operational cost of manually revoking access across all systems that the agent touched.


Here's what that looks like in practice. A developer needs an agent to access GitHub and Slack. They drop a personal access token and a Slack token into the agent's configuration. Now there's an agent with standing access to source control and internal channels.


The credentials sit in an unprotected file. No one can say which actions were the agent's and which were the human's. If a poisoned prompt compromises that agent, the tokens go with it.


Live credentials have no expiration and no audit trail, making them a breach waiting to happen. Multiply that by each developer and each coding assistant your teams use.


## Why AI agent governance requires runtime identity


Governing agents requires identity enforcement at runtime. Without it, you lose the ability to answer the questions that matter most when something goes wrong.


The most efficient place you can capture key information—such as which agent accessed this data, on whose behalf, and under what policy—is at the moment a tool call executes, not at configuration time, not after the fact. That requires an identity layer in the request path.


## Traditional gateways vs. identity-native gateways


Capability Traditional API/MCP Gateway Identity-Native Agent Gateway


**Primary Focus** Traffic routing, load balancing, and rate limiting Identity verification and policy enforcement


**User Attribution** None (tracks IP/system, not individual users) Maps every call to a specific agent and human user


**Credential Management** Stores or passes static, long-lived API keys Brokers short-lived, isolated tokens dynamically


**Access Revocation** Requires rotating keys across all downstream apps Instant revocation at the gateway endpoint


## How Agent Gateway secures tool calls with your access policies


[Agent Gateway, a new capability](https://www.okta.com/newsroom/articles/okta-july-2026-product-innovations/) within Okta for AI Agents, closes that gap. AI agents can securely access enterprise tools via a single Okta-secured endpoint. Agent Gateway brokers credentials at runtime, attributes tool calls, and requires no code changes to the agent. The result: Security teams can answer their audit questions, and AI teams can ship faster.


Okta is the identity provider in the path: It verifies the agent, controls which tools it can use, holds credentials so the agent cannot touch them, and logs tool calls against a managed identity.


Agent Gateway sits in the path of tool calls your agents make and answers a critical question from the[Blueprint for the Secure Agentic Enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) : What can your agents connect to?


Agent Gateway enforces policy at runtime. But runtime enforcement requires knowing who your agents are, what they should access, and what policies govern them.[Okta for AI Agents](https://www.okta.com/products/govern-ai-agent-identity/) provides the identity and governance foundation by discovering, onboarding, protecting, and governing agents across your enterprise.


### The Agent Gateway architecture


Fill out the form to access this content.


Agent Gateway delivers three critical outcomes:


- **Enforcement:** You control agent access to enterprise tools from one place and can revoke it instantly
- **Security:** Agents hold only short-lived tokens, so adversaries can't exfiltrate downstream credentials
- **Visibility:** All actions are auditable with full attribution across systems


You don't need to own the agent’s code. Agent Gateway delivers vendor-neutral protection across platforms and clouds, including Claude Code, Cursor, GitHub Copilot, Salesforce Agentforce, and any agent your teams can point at an MCP endpoint.


## Integrating Agent Gateway into your existing infrastructure


Agent Gateway doesn't replace your existing MCP or API gateway. Instead, it adds an identity and policy layer on top of your current infrastructure.


Your existing gateway continues to handle routing, rate limiting, and connectivity. Agent Gateway handles identity enforcement.


Your teams point their agent clients at the Agent Gateway endpoint. No code changes are required for agents, and no modifications are needed to downstream systems. The agent makes a call and gets a response. What changes is that all tool calls now flow through Okta policy.


### Supported credential patterns


Agent Gateway currently supports two credential patterns:


- **[Cross-App Access](https://www.okta.com/solutions/cross-app-access/) (XAA):** The new protocol enabling secure agent-to-app access for Okta-enabled resources
- **Brokered consent:** Okta’s Secure Token Service (STS) dynamically brokers OAuth consent for external systems like GitHub and Slack


The agent's experience is identical in both cases.


## Secure on-premises requirements with MCP Bridge


Organizations with strict regulatory requirements—such as FedRAMP compliance, private networks, or data residency constraints—can deploy MCP Bridge.


Available through[Okta Professional Services](https://www.okta.com/services/professional-services/) , MCP Bridge provides similar identity enforcement and credential isolation within your own infrastructure.


## How to get started with Agent Gateway today


If your security team reviewed last week’s AI agent tool calls, could they determine which agent acted, on whose behalf, what data it accessed, and whether any credentials were exposed?


A traditional MCP gateway can route tool calls, but it can’t provide the identity context needed to answer those questions.


An identity-native gateway can. It shows which agent accessed which data, for whom, and under what policy, at the moment each call executes.


**Be among the first to test Agent Gateway.** We're offering this new Okta for AI Agents capability as a research release. To submit your interest,[apply to join our Research Partner Program](https://greatquestion.co/oktainc/wm8ztony) .


Any mention of future products, features, functionalities, or certifications in this blog is for informational purposes only. These items are not commitments to deliver and should not be relied upon to make purchasing decisions.


About the Author


[Srujana Puttagunta Staff Product Marketing Manager, Okta for AI Agents Srujana leads product marketing for Okta's AI agent identity security solution. She's passionate about demystifying AI agent security and helping organizations confidently deploy autonomous systems without compromising on trust, governance, or control. Through thought leadership and industry engagement, she ensures that enterprises understand why securing AI agents is critical to their business.](https://www.okta.com/blog/author/srujana-puttagunta/)
