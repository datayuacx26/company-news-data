---
schema_version: "1.0.0"
document_id: "e5a7332ed8e8d3d4bab09479ff5f3d99dcd1125fc0b26d351837f2845b90cc0b"
company_key: "yc-unthread"
company: "Unthread"
source_id: "yc-unthread-rss-c618c87b9ca4"
canonical_url: "https://unthread.io/blog/migrate-freshservice-jira-service-management/"
published_at: "2026-08-14T13:00:00+00:00"
first_seen_at: "2026-08-15T04:23:32.205337+00:00"
fetched_at: "2026-08-15T04:23:33.757353+00:00"
content_hash: "sha256:1ac384897507c69e53f39af5368b9c87761a35f990814936ae53ae84be2fa2b5"
---

# Migrate from Freshservice to Jira Service Management: Step-by-Step Freshservice Migration Guide

What most IT teams underestimate about migrating from Freshservice to Jira Service Management is that the complexity lies in transforming data, not simply moving it. Freshservice uses ITIL-specific modules, while JSM organizes work as issues within projects, so tickets, custom fields, and workflows need to be carefully mapped.


Organizations often move to JSM to unify IT and development teams within the Atlassian ecosystem, improve DevOps alignment, and consolidate tools. But migration can disrupt service delivery if teams are not prepared. IT must maintain SLAs, HR needs uninterrupted employee support workflows, and internal departments still expect business as usual.


This guide covers each migration phase, from planning through optimization, with a focus on common technical challenges. Teams looking to reduce context-switching can also layer[Slack-native service management](https://unthread.io/products/slack-support?ref=unthread.io) tools like Unthread on top of JSM so employees can submit and track requests without leaving Slack.


## **Key Takeaways**


- **Migration is a structural data transition, not a simple export.** Freshservice's purpose-built ITIL modules must be mapped to Jira Service Management's issue-based architecture, requiring careful field mapping, workflow redesign, and schema planning before any data moves.
- **Custom field type mismatches cause most migration failures.** Pre-create all JSM custom fields before migration begins, paying close attention to type conversions (Freshservice Booleans become JSM Radio Buttons, not Checkboxes).
- **Atlassian Guard can add identity and security costs for licensed or managed users, so organizations should confirm which users are billable before comparing JSM's total cost with Freshservice.**
- **CMDB migration requires schema redesign for most organizations.** JSM Assets enforces a[2 unique constraint limit](https://clonepartner.com/blog/freshservice-vs-jira-service-management-the-2026-ctos-guide?ref=unthread.io) per object type. If your Freshservice schema requires more than two attributes per object type to enforce uniqueness, plan to redesign how those identifiers are modeled before importing them into JSM Assets.
- **Knowledge base migration requires planning around JSM's Confluence-backed knowledge base.** Articles are stored as Confluence content, with[inline links breaking](https://help-desk-migration.com/freshservice-to-jira-service-management-migration?ref=unthread.io) during transfer and requiring redirect mapping.
- **Realistic timelines vary substantially.** Migration timelines depend on ticket volume, custom fields, attachments, CMDB complexity, knowledge-base structure, user mapping, testing requirements, and whether delta migrations are needed. Large environments with 100,000+ records should plan for a staged migration rather than assuming a fixed duration.


## **Understanding the Need for Migration**


The decision to migrate service desk platforms affects every internal team that submits or resolves requests. IT handles infrastructure tickets, HR manages employee queries, finance processes procurement requests, and operations coordinates cross-functional workflows. Before committing to migration, evaluate whether the disruption delivers sufficient long-term value.


### **Assessing Your Current Freshservice Setup**


Start by auditing what is actually being used. Many organizations pay for Freshservice Pro features they have never configured. Document the current state:


- **Ticket volumes and patterns** - How many tickets per month? Which categories drive the highest volume? What is the average resolution time?
- **Custom fields in active use** - Which custom fields appear on tickets versus which exist but collect no data?
- **Automation rules** - How many workflows run automatically, and how critical are they to daily operations?
- **Asset management complexity** - How many CMDB objects exist? How many unique identifiers does each asset type use?
- **Integration dependencies** - Which third-party tools connect to Freshservice, and how would migration affect those connections?


This audit serves two purposes. First, it reveals whether current platform limitations genuinely constrain operations or simply reflect configuration gaps that could be addressed without migrating. Second, it creates the inventory needed for accurate migration planning.


### **Key Advantages of Jira Service Management**


JSM delivers specific advantages for organizations already invested in the Atlassian ecosystem or requiring tighter development team integration.


**DevOps alignment stands out as the primary differentiator.** JSM connects natively to Jira Software, Bitbucket, and Confluence. When an IT team receives an incident report, they can link it directly to a development bug, track the fix through code review, and close the service ticket when deployment completes. This cross-functional transparency eliminates the manual coordination that slows resolution in siloed environments.


**Change management with CI/CD integration** allows IT teams to assess deployment risks, automate CAB approvals, and[gate releases](https://clonepartner.com/blog/freshservice-vs-jira-service-management-the-2026-ctos-guide?ref=unthread.io) based on critical incident status. JSM Premium includes AI risk scoring for change requests, flagging high-risk deployments before they impact production.


**ITIL 4 certification** provides third-party validation of JSM's alignment with recognized IT service management practices. Atlassian's earlier PinkVERIFY certification expired in 2024 as certification processes transitioned to PeopleCert. Freshservice aligns with ITIL but lacks this third-party certification, which matters for organizations in regulated industries requiring auditable compliance.


**Consolidated licensing** reduces costs when organizations are already paying for Jira Software and Confluence. Adding JSM becomes an incremental expense rather than a separate platform cost. Atlassian customer research reports measurable productivity improvements after JSM adoption, although results vary by organization, implementation scope, and the systems being replaced.


## **Phase 1: Pre-Migration Planning and Strategy**


Successful migrations fail or succeed during planning, not execution. Organizations that rush to move data before establishing clear objectives, stakeholder alignment, and success metrics encounter problems that could have been prevented with an additional week of preparation.


### **Defining Migration Goals and Objectives**


Every stakeholder group needs to understand what the migration accomplishes and how success will be measured:


- **For IT teams** - Faster incident resolution through development integration, reduced tool sprawl, improved change management controls
- **For HR teams** - Continued employee support capabilities, preserved historical data for compliance, minimal disruption during transition
- **For finance** - Clear ROI timeline, documented cost comparison, understanding of hidden expenses like Guard licensing
- **For leadership** - Risk mitigation plan, communication timeline, rollback procedures if critical issues emerge


Define specific success metrics before migration begins. These might include: maintaining current SLA compliance during transition, completing migration within budget, achieving target user adoption within 30 days of go-live, and preserving 100% of historical ticket data for audit purposes.


### **Inventorying Freshservice Assets and Data**


Create a comprehensive inventory of everything that needs to migrate:


**Ticket data:**


- Total ticket count by status (open, pending, resolved, closed)
- Custom fields and their data types
- Attachments and inline images
- Public comments and private notes
- SLA timestamps and due dates
- Assignment history and audit trails


**User data:**


- Agent accounts and permission levels
- Requester accounts and organization associations
- Group memberships and team structures


**Configuration data:**


- Workflow rules and automation logic
- SLA policies and business hours
- Email channel configurations
- Portal customizations


**Asset data:**


- CMDB object types and relationships
- Unique identifiers per asset type (critical for schema planning)
- Custom attributes and their values


This inventory determines migration timeline, identifies schema conflicts, and reveals dependencies that require special handling. For organizations with 100,000+ tickets, expect a staged migration involving batching, validation, and potentially delta synchronization; the actual duration depends on schema complexity, attachments, relationships, and API throughput.


## **Data Migration from Freshservice**


The technical migration transforms Freshservice data structures into JSM's issue-based architecture. This is not a file transfer; it is a systematic conversion requiring field mapping, type transformation, and relationship preservation.


### **Choosing the Right Migration Approach**


Three primary approaches exist, each suited to different organizational contexts:


**Tool-assisted migration** using platforms like Help Desk Migration or OpsHub Migration Manager provides automated field mapping, delta migration support, and[SOC 2 Type II](https://help-desk-migration.com/freshservice-to-jira-service-management-migration?ref=unthread.io) compliance. These tools handle API rate limiting automatically and preserve ticket relationships. Setup takes 1-2 hours for basic configurations, with costs ranging from free demos to enterprise quotes for large-scale migrations.


**Manual CSV export/import** can work for simple, low-volume migrations where preserving complex relationships, attachments, full conversation history, and delta changes is not required. This approach sacrifices ticket relationships, attachment handling, and complete history. It requires technical staff comfortable with data transformation and cannot handle delta migration for tickets created during the transition window.


**Professional services engagement** makes sense for organizations with 50,000+ tickets, complex custom schemas, or regulated industries requiring audit trail preservation. Atlassian partners such as Deviniti provide dedicated migration services for complex environments, with pricing typically determined after assessing data volume, configuration complexity, integrations, and compliance requirements.


### **Handling Custom Fields and Workflows**


Custom field migration causes more problems than any other technical element.[Field type mismatches](https://help-desk-migration.com/freshservice-to-jira-service-management-migration?ref=unthread.io) cause most migration errors, and fixing them after migration is far more difficult than preventing them beforehand.


**Type conversion rules to follow:**


- Freshservice Boolean fields to JSM Radio Button fields (not Checkbox)
- Freshservice Select List fields to JSM Select List fields (ensure values match exactly)
- Freshservice Date Picker fields to JSM Date fields
- Freshservice Text fields to JSM Text fields (validate character limits)


**Pre-create and validate the required JSM custom fields before migration unless your chosen migration method explicitly supports target-schema creation.** Missing or incompatible destination fields can otherwise block mappings or cause data to be omitted.


**Map resolution states explicitly.** If Freshservice "Resolved" and "Closed" statuses are not mapped to corresponding JSM workflow end states, historical closed tickets flood "Open" queues and create false SLA breaches.


**Define a fallback identity strategy before migration.** For Freshservice users who cannot be matched to JSM accounts, decide whether to preserve the original identity in a custom field, map the record to a migration account, or use another supported fallback without losing historical attribution.


For organizations managing[IT service desk](https://unthread.io/solutions/it-service-desk?ref=unthread.io) operations, maintaining workflow continuity during migration is critical. Consider layering Slack-native ticketing on top of JSM to give employees a familiar intake channel while the backend transition occurs.


## **Configuring Jira Service Management**


JSM configuration determines whether agents can work efficiently and whether the platform meets organizational requirements. Complete this setup before any data migrates.


### **Mapping Freshservice Workflows to JSM**


JSM workflows operate differently than Freshservice's. Where Freshservice uses predefined ITIL workflows, JSM requires explicit workflow configuration for each issue type.


**For each Freshservice workflow, document:**


- Entry conditions (how tickets enter this workflow)
- Status transitions (which statuses can move to which other statuses)
- Transition conditions (who can trigger each transition)
- Post-transition actions (automations that fire after transitions)


Then recreate these in JSM using the workflow editor. Pay particular attention to:


- **Approval workflows** - JSM handles approvals differently than Freshservice. Configure approval schemes before migration if workflows require manager sign-off.
- **SLA configurations** - JSM SLAs are configured through metrics, conditions, calendars, and goals that apply to matching work items. Recreate Freshservice SLA policies using equivalent JSM targets such as Time to Resolution and Time to First Response.
- **Automation rules** - Freshservice automation rules do not migrate. Document each rule's trigger, condition, and action, then rebuild them in JSM's automation builder.


### **Setting Up User Access and Permissions**


JSM permissions are more granular than Freshservice's, which creates both opportunity and complexity:


- **Project roles** define what users can do within a service project
- **Permission schemes** assign specific capabilities to project roles
- **Issue security schemes** control who can see specific tickets (critical for HR support handling sensitive employee requests)


For organizations supporting[employee support workflows](https://unthread.io/solutions/employee-support?ref=unthread.io) across departments, ensure that HR tickets remain visible only to HR agents, IT tickets route to IT teams, and finance requests stay within finance's view. JSM's permission schemes support this separation, but configuration requires deliberate planning.


**Group mapping from Freshservice:**


Freshservice Agents map to Service Desk Team Role (assign to service project). Freshservice Supervisors map to Project Administrator Role (can manage project settings). Freshservice Requesters map to Customers (portal access only). Freshservice Groups map to JSM Teams (used for assignment and routing).


## **Ensuring Seamless Support**


The migration itself matters less than what happens immediately before and after. Testing, training, and communication determine whether users adopt the new platform or work around it.


### **Testing Your JSM Configuration Thoroughly**


Never run a full migration without demo migrations first.[Run demo migrations](https://help-desk-migration.com/freshservice-to-jira-service-management-migration?ref=unthread.io) until stakeholders approve the data quality. This is the only chance to catch problems before they affect production data.


**Demo migration checklist:**


- Transfer 20-100 representative tickets across different types and statuses
- Verify all custom field values transferred correctly
- Confirm attachments are accessible and intact
- Check that public comments and private notes maintained proper visibility
- Validate SLA timestamps preserved correctly
- Ensure user assignments mapped to correct JSM accounts
- Test workflow transitions on migrated tickets


Document every issue found during demo migration. Adjust field mappings, add missing custom fields, or fix user account problems before proceeding.


**Critical pre-migration step:**[Disable active Jira](https://help-desk-migration.com/freshservice-to-jira-service-management-migration?ref=unthread.io) integrations (Slack notifications, Jira Software syncs, email notifications) before starting migration. Migrating thousands of tickets with notifications enabled creates a notification storm that overwhelms agents and floods communication channels.


### **Training Your Teams for the New Platform**


Training requirements differ by role:


**For agents:**


- Navigating the JSM interface and inbox views
- Processing tickets through workflows
- Using keyboard shortcuts and bulk actions
- Finding knowledge base articles to share with requesters
- Understanding queue management and assignment


**For administrators:**


- Modifying workflows and automation rules
- Managing user permissions and project settings
- Creating and editing SLA policies
- Building reports and dashboards
- Troubleshooting common issues


**For end users (requesters):**


- Submitting requests through the portal
- Tracking ticket status
- Responding to agent questions
- Using self-service knowledge base


Plan for 2-3 days of training workshops for agent teams, with ongoing support available during the first weeks after go-live.


For teams that want to minimize training burden,[Slack-native service management](https://unthread.io/products/slack-support?ref=unthread.io) provides an alternative intake channel. Employees submit requests in Slack channels they already use daily, while tickets sync bidirectionally with JSM for agent management. This approach lets organizations benefit from JSM's backend capabilities while reducing the change management required for requesters.


## **Post-Migration Optimization**


Migration completion marks the beginning of optimization, not the end of the project. The first 30-90 days reveal which configurations work and which need adjustment.


### **Leveraging JSM's Automation Capabilities**


Rebuild the automation rules documented from Freshservice, then expand beyond what was previously possible:


- **Auto-assignment based on request type and content analysis** - Route tickets to appropriate teams without manual triage
- **SLA breach escalation** - Automatically notify managers when tickets approach due dates
- **Cross-project automation** - Trigger actions in Jira Software projects when service tickets meet specific conditions
- **Scheduled bulk actions** - Close stale tickets, send reminder notifications, or generate reports on schedules


JSM's automation rules connect to the broader Atlassian ecosystem in ways Freshservice cannot. When a major incident occurs, automation can create linked documentation, trigger JSM's integrated alerting and on-call workflows, and coordinate related service-management actions across the Atlassian environment.


### **Integrating with Other Enterprise Tools**


Post-migration is the right time to evaluate integration opportunities:


- **Communication platforms** - Configure Slack or Teams webhooks for ticket notifications and ChatOps workflows
- **Development tools** - Connect GitHub or GitLab for code-related incident tracking
- **Identity management** - Implement SSO through Guard (required for enterprise security compliance)
- **HR systems** - Sync employee directories for accurate requester information


For organizations wanting deeper Slack integration than JSM's native webhooks provide,[Unthread's Jira integration](https://unthread.io/products/integrations?ref=unthread.io) enables bidirectional ticket sync. Employees create tickets in Slack channels, agents manage them in either Slack or JSM, and configured Jira comments, task updates, and related workflow events can sync between Unthread and Jira. This approach eliminates the context-switching that slows resolution times.


## **Streamlining IT Operations**


Traditional service desks require employees to leave their work context, navigate to a portal, and fill out forms. Conversational ticketing removes these friction points by meeting employees where they already work.


### **Reducing Manual Effort with Purpose-Built AI Workflows**


Purpose-built AI agents transform service desk operations by handling routine requests automatically. Rather than requiring agents to manually triage, categorize, and respond to every ticket, AI handles repetitive tasks while escalating complex issues to humans.


The impact is substantial. Unthread reports automatically resolving about 40% of incoming tickets across different teams with its purpose-built AI agent, although actual deflection will vary by workflow, knowledge quality, integrations, and request mix.


**AI automation spans multiple internal support workflows:**


- **Access requests** - Provision accounts, grant permissions, reset passwords without human intervention
- **IT troubleshooting** - Diagnose common issues, provide step-by-step solutions, escalate when needed
- **HR queries** - Answer benefits questions, explain policies, direct employees to appropriate resources
- **Onboarding tasks** - Create accounts across systems, assign required training, schedule orientation sessions


Some vendors emphasize high deflection rates by mainly automating access requests. Broader coverage across internal support use cases delivers more comprehensive value, reducing agent workload across departments rather than just optimizing one workflow.


### **Improving Response Times with Intelligent Automation**


Conversational AI combined with[workflow automation](https://unthread.io/products/automations?ref=unthread.io) creates response times impossible with manual processes:


- **Instant acknowledgment** - Employees receive immediate confirmation their request was received
- **Automatic categorization** - AI analyzes request content and assigns appropriate type, priority, and routing
- **Knowledge base integration** - AI references documentation to answer common questions without agent involvement
- **Smart escalation** - Complex issues route to appropriate specialists with full context preserved


The result is resolution times that outperform traditional approaches. When employees can ask "How do I reset my VPN password?" in Slack and receive step-by-step instructions within seconds, they return to productive work faster than any portal-based system allows.


## **Beyond Ticketing**


JSM is also used beyond traditional IT service management, including internal workflows for HR, facilities, legal, finance, and other business teams.


### **Leveraging JSM for HR and Other Internal Teams**


HR teams handle sensitive employee requests requiring privacy controls that standard ticketing lacks. Payroll questions, parental leave requests, workplace complaints, and benefits inquiries need careful handling.


JSM's issue security schemes support this requirement, but configuration complexity increases administrative burden. HR administrators must understand permission schemes, security levels, and project configurations to ensure only appropriate personnel access sensitive tickets.


For HR teams wanting simpler privacy controls,[Unthread's private ticketing](https://unthread.io/solutions/hr-service-desk?ref=unthread.io) provides an alternative approach. Employees submit sensitive requests without leaving Slack, tickets automatically move to private channels or DMs for confidential handling, and HR maintains complete visibility without exposing information to unintended audiences.


**Common HR use cases for service management:**


- **Employee onboarding** - New hire paperwork, benefits enrollment, equipment provisioning
- **Payroll and compensation** - Direct deposit changes, tax form requests, salary questions
- **Leave management** - PTO requests, parental leave applications, medical leave documentation
- **Policy questions** - Dress code clarifications, expense reimbursement procedures, remote work policies
- **Exit processing** - Resignation documentation, final paycheck coordination, equipment return


### **Integrating Live Chat and Customer Portals**


JSM includes a customer portal for ticket submission, but the interface requires users to learn a new system. Multi-channel intake that meets employees in familiar tools improves adoption and satisfaction.


For internal support, turning a Slack channel like #it-help into a full help desk provides several advantages:


- **Single intake location** - Employees know exactly where to submit requests
- **In-channel visibility** - Some ticket types remain visible for community knowledge sharing
- **Privacy when needed** - Sensitive requests automatically move to DMs or private channels
- **Structured ticketing** - Despite the conversational interface, full ticketing capabilities (SLAs, assignments, escalations) remain available
- **No training required** - Employees already know how to use Slack


This approach layers on top of JSM rather than replacing it. The[shared email inbox](https://unthread.io/products/shared-email-inbox?ref=unthread.io) converts support emails into Slack tickets while maintaining JSM as the system of record. Agents work in whichever interface they prefer; data syncs bidirectionally.


## **Leveraging Unthread to Complement Your Jira Service Management Migration**


JSM provides robust backend capabilities for IT service management. Unthread adds a conversational layer that reduces friction for employees submitting requests and agents resolving them.


Unthread's[agentic AI](https://unthread.io/products/agentic-ai?ref=unthread.io) works alongside JSM to handle routine internal requests before they require human attention. Key capabilities include:


- **Knowledge base deflection:** References documentation to answer common employee questions instantly.
- **Intent understanding:** Identifies what employees need, even when requests are vague or imprecise.
- **Workflow execution:** Triggers approved workflows directly instead of creating unnecessary tickets.
- **Smart escalation:** Creates a JSM ticket with the full conversation context when human attention is required.


This setup lets organizations keep JSM as their IT service management platform while adding Unthread's Slack-native conversational AI and internal workflow capabilities. Employees can get faster resolutions in Slack, while agents continue managing structured service workflows in JSM.


For agents who spend most of their day in Slack, Unthread's[Jira integration](https://unthread.io/products/integrations?ref=unthread.io) can also reduce context-switching. It supports:


- Tickets created from Slack and connected to Jira
- Bidirectional updates between Slack and Jira workflows
- Status updates, comments, and ticket management from Slack
- Shared visibility for agents working in either Slack or JSM


The[automation builder](https://unthread.io/products/automations?ref=unthread.io) extends these workflows further. Teams can create automations using natural language, visual workflows, or API integrations, then trigger actions based on ticket content, status changes, or scheduled rules.


For reporting,[AI analytics](https://unthread.io/products/analytics?ref=unthread.io) can complement JSM by tracking support volume, SLA performance, and recurring internal request patterns. This helps IT leaders identify high-volume workflows, recurring issues, and opportunities for further automation.


## **Frequently Asked Questions**


### **What are the main challenges when migrating from Freshservice to Jira Service Management?**


The main challenges are custom field mismatches, CMDB schema differences, and knowledge base migration. Freshservice fields may not map directly to JSM, while JSM Assets allows a maximum of 2 unique constraints per object type. Knowledge base articles also require separate planning because they move into Confluence and internal links may need updating.


### **How can data integrity be ensured during the migration process?**


Run demo migrations before production. Test 20-100 representative tickets and verify custom fields, attachments, comments, SLA data, and user assignments. Have key stakeholders review the results, fix mapping issues, and reconcile ticket counts after the full migration.


### **What kind of downtime should be expected when switching from Freshservice to JSM?**


Technical downtime may be minimal because migration tools typically use APIs. However, plan for a short cutover period where both systems may be active. Use delta migration where available, validate the final data, and clearly communicate when employees should start using JSM.


### **Can existing Freshservice automations be replicated in Jira Service Management?**


Yes, but they generally need to be rebuilt rather than migrated automatically. Document each automation's triggers, conditions, and actions, then recreate the highest-priority workflows in JSM before go-live.


### **How does Unthread complement Jira Service Management for IT and internal support teams?**


Unthread adds a Slack-native intake and automation layer on top of JSM. Employees can submit internal requests from Slack while tickets remain connected to JSM for structured management.[Unthread's purpose-built AI](https://unthread.io/products/agentic-ai?ref=unthread.io) can also handle routine requests automatically, with results varying by workflow, integrations, and knowledge quality.
