---
schema_version: "1.0.0"
document_id: "80f09f71dc3c3183132323d4c2ad6ec09b5798519eecbda8366998d74584fcd8"
company_key: "yc-unthread"
company: "Unthread"
source_id: "yc-unthread-rss-c618c87b9ca4"
canonical_url: "https://unthread.io/blog/migrate-zendesk-jira-service-management/"
published_at: "2026-08-01T16:17:58+00:00"
first_seen_at: "2026-08-01T17:15:54.398553+00:00"
fetched_at: "2026-08-01T17:15:55.343035+00:00"
content_hash: "sha256:f030b776f68244a13f34c26a11df188193b2c7094b092904dd391c70c39774d0"
---

# Migrate from Zendesk to Jira Service Management: Step-by-Step Zendesk Migration Guide

Moving from Zendesk to Jira Service Management represents one of the most common helpdesk migrations for IT and internal support teams looking to consolidate their Atlassian stack. But this migration is fundamentally different from switching between similar tools. Organizations translate between two distinct data models, and the stakes include entire support history, workflow automation, and team productivity.


For organizations evaluating this transition, it is worth considering whether migrating to another traditional helpdesk is the right long-term strategy. Modern[Slack-native service desks](https://unthread.io/products/slack-support?ref=unthread.io) like Unthread eliminate context-switching entirely by turning Slack conversations into structured tickets with AI-powered automation. Before committing to a complex migration, evaluate whether a purpose-built solution might serve internal support needs better than either legacy platform.


## **Key Takeaways**


- **Migration is a data model translation, not a simple file transfer.** Zendesk organizes around customer conversations while Jira Service Management organizes around workflows and issue types. Forcing a 1:1 field map without understanding the destination model leads to unusable data.
- **API-based migration tools can preserve public replies, internal notes, attachments, and conversation relationships more reliably than basic CSV exports.** Because preservation depends on the selected tool, permissions, and destination support, run a test migration and validate representative records before moving the full dataset.
- **Based on illustrative pricing and migration-cost assumptions, a 50-agent team could save approximately $19,400-$31,400 in Year 1.** Actual savings depend on contract terms, add-ons, implementation scope, and future pricing, with illustrative three-year savings of approximately $96,200-$108,200 before accounting for renewals, add-ons, administration, or other operating costs.
- **A staged migration with a final delta transfer can allow teams to continue using Zendesk during the main data move.** Plan a controlled cutover because email routing, portal configuration, authentication, DNS changes, and final validation can still create brief service disruption.
- **Macros, triggers, and automations often require manual recreation** because equivalent objects and logic differ between platforms. Check the selected migration provider's supported-object list, since some configurations may be transferable through custom services. Teams must document and manually rebuild top workflows in the destination platform, whether that is JSM or a modern alternative.
- **In one documented customer example, Unthread automatically resolved about 40% of incoming tickets** across IT, HR, legal, procurement, and finance workflows. Results vary with request mix, documentation quality, integrations, and automation coverage, eliminating the need for complex platform migrations entirely for internal support teams.


## **Why Consider Migrating from Zendesk to Jira Service Management**


Teams migrate from Zendesk to JSM for three primary reasons:


- Atlassian stack consolidation
- ITIL adoption
- The need to unify support and engineering workflows


Each motivation carries different implementation requirements and success criteria.


### **Atlassian Stack Consolidation**


Atlassian stack consolidation is a common reason organizations evaluate a move from Zendesk to JSM. Organizations already using Jira Software and Confluence for engineering may face:


- Redundant licensing costs
- Duplicate identity management
- Fragmented workflows
- Additional integration maintenance
- Separate reporting and administration processes


Consolidating platforms may reduce duplicate administration, identity management, integration maintenance, and reporting work. Quantify these savings using the organization's actual admin hours and tooling costs rather than applying a universal estimate.


### **ITIL Adoption Requirements**


ITIL adoption motivates regulated industries that need formal Incident, Problem, and Change management structures. Teams with formal incident, problem, and change-management requirements may prefer JSM's:


- Dedicated ITSM workflows
- Approval controls
- Audit-history features
- Structured governance processes


Zendesk can be configured for some of these processes, but the implementation approach and governance model differ.


### **Support and Engineering Unification**


Support and engineering unification addresses the handoff problem. When internal IT support tickets require engineering work, agents using Zendesk manually copy ticket details into Jira.


Manual copying between Zendesk and Jira can omit:


- Comments
- Attachments
- Field values
- Later ticket updates
- Relevant troubleshooting context


These gaps may force engineers to request missing information and increase resolution time.


Before committing to migration, consider whether JSM actually solves the team's core problems. If the IT team primarily needs[Slack-native ticketing](https://unthread.io/products/agentic-ai?ref=unthread.io) with AI automation, a platform like Unthread may deliver better outcomes without the migration complexity.


## **Planning Your Zendesk to Jira Service Management Migration**


Successful migrations start with a complete inventory of what is being moved and clear decisions about what stays behind. This planning phase prevents costly surprises during execution.


### **Audit Your Zendesk Instance Thoroughly**


Document the following before selecting a migration approach:


- Total ticket count by status and age
- All custom fields and their data types, including text, dropdown, and multi-select fields
- Ticket forms
- Brands and organizations
- Macros, triggers, and automations currently in use
- Help Center structure and article count


This inventory provides the foundation for field mapping, migration pricing, test planning, and post-migration validation.


### **Enable Zendesk Exports Before Migration**


Confirm Zendesk export and API settings before migration. Zendesk's downloadable JSON, CSV, and XML account exports are not enabled by default and may require the account owner to contact Zendesk Support.


REST API authentication is configured separately, so verify the exact access method required by the migration tool. Start this process immediately, as it can take several business days.


If the migration tool reports an export or authentication error, check whether it requires:


- Zendesk's account-export feature
- REST API access
- OAuth authentication
- An API token
- A combination of export and API permissions


The required setting varies by tool and authentication method.


### **Configure JSM Before Migration Begins**


Create the destination environment before transferring data. This typically includes:


- Service projects
- Request types
- Custom fields
- Agent accounts
- Workflows
- Statuses
- Permissions and roles


Many self-service migration tools require destination fields, request types, statuses, and agent accounts to be configured before transfer.


Confirm whether the selected provider can create missing objects through custom migration services or whether the team must create them manually. If a Zendesk custom field has no JSM equivalent, that data may be lost or mapped to a generic text field.


### **Choose Knowledge Base Destination**


Decide whether Help Center articles migrate to:


- JSM's Customer Portal
- Confluence
- A combination of both destinations


This decision affects licensing costs, article permissions, employee search behavior, and ongoing content-management workflows.


## **Step-by-Step Guide to Migrating Data from Zendesk**


The migration process follows six distinct phases. Skipping any phase, particularly testing, creates compounding problems that surface after teams have already cut over.


### **Phase 1: Connect Both Platforms**


Using an automated migration tool like Help Desk Migration or Getint, authenticate with OAuth or API tokens to both Zendesk and the JSM instance.


During this phase:


- Grant read access to Zendesk
- Grant write access to JSM
- Confirm that the authenticated accounts can access all required records
- Review any plan-based or endpoint-specific API restrictions
- Allow the migration tool to detect applicable rate limits


This phase typically takes 15-30 minutes to complete.


### **Phase 2: Map Data Fields**


The migration wizard displays source and target fields side by side. Standard fields like subject, status, priority, and assignee map automatically. Each Zendesk custom field must be manually mapped to its JSM equivalent.


Common field-mapping decisions include:


- **Ticket forms:** Map to JSM request types.
- **Zendesk brands:** May map to separate JSM projects for multi-brand accounts.
- **Zendesk Light Agents:** Require role-by-role mapping. In JSM, some users may become collaborators who add internal comments, while others may need full agent licenses or customer access depending on what they must view and update.
- **Multi-select fields:** Require matching multi-select fields in JSM or must be mapped to text strings.
- **Statuses:** Must be aligned with the destination workflow.
- **Custom user and organization fields:** Need compatible destination fields or an agreed fallback format.


This phase typically takes 1-2 hours to complete properly.


### **Phase 3: Run Free Demo Migration**


Some migration providers offer a free test migration. For example, Help Desk Migration currently advertises a demo of up to 20 selected records, while other providers may use different limits or testing models.


Run the demo 3-5 times with different field mappings. The demo can reveal:


- Field mismatches
- Visibility errors
- Missing attachments
- Broken comment sequencing
- Incorrect user mappings
- Portal-rendering problems


Review migrated tickets for field values, attachments, comment threads, and customer portal rendering. Adjust mappings and rerun until results match expectations.


This phase typically takes 30-60 minutes per test run.


### **Phase 4: Execute Full Migration**


Configure the migration options before starting the full transfer. These may include:


- Tagging migrated tickets, such as source=zendesk
- Migrating the newest records first
- Preserving comment and attachment history
- Enabling cross-link updates for knowledge base articles
- Selecting whether closed, archived, or deleted records are included
- Defining retry behavior for failed records


The migration tool should monitor Zendesk's account-level and endpoint-specific rate limits and throttle requests accordingly. Verify the limits shown in the Zendesk API dashboard and response headers because they vary by account, endpoint, and API usage pattern.


Migration duration can range from hours to several days depending on:


- Ticket volume
- Attachment size
- Comment count
- API limits
- Custom mappings
- Knowledge-base content
- Failed-record retries


Request a dataset-specific estimate from the selected migration provider after completing a test migration.


A staged migration with a final delta transfer can allow teams to continue using Zendesk during the main data move. Plan a controlled cutover because email routing, portal configuration, authentication, DNS changes, and final validation can still create brief service disruption.


### **Phase 5: Validate and Run Delta Migration**


Review the migration report for errors and complete a structured validation process:


- Spot-check 50 tickets across different request types
- Confirm ticket statuses and priorities
- Check requester and assignee mappings
- Review attachments and comment threads
- Test customer portal search
- Verify knowledge base article rendering
- Confirm internal and external comment visibility
- Test notification behavior


Then run Delta Migration to capture tickets created or updated in Zendesk during the main migration window.


Delta migration is usually faster than the full transfer because it moves only new or updated records. Its duration depends on the number of changed tickets, comments, attachments, API limits, and any records requiring retries.


This phase typically takes 1-2 days for validation and delta completion.


### **Phase 6: Execute Cutover**


Complete the final operational changes:


- Switch email routing from Zendesk to JSM
- Update portal DNS to point to the new JSM Customer Portal
- Set Zendesk to read-only or deactivate new ticket creation
- Verify authentication and portal access
- Confirm notifications and routing rules
- Train agents on JSM queues and JQL basics in a 2-hour hands-on session


The cutover window typically takes 1-2 hours.


## **Configuring Jira Service Management for Optimal Performance**


Post-migration configuration determines whether teams actually adopt JSM or quietly resent the change. Focus on three areas:


- Request queues
- SLA policies
- Automation rules


### **Request Queues**


Request queues organize incoming work by criteria agents understand. Create queues that match the team's mental models, such as:


- Hardware Requests
- Access Issues
- Software Installation
- Employee Onboarding
- Security Reviews
- Procurement Requests


Use JQL filters to automatically route tickets to appropriate queues based on request type, priority, team, or custom field values.


### **SLA Policies**


SLA policies must reflect realistic response expectations. JSM supports multiple SLA calendars for different ticket types.


During configuration:


- Set business hours accurately
- Account for regional and team-specific schedules
- Separate response and resolution targets
- Create different policies for request categories or priorities
- Start with achievable targets
- Tighten SLAs after the team stabilizes on the new platform


### **Automation Rules**


Automation rules replace Zendesk triggers and macros. This is the most labor-intensive post-migration work because macros, triggers, automations, views, and SLA configurations often require manual recreation since equivalent objects and logic differ between platforms.


Check the selected migration provider's supported-object list, since some configurations may be transferable through custom services.


A practical starting process is to:


1. Document the 10 most-used Zendesk macros and triggers.
2. Identify the volume handled by each automation.
3. Recreate the highest-value processes as JSM automation rules.
4. Test each rule with sample internal requests.
5. Monitor for loops, duplicate notifications, or incorrect routing.


Prioritize high-volume automations that save significant agent time.


For teams seeking automation without this manual rebuilding, Unthread's[workflow automation](https://unthread.io/products/automations?ref=unthread.io) builder lets teams create automations using natural-language descriptions. Teams describe what should happen, and the system generates the working automation.


## **Best Practices for Launching Your New Jira Service Management Instance**


Launch success depends on agent training, clear communication, and realistic expectations about the learning curve. JSM is not Zendesk with a different interface. It is a workflow engine designed for ITSM, not a conversation-first support tool.


### **Agent Training**


Agent training requires 2-4 hours per person. Training should cover:


- Queue navigation
- JQL basics for filtering
- How to update and transition tickets
- Internal and public comments
- Escalation procedures
- New automation rules
- Knowledge base usage
- Request-type and priority selection


Use real tickets from the migration for practice rather than generic training scenarios.


### **Employee Communication**


Communicate the change to employees submitting tickets. The customer portal looks and functions differently.


Prepare FAQ content explaining:


- How to submit requests
- Where to find the new portal
- How to check ticket status
- How to respond to agent questions
- How to search the knowledge base
- Which email addresses or Slack channels remain available


Send this communication 1-2 weeks before cutover.


### **Monitor Initial Tickets**


Monitor the first 10-20 new tickets closely. Verify that:


- Routing works correctly
- Notifications fire as expected
- Forms capture the required information
- Permissions protect sensitive requests
- The customer portal behaves properly
- Automation rules do not create duplicate actions


Have a rollback plan ready. Zendesk can be re-enabled and DNS reverted within 10-15 minutes if critical issues emerge.


### **Expect Productivity Adjustment**


Expect a productivity dip during the first 2-4 weeks. Agent ticket-handling times will increase as they learn the new interface.


This is normal. Resist the urge to add complexity during this period. Let the team stabilize before optimizing further.


## **Enhancing Service Management Beyond Traditional Help Desks: The Unthread Advantage**


While Zendesk-to-JSM migration solves specific problems like Atlassian consolidation and ITIL adoption, it does not address the fundamental friction in many internal support workflows: context switching.


Common sources of friction include:


- Employees leaving Slack to submit tickets
- Agents toggling between chat and ticketing interfaces
- Historical conversations becoming disconnected from ticket history
- Internal teams repeatedly asking employees for missing context
- Employees losing visibility after a request moves into another platform


### **Unthread's Agentic AI for Automated Resolution**


A documented Unthread customer reported approximately 40% automatic ticket resolution across IT, HR, legal, procurement, and finance workflows. Organizations should evaluate expected resolution rates against their own knowledge quality, ticket mix, approval requirements, and integrations.


This is not simple chatbot deflection. The AI can:


- Understand request intent
- Reference the knowledge base
- Draft accurate responses
- Route requests to the appropriate internal team
- Escalate to a human when needed
- Support workflows across multiple internal departments


Unlike traditional helpdesks adding AI features retroactively, Unthread supports bring-your-own-AI options on eligible plans and documents support for multiple model providers. It also provides MCP-related capabilities for connecting AI tools and workflows.


Organizations should confirm supported deployment models and private-instance requirements with Unthread.


### **Seamless Slack Integration**


JSM supports Slack-based service interactions through Atlassian capabilities available on eligible plans, while additional Marketplace apps can extend intake and synchronization.


Unthread takes a Slack-native approach in which configured Slack conversations can become trackable tickets and be managed without moving employees into a separate portal.


Unthread can automatically convert messages from configured Slack channels and supported private intake flows into tickets while preserving the conversational interface. Administrators control which channels and workflows use automatic ticket creation.


Agents can perform common ticket actions directly in Slack, including:


- Viewing their inbox
- Updating ticket status
- Setting priority
- Assigning or routing work
- Responding to employees
- Closing resolved tickets


For[IT service desk teams](https://unthread.io/solutions/it-service-desk?ref=unthread.io) , this means turning a channel like #it-help into a full internal help desk.


Employees submit requests naturally in Slack. Some ticket types can remain in-channel, while sensitive requests can move to DMs or private flows automatically.


### **Self-Learning Knowledge Base**


Traditional helpdesks require manual article creation and maintenance. Unthread's[AI Knowledge Base](https://unthread.io/products/knowledge-base?ref=unthread.io) supports a more continuous process by:


- Detecting repeat questions from ticket history
- Generating draft help articles for team review
- Identifying potential documentation gaps
- Flagging outdated information
- Showing before-and-after changes
- Allowing reviewers to approve updates


## **Understanding Jira Service Management Pricing and Alternatives**


Cost drives many migration decisions, but total cost of ownership extends beyond license fees.


Teams should account for:


- Platform subscriptions
- Implementation services
- Migration tooling
- Training
- Workflow rebuilding
- Add-ons
- Administration
- Ongoing maintenance


Zendesk Suite Professional is priced at approximately $115 per agent per month when billed annually, resulting in roughly $69,000 annually for a 50-agent team.


JSM pricing varies by billing interval, region, contract, packaging, add-ons, and volume, so organizations should verify current rates using Atlassian's calculator and Service Collection packaging before making decisions.


Unthread Pro is priced at approximately $75 per agent per month based on currently published materials, resulting in roughly $45,000 annually for a 50-agent team.


Migration costs add to Year 1 expenses. Illustrative cost categories include:


- **Migration tool fees:** Typically $2,000-$5,000 for 50,000 tickets
- **Configuration and training:** Approximately $3,000-$10,000 depending on complexity
- **Post-migration cleanup:** Approximately $2,000-$4,000 for workflow rebuilding


Using illustrative figures, JSM would produce estimated Year 1 savings of approximately $19,400-$31,400 compared to Zendesk. Over three years, estimated net savings would range from roughly $96,200 to $108,200 before accounting for renewals, add-ons, administration, or other operating costs.


Unthread's pricing includes capabilities that may require premium tiers or add-ons on traditional platforms, including:


- AI automation
- A self-learning knowledge base
- Native Slack integration
- Internal workflow automation
- Cross-department request handling


For teams prioritizing AI-first automation and Slack-native operations over formal ITIL workflows, Unthread often delivers strong value for internal support teams.


Unthread advertises a[14-day free trial](https://unthread.io/pricing?ref=unthread.io) . Confirm eligibility and trial terms for Enterprise features, dedicated hosting, and custom integrations directly with Unthread. This allows teams to evaluate the platform before committing to migration from an existing system.


## **Streamlining Internal Support with Unthread**


For teams evaluating the complexity of a Zendesk-to-JSM migration, Unthread offers a fundamentally different approach to internal service delivery.


Rather than translating data between traditional helpdesk platforms, organizations can move to a Slack-native model that reduces the friction of portal-based ticketing.


Unthread turns existing Slack conversations into structured, trackable tickets without forcing employees to change their behavior.


Internal support teams can manage requests across:


- IT
- HR
- Legal
- Procurement
- Finance
- Workplace operations


The platform's AI agent handles routine requests automatically, while the self-learning knowledge base continuously improves without relying entirely on manual article creation.


Organizations evaluating migration options should consider whether the goal is:


- To move from one traditional helpdesk to another
- To consolidate an existing Atlassian environment
- To adopt more formal ITIL workflows
- To reduce context switching for employees and internal teams
- To rethink how internal support operates


For teams where Slack is the primary communication platform and employee experience matters, Unthread provides a modern alternative to complex platform migrations.


## **Frequently Asked Questions**


### **What data does NOT transfer during migration?**


Macros, triggers, automations, views, and SLA configurations often require manual recreation because equivalent objects and logic differ between platforms. Check the selected migration provider's supported-object list, since some configurations may be transferable through custom services. Additionally, CC lists have limited support, and Zendesk App data from third-party integrations typically does not transfer. Plan to spend 20-40 hours rebuilding the most critical automations after migration completes.


### **How are historical tickets with Help Center article references handled?**


Enable the "Update cross-links between articles" option in the migration tool to automatically rewrite internal URLs to point to the new JSM Customer Portal or Confluence locations. For external links shared in emails or documentation, set up 301 redirects from old Zendesk Help Center URLs to their new destinations. Audit the top 20 most-viewed articles manually to verify links function correctly.


### **Can only recent tickets be migrated while archiving older ones?**


Yes. Most migration tools support date range filters, allowing migration of only tickets from the past 2-3 years to the active JSM instance. Export older tickets using a format appropriate for archival requirements. A basic CSV ticket export may not contain complete threaded comments, internal notes, attachments, and relationships, so consider Zendesk's JSON export or API-based extraction when full historical context must be retained. Some organizations maintain Zendesk in read-only mode for historical reference while only migrating recent active tickets to JSM.


### **What happens to ongoing tickets during migration?**


With a staged migration, teams can generally continue working in Zendesk during the main transfer. Confirm how the migration provider handles records updated in transit, and schedule a controlled final cutover for routing, portal access, authentication, and the delta transfer. The Delta Migration phase, run after the main migration completes, captures all tickets created or updated during the migration window. Schedule cutover (email routing switch, portal DNS change) only after Delta Migration confirms all recent tickets transferred successfully.


### **Is Unthread compatible with existing JSM deployments?**


Yes. Unthread integrates with Jira for bidirectional ticket sync, allowing teams to use Unthread for Slack-native employee requests while maintaining JSM for ITIL workflows or engineering escalations. This hybrid approach lets IT teams keep their existing JSM investment while adding conversational AI capabilities through Unthread for front-line employee support. Tickets can automatically escalate from Unthread to JSM based on request type or routing rules.
