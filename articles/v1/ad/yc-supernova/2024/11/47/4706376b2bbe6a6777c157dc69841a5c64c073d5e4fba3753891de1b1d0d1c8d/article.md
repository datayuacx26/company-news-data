---
schema_version: "1.0.0"
document_id: "4706376b2bbe6a6777c157dc69841a5c64c073d5e4fba3753891de1b1d0d1c8d"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/design-system-at-scale-managing-roles-teams-and-collaboration-with-supernova"
published_at: "2024-11-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:392eae3cf40ef89036b69569759fc329fde300a877f2a0e664c0f1f409a9cb3d"
---

# Design System at Scale: Managing Roles, Teams, and Collaboration with Supernova

Managing large-scale design systems across multiple teams requires strategic planning and robust tools to ensure consistency, efficiency, and alignment. Supernova empowers design system managers with features like design system roles, access control, approval workflows, and multi-design system navigation. These tools not only enable you to manage access but also to structure and scale your design systems effectively.


This guide provides actionable strategies for distributing teams, assigning roles, and integrating external contributors and consultants into your workflow.


## Strategically Distribute Roles: Empowering Core Teams


When managing a large, multi-team design system, start by identifying the roles your teams need at both the workspace and design system levels. Consider:


- Core team: Responsible for maintaining the global design system.
- Peripheral teams: Contributing localized or product-specific adaptations.
- External contributors: External collaborators, such as agencies or freelancers, who help develop or update system components.
- External consumers: Consultants or third-party vendors who need limited access to specific parts of the system, such as documentation or tokens.


### How to Do It in Supernova:


Assign workspace-level roles to internal, core teams to manage global permissions and maintain a clear hierarchy.


- Admins: Lead teams and oversee all design systems.
- Editors: Core contributors responsible for maintaining tokens, components, and documentation.


Use design system-level roles to tailor permissions for external contributors and consumers:


- Assign Contributors to external collaborators or peripheral team members who need to draft changes but lack publishing rights.
- Use Viewers for external consumers to allow access to specific design system documentation without edit privileges.


By strategically distributing roles, internal and external teams will only interact with relevant parts of the design system, reducing confusion and ensuring only desired content gets showcased to consumers at the right time.


## Leverage Multi-Design System Navigation: Structuring a System of Systems


Multi-design system navigation allows you to scale by creating separate but interconnected design systems. Strategize how to structure your systems to support both internal and external teams:


- A core system can house universal assets like design tokens and foundational components.
- Satellite systems can cater to regional or product-specific needs, or serve as dedicated spaces for external contributors.


You can take this granularity a step further by setting design system access of specific design systems to Invite-only. ideal for maintaining privacy and control over your design assets and documentation. By limiting access to only invited team members, you can safeguard sensitive data while enabling targeted collaboration.


See how the[Zinnia design systems team](https://zinnia-design-system.supernova-docs.io/latest/welcome-to-bloom-c04etEZt) leverages the multi-design system navigation to showcase their core design system - Bloom - and a satellite system called Layout.


### How to Do It in Supernova:


1. Go to Documentation > Settings > Advanced and look for the Multi-design system navigation section.
2. Beside Switch between design systems in documentation, toggle the button to Yes.
3. In the dropdown, select the design systems from your workspace that you want to enable for viewers to choose between in your documentation.
4. Publish your documentation to apply this setting.


To use multiple design systems in your workspace, you’ll need to enable it in each design system individually. Learn more about[multi-design system navigation setup](https://learn.supernova.io/latest/documentation/customizing-your-documentation/advanced-settings-ZFqrVkY7#search-d4323341-6684-11ee-8899-f734b997fb4d) .


This approach ensures internal teams have access to comprehensive resources while external collaborators see only what’s relevant to their work, maintaining clarity and security.


## Approval Workflows and Page Statuses: Ensuring Quality


Approval workflows help maintain quality and consistency across documentation by introducing a review process. Assigning clear responsibilities for drafting, reviewing, and approving ensures accountability.


### How to Do It in Supernova:


1. Go to the Documentation Settings / Workflows section
2. Toggle the option to enable approval workflow. This will activate the feature and enable statuses for all edited pages
3. Toggle the option to “Restrict publishing of unapproved pages” to ON. This will prevent users from publishing pages that haven’t been approved with the “Ready for publish” status
4. Assign Contributors to draft content and Editors to review and approve and publish updates.


Enable your team and set a system to uses Page Statuses like Draft, In Review, and Approved to track progress and communicate page readiness.


Approval workflows streamline collaboration while ensuring only vetted content is published. This is especially valuable when external teams contribute to documentation. Read more[about page statuses and workflows](https://learn.supernova.io/latest/releases/august-2024/approval-workflows-Qzlx0Ubv#section-approval-workflow-db) .


Managing large, multi-team design systems requires careful planning and the right tools. Supernova’s features—like design system roles, access control, multi-design system navigation, and approval workflows—help design system managers strategically distribute roles, integrate external collaborators, and maintain a scalable system of systems.


By thoughtfully structuring access and responsibilities, you can create a design system that serves diverse audiences while ensuring security and efficiency. Start implementing these strategies today and unlock the full potential of Supernova for your team.
