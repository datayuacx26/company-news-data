---
schema_version: "1.0.0"
document_id: "8f1c366e5a176c70f902e0eb4948fbeb18268409871616bb32d188d75314373e"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0d567709f64e"
canonical_url: "https://www.microsoft.com/en-us/power-platform/blog/2026/08/06/microsoft-entra-agent-id-for-dataverse/"
published_at: "2026-08-06T14:00:00+00:00"
first_seen_at: "2026-08-07T16:38:34.229864+00:00"
fetched_at: "2026-08-07T16:38:36.216920+00:00"
content_hash: "sha256:260e0844062cfd9760bc33950c8cdada4e20edd83e52b5bd32a188196ca20008"
---

# Secured and Governed your AI Agents: Microsoft Entra Agent ID for Dataverse

## Now in public preview: Microsoft Entra Agent ID and Dataverse agent users


AI agents are beginning to play a larger role in everyday business processes—from researching prospects and updating records to supporting outreach and follow-up. As organizations explore these possibilities, it is helpful to have a clear and manageable way to understand which agent is taking action and what it is permitted to do.


Microsoft Entra Agent ID and Dataverse agent users offer a new identity foundation for these scenarios. Together, they help organizations give each agent a dedicated identity for accessing Dataverse, assign appropriate security roles, and maintain visibility into the agent’s activity.


*This approach can help teams introduce agent-based experiences thoughtfully, with clearer separation between people, applications, and autonomous agents.*


## What is Microsoft Entra Agent ID?


[Microsoft Entra Agent ID](https://learn.microsoft.com/en-us/entra/agent-id/what-is-microsoft-entra-agent-id) extends Microsoft Entra with identity constructs designed for AI agents. Instead of treating an agent as an anonymous automation or sharing a generic application identity, an organization can give the agent an individually identifiable, policy-controlled identity with its own permissions and audit trail.


## How Dataverse agent users complete the model


When an agent needs to work with business data in a Dataverse environment, an administrator creates a Dataverse agent user associated with the agent’s Entra identity. The agent user becomes the security principal inside that environment, where it can be assigned dedicated, least-privileged Dataverse security roles.


- **Authenticate the agent:** Establish a distinct enterprise identity for the agent.


- **Authorize only what it needs:** Assign Dataverse security roles aligned to the agent’s intended tasks.


- **Separate duties:** Distinguish agent activity from human users and conventional application integrations.


- **Audit agent actions:** Attribute data access and changes to the agent identity.


- **Manage the lifecycle:** Apply administrative ownership and governance as the agent is created, updated, or retired.


## Why this matters


Agent identity turns governance into an architectural capability rather than an afterthought. Security teams gain clearer accountability, administrators can enforce least privilege, and makers can build agentic experiences on a foundation designed for enterprise access and compliance.


### Example: Sales Development Agent


Consider a[Sales Development Agent](https://learn.microsoft.com/en-us/microsoft-agent-365/use#use-the-sales-development-agent) that helps a sales organization keep up with a large volume of prospects. The agent can continuously research leads, prepare personalized outreach, follow up, and hand promising leads to sellers. To do that responsibly, it needs access to CRM data—but it should not inherit broad permissions or operate under a shared identity.


How the identity-enabled flow works


1. **The agent receives an Entra agent identity.** The identity establishes the agent as a distinct nonhuman actor that the organization can manage and govern.


1. **The Power Platform administrator adds the agent to Dataverse.** A Dataverse agent user is created and associated with the Entra agent identity.


1. **The **Power Platform** administrator assigns a purpose-built security role.** For example, the role can permit the agent to read and update eligible leads, create activities, and record outreach outcomes—without granting access to unrelated tables or sensitive fields.


1. **The agent works within those permissions.** It researches assigned leads, evaluates fit, generates or sends approved outreach, follows up, and updates lead status according to the configured sales process.


1. **Actions remain attributable.** Administrators can distinguish changes made by the agent from those made by a seller or a conventional application user.


## Get started with the public preview


Start in a non-production environment and identify one well-bounded agent scenario. Define the data and operations the agent needs before assigning permissions.


1. Create or enable the agent identity through a supported[Microsoft agent creation experience](https://learn.microsoft.com/en-us/microsoft-copilot-studio/agents-experience/overview) .


1. In the Power Platform admin experience, add the Entra agent identity to the target Dataverse environment as an agent user.


1. Assign a dedicated Dataverse security role that follows least-privilege principles.


1. Test expected and denied operations, and verify that the agent’s actions are visible in your audit and monitoring processes.


1. Document the business owner, security owner, intended purpose, and retirement process for the agent.


**Preview reminder:** Preview capabilities are intended for evaluation and may have restricted functionality. Validate availability, licensing, regional support, and current documentation before adopting the capability for production workloads.


## Build agents the enterprise can trust


The next generation of business applications will include people and agents working side by side. Microsoft Entra Agent ID and Dataverse agent users help make that collaboration secure by design: every agent can have a recognizable identity, only the access its role requires, and an auditable record of its work.


Explore the public preview, test a scoped scenario such as sales development, and share your feedback with the Power Platform community. What business process would you entrust to an agent with its own governed identity? To Learn more:


- [Microsoft Entra Agent ID documentation | Microsoft Learn](https://learn.microsoft.com/en-us/entra/agent-id/)
- Dataverse Agent User:[Create Entra agent users (preview) – Power Platform | Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/admin/agent-users)
