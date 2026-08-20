---
schema_version: "1.0.0"
document_id: "bd3d1941a7ea517aeec28fcb1a47abaf6298f81440d5adc0cc87e43da6e5b6f9"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/airbyte-agents-workspaces"
published_at: "2026-07-14T16:52:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:4310e89a167f97e50fe956080ace947678a789d9c5189dde3779788bbd384ed8"
---

# Workspaces Now Available in Airbyte Agents

Today we are rolling out workspaces to Airbyte Agents, the headline feature of our Team plan. Workspaces allow people on a team to share one Airbyte organization while maintaining control over who can see which connectors.


## **The problem with one shared space**


Until now, everybody within an Airbyte Agents organization shared one space. That works for the two of you in your office. But once you have a real team working together, it causes issues. Sales, marketing, and support may have different tools that they use daily. Some of them may belong to the company or your organization as a whole, while others are more personal and may belong to one individual alone. Workspaces allow teams to share tools while ensuring that only those who are supposed to see and use a particular connector have access to it.


## **What workspaces do**


Workspaces allow a team to create a space, add the connectors that the team or individuals should have access to, and define which people are members of that space. Only members of a workspace can view and use the connectors within it.


The difference between a shared and a private workspace is simpler than it sounds. A shared workspace is one that several people have been granted access to, so everyone can see and use the same connectors. A private workspace is one that nobody else has been added to yet. Beyond who has access, the two behave in exactly the same way, and you can grant access to anyone at any time. Keep in mind that organization administrators can see every workspace regardless, since they need visibility into usage and billing across the platform.


You can add or remove individuals from a workspace as needed. Additionally, all the information stored in the default workspace that was created for each Airbyte Agents organization will remain available to every team member.


## **A concrete example of how to use workspaces**


Let's say you are the marketing and sales manager and both departments live under your company. You can place shared sales and marketing tools into a shared workspace so that both departments have access to the same tools. You can then place your own tools into a private workspace, so that you keep access to your individual tools without giving others in the organization the ability to view them.


The shared context remains shared, and the personal context stays personal. Airbyte Agents can manage both types of connectors and ensure that individuals do not view a connector they should not have access to.


## **How workspaces relate to the Context Store**


Each connector that you define with Airbyte Agents syncs data to the Context Store. This is the indexed data that your agents use to find the information they will query against the live API of the connector. Workspaces do not change this functionality. Each connector within a workspace has its own context, and you can move it as you would any other tool.


For those using the MCP server or SDK, Airbyte Agents can work across more than one workspace. Point your agents to the workspace you want them to work within, and they will execute within that defined workspace.


One thing to note: you cannot query two workspaces at the same time. An agent has to call each workspace independently, then weave the results together with its own reasoning. This is by design, and it is what keeps the data within each workspace from being accessed by anyone who should not see it.


## **Part of the Team plan**


Workspaces are part of the Team plan, which is built for organizations that require SSO, multiple users, and workspaces. The Airbyte Agents Team plan is priced at $299 a month and comes with 10,000 Agent Operations (AOs) a month. This is enough for your whole team to use Airbyte Agents with multiple workspaces, workflow execution in parallel, and access to a shared Context Store. Any additional AOs beyond the 10,000 a month will cost $0.005 per operation.


If you are already using Airbyte Agents, workspaces have been added to the sidebar of your Airbyte Agents organization. Click the plus sign to create a workspace, add the connectors that you would like to allow individuals to use, and invite the users that need access. If you are new to Airbyte Agents, you can start for free and upgrade to the Team plan when you need it.


Instead of adding governance later out of necessity, we want it to be the easy path for all users from the very start. Workspaces are the first piece of functionality we are rolling out here, but there will be more throughout the summer. We will also add more control over what individual users within a workspace can read and write, to ensure better governance within teams. In the meantime, get your teams up and running the way they actually work, so that you can share the space and tools your organization requires. Let us know how it's working for you!
