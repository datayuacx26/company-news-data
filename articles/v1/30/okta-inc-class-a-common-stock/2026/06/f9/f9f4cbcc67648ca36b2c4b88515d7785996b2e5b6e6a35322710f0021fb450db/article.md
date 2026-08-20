---
schema_version: "1.0.0"
document_id: "f9f4cbcc67648ca36b2c4b88515d7785996b2e5b6e6a35322710f0021fb450db"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-f76a50baf54b"
canonical_url: "https://www.okta.com/newsroom/press-releases/okta-announces-cross-app-access-partners/"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-22T07:05:39.149088+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:3f941aba631263da9a15436c1e58a619882048143a50945e8f09e25768501f52"
---

# Okta advances the industry standard for secure AI agent connections with expanding Cross App Access ecosystem

### Table of Contents


---


---


### Share


-
-
-


---


Ready to secure your AI agents?


[Learn how](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/)


*Expanding Cross App Access ecosystem features the industry’s most critical applications and brings agent-to-app and app-to-app connections within Okta’s identity perimeter.*


*25+ early adopters include Anthropic, Aquera, Archestra.AI, Asana, Atlassian, Canva, Cloudflare, Cursor, Datadog, Docker, Figma, Glean, Granola, Keycard, Keycloak, Linear, MintMCP, Scalekit, Serval, Slack, Stytch by Twilio, Supabase, VS Code, WorkOS, Zoom, and Zuplo.*


As organizations increasingly deploy AI agents to drive productivity, they face a critical challenge: most connections still rely on high-risk static API keys and user consent screens invisible to the IT admin. This introduces permanent standing privileges and gaps in security, forcing IT to either accept unmanaged risk or stall AI adoption entirely.


Today, Okta is announcing an expansion of its[Cross App Access](https://www.okta.com/solutions/cross-app-access/) (XAA) ecosystem to enable customers to help securely manage AI agent-to-app and app-to-app connections within the identity perimeter. Through 25+ new XAA integrations, Okta is providing a standardized way to govern how AI agents connect to enterprise applications and resources, helping ensure every connection flows through centralized identity policy, every action is logged, and access is tightly scoped.


Built as an extension of OAuth and[formally incorporated as an official MCP authorization extension](https://www.okta.com/newsroom/articles/cross-app-access-extends-mcp-to-bring-enterprise-grade-security-to-ai-agents/) , XAA is an open, vendor-neutral protocol,[initially led by Okta](https://www.okta.com/newsroom/press-releases/okta-introduces-cross-app-access-to-help-secure-ai-agents-in-the/) , that reduces enterprise blind spots by allowing identity security to follow agents dynamically across applications.


Fill out the form to access this content.


Implementing XAA is a core part of the[blueprint for the secure agentic enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) —a broader industry framework built on knowing exactly where agents are, what they can connect to, and what they are authorized to do. By anchoring these workflows in Okta's identity and policy engine, XAA delivers three universal benefits:


-


**Improved IT visibility & governance:** Provides a centralized audit trail of actions an AI agent or integrated app takes across an organization’s footprint.


-


**Robust access control** : Removes standing privilege with identity-based tokens scoped only to what the agent needs.


-


**Frictionless user experience:** Helps eliminate repetitive consent prompts that can lead to cognitive fatigue and increase the likelihood of users approving high-risk permissions without proper scrutiny.


Fill out the form to access this content.


Beyond improving the experience for IT administrators and users, XAA enables engineering and AI leaders to safely deploy agents at scale without lengthy security reviews or building custom governance infrastructure.


“With AI agents becoming increasingly core to daily workflows, organizations are aligning around XAA as the secure path to deploy agents in production,” said Ely Kahn, Chief Product Officer, Okta. “This is a powerful reflection of Okta’s commitment to driving open, vendor-neutral standards. By partnering across a vibrant ecosystem of agents, apps, and developer platforms to anchor these automated workflows in identity, we help ensure corporate trust seamlessly follows every agent, safely unlocking the next wave of enterprise productivity.”


### Industry leaders drive adoption of XAA to build a secure AI ecosystem


This announcement is supported by strategic partners across the AI ecosystem: the tools where employees initiate work, the applications where enterprise data lives, and the infrastructure developers use to build, route, and govern agentic experiences.


Together, these integrations help establish the trusted chain required to secure AI workflows across the enterprise.


### XAA requesting app integrations (client AI agents): *Claude, Cursor, Docker, VS Code, and Zoom*


A requesting app is any AI agent, client assistant, or developer tool that initiates a request to access company data.


The workflow starts where employees ask AI agents to get work done. A product manager might ask Claude to prepare a launch readiness summary, while a developer might ask an agent in Cursor, Docker or VS Code to inspect code or check deployment readiness. When it comes to meeting prep, an employee might ask Zoom's AI assistant, ZoomMate, to gather context from connected apps.


By supporting XAA, these tools can request access to another application through the user’s active Okta identity, so the request is checked against enterprise policy before access is granted, while also reducing the need for the user to manually connect to each tool.


### XAA resource app integrations (downstream applications & MCP servers): *Asana, Atlassian, Canva, Datadog, Figma, Glean, Granola, Linear, Serval, Slack, Supabase, and Zoom*


A resource app is any downstream system, database, or application that holds the data the agent is trying to access.


For the product manager’s launch readiness summary, the agent may need project milestones from Asana or Linear, requirements and documentation from Atlassian, the latest designs from Figma or Canva, internal knowledge and company context from Glean, Serval, or Slack, and meeting discussions and notes from Zoom or Granola.


For a developer's workflow, the context needed may include application and database information from Supabase, as well as performance, reliability, and observability data from Datadog. By supporting XAA, these resource applications help ensure that AI agents can securely pull necessary data context under the umbrella of a single, enterprise-approved Okta identity.


Fill out the form to access this content.


### XAA Supported Identity Infrastructure, Gateways & Frameworks: *Aquera, Archestra.AI, Cloudflare, Keycard, Keycloak, MintMCP, Scalekit, Stytch by Twilio, WorkOS, and Zuplo.*


Identity infrastructure, gateways, and frameworks are the backend developer platforms used to securely route, manage, and connect that agent traffic.


In more complex environments, requests may move through custom agents, MCP servers, API gateways, and orchestration layers before reaching the right system.


Platforms such as Aquera, Archestra.AI, Cloudflare, Keycard, Keycloak, MintMCP, Scalekit, Stytch by Twilio, WorkOS, and Zuplo help developers build, route, and secure these workflows. By adopting XAA, their customers can plug directly into Okta’s identity and policy engine. This establishes a standardized framework for developer platforms to route and govern agent traffic at enterprise scale in a more secure manner.


To solidify this infrastructure as the industry standard, official MCP SDKs are natively adopting XAA as the Enterprise-Managed Authorization extension, with support for TypeScript and Java environments available, and Python support in progress.


### Anthropic provides customer validation of XAA in production


This milestone builds on the success of[Anthropic’s beta program](https://claude.com/blog/enterprise-managed-auth) , which includes Okta as the featured identity provider helping joint customers, including HubSpot, Ramp, Webflow and others, govern their use of Claude and its access to participating MCP providers.


The program provides a first look into XAA-enabled workflows and helps validate the protocol's ability to centralize authorization, enforce robust access policies, and automate offboarding of agent permissions.


Learn more about this[live customer beta with Anthropic](https://www.okta.com/newsroom/press-releases/okta-becomes-a-featured-identity-provider-powering-secure-ai-agent-connections-for-claude-enterprise/) .


Fill out the form to access this content.


## Supporting partner quotes


### Requesting apps


Anthropic


"As AI workflows become more integrated, interoperability and enterprise security need to advance together. We built Enterprise Managed Auth with Okta to replace fragmented, connector-by-connector authorization with a single, centrally managed model, so admins can extend their existing identity investment instead of provisioning access one connector at a time. The expanding Cross App Access ecosystem means enterprises can scale AI while standardizing on the identity provider already running their environment."


– Mayank Malhotra, Product, Anthropic


Docker


"Docker AI Governance is the control plane for enterprise agentic AI, giving security and platform teams the visibility and management they need to run AI agents safely at scale. Integrating XAA for human and non-human identity provides end-to-end security seamlessly, with no extra login, and giving enterprise admins the control they need to adopt enterprise-wide agentic AI and end the proliferation of uncontrolled authentication."


– Eli Aleyner, VP of Strategy, Docker


Zoom


"Zoom sits at the center of how teams communicate and get work done. By supporting Cross App Access as both a requesting and resource application, we're ensuring that AI agents can securely bring meeting context to other workflows — and pull relevant information into Zoom to help employees move work from conversation to completion. That's the kind of seamless, identity-governed experience our joint customers expect."


– Brendan Ittelson, Chief Ecosystem Officer, Zoom


###


### Resource apps


Asana


"Asana is building the system of record for human-agent teamwork, and that depends on enterprises trusting how AI agents access the work they manage in Asana. Cross App Access lets our shared customers connect agents to Asana through a single, governed Okta identity, with the visibility and control their security teams require. We see secure, standards-based access as the foundation for agentic work."


– Arnab Bose, Chief Product Officer, Asana


Atlassian


"Enterprises need two things before they can trust AI agents at scale: the organizational context to act effectively and the identity governance to act securely. Atlassian's Teamwork Graph gives agents access to billions of nodes of organizational knowledge, providing the right context to deliver better results with less compute. The XAA protocol available through Okta ensures those connections remain secure and centrally managed, giving enterprises the confidence to move from experimental AI pilots to full-scale business transformation."


– Jamil Valliani, Head of AI Product, Atlassian


Canva


"Canva is already trusted by 95% of the Fortune 500, and our MCP server lets even more teams create, edit and publish on-brand designs with Canva's AI and design tools, all in the same workflow. Enterprise Managed Auth (also known as Cross App Access) makes it clear and simple for enterprises to manage AI access with a system they already trust, enabling teams to create with AI, safely and at scale."


– Anwar Haneef, GM & Head of Ecosystem, Canva


Glean


“AI agents need the same core security principles enterprises already expect for every other identity: explicit identity, centrally governed access, least privilege, and full auditability. By supporting Okta Cross App Access, Glean helps customers control what each agent is allowed to access through their existing identity and policy infrastructure, rather than relying on broad user-granted connections. That gives enterprises a more secure path to deploying agents in production.”


– Sunil Agrawal, CISO, Glean


Linear


"As AI agents evolve into autonomous teammates that triage bug reports, write code, and manage pull requests right within Linear, securing these workflows becomes paramount. By bringing Linear into Okta’s Cross App Access ecosystem, our customers can safely delegate complex tasks to agents under a single, enterprise-approved identity. This ensures engineering teams can safely unlock massive productivity gains without IT ever losing visibility or control over corporate context."


– Jori Lallo, Co-founder, Linear


Slack


"The richest context in any organization lives in its conversations: the decisions, the reasoning, the work in motion. Slack's MCP Server gives AI agents access to that context. And with Cross App Access built into MCP, employees never have to think about it; no authorization prompts, no per-server sign-ins. Organizations govern agent access to Slack once, through the identity policies they already have in place."


– Rod García, VP of Engineering, Slack


Serval


“Serval has always met people where they work. First in Slack and Teams, and now increasingly in agentic surfaces. Whether it's an MCP agent in Claude or a home-grown copilot, XAA lets Serval show up natively in those surfaces while allowing IT to manage that cross-app connection through their trusted IdP.”


– Jake Stauch, Founder & CEO, Serval


### Identity infrastructure, gateways & frameworks


Aquera


"Cross App Access, the open, vendor-neutral OAuth extension Okta has championed, delivers a much-needed standards-based way to govern how agentic platforms and autonomous agents interact with enterprise applications. The Aquera Agentic AI Bridge and MCP Servers build on that standard, connecting, via the Model Context Protocol, the systems of record that run the business — HR, CRM, finance, ITSM, etc. — to client AI agents that are authenticated and authorized by Okta."


– Reed Henry, Co-founder & CEO, Aquera


Archestra.ai


“Every enterprise we talk to wants to deploy AI agents, and almost every one is stuck on the same problem: managing agent authorization, smoothly, and at scale. By building Archestra.AI on Cross App Access, we’re letting companies extend the Okta identity they already trust to every agent connection, so security stops being the reason agent projects stall. Partnering with Okta here is how we make secure agent access something every enterprise can adopt, not just the ones with a platform team to build it themselves.”


– Joey Orlando, Co-founder, Archestra.AI


Keycard


“As developers build increasingly capable agents, authorization is becoming infrastructure. Standards like XAA and ID-JAG create a common foundation for how agents identify themselves, receive delegated access, and operate on behalf of users across enterprise systems.


We’re excited to support the MCP Enterprise Authorization extension based on XAA because it helps developers build interoperable agents without reinventing identity and access controls for every application. When paired with runtime enforcement, organizations can move beyond static credentials and standing privileges, giving agents task-scoped access that is attributable, auditable, and revocable by default.”


– Ian Livingstone, Co-founder & CEO, Keycard


Keycloak (provided by Hitachi)


"As a long-standing contributor to Keycloak, Hitachi is proud to help the support of Model Context Protocol (MCP) authorization capabilities and Cross App Access with the open-source community. Through the powerful alignment of Okta and Keycloak, we are ensuring that enterprises can seamlessly scale secure AI automation while maintaining strict, standardized identity governance."


– Yuichi Nakamura, Head of OSPO, Hitachi, Ltd.


MintMCP


"Agents should not directly impersonate a user, and users should not have to sign in twice just to let an agent help them. With the Cross App Access protocol, MintMCP's gateway can give each agent its own identity while it acts on behalf of a real user. That means security teams can limit and revoke access, and users stay out of the repeat login loop."


– Jiquan Ngiam, Co-founder & CEO, MintMCP


ScaleKit


“Static API keys and fragmented per-user consent screens are the biggest security blind spots in enterprise AI today, and XAA fixes both by bringing every agent connection under a single, centrally governed identity perimeter. Scalekit gives developers a straightforward path to building XAA-compliant flows without reinventing the authorization layer. Together, this lets enterprises deploy agents at scale with the visibility, control, and frictionless experience that both IT and end users need.”


– Ravi Madabhushi, Co-Founder & CTO, Scalekit


WorkOS


“AI agents are becoming a new class of enterprise client, and they need the same identity controls, auditability, and least-privilege access that companies expect from every production application. Cross App Access gives developers a standards-based way to connect agents to enterprise systems without falling back to static API keys or one-off integrations. At WorkOS, we’re focused on making enterprise-ready auth simple for developers, and our support for XAA helps customers build agentic products that plug directly into Okta’s identity and policy engine from day one.”


— Michael Grinich, Founder & CEO, WorkOS


Zuplo


"APIs and MCP are how AI gets work done. One of the biggest obstacles today is a tangled mess of auth options that aren't easy to use. XAA is a game changer – delivering on both an excellent customer experience and compatibility with governance layers like the Zuplo MCP Gateway."


– Josh Twist, CEO, Zuplo


### XAA availability and access within Okta


-


Okta Workforce Customers: Admins can begin accessing supported XAA applications through the[Okta Integration Network](https://www.okta.com/integrations/) (OIN) starting August 2026.


-


Auth0 B2B SaaS Customers: XAA is planned to be available to Auth0 developers in early access at the end of July 2026. ISVs and developers interested in building or consuming these secure flows early are invited to sign up for the[Auth0 XAA Beta Program](https://docs.google.com/forms/d/e/1FAIpQLSfmhg0hi3qaJ7BZy92eTQ1zk6FWcP89hFwOk0hPJEoERZms5A/viewform) .


[Get started today with Okta and the XAA partner ecosystem.](https://www.okta.com/solutions/cross-app-access/)


*Any mention in this article of solutions, features, functionalities, certifications, authorizations, or attestations that are not currently generally available or have not yet been obtained may not be delivered or obtained on time or at all. We assume no obligation to deliver on such items and you should not rely on them to make your purchase decisions.*


About Okta


Okta


Okta, Inc. is The World’s Identity Company™. We secure AI, machine, and human identity so everyone is free to safely use any technology. Our customer and workforce solutions empower businesses and developers to protect their AI agents, users, employees, and partners while driving security, efficiencies, and innovation. Learn why the world’s leading brands trust Okta for authentication, authorization, and more at[okta.com](http://okta.com/) .
