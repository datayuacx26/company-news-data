---
schema_version: "1.0.0"
document_id: "411339f5135e37efcfd6bd259aef50ececc69db5ff800e63dc82a217aba80f0e"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-f76a50baf54b"
canonical_url: "https://www.okta.com/newsroom/press-releases/okta-becomes-a-featured-identity-provider-powering-secure-ai-agent-connections-for-claude-enterprise/"
published_at: null
first_seen_at: "2026-07-22T07:05:39.149088+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:09209e120be3a7ccd09b9234229e8d8465a62532a277c166931b97b1c085a5ca"
---

# Okta becomes a featured identity provider powering secure AI agent connections for Anthropic’s Claude

Today,[Anthropic announced Okta as a featured identity provider](https://claude.com/blog/enterprise-managed-auth) supporting their beta program, which enables joint customers, including Ramp, Webflow, Hubspot and others, to leverage Okta to help govern their use of Claude and access to applications from participating MCP providers including Asana, Atlassian Canva, Figma, Granola, Linear, and Supabase.


The beta program demonstrates how secure Claude connections can be established when AI tools and downstream resource applications standardize around[Cross App Access (XAA)](https://www.okta.com/solutions/cross-app-access/) . This marks an important milestone for XAA,[an open protocol led by Okta](https://www.okta.com/newsroom/press-releases/okta-introduces-cross-app-access-to-help-secure-ai-agents-in-the/) that extends OAuth to secure agent-to-app and app-to-app access and is an official authorization extension within MCP listed under the name “Enterprise Managed Auth.”


With Okta as the governance layer, customers can apply the model to Claude Enterprise workflows. This enables IT admins to authorize MCP connectors once for their organization, scope access by Okta groups or roles, and revoke access through Okta when users or deployed agents are offboarded.


Key XAA capabilities provided to participating customers, and planned to be available more broadly, include:


-


**Centralized authorization:** Admins provision and authorize MCP connectors for their organization once, helping reduce fragmented agent configurations and repetitive consent prompts.


-


**Robust access control:** Users and their AI agents inherit access to specific MCP connectors based on the existing Okta groups and roles they already belong to.


-


**Automated offboarding:** Offboarding flows through Okta's standard revocation path. When a user is deactivated or their agent’s role changes, their access to MCP connectors is quickly revoked alongside their other enterprise application permissions.


“The industry has seen that when technology ecosystems grow quickly, open standards become critical to helping them scale securely,” said Ely Kahn, Chief Product Officer, Okta. “Okta first championed Cross App Access to give organizations a common way to secure AI agent connections, and continuing that work with Anthropic and other partners marks a significant milestone in the journey to broad industry adoption. Together, we’re helping drive ecosystem alignment around the standards shaping the AI era.”


“Enterprise-managed auth gives MCP the foundation it needs to scale across an enterprise, with Okta as our first identity provider partner,” said Mayank Malhotra, Product, Anthropic. “When an admin authorizes a connector once for the whole organization, every employee gets instant access to more of their tools through Claude, governed by the IDP they already trust. We invite MCP developers to support enterprise-managed auth so their connectors are enterprise ready on day one."


### Where It Began: XAA Extends MCP with Enterprise-Grade Authorization


Enterprise AI workflows are becoming more tool-rich, a shift driven in part by MCP. AI agents work across hundreds or thousands of tools, from developer systems and deployment pipelines to messaging, project management, documentation, and company databases.


Yet agent access too often relies on static credentials, unmanaged consent prompts, or one-off approvals that are difficult for IT and security teams to manage. Every connection needs to be visible, authorized, and scoped while the user experience remains seamless.


In June 2025, XAA was introduced to help solve this challenge: extending identity governance to AI agents and applications as they connect to business tools, data, and systems, and improving the end-user experience by reducing repetitive authorization consent screens.


With XAA, organizations can bring agent-to-app and app-to-app access inside the identity perimeter by making the identity provider the control point. Instead of users handling the connection between an AI tool and a business application, the tool can request access through the identity provider. Okta, for example, can evaluate the request against enterprise policy, issue a scoped token when appropriate, and log the access event.


In September 2025, XAA was adopted by the OAuth working group, and in[November 2025 was incorporated within MCP](https://www.okta.com/newsroom/articles/cross-app-access-extends-mcp-to-bring-enterprise-grade-security-to-ai-agents/) to provide the missing authorization layer. This extended XAA’s reach to power identity and authorization across the entire ecosystem of MCP-connected AI tools.


### What’s Next: A Broader Commitment to Secure AI Deployment


This XAA work addresses one of the core questions in the[blueprint for the secure agentic enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) : what can agents connect to? However, Okta’s broader work with Anthropic helps organizations further advance their implementation by addressing where agents are. Okta for AI Agents can now import Claude Managed Agents and register them within Universal Directory, enabling organizations to assign human owners and enforce centralized policies.


Okta is also supporting secure enterprise AI deployment by securing the identities that interact with these tools. Through[an integration between Okta Identity Security Posture Management and the Claude Compliance](https://www.okta.com/newsroom/articles/okta-identity-security-posture-management-integrates-with-anthropics-new-compliance-api/) API, security teams gain centralized visibility and the ability to remediate identity risks, dormant accounts, and misconfigurations within Claude Enterprise.


To strengthen Okta’s infrastructure that supports AI systems, Okta is part of[Anthropic’s Project Glasswing](https://www.anthropic.com/glasswing) , leveraging Claude Mythos Preview to accelerate vulnerability discovery.


*Any mention in this article of solutions, features, functionalities, certifications, authorizations, or attestations that are not currently generally available or have not yet been obtained may not be delivered or obtained on time or at all. We assume no obligation to deliver on such items and you should not rely on them to make your purchase decisions.*


About Okta


Okta


Okta, Inc. is The World’s Identity Company™. We secure AI, machine, and human identity so everyone is free to safely use any technology. Our customer and workforce solutions empower businesses and developers to protect their AI agents, users, employees, and partners while driving security, efficiencies, and innovation. Learn why the world’s leading brands trust Okta for authentication, authorization, and more at[okta.com](http://okta.com/) .
