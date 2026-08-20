---
schema_version: "1.0.0"
document_id: "58dd8736b73bcf2f32bf7b2cb5e3bcd88abfc9984d77160f0b5f9a57f30164d9"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/product-innovation/okta-ai-agent-import-expansion/"
published_at: "2026-08-04T07:00:00+00:00"
first_seen_at: "2026-08-04T17:22:20.873737+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:a602cab9bf71df3af0eaa3a537416434e6673a0ce1bb205a8504608d4f2bb91d"
---

# AI Agent Import: Govern agents where teams build them

### Topics


---


AI Agents


,


Okta Platform


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


## Key takeaways: Expanded platform coverage


- **Say yes to more agent builder platforms:** Okta’s AI Agent Import library now supports six major agent builder platforms: Gemini Enterprise Agent Platform, DataRobot, Workday, Microsoft, Glean, and LangSmith.
- **Easily bring agents under governance:** Custom agents running inside these platforms frequently operate outside central IAM visibility, creating compliance risks and security blind spots.
- **Onboard all your agents into one registry:** Security teams can now automatically sync and govern custom agents from these newly supported platforms to manage access, lifecycle controls, and deactivations within Okta’s single control plane.


Enterprise teams adopt AI agent platforms for specific, targeted advantages. They choose Anthropic's Claude because it excels at parsing complex code, legal contracts, and more. They choose Azure AI Foundry because it naturally plugs into their existing Microsoft stack. They choose DataRobot because it runs their predictive models.


The governance problem begins when each platform maintains its own isolated agent registry and policy layer, completely disconnected from the identity systems your security team uses to govern the rest of the organization. Meanwhile, agent adoption is outpacing governance programs by a wide margin.


**Gartner predicts[40% of enterprise applications will feature task-specific AI agents](https://www.okta.com/reports/businesses-at-work/) by the end of 2026.** Yet, **fewer than one-third (32%) of organizations currently hold those agents to the same strict governance standards as human identities.** That risk surface grows with every new agent a business team spins up.


Closing that gap starts with a foundational question from the[blueprint for the secure agentic enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) : Where are my agents?


The AI Agent Import feature of[Okta for AI Agents](https://www.okta.com/products/govern-ai-agent-identity/) does exactly that. It integrates with the builder tools your teams are actually using, pulling critical information about your agents’ identities from siloed platforms into a single, unified identity registry.


Once the data is imported into Okta, security and identity and access management (IAM) teams have a single place to manage ownership, lifecycle controls, access certifications, and deactivation.


A single platform's native registry can't offer that same coverage. It governs the agents within its walls. AI Agent Import is platform-agnostic, which means AI teams can keep building on whatever platform fits the job, and security gets the same governance guardrails everywhere. This way, you get the most out of your AI agents.


## Supported agent platforms and governance capabilities


Connecting each builder tool directly to Okta extends visibility and automated control across your entire AI ecosystem. AI Agent Import already supports existing platforms, like Claude Managed Agents. With this latest expansion, the AI Agent Import library can help security teams govern and secure AI agents across the enterprise.


This capability now expands to additional agent builder platforms like:


- Gemini Enterprise Agent Platform
- DataRobot
- Workday
- Microsoft
- Glean
- LangSmith


## More platforms, more tools, less visibility


Coverage gaps multiply with every platform you add. If your teams build on two platforms, governance often means managing two separate tools. Four platforms, four tools. Each covers its own agents and nothing else.


Discovery tools can surface a list of agents and flag risks. Detection tools can tell you what an agent did after something goes wrong. Neither can tell you who owns the agent running right now, what it has access to, or how to deactivate it before the next review forces the question.


Centralizing agent governance in your identity control plane is critical for keeping pace with how fast business teams adopt new tools. That is the power of AI Agent Import. Connect a platform once, and every agent registers automatically in Okta.


This means Okta registers the agent’s base profile, allowing you to view it in the same registry you use to govern your human identities. The registry stays current every time you run an import, reflecting any agents you add or remove at the source. Each platform uses a pre-built integration, not a custom connector your team has to build and maintain.


## How to control and secure your AI agents


From Okta’s AI Identity Summit, discover Okta’s blueprint for managing agent visibility, access, and actions.


[Watch the keynote](https://www.okta.com/resources/videos/ai-summit-2026-keynote/)


### Where agents already touch your most sensitive workflows


AI teams don't wait for governance to catch up; they build. Take a few examples:


- A team spins up a custom customer service operations agent on the Gemini Enterprise Agent Platform (using Vertex AI Agent Engine) to handle tier-one support. It reads the customer relationship management (CRM) system, queries order history, and updates support tickets across thousands of daily interactions.
- Another team builds a custom HR onboarding agent in an enterprise HR system, such as Workday, that reads employee profiles, queries tax documents, and updates payroll routing details to automate internal transfers.


Both agents operate in production, have access to highly sensitive customer and employee data, and have permission to modify records.


But when your security team runs an access review, or a compliance officer processes a data subject request (DSR) or payroll audit requiring a paper trail of every system that touches that data, these agents are invisible. They do not exist in your identity inventory. They only live deep inside the Gemini or Workday platforms.


AI Agent Import changes that by pulling the identity of these agents directly into Okta, assigning them owners, and making them visible in the same reviews as every other human identity.


This logic holds across your SaaS ecosystem, including:


- **[Claude Managed Agents](https://www.okta.com/newsroom/press-releases/okta-becomes-a-featured-identity-provider-powering-secure-ai-agent-connections-for-claude-enterprise/)** conducting automated code reviews
- **Microsoft Copilot Studio and Microsoft Foundry** agents accessing sensitive enterprise data
- **Glean** agents surfacing deep organizational knowledge
- **Workday Agent System of Record (ASOR)** automating critical HR workflows
- **LangSmith** deploying agents that orchestrate workflows, accessing internal tools and data


While the platforms and use cases differ, the solution remains the same. Connect the platform to Okta, run a sync, and every agent on it is automatically added to your central identity registry.


### Extend visibility and control with every pre-built integration you add


Your teams will keep adopting new agent platforms. If closing each new gap requires custom code, your governance model will inevitably break.


We designed Okta’s expanding import library to stay ahead of that pace. You connect each platform to Okta individually using pre-built integrations, switching the workflow from building custom connectors to simply configuring them. We’ve already done the heavy lifting for you.


Each new integration adds to a shared governance layer that grows more complete with every platform you connect, and every agent your teams deploy on it.


**The bottom line:** The organization that centralizes agent governance today can confidently say "yes" to the platform their business teams want to use tomorrow. Set up the import, run the sync, and secure their access (and don't be surprised if the sheer number of registered agents catches your security team off guard.)


## Secure the agentic enterprise with Okta for AI Agents


AI agents represent the fastest-growing identity in the enterprise and the least governed.[Okta for AI Agents](https://www.okta.com/products/govern-ai-agent-identity/) provides the central identity layer to securely manage your AI agents from a single control plane. By importing your AI agents into Okta, your security teams can discover, onboard, protect, and govern them at scale.


**Ready to close the identity gap at the center of your AI security?** Don’t leave your agentic enterprise unprotected. Get our[blueprint for the secure agentic enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) to help you answer three non-negotiable questions of agent security: Where are they? What can they connect to? What can they do?


*These materials are intended for general informational purposes only and are not intended to be legal, privacy, security, compliance, or business advice. You are responsible for obtaining security, privacy, compliance, or business advice from your own professional advisors. Any mention of future products, features, functionalities, or certifications in this blog is for informational purposes only. These items are not commitments to deliver and should not be relied upon to make purchasing decisions. © Okta, Inc. and/or its affiliates 2026.*


About the Author


[Dipti Kanthilal Senior Product Manager Dipti Kanthilal is a Senior Product Manager at Okta, currently leading product for AI Agent imports, registry, and governance to help organizations secure non-human identities within their digital infrastructure. She previously led Universal Directory, delivering flexible solutions like Okta Realms and Secure Partner Access (SPA) to help enterprises scale their workforce and partner management. Outside of work, Dipti is an avid traveler and a focaccia enthusiast who loves experimenting with new recipes.](https://www.okta.com/blog/author/dipti-kanthilal/)


[Charu Jain Product Manager Charu Jain is a Product Manager at Okta, where she specializes in driving the next generation of identity security. Having previously led product initiatives for SaaS application integrations, she now focuses on the emerging frontier of AI agent imports and securing their connections to critical enterprise resources. Charu completed her MBA from the Indian School of Business (ISB) before joining Okta full-time in 2025. Before this, she worked as a Sr. Software Engineer at JPMorgan Chase & Co. Outside of work, Charu is passionate about the performing arts and enjoys attending dance workshops, watching theatre, and exploring cinema.](https://www.okta.com/blog/author/charu-jain/)
